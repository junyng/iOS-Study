# Method overriding

메서드 오버라이딩은 클래스가 메서드 구현을 제공할 수 있는 언어기능이다. 그것은 이미 부모 클래스 중 하나에 의해 제공되고 있다. 이 클래스의 구현은 상위 클래스의 구현을 대체\(즉, 오버라이드\)한다.

상위 클래스의 이름과 동일한 이름을 가진 메서드를 정의할 때, 그 새로운 메서드는 상속된 정의를 대체한다. 새로운 메서드는 리턴 타입이 동일해야 하며 오버라이드 메서드와 동일한 수의 매개 변수를 사용해야 한다. 다음은 예시이다.

```text
@interface MyClass : NSObject {
}
- (int)myNumber;
@end
 
@implementation MyClass : NSObject {
}
- (int)myNumber {
    return 1;
}
@end
 
@interface MySubclass : MyClass {
}
- (int)myNumber;
@end
 
@implementation MySubclass
- (int)myNumber {
    return 2;
}
@end
```

메서드 정의 내에서 `super`는 현재 객체의 상위 클래스\(superclass\)를 가리킨다. 슈퍼 클래스의 메서드 구현을 실행하기 위해 `super`에게 메시지를 전송한다. 당신은 종종 그것을 기능적으로 확장하기 위해 같은 메서드의 새로운 구현 안에서 이것을 한다. 다음 예제에서 myNumber의 MySubclass에 의한 구현은 MyClass의 구현에 의해 반환되는 모든 값에 단순히 1을 더한다.

```text
@implementation MySubclass
- (int)myNumber {
    int subclassNumber = [super myNumber] + 1;
    return subclassNumber;
}
@end
```

#### Prerequisite Articles

\(None\)

#### Related Articles

\(None\)

#### Definitive Discussion

[Objects Can Call Methods Implemented by Their Superclasses](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW11)



