# PHObject

Photos 모델 객체\(에셋 및 컬렉션\)에 대한 추상 슈퍼 클래스

## Declaration

```swift
class PHObject : NSObject
```

## Overview

이 클래스의 인스턴스를 직접 만들거나 사용하지 않는다. 대신에 구체적인 하위 클래스 인스턴스와 함께 작업한다. 예를 들어 `PHAsset`,`PHAssetCollection`, `PHCollectionList`, `PHObject Placeholder`

PHObject 클래스는 `localIdentifier` 프로퍼티 측면에서 `isEqual(_:)` 및 `hash` 메서드를 구현하기 때문에, 이러한 방법에 의존하는 기술을 사용하여 에셋과 컬렉션 객체를 추적할 수 있다.

