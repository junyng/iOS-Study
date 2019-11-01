# PHAssetCollection

moment 앨범, 사용자가 만든 앨범 또는 스마트 앨범과 같은 사진 에셋 그룹 표현

## Declaration

```swift
class PHAssetCollection : PHCollection
```

## Overview

Photo 프레임워크에서는 컬렉션 객체\(에셋 컬렉션 포함\)가 구성원 객체를 직접 참조하지 않으며, 컬렉션 객체를 직접 참조하는 다른 객체는 없다. 에셋 컬렉션의 구성원을 검색하려면 `fetchAssets(in:options:)` 와 같은 PHAsset 클래스 메서드로 이들을 가져와라. 에셋 컬렉션을 찾으려면 Fetching Asset Collections에 나열된 메서드 중 하나를 사용하라.

> **Important**
>
> 사진 라이브러리에 접근하거나 수정하려면 사용자의 명시적 권한이 필요하다. Fetching Asset Collections에 나열된 메서드 중 하나를 처음 호출할 때, Photos는 자동으로 사용자에게 권한을 표시한다. \(대체로 유저가 선택한 시간을 보여주기 위해 `PHPhotoLibrary request Authorization(_:)` 메서드를 사용할 수 있다.\)
>
> 앱의 Info.plist 파일은 `NSPhotoLibraryUsageDescription` 키에 대한 값을 제공해야 한다. 이 키는 앱이 사진 접근을 요청하는 이유를 사용자에게 설명하는 것이다. 이 키가 없으면 iOS 10.0 이상의 앱에서 키가 존재하지 않다고 충돌이 발생한다.

에셋이나 컬렉션 목록처럼 에셋 컬렉션은 불변이다. 에셋 컬렉션을 생성, 이름 변경 또는 삭제하거나 에셋 컬렉션에서 구성원을 추가, 제거 또는 다시 정렬하려면 사진 라이브러리 변경 블록 내에 `PHAssetCollectionChangeRequest` 객체를 생성하라. 변경 요청 및 변경 블록을 사용하여 사진 라이브러리를 업데이트하는 방법에 대한 자세한 내용은 PHPhotoLibrary를 참고하라.

