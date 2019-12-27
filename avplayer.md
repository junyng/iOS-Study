---
description: 플레이어의 전송 동작을 제어하기 위한 인터페이스를 제공하는 객체
---

# AVPlayer

### Declaration

```swift
class AVPlayer : NSObject
```

### Overview

* **General State Observations:** 키 값 관찰\(KVO\)을 사용하여[`currentItem`](https://developer.apple.com/documentation/avfoundation/avplayer/1387569-currentitem) 는 재생 [`rate`](https://developer.apple.com/documentation/avfoundation/avplayer/1388846-rate)와 같은 플레이어의 많은 동적 특성에 대한 상태 변화를 관찰할 수 있다. 메인 쓰레드의 `KVO` 변경 알림을 등록하고 등록 취소해야 한다. 이는 다른 쓰레드를 변할 때 부분적으로 알림을 받을 가능성을 피한다. AVFoundation은 다른 쓰레에서 변경 작업을 수행할 때에도 메인 쓰레드에서 [`observeValue(forKeyPath:of:change:context:)`](https://developer.apple.com/documentation/objectivec/nsobject/1416553-observevalue)를 호출한다.

