# Tuning Your OpenGL ES App

iOS에서 OpenGL ES 앱의 성능은 OS X나 다른 데스크탑 운영 체제의 OpenGL과 다르다. 비록 강력한 컴퓨팅 장치들이지만, iOS 기반 장치들은 데스크탑이나 노트북 컴퓨터가 가지고 있는 메로리나 CPU 파워를 가지고 있지 않다. 임베디드 GPU는 일반적인 데스크탑 또는 노트북 GPU와 다른 알고리즘을 사용하여 낮은 메모리 및 전력 사용량에 최적화되어 있다. 그래픽 데이터를 비효율적으로 렌더링하면 프레임률이 저하되거나 iOS 기반 장치의 배터리 수명을 획기적으로 줄일 수 있다.

이후 장에서는 앱의 성능을 향상시키기 위한 많은 기법을 설명한다. 이 장에서는 전반적인 전략을 다룬다. 라벨에 부착되지 않는 한, 이 장의 조언은 OpenGL ES의 모든 버전에 관련된다.

### Debug and Profile Your App with Xcode and Instruments

다양한 장치에서 다양한 시나리오에서 앱 성능을 테스트하기 전에는 앱을 최적화하지 마라. Xcode와 Instruments에는 앱에서 성능과 정확성 문제를 식별하는 데 도움이 되는 도구가 포함되어 있다.

* _Xcode 디버그 게이지_를 모니터링하여 일반적인 성능 개요를 확인하라. 이러한 게이지는 Xcode에서 앱을 실행할 때마다 표시되므로 앱을 개발하는 동안 성능 변화를 쉽게 발견할 수 있다.
* 런타임 성능에 대한 자세한 이해를 위해 Instruments의 OpenGL ES 분석 및 OpenGL ES 드라이버 도구를 사용하라. 앱의 리소스 사용 및 OpenGL ES 모범 사례에 대한 자세한 정보를 얻고 그래픽 파이프라인의 일부를 선택적으로 비활성화하여 앱에서 중요한 병목 현상이 되는 부분을 결정할 수 있다. 자세한 내용은 [_Instruments User Guide_](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/index.html#//apple_ref/doc/uid/TP40004652)를 참조하라.
* 성능 문제 해결 및 렌더링 문제를 정확하게 해결하려면 Xcode의 OpenGL ES Frame Debugger 및 Performance Analyzer 도구를 사용하라. 단일 프레임을 렌더링하고 표시하는 데 사용되는 모든 OpenGL ES 명령을 캡처한 다음 해당 명령을 통해 OpenGL ES 상태, 바인딩된 리소스 및 출력 프레임 버퍼에 대한 각 명령의 효과를 확인하라. 또한 셰이더 소스 코드를 보고, 편집하며, 변경사항이 렌더링된 이미지에 어떤 영향을 미치는 지 볼 수 있다. OpenGL ES 3.0 지원 기기의 경우 ,프레임 디버거는 도한 어떤 그리기 호출과 셰이더 명령이 렌더링 시간에 가장 큰 기여를 하는지 표시한다. 이러한 도구에 대한 자세한 내용은 [Xcode OpenGL ES Tools Overview](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/ToolsOverview/ToolsOverview.html#//apple_ref/doc/uid/TP40008793-A2-SW7)를 참조하라.

#### Watch for OpenGL ES Errors in Xcode and Instruments

OpenGL ES 오류는 앱이 OpenGL ES API를 잘못 사용할 때 발생한다. \(예를 들어, 기본 하드웨어가 수행할 수 없는 작업을 요청하여\). 콘텐츠가 올바르게 렌더링되더라도 이러한 오류는 성능 문제를 나타낼 수 있다. OpenGL ES 오류를 확인하는 기존 방법은 `glGetError` 함수를 호출하는 것이다. 그러나 이 함수를 반복적으로 호출하면 성능이 현저히 저하될 수 있다. 대신 위에 설명된 도구를 사용하려 오류를 테스트하라:

* Instruments에서 앱을 프로파일링할 때 OpenGL ES Analyzer 도구의 세부 정보 창을 참조하여 녹화하는 동안 보고된 OpenGL ES 오류를 확인하라.
* Xcode에서 앱을 디버깅하는 동안 프레임을 캡처하여 앱을 만드는 데 사용되는 그리기 명령과 해당 명령을 수행하는 동안 발생하는 오류를 검사하라.

또한 OpenGL ES 오류가 발생할 때 프로그램 실행을 중지하도록 Xcode를 구성할 수도 있다. \(자세한 내용은 OpenGL ES Error Breakpoint를 참조하라.\)

#### Annotate Your OpenGL ES Code for Informative Debugging and Profiling

OpenGL ES 명령을 논리 그룹으로 구성하고 OpenGL ES 객체에 의미 있는 레이블을 추가함으로써 디버깅과 프로파일링을 더욱 효율적으로 만들 수 있다. 이러한 그룹과 라벨은 Figure 7-1과 같이 Xcode의 OpenGL ES Frame Debugger와 Instruments의 OpenGL ES Analyzer에 나타난다. 그룹 및 레이블을 추가하려면 [EXT\_debug\_marker](http://www.khronos.org/registry/gles/extensions/EXT/EXT_debug_marker.txt) 및 [EXT\_debug\_label](http://www.khronos.org/registry/gles/extensions/EXT/EXT_debug_label.txt) 익스텐션을 사용하라.

**Figure 7-1** Xcode Frame Debugger before and after adding debug marker groups

![](../.gitbook/assets/gputrace-before-after_2x.png)

게임 캐릭터 그리기와 같이 하나의 의미 있는 작업을 나타내는 일련의 그리기 명령이 있을 경우, 마커를 사용하여 디버깅을 위해 그룹을 만들 수 있다. Figure 7-1은 장면의 단일 요소에 대한 텍스처, 프로그램, 버텍스 배열 및 그리기 호출을 그룹화하는 방법을 보여준다. 먼저 `glPushGroupMarkerEXT` 함수를 불러 의미 있는 이름을 제공한 다음 명령 그룹을 발행한다. 마지막으로 `glPopGroupMarkerEXT` 함수에 대한 호출로 그룹을 닫는다.

**Listing 7-1**  Using a debug marker to annotate drawing commands

```objectivec
glPushGroupMarkerEXT(0, "Draw Spaceship");
glBindTexture(GL_TEXTURE_2D, _spaceshipTexture);
glUseProgram(_diffuseShading);
glBindVertexArrayOES(_spaceshipMesh);
glDrawElements(GL_TRIANGLE_STRIP, 256, GL_UNSIGNED_SHORT, 0);
glPopGroupMarkerEXT();
```

여러 개의 내포된 마커를 사용하여 복잡한 장면에서 의미 있는 그룹의 계층을 만들 수 있다. [`GLKView`](https://developer.apple.com/documentation/glkit/glkview) 클래스를 사용하여 OpenGL ES 콘텐츠를 그리면 그리기 메서드의 모든 명령이 포함된 "렌더링" 그룹이 자동으로 생성된다. 작성한 모든 마커는 이 그룹 내에 중첩되어 있다.

라벨은 텍스처, 셰이더 프로그램, 정점 배열 객체 등 OpenGL ES 객체에 의미 있는 이름을 제공한다. glLabelObjectEXT 함수를 호출하여 디버깅 및 프로파일링시 표시할 객체 이름을 지정하라. Listing 7-2는 이 함수를 사용해 정점 배열 객체에 레이블을 지정할 수 있다. [`GLKTextureLoader`](https://developer.apple.com/documentation/glkit/glktextureloader) 클래스를 사용하여 텍스처 데이터를 로드하면 파일 이름으로 작성한 OpenGL ES 텍스처 객체에 자동으로 레이블을 지정한다.

**Listing 7-2**  Using a debug label to annotate an OpenGL ES object

```objectivec
glGenVertexArraysOES(1, &_spaceshipMesh);
glBindVertexArrayOES(_spaceshipMesh);
glLabelObjectEXT(GL_VERTEX_ARRAY_OBJECT_EXT, _spaceshipMesh, 0, "Spaceship");
```

### General Performance Recommendations

상식적으로 성능 튜닝 작업을 안내하라. 예를 들어, 앱이 프레임당 몇 십 개의 삼각형만 그리는 경우, 정점 데이터를 제출하는 방법을 변경해도 성능이 향상될 것 같지 않다. 귀사의 노력에 가장 큰 성능 향상을 제공하는 최적화를 찾아봐라.

#### Redraw Scenes Only When the Scene Data Changes

당신의 앱은 새로운 프레임을 렌더링하기 전에 장면의 무언가가 바뀔때 때까지 기다려야 한다. Core Animation은 사용자가 제시된 마지막 이미지를 캐시하고 새로운 프레임이 표시될 때까지 계속 표시한다.

데이터가 변경되더라도 하드웨어 프로세스 명령의 속도로 프레임을 렌더링할 필요는 없다. 속도가 느리지만 고정된 프레임률이 빠르지만 가변 프레임률보다 사용자에게 더 부드럽게 나타나는 경우가 많다. 초당 30프레임의 고정 프레임률은 대부분의 애니메이션에 충분하며 전력 소비량을 줄이는 데 도움이 된다.

#### Disable Unused OpenGL ES Features

가장 좋은 계산은 당신의 앱이 절대 수행하지 않는 계산이다. 예를 들어, 결과를 미리 계산하여 모델 데이터에 저장할 수 있는 경우 런타임에 해당 계산을 수행하지 않아도 된다.

앱이 OpenGL ES 2.0 이상에 작성된 경우 앱이 장면을 렌더링하는 데 필요한 모든 작업을 수행하는 많은 스위치와 조건부로 단일 셰이더를 만들지 마라. 대신, 각각 특정 집중 작업을 수행하는 여러 셰이더 프로그램을 컴파일한다.

앱이 OpenGL ES 1.1을 사용하는 경우 장면을 렌더링하는 데 필요하지 않은 고정 기능 작업을 모두 비활성화하라. 예를 들어, 앱은 조명이나 블렌딩이 필요하지 않은 경우 해당 기능을 비활성화하라. 마찬가지로, 앱이 2D 모델만 그릴 경우 안개와 깊이 테스트를 비활성화해야 한다.

### Minimize the Number of Drawing Commands

당신의 앱이 OpenGL ES에 의해 처리될 원시 요소를 제출할 때마다 CPU는 그래픽 하드웨어에 대한 명령을 준비한다. 앱에서 장면을 렌더링하기 위해 `glDrawArray` 또는 `glDrawElements` 호출을 많이 사용하는 경우 GPU를 완전히 사용하지 않고 CPU 자원에 의해 성능이 제한될 수 있다.

이러한 오버헤드를 줄이려면 렌더링을 더 적은 수의 그리기 호출로 통합하는 방법을 찾아봐라. 유용한 전략에는 다음이 포함된다:

* 삼각형 스트립과 배치 정점 데이터 사용에 설명된 대로 여러 원시 요소를 단일 삼각형 스트립에 병합하라. 최상의 결과를 얻으려면 가까운 공간에 그려진 원시 요소를 통합하라. 대형 모델의 경우 프레임에 표시되지 않을 경우 앱이 효율적으로 도태하기가 더 어렵다.
* [Combine Textures into Texture Atlases](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/TechniquesForWorkingWithTextureData/TechniquesForWorkingWithTextureData.html#//apple_ref/doc/uid/TP40008793-CH104-SW2)에 설명된 대로, 동일한 텍스처 이미지의 다른 부분을 사용하여 여러 프리미티브를 그리기 위해 텍스처 아틀라스를 작성하라.
* 아래에 설명된 것처럼 많은 유사 객체를 렌더링하기 위해 분할된 그리기를 사용하라.

#### Use Instanced Drawing to Minimize Draw Calls

분할된 그리기 명령을 사용하면 단일 그리기 호출을 사용하여 동일한 꼭지점 데이터를 여러 번 그릴 수 있다. CPU 시간을 사용하여 위치 오프셋, 변환 매트릭스, 색상 또는 텍스처 좌표와 같은 메쉬의 여러 인스턴스 간의 그리기를 설정하고 각 인스턴스에 대한 그리기 명령을 실행하는 대신, 인스턴스 그리기의 처리를 GPU에서 실행 중인 셰이더 코드로 이동시킨다.

여러 번 재사용되는 정점 데이터는 분할 그리기에 가장 적합한 대상이다. 예를 들어, Listing 7-3의 코드는 한 장면 내의 여러 위치에 객체를 그린다. `glUniform`과 `glDrawArray`의 많은 호출은 CPU 오버헤드를 추가하여 성능을 감소시킨다.

**Listing 7-3**  Drawing many similar objects without instancing

```objectivec
for (x = 0; x < 10; x++) {
    for (y = 0; y < 10; y++) {
        glUniform4fv(uniformPositionOffset, 1, positionOffsets[x][y]);
        glDrawArrays(GL_TRIANGLES, 0, numVertices);
    }
}
```

분할된 그리기를 채택하려면 두 가지 단계가 필요하다: 첫째, 위와 같은 루프를 `glDrawArraysInstanced` 또는 `glDrawElementsInstanced` 대한 단일 호출로 교체하라. 이러한 호출과 달리 glDrawArray 또는 glDrawElements와 동일하지만 그릴 인스턴스 수를 나타내는 추가 파라미터가 있다 \(Listing 7-3의 예시 100\). 두 번째, OpenGL ES가 제공하는 두 가지 전략 중 하나를 선택하여 구현하여 정점 세이더에서 인스턴스별 정보를 사용할 수 있도록 한다.

셰이더 인스턴스 ID 전략을 사용하여 버텍스 셰이더는 인스턴스당 정보를 얻거나 검색한다. 버텍스 셰이더가 실행될 때마다 gl\_InstanceID 내장 변수는 현재 그려지고 있는 인스턴스를 식별하는 숫자를 포함하고 있다. 셰이더 코드에서 위치 오프셋, 색상 또는 기타 인스턴스당 변화를 계산하거나, 균일한 배열 또는 기타 대량 저장소에서 인스턴스당 정보를 검색하려면 이 숫자를 사용하라. 예를 들어, Listing 7-4는 10x10 그리드에 위치한 메쉬의 100개 인스턴스를 그리기 위해 이 기술을 사용한다.

**Listing 7-4** OpenGL ES 3.0 vertex shader using `gl_InstanceID` to compute per-instance information

```text
#version 300 es
 
in vec4 position;
 
uniform mat4 modelViewProjectionMatrix;
 
void main()
{
    float xOffset = float(gl_InstanceID % 10) * 0.5 - 2.5;
    float yOffset = float(gl_InstanceID / 10) * 0.5 - 2.5;
    vec4 offset = vec4(xOffset, yOffset, 0, 0);
 
    gl_Position = modelViewProjectionMatrix * (position + offset);
}
```

초기화된 배열 전략을 사용하여 인스턴스당 정보를 정점 배열 속성에 저장하라. 그러면 정점 셰이더가 해당 속성에 접근하여 인스턴스별 정보를 사용할 수 있다. OpenGL ES가 각 인스턴스를 그릴 때 해당 속성이 어떻게 진전되는지 지정하려면 `glVertexAttribDivisor` 함수를 호출하라. Listing 7-5는 분할 그리기를 위한 정점 배열을 설정하는 것을 보여주고, Listing 7-6은 해당 셰이더를 보여준다.

**Listing 7-5**  Using a vertex attribute for per-instance information

```text
#define kMyInstanceDataAttrib 5
 
glGenBuffers(1, &_instBuffer);
glBindBuffer(GL_ARRAY_BUFFER, _instBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(instData), instData, GL_STATIC_DRAW);
glEnableVertexAttribArray(kMyInstanceDataAttrib);
glVertexAttribPointer(kMyInstanceDataAttrib, 2, GL_FLOAT, GL_FALSE, 0, 0);
glVertexAttribDivisor(kMyInstanceDataAttrib, 1);
```

**Listing 7-6**  OpenGL ES 3.0 vertex shader using instanced arrays

```text
#version 300 es
 
layout(location = 0) in vec4 position;
layout(location = 5) in vec2 inOffset;
 
uniform mat4 modelViewProjectionMatrix;
 
void main()
{
    vec4 offset = vec4(inOffset, 0.0, 0.0)
    gl_Position = modelViewProjectionMatrix * (position + offset);
}
```

인스턴스 그리기는 core OpenGL ES 3.0 API와 OpenGL ES 2.0에서 [EXT\_draw\_instanced](http://www.khronos.org/registry/gles/extensions/EXT/draw_instanced.txt) 및 [EXT\_instanced\_arrays](http://www.khronos.org/registry/gles/extensions/EXT/EXT_instanced_arrays.txt) 익스텐션을 통해 사용할 수 있다.

