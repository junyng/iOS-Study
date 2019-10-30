# Loading and Caching Assets and Thumbnails

빠른 재사용을 위해 이미지, 비디오, 라이브 사진 컨텐츠 요청 및 캐시 요청

## Overview

Photos는 자동으로 이미지를 다운로드하거나 규격에 맞게 이미지를 생성하고, 빠르게 재사용할 수 있도록 캐싱한다. `PHImageManager` 클래스를 사용하여 지정된 크기의 에셋 이미지를 요청하거나 비디오 에셋과 함께 작동하도록 `AVFoundation` 객체를 요청하라. 많은 수의 에셋으로 작업할 때 예를 들어, 콜렉션 뷰를 썸네일 이미지로 채울 때 `PHCachingImageManager` 하위 클래스를 사용하여 이미지를 일괄 로드하라.

