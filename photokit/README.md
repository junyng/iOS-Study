# PhotoKit

iCloud Photos 및 Live Photos를 포함하여 사진 앱에서 관리하는 이미지 및 비디오 에셋으로 작업하라.

## Overview

iOS와 macOS에서 PhotoKit은 포토 앱의 사진 편집 확장 구축을 지원하는 클래스를 제공한다. iOS와 tvOS에서도 PhotoKit은 포토 앱이 관리하는 포토, 비디오 자산에 직접 접근할 수 있다.

![photokit](../.gitbook/assets/photokit.png)

PhotoKit를 사용하여 디스플레이 및 재생, 이미지 및 비디오 콘텐츠 편집, 앨범, 모멘트, 공유 앨범 등의 자산 컬렉션을 관리할 수 있다.

## Topics

### Shared Photo Library

* Browsing and Modifying Photo Albums

  사용자가 PhotoKit을 사용하여 사진을 앨범을 구성하고 그리드 기반 레이아웃에서 사진 컬렉션을 찾아보도록 지원하라.

* Requesting Authorization to Access Photos

  사용자의 사진 라이브러리에 접근할 때 사용 권한을 요청할 수 있도록 앱을 준비하라.

* class PHPhotoLibrary

  사용자의 공유 사진 라이브러리에 대한 접근 및 변경을 관리하는 공유 객체

### Asset Retrieval

* Fetching Objects and Requesting Changes

  지정된 쿼리와 일치하는 에셋, 에셋 모음 및 수집 목록을 가져와라.

* class PHAsset

  사진 라이브러리에 있는 이미지, 비디오 또는 라이브 사진 표현하는 클래스

* class PHAssetCollection

  moment와 같이 사용자가 만든 앨범 또는 스마트 앨범과 같은 사진 에셋 그룹 표현

* class PHCollection

  사진 에셋 콜렉션 및 컬렉션 목록의 추상 슈퍼 클래스

* class PHCollectionList

  Moment, Years 또는 사용자가 만든 앨범의 폴더와 같은 사진 에셋 컬렉션을 포함하는 그룹

* class PHObject

  포토 모델 객체\(에셋 및 컬렉션\)에 대한 추상적인 슈퍼 클래스

* class PHFetchResult

  Photos fetch 메서드에서 반환된 순서를 갖는 에셋 리스트 또는 컬렉션 목록

* class PHFetchOptions

  에셋 또는 컬렉션 객체를 가져올 때 사진이 반환하는 결과의 필터링, 정렬 및 관리에 영향을 주는 옵션 세트

### Asset Loading

* Loading and Caching Assets and Thumbnails

  빠른 재사용을 위해 이미지, 비디오 또는 라이브 사진 컨텐츠 및 캐시 요청

* class PHImageManager

  미리 보기 썸네일과 에셋 데이터를 생성하거나 검색을 용이하게 하는 객체

* class PHCachingImageManager

  대량의 에셋을 사전 로드하는 배치에 최적화된 미리보기 썸네일을 생성하거나 검색을 용이하게하는 객체

* class PHImageRequestOptions

  이미지 관리자에게 요청한 Photos 에셋의 스틸 이미지 표현에 영향을 주는 옵션 세트

* class PHVideoRequestOptions

  이미지 관리자에게 요청하는 비디오 에셋 데이터 전송에 영향을 주는 옵션 세트

* class PHLivePhotoRequestOptions

  이미지 관리자에게 요청한 Live Photo 에셋 전송에 영향을 주는 옵션 세트

### Live Photos

* Displaying Live Photos

  iOS Photos 앱과 동일한 대화형 Live Photos 재생 제공

* class PHLivePhotoView

  Live Photo를 표시하는 뷰 - 포착 직전과 이후의 순간에서 나오는 움직임과 소리 또한 포함하는 사진

* class PHLivePhoto

  표시가능한 라이브 사진 표현 객체

### Asset Resource Management

* class PHAssetResource

  포토 라이브러리의 사진, 비디오 또는 라이브 사진 에셋과 관련된 기본 데이터 리소스

* class PHAssetCreationRequest

  포토 라이브러리 변경 블록에서 사용할 수 있도록 기본 데이터 리소스에서 새 사진 에셋을 생성하라는 요청

* class PHAssetResourceCreationOptions

  기본 리소스에서 새 사진 에셋 생성에 영향을 미치는 옵션 세트

* class PHAssetResourceManager

  사진 에셋의 기반이 되는 데이터 스토리지의 리소스 관리자

* class PHAssetResourceRequestOptions

  에셋 리소스 관리자에서 요청하는 기본 에셋 데이터 전송에 영향을 주는 옵션 세트

### Photo Editing Extensions

* Creating Photo Editing Extensions

  앱이 사진 앱 내에서 직접 에셋을 편집하도록 허용하라.

* protocol PHContentEditingController

  커스텀 컨트롤러 클래스가 구현하는 프로토콜로 확장 Photos 유저 인터페이스를 제공하라.

### Classes

* class PHChangeRequest

### Protocols

* protocol PHPhotoLibraryAvailabilityObserver

### Reference

* PhotoKit Constants

## 출처

### Apple documentation

[**PhotoKit**](https://developer.apple.com/documentation/photokit)

### 부스트 코스

[**4. 내 사진 관리 애플리케이션 - 1\) Photos 프레임워크**](https://www.edwith.org/boostcourse-ios/lecture/16867/)

