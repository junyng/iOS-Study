---
description: 레이어 객체에 키프레임 애니메이션 기능을 제공하는 객체.
---

# CAKeyframeAnimation

### Declaration

```swift
class CAKeyframeAnimation : CAPropertyAnimation
```

### Overview

상속된 [`init(keyPath:)`](https://developer.apple.com/documentation/quartzcore/capropertyanimation/1412534-init) 메서드를 사용하여 [`CAKeyframeAnimation`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation) 객체를 생성하여 레이어에서 애니메이션할 속성의 키 경로를 지정한다. 그런 다음 타이밍과 애니메이션 동작을 제어하는 데 사용할 키프레임 값을 지정할 수 있다.

대부분의 애니메이션 유형의 경우 및 속성을 사용하여 [`values`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412498-values) 및 [`keyTimes`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412522-keytimes) 키프레임 값을 지정하라. 애니메이션 중에 Core Animation은 제공한 값 사이를 보간하여 중간 값을 생성한다. 레이어의 위치와 같이 좌표점인 값을 애니메이션화할 때, 개별 값 대신 해당 점에 대한 [`path`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412474-path)를 지정할 수 있다. 애니메이션의 페이싱은 당신이 제공하는 타이밍 정보에 의해 제어된다.

[Listing 1](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation#2826916)은 2초 동안 빨간색에서 녹색, 파란색까지 레이어의 배경색을 애니메이션으로 만드는 방법을 보여준다.

**Listing 1** 키프레임 애니메이션을 사용하여 레이어의 배경색 애니메이션 실행

```swift
let colorKeyframeAnimation = CAKeyframeAnimation(keyPath: "backgroundColor")

colorKeyframeAnimation.values = [UIColor.red.cgColor,
                                 UIColor.green.cgColor,
                                 UIColor.blue.cgColor]
colorKeyframeAnimation.keyTimes = [0, 0.5, 1]
colorKeyframeAnimation.duration = 2
```

### Topics

#### Providing keyframe values

* [`var values: [Any]?`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412498-values) 애니메이션에 사용할 키프레임 값을 지정하는 객체 배열.
* [`var path: CGPath?`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412474-path) 점-기반 속성이 따르는 경로.

#### Keyframe timing

* [`var keyTimes: [NSNumber]?`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412522-keytimes) 주어진 키프레임 세그먼트를 적용할 시간을 정의하는 `NSNumber` 객체의 옵셔널 배열.
* [`var timingFunctions: [CAMediaTimingFunction]?`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412465-timingfunctions) 각 키프레임 세그먼트의 페이싱\(pacing\)을 정의하는 `CAMediaTimingFunction` 객체의 옵셔널 배열.
* [`var calculationMode: CAAnimationCalculationMode`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412500-calculationmode)  수신기가 중간 키프레임 값을 계산하는 방법을 지정한다.

#### Rotation Mode Attribute

* [`var rotationMode: CAAnimationRotationMode?`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412454-rotationmode) 경로를 따라 애니메이션화하는 객체가 경로 접선과 일치하도록 회전하는지 여부.

#### Cubic Mode Attributes

* [`var tensionValues: [NSNumber]?`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412475-tensionvalues) 일련의 [`NSNumber`](https://developer.apple.com/documentation/foundation/nsnumber) 커브의 tightness를 정의하는 객체.
* [`var continuityValues: [NSNumber]?`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412491-continuityvalues) 일련의 [`NSNumber`](https://developer.apple.com/documentation/foundation/nsnumber) 타이밍 곡선의 모서리의 선명도를 정의하는 객체.
* [`var biasValues: [NSNumber]?`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412485-biasvalues) 일련의 [`NSNumber`](https://developer.apple.com/documentation/foundation/nsnumber) 제어점에 상대적인 원곡선의 위치를 정의하는 객체.

#### Constants

* [Rotation Mode Values](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/rotation_mode_values) 이 상수는 [`rotationMode`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412454-rotationmode) 속성으로 사용된다.
* [Value calculation modes](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/value_calculation_modes) 이 상수는 [`calculationMode`](https://developer.apple.com/documentation/quartzcore/cakeyframeanimation/1412500-calculationmode) 속성으로 사용된다.



