---
description: 시간 범위를 나타내는 구조체.
---

# CMTimeRange

이 문서에서는 `CMTimeRange` 구조체를 만들고 조작하기 위한 API를 설명한다.

`CMTimeRange` 구조체는 시간 범위를 나타내는 불투명하지 않은 변경 가능한 구조체이다. `CMTimeRange`는 두 개의 `CMTime` 구조체로 표시되며, 하나는 범위의 시작 시간을 지정하고 다른 하나는 범위의 지속 시간을 지정한다. 시간 범위는 시작 시간에 지속 시간을 추가하여 계산되는 종료 시간을 포함하지 않는다. 다음 식은 항상 거짓으로 평가된다:

```swift
CMTimeRangeContainsTime(range, CMTimeRangeGetEnd(range))
```

[`CMTimeRangeCopyAsDictionary(_:allocator:)`](https://developer.apple.com/documentation/coremedia/1462781-cmtimerangecopyasdictionary) 및  [`CMTimeRangeMakeFromDictionary(_:)`](https://developer.apple.com/documentation/coremedia/1462777-cmtimerangemakefromdictionary)를 사용하여 `CMTimeRanges`를 `CFDictionaries`\([`CFDictionary`](https://developer.apple.com/documentation/corefoundation/cfdictionary) 참조\)에서 주고 받을 수 있으며, 주석 및 다양한 Core Foundation 컨테이너에 사용할 수 있다.

지속 시간을 나타내는 `CMTime`의 epoch는 항상 0이어야 하며, 값은 음수가 아니어야 한다. 타임스탬프를 나타내는 `CMTime`의 epoch는 0이 아닐 수 있지만 범위 연산\([`CMTimeRangeGetUnion(_:otherRange:)`](https://developer.apple.com/documentation/coremedia/1462837-cmtimerangegetunion) 와 같은\)은 시작 필드가 같은 epoch 범위에 대해서만 수행할 수 있다. `CMTimeRanges`는 다른 epochs를 초월할 수 없다. 

날짜 및 시간 관리를 위한 추가 함수는 [Time Utilities](https://developer.apple.com/documentation/corefoundation/time_utilities)에 설명되어 있다. [AVFoundation Constants](https://developer.apple.com/documentation/avfoundation/avfoundation_constants)를 참조하라.

### Topics

#### Creating Time Ranges

#### Comparing Time Ranges

#### Inspecting Time Ranges

#### Utility Functions

#### Data Types

#### Constants

