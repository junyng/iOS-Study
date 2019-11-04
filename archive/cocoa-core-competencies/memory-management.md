# Memory management

메모리 관리는 객체의 생명주기를 관리하고 더 이상 필요하지 않을 때 해제하는 프로그래밍 규율이다. 객체 메모리를 관리하는 것은 성능의 문제이다. 애플리케이션이 불필요한 객체를 해제하지 못하면, 메모리 설치 공간이 증가하고 성능이 저하된다. Cocoa 애플리케이션의 메모리 관리는 가비지 컬렉션을 사용하지 않는 참조 카운트 모델에 기반한다. 객체를 생성하거나 복사할 때 해당 객체의 리테인 카운트는 1이다. 그 후 다른 객체가 당신의 객체에 대한 소유지분을 나타낼 수 있으며, 리테인 카운트를 증가시킨다. 또한 객체의 소유주들은 객체의 소유지분을 포기하여 리테인 카운트를 감소시킬 수 있다. 리테인 카운트가 0이 되면 객체가 할당해제된다.

메모리 관리를 지원하기 위해 Objective-C는 규칙 집합을 준수하여 사용해야 하는 메서드와 메커니즘을 제공한다.

> **Note**: OS X에서 메모리를 명시적으로 관리하거나 Objective-C의 가비지 컬렉션 기능을 사용할 수 있다.

![](file:///Users/BLU/TIL/iOS/Cocoa-Core-Competencies/Images/memory_management_2x.png?lastModify=1572840933)

### Memory-Management Rules

소유 정책이라고도 하는 메모리 관리 규칙은 Objective-C 코드로 메모리를 명시적으로 관리하는 데 도움이 된다.

* 메모리를 할당하거나 복사하여 생성한 객체를 소유한다. 관련 메서드: `alloc`, `allocWithZone:`, `copy`, `copyWithZone:`, `mutableCopy`, `mutableCopyWithZone:`
* 만약 당신이 어떤 객체의 창조자가 아니라, 당신이 사용할 수 있도록 그 객체가 메모리에 남아있기를 원한다면, 당신은 그 객체에 대한 소유를 표현할 수 있다. 관련 메서드: `retain`
* 객체를 만들거나 소유를 표현함으로써 객체를 소유한다면, 더 이상 필요하지 않을 때 객체를 해제할 책임이 있다. 관련 메서드: `release`, `autorelease`

  반대로, 당신이 어떤 객체의 창조자가 아니고 소유를 표현하지 않았다면, 당신은 그것을 해제해서는 안된다.

  만약 당신이 당신의 프로그램의 다른 곳으로부터 어떤 객체를 받는다면, 그것은 일반적으로 그것이 수신된 메서드가 함수 안에서 유효하게 유지될 것을 보증한다. 해당 범위를 벗어나 유효하게 유지하려면 리테인하거나 복사하라. 만약 이미 할당 해제된 객체에 릴리스를 시도한다면, 프로그램은 충돌이 난다.

  **Aspects of Memory Management**

  다음 개념은 객체 메모리를 이해하고 적절하게 관리하는 데 필수적이다.

  * **자동 해제 풀** 객체에 `autorelease`를 보내면 객체가 나중에 릴리스될 수 있도록 표시되며, 릴리스된 객체가 현재 범위를 넘어 유지되도록 하려면 유용하다. 객체를 자동 해제하면 임의 프로그램 범위에 대해 생성된 자동 해제 풀\(`NSAutoreleasePool`의 인스턴스\)에 저장된다. 프로그램 실행이 해당 범위를 벗어나면 풀의 객체가 해제된다.
  * **할당 해제**. 객체의 리테인 카운트가 0으로 떨어질 때, 런타임은 객체가 객체를 파괴하기 직전에 객체의 클래스 `dealloc` 메서드를 호출한다. 클래스는 해당 인스턴스 변수로 가리킨 객체를 포함하여 객체가 보유한 모든 리소스를 확보하기 위해 이 메서드를 구현한다.
  * **팩토리 메서드**. 많은 프레임워크 클래스는 여러분을 위한 클래스의 객체를 만드는 클래스 메서드를 정의한다. 이러한 리턴된 객체는 수신 메서드의 범위를 넘어 유효하다고 보장되지 않는다.

\*\*\*\*

**Prerequisite Articles**

[Object creation](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39-SW1)  
[Object copying](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/ObjectCopying.html#//apple_ref/doc/uid/TP40008195-CH38-SW1)

**Related Articles**

[Object life cycle](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/ObjectLifeCycle.html#//apple_ref/doc/uid/TP40008195-CH55-SW1)

**Definitive Discussion**

_Advanced Memory Management Programming Guide_



