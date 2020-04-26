---
description: 균일한 미디어 유형의 미디어 샘플이 0개 이상인 객체이다.
---

# CMSampleBuffer

### 개요

`CMSampleBuffer`는 미디어 파이프라인을 통해 미디어 샘플 데이터를 이동하는 데 사용되는 특정 미디어 유형\(오디오, 비디오, 먹스 등\)의 0이상의 압축\(또는 압축되지 않은\) 샘플을 포함하는 Core Foundation 객체이다. `CMSampleBuffer`는 다음을 포함할 수 있다:

* 하나 이상의 미디어 샘플로 구성된 `CMBlockBuffer`, 또는
* `CVImageBuffer`, `CMSampleBuffer` 각 미디어 샘플에 대한 크기 및 타이밍 정보, 버퍼 레벨과 샘플 레벨 첨부 파일을 포함하여 스트림에 대한 포멧 설명에 대한 참조

샘플 버퍼는 샘플 레벨과 버퍼 레벨 첨부 파일을 모두 포함할 수 있다. 샘플 레벨 첨푸 파일은 버퍼의 각 개별 샘플\(프레임\)과 관련되며 타임스탬프 및 비디오 프레임 종속성 등의 정보를 포함한다. [`CMSampleBufferGetSampleAttachmentsArray(_:createIfNecessary:)`](https://developer.apple.com/documentation/coremedia/1489189-cmsamplebuffergetsampleattachmen) 함수를 사용하여 샘플 레벨 첨부 파일을 읽고 쓸 수 있다. 버퍼 레벨 첨부 파일은 재생 속도 및 버퍼 소모 시 수행되는 동작과 같은 버퍼 전체에 대한 정보를 제공한다. [`CMAttachment`](https://developer.apple.com/documentation/coremedia/cmattachment) 에서 설명한 API와 [Sample Buffer Attachment Keys](https://developer.apple.com/documentation/coremedia/cmsamplebuffer/sample_buffer_attachment_keys)에 나열된 키를 사용하여 버퍼 레벨 첨부 파일을 읽고 쓸 수 있다.

`CMSampleBuffer`는 아직 포함하지 않은 샘플을 설명할 수 있다. 예를 들어, 일부 미디어 서비스는 데이터를 읽기 전에 샘플 크기, 타이밍 및 포멧 정보에 접근할 수 있다. 이러한 서비스는 해당 정보를 가진 `CMSampleBuffers`를 생성하여 큐에 조기에 삽입하고, 데이터가 준비되면 나중에 미디어 데이터의 `CMBlockBuffers`를 연결\(또는 채우기\) 할 수 있다. 이를 위해 `CMSampleBuffers`는 "지금" 준비가 되도록 테스트, 설정, 강제할 수 있는 데이터-판독성 개념을 가지고 있다. 또한 `CMSampleBuffer`는 미디어 스트림 이벤트를 설명하는 특별한 버퍼 레벨 첨부 파일만 포함할 수 있다. \(예를 들어, "불연속: 다음 `CMSampleBuffer`를 처리하기 전에 디코더 배수 및 재설정"\). 이러한 특별한 첨부파일은 일반 `CMSampleBuffers` \(즉, 미디어 샘플 데이터를 포함\)에도 첨부할 수 있으며, 그렇게 되면 `CMSampleBuffer`의 샘플 이후에 발생하도록 정의된다.



