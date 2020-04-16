---
description: QuickTime 또는 ISO 기반 미디어 파일 형식을 따르는 시청각 컨테이너를 나타내는 변경가능한 객체이다.
---

# AVMutableMovie

### Declaration

```swift
class AVMutableMovie : AVMovie
```

### Overview

[`AVMovie`](https://developer.apple.com/documentation/avfoundation/avmovie)의 가변 서브 클래스인 `AVMutableMovie`는 친숙한 동영상 편집 모델을 지원하는 메서드를 제공한다. 예를 들어, `AVMutableMovie`를 사용하여 한 트랙에서 미디어 데이터를 복사하고 해당 데이터를 다른 트랙에 붙여 넣을 수 있다. `AVMutableMovie`를 사용하여 한 트랙에서 다른 트랙으로 트랙 참조를 설정할 수도 있다. \(예를 들어, 한 트랙을 다른 트랙의 챕터 트랙으로 설정\) 개별 트랙에서 편집 작업을 수행하려면 관련 클래스 [`AVMovieTrack`](https://developer.apple.com/documentation/avfoundation/avmovietrack) 및 [`AVMutableMovieTrack`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack)를 사용할 수 있다.

QuickTime 또는 ISO 기본 미디어 파일의 형식별 기능을 작동할 때만 `AVMovie` 및 `AVMutableMovie`를 사용한다. 일반적으로 `QuickTime` 동영상 파일이나 `ISO` 기본 미디어 파일을 열고 재생하기 위해 이러한 클래스를 사용할 필요가 없다. 대신 [`AVURLAsset`](https://developer.apple.com/documentation/avfoundation/avurlasset)와 [`AVPlayerItem`](https://developer.apple.com/documentation/avfoundation/avplayeritem)를 사용한다.

미디어 삽입을 수행할 때, `AVMutableMovie`는 재생에 최적화된 동영상 파일을 만들기 위해 소스 에셋의 트랙에서 미디어 데이터를 인터리빙한다. 그러나 일련의 미디어 삽입을 수행하려면 최적의 인터리빙이 되지 않는 동영상 파일이 발생할 수 있다. `AVMutableMovie` 인스턴스의 프리셋 [`AVAssetExportPresetPassthrough`](https://developer.apple.com/documentation/avfoundation/avassetexportpresetpassthrough)을 내보내고 [`shouldOptimizeForNetworkUse`](https://developer.apple.com/documentation/avfoundation/avassetexportsession/1390593-shouldoptimizefornetworkuse) 속성을 `YES`로 설정하고 [`AVAssetExportSession`](https://developer.apple.com/documentation/avfoundation/avassetexportsession)객체로 전달하여 잘 분리되고 자체적인 고속 시작 동영상 파일을 만들 수 있다.

### Topics

#### Creating a Mutable Movie

[`init(url: URL, options: [String : Any]?, error: ())`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1386052-init)

ISO 기본 미디어 파일의 QuickTime 동영상 파일에 저장된 동영상 헤더에서 변경 가능한 동영상 객체를 만든다.

[`init(data: Data, options: [String : Any]?, error: ())`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1388442-init)

[`NSData`](https://developer.apple.com/documentation/foundation/nsdata) 객체에 저장된 동영상에서 변경 가능한 동영 객체를 만든다.

[`init(settingsFrom: AVMovie?, options: [String : Any]?)`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1386408-init)

트랙없이 변경 가능한 동영상 객체를 만든다.

#### Modifying Tracks

[`func addMutableTrack(withMediaType: AVMediaType, copySettingsFrom: AVAssetTrack?, options: [String : Any]?) -> AVMutableMovieTrack?`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1390063-addmutabletrack)

대상 동영상에 빈 트랙을 추가한다.

[`func addMutableTracksCopyingSettings(from: [AVAssetTrack], options: [String : Any]?) -> [AVMutableMovieTrack]`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1389215-addmutabletrackscopyingsettings)

하나 이상의 빈 트랙을 대상 동영상에 추가하고 소스 트랙의 트랙 설정을 복사한다.

[`func removeTrack(AVMovieTrack)`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1386735-removetrack)

대상 동영상에서 지정된 트랙을 제거한다.

[`var tracks: [AVMutableMovieTrack]`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1387739-tracks)

변경 가능한 동영상의 트랙 배열.

[`func track(withTrackID: CMPersistentTrackID) -> AVMutableMovieTrack?`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1389467-track)

지정된 트랙 식별자가 있는 트랙을 나타내는 가변 동영상 트랙.

[`func tracks(withMediaCharacteristic: AVMediaCharacteristic) -> [AVMutableMovieTrack]`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1388547-tracks)

지정된 미디어 특성과 일치하는 변경 가능한 동영상 트랙 배열.

[`func tracks(withMediaType: AVMediaType) -> [AVMutableMovieTrack]`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1390443-tracks)

지정된 미디어 유형과 일치하는 가변 동영상 트랙 배열.

[`func mutableTrack(compatibleWith: AVAssetTrack) -> AVMutableMovieTrack?`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1388669-mutabletrack)

시간 범위를 삽입 할 수 있는 변경 가능한 동영상의 트랙에 대한 참조를 제공한다.

#### Modifying Time Ranges

[`func insertEmptyTimeRange(CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1387515-insertemptytimerange)

동영상에 빈 시간 범위를 추가한다.

[`func insertTimeRange(CMTimeRange, of: AVAsset, at: CMTime, copySampleData: Bool)`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1389598-inserttimerange)

에셋의 지정된 시간 범위에 있는 모든 트랙을 동영상에 삽입한다.

[`func removeTimeRange(CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1385605-removetimerange)

동영상에서 지정된 시간 범위를 제거한다.

[`func scale(CMTimeRange, toDuration: CMTime)`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1385653-scale)

동영상의 시간 범위의 지속 시간을 변경한다.

#### Configuring Movie Properties

[`var defaultMediaDataStorage: AVMediaDataStorage?`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1389320-defaultmediadatastorage)

동영상에 추가된 미디어 데이터의 기본 저장 컨테이너.

[`var metadata: [AVMetadataItem]`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1388742-metadata)

동영상에 저장된 메타데이터 배열.

[`var interleavingPeriod: CMTime`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1386969-interleavingperiod)

각 트랙에 대해 샘플의 인터리빙 실행 기간을 나타내는 시간.

[`var isModified: Bool`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1389960-ismodified)

동영상이 수정되었는지 여부를 나타내는 불 값.

[`var preferredRate: Float`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1387335-preferredrate)

동영상이 자연스럽게 재생될 속도.

[`var preferredTransform: CGAffineTransform`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1388771-preferredtransform)

표시 목적으로 동영상의 시각적 미디어 데이터에 대해 수행될 변환.

[`var preferredVolume: Float`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1388614-preferredvolume)

동영상의 가청 메타 데이터에 대한 선호되는 음량.

[`var timescale: CMTimeScale`](https://developer.apple.com/documentation/avfoundation/avmutablemovie/1390622-timescale)

 `moov` 원자를 포함하는 동영상의 시간 스케일.

