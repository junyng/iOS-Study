# Metal Programming Guide

## About Metal and This Guide <a id="pageTitle"></a>

Metal 프레임워크는 GPU 가속 고급 3D 그래픽 렌더링 및 데이터 병렬 컴퓨팅 워크로드를 지원한다. Metal은 조직, 그래픽 및 계산 명령의 처리, 제출 및 이러한 명령들에 대한 관련 데이터와 리소스의 관리를 미세하고 저수준의 제어를 위한 현대적이고 능률적인 API를 제공한다. Metal의 1차 목표는 GPU 워크로드를 실행함으로써 발생하는 CPU 오버헤드를 최소화하는 것이다.

### At a Glance

본 문서는 Metal의 기본 개념인 명령 제출 모델, 그래픽 셰이더 및 데이터 병렬 계산 기능에 대해 독립적으로 컴파일된 코드 사용 등을 설명한다. 그런 다음 이 문서에는 Metal API를 사용하여 앱을 작성하는 방법이 자세히 설명되어 있다.

자세한 내용은 다음 장을 참조하라:

* [Fundamental Metal Concepts](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Device/Device.html#//apple_ref/doc/uid/TP40014221-CH2-SW1)은 Metal의 주요 특징을 간략하게 설명한다. 
* [Command Organization and Execution Model](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Cmd-Submiss/Cmd-Submiss.html#//apple_ref/doc/uid/TP40014221-CH3-SW1)는 실행을 위해 GPU에 명령을 만들고 제출하는 방법을 설명한다.
* [Resource Objects: Buffers and Textures](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Mem-Obj/Mem-Obj.html#//apple_ref/doc/uid/TP40014221-CH4-SW1)는 GPU 메모리 할당을 나타내는 버퍼 및 텍스처 객체를 포함한 장치 메모리 관리에 대해 논의한다.
* [Functions and Libraries](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Prog-Func/Prog-Func.html#//apple_ref/doc/uid/TP40014221-CH5-SW1)는 Metal 앱에서 셰이딩 언어 코드를 표시하는 방법을 설명한다. 그리고 GPU에 의해 Metal 셰이딩 언어 코드가 로드되고 실행되는 방법을 설명한다.
* [Graphics Rendering: Render Command Encoder](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Render-Ctx/Render-Ctx.html#//apple_ref/doc/uid/TP40014221-CH7-SW1)는 멀티 쓰레드에 걸쳐 그래픽 작업을 배포하는 방법을 포함하여 렌더 3D 그래픽 렌더링 방법을 설명한다.
* [Data-Parallel Compute Processing: Compute Command Encoder](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Compute-Ctx/Compute-Ctx.html#//apple_ref/doc/uid/TP40014221-CH6-SW1)는 데이터 병렬 처리를 수행하는 방법을 설명한다.
* [Buffer and Texture Operations: Blit Command Encoder](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Blit-Ctx/Blit-Ctx.html#//apple_ref/doc/uid/TP40014221-CH9-SW3)는 텍스처와 버퍼 간에 데이터를 복사하는 방법을 설명한다.
* [Metal Tools](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Dev-Technique/Dev-Technique.html#//apple_ref/doc/uid/TP40014221-CH8-SW1)는 개발 워크플로우를 사용자 정의하고 개선하는 데 도움이 되는 도구를 나열한다.
* [Metal Feature Set Tables](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/MetalFeatureSetTables/MetalFeatureSetTables.html#//apple_ref/doc/uid/TP40014221-CH13-SW1)에는 각 Metal 기능 세트의 기능 사용성, 구현 한계 및 픽셀 형식 기능이 나열되어 있다.
* [What's New in iOS 9 and OS X 10.11](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/WhatsNewiniOS9andOSX1011/WhatsNewiniOS9andOSX1011.html#//apple_ref/doc/uid/TP40014221-CH12-SW11)의 새로운 기능에는 iOS 9와 OS X 10.11에 도입된 새로운 기능이 요약되어 있다.
* [What’s New in iOS 10, tvOS 10, and OS X 10.12](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/WhatsNewiniOS10tvOS10andOSX1012/WhatsNewiniOS10tvOS10andOSX1012.html#//apple_ref/doc/uid/TP40014221-CH14-SW1)는 iOS 10, tvOS 10, OS X 10.12에 도입된 새로운 기능을 요약한다.
* [Tessellation](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Tessellation/Tessellation.html#//apple_ref/doc/uid/TP40014221-CH15-SW1)은 컴퓨팅 커널, 테셀레이터 및 테셀레이션 후 정점 함수의 사용을 포함하여 패치를 테셀레이션하는 데 사용되는 Metal 테셀레이션 파이프라인을 설명한다.
* [Resource Heaps](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/ResourceHeaps/ResourceHeaps.html#//apple_ref/doc/uid/TP40014221-CH16-SW1)은 힙으로 부터 자원, 그 사이의 별칭에서 자원을 하위 할당하고 펜스로 추적하는 방법을 설명한다.

### Prerequisites

OpenGL, OpenCL 또는 이와 유사한 API로 프로그래밍하는 데 익숙하고 Objective-C 언어에 익숙해야 한다.

### See Also

[_Metal Framework Reference_](https://developer.apple.com/documentation/metal)은 Metal 프레임워크의 인터페이스를 설명하는 문서 모음이다.

[Metal Shading Language Specification](https://developer.apple.com/metal/Metal-Shading-Language-Specification.pdf)은 메탈 앱에서 사용하는 그래픽 셰이더나 컴퓨팅 기능을 쓸 때 사용하는 메탈 셰이딩 언어를 지정하는 문서이다.

또한 애플 개발자 라이브러리에서 Metal을 이용한 여러 샘플 코드 프로젝트를 이용할 수 있다.

