# Threading Programming Guide

## Introduction <a id="pageTitle"></a>

쓰레드는 단일 애플리케이션 내에서 여러 코드 경로를 동시에 실행할 수 있는 여러 기술 중 하나이다. operation 객체 및 GCD와 같은 최신 기술이 동시성을 구현하기 위한 보다 현대적이고 효율적인 인프라를 제공하지만 OS X 및 iOS는 쓰레드 작성 및 관리를 위한 인터페이스도 제공한다.

이 문서는 OS X에서 사용할 수 있는 쓰레드 패키지를 소개하고 사용 방법을 보여준다. 이 문서는 또한 애플리케이션 내에서 쓰레딩 및 멀티 쓰레드 코드의 동기화를 지원하기 위해 제공되는 관련 기술에 대해 설명한다.

> 중요
>
> 새 애플리케이션을 개발하는 경우 동시성을 구현하기 위한 대체 OS X 기술을 조사하는 것이 좋다. 쓰레딩 애플리케이션을 구현하는 데 필요한 디자인 기술에 익숙하지 않은 경우 특히 그렇다. 이러한 대체 기술은 동시 실행 경로를 구현하고 기존 쓰레드 보다 훨씬 우수한 성능을 제공하기 위해 수행해야하는 작업량을 단순화한다. 이러한 기술에 대한 자세한 내용은 [_Concurrency Programming Guide_](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)을 참조하라.

### Organization of This Document

이 문서에는 다음과 같은 장과 부록이 있다.

* [About Threaded Programming](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/AboutThreads/AboutThreads.html#//apple_ref/doc/uid/10000057i-CH6-SW2)는 쓰레드의 개념과 애플리케이션 설계에서의 역할이 소개된다.
* [Thread Management](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW2)는 OS X의 쓰레드 기술 및 사용 방법에 대한 정보를 제공한다.
* [Run Loops](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW1)는 보조 쓰레드의 이벤트 처리 루프를 관리하는 방법에 대한 정보를 제공한다.
* [Synchronization](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/ThreadSafety/ThreadSafety.html#//apple_ref/doc/uid/10000057i-CH8-SW1)는 동기화 문제와 여러 쓰레드가 데이터를 손상시키거나 프로그램을 손상시키지 않도록 하는 데 사용하는 도구를 설명한다.
* [Thread Safety Summary](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/ThreadSafetySummary/ThreadSafetySummary.html#//apple_ref/doc/uid/10000057i-CH12-SW1)는 OS X와 iOS의 고유한 쓰레드 안정성과 일부 핵심 프레임워크에 대한 고수준의 개요를 제공한다. 

### See Also

쓰레드 대안에 대한 자세한 내용은 [_Concurrency Programming Guide_](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)를 참조하라.

이 문서는 POSIX 쓰레드 API 사용에 대한 가벼운 범위만 제공한다. 사용 가능한 POSIX 쓰레드 루틴에 대한 자세한 내용은 메인 페이지를 참조하라. POSIX 쓰레드 및 그 사용에 대한 자세한 설명은 저자 David R. Butenhof. _Programming with POSIX Threads_ 를 참조하라.

