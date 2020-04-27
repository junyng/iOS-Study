---
description: 플레이어의 시각적 출력을 관리하는 객체
---

# AVPlayerLayer

### Declaration

```swift
class AVPlayerLayer : CALayer
```

### Overview

iOS 또는 tvOS에서 `AVPlayerLayer`를 사용하는 편리한 방법은 다음 코드 예제와 같이 `UIView`의 백업 레이어이다:

```swift
class PlayerView: UIView {
    var player: AVPlayer? {
        get {
            return playerLayer.player
        }
        set {
            playerLayer.player = newValue
        }
    }
    
    var playerLayer: AVPlayerLayer {
        return layer as! AVPlayerLayer
    }
    
    // Override UIView property
    override static var layerClass: AnyClass {
        return AVPlayerLayer.self
    }
}
```

> 중요  
> 플레이어 레이어의 상속된 `contents` 속성의 값은 불투명하므로 사용해서는 안된다.

