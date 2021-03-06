# Concurrency and Application Design

컴퓨팅 초기에는 컴퓨터가 수행할 수 있는 시간 단위당 최대 작업량을 CPU의 클럭 속도에 따라 결정하였다. 그러나 기술이 발전하고 프로세서 설계가 점점 더 복잡해지면서 열 및 기타 물리적 제약으로 인해 프로세서의 최대 클럭 속도를 제한되기 시작했다. 그래서 칩 제조업체들은 칩의 총 성능을 증가시키기 위한 다른 방법들을 모색했다. 그들이 정착시킨 솔루션은 각 칩의 프로세서 코어 수를 늘리는 것이였다. 코어 수를 늘림으로써, 단일 칩은 CPU 속도를 높이거나 칩 크기나 열적 특성을 바꾸지 않고도 초당 더 많은 명령을 실행할 수 있었다. 문제는 여분의 코어를 어떻게 활용하느냐뿐이였다.

여러 코어를 활용하기 위해서는 컴퓨터가 여러 가지 일을 동시에 할 수 있는 소프트웨어가 필요하다. OS X와 iOS와 같은 현대의 멀티태스킹 운영체제의 경우, 주어진 시간에 100개 이상의 프로그램이 실행될 수 있으므로, 다른 코어의 각 프로그램을 스케줄링할 수 있어야 한다. 그러나 이러한 프로그램의 대부분은 시스템 데몬 또는 백그라운드 애플리케이션으로 실제 처리 시간이 거의 소요되지 않는다. 대신, 정말로 필요한 것은 개별 애플리케이션이 여분의 코어를 더 효과적으로 사용할 수 있는 방법이다.

애플리케이션이 여러 코어를 사용하는 전통적인 방법은 여러 쓰레드를 생성하는 것이다. 그러나 코어 수가 증가함에 따라 쓰레드 솔루션에 문제가 생긴다. 가장 큰 문제는 쓰레드 코드가 임의의 코어 수로 잘 확장되지 않는다는 것이다. 코어만큼 쓰레드를 많이 만들 수 없으며 프로그램이 잘 실행될 것으로 기대할 수 없다. 우리가 알아야 할 것은 효율적으로 사용할 수 있는 코어의 수이다. 이는 애플리케이션이 스스로 계산하기 어렵다. 그럭저럭 숫자를 맞추더라도 그렇게 많은 쓰레드를 프로그래밍하고 효율적으로 작동시키고 서로 간섭하지 않게 하는 어려움은 여전히 남아 있다.

그래서 이 문제를 요약하자면, 애플리케이션이 다양한 수의 컴퓨터 코어를 활용할 수 있는 방법이 필요하다. 단일 애플리케이션에 의해 수행되는 작업의 양 또한 변화하는 시스템 조건에 맞게 동적으로 확장할 수 있어야 한다. 또한 이 솔루션은 이러한 코어들을 활용하는 데 필요한 작업량을 늘리지 않도록 충분히 단순해야 한다. 좋은 소식은 애플의 운영체제가 이러한 모든 문제에 대한 해결책을 제공한다는 것이다. 이 장은 이 솔루션을 구성하는 기술과 코드를 이용하여 만들 수 있는 설계 수정을 살펴본다.

## The Move Away from Threads

쓰레드는 여러 해 동안 존재해 왔고 계속해서 사용되었지만 확장 가능한 방식으로 여러 작업을 실행하는 일반적인 문제를 해결하지는 못한다. 쓰레드를 사용하면 확장 가능한 솔루션을 만들어야 하는 부담은 개발자에게 달려 있다. 시스템 상태가 변경됨에 따라 그 수를 동적으로 조정하고 생성할 쓰레드 수를 결정해야 한다. 또 다른 문제는 애플리케이션이 사용하는 쓰레드 생성 및 유지 보수와 관련된 대부분의 비용을 부담한다는 것이다.

OS X와 iOS는 쓰레드에 의존하는 대신, 동시성 문제를 해결하기 위해 비동기 설계 접근 방식을 채택한다. 비동기 함수는 수년 동안 운영체제에 존재해왔으며 디스크에서 데이터를 읽는 것과 같이 시간이 오래 걸릴 수 있는 작업을 시작하는데 자주 사용된다. 호출되면 비동기 함수는 작업 실행을 시작하지만 작업이 실제로 완료되기 전에 반환된다. 일반적으로 이 작업은 백그라운드 쓰레드를 획득하고, 해당 쓰레드에서 원하는 작업을 시작한 다음, 작업이 완료되면 \(일반적으로 콜백 기능을 통해\) 호출자에게 알림을 보내는 것을 포함한다. 과거에는 원하는 작업에 대해 비동기 함수가 존재하지 않는 경우 자신만의 비동기 함수를 작성하고 자신만의 쓰레드를 생성해야 했다. 그러나 이제 OS X와 iOS는 쓰레드를 직접 관리하지 않고도 비동기적으로 작업을 수행할 수 있는 기술을 제공한다.

작업을 비동기적으로 시작하는 기술 중 하나는 _GCD\(Grand Central Dispatch\)_이다. 이 기술은 일반적으로 자신의 애플리케이션에서 작성하는 쓰레드 관리 코드를 가져와 시스템 수준으로 이동시킨다. 실행할 태스크를 정의하고 적절한 디스패치 큐에 추가하기만 하면 된다. GCD는 필요한 쓰레드를 생성하고 해당 쓰레드에서 작업을 실행하도록 예약하는 작업을 처리한다. 쓰레드 관리는 현재 시스템의 일부이기 때문에 GCD는 작업 관리와 실행에 대한 전체적인 접근 방식을 제공하여 전통적인 쓰레드보다 더 나은 효율성을 제공한다.

_오퍼레이션 큐_는 디스패치 큐과 매우 유사한 역할을 하는 Objective-C 객체이다. 실행할 작업을 정의한 다음 작업 큐에 추가하여 해당 작업의 스케줄링 및 실행을 처리한다. GCD와 마찬가지로 오퍼레이션 큐는 모든 쓰레드 관리를 처리하여 시스템에서 가능한 한 빠르고 효율적으로 작업이 실행되도록 한다.

다음 섹션에서는 응용 프로그램에서 사용할 수 있는 디스패치 큐, 오퍼레이션 큐 및 기타 관련 비동기 기술에 대한 자세한 정보를 제공한다.

### Dispatch Queues

디스패치 큐는 사용자 정의 태스크를 실행하기 위한 C-기반 메커니즘이다. _디스패치 큐_는 작업을 연속적으로 또는 동시에 실행하지만 항상 FIFO 순서에 따라 실행한다. \(즉, 디스패치 큐는 항상 대기열에 추가된 것과 동일한 순서로 작업을 디큐잉하고 시작한다.\) 시리얼 디스패치 큐는 한 번에 하나의 작업만 실행되며, 작업이 완료될 때까지 대기한 후 새 작업을 시작한다. 반면 콘커런트 디스패치 큐는 이미 시작된 작업이 완료될 때까지 기다리지 않고 가능한 많은 작업을 시작한다.

디스패치 큐는 그 밖의 이점이 있다:

* 직관적이고 간단한 프로그래밍 인터페이스를 제공한다.
* 자동적이고 전체적인 스레드 풀 관리를 제공한다.
* 튜닝된 어셈블리 속도를 제공한다.
* 훨씬 더 메모리 효율적이다. \(스레드 스택이 애플리케이션 메모리에 남아있지 않기 때문이다.\)
* 부하가 걸린 상태에서 커널에 트랩을 놓지 않는다.
* 비동기적으로 태스크를 디스패치 큐에 가져오는 것은 큐를 교착시킬 수 없다.
* 논쟁 중에 우아하게 규모를 조정한다.
* 시리얼 디스패치 큐는 락 및 기타 동기화 기본 요소들에 대한 보다 효율적인 대안을 제공한다.

디스패치 큐에 제출하는 작업은 함수 또는 블록 객체 내부에 캡슐화되어야 한다. _블록 객체_는 개념적으로 기능 포인터와 유사하지만 몇 가지 추가적인 이점을 가진 OS X v10.6 및 iOS 4.0에 도입된 C 언어 기능이다. 블록을 자체 어휘 범위로 정의하는 대신 일반적으로 다른 함수나 메소드 내에서 블록을 정의하여 해당 함수나 메서드의 다른 변수에 접근할 수 있다. 블록을 원래 범위에서 벗어나 힙에 복사할 수도 있는데, 이는 블록을 디스패치 대기열에 제출할 때 발생하는 현상이다. 이 모든 의미론들은 비교적 적은 코드로 매우 동적인 작업을 수행할 수 있게 해준다.

디스패치 큐는 Grand Central Dispatch 기술의 일부분이며 C 런타임의 일부분이다. 애플리케이션에서 디스패치 큐 사용에 대한 자세한 내용은 [Dispatch Queues](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW1)를 참조하라. 블록 및 해당 이점에 대한 자세한 내용은 [_Blocks Programming Topics_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Blocks/Articles/00_Introduction.html#//apple_ref/doc/uid/TP40007502)를 참조하라.

### Dispatch Sources

디스패치 소스는 특정 유형의 시스템 이벤트를 비동기적으로 처리하기 위한 C-기반 메커니즘이다. 디스패치 소스는 특정 유형의 시스템 이벤트에 대한 정보를 캡슐화하고 해당 이벤트가 발생할 때마다 특정 블록 객체 또는 함수를 디스패치 큐에 제출한다. 다음 유형의 시스템 이벤트를 모니터링하기 위해 디스패치 소스를 사용할 수 있다.

* 타이머
* 신호 핸들러
* 설명자 관련 이벤트
* 프로세스 관련 이벤트
* Mach 포트 이벤트
* 트리거하는 사용자 정의 이벤트

디스패치 소스는 Grand Central Dispatch 기술의 일부이다. 애플리케이션에서 이벤트를 수신하기 위해 디스패치 소스를 사용하는 방법에 대한 자세한 내용은 [Dispatch Sources](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html#//apple_ref/doc/uid/TP40008091-CH103-SW1)를 참조하라.

### Operation Queues

오퍼레이션 큐는 Cocoa의 콘커런트 큐와 동등하며 [`NSOperationQueue`](https://developer.apple.com/documentation/foundation/operationqueue) 클래스에 의해 구현된다. 디스패치 큐는 항상 FIFO 순서로 작업을 실행하는 반면, 오퍼레이션 큐는 작업의 실행 순서를 결정할 때 다른 요소를 고려한다. 이러한 요인 중 가장 중요한 것은 주어진 작업이 다른 작업의 완료에 따라 달라지는가 하는 것이다. 작업을 정의할 때 종속성을 구성하고 작업에 대해 복잡한 실행 순서 그래프를 생성하는 데 사용할 수 있다.

오퍼레이션 큐에 제출하는 작업은 [`NSOperation`](https://developer.apple.com/documentation/foundation/nsoperation) 클래스의 인스턴스여야 한다. _오퍼레이션 객체_는 수행하고자 하는 작업과 이를 수행하는 데 필요한 데이터를 캡슐화하는 Objective-C 객체이다. `NSOperation` 클래스는 기본적으로 추상 기본 클래스이므로 일반적으로 작업을 수행할 사용자 정의 하위 클래스를 정의하라. 그러나 Foundation 프레임워크에는 작업을 수행하는 것과 마찬가지로 작성하고 사용할 수 있는 일부 구체적인 하위 클래스가 포함되어 있다.

오퍼레이션 객체는 키-값 관찰\(KVO\) 알림을 생성하며, 이는 작업 진행률을 모니터링하는 유용한 방법이 될 수 있다. 오퍼레이션 큐는 항상 동시에 작업을 실행하지만, 종속성을 사용하여 필요할 때 연속적으로 실행되도록 할 수 있다.

오퍼레이션 큐를 사용하는 방법과 사용자 정의 오퍼레이션 객체를 정의하는 방법에 대한 자세한 내용은 [Operation Queues](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW1)를 참조하라.

## Asynchronous Design Techniques

동시성을 지원하기 위해 코드를 재설계하는 것을 고려하기 전에, 그렇게 하는 것이 필요한지 자문해 보아야 한다. 동시성은 메인 쓰레드가 사용자 이벤트에 자유롭게 응답할 수 있도록 함으로써 코드의 응답성을 향상시킬 수 있다. 그것은 동일한 시간 내에 더 많은 작업을 수행하기 위해 더 많은 코어를 활용함으로써 코드의 효율성을 향상시킬 수 있다. 그러나 그것은 또한 오버헤드를 증가시키고 코드의 전반적인 복잡성을 증가시켜 코드의 쓰기 및 디버그를 어렵게 만든다.

복잡성을 가중시키기 때문에 동시성은 제품 주기가 끝날 때 애플리케이션에 접목할 수 있는 기능이 아니다. 올바르게 실행하려면 애플리케이션이 수행하는 작업과 이러한 작업을 수행하는 데 사용되는 데이터 구조를 신중하게 고려해야 한다. 잘못 수행하면 코드가 이전보다 느리게 실행되고 사용자에게 응답하지 않을 수 있다. 따라서 설계 주기가 시작될 때 시간을 두고 몇 가지 목표를 설정하고 취해야 할 접근법에 대해 생각해 보는 것은 가치 있는 일이다.

모든 애플리케이션은 다른 요구사항과 다른 작업 집합을 가지고 있다. 문서가 애플리케이션 및 관련 작업을 설계하는 방법을 정확하게 알려주는 것은 불가능하다. 그러나 다음 절에서는 설계 프로세스 중에 좋은 선택을 할 수 있도록 몇 가지 가이드라인을 제공하려고 한다.

### Define Your Application’s Expected Behavior

애플리케이션에 동시성을 추가하는 것을 고려하기 전에 항상 애플리케이션의 올바른 동작으로 간주되는 것을 정의하는 것부터 시작해야 한다. 애플리케이션의 예상 동작을 이해하면 나중에 설계를 검증할 수 있다. 또한 동시성을 도입함으로써 얻을 수 있는 기대 성능 이점을 어느 정도 알 수 있어야 한다.

가장 먼저 해야 할 일은 애플리케이션이 수행하는 작업과 각 작업과 관련된 객체 또는 데이터 구조를 열거하는 것이다. 처음에는 사용자가 메뉴 항목을 선택하거나 버튼을 클릭할 때 수행되는 작업으로 시작하길 원할 것이다. 이러한 작업은 별개의 동작을 제공하며 시작점과 끝점이 잘 정의되어 있다. 또한 타이머 기반 작업과 같이 사용자 상호 작용 없이 애플리케이션이 수행할 수 있는 다른 유형의 작업도 열거하라.

고 수준의 작업 목록을 확보한 후 각 작업을 성공적으로 완료하기 위해 수행해야 하는 단계 집합으로 세분화하라. 이 수준에서는 데이터 구조와 객체를 수정하는데 필요한 변경 사항과 이러한 수정 사항이 애플리케이션의 전체 상태에 어떤 영향을 미치는지에 대해 우선적으로 관심을 가져야 한다. 또한 객체와 데이터 구조 간의 종속성에도 유의하라. 예를 들어, 한 작업에서 일련의 객체에 대해 동일한 변경을 수행하는 경우, 한 객체에 대한 변경이 다른 객체에 영향을 미치는지 여부를 주목할 필요가 있다. 만약 객체가 서로 독립적으로 수정될 수 있다면, 그것은 동시에 수정될 수 있는 장소일 것이다.

### Factor Out Executable Units of Work

애플리케이션의 작업에 대한 이해로, 당신은 이미 당신의 코드가 동시성으로 이익을 얻을 수 있는 장소를 식별할 수 있어야 한다. 작업에서 하나 이상의 단계 순서를 변경하면 결과가 변경되는 경우 해당 단계를 연속적으로 계속 수행해야 할 수 있다. 그러나 순서를 변경해도 출력에 영향을 미치지 않는 경우 이러한 단계를 동시에 수행하는 것을 고려해야 한다. 두 경우 모두 수행할 단계 또는 단계를 나타내는 실행 가능한 작업 단위를 정의하라. 그러면 이 작업 단위는 블록 또는 작업 객체를 사용하여 캡슐화하고 적절한 대기열로 전송하는 것이 된다.

식별하는 각 실행 가능한 작업 단위에 대해, 적어도 초기에는 수행되는 작업의 양에 대해 걱정할 필요가 없다. 쓰레드를 회전시키는 데 항상 비용이 들지만, 디스패치 큐와 오퍼레이션 큐의 장점 중 하나는 대다수의 경우 그러한 비용이 기존 쓰레드에 비해 훨씬 적다는 것이다. 따라서, 쓰레드를 사용할 때보다 큐를 사용하여 더 적은 양의 작업을 더 효율적으로 실행할 수 있다. 물론, 항상 실제 성과를 측정하고 필요에 따라 작업의 크기를 조정해야 하지만, 처음에는 어떤 작업도 작은 것으로 간주해서는 안된다.

### Identify the Queues You Need

이제 작업 단위가 구분되어 블록 객체 또는 작업 객체를 사용하여 캡슐화되므로 해당 코드를 실행하는 데 사용할 큐를 정의하라. 지정된 작업의 경우 생성한 블록 또는 작업 객체와 작업을 올바르게 수행하기 위해 객체를 실행해야 하는 순서를 검사하라.

블록을 사용하여 작업을 구현한 경우, 블록을 시리얼 또는 컨커런트 큐에 추가할 수 있다. 특정 주문이 필요한 경우, 항상 블록을 시리얼 디스패치 큐에 추가하라. 특정 주문이 필요하지 않은 경우 필요에 따라 블록을 컨커런트 디스패치 큐에 추가하거나 여러 다른 디스패치 큐에 추가할 수 있다.

작업 객체를 사용하여 작업을 구현한 경우 큐의 선택이 객체의 구성보다 덜 흥미로울 수 있다. 작업 객체를 연속적으로 수행하려면 관련 객체 간에 종속성을 구성해야 한다. 종속성은 한 작업이 종속된 객체가 작업을 마칠 때까지 실행되지 않도록 한다.

### Tips for Improving Efficiency

코드를 더 작은 작업으로 간주하여 큐에 추가하는 것 외에도 큐를 사용하여 코드의 전체 효율성을 향상시키는 다른 방법이 있다.

* **메모리 사용량이 요인인 경우 작업 내에서 직접 값을 계산하는 것을 고려하라.** 애플리케이션이 이미 메모리 바인딩되어 있는 경우, 지금 바로 값을 계산하는 것이 메인 메모리에서 캐시된 값을 로드하는 것보다 더 빠를 수 있다. 연산 값은 메인 메모리보다 훨씬 빠른 지정된 프로세서 코어의 레지스터와 캐시를 직접 사용한다. 물론, 테스트 결과 이것이 성능상으로 우위일 때 이렇게 해야 한다.
* **시리얼 작업을 조기에 식별하고 동시 작업이 더 많이 수행할 수 있는 작업을 수행하라.** 태스크가 일부 공유 리소스에 의존하기 때문에 태스크를 연속적으로 실행해야 하는 경우, 해당 공유 리소스를 제거하기 위해 아키텍처를 변경하는 것을 고려하라. 필요한 각 클라이언트에 대한 리소스 복사본을 생성하거나 리소스를 모두 제거하는 것을 고려할 수 있다.
* **락을 사용하지 않아야 한다.** 디스패치 큐와 오퍼레이션 큐에서 제공하는 지원은 대부분의 상황에서 락을 불필요하게 만든다. 일부 공유 리소스를 보호하기 위해 락을 사용하는 대신 시리얼 큐를 지정하거나 작업 객체 종속성을 사용하여 작업을 올바른 순서로 실행하라.
* **가능하면 시스템 프레임워크에 의존하라.** 동시성을 달성하는 가장 좋은 방법은 시스템 프레임워크가 제공하는 내장된 동시성을 이용하는 것이다. 많은 프레임워크는 내부적으로 쓰레드 및 기타 기술을 사용하여 동시 동작을 구현한다. 작업을 정의할 때 기존 프레임워크에서 원하는 작업을 정확하게 수행하는 함수 또는 메서드를 정의하는지 확인하라. API를 사용하면 노력을 절약할 수 있으며 가능한 최대 동시성을 제공할 가능성이 더 높다.

## Performance Implications

오퍼레이션 큐, 디스패치 큐 및 디스패치 소스가 제공되어 더 많은 코드를 동시에 쉽게 실행할 수 있다. 그러나 이러한 기술이 애플리케이션의 효율성이나 응답성 향상을 보장하지는 않는다. 큐를 사용자 요구에 효과적이고 애플리케이션의 다른 리소스에 과도한 부담을 주지 않은 방식으로 사용하는 것은 여전히 사용자의 책임이다. 예를 들어, 10, 000개의 작업 객체를 생성하여 오퍼레이션 큐에 제출할 수 있지만, 그렇게 하면 애플리케이션이 잠재적으로 비독점적인 양의 메모리를 할당하여 페이징 및 성능 저하를 초래할 수 있다.

큐나 쓰레드를 사용하든 코드에 어느 정도의 동시성을 도입하기 전에 항상 애플리케이션의 현재 성능을 반영하는 기본 메트릭 셋을 수집해야 한다. 변경 사항을 도입한 후에는 추가 메트릭을 수집하여 기준과 비교하여 애플리케이션의 전반적인 효율성이 향상되었는지 확인해야 한다. 동시성의 도입으로 애플리케이션의 효율성이나 응답성이 저하되는 경우 사용 가능한 성능 도구를 사용하여 잠재적인 원인을 확인하라.

성능 및 사용가능한 성능 도구에 대한 소개 및 고급 성능 관련 주제에 대한 링크는 [_Performance Overview_](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/PerformanceOverview/Introduction/Introduction.html#//apple_ref/doc/uid/TP40001410)를 참조하라.

## Concurrency and Other Technologies

코드를 모듈형 작업으로 변환하는 것이 애플리케이션의 동시성을 향상시키기 위한 최선의 방법이다. 그러나 이러한 설계 접근방식은 모든 경우에 모든 애플리케이션의 요구를 충족시키지 못할 수 있다. 작업에 따라 애플리케이션 전체 동시성을 추가로 개선할 수 있는 다른 옵션이 있을 수 있다. 이 절에서는 설계의 일부로 사용할 수 있는 다른 기술의 개요를 설명한다.

### OpenCL and Concurrency

OS X에서 _Open Computing Language\(OpenCL\)_는 컴퓨터의 그래픽 프로세서에서 범용 계산을 수행하기 위한 표준 기반 기술이다. OpenCL은 대규모 데이터 셋에 적용할 계산 집합이 잘 정의되어 있는 경우 사용하기에 좋은 기술이다. 예를 들어 OpenCL을 사용하여 이미지의 픽셀에서 필터 계산을 수행하거나 여러 값에 대해 복잡한 수학 계산을 한 번에 수행할 수 있다. 즉, OpenCL은 데이터를 병렬로 작동할 수 있는 문제 집합에 더 적합하다.

OpenCL은 대규모 데이터 병렬 연산을 수행하기에 좋지만 좀 더 범용적인 계산에는 적합하지 않다. GPU에 의해 작동될 수 있도록 데이터와 필요한 작업 커널을 모두 준비하고 그래픽 카드로 전송하는 데 필요한 많은 노력이 필요하다. 마찬가지로 OpenCL에서 생성된 결과를 검색하는 데 필요한 비사소한 노력이 있다. 결과적으로 시스템과 상호 작용하는 작업은 일반적으로 OpenCL과 함께 사용하지 않는 것이 좋다. 예를 들어, OpenCL을 사용하여 파일 또는 네트워크 스트림의 데이터를 처리하지 않는다. 대신, OpenCL을 사용하여 수행하는 작업은 그래픽 프로세서로 전송되고 독립적으로 계산될 수 있도록 훨씬 더 독립적으로 처리되어야 한다.

OpenCL과 사용 방법에 대한 자세한 내용은 [_OpenCL Programming Guide for Mac_](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/OpenCL_MacProgGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008312)을 참조하라.

### When to Use Threads

오퍼레이션 큐와 디스패치 큐가 동시에 작업을 수행하는 데 선호되는 방식이지만 만병통치약은 아니다. 애플리케이션에 따라 사용자 정의 쓰레드를 생성해야 하는 경우가 있을 수 있다. 만약 사용자 정의 쓰레드를 생성하는 경우, 가능한 한 적은 수의 쓰레드를 만들기 위해 노력해야 하며, 다른 방법으로 구현할 수 없는 특정 작업에만 해당 쓰레드를 사용해야 한다.

쓰레드는 여전히 실시간으로 실행되어야 하는 코드를 구현하는 좋은 방법이다. 디스패치 큐는 가능한 한 빨리 작업을 실행하려는 모든 시도를 하지만 실시간 제약을 해결하지는 않는다. 백그라운드에서 실행되는 코드에서 보다 예측 가능한 동작이 필요한 경우, 쓰레드는 여전히 더 나은 대안을 제공할 수 있다.

다른 쓰레드 프로그래밍과 마찬가지로, 반드시 필요할 때만 쓰레드를 신중하게 사용해야 한다. 쓰레드 패키지 및 쓰레드 사용 방법에 대한 자세한 내용은 [_Threading Programming Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/Introduction/Introduction.html#//apple_ref/doc/uid/10000057i)를 참조하라.

