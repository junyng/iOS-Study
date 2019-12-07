# Configuring Audio Settings for iOS and tvOS

iOS 및 tvOS 용 미디어 재생 앱은 특정 동작과 기능을 활성화하기 위해 오디오 세션의 일부 구성을 수행해야 한다. 이 장에서는 사용자에게 최상의 재생 환경을 제공하기 위해 이 구성을 수행하는 방법에 대해 설명한다.

### Configuring Your iOS and tvOS Audio Session

오디오 세션은 앱과 운영 체제 그리고 다시 기본 오디오 하드웨어 사이에서 중개 역할을 한다. 특정 동작을 자세히 설명하지 않고 운영 체제에 앱 오디오의 특성 또는 오디오 하드웨어와 필요한 상호 작용 특성을 전달하기 위해 사용한다. 이는 운영 체제가 사용자의 오디오 환경을 가장 잘 관리할 수 있도록 하기 위해 이러한 세부 사항의 관리를 오디오 세션에 위임한다.

모든 iOS 및 tvOS 앱에 다음과 같이 사전 구성된 기본 오디오 세션이 있다.

* 오디오 재생은 지원되지만 오디오 녹화는 허용되지 않는다. \(tvOS에서는 오디오 녹화가 지원되지 않는다.\)
* iOS에서, Ring/Silent 스위치를 자동 모드로 설정하면 앱에서 재생되는 모든 오디오가 묵음이 된다.
* 앱이 오디오를 재생하면 다른 백그라운드 오디오는 묵음이 된다.

기본 오디오 세션에서 많은 유용한 동작을 얻지만, 미디어 재생 앱을 만들 때 필요한 동작은 아니다. 이 동작을 변경하려면 앱의 오디오 세션 _카테고리_를 구성하라.

오디오 세션 카테고리는 앱에 필요한 일반적인 오디오 동작을 정의한다. 사용할 수 있는 카테고리는 7가지지만\([Audio Session Categories and Modes](https://developer.apple.com/library/archive/documentation/Audio/Conceptual/AudioSessionProgrammingGuide/AudioSessionCategoriesandModes/AudioSessionCategoriesandModes.html#//apple_ref/doc/uid/TP40007875-CH10) 참조\), 그러나 대부분의 재생 앱에 필요한 것은 [AVAudioSessionCategoryPlayback](https://developer.apple.com/documentation/avfoundation/avaudiosessioncategoryplayback) 이라고 불린다. 이 카테고리는 오디오 재생이 앱의 중심 기능임을 나타낸다. 이 카테고리를 지정하면 Ring/Silent 스위치가 자동 모드로 설정된 상태에서 앱의 오디오가 계속된다. \(iOS만 해당\) 이 카테고리로 사진 백그라운드 모드에서 오디오, AirPlay 및 Picture를 사용할 경우 백그라운드 오디오도 재생할 수 있다. 더 자세한 정보는 [Enabling Background Audio](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/MediaPlaybackGuide/Contents/Resources/en.lproj/ConfiguringAudioSettings/ConfiguringAudioSettings.html#//apple_ref/doc/uid/TP40016757-CH9-SW4) 를 참조하라.

[AVAudioSession](https://developer.apple.com/documentation/avfoundation/avaudiosession) 객체를 사용하여 앱의 오디오 세션을 구성하라. 이 객체는 오디오 세션 카테고리를 설정하고 다른 구성 설정을 수행하는 데 사용되는 싱글톤 객체이다. 앱의 생명 주기 동안 오디오 세션과 상호 작용할 수 있지만, 다음 예와 같이 앱 시작 시 이 구성을 수행하는 것이 종종 유용하다.

```swift
func application(_ application: UIApplication,
                 didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey : Any]?) -> Bool {
    let audioSession = AVAudioSession.sharedInstance()
    do {
        try audioSession.setCategory(AVAudioSessionCategoryPlayback)
    } catch {
        print("Setting category to AVAudioSessionCategoryPlayback failed.")
    }
    // Other project setup
    return true
}
}
```

이 카테고리는 [setActive:error:](https://developer.apple.com/documentation/avfoundation/avaudiosession/1616597-setactive) 또는 [setActive:withOptions:error:](https://developer.apple.com/documentation/avfoundation/avaudiosession/1616627-setactive) 메서드를 사용함으로써 오디오 세션을 활성화한다.

> **참고**: 카테고리를 설정한 후에는 언제든지 오디오 세션을 활성화할 수 있지만 일반적으로 앱이 오디오 재생을 시작할 때까지 이 호출을 연기하는것이 좋다. 이렇게 하면 진행 중인 다른 백그라운드 오디오를 조기에 중단하지 않도록 할 수 있다.

카테고리를 설정하는 것은 `AVAudioSession`과 최소한의 상호 작용이지만, 다른 많은 구성 옵션과 기능도 사용할 수 있다. 오디오 세션에 대한 자세한 내용은 [_Audio Session Programming Guide_](https://developer.apple.com/library/archive/documentation/Audio/Conceptual/AudioSessionProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007875) __을 참조하라.

### Enabling Background Audio

iOS 및 tvOS 애플리케이션에서 일부 백그라운드 작업에 대해 특정 기능을 사용하도록 설정해야한다. 재생 앱에 필요한 일반적인 기능은 백그라운드 오디오 재생이다. 이 기능은 iOS에서 AirPlay 스트리밍 및 Picture in Picture Play와 같은 고급 재생 기능을 활성화하는 데도 필요하다.

이러한 기능을 구성하는 데 가장 간단한 방법은 Xcode를 사용하는 것이다. Xcode에서 앱의 타켓을 선택하고 기능 탭을 선택하라. 기능 탭에서 백그라운드 모드 스위치를 ON으로 설정하고 사용 가능한 모드 목록에서 "Audio, AirPlay, and Picture in Picture" 옵션을 선택하라.

![](../.gitbook/assets/background_modes_2x.png)

이 모드가 활성화되고 오디오 세션이 구성되면 앱에서 백그라운드 오디오를 재생할 수 있다.

