---
description: 믹스에 오디오 트랙을 추가할 때 사용하는 매개 변수.
---

# AVMutableAudioMixInputParameters

### Declaration

```swift
class AVMutableAudioMixInputParameters : AVAudioMixInputParameters
```

### Topics

#### Creating Input Parameters

[`init(track: AVAssetTrack?)`](https://developer.apple.com/documentation/avfoundation/avmutableaudiomixinputparameters/1386858-init)

주어진 트랙에 대해 변경가능한 입력 매개변수 객체를 생성한다.

#### Managing the Track ID

[`var trackID: CMPersistentTrackID`](https://developer.apple.com/documentation/avfoundation/avmutableaudiomixinputparameters/1389209-trackid)

매개 변수를 적용해야 하는 오디오 트랙의 식별자.

#### Setting the Volume

[`func setVolume(Float, at: CMTime)`](https://developer.apple.com/documentation/avfoundation/avmutableaudiomixinputparameters/1389875-setvolume)

지정된 시간에 시작하는 오디오 볼륨 값을 설정한다.

[`func setVolumeRamp(fromStartVolume: Float, toEndVolume: Float, timeRange: CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avmutableaudiomixinputparameters/1386056-setvolumeramp)

지정된 시간 범위 동안 적용할 볼륨 램프를 설정한다.

#### Getting an Audio Tap

[`var audioTapProcessor: MTAudioProcessingTap?`](https://developer.apple.com/documentation/avfoundation/avmutableaudiomixinputparameters/1389296-audiotapprocessor)

트랙과 연결된 오디오 처리 탭.

#### Time Pitch Settings

[`var audioTimePitchAlgorithm: AVAudioTimePitchAlgorithm?`](https://developer.apple.com/documentation/avfoundation/avmutableaudiomixinputparameters/1388300-audiotimepitchalgorithm)

스케일링된 오디오 편집의 오디오 피치를 관리하는 데 사용되는 처리 알고리즘.

[`struct AVAudioTimePitchAlgorithm`](https://developer.apple.com/documentation/avfoundation/avaudiotimepitchalgorithm)

속도가 변경될 때 오디오 피치를 설정하는 데 사용되는 알고리즘.  


