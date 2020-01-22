---
description: 오프라인 렌더링에서 Core Animation과 함께 사용하는 비디오 구성 도구.
---

# animationTool

### Declaration

```swift
var animationTool: AVVideoCompositionCoreAnimationTool? { get }
```

### Discussion

이 속성은 `nil`이 될것이다.  [`AVAssetExportSession`](https://developer.apple.com/documentation/avfoundation/avassetexportsession)을 [`AVPlayer`](https://developer.apple.com/documentation/avfoundation/avplayer)가 아닌 오프라인 렌더링을 위한 세션과 함께 사용하는 경우 애니메이션 도구를 설정하라.

### See Also

#### Configuring Video Composition Properties

[`var frameDuration: CMTime`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1388013-frameduration)

비디오 컴포지션이 합성된 비디오 프레임을 렌더링해야 하는 시간 간격.

[`var renderSize: CGSize`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1388705-rendersize)

비디오 컴포지션이 렌더링해야할 크기.

[`var renderScale: Float`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1615786-renderscale)

비디오 컴포지션이 렌더링해야할 척도.

[`var instructions: [AVVideoCompositionInstructionProtocol]`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1389211-instructions)

비디오 컴포지션 인스트럭션.

[`protocol AVVideoCompositionInstructionProtocol`](https://developer.apple.com/documentation/avfoundation/avvideocompositioninstructionprotocol)

컴포지터가 수행할 작업을 나타내기 위해 구현할 수 있는 메서드.

[`var customVideoCompositorClass: AVVideoCompositing.Type?`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1389622-customvideocompositorclass)

사용할 커스텀 컴포지터 클래스.

[`var sourceTrackIDForFrameTiming: CMPersistentTrackID`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/2873798-sourcetrackidforframetiming)

비디오 컴포지션에 대한 프레임 타이밍이 소스의 에셋 트랙에서 파생되는지 여부를 나타내는 값.

[`var colorPrimaries: String?`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1643235-colorprimaries)

비디오 컴포지션에 사용되는 색상 기본값.

[`var colorTransferFunction: String?`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1643230-colortransferfunction)

비디오 컴포지션에 사용되는 전송 함수.

[`var colorYCbCrMatrix: String?`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1643236-colorycbcrmatrix)

비디오 컴포지션에 사용되는 YCbCr 행렬.

