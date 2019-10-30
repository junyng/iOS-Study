# Events \(iOS\)

iOS의 이벤트는 손가락으로 터치한 애플리케이션 또는 사용자가 기기를 흔드는 모습을 나타낸다. 하나 이상의 손가락이 하나 이상의 뷰를 터치하고, 아마도 이리저리 움직인 다음, 뷰에서 들어올린다. iPhone 멀티터치 시스템은 이 터치들을 이벤트로 등록해 현재 활성화된 애플리케이션에 전송해 처리한다. 첫 번째 동작이 뷰를 터치하는 순간부터 마지막 손가락이 뷰에서 들어올릴 때까지 가능한 터치 동작의 범위는 _multitouch sequence_를 정의한다. 애플리케이션\(및 프레임워크 객체\)은 멀티터치 시퀀스를 분석하며, 대개 그것들이 핀치나 스와이프와 같은 제스처인지 여부를 결정한다.

운영 체제는 사용자가 기기를 흔들 때 동작 이벤트를 발생시킬 수도 있다. 이것들은 애플리케이션에 별개의 이벤트로 전달된다.

### Objects Represent Fingers Touching a View

뷰에 터치하는 손가락은 [`UITouch`](https://developer.apple.com/documentation/uikit/uitouch) 객체로 표시된다. 터치 객체에는 손가락이 터치되는 뷰, 뷰에서 손가락 위치, 타임스탬프, 단계와 같은 정보가 포함된다. 터치 객체는 지정된 순서대로 멀티 터치 시퀀스 동안 여러 단계를 거친다:

[`UITouchPhaseBegan`](https://developer.apple.com/documentation/uikit/uitouchphase/uitouchphasebegan)

     손가락이 뷰를 터치 다운했다.

[`UITouchPhaseMoved`](https://developer.apple.com/documentation/uikit/uitouch/phase/moved)

     손가락 뷰 또는 인접한 뷰로 움직였다.

[`UITouchPhaseEnded`](https://developer.apple.com/documentation/uikit/uitouch/phase/ended)

     손가락이 뷰로 부터 들어올려졌다.

터치 객체는 정지 단계일 수도 있고 취소될 수도 있다. 터치 객체는 멀티터치 시퀀스를 통해 지속되지만 상태는 변한다. 애플리케이션 패키지는 객체를 다루기 위해 뷰에 전송 때, [`UIEvent`](https://developer.apple.com/documentation/uikit/uievent) 객체안의 객체를 터치한다.

### The Delivery of Touch Objects Follows a Defined Path

메인 이벤트 루프에서 애플리케이션 객체는 이벤트 큐의 \(날것의\) 터치 이벤트를 얻고 `UIEvent`객체의 `UITouch`객체로 패키지화하여 터치가 발생한 뷰의 윈도우로 전송한다. 윈도우 객체는 차례로 _hit-test_ 뷰로 알려진 객체를 이 뷰로 전송한다. 이 뷰가 터치 이벤트를 처리할 수 없는 경우\(보통 필요한 이벤트 처리 메서드를 구현하지 않았기 때문에\), 이벤트는 처리되거나 폐기될 때까지 응답자 체인 위로 이동한다.

### To Handle Events You Must Override Four Methods

사용자 정의 뷰 및 뷰 컨트롤러가 포함된 응답자 객체는 `UIResponder` 클래스에서 선언하는 네 가지 메서드를 구현하여 이벤트를 처리한다

* [`touchesBegan:withEvent:`](https://developer.apple.com/documentation/uikit/uiresponder/1621142-touchesbegan) Began 단계에서 터치 객체에 대해 호출된다
* [`touchesMoved:withEvent:`](https://developer.apple.com/documentation/uikit/uiresponder/1621107-touchesmoved) Moved 단계에서 터치 객체에 대해 호출된다
* [`touchesEnded:withEvent:`](https://developer.apple.com/documentation/uikit/uiresponder/1621084-touchesended) Ended 단계에서 터치 객체에 대해 호출된다
* [`touchesCancelled:withEvent:`](https://developer.apple.com/documentation/uikit/uiresponder/1621116-touchescancelled) 일부 외부 이벤트가 발생할때 호출된다. 예를 들어, 수신 통화는 운영체제가 멀티터치 시퀀스로 터치 객체를 취소하게 한다.

각 메서드의 첫 번째 인자는 지정된 단계의 터치 객체 집합이다. 두 번째 인자는 현재 멀티터치 시퀀스에서 모든 터치 객체를 추적하는 [`UIEvent`](https://developer.apple.com/documentation/uikit/uievent) 객체이다. 

![](../../.gitbook/assets/event_delivery.jpg)

기본적으로 뷰는 멀티터치 이벤트를 수신할 수 있다. 그러나 일부 `UIKit` 뷰 클래스의 인스턴스는 [`userInteractionEnabled`](https://developer.apple.com/documentation/uikit/uiview/1622577-isuserinteractionenabled) 프로퍼티가 `NO`로 설정되어 멀티 터치 이벤트를 수신할 수 없다. 이러한 클래스를 서브클래싱하고 이벤트를 수신하려면 이 프로퍼티를 `YES`로 설정하라. 사용자 정의 뷰 및 뷰 컨트롤러는 네 가지 메서드를 모두 구현해야 하지만 `UIKit` 뷰 클래스의 하위 클래스는 관심 단계에 해당하는 메서드만 구현하면 된다. 그러나 이 경우 반드시 슈퍼클래스 구현을 먼저 호출해야 한다.

### Handling Motion Events

운영체제는 장치의 가속도계를 통해 특정 모션을 감지하여 활성 애플리케이션에 이벤트로 전송한다. \(현재 지원되는 유일한 동작은 장치를 흔드는 것이다.\) 첫 번째 응답자는 이러한 모션 이벤트\(`UIEvent` 객체\)를 처음 수신하고 이를 처리할 수 없을 경우 응답자 체인을 따라 이동한다. 시스템은 모션이 시작될 때와 모션이 정지될 때만 애플리케이션에 알려준다. 해당 `UIResponder` 메서드는 [`motionBegan:withEvent:`](https://developer.apple.com/documentation/uikit/uiresponder/1621120-motionbegan) and [`motionEnded:withEvent:`](https://developer.apple.com/documentation/uikit/uiresponder/1621090-motionended)이다. 핸들링 응답 객체에 전달되는 정보는 이벤트 객체에 의해 캡슐화된 데이터\(이 경우, [`UIEventTypeMotion`](https://developer.apple.com/documentation/uikit/uievent/eventtype/motion)\), 이벤트 하위 유형\(이 경우, [`UIEventSubtypeMotionShake`](https://developer.apple.com/documentation/uikit/uievent/eventsubtype/motionshake)\), 타임스탬프가 있다.

#### Prerequisite Articles

[Responder object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Responder.html#//apple_ref/doc/uid/TP40009071-CH1-SW1)

**Related Articles**

[Application object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/ApplicationObject.html#//apple_ref/doc/uid/TP40009071-CH10-SW1)  
[Main event loop](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/MainEventLoop.html#//apple_ref/doc/uid/TP40009071-CH18-SW1)  
[View object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/ViewObject.html#//apple_ref/doc/uid/TP40009071-CH5-SW1)  
[Target-Action](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/TargetAction.html#//apple_ref/doc/uid/TP40009071-CH3-SW1)

**Definitive Discussion**

The iOS Environment

**Sample Code Projects**

[Handling Touches Using Responder Methods and Gesture Recognizers](https://developer.apple.com/library/archive/samplecode/Touches/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007435)

