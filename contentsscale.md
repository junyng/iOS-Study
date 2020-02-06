---
description: 레이어에 적용된 스케일 팩터.
---

# contentsScale

### 정의

```swift
var contentsScale: CGFloat { get set }
```

### 논의

이 값은 레이어의 논리적 좌표 공간 \(포인트 단위로 측정\)과 물리적 좌표 공간 \(픽셀 단위로 측정\) 사이의 매핑을 정의한다. 더 큰 스케일 팩터는 레이어의 각 포인트가 렌더 시간에 둘 이상의 픽셀으로 표현된다는 것을 나타낸다. 예를 들어, 스케일 팩터가 2.0이고 레이어의 바운드가 50 x 50 포인트인 경우, 레이어의 내용을 표시하는 데 사용되는 비트맵의 크기는 100 x 100 픽셀이다.

이 속성의 기본값은 1.0이다. 뷰에 부착된 레이어의 경우, 뷰는 스케일 팩터를 현재 화면에 적합한 값으로 자동 변경한다. 직접 작성하고 관리하는 레이어의 경우, 화면의 해상도와 제공 중인 콘텐츠에 따라 이 속성의 값을 직접 설정해야 한다. Core Animation은 사용자가 지정한 값을 큐로 사용하여 콘텐츠를 렌더링하는 방법을 결정한다.

### 참고 항목

#### 레이어 기하학적 구조 수정

#### [`var frame: CGRect`](https://developer.apple.com/documentation/quartzcore/calayer/1410779-frame)

레이어의 프레임 직사각형.

[`var bounds: CGRect`](https://developer.apple.com/documentation/quartzcore/calayer/1410915-bounds)

레이어의 바운드 직사각형. 애니메이션에 적합하다.

[`var position: CGPoint`](https://developer.apple.com/documentation/quartzcore/calayer/1410791-position)

슈퍼레이어의 좌표 공간에서 레이어의 위치. 애니메이션에 적합하다.

[`var zPosition: CGFloat`](https://developer.apple.com/documentation/quartzcore/calayer/1410884-zposition)

z축에 있는 레이어의 위치. 애니메이션에 적합하다.

[`var anchorPointZ: CGFloat`](https://developer.apple.com/documentation/quartzcore/calayer/1410796-anchorpointz)

z축을 따라 레이어의 위치에 대한 앵커 포인트. 애니메이션에 적합하다.

[`var anchorPoint: CGPoint`](https://developer.apple.com/documentation/quartzcore/calayer/1410817-anchorpoint)

레이어 바운드 직사각형의 앵커 포인트를 정의한다. 애니메이션에 적합하다.

 

