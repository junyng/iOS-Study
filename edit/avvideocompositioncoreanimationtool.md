---
description: 코어 애니메이션을 비디오 구성에 통합하는 데 사용되는 객체.
---

# AVVideoCompositionCoreAnimationTool

### Declaration

```swift
class AVVideoCompositionCoreAnimationTool : NSObject
```

### Overview

애니메이션은 실시간이 아니라 비디오 타임라인에서 해석되므로 다음 작업을 수행하라:

1. 애니메이션의 [`beginTime`](https://developer.apple.com/documentation/quartzcore/camediatiming/1427654-begintime) 속성이 0이 아닌 [`AVCoreAnimationBeginTimeAtZero`](https://developer.apple.com/documentation/avfoundation/avcoreanimationbegintimeatzero)로 설정하라.\(`CoreAnimation`이 [`CACurrentMediaTime()`](https://developer.apple.com/documentation/quartzcore/1395996-cacurrentmediatime)으로 대체한다.\)
2. [`isRemovedOnCompletion`](https://developer.apple.com/documentation/quartzcore/caanimation/1412458-isremovedoncompletion)을 `false`로 설정하여 자동으로 제거되지 않는다.
3. [`UIView`](https://developer.apple.com/documentation/uikit/uiview) 객체와 연관된 레이어 사용을 피하라.

### Topics

#### Creating a Composition Tool

[`init(additionalLayer: CALayer, asTrackID: CMPersistentTrackID)`](https://developer.apple.com/documentation/avfoundation/avvideocompositioncoreanimationtool/1388345-init)

비디오 컴포지션에 `Core Animation` 레이어를 추가한다.

[`init(postProcessingAsVideoLayer: CALayer, in: CALayer)`](https://developer.apple.com/documentation/avfoundation/avvideocompositioncoreanimationtool/1389594-init)

컴포지트된 비디오 프레임을 `Core Animation` 레이어로 구성한다.

[`init(postProcessingAsVideoLayers: [CALayer], in: CALayer)`](https://developer.apple.com/documentation/avfoundation/avvideocompositioncoreanimationtool/1389778-init)

컴포지트된 비디오 프레임을 `Core Animation` 레이어로 구성한다.

