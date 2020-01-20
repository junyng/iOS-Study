---
description: 재생과 독립적으로 에셋의 미리 보기 또는 축소 이미지를 제공하는 객체.
---

# AVAssetImageGenerator

### Declaration

```swift
class AVAssetImageGenerator : NSObject
```

### Overview

`AVAssetImageGenerator`는 기본 활성화된 비디오 트랙\(들\)을 사용하여 이미지를 생성한다. 단일 이미지를 분리하여 생성하려면 복잡한 상호의존성을 가진 다수의 비디오 프레임을 디코딩 해야한다. 일련의 이미지가 필요할 경우 비동기식 메서드를 사용하여 훨씬 더 높은 효율성을 달성할 수 있으며, [`generateCGImagesAsynchronously(forTimes:completionHandler:)`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1388100-generatecgimagesasynchronously)메서드를 통해 재생 중에 사용된 것과 유사한 디코딩 효율을 사용한다.

init 또는 Asset 을 사용하여 에셋 생성기를 생성한다. 이 메서드는 초기화 당시 에셋에 시각적 트랙이 없더라도 성공할 수 있다. [`AVAsset`](https://developer.apple.com/documentation/avfoundation/avasset)의 클래스 메서드 [`tracks(withMediaCharacteristic:)`](https://developer.apple.com/documentation/avfoundation/avasset/1389554-tracks)를 사용하여 에셋에 시각적 특성이 있는 트랙이 있는지 테스트할 수 있다.

생성된 이미지의 실제 시간이 \[`requestedTime`-[`requestedTimeToleranceBefore`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1390571-requestedtimetolerancebefore), `requestedTime`+[`requestedTimeToleranceAfter`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1387751-requestedtimetoleranceafter) \] 범위에 있으면 효율성을 위해 요청한 시간과 다를 수 있다.

### Topics

#### Creating an Image Generator

[`init(asset: AVAsset)`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1387855-init)

지정된 에셋과 함께 사용할 이미지 에셋을 생성한다.

#### Generating Images

[`func copyCGImage(at: CMTime, actualTime: UnsafeMutablePointer<CMTime>?) -> CGImage`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1387303-copycgimage)

지정된 시간 또는 그 근처에서 에셋에 대한 이미지를 반환한다.

[`func generateCGImagesAsynchronously(forTimes: [NSValue], completionHandler: AVAssetImageGeneratorCompletionHandler)`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1388100-generatecgimagesasynchronously)

지정된 시간 또는 그 부근에 에셋에 대한 일련의 이미지 객체를 작성한다.

[`typealias AVAssetImageGeneratorCompletionHandler`](https://developer.apple.com/documentation/avfoundation/avassetimagegeneratorcompletionhandler)

에셋에서 생성된 축소 이미지를 수신하는 데 사용하는 블록.

[`enum AVAssetImageGenerator.Result`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/result)

이미지 생성 결과를 나타내는 상태.

[`func cancelAllCGImageGeneration()`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1385859-cancelallcgimagegeneration)

보류 중인 모든 이미지 생성 요청을 취소한다.

#### Managing Image-Generation Time Tolerances

[`var requestedTimeToleranceBefore: CMTime`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1390571-requestedtimetolerancebefore)

요청된 시간 전에 이미지를 생성할 수 있는 최대 시간.

[`var requestedTimeToleranceAfter: CMTime`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1387751-requestedtimetoleranceafter)

요청한 시간 이후 이미지를 생성할 수 있는 최대 시간.

#### Configuring Image-Generation Behavior

[`var apertureMode: AVAssetImageGenerator.ApertureMode?`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1389314-aperturemode)

생성된 이미지에 대해 지정하는 조리개 모드.

[`struct AVAssetImageGenerator.ApertureMode`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/aperturemode)

이미지를 생성할 때 사용

[`var appliesPreferredTrackTransform: Bool`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1390616-appliespreferredtracktransform)

에셋에서 이미지를 추출할 때 트랙 행렬 또는 행렬을 적용할지 여부를 지정하는 불 값.

[`var asset: AVAsset`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1390689-asset)

이미지 생성기에 의해 초기화된 에셋.

[`var maximumSize: CGSize`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1387560-maximumsize)

생성된 이미지의 최대 치수.

[`var videoComposition: AVVideoComposition?`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1390189-videocomposition)

여러 비디오 트랙이 있는 에셋에서 영상을 추출할 때 사용하는 비디오 컴포지션.

[`var customVideoCompositor: AVVideoCompositing?`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator/1386469-customvideocompositor)

사용된 사용자 지정 비디오 컴포지터 인스턴스\(있는 경우\)를 반환.

