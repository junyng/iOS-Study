# Graphics Rendering: Render Command Encoder

### Displaying Rendered Content with Core Animation

Core Animation은 Metal을 사용하여 콘텐츠를 렌더링하는 레이어 백 뷰의 전문화된 동작을 위해 설계된 [`CAMetalLayer`](https://developer.apple.com/documentation/quartzcore/cametallayer)를 정의한다. CAMetalLayer 객체는 콘텐츠의 기하학적 구조 \(위치 및 크기\), 시각적 속성\(배경색, 테두리, 그림자\) 및 Metal이 콘텐츠를 색상 attachment로 표시하는 데 사용하는 자원에 대한 정보를 나타낸다. 또한 콘텐츠가 가능한 즉시 또는 지정된 시간에 콘텐츠가 표시될 수 있도록 콘텐츠 나타나는 시간을 캡슐화한다. Core Animation에 대한 자세한 내용은 [_Core Animation Programming Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004514)을 참조하라.

Core Animation은 또한 디스플레이 가능한 자원인 [`CAMetalDrawable`](https://developer.apple.com/documentation/quartzcore/cametaldrawable) 프로토콜을 정의한다. `CAMetalDrawable` 프로토콜은 `MTLDrawable`을 확장하고 [`MTLTexture`](https://developer.apple.com/documentation/metal/mtltexture) 프로토콜에 부합하는 객체를 제공하므로 커맨드 렌더링의 대상으로 사용할 수 있다. `CAMetalLayer` 객체로 렌더링하려면 각 렌더링 패스마다 새로운 `CAMetalDrawable` 객체를 가져와 이 객체가 제공하는 `MTLTexture` 객체를 가져와 해당 텍스처를 사용하여 색상 attachment를 생성하라. 색상 attachment와 달리 깊이 또는 스텐실 attachment의 생성 및 파괴는 비용이 많이 든다. 깊이 또는 스텐실 attachment가 필요한 경우 한 번 만든 다음 프레임이 렌더링될 때마다 다시 사용한다.

