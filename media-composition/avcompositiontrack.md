---
description: '미디어 유형, 트랙 식별자 및 트랙 세그먼트로 구성된 컴포지션 객체의 트랙.'
---

# AVCompositionTrack

### Declaration

```swift
class AVCompositionTrack : AVAssetTrack
```

### Overview

컴포지션 트랙에서 첫 번째 트랙 세그먼트의 `timeMapping.target.start`는 `kCMTimeZero`이며 각 후속 트랙 세그먼트의 `timeMapping.target.start`는 `CMTimeRangeGetEnd`\(`<#previousTrackSegment #>.timeMapping.target`\)와 같다.

AVFoundation 프레임워크는 또한 mutable 가능한 하위 클래스인 [`AVMutableCompositionTrack`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack)도 지원한다.

### Topics

#### Accessing Track Segments

[`func segment(forTrackTime: CMTime) -> AVCompositionTrackSegment?`](https://developer.apple.com/documentation/avfoundation/avcompositiontrack/2865555-segment)

제공된 트랙 시간을 포함하거나 가장 근접한 세그먼트 배열에서 컴포지션 트랙 세그먼트를 반환한다.

[`var segments: [AVCompositionTrackSegment]`](https://developer.apple.com/documentation/avfoundation/avcompositiontrack/1387267-segments)

컴포지션 트랙의 트랙 세그먼트

#### Instance Properties

[`var formatDescriptionReplacements: [AVCompositionTrackFormatDescriptionReplacement]`](https://developer.apple.com/documentation/avfoundation/avcompositiontrack/3194194-formatdescriptionreplacements)

