# Operation Queues

Cocoa 오퍼레이션은 비동기적으로 수행하고자 하는 작업을 캡슐화하는 객체 지향적인 방법이다. 오퍼레이션은 오퍼레이션 큐 혹은 자체적으로 사용하도록 설계된다. Objective-C 기반이기 때문에, 오퍼레이션은 OS X와 iOS의 코코아 기반 애플리케이션에서 가장 일반적으로 사용된다.

이 장에서는 오퍼레이션을 정의하고 사용하는 방법을 보여 준다.

## About Operation Objects

오퍼레이션 객체는 애플리케이션이 수행할 작업을 캡슐화하는 데 사용하는 `NSOperation` 클래스의 인스턴스다. `NSOperation` 클래스 자체는 유용한 작업을 수행하기 위해 서브 클래싱되는 추상 기본 클래스이다. 추상적이긴 하지만, 이 클래스는 당신의 서브 클래스에서 해야 하는 일의 양을 최소화하기 위해 상당한 인프라를 제공한다. 또한 Foundation 프레임워크는 기존 코드와 함께 사용할 수 있는 두 개의 구체적인 서브 클래스를 제공한다. 표 2-1은 각 클래스를 사용하는 방법에 대한 요약과 함께 이러한 클래스를 나열한다.

**Table 2-1** Operation classes of the Foundation framework

<table>
  <thead>
    <tr>
      <th style="text-align:left">Class</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><a href="https://developer.apple.com/documentation/foundation/nsinvocationoperation">NSInvocationOperation</a>
      </td>
      <td style="text-align:left">&#xC560;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;&#xC5D0;&#xC11C; &#xAC1D;&#xCCB4;&#xC640; <code>selector</code>&#xC5D0;
        &#xAE30;&#xBC18;&#xC73C;&#xB85C; &#xC624;&#xD37C;&#xB808;&#xC774;&#xC158;
        &#xAC1D;&#xCCB4;&#xB97C; &#xC0DD;&#xC131;&#xD558;&#xAE30; &#xC704;&#xD574;
        &#xC0AC;&#xC6A9;&#xD558;&#xB294; &#xD074;&#xB798;&#xC2A4;&#xC774;&#xB2E4;.
        &#xD544;&#xC694;&#xD55C; &#xD0DC;&#xC2A4;&#xD06C;&#xB97C; &#xC774;&#xBBF8;
        &#xC218;&#xD589;&#xD558;&#xB294; &#xAE30;&#xC874; &#xBA54;&#xC11C;&#xB4DC;&#xAC00;
        &#xC788;&#xB294; &#xACBD;&#xC6B0; &#xC774; &#xD074;&#xB798;&#xC2A4;&#xB97C;
        &#xC0AC;&#xC6A9;&#xD560; &#xC218; &#xC788;&#xB2E4;. &#xC11C;&#xBE0C; &#xD074;&#xB798;&#xC2F1;&#xC774;
        &#xD544;&#xC694; &#xC5C6;&#xAE30; &#xB54C;&#xBB38;&#xC5D0; &#xC774; &#xD074;&#xB798;&#xC2A4;&#xB97C;
        &#xC0AC;&#xC6A9;&#xD558;&#xC5EC; &#xBCF4;&#xB2E4; &#xB3D9;&#xC801;&#xC778;
        &#xBC29;&#xC2DD;&#xC73C;&#xB85C; &#xC624;&#xD37C;&#xB808;&#xC774;&#xC158;
        &#xAC1D;&#xCCB4;&#xB97C; &#xB9CC;&#xB4E4; &#xC218;&#xB3C4; &#xC788;&#xB2E4;.
        &#xC774; &#xD074;&#xB798;&#xC2A4;&#xB97C; &#xC0AC;&#xC6A9;&#xD558;&#xB294;
        &#xBC29;&#xBC95;&#xC5D0; &#xB300;&#xD55C; &#xC790;&#xC138;&#xD55C; &#xC815;&#xBCF4;&#xB294;
        <a
        href="https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW6">Creating an NSInvocationOperation Object</a>&#xB97C; &#xCC38;&#xC870;&#xD558;&#xB77C;.</td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://developer.apple.com/documentation/foundation/blockoperation">NSBlockOperation</a>
      </td>
      <td style="text-align:left">
        <p>&#xD558;&#xB098; &#xC774;&#xC0C1;&#xC758; block object&#xB97C; &#xB3D9;&#xC2DC;&#xC5D0;
          &#xC2E4;&#xD589;&#xD558;&#xAE30; &#xC704;&#xD574; &#xC0AC;&#xC6A9;&#xD558;&#xB294;
          &#xD074;&#xB798;&#xC2A4;&#xC774;&#xB2E4;. &#xBE14;&#xB85D;&#xC740; &#xB458;
          &#xC774;&#xC0C1; &#xC2E4;&#xD589;&#xD560; &#xC218; &#xC788;&#xAE30; &#xB54C;&#xBB38;&#xC5D0;
          &#xBE14;&#xB85D; &#xC791;&#xC5C5; &#xAC1D;&#xCCB4;&#xB294; &#xADF8;&#xB8F9;
          &#xC758;&#xBBF8;&#xB860;&#xC801;&#xC73C;&#xB85C; &#xC791;&#xB3D9;&#xD55C;&#xB2E4;.
          &#xBAA8;&#xB4E0; &#xC5F0;&#xAD00;&#xB41C; &#xBE14;&#xB85D;&#xC774; &#xC2E4;&#xD589;&#xC744;
          &#xC644;&#xB8CC;&#xD588;&#xC744; &#xACBD;&#xC6B0;&#xC5D0; &#xC791;&#xC5C5;
          &#xC790;&#xCCB4;&#xAC00; &#xC644;&#xB8CC;&#xB41C; &#xAC83;&#xC73C;&#xB85C;
          &#xAC04;&#xC8FC; &#xB41C;&#xB2E4;.</p>
        <p>&#xC774; &#xD074;&#xB798;&#xC2A4;&#xB97C; &#xC0AC;&#xC6A9;&#xD558;&#xB294;
          &#xBC29;&#xBC95;&#xC5D0; &#xB300;&#xD55C; &#xC790;&#xC138;&#xD55C; &#xB0B4;&#xC6A9;&#xC740;
          <a
          href="https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW2">Creating an NSBlockOperation Object</a>&#xB97C; &#xCC38;&#xC870;&#xD558;&#xB77C;.
            &#xC774; &#xD074;&#xB798;&#xC2A4;&#xB294; OS X v10.6 &#xC774;&#xC0C1;&#xC5D0;&#xC11C;
            &#xC0AC;&#xC6A9;&#xD560; &#xC218; &#xC788;&#xB2E4;. &#xBE14;&#xB85D;&#xC5D0;
            &#xB300;&#xD55C; &#xC790;&#xC138;&#xD55C; &#xB0B4;&#xC6A9;&#xC740; <a href="https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Blocks/Articles/00_Introduction.html#//apple_ref/doc/uid/TP40007502"><em>Blocks Programming Topics</em></a> &#xB97C;
            &#xCC38;&#xC870;&#xD558;&#xB77C;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><a href="https://developer.apple.com/documentation/foundation/nsoperation">NSOperation</a>
      </td>
      <td style="text-align:left">
        <p>&#xC0AC;&#xC6A9;&#xC790; &#xC9C0;&#xC815; &#xC791;&#xC5C5; &#xAC1D;&#xCCB4;&#xB97C;
          &#xC815;&#xC758;&#xD558;&#xAE30; &#xC704;&#xD55C; &#xAE30;&#xBCF8; &#xD074;&#xB798;&#xC2A4;&#xC774;&#xB2E4;. <code>NSOperation</code> &#xC11C;&#xBE0C;&#xD074;&#xB798;&#xC2F1;&#xC740;
          &#xC791;&#xC5C5;&#xC774; &#xC2E4;&#xD589;&#xB418;&#xACE0; &#xC0C1;&#xD0DC;&#xB97C;
          &#xBCF4;&#xACE0;&#xD558;&#xB294; &#xAE30;&#xBCF8; &#xBC29;&#xC2DD;&#xC744;
          &#xBCC0;&#xACBD;&#xD560; &#xC218; &#xC788;&#xB294; &#xAE30;&#xB2A5;&#xC744;
          &#xD3EC;&#xD568;&#xD558;&#xC5EC; &#xC790;&#xC2E0;&#xC758; &#xC791;&#xC5C5;
          &#xAD6C;&#xD604;&#xC5D0; &#xB300;&#xD55C; &#xC644;&#xC804;&#xD55C; &#xC81C;&#xC5B4;
          &#xAE30;&#xB2A5;&#xC744; &#xC81C;&#xACF5;&#xD55C;&#xB2E4;.</p>
        <p>&#xC0AC;&#xC6A9;&#xC790; &#xC815;&#xC758; &#xC624;&#xD37C;&#xB808;&#xC774;&#xC158;
          &#xAC1D;&#xCCB4;&#xC5D0; &#xB300;&#xD55C; &#xC790;&#xC138;&#xD55C; &#xC815;&#xBCF4;&#xB97C;
          <a
          href="https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW16">Defining a Custom Operation Object</a>&#xB97C; &#xCC38;&#xC870;&#xD558;&#xB77C;.</p>
      </td>
    </tr>
  </tbody>
</table>

모든 오퍼레이션 객체는 다음과 같은 주요 기능을 지원한다.

* 오퍼레이션 객체 간의 그래프 기반 종속성 설정을 지원한다. 이러한 종속성으로 인해 해당 작업이 종속된 모든 작업이 실행될때까지 해당 작업이 실행되지 않는다. 종속성을 구성하는 방법에 대한 자세한 내용은 [Configuring Interoperation Dependencies](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW17) 를 참조하라.
* 오퍼레이션의 메인 태스크가 완료된 후 실행되는 선택적 완료 블록을 지원한다. \(OS X v10.6 이상\) 완료 블록 설정 방법에 대한 자세한 내용은 [Setting Up a Completion Block](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW33) 을 참조하라.
* KVO 알림을 사용하여 작업 실행 상태에 대한 변경 사항을 모니터링할 수 있도록 지원한다. KVO 알림을 관찰하는 방법에 대한 자세한 내용은 [_Key-Value Observing Programming Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i) __를 참조하라.
* 상대적 실행 순서에 영향을 미치는 작업 우선 순위 지정을 지원한다. 자세한 내용은 [Changing an Operation’s Execution Priority](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW31) 를 참조하라.
* 작업을 실행하는 동안 작업을 중지할 수 있는 취소를 지원한다. 작업을 취소하는 방법에 대한 자세한 내용은 [Canceling Operations](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW39) 를 참조하라. 자체 오퍼레이션에서 취소 지원 방법에 대한 자세한 내용은 [Responding to Cancellation Events](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW24) 를 참조하라.

오퍼레이션은 애플리케이션의 동시성 수준을 향상시키는 데 도움이 되도록 설계되어 있다. 오퍼레이션은 또한 프로그램의 동작을 간단한 개별적인 덩어리로 구성하고 캡슐화하는 좋은 방법이다. 애플리케이션의 메인 쓰레드에서 일부 코드를 실행하는 대신, 하나 이상의 작업 객체를 대기열에 제출하고 하나 이상의 개별 쓰레드에서 해당 작업을 비동기식으로 수행하도록 할 수 있다.

## Concurrent Versus Non-concurrent Operations

일반적으로 오퍼레이션 큐에 추가하여 오퍼레이션을 실행하지만, 그렇게할 필요는 없다. 또한 오퍼레이션 객체의 `start` 메서드를 호출하여 수동으로 실행할 수도 있다. 하지만 그렇게 한다고 해서 당신의 코드의 나머지 부분과 동시에 작업이 실행된다는 것을 보장하지는 않는다. `NSOperation`클래스의 `isConcurrent` 메서드는 `start`메서드가 호출된 스레드와 관련하여 오퍼레이션이 동기적으로 실행되는지 또는 비동기적으로 실행되는지 여부를 알려준다. 기본적으로 이 메서드는 `NO`를 리턴하며, 이는 호출 스레드에서 오퍼레이션이 동기적으로 실행된다는 것을 의미한다.

컨커런트 오퍼레이션\(즉, 호출 스레드에 대해 비동기식으로 실행되는 오퍼레이션\)을 구현하려면 오퍼레이션을 비동기식으로 시작하는 추가적인 코드를 작성해야 한다. 예를 들어 별도의 스레드를 생성하여 비동기 시스템 메서드를 호출할 수 있다. 또는 `start` 메서드가 태스크를 시작하고 즉시 리턴하는 것을 보장하며 가능한 모든 경우에 다시 시작하도록 하기 위해 다른 작업을 수행한다.

대부분의 개발자는 컨커런트 오퍼레이션 객체를 구현할 필요가 없어야 한다. 오퍼레이션 큐에 항상 오퍼레이션을 추가하는 경우 컨커런트 오퍼레이션을 구현할 필요가 없다. 오퍼레이션 큐에 논 컨커런트 오퍼레이션을 제출하면 큐 자체가 작업을 실행할 스레드를 생성한다. 따라서 오퍼레이션 큐에 비동기 작업을 추가하면 오퍼레이션 객체 코드가 여전히 비동기적으로 실행된다. 동시 작업을 정의하는 기능은 오퍼레이션을 오퍼레이션 큐에 추가하지 않고 비동기식으로 실행해야 하는 경우에만 필요하다.

컨커런트 오퍼레이션을 생성하는 방법에 대한 자세한 내용은 Configuring Operations for Concurrent Execution과 NSOperation Class Reference를 참조하라.

