# Key-value observing

Key-value observing은 다른 객체의 프로퍼티가 변경될 때 객체가 직접 통지할 수 있도록 하는 메커니즘이다. Key-value observing \(KVO\)는 애플리케이션의 응집성에 중요한 요소가 될 수 있다. 그것은 모델-뷰-컨트롤러 설계 패턴에 따라 설계된 애플리케이션의 객체간의 의사소통의 모드이다. 예를 들어, 뷰 및 컨트롤러 계층의 객체와 모델 객체의 상태를 동기화하는 데 사용할 수 있다. 일반적으로 컨트롤러 객체는 모델 객체를 관찰하고 뷰는 컨트롤러 객체 또는 모델 객체를 관찰한다.

> **Note**: UIKit 프레임워크의 클래스는 일반적으로 KVO를 지원하지 않지만, 당신은 여전히 그것을 사용자 정의 뷰를 포함하여 애플리케이션의 사용자 지정 객체에서 구현할 수 있다.

![](file:///Users/BLU/TIL/iOS/Cocoa-Core-Competencies/Images/kvo_2x.png?lastModify=1572840924)

KVO를 사용하면 한 객체는 단순한 속성, 일대일 관계 그리고 일대다 관계를 포함한 다른 객체의 프로퍼티를 관찰할 수 있다. 객체는 프로퍼티의 현재 및 이전 값이 무엇인지 알아낼 수 있다. 일대다 관계 관찰자들은 만들어진 변화 유형 뿐만아니라, 어떤 객체가 그 변화에 관여하는지도 알게 된다.

노티피케이션 메커니즘으로서 key-value observing은 `NSNotification` 및 `NSNotificationCenter` 클래스에서 제공하는 메커니즘과 유사하지만 상당한 차이도 있다. KVO 노티피케이션은 관찰자로 등록된 모든 객체에 알림을 브로드캐스트하는 중앙 객체 대신 프로퍼티 값의 변경이 발생했을 때 관찰 객체로 직접 이동한다.

### Implementing KVO

루트 클래스 NSObject는 사용자가 거의 재정의할 필요가 없는 key-value observing의 기본 구현을 제공한다. 따라서 모든 Cocoa 객체는 본질적으로 key-value observing이 가능하다. 프로퍼티에 대한 KVO 노티피케이션을 받으려면 다음 작업을 수행해야 한다.

* 관찰된 클래스의 관찰하려는 프로퍼티에 대해 key-value observing 준수 여부를 확인해야 한다. KVO 준수는 관찰된 객체 클래스도 KVC 준수 및 프로퍼티에 대한 자동 관찰자 알림을 허용하거나 프로퍼티에 대한 수동 key-value observing을 구현할 것을 요구한다.
* 값이 변경될 수 있는 객체의 관찰자를 추가한다. `addObserver:forKeyPath:options:context:`를 호출하여 이 작업을 수행하라. 관찰자는 당신의 애플리케이션에 있는 또 다른 객체일 뿐이다.
* 관찰자 객체에서 `observeValueForKeyPath:ofObject:change:context:`메서드를 구현하라. 이 메서드는 관찰된 객체의 프로퍼티 값이 변경될 때 호출된다.

### KVO Is an Integral Part of Bindings \(OS X\)

Cocoa 바인딩은 "glue code"를 많이 쓰지 않고도 모델의 값을 유지하고 애플리케이션의 레이어를 동기화된 상태로 볼 수 있도록하는 OS X 기술이다. Interface Builder Inspector를 통해 뷰의 프로퍼티와 데이터 조각 사이에 중재하는 연결을 설정할 수 있으며, 하나의 변경사항이 다른 것에 반영되도록 뷰를 "바인딩" 할 수 있다. KVO는 key-value coding 및 key-value binding과 함께 Cocoa 바인딩에 중요한 기술이다.

#### Prerequisite Articles

* Key-value coding

#### Related Articles

* Model-View-Controller
* Dynamic binding

#### Definitive Discussion

* Key-Value Observing Programming Guide

#### Sample Code Projects

* SourceView: Using NSOutlineView with NSTreeController
* BindingsJoystick

