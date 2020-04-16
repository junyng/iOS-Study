# Core Video Programming Guide

> **Important:** 이 문서는 더 이상 업데이트되지 않는다. Apple SDK에 대한 최신 정보는 [documentation website](https://developer.apple.com/documentation)를 참조하라.

이 문서는 Core Video 개념을 설명하고 Core Video 프로그래밍 인터페이스를 사용하여 비디오 프레임을 얻고 조작하는 방법을 설명한다.

### What Is Core Video?

Core Video는 OS X에서 디지털 비디오를 위한 새로운 파이프라인 모델이다. 프로세싱을 별도의 단계로 분할하면 개발자가 데이터 유형 \(QuickTime, OpenGL 등\)을 변환하거나 동기화 문제를 표시하지 않고도 개별 프레임에 접근하고 조작할 수 있다.

Core Video는 Core Image 및 Core Audio 기술과 비교할만 하다.

Core Video는 다음에서 사용할 수 있다:

* OS X v10.4 이상
* OS X v10.3, QuickTime 7.0 이상이 설치된 경우

최상의 결과를 얻으려면 하드웨어 그래픽 가속\(즉, Quartz Extreme\)을 지원하는 컴퓨터에서만 Core Video 기능을 사용해야 한다.

### Who Should Read This Document?

이 문서의 독자는 비디오 이미지를 조작하는 데 있어 더 큰 수준의 제어를 원하는 Carbon 또는 Cocoa 개발자이다. 개발자는 디지털 비디오와 OpenGL 뿐만 아니라 멀티쓰레드 프로그래밍에도 익숙해야 한다.

Core Video는 개별 비디오 프레임을 조작하려면 필수적이다. 예를 들어, 다음 유형의 비디오 처리에는 Core Video가 필요하다:

* Core Image 필터에서 제공하는 색 보정 또는 기타 필터링.
* 비디오 이미지의 물리적 변환 \(예를 들어, 뒤틀림 또는 표면 매핑\)
* OpenGL 씬에 비디오 추가
* 프레임에 가시적 시간 코드와 같은 추가 정보 추가
* 다중 비디오 스트림을 합성하기

이 정도의 정교함이 필요하지 않다면, \(예를 들어, 애플리케이션에 동영상만 표시하려면\) 당신은 동영상을 표시하기 위해 HIMovieView \(Carbon\) 또는 QTKit \(Cocoa\)과 같은 단순화된 비디오 플레이어를 사용해야 한다.

### Organization of This Document

본 문서는 다음 장으로 구성되어 있다:

* [Core Video Concepts](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreVideo/CVProg_Concepts/CVProg_Concepts.html#//apple_ref/doc/uid/TP40001536-CH202-BABJDFHJ)는Core Video 파이프라인 모델을 설명하고 Core Video API를 사용하기 위해 필요한 핵심 개념을 설명한다.
* [Core Video Tasks](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreVideo/CVProg_Tasks/CVProg_Tasks.html#//apple_ref/doc/uid/TP40001536-CH203-CIHCGHGG)는 Core Video를 사용하여 개별 비디오 프레임을 얻고 조작하는 방법을 보여준다.

### See Also

Apple은 _Core Video Programming Guide_를 보완하는 ADC Reference 라이브러리에 다음과 같은 추가 리소스를 제공한다.

* _Core Video Reference_는 Core Video API에 대한 자세한 설명을 제공한다.
* [_OpenGL Programming Guide for Mac_](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/OpenGL-MacProgGuide/opengl_intro/opengl_intro.html#//apple_ref/doc/uid/TP40001987)는 GL 텍스처 및 CGL 그래픽 컨텍스트에 대한 정보를 제공한다.
* Cocoa OpenGL에는 Cocoa에서 사용할 수 있는 OpenGL 클래스에 대한 정보가 포함되어 있다. \(예: NSOepnGLView\)
* [_Core Image Programming Guide_](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_intro/ci_intro.html#//apple_ref/doc/uid/TP30001185)에는 비디오 프레임에 적용할 수 있는 Core Image filters 생성 방법에 대한 정보가 포함되어 있다.

또한 OpenGL 웹사이트\([http://www.opengl.org](http://www.opengl.org)\)는 OpenGL API에 대한 정보를 제공하는 주요 소스다.

 













