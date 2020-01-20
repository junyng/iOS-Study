---
description: 에셋의 미디어 트랙에 대한 트랙 레벨 검사 인터페이스를 제공하는 객체.
---

# AVAssetTrack

### Declaration

```swift
class AVAssetTrack : NSObject
```

### Overview

AVAssetTrack은 [`AVAsynchronousKeyValueLoading`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading)프로토콜을 채택한다. 프로토콜의 [`statusOfValue(forKey:error:)`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading/1386816-statusofvalue)메서드를 사용하여 호출 쓰레드를 차단하지 않고 트랙 속성에 접근할 수 있는지 여부를 확인할 수 있다. 속성의 상태가 [`AVKeyValueStatus.loaded`](https://developer.apple.com/documentation/avfoundation/avkeyvaluestatus/loaded)이외의 값인 경우, 프로토콜의 [`loadValuesAsynchronously(forKeys:completionHandler:)`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading/1387321-loadvaluesasynchronously)메서드를 사용하여 사용하기 전에 키를 비동기적으로 로드해야 한다. `AVAssetTrack`의 모든 키에 대해 보류 중인 로드 요청을 취소하려면 소유 [`AVAsset`](https://developer.apple.com/documentation/avfoundation/avasset)객체\(예: \[track.asset cancelLoading\]\)에 메시지해야 한다.

