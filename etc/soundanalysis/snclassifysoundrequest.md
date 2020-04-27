---
description: Core ML 모델을 사용하여 사운드 분류를 수행하는 요청.
---

# SNClassifySoundRequest

### Declaration

```swift
class SNClassifySoundRequest : NSObject
```

### 

### Topics

#### Creating a Request

* [`init(mlModel: MLModel)`](https://developer.apple.com/documentation/soundanalysis/snclassifysoundrequest/3182417-init) 제공된 [`MLModel`](https://developer.apple.com/documentation/coreml/mlmodel)과 함께 사운드 분류 요청을 초기화하라.

#### Configuring a Request

* [`var overlapFactor: Double`](https://developer.apple.com/documentation/soundanalysis/snclassifysoundrequest/3203753-overlapfactor) 모델이 고정 크기 오디오 블록에서 작동할 때, 분석 윈도우의 겹침.



