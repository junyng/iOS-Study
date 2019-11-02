# Undo manager

실행 취소 관리자는 객체 상태에 대한 변경사항을 기록하고 나중에 사용자 요청에 따라 변경사항을 실행 취소하기 위한 프레임워크 객체다. `NSUndoManager` 클래스의 인스턴스인 실행 취소 관리자는 실행 취소 및 재실행 작업을 모두 수행한다. 실행 취소 작업은 변경 사항을 객체의 프로퍼티로 되돌린다. 취소 작업이 수행된 경우 재실행 작업은 원래 값을 복원한다. 실행 취소 및 재실행 작업을 수행할 수 있는 능력은 특히 사용자가 데이터를 광범위하게 변경할 수 있도록 하는 애플리케이션의 가치 있고 차별화된 기능이다.

당신은 `UIResponder`\(iOS\)의 `undoManager`프로퍼티를 통해 실행 취소 관리자를 얻거나 `NSResponder`\(OS X\)의 `undoManager`의 메서드를 호출한다. 특정 컨텍스트에 대해 private 윈도우를 만들 수 있지만 애플리케이션의 모든 윈도우는 기본 실행 취소 관리자를 제공한다.

### Undo and Redo Operations Send Messages to the Changed Object

실행 취소 작업을 실행 취소 관리자에 등록해야 한다. 실행 취소 관리자가 작업을 실행할 때 등록에 제공된 여러 구성 요소에서 구성된 메시지를 전송한다.

* **The object to receive the message.** 수신 객체는 일반적으로 값이 변경된 속성을 가진 객체이다.
* **The signature of the method to invoke.** 서명은 종종 세터 접근자 메서드를 식별한다.
* **The argument of the message.** 인수는 일반적으로 변경된 프로퍼티의 이전 값이다. 재실행 작업의 경우 인수는 변경된 값이다.

실행 취소 관리자는 애플리케이션의 메인 이벤트 루프와 같은 실행 루프의 단일 사이클 내에서 발생하는 모든 실행 취소 작업을 수집한다. 실행 취소 작업을 실행하면 사이클 중에 발생한 모든 변경사항이 복구된다. 실행 취소 관리자는 사용자가 실행 취소한 작업을 다시 수행할 수 있도록 되돌린 작업을 저장한다.

![](file:///Users/BLU/TIL/iOS/Cocoa-Application-Competencies-for-iOS/Images/undo_redo.jpg?lastModify=1572690229)

### Undo and Redo Operations Are Put on Stacks

실행 취소 관리자는 실행 취소 및 재실행 작업을 FIFO 스택에 저장한다. 애플리케이션이 실행 취소 작업을 처음 등록할 때 실행 취소 관리자가 실행 취소 스택에 추가한다. 그러나 사용자가 실행 취소 작업을 요청할 때까지 재실행 스택은 비어 있다. 이 경우 실행 취소 관리자가 변경 사항을 되돌리기 위해 하나 이상의 메시지를 작성하고 전송하기 위해 실행 취소 스택에서 가장 높은 작업 그룹을 사용한다.

이러한 작업은 하나 이상의 객체 상태를 다시 변경하므로, 객체는 이번에는 원래 작업과 반대 방향으로 실행 취소 관리자에 새 작업을 등록한다. 실행 취소 관리자가 현재 변경사항을 실행 취소하고 있기 때문에 이러한 작업을 재실행 스택에서 재실행 작업으로 기록 한다. 이후 재실행 작업은 재실행 스택에서 작업을 가져와 객체에 적용한 다음 다시 실행 취소 스택으로 밀어 넣어라. 객체 상태에 대한 새로운 변경으로 이전 변경 사항이 무효화되기 때문에 새 실행 취소 작업이 등록되는 즉시 실행 취소 관리자가 재실행 스택을 삭제한다.

### Operations Are Coalesced into Undo Groups

실행 취소 관리자는 완전히 되돌릴 수 없는 작업을 나타내는 그룹으로 실행 취소 및 재실행 작업을 수행한다. 그룹은 일반적으로 어떤 식으로든 관련된 프로퍼티를 식별한다. 이러한 작업 그룹을 실행 취소 및 재실행 스택의 항목으로 저장한다. 단 하나의 액션이라도 그룹으로 포장해야 한다. `NSUndoManager`는 일반적으로 실행 루프의 주기 동안 자동으로 실행 취소 그룹을 생성한다. 사이클에서 실행 취소 작업을 처음으로 기록하도록 요청받으면 새로운 그룹을 생성한다. 그리고 사이클이 끝나면 그룹을 닫는다. 중첩된 추가 실행 취소 그룹을 만들 수 있다.

### How to Register and Request Undo and Redo Operations

`NSUndoManager`클래스를 통해 실행 취소 작업을 등록할 수 있는 두 가지 방법이 있다.

* 간단한 등록을 위해 대상 객체, 셀렉터 및 단일 인수를 사용하는 `registerUndoWithTarget:selector:object:`를 호출하라.
* 호출 기반 등록의 경우, `prepareWithInvocationTarget:` 을 호출하고 변경 내용을 되돌리기 위해 보낼 메시지를 캡슐화하는 `NSInvocation` 객체를 전달하라. 이 접근법으로 당신은 어떤 종류의 인수를 가질 수 있다.

변경 중인 객체의 프로퍼티에 대해 접근자 메서드로 이러한 메서드를 종종 호출한다. 실행 취소 또는 재실행 작업을 요청하려면 `NSUndoManager`객체로 실행 취소 또는 다시 실행메시지를 전송하라.

### The Undo Manager and the Responder Chain

응답자 체인은 특히 기본 실행 취소 관리자가 실행 중인 경우 실행 취소 관리에 중요한 역할을 한다. UIKit 또는 AppKit 프레임워크\(`UIResponder` 또는 `NSResponder`\)의 응답자 클래스는 각각 `undoManager` 프로퍼티와 `undoManager` 메서드를 선언한다. 애플리케이션이 실행 취소 이벤트를 수신하면, 응답자 클래스는 첫 번째 응답자와 함께 시작하고, 거기서 `NSUndoManager`객체를찾는 응답자 체인으로 올라간다. \(`undoManager` 프로퍼티 또는 메서드를 통해\) 응답자 체인의 끝에 도달하면 윈도우 객체가 기본 실행 취소 관리자를 생성하고 반환한다.

기본 실행 취소 관리자 외에 특정 컨텍스트에 대해 실행 취소 관리자를 원하는 경우, 응답자 하위 클래스는 `undoManager` 프로퍼티 또는 생성하거나 반환하는 메서드를 구현할 수 있다. 응답자 하위 클래스는 종종 사용자 정의 뷰이지만 iOS에서는 뷰 컨트롤러일 수도 있다. \(첫 번째 응답자가 되도록 해야 한다\) OS X에서 윈도우 객체의 델리게이트는 실행 취소 관리자를 반환하기 위해 `windowWillReturnUndoManager:` 를 구현할 수 있다.

### The User Interface for Requesting Undo and Redo

사용자들은 OS X와 iOS에서 다른 방법으로 실행 취소와 재실행을 요청한다. Mac 앱에서 사용자는 편집 메뉴에서 실행 취소 또는 재실행 메뉴 명령을 선택한다. iOS 애플리케이션에서 사용자가 기기를 흔들 때이다. 흔들림 동작이 명령으로 기능하도록 하려면, `UIApplication`의 `applicationSupportsShakeToEdit` 프로퍼티를 `YES`로 설정해야 한다. 그리고 적절한 상태에서는 반드시 실행 취소 관리자가 있어야 한다. 이러한 조건이 적용될 경우 애플리케이션은 실행 취소 및 재실행을 위한 버튼이 있는 얼럿 뷰를 표시한다. iOS 및 OS X에서 특정 작업 이름을 지정하여 실행 취소 및 다시 실행을 할 수 있다.\(예: 색 변경 취소\)

#### Prerequisite Articles

[Responder object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Responder.html#//apple_ref/doc/uid/TP40009071-CH1-SW1)  
[Accessor method](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/AccessorMethod.html#//apple_ref/doc/uid/TP40008195-CH2)

**Related Articles**

[Main event loop](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/MainEventLoop.html#//apple_ref/doc/uid/TP40009071-CH18-SW1)

**Definitive Discussion**

_Undo Architecture_

**Sample Code Projects**

[SimpleUndo](https://developer.apple.com/library/archive/samplecode/SimpleUndo/Introduction/Intro.html#//apple_ref/doc/uid/DTS40008408)

