# Performance Overview

성능은 모든 소프트웨어 제품에서 중요한 설계 요인이다. 프로그램이 느리게 실행되거나 회전하는 커서를 표시하면 사용자들은 프로그램에 좌절하고 대안을 찾기 쉽다. 합리적인 수준의 성능을 유지하려면 근면함이 필요하지만, 일찍 생각하기 시작할수록 문제를 잡고 고치는 것은 더 쉽다.

### Who Should Read This Document

_Performance Overview_는 소프트웨어 성능 분석 분야에 새롭게 등장한 개발자에게 필수적인 가이드이다. 이 문서에서는 성능을 좌우하는 요인에 대한 개요를 제공하고 공통 성능 문제를 식별하고 수정하기 위한 접근 방식을 제공한다. 또한 성능 문제를 식별하고 해결하는 데 사용할 수 있는 특정 도구와 문서를 소개한다.

### Organization of This Document

이 문서에는 다음과 같은 장이 있다:

* [Developing for Performance](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/PerformanceOverview/DevelopingForPerf/DevelopingForPerf.html#//apple_ref/doc/uid/TP40001410-CH203-CJBEFGHG) 성능을 구성하는 요소와 소프트웨어에서 최상의 성능을 달성하기 위한 접근 방법을 설명한다.
* [Basic Performance Tips](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/PerformanceOverview/BasicTips/BasicTips.html#//apple_ref/doc/uid/TP40001410-CH204-BBCGCFGF) 분석하기 위한 코드의 공통 영역을 설명하고 몇 가지 기본적인 성능 기법을 제공한다.
* [Performance Tools](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/PerformanceOverview/PerformanceTools/PerformanceTools.html#//apple_ref/doc/uid/TP40001410-CH205-BCIIHAAJ) 프로그램의 성능 분석을 위해 사용 가능한 도구에 대해 설명한다.
* [Doing an Initial Performance Evaluation](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/PerformanceOverview/InitialEvaluation/InitialEvaluation.html#//apple_ref/doc/uid/TP40001410-CH206-CJBFHBDB) 주요 도구의 기본을 안내하고 성능 문제를 찾는 데 사용하는 방법을 보여준다.

### Providing Feedback

문서에 대한 피드백이 있는 경우, 모든 페이지 하단에 있는 기본 제공 피드백 양식을 사용하여 제공할 수 있다.

만약 당신이 애플 소프트웨어나 문서에서 버그를 마주하면, 당신은 그것들을 애플에 보고하도록 권장된다. 또한 제품 또는 문서의 향후 수정사항에서 볼 기능을 표시하기 위한 개선 요청을 제출할 수 있다. 버그 또는 개선 요청을 파일화하려면 [Apple Developer website](http://developer.apple.com/)의 Bug Reporting 페이지로 이동하라:

{% embed url="http://developer.apple.com/bugreporter/" %}

버그를 저장하려면, Apple Developer로 등록되어야 한다. [Apple Developer Registration page](http://developer.apple.com/programs/start/register/create.php)의 가이드에 따라 아이디를 무료로 얻을 수 있다.

### See Also

이 문서 외에도 성능의 보다 구체적인 측면을 다루는 여러 문서가 있다. 성능 문제를 분석하고 해결하는 방법에 대한 자세한 팁을 보려면 이 문서를 조사하라.

* [_Code Size Performance Guidelines_](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/CodeFootprint/CodeFootprint.html#//apple_ref/doc/uid/10000149i) 프로그램의 메모리 설치 공간을 개선하는 방법에 대한 조언을 제공한다.
* [_Code Speed Performance Guidelines_](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/CodeSpeed/CodeSpeed.html#//apple_ref/doc/uid/10000150i) 알고리즘을 조정하고 성능 병목 현상을 찾는 방법에 대한 조언을 제공한다.
* [_Drawing Performance Guidelines_](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/Drawing/Articles/DrawingPerformance.html#//apple_ref/doc/uid/10000151i) 프로그램의 그리기 관련 코드를 최적화하는 방법에 대한 조언을 제공한다.
* [_File-System Performance Guidelines_](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/FileSystem/FileSystem.html#//apple_ref/doc/uid/10000161i) 파일에 보다 효율적으로 접근하는 방법에 대한 조언을 제공한다.
* [_Launch Time Performance Guidelines_](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/LaunchTime/LaunchTime.html#//apple_ref/doc/uid/10000148i) 애플리케이션의 시작 시간을 단축하는 방법에 대한 조언을 제공한다.
* [_Memory Usage Performance Guidelines_](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/ManagingMemory/ManagingMemory.html#//apple_ref/doc/uid/10000160i) 메모리를 더 효율적으로 사용하는 방법과 현재 메모리 사용량을 분석하는 방법에 대한 조언을 제공한다.
* [_Concurrency Programming Guide_](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091) 작업을 병렬로 실행하는 방법에 대한 자세한 정보와 예시를 제공한다.
* [_64-Bit Transition Guide_](https://developer.apple.com/library/archive/documentation/Darwin/Conceptual/64bitPorting/intro/intro.html#//apple_ref/doc/uid/TP40001064) 64비트 바이너리의 성능 영향을 논의하고 이러한 바이너리를 작성할 때 적절한 가이드를 제공한다.

