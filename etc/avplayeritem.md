---
description: 플레이어가 재생하는 에셋의 타이밍 및 프리젠테이션 상태를 모델링하는 데 사용되는 객체이다.
---

# AVPlayerItem

### Declaration

```swift
class AVPlayerItem : NSObject
```

### Overview

`AVPlayerItem`은 재생할 미디어를 나타내는 [`AVAsset`](https://developer.apple.com/documentation/avfoundation/avasset) 객체에 대한 참조를 저장한다. 재생을 위해 에셋에 대한 정보에 접근해야 하는 경우 [`AVAsynchronousKeyValueLoading`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading) 메서드를 사용하여 필요한 값을 로드할 수 있다. 또는 `AVPlayerItem`은 원하는 키 집합을 [`init(asset:automaticallyLoadedAssetKeys:)`](https://developer.apple.com/documentation/avfoundation/avplayeritem/1387529-init) 생성자에 전달함으로써 필요한 에셋 데이터를 자동으로 로드할 수 있다. 플레이어 항목이 재생할 준비가 되면 해당 에셋 속성이 로드되고 사용할 준비가 된다.

`AVPlayerItem`은 동적 객체이다. 변경할 수 있는 속성 값 외에도, 읽기 전용 속성 값 대부분은 항목의 준비 및 재생 중에 관련 `AVPlayer`에 의해 변경될 수 있다. [Key-value observing](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/KVO.html#//apple_ref/doc/uid/TP40008195-CH16)를 사용하여 이러한 상태 변경이 발생하는 경우 관찰할 수 있다. 관찰할 수 있는 가장 중요한 플레이어 항목 속성 중 하나는 [`status`](https://developer.apple.com/documentation/avfoundation/avplayeritem/1389493-status) 이다. `status`는 항목이 재생 준비가 되어 있으며 일반적으로 사용할 수 있는지 여부를 나타낸다. 플레이어 항목을 처음 만들 때, 플레이어 항목의 상태는 [`AVPlayerItem.Status.unknown`](https://developer.apple.com/documentation/avfoundation/avplayeritem/status/unknown)의 값을 가지고 있는데, 이는 미디어가 로드되지 않았고 아직 재생을 위해 큐에 들어가지 않았다는 것을 의미한다. 플레이어 항목을 `AVPlayer`와 연관시키면 즉시 아이템의 미디어를 큐에 넣고 재생을 준비하기 시작하지만, 사용 준비가 되기 전에 [`AVPlayerItem.Status.readyToPlay`](https://developer.apple.com/documentation/avfoundation/avplayeritem/status/readytoplay)의 상태가 변경될 때까지 기다려야 한다. 다음 코드는 상태 변경 사항을 등록하고 통지하는 방법의 예를 보여준다.

```swift
func prepareToPlay() {
    let url = <#Asset URL#>
    // Create asset to be played
    asset = AVAsset(url: url)
    
    let assetKeys = [
        "playable",
        "hasProtectedContent"
    ]
    // Create a new AVPlayerItem with the asset and an
    // array of asset keys to be automatically loaded
    playerItem = AVPlayerItem(asset: asset,
                              automaticallyLoadedAssetKeys: assetKeys)
    
    // Register as an observer of the player item's status property
    playerItem.addObserver(self,
                           forKeyPath: #keyPath(AVPlayerItem.status),
                           options: [.old, .new],
                           context: &playerItemContext)
    
    // Associate the player item with the player
    player = AVPlayer(playerItem: playerItem)
}
```

[`addObserver(_:forKeyPath:options:context:)`](https://developer.apple.com/documentation/objectivec/nsobject/1412787-addobserver) 메서드를 사용하여 플레이어 항목의 상태 속성을 관찰하기 위해 prepareToPlay 메서드가 등록된다. 플레이어 항목을 플레이어와 연결하기 전에 이 메서드를 호출하여 항목의 상태에 대한 모든 상태 변경을 캡처하는지 확인하라.

상태 변경에 대한 알림을 받으려면 [`observeValue(forKeyPath:of:change:context:)`](https://developer.apple.com/documentation/objectivec/nsobject/1416553-observevalue) 메서드를 구현해야 한다. 이 메서드는 `status`가 변경될 때마다 호출되어 대응 조치를 취할 수 있다. \(예제 참조\)

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
        
        // Get the status change from the change dictionary
        if let statusNumber = change?[.newKey] as? NSNumber {
            status = AVPlayerItemStatus(rawValue: statusNumber.intValue)!
        } else {
            status = .unknown
        }
        
        // Switch over the status
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

예제에서는 변경 딕셔너리에서 새 상태를 검색하고 해당 값으로 전환한다. 플레이어 항목의 상태가 `AVPlayerItem.Status,readyToPlay`이면 사용할 준비가 되었다. 플레이어 항목의 미디어를 로드하는 동안 문제가 발생한 경우 AVPlayerItem.Status.failed 상태가 된다. 플레이어 항목의 [`error`](https://developer.apple.com/documentation/avfoundation/avplayeritem/1389185-error) 속성을 쿼리하여 실패 세부 정보를 제공하는 `NSError`를 얻을 수 있다.

