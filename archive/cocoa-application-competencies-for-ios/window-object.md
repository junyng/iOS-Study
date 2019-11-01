# Window object

윈도우 객체는 화면의 직사각형 영역에 그래픽 콘텐츠를 표시하고 이벤트를 분배하는 역할을 한다. Cocoa Touch의 [`UIWindow`](https://developer.apple.com/documentation/uikit/uiwindow)와 Cocoa의 [`NSWindow`](https://developer.apple.com/documentation/appkit/nswindow)\(OS X\)에서 상속받은 이 객체는 애플리케이션에 나타나는 윈도우를 나타낸다. 윈도우 객체 \(또는 단순히 윈도우\)는 본질적으로 뷰의 컨테이너이며, 그러한 뷰에 의해 수행되는 그리기 및 이벤트 처리와 관련된 주요 책임을 갖는다.

## Windows Coordinate Drawing and Distribute Events

윈도우는 다른 뷰를 둘러싸는 계층에서 가장 높은 뷰를 가진 뷰 계층의 근원에 있다. 윈도우가 내용을 표시할 때, 각 뷰가 차례로 슈퍼 뷰의 컨텍스트에서 그 자신을 그리도록 요청하면서, 이 계층구조를 진행한다. 계층의 맨 위에 있는 뷰는 콘텐츠 뷰이다. 콘텐츠 뷰는 윈도우의 전체 그리기 영역을 차지하며, 윈도우의 다른 모든 뷰의 배경 역할을 한다. \(`UIWindow` 객체의 경우 이 콘텐츠 뷰는 윈도우 자체이다.\)

{% embed url="https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Art/window\_object.m4v" %}

기본 좌표계라고 불리는 윈도우의 좌표계는 콘텐츠를 표시하는 역할과 관련이 있다. 윈도우에서 뷰에 의해 수행되는 모든 그리기는 궁극적으로 기본 좌표계를 참조한다. 그러나 윈도우 자체는 화면 자체 좌표계를 사용하여 화면에 배치하고 크기를 조정한다. 윈도우 클래스는 뷰의 로컬 좌표계와 기본 좌표계 사이를 변환하는 메서드를 정의한다.

윈도우는 들어오는 이벤트를 해당 이벤트의 가장 적절한 수신 계층의 뷰에 분배한다. 마우스 이벤트, 터치 이벤트 및 유사한 이벤트의 경우, 해당 수신자 마우스 포인터 아래 또는 뷰에 터치한 손가락 아래에 있는 뷰이다. 대상이 없는 주요 이벤트 및 작업 메시지의 경우 수신자 첫 번째 응답자이다. 즉, 그 객체는 먼저 이벤트나 메시지에 응답할 기회를 얻었다. 윈도우는 첫 번째 응답자를 추적하는 일을 담당한다.

Cocoa Touch와 Cocoa의 윈도우는 이러한 일반적인 책임을 공유하지만, 그들은 다른 특성과 행동을 가지고 있다. 그 차이점은 다른 사용자 환경에서 유래하고 있다. 즉 데스크탑 시스템과 화면 영역이 제한된 휴대용 장치이다. 사용자 환경이 다르기 때문에 사용 패턴과 요구 사항이 달라진다.

## Window Objects in iOS

`UIWindow`객체는 역할이 제한된 특수 뷰이다.\(`NSWindow` 객체와 비교된다.\) 비록 iOS 애플리케이션은 기술적으로 한 계층이 다른 계층 위에 있는 한 개 이상의 윈도우를 가질 수 있지만, 관례상 그것들은 단일 윈도우로 제한된다. 윈도우는 화면 전체를 점유하고 있으며, OS X의 윈도우처럼 제목이나 제어장치 또는 기타 장식이 없기 때문에 사용자가 조작할 수 없다. 실행 시 애플리케이션은 이 윈도우를 생성하고 뷰를 추가하거나, nib 파일에서 윈도우와 뷰를 로드한다. 윈도우를 표시한 후에는 다시 참조할 필요가 거의 없다.

## Window Objects in OS X

`NSWindow` 객체로 표시되는 윈도우에는 두 가지 주요 부분이 있다: 프레임 영역 및 콘텐츠 뷰이다. private 뷰로 구성된 프레임 영역은 윈도우 전체 영역을 둘러싸고 윈도우의 테두리, 제목 표시줄 및 제목 표시줄에 있는 항목 \(닫기 버튼, 축소 버튼, 제목 등\)을 그린다. 또한 오른쪽 하단에 있는 크기 조정 삼각형을 포함한다. 콘텐츠 뷰는 프레임 영역으로 둘러싸인 영역을 차지하며, 윈도우의 뷰 계층의 루트이다.

OS X의 윈도우 객체는 몇 가지 다른 상태를 가질 수 있다.

* Main or key window: 메인 윈도우는 애플리케이션에 대한 사용자 액션의 주요 초점이다. 키 윈도우는 키보드 이벤트에 대한 현재 초이다.\( 예를 들어, 사용자가 입력하는 텍스트 필드가 들어 있다.\) 애플리케이션의 다른 윈도우는 메인 및 윈도우 상태를 가질 수 있다. 윈도우는 메인과 키 상태를 동시에 가질 수 있다.
* Active or inactive window: 활성 윈도우는 현재 사용자 관심의 초점이다. 윈도우가 비활성 상태인 경우, 이 상태를 나타내기 위해 스스로 다르게 그린다. 사용자들은 그것을 다시 활성화하기 위해 클릭해야 한다. 만약 애플리케이션이 비활성화 된다면 윈도우도 비활성화된다.

OS X의 애플리케이션은 여러 개의 윈도우를 가질 수 있다. 문서 기반 애플리케이션으로, 각 문서에는 고유한 윈도우가 있다. 애플리케이션의 메인 윈도우\(또는 복수의 윈도우\) 외에도 모든 애플리케이션에는 색 선택 또는 애플리케이션 기본 설정과 같은 작업을 위한 보조 윈도우 또는 패널이 있을 수 있다. [`NSPanel`](https://developer.apple.com/documentation/appkit/nspanel)객체 \(또는 단순하게 패널\)는 어떤 방식으로든 애플리케이션 또는 문서를 지원하는 보조 윈도우를 나타낸다. 이는 키 윈도우가 될 수 있지만, 메인 윈도우는 절대 사용하지 않으며, 애플리케이션이 비활성화되면 대개 화면에서 제거된다. 애플리케이션은 사용자가 작업을 완료할 때까지 윈도우에 대한 입력을 제한하며 윈도우를 모달 방식으로 실행한다.

윈도우는 또한 수준과 Z-order를 가지고 있으며, 운영체제에 의해 유지된다. 레벨은 특정한 기능적 형식의 윈도우에 표시된 하위집합이다. 예를 들어 모달 창이 떠 있는 윈도우 위에 표시되도록 레벨 자체가 계층적으로 배열되어 있다. 현재 화면 또는 밖의 모든 윈도우는 각 윈도우의 레벨에 따라 영향을 받는 특정한 앞뒤 순서로 되어 있다. 이 목록에서 윈도우의 위치는 Z-order이다.

### Prerequisite Articles

\(None\)

### Related Articles

[View hierarchy](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/View%20Hierarchy.html#//apple_ref/doc/uid/TP40009071-CH2-SW1)  
[Coordinate system](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/CoordinateSystem.html#//apple_ref/doc/uid/TP40009071-CH8-SW1)  
[Drawing model](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/DrawingModel.html#//apple_ref/doc/uid/TP40009071-CH9-SW1)  
[Target-Action](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/TargetAction.html#//apple_ref/doc/uid/TP40009071-CH3-SW1)  
[Responder object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Responder.html#//apple_ref/doc/uid/TP40009071-CH1-SW1)

**Definitive Discussion**

[Performance Tuning](https://developer.apple.com/library/archive/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/StrategiesforHandlingAppStateTransitions/StrategiesforHandlingAppStateTransitions.html#//apple_ref/doc/uid/TP40007072-CH8)

### Sample Code Projects

\(None\)

