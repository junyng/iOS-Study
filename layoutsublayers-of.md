---
description: 델리게이트에게 하나의 레이어의 바운드가 바뀌었다고 말한다.
---

# layoutSublayers\(of:\)

### 정의

```swift
optional func layoutSublayers(of layer: CALayer)
```

### 인자

**`layer`**

하위 레이어의 레이아웃이 필요한 레이어

### 논의

[`layoutSublayers(of:)`](https://developer.apple.com/documentation/quartzcore/calayerdelegate/2097257-layoutsublayers) 메서드는 레이어 바운드가 변경되고 해당 하위 레이어가 프레임 크기를 변경하여 재배치가 필요할 때 호출된다. 레이어의 하위 레이어의 레이아웃에 대한 정확한 제어가 필요한 경우 이 메서드를 구현할 수 있다.

[**목** 1](https://developer.apple.com/documentation/quartzcore/calayerdelegate/2097257-layoutsublayers#2776564)은 CALayerDelegate를 구현하는 LayerDelegate라는 클래스를 생성하며 레이어의 \(하위 레이어 이름의\) 델리게이트로 설정하는 방법을 보여준다. 레이어의 크기가 변경되면, 델리게이트의 [`layoutSublayers(of:)`](https://developer.apple.com/documentation/quartzcore/calayerdelegate/2097257-layoutsublayers)는 하위 레이어의 모든 하위 레이어에 반복하여 레이어 내에 맞도록 크기를 조정한다.

**목록 1** 레이어의 하위 레이어 배치.

```swift
let delegate = LayerDelegate()
    
lazy var sublayer: CALayer = {
    let layer = CALayer()
    
    layer.addSublayer(CALayer())
    layer.sublayers?.first?.backgroundColor = UIColor.blue.cgColor
    
    layer.delegate = self.delegate
    
    return layer
}()
    
// sublayer.frame = CGRect(x: 0, y: 0, width: 510, height: 510)
    
class LayerDelegate: NSObject, CALayerDelegate {
    func layoutSublayers(of layer: CALayer) {
        layer.sublayers?.forEach {
            $0.frame = layer.bounds
        }
    }
}
```

