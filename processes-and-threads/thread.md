---
description: 실행 쓰레드.
---

# Thread

### Declaration

```swift
class Thread : NSObject
```

### Overview

Objective-C 메서드를 실행 쓰레드에서 실행하려면 이 클래스를 사용하라. 쓰레드는 긴 태스크를 수행해야 할 때 특히 유용하지만, 나머지 애플리케이션의 실행을 차단하지 않기를 바란다. 특히 쓰레드를 사용하여 사용자 인터페이스와 이벤트 관련 작업을 처리하는 애플리케이션의 메인 쓰레드를 차단하지 않도록 할 수 있다. 쓰레드는 또한 큰 작업을 몇 개의 작은 작업으로 나누는데 사용할 수 있으며, 이는 멀티 코어 컴퓨터의 성능 향상으로 이어질 수 있다.

`Thread` 클래스는 쓰레드의 런타임 상태를 모니터링하기 위해 [`Operation`](https://developer.apple.com/documentation/foundation/operation)과 유사한 의미론을 지원한다. 이러한 의미론을 사용하여 쓰레드 실행을 취소하거나 쓰레드가 여전히 실행 중인지 또는 태스크를 완료했는지 확인할 수 있다. 쓰레드를 취소하려면 쓰레드 코드의 지원이 필요하다. 자세한 내용은 [`cancel()`](https://developer.apple.com/documentation/foundation/thread/1411303-cancel) 설명을 참조하라.

#### Subclassing Notes <a id="1679134"></a>

`Thread`를 서브 클래싱하고 [`main()`](https://developer.apple.com/documentation/foundation/thread/1418421-main) 메서드를 오버라이드하여 쓰레드의 메인 진입점을 구현할 수 있다. [`main()`](https://developer.apple.com/documentation/foundation/thread/1418421-main)을 오버라이드 하는 경우, `super`를 호출하여 상속된 동작을 호출할 필요가 없다.

### Topics

#### Initializing an NSThread Object

* [`init()`](https://developer.apple.com/documentation/foundation/thread/1416464-init) 초기화된 `NSThread` 객체를 반환한다.
* [`init(target: Any, selector: Selector, object: Any?)`](https://developer.apple.com/documentation/foundation/thread/1414773-init) 지정된 전달인자로 초기화된 `NSThread` 객체를 반환한다.

#### Starting a Thread

* [`class func detachNewThreadSelector(Selector, toTarget: Any, with: Any?)`](https://developer.apple.com/documentation/foundation/thread/1415633-detachnewthreadselector) 새 쓰레드를 분리하고 지정된 셀렉터를 쓰레드 진입점으로 사용하라.
* [`func start()`](https://developer.apple.com/documentation/foundation/thread/1418166-start) 수신기를 시작한다.
* [`func main()`](https://developer.apple.com/documentation/foundation/thread/1418421-main) 쓰레드의 기본 진입점 루틴.

#### Stopping a Thread

* [`class func sleep(until: Date)`](https://developer.apple.com/documentation/foundation/thread/1415959-sleep) 지정된 시간까지 현재 쓰레드를 차단한다.
* [`class func sleep(forTimeInterval: TimeInterval)`](https://developer.apple.com/documentation/foundation/thread/1413673-sleep) 주어진 시간 간격동안 쓰레드를 차단한다.
* [`class func exit()`](https://developer.apple.com/documentation/foundation/thread/1409404-exit) 현재 쓰레드를 종료한다.
* [`func cancel()`](https://developer.apple.com/documentation/foundation/thread/1411303-cancel) 수신기가 종료되어야 함을 나타내도록 수신기의 취소 상태를 변경한다.

#### Determining the Thread’s Execution State

* [`var isExecuting: Bool`](https://developer.apple.com/documentation/foundation/thread/1411240-isexecuting) 수신기가 실행 중인지 여부를 나타내는 불 값.
* [`var isFinished: Bool`](https://developer.apple.com/documentation/foundation/thread/1409297-isfinished) 수신기가 실행을 완료했는지 여부를 나타내는 불 값.
* [`var isCancelled: Bool`](https://developer.apple.com/documentation/foundation/thread/1417366-iscancelled) 수신기가 취소되었는 여부를 나타내는 불 값.

#### Working with the Main Thread

* [`class var isMainThread: Bool`](https://developer.apple.com/documentation/foundation/thread/1412704-ismainthread) 현재 쓰레드가 메인 쓰레드인지 여부를 나타내는 불 값을 반환한다.
* [`var isMainThread: Bool`](https://developer.apple.com/documentation/foundation/thread/1408455-ismainthread) 수신기가 메인 쓰레드인지 여부를 나타내는 불 값.
* [`class var main: Thread`](https://developer.apple.com/documentation/foundation/thread/1414782-main) 메인 쓰레드를 나타내는 `NSThread` 객체를 반환한다.

#### Querying the Environment

* [`class func isMultiThreaded() -> Bool`](https://developer.apple.com/documentation/foundation/thread/1410702-ismultithreaded) 애플리케이션의 멀티쓰레드 여부를 반환한다.
* [`class var current: Thread`](https://developer.apple.com/documentation/foundation/thread/1410679-current) 현재 실행 쓰레드를 나타내는 쓰레드 객체를 반환한다.
* [`class var callStackReturnAddresses: [NSNumber]`](https://developer.apple.com/documentation/foundation/thread/1409565-callstackreturnaddresses) 콜 스택 반환 주소가 들어 있는 배열을 반환한다.
* [`class var callStackSymbols: [String]`](https://developer.apple.com/documentation/foundation/thread/1414836-callstacksymbols) 콜 스택 기호가 들어 있는 배열을 반환한다.

#### Working with Thread Properties

* [`var threadDictionary: NSMutableDictionary`](https://developer.apple.com/documentation/foundation/thread/1411433-threaddictionary) 쓰레드 객체의 딕셔너리.
* [`let NSAssertionHandlerKey: String`](https://developer.apple.com/documentation/foundation/nsassertionhandlerkey)
* [`var name: String?`](https://developer.apple.com/documentation/foundation/thread/1414122-name) 수신기의 이름.
* [`var stackSize: Int`](https://developer.apple.com/documentation/foundation/thread/1415190-stacksize) 바이트 단위의 수신기 스택 크기.

#### Prioritizing Thread Work

* [`var qualityOfService: QualityOfService`](https://developer.apple.com/documentation/foundation/thread/1409426-qualityofservice)
* [`enum QualityOfService`](https://developer.apple.com/documentation/foundation/qualityofservice) 시스템에 대한 작업의 본질과 중요성을 나타내는 데 사용된다. 서비스 클래스의 품질이 높은 작업은 리소스 경쟁이 있을때마다 서비스 클래스의 품질이 낮은 작업보다 더 많은 리소스를 수신한다.
* [`class func threadPriority() -> Double`](https://developer.apple.com/documentation/foundation/thread/1417675-threadpriority) 현재 쓰레드의 우선 순위를 반환한다.
* [`var threadPriority: Double`](https://developer.apple.com/documentation/foundation/thread/1411927-threadpriority) 수신자의 우선순위
* [`class func setThreadPriority(Double) -> Bool`](https://developer.apple.com/documentation/foundation/thread/1407523-setthreadpriority) 현재 쓰레드의 우선순위를 설정한다.

#### Notifications

* [`static let NSDidBecomeSingleThreaded: NSNotification.Name`](https://developer.apple.com/documentation/foundation/nsnotification/name/1412246-nsdidbecomesinglethreaded) 구현되지 않음.
* [`static let NSThreadWillExit: NSNotification.Name`](https://developer.apple.com/documentation/foundation/nsnotification/name/1408204-nsthreadwillexit) 하나의 `NSThread` 객체는 쓰레드가 종료되기 전에 [ `exit()`](https://developer.apple.com/documentation/foundation/thread/1409404-exit)  메시지를 수신하는 노티피케이션을 게시한다. 이 노티피케이션을 수신하기 위해 호출된 옵저버 메서드는 종료하기 전에 종료 쓰레드에서 실행된다.
* [`static let NSWillBecomeMultiThreaded: NSNotification.Name`](https://developer.apple.com/documentation/foundation/nsnotification/name/1417567-nswillbecomemultithreaded) 첫 번째 쓰레드가 현재 쓰레드에서 분리될 때 게시된다. `NSThread` 클래스는 이 노티피케이션을 한번 게시한다. 처음으로 쓰레드가 분리되었을 때 [`detachNewThreadSelector(_:toTarget:with:)`](https://developer.apple.com/documentation/foundation/thread/1415633-detachnewthreadselector) 또는 [`start()`](https://developer.apple.com/documentation/foundation/thread/1418166-start) 메서드를 사용된다. 그러한 메서드의 후속 호출은 이 노티피케이션을 게시하지 않는다. 이 노티피케이션의 옵저버는 새 쓰레드가 아닌 기본 쓰레드에서 노티피케이션 메서드를 호출한다. 옵저버 노티피케이션 메서드는 항상 새 쓰레드가 실행되기 전에 실행된다.

#### Initializers

* [`init(block: () -> Void)`](https://developer.apple.com/documentation/foundation/thread/2088561-init)

#### Type Methods

* [`class func detachNewThread(() -> Void)`](https://developer.apple.com/documentation/foundation/thread/2088563-detachnewthread)

