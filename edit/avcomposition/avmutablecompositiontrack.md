---
description: '저수준의 표현에 영향을 주지 않고 트랙 세그먼트를 삽입, 제거 및 축척하는 데 사용하는 컴포지션 객체의 변경가능한 트랙이다.'
---

# AVMutableCompositionTrack

### Declaration

```swift
class AVMutableCompositionTrack : AVCompositionTrack
```

### Overview

[`AVCompositionTrack`](https://developer.apple.com/documentation/avfoundation/avcompositiontrack) 는 트랙 세그먼트의 일시적인 정렬에 대한 제약 조건을 정의한다. \([`segments`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1390321-segments) 참조\) 변경 가능한 컴포지션에서 트랙 세그먼트 배열을 설정하면 [`validateSegments(_:)`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1388746-validatesegments) 를 사용하여 세그먼트가 제약 조건을 충족하는지 테스트 할 수 있다.

### Topics

#### Configuring Track Properties

[`var languageCode: String?`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1387192-languagecode)

ISO 639-2 / T 언어 코드로서 트랙과 관련된 언어.

[`var extendedLanguageTag: String?`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1388866-extendedlanguagetag)

RFC 4646 언어 태그로 트랙과 관련된 언어 태그.

[`var naturalTimeScale: CMTimeScale`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1389030-naturaltimescale)

외부 수치 변환없이 트랙의 시간 값을 조작할 수 있는 시간 스케일.

[`var preferredTransform: CGAffineTransform`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1388304-preferredtransform)

표시 목적을 위한 비주얼 미디어 데이터의 기본 변환.

[`var preferredVolume: Float`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1388649-preferredvolume)

가청 미디어 데이터의 선호 볼륨.

#### Associating Tracks

[`func addTrackAssociation(to: AVCompositionTrack, type: AVAssetTrack.AssociationType)`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/3013764-addtrackassociation)

두 트랙 사이에 특정 유형의 트랙 연결을 설정한다.

[`func removeTrackAssociation(to: AVCompositionTrack, type: AVAssetTrack.AssociationType)`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/3013765-removetrackassociation)

두 트랙 사이의 특정 연결 유형을 제거한다.

[`struct AVAssetTrack.AssociationType`](https://developer.apple.com/documentation/avfoundation/avassettrack/associationtype)

다른 트랙이 트랙과 연결되는 방식을 식별하기위한 상수.

#### Managing Time Ranges

[`func insertEmptyTimeRange(CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1389483-insertemptytimerange)

수신기 내에서 빈 시간 범위를 추가하거나 확장한다.

[`func insertTimeRange(CMTimeRange, of: AVAssetTrack, at: CMTime)`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1390691-inserttimerange)

소스 트랙의 시간 범위를 컴포지션의 트랙에 삽입한다.

[`func insertTimeRanges([NSValue], of: [AVAssetTrack], at: CMTime)`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1388629-inserttimeranges)

여러 소스 트랙의 시간 범위를 컴포지션의 트랙에 삽입한다.

[`func removeTimeRange(CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1386048-removetimerange)

수신기에서 지정된 시간 범위를 제거한다.

[`func scaleTimeRange(CMTimeRange, toDuration: CMTime)`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1388212-scaletimerange)

수신기에서 시간 범위의 지속 시간을 변경한다.

[`var segments: [AVCompositionTrackSegment]!`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1390321-segments)

컴포지션 트랙에서 트랙 세그먼트.

#### Validating Segments

[`func validateSegments([AVCompositionTrackSegment])`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/1388746-validatesegments)

주어진 트랙 세그먼트 배열이 컴포지션 트랙의 타이밍 규칙을 따르는 지 여부를 나타내는 불 값을 반환한다.

#### Instance Properties

[`var isEnabled: Bool`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/3334926-isenabled)

#### Instance Methods

[`func replaceFormatDescription(CMFormatDescription, with: CMFormatDescription?)`](https://developer.apple.com/documentation/avfoundation/avmutablecompositiontrack/3180005-replaceformatdescription)

