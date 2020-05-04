---
description: 입력 소스를 관리하는 객체에 대한 프로그램상의 인터페이스.
---

# RunLoop

### Declaration

```swift
class RunLoop : NSObject
```

### Overview

`RunLoop` 객체는 윈도우 시스템, [`Port`](https://developer.apple.com/documentation/foundation/port) 객체 및 [`NSConnection`](https://developer.apple.com/documentation/foundation/nsconnection) 객체의 마우스 및 키보드 이벤트와 같은 소스에 대한 입력을 처리한다. `RunLoop` 객체는 또한 [`Timer`](https://developer.apple.com/documentation/foundation/timer) 이벤트를 처리한다.

애플리케이션이 `RunLoop` 객체를 생성하거나 명시적으로 관리하지 않는 경우, 애플리케이션의 메인 쓰레드를 포함한 각 쓰레드 객체에는 필요에 따라 `RunLoop` 객체가 자동으로 생성된다. 현재 쓰레드의 런 루프에 접근해야 하는 경우 클래스 메서드 [`current`](https://developer.apple.com/documentation/foundation/runloop/1412291-current)를 사용하라.

`RunLoop`의 관점에서 [`Timer`](https://developer.apple.com/documentation/foundation/timer) 객체는 "입력"이 아니다. 그것들은 특별한 유형이며, 그 중 하나는 작동시 런 루프가 되돌아오게 하지 않는다는 것을 의미한다.

> **경고**
>
> `RunLoop` 클래스는 일반적으로 쓰레드 세이프로 간주되지 않으며, 그 메서드는 현재 쓰레드의 컨텍스트 내에서만 호출되어야 한다. 다른 쓰레드에서 실행되는 `RunLoop` 객체의 메서드를 호출하지 않아야 한다. 그렇지 않으면 예기치 않은 결과가 발생할 수 있다.



### Topics

#### Accessing Run Loops and Modes

* [`class var current: RunLoop`](https://developer.apple.com/documentation/foundation/runloop/1412291-current) 현재 쓰레드의 런 루프를 반환한다.
* [`var currentMode: RunLoop.Mode?`](https://developer.apple.com/documentation/foundation/runloop/1412652-currentmode) 수신기의 현재 입력 모드.
* [`func limitDate(forMode: RunLoop.Mode) -> Date?`](https://developer.apple.com/documentation/foundation/runloop/1412837-limitdate) 지정된 모드에서 런 루프를 통과하는 한 번의 통과를 수행하고 다음 타이머가 작동하도록 예약된 데이트를 반환한다.
* [`class var main: RunLoop`](https://developer.apple.com/documentation/foundation/runloop/1418388-main) 메인 쓰레드의 런 루프를 반환한다.
* [`func getCFRunLoop() -> CFRunLoop`](https://developer.apple.com/documentation/foundation/runloop/1410140-getcfrunloop)  수신기의 기본 [`CFRunLoop`](https://developer.apple.com/documentation/corefoundation/cfrunloop-rht) 객체를 반환한다.
* [`struct RunLoop.Mode`](https://developer.apple.com/documentation/foundation/runloop/mode)

#### Managing Timers

* [`func add(Timer, forMode: RunLoop.Mode)`](https://developer.apple.com/documentation/foundation/runloop/1418468-add) 주어진 입력 모드로 지정된 타이머를 등록한다.

#### Managing Ports

* [`func add(Port, forMode: RunLoop.Mode)`](https://developer.apple.com/documentation/foundation/runloop/1417637-add) 런 루프의 지정된 모드에 입력 소스로 포트를 추가한다.
* [`func remove(Port, forMode: RunLoop.Mode)`](https://developer.apple.com/documentation/foundation/runloop/1414332-remove) 런 루프의 지정된 입력 모드에서 포트를 제거한다.

#### Running a Loop

* [`func run()`](https://developer.apple.com/documentation/foundation/runloop/1412430-run) 수신기를 영구 루프에 넣고, 이 기간 동안 모든 부착된 입력 소스의 데이터를 처리한다.
* [`func run(mode: RunLoop.Mode, before: Date) -> Bool`](https://developer.apple.com/documentation/foundation/runloop/1411525-run) 루프를 한 번 실행하여 지정된 날짜까지 지정된 모드에서 입력을 차단하라.
* [`func run(until: Date)`](https://developer.apple.com/documentation/foundation/runloop/1415778-run) 지정된 날짜까지 루프를 실행하며, 이 기간 동안 연결된 모든 입력 소스의 데이터를 처리한다.
* [`func acceptInput(forMode: RunLoop.Mode, before: Date)`](https://developer.apple.com/documentation/foundation/runloop/1417143-acceptinput) 루프를 한 번 또는 지정된 날짜까지 실행하고, 지정된 모드에 대해서만 입력을 허용한다.

#### Scheduling and Canceling Messages

* [`func perform(Selector, target: Any, argument: Any?, order: Int, modes: [RunLoop.Mode])`](https://developer.apple.com/documentation/foundation/runloop/1409310-perform) 수신기의 메시지 전송을 예약한다.
* [`func cancelPerform(Selector, target: Any, argument: Any?)`](https://developer.apple.com/documentation/foundation/runloop/1418077-cancelperform) 이전에 예약된 메시지의 전송을 취소한다.
* [`func cancelPerformSelectors(withTarget: Any)`](https://developer.apple.com/documentation/foundation/runloop/1414208-cancelperformselectors) 지정된 대상에 예약된 모든 미해결된 명령을 수행 취소한다.

#### Scheduling Combine Publishers

* [`func schedule(options: RunLoop.SchedulerOptions?, () -> Void)`](https://developer.apple.com/documentation/foundation/runloop/3329474-schedule) 스케줄러의 최소 허용오차를 사용하여 지정된 날짜 이후 특정 시간에 작업을 수행한다.
* [`func schedule(after: RunLoop.SchedulerTimeType, tolerance: RunLoop.SchedulerTimeType.Stride, options: RunLoop.SchedulerOptions?, () -> Void)`](https://developer.apple.com/documentation/foundation/runloop/3329473-schedule) 지정된 날짜 이후 지정된 허용오차 및 옵션을 사용하여 작업을 수행한다.
* [`func schedule(after: RunLoop.SchedulerTimeType, interval: RunLoop.SchedulerTimeType.Stride, tolerance: RunLoop.SchedulerTimeType.Stride, options: RunLoop.SchedulerOptions?, () -> Void) -> Cancellable`](https://developer.apple.com/documentation/foundation/runloop/3329472-schedule) 지정된 허용 오차 및 옵션을 사용하여 지정된 날짜 이후의 일부 시간에 지정된 빈도에서 작업을 수행한다.
* [`var minimumTolerance: RunLoop.SchedulerTimeType.Stride`](https://developer.apple.com/documentation/foundation/runloop/3329470-minimumtolerance) 런 루프 스케줄러가 허용하는 최소 허용 오차.
* [`var now: RunLoop.SchedulerTimeType`](https://developer.apple.com/documentation/foundation/runloop/3329471-now) 런 루프 스케줄러의 현재 시간 정의.
* [`struct RunLoop.SchedulerTimeType`](https://developer.apple.com/documentation/foundation/runloop/schedulertimetype) 런 루프가 사용하는 스케줄러 시간 타입.
* [`struct RunLoop.SchedulerOptions`](https://developer.apple.com/documentation/foundation/runloop/scheduleroptions) 런 루프 스케줄러의 작동에 영향을 미치는 일련의 옵션.

#### Constants

[Run Loop Modes](https://developer.apple.com/documentation/foundation/runloop/run_loop_modes)

`NSRunLoop`는 다음과 같은 런 루프 모드를 정의한다.

#### Instance Methods

* [`func perform(() -> Void)`](https://developer.apple.com/documentation/foundation/runloop/2091881-perform)
* [`func perform(inModes: [RunLoop.Mode], block: () -> Void)`](https://developer.apple.com/documentation/foundation/runloop/2091880-perform)

