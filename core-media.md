---
description: 필수 데이터 유형으로 시간 기반 시청각 에셋을 표현한다.
---

# Core Media

### Overview

Core Media 프레임워크는 AVFoundation에서 사용하는 미디어 파이프라인과 애플 플랫폼에서 발견되는 기타 고수준의 프레임워크를 정의한다. Core Media의 저수준의 데이터 유형과 인터페이스를 사용하여 미디어 샘플을 효율적으로 처리하고 미디어 데이터 큐를 관리하라.

### Topics

#### Sample Processing

[CMSampleBuffer](https://developer.apple.com/documentation/coremedia/cmsamplebuffer-u71)

동일한 미디어 유형의 0개 이상의 미디어 샘플을 포함하는 객체.

[CMBlockBuffer](https://developer.apple.com/documentation/coremedia/cmblockbuffer-u9i)

처리 시스템을 통해 메모리 블록을 이동하는데 사용되는 객체.

[CMFormatDescription](https://developer.apple.com/documentation/coremedia/cmformatdescription-u8g)

샘플 버퍼의 샘플을 설명하는 미디어 형식 설명자.

[CMAttachment](https://developer.apple.com/documentation/coremedia/cmattachment)

샘플 버퍼에 추가 메타데이터를 연결하는 API.

#### Time Representation

[CMTime](https://developer.apple.com/documentation/coremedia/cmtime-u58)

타임스탬프 또는 지속시간과 같은 시간 값을 나타내는 구조체.

[CMTimeRange](https://developer.apple.com/documentation/coremedia/cmtimerange-qts)

시간 범위를 나타내는 구조체.

[CMTimeMapping](https://developer.apple.com/documentation/coremedia/cmtimemapping-b3)

한 시간의 세그먼트를 다른 시간에 매핑하는 것을 지정하는 데 사용되는 구조체.

#### Media Synchronization

[CMClock](https://developer.apple.com/documentation/coremedia/cmclock-u5q)

애플리케이션과 장치를 동기화하는 데 사용되는 기준 클럭.

[CMAudioClock](https://developer.apple.com/documentation/coremedia/cmaudioclock)

오디오 소스와 동기화하는 데 사용되는 특수 기준 시계.

[CMTimebase](https://developer.apple.com/documentation/coremedia/cmtimebase)

애플리케이션 제어하에 있는 타임라인 모델.

#### Text Markup

[CMTextMarkup](https://developer.apple.com/documentation/coremedia/cmtextmarkup)

Core Media에서 지원하는 텍스트 마크업 관련 속성 모음.

#### Metadata

[CMMetadata](https://developer.apple.com/documentation/coremedia/cmmetadata)

프레임워크의 메타데이터 식별자 서비스 및 메타데이터 데이터 유형 레지스트리 작업을 위한 API.

#### Queues

[CMSimpleQueue](https://developer.apple.com/documentation/coremedia/cmsimplequeue)

단순하고 잠금 없는 `(void *)` 요소의 FIFO 큐

[CMBufferQueue](https://developer.apple.com/documentation/coremedia/cmbufferqueue)

시간 지정 버퍼의 큐.

[CMMemoryPool](https://developer.apple.com/documentation/coremedia/cmmemorypool-u89)

대용량 메모리 블록을 반복적으로 할당하고, 할당 취소한 다음 할당해야 할 때 메모리 할당을 최적화하는 데 사용되는 풀.

#### Reference

[Core Media Constants](https://developer.apple.com/documentation/coremedia/core_media_constants)

[Core Media Structures](https://developer.apple.com/documentation/coremedia/core_media_structures)

[Core Media Functions](https://developer.apple.com/documentation/coremedia/core_media_functions)

[Core Media Data Types](https://developer.apple.com/documentation/coremedia/core_media_data_types)

#### Protocols

[`protocol CMAttachmentBearerProtocol`](https://developer.apple.com/documentation/coremedia/cmattachmentbearerprotocol)

[`protocol CMBlockBufferProtocol`](https://developer.apple.com/documentation/coremedia/cmblockbufferprotocol)

[`protocol CMSyncProtocol`](https://developer.apple.com/documentation/coremedia/cmsyncprotocol)



