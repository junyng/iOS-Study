---
description: 프레임워크의 메타데이터 식별자 서비스 및 메타데이터 데이터 타입 레지스트리로 작업하기 위한 API.
---

# CMMetadata

### Overview

Core Media 프레임워크는 두 가지 서비스를 제공한다: 메타 데이터 식별자 서비스 및 메타데이터 데이터 타입 레지스트리.

메타데이터 식별자 서비스는 메타데이터 식별 튜플\(4 바이트 키 네임스페이스 및 n-바이트 키 값\)을 [`CFString`](https://developer.apple.com/documentation/corefoundation/cfstring)으로 인코딩하고 다시 인코딩하는 수단을 제공한다.

메타데이터 데이터 타입 레지스트리는 기본 데이터 타입과 \(선택적으로\) 기타 등록 데이터 타입에 부합하는 메타데이터 데이터 타입을 등록하는 프로세스를 허용한다. 레지스트리는 타의 추종을 불허하는 메타데이터 값에 대한 형식 설명 작성 프로세스를 간소화하고, 클라이언트가 메타데이터를 해석할 수 있는 방법을 표시할 수 있도록 한다.

### Topics

#### Metadata Identifier Services

#### Metadata Data Type Registry

#### Constants

#### Result Codes

