---
description: 기존 에셋에서 새로운 컴포지션을 만드는 데 사용되는 변경 가능한 객체이다.
---

# AVMutableComposition

### Declaration

```swift
class AVMutableComposition : AVComposition
```

### Overview

이 클래스는 트랙을 추가 및 제거하는 기능을 제공하며, 시간 범위를 추가, 제거 및 확장할 수 있다. 재생 또는 검사를 위해 다음과 같이 변경 불가능한 컴포지션의 스냅샷을 만들 수 있다.

```objectivec
AVMutableComposition *myMutableComposition =
    <#a mutable composition you want to inspect or play in its current state#>;
 
AVComposition *immutableSnapshotOfMyComposition = [myMutableComposition copy];
 
// Create a player to inspect and play the composition.
AVPlayerItem *playerItemForSnapshottedComposition =
    [[AVPlayerItem alloc] initWithAsset:immutableSnapshotOfMyComposition];
```

### Topics

#### Creating a Mutable Composition

[`init(urlAssetInitializationOptions: [String : Any]?)`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1390705-init)

새로운, 빈, 변경가능한 컴포지션을 반환한다.

#### Managing Time Ranges

[`func insertEmptyTimeRange(CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1386710-insertemptytimerange)

컴포지션의 모든 트랙 내에 빈 시간 범위를 추가하거나 확장한다.

[`func insertTimeRange(CMTimeRange, of: AVAsset, at: CMTime)`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1385943-inserttimerange)

지정된 에셋의 주어진 시간 범위 내에 있는 모든 트랙을 수신기에 삽입한다.

[`func removeTimeRange(CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1387768-removetimerange)

컴포지션의 모든 트랙에서 지정된 시간 범위를 제거한다.

[`func scaleTimeRange(CMTimeRange, toDuration: CMTime)`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1390549-scaletimerange)

주어진 시간 범위에서 모든 트랙의 지속시간을 변경한다.

#### Managing Tracks

[`var tracks: [AVMutableCompositionTrack]`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1389937-tracks)

컴포지션에 포함된 변경가능한 컴포지션 트랙의 배열.

[`func addMutableTrack(withMediaType: AVMediaType, preferredTrackID: CMPersistentTrackID) -> AVMutableCompositionTrack?`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1387601-addmutabletrack)

수신기에 빈 트랙 추가

[`func removeTrack(AVCompositionTrack)`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1386818-removetrack)

수신기에서 지정된 트랙 제거

[`func mutableTrack(compatibleWith: AVAssetTrack) -> AVMutableCompositionTrack?`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1386662-mutabletrack)

수신기에서 주어진 에셋 트랙의 시간 범위를 삽입할 수 있는 트랙을 반환한다.

[`func track(withTrackID: CMPersistentTrackID) -> AVMutableCompositionTrack?`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1390074-track)

지정된 트랙 ID와 연관된 컴포지션 트랙을 제공한다.

[`func tracks(withMediaCharacteristic: AVMediaCharacteristic) -> [AVMutableCompositionTrack]`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1388464-tracks)

에셋과 관련된 지정된 미디어 특성의 컴포지션 트랙을 제공한다.

[`func tracks(withMediaType: AVMediaType) -> [AVMutableCompositionTrack]`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1385724-tracks)

에셋과 연결된 지정된 미디어 유형의 컴포지션 트랙을 제공한다.

#### Configuring Video Size

[`var naturalSize: CGSize`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1390424-naturalsize)

에셋의 시각적 부분의 인코딩되거나 작성된 크기.

