---
description: 호스트 운영 체제 및 기타 프로세스와의 앱의 상호 작용을 관리하고 저수준 동시성 기능을 구현한다.
---

# Processes and Threads

### Topics

#### Run Loop Scheduling

* [`class RunLoop`](https://developer.apple.com/documentation/foundation/runloop) 입력 소스를 관리하는 객체에 대한 프로그램 인터페이스.
* [`class Timer`](https://developer.apple.com/documentation/foundation/timer) 특정 시간 간격이 경과한 후 작동되는 타이로 타겟 객체로 지정된 메시지를 보낸다.

#### Process Info

* [`class ProcessInfo`](https://developer.apple.com/documentation/foundation/processinfo) 현재 프로세스에 대한 정보 모음.

#### Threads and Locking

* [`class Thread`](https://developer.apple.com/documentation/foundation/thread) 실행 쓰레드.
* [`protocol NSLocking`](https://developer.apple.com/documentation/foundation/nslocking) 락 객체를 정의하는 클래스에서 채택된 기본 메서드.
* [`class NSLock`](https://developer.apple.com/documentation/foundation/nslock) 동일한 애플리케이션 내에서 여러 실행 쓰레드의 작동을 조정하는 객체.
* [`class NSRecursiveLock`](https://developer.apple.com/documentation/foundation/nsrecursivelock) 교착 상태를 일으키지 않고 동일한 쓰레드에 의해 여러번 획득할 수 있는 락.
* [`class NSDistributedLock`](https://developer.apple.com/documentation/foundation/nsdistributedlock) 여러 호스트에 있는 여러 애플리케이션이 파일과 같은 일부 공유 리소스에 대한 접근을 제한하는 데 사용할 수 있는 락.
* [`class NSConditionLock`](https://developer.apple.com/documentation/foundation/nsconditionlock) 특정 사용자 정의 조건과 연관될 수 있는 락.
* [`class NSCondition`](https://developer.apple.com/documentation/foundation/nscondition) POSIX 스타일 조건에 사용되는 의미 변수를 따르는 컨디션 변수.

#### Operations

* [`class OperationQueue`](https://developer.apple.com/documentation/foundation/operationqueue) 오퍼레이션 실행을 규제하는 큐.
* [`class Operation`](https://developer.apple.com/documentation/foundation/operation) 단일 태스크과 연관된 코드 및 데이터를 나타내는 추상 클래스이다.
* [`class BlockOperation`](https://developer.apple.com/documentation/foundation/blockoperation) 하나 이상의 블록의 동시 실행을 관리하는 오퍼레이션.

#### Scripts and External Tasks

* [`class Process`](https://developer.apple.com/documentation/foundation/process) 현재 프로세스의 하위 프로세스를 나타내는 객체.
* [`class NSUserScriptTask`](https://developer.apple.com/documentation/foundation/nsuserscripttask) 스크립트를 실행하는 객체.
* [`class NSUserAppleScriptTask`](https://developer.apple.com/documentation/foundation/nsuserapplescripttask) AppleScript 스크립트를 실행하는 객체.
* [`class NSUserAutomatorTask`](https://developer.apple.com/documentation/foundation/nsuserautomatortask) Automator 워크플로우를 실행하는 객체.
* [`class NSUserUnixTask`](https://developer.apple.com/documentation/foundation/nsuserunixtask)

  유닉스 응용 프로그램을 실행하는 객체.

### See Also

#### Low-Level Utilities

* [XPC](https://developer.apple.com/documentation/foundation/xpc) 시큐어 프로세스 간의 통신을 관리한다.
* [Object Runtime](https://developer.apple.com/documentation/foundation/object_runtime) 기본 Objective-C 기능, Cocoa 디자인 패턴 및 Swift 통합에 대한 낮은 수준의 지원을 받는다.
* [Streams, Sockets, and Ports](https://developer.apple.com/documentation/foundation/streams_sockets_and_ports) 파일, 프로세스 및 네트워크 간의 입력 및 출력을 관리하려면 저 수준의 Unix 기능을 사용하라.

