# Singleton

애플리케이션이 얼마나 많은 요청을 하더라도 싱글톤 클래스는 동일한 인스턴스를 반환한다. 일반적인 클래스는 호출자가 원하는 만큼의 클래스 인스턴스를 만들 수 있도록 허용하는 반면, 싱글톤 클래스의 경우 프로세스당 하나의 클래스 인스턴스만 있을 수 있다. 싱글톤 객체는 그 클래스의 자원에 대한 전역적인 접근 방법을 제공한다. 싱글톤은 일부 일반 서비스나 자원을 제공하는 클래스와 같이 단일 제어 지점이 바람직한 상황에서 사용된다.

![](file:///Users/BLU/TIL/iOS/Cocoa-Core-Competencies/Images/singleton_2x.png?lastModify=1572841250)

팩토리 메서드를 통해 싱글톤 클래스에서 글로벌 인스턴스를 얻는다. 클래스가 요청될 때 유일한 인스턴스를 lazy 생성하고 이후 다른 인스턴스가 생성되지 않도록 보장한다. 싱글톤 클래스는 호출자가 인스턴스를 복사, retain, release 하는 것을 방지한다. 당신은 필요한 자체 싱글톤 클래스를 만들 수 있다. 예를 들어, 애플리케이션에서 다른 객체에 소리를 제공하는 클래스가 있다면 싱글톤으로 만들 수 있다.

몇몇 Cocoa 프레임워크 클래스는 싱글톤이다. 여기에는 NSFileManager, NSWorkspace 및 UIKit에서 UIApplication 및 UIAccelerometer가 포함된다. 싱글톤 인스턴스를 반환하는 팩토리 메서드의 이름에는 관례적으로 shared 클래스 타입이 있다. Cocoa 프레임워크의 예로는 sharedFileManager, sharedColorPanel, sharedWorkspace가 있다.

#### Prerequisite Articles

Object creation

#### Related Articles

Object copying  
Memory management

#### Definitive Discussion

\(None\)

