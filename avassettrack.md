---
description: 에셋의 미디어 트랙에 대한 트랙 레벨 검사 인터페이스를 제공하는 객체.
---

# AVAssetTrack

### Declaration

```swift
class AVAssetTrack : NSObject
```

### Overview

AVAssetTrack은 [`AVAsynchronousKeyValueLoading`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading)프로토콜을 채택한다. 프로토콜의 [`statusOfValue(forKey:error:)`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading/1386816-statusofvalue)메서드를 사용하여 호출 쓰레드를 차단하지 않고 트랙 속성에 접근할 수 있는지 여부를 확인할 수 있다. 속성의 상태가 [`AVKeyValueStatus.loaded`](https://developer.apple.com/documentation/avfoundation/avkeyvaluestatus/loaded)이외의 값인 경우, 프로토콜의 [`loadValuesAsynchronously(forKeys:completionHandler:)`](https://developer.apple.com/documentation/avfoundation/avasynchronouskeyvalueloading/1387321-loadvaluesasynchronously)메서드를 사용하여 사용하기 전에 키를 비동기적으로 로드해야 한다. `AVAssetTrack`의 모든 키에 대해 보류 중인 로드 요청을 취소하려면 소유 [`AVAsset`](https://developer.apple.com/documentation/avfoundation/avasset)객체\(예: \[track.asset cancelLoading\]\)에 메시지해야 한다.

#### Retrieving Track Information

[`var asset: AVAsset?`](https://developer.apple.com/documentation/avfoundation/avassettrack/1385611-asset)

트랙이 속한 에셋.

[`var trackID: CMPersistentTrackID`](https://developer.apple.com/documentation/avfoundation/avassettrack/1385799-trackid)

이 트랙의 영구 고유 식별자.

[`var mediaType: AVMediaType`](https://developer.apple.com/documentation/avfoundation/avassettrack/1385741-mediatype)

트랙의 미디어 타입.

[`func hasMediaCharacteristic(AVMediaCharacteristic) -> Bool`](https://developer.apple.com/documentation/avfoundation/avassettrack/1385847-hasmediacharacteristic)

트랙이 지정된 미디어 특성을 가진 미디어를 참조하는지 여부를 나타내는 불 값을 반환한다.

[`struct AVMediaCharacteristic`](https://developer.apple.com/documentation/avfoundation/avmediacharacteristic)

미디어 종류 특성을 지정하기위한 옵션.

[`var formatDescriptions: [Any]`](https://developer.apple.com/documentation/avfoundation/avassettrack/1386694-formatdescriptions)

트랙이 참조하는 미디어 샘플의 형식 설명.

[`var isEnabled: Bool`](https://developer.apple.com/documentation/avfoundation/avassettrack/1387546-isenabled)

컨테이너에 저장된 상태에 따라 트랙을 사용할 수 있는지 여부를 나타내는 불 값.

[`var isPlayable: Bool`](https://developer.apple.com/documentation/avfoundation/avassettrack/1388276-isplayable)

현재 환경에서 트랙을 재생할 수 있는지 여부를 나타내는 불 값.

[`var isSelfContained: Bool`](https://developer.apple.com/documentation/avfoundation/avassettrack/1387643-isselfcontained)

트랙이 해당 저장소 컨테이너에만 포함 된 샘플 데이터를 참조하는지 여부를 나타내는 불 값.

[`var estimatedDataRate: Float`](https://developer.apple.com/documentation/avfoundation/avassettrack/1389758-estimateddatarate)

트랙이 참조하는 미디어 데이터의 예상 데이터 속도\(초당 비트 수\).

[`var totalSampleDataLength: Int64`](https://developer.apple.com/documentation/avfoundation/avassettrack/1388900-totalsampledatalength)

트랙에 필요한 샘플 데이터의 총 바이트 수.

[`var isDecodable: Bool`](https://developer.apple.com/documentation/avfoundation/avassettrack/2887366-isdecodable)

현재 환경에서 수신기를 디코딩할 수 있는지 여부를 나타내는 불 값.

#### Retrieving Temporal Properties

[`var timeRange: CMTimeRange`](https://developer.apple.com/documentation/avfoundation/avassettrack/1388335-timerange)

에셋의 전체 타임 라인 내 트랙의 시간 범위.

[`var naturalTimeScale: CMTimeScale`](https://developer.apple.com/documentation/avfoundation/avassettrack/1389233-naturaltimescale)

이 트랙이 참조한 미디어의 자연스러운 시간 스케일.

#### Retrieving Language Properties

[`var languageCode: String?`](https://developer.apple.com/documentation/avfoundation/avassettrack/1388627-languagecode)

[ISO 639-2/T](http://www.loc.gov/standards/iso639-2/php/code_list.php) 언어 코드로 트랙과 관련된 언어.

[`var extendedLanguageTag: String?`](https://developer.apple.com/documentation/avfoundation/avassettrack/1389105-extendedlanguagetag)

트랙과 관련된 언어 태그\([BCP-47](https://tools.ietf.org/html/bcp47) 언어 태그\)

#### Retrieving Visual Characteristics

[`var naturalSize: CGSize`](https://developer.apple.com/documentation/avfoundation/avassettrack/1387724-naturalsize)

트랙이 참조하는 미디어 데이터의 자연 치수.

[`var preferredTransform: CGAffineTransform`](https://developer.apple.com/documentation/avfoundation/avassettrack/1389837-preferredtransform)

표시 목적으로 비주얼 미디어 데이터의 선호하는 변환으로 트랙의 저장 컨테이너에 지정된 변환.

#### Retrieving Audible Characteristics

[`var preferredVolume: Float`](https://developer.apple.com/documentation/avfoundation/avassettrack/1388832-preferredvolume)

가청 미디어 데이터의 선호 볼륨으로 트랙의 저장 컨테이너에 지정된 볼륨.

#### Retrieving Frame-Based Characteristics

[`var nominalFrameRate: Float`](https://developer.apple.com/documentation/avfoundation/avassettrack/1386182-nominalframerate)

트랙의 프레임 속도\(초당 프레임 수\).

[`var minFrameDuration: CMTime`](https://developer.apple.com/documentation/avfoundation/avassettrack/1388608-minframeduration)

트랙 프레임의 최소 지속 시간.

[`var requiresFrameReordering: Bool`](https://developer.apple.com/documentation/avfoundation/avassettrack/1390844-requiresframereordering)

트랙의 샘플 프레젠테이션 및 디코딩 타임 스탬프에 대해 다른 값을 가질 수 있는지 여부를 나타내는 불 값.

#### Finding Track Segments

[`var segments: [AVAssetTrackSegment]`](https://developer.apple.com/documentation/avfoundation/avassettrack/1390665-segments)

트랙의 미디어 샘플에서 타임 라인으로 시간 매핑

[`func segment(forTrackTime: CMTime) -> AVAssetTrackSegment?`](https://developer.apple.com/documentation/avfoundation/avassettrack/1387186-segment)

지정된 트랙 시간에 해당하거나 가장 가까운 트랙 세그먼트를 반환한다.

[`func samplePresentationTime(forTrackTime: CMTime) -> CMTime`](https://developer.apple.com/documentation/avfoundation/avassettrack/1388248-samplepresentationtime)

적절한 시간 매핑을 통해 지정된 트랙 시간을 매핑하고 결과 샘플 프레젠테이 시간을 반환한다.

#### Managing Metadata

[`var metadata: [AVMetadataItem]`](https://developer.apple.com/documentation/avfoundation/avassettrack/1389054-metadata)

값을 사용할 수 있는 모든 메타데이터 식별자에 대한 메타 데이터 항목 배열.

[`var commonMetadata: [AVMetadataItem]`](https://developer.apple.com/documentation/avfoundation/avassettrack/1390832-commonmetadata)

값을 사용할 수 있는 각 공통 메타 데이터 키에 대한 메타 데이터 항목 객체의 배열.

[`var availableMetadataFormats: [AVMetadataFormat]`](https://developer.apple.com/documentation/avfoundation/avassettrack/1385751-availablemetadataformats)

트랙이 사용 가능한 메타 데이터 형식이 포함된 배열.

[`func metadata(forFormat: AVMetadataFormat) -> [AVMetadataItem]`](https://developer.apple.com/documentation/avfoundation/avassettrack/1387921-metadata)

지정된 형식의 컨테이너에 있는 각 메타 데이터 항목마다 하나씩 메타 데이터 객체의 배열을 만든다.

[`struct AVMetadataFormat`](https://developer.apple.com/documentation/avfoundation/avmetadataformat)

메타 데이터 형식을 정의하는 값.

#### Working with Associated Tracks

[`var availableTrackAssociationTypes: [AVAssetTrack.AssociationType]`](https://developer.apple.com/documentation/avfoundation/avassettrack/1388065-availabletrackassociationtypes)

트랙을 다른 트랙과 연결하는 데 사용되는 연결 유형의 배열.

[`func associatedTracks(ofType: AVAssetTrack.AssociationType) -> [AVAssetTrack]`](https://developer.apple.com/documentation/avfoundation/avassettrack/1389251-associatedtracks)

지정된 연결 유형을 사용하는 트랙과 관련된 다른 트랙을 포함하는 배열 생성.

[`struct AVAssetTrack.AssociationType`](https://developer.apple.com/documentation/avfoundation/avassettrack/associationtype)

다른 트랙이 트랙과 연결되는 방식을 식별하기 위한 상수

#### Creating Sample Cursors

[`var canProvideSampleCursors: Bool`](https://developer.apple.com/documentation/avfoundation/avassettrack/1386692-canprovidesamplecursors)

에셋 트랙이 미디어 샘플을 통과하고 정보를 검색하기 위해 샘플 커서 인스턴스를 제공할 수 있는지 여부를 나타내는 불 값.

[`func makeSampleCursor(presentationTimeStamp: CMTime) -> AVSampleCursor?`](https://developer.apple.com/documentation/avfoundation/avassettrack/1390248-makesamplecursor)

샘플 커서의 인스턴스를 만들어 지정된 프레젠테이션 타임 스탬프 또는 그 근처에 배치한다.

[`func makeSampleCursorAtFirstSampleInDecodeOrder() -> AVSampleCursor?`](https://developer.apple.com/documentation/avfoundation/avassettrack/1387226-makesamplecursoratfirstsampleind)

샘플 커서의 인스턴스를 작성하여 수신된 첫 번째 미디어 샘플에 디코딩 순서로 배치한다.[`func makeSampleCursorAtLastSampleInDecodeOrder() -> AVSampleCursor?`](https://developer.apple.com/documentation/avfoundation/avassettrack/1386014-makesamplecursoratlastsampleinde)

샘플 커서의 인스턴스를 생성하고 이를 수신기의 마지막 미디어 샘플에 디코딩 순서로 배치한다.

#### Instance Properties

[`var hasAudioSampleDependencies: Bool`](https://developer.apple.com/documentation/avfoundation/avassettrack/3131265-hasaudiosampledependencies)

