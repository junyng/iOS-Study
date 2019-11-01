# PHFetchOptions

에셋 또는 컬렉션 객체를 가져올 때 Photos가 반환하는 결과의 필터링, 정렬 및 관리에 영향을 주는 옵션 세트

## Declaration

```swift
class PHFetchOptions : NSObject
```

## Overview

에셋 또는 컬렉션을 가져오기 위해 `PHAsset`, `PHCollection`, `PHAssetCollection`, `PHCollectionList` 클래스의 클래스 메서드를 사용하면 요청된 객체가 포함된 `PHFetchResult` 객체가 생성된다. 지정한 옵션은 가져오기 결과가 포함된 객체, 가져오기 결과에서 해당 객체가 배열되는 방법 및 사진에서 가져오기 결과의 변경 사항을 앱에 통지하는 방법을 제어한다.

Photos는 `predicate` 및 `sortDescriptors` 프로퍼티에 대한 제한된 키 집합을 지원한다. 사용가능한 키 세트는 에셋 또는 컬렉션을 가져오는데 사용하는 클래스에 따라 달라진다. 각 클래스에서 지원하는 키 목록은 Table 1을 참조하라.

**Table 1** Supported predicate and sort descriptor keys

| Class for Fetch Method | Supported Keys |
| :--- | :--- |
| `PHAsset` | SELF, `localIdentifier`, `creationDate`, `modificationDate`, `mediaType`, `mediaSubtypes`, `duration`, `pixel Width`, `pixelHeight`, `isFavorite`\(or isFavorite\), `isHidden`\(or isHidden\), `burstIdentifier` |
| `PHAssetCollection` | SELF, `localIdentifier`, `localizedTitle`\(or title\), `startDate`, `endDate`, `estimatedAssetCount` |
| `PHCollectionList` | SELF, `localIdentifier`, `localizedTitle`\(or title\), `startDate`, `endDate` |
| `PHCollection` \(can fetch a mix of PHCollectionList and PHAsset Collection objects\) | SELF, `localIdentifier`, `localizedTitle`\(or title\), `startDate`, `endDate` |

