# Storyboard

스토리 보드는 iOS 애플리케이션의 사용자 인터페이스를 시각적으로 표현한 것으로, 콘텐츠 화면과 화면 간의 연결을 보여준다. 스토리 보드는 일련의 씬으로 구성되며, 각 씬은 뷰 컨트롤러와 뷰를 나타낸다. 씬은 두 뷰 컨트롤러 사이의 전환을 나타내는 세그 객체에 의해 연결된다.

Xcode는 스토리보드에 대한 시각적 편집기를 제공하며, 버튼, 테이블 뷰, 텍스트 뷰 등의 뷰를 씬에 추가하여 애플리케이션의 사용자 인터페이스를 배치하고 설계할 수 있다. 또한 스토리보드를 통해 뷰 컨트롤러 객체에 연결하고 뷰 컨트롤러간 데이터 전송을 관리할 수 있다. 스토리보드를 사용하면 사용자 인터페이스의 모양과 흐름을 하나의 캔버스에서 시각화할 수 있기 때문에 애플리케이션의 사용자 인터페이스를 설계하는 데 권장되는 방법이다.

![](../../.gitbook/assets/storyboard.jpg)

### A Scene Corresponds to a Single View Controller and Its Views

iPhone에서는 각 씬이 전체 화면의 콘텐츠에 해당하며, iPad에서는 여러 씬이 한 번에 화면에 나타날 수 있다.\(예: 팝업 뷰 컨트롤러 사용\) 각 씬에는 씬의 최상위 객체를 나타내는 아이콘을 표시하는 독이 있다. dock는 주로 뷰 컨트롤러와 뷰 사이의 액션 및 아울렛 연결에 사용된다.

스토리보드에서 로드된 모든 객체와 마찬가지로 스토리보드에서 로드된 뷰 컨트롤러를 초기화하려면 [`awakeFromNib`](https://developer.apple.com/documentation/objectivec/nsobject/1402907-awakefromnib)을 오버라이드하라.

### A Segue Manages the Transition Between Two Scenes

전환 타입\(예를 들어, modal 또는 push\)을 세그에 설정할 수 있다. 또한 사용자 정의 전환을 구현하기 위해 세그 객체를 서브 클래싱할 수 있다

[`prepareForSegue:sender:`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621490-prepareforsegue)메서드를 사용하여 씬 간에 데이터를 전달 할 수 있으며, 이 메서드는 세그가 트리거될 때 뷰 컨트롤러에서 호출된다. 이 메서드를 사용하면 다음 뷰 컨트롤러가 화면에 나타나기 전에 사용자 정의 설정을 할 수 있다. 전환은 대개 버튼을 누르는 것과 같은 일부 이벤트의 결과로 발생하지만, 뷰 컨트롤러에서 [`performSegueWithIdentifier:sender:`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621413-performseguewithidentifier) 메서드를 호출하여 프로그래밍 방식으로 전환을 강제할 수 있다.

#### Prerequisite Articles

* \(None\)

#### Related Articles

[Nib file](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34)  
[Object graph](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/ObjectGraph.html#//apple_ref/doc/uid/TP40008195-CH54)

**Definitive Discussion**

Using Storyboards

