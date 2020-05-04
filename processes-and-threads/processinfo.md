---
description: 현재 프로세스에 대한 정보 모음.
---

# ProcessInfo

### Declaration

```swift
class ProcessInfo : NSObject
```

### Overview

각 프로세스에는 _process information agent_로 알려진 하나의 공유 `ProcessInfo`가 있다.

프로세스 정보 에이전트는 전달인자, 환경 변수, 호스트 이름 및 프로세스 이름과 같은 정보를 반환할 수 있다. [`processInfo`](https://developer.apple.com/documentation/foundation/processinfo/1408734-processinfo) 클래스 메서드는 현재 프로세스에 대한 공유 에이전트를 반환한다. 즉, 객체가 메시지를 보낸 프로세스이다. 예를 들어, 다음 라인은 현재 프로세스의 이름을 제공하는 `ProcessInfo` 객체를 반환한다:

```swift
let processName = NSProcessInfo.processInfo().processName
```

> **참고**
>
> `ProcessInfo` 는 mac 10.7 이후에 쓰레드 세이프하다.

`ProcessInfo` 클래스는 프로세스가 실행 중인 운영 체제를 식별하는 열거형 상수를 반환하는 [`operatingSystem()`](https://developer.apple.com/documentation/foundation/processinfo/1416341-operatingsystem) 메서드 또한 포함한다.

`ProcessInfo` 객체는 유니코드 문자열로 변환할 수 없는 경우 사용자의 기본 C 문자열 인코딩에서 환경 변수 및 커맨드라인 전달인자를 UTF-8 문자열로 해석하려고 시도한다. 유니코드 및 C 문자열 변환이 작동하지 않을 경우 `ProcessInfo` 객체에서 이러한 값은 무시된다.

#### Managing Activities <a id="1651116"></a>

이 시스템은 사용자의 이점을 위해 애플리케이션의 배터리 수명, 성능 및 응답성을 개선하기 위한 경험적 특성을 가지고 있다. 다음 메서드를 사용하여 애플리케이션에 특별한 요구 사항이 있음을 시스템에 알리는 _activities_을 관리할 수 있다:

* [`beginActivity(options:reason:)`](https://developer.apple.com/documentation/foundation/processinfo/1415995-beginactivity)
* [`endActivity(_:)`](https://developer.apple.com/documentation/foundation/processinfo/1411321-endactivity)
* [`performActivity(options:reason:using:)`](https://developer.apple.com/documentation/foundation/processinfo/1418048-performactivity)

액티비티 생성에 대한 응답으로 시스템은 휴리스틱스의 일부 또는 전부를 비활성화하여 사용자가 필요로 하는 응답 동작을 제공하는 동시에 애플리케이션이 빠르게 완료될 수 있도록 한다.

애플리케이션이 장기 실행 작업을 수행할 때 액티비티를 사용하라. 만약 액티비티가 다른 시간\(예를 들어, 체스 게임에서 다음 동작 계산\)이 걸린다면, 이 API를 사용해야 한다. 이렇게 하면 데이터 양이나 사용자 컴퓨터의 기능이 다를 때 올바른 동작을 보장할 수 있다. 액티비티를 다음 두 가지 주요 범주 중 하나에 포함시켜야 한다:

1. _User initiated:_ 사용자가 명시적으로 시작한 유한한 길이의 액티비티이다. 예를 들어, 사용자 정의 파일 내보내기 또는 다운로드가 포함된다.
2. Background: 애플리케이션의 정상적인 동작에 속하지만 사용자가 명시적으로 시작하지는 않는 유한한 길이의 액티비티이다. 예를 들어, 파일의 자동 저장, 인덱싱 및 자동 다운로드가 있다.

또한 애플리케이션에 높은 우선 순위 I/O가 필요한 경우 [`latencyCritical`](https://developer.apple.com/documentation/foundation/processinfo/activityoptions/1415541-latencycritical) 플래그\(비트 OR 사용\)를 포함할 수 있다. 이 플래그는 오디오 또는 비디오 녹화와 같은 작업에만 사용해야 하며, 우선 순위가 높은 작업에만 사용하라.

액티티비가 메인 쓰레드의 이벤트 콜백 내에서 동시에 수행되는 경우 이 API를 사용할 필요가 없다.

이러한 액티비티를 장기간 종료하지 않을 경우 사용자 컴퓨터 성능에 큰 부정적인 영향을 미칠 수 있으므로 필요한 최소 시간만 사용하라. 사용자 환경설정이 애플리케이션의 요청을 무시할 수 있다.

또한 이 API를 사용하여 자동 종료 또는 갑작스러운 종료를 제어할 수 있다 \([Sudden Termination](https://developer.apple.com/documentation/foundation/processinfo#1651129) 참조\). 예를 들어:

```swift
let activity = NSProcessInfo.processInfo().beginActivityWithOptions(.AutomaticTerminationDisabled, reason: "Good Reason")
// Perform some work
NSProcessInfo.processInfo().endActivity(activity)
```

이는 다음과 동일하다:

```swift
NSProcessInfo.processInfo().disableAutomaticTermination("Good Reason")
// Perform some work
NSProcessInfo.processInfo().enableAutomaticTermination("Good Reason")
```

이 API는 객체를 반환하기 때문에 자동 종료 API를 사용할때보다 시작과 종료가 더 쉬워질 수 있다. [`endActivity(_:)`](https://developer.apple.com/documentation/foundation/processinfo/1411321-endactivity) 호출 전에 객체가 할당 해제되면 액티비티가 자동으로 종료된다.

이 API는 시스템 전체의 유휴 슬립을 비활성화하고 유휴 슬립을 표시하는 메커니즘을 제공한다. 이는 사용자 경험에 큰 영향을 미칠 수 있으므로 슬립을 비활성화하는 액티비티를 종료하는 것을 잊어선 안된다. \([`userInitiated`](https://developer.apple.com/documentation/foundation/processinfo/activityoptions/1412233-userinitiated)를 포함하여\).

#### Sudden Termination <a id="1651129"></a>

macOS 10.6 이상에는 가능한한 언제든지 애플리케이션을 종료하도록 요청하는 대신 시스템이 로그아웃하거나 종료할 수 있는 메커니즘이 포함된다.

애플리케이션은 글로벌 기반에서 이 기능을 활성화한 다음 갑작스런 종료를 허용하여 데이터 손상이나 사용자 경험 저하를 초래할 수 있는 작업 중에 가용성을 수동으로 재지정할 수 있다. 또는 애플리케이션이 이 기능을 수동으로 활성화하고 비활성화할 수 있다.



