# PHFetchResult

Photos 가져오기 메서드로 리턴되는 순서 있는 에셋 혹은 컬렉션 리스트

## Declaration

```swift
class PHFetchResult<ObjectType> : NSObject where ObjectType : AnyObject
```

## Overview

`PHAsset`, `PHCollection`, `PHAssetCollection`, `PHCollectionList` 클래스에서 클래스 메서드를 사용하여 객체를 검색할 때, Photos는 결과 객체를 가져오기 결과로 제공한다. NSArray 클래스에서 사용하는 것과 동일한 메서드 및 규칙을 사용하여 가져오기 결과의 내용에 접근하라. `NSArray` 객체와는 달리 `PHFetchResult` 객체는 필요에 따라 Photos 라이브러리에서 동적으로 컨텐츠를 로딩하여 다수의 결과를 처리할 때에도 최적의 성능을 제공한다.

가져오기 결과는 그 내용에 대한 스레드를 안전하게 접근할 수 있게 해준다. 가져오기 후 가져오기의 결과의 `count` 값은 일정하며 가져오기 결과의 모든 객체는 동일한 `localIdentifier` 값을 유지한다. \(가져오기 내용을 업데이트하려면 공유 PHPhotoLibrary 객체에 변경 관찰자를 등록하라.\)

가져오기 결과는 콘텐츠를 캐시하여 가장 최근에 접근한 인덱스 주위에 객체 배치를 유지한다. 배치 외부에 있는 객체는 더 이상 캐시되지 않기 때문에 이러한 객체에 접근하면 해당 객체가 다시 설정된다. 이 프로세스는 해당 객체에서 이전에 읽은 값에 대한 변경을 초래할 수 있다.

