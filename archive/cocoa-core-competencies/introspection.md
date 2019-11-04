# Introspection

Introspection은 객체의 본질적 특성을 런타임에 드러낼 수 있는 고유 능력을 말한다. 객체에 특정 메시지를 전송하여 객체로서의 자신에 대한 질문을 할 수 있으며 Objective-C 런타임이 답을 제공한다. Introspection은 중요한 코딩 도구이기 때문에 프로그램을 효율적이고 튼튼하게 해준다. Introspection이 얼마나 유용한지 보여주는 몇 가지 예는 다음과 같다.

* 예를 들어 응답할 수 없는 객체에 메시지를 보낼 때 발생하는 예외 등의 문제를 방지하기 위해 런타임 검사로 Introspection 메서드를 호출할 수 있다.
* Introspection을 사용하여 상속 계층에서 객체를 찾을 수 있으며, 이 계층은 객체의 기능에 대한 정보를 제공할 수 있다.

![](file:///Users/BLU/TIL/iOS/Cocoa-Core-Competencies/Images/introspection_2x.png?lastModify=1572840890)

### Types of Introspection Information

`NSObject`클래스에서 채택된 `NSObject` 프로토콜은 객체에 대해 다음과 같은 종류의 정보를 제공하는 Introspection 메서드를 정의한다.

* **클래스 멤버쉽**. 객체가 특정 클래스에서 직접 또는 간접적으로 상속되는지 여부를 확인하려면 `isKindOfClass:` 메시지를 보내고 결과를 평가하라. 이 메서드는 객체가 지정된 클래스의 직접 인스턴스인지 여부를 알려준다. 또한 클래스나 슈퍼 클래스 메서드를 사용하여 객체의 클래스나 슈퍼클래스를 획득한 다음 그 결과를 비교 연산으로 사용할 수 있다.
* **메시지가 응답함**. 객체의 클래스 또는 슈퍼 클래스가 메서드를 구현하는지 확인하려면 객체에 `respondsToSelector:`메시지를 보내라. 파라미터는 @selector 지시어를 사용하여 메서드의 서명으로 부터 생성된 SEL 타입 값이다.

  ```text
  BOOL doesRespond = [anObject respondsToSelector:@selector(writeToFile:atomically:)];
  ```

* **프로토콜 준수**. 클래스가 공식 프로토콜을 준수하는 경우, 클래스가 해당 프로토콜의 필요한 메서드를 구현하고 그에 따라 메시지를 보낼 것으로 예상할 수 있다. 이 정보를 얻으려면 `conformsToProtocol:`메서드를 사용하라. @protocol 지시어를 사용하여 이 메서드의 인수를 지정한다.

#### Prerequisite Articles

[Message](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59-SW1)

**Related Articles**

[Object comparison](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/ObjectComparison.html#//apple_ref/doc/uid/TP40008195-CH37-SW1)  
[Protocol](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45-SW1)  
[Selector](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html#//apple_ref/doc/uid/TP40008195-CH48-SW1)

**Definitive Discussion**

_NSObject Protocol Reference_

