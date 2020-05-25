# Image I/O Programming Guide

Image I/O 프로그래밍 인터페이스는 애플리케이션이 대부분의 이미지 파일 형식을 읽고 쓸 수 있도록 한다. 원래 Core Graphics 프레임워크의 일부였던 Image I/O는 개발자가 Core Graphics \(Quartz 2D\) 프레임워크와 독립적으로 사용할 수 있도록 자체 프레임워크에 상주한다. Image I/O는 효율성이 뛰어나고 메타데이터에 쉽게 접근할 수 있으며 색 관리 기능을 제공하기 때문에 이미지 데이터에 접근할 수 있는 결정적인 방법을 제공한다. Image I/O 인터페이스는 OS X v10.4 이상 및 iOS 4 이상에서 사용할 수 있다.

### Who Should Read This Document?

이 문서는 애플리케이션에서 이미지 데이터를 읽거나 쓰는 개발자를 위한 것이다. 현재 이미지 가져오기 기능 또는 기타 이미지 처리 라이브러리를 사용하는 개발자는 이 문서를 읽고 I/O 프레임워크를 대신 사용하는 방법을 확인하라.

### Organization of This Document

이 문서는 다음 장으로 구성된다:

* [Basics of Using Image I/O](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/ImageIOGuide/imageio_basics/ikpg_basics.html#//apple_ref/doc/uid/TP40005462-CH216-TPXREF101)는 지원되는 이미지 형식을 논의하고 Xcode 프로젝트에 프레임워크를 포함시키는 방법을 보여준다.
* [Creating and Using Image Sources](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/ImageIOGuide/imageio_source/ikpg_source.html#//apple_ref/doc/uid/TP40005462-CH218-SW3)은 이미지 소스 생성, 이미지 생성 및 사용자 인터페이스에 표시할 속성 추출 방법을 보여준다.
* [Working with Image Destinations](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/ImageIOGuide/ikpg_dest/ikpg_dest.html#//apple_ref/doc/uid/TP40005462-CH219-SW3)는 이미지 대상 생성, 속성 설정 및 이미지 추가에 대한 정보를 제공한다.

### See Also

[_Image I/O Reference Collection_](https://developer.apple.com/documentation/imageio)는 Image I/O 프레임워크의 함수, 데이터 타입 및 상수에 대한 자세한 설명을 제공한다.

