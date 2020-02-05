---
description: 객체가 자신의 기능 복사본을 제공하기 위해 채택하는 프로토콜.
---

# NSCopying

### 정의

```swift
protocol NSCopying
```

### 개요

"복사본"의 정확한 의미는 클래스에 따라 다를 수 있지만, 사본은 사본이 만들어질 당시 원본과 동일한 값을 가지는 기능적으로 독립된 객체여야 한다. `NSCopying`으로 제작한 사본은 발신자가 암묵적으로 리테인하고 있으며, 사본은 이를 릴리즈할 책임이 있다.

`NSCopying`은 하나의 메서드 [`copy()`](https://developer.apple.com/documentation/objectivec/nsobject/1418807-copy)를 정의하지만, 복사는 일반적으로 편의 메서드 [`copy()`](https://developer.apple.com/documentation/objectivec/nsobject/1418807-copy) 와 함께 호출된다. [`copy()`](https://developer.apple.com/documentation/objectivec/nsobject/1418807-copy) 메서드는 [`NSObject`](https://developer.apple.com/documentation/objectivec/nsobject)에서 상속되는 모든 객체에 대해 정의되며, 기본 구역으로 [`copy(with:)`](https://developer.apple.com/documentation/foundation/nscopying/1410311-copy)를 호출하기만 하면 된다.

이 프로토콜을 구현하기 위한 선택사항은 다음과 같다:

* [`copy(with:)`](https://developer.apple.com/documentation/foundation/nscopying/1410311-copy)를 상속하지 않는 클래스에서 [`alloc`](https://developer.apple.com/documentation/objectivec/nsobject/1571958-alloc) 및 init... 을 사용하여 `NSCopying`을 구현한다.
* `NSCopying` 동작이 상속될 때 슈퍼클래스의 [`copy(with:)`](https://developer.apple.com/documentation/foundation/nscopying/1410311-copy)를 호출하여 `NSCopying`을 구현하라. 슈퍼 클래스 구현에서 [`NSCopyObject`](https://developer.apple.com/documentation/foundation/1587928-nscopyobject) 함수를 사용할 수 있는 경우 리테인된 객체에 대해 인스턴스 변수를 포인터로 명시적으로 할당하라.
* 클래스와 클래스의 내용을 변경할 수 없을 때 새 복사본을 만드는 대신 원본으로 유지하여 `NSCopying`을 구현하라.

하위 클래스가 슈퍼 클래스에서 `NSCopying`을 상속하고 추가 인스턴스 변수를 선언하는 경우 하위 클래스는 자체 인스턴스 변수를 적절하게 처리하기 위해 [`copy(with:)`](https://developer.apple.com/documentation/foundation/nscopying/1410311-copy) 를 재정의해야 하며, 슈퍼 클래스의 구현을 먼저 호출해야 한다.

### Topics

#### Copying

[`func copy(with: NSZone?) -> Any`](https://developer.apple.com/documentation/foundation/nscopying/1410311-copy)

수신기의 복사본인 새 인스턴스를 반환한다.



