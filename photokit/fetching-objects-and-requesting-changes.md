# Fetching Objects and Requesting Changes

에셋, 에셋 컬렉션, 지정된 쿼리와 일치하는 컬렉션 목록을 가져온다.

## Overview

모델 클래스 PHAsset, PHAssetCollection, PHCollectionList의 인스턴스는 사용자가 사진 앱에서 작업하는 항목을 나타낸다.

* Assets: 이미지, 비디오, 라이브 포토
* Collections of assets: 앨범 또는 모멘트
* Lists of collections: 앨범 폴더 또는 모멘트 클러스터

이러한 클래스의 인스턴스는 읽기 전용이며 변경할 수 없으며 메타데이터만 포함되어 있다. 이 클래스를 사용하여 지정된 쿼리와 일치하는 객체 집합을 가져와라.

## Commit Change Requests to the Shared Photo Library

관심 있는 에셋 및 컬렉션을 가져와서 해당 객체를 사용하여 참조하거나 편집하는데 필요한 원시데이터를 가져와라. 변경을 만들기 위해서 변경 요청 객체를 생성하고 공유 PHPhotoLibrary 객체에 명시적으로 커밋하라. 이 아키텍처를 통해 여러 쓰레드 또는 여러 애플리케이션 및 애플리케이션 확장 기능을 통해 동일한 에셋으로 쉽고 안전하고 효율적으로 작업이 가능하다.

## Handle Special Assets

PhotoKit은 사용자의 사진 라이브러리와 직접 작업할 수 있도록 많은 사진 앱 기능을 지원한다. 예를 들어, 라이브 사진은 비디오 또는 프레임 시퀀스로서 앱이 다르게 표시할 수 있는 추가 데이터를 가진 특수한 에셋이다.

PHCollectionList 클래스를 사용하여 사진 앱에서 모멘트 계층에 해당하는 에셋을 찾아라. 컬렉션 목록은 읽기 전용이고 불변하며 메타데이터만 포함하고 있다. PHAsset 클래스를 사용하여 burst 사진, 라이브 사진, 파노라믹 사진 및 고프레임 비디오를 식별할 수 있다. iCloud Photos가 활성화된 경우, 포토 프레임워크의 에셋과 컬렉션은 동일한 iCloud 계정의 모든 장치에서 사용할 수 있는 콘텐츠를 반영한다.

라이브 사진에 대한 자세한 내용은 Displaying Live Photos를 참고하라

