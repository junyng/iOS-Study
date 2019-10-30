# View hierarchy

뷰 계층은 윈도우의 뷰 간의 상호 관계를 정의한다. 뷰 계층 구조를 트리의 상단 노드인 윈도우와 함께 반전 트리 구조로 생각할 수 있다. 그 아래에는 구조적으로 부모-자식 관계에 의해 지정된 뷰가 나온다. 시각적 관점에서 뷰 계층의 본질적인 사실은 둘러싸는 것이다: 하나의 뷰는 하나 이상의 다른 뷰를 포함하며, 윈도우는 모두 포함한다.

뷰 계층은 응답자 체인의 주요 부분이며, 애플리케이션 프레임워크가 drawing pass에서 윈도우 내용을 렌더링할 때 뷰의 계층화 순서를 결정하는 데 사용하는 것이다. 뷰 계층은 뷰 구성 뒤에 있는 관리 개념이기도 하다: 슈퍼 뷰에 하위 뷰를 추가하여 복합 뷰를 구성한. 마지막으로, 뷰 계층은 윈도우에서 발견된 여러 좌표계의 핵심적인 요소이다.

![](../../.gitbook/assets/view_hierarchy_enclose.jpg)

### Three View Properties Define Relationships in the Hierarchy

뷰는 두 가지 프로퍼티를 통해 다른 뷰와 연관되며, 이러한 관계는 계층의 형태를 결정한다:

* [`superview`](https://developer.apple.com/documentation/uikit/uiview/1622474-superview) - 계층에서 지정된 뷰 위에 있는 뷰; 이것이 해당 뷰를 둘러싸는 뷰이다. 맨 위 뷰를 제외한 모든 뷰에는 슈퍼뷰가 있어야 한다.
* [`subviews`](https://developer.apple.com/documentation/uikit/uiview/1622614-subviews) - 계층에서 지정된 뷰 아래의 뷰. 이들은 둘러싸여 있는 뷰이다. 뷰는 몇 개의 하위 뷰를 가질 수 있으며, 또는 뷰가 없을 수 있다.

![](../../.gitbook/assets/view_hierarchy_relationships.jpg)

뷰는 또한 윈도우를 식별하는 다른 프로퍼티도 포함한다.

### In iOS, a Window is a View

OS X에서 윈도우는 구조적으로 계층의 다른 모든 뷰가 아래로 내려가는 하나의 "콘텐츠 뷰"를 가지고 있다. 그러나 iOS 애플리케이션에서 윈도우는 뷰\([`UIWindow`](https://developer.apple.com/documentation/uikit/uiwindow)는 [`UIView`](https://developer.apple.com/documentation/uikit/uiview)로 부터 상속됨\)이므로 자체 콘텐츠 뷰로 작용한다.

#### Prerequisite Articles

[View object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/ViewObject.html#//apple_ref/doc/uid/TP40009071-CH5-SW1)  
[Declared property](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/DeclaredProperty.html#//apple_ref/doc/uid/TP40008195-CH13)

**Related Articles**

[Responder object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Responder.html#//apple_ref/doc/uid/TP40009071-CH1-SW1)  
[Drawing model](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/DrawingModel.html#//apple_ref/doc/uid/TP40009071-CH9-SW1)  
[Coordinate system](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/CoordinateSystem.html#//apple_ref/doc/uid/TP40009071-CH8-SW1)

**Definitive Discussion**

The View Hierarchy

**Sample Code Projects**

\(None\)

