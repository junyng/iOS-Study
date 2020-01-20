---
description: 하나 이상의 트랙에서 오디오를 혼합하여 발생하는 오디오 샘플을 읽기 위한 인터페이스를 정의하는 객체.
---

# AVAssetReaderAudioMixOutput

### Declaration

```swift
class AVAssetReaderAudioMixOutput : AVAssetReaderOutput
```

### Overview

`AVAssetReaderAudioMixOutput`은 [`AVAssetReaderOutput`](https://developer.apple.com/documentation/avfoundation/avassetreaderoutput)의 구체적인 하위 클래스이다.

[`AVAssetReaderAudioMixOutput`](https://developer.apple.com/documentation/avfoundation/avassetreaderaudiomixoutput)의 인스턴스를 [`add(_:)`](https://developer.apple.com/documentation/avfoundation/avassetreader/1390110-add)를 사용하여 에셋 판독기에 추가하여 하나 이상의 에셋 트랙에서 믹스 오디오 데이터를 읽을 수 있다. 샘플을 기본 형식으로 읽거나 다른 형식으로 변환할 수 있다.

### Topics

#### Creating an Audio Mix Output

[`init(audioTracks: [AVAssetTrack], audioSettings: [String : Any]?)`](https://developer.apple.com/documentation/avfoundation/avassetreaderaudiomixoutput/1388883-init)

옵셔널 audioSettings를 사용하여 지정된 오디오 트랙에서 믹스 오디오를 읽기 위한 오디오 믹스 출력 인스턴스를 초기화한다.

#### Configuring Audio Properties

[`var audioMix: AVAudioMix?`](https://developer.apple.com/documentation/avfoundation/avassetreaderaudiomixoutput/1387074-audiomix)

출력의 오디오 믹스.

[`var audioSettings: [String : Any]?`](https://developer.apple.com/documentation/avfoundation/avassetreaderaudiomixoutput/1388860-audiosettings)

오디오 출력에 사용되는 오디오 설정.

[`var audioTracks: [AVAssetTrack]`](https://developer.apple.com/documentation/avfoundation/avassetreaderaudiomixoutput/1385635-audiotracks)

수신기가 혼합 오디오를 읽는 트랙.

[`var audioTimePitchAlgorithm: AVAudioTimePitchAlgorithm`](https://developer.apple.com/documentation/avfoundation/avassetreaderaudiomixoutput/1388713-audiotimepitchalgorithm)

스케일링된 오디오 편집의 오디오 피치를 관리하는 데 사용되는 처리 알고리즘.

