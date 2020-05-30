# Fundamental Metal Concepts

Metal은 그래픽 및 데이터 병렬 계산 작업량에 대한 단일 통일 프로그래밍 인터페이스 및 언어를 제공한다. Metal은 별도의 API와 셰이더 언어를 사용하지 않고도 그래픽과 계산 작업을 훨씬 더 효율적으로 통합할 수 있게 해준다.

Metal 프레임워크는 다음과 같이 제공한다:

* **낮은 오버헤드 인터페이스.** Metal은 암묵적 상태 검증과 같은 "hidden" 성능 병목 현상을 제거하도록 설계되었다. 명령 버퍼를 병렬로 만들고 커밋하는 데 사용되는 효율적인 멀티쓰레딩을 위하 GPU의 비동기 동작을 제어할 수 있다. Metal 명령 제출에 대한 자세한 내용은 [Command Organization and Execution Model](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Cmd-Submiss/Cmd-Submiss.html#//apple_ref/doc/uid/TP40014221-CH3-SW1)를 참조하라.
* **메모리 및 자원 관리.** Metal 프레임워크는 GPU 메모리 할당을 나타내는 버퍼 및 텍스처 객체를 설명한다. 텍스처 객체는 특정한 픽셀 형식을 가지고 있으며 텍스처 이미지나 첨부파일에 사용될 수 있다. Metal 메모리 객체에 대한 자세한 내용은 [Resource Objects: Buffers and Textures](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Mem-Obj/Mem-Obj.html#//apple_ref/doc/uid/TP40014221-CH4-SW1)를 참조하라.
* **그래픽 및 컴퓨팅 작업에 대한 통합 지원.** Metal은 그래픽 및 컴퓨팅 작업 모두에 동일한 데이터 구조체 및 자원\(버퍼, 텍스처 및 명령 큐 등\)을 사용한다. 또한 Metal 셰이딩 언어는 그래픽과 컴퓨팅 기능을 모두 지원한다. Metal 프레임워크는 자원을 런타임 인터페이스, 그래픽 셰이더 및 계산 기능 간에 공유할 수 있게 한다. 그래픽 렌더링 또는 데이터 병렬 컴퓨팅 작업에 Metal을 사용하는 앱 작성에 대한 자세한 내용은 [Graphics Rendering: Render Command Encoder](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Render-Ctx/Render-Ctx.html#//apple_ref/doc/uid/TP40014221-CH7-SW1) 또는 [Data-Parallel Compute Processing: Compute Command Encoder](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Compute-Ctx/Compute-Ctx.html#//apple_ref/doc/uid/TP40014221-CH6-SW1)를 참조하라.
* **미리 컴파일된 셰이더.** Metal 셰이더는 빌드 시간에 앱 코드와 함께 컴파일된 후 런타임에 로드할 수 있다. 이 워크플로우는 셰이더 코드의 디버깅을 용이하게 할 뿐만 아니라 더 나은 코드 생성을 제공한다. \(Metal은 셰이더 코드의 런타임 컴파일도 지원한다.\) Metal 프레임워크 코드의 Metal 셰이더 작업에 대한 자세한 내용은 [Functions and Libraries](https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Prog-Func/Prog-Func.html#//apple_ref/doc/uid/TP40014221-CH5-SW1)를 참조하라. Metal 셰이딩 언어 자체에 대한 자세한 내용은 _Metal Shading Language Guide_를 참조하라.

Metal 앱은 백그라운드에서 Metal 명령을 실행할 수 없고, 이를 시도하는 Metal 앱은 종료된다.

