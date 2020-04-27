---
description: 지정된 시간 범위의 최상위 분류가 포함된 결과.
---

# SNClassificationResult

### Declaration

```swift
class SNClassificationResult : NSObject
```



### Topics

#### Inspecting the Result

* [`var classifications: [SNClassification]`](https://developer.apple.com/documentation/soundanalysis/snclassificationresult/3182414-classifications) 신뢰도 점수별로 정렬되는 상위 분류 후보.
* [`var timeRange: CMTimeRange`](https://developer.apple.com/documentation/soundanalysis/snclassificationresult/3182415-timerange) 클라이언트가 제공한 오디오 스트림의 분류 결과가 해당하는 시간 범위.

