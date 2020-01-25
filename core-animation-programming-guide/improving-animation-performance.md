# Improving Animation Performance

코어 애니메이션은 앱 기반 애니메이션의 프레임률을 높이는 좋은 방법이지만 그 사용은 성능 향상을 보장하는 것은 아니다. 특히 OS X에서는 여전히 코어 애니메이션 동작을 가장 효과적으로 사용하는 방법을 선택해야 한다. 그리고 모든 성능 관련 문제와 마찬가지로, 시간이 지남에 따라 앱의 성능을 측정하고 추적하여 성능이 향상되고 퇴보하지 않도록 해야 한다.

### Choose the Best Redraw Policy for Your OS X Views

NSView 클래스에 대한 기본 다시 그리기 정책은 뷰가 레이어백으로 되어 있더라도 해당 클래스의 원래 그리기 동작을 보존한다. 앱에서 레이어백 뷰를 사용하는 경우 정책 선택을 다시 검토하고 애플리케이션에 가장 적합한 성능을 제공하는 뷰를 선택하라. 대부분의 경우 디폴트 정책은 최상의 성능을 제공할 가능성이 높은 정책이 아니다. 대신 [`NSViewLayerContentsRedrawOnSetNeedsDisplay`](https://developer.apple.com/documentation/appkit/nsviewlayercontentsredrawpolicy/nsviewlayercontentsredrawonsetneedsdisplay) 정책은 앱의 그리기 작업을 줄이고 성능을 향상시킬 가능성이 더 높다. 다른 정책들도 특정 유형의 견해에 대해 더 나은 성과를 제공할 수 있다.

다시 그리기 정책에 대한 자세한 내용은 [The Layer Redraw Policy for OS X Views Affects Performance](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/SettingUpLayerObjects/SettingUpLayerObjects.html#//apple_ref/doc/uid/TP40004514-CH13-SW22)를 참조하라.

### Update Layers in OS X to Optimize Your Rendering Path

OS X v10.8 이상 에서는 기본 레이어 콘텐츠를 업데이트하기 위한 두 가지 옵션이 있다. OS X v10.7 이전 버전에서 레이어백 뷰를 업데이트하면 레이어는 뷰의 `drawRect:` 메서드에서 백업 비트맵 이미지로 그리기 명령을 캡처한다. 레이어 명령을 캐싱하는 것이 효과적이지만 모든 경우에 가장 효율적인 옵션은 아니다. 실제로 렌더링하지 않고 직접 레이어의 콘텐츠를 제공하는 방법을 알고 있다면 [`updateLayer`](https://developer.apple.com/documentation/appkit/nsview/1483580-updatelayer)메서드를 사용하여 이를 수행할 수 있다.

`updateLayer`메서드를 포함하여 렌더링을 위한 다양한 경로에 대한 자세한 내용은 [Using a Delegate to Provide the Layer’s Content](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/SettingUpLayerObjects/SettingUpLayerObjects.html#//apple_ref/doc/uid/TP40004514-CH13-SW14)를 참조하라. 

### General Tips and Tricks

레이어 구현을 보다 효율적으로 만드는 몇 가지 방법이 있다. 그러나 이러한 최적화와 마찬가지로, 최적화를 시도하기 전에 항상 코드의 현재 성능을 측정해야 한다. 이것은 최적화가 작동하는지 여부를 결정하는 데 사용할 수 있는 기준선을 제공한다.

#### Use Opaque Layers Whenever Possible

레이어의 [`opaque`](https://developer.apple.com/documentation/quartzcore/calayer/1410763-isopaque)속성을 `YES`로 설정하면 코어 애니메이션 레이어의 알파 채널을 유지할 필요가 없음을 알 수 있다. 알파 채널이 없다는 것은 컴포지터가 레이어의 콘텐츠와 백그라운드 콘텐츠를 혼합할 필요가 없다는 것을 의미하므로 렌더링하는 동안 시간을 절약할 수 있다. 그러나 이 속성은 주로 레이어백 뷰의 일부인 레이어 또는 코어 애니메이션이 기본 레이어 비트맵을 생성하는 상황과 관련이 있다. 레이어의 [`contents`](https://developer.apple.com/documentation/quartzcore/calayer/1410773-contents)속성에 직접 영상을 할당하면 `opaque`속성의 값에 관계없이 해당 이미지의 알파 채널이 보존된다.

#### Use Simpler Paths for CAShapeLayer Objects

[`CAShapeLayer`](https://developer.apple.com/documentation/quartzcore/cashapelayer) 클래스는 사용자가 제공한 경로를 복합 시간에 비트맵 이미지로 렌더링하여 콘텐츠를 생성한다. 이점은 레이어는 항상 가능한 치선의 해상도로 경로를 그리지만 그 장점은 추가 렌더링 시간의 비용이 든다는 것이다. 만약 당신이 제공하는 경로가 복잡하다면, 그 경로를 파괴하는 것은 너무 비싸질 수가 있다. 그리고 레이어의 크기가 자주 변경되면\(따라서 자주 다시 그려야 한다\), 그리는데 소요되는 시간이 합산되어 성능 병목 현상이 될 수 있다.

형상 레이어의 그리기 시간을 최소화하는 한 가지 방법은 복잡한 형상을 단순한 형상으로 분할하는 것이다. 컴포지터의 서로 위에 더 단순한 경로를 사용하고 여러 `CAShapeLayer` 객체를 레이어하는 것은 하나의 큰 복합 경로를 그리는 것보다 훨씬 더 빠를 수 있다. 그 이유는 그리기 작업이 CPU에서 이루어지는 반면, 합성은 GPU에서 이루어지기 때문이다. 그러나 이러한 성격의 단순화와 마찬가지로, 잠재적 성능 이점은 콘텐츠에 의존한다. 따라서, 비교에 사용할 기준선을 가지도록 최적화하기 전에 코드의 성능을 측정하는 것이 특히 중요하다.

#### Set the Layer Contents Explicitly for Identical Layers

여러 레이어 객체에서 동일한 이미지를 사용하는 경우 직접 이미지를 로드하여 해당 레이어 객체의 [`contents`](https://developer.apple.com/documentation/quartzcore/calayer/1410773-contents)속성에 직접 할당하라. 이미지를 `contents`속성에 할당하면 레이어가 백업 저장소에 대한 메모리를 할당하지 못하게 된다. 대신, 레이어는 당신이 제공한 이미지를 백업 저장소로 사용한다. 여러 레이어가 동일한 이미지를 사용할 경우, 이것은 모든 레이어가 그들 스스로 이미지의 복사본을 할당하기 보다는 동일한 메모리를 공유하고 있다는 것을 의미한다.

#### Always Set a Layer’s Size to Integral Values

최상의 결과를 얻으려면 레이어 객체의 너비와 높이를 항상 적분 값으로 설정하라. 부동 소수점 숫자를 사용하여 레이어 바운드 폭과 높이를 지정하더라도, 레이어 바운드는 궁극적으로 비트맵 이미지를 생성하는 데 사용된다. 폭과 높이에 대한 통합 값을 지정하면 코어 애니메이션이 백업 저장소 및 기타 레이어 정보를 생성하고 관리하기 위해 수행해야 하는 작업이 간소화된다.

#### Use Asynchronous Layer Rendering As Needed

델리게이트의 [`drawLayer:inContext:`](https://developer.apple.com/documentation/quartzcore/calayerdelegate/2097262-drawlayer) 메서드 또는 뷰의 `drawRect:` 메서드는 일반적으로 앱의 메인 쓰레드에서 동시에 발생한다. 그러나 일부 상황에서는 콘텐츠를 동기적으로 그리는 것이 최상의 성능을 제공하지 못할 수 있다. 애니메이션이 제대로 수행되지 않는 경우 레이어에서 [`drawsAsynchronously`](https://developer.apple.com/documentation/quartzcore/calayer/1410974-drawsasynchronously)속성을 설정하여 해당 작업을 백그라운드 쓰레드로 이동하라. 이렇게 하면 그리기 코드가 쓰레드에 안전한지 확인하라. 그리고 항상 그렇듯이 생산 코드에 넣기 전에 항상 비동기적으로 그리기 성능을 측정해야 한다.

#### Specify a Shadow Path When Adding a Shadow to Your Layer

코어 애니메이션이 그림자 모양을 결정하게 되면 비용이 많이 들고 앱 성능에 영향을 줄 ㅅ 있다. 코어 애니메이션이 그림자의 모양을 결정하도록 하는 대신 `CALayer`의 [`shadowPath`](https://developer.apple.com/documentation/quartzcore/calayer/1410771-shadowpath)속성을 사용하여 명시적으로 그림자 모양을 지정하라. 이 속성의 경로 객체를 지정할 때 코어 애니메이션은 이 모양을 사용하여 그림자 효과를 그리고 캐시한다. 모양이 결코 변하지 않거나 거의 변하지 않는 레이어의 경우, 코어 애니메이션의 렌더링량을 줄임으로써 성능을 크게 향상시킨다.



