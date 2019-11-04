# Multiple initializers

클래스는 다른 형태의 입력을 취할 수 있거나 클라이언트의 편의를 위해 기본 초기화 값을 제공할 수 있는 복수의 이니셜라이저 메서드를 정의할 수 있다. 클래스가 다른 형태 또는 다른 소스에서 초기화 데이터를 가져올 수 있는 경우, 각 폼 또는 소스에 대해 이니셜라이저를 선언할 수 있다. 예를 들어 `NSString` 클래스는 유니코드 문자열 배열 또는 URL 리소스에서 문자열 데이터를 얻기 위한 이니셜라이저를 가지고 있다. \(다른 것들 중에\)

다수의 이니셜라이저는 프로그래밍의 편리함이 될 수 있다. 클래스는 한 쪽 끝에는 클라이언트가 모든 초기 값을 지정하고 다른 쪽 끝에는 이러한 값의 대부분 또는 전부를 기본값으로 제공할 수 있는 일련의 이니셜라이저를 가질 수 있다. 클래스의 클라이언트들은 나중에 접근자 메서드 또는 프로퍼티를 통해 기본 값에 대한 새로운 값을 대체할 수 있다.

프레임워크 클래스는 가끔 여러개의 팩토리 메서드뿐만 아니라 여러개의 이니셜라이저를 가지고 있다. 그것들은 편리하거나 다른 형태로 초기화 데이터를 취할 수 있다는 점에서 유사하다. 그러나 반환된 객체를 초기화할 뿐 아니라 할당한다.

![](file:///Users/BLU/TIL/iOS/Cocoa-Core-Competencies/Images/multiple_initializers_2x.png?lastModify=1572841049)

### The Designated Initializer

초기화 매개 변수의 전체 보완을 수행하는 클래스의 이니셜라이저는 보통 지정된 이니셜라이저이다. 하위 클래스의 지정된 이니셜라이저는 슈퍼 클래스의 메시지를 전송하여 슈퍼 클래스의 지정된 이니셜라이저를 호출해야 한다. `init`을 포함할 수 있는 편의 이니셜라이저\(또는 2차\)는 `super`를 호출하지 않는다. 대신 그들은 \(자체에게 메시지를 통해\) 다음 가장 많은 파라미터를 가진 시리즈에서 이니셜라이저를 호출하여 전달되지 않은 파라미터에 대한 기본값을 제공한다. 이 시리즈의 최종 이니셜라이저는 지정된 이니셜라이저이다.

#### Prerequisite Articles

[Object creation](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39-SW1)  
[Initialization](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Initialization.html#//apple_ref/doc/uid/TP40008195-CH21-SW1)

**Related Articles**

[Declared property](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/DeclaredProperty.html#//apple_ref/doc/uid/TP40008195-CH13-SW1)

**Definitive Discussion**

[Objects Are Created Dynamically](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW7)

