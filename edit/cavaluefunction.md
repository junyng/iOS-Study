---
description: 애니메이션 트랜지션을 정의하는 유연한 메서드를 제공하는 객체.
---

# CAValueFunction

### Declaration

```swift
class CAValueFunction : NSObject
```

### Overview

값 함수를 사용하여 애니메이션 트랜지션의 개별 구성요소를 지정할 수 있다.

예를 들어, z축을 중심으로 레이어를 회전하는 기본 애니메이션을 만들려면 [`fromValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue) 는 0, [`toValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue) 는 [`pi`](https://developer.apple.com/documentation/swift/float/1845969-pi) , [`valueFunction`](https://developer.apple.com/documentation/quartzcore/capropertyanimation/1412447-valuefunction) 는 [`CAValueFunction`](https://developer.apple.com/documentation/quartzcore/cavaluefunction)과 함께 [`rotateZ`](https://developer.apple.com/documentation/quartzcore/cavaluefunctionname/1521885-rotatez)의 함수 이름을 가진 [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) 객체를 생성한다.

[Listing 1](https://developer.apple.com/documentation/quartzcore/cavaluefunction#2776424)은 이러한 회전을 만들어 rotatingLayer라는 [`CALayer`](https://developer.apple.com/documentation/quartzcore/calayer)에 적용하는 방법을 보여준다.

**Listing 1** 레이어의 회전 애니메이션화하기

```swift
let rotateAnimation = CABasicAnimation()
rotateAnimation.valueFunction = CAValueFunction(name: kCAValueFunctionRotateZ)
rotateAnimation.fromValue = 0
rotateAnimation.toValue = Float.pi
rotateAnimation.duration = 3
rotatingLayer.add(rotateAnimation,
                  forKey: "transform")
```

값 함수의 스케일 및 트랜지션에는 x, y 및 z 구성요소에 대해 3개의 값이 필요하다. 이러한 값 함수로 작업할 때 애니메이션의 [`fromValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue) 및 [`toValue`](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)를 배열로 지정하라.

[Listing 2](https://developer.apple.com/documentation/quartzcore/cavaluefunction#2776425)는 값 함수를 사용하여 0부터 1까지의 레이어 크기를 어떻게 애니메이션화할 수 있는지 보여준다.

**Listing 2** 레이어의 크기 애니메이션화하기

```swift
let scaleAnimation = CABasicAnimation()
scaleAnimation.valueFunction = CAValueFunction(name: kCAValueFunctionScale)
scaleAnimation.fromValue = [0, 0, 0]
scaleAnimation.toValue = [1, 1, 1]
scaleAnimation.duration = 3
scalingLayer.add(scaleAnimation,
                 forKey: "transform")
```

### Topics

#### Getting Value Function Properties

* [`var name: CAValueFunctionName`](https://developer.apple.com/documentation/quartzcore/cavaluefunction/1521888-name) 값 함수의 이름을 반환한다.

#### Creating and Initializing Value Functions

* [`init?(name: CAValueFunctionName)`](https://developer.apple.com/documentation/quartzcore/cavaluefunction/1522115-init) 이름으로 식별된 값 함수 객체를 반환한다.

#### Constants

* [Rotate Value Functions](https://developer.apple.com/documentation/quartzcore/cavaluefunction/rotate_value_functions) 회전 값 변환 함수는 해당 회전 행렬을 나타내는 4x4 행렬을 생성한다.
* [Scale Value Functions](https://developer.apple.com/documentation/quartzcore/cavaluefunction/scale_value_functions) 크기 값 변환 함수는 해당 스케일 행렬을 나타내는 4x4 행렬을 생성한다.
* [Translate Functions](https://developer.apple.com/documentation/quartzcore/cavaluefunction/translate_functions) 변환 값 변환 함수는 해당 변환 행렬을 나타내는 4x4 행렬을 생성ㅎ나다.

