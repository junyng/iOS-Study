# Image, Sound, and Video Resources

OS X와 iOS 플랫폼은 풍부한 멀티미디어 경험을 제공하기 위해 구축되었다. 이러한 경험을 지원하기 위해 두 플랫폼 모두 애플리케이션에서 이미지, 사운드 및 비디오 리소스를 로드하고 사용할 수 있는 많은 지원을 제공한다. 이미지 리소스는 일반적으로 애플리케이션의 사용자 인터페이스의 일부를 그리는 데 사용된다. 사운드 및 비디오 리소스는 덜 자주 사용되지만 애플리케이션의 기본적인 외관과 매력을 향상시킬 수도 있다. 다음 절에서는 애플리케이션에서 이미지, 사운드 및 비디오 리소스를 사용하는 데 사용할 수 있는 지원에 대해 설명한다.

### Nib 파일의 이미지 및 사운드

Xcode를 사용하면 Nib 파일 내에서 애플리케이션의 사운드 및 이미지 파일을 참조할 수 있다. 이러한 이미지 또는 사운드를 뷰 또는 컨트롤의 다른 속성과 연관시킬 수 있다. 예를 들어, 기본 이미지를 이미지 뷰에 표시하도록 설정하거나 이미지를 버튼에 표시하도록 설정할 수 있다. 닙 파일에 이러한 연결을 작성하면 닙 파일이 로드될 때 나중에 해당 연결을 해야 하는 번거로움을 줄일 수 있다.

닙 파일에서 이미지 및 사운드 리소스를 사용 가능하게 하려면 Xcode 프로젝트에 추가하기만 하면 된다. Xcode는 라이브러리 창에 나열한다. 주어진 리소스 파일에 연결할 때 Xcode는 닙 파일에 있는 해당 연결을 메모한다. 로드 타임에서 닙 로딩 코드는 프로젝트 번들에서 해당 리소스를 찾는다. 이 번들은 빌드 타임에 Xcode에 의해 배치되어야 했다.

이미지 및 사운드 리소스에 대한 참조가 포함된 닙 파일을 로드할 때, 닙-로딩 코드는 나중에 쉽게 검색할 수 있도록 가능할 때마다 리소스를 캐시한다. 예를 들어, 닙 파일을 로드한 후 [`NSImage`](https://developer.apple.com/documentation/appkit/nsimage) 또는 [`UIImage`](https://developer.apple.com/documentation/uikit/uiimage) \(플랫폼에 따라 다름\) 의 `imageNamed:` 메서드를 사용하여 해당 닙 파일과 연결된 이미지를 검색할 수 있다. OS X에서는 [NSSound](https://developer.apple.com/documentation/appkit/nssound)의 [`soundNamed:`](https://developer.apple.com/documentation/appkit/nssound/1477318-soundnamed)를 사용하여 캐시된 사운드 리소스를 검색할 수 있다.

### 이미지 리소스 불러오기

이미지 리소스는 대부분의 애플리케이션에서 일반적으로 사용된다. 아주 간단한 애플리케이션도 이미지를 사용하여 컨트롤과 뷰에 대한 사용자 정의 뷰를 만든다. OS X와 iOS는 Objective-C 객체를 사용하여 이미지 데이터를 조작하는 데 광범위한 지원을 제공한다. 이러한 객체는 이미지를 사용한 이미지를 매우 쉽게 사용할 수 있도록 하며, 종종 이미지를 로드하고 그리기 위해 몇 줄의 코드만 필요로 한다. Objective-C 객체를 사용하지 않으려는 경우 Quartz를 사용하여 이미지를 로드할 수도 있다. 다음 절에서는 사용 가능한 각 기술을 사용하여 이미지 리소스 파일을 로드하는 프로세스에 대해 설명한다.

#### Objective-C에서 이미지 불러오기

Objective-C에서 영상을 로드하려면 현재 플랫폼에 따라 [`NSImage`](https://developer.apple.com/documentation/appkit/nsimage) 또는 [`UIImage`](https://developer.apple.com/documentation/uikit/uiimage) 객체를 사용하라. AppKit 프레임워크를 사용하여 OS X 용으로 구축된 애플리케이션은 `NSImage` 객체를 사용하여 이미지를 로드하고 그린다. iOS 용으로 제작된 애플리케이션은 `UIImage` 객체를 사용한다. 애플리케이션 번들의 이미지 파일에 포인터를 전달하여 객체를 초기화하면 이미지 객체가 이미지 데이터로드에 대한 세부 사항을 처리한다.

목록 3-1은 OS X에서 `NSImage` 클래스를 사용하여 이미지 리소스를 로드하는 방법을 보여준다. 이 경우 애플리케이션 번들에 있는 이미지 리소스를 찾은 후 해당 경로를 사용하여 이미지 객체를 초기화하면 된다. 초기화 후 `NSImage`의 메서드를 사용하여 이미지를 그리거나 해당 객체를 사용할 수 있는 다른 메서드로 해당 객체를 전달할 수 있다. iOS에서 정확히 동일한 작업을 수행하려면 `NSImage`에 대한 참조를 `UIImage`로 변경하기만 하면 된다.

**목록 3-1**  하나의 이미지 리소스 불러오

```objectivec
NSString* imageName = [[NSBundle mainBundle] pathForResource:@"image1" ofType:@"png"];
NSImage* imageObj = [[NSImage alloc] initWithContentsOfFile:imageName];
```

이미지 객체를 사용하여 대상 플랫폼에서 지원되는 모든 유형의 이미지를 열 수 있다. 각 객체는 일반적으로 고급 이미지 처리 코드를 위한 경량 래퍼이다. 현재 그래픽 컨텍스트에서 이미지를 그리려면 그리기 관련 메서드 중 하나를 사용하면 된다. `NSImage`와 `UIImage`에는 이미지를 여러가지 메서드가 있다. `NSImage` 클래스는 로드한 이미지 조작을 위한 추가 지원 또한 제공한다.

NSImage 및 UIImage 클래스의 메서드에 대한 자세한 내용은 [_NSImage Class Reference_](https://developer.apple.com/documentation/appkit/nsimage) 및 [_UIImage Class Reference_](https://developer.apple.com/documentation/uikit/uiimage)를 참조하라. NSImage 클래스의 추가 기능에 대한 자세한 내용은 [_Cocoa Drawing Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290)의 [Images](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaDrawingGuide/Images/Images.html#//apple_ref/doc/uid/TP40003290-CH208)를 참조하라.

#### Quartz를 사용하여 이미지 불러오기

C-기반 코드를 작성하는 경우 Core Foundation과 Quartz 호출을 함께 사용하여 이미지 리소스를 애플리케이션에 로드할 수 있다. Core Foundation은 영상 리소스를 찾고 해당 영상 데이터를 메모리에 로드하기 위한 초기 지원을 제공한다. Quartz는 당신이 메모리에 로드한 이미지 데이터를 가지고 그것을 당신의 코드가 이미지를 그리는데 사용할 수 있는 [`CGImageRef`](https://developer.apple.com/documentation/coregraphics/cgimageref)로 바꾼다.

Quartz를 사용하여 이미지를 로드하는 방법에는 데이터 공급자와 이미지 소스 객체라는 두 가지가 있다. 데이터 공급자는 iOS와 OS X 모두에서 사용할 수 있다. 이미지 소스 객체는 OS X v10.4 이상에서만 사용할 수 있지만, I/O 프레임워크를 할용하여 데이터 공급자의 기본 이미지 처리 기능을 강화한다. 이미지 리소스를 로드하고 표시하는 데 있어서는 두 가지 기술이 모두 이 작업에 적합하다. 데이터 공급자보다 이미지 소스를 선호할 수 있는 유일한 시간은 이미지 관련 데이터에 대한 더 큰 접근을 원할 때이다.

목록 3-2는 데이터 공급자를 사용하여 JPEG 이미지를 로드하는 방법을 보여준다. 이 메서드는 Core Foundation 번들 지원을 사용하여 애플리케이션의 메인 번들에서 이미지를 찾고 URL을 얻는다. 그런 다음 해당 URL을 사용하여 데이터 공급자 객체를 생성한 다음 해당 JPEG 데이터에 대한 [`CGImageRef`](https://developer.apple.com/documentation/coregraphics/cgimageref)를 생성한다. \(간결하게 하게 위해 이 예제는 오류 처리 코드를 생략한다. 자신의 코드는 참조 된 데이터 구조체가 유효한지 확인해야 한다.\)

**목록 3-2** 데이터 공급자를 사용하여 이미지 리소스 로드

```objectivec
CGImageRef MyCreateJPEGImageRef (const char *imageName);
{
    CGImageRef image;
    CGDataProviderRef provider;
    CFStringRef name;
    CFURLRef url;
    CFBundleRef mainBundle = CFBundleGetMainBundle();
 
    // Get the URL to the bundle resource.
    name = CFStringCreateWithCString (NULL, imageName, kCFStringEncodingUTF8);
    url = CFBundleCopyResourceURL(mainBundle, name, CFSTR("jpg"), NULL);
    CFRelease(name);
 
    // Create the data provider object
    provider = CGDataProviderCreateWithURL (url);
    CFRelease (url);
 
    // Create the image object from that provider.
    image = CGImageCreateWithJPEGDataProvider (provider, NULL, true,
                                    kCGRenderingIntentDefault);
    CGDataProviderRelease (provider);
 
    return (image);
}
```

Quartz 이미지 작업에 대한 자세한 내용은 [_Quartz 2D Programming Guide_](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/Introduction/Introduction.html#//apple_ref/doc/uid/TP30001066)를 참조하라. 데이터 공급자에 대한 참조 정보는 _Quartz 2D Reference Collection_ \(OS X\) 또는 _Core Graphics Framework Reference_ \(iOS\)을 참조하라.

#### iOS에서 고해상도 이미지 지정

iOS 앱은 이미지 리소스의 고해상도 버전을 포함해야 한다. 고해상도 화면이 있는 장치에서 앱을 실행하면 고해상도 이미지가 추가 디테일을 제공하며 공간에 맞게 크기를 조정할 필요가 없어 더 보기 좋다. 아이콘 및 런치 이미지를 포함하여 애플리케이션 번들의 각 이미지 리소스에 대해 고해상도 이미지를 제공한다.

이미지의 고해상도 버전을 지정하려면 너비와 높이 \(픽셀 단위로 측정\)가 원본의 두 배인 버전을 만들어라. 추가 세부 정보를 제공하기 위해 이미지의 추가 픽셀을 사용하라. 이미지를 제공할 때는 동일한 기본 이름을 사용하지만 기본 파일 이름과 파일 이름 확장자 사이에 문자열 @2x를 포함하라. 예를 들어, MyImage.png 라는 이미지를 가지고 있다면 고해상도 버전의 이름은 MyImage@2x.png일 것이다. 이미지의 고해상도 및 원본 버전을 애플리케이션 번들의 동일한 위치에 둬라.

번들 및 이미지 로드 루틴은 기본 장치에 고해상도 화면이 있을 때 자동으로 @2x 문자열이 있는 이미지 파일을 검색한다. @2x 문자열을 다른 수식어와 결합하는 경우 @2x 문자열은 장치 수정자보다 우선하지만 시작 방향이나 URL 구성 수식어와 같은 다른 모든 수식어가 뒤에 와야 한다. 예를 들면 다음과 같다.

* `MyImage.png` - 이미지 리소스의 기본 버전
* `MyImage@2x.png` - 레티나 디스플레이가 있는 장치용 이미지 리소스의 고해상도 버전
* `MyImage~iphone.png` - iPhone 및 iPod Touch용 이미지 버전.
* `MyImage@2x~iphone.png` - 레티나 디스플레이가 있는 iPhone 및 iPod Touch 장치용 이미지의 고해상도 버전.

이미지를 로드하려면 코드에 이미지 이름을 지정할 때 @2x 또는 장치 수정자를 포함하지 말아라. 예를 들어, 애플리케이션 번들에 이전 목록의 이미지 파일이 포함된 경우 MyImage.png라는 이미지를 요청하라. 시스템이 가장 적합한 이미지의 버전을 자동으로 결정하여 로드한다. 마찬가지로, 그 이미지를 사용하거나 그릴 때, 당신은 그것이 원래의 해상도인지 고해상도 버전인지를 알 필요가 없다. 이미지 그리기 루틴은 로드된 이미지를 기반으로 자동으로 조정된다. 그러나 이미지가 원본인지, 고해상도 버전인지 여전히 알고 싶다면 그 스케일 팩터를 확인할 수 있다. 영상이 고해상도 버전인 경우, 그 스케일 팩터는 1.0이 아닌 값으로 설정된다.

고해상도 장치를 지원하는 방법에 대한 자세한 내용은 [Supporting High-Resolution Screens In Views](https://developer.apple.com/library/archive/documentation/2DDrawing/Conceptual/DrawingPrintingiOS/SupportingHiResScreensInViews/SupportingHiResScreensInViews.html#//apple_ref/doc/uid/TP40010156-CH15)를 참조하라.

