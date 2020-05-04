---
description: 단일 태스크와 관련된 코드 및 데이터를 나타내는 추상 클래스.
---

# Operation

### Declaration

```swift
class Operation : NSObject
```

### Overview

`Operation` 클래스는 추상 클래스이기 때문에, 직접 사용하지 않고 대신 서브 클래스를 사용하거나 시스템 정의 서브 클래스\([`NSInvocationOperation`](https://developer.apple.com/documentation/foundation/nsinvocationoperation) 또는 [`BlockOperation`](https://developer.apple.com/documentation/foundation/blockoperation)\)중 하나를 사용하여 실제 작업을 수행하라. 추상적이기는 하지만 `Operation`의 기본 구현에는 작업 수행의 안전한 실행을 조정하기 위한 중요한 논리가 포함되어 있다. 이 내장 논리가 존재하면 다른 시스템 객체와 올바르게 작동하도록 보장하는 데 필요한 접착제 코드보다는 실제 작업 구현에 집중할 수 있다.

오퍼레이션 객체는 싱글-샷 객체이다\(즉, 작업을 한 번 실행하고 다시 실행하는 데 사용할 수 없음\). 일반적으로 오퍼레이션 큐\([`OperationQueue`](https://developer.apple.com/documentation/foundation/operationqueue) 클래스의 인스턴스\)에 오퍼레이션을 추가하여 실행한다. 오퍼레이션 큐는 직접 보조 쓰레드에서 실행하거나 `libdispatch` 라이브러리\(Grand Central Dispatch라고도 함\)를 간접적으로 사용하여 오퍼레이션을 실행한다. 큐가 오퍼레이션을 실행하는 방법에 대한 자세한 내용은 [`OperationQueue`](https://developer.apple.com/documentation/foundation/operationqueue)을 참조하라.

오퍼레이션 큐를 사용하지 않으려면 직접 `start()` 메서드를 호출하여 직접 오퍼레이션을 실행할 수 있다. 준비 상태가 아닌 오퍼레이션을 시작하면 예외가 발생하기 때문에 수동으로 오퍼레이션을 실행하면 코드에 부담이 가중된다. `isReady` 속성은 오퍼레이션 준비 상태에 대해 보고한다.

#### Operation Dependencies <a id="1661157"></a>

종속성은 특정 순서로 오퍼레이션을 실행하는 편리한 방법이다. `addDepency(_:)` 및 `Dependency(_:)` 메서드를 사용하여 오퍼레이션에 대한 종속성을 추가 및 제거할 수 있다. 기본적으로 종속성이 있는 오퍼레이션 객체는 종속된 모든 오퍼레이션 객체가 실행을 마칠 때까지 준비되지 않은 것으로 간주된다. 그러나 마지막 종속 오퍼레이션이 끝나면 오퍼레이션 객체가 준비되고 실행할 수 있다.

`NSOperation`이 지원하는 종속성은 종속 오퍼레이션이 성공적으로 완료되었는지 성공하지 못했는지에 대해 구별하지 않는다. \(즉, 오퍼레이션을 취소하는 것과 마찬가지로 오퍼레이션을 완료한 것으로 표시한다.\) 종속적인 오퍼레이션이 취소되었거나 오퍼레이션을 성공적으로 완료하지 못한 경우 종속성이 있는 오퍼레이션이 진행되어야 하는지 여부는 당신에게 달려 있다. 이를 위해 오퍼레이션 객체에 몇 가지 추가 에러 추적 기능을 통합해야 할 수 있다.

#### KVO-Compliant Properties <a id="1661183"></a>

NSOperation 클래스는 key-value coding \(KVC\)와 key-value observing \(KVO\)은 그 속성 중 몇 가지를 준수한다. 필요에 따라 이러한 속성을 옵저빙하여 애플리케이션의 다른 부분을 제어할 수 있다. 속성을 옵저빙하려면 다음 키패스를 사용하라:

* `isCancelled` - read-only
* `isAsynchronous` - read-only
* `isExecuting` - read-only
* `isFinished` - read-only
* `isReady` - read-only
* `dependencies` - read-only
* `queuePriority` - readable and writable
* `completionBlock` - readable and writable

이러한 속성에 옵저버를 부착할 수는 있지만 Cocoa 바인딩을 사용하여 애플리케이션 사용자 인터페이스 요소에 바인딩해서는 안 된다. 사용자 인터페이스와 관련된 코드는 일반적으로 애플리케이션의 메인 쓰레드에서만 실행해야 한다. 오퍼레이션은 쓰레드에서 실행될 수 있기 때문에 해당 오퍼레이션과 관련된 KVO 노티피케이션은 쓰레드에서 유사하게 발생할 수 있다.

이전 속성 중 하나에 대해 사용자 정의 구현을 제공하는 경우 구현시 KVC 및 KVO를 준수하는 것을 유지해야 한다. `NSOperation` 객체에 대한 추가 속성을 정의하는 경우 해당 속성 KVC 및 KVO도 준수하도록 하는 것이 좋다. key-value coding을 지원하는 방법에 대한 내용은 [Key-Value Coding Programming Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/KeyValueCoding/index.html#//apple_ref/doc/uid/10000107i)을 참조하라. key-value observing을 지원하는 방법에 대한 내용은 [Key-Value Observing Programming Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)을 참조하라.

#### Multicore Considerations <a id="1661204"></a>

`NSOperation` 클래스는 그 자체로 멀티코어 인식이다. 따라서 객체에 대한 접근을 동기화하기 위해 추가적인 락을 만들지 않고 여러 쓰레드에서 `NSOperation` 객체의 메서드를 호출하는 것이 안전하다. 이 동작은 일반적으로 오퍼레이션이 생성하고 모니터링하는 별도의 하나의 쓰레드에서 실행되기 때문에 필요하다. 

`NSOperation` 서브 클래스에서는 오버라이드된 메서드가 여러 쓰레드에서 호출할 수 있도록 안전하게 유지되었는지 확인하라. 사용자 정의 데이터 접근기와 같은 서브 클래스에서 사용자 정의 메서드를 구현하는 경우 이러한 메서드가 쓰레드 세이프한지 확인해야 한다. 따라서 잠재적인 데이터 손상을 방지하기 위해 오퍼레이션 중 모든 데이터 변수에 대한 접근을 동기화해야 한다. 동기화에 대한 자세한 내용은 [Threading Programming Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/Introduction/Introduction.html#//apple_ref/doc/uid/10000057i)을 참조하라.

#### Asynchronous Versus Synchronous Operations <a id="1661231"></a>

수동으로 오퍼레이션 객체를 실행할 경우 큐에 추가하는 대신 동기식 또는 비동기식으로 실행하도록 오퍼레이션을 설계할 수 있다. 오퍼레이션 객체는 기본적으로 동기식이다. 동기식 오퍼레이션에서는 오퍼레이션 객체가 작업을 실행할 별도의 쓰레드를 생성하지 않는다. 코드에서 동기식 오퍼레이션의 `start()` 메서드를 직접 호출하면, 연산은 현재 쓰레드에서 즉시 실행된다. 그러한 객체의 `start()` 메서드가 호출자에게 제어권을 반환할쯤에 작업 자체가 완료된다.

비동기 오퍼레이션의 `start()` 메서드를 호출하면 해당 작업이 완료되기전에 해당 메서드가 반환될 수 있다. 비동기 오퍼레이션 객체는 변도의 쓰레드에서 작업을 스케줄링하는 역할을 한다. 이 오퍼레이션은 새 쓰레드를 직접 시작하거나, 비동기식 메서드를 호출하거나, 실행을 위해 디스패치 큐에 블록을 제출하는 방식으로 수행할 수 있다. 통제가 호출자에게 돌아올 때 오퍼레이션이 진행 중인지 여부는 실제로 중요하지 않으며, 다만 진행 중일 수 있다.

큐를 사용하여 오퍼레이션을 실행할 계획인 경우, 큐를 동기식으로 정의하는 것이 더 간단하다. 그러나 수동으로 오퍼레이션을 실행하는 경우 오퍼레이션 객체를 비동기식으로 정의할 수 있다. 비동기 오퍼레이션을 정의하려면 작업이 더 많이 필요하다. 작업의 진행 상태를 모니터링하고 KVO 알림을 사용하여 해당 상태의 변경 사항을 보고해야 하기 때문이다. 그러나 수동으로 실행된 오퍼레이션이 호출 쓰레드를 차단하지 않도록 하려면 비동기식 오퍼레이션을 정의하는 것이 유용하다.

오퍼레이션 큐에 오퍼레이션을 추가할 때 큐는 [`isAsynchronous`](https://developer.apple.com/documentation/foundation/operation/1408275-isasynchronous) 속성 값을 무시하고 항상 별도의 쓰레드에서 `start()` 메서드를 호출한다. 따라서 항상 오퍼레이션 큐에 추가하여 오퍼레이션을 실행한다면 비동기화시킬 이유가 없다.

동기식 및 비동기식 오퍼레이션을 정의하는 방법에 대한 자세한 내용은 서브클래싱 노트를 참조하라.

#### Subclassing Notes <a id="1673799"></a>

`NSOperation` 클래스는 오퍼레이션의 실행 상태를 추적하기 위한 기본 논리를 제공하지만 그렇지 않은 경우 실제 작업을 수행하려면 서브클래싱되어야 한다. 서브클래스를 만드는 방법은 오퍼레이션의 동시 실행 여부에 따라 달라진다.

**Methods to Override**

비-동시 오퍼레이션의 경우, 일반적으로 다음 한 가지 메서드만 오버라이드하라:

* [`main()`](https://developer.apple.com/documentation/foundation/operation/1407732-main)

이 메서드에 지정된 작업을 수행하는 데 필요한 코드를 배치해야 한다. 물론 사용자 정의 클래스의 인스턴스를 더 쉽게 만들 수 있도록 사용자 정의 이니셜라이저 메서드를 정의해야 한다. 또한 오퍼레이션에서 데이터에 접근하는 게터 및 세터 메서드를 정의할 수 있다. 그러나 사용자 정의 게터 및 세터 메서드를 정의하는 경우, 여러 쓰레드에서 안전하게 호출할 수 있는지 확인해야 한다.

동시 오퍼레이션을 작성하는 경우, 다음 메서드 및 속성을 최소한 오버라이드해야 한다:

* [`start()`](https://developer.apple.com/documentation/foundation/operation/1416837-start)
* [`isAsynchronous`](https://developer.apple.com/documentation/foundation/operation/1408275-isasynchronous)
* [`isExecuting`](https://developer.apple.com/documentation/foundation/operation/1415621-isexecuting)
* [`isFinished`](https://developer.apple.com/documentation/foundation/operation/1413540-isfinished)

동시 오퍼레이션에서, 당신의 `start()` 메서드는 비동기식으로 오퍼레이션을 시작하는 책임이 있다. 쓰레드를 생성을 하던 비동기 메서드를 호출하던 이 메서드로 한다. 오퍼레이션을 시작할 때 `start()` 메서드는 `isExecuting` 속성이 보고한 작업의 실행 상태를 업데이트해야 한다. 이 작업을 수행하려면 isExecuting 키패스에 대한 KVO 노티피케이션을 보내라. 이로 인해, 관심있는 클라이언트는 현재 오퍼레이션이 실행 중임을 알 수 있다. `isExecuting` 속성은 또한 쓰레드 세이프하게 상태를 제공해야 한다.

작업을 완료하거나 취소하면 동시 오퍼레이션 객체가 isExecution 및 isFinished 키패스 모두에 대해 KVO 노티피케이션을 생성하여 오퍼레이션에 대한 최종 상태 변경을 표시해야 한다. \(취소의 경우, 오퍼레이션이 완전히 완료되지 않았더라도 `isFinished` 키패스를 업데이트하는 것이 여전히 중요하다. 대기 중인 오퍼레이션은 큐에서 제거되기 전에 완료되었음을 보고해야 한다.\) KVO 노티피케이션을 생성하는 것 외에도, `isExecuting` 및 `isFinished` 속성의 오버라이드는 오퍼레이션 상태에 따라 정확한 값을 계속 보고해야 한다.

동시 오퍼레이션을 정의하는 방법에 대한 자세한 정보와 가이드라인은 [Concurrency Programming Guide](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)을 참조하라.

> 중요
>
> `start()` 메서드와 동시에 `super`를 호출하면 안된다. 동시 오퍼레이션을 정의할 때 기본 `start()` 메서드가 제공하는 작업을 시작해 적절한 KVO 노티피케이션을 생성하는 것과 동일한 동작을 제공하도록 스스로에게 맡긴다. `start()` 메서드도 작업을 실제로 시작하기 전에 오퍼레이션 자체가 취소되었는지 확인해야 한다. 취소 의미론에 대한 자세한 내용은 [Responding to the Cancel Command](https://developer.apple.com/documentation/foundation/operation#1661262)을 참조하라.

동시 오퍼레이션의 경우에도 위에서 설명한 메서드 이외의 메서드를 재정의할 필요가 거의 없어야 한다. 그러나 오퍼레이션의 종속성 기능을 사용자 정의할 경우 추가적인 메서드를 재정의하고 KVO 노티피케이션을 추가로 제공해야 할 수 있다. 종속성의 경우, `isReady` 키패스에 대한 노티피케이션만 제공하면 될 것이다. `dependencies` 속성에 종속성 오퍼레이션 목록이 포함되어 있기 때문에 변경 사항은 기본 `NSOperation` 클래스에서 이미 처리된다.

**Maintaining Operation Object States**

오퍼레이션 객체는 언제 실행이 안전한지 판단하기 위해 내부적으로 상태 정보를 유지하고, 또한 오퍼레이션 생명주기를 통해 진행 상황을 외부 클라이언트에게 통지한다. 사용자 정의 서브 클래스는 코드에서 올바른 오퍼레이션 실행을 보장하기 위해 이 상태 정보를 유지한다. 오퍼레이션 상태와 관련된 키패스는:

* **`isReady`** `isReady` 키패스는 오퍼레이션을 실행할 준비가 되면 클라이언트에 알려준다. [`isReady`](https://developer.apple.com/documentation/foundation/operation/1412992-isready) 속성은 오퍼레이션이 지금 실행될 준비가 되었을 때 true 값을 포함하거나, 아직 오퍼레이션이 종속된 미완성 오퍼레이션이 있을 경우 false 값을 포함한다. 대부분의 경우, 이 키패스의 상태를 직접 관리할 필요는 없다. 그러나 오퍼레이션 준비도가 종속 오퍼레이션 이외의 요인에 의해 결정된다면, 자신의 [`isReady`](https://developer.apple.com/documentation/foundation/operation/1412992-isready)를 구현하고 오퍼레이션 준비도를 추적할 수 있다. 외부 상태가 허용할 때만 오퍼레이션 객체를 작성하는 것이 더 간단할 수 있다. macOS 10.6 이상에서는 하나 이상의 종속 오퍼레이션이 완료되기를 기다리는 동안 오퍼레이션을 취소하면 그 종속성이 나중에 무시되고 이 속성의 값이 현재 실행 준비가 되었음을 반영하여 업데이트된다. 이 동작은 오퍼레이션 큐에서 취소된 오퍼레이션을 더 빨리 큐 밖으로 플러시할 수 있는 기회를 제공한다.
* **`isExecuting`** `isExecuting` 키패스는 오퍼레이션이 할당된 작업에 대해 현재 작업 중인지 여부를 클라이언트에 알린다. [`isExecuting`](https://developer.apple.com/documentation/foundation/operation/1415621-isexecuting) 속성은 오퍼레이션이 해당 작업에 대해 작업 중인 경우 true 값을 보고해야 하며 그렇지 않은 경우 false 값을 보고해야 한다. 오퍼레이션 객체의 [`start()`](https://developer.apple.com/documentation/foundation/operation/1416837-start) 메서드를 교체하는 경우 [`isExecuting`](https://developer.apple.com/documentation/foundation/operation/1415621-isexecuting) 속성을 교체하고 오퍼레이션의 실행 상태가 변경될 때 KVO 노티피케이션을 생성해야 한다.
* **`isFinished`** `isFinished` 키패스는 오퍼레이션이 성공적으로 완료되었거나 취소되어 종료 중임을 클라이언트에 알려준다. `isFinished` 키패스의 값이 true로 변경될 때까지 오퍼레이션 객체는 종속성을 지우지 않는다. 마찬가지로, 오퍼레이션 큐는 완료된 속성에 true 값이 포함될 때까지 오퍼레이션을 디큐하지 않는다. 따라서 오퍼레이션을 완료로 표시하는 것은 큐가 진행 중이거나 취소된 오퍼레이션을 백업하지 않도록 하는 데 매우 중요하다. [`start()`](https://developer.apple.com/documentation/foundation/operation/1416837-start) 메서드 또는 오퍼레이션 객체를 교체하는 경우, 오퍼레이션이 완료되거나 취소될 때 [`isFinished`](https://developer.apple.com/documentation/foundation/operation/1413540-isfinished) 속성을 교체하고 KVO 노티피케이션을 생성해야 한다.
* **`isCancelled`** isCancelled 키패스를 통해 클라이언트는 오퍼레이션 취소가 요청되었음을 알 수 있다. 취소 지원은 자발적이지만 권장되며 이 키패스에 대해 KVO 알림을 보낼 필요가 없는 자체코드이다. 오퍼레이션 중 취소 통지 처리는 [Responding to the Cancel Command](https://developer.apple.com/documentation/foundation/operation#1661262)에 자세히 설명되어 있다.

**Responding to the Cancel Command**

일단 오퍼레이션을 큐에 추가하면 오퍼레이션을 손을 쓸 수 없게 된다. 큐가 작업을 인계받아 스케줄링을 처리한다. 그러나 나중에 사용자가 진행 패널에서 취소 버튼을 누르거나 애플리케이션을 종료했기 때문에 오퍼레이션을 실행하지 않겠다고 결정하면 작업을 취소하여 CPU 시간을 불필요한 것으로 소비하지 않도록 할 수 있다. 오퍼레이션 객체 자체의 `cancel()` 메서드를 호출하거나 `OperationQueue` 클래스의 [`cancelAllOperations()`](https://developer.apple.com/documentation/foundation/operationqueue/1417849-cancelalloperations) 메서드를 호출하여 이 오퍼레이션을 취소하라.

오퍼레이션을 취소한다고 해서 당장 중단하는 것은 아니다. `isCancelled` 속성의 값을 중요시하는 것이 모든 오퍼레이션에서 예상 되지만 당신의 코드는 이 속성의 값을 명시적으로 확인하고 필요에 따라 중단해야 한다. `NSOperation`의 기본 구현에는 취소에 대한 검사가 포함된다. 예를 들어, `start()` 메서드가 호출되기 전에 오퍼레이션을 취소하면 작업을 시작하지 않고 `start()` 메서드가 종료된다.

> **참고**
>
> macOS 10.6 이상에서 오퍼레이션 큐에 있고 완료되지 않은 종속 오퍼레이션이 있는 작업에 대해 `cancel()` 메서드를 호출하면 해당 종속 오퍼레이션은 나중에 무시된다. 오퍼레이션이 이미 취소되었기 때문에 이 동작을 통해 큐가 `main()` 메서드를 호출하지 않고 오퍼레이션의 `start()` 메서드를 호출하여 오퍼레이션을 큐에서 제거할 수 있다. 큐에 없는 오퍼레이션에서 `cancel()` 메서드를 호출하면 즉시 취소로 표시된다. 각 경우에, 오퍼레이션을 준비 또는 완료로 표시하면 적절한 KVO 노티피케이션이 생성된다.

작성하는 모든 사용자 정의 코드에서 취소 의미론을 항상 지원해야 한다. 특히 메인 작업 코드는 `isCancelled` 속성의 값을 주기적으로 확인해야 한다. 속성이 true 값을 보고한다면, 오퍼레이션 객체는 가능한 빨리 정리 후 종료해야 한다. 사용자 정의 `start()` 메서드를 구현한다면, 그 메서드는 취소와 적절히 행동하는 것에 대한 초기 검사를 포함해야 한다. 이러한 유형의 조기 취소를 처리하려면 사용자 정의 `start()` 메서드를 준비해야 한다.

오퍼레이션이 취소될 때 간단히 종료할 뿐만 아니라 취소된 오퍼레이션을 적절한 최종 상태로 이동하는 것도 중요하다. 특히, `isFinished` 및 `isExecuting` 속성을 직접 관리하는 경우 \(동시 오퍼레이션을 구현하고 있기 때문일 수 있다\), 해당 속성을 적절하게 업데이트 해야 한다. 구체적으로 `isFinished`가 반환한 값을 true로 `isExecution`이 반환한 값을 false로 변경해야 한다. 오퍼레이션이 실행을 시작하기 전에 취소된 경우에도 이러한 변경사항을 적용하라.



### Topics

#### Executing the Operation

* [`func start()`](https://developer.apple.com/documentation/foundation/operation/1416837-start) 오퍼레이션의 실행을 시작한다.
* [`func main()`](https://developer.apple.com/documentation/foundation/operation/1407732-main) 수신자의 비 동시성 작업을 수행한다.
* [`var completionBlock: (() -> Void)?`](https://developer.apple.com/documentation/foundation/operation/1408085-completionblock) 오퍼레이션의 메인 작업이 완료된 후 실행할 블록.

#### Canceling Operations

* [`func cancel()`](https://developer.apple.com/documentation/foundation/operation/1411672-cancel) 오퍼레이션 객체가 작업 실행을 중지해야 함을 통지한다.

#### Getting the Operation Status

* [`var isCancelled: Bool`](https://developer.apple.com/documentation/foundation/operation/1408418-iscancelled) 오퍼레이션이 취소되었는지 여부를 나타내는 불 값.
* [`var isExecuting: Bool`](https://developer.apple.com/documentation/foundation/operation/1415621-isexecuting) 오퍼레이션이 현재 실행 중인지 여부를 나타내는 불 값.
* [`var isFinished: Bool`](https://developer.apple.com/documentation/foundation/operation/1413540-isfinished)  오퍼레이션이 작업 실행을 완료했는지 여부를 나타내는 불 값.
* [`var isConcurrent: Bool`](https://developer.apple.com/documentation/foundation/operation/1411089-isconcurrent) 오퍼레이션이 비동기식으로 작업을 실행하는지 여부를 나타내는 불 값.
* [`var isAsynchronous: Bool`](https://developer.apple.com/documentation/foundation/operation/1408275-isasynchronous) 오퍼레이션이 비동기식으로 작업을 실행하는지 여부를 나타내는 불 값.
* [`var isReady: Bool`](https://developer.apple.com/documentation/foundation/operation/1412992-isready) 지금 오퍼레이션을 수행할 수 있는지 여부를 나타내는 불 값.
* [`var name: String?`](https://developer.apple.com/documentation/foundation/operation/1416089-name) 오퍼레이션의 이름.

#### Managing Dependencies

* [`func addDependency(Operation)`](https://developer.apple.com/documentation/foundation/operation/1412859-adddependency) 지정된 오퍼레이션의 완료에 따라 수신기를 종속시킨다.
* [`func removeDependency(Operation)`](https://developer.apple.com/documentation/foundation/operation/1414945-removedependency) 지정된 오퍼레이션에 대한 수신기의 종속성을 제거한다.
* [`var dependencies: [Operation]`](https://developer.apple.com/documentation/foundation/operation/1416668-dependencies) 현재 객체가 실행을 시작하기 전에 실해을 완료해야 하는 오퍼레이션 객체 배열.

#### Configuring the Execution Priority

* [`var qualityOfService: QualityOfService`](https://developer.apple.com/documentation/foundation/operation/1413553-qualityofservice) 오퍼레이션에 시스템 리소스를 부여하기 위한 상대적 중요도.
* [`var queuePriority: Operation.QueuePriority`](https://developer.apple.com/documentation/foundation/operation/1411204-queuepriority) 오퍼레이션 큐에서 오퍼레이션의 실행 우선 순위.

#### Waiting on an Operation Object

* [`func waitUntilFinished()`](https://developer.apple.com/documentation/foundation/operation/1409256-waituntilfinished) 오퍼레이션 객체가 작업을 완료할 때까지 현재 쓰레드의 실행을 차단한다.

#### Constants

* [`enum Operation.QueuePriority`](https://developer.apple.com/documentation/foundation/operation/queuepriority) 이러한 상수를 사용하면 오퍼레이션이 실행되는 순서에 우선순위를 지정할 수 있다.
* [`enum QualityOfService`](https://developer.apple.com/documentation/foundation/qualityofservice) 시스템에 대한 작업의 성격 및 중요성을 나타내기 위해 사용된다.  서비스 클래스의 퀄리티가 더 높은 작업에서는 리소스 경합이 있을 때마다 서비스 클래스의 퀄리티가 낮은 작업보다 더 많은 리소스를 받는다.



