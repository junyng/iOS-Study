# Main event loop

메인 이벤트 루프의 경우, 애플리케이션은 수신 이벤트 처리를 위해 객체로 지속적으로 라우팅하고, 그 결과 그것의 모양과 상태를 업데이트한다. 이벤트 루프는 단순히 실행 루프일 뿐이다. 실행 루프에 부착된 다양한 입력 소스에서 이벤트를 수신하고 작업을 스케줄링하기 위한 이벤트 처리 루프이다. 모든 쓰레드는 실행 루프에 접근할 수 있다. 메인 쓰레드를 제외한 모든 경우, 실행 루프는 당신의 코드에 의해 수동으로 구성되고 실행되어야 한다. Cocoa 애플리케이션에서 메인 쓰레드의 실행 루프는 애플리케이션 객체에 의해 자동으로 실행된다. 메인 이벤트 루프를 구별하는 것은 뷰를 탭하거나 키보드를 사용해 텍스트를 입력하는 것과 같이 주요 입력 소스가 사용자 액에 의해 생성된 운영 체제로부터 이벤트를 수신한다는 것이다.

![](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/main_event_loop.jpg)

## The Application Object Gets and Dispatches Events

애플리케이션이 실행된 직후, 그것은 메인 이벤트 루프를 위한 인프라를 설정한다. 그것은 낮은 수준의 사용자 이벤트 제공을 담당하는 기본 시스템 구성요소와의 연결을 설정한다. 애플리케이션은 메인 쓰레드의 실행 루프에 설치된 입력 소스를 통해 이러한 이벤트를 수신한다. 애플리케이션이 각 이벤트를 별도로 처리해야 하기 때문에, 애플리케이션이 도착 순서에 따라 이러한 낮은 수준의 이벤트가 선입 선출 _event queue_에 배치된다.

초기 사용자 인터페이스가 화면에 표시되면, 그 후에 애플리케이션은 외부 이벤트에 의해 구동된다. 애플리케이션 객체는 이벤트 대기열에서 맨 위 객체를 가져와서 이벤트 객체\(iOS의 경우 [`UIEvent`](https://developer.apple.com/documentation/uikit/uievent), OS X의 경우 [`NSEvent`](https://developer.apple.com/documentation/appkit/nsevent)\)로 변환한 후 처리하기 위해 애플리케이션의 다른 객체에 발송한다. 이벤트를 발송한 호출이 반환되면 애플리케이션은 대기열에서 다음 객체를 가져와 발송한다. 이는 애플리케이션이 종료될 때까지 계속 진행한다.

## Core Objects Respond to Events and Draw the User Interface

애플리케이션이 실행되면, 그것은 또한 사용자 인터페이스의 그리기와 이벤트 처리를 담당하는 핵심 객체 그룹을 설정한다. 이 핵심 객체는 윈도우와 다양한 뷰를 포함한다. 애플리케이션 객체가 이벤트 대기열에서 이벤트를 수신하면 사용자 이벤트가 발생한 윈도우로 해당 객체를 발송한다. 윈도우는 이벤트를 가장 적절한 처리기인 뷰로 전송한다.

* 멀티터치 및 마우스 이벤트의 경우, 터치 또는 마우스 포인터 아래의 뷰
* 키보드, 동작 및 기타 이벤트의 경우, 뷰가 첫 번째 응답자이다.

이 초기 뷰가 이벤트를 처리하지 않는 경우, 응답자 체인을 통해 애플리케이션의 다른 뷰로 전달할 수 있다.

이벤트를 처리할 때, 뷰는 종종 애플리케이션의 모양을 수정하고 애플리케이션의 상태와 데이터를 업데이트하는 일련의 액션을 시작한다. 이러한 작업이 완료되면 컨트롤은 이벤트 대기열에서 다음 이벤트를 가져오는 애플리케이션으로 돌아간다.

### Prerequisite Articles

[Application object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/ApplicationObject.html#//apple_ref/doc/uid/TP40009071-CH10-SW1)  
[Responder object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Responder.html#//apple_ref/doc/uid/TP40009071-CH1-SW1)

**Related Articles**

[Events \(iOS\)](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/EventHandlingiPhone.html#//apple_ref/doc/uid/TP40009071-CH13-SW1)  
[Responder object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Responder.html#//apple_ref/doc/uid/TP40009071-CH1-SW1)

**Definitive Discussion**

_Event Handling Guide for iOS_

**Sample Code Projects**

\(None\)

