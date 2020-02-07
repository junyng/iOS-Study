---
description: 레이어 콘텐츠가 바운드 내에서 배치되거나 확장되는 방법을 지정하는 상수.
---

# contentsGravity

### 정의

```swift
var contentsGravity: CALayerContentsGravity { get set }
```

### 논의

이 속성의 가능한 값은 [Contents Gravity Values](https://developer.apple.com/documentation/quartzcore/calayer/contents_gravity_values)에 나열되어 있다.

이 속성의 기본값은 [`resize`](https://developer.apple.com/documentation/quartzcore/calayercontentsgravity/1410811-resize)이다.

> 중요
>
> 내용 중력 상수의 명칭은 수직 축의 방향에 기초한다. 예를 들어, 상단과 같은 수직 구성 요소가 있는 중력 상수를 사용하는 경우 레이어의 [`contentsAreFlipped()`](https://developer.apple.com/documentation/quartzcore/calayer/1410777-contentsareflipped) 또한 확인해야 한다. 이것이 `true` 일 때, 상단은 콘텐츠를 레이어의 하단에 정렬하고 [`bottom`](https://developer.apple.com/documentation/quartzcore/calayercontentsgravity/1410919-bottom)은 콘텐츠를 레이어의 상단에 정렬한다.
>
> macOS 및 iOS에서의 뷰에 대한 기본 좌표계는 수직축의 방향에 따라 다르다: macOS에서는 기본 좌표계는 레이어 영역의 왼쪽 하단에 원점을 가지고 있고, 그로부터 양수 값이 위로 확장된다. iOS에서는 기본 좌표계는 레이어 영역의 왼쪽 상단에 원점을 가지고 있으며, 그로부터 양수 값이 아래로 확장된다.
>
> 자세한 내용은 [Coordinate system](https://developer.apple.com/library/content/documentation/General/Conceptual/Devpedia-CocoaApp/CoordinateSystem.html)을 참조하라.

[Figure 1](https://developer.apple.com/documentation/quartzcore/calayer/1410872-contentsgravity#2851774)은 한 레이어의 콘텐츠 중력 속성에 대해 다른 값을 설정하는 효과의 네 가지 예를 보여준다.

**그림 1** 다른 레이어 콘텐츠 중력 설정의 효과

![](.gitbook/assets/contents_gravity.png)

1. 중력 상수가 [`resize`](https://developer.apple.com/documentation/quartzcore/calayercontentsgravity/1410811-resize)이다. - 기본 값
2. 중력 상수가 [`center`](https://developer.apple.com/documentation/quartzcore/calayercontentsgravity/1410982-center)이다.
3. 중력 상수가 [`contentsAreFlipped()`](https://developer.apple.com/documentation/quartzcore/calayer/1410777-contentsareflipped)? [`top`](https://developer.apple.com/documentation/quartzcore/calayercontentsgravity/1410913-top) : [`bottom`](https://developer.apple.com/documentation/quartzcore/calayercontentsgravity/1410919-bottom)이다.
4. 중력 상수가 [`contentsAreFlipped()`](https://developer.apple.com/documentation/quartzcore/calayer/1410777-contentsareflipped) `?` [`bottomLeft`](https://developer.apple.com/documentation/quartzcore/calayercontentsgravity/1410964-bottomleft) : [`topLeft`](https://developer.apple.com/documentation/quartzcore/calayercontentsgravity/1410738-topleft)이다.

