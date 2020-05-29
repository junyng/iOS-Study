---
description: 두 레이어 사이의 단일 레이아웃 제약 조건의 표현.
---

# CAConstraint

### Declaration

```swift
class CAConstraint : NSObject
```

### Overview

각 인스턴스는 [`CAConstraint`](https://developer.apple.com/documentation/quartzcore/caconstraint) 동일한 축에 있는 두 레이어 사이의 하나의 지오메트리 관계를 캡슐화한다.

형제 레이어는 각 레이어의 이름 속성을 사용하여 이름별로 참조된다. 특수 이름 `superlayer`는 레이어의 슈퍼레이어를 가리키는데 사용된다.

예를 들어, 레이어를 슈퍼뷰에서 수평 중간에 지정하려면 다음을 사용하라:

```swift
let theConstraint = CAConstraint(attribute: .midX,
                                 relativeTo: "superlayer",
                                 attribute: .midX)
```

축당 최소 두 개의 관계를 지정해야 한다. 레이어의 왼쪽 가장자리와 오른쪽 가장자리의 제약조건을 지정하면 너비가 달라진다. 왼쪽 가장자리와 너비에 대한 제약조건을 지정하면 레이어의 오른쪽 가장자리가 슈퍼레이어의 프레임에 대해 이동한다. 종종 단일 가장자리 제약조건만 지정하면 동일한 축에 있는 레이어의 크기가 두 번째 관계로 사용된다.

> **중요**
>
> 동일한 속성에 대한 순환 참조를 초래하는 제약조건을 만들 수 있다. 레이아웃을 계산할 수 없는 경우 동작은 정의되지 않는다.

### Topics

#### Create a New Constraint

* [`init(attribute: CAConstraintAttribute, relativeTo: String, attribute: CAConstraintAttribute, offset: CGFloat)`](https://developer.apple.com/documentation/quartzcore/caconstraint/1522328-init)  지정된 지정된 파라미터를 사용하여 `CAConstraint` 객체 생성 및 반환.
* [`init(attribute: CAConstraintAttribute, relativeTo: String, attribute: CAConstraintAttribute)`](https://developer.apple.com/documentation/quartzcore/caconstraint/1521924-init)  지정된 지정된 파라미터를 사용하여 `CAConstraint` 객체 생성 및 반환.
* [`init(attribute: CAConstraintAttribute, relativeTo: String, attribute: CAConstraintAttribute, scale: CGFloat, offset: CGFloat)`](https://developer.apple.com/documentation/quartzcore/caconstraint/1522213-init)  지정된 지정된 파라미터를 사용하여 `CAConstraint` 객체 생성 및 반환. 지정 이니셜라이저.

#### Accessing Constraint Values

* [`var attribute: CAConstraintAttribute`](https://developer.apple.com/documentation/quartzcore/caconstraint/1522186-attribute) 제약 조건이 영향을 미치는 속성.
* [`var offset: CGFloat`](https://developer.apple.com/documentation/quartzcore/caconstraint/1522142-offset) 제약 조건 속성의 간격띄우기 값.
* [`var scale: CGFloat`](https://developer.apple.com/documentation/quartzcore/caconstraint/1521911-scale) 제약 조건의 스케일 팩터.
* [`var sourceAttribute: CAConstraintAttribute`](https://developer.apple.com/documentation/quartzcore/caconstraint/1522385-sourceattribute) 수신기가 상대적으로 계산되는 레이어의 제약 조건 속성.
* [`var sourceName: String`](https://developer.apple.com/documentation/quartzcore/caconstraint/1522224-sourcename) 제약 조건이 상대적으로 계산되는 레이어 이름.

#### Constants

* [`enum CAConstraintAttribute`](https://developer.apple.com/documentation/quartzcore/caconstraintattribute) 제약 조건 속성 타입.

