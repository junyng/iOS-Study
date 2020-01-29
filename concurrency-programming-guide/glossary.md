# Glossary

* **application**  

  사용자에 대한 그래픽 인터페이스를 표시하는 특정 프로그램 유형.  

* **asynchronous design approach**  

  애플리케이션의 메인 쓰레드 또는 다른 실행 쓰레드와 동시에 실행할 수 있는 코드 블록 주위에 애플리케이션을 구성하는 원칙. 비동기 태스크는 하나의 쓰레드에 의해 시작되지만 실제로 다른 쓰레드에서 실행되어 태스크를 보다 빨리 끝내기 위해 추가 프로세서 리소스를 사용한다.  

* **block object**  인라인 코드 및 데이터를 캡슐화하여 나중에 수행할 수 있도록하는 C 구조체. 블록을 사용하여 현재 쓰레드의 인라인 또는 디스패치 큐를 사용하여 별도의 쓰레드에서 수행할 태스크를 캡슐화한다. 자세한 내용은 [_Blocks Programming Topics_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Blocks/Articles/00_Introduction.html#//apple_ref/doc/uid/TP40007502)를 참조하라. 
* **concurrent operation**  

  start 메서드가 호출된 쓰레드에서 대스크를 수행하지 않는 오퍼레이션 객체. 컨커런트 오퍼레이션은 일반적으로 자체 쓰레드를 설정하거나 태스크를 수행할 별도의 쓰레드를 설정하는 인터페이스를 호출한다.  

* **condition**  

  리소스에 대한 접근을 동기화하는 데 사용되는 구성이다. condition에서 대기하는 쓰레드는 다른 쓰레드가 명시적으로 condition을 신호할 때까지 진행할 수 없다.  

* **critical section**  

  한 번에 하나의 쓰레드만 실행해야 하는 코드 부분.  

* **custom source**  

  애플리케이션-정의 이벤트를 처리하는 데 용되는 [`dispatch source`](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Glossary/Glossary.html#//apple_ref/doc/uid/TP40008091-CH104-SW19)이다. 사용자 정의 소스는 애플리케이션이 생성하는 이벤트에 대한 응답으로 사용자 정의 이벤트 처리기를 호출한다.  

* **descriptor**  

  파일, 소켓 또는 기타 시스템 리소스에 접근하는 데 사용되는 추상 식별자.  

* **dispatch queue**  

  애플리케이션의 태스크를 실행하는 데 사용하는 [Grand Central Dispatch \(GCD\)](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Glossary/Glossary.html#//apple_ref/doc/uid/TP40008091-CH104-SW23) 구조체. GCD는 태스크를 직렬로 또는 병렬로 실행하기 위한 디스패치 큐들을 정의한다.  

* **dispatch source**  

  시스템-관련 이벤트를 처리하기 위해 작성하는 [Grand Central Dispatch \(GCD\)](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Glossary/Glossary.html#//apple_ref/doc/uid/TP40008091-CH104-SW23) 데이터 구조체.  

* **descriptor dispatch source**  

  파일 관련 이벤트를 처리하는 데 사용되는 [`dispatch source`](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Glossary/Glossary.html#//apple_ref/doc/uid/TP40008091-CH104-SW19). 파일 설명자 소스는 파일 데이터를 읽기 또는 쓰기에 사용할 수 있거나 파일 시스템 변경에 대한 응답으로 사용자 정의 이벤트 처리기를 호출한다.  

* **dynamic shared library**  

  애플리케이션 바이너리의 일부로 정적 연결되지 않고 애플리케이션의 프로세스 공간에 동적으로 로드되는 이진 실행 파일.  

* **framework**  

  동적 공유 라이브러리를 지원하는 리소스 및 헤더 파일을 패키징하는 번들 타입. 자세한 내용은 [_Framework Programming Guide_](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPFrameworks/Frameworks.html#//apple_ref/doc/uid/10000183i)을 참조하라.  

* **global dispatch queue**  

  [Grand Central Dispatch \(GCD\)](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Glossary/Glossary.html#//apple_ref/doc/uid/TP40008091-CH104-SW23)가 자동으로 애플리케이션에 제공하는 . 글로벌 큐를 직접 만들거나 보유하거나 릴리즈할 필요가 없다. 대신 시스템-제공 함수를 사용하여 돌려 받아라.  

* **Grand Central Dispatch \(GCD\)**  

  비동기적인 태스크를 병렬로 처리하는 기술. GCD는 OS X v10.6 이후 또는 iOS 4.0 이후에서 사용가능하다.  

* **input source**  

  쓰레드의 비동기 이벤트 소스. 입력 소스는 포트 기반 또는 수동으로 트리거 할 수 있으며 쓰레드의 런 루프에 부착해야 한다.  

* **joinable thread**  

  종료 즉시 리소스가 회수되지 않는 쓰레드. 결합 가능한 쓰레드는 리소스를 재확보하기 전에 명시적으로 분리하거나 다른 쓰레드로 연결해야 한다. 결합 가능한 쓰레드는 결합된 쓰레드에 반환 값을 제공한다.  

* **library**  

  낮은 수준의 시스템 이벤트를 모니터링하기 위한 UNIX 기능. 자세한 내용은[`kqueue`](https://developer.apple.com/library/archive/documentation/System/Conceptual/ManPages_iPhoneOS/man2/kqueue.2.html#//apple_ref/doc/man/2/kqueue) 메뉴얼 페이지를 참조하라.  

* **Mach port dispatch source**  

  Mach 포트에 도착한 이벤트를 처리하는 데 사용되는 [`dispatch source`](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Glossary/Glossary.html#//apple_ref/doc/uid/TP40008091-CH104-SW19)  

* **main thread**  

  소유 프로세스가 생성될 때 생성되는 특수한 유형의 쓰레드. 프로그램의 메인 쓰레드가 종료되면 프로세스가 종료된다.  

* **mutex**  

  공유 리소스에 대한 상호 배타적 접근을 제공하는 락. 뮤텍스 락은 한 번에 하나의 쓰레드만으로 고정할 수 있다. 다른 쓰레드에 의해 고정된 뮤텍스를 획득하려고 하면 락이 최종적으로 획득될때까지 현재 쓰레드를 절전 모드로 전환한다.  

* **Open Computing Language \(OpenCL\)**  

  컴퓨터 그래픽 프로세서에서 범용 연산 수행을 위한 표준 기반 기술. 자세한 내용은 [_OpenCL Programming Guide for Mac_](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/OpenCL_MacProgGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008312)을 참조하라.  

* **operation object**  

  [`NSOperation`](https://developer.apple.com/documentation/foundation/nsoperation) 클래스의 인스턴스. 오퍼레이션 객체는 태스크와 관련된 코드와 데이터를 실행 가능한 단위로 래핑한다.  

* **operation queue**   [`NSOperationQueue`](https://developer.apple.com/documentation/foundation/operationqueue) 클래스의 인스턴스. 오퍼레이션 큐는 오퍼레이션 객체의 실행을 관리한다. 
* **private dispatch queue**  

  명시적으로 생성, 리테인 및 릴리스하는 디스패치 큐.  

* **process**  

  애플리케이션 또는 프로그램의 런타임 인스턴스. 프로세스는 다른 프로그램에 할당된 것과 독립적인 고유한 가상 메모리 공간과 시스템 리소스 \(포트 권한 포함\)을 가지고 있다. 프로세스는 항상 하나 이상의 쓰레드 \(메인 쓰레드\)를 포함하며, 임의의 수의 추가 쓰레드를 포함할 수 있다.  

* **process dispatch source**  

  프로세스 관련 이벤트를 처리하는 데 사용되는 디소스스패치   

* **program**  

  일부 태스크를 수행하기 위해 실행할 수 있는 코드와 리소스의 조합. 그래픽 애플리케이션도 프로그램으로 간주되지만 프로그램에는 그래픽 사용자 인터페이스가 필요하지 않다.  

* **reentrant**  

  이미 다른 쓰레드에서 실행 중일 때 안전하게 새 쓰레드에서 시작할 수 있는 코드.  

* **run loop**  

  이벤트를 수신하고 적절한 핸들러에 보내는 이벤트 처리 루프.  

* **run loop mode**  

  특정 이름과 관련된 입력 소스, 타이머 소스 및 런 루프 관찰자의 모음. 특정 "모드"에서 실행되는 경우, 런 루프는 해당 모드와 관련된 소스 및 옵저버만 모니터링한다.  

* **run loop object**  

  [`NSRunLoop`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop) 클래스 또는 [`CFRunLoopRef`](https://developer.apple.com/documentation/corefoundation/cfrunloopref) 불투명 유형의 인스턴스이다. 이러한 객체는 쓰레드에서 이벤트 처리 루프를 구현하기 위한 인터페이스를 제공한다.  

* **run loop observer**  

  런 루프 실행 여러 단계 중 노티피케이션 수신기.  

* **semaphore**  

  공유 리소스에 대한 접근을 제한하는 보호된 변수. 뮤텍스와 condition은 모두 다른 형태의 세마포어이다.  

* **signal**  

  도메인 외부에서 프로세스를 조작하기 위한 UNIX 메커니즘. 이 시스템은 애플리케이션이 불법적인 지시를 실행했는지와 같은 중요한 메시지를 애플리케이션에 전달하기 위한 신호를 사용한다. 자세한 내용은 [signal](https://developer.apple.com/library/archive/documentation/System/Conceptual/ManPages_iPhoneOS/man3/signal.3.html#//apple_ref/doc/man/3/signal) 메뉴얼 페이지를 참조하라.  

* **signal dispatch source**  

  UNIX 신호를 처리하는 데 사용되는 디스패치 소스. 프로세스에서 UNIX 신호를 수신할 때마다 디스패치 소스는 사용자 정의 이벤트 핸들러를 호출한다.  

* **task**  

  수행해야 할 작업량. 일부 기술 \(가장 주목할 만한 Carbon Multiprocessing Services\)은 이 용어를 다르게 사용하지만, 선호되는 용도는 수행할 작업의 양을 나타내는 추상적 개념이다.  

* **thread**  

  프로세스의 실행 흐름. 각 쓰레드는 자체 스택 공간을 가지지만, 그렇지 않으면 동일한 프로세스에서 다른 쓰레드와 메모리를 공유한다.  

* **timer dispatch source**  

  주기적 이벤트를 처리하는 데 사용되는 디스패치 소스. 타이머 소스는 사용자 정의 이벤트 핸들러를 정기적인 시간-기반 간격으로 호출된다.

