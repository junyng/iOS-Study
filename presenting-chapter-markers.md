---
description: 플레이어의 상태를 업데이트하려면 에셋의 재생 시간을 관찰하라.
---

# Observing the Playback Time

### Overview

일반적으로 재생 위치를 업데이트하거나 사용자 인터페이스의 상태를 동기화할 수 있도록 에셋의 재생 시간이 진행되는 것을 관찰하고자 할 것이다. 비록 key-value observing\(KVO\)이 일반적인 상태 관찰에는 잘 작동하지만, 지속적인 상태 변화를 관찰하는 데는 적합하지 않기 때문에 플레이어의 타이밍을 관찰하는 것은 올바른 선택이 아니다. 대신, [`AVPlayer`](https://developer.apple.com/documentation/avfoundation/avplayer)는 플레이어 시간 변화를 관찰할 수 있는 두 가지 다른 방법을 제공한다. 주기적인 관찰과 경계의 관찰

#### Observe Periodic Timing <a id="2948356"></a>

당신은 일정한 주기적인 간격으로 똑딱거리는 시간을 관찰할 수 있다. 사용자 지정 플레이어를 구축하는 경우, 주기적으로 관찰할 수 있는 가장 일반적인 사용 사례는 사용자 인터페이스의 시간 표시를 업데이트하는 것이다.

주기적인 타이밍을 관찰하려면 플레이어의 [`addPeriodicTimeObserver(forInterval:queue:using:)`](https://developer.apple.com/documentation/avfoundation/avplayer/1385829-addperiodictimeobserver) 메서드를 사용하라. 이 메서드는 시간 간격, 디스패치 큐 및 콜백 블록을 나타내는 [CMTime](https://developer.apple.com/documentation/coremedia/cmtime-u58) 값을 지정 시간 간격에 호출한다. 다음 예제에서는 정상 재생 중에 0.5초마다 호출되도록 블록을 설정하는 방법을 보여준다.

```swift
var player: AVPlayer!
var playerItem: AVPlayerItem!
var timeObserverToken: Any?

func addPeriodicTimeObserver() {
    // Notify every half second
    let timeScale = CMTimeScale(NSEC_PER_SEC)
    let time = CMTime(seconds: 0.5, preferredTimescale: timeScale)

    timeObserverToken = player.addPeriodicTimeObserver(forInterval: time,
                                                      queue: .main) {
        [weak self] time in
        // update player transport UI
    }
}

func removePeriodicTimeObserver() {
    if let timeObserverToken = timeObserverToken {
        player.removeTimeObserver(timeObserverToken)
        self.timeObserverToken = nil
    }
}
```

#### Observe Boundary Timing <a id="2948359"></a>

당신이 시간을 관찰할 수 있는 다른 방법은 경계선이다. 미디어의 타임라인 내에서 다양한 관심 지점을 정의하고, 일반적인 재생 중에 해당 시간이 전달될 때 프레임워크에서 다시 호출한다. 경계 관찰은 주기적인 관측보다 덜 자주 사용되지만, 여전히 특정 상황에서 유용하다는 것을 증명할 수 있다. 예를 들어, 재생 컨트롤이 없는 비디오를 표시하면서 디스플레이 요소를 동기화하거나 해당 시간이 전달될 때 추가 콘텐츠를 표시하려는 경우 경계 관찰을 사용할 수 있다.

경계 시간을 관찰하려면 플레이어의 [`addBoundaryTimeObserver(forTimes:queue:using:)`](https://developer.apple.com/documentation/avfoundation/avplayer/1388027-addboundarytimeobserver) 메서드를 사용하라. 이 메서드는 경계 시간, 시리얼 디스패치 큐 및 콜백 블록을 정의하는 `CMTime` 값을 감싸는 일련의 [`NSValue`](https://developer.apple.com/documentation/foundation/nsvalue) 객체를 가져간다. 다음 예제는 각 재생 분기에 대한 경계 시간을 정의하는 방법을 보여준다.

```swift
var asset: AVAsset!
var player: AVPlayer!
var playerItem: AVPlayerItem!
var timeObserverToken: Any?

func addBoundaryTimeObserver() {

    // Divide the asset's duration into quarters.
    let interval = CMTimeMultiplyByFloat64(asset.duration, 0.25)
    var currentTime = kCMTimeZero
    var times = [NSValue]()

    // Calculate boundary times
    while currentTime < asset.duration {
        currentTime = currentTime + interval
        times.append(NSValue(time:currentTime))
    }

    timeObserverToken = player.addBoundaryTimeObserver(forTimes: times,
                                                       queue: .main) {
        // Update UI
    }
}

func removeBoundaryTimeObserver() {
    if let timeObserverToken = timeObserverToken {
        player.removeTimeObserver(timeObserverToken)
        self.timeObserverToken = nil
    }
}
```

