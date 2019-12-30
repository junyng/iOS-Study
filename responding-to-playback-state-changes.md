---
description: 플레이어의 재생 상태 변경에 대응한다.
---

# Responding to Playback State Changes

### Overview

[`AVPlayer`](https://developer.apple.com/documentation/avfoundation/avplayer)와 [`AVPlayerItem`](https://developer.apple.com/documentation/avfoundation/avplayeritem)은 상태가 자주 바뀌는 동적 객체다. 이러한 상태 변화를 관찰하고 대응하는 간단한 방법은 키 값 관찰\(KVO\)을 사용하는 것이다. KVO으로 개체를 다른 객체의 상태를 관찰하기 위해 등록할 수 있다. 관찰된 객체의 상태가 변경되면 관찰자에게 상태 변경에 대한 세부 정보가 통지된다. KVO에 대한 자세한 내용은 [Key-Value Observing Programming Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)를 참조하라.

#### Observe the Player's State <a id="2948353"></a>

관찰해야 할 가장 중요한 `AVPlayerItem` 속성 중 하나는 [`status`](https://developer.apple.com/documentation/avfoundation/avplayeritem/1389493-status)이다. 이 속성은 플레이어 항목을 재생할 준비가 되었는지, 일반적으로 사용할 수 있는지 여부를 표시한다. 플레이어 항목을 처음만들 때 `status` 값은 [`AVPlayerItem.Status.unknown`](https://developer.apple.com/documentation/avfoundation/avplayeritem/status/unknown)이며, 해당 미디어가 로드되지 않았거나 재생 대기 상태가 되었음을 의미한다. 플레이어 항목을 `AVPlayer`와 연결하면 플레이어 항목이 즉시 해당 미디어에 큐를 보내고 재생할 수 있도록 준비하기 시작한다. 플레이어 항목은 상태 값이 [`AVPlayerItem.Status.readyToPlay`](https://developer.apple.com/documentation/avfoundation/avplayeritem/status/readytoplay)으로 변경될 때 사용할 준비가 된다. 다음 예제는 이 상태 변경을 관찰하는 방법을 보여준다:

```swift
let url: URL = // Asset URL
var asset: AVAsset!
var player: AVPlayer!
var playerItem: AVPlayerItem!

// Key-value observing context
private var playerItemContext = 0

let requiredAssetKeys = [
    "playable",
    "hasProtectedContent"
]

func prepareToPlay() {
    // Create the asset to play
    asset = AVAsset(url: url)

    // Create a new AVPlayerItem with the asset and an
    // array of asset keys to be automatically loaded
    playerItem = AVPlayerItem(asset: asset,
                              automaticallyLoadedAssetKeys: requiredAssetKeys)

    // Register as an observer of the player item's status property
    playerItem.addObserver(self,
                           forKeyPath: #keyPath(AVPlayerItem.status),
                           options: [.old, .new],
                           context: &playerItemContext)
    
    // Associate the player item with the player
    player = AVPlayer(playerItem: playerItem)
}
```

[`prepareToPlay()`](https://developer.apple.com/documentation/avfoundation/avaudioplayer/1386886-preparetoplay)메서드는 [`addObserver(_:forKeyPath:options:context:)`](https://developer.apple.com/documentation/objectivec/nsobject/1412787-addobserver)메서드를 사용하여 플레이어 항목의 `status` 속성을 관찰하기 위해 등록한다. 플레이어 항목을 플레이어와 연결하기 전에 이 메서드를 호출하여 항목의 `status`에 대한 모든 상태 변경 사항을 캡처하는지 확인하라.

#### Respond to a State Change <a id="2948355"></a>

상태 변경에 대한 알림을 받으려면 [`observeValue(forKeyPath:of:change:context:)`](https://developer.apple.com/documentation/objectivec/nsobject/1416553-observevalue)메서드를 구현하라. 이 메서드는 `status` 값이 변경될 때마다 호출되므로 다음과 같은 액션을 취할 수 있다:

```swift
override func observeValue(forKeyPath keyPath: String?,
                           of object: Any?,
                           change: [NSKeyValueChangeKey : Any]?,
                           context: UnsafeMutableRawPointer?) {

    // Only handle observations for the playerItemContext
    guard context == &playerItemContext else {
        super.observeValue(forKeyPath: keyPath,
                           of: object,
                           change: change,
                           context: context)
        return
    }

    if keyPath == #keyPath(AVPlayerItem.status) {
        let status: AVPlayerItemStatus
        if let statusNumber = change?[.newKey] as? NSNumber {
            status = AVPlayerItemStatus(rawValue: statusNumber.intValue)!
        } else {
            status = .unknown
        }

        // Switch over status value
        switch status {
        case .readyToPlay:
            // Player item is ready to play.
        case .failed:
            // Player item failed. See error.
        case .unknown:
            // Player item is not yet ready.
        }
    }
}
```

이 예제는 변경 딕셔너리에서 새로운 `status` 값을 검색하고 그 값을 전환한다. 플레이어 항목의 `status` 값이 [`AVPlayerItem.Status.readyToPlay`](https://developer.apple.com/documentation/avfoundation/avplayeritem/status/readytoplay) 인 경우, 사용할 수 있다. 플레이어 항목의 미디어를 로드하려고 시도하는 동안 문제가 발생하면 `status` 값은 [`AVPlayerItem.Status.failed`](https://developer.apple.com/documentation/avfoundation/avplayeritem/status/failed)이다. 오류가 발생한 경우 플레이어 항목의 [`error`](https://developer.apple.com/documentation/avfoundation/avplayeritem/1389185-error) 속성을 쿼리하여 실패의 세부 정보를 제공하는 NSError 객체를 검색할 수 있다.

