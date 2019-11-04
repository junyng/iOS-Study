# Cocoa \(Touch\)

Cocoa와 Cocoa Touch는 각각 OS X와 iOS용 애플리케이션 개발 환경이다. Cocoa 및 Cocoa 터치 모두 Objective-C 런타임 및 2가지 핵심 프레임워크를 포함한다.

* Cocoa는 Foundation과 AppKit 프레임워크를 포함하여 OS X에서 실행되는 애플리케이션 개발을 위해 사용된다.
* Cocoa Touch는 Foundation과 UIKit 프레임워크를 포함하여 iOS에서 실행되는 애플리케이션 개발을 위해 사용된다.

> **Note**: "Cocoa" 용어는 Objective-C 런타임 및 루트 클래스 NSObject로 상속을 기반으로한 any 클래스 또는 객체를 일반적으로 지칭하는 데 사용되어 왔다. "Cocoa" 혹은 "Cocoa Touch"는 각 플랫폼의 프로그램 인터페이스를 사용한 애플리케이션 개발을 언급할 때도 사용된다.

### The Frameworks

Foundation 프레임워크는 기본적인 객체 행동을 정의하는 루트 클래스 NSObject를 구현한다. 그것은 기본 타입\(문자열, 숫자\)과 컬렉션\(배열, 딕셔너리\) 클래스를 구현한다. Foundation은 또한 국제화, 객체 지속성, 파일 관리, XML 프로세싱 기능을 제공한다. 포트, 스레드, 락, 프로세스와 같은 기본 시스템 엔티티 및 서비스를 접근할 클래스를 사용할 수 있다. Foundation은 절차적인 \(ANSI C\) 인터페이스를 발행하는 Core Foundation 프레임워크에 기반한다.

애플리케이션 유저 인터페이스를 개발하기 위해 AppKit과 UIKit 프레임워크를 사용한다. 이 두 프레임워크는 목적이 같지만 플랫폼에 특정되어 있다. 이들은 이벤트 처리, 그리기, 이미지 처리, 텍스트 처리, 타이포그래피, 애플리케이션 내 데이터 전송에 대한 클래스를 포함한다. 또한 테이블, 슬라이더, 버튼, 텍스트 필드, 경고창과 같은 사용자 인터페이스 구성 요소를 포함한다.

### The Language

Objective-C는 Cocoa 및 Cocoa Touch 애플리케이션 개발을 위한 네이티브 언어이다. 하지만 Cocoa와 Cocoa Touch 애플리케이션을 위한 프로젝트들이 C++ 및 ANSI C 코드가 포함될 수 있다. 또한 Objective-C 런타임에 브릿징하는 스크립팅 언어 PyObjC와 RubyCocoa를 사용해 Cocoa 애플리케이션을 개발할 수 있다.

#### Prerequisite Articles

\(None\)

#### Related Articles

* Root class
* Objective-C

#### Definitive Discussion

\(None\)

