---
description: 파일 또는 메모리로부터 오디오 데이터의 재생을 제공하는 오디오 플레이어.
---

# AVAudioPlayer

### Declaration

```swift
class AVAudioPlayer : NSObject
```

### Overview

네트워크 스트림에서 캡처한 오디오를 재생하거나 I/O 대기시간이 매우 낮을 경우 오디오 재생에 이 클래스를 사용한다.

오디오 플레이어를 사용하여 다음과 같은 작업을 수행할 수 있다:

* 모든 기간의 사운드 재생
* 파일 또는 메모리 버퍼에서 사운드 재생
* 사운드 루프
* 정확한 동기화를 통해 오디오 플레이어당 한 개의 사운드씩 여러 개의 사운드를 동시에 재생
* 재생 중인 각 사운드에 대해 상대 재생 수준, 스테레오 위치 지정 및 재생 속도 제어
* 빨리감기 및 되감기와 같은 애플리케이션 기능을 지원하는 사운드 파일의 특정 지점을 시킹한다.
* 재생 수준 측정에 사용할 수 있는 데이터를 획득한다.

AVAudioPlayer 클래스는 iOS와 macOS에서 사용 가능한 모든 오디오 포맷으로 사운드를 재생할 수 있게 해준다. iOS에서 수신 전화와 같은 인터럽션을 처리하고 사운드 재생이 완료되면 사용자 인터페이스를 업데이트하는 델리게이트를 구현하라. 델리게이트 메서드는 [`AVAudioPlayerDelegate`](https://developer.apple.com/documentation/avfoundation/avaudioplayerdelegate)에 설명되어 있다.

오디오 플레이어를 재생, 일시 중지 또는 중지하려면 [Configuring and Controlling Playback](https://developer.apple.com/documentation/avfoundation/avaudioplayer#1669216)에 설명된 재생 제어 메서드 중 하나를 호출하라.

이 클래스는 Objective-C에 선언된 속성 기능을 사용하여 사운드의 타임라인 내의 재생 지점과 같은 사운드에 대한 정보를 관리하고 volume 및 looping 같은 재생 옵션에 접근한다.

iOS에서 재생할 적절한 오디오 세션을 구성하려면 [`AVAudioSession`](https://developer.apple.com/documentation/avfoundation/avaudiosession)및 [`AVAudioSessionDelegate`](https://developer.apple.com/documentation/avfoundation/avaudiosessiondelegate)를 참조하라.

