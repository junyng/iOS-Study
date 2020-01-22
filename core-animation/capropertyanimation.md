---
description: 레이어 속성의 값을 조작하는 애니메이션을 만들기 위한 CAAnimation의 추상 하위 클래스.
---

# CAPropertyAnimation

### Declaration

```swift
class CAPropertyAnimation : CAAnimation
```

### Overview

애니메이션에 대한 속성은 애니메이션을 사용하여 레이어에 상대적인 키 경로를 사용하여 지정된다.

[`CAPropertyAnimation`](https://developer.apple.com/documentation/quartzcore/capropertyanimation) 인스턴스를 생성하지 마라: `Core Animation` 레이어의 속성을 애니메이션화하려면 [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) 또는 [`CAKeyframeAnimation`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation)의 구체적인 하위 클래스 인스턴스를 생성하라.

### Topics

#### Animated Key Path

[`var keyPath: String?`](https://developer.apple.com/documentation/quartzcore/capropertyanimation/1412496-keypath)

수신기가 애니메이션화하는 키 경로를 지정한다.

#### Property Value Calculation Behavior

[`var isCumulative: Bool`](https://developer.apple.com/documentation/quartzcore/capropertyanimation/1412538-iscumulative)

속성의 값이 이전 반복 주기가 종료될 때의 값과 현재 반복 주기의 값인지 여부를 결정한다.

[`var isAdditive: Bool`](https://developer.apple.com/documentation/quartzcore/capropertyanimation/1412493-isadditive)

애니메이션에서 지정한 값을 현재 렌더 트리 값에 추가하여 새 렌더 트리 값을 생성할지 여부를 결정한다.

[`var valueFunction: CAValueFunction?`](https://developer.apple.com/documentation/quartzcore/capropertyanimation/1412447-valuefunction)

보간 값에 적용되는 옵션 값 함수.

#### Creating an Animation

[`init(keyPath: String?)`](https://developer.apple.com/documentation/quartzcore/capropertyanimation/1412534-init)

지정된 키경로에 대한 `CAPropertyAnimation` 인스턴스를 생성하고 반환한다.

