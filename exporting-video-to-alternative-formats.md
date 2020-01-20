---
description: 기존 비디오 파일을 다른 형식으로 변환한다.
---

# Exporting Video to Alternative Formats

### Overview

기존 동영상 파일을 다른 장치와 호환되는 형식으로 변환하려면 기존 파일의 내용을 기반으로 새 동영상 파일을 생성해야 한다. 저장된 비디오의 형식을 변경할 수 없으며 원하는 형식의 두 번째 파일을 생성해야 한다.

이 기사는 이미 비디오 파일을 다른 형식으로 저장했다고 가정한다. 동영상 캡처에서 동영상을 직접 저장할 경우 다음 [Capturing Video in Alternative Formats](https://developer.apple.com/documentation/avfoundation/cameras_and_media_capture/capturing_video_in_alternative_formats) 단계를 통해 캡처 중에 기본 형식을 변경하는 것이 더 효과적이다.

#### Export the New Video into the Desired Format <a id="3021727"></a>

에셋을 원하는 파일 유형으로 내보내어 동영상 파일을 변환한다. AVFoundation이 제공하는 [`AVFileType`](https://developer.apple.com/documentation/avfoundation/avfiletype)사전 설정 목록에서 최종 비디오에 사용할 유형을 선택하라. 이 유형을 사용하여 기존 유형에서 내보내기 프로세스를 관리하는 [`AVAssetExportSession`](https://developer.apple.com/documentation/avfoundation/avassetexportsession)객체를 구성한다.

예를 들어 동영상 파일을 `H.264`로 변환하려면 [`AVAssetExportPresetHighestQuality`](https://developer.apple.com/documentation/avfoundation/avassetexportpresethighestquality)와 같이 `H.264/MPEG-4 AVC`로 인코딩되는 기존 사전 설정을 사용하고 출력 파일 형식을 [`mov`](https://developer.apple.com/documentation/avfoundation/avfiletype/1386770-mov)로 설정하라.

```swift
import AVFoundation

let anAsset = // Your source AVAsset movie in HEVC format //
let outputURL = // URL of your exported output // 
 
// These settings will encode using H.264. 
let preset = AVAssetExportPresetHighestQuality 
let outFileType = AVFileTypeQuickTimeMovie
```

다음으로 비디오 에셋을 변환할 수 있는지 확인하라. 특정 에셋은 사전 설정 조건 하에서 전환되지 않을 수 있으므로 두 형식 간의 호환성을 보장하기 위해 검사를 수행하라.

```swift
AVAssetExportSession.determineCompatibility(ofExportPreset: preset, with: anAsset, outputFileType: outFileType, completionHandler: { (isCompatible) in
    if !isCompatible {
        return
}}) 
```

호환성을 결정한 후 A에 구성한 사전 설정을 사용하여 변환을 수행하라고 알려 준다. 대형 비디오 파일을 만드는 것은 시간이 많이 걸리는 작업이므로 [`exportAsynchronously(completionHandler:)`](https://developer.apple.com/documentation/avfoundation/avassetexportsession/1388005-exportasynchronously)를 사용하여 비디오를 비동기적으로 내보내라. 이 메서드는 세션이 새 동영상 파일 작성을 완료한 후 결과를 처리할 수 있는 완료 블록을 제공한다.

```swift
guard let export = AVAssetExportSession(asset: anAsset, presetName: preset) else { 
    return 
} 

export.outputFileType = outFileType 
export.outputURL = outputURL 
export.exportAsynchronously { () -> Void in
   // Handle export results. 
}
```

### See Also

#### File Export

[`class AVAssetExportSession`](https://developer.apple.com/documentation/avfoundation/avassetexportsession)

에셋 소스 객체의 내용을 트랜스코딩하여 지정된 내보내기 사전 설정에 의해 설명된 양식의 출력을 생성하는 객체.

[`class AVAssetWriter`](https://developer.apple.com/documentation/avfoundation/avassetwriter)

지정된 시청각 컨테이너 유형의 새 파일에 미디어 데이터를 쓰는 데 사용되는 객체.

[`class AVAssetWriterInput`](https://developer.apple.com/documentation/avfoundation/avassetwriterinput)

에셋 작성기의 출력 파일의 단일 트랙에 미디어 샘플을 추가하는 데 사용되는 작성기.

[`let AVVideoTransferFunction_ITU_R_2100_HLG: String`](https://developer.apple.com/documentation/avfoundation/avvideotransferfunction_itu_r_2100_hlg)

ITU\_R BT.2100 색상 공간에 대한 전송 함수.

[`class AVOutputSettingsAssistant`](https://developer.apple.com/documentation/avfoundation/avoutputsettingsassistant)

출력 설정 딕셔너리를 사용하는 객체를 구성하기 위한 매개 변수 집합을 지정하는 객체.

[`class AVAssetWriterInputGroup`](https://developer.apple.com/documentation/avfoundation/avassetwriterinputgroup)

서로 배타적인 관계에 있는 트랙의 그룹.

[`class AVAssetWriterInputMetadataAdaptor`](https://developer.apple.com/documentation/avfoundation/avassetwriterinputmetadataadaptor)

시간적 메타데이터 그룹으로 패키지된 메타데이터를 단일 에셋 작성기 입력으로 쓰기 위한 인터페이스를 정의하는 객체.

[`class AVAssetWriterInputPassDescription`](https://developer.apple.com/documentation/avfoundation/avassetwriterinputpassdescription)

현재 패스의 요구 사항을 쿼리하기 위한 인터페이스를 정의하는 객체.

[`class AVAssetWriterInputPixelBufferAdaptor`](https://developer.apple.com/documentation/avfoundation/avassetwriterinputpixelbufferadaptor)

픽셀 버퍼로 패키지된 비디오 샘플을 단일 에셋 작성기 입력에 추가하는 데 사용되는 버퍼.

