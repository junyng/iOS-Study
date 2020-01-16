---
description: 컴포지터가 수행하는 작업.
---

# AVMutableVideoCompositionInstruction

### Declaration

```swift
class AVMutableVideoCompositionInstruction : AVVideoCompositionInstruction
```

### Overview

[`AVVideoComposition`](https://developer.apple.com/documentation/avfoundation/avvideocomposition) 객체는 그 컴포지션을 수행하기 위한 일련의 인스트럭션을 유지한다.

#### Configuring the Instructions

[`var backgroundColor: CGColor?`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositioninstruction/1390236-backgroundcolor)

컴포지션의 배경색.

[`var layerInstructions: [AVVideoCompositionLayerInstruction]`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositioninstruction/1388912-layerinstructions)

소스 트랙의 비디오 프레임을 레이어하고 구성하는 방법을 지정하는 일련의 [`AVVideoCompositionLayerInstruction`](https://developer.apple.com/documentation/avfoundation/avvideocompositionlayerinstruction) 인스턴스 배열

[`var timeRange: CMTimeRange`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositioninstruction/1390418-timerange)

인스트럭션이 유효한 시간 범위.

[`var enablePostProcessing: Bool`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositioninstruction/1385876-enablepostprocessing)

비디오 컴포지션 인스트럭션이 사후 처리가 필요한지 여부를 나타내는 불 값.

