---
description: 스프링과 같은 힘을 레이어의 속성에 적용하는 애니메이션.
---

# CASpringAnimation

### Declaration

```swift
class CASpringAnimation : CABasicAnimation
```

### Overview

일반적으로 스프링 애니메이션을 사용하여 레이어의 위치를 애니메이션화하여 스프링에 의해 타겟을 당겨지는 것처럼 보이도록 할 수 있다. 레이어가 타겟으로부터 멀어질수록, 그것을 향한 가속도는 커진다.

[`CASpringAnimation`](https://developer.apple.com/documentation/quartzcore/caspringanimation)은 스프링의 감쇠 및 강성과 같은 물리적 기반 속성을 제어할 수 있다.

스프링 애니메이션을 사용하여 위치가 아닌 레이어의 애니메이션 속성을 사용할 수 있다. [Listing 1](https://developer.apple.com/documentation/quartzcore/caspringanimation#2826917)은 0에서 1까지의 크기를 애니메이션화하여 레이어를 시야에 튕겨내는 스프링 애니메이션의 제작 방법을 보여준다. 스프링 애니메이션은 그것의 [`toValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)를 오버슈팅할 수 있기 때문에 애니메이션화된 레이어는 그것의 프레임을 초과할 수 있다.

**Listing 1** 스프링 애니메이션을 사용하여 레이어 크기 애니메이션화 하기

```swift
let springAnimation = CASpringAnimation(keyPath: "transform.scale")

springAnimation.fromValue = 0
springAnimation.toValue = 1
```

### Topics

#### Configuring Physical Attributes

* [`var damping: CGFloat`](https://developer.apple.com/documentation/quartzcore/caspringanimation/1412532-damping) 마찰력으로 인해 스프링의 움직임을 감쇠시키는 방법을 정의한다.
* [`var initialVelocity: CGFloat`](https://developer.apple.com/documentation/quartzcore/caspringanimation/1412443-initialvelocity) 스프링에 부착된 객체의 초기 속도.
* [`var mass: CGFloat`](https://developer.apple.com/documentation/quartzcore/caspringanimation/1412540-mass) 스프링의 끝단에 부착된 객체의 질량.
* [`var settlingDuration: CFTimeInterval`](https://developer.apple.com/documentation/quartzcore/caspringanimation/1412524-settlingduration) 스프링 시스템이 정지 상태에서 고려되는 데 필요한 예상 지속시간.
* [`var stiffness: CGFloat`](https://developer.apple.com/documentation/quartzcore/caspringanimation/1412515-stiffness) 스프링 강성 계수.

