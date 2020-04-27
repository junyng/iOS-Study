---
description: 오디오 파일을 분석하고 결과를 앱에 제공하기 위해 생성하는 객체.
---

# SNAudioFileAnalyzer

### Declaration

```swift
class SNAudioFileAnalyzer : NSObject
```

### Topics

#### Creating an Analyzer

* [`init(url: URL)`](https://developer.apple.com/documentation/soundanalysis/snaudiofileanalyzer/3182401-init) 새로운 오디오 파일 분석기를 생성한다.

#### Managing Requests

* [`func add(SNRequest, withObserver: SNResultsObserving)`](https://developer.apple.com/documentation/soundanalysis/snaudiofileanalyzer/3182398-add) 오디오 파일 분석기에 새 분석 요청을 추가한다.
* [`func remove(SNRequest)`](https://developer.apple.com/documentation/soundanalysis/snaudiofileanalyzer/3182402-remove) 오디오 파일 분석기에 기존 요청을 제거한다.
* [`func removeAllRequests()`](https://developer.apple.com/documentation/soundanalysis/snaudiofileanalyzer/3203751-removeallrequests) 오디오 파일 분석기에서 모든 요청을 제거한다.

#### Analyzing Data

* [`func analyze()`](https://developer.apple.com/documentation/soundanalysis/snaudiofileanalyzer/3182399-analyze) 오디오 파일을 동기적으로 분석한다.
* [`func analyze(completionHandler: (Bool) -> Void)`](https://developer.apple.com/documentation/soundanalysis/snaudiofileanalyzer/3197866-analyze) 오디오 파일을 비동기적으로 분석한다.
* [`func cancelAnalysis()`](https://developer.apple.com/documentation/soundanalysis/snaudiofileanalyzer/3182400-cancelanalysis) 오디오 파일의 진행 중인 분석을 취소하라.

