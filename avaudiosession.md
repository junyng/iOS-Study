---
description: 앱에서 오디오를 사용하는 방법을 시스템에 전달하는 객체
---

# AVAudioSession

### Declaration

```swift
class AVAudioSession : NSObject
```

### Overview

오디오 세션 앱과 운영 체제 사이의 중개자 역할을 한다. 그 결과, 기초적인 오디오 하드웨어의 역할을 한다. 오디오 세션을 사용하여 특정 동작이나 오디오 하드웨어와의 필요한 상호 작용을 상세히 설명하지 않고 앱 오디오의 일반적인 특성을 운영 체제에 전달한다. 사용자는 이러한 세부사항의 관리를 오디오 세션에 위임하여 운영 체제가 사용자의 오디오 환경을 잘 관리할 수 있도록 보장한다.

모든 iOS, tvOS 및 watchOS 앱에 다음 동작으로 미리 구성된 기본 오디오 세션이 있다.

* 오디오 재생을 지원하지만 오디오 녹음을 허용하지 않는다 \(tvOS는 오디오 녹음을 지원하지 않는다\).
* iOS에서 링/음소거 스위치를 자동 모드로 설정하면 앱의 재생중인 오디오가 모두 음소거된다.
* iOS에서 장비를 잠그면 앱의 오디오가 음소거된다.
* 앱의 오디오를 재생하면 다른 백그라운드 오디오를 음소거한다.

기본 오디오 세션은 유용한 동작을 제공하지만 일반적으로 미디어 앱에 필요한 오디오 동작을 제공하지 않는다. 기본 동작을 변경하려면 앱의 오디오 세션 범주를 구성하라.

사용할 수 있는 카테고리는 7가지가 있지만\([Audio Session Categories and Modes](https://developer.apple.com/library/archive/documentation/Audio/Conceptual/AudioSessionProgrammingGuide/AudioSessionCategoriesandModes/AudioSessionCategoriesandModes.html#//apple_ref/doc/uid/TP40007875-CH10) 참조\), 그러나 [`playback`](https://developer.apple.com/documentation/avfoundation/avaudiosession/category/1616509-playback)은 재생 앱이 가장 일반적으로 사용하는 것이다. 이 카테고리는 오디오 재생이 앱의 핵심 기능임을 나타낸다. 이 카테고리를 지정하면 링/음소거 스위치가 무음 모드\(iOS 전용\)로 설정되어 앱의 오디오가 계속된다. 이 카테고리를 사용하면 오디오, AirPlay 및 PiP에서 백그라운드 오디오를 재생할 수도 있다. 자세한 내용은 [Enabling Background Audio](https://developer.apple.com/documentation/avfoundation/media_assets_playback_and_editing/creating_a_basic_video_player_ios_and_tvos/enabling_background_audio)을 참조하라. AVAudioSession 객체를 사용하여 앱의 오디오 세션을 구성하라. 이 클래스는 오디오 세션의 카테고리, 모드 및 기타 구성을 설정하는 데 사용되는 싱글톤 객체이다. 앱 생명주기 동안 오디오 세션과 상호 작용할 수 있지만, 다음 예제와 같이 앱 시작시 이 구성을 수행하는 것이 종종 유용하다.

```swift
func application(_ application: UIApplication,
                 didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    
    // Get the singleton instance.
    let audioSession = AVAudioSession.sharedInstance()
    do {
        // Set the audio session category, mode, and options.
        try audioSession.setCategory(.playback, mode: .moviePlayback, options: [])
    } catch {
        print("Failed to set audio session category.")
    }
    
    // Other post-launch configuration.
    return true
}
```

오디오 세션은 [`setActive(_:)`](https://developer.apple.com/documentation/avfoundation/avaudiosession/1616597-setactive) 또는 [`setActive(_:options:)`](https://developer.apple.com/documentation/avfoundation/avaudiosession/1616627-setactive)메서드를 사용하여 세션을 활성화할 때 이 구성을 사용하라.

> **참고**  
> 카테고리를 설정한 후 언제든지 오디오 세션을 활성화할 수 있지만 일반적으로 앱이 오디오 재생을 시작할 때까지 이 호출을 연기하는 것이 바람직하다. 호출을 지연시키면 진행 중인 다른 백그라운드 오디오를 조기에 차단하지 않도록 보장한다.

