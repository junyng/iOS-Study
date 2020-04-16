# Core Video Tasks

이 장에서는 Core Video를 처리할 때 몇 가지 일반적인 프로그래밍 작업을 설명한다. 이 장의 예는 Objective-C로 작성되어 Cocoa를 사용하지만 Core Video는 Carbon 프로그램에서도 사용할 수 있다.

대부분의 경우, 개별 비디오 프레임에 접근하기 위해 디스플레이 링크를 사용하려고 한다. 만약 애플리케이션이 실제 비디오 프레임을 생성하는 데 관여한다면\(예를 들어, 비디오 컴프레서를 쓰거나 애니메이션 이미지를 만드는 경우\), 당신은 당신의 프레임 데이터를 유지하기 위해 Core Video 버퍼를 사용하는 것을 고려해야 한다.

### Obtaining Frames Using the Display Link

가장 일반적인 Core Video 태스크는 디스플레이 링크를 사용하여 압축되지 않은 비디오의 프레임을 얻는 것이다. 그러면 애플리케이션은 프레임을 디스플레이로 전송하기 전에 원하는 대로 자유롭게 조작할 수 있다.

단순성을 위해 이 섹션의 모든 메서드가 `NSOpenGLView` 클래스에서 서브클래싱된 `MyVideoView` 객체에 대해 작동한다고 가정하라.

**목록 2-1**  MyVideoView 인터페이스

```objectivec
@interface MyVideoView : NSOpenGLView
{
 
    NSRecursiveLock         *lock;
    QTMovie                 *qtMovie;
    QTTime                  movieDuration;
    QTVisualContextRef      qtVisualContext;
    CVDisplayLinkRef        displayLink;
    CVImageBufferRef        currentFrame;
    CIFilter                *effectFilter;
    CIContext               *ciContext;
 
    NSDictionary            *fontAttributes;
 
    int                     frameCount;
    int                     frameRate;
    CVTimeStamp             frameCountTimeStamp;
    double                  timebaseRatio;
    BOOL                    needsReshape;
    id                      delegate;
}
…
@end
```

> **중요:** OpenGL은 쓰레드에 안전하지 않다. 예를 들어, `NSRecursiveLock` 객체를 인스턴스화하고 `lock` 메서드를 호출하여 OpenGL 호출을 할 때 애플리케이션이 쓰레드를 잠그는지 확인해야 한다.

`NSOpenGLView` 클래스 사용에 대한 자세한 내용은 Cocoa OpenGL 프로젝트 예제를 참조하라.

#### Setting Up the Display Link

디스플레이 링크 설정에는 다음 단계가 포함된다:

* 디스플레이 링크 쓰레드를 만든다.
* 링크를 특정 디스플레이에 바인딩한다.
* 디스플레이 출력 콜백을 등록한다.
* 디스플레이 링크 쓰레드를 시작한다.

**목록 2-2**의 `awakeFromNib` 메서드는 디스플레이 링크를 구현하는 방법을 보여준다.

**목록 2-2** 디스플레이 링크 설정

```objectivec
- (void)awakeFromNib
{
    CVReturn            error = kCVReturnSuccess;
    CGDirectDisplayID   displayID = CGMainDisplayID();// 1
 
    error = CVDisplayLinkCreateWithCGDisplay(displayID, &displayLink);// 2
    if(error)
    {
        NSLog(@"DisplayLink created with error:%d", error);
        displayLink = NULL;
        return;
    }
    error = CVDisplayLinkSetOutputCallback(displayLink,// 3
                                 MyDisplayLinkCallback, self);
 
}
```

코드가 작동하는 방법은 다음과 같다:

1. 이 디스플레이 링크와 연결할 디스플레이에 대한 Core Graphics 디스플레이 ID를 얻어라. Core Graphics 함수 `CGMainDisplayID`\(즉, 메뉴 표시줄이 들어 있는 ID\)를 반환하기만 하면 된다.
2. 지정된 디스플레이에 대한 디스플레이 링크를 생성하라. 원하는 경우 `CVDisplayLinkCreateActiveCGDisplays`를 대신 호출하여 현재 활성 디스플레이와 함께 작동할 수 있는 표시 링크를 생성할 수 있다. 그런 다음 `CVDisplayLinkSetCurrentCGDisplay`를 호출하여 디스플레이 링크에 대한 특정 디스플레이를 지정해야 한다. 사용자가 비디오가 들어있는 윈도우를 다른 모니터로 이동할 경우 디스플레이 링크를 적절하게 업데이트 해야 한다. Cocoa에서 다음과 같은 핸들러로부터 `NSWindowDidMoveNotification` 노티피케이션을 받을 때 윈도우 위치를 확인할 수 있다: Carbon에서 윈도우 관리자 함수 `GetWindowGreatestAreaDevice`를 호출하여 윈도우 디스플레이에 대한 `GDevice` 구조체를 얻어라.
3. 디스플레이 링크의 출력 콜백을 설정하라. 이것은 디스플레이 링크가 비디오 프레임을 출력하기를 원할 때마다 호출하는 함수이다. 이 예제는 이 메서드\(즉, `self`\)를 사용자 데이터로 사용하여 인스턴스에 대한 참조를 전달한다. 예를 들어, 이 메서드가 `MyVideoView` 클래스의 일부인 경우 사용자 데이터는 `MyVideoView` 인스턴스에 대한 참조가 된다.

비디오 프레임 처리를 시작할 준비가 되면 `CVDisplayLinkStart`를 호출하여 디스플레이 링크 쓰레드를 활성화하라. 이 쓰레드는 애플리케이션 프로세스와 독립적으로 실행된다. 애플리케이션이 종료되거나 비디오 표시를 중지할 때 `CVDisplayLinkStop`을 호출하여 쓰레드를 중지해야 한다.

> **참고:** OS v10.3에서는 Fast User Switching이 호출된 경우에도 디스플레이 링크를 중지하라. OS X v10.4 이상에서는 사용자 전환 시 디스플레이 링크가 자동으로 중지된다.

#### Initializing Your Video Source

프로세싱을 시작하기 전에 프레임을 제공하도록 비디오 소스를 설정해야 한다. 비디오 소스는 압축되지 않은 비디오 데이터를 OpenGL 텍스처로 제공할 수 있는 모든 것일 수 있다. 예를 들어, 이 소스는 QuickTime, OpenGL 또는 고유한 비디오 프레임 생성기일 수 있다.

각 경우에 생성된 비디오를 표시하려면 OpenGL 컨텍스트를 생성해야 한다. 비디오 소스에 이 내용을 전달하여 비디오가 표시되기를 원하는 곳이라는 것을 나타낸다.

**목록 2-3**은 QuickTime 동영상을 비디오 소스로 설정하는 메서드를 보여준다.

**목록 2-3** QuickTime 비디오 소스 초기화

```objectivec
- (id)initWithFilePath:(NSString*)theFilePath // 1
{
    self = [super init];
 
    OSStatus        theError = noErr;
    Boolean         active = TRUE;
    UInt32          trackCount = 0;
    OSType          theTrackType;
    Track           theTrack = NULL;
    Media           theMedia = NULL;
 
    QTNewMoviePropertyElement newMovieProperties[] = // 2
        {
        {kQTPropertyClass_DataLocation,
            kQTDataLocationPropertyID_CFStringNativePath,
            sizeof(theFilePath), &theFilePath, 0},
        {kQTPropertyClass_NewMovieProperty, kQTNewMoviePropertyID_Active,
            sizeof(active), &active, 0},
        {kQTPropertyClass_Context, kQTContextPropertyID_VisualContext,
            sizeof(qtVisualContext), &qtVisualContext, 0},
        };
 
    theError = QTOpenGLTextureContextCreate( NULL, NULL, // 3
        [[NSOpenGLView defaultPixelFormat]
         CGLPixelFormatObj], NULL, &qtVisualContext);
 
    if(qtVisualContext == NULL)
     {
        NSLog(@"QTVisualContext creation failed with error:%d", theError);
        return NULL;
    }
 
    theError = NewMovieFromProperties(
        sizeof(newMovieProperties) / sizeof(newMovieProperties[0]),// 4
        newMovieProperties, 0, NULL, &channelMovie);
 
    if(theError)
    {
        NSLog(@"NewMovieFromProperties failed with %d", theError);
        return NULL;
    }
 
    // setup the movie
    GoToBeginningOfMovie(channelMovie);// 5
    SetMovieRate(channelMovie, 1 << 16);
    SetTimeBaseFlags(GetMovieTimeBase(channelMovie), loopTimeBase);
    trackCount = GetMovieTrackCount(channelMovie);
    while(trackCount > 0)
    {
        theTrack = GetMovieIndTrack(channelMovie, trackCount);
        if(theTrack != NULL)
        {
            theMedia = GetTrackMedia(theTrack);
            if(theMedia != NULL)
            {
                GetMediaHandlerDescription(theMedia, &theTrackType, 0, 0);
                if(theTrackType != VideoMediaType)
                {
                    SetTrackEnabled(theTrack, false);
                }
            }
        }
        trackCount--;
    }
 
    return self;
}
```

 코드 작동 방법은 다음과 같다:

1. 이 메서드는 QuickTime 동영상의 파일 경로를 입력 매개 변수로 사용한다.
2. 동영상 속성 배열을 설정하라. 이러한 속성은 다음을 지정한다

   * 동영상의 파일 경로
   * 새 동영상이 활성화되어야 하는지 여부\(예를 들어, 이 경우\)
   * 이 동영상과 연관지을 시각적 컨텍스트. `qtVisualContext` 변수는 이 메서드의 클래스 인스턴스 변수이다.

   이러한 속성은 나중에 `NewMovieFromProperties` 함수로 전달된다.

3. OpenGL 텍스처 컨텍스트를 생성하라. 이것은 OpenGL 텍스처가 그려지는 추상적인 목적지다. QuickTime 함수 `QTOpenGLTextureContextCreate`는 `CGLContext`와 `CGLPixelFormat` 객체를 통과해야 한다. Cocoa에서는 OpenGL을 초기화할 때 생성된 NSOpenGLContext 및 NSOpenGLPixelFormat 객체에서 이러한 객체를 얻을 수 있다. Carbon에서 AGL 함수 `aglGetCGLContext` 및 `aglGetCGLPixelFormat` 객체에서 기본 컨텍스트와 픽셀 형식을 가져올 수 있다. 인스턴스 변수 `qtVisualContext`에 저장된 이 컨텍스트는 QuickTime이 동영상을 그리는 시각적 컨텍스트로 `NewMovieFromProperties`로 전달된다.
4. 동영상을 만든다. OS X v10.4 이상 또는 QuickTime 7.0 이상에서 사용할 수 있는 QuickTime 함수 `NewMovieFromProperties`는 동영상을 인스턴스화하는 데 선호되는 방법이다. Cocoa를 사용하고 있다면 QTKit 메서드 `movieFromFile`로 대신 호출하면 된다. 동영상을 만든 후 시각적 컨텍스트를 설정하거나 변경하려는 경우 QuickTime 함수 `SetMovieVisualContext`를 호출할 수 있다.
5. 동영상에서 다양한 초기화를 수행하라. 이 섹션은 대부분 처음부터, 일반적인 프레임률에서, 연속 루프로 동영상을 시작하기 위한 보일러 플레이트 코드다. 이 코드는 또한 사용 가능한 트랙을 루프하고 비디오가 아닌 트랙을 비활성화한다.

**Implementing the Display Link Output Callback Function**

디스플레이 링크가 실행 중일 때 프레임을 준비해야 할 때마다 주기적으로 애플리케이션으로 다시 호출된다. 콜백 함수는 지정된 비디오 소스에서 OpenGL 텍스처로 프레임을 얻은 다음에 화면에 출력해야 한다.

객체 지향 프로그래밍을 사용하는 경우, 목록 2-4 및 목록 2-5에서와 같이 콜백이 메서드를 호출하도록 원할 수 있다.

**목록 2-4** 콜백으로 부터 메서드를 호출

```objectivec
CVReturn MyDisplayLinkCallback (
    CVDisplayLinkRef displayLink,
    const CVTimeStamp *inNow,
    const CVTimeStamp *inOutputTime,
    CVOptionFlags flagsIn,
    CVOptionFlags *flagsOut,
    void *displayLinkContext)
{
 CVReturn error =
        [(MyVideoView*) displayLinkContext displayFrame:inOutputTime];
 return error;
}
```

콜백 함수는 `MyVideoView` 클래스에서 구현된 `displayFrame` 메서드를 간단하게 호출한다. 이 클래스의 인스턴스가 디스플레이 링크 컨텍스트 매개변수의 콜백으로 전달된다. \(프레임을 표시하는 `MyVideoView` 클래스는 목록 2-1에서와 같이 `NSOpenGLView`의 서브클래스여야 한다.\)

**목록 2-5**  `displayFrame` 메서드 구현

```objectivec
- (CVReturn)displayFrame:(const CVTimeStamp *)timeStamp
{
    CVReturn rv = kCVReturnError;
    NSAutoreleasePool *pool;
 
    pool = [[NSAutoreleasePool alloc] init];
    if([self getFrameForTime:timeStamp])
    {
        [self drawRect:NSZeroRect];
        rv = kCVReturnSuccess;
    }
    else
    {
       rv = kCVReturnError;
    }
    [pool release];
    return rv;
}
 
```

지정된 시간 동안 OpenGL 텍스처로 프레임을 얻어라. 목록 2-6은 QuickTime에서 비디오 프레임을 얻는 경우 `getFrameForTime` 메서드를 구현하는 방법을 보여준다. 이 예제는 메서드가 사용자 정의 `MyVideoView` 클래스의 일부라고 가정한다.

**목록 2-6**  QuickTime으로 부터 프레임 얻기

```objectivec
- (BOOL)getFrameForTime:(const CVTimeStamp*)syncTimeStamp
{
    CVOpenGLTextureRef      newTextureRef = NULL;
 
    QTVisualContextTask(qtVisualContext);// 1
    if(QTVisualContextIsNewImageAvailable(qtVisualContext, syncTimeStamp))// 2
    {
        QTVisualContextCopyImageForTime(qtVisualContext, NULL, syncTimeStamp,// 3
                 &newTextureRef);
 
        CVOpenGLTextureRelease(currentFrame);// 4
        currentFrame = newTextureRef;
 
        CVOpenGLTextureGetCleanTexCoords (// 5
                    currentFrame, lowerLeft, lowerRight, upperRight, upperLeft);
        return YES; // we got a frame from QT
    }
    else
    {
        //NSLog(@"No Frame ready");
    }
    return NO;  // no frame available
}
```

코드가 작동하는 방법은 다음과 같다:

1. 컨텍스트에 시간을 부여하여 필요한 모든 하우스키핑을 수행할 수 있도록 한다. 각 프레임을 어딕 전에 이 함수를 호출해야 한다.
2. 지정된 시간 동안 새 프레임을 사용할 수 있는지 확인하라. 디스플레이 링크로 콜백에 전달된 요청 시간은 현재 시간이 아니라 향후 프레임이 표시되는 시간을 나타낸다.
3. 프레임을 사용할 수 있는 경우, OpenGL 텍스처로 얻는다. QuickTime 함수 `QTVisualContextCopyImageForTime`은 QuickTime에서 Core Video 이미지 버퍼 타입으로 프레임을 얻을 수 있게 해준다.
4. 현재 텍스처\(인스턴스 변수 `currentFrame`에 저장\)를 해제하고 새로 획득한 텍스처를 설정하여 교체한다. 비디오 메모리를 채울 가능성을 최소화하기 위해 OpenGL 텍스처를 사용할 때 해제하라.
5. 텍스처에 대한 깨끗한 조리개의 좌표를 구한다. 대부분의 경우, 렌더링에 필요한 텍스처 한계값이다.

### Manipulating Frames

비디오 소스에서 프레임을 획득한 후, 프레임을 어떻게 할지 결정하는 것은 당신에게 달려 있다. 프레임은 OpenGL 텍스처로 제공되므로 OpenGL 호출로 조작할 수 있다. 목록 2-7은 표준 NSView `drawRect` 메서드를 재정의하여 뷰 바운드로 그리도록 OpenGL을 설정하는 방법을 보여준다.

**목록 2-7**  직사각형에 OpenGL 표시

```objectivec
- (void)drawRect:(NSRect)theRect
{
    [lock lock];    // 1
    NSRect      frame = [self frame];
    NSRect      bounds = [self bounds];
 
    [[self openGLContext] makeCurrentContext];// 2
    if(needsReshape)// 3
    {
        GLfloat     minX, minY, maxX, maxY;
 
        minX = NSMinX(bounds);
        minY = NSMinY(bounds);
        maxX = NSMaxX(bounds);
        maxY = NSMaxY(bounds);
 
        [self update];
 
        if(NSIsEmptyRect([self visibleRect])) // 4
        {
            glViewport(0, 0, 1, 1);
        } else {
            glViewport(0, 0,  frame.size.width ,frame.size.height);
        }
        glMatrixMode(GL_MODELVIEW);
        glLoadIdentity();
        glMatrixMode(GL_PROJECTION);
        glLoadIdentity();
        glOrtho(minX, maxX, minY, maxY, -1.0, 1.0);
 
        needsReshape = NO;
    }
 
    glClearColor(0.0, 0.0, 0.0, 0.0);
    glClear(GL_COLOR_BUFFER_BIT);
 
    if(!currentFrame)// 5
        [self updateCurrentFrame];
    [self renderCurrentFrame];      // 6
    glFlush();// 7
    [lock unlock];// 8
}
```

코드 작동 방식은 다음과 같다:

1. 현재 쓰레드를 잠근다. OpenGL은 쓰레드 안전하지 않으므로 쓰레드가 하나만 주어진 시간에 OpenGL 호출을 할 수 있는지 확인해야 한다.
2. 이 객체의 컨텍스트에 렌더링할 OpenGL을 설정하라.
3. 도면 직사각형의 크기를 조정한 경우, OpenGL 컨텍스트의 크기를 업데이트하는 단계를 수행한다.
4. 뷰가 보이는 경우, OpenGL 컨텍스트를 뷰의 새 바운드에 매핑한다. 그렇지 않은 경우, 컨텍스트를 효과적으로 보이지 않도록 매핑하라.
5. 현재 프레임이 존재하지 않는 경우 현재 프레임을 다시 얻어라. `drawRect` 메서드가 뷰 크기에 대응하여 호출될 경우 이 상황이 발생할 수 있다.
6. 현재프레임을 OpenGL 컨텍스트에 그려라. `renderCurrentFrame`는 사용자 정의 프레임 코드를 보유하는 메서드이다.
7. 도면을 OpenGL 렌더러로 플러시한다. 그런 다음 프레임은 적절한 시간에 자동으로 화면에 표시된다.
8. 모든 OpenGL 호출을 완료한 후 쓰레드를 잠금 해제한다.

`renderCurrentFrame` 메서드에는 애플리케이션이 프레임에 적용할 사용자 정의 처리가 포함되어 있다. **목록 2-8**은 이 메서드를 구현할 수 있는 간단한 예를 보여준다. 이 예제는 Core Image를 사용하여 프레임을 OpenGL 컨텍스트에 그려 넣는다.

**목록 2-8**  프레임 그리기

```objectivec
- (void)renderCurrentFrame
{
    NSRect      frame = [self frame];
 
    if(currentFrame)
    {
        CGRect      imageRect;
        CIImage     *inputImage;
 
        inputImage = [CIImage imageWithCVImageBuffer:currentFrame];// 1
 
        imageRect = [inputImage extent];// 2
        [ciContext drawImage:inputImage // 3
                atPoint:CGPointMake(
                (int)((frame.size.width - imageRect.size.width) * 0.5),
                (int)((frame.size.height - imageRect.size.height) * 0.5))
                fromRect:imageRect];
 
    }
    QTVisualContextTask(qtVisualContext);// 4
}
```

코드 동작은 다음과 같다:

1. 현재 프레임에서 Core Image 이미지를 생성한다. Core Image 메서드 ImageWithCVImageBuffer는 임의의 이미지 버퍼 유형\(즉, 픽셀 버퍼, OpenGL 버퍼 또는 OpenGL 텍스처\)에서 이미지를 생성한다.
2. 이미지의 경계 사각형을 가져온다.
3. 이미지를 Core Image 컨텍스트로 그린다. Core Image 메서드 `drawImage:atPoint:fromRect`는 지정된 시각적 컨텍스트에서 프레임을 그린다. 그리기 전에 OpenGL 컨텍스트와 동일한 그리기 공간을 참조하는 Core Image 컨텍스트를 생성해야 한다. 이렇게 하면 Core Image API를 사용하여 공간에 그린 다음 OpenGL을 사용하여 표시할 수 있다. 예를 들어, OpenGL 컨텍스트를 작성한 후 다음 코드를 목록 2-3에 추가할 수 있다:

```objectivec
/* Create CGColorSpaceRef */
CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceRGB();
 
/* Create CIContext */
ciContext = [[CIContext contextWithCGLContext:
                (CGLContextObj)[[self openGLContext] CGLContextObj]
                pixelFormat:(CGLPixelFormatObj)
                [[self pixelFormat] CGLPixelFormatObj]
                options:[NSDictionary dictionaryWithObjectsAndKeys:
                (id)colorSpace,kCIContextOutputColorSpace,
                (id)colorSpace,kCIContextWorkingColorSpace,nil]] retain];
CGColorSpaceRelease(colorSpace);
```

Core Image 컨텍스트 생성에 대한 자세한 내용은 [_Core Image Programming Guide_](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_intro/ci_intro.html#//apple_ref/doc/uid/TP30001185)을 참조하라.

4. 필요한 하우스키핑을 수행할 수 있는 시각적 컨텍스트에 시간을 부여한다. 그리기 메서드를 통해 매번 `QTVisualContextTask`에 호출해야 한다.

### Using Core Image Filtering With Core Video

동영상에 필터링 효과를 적용하려면 자신의 이미지 프로세싱을 구현하기보다는 Core Image 필터를 적용하는 것이 더 간단하다. 그러기 위해서는 Core Image 이미지로 프레임을 획득해야 한다.

Core Image CIFilter 메서드 `filterWithName:`를 사용하여 Core Image 필터를 로드할 수 있다:

```objectivec
effectFilter = [[CIFilter filterWithName:@"CILineScreen"] retain];
[effectFilter setDefaults];
```

이 예제는 표준 Core Image 라인 스크린 필터를 로드하지만, 애플리케이션에 적합한 모든 것을 사용해야 한다.

필터를 로드한 후 그리기 메서드로 이미지를 처리하라. **목록 2-9**는 Core Image 필터를 적용할 수 있는 방법을 보여준다. 이 목록은 입력 이미지를 Core Image 컨텍스트에 그리기 전에 필터링한다는 점을 제외하고 목록 2-8과 동일하다.

**목록 2-9**  이미지에 Core Image 필터 적용

```objectivec
- (void)renderCurrentFrameWithFilter
{
    NSRect      frame = [self frame];
 
    if(currentFrame)
    {
        CGRect      imageRect;
        CIImage     *inputImage, *outputImage;
 
        inputImage = [CIImage imageWithCVImageBuffer:currentFrame];
 
        imageRect = [inputImage extent];
        [effectFilter setValue:inputImage forKey:@"inputImage"];// 1
        [[[NSGraphicsContext currentContext] CIContext]// 2
            drawImage:[effectFilter valueForKey:@"outputImage"]
            atPoint:CGPointMake((int)
                ((frame.size.width - imageRect.size.width) * 0.5),
                (int)((frame.size.height - imageRect.size.height) * 0.5))
            fromRect:imageRect];
 
    }
    QTVisualContextTask(qtVisualContext);
}
```

코드 작동 방식은 다음과 같다:

1. 프레임에 적용할 CIImage 필터를 설정한다.
2. 지정된 필터를 사용하여 이미지를 그린다.

Core Image 이미지는 불변하므로 프레임을 얻을때마다 새 이미지를 생성해야 한다.

Core Image 필터 생성 및 사용에 대한 자세한 내용은 [_Core Image Programming Guide_](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_intro/ci_intro.html#//apple_ref/doc/uid/TP30001185)를 참조하라.

