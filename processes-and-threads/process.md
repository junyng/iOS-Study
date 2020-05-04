---
description: 현재 프로세스의 하위 프로세스를 나타내는 객체.
---

# Process

### Declaration

```text
class Process : NSObject
```

### Overview

[`Process`](https://developer.apple.com/documentation/foundation/process) 클래스를 사용하여 프로그램은 하위 프로세스로 다른 프로그램을 실행할 수 있으며 해당 프로그램의 실행을 모니터링할 수 있다. [`Process`](https://developer.apple.com/documentation/foundation/process) 객체는 별도의 실행 엔티티를 생성한다. 메모리 공간을 생성하는 프로세스와 공유하지 않는다는 점에서 [`Thread`](https://developer.apple.com/documentation/foundation/thread)와 다르다.

프로세스가 여러 항목에 대해 현재 값으로 정의된 환경 내에서 작동한다: 현재 디렉터리, 표준 입력, 표준 출력, 표준 오류 및 환경 변수의 값. 기본적으로 [`Process`](https://developer.apple.com/documentation/foundation/process) 객체는 프로세스를 시작하는 프로세스에서 환경을 상속한다. 프로세스에 대해 달라야 하는 값이 있는 경우, 예를 들어 현재 디렉터리가 변경되어야 한다면 해당 값을 실행하기 전에 변경해야 한다. 프로세스의 환경은 실행 중에 변경할 수 없다.

[`Process`](https://developer.apple.com/documentation/foundation/process) 객체는 한 번만 실행할 수 있다. 후속 시도는 에러를 발생한다.

> **중요**
>
> 샌드박스형 애플리케이션에서 [`Process`](https://developer.apple.com/documentation/foundation/process) 클래스로 생성된 하위 프로세스는 상위 앱의 샌드박스를 상속한다. XPC 서비스에서는 도우미 앱에 대해 서로 다른 샌드박스 사용 권한을 지정할 수 있으므로 일반적으로 도우미 애플리케이션을 XPC 서비스로 작성하라.

### Topics

#### Creating and Initializing an NSTask Object

* [`init()`](https://developer.apple.com/documentation/foundation/process/1415790-init) 현재 프로세스의 환경을 사용하여 초기화된 `NSTask` 객체를 반환한다.

#### Returning Task Information

* [`var arguments: [String]?`](https://developer.apple.com/documentation/foundation/process/1408983-arguments) 실행 파일을 시작하는 데 사용할 전달인자를 설정하라.
* [`var environment: [String : String]?`](https://developer.apple.com/documentation/foundation/process/1409412-environment) 수신기의 환경을 설정한다.
* [`var processIdentifier: Int32`](https://developer.apple.com/documentation/foundation/process/1412022-processidentifier) 수신기의 프로세스 식별자를 반환한다.
* [`var standardError: Any?`](https://developer.apple.com/documentation/foundation/process/1414916-standarderror) 수신기의 표준 에러를 설정한다.
* [`var standardInput: Any?`](https://developer.apple.com/documentation/foundation/process/1411576-standardinput) 수신기의 표준 입력을 설정한다.
* [`var standardOutput: Any?`](https://developer.apple.com/documentation/foundation/process/1407627-standardoutput) 수신기의 표준 출력을 설정한다.

#### Running and Stopping a Task

* [`func interrupt()`](https://developer.apple.com/documentation/foundation/process/1410874-interrupt) 인터럽트 신호를 수신기 및 모든 서브태스크에 보낸다.
* [`func resume() -> Bool`](https://developer.apple.com/documentation/foundation/process/1407819-resume) [`suspend()`](https://developer.apple.com/documentation/foundation/process/1411590-suspend) 메시지와 함께 이전에 일시 중단된 수신기 태스크의 실행을 재개한다.
* [`func suspend() -> Bool`](https://developer.apple.com/documentation/foundation/process/1411590-suspend) 수신기 태스크의 실행을 중지한다.
* [`func terminate()`](https://developer.apple.com/documentation/foundation/process/1409620-terminate) 종료 신호를 수신기와 모든 서브태스크로 전송한다.
* [`func waitUntilExit()`](https://developer.apple.com/documentation/foundation/process/1415808-waituntilexit) 수신기가 끝날 때까지 차단한다.

#### Querying the Task State

* [`var isRunning: Bool`](https://developer.apple.com/documentation/foundation/process/1415788-isrunning) 수신기가 아직 실행 중인지 여부를 반환한다.
* [`var terminationStatus: Int32`](https://developer.apple.com/documentation/foundation/process/1415801-terminationstatus) 수신기의 실행 파일이 반환한 종료 상태를 반환한다.
* [`var terminationReason: Process.TerminationReason`](https://developer.apple.com/documentation/foundation/process/1415605-terminationreason) 태스크가 종료된 이유를 반환한다. 

#### Configuring an NSTask Object

* [`var arguments: [String]?`](https://developer.apple.com/documentation/foundation/process/1408983-arguments) 실행 파일을 시작하는 데 사용할 명령 전달인자를 설정하라.
* [`var environment: [String : String]?`](https://developer.apple.com/documentation/foundation/process/1409412-environment) 수신기의 환경을 설정한다.
* [`var standardError: Any?`](https://developer.apple.com/documentation/foundation/process/1414916-standarderror) 수신기의 표준 에러를 설정한다.
* [`var standardInput: Any?`](https://developer.apple.com/documentation/foundation/process/1411576-standardinput) 수신기의 표준 입력을 설정한다.
* [`var standardOutput: Any?`](https://developer.apple.com/documentation/foundation/process/1407627-standardoutput) 수신기의 표준 출력을 설정한다.

#### Task Termination Handler

* [`var terminationHandler: ((Process) -> Void)?`](https://developer.apple.com/documentation/foundation/process/1408746-terminationhandler) 태스크가 완료되면 호출된다.

#### Constants

* [`enum Process.TerminationReason`](https://developer.apple.com/documentation/foundation/process/terminationreason) [`terminationReason`](https://developer.apple.com/documentation/foundation/process/1415605-terminationreason)로부터 반환된 값을 지정하는 상수.
* [`enum QualityOfService`](https://developer.apple.com/documentation/foundation/qualityofservice) 시스템에 대한 작업의 성격과 중요성을 나타내기 위해 사용된다. 서비스 클래스의 퀄리티가 더 높은 작업에서는 리소스 경합이 있을 때마다 서비스 클래스의 품질이 낮은 작업보다 더 많은 리소스를 받는다.

#### Notifications

[`class let didTerminateNotification: NSNotification.Name`](https://developer.apple.com/documentation/foundation/process/1413681-didterminatenotification)

태스크가 실행을 중지할 때 게시된다. 이 노티피케이션은 작업이 정상적으로 종료되었을 때 또는 [`terminate()`](https://developer.apple.com/documentation/foundation/process/1409620-terminate) 의 결과로 `NSTask` 객체로 전송된다. 그러나 `NSTask` 객체가 릴리즈되면 메시지가 전송되었을 포트가 태스크 릴리즈의 일부로 릴리즈되었기 때문에 이 노티피케이션은 전송되지 않는다. 옵저버 메서드는 태스크가 종료되었는지 결정하기 위해 [`terminationStatus`](https://developer.apple.com/documentation/foundation/process/1415801-terminationstatus)를 사용할 수 있다.

#### Instance Properties

* [`var currentDirectoryURL: URL?`](https://developer.apple.com/documentation/foundation/process/2890107-currentdirectoryurl)
* [`var executableURL: URL?`](https://developer.apple.com/documentation/foundation/process/2890106-executableurl)
* [`var qualityOfService: QualityOfService`](https://developer.apple.com/documentation/foundation/process/1415794-qualityofservice)

#### Instance Methods

* [`func run()`](https://developer.apple.com/documentation/foundation/process/2890105-run)

#### Type Methods

* [`class func run(URL, arguments: [String], terminationHandler: ((Process) -> Void)?) -> Process`](https://developer.apple.com/documentation/foundation/process/2890108-run)



