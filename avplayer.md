---
description: 플레이어의 전송 동작을 제어하기 위한 인터페이스를 제공하는 객체
---

# AVPlayer

### Declaration

```swift
class AVPlayer : NSObject
```

### Overview

`AVPlayer`는 미디어 에셋의 재생 및 타이밍을 관리하는 데 사용되는 컨트롤러 객체이다. `AVPlayer`를 사용하여 QuickTime 영화 및 MP3 오디오 파일과 같은 로컬 및 원격 파일 기반 미디어와 HTTP Live Streaming을 사용하여 제공되는 시청각 미디어를 재생할 수 있다.

`AVPlayer`는 한 번에 단일 미디어 에셋을 재생하는 것이다. 플레이어 인스턴스를 재사용하여 [`replaceCurrentItem(with:)`](https://developer.apple.com/documentation/avfoundation/avplayer/1390806-replacecurrentitem) 메서드를 사용해 추가 미디어 에셋을 재생할 수 있으나, 한 번에 단일 미디어 에셋만 재생 관리한다. 프레임워크는 또한 [`AVQueuePlayer`](https://developer.apple.com/documentation/avfoundation/avqueueplayer)라고 불리는 `AVPlayer`의 하위 클래스를 제공하며, 순차적으로 재생되는 미디어 에셋의 큐잉을 만들고 관리하는데 사용된다.

`AVPlayer`를 사용하여 미디어 에셋을 재생하는 경우, AVFoundation은 [`AVAsset`](https://developer.apple.com/documentation/avfoundation/avasset) 클래스를 사용하여 모델링한다. `AVAsset`은 미디어의 지속 시간 또는 생성 날짜와 같은 정적 측면만 모델링하고 자체적으로 `AVPlayer`와 함께 재생하기에 적합하지 않다. 에셋을 재생하려면 `AVPlayerItem`에서 찾을 수 있는 동적 대응 요소의 인스턴스를 생성해야 한다. 이 객체는 `AVPlayer`의 인스턴스가 수행하는 에셋의 타이밍 및 프리젠테이션 상태를 모델링한다. 자세한 내용은 [`AVPlayerItem`](https://developer.apple.com/documentation/avfoundation/avplayeritem) 레퍼런스를 참조하라.

 AVPlayer는 상태의 변화가 지속적으로 일어나는 동적 객체이다.

* **General State Observations:** 키 값 관찰\(KVO\)을 사용하여[`currentItem`](https://developer.apple.com/documentation/avfoundation/avplayer/1387569-currentitem) 는 재생 [`rate`](https://developer.apple.com/documentation/avfoundation/avplayer/1388846-rate)와 같은 플레이어의 많은 동적 특성에 대한 상태 변화를 관찰할 수 있다. 메인 쓰레드의 `KVO` 변경 알림을 등록하고 등록 취소해야 한다. 이는 다른 쓰레드를 변할 때 부분적으로 알림을 받을 가능성을 피한다. AVFoundation은 다른 쓰레에서 변경 작업을 수행할 때에도 메인 쓰레드에서 [`observeValue(forKeyPath:of:change:context:)`](https://developer.apple.com/documentation/objectivec/nsobject/1416553-observevalue)를 호출한다.
* **Timed State Observations:** KVO는 일반적인 상태 관찰에 잘 작동하지만 플레이어의 시간처럼 지속적으로 변화하는 상태를 관찰하기 위한 것은 아니다. AVPlayer는 시간 변화를 관찰하는 두 가지 메서드를 제공한다:  


  * [`addPeriodicTimeObserver(forInterval:queue:using:)`](https://developer.apple.com/documentation/avfoundation/avplayer/1385829-addperiodictimeobserver)
  * [`addBoundaryTimeObserver(forTimes:queue:using:)`](https://developer.apple.com/documentation/avfoundation/avplayer/1388027-addboundarytimeobserver)

  이러한 메서드는 사용하면 주기적으로 또는 경계별로 시간 변화를 관찰할 수 있다. 변경사항이 발생하면 이러한 메서드가 제공하는 콜백 블록 또는 종료를 실행하여 플레이어의 사용자 인터페이스 상태를 업데이트하는 등의 조치를 취할 수 있는 기회를 제공한다.

`AVPlayer`와 `AVPlayerItem`은 비 시각적 객체이며, 이는 자체적으로 에셋의 비디오를 화면에 표시할 수 없다는 것을 의미한다. 비디오 콘텐츠를 화면에 표시할 때 사용할 수 있는 두 가지 기본 접근 방식이 있다:

* AVKit: 동영상 콘텐츠를 제시하는 가장 좋은 방법은 iOS와 tvOS의 [`AVPlayerViewController`](https://developer.apple.com/documentation/avkit/avplayerviewcontroller) 클래스 또는 macOS의 [`AVPlayerView`](https://developer.apple.com/documentation/avkit/avplayerview) 클래스와 함께하는 것이다. 이 클래스는 재생 제어 및 기타 미디어 기능과 함께 비디오 콘텐츠를 제공하여 전체 기능의 재생 경험을 제공한다.
* AVPlayerLayer: 플레이어에 대한 사용자 지정 인터페이스를 만들 때 AVPlayerLayer를 사용하라. 플레이어 레이어는 뷰의 백킹 레이어로 설정되거나 레이어 계층 구조에 직접 추가 될 수 있다. AVPlayerView 및 AVPlayerViewController와 달리 플레이어 계층은 재생 제어를 제시하지 않으며 화면의 시각적 콘텐츠를 제공한다. 재생 전송 컨트롤을 구축하여 미디어를 재생하고 일시 중지하며 검색하는 것은 당신에게 달려 있다.

AVKit 또는 AVPlayerLayer가 제공되는 시각적 콘텐츠와 함께 [`AVSynchronizedLayer`](https://developer.apple.com/documentation/avfoundation/avsynchronizedlayer)를 사용하여 플레이어의 타이밍과 동기화된 애니메이션 콘텐츠를 제공할 수도 있다. 동기화된 레이어를 사용하여 현재 플레이어 타이밍을 레이어 하위 트리에 부여한다. AVSynchronizedLayer를 사용하여 애니메이션 하위 3분의 1또는 비디오 전환과 같은 코어 애니메이션에서 사용자 정의 효과를 구축할 수 있다. 그리고 플레이어의 현재 `AVPlayerItem`의 타이밍과 함께 플레이하도록 한다.

#### External Playback Mode <a id="1668782"></a>

외부 재생 모드는 AirPlay를 통해 Apple TV와 같은 외부 장치에 비디오 데이터를 전송하거나 미니 커넥터 기반 HDMI/VGA 어댑터를 통해 원래의 정확도로 전체 화면을 재생하는 것을 말한다. "외부 화면" 모드\(미러링 및 두 번째 디스플레이라고도 함\)에서 호스트 디바이스가 비디오 데이터를 렌더링한다. 호스트 장치는 렌더링된 비디오를 외부 장치로 재압축 및 전송하고, 외부 장치는 비디오를 압축 해제하여 표시한다. 외부 재생 속성은 AirPlay 비디오 재생에 영향을 미치며 사용되지 않는 AirPlay 지원 속성의 대체이다.

