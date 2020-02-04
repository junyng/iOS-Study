# Nib Files

Nib 파일은 OS X와 iOS에서 애플리케이션을 만드는 데 중요한 역할을 한다. nib 파일을 사용하면 프로그래밍 방식 대신 Xcode를 사용하여 그래픽으로 사용자 인터페이스를 생성하고 조작할 수 있다. 변경 결과를 즉시 볼 수 있기 때문에 다른 레이아웃과 구성으로 매우 빠르게 실험할 수 있다. 코드를 다시 작성하지 않고도 나중에 사용자 인터페이스의 여러 측면을 변경할 수도 있다.

AppKit 또는 UIKit 프레임워크를 사용하여 구축된 애플리케이션의 경우 nib 파일은 추가적으로 중요한 의미를 갖는다. 이러한 두 프레임워크 모두 윈도우, 뷰 및 컨트롤을 배치하고 이러한 항목을 애플리케이션의 이벤트 처리 코드에 통합하기 위해 nib 파일 사용을 지원한다. Xcode는 이러한 프레임워크와 함께 작동하여 사용자 인터페이스의 제어를 해당 제어에 응답하는 프로젝트의 객체에 연결할 수 있다. 이러한 통합은 nib 파일을 로드한 후에 필요한 설정의 양을 크게 줄이고 코드와 사용자 인터페이스 간의 관계를 쉽게 변경할 수 있게 해준다.

> **참고:** nib 파일을 사용하지 않고 Objective-C 애플리케이션을 만들 수 있지만, 그렇게 하는 것은 매우 드물고 권장되지 않는다. 애플리케이션에 따라 nib 파일을 피하려면 nib 파일을 사용할 때와 동일한 결과를 얻기 위해 대량의 프레임워크 동작을 교체해야 할 수 있다.

### Nib 파일 해부

nib 파일은 윈도우, 뷰, 컨트롤 및 기타 여러 가지를 포함하여 애플리케이션의 사용자 인터페이스의 시각적 요소를 설명한다. 또한 윈도우와 뷰를 관리하는 애플리케이션의 객체와 같은 비시각적 요소를 설명할 수 있다. 가장 중요한 것은 nib 파일이 Xcode로 구성된 것과 정확히 같은 객체를 설명한다. 런타임에 이러한 설명은 애플리케이션 내에 객체와 해당 구성을 다시 만드는 데 사용된다. 런타임에 nib 파일을 로드하면 Xcode 문서에 있던 객체의 정확한 복제본이 나타난다. nib-loading 코드는 객체를 인스턴스화하고 구성하며 nib 파일에서 만든 객체간 연결을 다시 설정한다.

다음 절에서는 AppKit 및 UIKit 프레임워크와 함께 사용되는 nib 파일이 어떻게 체계화되는지, 해당 프레임에서 발견된 객체 유형 및 해당 객체를 효과적으로 사용하는 방법을 설명한다.

#### 인터페이스 객체에 대해

인터페이스 객체는 사용자 인터페이스를 구현하기 위해 nib 파일에 추가하는 것이다. 런타임에 nib를 로드할 때 인터페이스 객체는 실제로 nib-loading 코드에 의해 인스턴스화된 객체다. 대부분의 새로운 nib 파일은 기본적으로 적어도 하나의 인터페이스 객체 \(일반적으로 윈도우 또는 메뉴 리소스\)를 가지고 있으며 인터페이스 설계의 일부로 nib 파일에 더 많은 인터페이스 객체를 추가한다. 이는 nib 파일에서 가장 일반적인 객체 유형이며, 일반적으로 nib 파일을 처음 생성하는 이유이다.

윈도우, 뷰, 컨트롤 및 메뉴와 같은 시각적 객체를 나타내는 것 외에도 인터페이스 객체는 비시각적 객체를 나타낼 수 있다. 거의 모든 경우, nib 파일에 추가하는 비시각적 객체는 애플리케이션이 시각적 객체를 관리하는 데 사용하는 추가 컨트롤러 객체이다. 애플리케이션에서 이러한 객체를 만들 수 있지만 nib 파일에 추가하여 해당 객체를 구성하는 것이 더 편리할 수 있다. Xcode는 컨트롤러 및 기타 비시각적 객체를 nib 파일에 추가할 때 특별히 사용하는 일반 객체를 제공한다. 또한 일반적으로 코코아 바인딩을 관리하는 데 사용되는 컨트롤러 객체를 제공한다.

#### File's Owner에 대해

nib 파일에서 가장 중요한 객체 중 하나는 File's Owner 객체이다. 인터페이스 객체와 달리 File's Owner 객체는 nib 파일이 로드될 때 생성되지 않는 자리 표시자 객체이다. 대신 이 객체를 코드로 작성하여 nib-loading 코드로 전달한다. 이 객체가 그렇게 중요한 이유는 애플리케이션 코드와 nib 파일의 내용 사이의 주요 링크이기 때문이다. 보다 구체적으로는 nib 파일의 내용을 담당하는 컨트롤러 객체이다.

Xcode에서는 File's Owner와 nib 파일의 다른 인터페이스 객체 사이에 연결을 만들 수 있다. nib 파일을 로드할 때 nib-loading 코드는 지정한 대체 객체를 사용하여 이러한 연결을 다시 만든다. 이렇게 하면 객체가 nib 파일에서 객체를 참조하고 인터페이스 객체로부터 메시지를 자동으로 수신할 수 있다.

#### First Responder에 대해

nib 파일에서 First Responder는 애플리케이션의 동적으로 결정된 응답자 체인에서 첫 번째 객체를 나타내는 자리 표시자 객체이다. 애플리케이션의 응답자 체인은 설계 시간에 결정될 수 없기 때문에, 첫 번째 응답자 자리 표시자는 애플리케이션의 응답자 체인을 향해야 하는 모든 액션 메시지의 표준 타겟 역할을 한다. 메뉴 항목은 일반적으로 First Responder 자리 표시자를 대상으로 한다. 예를 들어, 창 메뉴 아이템 최소화 메뉴 항목은 특정 창만이 아닌 애플리케이션의 맨 앞 창을 숨기고, 복사 메뉴 항목은 단일 컨트롤이나 뷰의 선택만이 아니라 현재 선택 항목을 복사해야 한다. 애플리케이션의 다른 객체도 First Responder를 대상으로 할 수 있다.

nib 파일을 메모리에 로드하면 First Responder 자리 표시자 객체를 관리하거나 교체하기 위해 할 일이 없다. AppKit 및 UIKit 프레임워크는 애플리케이션의 현재 구성을 기반으로 First Responder를 자동으로 설정 및 유지 관리한다.

응답자 체인에 대한 자세한 내용과 이 체인이 AppKit 기반 애플리케이션에서 이벤트를 발송하는 데 사용되는 방법에 대한 자세한 내용은 [_Cocoa Event Handling Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/EventOverview/Introduction/Introduction.html#//apple_ref/doc/uid/10000060i)의 [Event Architecture](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/EventOverview/EventArchitecture/EventArchitecture.html#//apple_ref/doc/uid/10000060i-CH3)를 참조하라. iPhone 애플리케이션의 응답자 체인 및 처리 작업에 대한 내용은 _Event Handling Guide for iOS_를 참조하라.

#### Top-Level Objects에 대해

프로그램이 nib 파일을 로드하면 코코아는 Xcode에서 만든 객체의 전체 그래프를 다시 만든다. 이 객체  그래프에는 nib 파일에서 발견된 모든 윈도우, 뷰, 컨트롤, 셀, 메뉴 및 사용자 지정 객체가 포함된다. _최상위 객체_는 상위 객체가 없는 이러한 객체의 하위 집합이다. 최상위 객체에는 일반적으로 nib 파일에 추가하는 윈도우, 메뉴 바 및 사용자 정의 컨트롤러 객체만 포함된다. \(File's Owner, First Responder, Application과 같은 객체는 자리표시자 객체로 최상위 객체로 간주되지 않는다.\)

일반적으로 File's Owner 객체의 아울렛을 사용하여 nib 파일의 최상위 객체에 대한 참조를 저장한다. 그러나 아울렛을 사용하지 않으면 nib-loading 루틴에서 최상위 객체를 직접 검색할 수 있다. 애플리케이션이 해당 객체를 사용할 때 해당 객체를 릴리스할 책임이 있기 때문에 항상 이러한 객체를 어딘가에 포인터로 지정해야 한다. 로드 타임에서 nib 객체 동작에 대한 자세한 내용은 [Managing the Lifetimes of Objects from Nib Files](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/LoadingResources/CocoaNibs/CocoaNibs.html#//apple_ref/doc/uid/10000051i-CH4-SW6)를 참조하라.

#### 이미지 및 사운드 리소스에 대해

Xcode에서 nib 파일의 내용 내에서 외부 이미지 및 사운드 리소스를 참조할 수 있다. 일부 컨트롤과 뷰는 이미지를 표시하거나 기본 구성의 일부로 소리를 재생할 수 있다. Xcode 라이브러리는 Xcode 프로젝트의 이미지 및 사운드 리소스에 대한 접근을 제공하여 nib 파일을 이러한 리소스에 연결할 수 있다. nib 파일은 이러한 리소스를 직접 저장하지 않는다. 대신, nib-loading 코드가 나중에 찾을 수 있도록 리소스 파일의 이름을 저장한다.

이미지 또는 사운드 리소스에 대한 참조가 포함된 nib 파일을 로드하면 nib-loading 코드가 실제 이미지 또는 사운드 파일을 메모리로 읽고 캐시한다. OS X에서는 이미지 및 사운드 리소스가 명명된 캐시에 저장되어 필요에 따라 나중에 접근할 수 있다. iOS에서는 이미지 리소스만 명명된 캐시에 저장된다. 이미지에 접근하려면 플랫폼에 따라 [`NSImage`](https://developer.apple.com/documentation/appkit/nsimage) 또는 [`UIImage`](https://developer.apple.com/documentation/uikit/uiimage) 의 `imageNamed:` 메서드를 사용하라. OS X에서 캐시된 사운드에 접근하려면 [`NSSound`](https://developer.apple.com/documentation/appkit/nssound)의 [`soundNamed:`](https://developer.apple.com/documentation/appkit/nssound/1477318-soundnamed) 메서드를 사용하라.

### Nib 파일 디자인 지침

nib 파일을 생성할 때는 해당 파일의 객체를 사용하는 방법에 대해 신중하게 생각하는 것이 중요하다. 매우 간단한 애플리케이션은 모든 사용자 인터페이스 구성 요소를 단일 nib 파일에 저장할 수 있지만 대부분의 애플리케이션에서는 여러 Nib 파일에 걸쳐 구성 요소를 배포하는 것이 좋다. 더 작은 nib 파일을 생성하면 인터페이스에서 필요한 부분만 즉시 로드할 수 있다. 그들은 또한 문제를 찾을 수 있는 장소가 적기 때문에 여러분이 직면할 수 있는 어떤 문제를 더 쉽게 디버깅할 수 있게 해준다.

nib 파일을 만들 때 다음 지침을 염두에 둬라:

* lazy 로딩하는 것을 염두에 두고 nib 파일을 설계하라. 필요한 객체만 포함된 nib 파일을 즉시 로드할 계획.
* OS X 애플리케이션의 메인 nib 파일에서 애플리케이션 메뉴 바 및 옵션 애플리케이션 델리게이트 객체만 nib 파일에 저장하는 것을 고려하라. 애플리케이션이 실행될 때까지 사용하지 않을 윈도우 또는 사용자 인터페이스 요소를 포함하지 말라. 대신, 이 리소스를 별도의 nib 파일에 넣고 런치 후 필요에 따라 로드한다.
* 반복되는 사용자 인터페이스 구성 요소\(예: document windows\)를 별도의 nib 파일에 저장하라.
* 가끔 사용하는 윈도우나 메뉴의 경우 별도의 nib 파일에 저장한다. 별도의 nib 파일에 저장함으로써 실제로 사용되는 경우에만 리소스를 메모리에 로드할 수 있다.
* File's Owner를 nib 파일 외부의 모든 항목에 대한 단일 연락처로 설정하라. 자세한 내용은 [Accessing the Contents of a Nib File](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/LoadingResources/CocoaNibs/CocoaNibs.html#//apple_ref/doc/uid/10000051i-CH4-SW7)를 참조하라.

