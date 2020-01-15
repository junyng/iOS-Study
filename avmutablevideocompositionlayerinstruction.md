---
description: '변형가능한 컴포지션에서 주어진 트랙에 적용된 변환, 자르기 및 불투명도 램프를 수정하는 데 사용되는 객체.'
---

# AVMutableVideoCompositionLayerInstruction

```swift
class AVMutableVideoCompositionLayerInstruction : AVVideoCompositionLayerInstruction
```

### Topics

#### Creating an Instruction

[`init(assetTrack: AVAssetTrack)`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositionlayerinstruction/1389691-init)

주어진 트랙에 대해 새로운 변형가능한 비디오 컴포지션 레이어 인스트럭션을 생성한다.

#### Configuring a Track ID

[`var trackID: CMPersistentTrackID`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositionlayerinstruction/1387222-trackid)

컴포지터가 인스트럭션을 적용하는 소스 트랙의 트랙 식별자.

#### Managing Properties

[`func setOpacity(Float, at: CMTime)`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositionlayerinstruction/1390758-setopacity)

인스트럭션의 시간 범위 내에서 특정 시간에 불투명도 값을 설정한다.

[`func setOpacityRamp(fromStartOpacity: Float, toEndOpacity: Float, timeRange: CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositionlayerinstruction/1387532-setopacityramp)

지정된 시간 범위 동안 적용할 불투명 램프를 설정한다.

[`func setTransform(CGAffineTransform, at: CMTime)`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositionlayerinstruction/1390899-settransform)

인스트럭션의 시간 범위 내에서 한 번에 변환 값을 설정한다.

[`func setTransformRamp(fromStart: CGAffineTransform, toEnd: CGAffineTransform, timeRange: CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositionlayerinstruction/1388192-settransformramp)

주어진 시간 범위 동안 적용할 변환 램프를 설정한다.

#### Setting Crop Rectangle Values

[`func setCropRectangle(CGRect, at: CMTime)`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositionlayerinstruction/1387402-setcroprectangle)

인스트럭션의 시간 범위 내에서 한 번에 자를 직사각형 값을 설정하라.

[`func setCropRectangleRamp(fromStartCropRectangle: CGRect, toEndCropRectangle: CGRect, timeRange: CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositionlayerinstruction/1385677-setcroprectangleramp)

지정된 시간 범위 동안 적용할 자르기 직사각형 램프를 설정하라.

