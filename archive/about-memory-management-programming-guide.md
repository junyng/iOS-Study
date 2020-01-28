# About Memory Management Programming Guide

애플리케이션 메모리 관리란 프로그램 실행 시간 동안 메모리를 할당하고, 이를 사용하고, 그 작업을 완료하면 메모리를 자유롭게 하는 과정이다. 잘 작성된 프로그램은 가능한 한 적은 메모리를 사용한다. Objective-C에서는 제한된 메모리 자원의 소유권을 많은 데이터와 코드 조각에 분배하는 방법으로도 볼 수 있다. 이 가이드를 완료한 후 객체의 생명 주기를 명시적으로 관리하고 더 이상 필요하지 않을 때 자유롭게 애플리케이션 메모리를 관리하는 데 필요한 지식을 습득하라.

메모리 관리는 일반적으로 개별 객체 수준에서 고려되지만 실제로 객체 그래프를 관리하는 것이 목표이다. 실제로 필요한 것보다 더 많은 객체를 메모리에 담지 않기를 바란다.

![](../.gitbook/assets/memory_management_2x.png)

### 한눈에

Objective-C는 두 가지 애플리케이션 메모리 관리 메서드를 제공한다.

1. 본 가이드에 기술된, "manual retain-release" 또는 _MRR_로 불리는 메서드에서 당신은 자신이 소유한 객체를 추적하여 메모리를 명시적으로 관리한다. 이는 런타임 환경과 연계하여 제공하는 모델인 Foundation클래스 [`NSObject`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)을 사용하여 구현한다.
2. Automatic Reference Counting 또는 _ARC_에서 시스템은 MRR과 동일한 참조 카운팅 시스템을 사용하지만 컴파일-타임에 적절한 메모리 관리 메서드가 호출된다. 새로운 프로젝트에 ARC를 사용하는 것을 강력히 권장한다. ARC를 사용하는 경우, 일부 상황에서는 도움이 될 수 있지만 일반적으로 이 문서에 설명된 기본 구현을 이해할 필요가 없다. ARC에 대한 자세한 내용은 [_Transitioning to ARC Release Notes_](https://developer.apple.com/library/archive/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226)을 참조하라.

#### 메모리 관련 문제를 방지하는 모범 사례

잘못된 메모리 관리로 인한 두 가지 주요 문제가 있다:

* 아직 사용 중인 데이터의 여유 확보 또는 덮어쓰기 이로 인해 메모리 손상이 발생하며 일반적으로 애플리케이션이 손상되거나 사용자 데이터가 손상된다.
* 더 이상 사용되지 않는 데이터를 해제하지 않으면 메모리 누수가 발생한다. 메모리 누수는 할당된 메모리가 다시는 사용되지 않더라도 해제되지 않는 것이다. 누수로 인해 애플리케이션이 지속적으로 증가하는 메모리를 사용하게 되고, 이로 인해 시스템 성능이 저하되거나 애플리케이션이 종료될 수 있다.

그러나 레퍼런스 카운트의 관점에서 메모리 관리에 대해 생각하는 것은 종종 역효과를 내는데, 이는 실제 목표보다는 구현 세부사항 측면에서 메모리 관리를 고려하는 경향이 있기 때문이다. 대신 객체 소유와 객체 그래프라는 관점에서 메모리 관리를 생각해야 한다.

Cocoa는 어떤 방법으로 반환되는 객체를 소유할 때를 나타내기 위해 간단한 명명 규칙을 사용한다.

[Memory Management Policy](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmRules.html#//apple_ref/doc/uid/20000994-BAJHFBGH)를 참조하라.

기본 정책은 간단하지만 메모리 관리를 더 쉽게 하고 프로그램이 안정적이고 견고하게 유지되도록 하는 동시에 리소스 요구 사항을 최소화하는 데 도움이 되는 몇 가지 실질적인 단계가 있다.

[Practical Memory Management](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW1)를 참조하라.

오토릴리즈 풀 블록은 객체에 "추정된" `release` 메시지를 보낼 수 있는 메커니즘을 제공한다. 이는 객체의 소유권을 포기하고 싶지만 즉시 \(메서드에서 객체를 반환할 때와 같이\) 할당될 가능성을 피하고 싶은 상황에서 유용하다. 자기만의 오토릴리즈 풀 블록을 사용할 수도 있다.

[Using Autorelease Pool Blocks](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmAutoreleasePools.html#//apple_ref/doc/uid/20000047-CJBFBEDI)를 참조하라.

#### 분석 툴을 사용하여 메모리 문제 디버그

컴파일 타임에 코드의 문제를 식별하려면 Xcode에 내장된 Clang Static Analyzer를 사용할 수 있다.

그럼에도 불구하고 메모리 관리 문제가 발생하면 문제를 확인하고 진단하는 데 사용할 수 있는 툴과 기술이 있다.

* 많은 툴과 기법은 Technical Note TN2239, [_iOS Debugging Magic_](https://developer.apple.com/library/archive/technotes/tn2239/_index.html#//apple_ref/doc/uid/DTS40010638)에 기술되어 있다. 특히 NSZombie를 사용하여 과다-릴리즈된 객체를 찾는데 사용한다.
* Instruments를 사용하여 레퍼런스 카운트 이벤트를 추적하고 메모리 누수를 찾을 수 있다. [Collecting Data on Your App](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/TheInstrumentsWorkflow.html#//apple_ref/doc/uid/TP40004652-CH5)를 참조하라.



