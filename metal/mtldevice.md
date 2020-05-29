---
description: 그래픽을 그리거나 병렬 컴퓨팅을 하는 데 사용하는 GPU에 대한 Metal 인터페이스.
---

# MTLDevice

### Declaration

```swift
protocol MTLDevice
```

### Overview

`MTLDevice` 프로토콜은 GPU에 대한 인터페이스를 정의한다. `MTLDevice`에 Metal 앱을 제공하는 고유한 기능을 쿼리하고 `MTLDevice`를 사용하여 모든 Metal 명령을 실행하라. 이 프로토콜을 직접 구현하지 마라. 대신 iOS 및 tvOS에서 [`MTLCreateSystemDefaultDevice()`](https://developer.apple.com/documentation/metal/1433401-mtlcreatesystemdefaultdevice)를 사용하여 런타임에 시스템에 GPU를 요청하고, macOS에서 [`MTLCopyAllDevicesWithObserver(handler:)`](https://developer.apple.com/documentation/metal/2928189-mtlcopyalldeviceswithobserver)를 사용하여 사용 가능한 객체 목록을 가져와라. 올바른 GPU 선택에 대한 자세한 내용은 _Getting the Default_를 참조하라.

`MTLDevice` 객체는 Metal에서 모든 작업을 수행하는데 필요한 객체로, 앱이 상호 작용하는 모든 `MTLDevice` 객체는 런타임에 획득한 `MTLDevice` 인스턴스로부터 온다. `MTLDevice`에서 만든 객체는 비싸지만 지속적이며, 많은 객체는 한 번 초기화하고 앱의 수명 동안 재사용하도록 설계되어 있다. 그러나 이러한 객체는 이를 발행한 `MTLDevice`에 한정된다. 여러 `MTLDevice` 인스턴스를 사용하거나 한 `MTLDevice`에서 다른 `MTLDevice`로 전환하려면 각 `MTLDevice`에 대해 별도의 객체 집합을 생성해야 한다.

