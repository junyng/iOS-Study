---
description: 여러 파일 기반 소스의 미디어 데이터를 결합하여 여러 소스의 미디어 데이터를 제공하거나 처리하는 객체이다.
---

# AVComposition

### Declaration

```swift
class AVComposition : AVAsset
```

### Overview

`AVComposition`은 타임라인에 따라 특정 미디어 유형\(예: 오디오 또는 비디오\)의 미디어를 표시하는 트랙 모음이다. 각 트랙은 [`AVCompositionTrack`](https://developer.apple.com/documentation/avfoundation/avcompositiontrack)의 인스턴스로 표현된다. 각 트랙은 [`AVCompositionTrackSegment`](https://developer.apple.com/documentation/avfoundation/avcompositiontracksegment)의 인스턴스로 대표되는 일련의 트랙 세그먼트로 구성된다. 각 세그먼트는 URL, 트랙 식별자 및 시간 매핑에 의해 지정된 소스 컨테이너에 저장된 미디어 데이터의 일부를 보여준다. URL은 소스 컨테이너를 지정하고 트랙 식별자는 표시할 소스 컨테이너의 트랙을 나타낸다. 모든 파일 기반 시청각 에셋은 컨테이너 유형에 관계없이 결합될 수 있다.

타임 매핑은 표시할 소스 트랙의 시간 범위를 지정하고 또한 컴포지션 트랙에서 프레젠테이션의 시간 범위를 지정한다. 시간 매핑의 소스 및 타겟 범위가 동일한 경우 세그먼트에 대한 미디어 데이터가 자연 속도로 표시된다. 그렇지 않으면 세그먼트가 source.duration / target.duration 비율과 동일한 속도로 표시된다.

[`AVCompositionTrack`](https://developer.apple.com/documentation/avfoundation/avcompositiontrack)의 [`segments`](https://developer.apple.com/documentation/avfoundation/avcompositiontrack/1387267-segments) 속성\([`AVCompositionTrackSegment`](https://developer.apple.com/documentation/avfoundation/avcompositiontracksegment) 객체의 배열\)을 사용하여 트랙의 세그먼트에 접근할 수 있다. 각각에 대한 미디어 유형 정보가 있는 트랙 컬렉션과 각각의 트랙 세그먼트 배열\(URL, 트랙 식별자 및 타임 매핑\)은 컴포지션에 대한 완전한 저수준의 표현을 형성한다. 이러한 표현은 클라이언트가 편리한 형식으로 작성할 수 있다. 그리고 나중에 새로운 [`AVMutableComposition`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition)를 적절한 미디어 유형의 [`AVMutableCompositionTrack`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack)객체로 인스턴스화하여 컴포지션을 재구성할 수 있으며, 각 객체는 저장된 URL 배열, 트랙 식별자 및 시간 매핑에 따라 세그먼트 속성을 설정한다.

[`AVMutableComposition`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition) 및 [`AVMutableCompositionTrack`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack)에서는 컴포지션 트랙의 트랙 세그먼트 배열을 직접 조작하지 않고 삽입, 제거 및 확장 작업을 제공하는 고수준의 인터페이스도 제시된다. 이 인터페이스는 [`AVAsset`](https://developer.apple.com/documentation/avfoundation/avasset) 및 [`AVAssetTrack`](https://developer.apple.com/documentation/avfoundation/avassettrack)과 같은 상위 수준의 구조를 사용하므로, 클라이언트가 컴포지션에 포함하기 전에 검사하거나 미리 보기 위해 만들었을 후보 소스에 대해 동일한 참조를 허용한다.



#### Setting URL Initialization Options

* [`var urlAssetInitializationOptions: [String : Any]`](https://developer.apple.com/documentation/avfoundation/avcomposition/1387080-urlassetinitializationoptions)  수신자에 의한 URL 에셋 생성을 위한 초기화 옵션

#### Accessing Tracks

* [`var tracks: [AVCompositionTrack]`](https://developer.apple.com/documentation/avfoundation/avcomposition/1390165-tracks)  구성에 포함된 구성 트랙 객체 배열
* [`func track(withTrackID: CMPersistentTrackID) -> AVCompositionTrack?`](https://developer.apple.com/documentation/avfoundation/avcomposition/1388473-track)  지정된 트랙ID와 관련 구성 트랙을 제공한다.
* [`func tracks(withMediaCharacteristic: AVMediaCharacteristic) -> [AVCompositionTrack]`](https://developer.apple.com/documentation/avfoundation/avcomposition/1387525-tracks)  에셋과 관련된 지정된 미디어 특성의 구성 트랙을 제공한다.
* [`func tracks(withMediaType: AVMediaType) -> [AVCompositionTrack]`](https://developer.apple.com/documentation/avfoundation/avcomposition/1386534-tracks)  에셋과 연결된 지정된 미디어 유형의 구성 트랙을 제공한다.

#### Getting the Natural Size of a Composition

* [`var naturalSize: CGSize`](https://developer.apple.com/documentation/avfoundation/avcomposition/1387247-naturalsize)  구성의 시각적 부분의 작성자 크기

