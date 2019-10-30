# Control object

컨트롤은 사용자가 버튼을 누르거나 슬라이더를 끄는 등 특정 방으로 조작할 때 다른 객체에 메시지를 보내는 사용자 인터페이스 뷰 유형이다. 컨트롤은 타겟-액션 모델의 중개인이다. 컨트롤 \(또는 OS X에서 control's cell\)은 메시지를 수신할 객체\(타겟\)에 대한 참조와 타겟에서 호출하는 메서드를 식별하는 셀렉터\(액션\) 등 메시지 전송에 필요한 정보를 저장한다. 사용자가 특정한 방법으로 컨트롤을 조작할 때, 그것은 애플리케이션 객체로 메시지를 전송하고, 그 후 액션 메시지를 타겟에 전달한다.

![](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/target_action.jpg)

컨트롤에 대한 추상 기본 클래스는 UIKit 프레임워크의 [`UIControl`](https://developer.apple.com/documentation/uikit/uicontrol)과 AppKit 프레임워크의 [`NSControl`](https://developer.apple.com/documentation/appkit/nscontrol)이다. 모든 컨트롤은 이러한 기본 클래스의 구체적인 하위 클래스 인스턴스이다. 이러한 프레임워크에서 일반적인 유형의 컨트롤은 버튼, 슬라이더 및 텍스트 필드이다. UIKit과 AppKit은 플랫폼에 특정한 컨트롤 예를 들어 페이지 컨트롤\(UIKit\)과 컬러 웰\(OS X\) 또한 가지고 있다.

## In UIKit, Control Events Determine When Action Messages are Sent

컨트롤 이벤트는 UIKit 프레임워크 설계의 한 측면이다. UIKit에서 타겟과 액션 셀렉터가 액션 메시의 형식을 결정한다. 그러나 하나 이상의 컨트롤 이벤트\(컨트롤과도 연관됨\)가 메시지 발송시기를 결정한다. 컨트롤 이벤트는 터치 동작\(예: [`UIControlEventTouchUpInside`](https://developer.apple.com/documentation/uikit/uicontrol/event/1618236-touchupinside)\), 세션의 편집 단계\(예: [`UIControlEventEditingDidEnd`](https://developer.apple.com/documentation/uikit/uicontrolevents/uicontroleventeditingdidend)\), 값 변경\([`UIControlEventValueChanged`](https://developer.apple.com/documentation/uikit/uicontrol/event/1618238-valuechanged)\)를 나타내는 `enum` 상수이다. 당신은 여러 개의 컨트롤 이벤트를 컨트롤과 연관시킬 수 있으며, 이러한 상수로 대표되는 동작이 발생하면 컨트롤은 액션 메시지를 타겟에게 전송한다.

일 컨트롤은 특정한 컨트롤 이벤트를 설정해야 한다. 예를 들어, [`UISwitch`](https://developer.apple.com/documentation/uikit/uiswitch) 객체는 [`UIControlEventValueChanged`](https://developer.apple.com/documentation/uikit/uicontrol/event/1618238-valuechanged) 컨트롤 이벤트가 충족될 때만 액션 메시지를 전송한다.

## In AppKit, Controls Can Have One or More Cells

AppKit 프레임워크의 대부분의 컨트롤은 하나 이상의 셀 객체와 관련된다. 셀은 [`NSCell`](https://developer.apple.com/documentation/appkit/nscell)로부터 직접 또는 간접적으로 상속되는 클래스의 예다. 그것의 주요 역할은 그것의 컨트롤에 대한 타겟 객체와 액션 셀렉터를 저장하는 것이다. _control-cell architecture_에서 컨트롤은 셀에 대한 정면 뷰이다. 그것은 셀의 동작을 관리하고 고유한 사용자 이벤트가 발생하면 액션 메시지를 구성하고 전송한다.

AppKit의 몇 가지 컨트롤은 여러 개의 셀을 가지고 있다. 예를 들어 [`NSMatrix`](https://developer.apple.com/documentation/appkit/nsmatrix) 객체는 동일하거나 다른 유형의 셀의 행 또는 행렬을 포함하는 컨트롤이다. 각 셀은 자체 타겟 및 액션 셀렉터를 가질 수 있다. AppKit의 몇 가지 컨트롤이 타겟 및 액션 정보를 직접 저장하며 셀을 사용하지 않는다.

### Prerequisite Articles

[Selector](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html#//apple_ref/doc/uid/TP40008195-CH48)  
[Target-Action](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/TargetAction.html#//apple_ref/doc/uid/TP40009071-CH3-SW1)

**Related Articles**

[Application object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/ApplicationObject.html#//apple_ref/doc/uid/TP40009071-CH10-SW1)

**Definitive Discussion**

_UIControl Class Reference_

**Sample Code Projects**

[UIKit Catalog \(iOS\): Creating and Customizing UIKit Controls](https://developer.apple.com/library/archive/samplecode/UICatalog/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007710)

