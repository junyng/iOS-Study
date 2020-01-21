---
description: 특정 플레이어 항목과 동기화하는 데 사용되는 객체.
---

# AVSynchronizedLayer

### Declaration

```swift
class AVSynchronizedLayer : CALayer
```

### Overview

동일한 `AVPlayerItem` 객체에서 임의 수의 동기화된 레이어를 생성할 수 있다.

동기화된 레이어는 어떤 것도 표시하지 않고 단지 레이어 하위 트리 위에서 상태를 제공한다는 점에서 [`CATransformLayer`](https://developer.apple.com/documentation/quartzcore/catransformlayer) 객체와 유사하다. `AVSynchronizedLayer`는 그것의 하위 트리에서 레이어의 타이밍을 플레이어 아이템의 타이밍과 동기화하여 타이밍 상태를 전달한다.

`AVSynchronizedLayer`의 하위 레이어로 추가된 애니메이션 속성 세트가 있는 모든 `CoreAnimation` 계층은 애니메이션이 플레이어 아이템의 시간 표시 막대에서 해석되도록 0이 아닌 양의 값의 시간 속성으로 애니메이션의 [`beginTime`](https://developer.apple.com/documentation/quartzcore/camediatiming/1427654-begintime)을 설정해야 한다. `CoreAnimation`이 [`CACurrentMediaTime()`](https://developer.apple.com/documentation/quartzcore/1395996-cacurrentmediatime)을 사용한 0.0의 `beginTime`을 대체한다. 0시부터 애니메이션을 시작하려면 [`AVCoreAnimationBeginTimeAtZero`](https://developer.apple.com/documentation/avfoundation/avcoreanimationbegintimeatzero)와 같은 작은 양의 값을 사용하라.

다음 예제와 같이 레이어를 사용할 수 있다.

```objectivec
AVPlayerItem *playerItem = <#Get a player item#>;
CALayer *superLayer =  <#Get a layer#>;
// Set up a synchronized layer to sync the layer timing of its subtree
// with the playback of the playerItem/
AVSynchronizedLayer *syncLayer = [AVSynchronizedLayer synchronizedLayerWithPlayerItem:playerItem];
[syncLayer addSublayer:<#Another layer#>];    // These sublayers will be synchronized.
[superLayer addSublayer:syncLayer];
```

### Topics

#### Creating a Synchronized Layer

[`init(playerItem: AVPlayerItem)`](https://developer.apple.com/documentation/avfoundation/avsynchronizedlayer/1388781-init)

지정된 플레이어 아이템과 동기화된 타이밍을 가진 새 동기화된 레이어를 생성한다.

#### Managing the Player Item

[`var playerItem: AVPlayerItem?`](https://developer.apple.com/documentation/avfoundation/avsynchronizedlayer/1385679-playeritem)

레이어의 타이밍이 동기화되는 플레이어 아이템.

#### Supporting Types

[`let AVCoreAnimationBeginTimeAtZero: CFTimeInterval`](https://developer.apple.com/documentation/avfoundation/avcoreanimationbegintimeatzero)

애니메이션 시작 시간을 0으로 설정하는 값.

