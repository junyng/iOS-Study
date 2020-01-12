---
description: 불변의 비디오 구성을 나타내는 객체입니다.
---

# AVVideoComposition

### Declaration

```swift
class AVVideoComposition : NSObject
```

### Overview

`AVFoundation` 프레임워크는 또한 새로운 비디오를 만드는데 사용할 수 있는 mutable 서브클래스인 [`AVMutableVideoComposition`](https://developer.apple.com/documentation/avfoundation/avmutablevideocomposition)을 제공한다.

비디오 구성은 그 명령의 총 시간 범위에서 임의의 시간 동안 그 시간에 대응하는 구성된 비디오 프레임을 생성하기 위해 사용될 비디오 트랙의 수 및 ID를 기술한다. `AVFoundation`의 내장형 비디오 구성자를 사용할 때, `AVVideoComposition`에 포함된 명령어는 각 비디오 소스에 대해 공간 변환, 불투명도 값 및 자르기 직사각형을 지정할 수 있으며, 이는 단순한 선형 ramping 함수를 통해 시간이 지남에 따라 달라질 수 있다.

당신은 [`AVVideoCompositing`](https://developer.apple.com/documentation/avfoundation/avvideocompositing) 프로토콜을 구현함으로써 커스텀 비디오 구성자를 구현할 수 있다. 사용자 지정 비디오 구성자는 재생 및 기타 작동 중에 각각의 비디오 소스에 대해 픽셀 버퍼를 제공하고 시각적 출력을 생성하기 위해 그것들에 대해 임의의 그래픽 연산을 수행할 수 있다.

