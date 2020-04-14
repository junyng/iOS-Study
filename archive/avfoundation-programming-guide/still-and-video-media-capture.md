# Still and Video Media Capture

[setSampleBufferDelegate:queue:](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput/1389008-setsamplebufferdelegate)카메라나 마이크와 같은 장치에서 캡처를 관리하기 위해 객체를 조립하여 입력과 출력을 나타내고 [`AVCaptureSession`](https://developer.apple.com/documentation/avfoundation/avcapturesession)의 인스턴스를 사용하여 데이터 흐름을 조정한다. 최소한 당신은 필요하다:

* 카메라 또는 마이크와 같은 입력 장치를 나타내는 [`AVCaptureDevice`](https://developer.apple.com/documentation/avfoundation/avcapturedevice)의 인스턴스
* 입력 장치에서 포트를 구성하기 위한 [`AVCaptureInput`](https://developer.apple.com/documentation/avfoundation/avcaptureinput)의 구체적인 서브 클래스의 인스턴스
* 동영상 파일 또는 스틸 이미지의 출력을 관리하기 위한 [`AVCaptureOutput`](https://developer.apple.com/documentation/avfoundation/avcaptureoutput)의 구체적인 서브클래스의 인스턴스
* 입력에서 출력으로의 데이터 흐름을 조정하는 [`AVCaptureSession`](https://developer.apple.com/documentation/avfoundation/avcapturesession)의 인스턴스

사용자에게 카메라의 녹화 내용 미리 보기를 보여주려면 [`AVCaptureVideoPreviewLayer`](https://developer.apple.com/documentation/avfoundation/avcapturevideopreviewlayer)\(의 서브클래스\)의 인스턴스를 사용하라.

그림 4-1과 같이 단일 세션으로 조정된 여러 입력 및 출력을 구성할 수 있다.

**그림 4-1**  단일 세션이 여러 입력 및 출력을 구성할 수 있다.

![](../../.gitbook/assets/captureoverview_2x.png)

대부분의 애플리케이션에서, 이것은 당신이 필요한 만큼 상세하다. 그러나 일부 작업의 경우\(예를 들어, 오디오 채널에서 전원수준을 모니터링하려면\) 입력 장치의 다양한 포트가 어떻게 표현되고 이러한 포트가 출력에 어떻게 연결되는지 고려해야 한다.

캡처 세션에서 캡처 입력과 캡처 출력 간의 연결은 [`AVCaptureConnection`](https://developer.apple.com/documentation/avfoundation/avcaptureconnection) 객체로 표현된다. 캡처 입력 \([`AVCaptureInput`](https://developer.apple.com/documentation/avfoundation/avcaptureinput)의 인스턴스\)는 하나 이상의 입력 포트를 갖는다. 캡처 출력 \([`AVCaptureOutput`](https://developer.apple.com/documentation/avfoundation/avcaptureoutput)의 인스턴스\)는 하나 이상의 소스\(예를 들어, [`AVCaptureMovieFileOutput`](https://developer.apple.com/documentation/avfoundation/avcapturemoviefileoutput) 객체는 비디오 및 오디오 데이터를 모두 수신할 수 있다\).

세션에 입력 또는 출력을 추가할 때, 세션은 그림 4-2에서 보듯이 모든 호환 캡처 입력의 포트와 캡처 출력 사이에 연결을 형성한다. 캡처 입력과 캡처 출력 간의 연결은 [`AVCaptureConnection`](https://developer.apple.com/documentation/avfoundation/avcaptureconnection) 객체로 표현된다.

**그림 4-2** AVCaptureConnection은 입력과 출력 사이의 연결을 나타낸다.

![](../../.gitbook/assets/capturedetail_2x.png)

캡처연결을 사용하여 지정된 입력 또는 지정된 출력으로의 데이터 흐름을 활성화하거나 비활성화할 수 있다. 또한 연결을 사용하여 오디오 채널의 평균 및 피크 전력 레벨을 모니터링할 수 있다.

> **참고:** 미디어 캡처는 iOS 기기에서 전면 카메라와 후면 카메라를 동시에 캡처하는 것을 지원하지 않는다.



### 캡처 세션을 사용하여 데이터 흐름 조정

[`AVCaptureSession`](https://developer.apple.com/documentation/avfoundation/avcapturesession) 객체는 데이터 캡처를 관리하는 데 사용하는 중앙 조정 객체이다. 인스턴스를 사용하여 AV 입력 장치에서 출력으로 데이터 흐름을 조정하라. 세션에 원하는 캡처 장치와 출력을 추가한 다음 [`startRunning`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1388185-startrunning) 메시지를 세션에 전송하여 데이터 흐름을 시작하고 [`stopRunning`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1385661-stoprunning) 메시지를 전송하여 데이터 흐름을 중지하라.

```objectivec
AVCaptureSession *session = [[AVCaptureSession alloc] init];
// Add inputs and outputs.
[session startRunning];
```



#### 세션 구성

세션에서 _preset_을 사용하여 원하는 이미지 품질 및 해상도를 지정하라. 사전 설정은 가능한 여러 구성 중 하나를 식별하는 상수로서, 경우에 따라 실제 구성은 기기마다 다르다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Symbol</th>
      <th style="text-align:left">&#xD574;&#xC0C1;&#xB3C4;</th>
      <th style="text-align:left">Comments</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><a href="https://developer.apple.com/documentation/avfoundation/avcapturesession/preset/1388084-high"><code>AVCaptureSessionPresetHigh</code></a>
      </td>
      <td style="text-align:left">High</td>
      <td style="text-align:left">
        <p>Highest recording quality.</p>
        <p>This varies per device.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://developer.apple.com/documentation/avfoundation/avcapturesessionpresetmedium"><code>AVCaptureSessionPresetMedium</code></a>
      </td>
      <td style="text-align:left">Medium</td>
      <td style="text-align:left">
        <p>Suitable for Wi-Fi sharing.</p>
        <p>The actual values may change.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://developer.apple.com/documentation/avfoundation/avcapturesession/preset/1386357-low"><code>AVCaptureSessionPresetLow</code></a>
      </td>
      <td style="text-align:left">Low</td>
      <td style="text-align:left">
        <p>Suitable for 3G sharing.</p>
        <p>The actual values may change.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://developer.apple.com/documentation/avfoundation/avcapturesessionpreset640x480"><code>AVCaptureSessionPreset640x480</code></a>
      </td>
      <td style="text-align:left">640x480</td>
      <td style="text-align:left">VGA.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://developer.apple.com/documentation/avfoundation/avcapturesessionpreset1280x720"><code>AVCaptureSessionPreset1280x720</code></a>
      </td>
      <td style="text-align:left">1280x720</td>
      <td style="text-align:left">720p HD.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://developer.apple.com/documentation/avfoundation/avcapturesessionpresetphoto"><code>AVCaptureSessionPresetPhoto</code></a>
      </td>
      <td style="text-align:left">Photo</td>
      <td style="text-align:left">
        <p>Full photo resolution.</p>
        <p>This is not supported for video output.</p>
      </td>
    </tr>
  </tbody>
</table>미디어 프레임 크기별 구성을 설정하려면 다음과 같이 설정하기 전에 지원되는지 여부를 확인해야 한다.

```objectivec
if ([session canSetSessionPreset:AVCaptureSessionPreset1280x720]) {
    session.sessionPreset = AVCaptureSessionPreset1280x720;
}
else {
    // Handle the failure.
}
```

사전 설정으로 가능한 수준보다 더 세밀한 수준에서 세션 매개 변수를 조정해야 하거나 실행 중인 세션을 변경하려면 [`beginConfiguration`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1389174-beginconfiguration) 및 [`commitConfiguration`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1388173-commitconfiguration) 메서드로 변경 사항을 둘러싸라. [`beginConfiguration`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1389174-beginconfiguration) 및 [`commitConfiguration`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1388173-commitconfiguration) 메서드는 장치의 변화가 그룹으로 발생하도록 보장하여 상태 가시성이나 불일치를 최소화한다. [`beginConfiguration`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1389174-beginconfiguration)을 호출한 후 출력을 추가 또는 제거하거나 `sessionPreset` 속성을 변경하거나 개별 캡처 입력 또는 출력 속성을 구성할 수 있다. 호출할 때까지 실제로 변경되지 않으며, 이때 이 함께 적용된다. [`commitConfiguration`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1388173-commitconfiguration)을 호출할 때까지 실제로 변경되지 않으며, 이때 함께 적용된다.

```objectivec
[session beginConfiguration];
// Remove an existing capture device.
// Add a new capture device.
// Reset the preset.
[session commitConfiguration];
```

#### 모니터링 캡처 세션 상태

캡처 세션은 예를 들어 실행이 시작되거나 중지되거나 중단될 때 알림을 수신하도록 관찰할 수 있는 노티피케이션을 게시한다. 런타임 오류가 발생할 경우 [`AVCaptureSessionRuntimeErrorNotification`](https://developer.apple.com/documentation/avfoundation/avcapturesessionruntimeerrornotification)를 수신하기 위해 등록할 수 있다. 세션의 실행 중인 [`running`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1388133-isrunning) 속성을 조회하여 실행 중인지, 중단된 속성을 조회하여 중단 여부를 확인할 수도 있다. 또한 [`running`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1388133-isrunning) 및 [`interrupted`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1620475-isinterrupted) 속성은 모두 key-value 옵저빙 준수 및 메인 쓰레드에 노티피케이션이 게시된다.

### 입력 장치를 나타내는 AVCaptureDevice 객체

[`AVCaptureDevice`](https://developer.apple.com/documentation/avfoundation/avcapturedevice) 객체는 입력 데이터 \(오디오 또는 비디오 등\)를 `AVCaptureSession` 객체에 제공하는 물리적 캡처 장치를 추상화한다. 각 입력 장치에는 하나의 객체가 있다. 예를 들어, 두 개의 비디오 입력- 하나는 카메라를 정면으로 향하고 하나는 뒤로 향하는 카메라 그리고 오디오 입력을 위한 마이크.

AVCaptureDevice 클래스 메서드 [`devices`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1386237-devices) 및 [`devicesWithMediaType:`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1390520-devices) 을 사용하여 현재 사용 가능한 캡처 장치를 찾을 수 있다. 또한 필요한 경우 iPhone, iPad 또는 iPod에서 제공하는 기능을 확인할 수 있다. \([Device Capture Settings](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/04_MediaCapture.html#//apple_ref/doc/uid/TP40010188-CH5-SW18) 참조\). 그러나 사용 가능한 장치의 목록은 변경될 수 있다. 현재의 입력 장치는 사용할 수 없게 될 수 있으며\(다른 애플리케이션에서 사용하는 경우\) 새로운 입력 장치를 사용할 수 있게 될 수 있다\(다른 애플리케이션에서 사용을 포기하는 경우\). [`AVCaptureDeviceWasConnectedNotification`](https://developer.apple.com/documentation/foundation/nsnotification/name/1389018-avcapturedevicewasconnected) 및 [`AVCaptureDeviceWasDisconnectedNotification`](https://developer.apple.com/documentation/foundation/nsnotification/name/1387782-avcapturedevicewasdisconnected) 노티피케이션을 받으려면 등록해야하며 사용 가능한 장치 목록이 변경될 때 알림을 받는다.

캡처 입력을 사용하여 캡처 세션에 입력 장치를 추가하라\([Use Capture Inputs to Add a Capture Device to a Session](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/04_MediaCapture.html#//apple_ref/doc/uid/TP40010188-CH5-SW6) 참조\).

#### 장치 특성

기기에 다른 특징에 대해 물어볼 수 있다. 또한 [`hasMediaType:`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1389487-hasmediatype) 및 [`supportsAVCaptureSessionPreset:`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1386263-supportsavcapturesessionpreset)을 사용하여 특정 미디어 유형을 제공하는지 또는 지정된 캡처 세션 사전 설정을 지원하는지 테스트할 수도 있다. 사용자에게 정보를 제공하기 위해 캡처 장치의 위치 \(테스트 대상 장치의 앞면에 있는지 뒷면에 있는지 여부\)와 현지화된 이름을 확인할 수 있다. 사용자가 선택할 수 있도록 캡처 장치 목록을 표시하려면 이 옵션을 사용하라.

그림 4-3은 후면 \([`AVCaptureDevicePositionBack`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/position/back)\) 및 전면 \([`AVCaptureDevicePositionFront`](https://developer.apple.com/documentation/avfoundation/avcapturedeviceposition/avcapturedevicepositionfront)\) 카메라의 위치를 보여준다.

> **참고:** 미디어 캡처는 iOS 기기에서 전면 카메라와 후면 카메라를 동시에 캡처하는 것을 지원하지 않는다.

**그림 4-3** iOS 기기 전면 및 후면 카메라 위치

![](../../.gitbook/assets/cameras_2x.png)

다음 코드 예는 사용 가능한 모든 장치를 순회하고 장치 이름\(비디오 장치의 경우 장치 위치\)을 장치에 기록한다.

```objectivec
NSArray *devices = [AVCaptureDevice devices];
 
for (AVCaptureDevice *device in devices) {
 
    NSLog(@"Device name: %@", [device localizedName]);
 
    if ([device hasMediaType:AVMediaTypeVideo]) {
 
        if ([device position] == AVCaptureDevicePositionBack) {
            NSLog(@"Device position : back");
        }
        else {
            NSLog(@"Device position : front");
        }
    }
}
```

추가적으로, 기기의 모델 ID와 고유 ID도 확인할 수 있다.

#### 장치 캡처 설정

다른 장치들은 다른 기능을 가지고 있다. 예를 들어, 어떤 장치들은 다른 초점이나 플래시 모드를 지원할 수 있고, 어떤 장치들은 관심 지점에 초점을 맞출 수 있다.

다음 코드 조각은 토치 모드가 있는 비디오 입력 장치를 찾고 지정된 캡처 세션 사전 설정을 지원하는 방법을 보여준다:

```objectivec
NSArray *devices = [AVCaptureDevice devicesWithMediaType:AVMediaTypeVideo];
NSMutableArray *torchDevices = [[NSMutableArray alloc] init];
 
for (AVCaptureDevice *device in devices) {
    [if ([device hasTorch] &&
         [device supportsAVCaptureSessionPreset:AVCaptureSessionPreset640x480]) {
        [torchDevices addObject:device];
    }
}
```

기준에 맞는 장치를 여러 개 찾으면 사용자가 사용할 장치를 선택할 수 있다. 디바이스에 대한 설명을 사용자에게 표시하려면 해당 [`localizedName`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1388222-localizedname) 속성을 사용하라.

당신은 다양한 다른 특징들을 비슷한 방법으로 사용한다. 특정 모드를 지정하는 상수가 있으며 특정 모드를 지원하는지 여부를 장치에 물어볼 수 있다. 여러 경우, 기능이 변경될 때 통보할 속성을 관찰할 수 있다. 모든 경우, [Configuring a Device](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/04_MediaCapture.html#//apple_ref/doc/uid/TP40010188-CH5-SW19)에 설명된 대로 특정 기능의 모드를 변경하기 전에 장치를 잠궈라.

> **참고:** 초점 모드와 노출 모드와 마찬가지로 관심 지점과 노출 지점은 상호 배타적이다.



**초점 모드**

초점 모드는 세 가지가 있다:

* [AVCaptureFocusModeLocked](https://developer.apple.com/documentation/avfoundation/avcapturedevice/focusmode/locked): 초점은 고정되어 있다. 이것은 사용자가 장면을 구성한 후 포커스를 잠그도록 허용할 때 유용하다.
* [AVCaptureFocusModeAutoFocus](https://developer.apple.com/documentation/avfoundation/avcapturefocusmode/avcapturefocusmodeautofocus): 카메라는 한 번의 스캔 포커스를 맞춘 다음 잠금으로 되돌아간다. 이는 장면의 중심이 아니더라도 초점을 맞출 특정 항목을 선택한 후 해당 항목에 초점을 맞추고자 하는 상황에 적합하다.
* [AVCaptureFocusModeContinuousAutoFocus](https://developer.apple.com/documentation/avfoundation/avcapturefocusmode/avcapturefocusmodecontinuousautofocus): 카메라는 필요에 따라 연속적으로 자동 초점이 된다.

[`isFocusModeSupported:`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1390215-isfocusmodesupported) 메서드를 사용하여 장치가 특정 초점 모드를 지원하는지 확인한 다음 [`focusMode`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1389191-focusmode) 속성을 사용하여 모드를 설정하라.

또한 장치는 관심있는 초점을 지원할 수 있다. [`focusPointOfInterestSupported`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1390436-isfocuspointofinterestsupported)를 사용하여 지원을 위해 테스트한다. 지연되는 경우,  초점 설정은 [`focusPointOfInterest`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1385853-focuspointofinterest)을 사용하여 설정한다. 그림 영역의 왼쪽 상단 `{0,0}` 을 나타내는 `CGPoint`를 통과하고, {1, 1}은 오른쪽에 홈 버튼이 있는 _landscape mode_에서 오른쪽 하단을 나타낸다. 이는 장치가 세로 방향의 모드에 있어도 적용된다.

초점 모드 설정을 변경하면 다음과 같이 기본 구성으로 되돌릴 수 있다:

```objectivec
if ([currentDevice isFocusModeSupported:AVCaptureFocusModeContinuousAutoFocus]) {
    CGPoint autofocusPoint = CGPointMake(0.5f, 0.5f);
    [currentDevice setFocusPointOfInterest:autofocusPoint];
    [currentDevice setFocusMode:AVCaptureFocusModeContinuousAutoFocus];
}
```

**노출 모드**

두 가지 노출 모드가 있다:

* [`AVCaptureExposureModeContinuousAutoExposure`](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVCaptureDevice_Class/Reference/Reference.html#//apple_ref/doc/c_ref/AVCaptureExposureModeContinuousAutoExposure): 이 장치는 필요에 따라 노출 수준을 자동으로 조정한다.
* [`AVCaptureExposureModeLocked`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/exposuremode/locked): 노출 수준은 현재 수준으로 고정되어 있다.

[`isExposureModeSupported:`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1389048-isexposuremodesupported) 메서드를 사용하여 장치가 지정된 노출 모드를 지원하는지 여부를 결정한 다음 [exposureMode](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1388858-exposuremode) 속성을 사용하여 모드를 설정한다.

또한, 장치는 노출 관심 지점을 지원할 수 있다. [`exposurePointOfInterestSupported`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1387263-isexposurepointofinterestsupport)를 사용하여 지원 테스트를 수행한다. 지원되는 경우 [`exposurePointOfInterest`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1388777-exposurepointofinterest)를 사용하여 노출 포인트를 설정하라. `CGPoint`를 통과하면 `{0, 0}`이 그림 영역의 왼쪽 상단을 나타낸다. 그리고 `{1, 1}`은 _오른쪽의 홈 버튼이 있는 가로 모드에서 오른쪽 하단을 나타내며_, 이는 장치가 _세로 모드_인 경우에도 적용된다.

\`\`[`adjustingExposure`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1386253-adjustingexposure) 속성을 사용하여 장치가 현재 노출 설정을 변경하고 있는지 여부를 확인할 수 있다. 장치가 시작되면 key-value 옵저빙을 사용하여 속성을 관찰할 수 있으며 노출 설정 변경을 중지할 수 있다.

노출 설정을 변경하면 다음과 같이 기본 구성으로 반환할 수 있다:

```objectivec
if ([currentDevice isExposureModeSupported:AVCaptureExposureModeContinuousAutoExposure]) {
    CGPoint exposurePoint = CGPointMake(0.5f, 0.5f);
    [currentDevice setExposurePointOfInterest:exposurePoint];
    [currentDevice setExposureMode:AVCaptureExposureModeContinuousAutoExposure];
}
```

**플래시 모드**

플래시 모드에는 세 가지가 있다:

* [`AVCaptureFlashModeOff`](https://developer.apple.com/documentation/avfoundation/avcaptureflashmode/avcaptureflashmodeoff): 플래시가 터지지 않는다.
* [`AVCaptureFlashModeOn`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/flashmode/on): 플래시가 항상 터진다.
* [`AVCaptureFlashModeAuto`](https://developer.apple.com/documentation/avfoundation/avcaptureflashmode/avcaptureflashmodeauto): 주변 조명 조건에 따라 플래시가 점화된다.

[`hasFlash`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1388988-hasflash) 를 사용하여 장치에 플래시가 있는지 여부를 확인한다. 이 메서드가 `YES`를 반환하는 경우, [`isFlashModeSupported:`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1386434-isflashmodesupported) 메서드를 사용하여 원하는 모드를 통과하여 장치가 주어진 플래시 모드를 지원하는지 여부를 결정한 [`flashMode`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1388116-flashmode) 속성을 사용하여 모드를 설정하라.

**토치 모드**

토치 모드에서는 비디오 캡처를 켜기 위해 낮은 전력에서 플래시가 계속 활성화된다. 토치 모드는 세 가지가 있다:

* [`AVCaptureTorchModeOff`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/torchmode/off): 토치가 항상 꺼져있다.
* [`AVCaptureTorchModeOn`](https://developer.apple.com/documentation/avfoundation/avcapturetorchmode/avcapturetorchmodeon): 토치가 항상 켜져있다.
* [`AVCaptureTorchModeAuto`](https://developer.apple.com/documentation/avfoundation/avcapturetorchmode/avcapturetorchmodeauto): 토치는 필요에 따라 자동으로 켜지고 꺼진다.

[`hasTorch`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1387674-hastorch)를 사용하여 장치에 플래시가 있는지 확인하라. [`isTorchModeSupported:`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1388822-istorchmodesupported) 메서드를 사용하여 장치가 특정 플래시 모드를 지원하는지 확인한 다음 [`torchMode`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1386035-torchmode) 속성을 사용하여 모드를 설정하라.

토치가 있는 장치의 경우 장치가 실행 중인 캡처 세션과 연결된 경우에만 토치가 켜진다.

**비디오 안정화**

특정 장치 하드웨어에 따라 비디오로 작동하는 연결에 대해 시네마틱 비디오 안정화가 가능하다. 그렇다고 해서 모든 소스 포멧과 비디오 해상도가 지원되는 것은 아니다.

시네마틱 비디오 안정화를 활성화하면 비디오 캡처 파이프라인에 추가적인 대기 시간이 발생할 수도 있다. 비디오 안정화 사용 시기를 탐지하려면 [`videoStabilizationEnabled`](https://developer.apple.com/documentation/avfoundation/avcaptureconnection/1620485-videostabilizationenabled) 속성을 사용하라. [`enablesVideoStabilizationWhenAvailable`](https://developer.apple.com/documentation/avfoundation/avcaptureconnection/1620482-enablesvideostabilizationwhenava) 속성은 카메라가 지원하는 경우 애플리케이션이 자동으로 비디오 안정화를 가능하게 한다. 기본적으로 위의 한계로 인해 자동 안정화가 비활성화된다.

**화이트 밸런스**

두 가지 화이트 밸런스 모드가 있다:

* [`AVCaptureWhiteBalanceModeLocked`](https://developer.apple.com/documentation/avfoundation/avcapturewhitebalancemode/avcapturewhitebalancemodelocked): 화이트 밸런스 모드는 고정되어 있다.
* [`AVCaptureWhiteBalanceModeContinuousAutoWhiteBalance`](https://developer.apple.com/documentation/avfoundation/avcapturewhitebalancemode/avcapturewhitebalancemodecontinuousautowhitebalance): 카메라는 필요에 따라 지속적으로 화이트 밸런스를 조정한다.

[`isWhiteBalanceModeSupported:`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1388587-iswhitebalancemodesupported) 메서드를 사용해 디바이스가 주어진 화이트 밸런스 모드를 지원하는지 여부를 결정한 다음 [`whiteBalanceMode`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1386369-whitebalancemode) 속성을 사용하여 모드를 설정한다.

[`adjustingWhiteBalance`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1386544-adjustingwhitebalance) 속성을 사용하여 장치가 현재 화이트 밸런스 설정을 변경하고 있는지 여부를 확인할 수 있다. 장치를 시작하고 화이트 밸런스 설정 변경을 중지할 때 알림을 받기 위해 key-value 옵저빙을 사용하여 속성을 관찰할 수 있다.

**장치 방향 설정**

`AVCaptureConnection`에서 원하는 방향을 설정하여 연결을 위해 `AVCaptureOutput`\(`AVCaptureMovieFileOutput`, `AVCaptureStillImageOutput` 및 `AVCaptureVideoDataOutput`\)에서 이미지를 지향하는 방법을 지정한다.

`AVCaptureConnectionsupportsVideoOrientation` 속성을 사용하여 장치가 비디오 방향 변경을 지원하는지 여부를 결정하고, 출력 포트에서 이미지 방향을 지정하는 방법을 지정하려면 `videoOrientation` 속성을 사용하라. 목록 4-1은 [`AVCaptureConnection`](https://developer.apple.com/documentation/avfoundation/avcaptureconnection)을 [`AVCaptureVideoOrientationLandscapeLeft`](https://developer.apple.com/documentation/avfoundation/avcapturevideoorientation/landscapeleft)로 방향 설정을 하는 방법을 보여준다:

**목록 4-1** 캡처 연결의 방향 설정

```objectivec
AVCaptureConnection *captureConnection = <#A capture connection#>;
if ([captureConnection isVideoOrientationSupported])
{
    AVCaptureVideoOrientation orientation = AVCaptureVideoOrientationLandscapeLeft;
    [captureConnection setVideoOrientation:orientation];
}
```

**장치 구성**

장치에 캡처 속성을 설정하려면 먼저 [`lockForConfiguration:`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/1387810-lockforconfiguration)를 사용하여 장치의 잠금을 획득하라. 이렇게 하면 다른 애플리케이션의 설정과 호환되지 않을 수 있는 변경사항이 발생하지 않는다. 다음의 코드 조각은 먼저 모드가 지원되는지 여부를 결정한 다음 재구성을 위해 장치를 잠그는 방법을 설명한다. 포커스 모드는 잠금을 획득한 경우에만 변경되며, 이후 즉시 잠금이 해제된다.

```objectivec
if ([device isFocusModeSupported:AVCaptureFocusModeLocked]) {
    NSError *error = nil;
    if ([device lockForConfiguration:&error]) {
        device.focusMode = AVCaptureFocusModeLocked;
        [device unlockForConfiguration];
    }
    else {
        // Respond to the failure as appropriate.
```

설정 가능한 장치 속성을 변경하지 않고 유지해야 하는 경우에만 장치 잠금을 유지하라. 장치 잠금을 불필요하게 유지하면 장치를 공유하는 다른 애플리케이션에서 캡처 품질이 저하될 수 있다.

**장치 간 전환**

때로는 사용자가 입력 장치 사이를 전환할 수 있도록 허용해야 할수도 있다. 예를 들어, 전면 사용에서 후면 카메라로 전환하는 것이다. 일시 중지 또는 더듬기를 방지하려면 세션이 실행되는 동안 세션을 재구성할 수 있지만, [`beginConfiguration`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1389174-beginconfiguration) 및 [`commitConfiguration`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1388173-commitconfiguration)을 사용하여 구성 변경사항을 괄호로 묶어라.

```objectivec
AVCaptureSession *session = <#A capture session#>;
[session beginConfiguration];
 
[session removeInput:frontFacingCameraDeviceInput];
[session addInput:backFacingCameraDeviceInput];
 
[session commitConfiguration];
```

가장 바깥쪽 `commitConfiguration`이 호출되면, 모든 변경이 함께 이루어진다. 이것은 원활한 전환을 보장한다.

### 캡처 입력을 사용하여 세션에 캡처 장치 추가

캡처 세션에 캡처 장치를 추가하려면 [`AVCaptureDeviceInput`](https://developer.apple.com/documentation/avfoundation/avcapturedeviceinput) \(추상 [`AVCaptureInput`](https://developer.apple.com/documentation/avfoundation/avcaptureinput) 클래스의 구체적인 서브클래스\) 인스턴스를 사용하라. 캡처 장치 입력은 장치의 포트를 관리한다.

```objectivec
NSError *error;
AVCaptureDeviceInput *input =
        [AVCaptureDeviceInput deviceInputWithDevice:device error:&error];
if (!input) {
    // Handle the error appropriately.
}
```

[`addInput:`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1387239-addinput)을 사용하여 세션에 입력을 추가한다. 적절한 경우, [`canAddInput:`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1387180-canaddinput)를 사용하여 캡처 입력이 기존 세션과 호환되는지 여부를 확인할 수 있다.

```objectivec
AVCaptureSession *captureSession = <#Get a capture session#>;
AVCaptureDeviceInput *captureDeviceInput = <#Get a capture device input#>;
if ([captureSession canAddInput:captureDeviceInput]) {
    [captureSession addInput:captureDeviceInput];
}
else {
    // Handle the failure.
}
```

실행 중인 세션을 재구성하는 방법에 대한 자세한 내용은 [Configuring a Session](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/04_MediaCapture.html#//apple_ref/doc/uid/TP40010188-CH5-SW16)을 참조하라.

`AVCaptureInput`은 하나 이상의 미디어 데이터 스트림을 vend한다. 예를 들어, 입력 장치는 오디오 데이터와 비디오 데이터를 모두 제공할 수 있다. 입력에 의해 제공되는 각각의 미디어 스트림은 [`AVCaptureInputPort`](https://developer.apple.com/documentation/avfoundation/avcaptureinputport) 객체에 의해 표현된다. 캡처 세션은 `AVCaptureConnection` 객체를 사용하여 `AVCaptureInputPort` 객체 집합과 단일 `AVCaptureOutput`간의 매핑을 정의한다.

### Use Capture Outputs to Get Output from a Session

캡처 세션에서 출력을 가져오려면 하나 이상의 출력을 추가하라. 출력은 [`AVCaptureOutput`](https://developer.apple.com/documentation/avfoundation/avcaptureoutput)의 구체 하위 클래스의 한 인스턴스이다. 사용하는:

* [`AVCaptureMovieFileOutput`](https://developer.apple.com/documentation/avfoundation/avcapturemoviefileoutput) 영화 파일 출력
* [`AVCaptureVideoDataOutput`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput) 사용자 정의 커스텀 뷰 레이어를 생성하는 것과 같이 캡처되는 비디오에서 프레임을 처리하기를 원한다면
* [`AVCaptureAudioDataOutput`](https://developer.apple.com/documentation/avfoundation/avcaptureaudiodataoutput) 캡처되는 오디오 데이터를 처리하려면 
* [`AVCaptureStillImageOutput`](https://developer.apple.com/documentation/avfoundation/avcapturestillimageoutput) 첨부된 메타데이터로 정지 이미지를 캡처하려면

[`addOutput:`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1387325-addoutput)를 사용하여 캡처 세션에 출력을 추가한다. [`canAddOutput:`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1388944-canaddoutput)를 사용하여 캡처 출력이 기존 세션과 호환되는지 확인하라. 세션이 실행되는 동안 필요에 따라 출력을 추가 및 제거할 수 있다.

```objectivec
AVCaptureSession *captureSession = <#Get a capture session#>;
AVCaptureMovieFileOutput *movieOutput = <#Create and configure a movie output#>;
if ([captureSession canAddOutput:movieOutput]) {
    [captureSession addOutput:movieOutput];
}
else {
    // Handle the failure.
}
```

**동영상 파일에 저장**

\*\*\*\*[`AVCaptureMovieFileOutput`](https://developer.apple.com/documentation/avfoundation/avcapturemoviefileoutput) 객체를 사용하여 동영상 데이터를 파일에 저장하라. \(`AVCaptureMovieFileOutput`은 [`AVCaptureFileOutput`](https://developer.apple.com/documentation/avfoundation/avcapturefileoutput)의 구체적인 서브 클래스로서 기본 동작의 많은 부분을 정의한다.\) 최대 녹화 기간 또는 최대 파일 크기와 같은 동영상 파일 출력의 다양한 측면을 구성할 수 있다. 또한 주어진 디스크 공간이 남아 있는 경우 녹화를 금지할 수 있다.

```objectivec
AVCaptureMovieFileOutput *aMovieFileOutput = [[AVCaptureMovieFileOutput alloc] init];
CMTime maxDuration = <#Create a CMTime to represent the maximum duration#>;
aMovieFileOutput.maxRecordedDuration = maxDuration;
aMovieFileOutput.minFreeDiskSpaceLimit = <#An appropriate minimum given the quality of the movie format and the duration#>;
```

출력에 대한 비트 전송률과 해상도는 캡처 세션의 [`sessionPreset`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1389696-sessionpreset)에 따라 달라진다. 비디오 인코딩은 일반적으로 H.264이고 오디오 인코딩은 일반적으로 AAC이다. 실제 값은 장치에 따라 다르다.

**녹화 시작**

[`startRecordingToOutputFileURL:recordingDelegate:`](https://developer.apple.com/documentation/avfoundation/avcapturefileoutput/1387224-startrecording)를 사용하여 QuickTime 동영상 녹화를 시작하라. 파일 기반 URL과 델리게이트를 제공해야 한다. 동영상 파일 출력이 기존 리소스를 덮어쓰지 않기 때문에 URL은 기존 파일을 식별해서는 안된다. 또한 지정된 위치에 쓸 수 있는 권한이 있어야 한다. 델리게이트는 [`AVCaptureFileOutputRecordingDelegate`](https://developer.apple.com/documentation/avfoundation/avcapturefileoutputrecordingdelegate) 프로토콜을 준수해야 하며 [`captureOutput:didFinishRecordingToOutputFileAtURL:fromConnections:error:`](https://developer.apple.com/documentation/avfoundation/avcapturefileoutputrecordingdelegate/1390612-fileoutput) 메서드를 구현해야 한다. 

```objectivec
AVCaptureMovieFileOutput *aMovieFileOutput = <#Get a movie file output#>;
NSURL *fileURL = <#A file URL that identifies the output location#>;
[aMovieFileOutput startRecordingToOutputFileURL:fileURL recordingDelegate:<#The delegate#>];
```

[`captureOutput:didFinishRecordingToOutputFileAtURL:fromConnections:error:`](https://developer.apple.com/documentation/avfoundation/avcapturefileoutputrecordingdelegate/1390612-fileoutput) 를 구현할 때 델리게이트가 결과 동영상을 카메라 롤 앨범에 기록할 수 있다. 또한 발생할 수 있는 오류가 있는지 확인해야 한다.

**파일이 성공적으로 작성되었는지 확인**

파일이 성공적으로 저장되었는지 확인하려면 [`captureOutput:didFinishRecordingToOutputFileAtURL:fromConnections:error:`](https://developer.apple.com/documentation/avfoundation/avcapturefileoutputrecordingdelegate/1390612-fileoutput) 구현에서 오류뿐만 아니라 오류의 사용자 정보 딕셔너리에서 확인하라:

```objectivec
- (void)captureOutput:(AVCaptureFileOutput *)captureOutput
        didFinishRecordingToOutputFileAtURL:(NSURL *)outputFileURL
        fromConnections:(NSArray *)connections
        error:(NSError *)error {
 
    BOOL recordedSuccessfully = YES;
    if ([error code] != noErr) {
        // A problem occurred: Find out if the recording was successful.
        id value = [[error userInfo] objectForKey:AVErrorRecordingSuccessfullyFinishedKey];
        if (value) {
            recordedSuccessfully = [value boolValue];
        }
    }
    // Continue as appropriate...
```

\*\*\*\*

**파일에 메타데이터 추가**

녹화 중에도 언제든지 동영상 파일 메타데이터를 설정할 수 있다. 이것은 위치정보와 마찬가지로 녹화가 시작할 때 정보를 이용할 수 없는 상황에 유용하다. 파일 출력에 대한 메타데이터는 [`AVMetadataItem`](https://developer.apple.com/documentation/avfoundation/avmetadataitem)의 배열로 표시된다; 변이 가능한 하위 클래스인 [`AVMutableMetadataItem`](https://developer.apple.com/documentation/avfoundation/avmutablemetadataitem)의 인스턴스를 사용하는 경우 고유한 메타데이터를 만든다.

```objectivec
AVCaptureMovieFileOutput *aMovieFileOutput = <#Get a movie file output#>;
NSArray *existingMetadataArray = aMovieFileOutput.metadata;
NSMutableArray *newMetadataArray = nil;
if (existingMetadataArray) {
    newMetadataArray = [existingMetadataArray mutableCopy];
}
else {
    newMetadataArray = [[NSMutableArray alloc] init];
}
 
AVMutableMetadataItem *item = [[AVMutableMetadataItem alloc] init];
item.keySpace = AVMetadataKeySpaceCommon;
item.key = AVMetadataCommonKeyLocation;
 
CLLocation *location - <#The location to set#>;
item.value = [NSString stringWithFormat:@"%+08.4lf%+09.4lf/"
    location.coordinate.latitude, location.coordinate.longitude];
 
[newMetadataArray addObject:item];
 
aMovieFileOutput.metadata = newMetadataArray;

```

#### **비디오 프레임 처리**

[`AVCaptureVideoDataOutput`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput) 객체는 위임을 사용하여 비디오 프레임을 vend한다. [`setSampleBufferDelegate:queue:`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput/1389008-setsamplebufferdelegate)를 사용하여 델리게이트를 설정한다. 델리게이트를 설정할 뿐만 아니라 델리게이트 메서드가 호출되는 시리얼 큐를 지정하라. 프레임이 적절한 순서로 델리게이트에게 전달되도록 하려면 시리얼을 사용해야 한다. 큐를 사용하여 비디오 프레임을 전달하고 처리하는 데 주어진 우선 순위를 수정할 수 있다. 샘플 구현은 [_SquareCam_](https://developer.apple.com/library/archive/samplecode/SquareCam/Introduction/Intro.html#//apple_ref/doc/uid/DTS40011190)를 참조하라.

프레임은 [`CMSampleBufferRef`](https://developer.apple.com/documentation/coremedia/cmsamplebuffer) 불투명 타입의 예로서 델리게이트 메서드인 [`captureOutput:didOutputSampleBuffer:fromConnection:`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutputsamplebufferdelegate/1385775-captureoutput)로 표시된다. 기본적으로, 버퍼는 카메라의 가장 효율적인 형식으로 방출된다. [`videoSettings`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput/1389945-videosettings) 속성을 사용하여 사용자 정의 출력 형식을 지정할 수 있다. 비디오 설정 속성은 딕셔너리이다. 현재 지원되는 키는 [`kCVPixelBufferPixelFormatTypeKey`](https://developer.apple.com/documentation/corevideo/kcvpixelbufferpixelformattypekey)이다. 권장 픽셀 포멧은 [`availableVideoCVPixelFormatTypes`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput/1387050-availablevideocvpixelformattypes)속성에 의해 반환된다. Core Graphics와 OpenGL 모두 `BGRA` 형식과 잘호환된다:

```objectivec
AVCaptureVideoDataOutput *videoDataOutput = [AVCaptureVideoDataOutput new];
NSDictionary *newSettings =
                @{ (NSString *)kCVPixelBufferPixelFormatTypeKey : @(kCVPixelFormatType_32BGRA) };
videoDataOutput.videoSettings = newSettings;
 
 // discard if the data output queue is blocked (as we process the still image
[videoDataOutput setAlwaysDiscardsLateVideoFrames:YES];)
 
// create a serial dispatch queue used for the sample buffer delegate as well as when a still image is captured
// a serial dispatch queue must be used to guarantee that video frames will be delivered in order
// see the header doc for setSampleBufferDelegate:queue: for more information
videoDataOutputQueue = dispatch_queue_create("VideoDataOutputQueue", DISPATCH_QUEUE_SERIAL);
[videoDataOutput setSampleBufferDelegate:self queue:videoDataOutputQueue];
 
AVCaptureSession *captureSession = <#The Capture Session#>;
 
if ( [captureSession canAddOutput:videoDataOutput] )
     [captureSession addOutput:videoDataOutput];
     
```

**비디오 처리를 위한 성능 고려**

세션 출력을 애플리케이션에 대해 가장 낮은 실제 해상도로 설정해야 한다. 출력을 필요 이상의 고해상도로 설정하면 처리 주기가 낭비되고 불필요하게 전력이 소비된다.

[`captureOutput:didOutputSampleBuffer:fromConnection:`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutputsamplebufferdelegate/1385775-captureoutput)의 구현이 프레임에 할당된 시간 내에 샘플 버퍼를 처리할 수 있는지 확인해야 한다. 너무 오래 걸리고 비디오 프레임을 잡으면 AVFoundation은 델리게이트 뿐만 아니라 프리뷰 레이어와 같은 다른 출력물에 프레임을 전달하는 것을 중단한다.

캡처 비디오 데이터 출력의 [`minFrameDuration`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput/1616296-minframeduration) 속성을 사용하여 프레임을 처리할 충분한 시간이 있는지 확인할 수 있으며, 그렇지 않은 경우보다 프레임 속도가 더 낮은 비용을 든다. 또한 [`alwaysDiscardsLateVideoFrames`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput/1385780-alwaysdiscardslatevideoframes) 속성이 `YES`\(기본 값\). 이렇게 하면 처리를 위해 전달되는 것이 아니라 모든 최신 비디오 프레임이 삭제된다. 또는, 만약 당신이 녹화중이고 출력 패밀리가 조금 늦는 것이 문제가 되지 않는다면, 당신은 모든 출력 패밀리 얻고자 한다면, 당신은 속성 값을  `NO`로 설정할 수 있다. 이는 프레임이 삭제되지 않고\(즉, 프레임은 여전히 삭제될 수 있음\) 조기에 또는 효율적으로 삭제되지 않을 수 있음을 의미한다.

**스틸 이미지 캡처**

메타데이터와 함께 스틸 이미지를 캡처하려면 [`AVCaptureStillImageOutput`](https://developer.apple.com/documentation/avfoundation/avcapturestillimageoutput) 출력을 사용하라. 영상의 해상도는 장치뿐만 아니라 세션의 사전 설정에 따라 달라진다.

**픽셀 및 인코딩 형식**

다른 장치들은 다른 이미지 포멧을 지원한다. [`availableImageDataCVPixelFormatTypes`](https://developer.apple.com/documentation/avfoundation/avcapturestillimageoutput/1388622-availableimagedatacvpixelformatt) 및 [`availableImageDataCodecTypes`](https://developer.apple.com/documentation/avfoundation/avcapturestillimageoutput/1388312-availableimagedatacodectypes)을 각각 사용하여 장치에서 지원하는 픽셀과 코덱 유형을 확인할 수 있다. 각 메서드는 특정 디바이스에 대한 지원된 값들의 배열을 반환한다. [`outputSettings`](https://developer.apple.com/documentation/avfoundation/avcapturestillimageoutput/1389306-outputsettings) 딕셔너리를 설정하여 원하는 이미지 형식을 지정하라, 예를 들어:

```objectivec
AVCaptureStillImageOutput *stillImageOutput = [[AVCaptureStillImageOutput alloc] init];
NSDictionary *outputSettings = @{ AVVideoCodecKey : AVVideoCodecJPEG};
[stillImageOutput setOutputSettings:outputSettings];
```

JPEG 이미지를 캡처하려면 일반적으로 고유한 압축 형식을 지정하지 마라. 압축은 하드웨어 가속이므로 대신 스틸 이미지 출력이 압축을 수행하도록 해야 한다. 이미지의 데이터 표현이 필요한 경우 [`jpegStillImageNSDataRepresentation:`](https://developer.apple.com/documentation/avfoundation/avcapturestillimageoutput/1388131-jpegstillimagensdatarepresentati)를 사용하여 이미지의 메타데이터를 수정하더라도 데이터를 압축하지 않고 `NSData` 객체를 얻을 수 있다.

**이미지 캡처**

이미지를 캡처하려면 출력에[`captureStillImageAsynchronouslyFromConnection:completionHandler:`](https://developer.apple.com/documentation/avfoundation/avcapturestillimageoutput/1387374-capturestillimageasynchronouslyf)메시지를 보내라. 첫 번째 인수는 캡처에 사용할 연결이다. 입력 포트가 비디오를 수집하는 연결을 찾아야 한다:

```objectivec
AVCaptureConnection *videoConnection = nil;
for (AVCaptureConnection *connection in stillImageOutput.connections) {
    for (AVCaptureInputPort *port in [connection inputPorts]) {
        if ([[port mediaType] isEqual:AVMediaTypeVideo] ) {
            videoConnection = connection;
            break;
        }
    }
    if (videoConnection) { break; }
}
```

`captureStillImageAsynchronouslyFromConnection:completionHandler:`를 캡처하기 위한 두 번째 인수는 이미지 데이터를 포함하는 `CMSampleBuffer` 불투명 유형과 오류의 두 가지 인수를 사용하는 블록이다. 샘플 버터 자체는 EXIF 사전과 같은 메타데이터를 첨부 파일로 포함할 수 있다. 원하는 경우 첨부 파일을 수정할 수 있지만 [Pixel and Encoding Formats](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/04_MediaCapture.html#//apple_ref/doc/uid/TP40010188-CH5-SW31)에서 설명하는 JPEG 영상에 대한 최적화에 유의하라.

```objectivec
[stillImageOutput captureStillImageAsynchronouslyFromConnection:videoConnection completionHandler:
    ^(CMSampleBufferRef imageSampleBuffer, NSError *error) {
        CFDictionaryRef exifAttachments =
            CMGetAttachment(imageSampleBuffer, kCGImagePropertyExifDictionary, NULL);
        if (exifAttachments) {
            // Do something with the attachments.
        }
        // Continue as appropriate.
    }];
```

### 사용자에게 녹화 중인 내용 표시

사용자에게 카메라\(프리뷰 레이어 사용\) 또는 마이크\(오디오 채널 모니터링\)에 의해 녹음되는 내용의 프리뷰를 제공할 수 있다.

#### 비디오 프리뷰

[`AVCaptureVideoPreviewLayer`](https://developer.apple.com/documentation/avfoundation/avcapturevideopreviewlayer) 객체를 사용하여 녹화되는 내용을 미리 볼 수 있다. `AVCaptureVideoPreviewLayer`는 [`CALayer`](https://developer.apple.com/documentation/quartzcore/calayer)의 서브 클래스이다\([_Core Animation Programming Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004514) __참조\). 프리뷰를 보여주기 위해 출력이 필요하지 않다.

[`AVCaptureVideoDataOutput`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput) 클래스를 사용하면 클라이언트 애플리케이션에서 비디오 픽셀을 사용자에게 표시하기 전에 접근할 수 있는 기능을 제공한다.

캡처 출력과 달리, 비디오 프리뷰 레이어는 그것이 연관된 세션에 강한 참조를 유지한다. 레이어가 비디오를 표시하려고 시도 하는 동안 세션이 할당되지 않도록 하기 위한 것이다. 이는 프리뷰 레이어를 초기화하는 방법에 반영된다:

```objectivec
AVCaptureSession *captureSession = <#Get a capture session#>;
CALayer *viewLayer = <#Get a layer from the view in which you want to present the preview#>;
 
AVCaptureVideoPreviewLayer *captureVideoPreviewLayer = [[AVCaptureVideoPreviewLayer alloc] initWithSession:captureSession];
[viewLayer addSublayer:captureVideoPreviewLayer];
```

일반적으로, 프리뷰 레이어는 렌더 트리의 다른 `CALayer` 객체처럼 동작한다\([_Core Animation Programming Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004514) 참조\). 어떤 레이어처럼 이미지를 스케일링하고 변환, 회전 등을 수행할 수 있다. 한 가지 다른 점은 레이어의 [`orientation`](https://developer.apple.com/documentation/avfoundation/avcapturevideopreviewlayer/1623494-orientation) 속성을 설정하여 카메라에서 나오는 영상을 회전하는 방법을 지정해야 할 수 있다는 점이다. 또한 [`supportsVideoMirroring`](https://developer.apple.com/documentation/avfoundation/avcaptureconnection/1387424-isvideomirroringsupported) 속성을 쿼리하여 비디오 미러링에 대한 장치 지원을 테스트할 수 있다. 필요에 따라 [`videoMirrored`](https://developer.apple.com/documentation/avfoundation/avcaptureconnection/1389172-isvideomirrored) 속성을 설정할 수 있지만, [`automaticallyAdjustsVideoMirroring`](https://developer.apple.com/documentation/avfoundation/avcaptureconnection/1387082-automaticallyadjustsvideomirrori) 속성이 `YES`\(기본 값\)으로 설정되면 세션 구성에 따라 미러링 값이 자동으로 설정된다.

**비디오 중력 모드**

프리뷰 레이어는 [`videoGravity`](https://developer.apple.com/documentation/avfoundation/avcapturevideopreviewlayer/1386708-videogravity)를 사용하여 세가지 중력 모드를 지원한다:

* [`AVLayerVideoGravityResizeAspect`](https://developer.apple.com/documentation/avfoundation/avlayervideogravity/1387116-resizeaspect): 가로 세로 비율이 유지되어 비디오가 사용 가능한 화면 영역을 채우지 않는 검은색 막대가 남게 된다.
* [`AVLayerVideoGravityResizeAspectFill`](https://developer.apple.com/documentation/avfoundation/avlayervideogravity/1385607-resizeaspectfill): 가로 세로 비율은 유지되지만 사용 가능한 화면 영역을 채워 필요할 때 비디오를 자른다.
* [`AVLayerVideoGravityResize`](https://developer.apple.com/documentation/avfoundation/avlayervideogravity/1387460-resize): 영상이 왜곡되더라도 사용 가능한 화면 영역을 채우기 위해 단순히 비디오를 늘린다.

**미리보기를 사용하여 "Tap to Focus" 사용**

프리뷰 레이어와 함께 tap-to-focus를 구현할 때는 주의해야 한다. 미리보기 방향과 레이어의 gravity, 그리고 미리보기가 미러링될 수 있는 가능성을 고려해야 한다. 이 기능을 구현하려면 샘플 코드 프로젝트 [_AVCam-iOS: Using AVFoundation to Capture Images and Movies_](https://developer.apple.com/library/archive/samplecode/AVCam/Introduction/Intro.html#//apple_ref/doc/uid/DTS40010112)를 참조하라.

#### 오디오 수준 표시

캡처 연결에서 오디오 채널의 평균 및 피크 전력 수준을 모니터링 하려면, [`AVCaptureAudioChannel`](https://developer.apple.com/documentation/avfoundation/avcaptureaudiochannel) 객체를 사용한다. 오디오 수준은 키 값을 관측할 수 없으므로 사용자 인터페이스를 업데이트하려는 횟수만큼\(예: 초당 10회\) 업데이트된 수준에 대해 폴링해야 한다.

```objectivec
AVCaptureAudioDataOutput *audioDataOutput = <#Get the audio data output#>;
NSArray *connections = audioDataOutput.connections;
if ([connections count] > 0) {
    // There should be only one connection to an AVCaptureAudioDataOutput.
    AVCaptureConnection *connection = [connections objectAtIndex:0];
 
    NSArray *audioChannels = connection.audioChannels;
 
    for (AVCaptureAudioChannel *channel in audioChannels) {
        float avg = channel.averagePowerLevel;
        float peak = channel.peakHoldLevel;
        // Update the level meter user interface.
    }
}Ob
```

### 모든 것을 하나로 통합: UIImage 객체로 비디오 프레임 캡처

이 간단한 코드 예는 비디오를 캡처하고 UIImage 객체로 프레임을 변환하는 방법을 보여준다. 이 문서에서는 다음 방법을 보여준다:

* 입력 장치에서 출력으로 데이터 흐름을 조정하는 [`AVCaptureSession`](https://developer.apple.com/documentation/avfoundation/avcapturesession) 객체 생성
* 원하는 입력 타입에 대한 [`AVCaptureDevice`](https://developer.apple.com/documentation/avfoundation/avcapturedevice) 객체 찾기
* 장치에 대한 [`AVCaptureDeviceInput`](https://developer.apple.com/documentation/avfoundation/avcapturedeviceinput) 객체 만들기
* [`AVCaptureVideoDataOutput`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput) 객체를 생성하여 비디오 프레임 생성
* 비디오 프레임을 처리할 [`AVCaptureVideoDataOutput`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput) 객체에 대한 델리게이트 구현
* 델리게이트가 받은 `CMSampleBuffer`를 `UIImage` 객체로 변환하는 함수 구현

> **참고:** 가장 관련성이 높은 코드에 초점 맞추기 위해, 이 예에서는 메모리 관리를 포함한 전체 애플리케이션의 몇 가지 측면을 생략한다. AVFoundation을 이용하기 위해서는 Cocoa에 대한 충분한 경험이 있어야 잃어버린 조각을 유추할 수 있다.

**캡처 세션 만들기 및 구성**

[`AVCaptureSession`](https://developer.apple.com/documentation/avfoundation/avcapturesession) 객체를 사용하여 AV 입력 장치에서 출력으로 데이터 흐름을 조정하라. 세션을 생성하고 중간 해상도의 비디오 프레임을 생성하도록 구성하라.

```objectivec
AVCaptureSession *session = [[AVCaptureSession alloc] init];
session.sessionPreset = AVCaptureSessionPresetMedium;
```

**장치, 장치 입력을 생성 및 구성**

캡처 장치는 [`AVCaptureDevice`](https://developer.apple.com/documentation/avfoundation/avcapturedevice) 객체로 표시되며 클래스는 원하는 입력 유형에 대한 객체를 검색하는 방법을 제공한다. 장치에 [`AVCaptureInput`](https://developer.apple.com/documentation/avfoundation/avcaptureinput) 객체를 사용하여 설정된 하나 이상의 포트가 있다. 일반적으로 캡처 입력을 기본 구성에서 사용한다.

비디오 캡처 장치를 찾은 다음 장치로 장치 입력을 만들어 세션에 추가하라. 적절한 장치를 찾을 수 없는 경우 [`deviceInputWithDevice:error:`](https://developer.apple.com/documentation/avfoundation/avcapturedeviceinput/1450880-deviceinputwithdevice) 메서드는 참조로 오류를 반환한다.

```objectivec
AVCaptureDevice *device =
        [AVCaptureDevice defaultDeviceWithMediaType:AVMediaTypeVideo];
 
NSError *error = nil;
AVCaptureDeviceInput *input =
        [AVCaptureDeviceInput deviceInputWithDevice:device error:&error];
if (!input) {
    // Handle the error appropriately.
}
[session addInput:input];
```

#### 비디오 데이터 출력 생성 및 구성

[`AVCaptureVideoDataOutput`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput) 객체를 사용하여 캡처되는 비디오에서 압축되지 않은 프레임을 처리하라. 일반적으로 출력의 여러 측면을 구성하라. 비디오의 경우, [`videoSettings`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput/1389945-videosettings) 속성을 사용하여 픽셀 형식을 지정하고 [`minFrameDuration`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput/1616296-minframeduration) 속성을 설정하여 프레임 속도를 캡할 수 있다. 

비디오 데이터에 대한 출력을 생성하고 설정하여 세션에 추가하라. `minFrameDuration`속성을 1/15초로 구성하여 프레임률을 15fps로 제한하라.

```objectivec
AVCaptureVideoDataOutput *output = [[AVCaptureVideoDataOutput alloc] init];
[session addOutput:output];
output.videoSettings =
                @{ (NSString *)kCVPixelBufferPixelFormatTypeKey : @(kCVPixelFormatType_32BGRA) };
output.minFrameDuration = CMTimeMake(1, 15);
```

데이터 출력 객체는 비디오 프레임에 vend 하기 위해 델리게이션을 사용한다. 델리게이트는 [`AVCaptureVideoDataOutputSampleBufferDelegate`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutputsamplebufferdelegate) 프로토콜을 채택해야 한다. 데이터 출력의 델리게이트를 설정할 때는 콜백을 호출할 큐도 제공해야 한다.

```objectivec
dispatch_queue_t queue = dispatch_queue_create("MyQueue", NULL);
[output setSampleBufferDelegate:self queue:queue];
dispatch_release(queue);
```

큐를 사용하여 비디오 프레임 전송 및 처리에 지정된 우선 순위를 수정하라.

**샘플 버퍼 델리게이트 메서드 구현**

델리게이트 클래스에서 샘플 버퍼가 기록될 때 호출되는 메서드 [`captureOutput:didOutputSampleBuffer:fromConnection:`](https://developer.apple.com/documentation/avfoundation/avcaptureaudiodataoutputsamplebufferdelegate/1386039-captureoutput)를 실행하라. 비디오 데이터 출력 객체는 프레임을 `CMSampleBuffer` 불투명 타입으로 전달하므로 `CMSampleBuffer` 불투명 타입에서 `UIImage` 객체로 변환해야 한다. 이 작업의 함수는 [Converting CMSampleBuffer to a UIImage Object](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/06_MediaRepresentations.html#//apple_ref/doc/uid/TP40010188-CH2-SW4)에 보여진다.

```objectivec
- (void)captureOutput:(AVCaptureOutput *)captureOutput
         didOutputSampleBuffer:(CMSampleBufferRef)sampleBuffer
         fromConnection:(AVCaptureConnection *)connection {
 
    UIImage *image = imageFromSampleBuffer(sampleBuffer);
    // Add your code here that uses the image.
}
```

[`setSampleBufferDelegate:queue:`](https://developer.apple.com/documentation/avfoundation/avcapturevideodataoutput/1389008-setsamplebufferdelegate) 델리게이트 메서드에서 지정한 큐에 위임 메서드가 호출됨을 기억하라; 사용자 인터페이스를 업데이트하려면 메인 쓰레드에 관련 코드를 호출해야 한다.

**녹화 시작 및 중지**

캡처 세션을 구성한 후에는 사용자의 기본 설정에 따라 카메라에 녹화할 수 있는 권한이 있는지 확인하라.

```objectivec
NSString *mediaType = AVMediaTypeVideo;
 
[AVCaptureDevice requestAccessForMediaType:mediaType completionHandler:^(BOOL granted) {
    if (granted)
    {
        //Granted access to mediaType
        [self setDeviceAuthorized:YES];
    }
    else
    {
        //Not granted access to mediaType
        dispatch_async(dispatch_get_main_queue(), ^{
        [[[UIAlertView alloc] initWithTitle:@"AVCam!"
                                    message:@"AVCam doesn't have permission to use Camera, please change privacy settings"
                                   delegate:self
                          cancelButtonTitle:@"OK"
                          otherButtonTitles:nil] show];
                [self setDeviceAuthorized:NO];
        });
    }
}];
```

카메라 세션이 구성되어 있고 사용자가 카메라에 대한 접근\(필요한 경우 마이크\)를 승인한 경우 [`startRunning`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1388185-startrunning) 메시지를 보내 녹화를 시작하라.

> **중요:** `startRunning` 메서드는 시간이 좀 걸릴 수 있는 블로킹 호출이므로, 메인 큐가 차단되지 않도록 \(UI 응답을 유지\) 시리얼 큐에서 세션 설정을 수행해야 한다. 표준 구현 예제에 대해서는 [_AVCam-iOS: Using AVFoundation to Capture Images and Movies_](https://developer.apple.com/library/archive/samplecode/AVCam/Introduction/Intro.html#//apple_ref/doc/uid/DTS40010112)을 참조하여 이미지 및 동영상을 캡처하라.

```objectivec
[session startRunning];
```

녹화를 중지하려면 세션에 [`stopRunning`](https://developer.apple.com/documentation/avfoundation/avcapturesession/1385661-stoprunning) 메시지를 보내라.

### 높은 프레임 속도 비디오 캡처

iOS 7.0은 선택된 하드웨어에 높은 프레임 속도 비디어 캡처 지원\("SloMo" 비디오라고도 함\)을 도입한다. 전체 AVFoundation 프레임워크는 높은 프레임 속도 콘텐츠를 지원한다.

[`AVCaptureDeviceFormat`](https://developer.apple.com/documentation/avfoundation/avcapturedevice/format) 클래스를 사용하여 장치의 캡처 기능을 결정한다. 이 클래스는 지원되는 미디어 유형, 프레임 속도, 시야, 최대 확대/축소 비율, 비디오 안정화 지원 여부 등을 반환하는 방법 등의 메서드를 갖는다.

* 캡처는 비디오 안정화 및 드롭가능 P-프레임\(더 느리고 오래된 하드웨어에서도 원할하게 재생할 수 있는 H264 인코딩 영화의 특징\)을 포함한 초당 60 프레임\(fps\)에서 전체 720p \(1280 x 720 픽셀\) 해상도를 지원한다.
* 재생은 느리고 빠른 재생을 위한 오디오 지원을 강화하여 오디오의 시간 피치를 느리거나 빠른 속도로 보존할 수 있게 한다.
* 편집은 변이 가능한 컴포지션의 크기 조정 편집에 대한 완전한 지원을 가지고 있다.
* 내보내기는 60 fps 동영상을 지원할 때 두 가지 옵션을 제공한다. 느린 동작이나 빠른 동작의 가변 프레임률을 보존하거나 동영상을 보존하여 초당 30프레임과 같은 임의의 느린 프레임 속도로 변환할 수 있다.

_Slopoke_ 샘플 코드는 고속 비디오 캡처를 위한 AVFoundation 지원을 시연하고, 하드웨어가 높은 프레임 속도 비디오 캡처를 지원하는지 여부를 판단하고, 다양한 속도 및 시간 피치 알고리즘을 사용하여 재생하며, 편집\(컴포지션 부분의 시간 척도 설정 포함\)한다.

#### 재생

`AVPlayer`의 인스턴스는 `setRate:` 메서드 값을 설정하며 재생속도의 대부분을 자동으로 관리한다. 이 값은 재생 속도의 곱으로 사용된다. 1.0 의 값은 정상적인 재생을 일으키고 0.5 는 절반 속도로 재생하고 5.0 은 정상보다 5배 빨리 재생한다.

`AVPlayerItem` 객체는 [`audioTimePitchAlgorithm`](https://developer.apple.com/documentation/avfoundation/avplayeritem/1385855-audiotimepitchalgorithm) 속성을 지원한다. 이 속성을 사용하면 `Time Pitch Algorithm Settings` 상수를 사용하여 다양한 프레임 속도로 동영상을 재생할 때 오디오를 재생하는 방법을 지정할 수 있다.

다음 표는 지원되는시간 피치 알고리즘, 품질, 알고리즘이 오디오를 특정 프레임 속도에 스냅하도록 하는지 여부 및 각 알고리즘이 지원하는 플레임 속도 범위를 보여준다.

| Time pitch algorithm | Quality | Snaps to specific frame rate | Rate range |
| :--- | :--- | :--- | :--- |
| [`AVAudioTimePitchAlgorithmLowQualityZeroLatency`](https://developer.apple.com/documentation/avfoundation/avaudiotimepitchalgorithmlowqualityzerolatency) | Low quality, suitable for fast-forward, rewind, or low quality voice. | `YES` | 0.5, 0.666667, 0.8, 1.0, 1.25, 1.5, 2.0 rates. |
| [`AVAudioTimePitchAlgorithmTimeDomain`](https://developer.apple.com/documentation/avfoundation/avaudiotimepitchalgorithm/1387220-timedomain) | Modest quality, less expensive computationally, suitable for voice. | `NO` | 0.5–2x rates. |
| [`AVAudioTimePitchAlgorithmSpectral`](https://developer.apple.com/documentation/avfoundation/avaudiotimepitchalgorithm/1388153-spectral) | Highest quality, most expensive computationally, preserves the pitch of the original item. | `NO` | 1/32–32 rates. |
| [`AVAudioTimePitchAlgorithmVarispeed`](https://developer.apple.com/documentation/avfoundation/avaudiotimepitchalgorithmvarispeed) | High-quality playback with no pitch correction. | `NO` | 1/32–32 rates. |

**편집**

편집할 때, `AVMutableComposition` 클래스를 사용하여 시간 편집을 작성한다.

* [`composition`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1495098-composition) 클래스 메서드를 사용하여 새로운 [`AVMutableComposition`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition) 인스턴스를 생성하라.
* [`insertTimeRange:ofAsset:atTime:error:`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1385943-inserttimerange) 메서드를 사용하여 비디오 에셋을 삽입하라
* [`scaleTimeRange:toDuration:`](https://developer.apple.com/documentation/avfoundation/avmutablecomposition/1390549-scaletimerange) 를 사용하여 컴포지션 부분의 시간 척도를 설정하라.

#### 내보내기

60 fps 동영상 내보내기는 `AVAssetExportSession` 클래스를 사용하여 에셋을 내보낸다. 콘텐츠는 다음 두 가지 기법을 사용하여 내보낼 수 있다:

* 동영상을 재인코딩하지 않으려면 [`AVAssetExportPresetPassthrough`](https://developer.apple.com/documentation/avfoundation/avassetexportpresetpassthrough) 사전 설정을 사용하라. 미디어 섹션이 섹션 60fps로 태그가 지정된 상태에서 섹션 속도가 느려지거나 섹션 속도가 빨라진 상태로 미디어를 리테이닝한다.
* 최대 재생 호환성을 위해 일정한 프레임률 내보내기를 사용하라. 비디오 컴포지션의 [`frameDuration`](https://developer.apple.com/documentation/avfoundation/avvideocomposition/1388013-frameduration) 속성을 30fps로 설정하라. 내보내기 세션의 [`audioTimePitchAlgorithm`](https://developer.apple.com/documentation/avfoundation/avassetexportsession/1385835-audiotimepitchalgorithm) 속성을 설정하여 시간 피치를 설정할 수도 있다.

#### 녹음

높은 프레임률 녹음을 자동으로 지원하는 [`AVCaptureMovieFileOutput`](https://developer.apple.com/documentation/avfoundation/avcapturemoviefileoutput) 클래스를 사용하여 높은 프레임률 비디오를 캡처하라. 자동으로 올바른 H264 피치 레벨과 비트율을 선택한다.

사용자 정의 녹음을 하려면 [`AVAssetWriter`](https://developer.apple.com/documentation/avfoundation/avassetwriter) 클래스를 사용해야 하며, 이 클래스는 추가 설정이 필요하다.

```objectivec
assetWriterInput.expectsMediaDataInRealTime=YES;
```

이 설정은 캡처가 들어오는 데이터를 따라갈 수 있도록 보장한다.

