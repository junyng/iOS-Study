# PHCollection

사진 에셋 컬렉션 및 컬렉션 목록의 추상적 슈퍼 클래스

## Declaration

```swift
class PHCollection : PHObject
```

## Overview

이 클래스의 인스턴스를 직접 만들거나 작업하지 마라. 대신 `PHAssetCollection` 또는 `PHCollectionList` 라는 두 개의 구체적인 하위 클래스 중 하나를 사용하라.

* `PHAssetCollection` 객체는 앨범, 모먼트 또는 공유 사진 스트림과 같은 사진 또는 비디오 에셋의 집합을 나타낸다.
* `PHCollectionList` 객체는 앨범이 들어 있는 폴더나 달력 연도의 모든 모먼트 집합과 같은 다른 컬렉션을 포함하는 컬렉션을 표현한다.

> **Important**
>
> 사진 라이브러리에 접근하거나 수정하려면 사용자의 명시적 권한이 필요하다. Fetching Collections 에 나열된 메서드 중 하나를 처음 호출할 때, Photos는 자동으로 사용자에게 승인을 요청한다.\(대체로 원하는 시간에 사용자에게 메시지를 표시하는 방법으로 `PHPhotoLibrary requestAuthorization(_:)` 메서드를 사용할 수 있다.
>
> 앱의 Info.plist 파일은 `NSPhotoLibraryUsageDescription` 키에 대한 값을 제공해야 한다. 이 키는 앱이 사진 접근을 요청하는 이유를 사용자에게 설명하는 것이다. iOS 10.0 이후에 연결된 앱이 이 키가 없는 경우 충돌할 것이다.

