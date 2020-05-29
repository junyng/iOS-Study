---
description: Core Animation 레이어와 연관된 Metal drawable.
---

# CAMetalDrawable

### Declaration

```swift
protocol CAMetalDrawable
```

### Overview

이 프로토콜을 구현하는 객체는 `CAMetalLayer` 객체가 소유한다. 이 프로토콜을 직접 구현하지 마라. 대신, 레이어에서 drawable 객체를 요청하는 방법에 대한 자세한 내용은 [`CAMetalLayer`](https://developer.apple.com/documentation/quartzcore/cametallayer)를 참조하라.

### Topics

#### Getting the Drawable's Texture

* [`var texture: MTLTexture`](https://developer.apple.com/documentation/quartzcore/cametaldrawable/1478159-texture) drawable 콘텐츠가 들어 있는 Metal 텍스처 객체. **Required.**

#### Getting the Owning Layer

* [`var layer: CAMetalLayer`](https://developer.apple.com/documentation/quartzcore/cametaldrawable/1478165-layer) drawable 객체를 소유하는 레이어. **Required.**

