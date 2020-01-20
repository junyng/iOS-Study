---
description: 미디어 샘플을 파일 시스템으로 가져오고 내보낸다.
---

# Asset File Import and Export

### Topics

#### File Import

[`class AVAssetReader`](https://developer.apple.com/documentation/avfoundation/avassetreader)

파일 기반 또는 여러 소스로부터 미디어 데이터의 집합으로 구성된 에셋의 미디어 데이터를 얻기 위해 사용되는 작성기 객체.

[`class AVAssetReaderAudioMixOutput`](https://developer.apple.com/documentation/avfoundation/avassetreaderaudiomixoutput)

하나 이상의 트랙으로부터 오디오를 혼합함으로써 발생하는 오디오 샘플을 판독하기 위한 인터페이스를 정의하는 객체.

[`class AVAssetReaderTrackOutput`](https://developer.apple.com/documentation/avfoundation/avassetreadertrackoutput)

에셋 작성기 에셋의 단일 트랙에서 미디어 데이터를 읽기 위한 인터페이스를 정의하는 객체.

[`class AVAssetReaderSampleReferenceOutput`](https://developer.apple.com/documentation/avfoundation/avassetreadersamplereferenceoutput)

단일 에셋 트랙에서 샘플 참조를 읽기 위한 인터페이스를 정의하는 객체.

[`class AVAssetReaderVideoCompositionOutput`](https://developer.apple.com/documentation/avfoundation/avassetreadervideocompositionoutput)

작성기 에셋의 하나 이상의 트랙으로 부터 프레임에서 합성된 비디오 프레임을 읽는 객체.

[`class AVAssetReaderOutput`](https://developer.apple.com/documentation/avfoundation/avassetreaderoutput)

에셋 작성기 객체에서 공통 미디어 유형의 단일 샘플을 읽기 위한 인터페이스를 정의하는 추상 클래스.

[`class AVAssetReaderOutputMetadataAdaptor`](https://developer.apple.com/documentation/avfoundation/avassetreaderoutputmetadataadaptor)

메타데이터를 읽기 위한 인터페이스를 정의하는 객체.

[`class AVAssetImageGenerator`](https://developer.apple.com/documentation/avfoundation/avassetimagegenerator)

재생과 독립적으로 에셋의 미리 보기 또는 축소 이미지를 제공하는 객체.

#### File Export

[Exporting Video to Alternative Formats](https://developer.apple.com/documentation/avfoundation/media_assets_playback_and_editing/asset_file_import_and_export/exporting_video_to_alternative_formats)

기존 비디오 파일을 다른 형식으로 변환한다.

[`class AVAssetExportSession`](https://developer.apple.com/documentation/avfoundation/avassetexportsession)

에셋 소스 객체의 내용을 트랜스코딩하여 지정된 내보내기 사전 설정에 의해 설명된 양식의 출력을 생성하는 객체.

[`class AVAssetWriter`](https://developer.apple.com/documentation/avfoundation/avassetwriter)

지정된 시청각 컨테이너 유형의 새 파일에 미디어 데이터를 쓰는 데 사용되는 객체.

[`class AVAssetWriterInput`](https://developer.apple.com/documentation/avfoundation/avassetwriterinput)

에셋 작성기의 출력 파일의 단일 트랙에 미디어 샘플을 추가하는 데 사용되는 작성기

[`let AVVideoTransferFunction_ITU_R_2100_HLG: String`](https://developer.apple.com/documentation/avfoundation/avvideotransferfunction_itu_r_2100_hlg)

ITU\_R BT.2100 색상 공간에 대한 전송 함수.

[`class AVOutputSettingsAssistant`](https://developer.apple.com/documentation/avfoundation/avoutputsettingsassistant)

출력 설정 딕셔너리를 사용하는 객체를 구성하기 위한 매개 변수 집합을 지정하는 객체.

[`class AVAssetWriterInputGroup`](https://developer.apple.com/documentation/avfoundation/avassetwriterinputgroup)

서로 배타적인 관계에 있는 트랙의 그룹.

[`class AVAssetWriterInputMetadataAdaptor`](https://developer.apple.com/documentation/avfoundation/avassetwriterinputmetadataadaptor)

시간적 메타데이터 그룹으로 패키지된 메타데이터를 단일 에셋 작성기 입력으로 쓰기 위한 인터페이스를 정의하는 객체.

[`class AVAssetWriterInputPassDescription`](https://developer.apple.com/documentation/avfoundation/avassetwriterinputpassdescription)

현재 패스의 요구 사항에 대해 쿼리하기 위한 인터페이스를 정의하는 객체.

[`class AVAssetWriterInputPixelBufferAdaptor`](https://developer.apple.com/documentation/avfoundation/avassetwriterinputpixelbufferadaptor)

픽셀 버퍼로 패키지된 비디오 샘플을 단일 에셋 작성기 입력에 추가하는 데 사용되는 버퍼.

### See Also

#### Media Assets

[About the Asset Model](https://developer.apple.com/documentation/avfoundation/media_assets_playback_and_editing/about_the_asset_model)

미디어 플레이어의 구성 요소로 에셋이 사용되는 방법을 이해한다.

[`class AVAsset`](https://developer.apple.com/documentation/avfoundation/avasset)

비디오와 사운드와 같은 시간제한 시청각 미디어를 모델링하는 데 사용되는 추상 클래스.

[`class AVAssetTrack`](https://developer.apple.com/documentation/avfoundation/avassettrack)

에셋의 미디어 트랙에 대한 트랙 수준 검사 인터페이스를 제공하는 객체.

[Asset Manipulation](https://developer.apple.com/documentation/avfoundation/media_assets_playback_and_editing/asset_manipulation)

미디어 샘플을 파일 시스템으로 가져오고 내보낸다.

