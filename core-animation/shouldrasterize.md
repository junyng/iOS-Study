---
description: 합성하기 전에 레이어가 비트맵으로 렌더링되는지 여부를 나타내는 부울. 애니메이션에 적합하다.
---

# shouldRasterize

### 정의

```swift
var shouldRasterize: Bool { get set }
```

### 논의

이 속성의 값이 `true`일 때, 레이어는 로컬 좌표 공간에서 비트맵으로 렌더링된 다음 다른 내용과 함께 대상에 합성된다. [`filters`](https://developer.apple.com/documentation/quartzcore/calayer/1410901-filters) 속성의 섀도우 효과 및 다른 필터는 래스터화되며 비트맵에 포함된다. 그러나 레이어의 현재 불투명도는 래스터화되지 않는다. 합성 중에 래스터화된 비트맵에 스케일링이 필요한 경우 필터의 [`minificationFilter`](https://developer.apple.com/documentation/quartzcore/calayer/1410898-minificationfilter) 및 [`magnificationFilter`](https://developer.apple.com/documentation/quartzcore/calayer/1410907-magnificationfilter) 속성이 필요에 따라 적용된다.

이 속성의 값이 `false` 일 때, 레이어는 가능하면 언제든지 목적지로 직접 합성된다. 합성 모델의 특정 특징 \(필터 포함 등\) 이 필요한 경우 레이어는 컴포지팅 전에 래스터화 될 수 있다.

이 속성의 기본 값은 `false`이다.

### 참고 항목

#### 레이어의 렌더링 동작 구성

[`var isOpaque: Bool`](https://developer.apple.com/documentation/quartzcore/calayer/1410763-isopaque)

레이어에 완전히 불투명한 내용이 포함되어 있는지 여부를 나타내는 불 값.

[`var edgeAntialiasingMask: CAEdgeAntialiasingMask`](https://developer.apple.com/documentation/quartzcore/calayer/1410892-edgeantialiasingmask)

수신기의 가장자리가 래스터화되는 방법을 정의하는 비트마스크.

[`func contentsAreFlipped() -> Bool`](https://developer.apple.com/documentation/quartzcore/calayer/1410777-contentsareflipped)

렌더링할 때 레이어 내용이 암묵적으로 반전되는지 여부를 나타내는 불 값을 반환한다.

[`var isGeometryFlipped: Bool`](https://developer.apple.com/documentation/quartzcore/calayer/1410960-isgeometryflipped)

레이어와 그 하위 레이어의 기하학적 구조가 수직으로 반전되는지 여부를 나타내는 불 값.

[`var drawsAsynchronously: Bool`](https://developer.apple.com/documentation/quartzcore/calayer/1410974-drawsasynchronously)

그리기 명령이 백그라운드 쓰레드에서 비동기적으로 지연되고 처리되는지 여부를 나타내는 불 값.

[`var rasterizationScale: CGFloat`](https://developer.apple.com/documentation/quartzcore/calayer/1410801-rasterizationscale)

레이어의 좌표 공간에 상대적인 컨텐츠 래스터화 스케일.

[`var contentsFormat: CALayerContentsFormat`](https://developer.apple.com/documentation/quartzcore/calayer/1792104-contentsformat)

레이어의 컨텐츠의 원하는 스토리지 형식에 대한 힌트.

[`func render(in: CGContext)`](https://developer.apple.com/documentation/quartzcore/calayer/1410909-render)

레이어와 그 하위 레이어를 지정된 컨텍스트에 렌더링한다.



