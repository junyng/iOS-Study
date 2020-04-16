---
description: Core Image 필터를 사용하여 비디오 컴포지션에서 개별 비디오 프레임을 처리하는 객체.
---

# AVAsynchronousCIImageFilteringRequest

### Declaration

```swift
class AVAsynchronousCIImageFilteringRequest : NSObject
```

### Overview

[`init(asset:applyingCIFiltersWithHandler:)`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1389556-init) 메서드를 사용하여 Core Image 필터링에 대한 컴포지션을 생성할 때 이 클래스를 사용하라. 이 메서드 호출에서는 각 비디오 프레임을 처리할 때 AVFoundation 에서 호출할 블록을 제공하며, 블록의 유일한 매개 변수는 `AVAsynchronousCIImageFilteringRequest` 객체이다. 필터링할 비디오 프레임 이미지에 해당 객체를 사용하고 필터링된 이미지를 AVFoundation에 반환하여 표시하거나 내보낼 수 있도록 하라. 목록 1은 에셋에 필터를 적용하는 예를 보여준다.

**목록 1** 비디오 에셋에 Core Image 필터 적용

```swift
let filter = CIFilter(name: "CIGaussianBlur")!
let composition = AVVideoComposition(asset: asset, applyingCIFiltersWithHandler: { request in
    
    // Clamp to avoid blurring transparent pixels at the image edges
    let source = request.sourceImage.imageByClampingToExtent()
    filter.setValue(source, forKey: kCIInputImageKey)
    
    // Vary filter parameters based on video timing
    let seconds = CMTimeGetSeconds(request.compositionTime)
    filter.setValue(seconds * 10.0, forKey: kCIInputRadiusKey)
    
    // Crop the blurred output to the bounds of the original image
    let output = filter.outputImage!.imageByCroppingToRect(request.sourceImage.extent)
    
    // Provide the filter output to the composition
    request.finishWithImage(output, context: nil)
})
```

> 팁
>
> 생성된 비디오 컴포지션을 재생에 사용하려면 구성 원본과 동일한 에셋에서 [`AVPlayerItem`](https://developer.apple.com/documentation/avfoundation/avplayeritem) 객체를 생성한 다음 플레이어 아이템의 [`videoComposition`](https://developer.apple.com/documentation/avfoundation/avplayeritem/1388818-videocomposition) 속성에 컴포지션을 할당하라. 컴포지션을 새 동영상 파일로 내보내려면 동일한 원본 에셋에서 [`AVAssetExportSession`](https://developer.apple.com/documentation/avfoundation/avassetexportsession) 객체를 생성한 다음 내보내기 세션의 [`videoComposition`](https://developer.apple.com/documentation/avfoundation/avassetexportsession/1389477-videocomposition) 속성에 컴포지션을 할당하라.

### Topics

#### Getting the Image to be Filtered

[`var sourceImage: CIImage`](https://developer.apple.com/documentation/avfoundation/avasynchronousciimagefilteringrequest/1387577-sourceimage)

현재 비디오 프레임 이미지.

#### Getting Contextual Information for Filtering

[`var compositionTime: CMTime`](https://developer.apple.com/documentation/avfoundation/avasynchronousciimagefilteringrequest/1388240-compositiontime)

처리 중인 프레임에 해당하는 비디오 컴포지션의 시간.

[`var renderSize: CGSize`](https://developer.apple.com/documentation/avfoundation/avasynchronousciimagefilteringrequest/1387933-rendersize)

처리 중인 프레임의 폭과 높이\(픽셀\).

#### Returning the Filtered Image

[`func finish(with: CIImage, context: CIContext?)`](https://developer.apple.com/documentation/avfoundation/avasynchronousciimagefilteringrequest/1389124-finish)

필터링된 비디오 프레임 이미지를 AVFoundation에 제공하여 추가 처리 또는 표시.

[`func finish(with: Error)`](https://developer.apple.com/documentation/avfoundation/avasynchronousciimagefilteringrequest/1386608-finish)

이미지 필터링 요청을 수행할 수 없음을 AVFoundation에 통지.

