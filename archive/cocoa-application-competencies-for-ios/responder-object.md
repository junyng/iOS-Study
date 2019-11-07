# Responder object

응답자는 이벤트에 반응하고 이벤트를 처리할 수 있는 객체이다. 모든 응답자 객체는 궁극적으로 [`UIResponder`](https://developer.apple.com/documentation/uikit/uiresponder) \(iOS\) 또는 [`NSResponder`](https://developer.apple.com/documentation/appkit/nsresponder) \(OS X\)로부터 상속되는 클래스 인스턴스이다. 이러한 클래스는 이벤트 처리를 위한 프로그램 인터페이스를 선언하고 응답자를 위한 기본 동작을 정의한다. 애플리케이션의 가시적 객체는 거의 항상 응답자\(예를 들어, 윈도우, 뷰 및 컨트롤\)이며, 앱 객체도 응답자이다. iOS에서 뷰 컨트롤러 \([`UIViewController`](https://developer.apple.com/documentation/uikit/uiviewcontroller) 객체\)도 응답자 객체이다.

![](../../.gitbook/assets/responder.jpg)

이벤트를 수신하려면 응답자가 적절한 이벤트 처리 메서드를 구현해야 하며, 경우에 따라 첫 번째 응답자가 될 수 있다고 앱에 알려야 한다.

### The First Responder Receives Some Events First

앱에서, 많은 종류의 이벤트를 처음 받는 응답자 객체는 _first responder_로 알려져 있다. 이는 주요 이벤트, 모션 이벤트 액션 메시지를 수신한다. \(마우스 이벤트 및 멀티터치 이벤트가 먼저 마우스 포인터 또는 손가락 아래에 있는 뷰로 이동한다; 해당 뷰는 첫 번째 응답자일 수도 있고 아닐 수도 있다.\) 첫 번째 응답자는 일반적으로 앱이 이벤트를 처리하는데 가장 적합하다고 판단하는 윈도우의 뷰이다. 이벤트를 수신하려면, 응답자가 첫 번째 응답자가 되겠다는 의사도 표시해야 한다; 각 플랫폼에 대해 다양한 방식으로 이 작업을 수행한다:

```objectivec
// OS X
- (BOOL)acceptsFirstResponder { return YES; }
 
//iOS
- (BOOL)canBecomeFirstResponder { return YES; }
```

응답자는 이벤트 메시지를 수신하는 것 외에 타겟이 지정되지 않은 액션 메시지를 수신할 수 있다. \(액션 메시지는 버튼이나 컨트롤을 사용자가 조작할 때 전송된다.\)

### The Responder Chain Enables Cooperative Event Handling

첫 번째 응답자가 이벤트 또는 작업 메시지를 처리할 수 없는 경우, 그것은 _responder chain_ 라고 하는 연결된 시리의 "다음 응답자"에게 전달된다. 응답자 체인을 통해 응답자 객체가 이벤트 또는 액션 메시지를 앱의 다른 객체로 처리해야할 책임을 이전할 수 있다. 응답자 체인의 객체가 이벤트나 액션을 처리할 수 없는 경우, 체인의 다음 응답자에게 메시지를 전달한다. 메시지는 처리될 때까지 체인을 타고 더 높은 수준의 객체를 향해 이동한다. 처리하지 않으면 앱이 삭제한다.

The responder chain for iOS \(left\) and OS X \(right\)

![](../../.gitbook/assets/ios_and_osx_responder_chain_2x.png)

**이벤트의 경로**. 응답자 체인으로 올라가는 이벤트의 일반적인 경로는 첫 번째 응답자 또는 마우스 포인터 또는 손가락 아래의 뷰에서 시작된다. 여기서 뷰 계층을 윈도우 객체 계층까지 진행한 다음 전역 애플리케이션 객체로 이동한다. 하지만 iOS에서 이벤트에 대한 응답자 체인은 이 경로에 변화를 추가한다. 만약 뷰가 뷰 컨트롤러에 의해 관리되고 뷰 이벤트를 처리할 수 없다면 뷰 컨트롤러가 다음 응답자가 된다.

**액션 메시지의 경로**. 액션 메시지의 경우 OS X와 iOS 모두 응답자 체인을 다른 객체로 확장한다. OS X의 경우, 액션 메시지에 대한 응답자 체인은 문서 아키텍처를 기반으로 한 앱, 윈도우 컨트롤러\([`NSWindowController`](https://developer.apple.com/documentation/appkit/nswindowcontroller)\)를 사용하는 앱, 두 카테고리 모두에 맞지 않는 앱에 대해 다르다. 또한 OS X에서 키 윈도우와 메인 윈도우를 가지고 있다면 액 메시지가 이동하는 응답자 체인은 양쪽 윈도우의 뷰 계층을 포함할 수 있다.

#### Prerequisite Articles

[View hierarchy](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/View%20Hierarchy.html#//apple_ref/doc/uid/TP40009071-CH2-SW1)

**Related Articles**

[Events \(iOS\)](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/EventHandlingiPhone.html#//apple_ref/doc/uid/TP40009071-CH13-SW1)  
[Target-Action](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/TargetAction.html#//apple_ref/doc/uid/TP40009071-CH3-SW1)

**Definitive Discussion**

Event Delivery: The Responder Chain

**Sample Code Projects**

[TheElements](https://developer.apple.com/library/archive/samplecode/TheElements/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007419)

