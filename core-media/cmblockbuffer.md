---
description: 처리 시스템을 통해 메모리 블록을 이동하는 데 사용되는 객체.
---

# CMBlockBuffer

### Overview

`CMBlockBuffer`는 비연속 메모리 영역에 걸쳐 연속적인 범위의 데이터 오프셋 \(0부터 [`CMBlockBufferGetDataLength(_:)`](https://developer.apple.com/documentation/coremedia/1489292-cmblockbuffergetdatalength)\)을 나타내는 `CTFype` 객체이다. 메모리 영역은 메모리 블록과 버퍼 참조로 구성된다. 버퍼 참조는 다시 추가 영역을 참조할 수 있다. `CMBlockBuffer`는 `CMAttachment` 프로토콜을 사용하여 첨부 파일을 전파한다.

### Topics

#### Creating Block Buffers

#### Modifying Block Buffers

#### Inspecting Block Buffers

#### Data Types

#### Constants

