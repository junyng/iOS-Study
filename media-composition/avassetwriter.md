---
description: 지정된 시청각 컨테이너 유형의 새 파일에 미디어 데이터를 쓰는 데 사용되는 객체.
---

# AVAssetWriter

### Declaration

```swift
class AVAssetWriter : NSObject
```

### Overview

[`AVAssetReader`](https://developer.apple.com/documentation/avfoundation/avassetreader) 인스턴스 또는 AVFoundation API 세트 외부에서 하나 이상의 에셋에 대한 미디어 데이터를 가져올 수 있다. [`CMSampleBuffer`](https://developer.apple.com/documentation/coremedia/cmsamplebuffer-u71)형식으로 쓸 수 있도록 `AVAssetWriter`에 미디어 데이터를 보낸다. 에셋 작성기 입력에 추가된 샘플 데이터 시퀀스는 "샘플 작성 세션"에 포함된다. 이 세션 중 하나를 시작하려면 [`startSession(atSourceTime:)`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1389908-startsession) 를 호출해야 한다.

`AVAssetWriter`를 사용하면 쓰기 중에 선택적으로 미디어 샘플을 다시 인코딩할 수 있다. 선택적으로 출력 파일에 메타데이터 컬렉션을 쓸 수도 있다. `AVAssetWriter`는 여러 동시 트랙에 대한 미디어 데이터의 인터리빙을 자동으로 지원한다.

특정 `AVAssetWriter` 인스턴스를 한 번만 사용하여 단일 파일에 쓸 수 있다. 파일에 쓸 때마다 `AVAssetWriter`의 새 인스턴스를 사용해야 한다.



### Topics

#### Creating an Asset Writer

[`init(url: URL, fileType: AVFileType)`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1426663-init)

지정된 UTI가 지정한 형식으로 지정된 URL에서 식별된 파일에 쓰기 위한 에셋 작성기를 반환한다.

[`init(outputURL: URL, fileType: AVFileType)`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1389201-init)

지정된 UTI에서 지정한 형식으로 지정된 URL로 식별된 파일에 쓰기 위한 에셋 작성기를 생성한다.

[`var availableMediaTypes: [AVMediaType]`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1388730-availablemediatypes)

입력을 추가할 수 있는 미디어 유형.

#### Writing Data

[`func startWriting() -> Bool`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1386724-startwriting)

작성기에 결과물을 쓰기 시작하라고 말한다.

[`func finishWriting() -> Bool`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1426644-finishwriting)

출력 파일의 쓰기를 완료한다. Deprecated

[`func finishWriting(completionHandler: () -> Void)`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1390432-finishwriting)

모든 미완료 입력을 완료된 것으로 표시하고 출력 파일의 쓰기를 완료한다.

[`func cancelWriting()`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1387234-cancelwriting)

작성기에 글쓰기를 취소하라고 말한다.

[`var outputURL: URL`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1387731-outputurl)

직접 출력하는 URL.

[`var outputFileType: AVFileType`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1387349-outputfiletype)

작성기 출력의 파일 형식.

[`var error: Error?`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1390725-error)

실패를 초래한 오류에 대한 설명.

[`var status: AVAssetWriter.Status`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1389335-status)

수신기의 출력 파일에 샘플을 쓰는 상태.

[`enum AVAssetWriter.Status`](https://developer.apple.com/documentation/avfoundation/avassetwriter/status)

작성기가 출력 파일에 샘플을 성공적으로 쓸 수 있는지 여부를 나타내는 상태.

[`var directoryForTemporaryFiles: URL?`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1387445-directoryfortemporaryfiles)

내보내기 프로세스 중에 생성된 임시 파일을 포함하기 위한 적절한 디렉터리.

#### Managing Inputs

[`var inputs: [AVAssetWriterInput]`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1388264-inputs)

에셋 작성기와 연관된 에셋 작성기 입력.

[`func add(AVAssetWriterInput)`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1390389-add)

수신기에 입력 추가.

[`func canAdd(AVAssetWriterInput) -> Bool`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1387863-canadd)

수신기가 지정된 입력을 추가할 수 있는지 여부를 나타내는 불 값을 반환한다.

#### Managing Session Time

[`func startSession(atSourceTime: CMTime)`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1389908-startsession)

출력 에셋에 대한 샘플 쓰기 세션을 시작한다.

[`func endSession(atSourceTime: CMTime)`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1389921-endsession)

샘플 에셋 세션을 완료한다.

#### Configuring Output

[`func canApply(outputSettings: [String : Any]?, forMediaType: AVMediaType) -> Bool`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1388842-canapply)

지정된 출력 설정이 지정된 미디어 유형을 지원하는지 여부를 나타내는 불 값을 반환한다.

[`var metadata: [AVMetadataItem]`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1387974-metadata)

수신기의 출력 파일에 쓰여진 메타데이터의 모음.

[`var movieFragmentInterval: CMTime`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1387469-moviefragmentinterval)

동영상 조각을 쓰는 빈도.

[`var overallDurationHint: CMTime`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1388408-overalldurationhint)

동영상 파편을 지원하는 파일 형식에 대해 작성된 파일의 최종 기간의 힌트.

[`var movieTimeScale: CMTimeScale`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1386762-movietimescale)

사용할 에셋 수준 시간 척도.

[`var shouldOptimizeForNetworkUse: Bool`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1389811-shouldoptimizefornetworkuse)

네트워크를 통해 재생하는 데 더 적합한 출력 파일을 쓸지 여부를 나타내는 불 값.

#### Managing Asset Writer Input Groups

[`func add(AVAssetWriterInputGroup)`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1385643-add)

에셋 작성기에 에셋 작성기 입력 그룹 인스턴스를 추가한다.

[`func canAdd(AVAssetWriterInputGroup) -> Bool`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1386698-canadd)

수신기가 입력 그룹을 추가할 수 있는지 여부를 반환한다.

[`var inputGroups: [AVAssetWriterInputGroup]`](https://developer.apple.com/documentation/avfoundation/avassetwriter/1388432-inputgroups)

에셋 작성기에 추가된 에셋 작성기 입력 그룹을 포함하는 배열.

