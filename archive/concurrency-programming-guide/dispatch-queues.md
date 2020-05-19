# Dispatch Queues

GCD\(Grand Central Dispatch\) 디스패치 큐는 작업을 수행하기 위한 강력한 도구이다. 디스패치 큐를 통해 발신자와 관련하여 비동기 또는 동기식으로 임의의 코드 블록을 실행할 수 있다. 디스패치 큐를 사용하여 별도의 쓰레드에서 수행하는 데 사용한 거의 모든 태스크를 수행할 수 있다. 디스패치 큐의 장점은 해당 쓰레드 코드보다 사용이 간편하고 태스크를 실행하는 데 훨씬 효율적이라는 것이다.

이 장에서는 애플리케이션에서 일반 태스크를 실행하는 데 사용하는 방법에 대한 정보와 함께 디스패치 큐에 대한 소개를 제공한다. 기존의 쓰레드 코드를 디스패치 큐로 바꾸려면 [Migrating Away from Threads](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/ThreadMigration/ThreadMigration.html#//apple_ref/doc/uid/TP40008091-CH105-SW1)에서 쓰레드 코드 사용 방법에 대한 몇 가지 추가적인 팁을 참조하라.

## 디스패치 큐에 대해

디스패치 큐는 애플리케이션에서 작업을 비동기적으로 병렬적으로 수행하는 쉬운 방법이다. _태스크_는 단순히 당신의 애플리케이션이 수행해야 하는 일부 작업이다. 예를 들어, 어떤 계산을 수행하거나, 데이터 구조를 생성 또는 수정하거나, 파일에서 읽은 일부 데이터를 처리하거나, 또는 여러가지 작업을 처리하는 태스크를 정의할 수 있다. 함수 또는 블록 객체 내부에 해당 코드를 배치하고 디스패치 큐에 추가하여 태스크를 정의한다.

디스패치 큐는 제출하는 태스크를 관리하는 객체와 같은 구조체이다. 모든 디스패치 큐는 선입선출 데이터 구조이다. 따라서 큐에 추가하는 태스크는 항상 추가된 순서대로 시작된다. GCD는 자동으로 일부 디스패치 큐를 제공하지만 특정 목적을 위해 생성할 수 있는 다른 디스패치 큐도 제공한다. 목록 3-1에는 애플리케이션에 사용할 수 있는 디스패치 큐의 유형 및 사용 방법이 나와 있다.

**목록 3-1** 디스패치 큐 유형

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>&#xC720;&#xD615;</b>
      </th>
      <th style="text-align:left"><b>&#xC124;&#xBA85;</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&#xC2DC;&#xB9AC;&#xC5BC;</td>
      <td style="text-align:left">&#xC2DC;&#xB9AC;&#xC5BC; &#xD050; (<em>private &#xB514;&#xC2A4;&#xD328;&#xCE58; &#xD050;</em>&#xB77C;&#xACE0;&#xB3C4;
        &#xD568;)&#xB294; &#xD050;&#xC5D0; &#xCD94;&#xAC00;&#xB41C; &#xC21C;&#xC11C;&#xB300;&#xB85C;
        &#xD55C; &#xBC88;&#xC5D0; &#xD558;&#xB098;&#xC758; &#xD0DC;&#xC2A4;&#xD06C;&#xB97C;
        &#xC2E4;&#xD589;&#xD55C;&#xB2E4;. &#xD604;&#xC7AC; &#xC2E4;&#xD589; &#xC911;&#xC778;
        &#xD0DC;&#xC2A4;&#xD06C;&#xB294; &#xB514;&#xC2A4;&#xD328;&#xCE58; &#xD050;&#xC5D0;
        &#xC758;&#xD574; &#xAD00;&#xB9AC;&#xB418;&#xB294; &#xBCC4;&#xAC1C;&#xC758;
        &#xC4F0;&#xB808;&#xB4DC; (&#xD0DC;&#xC2A4;&#xD06C; &#xB9C8;&#xB2E4; &#xB2E4;&#xB97C;
        &#xC218; &#xC788;&#xC74C;)&#xC5D0;&#xC11C; &#xC2E4;&#xD589;&#xB41C;&#xB2E4;.
        &#xC2DC;&#xB9AC;&#xC5BC; &#xD050;&#xB294; &#xC885;&#xC885; &#xD2B9;&#xC815;
        &#xB9AC;&#xC18C;&#xC2A4;&#xC5D0; &#xB300;&#xD55C; &#xC561;&#xC138;&#xC2A4;&#xB97C;
        &#xB3D9;&#xAE30;&#xD654;&#xD558;&#xB294; &#xB370; &#xC0AC;&#xC6A9; &#xB41C;&#xB2E4;.
        &#xD544;&#xC694;&#xD55C; &#xB9CC;&#xD07C; &#xB9CE;&#xC740; &#xC2DC;&#xB9AC;&#xC5BC;
        &#xD050;&#xB97C; &#xC0DD;&#xC131;&#xD560; &#xC218; &#xC788;&#xC73C;&#xBA70;,
        &#xAC01; &#xD050;&#xB294; &#xB2E4;&#xB978; &#xBAA8;&#xB4E0; &#xD050;&#xC640;
        &#xB3D9;&#xC2DC;&#xC5D0; &#xC791;&#xB3D9;&#xD55C;&#xB2E4;. &#xC989;, 4&#xAC1C;&#xC758;
        &#xC2DC;&#xB9AC;&#xC5BC; &#xD050;&#xB97C; &#xC0DD;&#xC131;&#xD558;&#xBA74;
        &#xAC01; &#xD050;&#xB294; &#xD55C; &#xBC88;&#xC5D0; &#xD558;&#xB098;&#xC758;
        &#xC791;&#xC5C5;&#xB9CC; &#xC2E4;&#xD589;&#xD558;&#xC9C0;&#xB9CC; &#xAC01;
        &#xD050;&#xC5D0;&#xC11C; &#xCD5C;&#xB300; 4&#xAC1C;&#xC758; &#xC791;&#xC5C5;&#xC774;
        &#xB3D9;&#xC2DC;&#xC5D0; &#xC2E4;&#xD589;&#xB420; &#xC218; &#xC788;&#xB2E4;.
        &#xC2DC;&#xB9AC;&#xC5BC; &#xD050;&#xB97C; &#xC0DD;&#xC131;&#xD558;&#xB294;
        &#xBC29;&#xBC95;&#xC5D0; &#xB300;&#xD55C; &#xC790;&#xC138;&#xD55C; &#xB0B4;&#xC6A9;&#xC740;
        <a
        href="https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW6">Creating Serial Dispatch Queues</a>&#xB97C; &#xCC38;&#xC870;&#xD558;&#xB77C;.</td>
    </tr>
    <tr>
      <td style="text-align:left">&#xCEE8;&#xCEE4;&#xB7F0;&#xD2B8;</td>
      <td style="text-align:left">&#xCEE8;&#xCEE4;&#xB7F0;&#xD2B8; &#xD050; (<em>global &#xB514;&#xC2A4;&#xD328;&#xCE58; &#xD050;</em>&#xB77C;&#xACE0;&#xB3C4;
        &#xD568;)&#xB294; &#xD558;&#xB098; &#xC774;&#xC0C1;&#xC758; &#xD0DC;&#xC2A4;&#xD06C;&#xB97C;
        &#xB3D9;&#xC2DC;&#xC5D0; &#xC2E4;&#xD589;&#xD558;&#xC9C0;&#xB9CC; &#xD0DC;&#xC2A4;&#xD06C;&#xB294;
        &#xC5EC;&#xC804;&#xD788; &#xB300;&#xAE30;&#xC5F4;&#xC5D0; &#xCD94;&#xAC00;&#xB41C;
        &#xC21C;&#xC11C;&#xB300;&#xB85C; &#xC2DC;&#xC791;&#xB41C;&#xB2E4;. &#xD604;&#xC7AC;
        &#xC2E4;&#xD589; &#xC911;&#xC778; &#xD0DC;&#xC2A4;&#xD06C;&#xB294; &#xB514;&#xC2A4;&#xD328;&#xCE58;
        &#xD050;&#xC5D0; &#xC758;&#xD574; &#xAD00;&#xB9AC;&#xB418;&#xB294; &#xBCC4;&#xAC1C;&#xC758;
        &#xC4F0;&#xB808;&#xB4DC;&#xC5D0;&#xC11C; &#xC2E4;&#xD589;&#xB41C;&#xB2E4;.
        &#xD2B9;&#xC815; &#xC9C0;&#xC810;&#xC5D0;&#xC11C; &#xC2E4;&#xD589;&#xB418;&#xB294;
        &#xD0DC;&#xC2A4;&#xD06C;&#xC758; &#xC815;&#xD655;&#xD55C; &#xAC1C;&#xC218;&#xB294;
        &#xAC00;&#xBCC0;&#xC801;&#xC774;&#xBA70; &#xC2DC;&#xC2A4;&#xD15C; &#xC870;&#xAC74;&#xC5D0;
        &#xB530;&#xB77C; &#xB2EC;&#xB77C;&#xC9C4;&#xB2E4;. iOS 5 &#xC774;&#xC0C1;&#xC5D0;&#xC11C;&#xB294; <code>DISPATCH_QUEUE_CONCURRENT</code> &#xB97C;
        &#xD050; &#xC720;&#xD615;&#xC73C;&#xB85C; &#xC9C0;&#xC815;&#xD558;&#xC5EC;
        &#xCEE8;&#xCEE4;&#xB7F0;&#xD2B8; &#xD050;&#xB97C; &#xC9C1;&#xC811; &#xC0DD;&#xC131;&#xD560;
        &#xC218; &#xC788;&#xB2E4;. &#xB610;&#xD55C; &#xC560;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;&#xC774;
        &#xC0AC;&#xC6A9;&#xD560; &#xBBF8;&#xB9AC; &#xC815;&#xC758;&#xB41C; &#xAE00;&#xB85C;&#xBC8C;
        &#xCEE8;&#xCEE4;&#xB7F0;&#xD2B8; &#xD050; 4&#xAC1C;&#xAC00; &#xC788;&#xB2E4;.
        &#xAE00;&#xB85C;&#xBC8C; &#xCEE8;&#xCEE4;&#xB7F0;&#xD2B8; &#xD050;&#xB97C;
        &#xAC00;&#xC838;&#xC624;&#xB294; &#xBC29;&#xBC95;&#xC5D0; &#xB300;&#xD55C;
        &#xC790;&#xC138;&#xD55C; &#xB0B4;&#xC6A9;&#xC740; <a href="https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW5">Getting the Global Concurrent Dispatch Queues</a>&#xB97C;
        &#xCC38;&#xC870;&#xD558;&#xB77C;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>&#xBA54;&#xC778;</p>
        <p>&#xB514;&#xC2A4;&#xD328;&#xCE58; &#xD050;</p>
      </td>
      <td style="text-align:left">
        <p>&#xBA54;&#xC778; &#xB514;&#xC2A4;&#xD328;&#xCE58; &#xD050;&#xB294; &#xBA54;&#xC778;
          &#xC4F0;&#xB808;&#xB4DC;&#xC5D0;&#xC11C; &#xD0DC;&#xC2A4;&#xD06C;&#xB97C;
          &#xC2E4;&#xD589;&#xD558;&#xB294; &#xC804;&#xC5ED;&#xC801;&#xC73C;&#xB85C;
          &#xC0AC;&#xC6A9; &#xAC00;&#xB2A5;&#xD55C; &#xC2DC;&#xB9AC;&#xC5BC; &#xD050;&#xC774;&#xB2E4;.
          &#xC774; &#xD050;&#xB294; &#xC560;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;&#xC758;
          &#xB7F0; &#xB8E8;&#xD504; (&#xC788;&#xB294; &#xACBD;&#xC6B0;)&#xC640; &#xD568;&#xAED8;
          &#xC791;&#xB3D9;&#xD558;&#xC5EC; &#xB300;&#xAE30; &#xC911;&#xC778; &#xD0DC;&#xC2A4;&#xD06C;&#xC758;
          &#xC2E4;&#xD589;&#xC744; &#xB7F0; &#xB8E8;&#xD504;&#xC5D0; &#xC5F0;&#xACB0;&#xB41C;
          &#xB2E4;&#xB978; &#xC774;&#xBCA4;&#xD2B8; &#xC18C;&#xC2A4;&#xC758; &#xC2E4;&#xD589;&#xACFC;
          &#xC0C1;&#xD638; &#xC791;&#xC6A9;&#xD55C;&#xB2E4;. &#xC560;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;&#xC758;
          &#xBA54;&#xC778; &#xC4F0;&#xB808;&#xB4DC;&#xC5D0;&#xC11C; &#xC2E4;&#xD589;&#xB41C;
          &#xD050;&#xB294; &#xC885;&#xC885; &#xC560;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;&#xC5D0;
          &#xB300;&#xD55C; &#xC8FC;&#xC694; &#xB3D9;&#xAE30;&#xD654; &#xC9C0;&#xC810;&#xC73C;&#xB85C;
          &#xC0AC;&#xC6A9;&#xB418;&#xB294; &#xACBD;&#xC6B0;&#xAC00; &#xB9CE;&#xB2E4;.</p>
        <p>&#xBA54;&#xC778; &#xB514;&#xC2A4;&#xD328;&#xCE58; &#xD050;&#xB97C; &#xC0DD;&#xC131;&#xD560;
          &#xD544;&#xC694;&#xB294; &#xC5C6;&#xC9C0;&#xB9CC; &#xC560;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;&#xC5D0;&#xC11C;
          &#xD574;&#xB2F9; &#xD050;&#xB97C; &#xC801;&#xC808;&#xD788; &#xBC30;&#xCD9C;&#xD558;&#xB294;&#xC9C0;
          &#xD655;&#xC778;&#xD558;&#xB77C;. &#xC774; &#xD050;&#xAC00; &#xC5B4;&#xB5BB;&#xAC8C;
          &#xAD00;&#xB9AC;&#xB418;&#xB294;&#xC9C0;&#xC5D0; &#xB300;&#xD55C; &#xC790;&#xC138;&#xD55C;
          &#xB0B4;&#xC6A9;&#xC740; <a href="https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW15">Performing Tasks on the Main Thread</a>&#xB97C;
          &#xCC38;&#xC870;&#xD558;&#xB77C;.</p>
      </td>
    </tr>
  </tbody>
</table>애플리케이션에 동시성을 추가하는 경우, 디스패치 큐는 쓰레드보다 몇 가지 이점을 제공한다. 가장 직접적인 장점은 작업 큐 프로그래밍 모델의 단순성이다. 쓰레드의 경우, 수행하고자 하는 태스크와 쓰레드 자체의 생성과 관리를 위해 코드를 작성해야 한다. 디스패치 큐를 사용하면 쓰레드 생성 및 관리에 대한 걱정 없이 실제로 수행하고자 하는 작업에 집중할 수 있다. 대신 시스템이 모든 쓰레드 생성 및 관리를 처리해 준다. 장점은 단일 애플리케이션보다 훨씬 더 효율적으로 쓰레드를 관리할 수 있다는 것이다. 시스템은 가용 자원과 현재 시스템 조건에 따라 쓰레드 수를 동적으로 확장할 수 있다. 게다가, 시스템은 보통 직접 쓰레드를 만들었을 때보다 더 빠르게 당신의 태스크를 실행할 수 있다.

디스패치 큐를 위해 코드를 재작성하는 것이 어려울것이라고 생각 할 수 있지만, 쓰레드에 대한 코드를 작성하는 것보다 디스패치 큐에 대한 코드를 작성하는 것이 더 쉬운 경우가 많다. 코드를 작성하는 데 있어 핵심은 비동기적으로 실행할 수 있는 태스크를 설계하는 것이다. \(이것은 쓰레드와 디스패치 큐 모두 해당한다.\) 그러나 디스패치 큐가 예측 가능성에 있어 유리하다. 동일한 공유 리소스에 액세스하지만 서로 다른 쓰레드에서 실행되는 두 태스크가 있는 경우 두 쓰레드는 먼저 리소스를 수정할 수 있으며 두 태스크가 동시에 해당 리소스를 수정하지 않도록 락을 사용해야 한다. 디스패치 큐를 사용하면 두 태스크를 시리얼 디스패치 큐에 추가하여 지정된 시간에 한 태스크만 리소스를 수정하도록 할 수 있다. 이 유형의 큐 기반 동기화는 락이 항상 문제가 있거나 검증되지 않은 경우 모두 값비싼 커널 트랩을 필요로 하는 반면, 디스패치 큐는 주로 애플리케이션 프로세스 공간에서 작동하며 절대적으로 필요할 때만 커널로 호출하기 때문에 락보다 더 효율적이다.

시리얼 큐에서 실행되는 두 개의 태스크가 병렬적으로 실행되지 않는다는 점을 지적하는 것은 옳겠지만, 두 개의 쓰레드가 동시에 락을 취하면 쓰레드가 제공하는 동시성이 손실되거나 크게 감소한다는 것을 기억해야 한다. 더 중요한 것은 쓰레드 모델이 커널과 사용자 공간 메모리를 모두 차지하는 두 개의 쓰레드를 생성해야 한다는 것이다. 디스패치 큐는 쓰레드에 대해 동일한 메모리 패널티를 지불하지 않으며, 사용하는 쓰레드는 분주하게 유지되고 차단되지 않는다.

디스패치 큐에 대해 기억해야 할 다른 주요 사항에는 다음이 포함된다.

* 디스패치 큐는 다른 디스패치 큐와 관련해 태스크를 병렬적으로 실행한다. 태스크의 직렬화는 단일 디스패치 큐에 있는 태스크로 제한된다.
* 시스템은 한 번에 실행하는 총 태스크 수를 결정한다. 따라서 100개의 다른 큐에서 100개의 태스크를 가진 애플리케이션은 \(100개 이상의 유효 코어를 가지고 있지 않는 한\) 모든 태스크를 병렬적으로 실행하지 않을 수 있다.
* 시스템은 시작할 새 태스크를 선택할 때 큐 우선 순위 수준을 고려한다. 시리얼 큐의 우선 순위를 설정하는 방법에 대한 자세한 내용은 [Providing a Clean Up Function For a Queue](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW7)를 참조하라.
* 큐의 태스크는 큐에 추가될 때 실행할 준비가 되어 있어야 한다. \(이전에 Cocoa 오퍼레이션 객체를 사용한 적이 있는 경우 이 동작은 모델 오퍼레이션 사용과 다르다는 점에 유의하라.\)
* Private 디스패치 큐는 참조 객체로 계산된다. 큐를 자신의 코드로 유지하는 것 외에도, 디스패치 소스가 큐에 부착될 수 있고 보관횟수를 증가시킬 수 있다는 점에 유의하라. 따라서, 모든 디스패치 소스가 취소되고 모든 리테인 호출은 적절한 릴리즈 호출과 균형을 이루도록 해야 한다. 큐를 리테인 및 릴리스하는 방법에 대한 자세한 내용은 [Memory Management for Dispatch Queues](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW11)를 참조하라. 디스패치 소스에 대한 자세한 내용은 [About Dispatch Sources](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html#//apple_ref/doc/uid/TP40008091-CH103-SW12)를 참조하라.

디스패치 큐를 조작하는 데 사용하는 인터페이스에 대한 자세한 내용은 _Grand Central Dispatch \(GCD\)_를 참조하라.

## 큐-관련 기술

Grand Central Dispatch는 디스패치 큐 외에도 큐를 사용하여 코드를 관리하는 데 도움이 되는 몇 가지 기술을 제공한다. 표 3-2는 이러한 기술들을 나열하고 자세한 정보를 확인할 수 있도록 링크를 제공한다.

**표 3-2** 디스패치 큐에 사용되는 기술

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>&#xAE30;&#xC220;</b>
      </th>
      <th style="text-align:left"><b>&#xC124;&#xBA85;</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>&#xB514;&#xC2A4;&#xD328;&#xCE58;</p>
        <p>&#xADF8;&#xB8F9;</p>
      </td>
      <td style="text-align:left">&#xB514;&#xC2A4;&#xD328;&#xCE58; &#xADF8;&#xB8F9;&#xC740; &#xBE14;&#xB85D;
        &#xAC1D;&#xCCB4; &#xC14B;&#xC744; &#xBAA8;&#xB2C8;&#xD130;&#xB9C1;&#xD558;&#xC5EC;
        &#xC644;&#xB8CC;&#xD558;&#xB294; &#xBC29;&#xBC95;&#xC774;&#xB2E4;. (&#xD544;&#xC694;&#xC5D0;
        &#xB530;&#xB77C; &#xBE14;&#xB85D;&#xC744; &#xB3D9;&#xAE30;&#xC2DD; &#xB610;&#xB294;
        &#xBE44;&#xB3D9;&#xAE30;&#xC2DD;&#xC73C;&#xB85C; &#xBAA8;&#xB2C8;&#xD130;&#xB9C1;&#xD560;
        &#xC218; &#xC788;&#xB2E4;.) &#xADF8;&#xB8F9;&#xC740; &#xB2E4;&#xB978; &#xD0DC;&#xC2A4;&#xD06C;&#xC758;
        &#xC644;&#xB8CC;&#xC5D0; &#xB530;&#xB77C; &#xB2EC;&#xB77C;&#xC9C0;&#xB294;
        &#xCF54;&#xB4DC;&#xC5D0; &#xB300;&#xD55C; &#xC720;&#xC6A9;&#xD55C; &#xB3D9;&#xAE30;&#xD654;
        &#xBA54;&#xCEE4;&#xB2C8;&#xC998;&#xC744; &#xC81C;&#xACF5;&#xD55C;&#xB2E4;.
        &#xADF8;&#xB8F9; &#xC0AC;&#xC6A9;&#xC5D0; &#xB300;&#xD55C; &#xC790;&#xC138;&#xD55C;
        &#xC815;&#xBCF4;&#xB294; <a href="https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW25">Waiting on Groups of Queued Tasks</a>&#xB97C;
        &#xCC38;&#xC870;&#xD558;&#xB77C;.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>&#xB514;&#xC2A4;&#xD328;&#xCE58;</p>
        <p>&#xC138;&#xB9C8;&#xD3EC;&#xC5B4;</p>
      </td>
      <td style="text-align:left">&#xB514;&#xC2A4;&#xD328;&#xCE58; &#xC138;&#xB9C8;&#xD3EC;&#xC5B4;&#xB294;
        &#xC804;&#xD1B5;&#xC801;&#xC778; &#xC138;&#xB9C8;&#xD3EC;&#xC5B4;&#xC640;
        &#xBE44;&#xC2B7;&#xD558;&#xC9C0;&#xB9CC; &#xC77C;&#xBC18;&#xC801;&#xC73C;&#xB85C;
        &#xB354; &#xD6A8;&#xC728;&#xC801;&#xC774;&#xB2E4;. &#xB514;&#xC2A4;&#xD328;&#xCE58;
        &#xC138;&#xB9C8;&#xD3EC;&#xC5B4;&#xB294; &#xC138;&#xB9C8;&#xD3EC;&#xC5B4;&#xB97C;
        &#xC0AC;&#xC6A9;&#xD560; &#xC218; &#xC5C6;&#xAE30; &#xB54C;&#xBB38;&#xC5D0;
        &#xD638;&#xCD9C; &#xC4F0;&#xB808;&#xB4DC;&#xB97C; &#xCC28;&#xB2E8;&#xD574;&#xC57C;
        &#xD560; &#xACBD;&#xC6B0;&#xC5D0;&#xB9CC; &#xCEE4;&#xB110;&#xB85C; &#xD638;&#xCD9C;&#xD55C;&#xB2E4;.
        &#xC138;&#xB9C8;&#xD3EC;&#xC5B4;&#xB97C; &#xC0AC;&#xC6A9;&#xD560; &#xC218;
        &#xC788;&#xC73C;&#xBA74; &#xCEE4;&#xB110; &#xCF5C;&#xC774; &#xC774;&#xB8E8;&#xC5B4;&#xC9C0;&#xC9C0;
        &#xC54A;&#xB294;&#xB2E4;. &#xB514;&#xC2A4;&#xD328;&#xCE58; &#xC138;&#xB9C8;&#xD3EC;&#xC5B4;&#xC5D0;
        &#xB300;&#xD55C; &#xC0AC;&#xC6A9; &#xBC29;&#xBC95;&#xC5D0; &#xB300;&#xD55C;
        &#xC608;&#xB294; <a href="https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW24">Using Dispatch Semaphores to Regulate the Use of Finite Resources</a>&#xB97C;
        &#xCC38;&#xC870;&#xD558;&#xB77C;.</td>
    </tr>
    <tr>
      <td style="text-align:left">&#xB514;&#xC2A4;&#xD328;&#xCE58;&#xC18C;&#xC2A4;</td>
      <td style="text-align:left">&#xB514;&#xC2A4;&#xD328;&#xCE58; &#xC18C;&#xC2A4;&#xB294; &#xD2B9;&#xC815;
        &#xC720;&#xD615;&#xC758; &#xC2DC;&#xC2A4;&#xD15C; &#xC774;&#xBCA4;&#xD2B8;&#xC5D0;
        &#xB300;&#xC751;&#xD558;&#xC5EC; &#xC54C;&#xB9BC;&#xC744; &#xC0DD;&#xC131;&#xD55C;&#xB2E4;.
        &#xB514;&#xC2A4;&#xD328;&#xCE58; &#xC18C;&#xC2A4;&#xB97C; &#xC0AC;&#xC6A9;&#xD558;&#xC5EC;
        &#xD504;&#xB85C;&#xC138;&#xC2A4; &#xC54C;&#xB9BC;, &#xC2E0;&#xD638; &#xBC0F;
        &#xC124;&#xBA85;&#xC790; &#xC774;&#xBCA4;&#xD2B8;&#xC640; &#xAC19;&#xC740;
        &#xC774;&#xBCA4;&#xD2B8;&#xB97C; &#xBAA8;&#xB2C8;&#xD130;&#xB9C1;&#xD560;
        &#xC218; &#xC788;&#xB2E4;. &#xC774;&#xBCA4;&#xD2B8;&#xAC00; &#xBC1C;&#xC0DD;&#xD558;&#xBA74;,
        &#xB514;&#xC2A4;&#xD328;&#xCE58; &#xC18C;&#xC2A4;&#xB294; &#xCC98;&#xB9AC;&#xB97C;
        &#xC704;&#xD574; &#xC9C0;&#xC815;&#xB41C; &#xB514;&#xC2A4;&#xD328;&#xCE58;
        &#xD050;&#xC5D0; &#xC791;&#xC5C5; &#xCF54;&#xB4DC;&#xB97C; &#xBE44;&#xB3D9;&#xAE30;&#xC801;&#xC73C;&#xB85C;
        &#xC81C;&#xCD9C;&#xD55C;&#xB2E4;. &#xB514;&#xC2A4;&#xD328;&#xCE58;&#xC5D0;
        &#xB300;&#xD55C; &#xC790;&#xC138;&#xD55C; &#xC815;&#xBCF4;&#xB294; <a href="https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/GCDWorkQueues/GCDWorkQueues.html#//apple_ref/doc/uid/TP40008091-CH103-SW1">Dispatch Sources</a> &#xB97C;
        &#xCC38;&#xC870;&#xD558;&#xB77C;.</td>
    </tr>
  </tbody>
</table>### 블록을 사용하여 태스크 구현

블록 객체는 C, Objective-C 및 C++ 코드에 사용할 수 있는 C언어 기반 기능이다. 블록을 통해 자가 작업 단위를 쉽게 정의할 수 있다. 비록 그것들이 함수 포인터와 비슷하게 보일 수 있지만 블록은 실제로 객체와 유사하고 컴파일러가 작성하고 관리하는 기본 데이터 구조로 표시된다. 컴파일러는 당신이 제공하는 코드를 패키지화하고 \(관련 데이터와 함께\) 힙에 존재할 수 있고 당신의 애플리케이션을 통과할 수 있는 형태로 캡슐화한다.

블록의 주요 장점 중 하나는 자체 어휘 범위에서 외부 변수를 사용할 수 있다는 것이다. 함수나 메서드 내부의 블록을 정의할 때, 블록은 어떤 면에서 전통적인 코드 블록처럼 작용한다. 예를 들어, 블록은 상위 범위에 정의된 변수 값을 읽을 수 있다. 블록에 의해 접근된 변수는 블록이 나중에 접근할 수 있도록 힙의 블록 데이터 구조에 복사된다. 블록이 디스패치 큐에 추가되면 일반적으로 이러한 값은 읽기 전용 형식으로 남겨 두어야 한다. 그러나 동기식으로 실행되는 블록은 `__block` 키워드가 부모의 호출로 데이터를 반환할 준비가 되어 있는 변수를 사용할 수도 있다.

함수 포인터에 사용되는 구문과 유사한 구문을 사용하여 코드에 따라 블록을 인라인으로 선언할 수 있다. 블록과 함수 포인터의 주요 차이점은 블록 이름에 별표\(`*`\) 대신 캐럿\(`^`\)이 선행된다는 것이다. 함수 포인터처럼 블록에 인수를 전달하여 반환값을 수신할 수 있다. 목록 3-1은 당신의 코드에 블록을 동시에 선언하고 실행하는 방법을 보여준다. 변수 `aBlock`은 단일 정수 인자를 취하고 값을 반환하지 않는 블록으로 선언된다. 그런 다음 해당 프로토타입과 일치하는 실제 블록이 `aBlock` 에 할당되고 인라인으로 선언된다. 마지막 줄은 블록을 즉시 실행하여 지정된 정수를 표준 출력한다.

**목록 3-1**  간단한 블록 예제

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

* 디스패치 큐를 사용하여 비동기적으로 수행하려는 블록의 경우 상위 함수 또는 메서드에서 스칼라 변수를 캡처하여 블록에서 사용하는 것이 안전하다. 그러나 컨텍스트 호출에 의해 할당되고 삭제되는 큰 구조체나 다른 포인터 기반 변수를 캡처하려고 해서는 안된다. 블록이 실행될 때, 해당 포인터가 참조하는 메모리가 사라질 수 있다. 물론, 메모리\(또는 객체\)를 직접 할당하고 그 메모리 소유권을 블록에 명시적으로 넘기는 것이 안전하다.
* 디스패치 큐는 추가된 블록을 복사하고 실행을 마치면 블록을 해제한다. 즉, 블록을 큐에 추가하기 전에 블록을 명시적으로 복사할 필요는 없다.
* 작은 태스크를 실행할 때 큐가 원시 쓰레드보다 더 효율적이기는 하지만, 블록을 만들어 큐에서 실행하는 데는 여전히 오버헤드가 있다. 블록이 너무 작은 일을 하면, 큐에 보내는 것보다 인라인으로 실행하는 것이 더 적게 소모될 수 있다. 블록이 너무 적은 작업을 수행하는지 확인하는 방법은 성능 도구를 사용하여 각 경로에 대한 메트릭스를 수집하고 비교하는 것이다.
* 기본 쓰레드와 관련된 데이터를 캐시하지 않고 다른 블록에서 데이터를 접근할 수 있기를 기대하라. 동일한 큐의 태스크가 데이터를 공유해야 하는 경우, 디스패치 큐의 컨텍스트 포인터를 사용하여 데이터를 대신 저장한다. 디스패치 큐의 컨텍스트 데이터에 접근하는 방법에 대한 자세한 내용은 [Storing Custom Context Information with a Queue](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW13)를 참조하라.
* 블록이 여러 개의 Objective-C 객체를 생성하는 경우, 블록 코드의 일부를 @autorelease 블록에 포함하여 해당 객체의 메모리 관리를 처리할 수 있다. GCD 디스패치 큐는 자체적인 오토 릴리즈 풀이 있지만, 풀이 언제 배수되는지 보증하지 않는다. 애플리케이션이 메모리 제한을 받는 경우, 오토 릴리즈 풀을 생성하면 보다 정기적인 간격으로 오토 릴리즈된 객체에 대한 메모리를 확보할 수 있다.

블록을 선언하고 사용하는 방법을 포함하여 블록에 대한 자세한 정보는 [_Blocks Programming Topics_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Blocks/Articles/00_Introduction.html#//apple_ref/doc/uid/TP40007502)를 참조하라. 디스패치 큐에 블록을 추가하는 방법에 대한 자세한 내용은 [Adding Tasks to a Queue](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW20)를 참조하라.

### 디스패치 큐 생성 및 관리

큐에 태스크를 추가하기 전에 사용할 큐의 유형과 사용 방법을 결정해야 한다. 디스패치 큐는 태스크를 연속적으로 또는 동시에 실행할 수 있다. 또한 큐에 대한 특정 용도를 염두에 둔 경우 큐 속성을 그에 따라 구성할 수 있다. 다음 절에서는 디스패치 큐를 생성하고 사용할 수 있도록 구성하는 방법을 보여준다.

#### 글로벌 콘커런트 디스패치 큐 가져오기

콘커런트 디스패치 큐는 병렬로 실행할 수 있는 여러 태스크가 있을 때 유용하다. 콘커런트 큐는 첫 번째 입력, 첫 번째 순서로 태스크를 큐로 처리하지만, 콘커런트 큐는 이전 작업이 완료되기 전에 추가 태스크를 큐에 넣을 수 있다. 특정 순간에 콘커런트 큐에 의해 실행되는 태스크 수는 가변적이며 애플리케이션의 조건이 변경될 때 동적으로 변경될 수 있다. 이용 가능한 코어 수, 다른 프로세스에서 수행되는 태스크의 수, 다른 시리얼 디스패치 큐에 의해 실행되는 태스크의 수와 우선순위를 포함하여 콘커런트 큐에 의해 실행되는 태스크의 수에 많은 영향을 미치는 요인은 많다.

시스템은 각 애플리케이션에 4개의 콘커런트 큐를 제공한다. 이러한 큐는 애플리케이션에 대해 전역적이며 우선 순위 수준에 의해서만 구분된다. 그것들이 전역적이기 때문에, 당신은 그것들을 명시적으로 만들지 않는다. 대신 다음 예제에서와 같이 [`dispatch_get_global_queue`](https://developer.apple.com/documentation/dispatch/1452927-dispatch_get_global_queue) 함수를 사용하여 큐 중 하나를 요청하라.

```objectivec
dispatch_queue_t aQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
```

기본 컨커런트 큐를 가져오는 것 외에도 [`DISPATCH_QUEUE_PRIORITY_HIGH`](https://developer.apple.com/documentation/dispatch/dispatch_queue_priority_high) 및 [`DISPATCH_QUEUE_PRIORITY_LOW`](https://developer.apple.com/documentation/dispatch/dispatch_queue_priority_low) 상수를 함수로 전달하여 높은 우선순위와 낮은 우선순위의 큐를 가져오거나 [`DISPATCH_QUEUE_PRIORITY_BACKGROUND`](https://developer.apple.com/documentation/dispatch/dispatch_queue_priority_background) 상수를 전달하여 백그라운드 큐를 얻을 수 있다. 예상할 수 있듯이, 우선순위가 높은 컨커런트 큐의 태스크는 기본 및 우선순위가 낮은 대기열의 태스크보다 먼저 실행된다. 마찬가지로, 기본 대기열의 태스크는 우선순위가 낮은 큐의 태스크보다 먼저 실행된다.

> **참고**: `dispatch_get_global_queue` 함수에 대한 두 번째 인수는 향후 확장을 위해 유보되어 있다. 지금은 이 매개변수에 항상 `0` 을 전달한다.

디스패치 큐는 참조-카운트 객체지만 글로벌 콘커런트 큐를 리테인 및 릴리즈할 필요는 없다. 애플리케이션이 전역적이기 때문에 이러한 큐에 대한 리테인 및 릴리즈 호출은 무시하라. 따라서 이러한 큐에 대한 참조를 저장할 필요가 없다. 그 중 하나에 대한 참조가 필요할 때마다 `dispatch_get_global_queue` 함수를 호출하면 된다.

#### 시리얼 디스패치 큐 생성하기

시리얼 큐는 태스크를 특정 순서로 실행하려는 경우에 유용하다. 시리얼 큐는 한 번에 하나의 태스크만 실행하고 항상 큐의 머리에서 태스크를 끌어낸다. 공유 리소스 또는 변할 수 있는 데이터 구조를 보호하기 위해 락 대신 시리얼 큐를 사용할 수 있다. 락과 달리 시리얼 큐는 태스크가 예측 가능한 순서로 실행되도록 보장한다. 그리고 당신의 태스크를 비동기적으로 시리얼 큐에 제출하는 한, 그 큐는 결코 교착 상태에 빠질 수 없다.

사용자를 위해 생성된 콘커런트 큐와 달리, 사용할 시리얼 큐를 명시적으로 생성하고 관리해야 한다. 애플리케이션에 대해 원하는 수의 시리얼 큐를 생성할 수 있지만, 가능한 한 많은 태스크를 동시에 실행할 수 있는 수단으로만 많은 수의 시리얼 큐를 생성하는 것을 피해야 한다. 많은 태스크를 동시에 실행하려면 글로벌 콘커런트 큐 중 하나에 태스크를 제출하라. 시리얼 큐를 생성할 때 자원 보호 또는 애플리케이션의 일부 주요 동기화 등 각 큐에 대한 목적을 식별해보아라.

목록 3-2는 사용자 지정 시리얼 큐를 생성하는 데 필요한 단계가 표시 된다. [`dispatch_queue_create`](https://developer.apple.com/documentation/dispatch/1453030-dispatch_queue_create) 함수는 큐 이름과 큐 속성 집합의 두 가지 매개변수를 사용한다. 디버거 및 성능 도구는 태스크 실행 방법을 추적하는 데 도움이 되는 큐 이름이 표시된다. 큐 속성은 향후 사용을 위해 예약되어 있으며 `NULL` 이어야 한다.

**목록 3-2**  새로운 시리얼 큐 생성하기

```objectivec
dispatch_queue_t queue;
queue = dispatch_queue_create("com.example.MyQueue", NULL);
```

사용자가 생성한 사용자 지정 큐 외에도, 시스템은 자동으로 시리얼 큐를 생성하고 이를 애플리케이션의 메인 쓰레드에 바인딩한다. 메인 쓰레드의 큐를 가져오는 방법에 대한 자세한 내용은 [Getting Common Queues at Runtime](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW3)를 참조하라.

#### 런타임에 일반 큐 가져오기

Grand Central Dispatch는 애플리케이션에서 일반적인 디스패치 큐에 접근할 수 있는 몇 가지 함수를 제공한다.

* 디버깅을 위해 또는 현재 큐의 ID를 테스트하려면 [`dispatch_get_current_queue`](https://developer.apple.com/documentation/dispatch/1493248-dispatch_get_current_queue) 함수를 사용하라. 블록 객체 내부에서 이 함수를 호출하면 블록이 제출된 큐\(현재 실행 중인 것으로 추정됨\)가 반환된다. 블록 외부에서 이 함수를 호출하면 애플리케이션의 기본 콘커런트 큐가 반환된다.
* `dispatch_get_main_queue` 함수를 사용하여 애플리케이션의 메인 스레드와 연결된 시리얼 디스패치 큐를 가져온다. 해당 큐는 메인 쓰레드에서 [`dispatch_main`](https://developer.apple.com/documentation/dispatch/1452860-dispatchmain) 함수를 호출하거나 런 루프\([CFRunLoopRef](https://developer.apple.com/documentation/corefoundation/cfrunloopref) 유형 또는 [NSRunLoop](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop) 객체를 사용하여\)를 구성하는 애플리케이션에 대해 자동으로 구성된다.
* [`dispatch_get_global_queue`](https://developer.apple.com/documentation/dispatch/1452927-dispatch_get_global_queue) 함수를 사용하여 공유 콘커런트 큐를 가져온다. 자세한 내용은 [Getting the Global Concurrent Dispatch Queues](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW5)를 참조하라.

#### 디스패치 큐를 위한 메모리 관리

디스패치 큐 및 기타 디스패치 객체는 참조-카운트 데이터 타입이다. 시리얼 큐를 생성할 때 초기 참조 카운트는 1이다. [`dispatch_retain`](https://developer.apple.com/documentation/dispatch/1496306-dispatch_retain) 및 [`dispatch_release`](https://developer.apple.com/documentation/dispatch/1496328-dispatch_release) 함수를 사용하여 필요에 따라 해당 참조 카운트를 증가 및 감소시킬 수 있다. 큐의 참조 카운트가 0에 도달하면 시스템은 비동기적으로 큐를 할당 해제한다.

큐와 같은 디스패치 객체는 사용하는 동안 메모리에 리테인 및 릴리스하는 것이 중요하다. 메모리 관리 코코아 객체와 마찬가지로, 코드로 전달된 큐를 사용할 계획이라면 큐를 사용하기 전에 큐를 유지하고 더 이상 필요하지 않을 때 해제해야 한다는 것이 일반적인 규칙이다. 이 기본 패턴은 큐를 사용하는 한 큐가 메모리에 유지되도록 보장한다.

> **참고**: 콘커런트 디스패치 큐 또는 메인 디스패치 큐를 포함하여 글로벌 디스패치 큐를 유지하거나 해제할 필요는 없다. 큐를 유지하거나 해제하려는 시도는 무시된다.

가비지-콜렉션의 애플리케이션을 구현하더라도 디스패치 큐 및 기타 디스패치 객체를 리테인 및 릴리즈해야 한다. Grand Central Dispatch는 메모리 재확보를 위한 가비지 콜렉션 모델을 지원하지 않는다.

#### 큐를 사용하여 사용자 정의 컨텍스트 정보 저장

모든 디스패치 객체 \(디스패치 큐를 포함\)를 사용하여 사용자 정의 컨텍스트 데이터를 객체와 연결할 수 있다. 지정된 객체에 대해 이 데이터를 설정하고 가져오려면 [`dispatch_set_context`](https://developer.apple.com/documentation/dispatch/1452807-dispatch_set_context) 및 [`dispatch_get_context`](https://developer.apple.com/documentation/dispatch/1453005-dispatch_get_context) 함수를 사용하라. 시스템은 사용자 정의 데이터를 어떤 방식으로도 사용하지 않으며, 적절한 시간에 데이터를 할당하고 할당 해제하는 것은 여러분에게 달려 있다.

큐의 경우 컨텍스트 데이터를 사용하여 큐 또는 코드의 의도된 사용을 식별하는 데 도움이 되는 Objective-C 객체 또는 기타 데이터 구조에 포인터를 저장할 수 있다. 큐의 파이널라이저 함수를 사용하여 큐가 할당되기 전에 큐에서 컨텍스트 데이터를 할당\(또는 분리\) 할 수 있다. 큐의 컨텍스트 데이터를 지우는 파이널라이저 함수의 작성 방법은 Listing 3-3에 나와 있다.

#### 큐를 위한 정리 함수 제공

시리얼 큐를 생성한 후, 큐의 할당을 취소할 때 사용자 정의 정리를 수행하기 위한 파이널라이저 함수를 첨부할 수 있다. 디스패치 큐는 참조 카운트 객체로서, 당신은 당신의 큐의 참조 카운트가 0에 도달할 때 실행할 함수를 지정하기 위해 [`dispatch_set_finalizer_f`](https://developer.apple.com/documentation/dispatch/1453100-dispatch_set_finalizer_f) 함수를 사용할 수 있다. 이 함수를 사용하여 큐와 관련된 컨텍스트 데이터를 정리할 수 있으며 컨텍스트 포인터가 `NULL`이 아닌 경우에만 함수를 호출하라.

목록 3-3은 사용자 정의 파이널라이저 함수 및 큐를 생성하고 파이널라이저를 설치하는 함수를 보여준다. 큐는 파이널라이저 함수를 사용하여 큐의 컨텍스트 포인터에 저장된 데이터를 해제한다. \(코드에서 참조하는 `myInitializeDataContextFunction` 및 `myCleanUpDataContextFunction` 함수는 데이터 구조 자체의 내용을 초기화 및 정리하기 위해 제공하는 사용자 정의 함수이다.\) 파이널라이저 함수로 전달된 컨텍스트 포인터는 큐와 연결된 데이터 객체를 포함한다.

**목록 3-3**  큐 정리 함수 설치

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

### 큐에 태스크 추가

태스크를 실행하려면 적절한 디스패치 큐에 태스크를 전송해야 한다. 태스크를 동기적으로 또는 비동기적으로 발송할 수 있으며, 단일 또는 그룹으로 발송할 수 있다. 일단 큐에 들어가면, 그 제약조건과 이미 큐에 있는 기존 작업을 고려하여 가능한 한 빨리 태스크를 실행할 책임이 있다. 이 절에서는 큐에 태스크를 전송하는 몇 가지 기법을 보여주며 각 기술의 장점을 설명한다.

#### 큐에 단일 태스크 추가

큐에 태스크를 추가하는 두 가지 방법이 있다: 비동기식 또는 동기식. 가능한 경우, 동기식 대안보다 [`dispatch_async`](https://developer.apple.com/documentation/dispatch/1453057-dispatch_async) 및 [`dispatch_async_f`](https://developer.apple.com/documentation/dispatch/1452834-dispatch_async_f) 함수를 활용한 비동기식 실행이 선호된다. 큐에 블록 객체 또는 함수를 추가하면 해당 코드가 언제 실행되는지 알 수 없다. 그 결과, 블록이나 함수를 비동기적으로 추가하면 코드의 실행을 예약하고 호출 쓰레드에서 다른 작업을 계속 수행할 수 있다. 일부 사용자 이벤트에 대응하여 애플리케이션의 메인 쓰레드에서 태스크를 스케줄링하는 경우 특히 중요하다.

가능하면 태스크를 비동기적으로 추가해야 하지만, 경쟁 상태 또는 기타 동기화 오류를 방지하기 위해 태스크를 동기적으로 추가해야 하는 경우가 여전히 있을 수 있다. 이러한 경우 [`dispatch_sync`](https://developer.apple.com/documentation/dispatch/1452870-dispatch_sync) 및 [`dispatch_sync_f`](https://developer.apple.com/documentation/dispatch/1453123-dispatch_sync_f) 함수를 사용하여 태스크를 큐에 추가할 수 있다. 이러한 함수는 지정된 태스크가 실행을 완료할 때까지 현재 실행 쓰레드를 차단한다. 

> **중요**: 해당 함수에 전달하려는 큐의 실행 중인 태스크에서 `dispatch_sync` 및 `dispatch_sync_f` 함수를 호출하면 안된다. 이는 교착 상태가 보장되는 시리얼 큐의 경우 특히 중요하지만 시리얼 큐의 경우에도 피해야 한다.

다음 예제에서는 태스크를 비동기 및 동기적으로 배포하기 위해 블록-기반 변수를 사용하는 방법을 보여준다:

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

#### 태스크가 완료되었을 때 완료 블록 수행

그들의 본성에 따라, 큐에 발송된 태스크들은 그들을 만든 코드와 독립적으로 실행된다. 그러나 태스크가 완료되면 애플리케이션에서 해당 사실을 알려주고 결과를 통합할 수 있다. 기존의 비동기 프로그래밍에서는 콜백 메커니즘을 사용하여 이 작업을 수행할 수 있지만, 디스패치 큐에서는 완료 블록을 사용할 수 있다.

완료 블록은 원래 태스크가 끝날 때 큐로 발송하는 또 다른 코드 조각일 뿐이다. 호출 코드는 일반적으로 태스크를 시작할 때 완료 블록을 매개 변수로 제공한다. 태스크 코드가 작업을 마칠 때 지정된 블록 또는 기능을 지정된 큐에 제출하기만 하면 된다.

목록 3-4는 블록을 사용하여 구현된 평균화 함수가 표시된다. 평균 함수에 대한 마지막 두 개의 매개 변수를 사용하면 발신자가 결과를 보고할 때 사용할 큐와 블록을 지정할 수 있다. 평균화 함수가 값을 계산한 후 결과를 지정된 블록으로 전달하고 큐에 발송한다. 큐가 조기에 해제되지 않도록 하려면 처음에 해당 큐를 유지하고 완료 블록이 발송된 후 해제하는 것이 중요하다.

**목록 3-4**  태스크 완료 후 콜백 실행

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

#### 루프 반복을 동시에 수행

콘커런트 큐가 성능을 향상시킬 수 있는 한 장소는 고정 횟수의 반복을 수행하는 루프가 있는 장소에 있다. 예를 들어, 각 루프 반복을 통해 일부 작업을 수행하는 `for` 루프가 있다고 가정하라:

```objectivec
for (i = 0; i < count; i++) {
   printf("%u\n",i);
}
```

각 반복 중 수행되는 작업이 다른 모든 반복 중에 수행된 작업과 구별되고, 각각의 연속적인 루프가 끝나는 순서가 중요하지 않은 경우, 해당 루프를 [`dispatch_apply`](https://developer.apple.com/documentation/dispatch/1453050-dispatch_apply) 또는 [`dispatch_apply_f`](https://developer.apple.com/documentation/dispatch/1452846-dispatch_apply_f) 함수 호출로 대체할 수 있다. 이러한 기능은 각 루프 반복에 대해 지정된 블록 또는 함수를 큐에 한 번 제출한다. 따라서 콘커런트 큐에 발송할 경우 여러 개의 루프 반복을 동시에 수행할 수 있다.

`dispatch_apply` 또는 `dispatch_apply_f`를 호출할 때 시리얼 큐 또는 콘커런트 큐를 지정할 수 있다. 콘커런트 큐를 전달하면 다중 루프 반복을 동시에 수행할 수 있으며 이러한 함수를 사용하는 가장 일반적인 방법이 된다. 시리얼 큐를 사용하는 것은 허용되며 코드에 맞는 일을 하지만, 그러한 큐를 사용하는 것은 루프를 제자리에 두는 것보다 실질적인 성능 이점이 없다.

> **중요**: 일반적인 `for` 루프 처럼, 모든 루프 반복이 완료될 때까지 [`dispatch_apply`](https://developer.apple.com/documentation/dispatch/1453050-dispatch_apply) 및 `dispatch_apply_f` 함수는 반환되지 않는다. 따라서 큐의 컨텍스트에서 이미 실행 중인 코드에서 호출할 때는 주의해야 한다. 함수에 대한 매개 변수로 전달되는 큐가 시리얼 큐이고 현재 코드를 실행하는 큐와 동일하면 이러한 함수를 호출하면 큐가 교착 상태가 된다.
>
> 이러한 함수가 현재 쓰레드를 효과적으로 차단하기 때문에 메인 쓰레드에서 이러한 함수를 호출할 때 이벤트 처리 루프가 적시에 이벤트에 응답하지 않도록 주의해야 한다. 만약 당신의 루프코드에 현저한 처리시간이 요구된다면, 당신은 이 함수들을 다른 쓰레드에서 호출하고 싶을 것이다.

목록 3-5는 앞의 `for` 루프를 `dispatch_apply` 문법으로 대체되는 것을 보여준다. `dispatch_apply` 함수에 전달되는 블록은 현재 루프 반복을 식별하는 단일 매개변수를 포함해야 한다. 블록이 실행될 때 이 파라미터의 값은 첫 번째 반복의 경우 `0`, 두 번째 반복의 경우 `1` 등이 된다. 마지막 반복에 대한 파라미터의 값은 `count - 1`이며, 여기서 `count`는 반복의 총 횟수이다.

**목록 3-5** `for` 루프를 동시에 수행하기 위해 반복 수행

```objectivec
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
 
dispatch_apply(count, queue, ^(size_t i) {
   printf("%u\n",i);
});
```

태스크 코드가 각 반복을 통해 적절한 양의 작업을 수행하는지 확인해야 한다. 큐로 발송하는 모든 블록 또는 함수와 마찬가지로, 이 코드를 실행하기 위한 스케줄링에도 오버헤드가 있다. 루프의 각 반복이 소량의 작업만 수행하는 경우 코드 스케줄링의 오버헤드가 큐로 전송하여 얻을 수 있는 성능 이점을 초과할 수 있다. 테스트 중에 이것이 사실인 경우, 각 루프 반복 중에 수행되는 작업량을 늘리기 위해 striding을 사용할 수 있다. striding을 사용하면 원래 루프의 여러 번의 반복을 하나의 블록으로 묶고 반복 횟수를 비례적으로 줄인다. 예를 들어, 처음에 100회 반복을 수행하지만 4회 반복을 수행하고 반복 횟수는 25회이다. striding을 구현하는 방법에 대한 예는 [Improving on Loop Code](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/ThreadMigration/ThreadMigration.html#//apple_ref/doc/uid/TP40008091-CH105-SW2)를 참조하라.

#### 메인 쓰레드에서 태스크 수행

Grand Central Dispatch는 애플리케이션의 메인 쓰레드에서 태스크를 실행하는 데 사용할 수 있는 특수한 디스패치 큐를 제공한다. 이 큐는 모든 애플리케이션에 자동으로 제공되며, 메인 쓰레드에 런 루프 \([CFRunLoopRef](https://developer.apple.com/documentation/corefoundation/cfrunloopref) 또는 [NSRunLoop](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop) 객체로 관리됨\)를 설정하는 모든 애플리케이션에 의해 자동으로 배출된다. 코코아 애플리케이션을 작성하지 않고 명시적으로 런 루프를 설정하지 않으려면 [`dispatch_main`](https://developer.apple.com/documentation/dispatch/1452860-dispatchmain) 함수를 호출하여 메인 디스패치 큐를 명시적으로 배수해야 한다. 큐에 태스크를 추가할 수 있지만 이 함수를 호출하지 않으면 해당 태스크는 실행되지 않는다.

`dispatch_get_main_queue` 함수를 호출하여 애플리케이션의 메인 쓰레드에 대한 디스패치 큐를 가져올 수 있다. 해당 큐에 추가된 태스크는 메인 쓰레드 자체에서 연속적으로 수행된다. 따라서 이 큐를 애플리케이션의 다른 부분에서 수행되는 작업의 동기화 지점으로 사용할 수 있다.

#### 태스크에서 Objective-C 객체 사용

GCD는 코코아 메모리 관리 기법을 기본적으로 지원하므로, 디스패치 큐에 제출하는 블록에서 Objective-C 객체를 자유롭게 사용할 수 있다. 각 디스패치 큐는 오토 릴리즈 풀을 유지하여 어느 시점에서 오토 릴리즈된 객체가 해제되는지 확인한다. 큐는 실제로 이러한 객체를 해제하는 시기를 보장하지 않는다.

애플리케이션이 메모리에 제약되어 있고 블록이 몇 개 이상의 오토릴리즈된 객체를 생성하는 경우, 사용자 자신의 오토 릴리즈 풀을 생성하는 것이 객체를 적시에 릴리즈할 수 있는 유일한 방법이다. 블록이 수백 개의 객체를 생성하는 경우 둘 이상의 오토 릴리즈 풀을 생성하거나 정기적으로 풀을 배수하는 것이 좋을 수 있다.

오토 릴리즈 풀 및 Objective-C 메모리 관리에 대한 자세한 내용은 [_Advanced Memory Management Programming Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html#//apple_ref/doc/uid/10000011i)를 참조하라.

### 큐 일시 중지 및 재개

큐가 블록 객체를 일시 중단하여 일시적으로 실행되지 않도록 할 수 있다. [`dispatch_suspend`](https://developer.apple.com/documentation/dispatch/dispatchobject/1452801-suspend) 함수를 사용하여 디스패치 큐를 일시 중단한 후 [`dispatch_resume`](https://developer.apple.com/documentation/dispatch/dispatchobject/1452929-resume) 함수를 사용하여 다시 시작하라. `dispatch_suspend` 함수 호출은 큐의 일시 중단 참조 카운트를 증가시키고, `dispatch_resume` 함수 호출은 참조 카운트가 감소시킨다. 기준 카운트가 0보다 크면 큐는 일시 중단된 상태로 유지된다. 따라서 처리 블록을 다시 시작하려면 일시 중지된 모든 호출과 일치하는 재개 호출의 균형을 유지해야 한다.

> **중요**: 일시 중단 및 재개 호출은 비동기적이며 블록 실행 사이에만 적용된다. 큐를 일시 중단해도 이미 실행 중인 블록이 중지되는 것은 아니다.

### 디스패치 세마포어를 사용하여 한정된 리소 사용 규제

디스패치 큐에 제출하는 태스크가 일부 한정된 리소에 접근하는 경우 디스패치 세마포어를 사용하여 해당 리소스를 동시에 접근하는 태스크 수를 조정할 수 있다. 디스패치 세마포어는 한 가지 예외를 제외하고는 일반 세마포어처럼 작동한다. 리소스가 확보되면, 전통적인 시스템 세마포어를 획득하는 것보다 디스패치 세마포어를 획득하는 데 시간이 덜 걸린다. Grand Central Dispatch는 이 특정 사례에 대해 커널을 호출하지 않기 때문이다. 커널로 호출되는 유일한 시간은 리소스를 사용할 수 없고 시스템이 세마포어에 신호를 줄 때까지 쓰레드를 주차해야 할 때이다.

디스패치 세마포어의 사용 의미는 다음과 같다.

1. 세마포어를 생성할 때 \([`dispatch_semaphore_create`](https://developer.apple.com/documentation/dispatch/dispatchsemaphore/1452955-init) 함수 사용\), 사용 가능한 리소스의 수를 나타내는 양의 정수를 지정할 수 있다.
2. 각 태스크에서 [`dispatch_semaphore_wait`](https://developer.apple.com/documentation/dispatch/1453087-dispatch_semaphore_wait) 를 호출하여 세마포어를 기다려라.
3. 대기 호출이 반환하면 리소스를 가져와 작업을 수행하라.
4. 리소스가 완료되면 해당 리소스를 해제하고 [`dispatch_semaphore_signal`](https://developer.apple.com/documentation/dispatch/dispatchsemaphore/1452919-signal)  함수를 호출하여 세마포어 신호를 보내라.

이러한 단계가 작동하는 방법의 예는 시스템에서 파일 설명자의 사용을 고려하라. 각 애플리케이션에는 사용할 파일 설명자의 수가 제한되어 있다. 많은 수의 파일을 처리하는 태스크가 있는 경우 파일 설명자가 다 떨어지도록 한 번에 너무 많은 파일을 열지 않아야 한다. 대신 세마포어를 사용하여 파일 처리 코드에 의해 한 번에 사용 중인 파일 설명자 수를 제한할 수 있다. 태스크에 통합할 코드 조각의 기본 내용은 다음과 같다:

```objectivec
// Creabete the semaphore, specifying the initial pool size
dispatch_semaphore_t fd_sema = dispatch_semaphore_create(getdtablesize() / 2);
 
// Wait for a free file descriptor
dispatch_semaphore_wait(fd_sema, DISPATCH_TIME_FOREVER);
fd = open("/etc/services", O_RDONLY);
 
// Release the file descriptor when done
close(fd);
dispatch_semaphore_signal(fd_sema);
```

세마포어를 생성할 때 사용 가능한 리소스 수를 지정하라. 이 값은 세마포어의 초기 카운트 변수가 된다. 세마포어를 기다릴 때마다 `dispatch_semaphore_wait` 함수는 변수를 1씩 카운트한다. 결과 값이 음수일 경우 함수는 커널에게 쓰레드를 차단하라고 지시한다. 반면에, `dispatch_semaphore_signal` 함수는 카운트 변수를 1씩 증가시켜 리소스가 확보되었음을 표시한다. 차단된 태스크가 있고 리소스를 대기하는 태스크가 있으면 그 중 하나가 후속적으로 차단 해제되고 해당 작업을 수행할 수 있다.

### 대기중엔 큐 태스크 그룹

디스패치 그룹은 하나 이상의 태스크가 실행을 마칠 때까지 쓰레드를 차단하는 방법이다. 이 동작은 지정된 모든 태스크가 완료될 때까지 진행할 수 없는 곳에서 사용할 수 있다. 예를 들어, 일부 데이터를 계산하기 위해 여러 태스크를 전송한 후 그룹을 사용하여 해당 태스크에서 대기한 다음 완료되었을 때 결과를 처리할 수 있다. 쓰레드 조인의 대안으로 디스패치 그룹을 사용하는 또 다른 방법이 있다. 여러 개의 하위 쓰레드를 시작한 다음 각 쓰레드와 결합하는 대신, 해당 태스크를 디스패치 그룹에 추가하고 그룹 전체에서 대기할 수 있다.

목록 3-6 은 그룹 설정, 태스크 전송, 결과 대기를 위한 기본 프로세스를 보여준다. [`dispatch_async`](https://developer.apple.com/documentation/dispatch/1453057-dispatch_async) 함수를 사용하여 큐에 태스크를 전송하는 대신 [`dispatch_group_async`](https://developer.apple.com/documentation/dispatch/1453084-dispatch_group_async) 함수를 사용하라. 이 함수는 태스크를 그룹과 연결하고 실행을 위해 큐에 넣어라. 태스크 그룹이 완료될 때까지 기다리려면 해당 그룹을 전달하면서 [`dispatch_group_wait`](https://developer.apple.com/documentation/dispatch/1452794-dispatch_group_wait) 함수를 사용하라.

**목록 3-6**  대기 중인 비동기 태스크

```objectivec
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
dispatch_group_t group = dispatch_group_create();
 
// Add a task to the group
dispatch_group_async(group, queue, ^{
   // Some asynchronous work
});
 
// Do some other work while the tasks execute.
 
// When you cannot make any more forward progress,
// wait on the group to block the current thread.
dispatch_group_wait(group, DISPATCH_TIME_FOREVER);
 
// Release the group when it is no longer needed.
dispatch_release(group);
```

### 디스패치 큐 및 쓰레드 세이프

디스패치 큐의 컨텍스트에서 쓰레드 안전성에 대해 말하는 것이 이상하게 보일 수 있지만, 쓰레드 안전성은 여전히 관련 주제이다. 애플리케이션에서 동시성을 구현할 때마다 몇 가지 알아야 할 사항이 있다:

* 디스패치 큐 자체는 쓰레드에 안전하다. 즉, 먼저 락을 해제하거나 큐에 대한 접근을 동기화하지 않고 시스템의 쓰레드에서 태스크를 디스패치 큐에 제출할 수 있다.
* 함수 호출에 전달된 것과 동일한 큐에서 실행 중인 태스크에서 [`dispatch_sync`](https://developer.apple.com/documentation/dispatch/1452870-dispatch_sync) 함수를 호출하지 않아야 한다. 이렇게 하면 큐가 교착 상태가 된다. 현재 큐로 발송해야 하는 경우 [`dispatch_async`](https://developer.apple.com/documentation/dispatch/1453057-dispatch_async) 함수를 사용하여 비동기식으로 처리하라.
* 디스패치 큐에 제출하는 태스크에서 락을 해제하지 마라. 태스크에서 락을 사용하는 것은 안전하지만 락을 획득할 때 해당 락을 사용할 수 없는 경우 시리얼 큐를 완전히 차단할 위험이 있다. 마찬가지로 콘커런트 큐의 경우 락에서 대기하면 다른 태스크가 대신 실행되지 않을 수 있다. 코드의 일부를 동기화해야 하는 경우 락 대신 시리얼 디스패치 큐를 사용하라.
* 태스크를 실행하는 메인 쓰레드에 대한 정보를 얻을 수 있지만 그렇게 하지 않는 것이 좋다. 디스패치 큐와 쓰레드의 호환성에 대한 자세한 정보는 [Compatibility with POSIX Threads](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/ThreadMigration/ThreadMigration.html#//apple_ref/doc/uid/TP40008091-CH105-SW18) 를 참조하라.

디스패치 큐를 사용하도록 기존 쓰레드 코드를 변경하는 방법에 대한 추가 팁은 [Migrating Away from Threads](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/ThreadMigration/ThreadMigration.html#//apple_ref/doc/uid/TP40008091-CH105-SW1)을 참조하라.

