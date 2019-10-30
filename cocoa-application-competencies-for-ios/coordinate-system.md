# Coordinate system

좌표계는 애플리케이션의 가시적 객체를 배치, 크기 조정, 변환 및 그리고 사용자 이벤트를 찾는 2차원 공간이다. iOS와 OS X에 애플리케이션은 공통 원점\(0.0, 0.0\)에서 교차하는 수평축과 수직축\(즉, x축과 y축\)을 사용하여 점을 찾는 좌표계에 의존한다. 원점에서 양수 값은 두어느 한 축을 따라 한 방향으로 증가하고, 음수 값은 반대 방향으로 증가한다. 이 좌표 공간의 점을 픽셀과 같은 장치 공간에 고정되지 않은 사용자-공간 단위의 부동 소수 쌍으로 표현한다. 이는 픽셀과 같은 장치 공간에 있는 장치에 고정되지 않는다. 그리기는 거의 항상 x축과 y축 값이 모두 양인 좌표공간의 영역에서 발생한다.

### Coordinate Systems Can Have Different Drawing Orientations

iOS 및 OS X의 뷰에 대한 기본 좌표계는 수직축 방향이 다르다:

* **OS X.** 기본 좌표계는 그리기 영역의 왼쪽 하단에 원점이 있으며, 양수 값은 도면으로부터 위로 오른쪽으로 확장된다. OS X에서 뷰의 좌표계를 프로그래밍 방식으로 "뒤집기" 할 수 있다.
* **iOS.** 기본 좌표계에 그리기 영역의 왼쪽 상단에 원점이 있으며, 양수 값은 도면으로부터 아래쪽, 오른쪽으로 확장된. iOS에서 뷰 좌표계의 기본 방향을 변경할 수 없다. 즉 뷰의 "뒤집기"를 할 수 없다.

![](../../.gitbook/assets/flipped_coordinates.jpg)

### Windows and Views Have Their Own Coordinate Systems

애플리케이션은 언제든지 여러 좌표계를 가지고 있다. 윈도우는 화면 좌표로 배치되고 크기가 조정되어 디스플레이 좌표계로 정의된다. 윈도우 자체는 뷰에 의해 수행되는 모든 그리기와 이벤트 처리를 위한 기본 좌표계를 나타낸다. 윈도우의 각 뷰는 그 자체를 그리기 위해 자체적인 로컬 좌표계를 유지하며, 이 좌표계는 뷰의 `bounds` 프로퍼티에 의해 정의된다. 뷰의 `frame` 프로퍼티는 슈퍼뷰에서의 좌표계 위치 및 크기를 표현하며,  하위 뷰의 위치 및 크기를 위한 기본 좌표계를 제공한다.

AppKit과 UIKit 프레임워크는 모두 뷰와 다른 뷰, 뷰와 윈도우, \(OS X에서\) 화면과 윈도우의 좌표계 사이의 점과 직사각형을 변환하는 메서드를 제공한다. 애플리케이션은 윈도우의 좌표계에 마우스, 태블릿, 제스처 및 멀티 터치 이벤트를 위치시키지만, 뷰는 이러한 이벤트를 로컬 좌표계로 쉽게 변환할 수 있다.

변환이라고 알려진 2차원 수학적 배열을 사용하여 좌표 공간 간에 점을 매핑할 수도 있다. 변환을 이용하면 2차원 공간에서 쉽게 콘텐츠를 크기조정, 회전 및 변환할 수 있다.

#### Prerequisite Articles

[View object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/ViewObject.html#//apple_ref/doc/uid/TP40009071-CH5-SW1)

**Related Articles**

[Drawing model](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/DrawingModel.html#//apple_ref/doc/uid/TP40009071-CH9-SW1)  
[Window object](https://developer.apple.com/library/archive/documentation/General/Conceptual/Devpedia-CocoaApp/Window.html#//apple_ref/doc/uid/TP40009071-CH6-SW1)

**Definitive Discussion**

[Converting Coordinates in the View Hierarchy](https://developer.apple.com/library/archive/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/CreatingViews/CreatingViews.html#//apple_ref/doc/uid/TP40009503-CH5-SW40)

**Sample Code Projects**

[TheElements](https://developer.apple.com/library/archive/samplecode/TheElements/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007419)

