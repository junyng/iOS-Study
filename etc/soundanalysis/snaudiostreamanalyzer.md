---
description: 오디오 데이터 스트림을 분석하고 결과를 앱에 제공하기 위해 만든 객체.
---

# SNAudioStreamAnalyzer

### Declaration

```swift
class SNAudioStreamAnalyzer : NSObject
```

### Overview

이 클래스의 인스턴스를 사용하여 시간이 지남에 따라 일련의 오디오 버퍼로 표시되는 오디오 스트림을 분석한다.



### Topics

#### Creating an Analyzer

* [`init(format: AVAudioFormat)`](https://developer.apple.com/documentation/soundanalysis/snaudiostreamanalyzer/3182408-init) 새로운 오디오 스트림 분석기를 생성한다.

#### Managing Requests

* [`func add(SNRequest, withObserver: SNResultsObserving)`](https://developer.apple.com/documentation/soundanalysis/snaudiostreamanalyzer/3182404-add) 오디오 스트림 분석기에 새로운 분석 요청을 추가한다.
* [`func remove(SNRequest)`](https://developer.apple.com/documentation/soundanalysis/snaudiostreamanalyzer/3182409-remove) 오디오 스트림 분석기로부터 존재하는 요청을 제거한다.
* [`func removeAllRequests()`](https://developer.apple.com/documentation/soundanalysis/snaudiostreamanalyzer/3203752-removeallrequests) 오디오 스트림 분석기에서 모든 요청을 제거한다.

#### Analyzing Data

* [`func analyze(AVAudioBuffer, atAudioFramePosition: AVAudioFramePosition)`](https://developer.apple.com/documentation/soundanalysis/snaudiostreamanalyzer/3182405-analyze) 분석할 새로운 오디오 버퍼를 추가한다.
* [`func completeAnalysis()`](https://developer.apple.com/documentation/soundanalysis/snaudiostreamanalyzer/3182407-completeanalysis) 오디오 스트림이 종료되었고 분석기가 모든 오디오 버퍼의 처리를 완료했음을 표시한다.

