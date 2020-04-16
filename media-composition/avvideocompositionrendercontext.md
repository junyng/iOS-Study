---
description: 사용자 정의 컴포지터가 새 출력 픽셀 버퍼를 렌더링하는 컨텍스트를 정의하는 객체.
---

# AVVideoCompositionRenderContext

### Declaration

```swift
class AVVideoCompositionRenderContext : NSObject
```

### Overview

`AVVideoCompositionRenderContext`인스턴스는 크기와 스케일링 정보를 제공하며 관리되는 버퍼 풀에서 픽셀 버퍼를 효율적으로 제공하기 위한 서비스를 제공한다.

### Topics

#### Creating the Pixel Buffer

[`func newPixelBuffer() -> CVPixelBuffer?`](https://developer.apple.com/documentation/avfoundation/avvideocompositionrendercontext/1386802-newpixelbuffer)

렌더링에 사용할 픽셀 버퍼를 반환한다.

#### Getting the Render Settings

[`var videoComposition: AVVideoComposition`](https://developer.apple.com/documentation/avfoundation/avvideocompositionrendercontext/1390647-videocomposition)

렌더링된 비디오 컴포지션.

[`var highQualityRendering: Bool`](https://developer.apple.com/documentation/avfoundation/avvideocompositionrendercontext/1388758-highqualityrendering)

사용할 렌더링 품질.

[`var renderScale: Float`](https://developer.apple.com/documentation/avfoundation/avvideocompositionrendercontext/1387408-renderscale)

프레임을 렌더링할 때 적용되는 스케일링 비율.

[`var renderTransform: CGAffineTransform`](https://developer.apple.com/documentation/avfoundation/avvideocompositionrendercontext/1389831-rendertransform)

원본 이미지에 적용할 변환.

[`var size: CGSize`](https://developer.apple.com/documentation/avfoundation/avvideocompositionrendercontext/1389718-size)

렌더링 프레임의 너비 및 높이.

#### Getting Pixel and Edge Width Information

[`var edgeWidths: AVEdgeWidths`](https://developer.apple.com/documentation/avfoundation/avvideocompositionrendercontext/1387026-edgewidths)

왼쪽, 위쪽, 오른쪽 및 아래쪽 가장자리의 가장자리 처리 영역의 두께

[`struct AVEdgeWidths`](https://developer.apple.com/documentation/avfoundation/avedgewidths)

가장자리 처리 영역 두께.

[`var pixelAspectRatio: AVPixelAspectRatio`](https://developer.apple.com/documentation/avfoundation/avvideocompositionrendercontext/1389800-pixelaspectratio)

렌더링된 프레임에 대한 픽셀 종횡비.

[`struct AVPixelAspectRatio`](https://developer.apple.com/documentation/avfoundation/avpixelaspectratio)

렌더링 컨텍스트에 대한 픽셀 종횡비.

