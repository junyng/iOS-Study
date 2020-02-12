# Animation Types and Timing Programming Guide

## 애니메이션 유형 및 타이밍 프로그래밍 가이드 소개 <a id="pageTitle"></a>

> **중요:** 이 문서는 더 이상 업데이트되지 않는다. Apple SDK에 대한 최신 정보는 [documentation website](https://developer.apple.com/documentation)를 참조하라.

이 문서는 Core Animation과 함께 사용되는 타이밍 및 애니메이션 클래스와 관련된 기본 개념을 설명한다. 코어 애니메이션은 고성능 합성 엔진과 애니메이션 프로그래밍 인터페이스를 사용하기 쉬운 것을 결합한 Objective-C 프레임워크이다.

> **참고**: 애니메이션은 본질적으로 시각적인 매체다. _Animation Types and Timing Programming Guide_의 HTML 버전에는 개념 예제를 돕기 위한 예시 애니메이션을 보여주는 QuickTime 영화 \(정적 이미지와 함께\)가 포함되어 있다. PDF 버전에는 정적 이미지만 포함되어 있다.

Cocoa 애플리케이션에서 Core Animation과 함께 작업하는 것에 대한 이해를 얻으려면 이 문서를 읽어야 한다. Core Animation은 Objective-C 속성을 광범위하게 사용하기 때문에 Objective-C 2.0 프로그래밍 언어는 필수 요소로 간주되어야 한다. 또한 Key-Value Coding Programming Guide에 설명된 키-밸류 코딩에 익숙해야 한다. Quartz 2D 프로그래밍 가이드에 설명된 Quartz 2D 영상 기술에 대한 친숙함도 필요하지는 않지만 도움이 된다.

### 이 문서의 구성

_Animation Types and Timing_은 다음 주제로 구성된다:

* [Animation Class Roadmap](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Animation_Types_Timing/Articles/AnimationTimingTypesOverview.html#//apple_ref/doc/uid/TP40006669-SW1) 애니메이션 클래스 및 타이밍 프로토콜의 개요를 제공한다.
* [Timing, Timespaces, and CAAnimation](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Animation_Types_Timing/Articles/Timing.html#//apple_ref/doc/uid/TP40006670-SW1) 코어 애니메이션의 타이밍 모델과 `CAAnimation` 추상 클래스를 자세히 설명한다.
* [Property-Based Animations](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Animation_Types_Timing/Articles/PropertyAnimations.html#//apple_ref/doc/uid/TP40006672-SW1) 속성 기반 애니메이션인 `CABasicAnimation` 및 `CAKeyframeAnimation`을 설명한다.
* [Transition Animation](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Animation_Types_Timing/Articles/TransitionAnimations.html#//apple_ref/doc/uid/TP40006674-SW1) 전환 애니메이션 클래스인 `CATransition` 을 설명한다.

### 참고

이 프로그래밍 가이드에서는 Core Animation에서 사용하는 몇가지 기술에 대해 설명한다.

* [_Animation Overview_](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/Animation_Overview/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004952) 는 OS X에서 사용할 수 있는 애니메이션 기술을 설명한다.
* [_Core Animation Programming Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004514) 에는 일반적인 Core Animation 태스크를 보여주는 코드 조각이 포함되어 있다.
* [_Animation Programming Guide for Cocoa_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/AnimationGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003592) 는 Cocoa Applications에서 사용할 수 있는 애니메이션 기능을 설명한다.

