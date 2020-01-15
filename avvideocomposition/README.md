---
description: 불변의 비디오 구성을 나타내는 객체입니다.
---

# AVVideoComposition

### Declaration

```swift
class AVVideoComposition : NSObject
```

### Overview

`AVFoundation` 프레임워크는 또한 새로운 비디오를 만드는데 사용할 수 있는 mutable 서브클래스인 [`AVMutableVideoComposition`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition)을 제공한다.

비디오 구성은 그 명령의 총 시간 범위에서 임의의 시간 동안 그 시간에 대응하는 구성된 비디오 프레임을 생성하기 위해 사용될 비디오 트랙의 수 및 ID를 기술한다. `AVFoundation`의 내장형 비디오 구성자를 사용할 때, `AVVideoComposition`에 포함된 명령어는 각 비디오 소스에 대해 공간 변환, 불투명도 값 및 자르기 직사각형을 지정할 수 있으며, 이는 단순한 선형 ramping 함수를 통해 시간이 지남에 따라 달라질 수 있다.

당신은 [`AVVideoCompositing`](https://developer.apple.com/documentation/avfoundation/avvideocompositing) 프로토콜을 구현함으로써 커스텀 비디오 구성자를 구현할 수 있다. 사용자 지정 비디오 구성자는 재생 및 기타 작동 중에 각각의 비디오 소스에 대해 픽셀 버퍼를 제공하고 시각적 출력을 생성하기 위해 그것들에 대해 임의의 그래픽 연산을 수행할 수 있다.

#### Creating a Video Composition Object

[`init(propertiesOf: AVAsset)`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1385892-init)

지정된 에셋의 비디오 트랙을 표시하도록 구성된 비디오 컴포지션 객체를 생성한다.

[`init(asset: AVAsset, applyingCIFiltersWithHandler: (AVAsynchronousCIImageFilteringRequest) -> Void)`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1389556-init)

지정된 에셋의 각 비디오 프레임에 Core Image 필터를 적용하도록 구성된 비디오 컴포지션을 생성한다.

#### Configuring Video Composition Properties

[`var frameDuration: CMTime`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1388013-frameduration)

비디오 컴포지션이 합성된 비디오 프레임을 렌더링해야 하는 시간 간격.

[`var renderSize: CGSize`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1388705-rendersize)

비디오 컴포지션이 렌더링해야 하는 크기.

[`var renderScale: Float`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1615786-renderscale)

비디오 컴포지션이 렌더링해야 하는 척도.

[`var instructions: [AVVideoCompositionInstructionProtocol]`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1389211-instructions)

비디오 컴포지션 명령어.

[`protocol AVVideoCompositionInstructionProtocol`](https://developer.apple.com/documentation/avfoundation/avvideocompositioninstructionprotocol)

컴포지터가 수행할 작업을 나타내기 위해 구현할 수 있는 메서드.

[`var animationTool: AVVideoCompositionCoreAnimationTool?`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1387030-animationtool)

오프라인 렌더링에서 코어 애니메이션과 함께 사용할 비디오 컴포지션 툴.

[`var customVideoCompositorClass: AVVideoCompositing.Type?`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1389622-customvideocompositorclass)

사용할 사용자 정의 컴포지터 클래스.

[`var sourceTrackIDForFrameTiming: CMPersistentTrackID`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/2873798-sourcetrackidforframetiming)

비디오 컴포지션의 프레임 타이밍이 소스의 에셋 트랙에서 파생되었는지 여부를 나타내는 값.

[`var colorPrimaries: String?`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1643235-colorprimaries)

비디오 컴포지션에 사용되는 색상 기본값.

[`var colorTransferFunction: String?`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1643230-colortransferfunction)

비디오 컴포지션에 사용되는 전달 함수.

[`var colorYCbCrMatrix: String?`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1643236-colorycbcrmatrix)

비디오 컴포지션에 사용되는 YCbCr 행렬.

#### Validating the Time Range

[`func isValid(for: AVAsset?, timeRange: CMTimeRange, validationDelegate: AVVideoCompositionValidationHandling?) -> Bool`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1389917-isvalid)

컴포지션 인스트럭션의 시간 범위가 검증 요건을 준수하는지 여부를 나타낸다.

[`protocol AVVideoCompositionValidationHandling`](https://developer.apple.com/documentation/avfoundation/avvideocompositionvalidationhandling)

특정 오류가 발견된 후에도 비디오 컴포지션의 유효성 검사가 계속되어야 하는지 여부를 나타내기 위해 구현할 수 있는 메서드.

