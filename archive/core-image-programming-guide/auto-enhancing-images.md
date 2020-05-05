# Auto Enhancing Images

Core Image의 자동 향상 기능은 히스토그램, 얼굴 영역 내용 및 메타데이터 속성에 대해 이미지를 분석한다. 그런 다음 입력 파라미터가 이미 분석된 이미지를 개선하는 값으로 설정된 [`CIFilter`](https://developer.apple.com/documentation/coreimage/cifilter) 객체의 배열을 반환한다. 자동향상은 iOS v5.0 이상 및 OS X v10.8 이상에서 사용할 수 있다.

### Auto Enhancement Filters

Table 3-1은 이미지가 이미지를 자동으로 개선하는 데 사용하는 필터를 보여준다. 이 필터들은 사진에서 발견되는 가장 흔한 문제들 중 일부를 해결한다.

**Table 3-1** Core Image가 이미지를 개선하는 데 사용하는 필터

| Filter | Purpose |
| :--- | :--- |
| CIRedEyeCorrection | Repairs red/amber/white eye due to camera flash |
| CIFaceBalance | Adjusts the color of a face to give pleasing skin tones |
| CIVibrance | Increases the saturation of an image without distorting the skin tones |
| CIToneCurve | Adjusts image contrast |
| CIHighlightShadowAdjust | Adjusts shadow details |

### Using Auto Enhancement Filters

자동향상 API에는 [`autoAdjustmentFilters`](https://developer.apple.com/documentation/coreimage/ciimage/1645889-autoadjustmentfilters)와 [`autoAdjustmentFiltersWithOptions:`](https://developer.apple.com/documentation/coreimage/ciimage/1437792-autoadjustmentfilters) 두 메서드만 있다. 대부분의 경우 옵션 딕셔너리를 제공하는 방법을 사용하라.

다음과 같은 옵션을 설정할 수 있다:

* `CIRedEyeCorrection` 및 `CIFaceBalance` 필터에 중요한 이미지 방향은 Core Image가 정확하게 얼굴을 찾을 수 있도록 한다.
* 적목교정만 적용할지 여부.\(`kCIImageAutoAdjustEnhance`를 false로 설정하라.\)
* 적목교정을 제외한 모든 필터 적용 여부.\(`kCIImageAutoAdjustRedEye`를 false로 설정하라.\)

[`autoAdjustmentFiltersWithOptions:`](https://developer.apple.com/documentation/coreimage/ciimage/1437792-autoadjustmentfilters) 메서드는Listing 3-1에 표시된 것처럼 서로 연결하여 분석된 이미지에 적용할 옵션 필터 배열을 반환한다. 코드는 먼저 옵션 딕셔너리를 생성한다. 그런 다음 이미지의 방향을 얻고 이 값을 키 `CIDetectorImageOrientation`에 대한값으로 설정한다.

**Listing 3-1**  Getting auto enhancement filters and applying them to an image

```objectivec
NSDictionary *options = @{ CIDetectorImageOrientation :
                 [[image properties] valueForKey:kCGImagePropertyOrientation] };
NSArray *adjustments = [myImage autoAdjustmentFiltersWithOptions:options];
for (CIFilter *filter in adjustments) {
     [filter setValue:myImage forKey:kCIInputImageKey];
     myImage = filter.outputImage;
}
```

입력 파라미터 변수 값이 최상의 결과를 생성하기 위해 이미 Core Image에 의해 설정되었음을 상기하라.

자동조절필터를 바로 적용할 필요는 없다. 나중에 사용할 필터 이름 및 파라미터 값을 저장할 수 있다. 이러한 기능을 저장하면 나중에 앱이 이미지를 다시 분석하지 않고도 향상된 기능을 수행할 수 있다.

