---
description: HTTP 라이브 스트림을 재생하고 디스크의 스트림을 보존하여 오프라인으로 재생한다.
---

# Using AVFoundation to Play and Persist HTTP Live Streams

### Overview

이 샘플은 스트림에 해당하는 테이블의 행을 눌러 재생할 수 있는 HLS\(HTTP Live Streams\)의 카탈로그를 제공한다. HLS 스트림의 다운로드를 관리하려면 테이블의 행에 있는 스트림과 연결된 액세서리 버튼을 눌러라. 액세서리 버튼을 탭하면 다운로드를 시작하거나 이미 실행중인 다운로드를 취소하거나, 장치에서 다운로드한 HLS 스트림을 삭제할 수 있는 인터페이스를 제공하는 새로운 뷰 컨트롤러로 전환된다.

샘플은 HLS 스트림을 다운로드하기 위한 [`AVAggregateAssetDownloadTask`](https://developer.apple.com/documentation/avfoundation/avaggregateassetdownloadtask)를 생성하고 초기화 한다. 각 에셋의 미디어 선택 그룹에 대한 기본 미디어 선택 항목만 다운로드된다. \(이는 HLS 재생 목록의 EXT-X-MEDIA 태그에 YES로 표시된다.\)

_참조_

* 이 샘플은 FPS\(FairPlay Streaming\) 콘텐츠 저장을 지원하지 않는다. FPS 콘텐츠를 다운로드하는 방법을 설명하는 샘플 버전은 [FairPlay Streaming Server SDK](https://developer.apple.com/streaming/fps/)를 참조하라.

#### Play an HTTP Live Stream <a id="3016956"></a>

`AssetListTableViewController`는 이 샘플의 주요 사용자 인터페이스이다. 샘플은 재생, 다운로드, 다운로드 취소 및 삭제할 수 있는 에셋의 목록을 제공한다. `AssetListManager`는 `AssetListTableViewController`에 표시할 에셋 목록을 제공한다.

`AssetPlaybackManager`는 다운로드된 자산을 재생할 책임이 있으며, 관리하는 [`AVURLAsset`](https://developer.apple.com/documentation/avfoundation/avurlasset), [`AVPlayer`](https://developer.apple.com/documentation/avfoundation/avplayer) 및 [`AVPlayerItem`](https://developer.apple.com/documentation/avfoundation/avplayeritem) 객체에 대한 재생 관련 변경 사항을 모니터링하기 위해 키 값 관찰\(KVO\)을 사용한다. 플레이어 항목의 [`status`](https://developer.apple.com/documentation/avfoundation/avplayeritem/1389493-status)는 상태가 변경될 때 KVO 변경 알림을 전송한다. 앱은 이러한 변경을 모니터링하고 `status` 속성에 플레이어 항목이 재생 준비가 되었음을 표시하면 재생을 시작한다. 앱은 `AVURLAsset` [`isPlayable`](https://developer.apple.com/documentation/avfoundation/avasset/1385974-isplayable) 속성을 관찰하여 `AVPlayer`가 사용자의 기대에 부합하는 방식으로 자산의 내용을 재생할 수 있는지 여부를 결정한다. 앱은 또한 플레이어의 [`currentItem`](https://developer.apple.com/documentation/avfoundation/avplayer/1387569-currentitem) 속성을 관찰하여 주어진 스트림에 대해 생성된 플레이어 항목에 접근한다.

`StreamListManager` 클래스는 앱 번들에 `Streams.plist` 파일의 내용을 로드 및 읽기 관리한다.

항목을 재생하려면 테이블의 행 중 하나를 눌러라. 항목을 누르면 새 뷰 컨트롤러로 전환된다. 이러한 전환의 일부로 테이블 뷰는 `AssetPlaybackManager`를 생성하고 다음 예제에서 볼 수 있듯이 해당 에셋을 할당한다.

```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    super.prepare(for: segue, sender: sender)

    if segue.identifier == AssetListTableViewController.presentPlayerViewControllerSegueID {
        guard let cell = sender as? AssetListTableViewCell,
            let playerViewControler = segue.destination as? AVPlayerViewController else { return }

        /*
         Grab a reference for the destinationViewController to use in later delegate callbacks from
         AssetPlaybackManager.
         */
        playerViewController = playerViewControler

        // Load the new Asset to playback into AssetPlaybackManager.
        AssetPlaybackManager.sharedManager.setAssetForPlayback(cell.asset)
    }
}
```

에셋을 `AssetPlaybackManager`에 할당하면 에셋에 대한 `AVPlayerItem`을 생성하여 프로세스의 이전 에셋을 제거한다:

```swift
private var asset: Asset? {
    willSet {
        /// Remove any previous KVO observer.
        guard let urlAssetObserver = urlAssetObserver else { return }
        
        urlAssetObserver.invalidate()
    }
    
    didSet {
        if let asset = asset {
            urlAssetObserver = asset.urlAsset.observe(\AVURLAsset.isPlayable, options: [.new, .initial]) { [weak self] (urlAsset, _) in
                guard let strongSelf = self, urlAsset.isPlayable == true else { return }
                
                strongSelf.playerItem = AVPlayerItem(asset: urlAsset)
                strongSelf.player.replaceCurrentItem(with: strongSelf.playerItem)
            }
        } else {
            playerItem = nil
            player.replaceCurrentItem(with: nil)
            readyForPlayback = false
        }
    }
}
```

`AssetPlaybackManager`는 `KVO`를 사용하여 `AVPlayerItem` 객체의 상태를 모니터링하고, `status`가 재생 준비가 되면 재생을 시작한다:

```swift
playerItemObserver = playerItem?.observe(\AVPlayerItem.status, options: [.new, .initial]) { [weak self] (item, _) in
    guard let strongSelf = self else { return }
    
    if item.status == .readyToPlay {
        if !strongSelf.readyForPlayback {
            strongSelf.readyForPlayback = true
            strongSelf.delegate?.streamPlaybackManager(strongSelf, playerReadyToPlay: strongSelf.player)
        }
    } else if item.status == .failed {
        let error = item.error
        
        print("Error: \(String(describing: error?.localizedDescription))")
    }
```

#### Download an HTTP Live Stream <a id="3016957"></a>

`AssetPersistenceManager`는 이 샘플에서 다운로드되는 HLS 스트림을 관리하는 방법을 보여 주는 주요 클래스이다. 여기에는 다운로드를 시작하고 취소하는 메서드, 사용자 장치에 기본 에셋을 삭제하는 메서드, 다운로드 모니터링 메서드가 포함된다.

사용자가 해당 스트림의 테이블 뷰 셀에 있는 액세서리 버튼을 눌러 다운로드를 시작하면 AssetPersistenceManager의 인스턴스는 다음 메서드를 호출하여 `AVAggregateAssetDownloadTask` 객체를 생성하여 HLS 스트림의 [`AVURLAsset`](https://developer.apple.com/documentation/avfoundation/avurlasset)에 대한 여러 [`AVMediaSelection`](https://developer.apple.com/documentation/avfoundation/avmediaselection)을 다운로드한다:

```swift
func downloadStream(for asset: Asset) {

    // Get the default media selections for the asset's media selection groups.
    let preferredMediaSelection = asset.urlAsset.preferredMediaSelection

    /*
     Creates and initializes an AVAggregateAssetDownloadTask to download multiple AVMediaSelections
     on an AVURLAsset.
     
     For the initial download, we ask the URLSession for an AVAssetDownloadTask with a minimum bitrate
     corresponding with one of the lower bitrate variants in the asset.
     */
    guard let task =
        assetDownloadURLSession.aggregateAssetDownloadTask(with: asset.urlAsset,
                                                           mediaSelections: [preferredMediaSelection],
                                                           assetTitle: asset.stream.name,
                                                           assetArtworkData: nil,
                                                           options:
            [AVAssetDownloadTaskMinimumRequiredMediaBitrateKey: 265_000]) else { return }

    // To better track the AVAssetDownloadTask, set the taskDescription to something unique for the sample.
    task.taskDescription = asset.stream.name

    activeDownloadsMap[task] = asset

    task.resume()

    var userInfo = [String: Any]()
    userInfo[Asset.Keys.name] = asset.stream.name
    userInfo[Asset.Keys.downloadState] = Asset.DownloadState.downloading.rawValue
    userInfo[Asset.Keys.downloadSelectionDisplayName] = displayNamesForSelectedMediaOptions(preferredMediaSelection)

    NotificationCenter.default.post(name: .AssetDownloadStateChanged, object: nil, userInfo: userInfo)
}
```

**참조**

* 라이브 HLS 스트림이 진행 중인 동안에는 저장할 수 없다. 라이브 HLS 스트림을 저장하려고 하면 시스템에서 예외를 발생한다. VOD\(Video On Demand\) 스트림만 오프라인 재생을 지원한다.

#### Cancel an HTTP Live Stream <a id="3016958"></a>

해당 스트림의 테이블 보기 셀에 있는 액세서리 버튼을 눌러 액세서리 뷰를 표시한 다음 취소를 눌러 스트림 다운로드를 중지하라. `AssetPersenceManager`의 다음 기능은 `URLSessionTask` 의 [`cancel()`](https://developer.apple.com/documentation/foundation/urlsessiontask/1411591-cancel) 메서드를 호출하여 다운로드를 취소한다.

```swift
func cancelDownload(for asset: Asset) {
    var task: AVAggregateAssetDownloadTask?

    for (taskKey, assetVal) in activeDownloadsMap where asset == assetVal {
        task = taskKey
        break
    }

    task?.cancel()
}
```

#### Remove an HTTP Live Stream from Disk <a id="3016959"></a>

해당 스트림의 테이블 뷰 셀에 있는 액세서리 버튼을 눌러 액세서리 뷰를 표시한 다음, 다운로드한 스트림 파일을 삭제하라. `AssetPersistanceManager`의 다음 기능은 장치에서 다운로드한 스트림을 제거한다. 먼저 장치의 파일에 해당하는 에셋 URL을 확인한 다음 `FileManager` 의 [`removeItem(at:)`](https://developer.apple.com/documentation/foundation/filemanager/1413590-removeitem) 메서드를 호출하여 지정된 URL에서 다운로드한 스트림을 제거하라.

```swift
func deleteAsset(_ asset: Asset) {
    let userDefaults = UserDefaults.standard

    do {
        if let localFileLocation = localAssetForStream(withName: asset.stream.name)?.urlAsset.url {
            try FileManager.default.removeItem(at: localFileLocation)

            userDefaults.removeObject(forKey: asset.stream.name)

            var userInfo = [String: Any]()
            userInfo[Asset.Keys.name] = asset.stream.name
            userInfo[Asset.Keys.downloadState] = Asset.DownloadState.notDownloaded.rawValue

            NotificationCenter.default.post(name: .AssetDownloadStateChanged, object: nil,
                                            userInfo: userInfo)
        }
    } catch {
        print("An error occured deleting the file: \(error)")
    }
}
```

#### Measure Playback Performance <a id="3016960"></a>

`PerfMeasurements`에는 스트리밍 재생 중에 KPI\(핵심 성능 지표\)를 측정하는 유틸리티 코드가 포함되어 있다. 이 코드는 많은 계산을 위해 [`AVPlayerItemAccessLog`](https://developer.apple.com/documentation/avfoundation/avplayeritemaccesslog)를 사용한다. `AVPlayerItemAccessLog` 객체는 네트워크 재생에 대한 주요 메트릭을 축적하여 [`AVPlayerItemAccessLogEvent`](https://developer.apple.com/documentation/avfoundation/avplayeritemaccesslogevent) 인스턴스의 모음으로 제시한다. 각 이벤트 인스턴스는 재생의 중단되지 않은 각 기간과 관련된 데이터를 수집한다.

참조

* 재생 중에 콘솔의 다양한 성능 인디케이터를 볼 수 있다.

예를 들어, `AVPlayerItemAccessLog`에서 얻은 스트림을 재생하는 데 소요된 총 시간을 계산하는 코드는 다음과 같다:

```swift
var totalDurationWatched: Double {
    // Compute total duration watched by iterating through the AccessLog events.
    var totalDurationWatched = 0.0
    if accessLog != nil && !accessLog!.events.isEmpty {
        for event in accessLog!.events where event.durationWatched > 0 {
                totalDurationWatched += event.durationWatched
        }
    }
    return totalDurationWatched
}
```

