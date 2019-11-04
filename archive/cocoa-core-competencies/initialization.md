# Initialization

이니셜라이즈는 상태를 합리적인 초기값으로 설정하여 새로 할당된 객체를 사용할 수 있게 하는 객체 생성의 단계다. 이니셜라이즈는 항상 할당 직후 발생해야 한다. 이 작업은 항상 새로 할당된 객체에 대해 호출하는 이니셜라이즈 메서드\(또는 단순히 이니셜라이저\)에 의해 수행도니다. 이니셜라이저는 객체를 유용한 상태로 만드는 리소스 로드 및 힙 메모리 할당과 같은 다른 설정 작업도 수행할 수 있다.

### The Form of an Initializer Declaration

컨벤션에 따라 이니셜라이저의 이름은 항상 `init`으로 시작된다. 그것은 동적으로 입력된 객체\(`id`\)를 반환한다. 초기화가 성공하지 못하면 `nil`을 반환한다. 이니셜라이저는 초기 값을 지정하는 하나 이상의 파라미터를 포함할 수 있다.

`NSString` 클래스의 이니셜라이즈 선언 예제이다.

```text
- (id)initWithData:(NSData *)data encoding:(NSStringEncoding)encoding
```

### Implementing an Initializer

클래스는 일반적으로 객체에 대해 이니셜라이저를 구현하지만, 다음 작업을 수행할 필요는 없다. 클래스가 이니셜라이저를 구현하지 않으면, Cocoa는 클래스의 가장 가까운 조상의 이니셜라이저를 호출한다. 그러나 하위 클래스는 종종 자신의 이니셜라이저를 정의하거나, 클래스별 이니셜라이저를 추가하기 위해 슈퍼 클래스의 이니셜라이저를 오버라이드한다. 클래스가 이니셜라이저를 구현하는 경우, 첫 번째 단계로 슈퍼클래스의 이니셜라이저를 호출해야 한다. 이 요구 사항은 루트 객체로부터 시작하여 상속 체인 아래의 객체에 대한 일련의 초기화를 보장한다. `NSObject` 클래스는 초기화 메서드를 기본 객체 이니셜라이저로 선언하므로 항상 마지막에 호출되지만 먼저 리턴된다.

이니셜라이저 메서드를 구현하기 위한 기본 단계는 다음과 같다.

1. **슈퍼클래스 이니셜라이저를 호출하고 반환되는 값을 확인하라.** \(예약어 `super`를 사용하여 슈퍼클래스를 지정하라.\) 값이 `nil`이 아닌 경우 슈퍼클래스 이니셜라이저가 유효한 객체를 반환하므로 기화를 진행할 수 있다.
2. **객체의 인스턴스 변수에 값을 할당하라.** 메모리 관리 코드에서 이러한 값이 객체 자체인 경우, 필요에 따라 복사하거나 보관하라.
3. **초기화된 객체를 반환**하거나 초기화가 성공하지 않았다면 `nil`을 반환하라.

다음 단계에 따라 날짜 인스턴스 변수를 현재 날짜로 초기화하는 간단한 이니셜라이저이다.

```text
- (id)init {
    if (self = [super init]) { // equivalent to "self does not equal nil"
        date = [[NSDate date] retain];
    }
    return self;
}
```

한 클래스에 여러 이니셜라이저가 있을 수 있다. 이는 초기화 데이터가 다양한 형태를 취할 수 있거나 편의상 특정 이니셜라이저가 기본 값을 제공하는 경우에 발생한다. 이 경우 이니셜라이저 방법 중 하나로 초기화 파라미터의 완전한 보완을 필요로 하는 지정된 이니셜라이저라고 한다.

이 코드에서는 슈퍼클래스가 `nil`을 리턴할 때 메서드는 초기화를 건너뛰고 그 값을 호출자에게 반환한다.



#### Prerequisite Articles

[Object creation](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39-SW1)  
[Message](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59-SW1)

**Related Articles**

[Multiple initializers](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/MultipleInitializers.html#//apple_ref/doc/uid/TP40008195-CH33-SW1)  
[Object copying](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/ObjectCopying.html#//apple_ref/doc/uid/TP40008195-CH38-SW1)  
[Memory management](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/MemoryManagement.html#//apple_ref/doc/uid/TP40008195-CH27-SW1)

**Definitive Discussion**

[Objects Are Created Dynamically](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW7)

