---
description: 파일 기반 또는 여러 소스에서 미디어 데이터의 집합으로 구성된 에셋의 미디어 데이터를 가져오는 데 사용되는 reader 객체
---

# AVAssetReader

### Declaration

```swift
class AVAssetReader : NSObject
```

### Overview

`AVAssetReader`를 통해 얻을 수 있는 이점:

* 저장소에서 디코딩되지 않은 원시 미디어 샘플을 직접 읽고, 렌더링 가능한 형태로 디코딩된 샘플을 얻는다.
* 에셋의 여러 오디오 트랙을 구성하고 [`AVAssetReaderAudioMixOutput`](https://developer.apple.com/documentation/avfoundation/avassetreaderaudiomixoutput)와 [`AVAssetReaderVideoCompositionOutput`](https://developer.apple.com/documentation/avfoundation/avassetreadervideocompositionoutput)를 사용하여 비디오 트랙을 구성하라.

`AVAssetReader` 파이프라인은 내부적으로 멀티쓰레드된다. [`init(asset:)`](https://developer.apple.com/documentation/avfoundation/avassetreader/1385593-init)으로 읽기를 시작한 후, reader는 사용 전에 합리적인 양의 샘플 데이터를 로드하고 처리하여 [`copyNextSampleBuffer()`](https://developer.apple.com/documentation/avfoundation/avassetreaderoutput/1385732-copynextsamplebuffer)\(`AVAssetReaderOutput`\)과 같은 검색 작업이 매우 짧은 지연 시간을 가질 수 있다. `AVAssetReader`는 실시간 소스와 함께 사용하도록 되어 있지 않으며, 실시간 작동에 대해서는 성능이 보장되지 않는다.

### Topics

#### Creating a Reader

[`init(asset: AVAsset)`](https://developer.apple.com/documentation/avfoundation/avassetreader/1385593-init)

지정된 에셋에서 미디어 데이터를 읽기 위한 에셋 Reader 생성

#### Managing Outputs

[`var outputs: [AVAssetReaderOutput]`](https://developer.apple.com/documentation/avfoundation/avassetreader/1387132-outputs)

Reader의 클라이언트가 미디어 데이터를 읽을 수 있는 출력.

[`func add(AVAssetReaderOutput)`](https://developer.apple.com/documentation/avfoundation/avassetreader/1390110-add)

수신기에 출력을 추가한다.

[`func canAdd(AVAssetReaderOutput) -> Bool`](https://developer.apple.com/documentation/avfoundation/avassetreader/1387485-canadd)

수신기에 출력을 추가할지 여부를 나타내는 불 값을 반환한다.

#### Controlling Reading

[`var status: AVAssetReader.Status`](https://developer.apple.com/documentation/avfoundation/avassetreader/1390211-status)

에셋에서 샘플 버퍼를 읽는 상태.

[`enum AVAssetReader.Status`](https://developer.apple.com/documentation/avfoundation/avassetreader/status)

Reader의 상태.

[`func startReading() -> Bool`](https://developer.apple.com/documentation/avfoundation/avassetreader/1390286-startreading)

에셋에서 샘플 버퍼를 얻을 수 있도록 수신자를 준비한다.

[`func cancelReading()`](https://developer.apple.com/documentation/avfoundation/avassetreader/1390258-cancelreading)

백그라운드 작업을 취소하고 수신자의 출력이 더 많은 샘플을 읽지 못하게 한다.

[`var error: Error?`](https://developer.apple.com/documentation/avfoundation/avassetreader/1388114-error)

발생한 오류.

[`var timeRange: CMTimeRange`](https://developer.apple.com/documentation/avfoundation/avassetreader/1386400-timerange)

에셋에서 읽을 시간 범위.

#### Getting the Asset

[`var asset: AVAsset`](https://developer.apple.com/documentation/avfoundation/avassetreader/1389128-asset)

수신자를 초기화한 에셋.

