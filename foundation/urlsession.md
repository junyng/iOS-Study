# URLSession

**네트워크 데이터 전송 태스크 그룹을 조정하는 객체**

### Declaration

```text
class URLSession : NSObject
```

> 경고 시스템에서 보조 스레드에 대한 세션 delegate 메서드로 메소드 urlSessionDidFinishEvents를 호출할 수 있다. 그러나 iOS에서 해당 방법을 구현하려면 application\(\_:handleEventsForBackgroundURLSession:completionHandler:\)에서 제공된 완료 handler를 호출해야 할 수 있다.

URL 세션 API 자체는 완전히 스레드에 안전하다. 어떤 스레드 컨텍스트에서도 세션과 태스크를 자유롭게 생성할 수 있다. delegate 메서드가 제공된 완료 handler을 호출하면 작업은 올바른 delegate 큐에서 자동으로 예약된다.

### Thread Safety

* 앱이 세션 또는 작업 객체를 복사할 때 동일한 객체를 반환하라
* 앱이 구성 객체를 복사하면 독립적으로 수정할 수 있는 새 복사본이 나온다.

세션 및 작업 객체는 다음과 같이 NSCopying 프로토콜을 준수한다.

### NSCopying Behavior

자세한 내용은 NSAppTransportSecurity의 Information Property List Key Reference를 참조하라.

iOS 9.0과 OS X 10.11에서 시작하여 URLSession으로 만들어진 모든 HTTP 연결에 대해 기본적으로 ATS\(App Transport Security\)라는 새로운 보안 기능이 활성화되어 있다. ATS는 HTTP 연결이 HTTPS\(RFC 2818\)를 사용하도록 요구한다.

### App Transport Security \(ATS\)

URLProtocol을 하위 분류하여 사용자 지정 네트워킹 프로토콜 및 URL 구성\(앱 개인용으로 사용\)에 대한 지원을 추가할 수도 있다.

URLSession은 HTTP/1.1 및 HTTP/2 프로토콜을 지원한다. HTTP/2 지원은 RFC 7540에서 설명한 대로 ALPN\(Application-Layer Protocol Analancement\)을 지원하는 서버를 필요로 한다.

URLSession 클래스는 기본적으로 데이터, 파일, ftp, http 및 https URL 체계를 지원하며, 사용자의 시스템 기본 설정에 구성된 프록시 서버 및 SOCKS 게이트웨이를 투명하게 지원한다.

### Protocol Support

URL 세션은 또한 작업을 취소, 재시작, 재개 및 일시중단 하도록 지원하며 일시 중단, 취소 또는 실패한 다운로드를 중지한 경우 다시 시작할 수 있는 기능을 제공한다.

URLSession API는 delegate에 이 정보를 전달할 뿐만 아니라 태스크의 현재 상태에 따라 프로그래밍 방식으로 결정해야할 경우 쿼리할 수 있는 상태 및 진행 상태 속성을 제공한다.\(그 상태가 언제든지 변할 수 있다는 점과 함께\)

URLSession API는 delegate에게 이 정보를 전달하는 것 외에 태스크의 현재 상태에 따라\(그 상태가 언제든지 변경될 수 있다는 주의와 함께\) 프로그래밍 방식으로 결정해야 할 경우 쿼리할 수 있는 상태 및 진행 속성을 제공한다.

* 전송이 성공적으로 완료되거나 오류가 발생한 경우 handler 블록을 호출한다.
* 데이터가 수신되고 전송이 완료될 때 세션 delegate에게 메소드를 호출한다.

대부분의 네트워킹 API와 마찬가지로 URLSession API도 비동기성이 강하다. 사용자가 호출하는 방법에 따라 다음 두가지 방법 중 하나로 데이터를 앱으로 반환한다.

### Asynchronicity and URL Sessions

> Important 세션 객체는 앱이 종료되거나 세션이 명시적으로 무효화될 때까지 delegate에 대한 강력한 참조를 유지한다. 세션을 무효화하지 않으면 앱이 종료될 때까지 메모리를 누출한다.

세션의 태스크는 또한 인증 실패, 서버로부터 데이터가 도착할 때, 데이터를 캐싱할 준비가 되었을 때 등 다양한 이벤트가 발생할 때 정보를 제공하고 얻을 수 있도록 하는 공통 delegate를 공유한다. delegate가 제공하는 기능이 필요하지 않으면 세션을 생성할 때 0을 전달하여 이 API를 사용할 수 있다.

### Using a Session Delegate

* Data task는 NSData 객체를 사용하여 데이터를 송수신 한다. 데이터 태스크는 서버에 대한 짧고 종종 상호 작용적인 요청을 위한 것이다.
* Upload task는 데이터 작업과 비슷하지만, 데이터\(종종 파일 형식\)을 전송하기도 하고, 앱이 실행되지 않는 동안 백그라운드 업로드를 지원하기도 한다.
* 다운로드 작업은 파일 형식으로 데이터를 검색하고, 앱이 실행되지 않는 동안 백그라운드 다운로드 및 업로드를 지원한다.

세션 내에서 데이터를 서버에 업로드한 다음 서버에서 데이터를 디스크의 파일 또는 메모리에 있는 하나 이상의 NSData객체로 검색하는 태스크를 생성하라. URLSession API는 세 가지 유형의 작업을 제공한다.

### Types of URL Sessions

URLSession API를 사용하여 앱에서 하나 이상의 세션을 생성하며, 각 세션은 관련 데이터 전송 작업 그룹을 조정한다. 예를 들어 웹 브라우저를 만드는 경우 앱이 tab 혹은 window 당 하나의 세션을 생성하거나 대화형 사용을 위한 세션과 다운로드를 위한 세션을 생성할 수 있다. 각 세션 내에서 앱은 일련의 작업을 추가하며, 각 작업은 특정 URL에 대한 요청을 나타낸다. \(필요할 경우 HTTP 리다이렉션에 따른다.\)

> Important URLSession API는 많은 다양한 클래스가 함께 작업하는 것을 포함하며, 레퍼런스 문서를 따로 읽으면 분명하지 않을 수 있다. API를 사용하기 전에 URL Loading System 항목의 개요를 읽어라. First Steps, Uploading, Downloading 섹션은 URLSession으로 일반적인 작업을 수행하는 예제를 제공한다.

URLSession 클래스 및 관련 클래스는 URL로 표시된 Endpoint에서 데이터를 다운로드하고 업로드하기 위한 API를 제공한다. 이 API는 또한 앱이 실행되지 않을 때 혹은 iOS에서 앱이 일시 중단된 동안 백그라운드 다운로드를 수행할 수 있게 해준다. 다양한 delegate 메서드가 인증을 지원하고 앱에 리다이렉션과 같은 이벤트를 통지할 수 있다.

### Overview

