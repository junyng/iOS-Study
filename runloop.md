---
description: 입력 소스를 관리하는 객체에 대한 프로그래밍 방식 인터페이스.
---

# RunLoop

### Declaration

```swift
class RunLoop : NSObject
```

### Overview

`RunLoop` 객체는 윈도우 시스템, [`Port`](https://developer.apple.com/documentation/foundation/port)객체 및 [`NSConnection`](https://developer.apple.com/documentation/foundation/nsconnection) 객체의 마우스 및 키보드 이벤트와 같은 소스에 대한 입력을 처리한다. `RunLoop` 객체는 [`Timer`](https://developer.apple.com/documentation/foundation/timer) 이벤트도 처리한다.

애플리케이션은 `RunLoop` 객체를 생성하거나 명시적으로 관리하지 않는다. 애플리케이션의 메인 쓰레드를 포함한 각 [`Thread`](https://developer.apple.com/documentation/foundation/thread) 객체에는 필요에 따라 RunLoop 객체가 자동으로 생성된다. 현재 쓰레드의 런 루프에 접근해야 하는 경우 클래스 메서드 [`current`](https://developer.apple.com/documentation/foundation/runloop/1412291-current)에 접근하라.

`RunLoop`의 관점에서 [`Timer`](https://developer.apple.com/documentation/foundation/timer) 객체는 "입력"되지 않는다는 점에 유의하라 - 그들은 특별한 타입이며, 그들이 시작할 때 런루프가 되돌아오게 하지 않는다는 것을 의미한다.

> **경고**
>
> RunLoop 클래스는 일반적으로 쓰레드에 안전하게 간주되지 않으며 그 메서드는 현재 쓰레드의 컨텍스트 내에서만 호출되어야 한다. 다른 쓰레드로 실행되는 RunLoop 객체의 메서드를 호출하려고 해서는 안된다. 그렇게 하면 예상치 못한 결과가 발생할 수 있다.



