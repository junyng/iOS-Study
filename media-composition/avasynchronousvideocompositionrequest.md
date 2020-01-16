---
description: 비디오 컴포지터가 출력 픽셀 버퍼를 렌더링하는 데 필요한 정보가 포함된 객체이다.
---

# AVAsynchronousVideoCompositionRequest

### Declaration

```swift
class AVAsynchronousVideoCompositionRequest : NSObject
```

### Overview

비디오 컴포지터는 [`AVVideoCompositing`](https://developer.apple.com/documentation/avfoundation/avvideocompositing) 프로토콜을 구현해야 한다.

### Topics

#### Getting the Pixel Buffer for a Specific Frame

[`func sourceFrame(byTrackID: CMPersistentTrackID) -> CVPixelBuffer?`](https://developer.apple.com/documentation/avfoundation/avasynchronousvideocompositionrequest/1390379-sourceframe)

지정된 트랙ID의 소스 픽셀 버퍼를 반환한다.

#### Finishing the Composition Request

[`func finishCancelledRequest()`](https://developer.apple.com/documentation/avfoundation/avasynchronousvideocompositionrequest/1386261-finishcancelledrequest)

구성 요청이 취소되었음을 나타낸다.

[`func finish(withComposedVideoFrame: CVPixelBuffer)`](https://developer.apple.com/documentation/avfoundation/avasynchronousvideocompositionrequest/1387450-finish)

구성 요청이 성공했음을 나타낸다.

[`func finish(with: Error)`](https://developer.apple.com/documentation/avfoundation/avasynchronousvideocompositionrequest/1390797-finish)

구성 요청에 오류가 발생했음을 나타낸다.

#### Getting the Composition Request Settings

[`var compositionTime: CMTime`](https://developer.apple.com/documentation/avfoundation/avasynchronousvideocompositionrequest/1386888-compositiontime)

프레임을 구성해야 하는 시간.

[`var renderContext: AVVideoCompositionRenderContext`](https://developer.apple.com/documentation/avfoundation/avasynchronousvideocompositionrequest/1389112-rendercontext)

비디오 컴포지션은 요청을 만드는 컨텍스트를 제공한다.

[`var sourceTrackIDs: [NSNumber]`](https://developer.apple.com/documentation/avfoundation/avasynchronousvideocompositionrequest/1388898-sourcetrackids)

프레임을 구성하는 데 사용할 수 있는 모든 소스 버퍼의 트랙 ID.

[`var videoCompositionInstruction: AVVideoCompositionInstructionProtocol`](https://developer.apple.com/documentation/avfoundation/avasynchronousvideocompositionrequest/1386672-videocompositioninstruction)

프레임 작성에 사용되는 비디오 컴포지션.



