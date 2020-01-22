---
description: 배경색 위에 색상 그라디언트를 그려서 레이어의 모양을 채우는 레이어 (둥근 모서리 포함)
---

# CAGradientLayer

### Declaration

```swift
class CAGradientLayer : CALayer
```

### Overview

그라디언트 레이어를 사용하여 임의의 수의 색상을 포함하는 색상 그라디언트를 만든다. 기본적으로 색상은 레이어 전체에 균일하게 분산되지만 그라디언트를 통해 색상 위치를 제어 할 위치를 선택적으로 지정할 수 있다.

[Listing 1](https://developer.apple.com/documentation/quartzcore/cagradientlayer#2825191)은 그라디언트를 통해 고르게 분포된 4가지 색상을 포함하는 그라디언트 레이어를 만드는 방법을 보여준다. 레이어를 90 \([`pi`](https://developer.apple.com/documentation/coregraphics/cgfloat/1845230-pi) / 2 라디안\) 회전 시키면 수평 그라데이션이 나타난다.

**Listing 1** Creating a gradient layer

```swift
gradientLayer.colors = [UIColor.red.cgColor,
                        UIColor.yellow.cgColor,
                        UIColor.green.cgColor,
                        UIColor.blue.cgColor]
     
gradientLayer.transform = CATransform3DMakeRotation(CGFloat.pi / 2, 0, 0, 1)
```

[Figure 1](https://developer.apple.com/documentation/quartzcore/cagradientlayer#2825193)은 그라디언트의 모양을 보여준다.

**Figure 1** Color gradient layer

### Topics

#### Gradient Style Properties

[`var colors: [Any]?`](https://developer.apple.com/documentation/quartzcore/cagradientlayer/1462403-colors)

각 그라디언트 정지 점의 색상을 정의하는 `CGColorRef` 객체의 배열. 애니메이션에 적합하다.

[`var locations: [NSNumber]?`](https://developer.apple.com/documentation/quartzcore/cagradientlayer/1462410-locations)

각 그라데이션 중지 점의 위치를 정의하는 NSNumber 객체의 옵셔널 배열. 애니메이션에 적합하다.

[`var endPoint: CGPoint`](https://developer.apple.com/documentation/quartzcore/cagradientlayer/1462412-endpoint)

레이어의 좌표 공간에서 그릴 때 그라디언트의 끝점. 애니메이션에 적합하다.

[`var startPoint: CGPoint`](https://developer.apple.com/documentation/quartzcore/cagradientlayer/1462408-startpoint)

레이어의 좌표 공간에서 그릴 때 그라디언트의 시작점. 애니메이션에 적합하다.

[`var type: CAGradientLayerType`](https://developer.apple.com/documentation/quartzcore/cagradientlayer/1462413-type)

레이어로 그린 그라디언트 스타일.

#### Constants

[Gradient Types](https://developer.apple.com/documentation/quartzcore/cagradientlayer/gradient_types)

레이어로 그린 그라디언트 스타일.

