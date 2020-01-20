---
description: 코어 애니메이션을 비디오 컴포지션에 통합하는 데 사용되는 객체.
---

# AVVideoCompositionCoreAnimationTool

### Declaration

```swift
class AVVideoCompositionCoreAnimationTool : NSObject
```

### Overview

애니메이션은 실시간이 아니라 비디오 타임라인에서 해석되므로 다음 작업을 수행하라.

1. 애니메이션 [`beginTime`](https://developer.apple.com/documentation/quartzcore/camediatiming/1427654-begintime) 속성이 0이 아닌 [`AVCoreAnimationBeginTimeAtZero`](https://developer.apple.com/documentation/avfoundation/avcoreanimationbegintimeatzero) \(CoreAnimation이 A 대체\)
2. [`isRemovedOnCompletion`](https://developer.apple.com/documentation/quartzcore/caanimation/1412458-isremovedoncompletion)을 자동으로 제거되지 않도록 애니메이션에서 false로 설정.
3. [`UIView`](https://developer.apple.com/documentation/uikit/uiview) 객체와 연관된 레이어를 사용하지 않는다.

### Topics

#### Creating a Composition Tool

[`init(additionalLayer: CALayer, asTrackID: CMPersistentTrackID)`](https://developer.apple.com/documentation/avfoundation/avvideocompositioncoreanimationtool/1388345-init)

비디오 컴포지션에 코어 애니메이션 레이어 추가

[`init(postProcessingAsVideoLayer: CALayer, in: CALayer)`](https://developer.apple.com/documentation/avfoundation/avvideocompositioncoreanimationtool/1389594-init)

컴포지트된 비디오 프레임을 코어 애니메이션 레이어로 구성한다.

[`init(postProcessingAsVideoLayers: [CALayer], in: CALayer)`](https://developer.apple.com/documentation/avfoundation/avvideocompositioncoreanimationtool/1389778-init)

컴포지트된 비디오 프레임을 코어 애니메이션 레이어로 구성한다.

