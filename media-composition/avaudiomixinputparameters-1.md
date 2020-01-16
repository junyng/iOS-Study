---
description: 믹스에 오디오 트랙을 추가할 때 적용하는 매개 변수를 나타내는 객체.
---

# AVAudioMixInputParameters

### Declaration

```swift
class AVAudioMixInputParameters : NSObject
```

### Overview

오디오 믹스에 입력하기 위해 오디오 볼륨 램프를 적용하려면 `AVAudioMixInputParameters` 인스턴스를 사용하라. 믹스 파라미터는 [`trackID`](https://developer.apple.com/documentation/avfoundation/avaudiomixinputparameters/1387471-trackid) 속성을 통해 오디오 트랙과 연관된다.

오디오 볼륨은 시간 변동 매개변수로 지원된다. `AVAudioMixInputParameters`는 변경가능한 서브클래스 [`AVMutableAudioMixInputParameters`](https://developer.apple.com/documentation/avfoundation/avmutableaudiomixinputparameters) 를 가지고 있다.

볼륨이 처음 설정되기 전에는 1.0의 볼륨이 사용되며, 볼륨이 마지막으로 설정된 후에는 마지막 볼륨이 사용된다. 볼륨 램프의 시간 범위 내에서 볼륨은 램프의 시작 볼륨과 종료 볼륨 사이에 보간된다. 예를 들어, 볼륨을 0시에 1.0으로 설정하고, 또한 \[4.0, 5.0\]의 시간 범위에서 0.5에서 0.2로 볼륨 램프를 설정하면 볼륨 상수가 0.0초에서 4.0초까지 유지되는 오디오 볼륨 파라미터가 생성되며, 그 후에 0.5로 점프하여 4.0초에서 9.0초까지 0.2로 내려가고, 그 후에 0.2로 일정하게 유지된다.

이는 객체의 불변한 변형이므로, 이 클래스의 버전을 직접 할당하고 초기화해서는 안된다. 다른 클래스는 이 클래스의 인스턴스를 반환할 수 있다.

### Topics

#### Getting the Track ID

[`var trackID: CMPersistentTrackID`](https://developer.apple.com/documentation/avfoundation/avaudiomixinputparameters/1387471-trackid)

매개 변수를 적용해야 하는 오디오 트랙의 식별자.

#### Getting Volume Ramps

[`func getVolumeRamp(for: CMTime, startVolume: UnsafeMutablePointer<Float>?, endVolume: UnsafeMutablePointer<Float>?, timeRange: UnsafeMutablePointer<CMTimeRange>?) -> Bool`](https://developer.apple.com/documentation/avfoundation/avaudiomixinputparameters/1389578-getvolumeramp)

지정된 시간을 포함하는 볼륨 램프를 검색한다.

#### Getting an Audio Tap

[`var audioTapProcessor: MTAudioProcessingTap?`](https://developer.apple.com/documentation/avfoundation/avaudiomixinputparameters/1388578-audiotapprocessor)

트랙과 연결된 오디오 처리 탭.

#### Getting the Time Pitch Algorithm Setting

[`var audioTimePitchAlgorithm: AVAudioTimePitchAlgorithm?`](https://developer.apple.com/documentation/avfoundation/avaudiomixinputparameters/1387042-audiotimepitchalgorithm)

스케일링된 오디오 편집의 오디오 피치를 관리하는데 사용되는 처리 알고리즘.

[`struct AVAudioTimePitchAlgorithm`](https://developer.apple.com/documentation/avfoundation/avaudiotimepitchalgorithm)

속도가 변경될 때 오디오 피치를 설정하는 데 사용되는 알고리즘.

