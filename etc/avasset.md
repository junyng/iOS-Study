---
description: AVAsset은 에셋을 구성하는 트랙의 집합 속성을 정의한다.
---

# AVAsset

### Declaration

```swift
class AVAsset : NSObject
```

### Overview

`AVAsset`은 에셋을 구성하는 트랙의 집합 속성을 정의합니다.다음 예에서와 같이 미디어 리소스를 가리키는 로컬 또는 원격 URL로 초기화하여 에셋을 초기화한다.

```swift
let url: URL = // Local or Remote Asset URL
let asset = AVAsset(url: url)
```

`AVAsset`은 추상적인 클래스이므로, 예제에서 보여지는 대로 자산을 만들 때, 실제로 [`AVURLAsset`](https://developer.apple.com/documentation/avfoundation/avurlasset)이라고 불리는 구체적인 하위 클래스 중 하나의 인스턴스를 만든다. 많은 경우 에셋을 만드는 데 적합한 방법이지만 초기화에 대한 보다 세밀한 통제가 필요할 때 `AVURLAsset`을 직접 인스턴스화할 수도 있다. `AVURLAsset`의 생성자는 옵션 딕셔너리를 수용하며, 이 딕셔너리를 통해 에셋의 초기화를 특정 사용 사례에 맞게 조정한다. 예를 들어, HLS 스트림에 대한 에셋을 만들고 있는 경우, 사용자가 셀룰러 네트워크에 연결될 때 미디어를 검색하지 못하도록 할 수 있다. 다음 예제에서 보듯이 이 작업을 할 수 있다.

```swift
let url: URL = // Remote Asset URL
let options = [AVURLAssetAllowsCellularAccessKey: false]
let asset = AVURLAsset(url: url, options: options)
```

시간 편집을 위해 `AVComposition`이 하는 것처럼 또한 시청각 매체의 기본 모델을 유용한 방법으로 확장하는 다른 구체적인 하위 클래스를 사용하여 에셋을 인스턴스화할 수 있다.

시한 시청각 미디어의 특성상 에셋을 성공적으로 초기화하면 AVComposition이 시간 편집을 위해 사용하는 것처럼 키의 일부 또는 전부 값을 즉시 사용할 수 없다. 어떤 키의 값도 언제든지 요청할 수 있으며, 에셋은 호출 쓰레드를 차단해야할 수 있지만 항상 동기적으로 값을 반환한다. 차단을 피하기 위해 특정 키에 대한 관심을 등록하고 해당 값이 사용 가능해질 때 알림을 받을 수 있다. 더 자세한 세부사항은 [`AVAsynchronousKeyValueLoading`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading)을 참조하라.

`AVAsset`의 인스턴스를 재생하려면 [`AVPlayerItem`](https://developer.apple.com/documentation/avfoundation/avplayeritem)의 인스턴스를 초기화하고 플레이어 항목을 사용하여 프리젠테이션 상태 설정\(예: 에셋의 제한된 시간 범위만 재생해야 하는지 여부\) 및 해당 아이템이 자체적으로 재생될 것인지 또는 다른 아이템들의 집합으로 재생될 것인지에 따라 상기 플레이어 아이템을 [`AVPlayer`](https://developer.apple.com/documentation/avfoundation/avplayer) 객체에 제공하는 단계를 포함한다.

`AVAsset` 객체를 [`AVMutableComposition`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition) 객체에 삽입하여 하나 이상의 소스 에셋에서 시청각 구성을 조합할 수 있다.

#### Subclassing Notes <a id="1919546"></a>

프레임워크에서 지원되지 않는 스트리밍 프로토콜 또는 파일 형식을 처리하기 위해 현재 `AVAsset`을 하위 클래스로 지정할 수 없다.

