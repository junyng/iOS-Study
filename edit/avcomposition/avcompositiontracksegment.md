---
description: 'URL, 트랙 식별자 및 소스 트랙에서 컴포지션 트랙으로의 시간 매핑으로 구성된 트랙의 세그먼트이다.'
---

# AVCompositionTrackSegment

### Declaration

```swift
class AVCompositionTrackSegment : AVAssetTrackSegment
```

### Overview

일반적으로 이 클래스를 사용하여 컴포지션의 하위 수준 표현을 선택한 저장소 형식으로 저장하고 저장소에서 재구성한다.

### Topics

#### Creating a Segment

[`init(timeRange: CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avcompositiontracksegment/1386841-init)

빈 트랙 세그먼트를 나타내는 트랙 세그먼트를 초기화한다.[`init(url: URL, trackID: CMPersistentTrackID, sourceTimeRange: CMTimeRange, targetTimeRange: CMTimeRange)`](https://developer.apple.com/documentation/avfoundation/avcompositiontracksegment/1390282-init)

주어진 URL이 참조하는 파일의 일부를 나타내는 트랙 세그먼트를 초기화한다.

#### Getting Segment Properties

[`var sourceURL: URL?`](https://developer.apple.com/documentation/avfoundation/avcompositiontracksegment/1386814-sourceurl)

트랙 세그먼트가 제공하는 미디어의 컨테이너 파일이다.[`var sourceTrackID: CMPersistentTrackID`](https://developer.apple.com/documentation/avfoundation/avcompositiontracksegment/1388326-sourcetrackid)

트랙 세그먼트가 제시하는 미디어 컨테이너 파일의 트랙 ID이다.

[`var isEmpty: Bool`](https://developer.apple.com/documentation/avfoundation/avcompositiontracksegment/1389592-isempty)

세그먼트가 비어있는지 가리키는 불 값.

