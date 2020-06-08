# Multitasking, High Resolution, and Other iOS Features

OpenGL ES와 함께 작업하는 많은 측면은 플랫폼 중립적이지만, iOS에서 OpenGL ES와 작업하는 일부 세부 사항은 특별히 고려되어야 한다. 특히 OpenGL ES를 사용하는 iOS 앱은 멀티태스킹을 올바르게 처리하거나 백그라운드로 이동할 때 종료될 위험이 있다. iOS 기기용 OpenGL ES 콘텐츠를 개발할 때는 디스플레이 해상도 및 기타 기기 기능도 고려해야 한다.

### Implementing a Multitasking-Aware OpenGL ES App

사용자가 다른 앱으로 전환해도 앱은 계속 실행될 수 있다. iOS에서 멀티태스킹에 대한 전반적인 설명은 [App States and Multitasking](https://developer.apple.com/library/archive/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html#//apple_ref/doc/uid/TP40007072-CH4)을 참조하라.

OpenGL ES 앱은 백그라운드로 이동할 때 추가 작업을 수행해야 한다. 만약 앱이 이러한 작업을 부적절하게 처리한다면, 대신 iOS에 의해 종료될 수 있다. 또한 앱은 포그라운드 앱에서 사용할 수 있도록 OpenGL ES 자원을 확보하고자 할 수 있다.

#### Background Apps May Not Execute Commands on the Graphics Hardware

OpenGL ES 앱은 그래픽 하드웨어에서 OpenGL ES 명령을 실행하려고 하면 종료된다. iOS는 백그라운드 앱이 그래픽 프로세서에 접근하는 것을 방지하여 맨 앞부분 앱이 항상 사용자에게 좋은 경험을 제공할 수 있도록 한다. 앱은 백그라운드에서 OpenGL ES 호출을 할 뿐만 아니라 백그라운드에서 이전에 제출한 명령이 GPU로 플러시되는 경우에도 종료할 수 있다. 앱은 백그라운드로 이동하기 전에 이전에 제출된 모든 명령의 실행을 완료했는지 확인해야 한다.

GLK 뷰와 뷰 컨트롤러를 사용하고 그리기 메서드 중 OpenGL ES 명령만 제출하면 앱이 백그라운드로 이동할 때 자동으로 올바르게 동작한다. [`GLKViewController`](https://developer.apple.com/documentation/glkit/glkviewcontroller) 클래스는 기본적으로 앱이 비활성화되면 애니메이션 타이머를 일시 중지하여 그리기 메서드를 호출하지 않도록 보장한다.

GLKit 뷰 또는 뷰 컨트롤러를 사용하지 않거나 [`GLKView`](https://developer.apple.com/documentation/glkit/glkview) 그리기 메서드 외부에 OpenGL ES 명령을 제출하는 경우, 앱이 백그라운드에서 종료되지 않도록 다음 단계를 수행하라:

1. 앱 델리게이트의 [`applicationWillResignActive:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622950-applicationwillresignactive) 메서드에서 앱은 애니메이션 타이머\(있는 경우\)를 중지하고 알려진 양호한 상태로 전환한 다음 `glFinish` 함수를 호출해야 한다.
2. 앱 델리게이트의 메서드에서 앱은 일부 OpenGL ES 객체를 삭제하여 포그라운드 앱에서 메모리와 리소스를 사용할 수 있도록 할 수 있다. `glFinish` 함수를 사용하여 자원을 즉시 제거하라.
3. 앱이 애플리케이션을 종료한 후 [`applicationDidEnterBackground:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622997-applicationdidenterbackground) 메서드에서 OpenGL ES 호출을 새로 하지 마라. OpenGL ES 호출을 할 경우 iOS에 의해 종료된다.
4. [`applicationWillEnterForeground:`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623076-applicationwillenterforeground) 메서드에서 객체를 다시 생성하고 애니메이션 타이머를 다시 시작하라.

요약하자면, 당신의 앱은 이전에 제출한 모든 명령이 명령 버퍼에서 빠지고 OpenGL ES에 의해 실행되도록 `glFinish` 함수를 호출해야 한다. 앱이 백그라운드로 이동한 후에는 다시 포그라운드로 이동할 때까지 OpenGL ES의 모든 사용을 피해야 한다.

#### Delete Easily Re-Created Resources Before Moving to the Background

앱이 백그라운드로 이동할 때 OpenGL ES 객체를 해제할 필요가 없다. 일반적으로, 당신의 앱이 그것의 콘텐츠를 폐기하는 것을 피해야 한다. 다음 두 가지 시나리오를 고려하라.

* 사용자가 게임을 하고 있으며 달력을 확인하기 위해 게임을 잠시 종료한다. 플레이어가 당신의 게임에 돌아오면, 게임의 자원을 여전히 메모리에 있고 게임을 즉시 재개될 수 있다.
* 사용자가 다른 OpenGL ES 앱을 실행할 때 사용자의 OpenGL ES 앱이 백그라운드에 있다. 만약 그 앱이 단말기에서 사용할 수 있는 것보다 더 많은 메모리를 필요로 한다면, 시스템은 추가 작업을 수행할 필요 없이 자동으로 당신의 앱을 종료한다.

당신의 목표는 좋은 시민이 되기 위해 앱을 설계하는 것이어야 한다. 이것은 가능한 빨리 포그라운드로 이동하는 데 걸리는 시간을 유지하면서 백그라운드에 있는 동안 메모리 발자국을 줄이는 것을 의미한다.

다음 두 가지 시나리오를 어떻게 처리해야 하는지 알아봐라:

* 당신의 앱은 텍스처, 모델 및 기타 에셋을 메모리에 저장해야 한다. 당신의 앱이 백그라운드로 이동할 때 재생성하는데 오랜 시간이 걸리는 자원은 절대 폐기되어서는 안 된다.
* 당신의 앱은 빠르고 쉽게 다시 만들 수 있는 객체를 폐기해야 한다. 대량의 메모리를 사용하는 객체를 찾아라.

쉬운 타겟은 렌더링 결과를 보관하기 위해 앱이 할당한 프레임 버퍼이다. 앱이 백그라운드에 있으면 사용자에게 표시되지 않으며 OpenGL ES를 사용하여 새로운 콘텐츠를 렌더링할 수 없다. 즉, 앱의 프레임 버퍼에 의해 소비되는 메모리가 할당되지만 유용하지는 않다는 것을 의미한다. 또한, 프레임 버퍼의 내용은 일시적이다. 대부분의 앱은 새로운 프레임을 렌더링할 때마다 프레임 버퍼의 내용을 다시 만들어 낸다. 이를 통해 렌더버퍼는 쉽게 다시 만들 수 있는 메모리의 집약적인 자원으로, 백그라운드로 이동할 때 폐기할 수 있는 객체의 좋은 후보가 된다.

GLKit 뷰 및 뷰 컨트롤러를 사용하는 경우, [`GLKViewController`](https://developer.apple.com/documentation/glkit/glkviewcontroller) 클래스는 연결된 뷰 프레임 버퍼를 자동으로 삭제한다. 다른 용도에 대해 프레임버퍼를 수동으로 작성하는 경우 앱이 백그라운드로 이동할 때 해당 프레임버퍼를 폐기해야 한다. 어느 경우든, 당신은 또한 앱이 그 당시에 어떤 다른 일시적인 자원을 처분할 수 있는지 고려해야 한다.

### Supporting High-Resolution Displays

 기본적으로 GLKit 뷰의 [`contentScaleFactor`](https://developer.apple.com/documentation/uikit/uiview/1622657-contentscalefactor) 속성의 값은 해당 속성이 포함된 화면의 스케일과 일치하므로 관련 프레임 버퍼가 디스플레이 전체 해상도에서 렌더링하도록 구성된다. UIKit에서 고해상도 디스플레이를 지원하는 방법에 대한 자세한 내용은 [Supporting High-Resolution Screens In Views](https://developer.apple.com/library/archive/documentation/2DDrawing/Conceptual/DrawingPrintingiOS/SupportingHiResScreensInViews/SupportingHiResScreensInViews.html#//apple_ref/doc/uid/TP40010156-CH15)를 참조하라.

Core Animation 레이어를 사용하여 OpenGL ES 콘텐츠를 표시하는 경우, 스케일 팩터는 기본적으로 `1.0`으로 설정된다. 레티나 디스플레이의 전체 해상도에서 그리려면 [`CAEAGLLayer`](https://developer.apple.com/documentation/quartzcore/caeagllayer) 객체의 스케일 팩터를 화면의 스케일 팩터와 일치하도록 변경해야 한다.

고해상도 디스플레이를 통해 장치를 지원할 경우, 해당 앱의 모델 및 텍스처 에셋을 적절하게 조정해야 한다. 고해상도 장치에서 실행할 때 더 자세한 모델과 텍스처를 선택하여 더 나은 이미지를 렌더링할 수 있다. 반대로 표준 해상도 장치에서는 더 작은 모델과 텍스처를 사용할 수 있다.

> **중요:** 많은 OpenGL ES API 호출은 화면 픽셀 치수를 표현한다. 1.0보다 큰 스케일 팩터를 사용하는 경우 `glScissor`, `glBlitFramebuffer`, `glLineWidth` 또는 `glPointSize` 함수 또는 `gl_PointSize` 셰이더 변수를 사용할 때 이에 따라 치수를 조정해야 한다.

고해상도 디스플레이 지원 방법을 결정할 때 중요한 요소는 성능이다. 레티나 디스플레이에서 스케일 팩터가 두 배로 증가하면 픽셀 수가 네 배로 늘어나 GPU가 네 배나 많은 프레그먼트를 처리하게 된다. 만약 당신의 앱이 많은 프레그먼트당 계산을 수행한다면, 픽셀의 증가는 프레임률을 낮출 수 있다. 앱 실행 속도가 더 느릴 경우 다으 옵션 중 하나를 고려하라:

* 이 문서에 나와 있는 performance-tuning 가이드라인을 사용하여 프레그먼트 셰이더의 성능을 최적화하라.
* 프레그먼트 셰이더에 더 간단한 알고리즘을 구현하라. 이렇게 하면 전체 이미지를 더 높은 해상도로 렌더링하기 위해 개별 픽셀의 품질을 낮출 수 있다.
* 1.0과 화면 배율 사이의 스케일 팩터를 사용하라. 스케일 팩터 1.5는 스케일 팩터 1.0보다 우수한 품질을 제공하지만 이미지 2.0보다 더 적은 픽셀을 채울 필요가 있다.
* GLKView 객체의 [`drawableColorFormat`](https://developer.apple.com/documentation/glkit/glkview/1615587-drawablecolorformat) 및 [`drawableDepthFormat`](https://developer.apple.com/documentation/glkit/glkview/1615583-drawabledepthformat) 속성에 낮은 정밀도 형식을 사용하라. 이렇게 하면 기본 렌더버퍼에 동작하는 데 필요한 메모리 대역폭을 줄일 수 있다.
* 낮은 스케일 팩터를 사용하고 멀티샘플링을 활성화하라. 멀티샘플링은 고해상도 디스플레이를 지원하지 않는 기기에서도 높은 품질을 제공한다는 점도 장점이다. `GLKView` 객체에 대해 멀티샘플링을 사용하려면 해당 [`drawableMultisample`](https://developer.apple.com/documentation/glkit/glkview/1615601-drawablemultisample) 속성의 값을 변경하라. GLKView 뷰에 렌더링하지 않는 경우 최종 이미지를 표시하기 전에 수동으로 멀티샘플링 버퍼를 설정하고 해결하라. \([Using Multisampling to Improve Image Quality](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/WorkingwithEAGLContexts/WorkingwithEAGLContexts.html#//apple_ref/doc/uid/TP40008793-CH103-SW4) 참조\) 멀티샘플링은 무료가 아니다. 추가 샘플을 저장하려면 추가 메모리가 필요하며, 샘플을 resolve 프레임 버퍼로 분해하는 데는 시간이 걸린다. 앱에 멀티샘플링을 추가하는 경우 항상 앱 성능을 테스트하여 허용 가능한지 확인하라.

### Supporting Multiple Interface Orientations

다른 앱과 마찬가지로 OpenGL ES 앱은 콘텐츠에 적합한 사용자 인터페이스 방향을 지원해야 한다. 앱에 대해 지원되는 인터페이스 방향을 정보 속성 목록에 표시하거나 지원되는 콘텐츠를 호스팅하는 뷰 컨트롤러에 대해 선언하는 경우 [`supportedInterfaceOrientations`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621435-supportedinterfaceorientations) 메서드를 사용하라. \(자세한 내용은 [_View Controller Programming Guide for iOS_](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457)를 참조하라.\)

기본적으로 및 클래스는 방향 변경을 자동으로 처리한다. 사용자가 기기를 지원되는 방향으로 회전시킬 때 시스템은 방향 변경을 애니메이션화하고 뷰 컨트롤러의 뷰 크기를 변경한다. 크기가 변경되면 `GLKView` 객체는 프레임 버퍼 및 뷰 포트의 크기를 적절하게 조정한다. 이 변경에 응답해야 하는 경우 GLKViewController 하위 클래스에서 [`viewWillLayoutSubviews`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621437-viewwilllayoutsubviews) 또는 [`viewDidLayoutSubviews`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621398-viewdidlayoutsubviews) 메서드를 구현하거나 사용자 정의 하위 클래스를 사용하는 경우 [`layoutSubviews`](https://developer.apple.com/documentation/uikit/uiview/1622482-layoutsubviews) 메서드를 구현하라.

Core Animation 레이어를 사용하여 OpenGL ES 콘텐츠를 그리는 경우에도 사용자 인터페이스 방향을 관리하기 위해 앱에 뷰 컨트롤러가 포함되어야 한다.

### Presenting OpenGL ES Content on External Displays

iOS 기기는 외부 디스플레이에 부착할 수 있다. 외부 디스플레이의 해상도와 그 콘텐츠 스케일 팩터는 메인 화면의 해상도 및 스케일 팩터와 다를 수 있다. 프레임을 렌더링하는 코드는 일치하도록 조정되어야 한다.

외부 디스플레이에 그리는 절차는 메인 화면에서 실행하는 방법과 거의 동일하다.

1. [_Multiple Display Programming Guide for iOS_](https://developer.apple.com/library/archive/documentation/WindowsViews/Conceptual/WindowAndScreenGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40012555)의 단계를 따라 외부 디스플레이에 윈도우를 생성하라.
2. 윈도우에 렌더링 전략에 적합한 뷰 또는 뷰 컨트롤러 객체를 추가한다.
   * GLKit으로 렌더링하는 경우 [`GLKViewController`](https://developer.apple.com/documentation/glkit/glkviewcontroller) 및 [`GLKView`](https://developer.apple.com/documentation/glkit/glkview)의 인스턴스\(또는 사용자 정의 하위 클래스\)를 설정하고 [`rootViewController`](https://developer.apple.com/documentation/uikit/uiwindow/1621581-rootviewcontroller) 속성을 사용하여 해당 인스턴스를 윈도우에 추가하라.
   * Core Animation 레이어로 렌더링하는 경우 해당 레이어가 포함된 뷰를 윈도우 하위 뷰로 추가하라. 렌더링에 애니메이션 루프를 사용하려면 윈도우의 [`screen`](https://developer.apple.com/documentation/uikit/uiwindow/1621597-screen) 속성을 검색하고 [`displayLinkWithTarget:selector:`](https://developer.apple.com/documentation/uikit/uiscreen/1617820-displaylink) 메서드를 호출하여 외부 디스플레이에 최적화된 디스플레이 링크 객체를 만들어라.

