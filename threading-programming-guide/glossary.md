# Glossary

* **application**   사용자에 대한 그래픽 인터페이스를 표시하는 특정 [program](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/Glossary/Glossary.html#//apple_ref/doc/uid/10000057i-CH13-SW3) 유형.
* **condition**   리소스에 대한 접근을 동기화하는 데 사용되는 구조. 조건에 대기하고 있는 쓰레드는 다른 쓰레드가 명시적으로 조건을 표시할때까지 진행할 수 없다.
* **critical section**  

  한 번에 하나의 쓰레드만 실행해야 하는 코드 부분.

* **input source**  

  쓰레드의 비동기 이벤트 소스. 입력 소스는 포트 기반 또는 수동으로 트리거할 수 있으며 쓰레드의 런루프에 부착해야 한다.

* **joinable thread** 

  종료 즉시 리소스가 회수되지 않는 쓰레드. 결합 가능한 쓰레드는 리소스를 재확보하기 전에 명시적으로 분리하거나 다른 쓰레드로 연결해야 한다. 결합가능한 쓰레드는 결합된 쓰레드에 반환 값을 제공한다.

* **main thread**   자체 프로세스가 생성될 때 생성되는 특수 유형의 [thread](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/Glossary/Glossary.html#//apple_ref/doc/uid/10000057i-CH13-SW1)이다. 프로그램의 메인 쓰레드가 종료되면, 프로세스는 종료된다.
* **mutex**  

  공유 리소스에 대한 상호 배타적 접근을 제공하는 락. 뮤텍스 락은 한 번에 하나의 쓰레드만으로 고정할 수 있다. 다른 쓰레드에 의해 고정된 뮤텍스를 획득하려고 하면 락이 최종적으로 획득될 때까지 현재 쓰레드를 슬립 모드로 전환한다.

* **operation object**  

  [`NSOperation`](https://developer.apple.com/documentation/foundation/nsoperation)클래스의 인스턴스. 오퍼레이션 객체는 태스크와 관련된 코드와 데이터를 실행 가능한 단위로 래핑한다.

* **operation queue**  

  [`NSOperationQueue`](https://developer.apple.com/documentation/foundation/operationqueue)클래스의 인스턴스. 오퍼레이션 큐는 오퍼레이션 객체의 실행을 관리한다.

* **process**  

  애플리케이션 또는 프로그램의 런타임 인스턴스. 프로세스는 다른 프로그램에 할당된 것과 독립적인 고유한 가상 메모리 공간과 시스템 리소스\(포트 권한 포함\)를 가지고 있다. 프로세스는 항상 하나 이상의 쓰레드\(메인 쓰레드\)를 포함하며, 임의의 수의 추가 쓰레드를 포함할 수 있다.

* **program**  

  일부 태스크를 수행하기 위해 실행할 수 있는 코드와 리소스의 조합. 그래픽 애플리케이션도 프로그램으로 간주되지만 프로그램에는 그래픽 사용자 인터페이스가 필요하지 않다.

* **recursive lock**  

  동일한 쓰레드로 여러 번 잠글 수 있는 락.

* **run loop**  

  이벤트를 수신하고 적절한 처리기에 보내는 이벤트 처리 루프.

* **run loop mode**  

  특정 이름과 관련된 입력 소스, 타이머 소스 및 런 루프 옵저버의 집합이다. 특정 "mode"로 실행될 때, 런 루프는 그 모드와 관련된 소스와 관찰자만 모니터링한다.

* **run loop object**  

  [`NSRunLoop`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop)클래스 또는 [`CFRunLoopRef`](https://developer.apple.com/documentation/corefoundation/cfrunloopref)불투명 타입의 인스턴스. 이러한 객체는 쓰레드에서 이벤트 처리 루프를 구현하기 위한 인터페이스를 제공한다.

* **run loop observer**  

  런 루프의 여러 단계 중 알림 수신기.

* **semaphore**  

  공유 리소스에 대한 접근을 제한하는 보호된 변수. 뮤텍스와 컨디션은 모두 다른 형태의 세마포어다.

* **task**  

  수행해야 할 작업량.

* **thread**  

  프로세스의 실행 흐름. 각 쓰레드는 자체 스택 공간을 가지지만, 그렇지 않으면 동일한 프로세스에서 다른 쓰레드와 메모리를 공유한다.

* **timer source**  

  쓰레드의 동기식 이벤트 소스. 타이머는 예정된 미래 시간에 1번 또는 반복 이벤트를 생성한다.

