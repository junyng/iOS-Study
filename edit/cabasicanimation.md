---
description: 레이어 속성에 기본 단일 키프레임 애니메이션 기능을 제공하는 객체.
---

# CABasicAnimation

### Declaration

```swift
class CABasicAnimation : CAPropertyAnimation
```

### Overview

렌더 트리에서 애니메이션화할 속성의 키 경로를 지정하는 상속된 [`init(keyPath:)`](https://developer.apple.com/documentation/quartzcore/capropertyanimation/1412534-init)메서드를 사용하여 [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation)인스턴스를 생성하라.

예를 들어, [`opacity`](https://developer.apple.com/documentation/quartzcore/calayer/1410933-opacity)와 같은 레이어의 스칼라\(즉, 단일 값을 포함하는\) 특성을 애니메이션화할 수 있다. [Listing 1](https://developer.apple.com/documentation/quartzcore/cabasicanimation#2776772)은 0부터 1까지의 불투명도를 애니메이션화하여 페이드한다.

**Listing 1** 불투명도를 애니메이션화하는 애니메이션 생성

```swift
let animation = CABasicAnimation(keyPath: "opacity") 
animation.fromValue = 0 
animation.toValue = 1
```

백그라운드 컬러와 같은 Non-scalar 속성도 애니메이션화할 수 있다. 코어 애니메이션은 [`fromValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)색과 [`toValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)색 사이에 보간된다. [Listing 2](https://developer.apple.com/documentation/quartzcore/cabasicanimation#2776773)에 생성된 애니메이션은 레이어의 배경 색상을 빨강에서 파랑으로 변색한다.

**Listing 2** 백그라운드 색상을 애니메이션화하는 애니메이션 생성

```swift
let animation = CABasicAnimation(keyPath: "backgroundColor")
animation.fromValue = NSColor.red.cgColor
animation.toValue = NSColor.blue.cgColor
```

값이 다른 non-scalar 속성의 개별 구성요소를 애니메이션화하려면 값을 [`toValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)로 [`fromValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)를 배열로 전달한다. Listing 3에서 만들어진 애니메이션은 레이어를 `(0, 0)`에서 `(100, 100)`으로 이동시킨다.

**Listing 3** 위치를 애니메이션화하는 애니메이션 생성

```swift
let animation = CABasicAnimation(keyPath: "position")
animation.fromValue = [0, 0]
animation.toValue = [100, 100]
```

`keyPath`는 속성의 개별 구성요소에 접근할 수 있다. 예를 들어, [Listing 4](https://developer.apple.com/documentation/quartzcore/cabasicanimation#2776775)는 [`transform`](https://developer.apple.com/documentation/quartzcore/calayer/1410836-transform)객체의 x를 1에서 2로 애니메이션화하여 레이어를 확장한다.

**Listing 4** 크기를 애니메이션화하는 애니메이션 생성

```swift
let animation = CABasicAnimation(keyPath: "transform.scale.x")
animation.fromValue = 1
animation.toValue = 2
```

#### Setting Interpolation Values <a id="1668446"></a>

[`fromValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue), [`byValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue) 및 [`toValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue) 속성은 보간되는 값을 정의한다. 모든 것은 옵셔널이며, 2개 이상은 `non-nil`이 되어서는 안된다. 객체 유형은 애니메이션 중인 속성 유형과 일치해야 한다.

보간 값은 다음과 같이 사용한다:

* [`fromValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)와 [`toValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)는 둘다 non-`nil` 이다. [`fromValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)와 [`toValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue) 사이에 보간한다.
* [`fromValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)와 [`byValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue)는 non-`nil`이다. [`fromValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)와 \([`fromValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue) + [`byValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue)\) 사이에 보간한다.
* byValue와 toValue는 non-`nil`이다. \([`toValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue) - [`byValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue)\)와 [`toValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue) 사이에 보간한다.
* [`fromValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)는 non-`nil`이다. [`fromValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)와 현재 표시 값 사이에 보간한다.
* [`toValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)는 non-`nil`이다. 대상 레이어의 프리젠테이션 계층에서 `keyPath`의 현재 값과 [`toValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue) 사이에 보간한다.
* [`byValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue)는 non-`nil`이다. 대상 레이어의 프리젠테이션 계층에 있는 `keyPath` 의 현재 값과 해당 값 [`byValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue) 사이에 보간한다.
* 모든 속성은 `nil`이다. 대상 레이어의 프리젠테이션 레이어에 있는 이전 `keyPath` 값과 대상 계층의 프리젠테이션 레이어에 있는 `keyPath`의 현재 값 사이에 보간한다.

### Topics

#### Interpolation values

[`var fromValue: Any?`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)

보간을 시작할 때 수신기가 사용하는 값을 정의한다.

[`var toValue: Any?`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)

수신기가 보간을 종료하는 데 사용하는 값을 정의한다.

[`var byValue: Any?`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue)

수신기가 관련 보간을 수행하는 데 사용하는 값을 정의한다.

