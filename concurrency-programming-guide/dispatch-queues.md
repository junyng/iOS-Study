# Dispatch Queues

GCD\(Grand Central Dispatch\) 디스패치 큐는 작업을 수행하기 위한 강력한 도구이다. 디스패치 큐를 통해 발신자와 관련하여 비동기 또는 동기식으로 임의의 코드 블록을 실행할 수 있다. 디스패치 큐를 사용하여 별도의 쓰레드에서 수행하는 데 사용한 거의 모든 태스크를 수행할 수 있다. 디스패치 큐의 장점은 해당 쓰레드 코드보다 사용이 간편하고 태스크를 실행하는 데 훨씬 효율적이라는 것이다.

이 장에서는 애플리케이션에서 일반 태스크를 실행하는 데 사용하는 방법에 대한 정보와 함께 디스패치 큐에 대한 소개를 제공한다. 기존의 쓰레드 코드를 디스패치 큐로 바꾸려면 Migrating Away from Threads을 통해 쓰레드 코드 사용 방법에 대한 몇가지 추가적인 팁을 참조하라.

## About Dispatch Queues

디스패치 큐는 애플리케이션에서 작업을 비동기적으로 동시에 수행하는 쉬운 방법이다. 태스크는 단순히 당신의 애플리케이션이 수행해야 하는 일부 작업이다. 예를 들어, 어떤 계산을 수행하거나, 데이터 구조를 생성 또는 수정하거나, 파일에서 읽은 일부 데이터를 처리하거나, 또는 여러가지 작업을 처리하는 태스크를 정의할 수 있다. 함수 또는 블록 객체 내부에 해당 코드를 배치하고 디스패치 큐에 추가하여 태스크를 정의한다.

디스패치 큐는 제출하는 태스크를 관리하는 object-like 구조이다. 모든 디스패치 큐는 선입선출 데이터 구조이다. 따라서 큐에 추가하는 태스크는 항상 추가된 순서대로 시작된다. GCD는 자동으로 일부 디스패치 큐를 제공하지만 특정 목적을 위해 생성할 수 있는 다른 디스패치 큐도 제공한다. Table 3-1에는 애플리케이션에 사용할 수 있는 디스패치 큐의 유형 및 사용 방법이 나와 있다.

**Table 3-1** Types of dispatch queues

| Type | Description |
| :--- | :--- |
| Serial | 시리얼 큐\(private 디스패치 큐라고도 함\)는 큐에 추가된 순서대로 한 번에 하나의 태스크를 실행한다. 현재 실행 중인 태스크는 디스패치 큐에 의해 관리되는 별개의 쓰레드\(태스크 마다 다를 수 있음\)에서 실행된다. 시리얼 큐는 종종 특정 리소스에 대한 액세스를 동기화하는 데 사용 된다. 필요한 만큼 많은 시리얼 큐를 생성할 수 있으며, 각 큐는 다른 모든 큐와 동시에 작동한다. 즉, 4개의 시리얼 큐를 생성하면 각 큐는 한 번에 하나의 작업만 실행하지만 각 큐에서 최대 4개의 작업이 동시에 실행될 수 있다. 시리얼 큐를 생성하는 방법에 대한 자세한 내용은 Creating Serial Dispatch Queues를 참조하라. |
| Concurrent | 컨커런트 큐\(글로벌 디스패치 큐라고도 함\)는 하나 이상의 태스크를 동시에 실행하지만 태스크는 대기열에 추가된 순서대로 여전히 시작된다. 현재 실행 중인 태스크는 디스패치 큐에 의해 관리되는 별개의 쓰레드에서 실행된다. 특정 지점에서 실행되는 태스크의 정확한 수는 가변적이며 시스템 조건에 따라 달라진다. iOS 5 이상에서는 `DISPATCH_QUEUE_CONCURRENT` 를 큐 유형으로 지정하여 컨커런트 큐를 직접 생성할 수 있다. 또한 애플리케이션이 사용할 미리 정의된 글로벌 컨커런트 큐 4개가 있다. 글로벌 컨커런트 큐를 가져오는 방법에 대한 자세한 내용은 Getting the Global Concurrent Dispatch Queues를 참조하라. |
| Main dispatch queue | 메인 디스패치 큐는 메인 쓰레드 태스크를 실행하고 있는 시리얼 큐를 전역으로 사용 가능합니다. 이 큐는 애플리케이션의 런 루프\(있는 경우\)와 함께 작동하여 대기 중인 태스크의 실행을 런 루프에 연결된 다른 이벤트 소스의 실행과 상호 작용한다. 애플리케이션의 메인 쓰레드에서 실행된 큐는 종종 애플리케이션에 대한 주요 동기점으로 사용된다. 메인 디스패치 큐를 생성하지 않아도 애플리케이션이 그것을 적절하게 제거하는지 확인할 필요가 있다. 이 큐가 어떻게 관리되는지에 대한 자세한 내용은 Performing Tasks on the Main Thread를 참조하라. |

애플리케이션에 동시성을 추가하는 것에 관해 디스패치 큐 쓰레드에 대한 여럿의 이점을 제공한다. work-queue 프로그래밍 모델의 가장 직접적인 장점은 간단함이다. 당신은 수행을 원하는 쓰레드의 생성과 쓰레드를 관리하는 작업에 대한 코드를 작성해야한다. 디스패치 큐는 실제로 쓰레드 생성과 관리 수행에 걱정 없이 작업에 집중하게 해준다. 대신, 그 시스템이 모든 쓰레드의 생성 및 관리 전반을 담당한다. 시스템의 장점은 단일 애플리케이션보다 쓰레드를 더 효율적으로 관리할 수 있다. 그 시스템은 사용 가능한 자원과 현재 시스템 조건에 기초를 둔 쓰레드의 수를 동적으로 확장할 수 있다. 게다가, 시스템은 보통 직접 쓰레드를 만들었을 때보다 더 빨리 당신의 태스크를 실행할 수 있다.

디스패치 큐를 위해 코드를 재작성하는 것이 어려울것이라고 생각 할 수 있지만, 쓰레드에 코드를 작성하는 것보다 디스패치 큐의 코드를 작성하는 것이 더 쉬운 경우가 많다. 코드를 작성하는 데 있어 핵심은 비동기적으로 실행할 수 있는 태스크를 설계하는 것이다. \(이것은 쓰레드와 디스패치 큐 모두 해당한다.\) 그러나 디스패치 큐가 예측 가능성에 있어 유리하다. 동일한 공유 리소스에 액세스하지만 서로 다른 쓰레드에서 실행되는 두 태스크가 있는 경우 두 쓰레드는 먼저 리소스를 수정할 수 있으며 두 태스크가 동시에 해당 리소스를 수정하지 않도록 락을 사용해야 한다. 디스패치 큐를 사용하면 두 태스크를 시리얼 디스패치 큐에 추가하여 지정된 시간에 한 태스크만 리소스를 수정하도록 할 수 있다. 이 유형의 큐 기반 동기화는 락 보다 더 효율적이다. 락은 항상 경쟁 비경쟁상태 모두 값비싼 커널 트랩이 필요하기 때문이다. 디스패치 큐는 주로 애플리케이션 프로세스 공간에서 작동하며 절대적으로 필요한 경우에만 커널로 호출한다.

시리얼 큐에서 실행되는 두 개의 태스크가 동시에 실행되지 않는다는 점을 지적하는 것이 옳더라도 두 개의 쓰레드가 동시에 잠기면 쓰레드가 제공하는 동시성이 상실되거나 현저히 감소한다는 점을 기억해야 한다. 더 중요한 것은, 쓰레드 모델은 커널과 사용자-공간 메모리를 모두 차지하는 두 개의 쓰레드를 생성해야 한다는 것이다. 디스패치 큐는 쓰레드에 대해 동일한 메모리 패널티를 지불하지 않으며, 그들이 사용하는 쓰레드는 사용량이 많고 차단되지 않는다.

디스패치 큐에 대해 기억해야 할 다른 주요 사항에는 다음이 포함된다.

* 디스패치 큐는 다른 디스패치 큐와 동시에 태스크를 수행한다. 일련의 태스크는 단일 디스패치 큐에 있는 태스크로 제한된다.
* 시스템은 한 번에 실행하는 총 태스크 수를 결정한다. 따라서 100개의 다른 큐에서 100개의 태스크를 가진 애플리케이션은 모든 태스크를 동시에 실행하지 못할 수 있다. \(100개 이상의 유효 코어를 가지고 있지 않는 한\)
* 시스템은 시작할 새 태스크를 선택할 때 큐 우선 순위를 고려한다. 시리얼 큐의 우선 순위를 설정하는 방법에 대한 자세한 내용은 Providing a Clean Up Function For a Queue를 참조하라.
* 큐의 태스크는 큐에 추가될 때 실행할 준비가 되어 있어야 한다. \(이전에 Cocoa 오퍼레이션 객체를 사용한 적이 있는 경우 이 동작은 모델 오퍼레이션 사용과 다르다는 점에 유의하라.\)
* Private 디스패치 큐는 참조 카운트 객체이다. 코드로 큐를 유지하는 것 외에도 디스패치 소스가 큐에 부착될 수 있고 리테인 카운트를 증가시킬 수 있다는 점에 유의하라. 따라서 모든 디스패치 소스가 취소되고 모든 릴리스 호출로 리테인 호출이 균형을 이루도록 해야 한다. 큐 리테인 및 릴리스에 대한 자세한 내용은 Memory Management for Dispatch Queues를 참조하라. 디스패치 소스에 대한 자세한 내용은 About Dispatch Sources를 참조하라.

디스패치 큐를 조작하는 데 사용하는 인터페이스에 대한 자세한 내용은 GCD\(Grand Central Dispatch\) 를 참조하라.

## Queue-Related Technologies

Grand Central Dispatch는 디스패치 큐 외에도 큐를 사용하여 코드를 관리하는 데 도움이 되는 몇 가지 기술을 제공한다. Table 3-2는 이러한 기술들을 나열하고 당신이 그것에 대해 더 많은 정보를 찾을 수 있는 곳에 대한 링크를 제공한다.

**Table 3-2** Technologies that use dispatch queues

| **Technology** | **Description** |
| :--- | :--- |
| Dispatch groups | 디스패치 그룹은 블록 객체 셋을 모니터링하여 완료하는 방법이다. \(필요에 따라 블록을 동기식 또는 비동기식으로 모니터링할 수 있다.\) 그룹은 다른 태스크의 완료에 따라 달라지는 코드에 대한 유용한 동기화 메커니즘을 제공한다. 그룹 사용에 대한 자세한 정보는 Waiting on Groups of Queued Tasks를 참조하라. |
| Dispatch semaphores | 디스패치 세마포어는 전통적인 세마포어와 비슷하지만 일반적으로 더 효율적이다. 디스패치 세마포어는 세마포어를 사용할 수 없기 때문에 호출 쓰레드를 차단해야 할 경우에만 커널로 호출한다. 세마포어를 사용할 수 있으면 커널 콜이 이루어지지 않는다. 디스패치 세마포어에 대한 사용 방법에 대한 예는 [Using Dispatch Semaphores to Regulate the Use of Finite Resources](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW24) 를 참조하라. |
| Dispatch sources | 디스패치 소스는 특정 유형의 시스템 이벤트에 대응하여 알림을 생성한다. 디스패치 소스를 사용하여 프로세스 알림, 신호 및 설명자 이벤트와 같은 이벤트를 모니터링할 수 있다. 이벤트가 발생하면, 디스패치 소스는 처리를 위해 지정된 디스패치 큐에 작업 코드를 비동기적으로 제출한다. 디스패치에 대한 자세한 정보는 [Dispatch Sources](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html#//apple_ref/doc/uid/TP40008091-CH103-SW1) 를 참조하라. |

### Implementing Tasks Using Blocks

블록 객체는 C, Objective-C 및 C++ 코드에 사용할 수 있는 C언어 기반 기능이다. 블록을 통해 자기 포함 작업 단위를 쉽게 정의할 수 있다. 비록 그것들이 함수 포인터와 비슷하게 보일지 모르지만, 블록은 실제로 객체를 닮은 기본 데이터 구조로 표현되며, 컴파일러에 의해 생성되고 관리된다. 컴파일러는 당신이 제공하는 코드를 패키징하고\(관련 데이터와 함께\) 힙에 살면서 당신의 애플리케이션을 통과할 수 있는 형태로 캡슐화한다.

블록의 주요 장점 중 하나는 자체 어휘 범위에서 외부 변수를 사용할 수 있다는 것이다. 함수나 메서드 내부의 블록을 정의할 때, 블록은 어떤 면에서 전통적인 코드 블록으로 작용한다. 예를 들어, 블록은 상위 범위에 정의된 변수 값을 읽을 수 있다. 블록이 접근하는 변수는 나중에 블록이 접근할 수 있도록 힙의 블록 데이터 구조에 복사된다. 블록이 디스패치 큐에 추가되면 이러한 값은 일반적으로 읽기 전용 형식으로 남겨야 한다. 그러나 동시에 실행되는 블록은 데이터를 상위 호출 범위에 되돌리기 위해 `__block` 키워드를 미리 사용하는 변수를 사용할 수도 있다.

함수 포인터에 사용되는 구문과 유사한 구문을 사용하여 코드에 따라 블록을 인라인으로 선언할 수 있다. 블록과 함수 포인터의 주요 차이점은 블록 이름에 별표\(`*`\) 대신 캐럿\(`^`\)이 선행된다는 것이다. 함수 포인터처럼 블록에 인수를 전달하고 그로부터 반환값을 받을 수 있다. Listing 3-1은 당신의 코드에 블록을 동시에 선언하고 실행하는 방법을 보여준다. 변수 `aBlock` 은 블록에 선언된 값을 반환하는 단일 정수 매개 변수 인자이고 아무 값도 반환하지 않는다. 프로토타입과 일치하는 실제 블록이 `aBlock` 에 할당되고 인라인으로 선언된다. 마지막 줄은 블록을 즉시 실행하고 표준화할 지정된 정수를 출력한다.

**Listing 3-1**  A simple block example

```objectivec
int x = 123;
int y = 456;
 
// Block declaration and assignment
void (^aBlock)(int) = ^(int z) {
    printf("%d %d %d\n", x, y, z);
};
 
// Execute the block
aBlock(789);   // prints: 123 456 789
```

다음은 블록 설계 시 고려해야 할 주요 가이드라인의 요약이다.

* 디스패치 큐를 사용하여 비동기적으로 수행할 계획인 블록의 경우 상위 함수 또는 메서드에서 스칼라 변수를 캡처하고 블록에서 사용할 수 있다. 그러나 컨텍스트 호출에 의해 할당되고 삭제되는 큰 구조물이나 다른 포인터 기반 변수를 캡처하려고 해서는 안된다. 블록이 실행되면 해당 포인터가 참조하는 메모리가 사라질 수 있다. 물론 직접 메모리\(또는 객체\)를 할당하고 그 메모리 소유권을 블록에 명시적으로 넘기는 것이 안전하다.
* 디스패치 큐는 추가된 블록을 복사하고 실행을 마치면 블록을 해제한다. 즉, 블록을 큐에 추가하기 전에 블록을 명시적으로 복사할 필요가 없다.
* 비록 큐가 작은 작업을 실행할 때 원시 쓰레드보다 더 효율적이기는 하지만, 블록을 만들고 큐에서 실행하는 데는 여전히 오버헤드가 있다. 블록이 너무 작은 일을 하면, 큐에 보내는 것보다 인라인으로 실행하는 것이 더 적게 소모될 수 있다. 블록이 너무 적은 작업을 수행하는지 확인하는 방법은 성능 도구를 사용하여 각 경로에 대한 메트릭스를 수집하고 비교하는 것이다.
* 기본 쓰레드를 기준으로 데이터를 캐시하지 않고 다른 블록에서 데이터를 접근할 수 있기를 기대하라. 동일한 큐의 태스크가 데이터를 공유해야 하는 경우, 디스패치 큐의 컨텍스트 포인터를 사용하여 데이터를 대신 저장한다. 디스패치 큐의 컨텍스트 데이터에 접근하는 방법에 대한 자세한 내용은 [Storing Custom Context Information with a Queue](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW13) 를 참조하라.
* 블록이 몇 개 이상의 Objective-C 객체를 생성하는 경우, 블록 코드의 일부를 @autorelease 블록에 포함하여 해당 객체의 메모리 관리를 처리할 수 있다. GCD 디스패치 큐는 자체적인 오토릴리즈 풀이 있지만, 풀이 언제 drain되는지 보증하지 않는다. 애플리케이션이 메모리 제한을 받는 경우, 오토릴리즈 풀을 생성하면 자동 해제된 객체의 메모리를 더 정기적으로 확보할 수 있다.

블록을 선언하고 사용하는 방법과 블록에 대한 정보는 [_Blocks Programming Topics_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Blocks/Articles/00_Introduction.html#//apple_ref/doc/uid/TP40007502) __를 참조하라. 블록에 디스패치 큐를 추가하는 방법에 대한 자세한 내용은 [Adding Tasks to a Queue](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW20) 를 참조하라.

### Creating and Managing Dispatch Queues

큐에 태스크를 추가하기 전에 사용할 큐의 유형과 사용방법을 결정해야 한다. 디스패치 큐는 태스크를 연속적으로 또는 동시에 실행할 수 있다. 또한 대기열에 대한 특정 용도를 염두에 두고 있는 경우 그에 따라 큐 속성을 구성할 수 있다. 다음 섹션에서는 디스패치 큐를 생성하고 사용하도록 구성하는 방법을 보여준다.

#### Getting the Global Concurrent Dispatch Queues

콘커런트 디스패치 큐는 병렬로 실행할 수 있는 여러 태스크가 있을 때 유용하다. 콘커런트 큐는 여전히 첫 번째 순서로 태스크를 큐에 포함되지만, 콘커런트 큐는 이전 작업이 완료되기 전에 추가 태스크를 큐에 넣을 수 있다. 임의의 순간에 콘커런트 큐에 의해 실행되는 태스크 수는 가변적이며 애플리케이션의 조건이 변경될 때 동적으로 변경될 수 있다. 사용 가능한 코어 수, 다른 프로세스에서 수행되는 태스크의 양, 다른 시리얼 디스패치 큐에 의해 실행되는 태스크의 수와 우선순위를 포함하여 콘커런트 큐에 의해 실행되는 작업의 수에 많은 요인이 영향을 미친다.

시스템은 각 애플리케이션에 4개의 콘커런트 큐를 제공한다. 이러한 큐들은 애플리케이션에 전역적이며 우선 순위 레벨에 의해서만 차별화된다. 그것들은 전역적이기 때문에 당신은 그것들을 명시적으로 만들지 않는다. 대신 다음 예제에서와 같이 [dispatch\_get\_global\_queue](https://developer.apple.com/documentation/dispatch/1452927-dispatch_get_global_queue) 함수를 사용하여 큐 중 하나를 요청한다.

```objectivec
dispatch_queue_t aQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
```

기본 컨커런트 큐를 가져오는 것 외에도 [DISPATCH\_QUEUE\_PRIORITY\_HIGH](https://developer.apple.com/documentation/dispatch/dispatch_queue_priority_high) 및 [DISPATCH\_QUEUE\_PRIORITY\_LOW](https://developer.apple.com/documentation/dispatch/dispatch_queue_priority_low) 상수를 함수로 전달하여 높은 우선순위와 낮은 우선순위의 큐를 가져오거나 [DISPATCH\_QUEUE\_PRIORITY\_BACKGROUND](https://developer.apple.com/documentation/dispatch/dispatch_queue_priority_background) 상수를 넘겨 백그라운드 큐를 얻을 수 있다. 예상할 수 있듯이 우선 순위가 높은 컨커런트 큐의 태스크는 기본 대기열과 낮은 대기열의 태스크보다 먼저 실행된다. 마찬가지로, 기본 대기열의 태스크는 낮은 우선순위 대기열의 태스크보다 먼저 실행된다.

> **참고**: `dispatch_get_global_queue` 함수에 대한 두 번째 인수는 향후 확장을 위해 예약되어 있다. 지금은 이 매개변수에 항상 `0` 을 전달한다.

디스패치 큐는 참조 카운트된 객체지만 글로벌 콘커런트 큐를 리테인 및 릴리즈할 필요는 없다. 왜냐하면 이들은 애플리케이션에 전역적이기 때문에 이러한 큐에 대한 리테인 및 릴리즈 호출은 무시된다. 따라서 이러한 큐에 대한 참조를 저장할 필요가 없다. 이 중 하나에 대한 참조가 필요할 때마다 `dispatch_get_global_queue` 함수를 호출하면 된다.

#### Creating Serial Dispatch Queues

시리얼 큐는 태스크를 특정 순서로 실행하려는 경우에 유용하다. 시리얼 큐는 한 번에 하나의 태스크만 실행하고 항상 대기열의 머리에서 태스크를 끌어낸다. 당신은 공유 리소스 또는 변할 수 있는 데이터 구조를 지키기 위해 락 대신 시리얼 큐를 사용한다. 락과 달리 시리얼 큐는 태스크가 예측 가능한 순서로 실행되도록 보장한다. 태스크를 시리얼 큐에 비동기식으로 제출하는 한 큐는 교착 상태가 될 수 없다.

사용자를 위해 생성된 콘커런트 큐와 달리 사용할 시리얼 큐를 명시적으로 생성하고 관리해야 한다. 애플리케이션에 대해 원하는 수의 시리얼 큐를 생성할 수 있지만, 가능한 한 많은 태스크를 동시에 실행할 수 있는 수단으로만 많은 수의 시리얼 큐를 생성하는 것을 피해야 한다. 많은 태스크를 동시에 실행하려면 글로벌 콘커런트 큐 중 하나에 태스크를 제출하라. 시리얼 큐를 생성할 때 리소스 보호 또는 애플리케이션의 일부 주요 동기화 동작 등 각 큐에 대한 용도를 식별해라.

Listing 3-2는 사용자 지정 시리얼 큐를 생성하는 데 필요한 단계가 표시된다. [dispatch\_queue\_create](https://developer.apple.com/documentation/dispatch/1453030-dispatch_queue_create) 함수는 큐 이름과 큐 속성 집합의 두 가지 매개변수를 사용한다. 디버거 및 성능 도구에는 태스크 실행 방법을 추적하는 데 도움이 되는 큐 이름이 표시된다. 큐 속성은 향후 사용을 위해 예약되어 있으며 `NULL` 이어야 한다.

**Listing 3-2**  Creating a new serial queue

```objectivec
dispatch_queue_t queue;
queue = dispatch_queue_create("com.example.MyQueue", NULL);
```

사용자가 생성하는 모든 사용자 지정 큐 외에도, 시스템은 자동으로 시리얼 큐를 생성하고 이를 애플리케이션의 메인 쓰레드에 바인딩한다. 메인 쓰레드의 큐를 가져오는 방법에 대한 자세한 내용은 [Getting Common Queues at Runtime](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW3) 를 참조하라.

#### Getting Common Queues at Runtime

Grand Central Dispatch는 애플리케이션에서 여러 개의 공통 디스패치 큐에 접근할 수 있는 함수를 제공한다.

* 디버깅 또는 현재 큐의 ID를 테스트하려면 [`dispatch_get_current_queue`](https://developer.apple.com/documentation/dispatch/1493248-dispatch_get_current_queue) 를 사용하라. 블록 객체 내부에서 이 함수를 호출하면 블록이 대기열에 제출된다. \(그리고 현재 실행 중인 것으로 추정된다\). 블록 외부에서 이 함수를 호출하면 애플리케이션의 기본 콘커런트 큐가 반환된다.
* 애플리케이션의 메인 쓰레드와 연결된 시리얼 디스패치 큐를 가져오려면 `dispatch_get_main_queue` 함수를 사용하라. 해당 큐는 메인 쓰레드에서 [`dispatch_main`](https://developer.apple.com/documentation/dispatch/1452860-dispatchmain) 함수를 호출하거나 런 루프를 구성하는 애플리케이션에 대해 자동으로 구성된다. \([CFRunLoopRef](https://developer.apple.com/documentation/corefoundation/cfrunloopref) 유형 또는 [NSRunLoop](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop) 객체를 사용하여\)
* 공유 콘커런트 큐를 가져오려면 [`dispatch_get_global_queue`](https://developer.apple.com/documentation/dispatch/1452927-dispatch_get_global_queue) 함수를 사용하라. 더 자세한 정보는 [Getting the Global Concurrent Dispatch Queues](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW5) 를 참조하라.

#### Memory Management for Dispatch Queues

디스패치 큐 및 기타 디스패치 객체는 참조 카운트 데이터 타입이다. 시리얼 큐를 생성할 때 초기 참조 카운트가 1이다. [`dispatch_retain`](https://developer.apple.com/documentation/dispatch/1496306-dispatch_retain) 및 [`dispatch_release`](https://developer.apple.com/documentation/dispatch/1496328-dispatch_release) 함수를 사용하여 필요에 따라 해당 참조 카운트를 증가 및 감소시킬 수 있다. 큐의 참조 카운트가 0에 도달하면 시스템은 비동기적으로 큐를 할당 해제한다.

큐와 같은 디스패치 객체는 사용 중인 동안 메모리에 남아 있는지 확인하기 위해 리테인 및 릴리스하는 것이 중요하다. 메모리 관리형 코코아 객체와 마찬가지로 코드로 전달된 큐를 사용할 계획이라면 사용 전에 큐를 유지하고 더 이상 필요하지 않을 때 해제해야 한다는 것이 일반적인 규칙이다. 이 기본 패턴은 큐를 사용하는 한 큐가 메모리에 남아 있도록 보장한다.

> **참고**: 콘커런트 디스패치 큐 또는 메인 디스패치 큐 포함하여 글로벌 디스패치 큐를 유지하거나 해제할 필요가 없다. 큐를 유지하거나 해제하려는 모든 시도는 무시된다.

가비지 수집된 애플리케이션을 구현하더라도 디스패치 큐 및 기타 디스패치 객체를 유지 및 해제해야 한다. Grand Central Dispatch는 메모리 재확보를 위한 가비지 수집 모델을 지원하지 않는다.

#### Storing Custom Context Information with a Queue

모든 디스패치 객체\(디스패치 큐를 포함\)를 사용하여 사용자 지정 컨텍스트 데이터를 객체에 연결할 수 있다. 지정된 객체에 이 데이터를 설정 및 가져오려면 [`dispatch_set_context`](https://developer.apple.com/documentation/dispatch/1452807-dispatch_set_context) 및 [`dispatch_get_context`](https://developer.apple.com/documentation/dispatch/1453005-dispatch_get_context) 함수를 사용하라. 시스템은 사용자 정의 데이터를 어떤 방식으로도 사용하지 않으며, 적절한 시간에 데이터를 할당하고 할당 해제하는 것은 여러분에게 달려 있다.

큐의 경우 컨텍스트 데이터를 사용하여 큐 또는 코드의 의도된 사용을 식별하는 데 도움이 되는 Objective-C 객체 또는 기타 데이터 구조에 포인터를 저장할 수 있다. 큐의 파이널라이저 함수를 사용하여 큐가 할당되기 전에 큐에서 컨텍스트 데이터를 할당\(또는 분리\) 할 수 있다. 큐의 컨텍스트 데이터를 지우는 파이널라이저 함수의 작성 방법은 Listing 3-3에 나와 있다.

#### Providing a Clean Up Function For a Queue

시리얼 큐를 생성한 후, 큐의 할당이 취소될 때 사용자 지정 정리를 수행하기 위한 파이널라이저 함수를 첨부할 수 있다. 디스패치 큐는 참조 카운트 객체로서, 당신은 당신의 큐의 참조 카운트가 0에 도달할 때 실행할 함수를 지정하기 위해 [`dispatch_set_finalizer_f`](https://developer.apple.com/documentation/dispatch/1453100-dispatch_set_finalizer_f) 함수를 사용할 수 있다. 이 함수를 사용하여 큐와 관련된 컨텍스트 데이터를 정리할 수 있으며 컨텍스트 포인터가 `NULL`이 아닌 경우에만 함수를 호출하라.

Listing 3-3은 사용자 정의 파이널라이저 함수 및 큐를 생성하고 파이널라이저를 설치하는 함수를 보여준다. 큐는 파이널라이저 함수를 사용하여 큐의 컨텍스트 포인터에 저장된 데이터를 해제한다. \(코드에서 참조되는 `myInitializeDataContextFunction` 및 `myCleanUpDataContextFunction` 함수는 데이터 구조 자체의 내용을 초기화 및 정리하기 위해 제공하는 사용자 정의 함수이다.\) 파이널라이저 함수로 전달된 컨텍스트 포인터는 큐와 연결된 데이터 객체를 포함한다.

**Listing 3-3**  Installing a queue clean up function

```objectivec
void myFinalizerFunction(void *context)
{
    MyDataContext* theData = (MyDataContext*)context;
 
    // Clean up the contents of the structure
    myCleanUpDataContextFunction(theData);
 
    // Now release the structure itself.
    free(theData);
}
 
dispatch_queue_t createMyQueue()
{
    MyDataContext*  data = (MyDataContext*) malloc(sizeof(MyDataContext));
    myInitializeDataContextFunction(data);
 
    // Create the queue and set the context data.
    dispatch_queue_t serialQueue = dispatch_queue_create("com.example.CriticalTaskQueue", NULL);
    dispatch_set_context(serialQueue, data);
    dispatch_set_finalizer_f(serialQueue, &myFinalizerFunction);
 
    return serialQueue;
}
```

### Adding Tasks to a Queue

한 태스크를 실행하려면 적절한 디스패치 큐에 보내야 한다. 태스크를 동기식 또는 비동기식으로 발송할 수 있으며, 단독 또는 그룹으로 발송할 수 있다. 일단 큐에 있으면, 큐의 제약조건과 이미 큐에 있는 기존 작업을 고려하여 가능한 한 빨리 태스크를 실행할 책임이 있다. 이 섹션에서는 큐에 태스크를 전송하는 몇 가지 기법을 보여주며 각 기술의 장점을 설명한다.

#### Adding a Single Task to a Queue

큐에 태스크를 추가하는 두 가지 방법이 있다: 비동기식 또는 동기식. 가능한 경우, 동기식 대안보다 [`dispatch_async`](https://developer.apple.com/documentation/dispatch/1453057-dispatch_async) 및 [`dispatch_async_f`](https://developer.apple.com/documentation/dispatch/1452834-dispatch_async_f) 함수를 활용한 비동기식 실행을 선호한다. 큐에 블록 객체 또는 함수를 추가하면 해당 코드가 언제 실행되는지 알 수 없다. 결과적으로, 블록 또는 함수를 비동기식으로 추가하면 코드 실행을 예약하고 호출 쓰레드에서 다른 작업을 계속 수행할 수 있다. 만약 당신이 일부 사용자 이벤트에 대응하여 애플리케이션의 메인 쓰레드에서 태스크를 스케줄링하는 경우 특히 중요하다.

가능하면 태스크를 비동기식으로 추가해야 하지만 경쟁 상태 또는 기타 동기화 오류를 방지하기 위해 태스크를 동기적으로 추가해야 하는 경우가 여전히 있을 수 있다. 이러한 경우 [`dispatch_sync`](https://developer.apple.com/documentation/dispatch/1452870-dispatch_sync) 및 [`dispatch_sync_f`](https://developer.apple.com/documentation/dispatch/1453123-dispatch_sync_f) 함수를 사용하여 태스크를 대기열에 추가할 수 있다. 이러한 함수는 지정된 태스크의 실행이 완료될 때까지 현재 실행 쓰레드를 차단한다. 

> **중요**: 함수로 전달하려는 동일한 큐에서 실행 중인 태스크에서 `dispatch_sync` 및 `dispatch_sync_f` 함수를 호출하면 안된다. 교착 상태가 보장되는 시리얼 큐의 경우 특히 중요하지만 시리얼 큐의 경우에도 피해야 한다.

다음 예제에서는 태스크를 비동기 및 동기식으로 배포하기 위해 블록 기반 변수를 사용하는 방법을 보여준다.

```objectivec
dispatch_queue_t myCustomQueue;
myCustomQueue = dispatch_queue_create("com.example.MyCustomQueue", NULL);
 
dispatch_async(myCustomQueue, ^{
    printf("Do some work here.\n");
});
 
printf("The first block may or may not have run.\n");
 
dispatch_sync(myCustomQueue, ^{
    printf("Do some more work here.\n");
});
printf("Both blocks have completed.\n");
```

#### Performing a Completion Block When a Task Is Done

그들의 본성에 따라, 큐에 디스패치된 태스크들은 그들을 만든 코드와 독립적으로 실행된다. 그러나 태스크가 완료되면 애플리케이션에서 해당 사실을 알려주고 결과를 통합할 수 있다. 기존의 비동기 프로그래밍에서는 콜백 메커니즘을 사용하여 이 작업을 수행할 수 있지만, 디스패치 큐에서는 완료 블록을 사용할 수 있다.

완료 블록은 원래 태스크가 끝날 때 큐로 발송하는 또 다른 조각일 뿐이다. 호출 코드는 일반적으로 태스크를 시작할 때 완료 블록을 매개 변수로 제공한다. 태스크 코드는 작업을 마칠 때 지정된 블록 또는 기능을 지정된 큐에 제출하기만 하면 된다.

Listing 3-4를 나열하면 블록을 사용하여 구현된 평균 함수가 표시된다. 평균 함수에 대하 마지막 두 개의 매개 변수를 통해 발신자가 결과를 보고할 때 사용할 큐와 블록을 지정할 수 있다. 평균화 함수는 값을 계산한 후 결과를 지정된 블록으로 전달하고 큐로 디스패치한다. 큐가 조기에 해제되지 않도록 하려면 처음에 해당 큐를 유지한 후 완료 블록이 발송된 후 해제하는 것이 중요하다.

**Listing 3-4**  Executing a completion callback after a task

```objectivec
void average_async(int *data, size_t len,
   dispatch_queue_t queue, void (^block)(int))
{
   // Retain the queue provided by the user to make
   // sure it does not disappear before the completion
   // block can be called.
   dispatch_retain(queue);
 
   // Do the work on the default concurrent queue and then
   // call the user-provided block with the results.
   dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
      int avg = average(data, len);
      dispatch_async(queue, ^{ block(avg);});
 
      // Release the user-provided queue when done
      dispatch_release(queue);
   });
}
```

#### Performing Loop Iterations Concurrently

콘커런트 큐가 성능을 향상시킬 수 있는 한 곳은 고정 횟수의 반복을 수행하는 루프가 있는 장소이다. 예를 들어, 각 루프 반복을 통해 일부 작업을 수행하는 루프가 있다고 가정하라.

```objectivec
for (i = 0; i < count; i++) {
   printf("%u\n",i);
}
```

각 반복 중 수행된 작업이 다른 모든 반복 중 수행된 작업과 구별되고, 각 연속 루프가 끝나는 순서가 중요하지 않다면, [`dispatch_apply`](https://developer.apple.com/documentation/dispatch/1453050-dispatch_apply) 또는 [`dispatch_apply_f`](https://developer.apple.com/documentation/dispatch/1452846-dispatch_apply_f) 함수를 호출로 루프를 대체할 수 있다. 이러한 기능은 각 루프 반복에 대해 지정된 블록 또는 함수를 큐에 한 번 제출한다. 따라서 콘커런트 큐에 디스패치할 경우 여러 개의 루프 반복을 동시에 수행할 수 있다.

`dispatch_apply` 또는 `dispatch_apply_f`를 호출할 때 시리얼 큐 또는 콘커런트 큐를 지정할 수 있다. 콘커런트 큐를 넘기면 다중 루프 반복을 동시에 수행할 수 있으며 이러한 함수를 사용하는 가장 일반적인 방법이 된다. 비록 시리얼 큐를 사용하는 것이 허용되고 당신의 코드에 맞는 일을 하지만, 그러한 큐를 사용하는 것은 루프를 제자리에 두는 것보다 실제적인 성능상의 이점을 가지고 있지 않다.

> **중요**: 정규 `for` 루프 처럼, 모든 루프 반복이 완료될 때까지 [`dispatch_apply`](https://developer.apple.com/documentation/dispatch/1453050-dispatch_apply) 및 `dispatch_apply_f` 함수는 리턴하지 않는다. 따라서 대기열의 컨텍스트에서 이미 실행 중인 코드에서 호출할 때는 주의해야 한다. 함수에 대한 매개 변수로 전달되는 큐가 시리얼 큐이고 현재 코드를 실행하는 큐와 동일하면 이러한 함수를 호출하면 큐가 교착 상태가 된다.
>
> 이러한 함수가 현재 쓰레드를 효과적으로 차단하기 때문에 메인 쓰레드에서 이러한 함수를 호출할 때 이벤트 처리 루프가 적시에 이벤트에 응답하지 않도록 주의해야 한다. 만약 당신의 루프코드에 현저한 처리시간이 요구된다면, 당신은 다른 쓰레드에서 이러한 함수를 호출하고 싶을 것이다.

Listing 3-5는 앞의 `for` 루프를 `dispatch_apply` 문법으로 대체되는 것을 보여준다. `dispatch_apply` 함수에 전달되는 블록은 현재 루프 반복을 식별하는 단일 매개변수를 포함해야 한다. 블록이 실행될 때 이 파라미터의 값은 첫 번째 반복의 경우 `0`, 두 번째 반복의 경우 `1` 등이 된다.

**Listing 3-5**  Performing the iterations of a `for` loop concurrently

```objectivec
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
 
dispatch_apply(count, queue, ^(size_t i) {
   printf("%u\n",i);
});
```

태스크 코드가 각 반복을 통해 적절한 양의 작업을 수행하는지 확인해야 한다. 큐로 발송하는 모든 블록 또는 함수와 마찬가지로, 실행을 위해 코드를 예약하는 데 오버헤드가 있다. 루프의 각 반복이 적은 양의 작업만 수행한다면, 코드 스케줄링의 오버헤드가 큐의 코드를 배치함으로써 달성할 수 있는 성능상의 이점보다 클 수 있다. 테스트 중에 이것이 사실인 경우, 각 루프 반복 중에 수행되는 작업량을 늘리기 위해 striding을 사용할 수 있다. striding을 사용하면 원래 루프의 여러 반복을 하나의 블록으로 묶고 반복 횟수를 비례적으로 줄인다. 예를 들어, 처음에 100회 반복을 수행했지만 4단계를 사용하기로 결정하면 각 블록에서 4회 반복을 수행하고 반복 횟수는 25회이다. striding을 구현하는 방법에 대한 예는 [Improving on Loop Code](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/ThreadMigration/ThreadMigration.html#//apple_ref/doc/uid/TP40008091-CH105-SW2) 를 참조하라.

#### Performing Tasks on the Main Thread

Grand Central Dispatch는 애플리케이션의 메인 쓰레드에서 태스크를 실행하는 데 사용할 수 있는 특수한 디스패치 큐를 제공한다. 이 큐는 모든 애플리케이션에 자동으로 제공되며, 메인 쓰레드에 실행 루프\([CFRunLoopRef](https://developer.apple.com/documentation/corefoundation/cfrunloopref) 또는 [NSRunLoop](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop) 객체로 관리됨\)를 설정하는 모든 애플리케이션에 의해 자동으로 배출된다. 코코아 애플리케이션을 작성하지 않고 명시적으로 런 루프를 설정하지 않으려면 [`dispatch_main`](https://developer.apple.com/documentation/dispatch/1452860-dispatchmain) 함수를 호출하여 메인 디스패치 큐를 명시적으로 drain해야 한다.

`dispatch_get_main_queue` 함수를 호출하여 애플리케이션의 메인 쓰레드에 대한 디스패치 큐를 가져올 수 있다. 이 대기열에 추가된 태스크는 메인 쓰레드 자체에서 연속적으로 수행된다. 따라서 이 큐를 애플리케이션의 다른 부분에서 수행되는 작업의 동기화 지점으로 사용할 수 있다.

#### Using Objective-C Objects in Your Tasks

GCD는 코코아 메모리 관리 기법을 기본적으로 지원하므로, 디스패치 큐에 제출하는 블록에서 Objective-C 객체를 자유롭게 사용할 수 있다. 각 디스패치 큐는 오토릴리즈 풀을 유지하여 어느 시점에서 오토릴리즈된 객체가 해제되는지 확인한다. 큐는 실제로 이러한 객체를 해제하는 시기를 보장하지 않는다.

애플리케이션이 메모리에 제약되어 있고 블록이 몇 개 이상의 오토릴리즈된 객체를 생성하는 경우, 사용자 자신의 오토릴리즈 풀을 생성하는 것이 객체를 적시에 릴리즈할 수 있는 유일한 방법이다. 블록이 수백 개의 객체를 생성하는 경우 오토릴리즈 풀을 두 개 이상 생성하거나 정기적으로 풀을 drain 할수할 수 있다.

애플리케이션이 메모리에 제약되어 있고 블록이 몇 개 이상의 오토릴리즈된 객체를 생성하는 경우, 사용자 자신의 오토릴리즈 풀을 생성하는 것이 객체를 적시에 릴리즈할 수 있는 유일한 방법이다. 블록이 수백 개의 객체를 생성하는 경우 오토릴리즈 풀을 두 개 이상 생성하거나 정기적으로 풀을 drain 할수할 수 있다.

오토 릴리즈 풀 및 Objective-C 메모리 관리에 대한 자세한 내용은 [_Advanced Memory Management Programming Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html#//apple_ref/doc/uid/10000011i) __를 참조하라.

### Suspending and Resuming Queues

큐가 블록 객체를 일시 중단하여 일시적으로 실행되지 않도록 할 수 있다. [`dispatch_suspend`](https://developer.apple.com/documentation/dispatch/dispatchobject/1452801-suspend) 함수를 사용하여 디스패치 큐를 일시 중단하고 [`dispatch_resume`](https://developer.apple.com/documentation/dispatch/dispatchobject/1452929-resume) 함수를 사용하여 다시 시작하라. `dispatch_suspend` 함수를 호출하면 큐의 서스펜션 참조 카운트가 증가하며, `dispatch_resume` 함수를 호출하면 참조 카운트가 감소한다. 기준 카운트가 0보다 크지만 큐는 일시 중단된 상태로 유지된다. 따라서 처리 블록을 다시 시작하려면 일시 중지된 모든 호출과 일치하는 재개 호출의 균형을 유지해야 한다.

> **중요**: 일시 중단 및 재개 호출은 비동기적이며 블록 실행 사이에만 적용된다. 큐를 일시 중단해도 이미 실행 중인 블록이 중지되는 것은 아니다.

### Using Dispatch Semaphores to Regulate the Use of Finite Resources

디스패치 큐에 제출하는 태스크가 일부 유한 리소스에 접근하는 경우 디스패치 세마포어를 사용하여 해당 리소스에 동시에 접근하는 태스크 수를 조정할 수 있다. 디스패치 세마포어는 한 가지 예외를 제외하고는 일반 세마포어처럼 작동한다. 리소스를 이용할 수 있을 때, 전통적인 시스템 세마포어를 획득하는 것보다 디스패치 세마포어를 획득하는 데 시간이 덜 걸린다. 이것은 Grand Central Dispatch가 이 특별한 경우를 위해 커널을 호출하지 않기 때문이다. 그것이 커널로 호출되는 유일한 시간은 리소스를 사용할 수 없고 시스템이 세마포어가 신호를 줄 때까지 쓰레드를 주차해야 할 때다.

디스패치 세마포어의 사용 의미는 다음과 같다.

1. 세마포어를 생성할 때 \([`dispatch_semaphore_create`](https://developer.apple.com/documentation/dispatch/dispatchsemaphore/1452955-init) 함수 사용\), 사용 가능한 리소스 수를 나타내는 양의 정수를 지정할 수 있다.
2. 각 태스크에서 [`dispatch_semaphore_wait`](https://developer.apple.com/documentation/dispatch/1453087-dispatch_semaphore_wait) 를 호출하여 세마포어를 기다려라.
3. 대기 호출이 반환하면 리소스를 가져와 작업을 수행하라.
4. 리소스가 완료되면 해당 리소스를 해제하고 [`dispatch_semaphore_signal`](https://developer.apple.com/documentation/dispatch/dispatchsemaphore/1452919-signal)  함수를 호출하여 세마포어 신호를 보내라.

