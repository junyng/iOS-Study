# Concurrency Programming Guide

동시성은 여러 가지 일이 동시에 일어나는 개념이다. 멀티코어 CPU의 확산과 각 프로세서 코어 수가 증가할 뿐이라는 인식으로, 소프트웨어 개발자들은 이를 활용할 새로운 방법이 필요하다. OS X나 iOS와 같은 운영체제는 여러 프로그램을 병렬로 실행할 수 있지만, 이들 프로그램의 대부분은 백그라운드에서 실행되어 연속적인 프로세서 시간이 거의 필요하지 않은 작업을 수행한다. 사용자의 주의를 끌고 컴퓨터를 바쁘게 하는 것이 현재의 포그라운드 어플리케이션이다. 애플리케이션이 해야 할 일이 많지만 사용 가능한 코어의 일부만 차지하면 추가 처리 리소스가 낭비된다.

과거에는 애플리케이션의 동시성을 도입하기 위해서 하나 이상의 추가 쓰레드를 생성해야 했다. 불행하게도 쓰레드 코드를 쓰는 것은 어렵다. 쓰레드는 수동으로 관리해야 하는 낮은 레벨의 도구이다. 애플리케이션의 최적 레드 수가 현재 시스템 부하와 기본 하드웨어에 따라 동적으로 변경될 수 있으므로, 정확한 레드 솔루션을 구현하는 것은 불가능하지 않더라도 매우 어려워진다. 또한 일반적으로 레드와 함께 사용되는 동기화 메커니즘은 성능 향상의 보증 없이 소프트웨어 설계에 복잡성과 위험을 가중시킨다.

OS X와 iOS 모두 레드 기반 시스템과 애플리케이션에서 일반적으로 발견되는 것보다 더 비동기적인 동시 작업 실행 방식을 채택한다. 애플리케이션은 레드를 직접 생성하는 대신 특정 태스크만 정의하고 시스템이 쓰레드를 수행하도록 허용하면 된다. 시스템에서 쓰레드를 관리할 수 있도록 함으로써 애플리케이션이 원시 레드로는 불가능한 수준의 확장성을 확보한다. 또한 애플리케이션 개발자들은 보다 단순하고 효율적인 프로그래밍 모델을 얻는다.

이 문서는 애플리케이션에서 동시성을 구현하기 위해 사용해야 하는 기술을 정의한다. 이 문서에 설명된 기술은 OS X와 iOS 모두에서 사용할 수 있다.

## Organization of This Document

이 문서는 다음의 챕터를 포함한다.

* [Concurrency and Application Design](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/ConcurrencyandApplicationDesign/ConcurrencyandApplicationDesign.html#//apple_ref/doc/uid/TP40008091-CH100-SW1) 비동기 애플리케이션 설계의 기본 사항 및 사용자 지정 작업을 비동기식으로 수행하는 기술 소개
* [Operation Queues](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW1) Objective-C 객체를 사용하여 캡슐화 및 작업을 수행하는 방법을 보여준다.
* [Operation Queues](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW1) C 기반 응용프로그램에서 동시에 태스크를 실행하는 방법을 보여준다.
* [Dispatch Sources](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html#//apple_ref/doc/uid/TP40008091-CH103-SW1) 시스템 이벤트를 비동기식으로 처리하는 방법을 보여준다.
* [Migrating Away from Threads](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/ThreadMigration/ThreadMigration.html#//apple_ref/doc/uid/TP40008091-CH105-SW1) - 기존 쓰레드 기반 코드를 새로운 기술을 사용하기 위해 마이그레이션할 수 있는 팁 및 기술을 제공한다.

이 문서에는 관련 용어를 정의하는 용어집도 포함되어 있다.

## A Note About Terminology

동시성에 대한 논의를 시작하기 전에 혼동을 방지하기 이ㅜ해 몇 가지 관련 용어를 정의할 필요가 있다. UNIX 시스템 또는 이전 OS X 기술에 더 친숙한 개발자는 이 문서에서 다소 다르게 사용되는 "task", "process", "thread" 용어를 찾을 수 있다. 이 문서는 다음과 같은 방식으로 이 용어를 사용한다.

* _thread_  용어는 코드를 위한 별도의 실행 경로를 가리키는 데 사용된다. OS X의 레드에 대한 기본 구현은 POSIX threads API에 기초한다.
* _process_ 용어는 여러 개의 레드를 포함할 수 있는 실행 파일을 가리키는 데 사용된다.
* _task_ 용어는 수행되어야 할 작업의 추상적 개념을 가리킨다.

이 문서에서 사용하는 주요 용어에 대한 전체 정의는 [Glossary](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Glossary/Glossary.html#//apple_ref/doc/uid/TP40008091-CH104-SW2)를 참조하라.

## See Also

이 문서는 응용프로그램에서 동시성 구현을 위해 선호하는 기술에 초점을 맞추고 있으며 레드 사용은 다루지 않는다. 쓰레드 및 기타 스레드 관련 기술 사용에 대한 자세한 내용은 [_Threading Programming Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/Introduction/Introduction.html#//apple_ref/doc/uid/10000057i)를 참조하라.

