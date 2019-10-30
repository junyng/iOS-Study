# Animation

애니메이션은 iOS와 OS X에서 사용자 인터페이스를 제공하는 기술의 필수적인 부분이다. 또한 이러한 플랫폼에 대한 사용자 경험의 필수적인 부분으로 간주된다. 간단히 말해 애니메이션은 각 영상이 이전 영상과 약간 다른 빠른 순서로 연속된 영상 표시를 통해 움직임이나 변형의 환상이다. 애니메이션은 사용자 경험을 향상시키지만 단순한 "눈사탕"과는 거리가 멀다. 애니메이션은 사용자 인터페이스에서 일어나는 일에 대한 피드백이나 컨텍스트를 사용자에게 제공한다. 예를 들어, iPhone 생산성 애플리케이션에서 사용자가 테이블 뷰에서 행을 터치할 때, 일반적으로 해당 뷰가 왼쪽에서 벗어나고 오른쪽에서 새로운 뷰가 그 자리를 차지하기 위해 슬라이드되어 들어온다. 이러한 애니메이션은 나열된 항목과 상세 표시 사이의 연결을 시각적으로 강화한다. iOS나 OS X는 사용자가 보는 많은 애니메이션을 자동으로 지원하지만 애플리케이션 사용자 인터페이스의 다른 부분에 대한 애니메이션을 만들 수 있다.

## Core Animation Powers Animations in the User Interface

두 플랫폼의 애니메이션 능력의 주요 출는 Core Animation이다. Core Animation은 고속의 2D 레이어링 엔진으로, 이 엔진에 접근하기 위한 Objective-C 인터페이스를 갖추고 있다. Core Animation에서는 애니메이션을 설정한 후 해당 애니메이션을 실행하면 완전히 자동으로 실행된다. 루프나 타이머를 설치할 필요도 없고, 프레임별로 프레임을 그릴 필요도 없고, 애니메이션의 현재 상태를 추적할 필요도 없다.

Core Animation의 핵심 기술은 레이어다. 레이어는 뷰와 유사하지만 실제로 뷰에 할당된 모델 객체인 경량 객체\([`CALayer`](https://developer.apple.com/documentation/quartzcore/calayer)\)이다. 이러한 요인에 기반한 내용을 표시하는 뷰의 기하학, 타이밍 및 시각적 프로퍼티를 캡슐화한다. 당신이 해아하는 것은 레이어 콘텐츠를 설정 및 애니메이션 프로퍼티 구성 후 Core Animation이 그 역할을 수행하도록 한다.

## Animations Must Have a Target, a Type, and Timing Details

모든 애니메이션에 대해 대상 객체\(예: 해당 프레임\)를 지정해야 한다. 예를 들어, 대상을 이동할지, 크기나 규모를 조정할지, 페이드 인 또는 아웃 등 수행할 애니메이션 유형 또한 지정해야 한다. 마지막으로 모든 애니메이션에는 타이밍 정보가 필요하며, 여기에는 애니메이션의 지속시간, 반복 여부, 페이싱 등 여러 요소가 포함될 수 있다. 즉, 애니메이션의 시작과 끝 중 어느 쪽이든 속도가 느려지든 속도가 빨라지든 말이다.

## Implicit Versus Explicit Animations

iOS 또는 OS X의 모든 뷰는 뷰에 대한 구성 및 애니메이션 작업을 최적화하는 레이어 객체에 의해 지원된다. 뷰의 프레임, 색상 및 불투명도를 포함한 여러 뷰의 layered-backed 프로퍼티는 본질적으로 애니메이션이 가능하다. 그들의 값을 변경하면 Core Animation은 오래된 값을 새로운 값으로 변환을 애니메이션 한다. 이러한 암시적 애니메이션은 UIKit 또는 AppKit 프레임워크에 최소한의 지침을 제공해야 하지만 이 지시를 받은 후에, 그들은 자동적으로 애니메이션을 실행한다.

뷰의 프로퍼티를 애니메이션화할 수 없는 경우에도 해당 프로퍼티를 통해 뷰의 애니메이션을 명시적으로 만들 수 있다. 명시적 애니메이션을 위해서는 Core Animation의 기능을 이용하여 애니메이션과 렌더링된 콘텐츠를 보다 적극적으로 관리해야 한다.

## The Platforms Integrate Views and Core Animation Differently

Core Animation은 UIKit 프레임워크의 [`UIView`](https://developer.apple.com/documentation/uikit/uiview) 클래스와 AppKit 프레임워크의 [`NSView`](https://developer.apple.com/documentation/appkit/nsview) 클래스와 통합되지만, 그 성격은 다르다. 주요한은 차이점은 UIKit이 뷰 렌더링 구현의 중심에 Core Animation을 통합하는 반면 AppKit은 Core Animation 기능을 요청해야 한다는 것이다. UIKit에서 각 뷰\([`UIView`](https://developer.apple.com/documentation/uikit/uiview) 객체\)는 뷰를 초기화할 때 [`UIView`](https://developer.apple.com/documentation/uikit/uiview) 클래스가 자동으로 생성하는 [`CALayer`](https://developer.apple.com/documentation/quartzcore/calayer) 객체에 의해 지원된다. 레이어가 뷰와 매우 밀접하게 연결되어 있기 때문에, 당신은 `UIView` 클래스의 메서드와 프로퍼티를 통해 Core Animation에 접근할 수 있다. 뷰의 [`layer`](https://developer.apple.com/documentation/uikit/uiview/1622436-layer) 프로퍼티를 통해 레이어 객체에 직접 접근할 수 있지만 뷰가 생성된 후에는 레이어를 변경할 수 없다.

AppKit에서 `NSView` 클래스는 Core Animation 및 layers 두 가지 방식으로 통합되며, 두 가지 모두 사용자의 명확한 조치가 필요하다.

* **Layer-backed views**는 Core Animation 레이어를 백업 저장소로 사용하는 뷰이다. 이를 위해 애플리케이션은 `YES` 인수를 사용하여 [`setWantsLayer:`](https://developer.apple.com/documentation/appkit/nsview/1483695-wantslayer) 메시지를 뷰에 전송해야 한다.
* **Layer hosting**. 이 접근방식에서 애플리케이션은 뷰 계층 또는 해당 계층의 일부의 뷰를 미러링하는 Core Animation 계층 트리를 설정한다. 애플리케이션은 레이어 내용과 상호작용하기 위해 레이어를 직접 조작해야 한다. 뷰의 루트 레이어 객체를 명시적으로 설정\([`setLayer:`](https://developer.apple.com/documentation/appkit/nsview/1483298-layer)\)한 다음 `setWantsLayer:` 호출을 설정하여 layer-hosted 뷰를 요청하라.

AppKit 프레임워크는 또한 레이어 객체를 직접 조작할 필요가 없는 사용하기 쉬운 애니메이션 지원을 제공하는 _animation proxies_를 지원한다. 또한 [`NSAnimation`](https://developer.apple.com/documentation/appkit/nsanimation) 클래스와 이의 하위 클래스[`NSViewAnimation`](https://developer.apple.com/documentation/appkit/nsviewanimation)를 통해 일반화된 애니메이션 지원 및 여러 애니메이션을 연결하고 여러 윈도우 및 뷰를 애니메이션화하는 기능을 제공한다.

### Prerequisite Articles

[View object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/ViewObject.html#//apple_ref/doc/uid/TP40009071-CH5-SW1)

**Related Articles**

[Protocol](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45)  
[Drawing model](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/DrawingModel.html#//apple_ref/doc/uid/TP40009071-CH9-SW1)

**Definitive Discussion**

_Core Animation Programming Guide_

**Sample Code Projects**

[ViewTransitions](https://developer.apple.com/library/archive/samplecode/ViewTransitions/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007411)

