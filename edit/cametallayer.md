---
description: 'Metal이 렌더링할 수 있는 Core Animation 레이어, 일반적으로 화면에 표시될 수 있다.'
---

# CAMetalLayer

### Declaration

```swift
class CAMetalLayer : CALayer
```

### Overview

Metal을 사용하여 레이어의 내용을 렌더링하려면 [`CAMetalLayer`](https://developer.apple.com/documentation/quartzcore/cametallayer)를 사용하라. 대신 [`MTKView`](https://developer.apple.com/documentation/metalkit/mtkview)를 사용하는 것을 고려하라. 이 클래스는 자동으로 `CAMetalLayer` 객체를 감싸고 더 높은 수준의 추상화를 제공하기 때문이다.

UIKit을 사용하여 `CAMetalLayer`를 사용하는 뷰를 만들고, [`UIView`](https://developer.apple.com/library/archive/releasenotes/iPhone/RN-iPhoneSDK/index.html#//apple_ref/doc/uid/TP40007428-CH1-SW18) 하위 클래스를 만들고 [`layerClass`](https://developer.apple.com/documentation/uikit/uiview/1622626-layerclass) 클래스 메서드를 오버라이드하여 `CAMetalLayer`를 반환한다:

```text
+ (Class) layerClass
{
    return [CAMetalLayer class];
}
```

AppKit을 사용하는 경우 백킹 레이어를 사용하도록 [`NSView`](https://developer.apple.com/documentation/appkit/nsview) 객체를 구성하고 [`CAMetalLayer`](https://developer.apple.com/documentation/quartzcore/cametallayer) 객체를 뷰에 할당하라:

```text
myView.wantsLayer = YES;
myView.layer = [CAMetalLayer layer];
```

레이어의 속성을 조정하여 기본 픽셀 형식 및 기타 표시 동작을 구성하라.

#### Rendering the Layer's Contents <a id="3385505"></a>

`CAMetalLayer`는 Metal 그리기 가능한 객체의 풀을 생성한다 \([`CAMetalDrawable`](https://developer.apple.com/documentation/quartzcore/cametaldrawable)\). 어느 주어진 시간에, 이 그리기 가능한 객체들 중 하나는 레이어의 내용을 포함한다. 레이어의 내용을 변경하려면 레이어에 그리기 가능한 객체를 요청하여 레이어에 렌더링한 다음 레이어의 내용을 업데이트하여 새로운 레이어를 가리킨다.

레이어의 [`nextDrawable()`](https://developer.apple.com/documentation/quartzcore/cametallayer/1478172-nextdrawable) 메서드를 호출하여 그리기 가능한 객체를 얻어라. 아래 코드와 같이 그리기 가능한 객체의 텍스처를 가져와서 해당 텍스처로 렌더링하는 렌더 패스를 만들어라:

```text
CAMetalLayer *metalLayer = (CAMetalLayer*)self.layer;
id<CAMetalDrawable> *drawable = [metalLayer nextDrawable];

MTLRenderPassDescriptor *renderPassDescriptor
                               = [MTLRenderPassDescriptor renderPassDescriptor];

renderPassDescriptor.colorAttachments[0].texture = drawable.texture;
renderPassDescriptor.colorAttachments[0].loadAction = MTLLoadActionClear;
renderPassDescriptor.colorAttachments[0].clearColor = MTLClearColorMake(0.0,0.0,0.0,1.0);
...
```

레이어의 내용을 새로운 그리기 가능으로 변경하려면 인코딩된 렌더 패스를 포함하는 명령 버퍼에서 [`present(_:)`](https://developer.apple.com/documentation/metal/mtlcommandbuffer/1443029-present) 메서드 \(또는 그 변형 중 하나\)으로 호출하여 표시할 그리기 가능 객체를 전달하라.

```text
[commandBuffer presentDrawable:drawable];
```

#### Keeping References to Drawables <a id="3385893"></a>

레이어가 스크린에 표시되지 않고 강력한 참조가 없는 경우에만 레이어를 재사용한다. 또한 [`nextDrawable()`](https://developer.apple.com/documentation/quartzcore/cametallayer/1478172-nextdrawable)을 호출할 때 그리기가능을 사용할 수 없는 경우, 그 시스템은 그 사람이 이용 가능해지기를 기다린다. 앱에서 스톨을 방지하려면 필요한 경우에만 새로운 그리기가능을 요청하고, 완료 후 가능한 한 빨리 해당 프로그램에 대한 참조를 해제하라.

예를 들어, 새로운 그리기 가능을 검색하기 전에 CPU에서 다른 작업을 수행하거나 그리기가 필요하지 않은 명령을 GPU에 제출할 수 있다. 그런 다음, 그리기 가능한 명령을 획득하고 커맨드 버퍼를 인코딩하여 위에서 설명한 바와 같이 렌더링하라. 이 커맨드 버퍼를 커밋한 후 모든 강력한 참조를 그리기 가능으로 릴리즈하라. 그리기를 올바르게 해제하지 않으면 drawables이 부족해지고 [`nextDrawable()`](https://developer.apple.com/documentation/quartzcore/cametallayer/1478172-nextdrawable) 호출은 nil을 리턴한다.

#### Releasing the Drawable <a id="3172024"></a>

drawable을 명시적으로 해제하지 말고 렌더 루프를 오토릴리즈 풀 블록에 포함하라:

```swift
func draw(in view: MTKView) {
    autoreleasepool {
        render(view: view)
    }
}
```

이 블록은 drawables를 즉시 해제하고 다중 drawables로 인해 발생할 수 있는 교착상태를 방지한다. 화면 렌더 패스를 커밋한 후 가능한 한 빨리 drawables를 해제하라.

> **메모**
>
> iOS 10 및 tvOS 10은 [`drawableID`](https://developer.apple.com/documentation/metal/mtldrawable/2806860-drawableid) 및 [`presentedTime`](https://developer.apple.com/documentation/metal/mtldrawable/2806855-presentedtime)과 같은 속성을 쿼리하기 위해 속성을 안전하게 보관할 수 있다. 이러한 속성을 조회할 필요가 없는 경우 더 이상 해당 속성이 필요하지 않을 때 그릴 수 있는 속성을 해제하라.

### Topics

#### Configuring the Metal Device

* [`var device: MTLDevice?`](https://developer.apple.com/documentation/quartzcore/cametallayer/1478163-device) 레이어의 그릴 수 있는 리소스를 책임지는 Metal 장치.
* [`var preferredDevice: MTLDevice?`](https://developer.apple.com/documentation/quartzcore/cametallayer/3175021-preferreddevice) 시스템에서 이 레이어에 사용하도록 권장하는 장치 객체.

#### Configuring the Layer's Drawable Objects

* [`var pixelFormat: MTLPixelFormat`](https://developer.apple.com/documentation/quartzcore/cametallayer/1478155-pixelformat) 레이어의 텍스처 픽셀 포멧.
* [`var colorspace: CGColorSpace?`](https://developer.apple.com/documentation/quartzcore/cametallayer/1478170-colorspace) 렌더링된 내용의 색 공간.
* [`var framebufferOnly: Bool`](https://developer.apple.com/documentation/quartzcore/cametallayer/1478168-framebufferonly) 레이어의 텍스처가 렌더링에만 사용되는지 여부를 결정하는 불 값.
* [`var drawableSize: CGSize`](https://developer.apple.com/documentation/quartzcore/cametallayer/1478174-drawablesize) 픽셀에서 렌더링 레이어 내용에 대한 텍스처 크기이다.

#### Configuring Presentation Behavior

* [`var presentsWithTransaction: Bool`](https://developer.apple.com/documentation/quartzcore/cametallayer/1478157-presentswithtransaction) Core Animation 트랜잭션을 사용하여 컨텐츠를 표시하는지 여부를 결정하는 불 값.
* [`var displaySyncEnabled: Bool`](https://developer.apple.com/documentation/quartzcore/cametallayer/2887087-displaysyncenabled) 레이어가 업데이트를 디스플레이의 새로 고침 속도와 동기화하는지 여부를 결정하는 불 값.

#### Configuring Extended Dynamic Range Behavior

* [`var wantsExtendedDynamicRangeContent: Bool`](https://developer.apple.com/documentation/quartzcore/cametallayer/1478161-wantsextendeddynamicrangecontent) 화면에서 확장된 동적 범위 값을 활성화한다.
* [`var edrMetadata: CAEDRMetadata?`](https://developer.apple.com/documentation/quartzcore/cametallayer/3182052-edrmetadata) 레이어의 확장 동적 범위 \(EDR\) 값에 적용할 톤 매핑을 설명하는 메타데이터.

#### Obtaining a Metal Drawable

* [`func nextDrawable() -> CAMetalDrawable?`](https://developer.apple.com/documentation/quartzcore/cametallayer/1478172-nextdrawable) Metal drawable이 가능할 때까지 기다렸다가 반환한다.
* [`var maximumDrawableCount: Int`](https://developer.apple.com/documentation/quartzcore/cametallayer/2938720-maximumdrawablecount) Core Animation에서 관리하는 리소스 풀의 Metal 그리기 숫자.
* [`var allowsNextDrawableTimeout: Bool`](https://developer.apple.com/documentation/quartzcore/cametallayer/2887086-allowsnextdrawabletimeout) 시스템이 이 버퍼를 만족시키지 못할 경우 새 버퍼 요청이 만료되는지 여부를 결정하는 불 값.

