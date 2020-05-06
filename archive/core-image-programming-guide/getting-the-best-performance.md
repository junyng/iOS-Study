# Getting the Best Performance

Core Image는 이미지, 컨텍스트 및 렌더링 콘텐츠를 만드는 데 많은 옵션을 제공한다. 작업을 수행하는 방법을 선택하는 방법은 다음과 같다:

* 프로그램이 작업을 수행하는 데 필요한 빈도
* 앱이 스틸 이미지 또는 비디오 이미지와 함께 작동하는지 여부
* 실시간 처리 또는 분석을 지원해야 하는지 여부
* 사용자들에게 얼마나 중요한 색상 정확도가 있는지

앱을 최대한 효율적으로 실행하려면 performance best practices를 숙지하라.

### Performance Best Practices

최상의 성능을 얻으려면 다음 절차를 따라야 한다:

* 렌더링할 때마다 `CIContext` 객체를 만들지 마라. 컨텍스트는 많은 상태 정보를 저장한다. 그것을 재사용하는 것이 더 효율적이다.
* 앱에 색상 관리가 필요한지 여부를 평가하라. 필요하지 않으면 사용하지 마라. 자세한 내용은 [Does Your App Need Color Management?](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_performance/ci_performance.html#//apple_ref/doc/uid/TP30001185-CH10-SW7)를 참조하라.
* GPU 컨텍스트로 `CIImage` 객체를 렌더링하는 동안 Core Animation을 피하라. 두 가지를 동시에 사용해야 하는 경우 둘 다 CPU를 사용하도록 설정할 수 있다.
* 이미지가 CPU 및 GPU 제한을 초과하지 않도록 하라. CIContext 객체의 이미지 크기 제한은 Core Image가 CPU 또는 GPU를 사용하는지에 따라 다르다. [`inputImageMaximumSize`](https://developer.apple.com/documentation/coreimage/cicontext/1620425-inputimagemaximumsize) 및 [`outputImageMaximumSize`](https://developer.apple.com/documentation/coreimage/cicontext/1620335-outputimagemaximumsize) 메서드를 사용하여 한계를 확인하라.
* 가능하면 작은 이미지를 사용하라. 출력 픽셀 수에 따라 성능이 오른다. Core Image를 더 작은 뷰, 텍스처 또는 프레임 버퍼로 렌더링할 수 있다. Core Animation이 크기를 표시하도록 확장하라. Core Graphics 또는 Image I/O 함수\([`CGImageCreateWithImageInRect`](https://developer.apple.com/documentation/coregraphics/cgimage/1454683-cropping) 또는[`CGImageSourceCreateThumbnailAtIndex`](https://developer.apple.com/documentation/imageio/1465099-cgimagesourcecreatethumbnailatin)\)를 사용하여 자르거나 다운샘플링하라.
* `UIImageView` 클래스는 정적 이미지에서 가장 잘 작동한다. 앱이 최상의 성능을 얻으려면 저수준의 API를 사용하라.
* CPU 및 GPU 간에 불필요한 텍스처가 전송되지 않도록 하라.
* 스케일 팩터를 적용하기 전에 원본 이미지와 동일한 크기의 직사각형으로 렌더링하라.
* 알고리즘 필터와 유사한 결과를 생성할 수 있는 더 간단한 필터를 사용하는 것을 고려하라. 예를 들어, CIColorCube는 CISepiaTone과 유사한 출력을 생산할 수 있으며, 이를 보다 효율적으로 수행할 수 있다.
* iOS 6.0 이상에서 YUV 이미지 지원을 활용하라. 카메라 픽셀 버퍼는 기본적으로 YUV이지만 대부분의 이미지 처리 알고리즘은 RBGA 데이터를 예측한다. 둘 사이를 전환하는데 비용이 든다. Core Image는 CVPixelBuffer 객체에서 YUB를 판독하고 적절한 색상 변환을 적용할 수 있도록 지원한다.

```objectivec
options = @{ (id)kCVPixelBufferPixelFormatTypeKey :
    @(kCVPixelFormatType_420YpCbCr88iPlanarFullRange) };
```

### Does Your App Need Color Management?

기본적으로, Core Image는 모든 필터를 광선형 색 공간에 적용한다. 이것은 가장 정확하고 일관된 결과를 제공한다.

sRGB로의 변환과 sRGB로부터의 변환은 필터 복잡성을 가중시키고, 다음과 같은 방정식을 적용하기 위해 Core Image가 필요하다.

```objectivec
rgb = mix(rgb.0.0774, pow(rgb*0.9479 + 0.05213, 2.4), step(0.04045, rgb))
rgb = mix(rgb12.92, pow(rgb*0.4167) * 1.055 - 0.055, step(0.00313, rgb))
```

다음과 같은 경우 색 관리를 비활성화하는 것을 고려하라:

* 앱은 절대적으로 높은 성능을 필요로 한다.
* 사용자들은 과장된 조작 후에 품질 차이를 알아차리지 못할 것이다.

색 관리를 비활성화하려면 kCIImageColorSpace 키를 null로 설정한다. EAGL 컨텍스트를 사용하는 경우, EAGL 컨텍스트 색 공간을 null로 설정한다. [Building Your Own Workflow with a Core Image Context](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html#//apple_ref/doc/uid/TP30001185-CH3-SW5)를 참조하라.

