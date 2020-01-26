# About Threaded Programming

수년 동안, 최대 컴퓨터 성능은 주로 컴퓨터의 중심부에 있는 단일 마이크로프로세서의 속도에 의해 제한되었다. 그러나 개별 프로세서의 속도가 실제 한계에 도달하기 시작하면서 칩 제조업체들은 멀티코어 설계로 전환하여 컴퓨터가 동시에 여러 작업을 수행할 수 있는 기회를 갖게 되었다. 그리고 OS X는 시스템 관련 작업을 수행할 수 있을 때마다 이러한 코어를 활용하지만, 사용자 자신의 애플리케이션도 쓰레드를 통해 코어를 활용할 수 있다.

### What Are Threads?

쓰레드는 애플리케이션 내부에서 다중 실행 경로를 구현하는 비교적 가벼운 방법이다. 시스템 레벨에서 프로그램은 나란히 실행되며, 시스템은 그 필요성과 다른 프로그램의 요구에 따라 각 프로그램에 실행 시간을 할당한다. 그러나 각 프로그램 안에는 하나 이상의 실행 쓰레드가 존재하는데, 이 쓰레드는 다른 작업을 동시에 또는 거의 동시에 수행하는 데 사용될 수 있다. 시스템 자체는 실제로 이러한 실행 쓰레드를 관리하며, 가용 코어에서 실행되도록 예약하고 다른 쓰레드가 실행될 수 있도록 필요에 따라 사전 차단한다.

기술적 관점에서 쓰레드는 코드의 실행을 관리하는 데 필요한 커널 레벨과 애플리케이션 레벨 데이터 구조의 조합이다. 커널 레벨 구조는 사용 가능한 코어 중 하나에서 쓰레드로 이벤트를 디스패치하고 쓰레드의 선점형 스케줄을 조정한다. 애플리케이션 수준 구조에는 함수 호출을 저장하기 위한 콜스택과 애플리케이션이 쓰레드의 특성 및 상태를 관리하고 조작하는 데 필요한 구조가 포함된다.

비동시성 애플리케이션에는 실행 쓰레드가 하나만 있다. 이 쓰레드는 애플리케이션의 `main` 루틴으로 시작되고 끝난다. 애플리케이션의 전반적인 동작을 구현하기 위해 하나씩 다른 메서드나 함수로 나뉜다. 이와는 대조적으로 동시성을 지원하는 애플리케이션은 하나의 쓰레드로 시작하고 추가 실행 경로를 생성하기 위해 필요에 따라 더 추가된다. 각각의 새로운 경로는 애플리케이션의 main 루틴에 있는 코드와 독립적으로 실행되는 고유한 사용자 정의 시작 루틴을 가지고 있다. 애플리케이션에 여러 개의 쓰레드가 있으면 두 가지 매우 중요한 잠재적 이점을 얻을 수 있다:

* 멀티 쓰레드는 애플리케이션의 지각 응답성을 향상시킬 수 있다.
* 멀티 쓰레드는 멀티코어 시스템에서 애플리케이션의 실시간 성능을 향상시킬 수 있다.

만약 당신의 애플리케이션에 하나의 쓰레드만 있다면, 그 쓰레드 한개가 모든 것을 해야 한다. 이벤트에 대응하고, 애플리케이션의 윈도우를 업데이트하고, 애플리케이션의 동작을 구현하는 데 필요한 모든 계산을 수행해야 한다. 한 번에 한 가지 일만 할 수 있다는 것이 문제이다. 그러면 당신의 계산 중 하나가 끝나는데 오랜 시간이 걸리면 어떻게 될까? 코드가 필요한 값을 계산하는 동안 애플리케이션은 사용자 이벤트에 대한 응답을 중지하고 윈도우를 업데이트하지 않는다. 이 동작이 충분히 오래 지속되면 사용자는 애플리케이션이 걸려 있다고 생각하고 강제로 중단하려고 할 수 있다. 그러나 사용자 정의 연산을 별도의 쓰레드로 이동할 경우, 애플리케이션의 메인 쓰레드는 사용자 상호작용에 보다 적시에 자유롭게 응답할 수 있다.

오늘날 멀티코어 컴퓨터에서는 쓰레드가 일부 유형의 애플리케이션에서 성능을 향상시킬 수 있는 방법을 제공한다. 서로 다른 작업을 수행하는 쓰레드는 서로 다른 프로세서 코어에서 동시에 수행할 수 있어 애플리케이션이 주어진 시간 내에 수행하는 작업량을 늘릴 수 있다.

물론, 쓰레드는 애플리케이션의 성능 문제를 해결하기 위한 만병통치약이 아니다. 쓰레드가 제공하는 이점과 함께 잠재적인 문제가 발생한다. 애플리케이션에서 실행 경로가 여러 개인 경우 코드에 상당한 복잡성을 더할 수 있다. 각 쓰레드는 애플리케이션의 상태 정보를 손상시키지 않도록 다른 쓰레드와의 동작을 조정해야 한다. 단일 애플리케이션의 쓰레드는 동일한 메모리 공간을 공유하기 때문에 동일한 데이터 구조에 모두 접근할 수 있다. 두 개의 쓰레드가 동일한 데이터 구조를 동시에 조작하려고 할 경우, 하나의 쓰레드가 결과 데이터 구조를 손상시키는 방식으로 다른 쓰레드의 변경을 덮어쓸 수 있다. 적절한 보호 장치를 갖추었더라도, 당신은 당신의 코드에 미묘한\(그리고 그렇게 미묘하지 않은\) 버그를 도입하는 컴파일러 최적화를 여전히 조심해야 한다.

### Threading Terminology

쓰레드와 그 지원 기술에 대한 논의를 너무 많이 하기 전에, 몇 가지 기본적인 용어를 정의할 필요가 있다.

유닉스 시스템에 대해 잘 알고 있는 경우, "task"라는 용어가 이 문서에 의해 다르게 사용된다는 것을 발견할 수 있다. UNIX 시스템에서는 실행 중인 프로세스를 지칭할 때 "task"라는 용어를 사용한다.

이 문서는 다음 용어를 채택한다.

* thread라는 용어는 코드에 대한 별도의 실행 경로를 가리키는 데 사용된다.
* _process_라는 용어는 여러 쓰레드를 포함할 수 있는 실행 파일을 가리키는 데 사용된다.
* _task_라는 용어는 수행이 필요한 작업의 추상적 개념을 가리키는 데 사용된다.

### Alternatives to Threads

스스로 쓰레드를 만드는 것의 한 가지 문제는 그것이 당신의 코드에 불확실성을 더한다는 것이다. 쓰레드는 애플리케이션의 동시성을 지원하는 비교적 낮고 복잡한 방법이다. 설계 선택의 의미를 완전히 이해하지 못하면 동기화 또는 타이밍 문제에 쉽게 직면할 수 있으며, 그 심각성은 미묘한 행동 변화에서 애플리케이션의 충돌 및 사용자 데이터의 손상까지 다양할 수 있다.

고려해야 할 또 다른 요인은 쓰레드가 필요한지 아니면 동시성이 필요한지 여부다. 쓰레드는 동일한 프로세스 내에서 여러 코드 경로를 동시에 실행하는 방법에 대한 구체적인 문제를 해결한다. 하지만 당신이 하고 있는 일의 양이 동시성을 보장하지 않는 경우도 있을 수 있다. 쓰레드는 메모리 소비량과 CPU 시간 측면에서 모두 프로세스에 엄청난 양의 오버헤드를 유발한다. 이 오버헤드가 의도한 태스크에 비해 너무 크거나 다른 옵션을 구현하기가 더 쉽다는 것을 발견할 수 있다.

Table 1-1은 쓰레드에 대한 몇 가지 대안이 수록되어 있다. 이 표에는 이미 보여하고 있는 단일 쓰레드를 효율적으로 사용하기 위한 쓰레드\(Operation 객체, GCD 등\) 교체 기술과 대체 기술이 모두 포함되어 있다.

**Table 1-1** Alternative technologies to threads

<table>
  <thead>
    <tr>
      <th style="text-align:left">Technology</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Operation objects</td>
      <td style="text-align:left">
        <p>OS X v10.5&#xC5D0; &#xB3C4;&#xC784;&#xB41C; &#xC624;&#xD37C;&#xB808;&#xC774;&#xC158;
          &#xAC1D;&#xCCB4;&#xB294; &#xC77C;&#xBC18;&#xC801;&#xC73C;&#xB85C; 2&#xCC28;
          &#xC4F0;&#xB808;&#xB4DC;&#xC5D0;&#xC11C; &#xC2E4;&#xD589;&#xB418;&#xB294;
          &#xD0DC;&#xC2A4;&#xD06C;&#xC5D0; &#xB300;&#xD55C; &#xB798;&#xD37C;&#xC774;&#xB2E4;.
          &#xC774; &#xB798;&#xD37C;&#xB294; &#xD0DC;&#xC2A4;&#xD06C;&#xB97C; &#xC218;&#xD589;&#xD558;&#xB294;
          &#xC4F0;&#xB808;&#xB4DC; &#xAD00;&#xB9AC; &#xCE21;&#xBA74;&#xC744; &#xC228;&#xACA8;&#xC11C;
          &#xD0DC;&#xC2A4;&#xD06C; &#xC790;&#xCCB4;&#xC5D0; &#xC9D1;&#xC911;&#xD560;
          &#xC218; &#xC788;&#xB3C4;&#xB85D; &#xD574;&#xC900;&#xB2E4;. &#xC77C;&#xBC18;&#xC801;&#xC73C;&#xB85C;
          &#xC774;&#xB7EC;&#xD55C; &#xAC1D;&#xCCB4;&#xB97C; &#xC791;&#xC5C5; &#xB300;&#xAE30;&#xC5F4;
          &#xAC1D;&#xCCB4;&#xC640; &#xD568;&#xAED8; &#xC0AC;&#xC6A9;&#xD558;&#xBA70;,
          &#xD558;&#xB098; &#xC774;&#xC0C1;&#xC758; &#xC4F0;&#xB808;&#xB4DC;&#xC5D0;&#xC11C;
          &#xD0DC;&#xC2A4;&#xD06C; &#xAC1D;&#xCCB4;&#xC758; &#xC2E4;&#xD589;&#xC744;
          &#xC2E4;&#xC81C;&#xB85C; &#xAD00;&#xB9AC;&#xD55C;&#xB2E4;.</p>
        <p>&#xC624;&#xD37C;&#xB808;&#xC774;&#xC158; &#xAC1D;&#xCCB4; &#xC0AC;&#xC6A9;
          &#xBC29;&#xBC95;&#xC5D0; &#xB300;&#xD55C; &#xC790;&#xC138;&#xD55C; &#xB0B4;&#xC6A9;&#xC740;
          <a
          href="https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091"><em>Concurrency Programming Guide</em>
            </a>&#xB97C; &#xCC38;&#xC870;&#xD558;&#xB77C;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Grand Central Dispatch (GCD)</td>
      <td style="text-align:left">
        <p>Mac OS x v10.6&#xC5D0; &#xC18C;&#xAC1C;&#xB41C; Grand Central Dispatch&#xB294;
          &#xC4F0;&#xB808;&#xB4DC; &#xAD00;&#xB9AC;&#xBCF4;&#xB2E4;&#xB294; &#xC218;&#xD589;&#xD574;&#xC57C;&#xD558;&#xB294;
          &#xD0DC;&#xC2A4;&#xD06C;&#xC5D0; &#xC9D1;&#xC911;&#xD560; &#xC218; &#xC788;&#xB294;
          &#xC4F0;&#xB808;&#xB4DC;&#xC758; &#xB610; &#xB2E4;&#xB978; &#xB300;&#xC548;&#xC774;&#xB2E4;.
          GCD&#xB97C; &#xC0AC;&#xC6A9;&#xD558;&#xBA74; &#xC218;&#xD589;&#xD560; &#xD0DC;&#xC2A4;&#xD06C;&#xB97C;
          &#xC815;&#xC758;&#xD558;&#xACE0; &#xC791;&#xC5C5; &#xB300;&#xAE30;&#xC5F4;&#xC5D0;
          &#xCD94;&#xAC00;&#xD558;&#xC5EC; &#xD574;&#xB2F9; &#xC4F0;&#xB808;&#xB4DC;&#xC5D0;
          &#xC791;&#xC5C5; &#xC2A4;&#xCF00;&#xC904;&#xB9C1;&#xC744; &#xCC98;&#xB9AC;&#xD55C;&#xB2E4;.
          &#xC791;&#xC5C5; &#xB300;&#xAE30;&#xC5F4;&#xC740; &#xC4F0;&#xB808;&#xB4DC;&#xB97C;
          &#xC0AC;&#xC6A9;&#xD558;&#xB294; &#xAC83;&#xBCF4;&#xB2E4; &#xD0DC;&#xC2A4;&#xD06C;&#xB97C;
          &#xD6A8;&#xC728;&#xC801;&#xC73C;&#xB85C; &#xC2E4;&#xD589;&#xD558;&#xAE30;
          &#xC704;&#xD574; &#xC0AC;&#xC6A9; &#xAC00;&#xB2A5;&#xD55C; &#xCF54;&#xC5B4;
          &#xC218;&#xC640; &#xD604;&#xC7AC; &#xBD80;&#xD558;&#xB97C; &#xACE0;&#xB824;&#xD55C;&#xB2E4;.</p>
        <p>GCD &#xBC0F; &#xC791;&#xC5C5; &#xB300;&#xAE30;&#xC5F4; &#xC0AC;&#xC6A9;
          &#xBC29;&#xBC95;&#xC5D0; &#xB300;&#xD55C; &#xC790;&#xC138;&#xD55C; &#xB0B4;&#xC6A9;&#xC740;
          <a
          href="https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091"><em>Concurrency Programming Guide</em>
            </a>&#xB97C; &#xCC38;&#xC870;&#xD558;&#xB77C;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Idle-time notifications</td>
      <td style="text-align:left">&#xC0C1;&#xB300;&#xC801;&#xC73C;&#xB85C; &#xC9E7;&#xACE0; &#xC6B0;&#xC120;
        &#xC21C;&#xC704;&#xAC00; &#xB9E4;&#xC6B0; &#xB0AE;&#xC740; &#xD0DC;&#xC2A4;&#xD06C;&#xC758;
        &#xACBD;&#xC6B0;, &#xC720;&#xD734; &#xC2DC;&#xAC04; &#xD1B5;&#xC9C0;&#xB97C;
        &#xD1B5;&#xD574; &#xC560;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;&#xC758;
        &#xC0AC;&#xC6A9;&#xB7C9;&#xC774; &#xB9CE;&#xC9C0; &#xC54A;&#xC740; &#xC2DC;&#xAC04;&#xC5D0;
        &#xD0DC;&#xC2A4;&#xD06C;&#xB97C; &#xC218;&#xD589;&#xD560; &#xC218; &#xC788;&#xB2E4;.
        &#xCF54;&#xCF54;&#xC544;&#xB294; <a href="https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationQueue/Description.html#//apple_ref/occ/cl/NSNotificationQueue"><code>NSNotificationQueue</code></a>&#xAC1D;&#xCCB4;&#xB97C;
        &#xC0AC;&#xC6A9;&#xD55C; &#xC720;&#xD734; &#xC2DC;&#xAC04; &#xD1B5;&#xC9C0;&#xB97C;
        &#xC9C0;&#xC6D0;&#xD55C;&#xB2E4;. &#xC720;&#xD734; &#xC2DC;&#xAC04; &#xC54C;&#xB9BC;&#xC744;
        &#xC694;&#xCCAD;&#xD558;&#xB824;&#xBA74;<a href="https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSPostWhenIdle"><code>NSPostWhenIdle</code></a>&#xC635;&#xC158;&#xC744;
        &#xC0AC;&#xC6A9;&#xD558;&#xC5EC; &#xAE30;&#xBCF8; <code>NSNotificationQueue</code>&#xAC1D;&#xCCB4;&#xC5D0;
        &#xC54C;&#xB9BC;&#xC744; &#xAC8C;&#xC2DC;&#xD558;&#xB77C;. &#xB300;&#xAE30;&#xC5F4;&#xC740;
        &#xC2E4;&#xD589; &#xB8E8;&#xD504;&#xAC00; &#xC720;&#xD734; &#xC0C1;&#xD0DC;&#xAC00;
        &#xB420; &#xB54C;&#xAE4C;&#xC9C0; &#xC54C;&#xB9BC; &#xAC1D;&#xCCB4;&#xC758;
        &#xBC30;&#xB2EC;&#xC744; &#xC9C0;&#xC5F0;&#xC2DC;&#xD0A8;&#xB2E4;. &#xC790;&#xC138;&#xD55C;
        &#xB0B4;&#xC6A9;&#xC740; <a href="https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Notifications/Introduction/introNotifications.html#//apple_ref/doc/uid/10000043i"><em>Notification Programming Topics</em></a>&#xC744;
        &#xCC38;&#xC870;&#xD558;&#xB77C;.</td>
    </tr>
    <tr>
      <td style="text-align:left">Asynchronous functions</td>
      <td style="text-align:left">&#xC2DC;&#xC2A4;&#xD15C; &#xC778;&#xD130;&#xD398;&#xC774;&#xC2A4;&#xC5D0;&#xB294;
        &#xC790;&#xB3D9; &#xB3D9;&#xC2DC;&#xC131;&#xC744; &#xC81C;&#xACF5;&#xD558;&#xB294;
        &#xB9CE;&#xC740; &#xBE44;&#xB3D9;&#xAE30; &#xD568;&#xC218;&#xAC00; &#xD3EC;&#xD568;&#xB41C;&#xB2E4;.
        &#xC774;&#xB7EC;&#xD55C; API&#xB294; &#xC2DC;&#xC2A4;&#xD15C; &#xB370;&#xBAAC;
        &#xBC0F; &#xD504;&#xB85C;&#xC138;&#xC2A4;&#xB97C; &#xC0AC;&#xC6A9;&#xD558;&#xAC70;&#xB098;
        &#xC0AC;&#xC6A9;&#xC790; &#xC9C0;&#xC815; &#xC4F0;&#xB808;&#xB4DC;&#xB97C;
        &#xC791;&#xC131;&#xD558;&#xC5EC; &#xD0DC;&#xC2A4;&#xD06C;&#xB97C; &#xC218;&#xD589;&#xD558;&#xACE0;
        &#xACB0;&#xACFC;&#xB97C; &#xBC18;&#xD658;&#xD560; &#xC218; &#xC788;&#xB2E4;.
        (&#xC2E4;&#xC81C; &#xAD6C;&#xD604;&#xC740; &#xCF54;&#xB4DC;&#xC640; &#xBD84;&#xB9AC;&#xB418;&#xC5B4;
        &#xC788;&#xAE30; &#xB54C;&#xBB38;&#xC5D0; &#xBB34;&#xAD00;&#xD558;&#xB2E4;.)
        &#xC560;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;&#xC744; &#xC124;&#xACC4;&#xD560;
        &#xB54C; &#xBE44;&#xB3D9;&#xAE30; &#xB3D9;&#xC791;&#xC744; &#xC81C;&#xACF5;&#xD558;&#xB294;
        &#xD568;&#xC218;&#xB97C; &#xCC3E;&#xACE0; &#xC0AC;&#xC6A9;&#xC790; &#xC9C0;&#xC815;
        &#xC4F0;&#xB808;&#xB4DC;&#xC5D0;&#xC11C; &#xB3D9;&#xB4F1;&#xD55C; &#xB3D9;&#xAE30;
        &#xD568;&#xC218;&#xB97C; &#xC0AC;&#xC6A9;&#xD558;&#xB294; &#xB300;&#xC2E0;
        &#xC774; &#xAE30;&#xB2A5;&#xC744; &#xC0AC;&#xC6A9;&#xD558;&#xB294; &#xAC83;&#xC744;
        &#xACE0;&#xB824;&#xD558;&#xB77C;.</td>
    </tr>
    <tr>
      <td style="text-align:left">Timers</td>
      <td style="text-align:left">&#xC560;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;&#xC758; &#xBA54;&#xC778;
        &#xC4F0;&#xB808;&#xB4DC;&#xC5D0; &#xD0C0;&#xC774;&#xBA38;&#xB97C; &#xC0AC;&#xC6A9;&#xD558;&#xC5EC;
        &#xC4F0;&#xB808;&#xB4DC;&#xB97C; &#xD544;&#xC694;&#xB85C; &#xD558;&#xAE30;&#xC5D0;&#xB294;
        &#xB108;&#xBB34; &#xC0AC;&#xC18C;&#xD55C; &#xC8FC;&#xAE30;&#xC801;&#xC778;
        &#xD0DC;&#xC2A4;&#xD06C;&#xB97C; &#xC218;&#xD589;&#xD560; &#xC218; &#xC788;&#xC9C0;&#xB9CC;,
        &#xC5EC;&#xC804;&#xD788; &#xADDC;&#xCE59;&#xC801;&#xC778; &#xAC04;&#xACA9;&#xC73C;&#xB85C;
        &#xC11C;&#xBE44;&#xC2A4;&#xD574;&#xC57C; &#xD55C;&#xB2E4;. &#xD0C0;&#xC774;&#xBA38;&#xC5D0;
        &#xB300;&#xD55C; &#xB0B4;&#xC6A9;&#xC740; <a href="https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW21">Timer Sources</a>&#xB97C;
        &#xCC38;&#xC870;&#xD558;&#xB77C;.</td>
    </tr>
    <tr>
      <td style="text-align:left">Separate processes</td>
      <td style="text-align:left">&#xC4F0;&#xB808;&#xB4DC;&#xBCF4;&#xB2E4; &#xB354; &#xBB34;&#xAC81;&#xC9C0;&#xB9CC;
        &#xBCC4;&#xB3C4;&#xC758; &#xD504;&#xB85C;&#xC138;&#xC2A4;&#xB97C; &#xB9CC;&#xB4DC;&#xB294;
        &#xAC83;&#xC740; &#xD0DC;&#xC2A4;&#xD06C;&#xAC00; &#xC560;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;&#xACFC;
        &#xC811;&#xC120;&#xC801;&#xC73C;&#xB85C;&#xB9CC; &#xAD00;&#xB828;&#xB41C;
        &#xACBD;&#xC6B0;&#xC5D0; &#xC720;&#xC6A9;&#xD560; &#xC218; &#xC788;&#xB2E4;.
        &#xD0DC;&#xC2A4;&#xD06C;&#xC5D0; &#xC0C1;&#xB2F9;&#xD55C; &#xC591;&#xC758;
        &#xBA54;&#xBAA8;&#xB9AC;&#xAC00; &#xD544;&#xC694;&#xD558;&#xAC70;&#xB098;
        &#xB8E8;&#xD2B8; &#xAD8C;&#xD55C;&#xC744; &#xC0AC;&#xC6A9;&#xD558;&#xC5EC;
        &#xC2E4;&#xD589;&#xD574;&#xC57C; &#xD558;&#xB294; &#xACBD;&#xC6B0; &#xD504;&#xB85C;&#xC138;&#xC2A4;&#xB97C;
        &#xC0AC;&#xC6A9;&#xD560; &#xC218; &#xC788;&#xB2E4;. &#xC608;&#xB97C; &#xB4E4;&#xC5B4;
        32&#xBE44;&#xD2B8; &#xC560;&#xD50C;&#xB9AC;&#xCF00;&#xC774;&#xC158;&#xC774;
        &#xC0AC;&#xC6A9;&#xC790;&#xC5D0;&#xAC8C; &#xACB0;&#xACFC;&#xB97C; &#xD45C;&#xC2DC;&#xD558;&#xB294;
        &#xB3D9;&#xC548; 64&#xBE44;&#xD2B8; &#xC11C;&#xBC84; &#xD504;&#xB85C;&#xC138;&#xC2A4;&#xB97C;
        &#xC0AC;&#xC6A9;&#xD558;&#xC5EC; &#xB300;&#xC6A9;&#xB7C9; &#xB370;&#xC774;&#xD130;
        &#xC138;&#xD2B8;&#xB97C; &#xACC4;&#xC0B0;&#xD560; &#xC218; &#xC788;&#xB2E4;.</td>
    </tr>
  </tbody>
</table>

