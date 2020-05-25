# Doing an Initial Performance Evaluation

코드가 몇 개 있는데 성능 문제로 인해 문제가 발생하는지 확인하라. 어디서부터 시작할까? 모든 문제를 즉시 볼 수 있는 것은 아니다. 작업을 수행하는 데 몇 초가 걸렸지만 CPU 사이클을 너무 많이 소비하거나 메모리를 너무 많이 할당하는 작업은 알아채지 못할 수 있다. 애플의 성능 도구가 발휘되는 대목이다. 그것들은 당신이 쉽게 간과되는 당신의 프로그램의 측면을 볼 수 있도록 도와줄 수 있다.

다음 절에서는 프로그램 분석을 시작할 때 몇 가지 주요 도구를 사용하는 방법에 대해 간략히 설명한다. 이러한 도구는 잠재적인 문제를 식별하는 데 유용하며 상당한 양의 성능 데이터를 제공할 수 있다. 그러나 문제와 관련된 보다 구체적인 정보를 제공하는 다른 도구가 있을 수 있다는 점을 기억하라. 다른 여러 도구로 애플리케이션을 실행하면 특정 영역이 문제인지 확인하는 데 도움이 될 수 있다.

> **중요:** 성능 도구는 성능 문제를 조사하는 데 도움이 된다. 분석 중에 최대한 많은 데이터를 수집하라. 성능 분석은 어느 정도 기술이며 실제 문제를 찾기 위해 이용 가능한 모든 데이터를 신중하게 고려해야 한다.

성능 도구를 얻을 수 있는 위치 등 일반적인 성능 도구에 대한 자세한 내용은 [Performance Tools](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/PerformanceOverview/PerformanceTools/PerformanceTools.html#//apple_ref/doc/uid/TP40001410-CH205-BCIIHAAJ)를 참조하라.

### Using top

`top` 도구는 프로세스에서 잠재적인 문제 영역을 식별하는 중요한 도구이다. 이 도구는 시스템 사용에 대한 주기적으로 샘플링된 통계 집합을 표시한다. `top` 사용과 출력 이해는 잠재적인 성능 문제를 식별하는 훌륭한 방법이다.

`top` 도구는 CPU 사용, 메모리 사용 \(다양한 범주에서\), 자원 사용 \(쓰레드 및 포트 등\), 페이징 이벤트에 대해 주기적으로 업데이트된 통계를 표시한다. 기본 모드에서 `top`은 모든 시스템 프로세스의 CPU 및 메모리 사용률을 표시한다. 이 정보를 사용하여 프로그램에서 사용하는 메모리 양과 사용 중인 CPU 시간의 백분율을 확인하라. 유휴 프로그램은 CPU 시간을 사용하지 않아야 하며 활성 프로그램은 작업의 복잡성에 따라 비례적인 CPU 시간을 사용해야 한다.

> **참고:** 시간 경과에 따른 CPU 사용량 및 기타 통계를 추적하려면 인스트루먼트의 Activity Monitor 템플릿을 대신 사용하라. Activity Monitor는 메모리 사용량, CPU 사용량 및 기타 데이터의 실시간 표시를 제공하는 시간에 따른 성능 추세를 그래프로 표시한다.

Listing 4-1은 상단의 전형적인 통계 출력을 보여준다. 애플리케이션 개발자의 경우 CPU 사용량, 상주 개인 메모리 사용량 \(`RPRVT`\), 페이지인/페이지아웃 비율에 가장 관심을 가져야 하는 통계. 이러한 값은 애플리케이션의 자원 사용에 대한 몇가지 주요사항을 알려준다. CPU 사용량이 많다는 것은 애플리케이션의 작업이 적절하게 조정되지 않았다는 것을 의미할 수 있다. 메모리 사용량과 페이지 입력/페이지 출력 비율이 증가하면 애플리케이션의 메모리 사용 공간을 줄일 필요가 있음을 나타낼 수 있다.

**Listing 4-1**  top의 일반적인 출력

```text
Processes:  36 total, 2 running, 34 sleeping... 81 threads
Load Avg:  0.24, 0.27, 0.23     CPU usage:  12.5% user, 87.5% sys, 0.0% idle
SharedLibs: num =   77, resident = 10.6M code, 1.11M data, 4.75M LinkEdit
MemRegions: num = 1207, resident = 16.4M + 4.94M private, 22.2M shared
PhysMem:  16.0M wired, 25.8M active, 48.9M inactive, 90.7M used, 37.2M free
VM:  476M + 39.8M   6494(6494) pageins, 0(0) pageouts
 
  PID COMMAND      %CPU   TIME      #TH #PRTS #MREGS RPRVT  RSHRD  RSIZE  VSIZE
  318 top           0.0%  0:00.36   1    23    13   172K   232K   380K  1.31M
  316 zsh           0.0%  0:00.08   1    18    12   168K   516K   628K  1.67M
  315 Terminal      0.0%  0:02.25   4   112    50  1.32M  3.55M  4.88M  31.7M
  314 CPU Monito    0.0%  0:02.08   1    63    35   896K  1.34M  2.14M  27.9M
  313 Clock         0.0%  0:01.51   1    57    38  1.02M  2.01M  2.69M  29.0M
  312 Dock          0.0%  0:03.72   2    77    78  2.18M  2.28M  3.64M  30.0M
  311 Finder        0.0%  0:07.68   4    86   171  7.96M  9.15M  15.1M  52.1M
  308 pbs           0.0%  0:01.37   4    76    40   928K   684K  1.77M  15.4M
  285 loginwindow   0.0%  0:07.19   2    70    58  1.64M  1.93M  3.45M  29.6M
  282 cron          0.0%  0:00.00   1    11    14    88K   228K   116K  1.50M
  245 sshd          0.0%  0:02.48   1    10    15   176K   312K   356K  1.41M
  222 SecuritySe    0.0%  0:00.14   2    21    24   476K   828K  1.29M  3.95M
  209 automount     0.0%  0:00.03   2    13    20   336K   748K   324K  4.36M
  200 nfsiod        0.0%  0:00.00   1    10    12     4K   224K    52K  1.22M
  199 nfsiod        0.0%  0:00.00   1    10    12     4K   224K    52K  1.2
[...]
```

헤더 영역에서 상단에는 시스템의 전역 상태에 대한 통계가 표시된다. 이 정보에는 부하 평균, 총 프로세스 및 쓰레드 수, private, shared, wired 및 free와 같은 다양한 범주로 분류된 총 메모리가 포함된다. 또한 시스템 프레임워크에 관한 전역 정보를 포함한다. 정기적으로 최신 시스템 활동을 설명하기 위해 이 통계를 업데이트하라. 정기적으로, `top`은 최신 시스템 활동을 설명하기 위해 이러한 통계를 업데이트한다.

Table 4-1은 `-w` 파라미터를 사용하여 CPU 및 메모리 사용 모드에 나타나는 기둥형 데이터를 설명한다. `top` 보고서 정보에 대한 자세한 내용은 `top` 메뉴얼 페이지를 참조하라.

Table 4-1 -w 옵션을 사용한 top 출력

<table>
  <thead>
    <tr>
      <th style="text-align:left">Column</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>PID</code>
      </td>
      <td style="text-align:left">The BSD process ID.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>COMMAND</code>
      </td>
      <td style="text-align:left">The name of the executable or application package. (Note that Code Fragment
        Manager applications are named after the native process that launches them, <code>LaunchCFMApp</code>.)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>%CPU</code>
      </td>
      <td style="text-align:left">The percentage of CPU cycles consumed during the interval on behalf of
        this process (both kernel and user space).</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>TIME</code>
      </td>
      <td style="text-align:left">The amount of CPU time (<em>minute</em>:<em>seconds</em>.<em>hundredths</em>)
        consumed by this process since it was launched.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>#TH</code>
      </td>
      <td style="text-align:left">The number of threads owned by this process.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>#PRTS (delta)</code>
      </td>
      <td style="text-align:left">The number of Mach port objects owned by this process. (To display the
        delta value relative to the value first displayed when <code>top</code> was
        launched, use the <code>-w</code> parameter.)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>#MREG</code>
      </td>
      <td style="text-align:left">The number of memory regions.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>VPRVT</code>
      </td>
      <td style="text-align:left">The private address space currently allocated. (This value is displayed
        only with the <code>-w</code> parameter.)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>RPRVT (delta)</code>
      </td>
      <td style="text-align:left">The total amount of resident private memory. (To display the delta value
        relative to the previous sample, use the <code>-w</code> parameter when running <code>top</code>.)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>RSHRD (delta)</code>
      </td>
      <td style="text-align:left">The resident shared memory. (To display the delta value relative to the
        previous sample, use the <code>-w</code> parameter when running <code>top</code>.)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>RSIZE (delta)</code>
      </td>
      <td style="text-align:left">The total resident memory as real pages that this process currently has
        associated with it. Some may be shared by other processes. (To display
        the delta value relative to the previous sample, use the <code>-w</code> parameter
        when running <code>top</code>.)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>VSIZE (delta)</code>
      </td>
      <td style="text-align:left">
        <p>The total address space currently allocated, including shared memory.
          (To display the delta value relative to the previous sample, use the <code>-w</code> parameter
          when running <code>top</code>.)</p>
        <p>This value is mostly irrelevant for OS X processes. Every application
          has a large virtual size because of the shared region used to hold framework
          and library code.</p>
      </td>
    </tr>
  </tbody>
</table>



### Using Instruments

인스트루먼트는 성능 데이터를 수집하고 앱의 전반적인 동작을 분석하는 데 사용할 수 있는 매우 강력한 도구이다. 인스트루먼트는 메모리, CPU 사용량 등 다양한 유형의 성능 정보를 나란히 그래프로 표시할 수 있다. 이러한 방식으로 정보를 보면 겉으로 보기에 다른 측정 기준들 사이의 경향과 관계를 쉽게 식별할 수 있다.

인스트루먼트를 처음 시작할 때, 문서의 시작 템플릿을 선택하고 \(Figure 4-1\) 프로파일링할 앱을 선택하라. 각 템플릿은 특정 상황에 대한 데이터를 수집하도록 설계된 하나 이상의 기기로 사전 구성된다. 예를 들어, Leaks 템플릿에는 Allocations 인스트루먼트와 Leaks 인스트루먼트가 모두 포함되며 할당된 총 메모리 블록 수와 누수로 간주되는 메모리 블록의 하위 집합을 모두 볼 수 있다. 언제든지 문서에 더 많은 인스트루먼트를 추가할 수 있지만 일반적으로 템플릿에 의해 제공되는 공통 환경설정은 일반적인 태스크에 충분하다. 

**Figure 4-1**  인스트루먼트 템플릿 선택

![](../../.gitbook/assets/instruments_profilingtemplate_dialog_2x.png)

Figure 4-2는 기록 실행 후 Leaks 템플릿에 의해 수집된 데이터의 예를 보여준다. 시간 표시 막대 및 세부 정보 창은 구성 가능하고 필터링이 가능하므로 가장 흥미로운 데이터를 볼 수 있다.

**Figure 4-2**  기록된 데이터 검사

![](../../.gitbook/assets/instruments_trace_document_withdata_2x.png)

인스트루먼트 사용 방법과 수집할 수 있는 성능 데이터 유형에 대한 자세한 내용은 [_Instruments User Guide_](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/index.html#//apple_ref/doc/uid/TP40004652)를 참조하라.

