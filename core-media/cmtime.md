---
description: 타임스탬프 또는 지속시간과 같은 시간 값을 나타내는 구조체.
---

# CMTime

### Overview

이 문서에서는 `CMTime` 구조체를 만들고 조작하기 위한 API를 설명한다.

`CMTime` 구조체는 시간\(타임 스탬프 또는 지속 시간\)을 나타내는 변경 불가능한 구조체이다.

`CMTime`은 분자\(`int64_t` 값\)과 분모\(`int32_t` 시간 척도\)를 가진 합리적인 숫자로 표현된다. 개념적으로, 시간 척도는 분자의 각 단위가 차지하는 초의 분율을 명시한다. 따라서 시간 척도가 4이면 각 단위는 1/4초를 나타내고, 시간 척도가 10이면 각 단위는 1/10초를 나타낸다. 간단한 시간 값 외에도 `CMTime`은 `+infinity`, `-infinity` 및 `indefinite`의 비 숫자 값을 나타낼 수 있다. 플래그 `CMTime`을 사용하면 시간이 어느 시점에서 반올림되었는지 여부를 나타낸다.

`CMTimes`에는 일반적으로 0으로 설정되지만 관련 없는 타임라인을 구분하는 데 사용할 수 있는 에폭스 번호가 포함되어 있다: 예를 들어, 루프 0의 시간 N과 루프 1의 시간 N을 구별하기 위해 프리젠테이션 루프를 통해 매번 증가될 수 있다.

[`CMTimeCopyAsDictionary(_:allocator:)`](https://developer.apple.com/documentation/coremedia/1400845-cmtimecopyasdictionary) 및 [`CMTimeMakeFromDictionary(_:)`](https://developer.apple.com/documentation/coremedia/1400819-cmtimemakefromdictionary)를 사용하여 `CMTimes`를 불변 CFDictionaries\([`CFDictionary`](https://developer.apple.com/documentation/corefoundation/cfdictionary) 참조\)로 변환하여 주석 및 다양한 Core Foundation 컨테이너에 사용할 수 있다.

날짜 및 시간 관리를 위한 추가 함수는 [Time Utilities](https://developer.apple.com/documentation/corefoundation/time_utilities)에 설명되어 있다. CMTime은 미디어 타임라인을 위해 설계된 반면 Time Utilities Reference의 함수는 Core Foundation 프레임워크에서 벽 시계 시간과 함께 작업하도록 설계되었다. [AVFoundation Constants](https://developer.apple.com/documentation/avfoundation/avfoundation_constants) 참조.

### Topics

#### Creating Times

#### Performing Common Operations

#### Inspecting and Evaluating Times

#### Utility Functions

#### Data Types

#### Constants

