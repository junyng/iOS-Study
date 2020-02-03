# Dispatch Sources

당신이 기본 시스템과 상호작용할 때마다, 당신은 반드시 그 태스크를 할 준비가 되어 있어야만 한다. 커널이나 다른 시스템 계층으로 호출하는 것은 자신의 프로세스 내에서 발생하는 호출에 비해 상당히 비싼 컨텍스트의 변화를 포함한다. 결과적으로, 많은 시스템 라이브러리는 당신의 코드가 시스템에 요청을 제출하고 그 요청이 처리되는 동안 다른 작업을 계속할 수 있도록 비동기 인터페이스를 제공한다. Grand Central Dispatch는 요청을 제출하고 블록 및 디스패치 큐를 사용하여 결과를 코드로 다시 보고할 수 있도록 함으로써 이러한 일반적인 행동을 기반으로 한다.

### 디스패치 소스에 대해

_dispatch source_는 특정 저수준 시스템의 이벤트 처리를 조정하는 기본 데이터 타입이다. Grand Central Dispatch는 다음과 같은 유형의 디스패치 소스를 지원한다:

* _Timer dispatch source_는 주기적인 노티피케이션을 생성한다.
* _Signal_ dispatch source는 UNIX 신호가 도착하면 알려준다.
* _Descriptor sources_는 다음과 같은 다양한 파일 및 소켓 기반 작업을 알려 준다:
  * 데이터를 읽을 수 있는 경우
  * 데이터를 쓸 수 있을 때
  * 파일 시스템에서 파일을 삭제, 이동 또는 이름을 변경한 경우
  * 파일 메타 정보가 변경될 때
* _Process dispatch source_는 다음과 같은 프로세스 관련 이벤트를 알려 준다:
  * 프로세스가 종료될 때
  * 프로세스가 `fork` 또는 `exec` 유형의 호출을 실행한 경우
  * 프로세스에 신호가 전달되는 경우
* _Mach port dispatch sources_에서 마하 관련 이벤트를 알린다.
* _Custom dispatch sources_는 사용자가 정의하고 트리거하는 소스이다.

디스패치 소스는 일반적으로 시스템 관련 이벤트를 처리하는 데 사용되는 비동기 콜백 함수를 대체한다. 디스패치 소스를 구성할 때 모니터링할 이벤트와 해당 이벤트를 처리하는 데 사용할 디스패치 큐 및 코드를 지정하라. 관심 이벤트가 도착하면, 디스패치 소스는 실행을 위해 당신의 블록이나 함수를 지정된 디스패치 큐에 제출한다.

큐에 수동으로 제출하는 태스크와 달리, 디스패치 소스는 애플리케이션에 대한 연속적인 이벤트 소스를 제공한다. 디스패치 소스는 명시적으로 취소할 때까지 디스패치 큐에 첨부되어 있다. 첨부하는 동안 해당 이벤트가 발생할 때마다 해당 태스크 코드를 디스패치 큐에 제출한다. 타이머 이벤트와 같은 일부 이벤트는 일정한 간격으로 발생하지만 대부분은 특정 조건이 발생할 때 산발적으로만 발생한다. 이러한 이유로, 디스패치 소스는 이벤트가 보류 중인 동안 조기에 릴리즈되지 않도록 디스패치 큐를 유지한다.

이벤트가 디스패치 큐에서 백로그되는 것을 방지하기 위해 디스패치 소스는 이벤트 병합 스킴을 구현한다. 이전 이벤트에 대한 이벤트 핸들러가 디큐잉되고 실행되기 전에 새로운 이벤트가 도착하면, 디스패치 소스는 새로운 이벤트 데이터의 데이터를 이전 이벤트의 데이터와 병합한다. 이벤트의 유형에 따라, 병합은 이전 이벤트를 대체하거나 보유하고 있는 정보를 업데이트할 수 있다. 예를 들어, 신호 기반 디스패치 소스는 가장 최근의 신호에 대한 정보만 제공하지만 이벤트 핸들러의 마지막 호출 이후 전달된 총 신호 수 또한 보고한다.

### 디스패치 소스 생성

디스패치 소스를 만드는 것은 이벤트의 소스와 디스패치 소스 자체를 모두 만드는 것이다. 이벤트의 소스는 이벤트를 처리하기 위해 필요한 기본 데이터 구조체이다. 예를 들어, 설명자 기반 디스패치 소스의 경우 설명자를 열고 프로세스 기반 소스의 경우 대상 프로그램의 프로세스 ID를 획득해야 한다. 이벤트 소스가 있을 때 해당 디스패치 소스를 다음과 같이 작성할 수 있다:

1. [`dispatch_source_create`](https://developer.apple.com/documentation/dispatch/1385630-dispatch_source_create) 함수를 사용하여 디스패치 소스를 만든다.
2. 디스패치 소스를 구성한다:
   * 이벤트 핸들러를 디스패치 소스에 할당한다. [Writing and Installing an Event Handler](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html#//apple_ref/doc/uid/TP40008091-CH103-SW13)를 참조하라.
   * 타이머 소스의 경우 [`dispatch_source_set_timer`](https://developer.apple.com/documentation/dispatch/1385606-dispatch_source_set_timer) 함수를 사용하여 타이머 정보를 설정하라. 자세한 내용은 [Creating a Timer](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html#//apple_ref/doc/uid/TP40008091-CH103-SW2)를 참조하라.
3. 선택적으로 취소 핸들러를 디스패치 소스에 할당하라. [Installing a Cancellation Handler](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html#//apple_ref/doc/uid/TP40008091-CH103-SW14)를 참조하라.
4. [`dispatch_resume`](https://developer.apple.com/documentation/dispatch/dispatchobject/1452929-resume) 함수를 호출하여 이벤트를 처리하라. 자세한 내용은 [Suspending and Resuming Dispatch Sources](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html#//apple_ref/doc/uid/TP40008091-CH103-SW8)를 참조하라.

디스패치 소스는 사용할 수 있기 전에 약간의 추가 구성이 필요하기 때문에, `dispatch_source_create` 함수는 일시 중단된 상태의 디스패치 소스를 반환한다. 정지된 상태에서 디스패치 소스는 이벤트를 수신하지만 처리하지 않는다. 이렇게 하면 이벤트 핸들러를 설치하고 실제 이벤트를 처리하는 데 필요한 추가 구성을 수행할 수 있는 시간이 주어진다.

다음 절에서는 디스패치 소스의 다양한 측면을 구성하는 방법을 보여준다. 특정 유형의 디스패치 소스를 구성하는 방법을 보여 주는 자세한 예는 [Dispatch Source Examples](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html#//apple_ref/doc/uid/TP40008091-CH103-SW22)를 참조하라. 디스패치 소스를 생성하고 구성하는 데 사용되는 함수에 대한 자세한 내용은 _Grand Central Dispatch \(GCD\) Reference_를 참조하라.

### 디스패치 소스 예제

다음 절에서는 보다 일반적으로 사용되는 일부 디스패치 소스를 생성하고 구성하는 방법을 보여 준다. 특정 유형의 디스패치 소스 구성에 대한 자세한 내용은 _Grand Central Dispatch \(GCD\) Reference_를 참조하라.

#### 타이머 만들기

타이머 디스패치 소스는 정기적인 시간 기반 간격으로 이벤트를 생성한다. 타이머를 사용하여 정기적으로 수행해야 하는 특정 태스크를 시작할 수 있다. 예를 들어, 게임과 다른 그래픽 집약적인 애플리케이션은 화면이나 애니메이션 업데이트를 시작하기 위해 타이머를 사용할 수 있다. 또한 타이머를 설정하고 결과 이벤트를 사용하여 자주 업데이트되는 서버에서 새로운 정보를 확인할 수 있다.

모든 타이머 디스패치 소스는 인터벌 타이머로, 일단 생성되면 사용자가 지정한 간격으로 정기 이벤트를 전송한다. 타이머 디스패치 소스를 생성할 때 지정해야 하는 값 중 하나는 시스템에 타이머 이벤트에 대한 원하는 정확도에 대한 아이디어를 제공하는 리웨이 값이다. 이동 경로 값은 시스템이 전력을 관리하고 코어를 깨우는 방법에 있어 어느 정도 유연성을 제공한다. 예를 들어, 시스템은 발사 시간을 앞당기거나 지연시키고 다른 시스템과 더 잘 정렬하기 위해 리웨이 값을 사용할 수 있다. 그러므로 당신은 당신의 타이머에 대해 가능한 한 언제나 리웨이 값을 지정해야 한다.

> **참고**: 비록 당신이 0의 리웨이 값을 지정한다 하더라도, 당신이 요청한 정확한 나노초에서 타이머가 발사될 것이라고 결코 예상해서는 안 된다. 그 시스템은 당신의 요구를 수용하기 위해 최선을 다하지만 정확한 발사 시간은 보장할 수 없다.

컴퓨터가 절전되면 모든 타이머 디스패치 소스가 중단된다. 컴퓨터가 깨어나면, 그 타이머 디스패치 소스도 자동으로 깨어난다. 타이머의 구성에 따라, 이러한 성격의 일시 중지는 다음에 타이머가 작동될 때 영향을 줄 수 있다. 만약 당신이 [`dispatch_time`](https://developer.apple.com/documentation/dispatch/1420519-dispatch_time) 함수나 `DISPATCH_TIME_NOW` 상수를 이용하여 타이머 디스패치 소스를 설정하면 타이머 디스패치 소스는 기본 시스템 시계를 사용하여 발사 시기를 결정한다. 그러나 컴퓨터가 절전 상태인 동안에는 기본 시계가 진행되지 않는다. 반대로, 당신이 a 함수를 사용하여 타이머 디스패치 소스를 설정할 때, 타이머 디스패치 소스는 그것의 발사 시간을 벽시계 시간으로 추적한다. 후자 옵션은 이벤트 시간 사이에 너무 많은 표류가 발생하는 것을 방지하기 때문에 발사 주기가 상대적으로 큰 타이머에 일반적으로 적합하다.

목록 4-1은 30초에 한 번 발사하고 1초의 리웨이 값을 갖는 타이머의 예를 보여준다. 타이머 간격이 비교적 크기 때문에, `dispatch_walltime` 함수를 사용하여 디스패치 소스가 생성된다. 타이머의 첫 번째 발화는 즉시 발생하며 이후 이벤트는 30초마다 도착한다. `MyPeriodicTask` 및 `MyStoreTimer` 기호는 타이머 동작을 구현하고 타이머를 애플리케이션의 데이터 구조 어딘가에 저장하기 위해 쓰는 사용자 지정 함수를 나타낸다.

**목록 4-1**  Creating a timer dispatch source

```objectivec
dispatch_source_t CreateDispatchTimer(uint64_t interval,
              uint64_t leeway,
              dispatch_queue_t queue,
              dispatch_block_t block)
{
   dispatch_source_t timer = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER,
                                                     0, 0, queue);
   if (timer)
   {
      dispatch_source_set_timer(timer, dispatch_walltime(NULL, 0), interval, leeway);
      dispatch_source_set_event_handler(timer, block);
      dispatch_resume(timer);
   }
   return timer;
}
 
void MyCreateTimer()
{
   dispatch_source_t aTimer = CreateDispatchTimer(30ull * NSEC_PER_SEC,
                               1ull * NSEC_PER_SEC,
                               dispatch_get_main_queue(),
                               ^{ MyPeriodicTask(); });
 
   // Store it somewhere for later use.
    if (aTimer)
    {
        MyStoreTimer(aTimer);
    }
}
```

타이머 디스패치 소스를 생성하는 것이 시간 기반 이벤트를 수신하는 주된 방법이지만, 다른 옵션도 이용할 수 있다. 지정된 시간 간격 후에 블록을 한 번 수행하려면 [`dispatch_after`](https://developer.apple.com/documentation/dispatch/1452876-dispatch_after) 또는 [`dispatch_after_f`](https://developer.apple.com/documentation/dispatch/1452878-dispatch_after_f) 함수를 사용하라. 이 함수는 당신이 블록을 큐에 제출할 시간 값을 지정할 수 있다는 점을 제외하고, [`dispatch_async`](https://developer.apple.com/documentation/dispatch/1453057-dispatch_async) 함수와 매우 유사하게 동작한다. 시간 값을 필요에 따라 상대적인 값 또는 절대 시간 값으로 지정할 수 있다.

### 디스패치 소스 취소

디스패치 소스는 [`dispatch_source_cancel`](https://developer.apple.com/documentation/dispatch/1385604-dispatch_source_cancel) 함수를 사용하여 명시적으로 취소할 때까지 활성 상태로 유지된다. 디스패치 소스를 취소하면 새로운 이벤트의 전달이 중단되고, 취소할 수 없다. 따라서 일반적으로 다음과 같이 디스패치 소스를 취소한 후 즉시 해제하라:

```objectivec
void RemoveDispatchSource(dispatch_source_t mySource)
{
   dispatch_source_cancel(mySource);
   dispatch_release(mySource);
}
```

디스패치 소스의 취소는 비동기 작업이다. `dispatch_source_cancel` 함수를 호출한 후 새로운 이벤트가 처리되지 않지만, 이미 디스패치 소스에서 처리되고 있는 이벤트는 계속 처리된다. 최종 이벤트 처리를 완료한 후, 디스패치 소스는 존재하지 않는 경우 취소 핸들러를 실행한다.

취소 핸들러는 메모리 할당을 취소하거나 디스패치 소스를 대신하여 획득한 리소스를 정리할 수 있는 기회이다. 디스패치 소스가 설명자 또는 마하 포트를 사용하는 경우, 취소 핸들러를 제공하여 설명자를 닫거나 취소 발생시 포트를 파괴해야 한다. 다른 유형의 디스패치 소스에는 취소 핸들러가 필요하지 않지만, 메모리나 데이터를 디스패치 소스와 연결하면 해당 핸들러를 제공해야 한다. 예를 들어, 디스패치 소스의 컨텍스트 포인터에 데이터를 저장하는 경우 이를 제공해야 한다. 취소 핸들러에 대한 자세한 내용은 [Installing a Cancellation Handler](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html#//apple_ref/doc/uid/TP40008091-CH103-SW14)를 참조하라.

### 디스패치 소스 일시 중단 및 재개

`dispatch_suspend` 및 [`dispatch_resume`](https://developer.apple.com/documentation/dispatch/dispatchobject/1452929-resume) 메서드를 사용하여 임시로 디스패치 소스 이벤트 전송을 일시 중지 및 재개할 수 있다. 이러한 메서드는 당신의 디스패치 소 객체의 일시 중단 수를 증가시키고 감소시킨다. 따라서 이벤트 전달이 재개되기 전에 `dispatch_resume`에 대한 일치되는 호출과 [`dispatch_suspend`](https://developer.apple.com/documentation/dispatch/dispatchobject/1452801-suspend)의 각 호출의 균형을 맞춰야 한다.

디스패치 소스를 일시 중단하면 해당 디스패치 소스가 일시 중단된 동안 발생한 이벤트는 큐가 재개될 때까지 누적된다. 모든 이벤트를 전달하기 보다는 큐가 재개되면 이벤트는 전송전에 단일 이벤트로 병합된다. 예를 들어, 파일 이름 변경을 모니터링하는 경우 전송된 이벤트는 마지막 이름 변경만 포함할 수 있다. 이러한 방식으로 이벤트를 병합하면 이벤트가 큐에 쌓이고 태스크가 재개될 때 애플리케이션이 압도되는 것을 방지할 수 있다.

