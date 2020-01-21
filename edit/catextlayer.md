---
description: 단순 텍스트 레이아웃과 일반 또는 속성 문자열의 렌더링을 제공하는 레이어.
---

# CATextLayer

### Declaration

```swift
class CATextLayer : CALayer
```

### Overview

첫 번째 선은 레이어의 꼭대기에 정렬되어 있다.

> **Note**
>
> `CATextLayer`는 텍스트를 렌더링할 때 하위 픽셀 안티앨리어싱을 사용하지 않는다. 텍스트는 레스터화되는 동시에 기존의 불투명한 배경에 합성될 때 서브 픽셀 안티앨리어싱을 사용하여만 그려질 수 있다. 텍스트 픽셀을 짜 넣을 백그라운드 픽셀을 갖기 전에 이미지 또는 레이어로 하위 픽셀 안티앨리어싱을 사용하여 텍스트를 그릴 수 있는 방법이 없다. 레이어의 `opacity` 속성을 `true`로 설정해도 렌더링 모드는 변경되지 않는다.

> Note
>
> macOS에서 `CATextLayer` 인스턴스가 [`CAConstraintLayoutManager`](https://developer.apple.com/documentation/quartzcore/caconstraintlayoutmanager) 클래스를 사용하여 배치되는 경우 레이어의 바운드는 텍스트 내용에 맞게 크기가 조정된다.

### Topics

#### Getting and Setting the Text

[`var string: Any?`](https://developer.apple.com/documentation/quartzcore/catextlayer/1515295-string)

수신기가 렌더링할 텍스트.

#### Text Visual Properties

[`var font: CFTypeRef?`](https://developer.apple.com/documentation/quartzcore/catextlayer/1515303-font)

수신기의 텍스트를 렌더링하는 데 사용되는 글꼴.

[`var fontSize: CGFloat`](https://developer.apple.com/documentation/quartzcore/catextlayer/1515290-fontsize)

수신기의 텍스트를 렌더링하는 데 사용되는 글꼴 크기. 애니메이션에 적합하다.

[`var foregroundColor: CGColor?`](https://developer.apple.com/documentation/quartzcore/catextlayer/1515305-foregroundcolor)

수신기의 문자를 렌더링하는 데 사용되는 색상. 애니메이션에 적합하다.

[`var allowsFontSubpixelQuantization: Bool`](https://developer.apple.com/documentation/quartzcore/catextlayer/1515300-allowsfontsubpixelquantization)

텍스트 렌더링에 사용되는 그래픽 컨텍스트에 대해 서브픽셀 양자화를 허용할지 여부를 결정한다.

#### Text Alignment and Truncation

[`var isWrapped: Bool`](https://developer.apple.com/documentation/quartzcore/catextlayer/1515302-iswrapped)

텍스트가 수신기 바운드 내에 감싸져 있는지 여부를 결정한다.

[`var alignmentMode: CATextLayerAlignmentMode`](https://developer.apple.com/documentation/quartzcore/catextlayer/1515301-alignmentmode)

개별 텍스트 라인이 수신기 범위 내에서 수평으로 정렬되는 방법을 결정한다.

[`var truncationMode: CATextLayerTruncationMode`](https://developer.apple.com/documentation/quartzcore/catextlayer/1515296-truncationmode)

텍스트가 수신기 바운드 내에 맞도록 잘리는 방법을 결정한다.

#### Constants

[Truncation modes](https://developer.apple.com/documentation/quartzcore/catextlayer/truncation_modes)

이 상수는 [`truncationMode`](https://developer.apple.com/documentation/quartzcore/catextlayer/1515296-truncationmode) 속성에서 사용됨.

[Horizontal alignment modes](https://developer.apple.com/documentation/quartzcore/catextlayer/horizontal_alignment_modes)

이 상수는 [`alignmentMode`](https://developer.apple.com/documentation/quartzcore/catextlayer/1515301-alignmentmode) 속성에서 사용됨.

