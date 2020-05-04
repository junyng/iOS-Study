---
description: 오퍼레이션 실행을 규제하는 큐.
---

# OperationQueue

### Declaration

```swift
class OperationQueue : NSObject
```

### Overview

오퍼레이션 큐는 우선순위 및 준비 상태에 따라 대기 중인 [`Operation`](https://developer.apple.com/documentation/foundation/operation) 객체들을 실행한다. 오퍼레이션 큐에 추가된 오퍼레이션은 작업이 완료되었음을 보고할 때까지 큐에 남아 있다. 오퍼레이션을 추가한 후에는 대기열에서 직접 제거할 수 없다.

> **참고**
>
> 오퍼레이션 큐는 완료될 때까지 오퍼레이션을 리테인하며, 완료될 때까지 큐 자체를 리테인 한다. 완료되지 않은 오퍼레이션으로 오퍼레이션 큐를 중지하면 메모리 누수를 발생할 수 있다.

오퍼레이션 큐 사용에 대한 자세한 내용은 [Concurrency Programming Guide](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)를 참조하라.

#### Determining Execution Order <a id="2936058"></a>

큐 내의 오퍼레이션은 준비 상태, 우선순위 수준 및 상호운용 의존성에 따라 구성되며, 그에 따라 실행된다. 대기 중인 모든 오퍼레이션의 [`queuePriority`](https://developer.apple.com/documentation/foundation/operation/1411204-queuepriority)가 같으며 큐에 넣을 때 실행할 준비가 된 경우\(즉, 그들의 [`isReady`](https://developer.apple.com/documentation/foundation/operation/1412992-isready) 속성이 true이다.\) 큐에 제출된 순서대로 실행된다. 그렇지 않으면, 오퍼레이션 큐는 항상 다른 준비 오퍼레이션에 비해 우선순위가 가장 높은 오퍼레이션을 실행한다.

그러나 오퍼레이션 준비 상태의 변경으로 인해 결과적인 실행 순서가 변경될 수 있으므로, 오퍼레이션의 특정 실행 순서를 보장하기 위해 큐 의미론에 의존해서는 안된다. 상호 운용 의존성은 다른 운용 큐에 위치하더라도 운영에 대한 절대 실행 명령을 제공한다. 모든 의존 오퍼레이션 실행이 완료될 때까지 오퍼레이션 객체를 실행할 준비가 되어 있지 않은 것으로 간주된다.

우선 순위 수준 및 의존성 설정 방법에 대한 자세한 내용은 [`Operation`](https://developer.apple.com/documentation/foundation/operation)의 [Managing Dependencies](https://developer.apple.com/documentation/foundation/operation#1661485)를 참조하라.

#### Canceling Operations <a id="2936059"></a>

오퍼레이션을 끝내는 것이 반드시 그 오퍼레이션을 완료하기 위해 수행했다는 것을 의미하지는 않는다. 오퍼레이션은 취소될 수 있다. 오퍼레이션 객체를 취소하면 객체는 큐에 남게 되지만 객체는 가능한 빨리 작업을 중지해야 함을 통지한다. 현재 실행 중인 오퍼레이션의 경우, 오퍼레이션 객체의 작업 코드가 취소 상태를 확인하고, 수행 중인 작업을 중지하고, 완료됨으로 표시해야 함을 의미한다. 큐에 있지만 아직 실행되지 않은 오퍼레이션의 경우 큐는 여전히 오퍼레이션 객체의 [`start()`](https://developer.apple.com/documentation/foundation/operation/1416837-start) 메서드를 호출하여 취소 이벤트를 처리하고 완료된 대로 표시해야 한다.

> **참고**
>
> 오퍼레이션을 취소하면 오퍼레이션이 가질 수 있는 의존성을 무시하게 된다. 이 동작으로 큐가 가능한 한 빨리 오퍼레이션의 `start()` 메서드를 실행할 수 있다. `start()` 메서드는 차례로 오퍼레이션을 완료 상태로 이동시켜 큐에서 제거할 수 있다.

오퍼레이션 취소에 대한 자세한 내용은 [`Operation`](https://developer.apple.com/documentation/foundation/operation)의 [Responding to the Cancel Command](https://developer.apple.com/documentation/foundation/operation#1661262)를 참조하라.

#### KVO-Compliant Properties <a id="1661144"></a>

`OperationQueue` 클래스는 key-value 코딩\(KVC\)과 key-value 옵저빙\(KVO\)를 준수한다. 애플리케이션의 다른 부분을 제어하기 위해 원하는 대로 이러한 속성을 관찰할 수 있다. 속성을 관찰하려면 다음 키패스를 사용하라:

* [`operations`](https://developer.apple.com/documentation/foundation/operationqueue/1415168-operations) - read-only
* [`operationCount`](https://developer.apple.com/documentation/foundation/operationqueue/1415115-operationcount) - read-only
* [`maxConcurrentOperationCount`](https://developer.apple.com/documentation/foundation/operationqueue/1414982-maxconcurrentoperationcount) - readable and writable
* [`isSuspended`](https://developer.apple.com/documentation/foundation/operationqueue/1415909-issuspended) - readable and writable
* [`name`](https://developer.apple.com/documentation/foundation/operationqueue/1418063-name) - readable and writable

이러한 속성에 옵저버를 부착할 수는 있지만 Cocoa 바인딩을 사용하여 애플리케이션의 사용자 인터페이스 요소에 바인딩해서는 안된다. 사용자 인터페이스와 관련된 코드는 일반적으로 앱의 메인 쓰레드에서만 실행해야 한다. 그러나 오퍼레이션 큐와 관련된 KVO 노티피케이션은 모든 쓰레드에서 발생할 수 있다.

key-value 옵저빙과 옵저버를 객체에 부착하는 방법에 대한 자세한 내용은 [Key-Value Observing Programming Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)를 참조하라.

#### Thread Safety <a id="1661160"></a>

해당 객체에 대한 접근을 동기화하기 위해 추가적인 락을 만들지 않고 여러 쓰레드에서 단일 `OperationQueue` 객체를 사용하는 것이 안전하다.

오퍼레이션 큐는 [Dispatch](https://developer.apple.com/documentation/dispatch) 프레임워크를 사용하여 오퍼레이션 실행을 시작한다. 결과적으로, 오퍼레이션은 동기식 또는 비동기식 지정 여부와 상관없이 항상 별도의 쓰레드에서 실행된다.

### Topics

#### Accessing Specific Operation Queues

* [`class var main: OperationQueue`](https://developer.apple.com/documentation/foundation/operationqueue/1409193-main) 메인 쓰레드와 연결된 오퍼레이션 큐를 반환한다.
* [`class var current: OperationQueue?`](https://developer.apple.com/documentation/foundation/operationqueue/1413097-current) 현재 오퍼레이션을 시작한 오퍼레이션 큐를 반환한다.

#### Managing Operations in the Queue

* [`func addOperation(Operation)`](https://developer.apple.com/documentation/foundation/operationqueue/1410704-addoperation) 지정한 오퍼레이션을 수신기에 추가한다.
* [`func addOperations([Operation], waitUntilFinished: Bool)`](https://developer.apple.com/documentation/foundation/operationqueue/1408358-addoperations) 지정한 오퍼레이션을 큐에 추가한다.
* [`func addOperation(() -> Void)`](https://developer.apple.com/documentation/foundation/operationqueue/1412949-addoperation) 오퍼레이션에서 지정된 블록을 감싸 수신기에 추가한다.
* [`func cancelAllOperations()`](https://developer.apple.com/documentation/foundation/operationqueue/1417849-cancelalloperations) 대기 중인 모든 오퍼레이션 및 실행 오퍼레이션을 취소한다.
* [`func waitUntilAllOperationsAreFinished()`](https://developer.apple.com/documentation/foundation/operationqueue/1407971-waituntilalloperationsarefinishe) 수신기의 큐와 실행 오퍼레이션이 모두 실행될 때까지 현재 쓰레드를 차단한다.

#### Managing the Execution of Operations

* [`var qualityOfService: QualityOfService`](https://developer.apple.com/documentation/foundation/operationqueue/1417919-qualityofservice) 큐를 사용하여 실행된 오퍼레이션에 적용할 기본 서비스 수준.
* [`var maxConcurrentOperationCount: Int`](https://developer.apple.com/documentation/foundation/operationqueue/1414982-maxconcurrentoperationcount) 동시에 실행할 수 있는 대기 중인 오퍼레이션의 최대 수.
* [`class let defaultMaxConcurrentOperationCount: Int`](https://developer.apple.com/documentation/foundation/operationqueue/1411680-defaultmaxconcurrentoperationcou) 큐에서 동시에 실행할 기본 최대 오퍼레이션의 수.

#### Suspending Execution

* [`var isSuspended: Bool`](https://developer.apple.com/documentation/foundation/operationqueue/1415909-issuspended) 큐가 실행을 위한 오퍼레이션을 활발히 스케줄링하고 있는지 여부를 나타내는 불 값.

#### Configuring the Queue

* [`var name: String?`](https://developer.apple.com/documentation/foundation/operationqueue/1418063-name) 오퍼레이션 큐의 이름.
* [`var underlyingQueue: DispatchQueue?`](https://developer.apple.com/documentation/foundation/operationqueue/1415344-underlyingqueue) 오퍼레이션을 실행하는 데 사용되는 디스패치 큐.

#### Instance Properties

* [`var minimumTolerance: OperationQueue.SchedulerTimeType.Stride`](https://developer.apple.com/documentation/foundation/operationqueue/3329362-minimumtolerance)
* [`var now: OperationQueue.SchedulerTimeType`](https://developer.apple.com/documentation/foundation/operationqueue/3329363-now)
* [`var progress: Progress`](https://developer.apple.com/documentation/foundation/operationqueue/3172535-progress)

#### Instance Methods

* [`func addBarrierBlock(() -> Void)`](https://developer.apple.com/documentation/foundation/operationqueue/3172534-addbarrierblock)
* [`func schedule(after: OperationQueue.SchedulerTimeType, interval: OperationQueue.SchedulerTimeType.Stride, tolerance: OperationQueue.SchedulerTimeType.Stride, options: OperationQueue.SchedulerOptions?, () -> Void) -> Cancellable`](https://developer.apple.com/documentation/foundation/operationqueue/3329364-schedule)
* [`func schedule(after: OperationQueue.SchedulerTimeType, tolerance: OperationQueue.SchedulerTimeType.Stride, options: OperationQueue.SchedulerOptions?, () -> Void)`](https://developer.apple.com/documentation/foundation/operationqueue/3329365-schedule)[`func schedule(options: OperationQueue.SchedulerOptions?, () -> Void)`](https://developer.apple.com/documentation/foundation/operationqueue/3329366-schedule)

#### Structures

* [`struct OperationQueue.SchedulerOptions`](https://developer.apple.com/documentation/foundation/operationqueue/scheduleroptions)
* [`struct OperationQueue.SchedulerTimeType`](https://developer.apple.com/documentation/foundation/operationqueue/schedulertimetype)

