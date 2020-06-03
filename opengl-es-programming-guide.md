# OpenGL ES Programming Guide

> **중요** OpenGL ES는 iOS 12에서 deprecated되었다. GPU에서 고성능 코드를 만드려면 Metal 프레임워크를 대신 사용하라.

_Open Graphics Library\(Open Graphics Library\)_는 2D 및 3D 데이터를 시각화하는 데 사용된다. 2D 및 3D 디지털 콘텐츠 제작, 기계 및 건축 디자인, 가상 프로토타이핑, 비행 시뮬레이션, 비디오 게임 등을 위한 애플리케이션을 지원하는 다목적 개방형 표준 그래픽 라이브러리다. OpenGL을 사용하여 3D 그래픽 파이프라인을 구성하고 데이터를 제출하라. 정점들은 변환되고 켜지며 원시 형상으로 조립되고 래스터화 되어 2D 이미지를 만든다. OpenGL은 함수 호출을 기본 그래픽 하드웨어로 전송할 수 있는 그래픽 명령으로 변환하도록 설계되었다. 이 기본 하드웨어는 그래픽 명령 처리 전용이기 때문에, OpenGL 도면은 일반적으로 매우 빠르다.

_OpenGL for Embedded Systems\(OpenGL ES\)_는 중복 기능을 제거하여 모바일 그래픽 하드웨어에서 배우기 쉽고 구현하기 쉬운 라이브러리를 제공하는 OpenGL의 단순화된 버전이다.

![](.gitbook/assets/cpu_gpu_2x.png)

### At a Glance

OpenGL ES는 앱이 기본 그래픽 프로세서의 파워를 활용할 수 있도록 한다. iOS 기기의 GPU는 정교한 2D 및 3D 그리기 수행할 수 있으며, 최종 영상의 모든 픽셀에 대해 복잡한 셰이딩 계산을 수행할 수 있다. 앱의 설계 요구사항이 GPU 하드웨어에 대한 가장 직접적이고 포괄적인 액세스를 요구하는 경우 OpenGL ES를 사용하라. OpenGL ES의 일반적인 사용자에게는 3D 그래픽을 제공하는 비디오 게임과 시뮬레이션이 포함된다.

OpenGL ES는 하드웨어 중심의 저 수준의 API이다. 가장 강력하고 유연한 그래픽 처리 도구를 제공하지만 가파른 학습 곡선과 앱의 전반적인 디자인에 상당한 영향을 미친다. 보다 전문화된 용도로 고성능 그래픽이 필요한 앱의 경우 iOS는 몇 가지 고급 프레임워크를 제공한다.

* Sprite Kit 프레임워크는 2D 게임 제작에 최적화된 하드웨어 가속 애니메이션 시스템을 제공한다.
* Core Image는 스틸 및 비디오 이미지를 실시간으로 필터링 및 분석한다.
* Core Animation 모든 iOS 앱에 하드웨어 가속 그래픽 렌더링 및 애니메이션 인프라를 제공하고 정교한 사용자 인터페이스 애니메이션 구현을 간단하게 하는 간단한 선언 프로그래밍 모델을 제공한다.
* UIKit 프레임워크의 기능을 사용하여 Cocoa Touch 사용자 인터페이스에 애니메이션, 물리 기반 dynamics 및 기타 특수 효과를 추가할 수 있다.

#### OpenGL ES Is a Platform-Neutral API Implemented in iOS

OpenGL ES는 C 기반 API이기 때문에 휴대성이 뛰어나고 널리 지원되고 있다. C API로서 Objective-C Coaco Touch 앱과 매끄럽게 통합된다. OpenGL ES 규격은 윈도우 설정 레이어를 정의하지 않는다. 대신, 호스팅 운영 체제는 명령을 수용하는 OpenGL ES 렌더링 컨텍스트와 도면 명령의 결과가 기록되는 프레임 버퍼를 만드는 기능을 제공해야 한다. iOS에서 OpenGL ES로 작업하려면 iOS 클래스를 사용하여 도면 표면을 설정 및 표시하고 플랫폼 중립 API를 사용하여 내용을 렌더링해야 한다.

#### GLKit Provides a Drawing Surface and Animation Support

UIKit 프레임워크에 의해 정의된 뷰 및 뷰 컨트롤러는 iOS에 대한 시각적 컨텐츠의 표시를 제어한다. GLKit 프레임워크는 이러한 클래스의 OpenGL ES-aware 버전을 제공한다. OpenGL ES 앱을 개발할 때 GLKView 개체를 사용하여 OpenGL ES 콘텐츠를 렌더링하라. 또한 GLKViewController 객체를 사용하여 뷰를 관리하고 컨텐츠 애니메이션을 지원할 수 있다.

#### iOS Supports Alternative Rendering Targets

전체 화면 또는 뷰 계층의 일부를 채우기 위해 콘텐츠를 그리는 것 외에도 다른 렌더링 전략에 OpenGL ES 프레임버퍼 객체를 사용할 수도 있다. iOS는 OpenGL ES 프레임 버퍼 객체를 구현하며, 이는 OpenGL ES 씬의 다른 곳에서 사용하기 위해 오프스크린 버퍼 또는 텍스처로 렌더링하는 데 사용할 수 있다. 또한 iOS의 OpenGL ES는 Core Animation 레이어\(CAEAGLLayer 클래스\)로 렌더링을 지원하며, 이 레이어는 다른 레이어와 결합하여 앱의 사용자 인터페이스 또는 기타 시각적 디스플레이를 구축할 수 있다.

#### OpenGL ES May Not Be Used in Background Apps

백그라운드에서 실행 중인 앱은 OpenGL ES 기능을 호출하지 않을 수 있다. 앱이 백그라운드에 있는 동안 그래픽 프로세서에 액세스하면 iOS에 의해 자동으로 종료된다. 이를 방지하려면 앱이 백그라운드로 이동하기 전에 이전에 OpenGL ES에 제출된 보류 중인 명령을 플러시하고 다시 포그라운드로 이동할 때까지 OpenGL ES를 호출하지 마라.

#### OpenGL ES Places Additional Restrictions on Multithreaded Apps

동시성을 이용하도록 앱을 설계하는 것은 앱의 성능을 향상시키는 데 유용할 수 있다. OpenGL ES 앱에 동시성을 추가하려면 두 개의 다른 스레드에서 동시에 동일한 컨텍스트에 액세스하지 않도록 하라.

### How to Use This Document

[Checklist for Building OpenGL ES Apps for iOS](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/OpenGLESontheiPhone/OpenGLESontheiPhone.html#//apple_ref/doc/uid/TP40008793-CH101-SW1), [Configuring OpenGL ES Contexts](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/WorkingwithOpenGLESContexts/WorkingwithOpenGLESContexts.html#//apple_ref/doc/uid/TP40008793-CH2-SW1), [Drawing with OpenGL ES and GLKit](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/DrawingWithOpenGLES/DrawingWithOpenGLES.html#//apple_ref/doc/uid/TP40008793-CH503-SW1)를 읽는 것으로 시작하라. 이 장에서는 OpenGL ES를 iOS에 통합하는 방법과 iOS 기기에서 첫 번째 OpenGL ES 앱을 시작하고 실행하는 데 필요한 모든 세부 정보를 간략히 설명한다.

iOS에서 OpenGL ES를 사용하는 기본 사항에 대해 잘 알고 있다면 중요한 플랫폼별 가이드에는 [Drawing to Other Rendering Destinations](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/WorkingwithEAGLContexts/WorkingwithEAGLContexts.html#//apple_ref/doc/uid/TP40008793-CH103-SW1), [Multitasking, High Resolution, and Other iOS Features](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/ImplementingaMultitasking-awareOpenGLESApplication/ImplementingaMultitasking-awareOpenGLESApplication.html#//apple_ref/doc/uid/TP40008793-CH5-SW1)를 읽어봐라. 5.0 이전의 iOS에서 OpenGL ES를 사용하는 데 익숙한 개발자는 [Drawing with OpenGL ES and GLKit](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/DrawingWithOpenGLES/DrawingWithOpenGLES.html#//apple_ref/doc/uid/TP40008793-CH503-SW1)를 참고하여 OpenGL ES 개발의 효율화를 위한 새로운 기능에 대해 참조하라.

마지막으로 [OpenGL ES Design Guidelines](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/OpenGLESApplicationDesign/OpenGLESApplicationDesign.html#//apple_ref/doc/uid/TP40008793-CH6-SW1), [Tuning Your OpenGL ES App](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/Performance/Performance.html#//apple_ref/doc/uid/TP40008793-CH105-SW1) 읽어 더 깊게 OpenGL ES 앱을 효율적으로 설계하라.

