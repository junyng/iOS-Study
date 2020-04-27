---
description: 오디오를 분석하려면 SoundAnalysis 프레임워크와 Core ML을 사용하라.
---

# Analyzing Audio to Classify Sounds

### Overview

SoundAnalysis 프레임워크는 훈련된 Core ML 모델을 사용하여 스트리밍 또는 파일-기반 오디오를 분석하고 분류한다.

#### Prepare a Model <a id="3235286"></a>

SoundAnalysis 프레임워크는 ML [`MLSoundClassifier`](https://developer.apple.com/documentation/createml/mlsoundclassifier)를 사용하여 교육한 모델에서 작동하며, 그 후에 앱과 함께 번들로 제공된다. 모델 파일을 앱에 추가하면 Xcode는 자동으로 동일한 이름\(mlmodel 확장자 제외\)의 클래스를 자동으로 생성한다. 이 클래스의 인스턴스를 만들어 `SoundAnalysis` 프레임워크에 사용할 `MLModel`을 로드하라. 예를 들어, `InstrumentClassifier.mlmodel`이라는 모델 파일이 있는 경우, 아래와 같이 관련 `MLModel`을 로드하라.

```swift
let instrumentClassifier = InstrumentClassifier()
let model: MLModel = instrumentClassifier.model
```

#### Capture Streaming Audio <a id="3235282"></a>

아래 코드에 설명된 것처럼 AVFoundation의 [`AVAudioEngine`](https://developer.apple.com/documentation/avfoundation/avaudioengine)을 사용하여 내장형 또는 외장형 마이크에서 오디오 데이터를 캡처하면 분석용 데이터를 얻을 수 있다. 오디오 엔진 인스턴스를 자동으로 생성하면 장치의 기본 마이크에서 오디오 데이터에 접근할 수 있는 [`inputNode`](https://developer.apple.com/documentation/avfoundation/avaudioengine/1386063-inputnode)가 구성된다. 오디오 파이프라인을 통한 데이터 흐름을 시작하려면 엔진의 [`start()`](https://developer.apple.com/documentation/avfoundation/avaudioengine/1387024-start) 메서드를 호출하라.

```swift
func startAudioEngine() {
    // Create a new audio engine.
    audioEngine = AVAudioEngine()

    // Get the native audio format of the engine's input bus.
    inputBus = AVAudioNodeBus(0)
    inputFormat = audioEngine.inputNode.inputFormat(forBus: inputBus)
    
    do {
        // Start the stream of audio data.
        try audioEngine.start()
    } catch {
        print("Unable to start AVAudioEngine: \(error.localizedDescription)")
    }
}
```

> **Important**
>
> iOS 또는 watchOS에서 장치의 마이크에 접근하려면 앱의 Info.plist 파일에 [`NSMicrophoneUsageDescription`](https://developer.apple.com/documentation/bundleresources/information_property_list/nsmicrophoneusagedescription) 키에 대한 설명을 지정해야 한다.

#### Create a Stream Analyzer <a id="3261109"></a>

캡처된 오디오를 분석하려면 [`SNAudioStreamAnalyzer`](https://developer.apple.com/documentation/soundanalysis/snaudiostreamanalyzer)를 사용하라. 입력 장치의 기본 형식과 일치하는 PCM 오디오 형식으로 이 클래스의 인스턴스를 생성하라.

```swift
// Create a new stream analyzer.
streamAnalyzer = SNAudioStreamAnalyzer(format: inputFormat)
```

> **Important**
>
> 입력 장치의 오디오 형식이 변경되면 현재 `SNAudioStreamAnalyzer`를 폐기하고 입력의 업데이트된 오디오 형식과 일치하는 새 형식을 생성해야 한다.

분석 프로세스를 관찰하려면 [`SNResultsObserving`](https://developer.apple.com/documentation/soundanalysis/snresultsobserving) 프로토콜을 채택하는 객체를 생성하라. 이 프로토콜을 분석기가 새로운 결과를 생성하거나 오류를 발생시키거나 처리를 완료할 때 호출할 수 있도록 구현하는 방법을 정의한다.

예제는 다양한 악기 사운드를 인식하고 분류하도록 훈련된 사운드 분류기 모델을 사용한다. 아래와 같이, 모델이 분석된 오디오에서 악기를 인식할 때, 예측의 정확성에 대한 confidence와 함께 결과를 옵저버에게 전달한다. 결과가 수신되면 간단한 옵저버 구현이 콘솔 로그에 남는다.

```swift
// Observer object that is called as analysis results are found.
class ResultsObserver : NSObject, SNResultsObserving {
    
    func request(_ request: SNRequest, didProduce result: SNResult) {
        
        // Get the top classification.
        guard let result = result as? SNClassificationResult,
            let classification = result.classifications.first else { return }
        
        // Determine the time of this result.
        let formattedTime = String(format: "%.2f", result.timeRange.start.seconds)
        print("Analysis result for audio at time: \(formattedTime)")
        
        let confidence = classification.confidence * 100.0
        let percent = String(format: "%.2f%%", confidence)

        // Print the result as Instrument: percentage confidence.
        print("\(classification.identifier): \(percent) confidence.\n")
    }
    
    func request(_ request: SNRequest, didFailWithError error: Error) {
        print("The the analysis failed: \(error.localizedDescription)")
    }
    
    func requestDidComplete(_ request: SNRequest) {
        print("The request completed successfully!")
    }
}
```

다음으로 옵저버 객체 인스턴스와 함께 `MLModel`에 대한 [`SNClassifySoundRequest`](https://developer.apple.com/documentation/soundanalysis/snclassifysoundrequest)를 생성하고 분석기에 추가하라. 분석기는 옵저버를 리테인하지 않으므로, 그것을 계속 유지하기 위해 강한 참조를 유지하라.

```swift
// Create a new observer that will be notified of analysis results.
// Keep a strong reference to this object.
resultsObserver = ResultsObserver()

do {
    // Prepare a new request for the trained model.
    let request = try SNClassifySoundRequest(mlModel: model)
    try streamAnalyzer.add(request, withObserver: resultsObserver)
} catch {
    print("Unable to prepare request: \(error.localizedDescription)")
    return
}
```

#### Analyze Streaming Audio <a id="3261108"></a>

최상의 성능을 보장하려면 시리얼 디스패치 큐를 사용하여 오디오 분석을 수행하라. 이 작업을 별도의 큐에 배치하면 오디오 엔진이 분석 진행 중에 계속해서 새 버퍼를 효율적으로 처리할 수 있다.

```swift
// Serial dispatch queue used to analyze incoming audio buffers.
let analysisQueue = DispatchQueue(label: "com.apple.AnalysisQueue")
```

오디오 엔진의 입력 노드에 오디오 탭을 설치하면 마이크에서 캡처한 데이터 스트림에 접근할 수 있다. 오디오 엔진이 오디오 탭에 새 버퍼를 전달할 때 각 버퍼를 분석 큐에 전송하고 분석기에 현재 프레임 위치에서 분석하도록 요청하라.

```swift
// Install an audio tap on the audio engine's input node.
audioEngine.inputNode.installTap(onBus: inputBus,
                                 bufferSize: 8192, // 8k buffer
                                 format: inputFormat) { buffer, time in
    
    // Analyze the current audio buffer.
    self.analysisQueue.async {
        self.streamAnalyzer.analyze(buffer, atAudioFramePosition: time.sampleTime)
    }
}
```

분석기가 오디오 처리를 마치면 결과를 옵저버 객체로 보내는 데, 이 객체는 다음과 유사한 결과물을 낸다. 결과물은 마지막으로 처리된 오디오 버퍼에서 인식된 계측기와 해당 예측에 대한 모델의 신뢰 수준을 나타낸다.

```text
Analysis result for audio at time: 1.45
Acoustic Guitar: 92.39% confidence.

...

Analysis result for audio at time: 8.74
Acoustic Guitar: 94.45% confidence.

...

Analysis result for audio at time: 14.15
Tambourine: 85.39% confidence.

...

Analysis result for audio at time: 20.92
Snare Drum: 96.87% confidence.

```

#### Analyze Audio Files Offline <a id="3235291"></a>

또한 분석할 오디오 파일로 [`SNAudioFileAnalyzer`](https://developer.apple.com/documentation/soundanalysis/snaudiofileanalyzer) 클래스의 새로운 인스턴스를 만들어 오디오 파일 데이터의 오프라인 분석을 수행할 수 있다. PCM 형식의 오디오 데이터로만 작동하는 `SNAudioStreamAnalyzer`와 달리 `SNAudioFileAnalyzer`는 Core Audio가 지원하는 압축 또는 압축되지 않은 형식으로 작동한다.

```swift
let audioFileURL = // URL of audio file to analyze (m4a, wav, mp3, etc.)
            
// Create a new audio file analyzer.
audioFileAnalyzer = try SNAudioFileAnalyzer(url: audioFileURL)
```

스트리밍 분석을 수행할 때처럼 분석기와 옵저버에게 요청을 추가하면 분석기가 결과를 생성한다.

```swift
// Create a new observer that will be notified of analysis results.
resultsObserver = ResultsObserver()

// Prepare a new request for the trained model.
let request = try SNClassifySoundRequest(mlModel: model)
try audioFileAnalyzer.add(request, withObserver: resultsObserver)
```

[`analyze()`](https://developer.apple.com/documentation/soundanalysis/snaudiofileanalyzer/3182399-analyze) 또는 [`analyze(completionHandler:)`](https://developer.apple.com/documentation/soundanalysis/snaudiofileanalyzer/3197866-analyze) 메서드를 호출하여 오디오 데이터 분석을 수행하라.

```swift
// Analyze the audio data.
audioFileAnalyzer.analyze()
```

