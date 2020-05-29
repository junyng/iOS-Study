---
description: 스프링 시스템이 정지된 상태에서 고려되는 데 필요한 예상 지속 시간.
---

# settlingDuration

### Declaration

```swift
var settlingDuration: CFTimeInterval { get }
```

### Discussion

은 현재 애니메이션 파라미터에 대해 평가되며, [`duration`](https://developer.apple.com/documentation/quartzcore/camediatiming/1427652-duration)과 동일하지 않을 수 있다.

[Listing 1](https://developer.apple.com/documentation/quartzcore/caspringanimation/1412524-settlingduration#2760148)을 나열하면 2초의 [`duration`](https://developer.apple.com/documentation/quartzcore/camediatiming/1427652-duration)을 가진 스프링 애니메이션이 생성된다.

**Listing 1** 스프링 애니메이션 생성

```swift
let spring = CASpringAnimation()

spring.keyPath = "position.x"
spring.fromValue = 0
spring.toValue = 640
spring.damping = 5
spring.duration = 2
```

damping 계수가 5인 경우, 안착 지속 시간은 약 2.85초이다: 안착하기 전에 애니메이션 레이어가 타겟 위치 주위를 여러 번 돈다. 그러나 damping 속성을 15로 변경하면 안착 지속시간이 1초 이상으로 단축된다. 즉, 애니메이션 레이어가 타겟 위치에 도달하면 빠르게 정지한다.

모든 스프링 애니메이션의 물리적 속성: damping, initialVelocity, mass 및 stiffness 등은 정착 지속시간에 영향을 미칠 수 있다.

### See Also

#### Configuring Physical Attributes

* [`var damping: CGFloat`](https://developer.apple.com/documentation/quartzcore/caspringanimation/1412532-damping) 마찰력으로 인해 스프링의 움직임을 감쇠시키는 방법을 규정한다.
* [`var initialVelocity: CGFloat`](https://developer.apple.com/documentation/quartzcore/caspringanimation/1412443-initialvelocity) 스프링에 부착된 물체의 초기 속도.
* [`var mass: CGFloa`](https://developer.apple.com/documentation/quartzcore/caspringanimation/1412540-mass) 스프링 끝에 부착된 물체의 질량.
* [`var stiffness: CGFloat`](https://developer.apple.com/documentation/quartzcore/caspringanimation/1412515-stiffness) 스프링 강성 계수.

