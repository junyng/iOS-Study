---
description: 스트리밍 및 파일 기반 오디오를 분석하여 특정 유형으로 분류한다.
---

# SoundAnalysis

### Overview

SoundAnalysis 프레임워크를 사용하여 오디오를 분석하고 웃음이나 박수갈채를 특정 유형으로 인식하라. 프레임워크는 [`MLSoundClassifier`](https://developer.apple.com/documentation/createml/mlsoundclassifier)에 의해 훈련된 Core ML 모델을 사용하여 분석을 수행한다. 프레임워크의 스트리밍 또는 파일-기반 오디오 분석 기능을 사용하면 지능형 오디오 인식의 안정성을 앱에 추가할 수 있다.

### Topics

#### Audio Analyzers

* [Analyzing Audio to Classify Sounds](https://developer.apple.com/documentation/soundanalysis/analyzing_audio_to_classify_sounds) 오디오를 분석하려면 SoundAnalysis 프레임워크와 Core ML을 사용하라.
* [`class SNAudioFileAnalyzer`](https://developer.apple.com/documentation/soundanalysis/snaudiofileanalyzer) 오디오 파일을 분석하고 결과를 앱에 제공하기 위해 만든 객체.
* [`class SNAudioStreamAnalyzer`](https://developer.apple.com/documentation/soundanalysis/snaudiostreamanalyzer) 오디오 데이터 스트림을 분석하고 결과를 앱에 제공하기 위해 생성한 객체.

#### Configuration and Classification

* [`class SNClassifySoundRequest`](https://developer.apple.com/documentation/soundanalysis/snclassifysoundrequest) Core ML 모델을 사용하여 사운드 분류를 수행하는 요청.
* [`class SNClassificationResult`](https://developer.apple.com/documentation/soundanalysis/snclassificationresult) 지정된 시간 범위의 최상위 분류가 포함된 결과.
* [`protocol SNResultsObserving`](https://developer.apple.com/documentation/soundanalysis/snresultsobserving) 앱이 분석 요청의 결과를 수신하는 인터페이스.
* [`class SNClassification`](https://developer.apple.com/documentation/soundanalysis/snclassification) 그 분류에 대한 신뢰수준과 결합된 분석된 사운드 모델의 분류.

#### Request Management

* [`protocol SNRequest`](https://developer.apple.com/documentation/soundanalysis/snrequest) 사운드 분석 요청의 구성을 정의하는 프로토콜.
* [`protocol SNResult`](https://developer.apple.com/documentation/soundanalysis/snresult) 사운드 분석 결과의 구성을 정의하는 프로토콜.

#### Errors

* [`let SNErrorDomain: String`](https://developer.apple.com/documentation/soundanalysis/snerrordomain) 사운드 분석 오류 도메인을 식별하는 문자열.
* [`struct SNError`](https://developer.apple.com/documentation/soundanalysis/snerror) 사운드 분석 프레임워크에서 발생한 오류.

