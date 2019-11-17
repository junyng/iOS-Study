# Cocoa \(Touch\)

Cocoa와 Cocoa Touch는 각각 OS X와 iOS용 애플리케이션 개발 환경이다. Cocoa 및 Cocoa 터치 모두 Objective-C 런타임 및 2가지 핵심 프레임워크를 포함한다.

* _Cocoa_는 Foundation과 AppKit 프레임워크를 포함하여 OS X에서 실행되는 애플리케이션 개발을 위해 사용된다.
* _Cocoa Touch_는 Foundation과 UIKit 프레임워크를 포함하여 iOS에서 실행되는 애플리케이션 개발을 위해 사용된다.

> **Note**: "코코아"라는 용어는 Objective-C 런타임에 기초하고 루트 클래스 `NSObject` 로부터 상속되는 모든 클래스 또는 객체를 일반적으로 지칭하기 위해 사용되었다. "코코아" 혹은 "코코아 터치"는 각 플랫폼의 프로그램 인터페이스를 사용한 애플리케이션 개발을 언급할 때도 사용된다.

### The Frameworks

Foundation 프레임워크는 기본적인 객체 행동을 정의하는 루트 클래스 `NSObject`를 구현한다. 그것은 기본 타입\(문자열, 숫자\)과 컬렉션\(배열, 딕셔너리\) 클래스를 구현한다. Foundation은 또한 국제화, 객체 지속성, 파일 관리, XML 프로세싱 기능을 제공한다. 이 클래스를 사용하여 포트, 스레드, 락, 프로세스와 같은 기본 시스템 엔티티 및 서비스를 접근할 수 있다. Foundation은 Core Foundation 프레임워크를 기반으로 하며, 절차적\(ANSI C\) 인터페이스를 발행한다.

애플리케이션의 사용자 인터페이스를 개발하기 위해 AppKit 및 UIKit 프레임워크를 사용하라. 이 두 프레임워크는 목적이 같지만 플랫폼에 특정되어 있다. 여기에는 이벤트 처리, 그리기, 이미지 처리, 텍스트 처리, 타이포그래피, 애플리케이션 간 데이터 전송을 위한 클래스가 포함한다. 또한 테이블, 슬라이더, 버튼, 텍스트 필드, 경고창과 같은 사용자 인터페이스 구성 요소를 포함한다.

### The Language

Objective-C는 코코아 및 코코아 터치 애플리케이션 개발을 위한 네이티브 언어이다. 단, 코코아 및 코코아 터치 애플리케이션용 프로젝트에는 C++ 및 ANSI C 코드가 포함될 수 있다. 또한 PyObjC와 RubyCocoa와 같은 Objective-C 런타임에 연결된 스크립팅 언어를 사용하여 코코 애플리케이션을 개발할 수 있다.

#### Prerequisite Articles

\(None\)

#### Related Articles

[Root class](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/RootClass.html#//apple_ref/doc/uid/TP40008195-CH46-SW1)  
[Objective-C](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/ObjectiveC.html#//apple_ref/doc/uid/TP40008195-CH43-SW1)

#### Definitive Discussion

\(None\)

