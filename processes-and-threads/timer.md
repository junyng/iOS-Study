---
description: 특정 시간 간격이 경과한 후 작동하여 지정된 메시지를 대상 객체로 전송하는 타이머.
---

# Timer

### Declaration

```swift
class Timer : NSObject
```

### Overview

타이머는 런 루프와 함께 작동한다. 런 루프는 타이머에 대한 강한 참조를 유지하므로, 타이머를 런 루프에 추가한 후 타이머에 대한 자신의 강한 참조를 유지할 필요가 없다.

타이머를 효과적으로 사용하려면 런 루프가 어떻게 작동하는지 알아야 한다. 자세한 내용은 [Threading Programming Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/Introduction/Introduction.html#//apple_ref/doc/uid/10000057i)를 참조하라.

타이머는 실시간 메커니즘이 아니다. 장기 런 루프 호출 중에 또는 런 루프가 타이머를 모니터링하지 않는 모드에 있는 동안 타이머의 작동 시간이 발생하는 경우, 다음 런 루프가 타이머를 점검할 때까지 타이머는 작동하지 않는다. 따라서 타이머가 작동되는 실제 시간은 상당히 늦어질 수 있다. [Timer Tolerance](https://developer.apple.com/documentation/foundation/timer#1667624)를 참조하라.

[`Timer`](https://developer.apple.com/documentation/foundation/timer)는 Core Foundation [`CFRunLoopTimer`](https://developer.apple.com/documentation/corefoundation/cfrunlooptimer)와 toll-free bridged 이다. 자세한 내용은 [Toll-Free Bridging](https://developer.apple.com/library/archive/documentation/General/Conceptual/CocoaEncyclopedia/Toll-FreeBridgin/Toll-FreeBridgin.html#//apple_ref/doc/uid/TP40010810-CH2)을 참조하라.

#### Comparing Repeating and Nonrepeating Timers <a id="1667611"></a>

생성 시 타이머가 반복되는지 또는 반복되지 않는지 지정한다. 반복되지 않는 타이머는 한 번 작동한 후 자동으로 무효화되므로 타이머가 다시 작동하지 못하게 된다. 이와는 대조적으로, 반복되는 타이머가 작동된 다음 동일한 런 루프에서 다시 조정한다. 반복 타이머는 실제 작동 시간과 달리 항상 예정된 작동 시간을 기준으로 자체 일정을 잡는다. 예를 들어, 타이머가 특정 시간에 작동되도록 예약되어 있고 그 후 5초마다 작동하는 경우, 실제 작동 시간이 지연되더라도 예정된 작동 시간은 항상 원래 5초 간격으로 떨어진다. 작동 시간이 예정된 작동 시간 중 하나 이상을 통과할 정도로 지금까지 지연되면 타이머는 해당 기간 동안 단 한 번 작동된다. 타이머는 작동 후 향후 예정된 다음 작동 시간 동안 재조정된다.

#### Timer Tolerance <a id="1667624"></a>

iOS 7 이상과 macOS 10.9 이상에서는 타이머에 대한 허용오차\([`tolerance`](https://developer.apple.com/documentation/foundation/timer/1415085-tolerance)\)를 지정할 수 있다. 타이머 작동 시 이러한 유연성은 전력 절감 및 응답성 향상을 위해 시스템의 최적화 능력을 향상시킨다. 타이머는 예정된 작동 날짜와 오차를 추가한 날짜에 언제든지 작동될 수 있다. 타이머는 예정된 작동 날짜 전에 작동하지 않는다. 반복 타이머의 경우, 표류를 피하기 위해 개별 작동 시간에 적용되는 허용오차에 관계없이 다음 작동날짜를 원래 작동 날짜를 원래 작동 날짜로부터 계산한다. 기본 값은 0이며, 이는 추가 허용오차가 적용되지 않음을 의미한다. 시스템은 [`tolerance`](https://developer.apple.com/documentation/foundation/timer/1415085-tolerance) 속성 값에 관계없이 특정 타이머에 소량의 허용오차를 적용할 권리를 보유한다.

타이머의 사용자로서 타이머에 대한 적절한 허용오차를 결정할 수 있다. 일반 규칙으로 반복 타이머에 대해 허용오차를 간격의 10%이상으로 설정하라. 적은 양의 허용오차는 애플리케이션의 전력 사용량에 상당한 긍정적인 영향을 미친다. 시스템은 최대 허용오차 값을 적용할 수 있다.

#### Scheduling Timers in Run Loops <a id="1667642"></a>

타이머는 해당 런 루프 내에서 여러 런 루프 모드에 추가할 수 있지만 한 번에 하나의 런 루프에서만 등록할 수 있다. 타이머를 만드는 세 가지 방법이 있다:

* [`scheduledTimer(timeInterval:invocation:repeats:)`](https://developer.apple.com/documentation/foundation/timer/1415941-scheduledtimer) 또는 [`scheduledTimer(timeInterval:target:selector:userInfo:repeats:)`](https://developer.apple.com/documentation/foundation/timer/1412416-scheduledtimer)  타이머를 생성하고 기본 모드에서 현재 런 루프에서 타이머를 예약하는 클래스 메서드를 사용하라.
* [`init(timeInterval:invocation:repeats:)`](https://developer.apple.com/documentation/foundation/timer/1407170-init) 또는 [`init(timeInterval:target:selector:userInfo:repeats:)`](https://developer.apple.com/documentation/foundation/timer/1408356-init)   런 루프에서 타이머 객체를 예약하지 않고 만드는 클래스 메서드를 사용하라. \(타이머를 생성한 후에는 해당 `RunLoop` 객체의 [`add(_:forMode:)`](https://developer.apple.com/documentation/foundation/runloop/1418468-add) 메서드를 호출하여 타이머를 런 루프에 수동으로 추가해야 한다.\)
* [`init(fireAt:interval:target:selector:userInfo:repeats:)`](https://developer.apple.com/documentation/foundation/timer/1415700-init) 메서드를 사용하여 타이머를 할당하고 초기화하라. \(타이머를 생성한 후에는 해당 `RunLoop` 객체의 [`add(_:forMode:)`](https://developer.apple.com/documentation/foundation/runloop/1418468-add) 메서드를 호출하여 타이머를 런 루프에 수동으로 추가해야 한다.\)

런 루프에서 예약되면 타이머가 무효화될 때까지 지정된 간격으로 작동한다. 반복되지 않는 타이머는 작동 후 즉시 무효화된다. 그러나 반복 타이머의 경우 타이머 객체를 [`invalidate()`](https://developer.apple.com/documentation/foundation/timer/1415405-invalidate) 메서드를 직접 호출하여 무효화해야 한다. 이 메서드를 호출하면 현재 런 루프에서 타이머 제거를 요청한다. 따라서 타이머가 설치된 동일한 쓰레드에서 항상 [`invalidate()`](https://developer.apple.com/documentation/foundation/timer/1415405-invalidate) 메서드를 호출해야 한다. 타이머를 무효화하면 런 루프에 더 이상 영향을 미치지 않도록 타이머가 즉시 비활성화된다. 그런 다음 런 루프는 [`invalidate()`](https://developer.apple.com/documentation/foundation/timer/1415405-invalidate) 메서드가 반환되기 직전 또는 이후 어느 시점에 타이머\(그리고 타이머에 대한 강한 참조\)를 제거한다. 일단 무효화되면 타이머 객체는 재사용할 수 없다.

반복 타이머 작동 후, 지정된 [`tolerance`](https://developer.apple.com/documentation/foundation/timer/1415085-tolerance) 내에서 마지막 예약된 작동일 이후 타이머 간격의 정수 배수로 가장 가까운 미래 날짜에 다음 작동을 예약한다. 셀렉터 또는 호출에 걸리는 시간이 지정된 간격보다 길면 타이머는 다음 작동만 예약한다. 즉, 타이머는 지정된 선택기 또는 호출 중 발생한 누락된 발생을 보상하려고 시도하지 않는다.

#### Subclassing Notes <a id="1770465"></a>

[`Timer`](https://developer.apple.com/documentation/foundation/timer)를 서브클래싱하지 말아라.

### Topics

#### Creating a Timer

* [`class func scheduledTimer(withTimeInterval: TimeInterval, repeats: Bool, block: (Timer) -> Void) -> Timer`](https://developer.apple.com/documentation/foundation/timer/2091889-scheduledtimer) 타이머를 생성하고 기본 모드에서 현재 런 루프에서 타이머를 예약한다.
* [`class func scheduledTimer(timeInterval: TimeInterval, target: Any, selector: Selector, userInfo: Any?, repeats: Bool) -> Timer`](https://developer.apple.com/documentation/foundation/timer/1412416-scheduledtimer) 타이머를 생성하고 기본 모드에서 현재 런 루프에서 타이머를 예약한다.
* [`class func scheduledTimer(timeInterval: TimeInterval, invocation: NSInvocation, repeats: Bool) -> Timer`](https://developer.apple.com/documentation/foundation/timer/1415941-scheduledtimer) 새 타이머를 생성하고 기본 모드에서 현재 런 루프에서 타이머를 예약한다.
* [`init(timeInterval: TimeInterval, repeats: Bool, block: (Timer) -> Void)`](https://developer.apple.com/documentation/foundation/timer/2091888-init) 지정된 시간 간격 및 블록을 사용하여 타이머 객체를 초기화한다.
* [`init(timeInterval: TimeInterval, target: Any, selector: Selector, userInfo: Any?, repeats: Bool)`](https://developer.apple.com/documentation/foundation/timer/1408356-init) 지정된 객체 및 셀렉터로 타이머 객체를 초기화한다.
* [`init(timeInterval: TimeInterval, invocation: NSInvocation, repeats: Bool)`](https://developer.apple.com/documentation/foundation/timer/1407170-init) 지정된 호출 객체로 타이머 객체를 초기화한다.
* [`init(fire: Date, interval: TimeInterval, repeats: Bool, block: (Timer) -> Void)`](https://developer.apple.com/documentation/foundation/timer/2091887-init) 지정된 블록으로 지정된 날짜 및 시간 간격의 타이머를 초기화한다.
* [`init(fireAt: Date, interval: TimeInterval, target: Any, selector: Selector, userInfo: Any?, repeats: Bool)`](https://developer.apple.com/documentation/foundation/timer/1415700-init) 지정된 객체 및 셀렉터를 사용하여 타이머를 초기화한다.

#### Firing a Timer

* [`func fire()`](https://developer.apple.com/documentation/foundation/timer/1414035-fire) 타이머의 메시지가 타겟으로 전송되도록 한다.

#### Stopping a Timer

* [`func invalidate()`](https://developer.apple.com/documentation/foundation/timer/1415405-invalidate) 타이머가 다시 작동되는 것을 막고 런 루프에서 제거를 요청한다.

#### Retrieving Timer Information

* [`var isValid: Bool`](https://developer.apple.com/documentation/foundation/timer/1408249-isvalid) 타이머가 현재 유효한지 여부를 나타내는 불 값.
* [`var fireDate: Date`](https://developer.apple.com/documentation/foundation/timer/1407353-firedate) 타이머가 작동되는 날짜.
* [`var timeInterval: TimeInterval`](https://developer.apple.com/documentation/foundation/timer/1409024-timeinterval) 타이머의 시간 간격, 몇 초.
* [`var userInfo: Any?`](https://developer.apple.com/documentation/foundation/timer/1408911-userinfo) 수신기의 `userInfo` 객체.

#### Configuring Firing Tolerance

* [`var tolerance: TimeInterval`](https://developer.apple.com/documentation/foundation/timer/1415085-tolerance) 타이머가 작동될 수 있는 예약된 작동 날짜 이후의 시간.

#### Firing Messages as a Combine Publisher

* [`static func publish(every: TimeInterval, tolerance: TimeInterval?, on: RunLoop, in: RunLoop.Mode, options: RunLoop.SchedulerOptions?) -> Timer.TimerPublisher`](https://developer.apple.com/documentation/foundation/timer/3329589-publish) 지정된 간격에 대해 현재 날짜를 반복적으로 내보내는 게시자를 반환한다.
* [`class Timer.TimerPublisher`](https://developer.apple.com/documentation/foundation/timer/timerpublisher) 지정된 간격으로 현재 날짜를 반복적으로 내보내는 게시자.



