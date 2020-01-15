---
description: 컴포지터가 수행하는 작업.
---

# AVVideoCompositionInstruction

### Declaration

```swift
class AVVideoCompositionInstruction : NSObject
```

### Overview

[`AVVideoComposition`](https://developer.apple.com/documentation/avfoundation/avvideocomposition) 객체는 컴포지션을 수행하기 위한 [`instructions`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1389211-instructions) 배열을 유지한다.

### Topics

#### Getting Composition Instruction Properties

[`var backgroundColor: CGColor?`](https://developer.apple.com/documentation/avfoundation/avvideocompositioninstruction/1389384-backgroundcolor)

컴포지션의 배경색.

[`var layerInstructions: [AVVideoCompositionLayerInstruction]`](https://developer.apple.com/documentation/avfoundation/avvideocompositioninstruction/1389689-layerinstructions)

소스 트랙의 비디오 프레임이 계층화되고 구성되어야하는 비디오 컴포지션 레이어 인스트럭션 인스턴스의 배열.

[`var timeRange: CMTimeRange`](https://developer.apple.com/documentation/avfoundation/avvideocompositioninstruction/1387857-timerange)

인스트럭션이 유효한 시간 범위.

[`var enablePostProcessing: Bool`](https://developer.apple.com/documentation/avfoundation/avvideocompositioninstruction/1388697-enablepostprocessing)

비디오 컴포지션 인스트럭션에 사후 처리가 필요한지 여부를 나타내는 불 값.

[`var passthroughTrackID: CMPersistentTrackID`](https://developer.apple.com/documentation/avfoundation/avvideocompositioninstruction/1387657-passthroughtrackid)

인스트럭션 소스 프레임의 트랙 식별자.

[`var requiredSourceTrackIDs: [NSValue]`](https://developer.apple.com/documentation/avfoundation/avvideocompositioninstruction/1390913-requiredsourcetrackids)

인스트럭션에 대한 프레임을 구성하는 데 필요한 트랙 식별자 배열.

