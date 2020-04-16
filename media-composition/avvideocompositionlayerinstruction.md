---
description: '컴포지션에서 주어진 트랙에 적용된 변환, 자르기 및 불투명도 램프를 수정하는 데 사용되는 객체.'
---

# AVVideoCompositionLayerInstruction

### Declaration

```swift
class AVVideoCompositionLayerInstruction : NSObject
```

### Topics

#### Getting the Track ID

[`var trackID: CMPersistentTrackID`](https://developer.apple.com/documentation/avfoundation/avvideocompositionlayerinstruction/1390240-trackid)

컴포지터가 인스트럭션을 적용할 소스 트랙의 트랙 식별자.

#### Getting Opacity, Transform, and Cropping Ramps

[`func getOpacityRamp(for: CMTime, startOpacity: UnsafeMutablePointer<Float>?, endOpacity: UnsafeMutablePointer<Float>?, timeRange: UnsafeMutablePointer<CMTimeRange>?) -> Bool`](https://developer.apple.com/documentation/avfoundation/avvideocompositionlayerinstruction/1388471-getopacityramp)

지정된 시간을 포함하는 불투명도 램프를 얻는다.

[`func getTransformRamp(for: CMTime, start: UnsafeMutablePointer<CGAffineTransform>?, end: UnsafeMutablePointer<CGAffineTransform>?, timeRange: UnsafeMutablePointer<CMTimeRange>?) -> Bool`](https://developer.apple.com/documentation/avfoundation/avvideocompositionlayerinstruction/1387257-gettransformramp)

지정된 시간을 포함하는 변환 램프를 얻다.

[`func getCropRectangleRamp(for: CMTime, startCropRectangle: UnsafeMutablePointer<CGRect>?, endCropRectangle: UnsafeMutablePointer<CGRect>?, timeRange: UnsafeMutablePointer<CMTimeRange>?) -> Bool`](https://developer.apple.com/documentation/avfoundation/avvideocompositionlayerinstruction/1387998-getcroprectangleramp)

지정된 시간을 포함하는 크롭 직사각형 램프를 얻는다.

