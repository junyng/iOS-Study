---
description: 앱이 분석 요청의 결과를 수신하는 인터페이스.
---

# SNResultsObserving

### Declaration

```swift
protocol SNResultsObserving
```

### Topics

#### Handling Requests

* [`func request(SNRequest, didProduce: SNResult)`](https://developer.apple.com/documentation/soundanalysis/snresultsobserving/3182423-request) 지정된 시간 범위를 사용하여 앱에 새 분석 결과를 제공한다. **Required.**
* \*\*\*\*[`func request(SNRequest, didFailWithError: Error)`](https://developer.apple.com/documentation/soundanalysis/snresultsobserving/3182422-request) 이 요청을 처리하는 동안 발생하는 모든 오류를 제공한다.
* [`func requestDidComplete(SNRequest)`](https://developer.apple.com/documentation/soundanalysis/snresultsobserving/3182424-requestdidcomplete) 분석 요청이 정상적으로 완료되었음을 앱에 알린다.

