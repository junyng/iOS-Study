# Glossary

* **attachment**  비디오 프레임과 연결된 Core Foundation 객체. key-value 쌍에 의해 지정된 attachment는 타임스탬프와 같은 프레임과 관련된 모든 종류의 정보를 보유할 수 있다.
* **buffer pool**  반복해서 사용할 수 있는 사전 할당된 버퍼 모음이다. 버퍼 풀을 유지하는 것은 필요할 때마다 버퍼를 할당하고 할당하는 것보다 오버헤드가 적다.
* **display link**  지정된 하드웨어 디스플레이에 기반하여 디스플레이의 새로 고침 속도와 동기화하기 위해 프레임을 출력해야 하는 빈도를 지능적으로 추측하기 쉬운 높은 우선순위 쓰레드.
* **image buffer**  Core Video 이미지를 보관하는 추상 버퍼 타입. 픽셀 버퍼, Core Video OpenGL 버퍼 및 OpenGL 텍스처는 `CVImageBuffer` 타입에서 파생된다.
* **OpenGL buffer**  그래픽 카드 메모리에 이미지 정보를 보관하는 버퍼. Core Video에서 표준 OpenGL 버퍼 타입을 래퍼로 묶는 CVOpenGLBufferRef 타입을 사용하여 OpenGL 버퍼를 조작하라.
* **OpenGL texture**  OpenGL이 원시 요소를 래핑하는 데 사용하는 불변의 이미지. Core Video에서 표준 OpenGL 텍스처 타입 주위에 래퍼인 `CVOpenGLTextureRef` 타입을 사용하여 OpenGL 텍스처를 조작하라.
* **pixel buffer**  메인 메모리에 이미지 정보를 저장하는 버퍼.
* **pool**  [buffer pool](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreVideo/CVProg_Gloss/CVProg_Gloss.html#//apple_ref/doc/uid/TP40001536-CH204-CIHDJEIE) 참조.
* **texture**   [OpenGL texture](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreVideo/CVProg_Gloss/CVProg_Gloss.html#//apple_ref/doc/uid/TP40001536-CH204-CIHCJHJD) 참조.
* **texture cache**  OpenGL 텍스처의 풀
* **visual context** 도면이 발생할 위치를 나타내는 추상적 공간. 예를 들어, OpenGL 컨텍스트는 OpenGL 도면이 발생할 위치를 지정한다. 시각적 컨텍스트는 일반적으로 `NSView` 또는 `HIView` 객체와 연관된다.

