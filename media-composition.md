---
description: '단일 구성으로 여러 소스의 오디오 및 비디오 트랙을 결합, 편집 및 리믹스한다.'
---

# Media Composition

### Topics

#### Media Composition

[`class AVComposition`](https://developer.apple.com/documentation/avfoundation/avcomposition)

여러 파일 기반 소스의 미디어 데이터를 결합하여 여러 소스의 미디어 데이터를 표시하거나 처리하는 객체.

[`class AVCompositionTrack`](https://developer.apple.com/documentation/avfoundation/avcompositiontrack)

미디어 유형, 트랙 식별자 및 트랙 세그먼트로 구성된 컴포지션 객체의 트랙.

[`class AVCompositionTrackSegment`](https://developer.apple.com/documentation/avfoundation/avcompositiontracksegment)

URL, 트랙 식별자 및 소스 트랙에서 구성 트랙까지의 시간 매핑으로 구성된 트랙의 세그먼트.

[`class AVMutableComposition`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition)

기존 에셋에서 새 컴포지션을 생성하는 데 사용되는 변형가능한 객체.

[`class AVMutableCompositionTrack`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack)

저 수준의 표현에 영향을 미치지 않고 트랙 세그먼트를 삽입, 제거 및 축척하는 데 사용하는 컴포지션 객체의 변형가능한 트랙.

#### Video Composition

[`class AVVideoComposition`](https://developer.apple.com/documentation/avfoundation/avvideocomposition)

불변의 비디오 컴포지션을 나타내는 객체.

[`class AVMutableVideoComposition`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition)

변형가능한 비디오 컴포지션을 나타내는 객체.

[`class AVAsynchronousCIImageFilteringRequest`](https://developer.apple.com/documentation/avfoundation/avasynchronousciimagefilteringrequest)

Core Image 필터를 사용하여 비디오 컴포지션에서 개별 비디오 프레임을 처리하는 객체.

[`class AVAsynchronousVideoCompositionRequest`](https://developer.apple.com/documentation/avfoundation/avasynchronousvideocompositionrequest)

비디오 컴포지터가 출력 픽셀 버퍼를 렌더링하는 데 필요한 정보를 포함하는 객체.

[`class AVMutableVideoCompositionInstruction`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositioninstruction)

컴포지터가 수행하는 작업.

[`class AVMutableVideoCompositionLayerInstruction`](https://developer.apple.com/documentation/avfoundation/avmutablevideocompositionlayerinstruction)

변형가능한 컴포지션에서 주어진 트랙에 적용된 변환, 자르기 및 불투명도 램프를 수정하는 데 사용되는 객체.

[`class AVVideoCompositionCoreAnimationTool`](https://developer.apple.com/documentation/avfoundation/avvideocompositioncoreanimationtool)

코어 애니메이션을 비디오 구성에 통합하는 데 사용되는 객체.

[`class AVVideoCompositionInstruction`](https://developer.apple.com/documentation/avfoundation/avvideocompositioninstruction)

컴포지터가 수행하는 작업.

[`class AVVideoCompositionLayerInstruction`](https://developer.apple.com/documentation/avfoundation/avvideocompositionlayerinstruction)

컴포지션에서 주어진 트랙에 적용된 변환, 자르기 및 불투명도 램프를 수정하는 데 사용되는 객체.

[`class AVVideoCompositionRenderContext`](https://developer.apple.com/documentation/avfoundation/avvideocompositionrendercontext)

사용자 지정 컴포지터가 새 출력 픽셀 버퍼를 렌더링하는 컨텍스트를 정의하는 객체.

#### Movie Editing

[`class AVMutableMovie`](https://developer.apple.com/documentation/avfoundation/avmutablemovie)

QuickTime 또는 ISO 기반 미디어 파일 형식을 준수하는 시청각 컨테이너를 나타내는 변경가능한 객체.

[`class AVMutableMovieTrack`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack)

QuickTime 또는 ISO 기반 미디어 파일 형식을 준수하는 변경가능한 트랙.

#### Audio Mixing

[`class AVAudioMix`](https://developer.apple.com/documentation/avfoundation/avaudiomix)

오디오 트랙을 혼합하기 위한 입력 매개 변수를 관리하는 객체.

[`class AVAudioMixInputParameters`](https://developer.apple.com/documentation/avfoundation/avaudiomixinputparameters)

혼합물에 오디오 트랙을 추가할 때 적용하는 매개 변수를 나타내는 객체.

[`class AVMutableAudioMix`](https://developer.apple.com/documentation/avfoundation/avmutableaudiomix)

오디오 트랙을 혼합하기 위한 입력 매개변수를 관리하는 객체.

[`class AVMutableAudioMixInputParameters`](https://developer.apple.com/documentation/avfoundation/avmutableaudiomixinputparameters)

혼합물에 오디오 트랙을 추가할 때 사용하는 매개 변수.

