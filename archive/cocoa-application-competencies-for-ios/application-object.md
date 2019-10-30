# Application object

애플리케이션 객체는 사용자 이벤트의 초기 라우팅과 실행 중인 애플리케이션의 전반적인 관리를 책임 진다. 애플리케이션이 실행되면 그것의 `main` 함수에서 애플리케이션 객체를 만든다. 애플리케이션의 메인 루프 내에서, 애플리케이션 객체는 수신 이벤트\(사용자 액션을 나타냄\)를 취하고 이를 액션의 초점이 되는 뷰를 포함하는 윈도우로 라우팅한다. 또한 컨트롤으로 부터 액션 메시지를 수신하여 적절한 대상으로 전달한다. 이는 윈도우의 목록을 유지하고 현재 상태를 관리한다.

## An Application Object Informs its Delegate of External Events

외부 이벤트가 애플리케이션 자체에 영향을 미칠 때, 애플리케이션 객체는 또한 운영 체제로부터 알림을 수신한다. \(예를 들어, 사용자가 컴퓨터를 종료하거나 사용 가능한 메모리가 부족할 때\) 애플리케이션 객체는 이러한 외부 이벤트 및 애플리케이션의 라이프 사이클과 관련된 이벤트를 관리하는데 있어 델리게이트에 도움을 요청한다. 이는 이러한 이벤트를 델리게이트에 알리고, 경우에 따라 델리게이트 메시지에 응답하여 행동한다.

![](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/application_object.jpg)

## An Application Has a Single Application Object

애플리케이션 객체는 싱글톤이다. 즉, 애플리케이션의 모든 객체가 하나의 인스턴스를 사용할 수 있다. iOS에서 애플리케이션 객체는 [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication) 클래스의 인스턴스\(또는 해당 클래스의 하위 클래스\)이며, OS X에서 애플리케이션 객체는 [`NSApplication`](https://developer.apple.com/documentation/appkit/nsapplication) 클래스에서 파생된다. OS X와 iOS 모두에서, 클래스 메시지 `sharedApplication`을 호출하여 애플리케이션 객체에 접근할 수 있다.

### Prerequisite Articles

[Singleton](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Singleton.html#//apple_ref/doc/uid/TP40008195-CH49)  
[Delegation](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14)  
[Main event loop](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/MainEventLoop.html#//apple_ref/doc/uid/TP40009071-CH18-SW1)

**Related Articles**

[Window object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Window.html#//apple_ref/doc/uid/TP40009071-CH6-SW1)  
[View object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/ViewObject.html#//apple_ref/doc/uid/TP40009071-CH5-SW1)  
[Control object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Control.html#//apple_ref/doc/uid/TP40009071-CH7-SW1)  
[Target-Action](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/TargetAction.html#//apple_ref/doc/uid/TP40009071-CH3-SW1)

**Definitive Discussion**

_App Programming Guide for iOS_

**Sample Code Projects**

\(None\)

