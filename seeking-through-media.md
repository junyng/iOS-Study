---
description: 특정 시점에 빠르게 접근하려면 미디어 항목을 찾거나 스크럽하라.
---

# Seeking Through Media

### Overview

사용자는 정상적인 선형 재생 외에도 비선형 방식으로 검색하거나 스크러빙할 수 있는 기능을 사용하여 미디어 내에서 다양한 관심 지점을 신속하게 파악하고자 한다. AVKit은 자동으로 스크러빙 컨트롤을 제공하지만 \(미디어의 지원을 받는 경우\), 사용자 지정 플레이어를 구축하려면 이 기능을 직접 구축해야 한다. AVKit를 사용하는 경우에도 사용자가 미디어의 다양한 위치로 빠르게 건너뛸 수 있는 테이블 뷰나 컬렉션 뷰와 같은 추가 사용자 인터페이스를 제공하고자 할 수 있다.

#### Jump to a Specific Time Quickly <a id="2948489"></a>

[`AVPlayer`](https://developer.apple.com/documentation/avfoundation/avplayer)와 [`AVPlayerItem`](https://developer.apple.com/documentation/avfoundation/avplayeritem)의 메서드를 사용하여 다양한 방법을 찾을 수 있다. 가장 일반적인 방법은 다음과 같이 플레이어의 [`seek(to:)`](https://developer.apple.com/documentation/avfoundation/avplayer/1385953-seek) 메서드를 사용하여 목적 [`CMTime`](https://developer.apple.com/documentation/coremedia/cmtime) 값을 전달하는 것이다.

```swift
// Seek to the 2 minute mark
let time = CMTime(value: 120, timescale: 1)
player.seek(to: time)
```

[`seek(to:)`](https://developer.apple.com/documentation/avfoundation/avplayer/1385953-seek) 메서드는 프레젠테이션을 통해 빠르게 탐색할 수 있는 편리한 방법이지만 정밀도보다는 속도를 위해 조정된다. 이것은 플레이어가 움직이는 실제 시간이 당신이 요청한 시간과 다를 수 있다는 것을 의미한다.

#### Jump to a Specific Time Accurately <a id="2948488"></a>

정확한 탐색 동작을 구현해야 하는 경우 목표 시간\(전후\)에서 허용되는 편차량을 표시할 수 있는 [`seek(to:toleranceBefore:toleranceAfter:)`](https://developer.apple.com/documentation/avfoundation/avplayer/1387741-seek) 메서드를 사용하라. sample-accurate 검색 동작을 제공해야 하는 경우, 허용오차가 없음을 나타낼 수 있다.

```swift
// Seek to the first frame at 3:25 mark
let seekTime = CMTime(seconds: 205, preferredTimescale: Int32(NSEC_PER_SEC))
player.seek(to: seekTime, toleranceBefore: kCMTimeZero, toleranceAfter: kCMTimeZero)
```

> 중요:
>
> 허용오차가 작거나 zero 값인 `seek(to:toleranceBefore:toleranceAfter:)` 메서드를 호출하면 추가 디코딩 지연이 발생하여 앱의 검색 동작에 영향을 줄 수 있다.

