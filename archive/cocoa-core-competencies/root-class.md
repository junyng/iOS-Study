# Root class

루트 클래스는 다른 클래스에서 상속되지 않으며, 해당 클래스 아래의 모든 객체에 공통적인 인터페이스와 동작을 정의한다. 해당 계층의 모든 객체가 궁극적으로 루트 클래스에서 상속된다. 루트 클래스를 베이스 클래스라고 부르기도 한다.

모든 Objective-C 클래스의 루트 클래스는 Foundation framework의 일부인 NSObject이다. Cocoa 또는 Cocoa Touch 애플리케이션의 모든 객체는 궁극적으로 NSObject로부터 상속된다. 이 클래스는 다른 클래스가 Objective-C 런타임과 상호 작용하는 기본 접근 지점이다. 또한 기본 객체 인터페이스를 선언하고 인트로스펙션, 메모리 관리, 메서드 호출 등 기본 객체 동작을 구현한다. Cocoa와 Cocoa Touch 객체는 루트 클래스로부터 객체로의 행동 능력을 얻는다.

![](file:///Users/BLU/TIL/iOS/Cocoa-Core-Competencies/Images/root_class_2x.png?lastModify=1572841236)

> **Note**: Foundation 프레임워크는 NSProxy라는 또 다른 루트 클래스를 정의하지만, 이 클래스는 Cocoa 애플리케이션에서 거의 사용되지 않으며 Cocoa Touch 애플리케이션에서는 절대 사용되지 않는다.

루트 클래스 NSObject는 프로그램 인터페이스에 기여하는 NSObject라고도 하는 프로토콜을 채택한다. 프로토콜은 루트 클래스에 필요한 기본 프로그램 인터페이스를 지정한다.

#### Prerequisite Articles

Cocoa \(Touch\)

#### Related Articles

Introspection  
Memory management

#### Definitive Discussion

Programming with Objective-C

