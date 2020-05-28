---
description: 레이어 상태 간의 애니메이션 트랜지션을 제공하는 객체.
---

# CATransition

### Declaration

```swift
class CATransition : CAAnimation
```

### Overview

[`CATransition`](https://developer.apple.com/documentation/quartzcore/catransition) 객체를 생성 및 추가함으로써 레이어 상태 간에 전환할 수 있다. 기본 전환은 크로스 페이드지만, 미리 정의된 트랜지션 집합과 다른 효과를 지정할 수 있다.

[Listing 1](https://developer.apple.com/documentation/quartzcore/catransition#2776806)은 `transitioningLayer`라는 [`CATextLayer`](https://developer.apple.com/documentation/quartzcore/catextlayer) 의 두 가지 상태 사이에서 전환할 수 있는 방법을 보여준다. 레이어가 처음 생성되면 [`backgroundColor`](https://developer.apple.com/documentation/quartzcore/calayer/1410966-backgroundcolor)가 빨간색으로 설정되고 [`string`](https://developer.apple.com/documentation/quartzcore/catextlayer/1515295-string) 속성이 Red로 설정된다. `runTransition()` 함수를 호출하면 새로운 [`CATransition`](https://developer.apple.com/documentation/quartzcore/catransition) 객체가 생성되고 `transitioningLayer`에 추가된다. 그리고 레이어의 상태는 배경 색상이 파란색이고 렌더링된 텍스트가 Blue로 읽히도록 변경된다.

최종 결과는 푸시 트랜지션이 왼쪽에서 오른쪽으로 빨강 상태를 왼쪽에서 오른쪽으로 진입하는 파란색 상태로 애니메이션화한다는 것이다.

**Listing 1** 레이어의 상태 간의 트랜지션

```swift
let transitioningLayer = CATextLayer()
     
override func viewDidLoad() {
    super.viewDidLoad()
    transitioningLayer.frame = CGRect(x: 10, y: 10,
                                      width: 320, height: 160)
    
    view.layer.addSublayer(transitioningLayer)
    
    // Initial "red" state
    transitioningLayer.backgroundColor = UIColor.red.cgColor
    transitioningLayer.string = "Red"
}
      
   
func runTransition() {
    let transition = CATransition()
    transition.duration = 2
    
    transition.type = kCATransitionPush
    
    transitioningLayer.add(transition,
                           forKey: "transition")
    
    // Transition to "blue" state
    transitioningLayer.backgroundColor = UIColor.blue.cgColor
    transitioningLayer.string = "Blue"
}
```

### Topics

#### Transition start and end point

* [`var startProgress: Float`](https://developer.apple.com/documentation/quartzcore/catransition/1412511-startprogress) 수신기의 시작점을 전체 트랜지션의 일부로 표시한다.
* [`var endProgress: Float`](https://developer.apple.com/documentation/quartzcore/catransition/1412509-endprogress) 수신기의 끝점을 전체 트랜지션의 일부로 표시한다.

#### Transition Properties

* [`var type: CATransitionType`](https://developer.apple.com/documentation/quartzcore/catransition/1412502-type) 미리 정의된 트랜지션 타입을 지정하라.
* [`var subtype: CATransitionSubtype?`](https://developer.apple.com/documentation/quartzcore/catransition/1412467-subtype) 미리 정의된 모션-기반 트랜지션 방향을 나타내는 옵셔널 서브타입을 지정하라.

#### Custom transition filter

* [`var filter: Any?`](https://developer.apple.com/documentation/quartzcore/catransition/1412506-filter) 트랜지션을 제공하는 옵셔널 Core Image 필터 객체이다.

#### Constants

* [Common Transition Types](https://developer.apple.com/documentation/quartzcore/catransition/common_transition_types) 이 상수는 [`type`](https://developer.apple.com/documentation/quartzcore/catransition/1412502-type) 속성과 함께 사용되는 트랜지션 타입을 지정한다.
* [Common Transition Subtypes](https://developer.apple.com/documentation/quartzcore/catransition/common_transition_subtypes) 이 상수는 모션-기반 트랜지션의 방향을 지정한다. 그것들은 [ `subtype`](https://developer.apple.com/documentation/quartzcore/catransition/1412467-subtype)  속성과 함께 사용된다.

