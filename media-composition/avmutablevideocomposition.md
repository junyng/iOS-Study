---
description: 변환가능한 비디오 컴포지션을 나타내는 객체
---

# AVMutableVideoComposition

### Declaration

```swift
class AVMutableVideoComposition : AVVideoComposition
```

### Overview

비디오 컴포지션은 인스트럭션의 총 시간 범위에서 그 시간에 해당하는 컴포지션 비디오 프레임을 생성하기 위해 사용할 비디오 트랙의 수와 ID를 설명한다. AVFoundation의 내장형 비디오 컴포지터를 사용하는 경우, [`AVVideoComposition`](https://developer.apple.com/documentation/avfoundation/avvideocomposition)이 구성하는 지침은 각 비디오 소스에 대해 공간 변환, 불투명도 값 및 자르기 직시각형을 지정할 수 있으며, 이러한 지침은 단순한 선형 램핑 함수를 통해 시간에 따라 달라질 수 있다.

당신은 또한 [`AVVideoCompositing`](https://developer.apple.com/documentation/avfoundation/avvideocompositing) 프로토콜을 구현함으로써 당신만의 커스텀 비디오 컴포지터를 구현할 수 있다. 사용자 지정 비디오 컴포지터는 재생과 다른 작동 동안에 각각의 비디오 소스에 대해 픽셀 버퍼를 제공하고 그것들에 대해 임의의 그래픽 연산을 수행하여 시각 출력을 생성할 수 있다.

#### Creating a Video Composition

[`init(propertiesOf: AVAsset)`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/1388430-init)

지정된 에셋 속성을 사용하여 새 변경 가능한 비디오 컴포지션을 생성한다.

[`init(asset: AVAsset, applyingCIFiltersWithHandler: (AVAsynchronousCIImageFilteringRequest) -> Void)`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/1387006-init)

지정된 에셋의 각 비디오 프레임에 Core Image 필터를 적용하도록 구성된 변경 가능한 비디오 컴포지션을 생성하라.

#### Configuring Video Composition Properties

[`var frameDuration: CMTime`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/1390059-frameduration)

비디오 컴포지션이 구성된 비디오 프레임을 렌더링해야하는 시간 간격.

[`var renderSize: CGSize`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/1386365-rendersize)

비디오 컴포지션이 렌더링해야 하는 크기.

[`var renderScale: Float`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/1615787-renderscale)

비디오 컴포지션이 렌더링해야 하는 척도.

[`var instructions: [AVVideoCompositionInstructionProtocol]`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/1385815-instructions)

비디오 컴포지션 인스트럭션.

[`protocol AVVideoCompositionInstructionProtocol`](https://developer.apple.com/documentation/avfoundation/avvideocompositioninstructionprotocol)

컴포지터가 수행할 작업을 나타내도록 구현할 수 있는 메서드를 정의한 프로토콜.

[`var animationTool: AVVideoCompositionCoreAnimationTool?`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/1390395-animationtool)

오프라인 렌더링에서 Core Animation과 함께 사용하는 비디오 컴포지션 툴.

[`var customVideoCompositorClass: AVVideoCompositing.Type?`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/1390649-customvideocompositorclass)

사용자 정의 컴포지터 클래스.

[`var sourceTrackIDForFrameTiming: CMPersistentTrackID`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/2873799-sourcetrackidforframetiming)

비디오 컴포지션에 대한 프레임 타이밍이 소스의 에셋 트랙에서 파생되는지 여부를 나타내는 값.

[`var colorPrimaries: String?`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/1643234-colorprimaries)

비디오 컴포지션에 사용되는 색상 기본값.

[`var colorTransferFunction: String?`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/1643237-colortransferfunction)

비디오 컴포지션에 사용되는 전송 함수.

[`var colorYCbCrMatrix: String?`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/1643231-colorycbcrmatrix)

비디오 컴포지션에 사용되는 YCbCr 행렬.

#### Initializers

[`init(propertiesOf: AVAsset, prototypeInstruction: AVVideoCompositionInstruction)`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition/3200159-init)





