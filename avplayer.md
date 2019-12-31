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

### Topics

#### Creating a Player

* [`init(url: URL)`](https://developer.apple.com/documentation/avfoundation/avplayer/1385706-init)  지정된 URL에서 참조하는 단일 시청각 리소스를 재생할 새 플레이어를 생성하라.
* [`init(playerItem: AVPlayerItem?)`](https://developer.apple.com/documentation/avfoundation/avplayer/1387104-init)  지정된 플레이어 항목을 재생할 새 플레이어 만들기

#### Managing Playback

* [`func play()`](https://developer.apple.com/documentation/avfoundation/avplayer/1386726-play)  현재 항목의 재생을 시작한다.
* [`func pause()`](https://developer.apple.com/documentation/avfoundation/avplayer/1387895-pause)  현재 항목의 재생을 일시 중지한다.
* [`var rate: Float`](https://developer.apple.com/documentation/avfoundation/avplayer/1388846-rate)  현재 재생률.
* [`var actionAtItemEnd: AVPlayer.ActionAtItemEnd`](https://developer.apple.com/documentation/avfoundation/avplayer/1387376-actionatitemend)  현재 플레이어 항목의 재생이 완료되면 수행하는 작업.
* [`enum AVPlayer.ActionAtItemEnd`](https://developer.apple.com/documentation/avfoundation/avplayer/actionatitemend)  플레이를 마치면 플레이어가 취해야 할 액션.
* [`func replaceCurrentItem(with: AVPlayerItem?)`](https://developer.apple.com/documentation/avfoundation/avplayer/1390806-replacecurrentitem)  현재 플레이어 항목을 새 플레이어 항목으로 대체.
* [`var preventsDisplaySleepDuringVideoPlayback: Bool`](https://developer.apple.com/documentation/avfoundation/avplayer/2990522-preventsdisplaysleepduringvideop)  비디오 재생이 디스플레이 및 장치 절전을 방해하는지 여부를 나타내는 불 값.

#### Managing Automatic Waiting Behavior

* [`var automaticallyWaitsToMinimizeStalling: Bool`](https://developer.apple.com/documentation/avfoundation/avplayer/1643482-automaticallywaitstominimizestal)  플레이어가 스톨을 최소화하기 위해 재생을 자동으로 연기해야 하는지 여부를 나타내는 불 값.
* [`var reasonForWaitingToPlay: AVPlayer.WaitingReason?`](https://developer.apple.com/documentation/avfoundation/avplayer/1643486-reasonforwaitingtoplay)  플레이어가 현재 재생이 시작 또는 재개되기를 기다리는 WaitingReason.
* [`struct AVPlayer.WaitingReason`](https://developer.apple.com/documentation/avfoundation/avplayer/waitingreason)  플레이어가 대기하고 있는 WaitingReason 시작 또는 재개.
* [`var timeControlStatus: AVPlayer.TimeControlStatus`](https://developer.apple.com/documentation/avfoundation/avplayer/1643485-timecontrolstatus)  적절한 네트워크 조건을 기다리는 동안 재생이 현재 진행 중인지, 무기한 일시 중지되었는지 또는 일시 중단 되었는지 여부를 나타내는 상태.
* [`enum AVPlayer.TimeControlStatus`](https://developer.apple.com/documentation/avfoundation/avplayer/timecontrolstatus)  재생 속도 변경을 나타내는 플레이어 상태.
* [`func playImmediately(atRate: Float)`](https://developer.apple.com/documentation/avfoundation/avplayer/1643480-playimmediately)   사용 가능한 미디어 데이터를 지정된 속도로 즉시 재생하라.

#### Managing Time

* [`func currentTime() -> CMTime`](https://developer.apple.com/documentation/avfoundation/avplayer/1390404-currenttime) 현재 플레이어 항목의 현재 시간을 반환한다.
* [`func seek(to: CMTime)`](https://developer.apple.com/documentation/avfoundation/avplayer/1385953-seek)

  현재 재생 시간을 지정된 시간으로 설정하라.

* [`func seek(to: Date)`](https://developer.apple.com/documentation/avfoundation/avplayer/1386114-seek)

  현재 재생 시간을 날짜 객체가 지정한 시간으로 설정하라.

* [`func seek(to: CMTime, completionHandler: (Bool) -> Void)`](https://developer.apple.com/documentation/avfoundation/avplayer/1387018-seek)

  현재 재생 시간을 지정된 시간으로 설정하고 탐색 작업이 완료되거나 중단될 때 지정된 블록을 실행한다.

* [`func seek(to: Date, completionHandler: (Bool) -> Void)`](https://developer.apple.com/documentation/avfoundation/avplayer/1386108-seek)

  현재 재생 시간을 지정된 시간으로 설정하고 탐색 작업이 완료되거나 중단될 때 지정된 블록을 실행한다.

* [`func seek(to: CMTime, toleranceBefore: CMTime, toleranceAfter: CMTime)`](https://developer.apple.com/documentation/avfoundation/avplayer/1387741-seek)  지정된 시간 내에 현재 재생 시간을 설정한다.
* [`func seek(to: CMTime, toleranceBefore: CMTime, toleranceAfter: CMTime, completionHandler: (Bool) -> Void)`](https://developer.apple.com/documentation/avfoundation/avplayer/1388493-seek)

  지정된 시간 내에 현재 재생 시간을 설정하고 탐색 작업이 완료되거나 중단될 때 지정된 블록을 호출한다.

#### Observing Time

* [`func addPeriodicTimeObserver(forInterval: CMTime, queue: DispatchQueue?, using: (CMTime) -> Void) -> Any`](https://developer.apple.com/documentation/avfoundation/avplayer/1385829-addperiodictimeobserver)

  재생하는 동안 특정 블록의 주기적인 호출에 대해 변경 시간을 보고하도록 요청한다.

* [`func addBoundaryTimeObserver(forTimes: [NSValue], queue: DispatchQueue?, using: () -> Void) -> Any`](https://developer.apple.com/documentation/avfoundation/avplayer/1388027-addboundarytimeobserver)

  정상 재생 중에 지정된 시간이 전달될 때 블록의 호출을 요청한다.

* [`func removeTimeObserver(Any)`](https://developer.apple.com/documentation/avfoundation/avplayer/1387552-removetimeobserver)

  이전에 등록된 주기적인 또는 경계 시간 관찰자를 취소한다.

#### Configuring Media Selection Criteria Settings

* [`var appliesMediaSelectionCriteriaAutomatically: Bool`](https://developer.apple.com/documentation/avfoundation/avplayer/1387178-appliesmediaselectioncriteriaaut)

  수신자가 현재 선택 기준을 플레이어 항목에 자동으로 적용해야 하는지 여부를 나타내는 불 값.

* [`func mediaSelectionCriteria(forMediaCharacteristic: AVMediaCharacteristic) -> AVPlayerMediaSelectionCriteria?`](https://developer.apple.com/documentation/avfoundation/avplayer/1387825-mediaselectioncriteria)

  지정된 미디어 특성을 가진 미디어 항목에 대한 자동 선택 기준을 반환한다.

* [`func setMediaSelectionCriteria(AVPlayerMediaSelectionCriteria?, forMediaCharacteristic: AVMediaCharacteristic)`](https://developer.apple.com/documentation/avfoundation/avplayer/1390563-setmediaselectioncriteria)

  지정된 미디어 특성을 가진 미디어에 대해 자동 선택 기준을 적용한다.[  
  ](https://developer.apple.com/documentation/avfoundation/avplayer/1390563-setmediaselectioncriteria)

#### Managing External Playback

* [`var allowsExternalPlayback: Bool`](https://developer.apple.com/documentation/avfoundation/avplayer/1387441-allowsexternalplayback)

  플레이어가 외부 재생 모드로 전환할 수 있는지 여부를 나타내는 불 값.

* [`var isExternalPlaybackActive: Bool`](https://developer.apple.com/documentation/avfoundation/avplayer/1388982-isexternalplaybackactive)

  플레이어가 현재 외부 재생 모드에서 비디오를 재생하고 있는지 여부를 나타내는 불 값.

* [`var usesExternalPlaybackWhileExternalScreenIsActive: Bool`](https://developer.apple.com/documentation/avfoundation/avplayer/1624255-usesexternalplaybackwhileexterna)

  외부 화면 모드가 활성화된 상태에서 플레이어가 자동으로 외부 재생 모드로 전환해야 하는지 여부를 나타내는 불 값.

* [`var externalPlaybackVideoGravity: AVLayerVideoGravity`](https://developer.apple.com/documentation/avfoundation/avplayer/1624251-externalplaybackvideogravity)

  외부 재생 모드용인 플레이어의 VideoGravity.

* [`struct AVLayerVideoGravity`](https://developer.apple.com/documentation/avfoundation/avlayervideogravity)

  비디오가 계층의 바운드 직사각형 내에 표시되는 방법을 정의하는 값.

#### Managing Audio Output

* [`var isMuted: Bool`](https://developer.apple.com/documentation/avfoundation/avplayer/1387544-ismuted)

  플레이어의 오디오 출력이 음소거되는지 여부를 나타내는 불 값.

* [`var volume: Float`](https://developer.apple.com/documentation/avfoundation/avplayer/1390127-volume)

  플레이어의 오디오 재생 볼륨.

* [`var audioOutputDeviceUniqueID: String?`](https://developer.apple.com/documentation/avfoundation/avplayer/1390717-audiooutputdeviceuniqueid)

  오디오 재생에 사용되는 Core Audio 출력 장치의 고유 ID를 지정한다.

#### Getting Player Properties

* [`var status: AVPlayer.Status`](https://developer.apple.com/documentation/avfoundation/avplayer/1388096-status)

  플레이어를 재생에 사용할 수 있는지 여부를 나타내는 상태.

* [`enum AVPlayer.Status`](https://developer.apple.com/documentation/avfoundation/avplayer/status)

  플레이어가 항목을 성공적으로 재생할 수 있는지 여부를 나타내는 상태.

* [`var error: Error?`](https://developer.apple.com/documentation/avfoundation/avplayer/1387764-error)

  실패를 초래한 에러.

* [`var currentItem: AVPlayerItem?`](https://developer.apple.com/documentation/avfoundation/avplayer/1387569-currentitem)

  플레이어의 현재 플레이어 항목.

* [`var isOutputObscuredDueToInsufficientExternalProtection: Bool`](https://developer.apple.com/documentation/avfoundation/avplayer/1624254-isoutputobscuredduetoinsufficien)

  외부 보호가 불충분하여 출력이 가려지는지 여부를 나타내는 불 값.

#### Determining HDR Playback

* [`class var availableHDRModes: AVPlayer.HDRMode`](https://developer.apple.com/documentation/avfoundation/avplayer/2936889-availablehdrmodes)

  재생 가능한 HDR 모드.

* [`struct AVPlayer.HDRMode`](https://developer.apple.com/documentation/avfoundation/avplayer/hdrmode)

  HDR 모드를 지정하는 비트필드 유형.

#### Setting The GPU

* [`var preferredVideoDecoderGPURegistryID: UInt64`](https://developer.apple.com/documentation/avfoundation/avplayer/2942616-preferredvideodecodergpuregistry)  비디오 디코딩에 사용되는 GPU의 레지스트리 식별자.

