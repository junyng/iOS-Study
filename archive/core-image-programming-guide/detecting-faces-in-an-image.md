# Detecting Faces in an Image

Core Image는 이미지에서 사람의 얼굴을 분석하고 찾을 수 있다. 이는 인식이 아닌 얼굴 검출을 수행한다. 얼굴 **검출**은 사람의 얼굴 특징을 포함하는 직사각형의 식별인 반면, 얼굴 **인식**은 특정한 사람의 얼굴을 식별하는 것이다 \(John, Mary 등\). Core Image가 얼굴을 감지한 후 눈 및 입 위치와 같은 얼굴 형상에 대한 정보를 제공할 수 있다. 또한 비디오에서 식별된 얼굴 위치를 추적할 수 있다.

**Figure 2-1**  Core Image identifies face bounds in an image

![](../../.gitbook/assets/face_detection.png)

이미지의 얼굴 위치를 알면 얼굴의 이미지 품질을 자르거나 조정하는 등 다른 작업을 수행할 수 있다\(톤 밸런스, 적목 현상 보정 등\). 또한 얼굴에서 다른 흥미로운 동작을 할 수 있다; 예를 들어:

* [Anonymous Faces Filter Recipe](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_filer_recipes/ci_filter_recipes.html#//apple_ref/doc/uid/TP30001185-CH4-SW22)는 이미지의 얼굴에만 픽셀레이트 필터를 적용하는 방법을 보여준다.
* [White Vignette for Faces Filter Recipe](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_filer_recipes/ci_filter_recipes.html#//apple_ref/doc/uid/TP30001185-CH4-SW12)는 얼굴에 vignette를 붙이는 방법을 보여준다.

> **참고:** 얼굴 검출은 iOS 5.0 이상 및 OS X 10.7 이상에서 사용할 수 있다.

### Detecting Faces

Listing 2-1에 표시된 것처럼 이미지에서 얼굴을 찾기 위해 [`CIDetector`](https://developer.apple.com/documentation/coreimage/cidetector) 클래스를 사용하라.

**Listing 2-1**  얼굴 검출기 생성하기

```objectivec
CIContext *context = [CIContext context];                    // 1
NSDictionary *opts = @{ CIDetectorAccuracy : CIDetectorAccuracyHigh };      // 2
CIDetector *detector = [CIDetector detectorOfType:CIDetectorTypeFace
                                          context:context
                                          options:opts];                    // 3
 
opts = @{ CIDetectorImageOrientation :
          [[myImage properties] valueForKey:kCGImagePropertyOrientation] }; // 4
NSArray *features = [detector featuresInImage:myImage options:opts];        // 5
```

코드가하는 일은 다음과 같다:

1. 기본 옵션으로 컨텍스트를 생성한다. [Processing Images](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html#//apple_ref/doc/uid/TP30001185-CH3-TPXREF101)에 설명된 대로 컨텍스트-생성 함수를 사용할 수 있다. 검출기를 생성할 때 컨텍스트 대신 `nil`을 공급할 수도 있다.
2. 검출기에 정확도를 지정하기 위해 옵션 딕셔너리를 생성한다. 정확도가 낮거나 높음을 지정할 수 있다. 낮은 정확도\(`CIDetectorAccuracyLow`\)는 빠르며, 이 예제에서 보여지는 높은 정확도는 철저하지만 느리다.
3. 얼굴 검출기를 생성한다. 당신이 만들 수 있는 유일한 유형의 검출기는 사람의 얼굴을 위한 것이다.
4. 얼굴 찾기 옵션 딕셔너리를 설정한다. Core Image가 이미지 방향을 알 수 있도록 하는 것이 중요하다. 그래서 검출기는 직립 얼굴을 찾을 수 있는 곳을 안다. 대부분의 경우 이미지 자체에서 이미지 방향을 읽은 다음 옵션 딕셔너리에 해당 값을 제공한다.
5. 검출기를 사용하여 이미지의 형상을 찾아라. 제공하는 이미지는 CIImage 객체여야 한다. Core Image는 [`CIFeature`](https://developer.apple.com/documentation/coreimage/cifeature) 객체의 배열을 반환하며, 각 객체는 이미지의 얼굴을 나타낸다.

여러 개의 얼굴들을 얻은 후에, 아마도 눈과 입이 어디에 위치해 있는지와 같은 그들의 특징들을 알아내고 싶을 것이다. 다음 섹션에서는 방법을 설명한다.

### Getting Face and Face Feature Bounds

얼굴 형상은 포함한다:

* 왼쪽과 오른쪽 눈의 위치
* 입의 위치
* 비디오 세그먼트\(iOS v6.0 이상 및 OS X v10.8 이상에서 사용 가능\)에서 Core Image가 얼굴을 따라가는 데 사용하는 추적 ID 및 추적 프레임 수

CIDetector 객체에서 얼굴 형상의 배열을 얻은 후, Listing 2-2와 같이 각 얼굴 및 얼굴 안의 각 형상의 바운드를 조사하기 위해 배열을 루프할 수 있다.

**Listing 2-2**  Examining face feature bounds

```objectivec
for (CIFaceFeature *f in features) {
    NSLog(@"%@", NSStringFromRect(f.bounds));
 
    if (f.hasLeftEyePosition) {
        NSLog(@"Left eye %g %g", f.leftEyePosition.x, f.leftEyePosition.y);
    }
    if (f.hasRightEyePosition) {
        NSLog(@"Right eye %g %g", f.rightEyePosition.x, f.rightEyePosition.y);
    }
    if (f.hasMouthPosition) {
        NSLog(@"Mouth %g %g", f.mouthPosition.x, f.mouthPosition.y);
    }
}
```



