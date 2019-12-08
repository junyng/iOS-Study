# Refining The User Experience

이 가이드에서 AVKit 및 AVFoundation을 사용하여 강력한 재생 앱을 구축하는 방법을 살펴보았다. 이 장에서는 앱의 재생 환경을 더욱 커스터마이징하고 세분화하는 데 사용할 수 있는 AVFoundation의 몇 가지 추가 기능을 설명한다.

### Presenting Chapter Markers 

챕터 마커를 통해 사용자가 컨텐츠를 빠르게 탐색할 수 있다. tvOS 및 macOS의  `AVPlayerViewController`는 현재 재생된 에셋에서 마커가 발견되면 자동으로 챕터 선택 인터페이스를 제공한다. 또한 사용자 정의 선택 인터페이스를 생성하려는 경우 언제든지 이 데이터를 직접 되찾을 수 있다.

챕터 마커는 _시간적 메타데이터_의 한 유형이다. 이는 본 가이드의 다른 절에서 논의된 것과 동일한 종류의 메타데이터이지만, 전체 에셋을 적용하는 대신 에셋의 일정 내에 있는 시간 범위에만 적용된다. [`chapterMetadataGroupsBestMatchingPreferredLanguages:`](https://developer.apple.com/documentation/avfoundation/avasset/1390909-chaptermetadatagroupsbestmatchin) 또는 [`chapterMetadataGroupsWithTitleLocale:containingItemsWithCommonKeys:`](https://developer.apple.com/documentation/avfoundation/avasset/1388966-chaptermetadatagroupswithtitlelo) 메서드를 사용하여 에셋의 챕터 메타데이터를 되찾을 수 있다. 에셋의 [`availableChapterLocales`](https://developer.apple.com/documentation/avfoundation/avasset/1388228-availablechapterlocales) 키의 값을 비동기식으로 로드한 후 이러한 메서드를 차단하지 않고 호출할 수 있다.

```swift
let asset = AVAsset(url: <# Asset URL #>)
let chapterLocalesKey = "availableChapterLocales"
 
asset.loadValuesAsynchronously(forKeys: [chapterLocalesKey]) {
    var error: NSError?
    let status = asset.statusOfValue(forKey: chapterLocalesKey, error: &error)
    if status == .loaded {
        let languages = Locale.preferredLanguages
        let chapterMetadata = asset.chapterMetadataGroups(bestMatchingPreferredLanguages: languages)
        // Process chapter metadata
    }
    else {
        // Handle other status cases
    }
}
```

이러한 메서드에서 반환되는 값은 각각 개별 챕터 마커를 나타내는 [`AVTimedMetadataGroup`](https://developer.apple.com/documentation/avfoundation/avtimedmetadatagroup) 객체의 배열이다. `AVTimedMetadataGroup` 객체는 `CMTimeRange`로 구성되며 메타데이터가 적용되는 시간 범위, `AVMetadataItem` 배열 객체의 챕터 제목을 나타내는 항목 객체, 선택적으로 해당 미리 보기 이미지를 정의한다. 다음 예제에서는 `AVTimedMetadataGroup` 데이터를 앱 뷰 레이어에 표시할 사용자 지정 모델 객체 배열로 변환하는 방법을 보여주며, 이는 `Chapter` 라고 한다.

```swift
func convertTimedMetadataGroupsToChapters(groups: [AVTimedMetadataGroup]) -> [Chapter] {
    return groups.map { group -> Chapter in
 
        // Retrieve AVMetadataCommonIdentifierTitle metadata items
        let titleItems =
            AVMetadataItem.metadataItems(from: group.items,
                                         filteredByIdentifier: AVMetadataCommonIdentifierTitle)
 
        // Retrieve AVMetadataCommonIdentifierTitle metadata items
        let artworkItems =
            AVMetadataItem.metadataItems(from: group.items,
                                         filteredByIdentifier: AVMetadataCommonIdentifierArtwork)
 
        var title = "Default Title"
        var image = UIImage(named: "placeholder")!
 
        if let titleValue = titleItems.first?.stringValue {
            title = titleValue
        }
 
        if let imgData = artworkItems.first?.dataValue, imageValue = UIImage(data: imgData) {
            image = imageValue
        }
 
        return Chapter(time: group.timeRange.start, title: title, image: image)
    }
}
```

관련 데이터를 변환하면 챕터 선택 인터페이스를 구축하고 챕터 객체의 `time` 값을 사용하여 플레이어의 [`seekToTime:`](https://developer.apple.com/documentation/avfoundation/avplayer/1385953-seek) 메서드를 사용하여 현재 프레젠테이션을 찾을 수 있다.

### Selecting Media Options

개발자로서, 앱이 가능한 광범위의 사용자에게 접근 가능하는 것을 원할 것이다. 앱이 범위를 확장하는 한 가지 방법은 사용자가 모국어로 앱을 사용할 수 있도록 하고 청각 장애 또는 기타 접근성이 필요한 사용자를 지원하는 것이다. AVKit 및 AVFoundation 은 쉽게 자막과 선택 캡션을 제출하는 내장형 지원을 제공하며 대체 오디오 및 비디오 트랙을 선택하도록 하여 이러한 우려를 쉽게 처리한다. 사용자 지정 플레이어를 만들거나 자신의 미디어 선택 인터페이스를 제공하려는 경우 AVFoundation의 [`AVMediaSelectionGroup`](https://developer.apple.com/documentation/avfoundation/avmediaselectiongroup) 및 [`AVMediaSelectionOption`](https://developer.apple.com/documentation/avfoundation/avmediaselectionoption) 클래스에서 제공하는 기능을 사용할 수 있다.

`AVMediaSelectionOption` 은 원본 미디어에 포함된 대체 오디오, 비디오 또는 텍스트 트랙을 모델링한다. 미디어 옵션은 대체 카메라 각도를 선택하거나, 사용자의 고유 언어로 더빙된 오디오를 제공하거나, 자막과 폐쇄 캡션을 표시하는 데 사용된다. 에셋의 프로퍼티를 비동기적으로 로드하고 호출하여 사용할 수 있는 대체 미디어 프레젠테이션을 결정하며, 이 [`availableMediaCharacteristicsWithMediaSelectionOptions`](https://developer.apple.com/documentation/avfoundation/avasset/1389433-availablemediacharacteristicswit) 프로퍼티는 사용 가능한 미디어 특성을 나타내는 일련의 문자열을 반환한다. 반환되는 가능한 값은 [`AVMediaCharacteristicAudible`](https://developer.apple.com/documentation/avfoundation/avmediacharacteristicaudible) \(대체 오디오 트랙\), [`AVMediaCharacteristicVisual`](https://developer.apple.com/documentation/avfoundation/avmediacharacteristicvisual) \(대체 비디오 트랙\), [`AVMediaCharacteristicLegible`](https://developer.apple.com/documentation/avfoundation/avmediacharacteristiclegible) \(자막과 폐쇄 캡션\)이다.

사용 가능한 옵션의 배열을 되찾은 후 에셋의 [`mediaSelectionGroupForMediaCharacteristic:`](https://developer.apple.com/documentation/avfoundation/avasset/1387496-mediaselectiongroupformediachara) 메서드를 원하는 특성을 전달하여 호출하라. 이 메서드는 연관된 `AVMediaSelectionGroup` 객체를 반환하거나, 지정된 특성에 대한 그룹이 없는 경우 `nil` 을 반환한다. `AVMediaSelectionGroup` 이 상호 배타적인 `AVMediaSelectionOption` 객체 컬렉션의 컨테이너 역할을 한다. 다음 예제에서는 에셋의 미디어 선택 그룹을 검색하고 사용 가능한 옵션을 표시하는 방법을 보여 준다.

```swift
for characteristic in asset.availableMediaCharacteristicsWithMediaSelectionOptions {
 
    print("\(characteristic)")
 
    // Retrieve the AVMediaSelectionGroup for the specified characteristic.
    if let group = asset.mediaSelectionGroup(forMediaCharacteristic: characteristic) {
        // Print its options.
        for option in group.options {
            print("  Option: \(option.displayName)")
        }
    }
}
```

오디오 및 자막 미디어 옵션이 포함된 에셋의 출력은 다음과 유사하다.

```text
[AVMediaCharacteristicAudible]
  Option: English
  Option: Spanish
[AVMediaCharacteristicLegible]
  Option: English
  Option: German
  Option: Spanish
  Option: French
```

특정 미디어 특성에 대한 `AVMediaSelectionGroup` 객체를 검색하고 원하는 `AVMediaSelectionOption` 객체를 확인한 후 다음 단계를 선택하라. 활성 `AVPlayerItem` 에서 [`selectMediaOption:inMediaSelectionGroup:`](https://developer.apple.com/documentation/avfoundation/avplayeritem/1389610-selectmediaoption) 를 호출하여 미디어 옵션을 선택하라. 예를 들어, 에셋의 스페인어 자막 옵션을 표시하려면 다음과 같이 선택하라.

```swift
if let group = asset.mediaSelectionGroup(forMediaCharacteristic: AVMediaCharacteristicLegible) {
    let locale = Locale(identifier: "es-ES")
    let options =
        AVMediaSelectionGroup.mediaSelectionOptions(from: group.options, with: locale)
    if let option = options.first {
        // Select Spanish-language subtitle option
        playerItem.select(option, in: group)
    }
}
```

미디어 옵션을 선택하면 바로 프리젠테이션이 가능해진다. 자막 또는 폐쇄 캡션 옵션을 선택하면 `AVPlayerViewController`, `AVPlayerView` 및 `AVPlayerLayer` 에서 제공하는 비디오 디스플레이 내의 관련 텍스트가 표시된다. 대체 오디오 또는 비디오 옵션을 선택하면 현재 표시된 미디어가 새로운 선택 미디어로 대체된다. 

> `참고`: iOS 7.0 및 OS X 10.9를 시작으로, `AVPlayer` 는 기본 동작으로 사용자의 시스템 기본 설정에 따른 자동 미디어 선택을 제공한다. 미디어 선택 항목이 표시되는 시기를 제어하려면 플레이어의 `appliesMediaSelectionCriteriaAutomatically` 값을 `NO`로 설정하여 기본 동작을 실행 중지하라.

### Working with the iOS Audio Environment

iOS 오디오 세션 API를 사용하여 실행 중인 장치의 전체 오디오 컨텍스트 내에서 애플리케이션의 일반적인 오디오 동작 및 역할을 정의하라. 다음 섹션에서는 앱의 오디오 재생을 관리하고 제어하는 추가적인 방법과 더 큰 iOS 오디오 환경의 변화에 대응하는 방법에 대해 설명한다.

#### Playing Background Audio

많은 미디어 재생 앱의 일반적인 특징은 앱이 백그라운드로 전송될 때 오디오를 계속 재생하는 것이다. 이는 사용자가 애플리케이션을 전환하거나 기기를 잠근 결과일 수 있다. 앱이 백그라운드 오디오를 재생할 수 있도록 하려면 [Configuring Audio Settings for iOS and tvOS](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/MediaPlaybackGuide/Contents/Resources/en.lproj/ConfiguringAudioSettings/ConfiguringAudioSettings.html#//apple_ref/doc/uid/TP40016757-CH9-SW1) 에 설명된 대로 앱의 기능 및 오디오 세션을 구성하라.

MP3 또는 M4A 파일과 같은 오디오 전용 에셋을 재생하는 경우, 설정이 완료되며 앱이 백그라운드 오디오를 재생할 수 있다. 비디오 에셋의 오디오 부분을 재생해야 하는 경우 추가 단계가 필요하다. 플레이어의 현재 항목이 디스플레이에 비디오를 표시하는 경우, 앱이 백그라운드에 전송될 때 `AVPlayer` 의 재생이 자동으로 일시 중지된다. 오디오 재생을 계속하려면 다음 예와 같이 백그라운드에 진입시 프레젠테이션에서 `AVPlayer` 인스턴스를 분리한 후 포그라운드로 돌아갈 때 다시 연결하라.

```swift
func applicationDidEnterBackground(_ application: UIApplication) {
    // Disconnect the AVPlayer from the presentation when entering background
 
    // If presenting video with AVPlayerViewController
    playerViewController.player = nil
 
    // If presenting video with AVPlayerLayer
    playerLayer.player = nil
}
 
func applicationWillEnterForeground(_ application: UIApplication) {
    // Reconnect the AVPlayer to the presentation when returning to foreground
 
    // If presenting video with AVPlayerViewController
    playerViewController.player = player
 
    // If presenting video with AVPlayerLayer
    playerLayer.player = player
}
```

#### Controlling Background Audio

백그라운드에서 오디오를 재생하는 경우 제어 센터와 iOS 락 화면에서 원격으로 재생 제어를 지원해야 한다. 재생 제어와 함께 현재 재생 중인 작업에 대한 의미 있는 정보도 이러한 인터페이스에서 제공해야 한다. 이 기능을 구현하려면 MediaPlayer 프레임워크의 [`MPRemoteCommandCenter`](https://developer.apple.com/documentation/mediaplayer/mpremotecommandcenter) 및 [`MPNowPlayingInfoCenter`](https://developer.apple.com/documentation/mediaplayer/mpnowplayinginfocenter) 클래스를 사용하라.

`MPRemoteCommandCenter` 클래스는 외부 부속품 및 시스템 전송 제어에 의해 전송되는 원격 제어 이벤트를 처리하기 위해 객체를 보증한다. [`MPRemoteCommand`](https://developer.apple.com/documentation/mediaplayer/mpremotecommand) 객체의 형태로 다양한 명령을 정의하며, 사용자 지정 이벤트 핸들러를 연결하여 앱에서 재생을 제어할 수 있다. 예를 들어 앱의 재생 및 일시 중지 동작을 원격으로 제어하려면 공유 명령 센터를 참조하고 아래 나온 것처럼 적절한 명령에 대한 처리기를 제공하라.

```swift
func setupRemoteTransportControls() {
    // Get the shared MPRemoteCommandCenter
    let commandCenter = MPRemoteCommandCenter.shared()
 
    // Add handler for Play Command
    commandCenter.playCommand.addTarget { [unowned self] event in
        if self.player.rate == 0.0 {
            self.player.play()
            return .success
        }
        return .commandFailed
    }
 
    // Add handler for Pause Command
    commandCenter.pauseCommand.addTarget { [unowned self] event in
        if self.player.rate == 1.0 {
            self.player.pause()
            return .success
        }
        return .commandFailed
    }
}
```

앞의 예에서는 [`addTargetWithHandler:`](https://developer.apple.com/documentation/mediaplayer/mpremotecommand/1622910-addtargetwithhandler) 메서드를 사용하여 명령 센터의 [`playCommand`](https://developer.apple.com/documentation/mediaplayer/mpremotecommandcenter/1619000-playcommand) 및 [pauseCommand](https://developer.apple.com/documentation/mediaplayer/mpremotecommandcenter/1618979-pausecommand) 명령의 핸들러를 추가하는 방법을 보여준다. 이 메서드에 부여한 콜백 블록을 사용하려면 명령의 성공 여부를 나타내는 [`MPRemoteCommandHandlerStatus`](https://developer.apple.com/documentation/mediaplayer/mpremotecommandhandlerstatus) 값을 반환해야 한다.

원격 명령 핸들러를 구성한 후 다음 단계는 iOS 잠금 화면과 Control Center의 전송 영역에 표시할 메타데이터를 제공하는 것이다. [`MPMediaItem`](https://developer.apple.com/documentation/mediaplayer/mpmediaitem) 및 [`MPNowPlayingInfoCenter`](https://developer.apple.com/documentation/mediaplayer/mpnowplayinginfocenter) 에서 정의한 키를 사용하여 메타데이터의 딕셔너리를 제공하고 해당 딕셔너리를 `MPNowPlayingInfoCenter` 의 기본 인스턴스에 설정하라. 다음 예제는 재생 타이밍 값과 함께 표시된 미디어의 제목 및 아트워크를 설정하는 방법을 보여 준다.

```swift
func setupNowPlaying() {
    // Define Now Playing Info
    var nowPlayingInfo = [String : Any]()
    nowPlayingInfo[MPMediaItemPropertyTitle] = "My Movie"
    if let image = UIImage(named: "lockscreen") {
        nowPlayingInfo[MPMediaItemPropertyArtwork] =
            MPMediaItemArtwork(boundsSize: image.size) { size in
                return image
        }
    }
    nowPlayingInfo[MPNowPlayingInfoPropertyElapsedPlaybackTime] = playerItem.currentTime().seconds
    nowPlayingInfo[MPMediaItemPropertyPlaybackDuration] = playerItem.asset.duration.seconds
    nowPlayingInfo[MPNowPlayingInfoPropertyPlaybackRate] = player.rate
 
    // Set the metadata
    MPNowPlayingInfoCenter.default().nowPlayingInfo = nowPlayingInfo
}
```

이 예제는 원격 제어 인터페이스에서 플레이어 타이밍과 진행률을 표시할 수 있는 [`nowPlayingInfo`](https://developer.apple.com/documentation/mediaplayer/mpnowplayinginfocenter/1615903-nowplayinginfo) 딕셔너리의 일부로 동적 타이밍을 전달한다. 이렇게 하면 앱이 백그라운드에 진입할 때 이 타이밍의 최신 스냅샷을 제공하라. 미디어 정보나 타이밍이 중요한 방식으로 변경되는 경우 앱이 백그라운드에 있는 동안 `nowPlayingInfo` 를 업데이트할 수도 있다.

#### Responding to Interruptions

인터럽트는 iOS 사용자 경험의 일반적인 부분이다. 예를 들어, 비디오 앱에서 영화를 보고 전화나 페이스타임 요청을 받으면 어떤 일이 일어날지 생각해봐라. 이 시나리오에서는 동영상의 오디오가 빠르게 희미해지고 재생이 일시 중지되며 벨소리도 희미해진다. 통화나 요청을 거절하면 비디오 앱으로 컨트롤이 반환되고, 동영상의 오디오가 사라지면서 재생이 다시 시작된다. 이 동작의 중심에는 앱의 `AVAudioSession` 이 있다. 인터럽트가 시작되고 종료될 때, 등록된 관찰자에게 적절한 조치를 취할 수 있도록 알린다. `AVPlayer` 는 오디오 세션을 인식하고 `AVAudioSession` 인터럽트 이벤트에 대응하여 자동으로 재생을 일시 중지하고 다시 시작한다. 이 `AVPlayer` 동작을 관찰하려면 플레이어의 `rate` 프로퍼티에 키-값 관찰\(KVO\)를 사용하여 플레이어가 일시중지되었다가 중단에 대한 응답으로 다시 시작될 때 사용자 인터페이스를 업데이트하라. 

당신은 또한 `AVAudioSession` 에서 게시한 인터럽트 알림도 직접 관찰할 수 있다. 이 기능은 경로 변경과 같은 다른 이유로 인해 재생을 일시 중지했는지 알고 싶을 때 유용할 수 있다. 오디오 인터럽트를 관찰하려면 등록부터 시작하여 [`AVAudioSessionInterruptionNotification`](https://developer.apple.com/documentation/avfoundation/avaudiosession/1616596-interruptionnotification) 타입의 통지를 관찰하라.

```swift
func setupNotifications() {
    let notificationCenter = NotificationCenter.default
    notificationCenter.addObserver(self,
                                   selector: #selector(handleInterruption),
                                   name: .AVAudioSessionInterruption,
                                   object: nil)
}
 
func handleInterruption(notification: Notification) {
 
}
```

게시된 `NSNotification` 객체는 인터럽트에 대한 세부 정보를 제공하는 채워진 `userInfo` 딕셔너리를 포함하고 있다. `userInfo` 딕셔너리에서 `AVAudioSessionInterruptionType` 값을 되찾아 중단 유형을 결정한다. 중단 유형은 중단이 시작되었는지 종료되었는지 여부를 나타낸다.

```swift
func handleInterruption(notification: Notification) {
    guard let userInfo = notification.userInfo,
        let typeValue = userInfo[AVAudioSessionInterruptionTypeKey] as? UInt,
        let type = AVAudioSessionInterruptionType(rawValue: typeValue) else {
            return
    }
    if type == .began {
        // Interruption began, take appropriate actions
    }
    else if type == .ended {
        if let optionsValue = userInfo[AVAudioSessionInterruptionOptionKey] as? UInt {
            let options = AVAudioSessionInterruptionOptions(rawValue: optionsValue)
            if options.contains(.shouldResume) {
                // Interruption Ended - playback should resume
            } else {
                // Interruption Ended - playback should NOT resume
            }
        }
    }
}
```

중단 유형이 [`AVAudioSessionInterruptionTypeEnded`](https://developer.apple.com/documentation/avfoundation/avaudiosessioninterruptiontype/avaudiosessioninterruptiontypeended) 인 경우, `userInfo` 딕셔너리에는 AVAudioSessionInterruptionOptions 값이 포함되어 있다.

#### Responding to Route Changes

`AVAudioSession` 에서 처리하는 중요한 책임은 오디오 경로 변경을 관리한다. iOS 기기에 오디오 입력 또는 출력이 추가되거나 제거될 때 경로 변경이 일어난다. 경로 변경은 사용자가 헤드폰을 꽂거나, Bluetooth LE 헤드셋을 연결하거나, USB 오디오 인터페이스를 분리하는 등 여러 가지 이유로 발생한다. 이러한 변경이 발생하면, `AVAudioSession` 은 그에 따라 오디오 신호를 다시 라우팅하고, 변경사항의 세부사항이 포함된 통지를 등록된 관찰자에게 브로드캐스트한다.

사용자가 헤드폰을 꽂거나 제거할 때 경로 변경과 관련된 중요한 요건이 발생한다. \(iOS 휴먼 인터페이스의 소리를 참조하라\) 사용자가 유선 또는 무선 헤드폰을 연결할 때, 그들은 은연중에 오디오 재생을 계속해야 함을 암시하고 있지만, 비공개로 표시한다. 이들은 현재 미디어를 재생하고 있는 앱이 중단 없이 계속 재생될 것으로 기대하고 있다. 사용자들은 헤드폰의 플러그를 뽑을 때, 그들이 듣고 있는 것을 다른 사람들과 자동으로 공유하기를 원하지 않는다. 애플리케이션은 내포된 개인 정보 요청을 존중해야 하며 헤드폰이 제거될 때 자동으로 재생을 일시 중지해야 한다.

`AVPlayer` 는 앱의 오디오 세션을 인식하고 경로 변경에 적절하게 대응한다. 헤드폰을 연결하면 재생이 예상대로 계속된다. 헤드폰을 제거하면 재생이 자동으로 일시 중지된다. 이 `AVPlayer` 동작을 관찰하려면 플레이어의 `rate` 프로퍼티에 KVO를 사용하여 오디오 경로 변경에 대한 응답으로 플레이어가 일시 중지될 때 사용자 인터페이스를 업데이트하라.

또한 `AVAudioSession` 에서 게시한 모든 경로 알림을 직접 관찰할 수 있다. 이 기능은 사용자가 헤드폰을 연결하거나 제거할 때 플레이어 인터페이스에 아이콘 또는 메시지를 표시하려는 경우에 유용할 수 있다. 오디오 경로 변경을 관찰하려면 등록부터 시작하여 [`AVAudioSessionRouteChangeNotification`](https://developer.apple.com/documentation/avfoundation/avaudiosession/1616493-routechangenotification) 타입의 통지를 관찰하라.

```swift
func setupNotifications() {
    let notificationCenter = NotificationCenter.default
    notificationCenter.addObserver(self,
                                   selector: #selector(handleRouteChange),
                                   name: .AVAudioSessionRouteChange,
                                   object: nil)
}
 
func handleRouteChange(notification: Notification) {
 
}
```

게시된 `NSNotification` 객체는 경로 변경에 대한 세부 정보를 제공하는 채워진 `userInfo` 딕셔너리를 포함한다. `userInfo` 딕셔너리에서 `AVAudioSessionRouteChangeReason` 값을 되찾아 이 변경 이유를 확인할 수 있다. 새 장치가 연결되면 그 이유는 [`AVAudioSessionRouteChangeReasonNewDeviceAvailable`](https://developer.apple.com/documentation/avfoundation/avaudiosession/routechangereason/newdeviceavailable) 이며, 하나를 제거하면 [`AVAudioSessionRouteChangeReasonOldDeviceUnavailable`](https://developer.apple.com/documentation/avfoundation/avaudiosession/routechangereason/olddeviceunavailable) 이다.

```swift
func handleRouteChange(notification: Notification) {
    guard let userInfo = notification.userInfo,
        let reasonValue = userInfo[AVAudioSessionRouteChangeReasonKey] as? UInt,
        let reason = AVAudioSessionRouteChangeReason(rawValue:reasonValue) else {
            return
    }
    switch reason {
    case .newDeviceAvailable:
        // Handle new device available.
    case .oldDeviceUnavailable:
        // Handle old device removed.
    default: ()
    }
}
```

새 장치를 사용할 수 있게 되면 `AVAudioSession` 에 `currentRoute` 를 요청하여 오디오 출력이 현재 라우팅된 위치를 확인하라. 그러면 오디오 세션의 모든 [inputs](https://developer.apple.com/documentation/avfoundation/avaudiosessionroutedescription/1616474-inputs) 및 [outputs](https://developer.apple.com/documentation/avfoundation/avaudiosessionroutedescription/1616552-outputs) 가 나열된 [`AVAudioSessionRouteDescription`](https://developer.apple.com/documentation/avfoundation/avaudiosessionroutedescription) 이 반환된다. 장치가 제거되면 `userInfo` 딕셔너리에서 이전 경로에 대한 `AVAudioSessionRouteDescription` 을 되찾아라. 두 경우 모두 오디오 출력 경로의 세부 정보를 제공하는 [`AVAudioSessionPortDescription`](https://developer.apple.com/documentation/avfoundation/avaudiosessionportdescription) 객체 배열을 반환하는 출력에 대한 경로 설명을 쿼리하라.

```swift
func handleRouteChange(notification: Notification) {
    guard let userInfo = notification.userInfo,
        let reasonValue = userInfo[AVAudioSessionRouteChangeReasonKey] as? UInt,
        let reason = AVAudioSessionRouteChangeReason(rawValue:reasonValue) else {
            return
    }
    switch reason {
    case .newDeviceAvailable:
        let session = AVAudioSession.sharedInstance()
        for output in session.currentRoute.outputs where output.portType == AVAudioSessionPortHeadphones {
            headphonesConnected = true
            break
        }
    case .oldDeviceUnavailable:
        if let previousRoute =
            userInfo[AVAudioSessionRouteChangePreviousRouteKey] as? AVAudioSessionRouteDescription {
            for output in previousRoute.outputs where output.portType == AVAudioSessionPortHeadphones {
                headphonesConnected = false
                break
            }
        }
    default: ()
    }
}
```



