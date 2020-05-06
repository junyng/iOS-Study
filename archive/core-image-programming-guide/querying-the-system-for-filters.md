# Querying the System for Filters

Core Image는 사용 가능한 내장 필터 및 각 필터에 대한 관련 정보\(디스플레이 이름, 입력 파라미터, 파라미터 타입, 기본 값 등\)를 시스템에 쿼리할 수 있는 메서드를 제공한다. 시스템 쿼리는 사용 가능한 필터에 대한 최신 정보를 제공한다. 앱에서 사용자가 필터를 선택하고 설정할 수 있도록 지원하는 경우 필터에 대한 사용자 인터페이스를 만들 때 이 정보를 사용할 수 있다.

### Getting a List of Filters and Attributes

[`filterNamesInCategory:`](https://developer.apple.com/documentation/coreimage/cifilter/1438145-filternames) 및 [`filterNamesInCategories:`](https://developer.apple.com/documentation/coreimage/cifilter/1437595-filternamesincategories) 메서드를 사용해 사용 가능한 필터를 정확하게 검색하라. 필터는 목록을 보다 쉽게 관리할 수 있도록 분류된다. 필터 범주를 알고 있는 경우 [`filterNamesInCategory:`](https://developer.apple.com/documentation/coreimage/cifilter/1438145-filternames) 메서드를 호출하여 해당 범주에 사용할 수 있는 필터를 찾을 수 있다. 그리고 Table 4-1, [Table 4-2](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_concepts/ci_concepts.html#//apple_ref/doc/uid/TP30001185-CH2-SW5), 또는 [Table 4-3](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_concepts/ci_concepts.html#//apple_ref/doc/uid/TP30001185-CH2-SW6)에 나열된 범주 상수 중 하나를 제공한다.

범주 목록에 대한 사용 가능한 모든 필터를 찾으려면 [`filterNamesInCategories:`](https://developer.apple.com/documentation/coreimage/cifilter/1437595-filternamesincategories) 메서드를 호출할 수 있고, 표에 나열된 범주 상수의 배열을 제공한다. 메서드는 각 범주의 필터 이름으로 채워진 `NSArray` 객체를 반환한다. 범주 상수의 배열 대신 `nil`을 제공하면 모든 범주에 대한 모든 필터 목록을 얻을 수 있다.

필터는 둘 이상의 범주의 구성원이 될 수 있다. 범주는 다음을 지정할 수 있다:

* 필터에서 발생하는 효과 유형\(색상 조정, 왜곡 등\). Table 4-1 참조.
* 필터 사용\(스틸 이미지, 비디, high dynamic range 등\) [Table 4-2](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_concepts/ci_concepts.html#//apple_ref/doc/uid/TP30001185-CH2-SW5) 참조.
* 필터를 Core Image\(내장된\)에서 제공하는지 여부. [Table 4-3](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_concepts/ci_concepts.html#//apple_ref/doc/uid/TP30001185-CH2-SW6) 참조.

**Table 4-1** 효과 유형에 대한 필터 범주 상수

| Effect type | Indicates |
| :--- | :--- |
| `kCICategoryDistortionEffect` | Distortion effects, such as bump, twirl, hole |
| `kCICategoryGeometryAdjustment` | Geometry adjustment, such as affine transform, crop, perspective transform |
| `kCICategoryCompositeOperation` | Compositing, such as source over, minimum, source atop, color dodge blend mode |
| `kCICategoryHalftoneEffect` | Halftone effects, such as screen, line screen, hatched |
| `kCICategoryColorAdjustment` | Color adjustment, such as gamma adjust, white point adjust, exposure |
| `kCICategoryColorEffect` | Color effect, such as hue adjust, posterize |
| `kCICategoryTransition` | Transitions between images, such as dissolve, disintegrate with mask, swipe |
| `kCICategoryTileEffect` | Tile effect, such as parallelogram, triangle |
| `kCICategoryGenerator` | Image generator, such as stripes, constant color, checkerboard |
| `kCICategoryGradient` | Gradient, such as axial, radial, Gaussian |
| `kCICategoryStylize` | Stylize, such as pixellate, crystallize |
| `kCICategorySharpen` | Sharpen, luminance |
| `kCICategoryBlur` | Blur, such as Gaussian, zoom, motion |

**Table 4-2** 필터 사용에 대한 필터 범주 상수

| Use | Indicates |
| :--- | :--- |
| `kCICategoryStillImage` | Can be used for still images |
| `kCICategoryVideo` | Can be used for video |
| `kCICategoryInterlaced` | Can be used for interlaced images |
| `kCICategoryNonSquarePixels` | Can be used for nonsquare pixels |
| `kCICategoryHighDynamicRange` | Can be used for high-dynamic range pixels |

**Table 4-3** 필터 원본에 대한 필터 범주 상수

| Filter origin | Indicates |
| :--- | :--- |
| `kCICategoryBuiltIn` | A filter provided by Core Image |

필터 이름 목록을 가져온 후 CIFilter 객체를 생성하고 메서드 [`attributes`](https://developer.apple.com/documentation/coreimage/cifilter/1437661-attributes)를 다음과 같이 호출하여 필터 속성을 검색할 수 있다:

```text
CIFilter *myFilter = [CIFilter filterWithName:@"<# Filter Name Here #>"];
NSDictionary *myFilterAttributes = [myFilter attributes];
```

문자열 "&lt;\# Filter Name Here \#"를 관심 있는 필터 이름으로 바꿔라. 속성에는 이름, 범주, 클래스, 최소 및 최대값 등이 포함된다. 반환할 수 있는 전체 속성 목록은 [_CIFilter Class Reference_](https://developer.apple.com/documentation/coreimage/cifilter)를 참조하라.

### Building a Dictionary of Filters

앱이 사용자 인터페이스를 제공하는 경우 필터 딕셔너리를 참조하여 사용자 인터페이스를 만들고 업데이트할 수 있다. 예를 들어, Boolean인 필터 특성은 체크박스나 유사한 사용자 인터페이스 요소가 필요하며, 범위에 걸쳐 지속적으로 변화하는 특성은 슬라이더를 사용할 수 있다. 텍스트 레이블의 기준으로 최대값과 최소값을 사용할 수 있다. 기본 속성 설정은 사용자 인터페이스의 초기 설정을 드러낸다.

필터 이름과 속성은 사용자가 필터를 선택하고 입력 파라미터를 제어할 수 있는 사용자 인터페이스를 구축하는 데 필요한 모든 정보를 제공한다. 필터 속성은 필터에 있는 입력 파라미터 수, 파라미터 이름, 데이터 타입, 최소값, 최대값 및 기본값을 알려준다.

> 참고: Core Image 필터에 대한 사용자 인터페이스를 구축하는 데 관심이 있는 경우, Core Image 필터에 대한 입력 파라미터 변수 컨트롤이 포함된 뷰를 제공하는 [_IKFilterUIView Class Reference_](https://developer.apple.com/documentation/quartz/ikfilteruiview)를 참조하라.

Listing 4-1은 필터 이름을 가져오고 기능 범주별로 필터 딕셔너리를 작성하는 코드를 보여준다. 코드는 이러한 범주의 `kCICategoryGeometryAdjustment`, `kCICategoryDistortionEffect`, `kCICategorySharpen`, 및 `kCICategoryBlur` 의 필터를 가져온다. 그러나 Distortion 및 Focus와 같은 앱 정의 기능 범주에 기초하여 딕셔너리를 정의한다. 기능적인 범주는 사용자에게 적합한 메뉴의 필터 이름을 구성하는데 유용하다. 코드는 가능한 모든 Core Image 필터 범주를 통해 반복되지 않지만, 동일한 프로세스를 수행하여 이 코드를 쉽게 확장할 수 있다.

**Listing 4-1**  기능적인 범주별로 필터 딕셔너리를 작성하는 코드

```objectivec
NSMutableDictionary *filtersByCategory = [NSMutableDictionary dictionary];
 
NSMutableArray *filterNames = [NSMutableArray array];
[filterNames addObjectsFromArray:
    [CIFilter filterNamesInCategory:kCICategoryGeometryAdjustment]];
[filterNames addObjectsFromArray:
    [CIFilter filterNamesInCategory:kCICategoryDistortionEffect]];
filtersByCategory[@"Distortion"] = [self buildFilterDictionary: filterNames];
 
[filterNames removeAllObjects];
[filterNames addObjectsFromArray:
    [CIFilter filterNamesInCategory:kCICategorySharpen]];
[filterNames addObjectsFromArray:
    [CIFilter filterNamesInCategory:kCICategoryBlur]];
filtersByCategory[@"Focus"] = [self buildFilterDictionary: filterNames];
```

Listing 4-2는 Listing 4-1에서 호출된 `buildFilterDictionary` 루틴을 보여준다. 이 루틴은 기능적인 범주의 각 필터에 대한 속성의 딕셔너리를 구축한다. 번호가 매겨진 각 코드 라인에 대한 자세한 설명은 목록을 따른다.

**Listing 4-2**  기능 이름으로 필터 딕셔너리 생성

```objectivec
- (NSMutableDictionary *)buildFilterDictionary:(NSArray *)filterClassNames  // 1
{
    NSMutableDictionary *filters = [NSMutableDictionary dictionary];
    for (NSString *className in filterClassNames) {                         // 2
        CIFilter *filter = [CIFilter filterWithName:className];             // 3
 
        if (filter) {
            filters[className] = [filter attributes];                       // 4
        } else {
            NSLog(@"could not create '%@' filter", className);
        }
    }
    return filters;
}
```

코드가 하는 일은 다음과 같다:

1. 필터 이름 배열을 입력 파라미터로 삼는다. [Listing 4-1](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_concepts/ci_concepts.html#//apple_ref/doc/uid/TP30001185-CH2-SW7)에서 이 배열이 둘 이상의 Core Image 필터 범주의 필터 이름을 연결할 수 있음을 상기하라. 이 예제에서 배열은 앱에서 설정한 기능 범주에 기반한다 \(Distortion or Focus\).
2. 필터 이름 배열을 통해 순회한다.
3. 필터 이름의 필터 객체를 검색한다.
4. 필터 속성 딕셔너리를 검색하여 루틴에서 반환하는 딕셔너리에 추가하라.

> 참고: OS X v10.5 이상에서 실행되는 앱은 CIFilter Image Kit 추가를 사용하여 필터 브라우저 및 필터 입력 파라미터 변수 설정 뷰를 제공할 수 있다. _CIFilter Image Kit Additions_ 및 [_ImageKit Programming Guide_](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/ImageKitProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004907)을 참조하라.

