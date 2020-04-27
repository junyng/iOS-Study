---
description: '개별 프레임의 조작을 포함한 디지털 비디오 처리(파이프라인 기반 API 사용, Metal 및 OpenGL 지원)'
---

# Core Video

### 개요

코어 비디오는 디지털 비디오를 위한 파이프라인 모델을 제공한다. 이는 프로세스를 별도의 단계로 분할하여 비디오 작업을 단순화한다. 이를 통해 개발자는 데이터 타입 간 변환이나 디스플레이 동기화 문제를 걱정할 필요 없이 개발 프레임에 쉽게 접근하고 조작할 수 있다. 개별 비디오 프레임을 조작할 필요가 없는 앱은 Core Video를 직접 사용할 필요가 없다.

### Topics

#### Data Processing

* [CVBuffer](https://developer.apple.com/documentation/corevideo/cvbuffer-nfm) 데이터 버퍼와 상호 작용하는 방법을 정의하는 추상 기본 클래스이다.
* [CVImageBuffer](https://developer.apple.com/documentation/corevideo/cvimagebuffer-q40) 서로 다른 유형의 이미지 데이터를 관리하기 위한 인터페이스이다.
* [CVPixelBuffer](https://developer.apple.com/documentation/corevideo/cvpixelbuffer-q2e) 메인 메모리에 픽셀을 저장하는 이미지 버퍼.
* [CVPixelBufferPool](https://developer.apple.com/documentation/corevideo/cvpixelbufferpool-77o) 재사용 가능한 픽셀 버퍼 객체 세트를 관리하기 위한 유틸리티 객체.
* [CVPixelFormatDescription](https://developer.apple.com/documentation/corevideo/cvpixelformatdescription) 사용자 정의 픽셀 형식을 정의하기 위한 함수 및 타입을 제공하는 API이다.

#### Time Management

* [CVTime](https://developer.apple.com/documentation/corevideo/cvtime-q1e) Core Video 시간 값을 저장하는 데 사용되는 구조체.
* [CVDisplayLink](https://developer.apple.com/documentation/corevideo/cvdisplaylink-k0k) 지정된 디스플레이가 각 프레임을 필요로 할 때 앱에 알리는 높은 우선순위 쓰레드.

**Metal**

* [CVMetalTextureCache](https://developer.apple.com/documentation/corevideo/cvmetaltexturecache-q3j) Metal 텍스처 객체를 만들고 관리하는 데 사용되는 캐시.
* [CVMetalTexture](https://developer.apple.com/documentation/corevideo/cvmetaltexture-q3g) Metal 프레임워크와 함께 사용하기 위한 소스 이미지 데이터를 제공하는 텍스처 기반 이미지 버퍼.

#### OpenGL

* [CVOpenGLTextureCache](https://developer.apple.com/documentation/corevideo/cvopengltexturecache-780) OpenGL 텍스처 객체를 만들고 관리하는 데 사용되는 캐시.
* [CVOpenGLTexture](https://developer.apple.com/documentation/corevideo/cvopengltexture-782) 소스 이미지 데이터를 OpenGL에 공급하는 텍스처 기반 이미지 버퍼.
* [CVOpenGLBuffer](https://developer.apple.com/documentation/corevideo/cvopenglbuffer-77s) 영상 데이터를 비디오 메모리에 저장하는 데 사용되는 이미지 버퍼.
* [CVOpenGLBufferPool](https://developer.apple.com/documentation/corevideo/cvopenglbufferpool-77j) 재사용 가능한 OpenGL 버퍼 객체 집합을 관리하기 위한 유틸리티 객체.

#### OpenGL ES

* [CVOpenGLESTextureCache](https://developer.apple.com/documentation/corevideo/cvopenglestexturecache-q2r) OpenGL ES 텍스처 객체를 만들고 관리하는 데 사용되는 캐시.
* [CVOpenGLESTexture](https://developer.apple.com/documentation/corevideo/cvopenglestexture-q2s) OpenGL ES에 소스 이미지 데이터를 제공하는 텍스처 기반 이미지 버퍼.

#### Core Video Error Constants

* [Result Codes](https://developer.apple.com/documentation/corevideo/1572713-result_codes) Core Video 작업에 의해 생성된 결과 코드를 기술한다.
* [Data Types](https://developer.apple.com/documentation/corevideo/data_types) Core Video 프레임워크에서 사용하는 일반적인 데이터 유형.

#### Reference

* [Core Video Constants](https://developer.apple.com/documentation/corevideo/core_video_constants)

