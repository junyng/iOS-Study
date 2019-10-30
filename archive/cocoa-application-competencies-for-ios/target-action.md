# Target-Action

타겟-액션은 이벤트가 발생했을 때 다른 객체에 메시지를 보내는데 필요한 정보를 객체가 보유하는 설계 패턴이다. 저장된 정보는 두 개의 데이터 항목으로 구성된다: 이는 호출할 메서드를 식별하는 액션 셀렉터와 메시지를 수신할 타겟 객체이다. 이벤트가 발생할 때 전송되는 메시지를 액션 메시지라고 한다. 타겟은 어떤 객체, 심지어 프레임워크 객체라도 될 수 있지만, 그것은 일반적으로 애플리케이션 특정 방식으로 액션 메시지를 처리하는 사용자 정의 컨트롤러이다.

![](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/target_action%20%281%29.jpg)

메시지를 보내는 객체가 어떤 객체가 될 수 있는 것처럼 액션 메시지를 트리거하는 이벤트는 무엇이든 될 수 있다. 예를 들어, 제스처 인식기 객체는 제스처를 인식할 때 다른 객체에 액 메시지를 보낼 수 있다. 그러나 타겟-액션 패러다임은 버튼이나 슬라이더와 같은 컨트롤에서 가장 흔히 발견된다. 사용자가 컨트롤 객체를 조작할 때 지정된 객체로 메시지를 전송한다. 컨트롤 객체는 [`UIControl`](https://developer.apple.com/documentation/uikit/uicontrol)\(iOS\) 또는 [`NSControl`](https://developer.apple.com/documentation/appkit/nscontrol)\(OS X\)의 하위 클래스 인스턴스이다. 액션 셀렉터와 타겟 객체는 컨트롤 객체의 프로퍼티 또는 AppKit 프레임워크에서 컨트롤 셀 객체의 프로퍼티이다.

## An Action Method Must Have a Certain Form

액션 메서드는 관습적인 서명이 있어야 한다. UIKit 프레임워크는 서명의 일부 변화를 허용하지만, 두 플랫폼 모두 다음과 같은 유사한 서명이 있는 액션 메서드를 수용한다:

```objectivec
- (IBAction)doSomething:(id)sender;
```

`void` 반환 타입 대신 사용되는 타입 한정자 `IBAction`은 선언된 메서드를 액션으로 플래그 지정하여 인터페이스 빌더가 인식하도록 한다. 인터페이스에 나타나는 액션 메서드를 사용하려면 먼저 액션 메시지를 수신할 인스턴스가 있는 클래스의 헤더 파일에 선언해야 한다.

`sender` 파라미터는 액션 메시지를 송신하는 제어 객체다. 액션 메시지에 응답할 때, 당신은 액션 메시지를 트리거하는 이벤트의 컨텍스트에 대한 더 많은 정보를 얻기 위해 `sender`를 질의할 수 있다.

## You Can Set Target and Action in Code or Using the Tools

제어\(또는 셀\) 객체의 액션과 대상을 프로그래밍 방식 또는 인터페이스 빌더에서 설정할 수 있다. 이러한 프로퍼티를 설정하면 액션을 통해 컨트롤과 대상이 효과적으로 연결된다. 컨트롤과 그 대상을 인터페이스 빌더에 연결하면 연결이 nib 파일로 아카이브된다. 나중에 애플리케이션이 nib 파일을 로드하면 연결이 복원된다.

액션 메시지의 타겟을 `nil`로 설정할 수 있다. 이 경우 애플리케이션이 런타임에 타겟을 결정한다. 먼저 첫 번째 응답자에게 액션 메시지를 전송하고, 거기서 처리될 때까지 응답자 체인으로 올라간다.

## Target-Action is Different in iOS and OS X

개념적으로 UIKit과 AppKit 두 프레임워크에 대해 타겟-액션 설계 패턴이 동일하지만, 서로 다르게 구현한다.

* UIKit에서 컨트롤은 컨트롤에서 발생할 수 있는 하나 이상의 멀티터치 이벤트에 타겟과 액션을 매핑한다.
* UIKit은 액션 메서드에 대해 몇 가지 다른 서명을 허용한다. 예를 들어, 다음과 같은 서명이 허용된다.

  ```text
  - (IBAction)action:(id)sender forEvent:(UIEvent *)event
  ```

* AppKit은 대부분의 \(전부는 아니지만\) 컨트롤에 대해 타겟-액션을 구현하기 위해 컨트롤-셀 아키텍처를 사용한다. 이 아키텍처에서 컨트롤은 하나 이상의 경량 셀 객체를 "소유"하며, 셀은 그 제어를 위한 타겟 및 액션 프로퍼티를 보유한다. 사용자가 컨트롤을 클릭하거나 활성화할 때 셀에서 이 정보를 추출하여 액션 메시지를 전송한다.

### Prerequisite Articles

[Selector](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html#//apple_ref/doc/uid/TP40008195-CH48)  
[Control object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Control.html#//apple_ref/doc/uid/TP40009071-CH7-SW1)

**Related Articles**

[Responder object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Responder.html#//apple_ref/doc/uid/TP40009071-CH1-SW1)

**Definitive Discussion**

[Target-Action](https://developer.apple.com/library/archive/documentation/General/Conceptual/CocoaEncyclopedia/Target-Action/Target-Action.html#//apple_ref/doc/uid/TP40010810-CH12)

**Sample Code Projects**

[UIKit Catalog \(iOS\): Creating and Customizing UIKit Controls](https://developer.apple.com/library/archive/samplecode/UICatalog/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007710)

