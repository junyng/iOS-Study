---
description: 호출 쓰레드를 차단하지 않고 에셋 또는 에셋 트랙을 사용하기 위해 구현할 수 있는 메서드.
---

# AVAsynchronousKeyValueLoading

### Declaration

```swift
protocol AVAsynchronousKeyValueLoading
```

### Overview

이 프로토콜에는 키의 상태\(비동기식으로 키, 값 로딩을 사용하는 클래스의 모든 속성\)를 찾는 데 사용할 수 있는 메서드가 포함되어 있다. 예를 들어 키의 값이 로드되었는지 여부를 확인할 수 있다. 또한 객체에 값을 비동기식으로 로드하도록 요청하여 작업이 완료되면 알려줄 수 있다.

시간적 시청각 미디어의 특성 때문에 에셋의 성공적인 초기화가 반드시 모든 데이터를 즉시 사용할 수 있다는 것을 의미하지는 않는다. 대신 에셋은 관련 A 메서드를 직접 호출하고, [`AVPlayerItem`](https://developer.apple.com/documentation/avfoundation/avplayeritem) 객체를 통해 재생하고, [`AVAssetExportSession`](https://developer.apple.com/documentation/avfoundation/avassetexportsession)를 사용하여 내보내기, [`AVAssetReader`](https://developer.apple.com/documentation/avfoundation/avassetreader)인스턴스를 사용하여 읽기 등의 작업을 수행할 때까지 데이터 로딩을 기다린다. 어떤 키의 값도 언제든지 요청할 수 있고 그 값도 동시에 반환되지만, 요청이 충족될 때까지 호출 쓰레드가 차단될 수 있다. 차단을 피하기 위해:

* 값이 아직 로드되지 않은 경우 [`loadValuesAsynchronously(forKeys:completionHandler:)`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading/1387321-loadvaluesasynchronously)메서드를 사용하여 에셋에 하나 이상의 값을 비동기식으로 로드하도록 요청하고 사용 가능해지면 사용자에게 알려라.
* [`statusOfValue(forKey:error:)`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading/1386816-statusofvalue)메서드를 사용하여 주어진 키에 대한 값이 사용가능한지 여부를 결정한다.

다음 예제는 에셋의 [`isPlayable`](https://developer.apple.com/documentation/avfoundation/avasset/1385974-isplayable)프로퍼티를 비동기적으로 로드하는 방법을 보여준다.

```swift
// URL of a bundle asset called 'example.mp4'
let url = Bundle.main.url(forResource: "example", withExtension: "mp4")!
let asset = AVAsset(url: url)
let playableKey = "playable"

// Load the "playable" property
asset.loadValuesAsynchronously(forKeys: [playableKey]) {
    var error: NSError? = nil
    let status = asset.statusOfValue(forKey: playableKey, error: &error)
    switch status {
    case .loaded:
        // Sucessfully loaded. Continue processing.
    case .failed:
        // Handle error
    case .cancelled:
        // Terminate processing
    default:
        // Handle all other cases
    }
}
```

일반적으로 일부 키에 대한 즉각적인 접근을 지원할 수 있는 사용 사례\(로컬 파일 시스템의 파일 URL로 초기화 되는 에셋 등\)에도, 느린 I/O는 키 값을 반환하기 전에 [`AVAsset`](https://developer.apple.com/documentation/avfoundation/avasset)를 차단해야 할 수 있다. 백그라운드 쓰레드 또는 작업 큐에서 에셋을 준비하는 경우 MacOS 클라이언트에서는 차단을 허용할 수 있지만 차단을 방지해야 할 경우 [`loadValuesAsynchronously(forKeys:completionHandler:)`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading/1387321-loadvaluesasynchronously)메서드를 사용하라. iOS 및 tvOS 애플리케이션은 I/O 요청을 차단하면 미디어 서비스가 종료될 수 있으므로 항상 이 메서드를 사용하여 값을 비동기식으로 로드해야 한다.

#### Loading Assets by Key

* [`func statusOfValue(forKey: String, error: NSErrorPointer) -> AVKeyValueStatus`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading/1386816-statusofvalue)  호출 쓰레드를 차단하지 않고 특정 키의 값을 즉시 사용할 수 있는지 여부를 나타내는 상태를 반환한다. **Required.**
* \*\*\*\*[`func loadValuesAsynchronously(forKeys: [String], completionHandler: (() -> Void)?)`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading/1387321-loadvaluesasynchronously)  에셋에 아직 로드되지 않은 모든 지정된 키\(프로퍼티 이름\)의 값을 로드하도록 지시한다. **Required.**

#### Constants

* [`enum AVKeyValueStatus`](https://developer.apple.com/documentation/avfoundation/avkeyvaluestatus)  속성의 로드 상태를 지정하는 상수.

