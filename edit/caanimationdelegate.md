---
description: 애니메이션이 시작되고 중지될 때 앱이 응답하도록 구현할 수 있는 메서드.
---

# CAAnimationDelegate

### Declaration

```swift
protocol CAAnimationDelegate
```

### Overview

애니메이션 시작 또는 종료시 애니메이션 델리게이트를 사용하여 추가 로직을 실행할 수 있다. 예를 들어, 페이드 아웃 애니메이션이 완료되면 부모로부터 레이어를 제거할 수 있다.

[Listing 1](https://developer.apple.com/documentation/quartzcore/caanimationdelegate#2776468)은 [`CAAnimationDelegate`](https://developer.apple.com/documentation/quartzcore/caanimationdelegate)를 구현하는 클래스에서 가져온 코드를 보여주며, sublayer라는 레이어가 그 레이어에 추가되었다. fadeOut 함수는 sublayer의 불투명도 애니메이션을 애니메이션화하며 [`animationDidStop(_:finished:)`](https://developer.apple.com/documentation/quartzcore/caanimationdelegate/2097259-animationdidstop)이 완료되면 슈퍼레이어에서 이를 제거한다.

**Listing 1** 애니메이션 완료 후 레이어 제거하기

```swift
func fadeOut() {
    let fadeOutAnimation = CABasicAnimation()
    fadeOutAnimation.keyPath = "opacity"
    fadeOutAnimation.fromValue = 1
    fadeOutAnimation.toValue = 0
    fadeOutAnimation.duration = 0.25
    
    fadeOutAnimation.delegate = self
    
    sublayer.add(fadeOutAnimation,
                      forKey: "fade")
}
    
func animationDidStop(_ anim: CAAnimation, finished flag: Bool) {
    sublayer.removeFromSuperlayer()
}
```

### Topics

#### Customizing Start and Stop Times

* [`func animationDidStart(CAAnimation)`](https://developer.apple.com/documentation/quartzcore/caanimationdelegate/2097265-animationdidstart) 델리게이트에게 애니메이션이 시작되었음을 알린다.
* [`func animationDidStop(CAAnimation, finished: Bool)`](https://developer.apple.com/documentation/quartzcore/caanimationdelegate/2097259-animationdidstop)  애니메이션이 종료되었음을 델리게이트에 알린다.

