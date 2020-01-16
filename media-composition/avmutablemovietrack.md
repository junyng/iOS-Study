---
description: QuickTime 또는 ISO 기반 미디어 파일 형식에 부합하는 변경가능한 트랙.
---

# AVMutableMovieTrack

### Declaration

```swift
class AVMutableMovieTrack : AVMovieTrack
```

### Topics

#### Associating Tracks

[`func addTrackAssociation(to: AVMovieTrack, type: AVAssetTrack.AssociationType)`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1390163-addtrackassociation)

두 트랙 사이의 특정 유형의 트랙 연결을 생성한다.

[`func removeTrackAssociation(to: AVMovieTrack, type: AVAssetTrack.AssociationType)`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1389620-removetrackassociation)

트랙 사이의 특정 유형의 트랙 연결을 제거한다. 

#### Accessing General Track Information

[`var alternateGroupID: Int`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1387206-alternategroupid)

트랙을 특정 대체 그룹의 구성원으로 식별하는 숫자이다.

[`var isEnabled: Bool`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1386340-isenabled)

트랙이 프레젠테이션에 기본적으로 활성화되어 있는지 여부를 나타내는 불 값.

[`var hasProtectedContent: Bool`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1389542-hasprotectedcontent)

트랙에 보호된 내용이 포함되어 있는지 여부를 나타내는 불 값.

[`var mediaDataStorage: AVMediaDataStorage?`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1386532-mediadatastorage)

미디어 데이터를 트랙에 추가할 수 있는 저장 컨테이너.

[`var isModified: Bool`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1390201-ismodified)

트랙이 수정되었는지 여부를 나타내는 불 값.

[`var sampleReferenceBaseURL: URL?`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1385583-samplereferencebaseurl)

샘플 참조의 기본 URL.

[`var timescale: CMTimeScale`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1388055-timescale)

`moov` 원자를 포함하는 트랙의 시간 스케일.

[`var metadata: [AVMetadataItem]`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1390023-metadata)

트랙이 저장한 메타 데이터 배열.

#### Accessing Visual Information

[`var naturalSize: CGSize`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1385900-naturalsize)

트랙의 시각 미디어 데이터를 표시하는 데 사용되는 치수.

[`var preferredTransform: CGAffineTransform`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1386593-preferredtransform)

표시 목적으로 트랙의 시각적 미디어 데이터에서 수행되는 변환.

[`var layer: Int`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1387655-layer)

트랙의 비주얼 미디어에 대한 레이어 수준.

[`var cleanApertureDimensions: CGSize`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1386454-cleanaperturedimensions)

트랙의 깨끗한 조리개 치수.

[`var productionApertureDimensions: CGSize`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1390108-productionaperturedimensions)

트랙의 생산 조리개 치수.

[`var encodedPixelsDimensions: CGSize`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1389417-encodedpixelsdimensions)

트랙의 인코딩된 픽셀 크기.

#### Accessing Audio and Language Information

[`var preferredVolume: Float`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1390391-preferredvolume)

트랙의 가청 메타 데이터에 대한 선호 음량.

[`var languageCode: String?`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1389736-languagecode)

트랙과 관련된 언어를 나타내는 ISO 639-2 / T 언어 코드.

[`var extendedLanguageTag: String?`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1389056-extendedlanguagetag)

트랙과 관련된 언어 태그를 식별하는 IETF BCP 47 언어 식별자.

#### Modifying Time Ranges

[`func insertTimeRange(CMTimeRange, of: AVAssetTrack, at: CMTime, copySampleData: Bool)`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1387665-inserttimerange)

대상 동영상에 에셋 트랙의 일부를 삽입한다.

[`func insertEmptyTimeRange(CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1389441-insertemptytimerange)

트랙에 빈 시간 범위를 추가한다.

[`func removeTimeRange(CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1385962-removetimerange)

트랙에서 지정된 시간 범위를 제거한다.

[`func scaleTimeRange(CMTimeRange, toDuration: CMTime)`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1388618-scaletimerange)

트랙에서 시간 범위의 지속 시간을 변경한다.

#### Accessing Media Chunks

동영상 파일의 샘플 데이터는 일련의 "chunks"에 저장된다. 청크에는 하나 이상의 샘플이 포함되어 있으며, 서로 크기가 다를 수 있다. 샘플을 청크로 수집하는 것은 동영상 재생과 같은 작업 중에 샘플 데이터에 최적화된 접근을 허용하기 위한 것이다. 트랙의 샘플 데이터를 미디어 저장소 컨테이너에 쓰기 전에 동영상 트랙의 청크 속성을 조정하여 동영상 파일의 기본 청크 크기를 변경할 수 있다.

[`var preferredMediaChunkAlignment: Int`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1390504-preferredmediachunkalignment)

미디어 청크 정렬을 지원하는 파일 형식의 미디어 청크 정렬 경계.

[`var preferredMediaChunkDuration: CMTime`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1390292-preferredmediachunkduration)

미디어 청크 기간을 지원하는 파일 형식의 파일에 기록된 샘플 데이터의 각 청크에 사용할 최대 기간. 

[`var preferredMediaChunkSize: Int`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1390149-preferredmediachunksize)

미디어 청크 기간을 지원하는 파일 형식의 파일에 기록된 각 샘플 데이터 청크에 사용할 최대 크기.

#### Appending Sample Data

[`func append(CMSampleBuffer, decodeTime: UnsafeMutablePointer<CMTime>?, presentationTime: UnsafeMutablePointer<CMTime>?)`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1638041-append)

샘플 데이터를 미디어 파일에 추가하고 추가된 데이터에 대한 샘플 참조를 트랙의 미디어 샘플 테이블에 추가한다.

[`func insertMediaTimeRange(CMTimeRange, into: CMTimeRange) -> Bool`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/1638038-insertmediatimerange)

미디어 시간 범위에 대한 참조를 트랙에 삽입한다.

#### Changing Format Descriptions

[`func replaceFormatDescription(CMFormatDescription, with: CMFormatDescription)`](https://developer.apple.com/documentation/avfoundation/avmutablemovietrack/2876160-replaceformatdescription)

수신기의 형식 설명을 새 형식 설명으로 대체한다.  


