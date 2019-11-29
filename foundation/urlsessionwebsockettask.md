---
description: WebSockets 프로토콜 표준을 통해 통신하는 태스크
---

# URLSessionWebSocketTask

### Declaration

```swift
class URLSessionWebSocketTask : URLSessionTask
```

### Overview

`URLSessionWebSocketTask`는 WebSocket 프레임의 형태로 TCP 및 TLS를 통한 메시지 지향 전송 프로토콜을 제공하는 [`URLSessionTask`](https://developer.apple.com/documentation/foundation/urlsessiontask)의 구체적인 하위 클래스이다. 그것은 [`RFC6455`](https://tools.ietf.org/html/rfc6455)에 정의된 WebSocket 프로토콜을 따른다.

`ws:` 또는 `wss:` URL을 사용하여 [`URLSessionWebSocketTask`](https://developer.apple.com/documentation/foundation/urlsessionwebsockettask)를 생성한다. 태스크를 생성할 때 핸드셰이크 단계 중 알릴 프로토콜 목록을 제공할 수도 있다. 핸드셰이크가 완료되면 앱은 세션의 [`delegate`](https://developer.apple.com/documentation/foundation/urlsession/1411530-delegate)를 통해 알림을 수신한다.

[`send(_:completionHandler:)`](https://developer.apple.com/documentation/foundation/urlsessionwebsockettask/3281790-send)으로 데이터를 보내고 [`receive(completionHandler:)`](https://developer.apple.com/documentation/foundation/urlsessionwebsockettask/3281789-receive)으로 데이터를 수신한다. 이 태스크는 읽기 및 쓰기를 비동기식으로 수행하며, 사용자가 이진 프레임과 UTF-8로 인코딩된 텍스트 프레임을 모두 포함하는 메시지를 보내고 받을 수 있도록 한다. 이 태스크는 핸드셰이크가 완료되기 전에 수행하는 모든 읽기 또는 쓰기를 큐에 넣고 완료된 후 실행한다. 

`URLSessionWebSocketTask`는 [`URLSessionTaskDelegate`](https://developer.apple.com/documentation/foundation/urlsessiontaskdelegate)의 메서드를 사용하여 다른 유형의 태스크처럼 리다이렉션 및 인증을 지원한다. WebSocket 태스크는 핸드셰이크를 완료하기 전에 리다이렉션 및 인증 위임 메서드를 호출한다. WebSocket 태스크는 세션 구성의 [`httpCookieStorage`](https://developer.apple.com/documentation/foundation/urlsessionconfiguration/1411599-httpcookiestorage)에 쿠키를 저장하여 쿠키를 지원하고 나가는 HTTP 핸드셰이크 요청에 쿠키를 붙인다.

### Topics

#### Sending and Receiving Data

[`func send(URLSessionWebSocketTask.Message, completionHandler: (Error?) -> Void)`](https://developer.apple.com/documentation/foundation/urlsessionwebsockettask/3281790-send)

WebSocket 메시지를 전송하고, 결과를 완료 핸들러로 수신한다.

[`enum URLSessionWebSocketTask.Message`](https://developer.apple.com/documentation/foundation/urlsessionwebsockettask/message)

송수신된 메시지 유형 열거형

[`func receive(completionHandler: (Result<URLSessionWebSocketTask.Message, Error>) -> Void)`](https://developer.apple.com/documentation/foundation/urlsessionwebsockettask/3281789-receive)

메시지의 모든 프레임을 사용할 수 있는 경우 WebSocket 메시지를 읽는다.

[`var maximumMessageSize: Int`](https://developer.apple.com/documentation/foundation/urlsessionwebsockettask/3181203-maximummessagesize)

에러로 수신 호출이 실패하기 전의 버퍼링할 최대 바이트 수

#### Sending Ping Frames

[`func sendPing(pongReceiveHandler: (Error?) -> Void)`](https://developer.apple.com/documentation/foundation/urlsessionwebsockettask/3181206-sendping)

서버 엔드포인트에서 pong을 수신하기 위해 닫힌 상태로 클라이언트 측에서 ping 프레임을 전송한다.

#### Closing the Connection

[`func cancel(with: URLSessionWebSocketTask.CloseCode, reason: Data?)`](https://developer.apple.com/documentation/foundation/urlsessionwebsockettask/3181200-cancel)주어진 close code와 옵셔널 close reason을 가진 close 프레임을 전송한다.

[`var closeCode: URLSessionWebSocketTask.CloseCode`](https://developer.apple.com/documentation/foundation/urlsessionwebsockettask/3181201-closecode)

연결이 닫힌 이유를 나타내는 코드.

[`enum URLSessionWebSocketTask.CloseCode`](https://developer.apple.com/documentation/foundation/urlsessionwebsockettask/closecode)

WebSocket 연결이 닫힌 이유를 나타내는 코드.

[`var closeReason: Data?`](https://developer.apple.com/documentation/foundation/urlsessionwebsockettask/3181202-closereason)

연결이 닫힌 이유에 대한 추가 정보를 제공하는 데이터 블록.

### Relationships

#### Inherits From

[`URLSessionTask`](https://developer.apple.com/documentation/foundation/urlsessiontask)

#### Conforms To

[`CVarArg`](https://developer.apple.com/documentation/swift/cvararg), [`Equatable`](https://developer.apple.com/documentation/swift/equatable), [`Hashable`](https://developer.apple.com/documentation/swift/hashable)

### See Also

#### Adding WebSocket Tasks to a Session

[`func webSocketTask(with: URL) -> URLSessionWebSocketTask`](https://developer.apple.com/documentation/foundation/urlsession/3181171-websockettask)

제공된 URL에 대한 WebSocket 태스크를 생성한다.[`func webSocketTask(with: URLRequest) -> URLSessionWebSocketTask`](https://developer.apple.com/documentation/foundation/urlsession/3235750-websockettask)

제공된 URL 요청에 대한 WebSocket 태스크를 생성한다.[`func webSocketTask(with: URL, protocols: [String]) -> URLSessionWebSocketTask`](https://developer.apple.com/documentation/foundation/urlsession/3181172-websockettask)

URL 및 프로토콜 배열이 지정된 WebSocket 태스크를 생성한다.[`protocol URLSessionWebSocketDelegate`](https://developer.apple.com/documentation/foundation/urlsessionwebsocketdelegate)

URL 세션 인스턴스가 WebSocket 태스크와 관련된 태스크 수준 이벤트를 처리하도록 위임자에게 요청하는 방법을 정의하는 프로토콜.



