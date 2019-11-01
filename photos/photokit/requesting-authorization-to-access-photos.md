# Requesting Authorization to Access Photos

사용자의 사진 라이브러리에 접근할 때 사용 권한을 요청할 수 있도록 앱을 준비하라.

## Overview

사용자는 앱이 사진에 접근할 수 있는 권한을 명시적으로 부여해야 한다. 정당성 문자열을 제공하여 이 요구 사항에 맞는 앱을 준비하라. 정당성 문자열은 앱의 Info.plist 파일에 추가하여 앱이 사용자의 사진 라이브러리에 접근해야 하는 이유를 알려주는 지역적 메시지이다. 그런 다음 사진에서 사용자에게 접근 권한을 부여하라는 메시지가 표시되면 경보에 사용자가 제공한 정당성 문자열이 사용자 장치에서 선택한 지역의 언어로 표시된다.

> **Note**
>
> UIImagePickerController를 사용하여 사용자의 사진 라이브러리를 불러올 때 앱이 명시적으로 권한을 요청할 필요는 없다.

응용 프로그램에서 PHAsset, PHCollection, PHAssetCollection, PHCollectionList 메서드를 사용하여 라이브러리에서 컨텐츠를 가져올 때 또는 사진 라이브러리에 Applying Changes to the Photo Library에 나열된 메서드 중 하나를 사용하여 라이브러리 컨텐츠 변경을 요청하거나 Photos는 자동으로 그리고 비동기적으로 사용자에게 승인 요청 메시지를 표시한다.

사용자가 권한을 부여한 후, 시스템은 앱에서 향후 사용할 수 있는 선택사항을 기억하지만, 사용자는 Settings 앱을 사용하여 언제든지 이 선택사항을 변경할 수 있다. 사용자가 앱 사진 라이브러리 접근을 거부하거나, 권한 프롬프트에 아직 응답하지 않았거나, 제한으로 인해 접근 권한을 부여할 수 없는 경우, 사진 라이브러리 컨텐츠를 가져오려고 하면 빈 PHFetchResult 객체가 반환되고, 사진 라이브러리를 변경하려는 시도가 실패하게 된다. 이 메서드가 `PHAuthorizationStatus.notDetermined` 를 반환하면 사용자에게 사진 라이브러리 접근 권한을 묻는 메서드 `requestAuthorization(_:)` 를 호출할 수 있다.

## Update Your App's Info.plist File

PHAsset, PHPhotoLibrary, PHImageManager 와 같은 사진 라이브러리와 상호 작용하는 클래스를 사용하려면, 앱의 Info.plist 파일에 사용자에게 접근 권한을 요청할 때 시스템이 표시하는 NSPhotoLibraryUsageDescription 키에 대한 사용자 대면 텍스트가 포함되어야 한다. iOS 10 이후의 앱에서 이 키가 존재하지 않으면 충돌된다.

**Figure 1** NSPhotoLibraryUsageDescription 키와 관련된 값은 사용자에게 접근 권한 메시지가 보여지는 문자열이다.

![nsphotolibrary\_usage\_description](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/nsphotolibrary_usage_description.png)

## Observe Changes Before Fetching

컨텐츠를 가져오기전에 사진 라이브러리 변경을 관찰하려면 `register(_:)` 메서드를 사용하라. 사용자가 사진 라이브러리에 앱에 접근 권한을 부여한 후, Photos는 빈 가져오기 결과에 대한 변경 메시지를 보낸다. 이러한 가져오기에 대한 라이브러리 컨텐츠를 지금 사용할 수 있음을 알려준다.

## 출처

### Apple documentation

[**PhotoKit - Requesting Authorization to Access Photos**](https://developer.apple.com/documentation/photokit/requesting_authorization_to_access_photos)

### 부스트 코스

[**4. 내 사진 관리 애플리케이션 - 1\) Photos 프레임워크**](https://www.edwith.org/boostcourse-ios/lecture/16867/)

