---
description: 렌더링하거나 쓸 수 있는 표시 가능한 리소스.
---

# MTLDrawable

### Declaration

```swift
protocol MTLDrawable
```

### Overview

이 프로토콜을 구현하는 객체는 Metal 프레임워크와 화면에 콘텐츠를 표시할 수 있는 기본 디스플레이 시스템 \(Core Animation 등\)에 모두 연결된다. 이미지를 Metal을 사용하여 렌더링하고 화면에 표시하려면 drawable 객체를 사용한다.

이 프로토콜을 직접 구현하지 마라. 대신, [`CAMetalLayer`](https://developer.apple.com/documentation/quartzcore/cametallayer)에서 그릴 수 있는 객체를 만들고 관리할 수 있는 클래스를 참조하라.



### Topics

#### Identifying the Drawable

* [`var drawableID: Int`](https://developer.apple.com/documentation/metal/mtldrawable/2806860-drawableid) drawable 항목을 식별하는 양의 정수. **Required.**

#### Presenting the Drawable

* [`func present()`](https://developer.apple.com/documentation/metal/mtldrawable/1470284-present) 가능한 한 빨리 화면에 그릴 수 있는 drawable을 표시한다. **Required.**
* \*\*\*\*[`func present(afterMinimumDuration: CFTimeInterval)`](https://developer.apple.com/documentation/metal/mtldrawable/2806859-present) 지정된 지속 시간 동안 이전 그리기가 표시되면 가능한 빨리 drawable 화면을 표시한다. **Required.**
* \*\*\*\*[`func present(at: CFTimeInterval)`](https://developer.apple.com/documentation/metal/mtldrawable/1470282-present) 특정 호스트 시간에 drawable 화면을 표시한다. **Required.**

#### Getting Presentation Information

* [`func addPresentedHandler(MTLDrawablePresentedHandler)`](https://developer.apple.com/documentation/metal/mtldrawable/2806858-addpresentedhandler) drawable이 표시된 직후 호출할 코드 블럭을 등록한다. **Required.**
* \*\*\*\*[`typealias MTLDrawablePresentedHandler`](https://developer.apple.com/documentation/metal/mtldrawablepresentedhandler) drawable 이후 호출된 코드 블록이 표시된다.
* [`var presentedTime: CFTimeInterval`](https://developer.apple.com/documentation/metal/mtldrawable/2806855-presentedtime) drawable이 화면에 표시된 호스트 시간 \(초\). **Required.**

