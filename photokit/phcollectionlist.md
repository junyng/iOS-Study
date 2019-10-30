# PHCollectionList

Moments, Years 또는 사용자가 만든 앨범의 폴더와 같은 사진 에셋 컬렉션을 포함하는 그룹

## Declaration

```swift
class PHCollectionList : PHCollection
```

## Overview

Photos 프레임워크에서는 컬렉션 객체\(에셋, 컬렉션을 포함\)를 직접 참조하지 않으며, 컬렉션 객체를 직접 참조하는 다른 객체는 없다. 컬렉션 목록의 구성원을 검색하려면 `fetchCollections(in:options:)` 와 같은 `PHCollection` 클래스 메서드로 가져와라. 컬렉션 목록 계층 구조\(상위 폴더가 없는 앨범 폴더\)의 루트에서 객체를 찾으려면 `fetchTopLevelUserCollections(with:)` 메서드를 사용하라.

> **Important**
>
> 사진 라이브러리에 접근하거나 수정하려면 사용자의 명시적 권한이 필요하다. Fetching Collections 에 나열된 메서드 중 하나를 처음 호출할 때, Photos는 자동으로 사용자에게 승인을 요청한다.\(대체로 원하는 시간에 사용자에게 메시지를 표시하는 방법으로 `PHPhotoLibrary requestAuthorization(_:)` 메서드를 사용할 수 있다.
>
> 앱의 Info.plist 파일은 `NSPhotoLibraryUsageDescription` 키에 대한 값을 제공해야 한다. 이 키는 앱이 사진 접근을 요청하는 이유를 사용자에게 설명하는 것이다. iOS 10.0 이후에 연결된 앱이 이 키가 없는 경우 충돌할 것이다.

에셋이나 에셋 컬렉션처럼, 컬렉션 목록은 불변이다. 컬렉션 목록을 생성, 이름 변경 또는 삭제하거나 컬렉션 목록에서 구성원을 추가, 제거 또는 다시 정렬하려면 사진 라이브러리 변경 블록 내에 `PHCollectionListChangeRequest` 객체를 생성하라. 변경 요청 및 변경 블록을 사용하여 사진 라이브러리를 업데이트하는 방법에 대한 자세한 내용은 `PHPhotoLibrary` 를 참고하라.

