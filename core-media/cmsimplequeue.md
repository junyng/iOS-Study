---
description: (void *) 요소의 단순하고 락이 없는 FIFO 큐.
---

# CMSimpleQueue

### Overview

`CMSimpleQueues`는 \(void \*\) 요소의 단순한 락이 없는 FIFO 큐를 구현하는 Core Foundation 기반 객체다. 요소는 포인터로 가정되지 않는다; 단순한 포인터 크기의 숫자 값일 수 있다. \(NULL 또는 0 값 요소는 허용되지 않는다\). 요소가 실제로 할당된 메모리 버퍼에 대한 포인터인 경우 버퍼 수명 관리를 외부에서 처리해야 한다.

`CMSimpleQueue`는 하나의 인큐잉 쓰레드와 하나의 디큐잉 쓰레드를 안전하게 처리할 수 있다. `CMSimpleQueue`는 락이 없다. 이와 같이, 락킹/블로킹이 금지된 `CoreAudio` `ioProc` 쓰레드에서 인큐 및/또는 디큐가 발생할 수 있다.

`CMSimpleQueue`의 현재 상태를 조회할 수 있다. 클라이언트는 큐\(`CMSimpleQueueGetCount`\)의 현재 요소 수와 큐\(`CMSimpleQueueGetCapacity`\)의 최대 용량을 얻을 수 있다. 또한 이 두 API를 사용하여 0.0에서 1.0 사이의 Float32를 계산하는 편의 매크로 \(CMSimpleQueueGetFullness\)가 있어 큐의 전체성을 나타낸다. CMSimpleQueues를 재설정할 수 있다. 이렇게 하면 큐에 요소가 없는 상태로 새로 생성된 상태를 반환한다 \(그러나 최대 용량은 변경되지 않음\).

### Topics

#### Creating Queues

#### Managing Queues

#### Inspecting Queues

#### Constants

