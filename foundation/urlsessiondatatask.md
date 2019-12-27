# URLSessionDataTask

다운로드한 데이터를 메모리에 있는 앱에 직접 반환하는 URL 세션 태스크

### Declaration

```swift
class URLSessionDataTask : URLSessionTask
```

### Overview

URLSessionDataTask는 [`URLSessionTask`](https://developer.apple.com/documentation/foundation/urlsessiontask)의 구체적인 하위 클래스이다. URLSessionDataTask의 클래스 메서드는 [`URLSessionTask`](https://developer.apple.com/documentation/foundation/urlsessiontask)에 문서화되어 있다.

데이터 태스크는 데이터를 하나 이상의 `NSData` 객체로 메모리에 직접 반환한다. 데이터 태스크 사용시:

* 바디 데이터를 업로드하는 동안 \(앱에서 제공하는 경우\) 세션은 주기적으로 델리게이트가 제공하는 메서드[`urlSession(_:task:didSendBodyData:totalBytesSent:totalBytesExpectedToSend:`](https://developer.apple.com/documentation/foundation/urlsessiontaskdelegate/1408299-urlsession)상태 정보로 호출한다.
* 초기 응답을 받은 세션은 델리게이트가 제공하는 [`urlSession(_:dataTask:didReceive:completionHandler:)`](https://developer.apple.com/documentation/foundation/urlsessiondatadelegate/1410027-urlsession) 메서드를 호출하여 상태 코드와 헤더를 검사하고 선택적으로 데이터 태스크를 다운로드 태스크로 변환할 수 있도록 한다.
* 전송하는 동안 세션은 앱이 도착하는 대로 콘텐츠를 제공하기 위해 델리게이트 메서드 [`urlSession(_:dataTask:didReceive:)`](https://developer.apple.com/documentation/foundation/urlsessiondatadelegate/1411528-urlsession) 를 호출한다.
* 세션이 완료되면, 응답의 캐시 여부를 결정할 수 있도록 델리게이트의 [`urlSession(_:dataTask:willCacheResponse:completionHandler:)`](https://developer.apple.com/documentation/foundation/urlsessiondatadelegate/1411612-urlsession) 메서드를 호출한다.

데이터 가져오기 및 업로드에 데이터 태스크를 사용하는 예는 [Fetching Website Data into Memory](https://developer.apple.com/documentation/foundation/url_loading_system/fetching_website_data_into_memory) 및 [Uploading Data to a Website](https://developer.apple.com/documentation/foundation/url_loading_system/uploading_data_to_a_website) 를 참조하라.

### Relationships

#### Inherits From

[`URLSessionTask`](https://developer.apple.com/documentation/foundation/urlsessiontask)

#### Conforms To

* [`CVarArg`](https://developer.apple.com/documentation/swift/cvararg), 
* [`Equatable`](https://developer.apple.com/documentation/swift/equatable), 
* [`Hashable`](https://developer.apple.com/documentation/swift/hashable)

### See Also

#### Adding Data Tasks to a Session

[`func dataTask(with: URL) -> URLSessionDataTask`](https://developer.apple.com/documentation/foundation/urlsession/1411554-datatask)

지정된 URL의 내용을 반환하는 태스크를 생성하라.[`func dataTask(with: URL, completionHandler: (Data?, URLResponse?, Error?) -> Void) -> URLSessionDataTask`](https://developer.apple.com/documentation/foundation/urlsession/1410330-datatask)

지정된 URL의 내용을 반환한 다음 완료 시 처리기를 호출하는 태스크를 생성하라.

[`func dataTask(with: URLRequest) -> URLSessionDataTask`](https://developer.apple.com/documentation/foundation/urlsession/1410592-datatask)

지정된 URL 요청 객체를 기반으로 URL의 내용을 반환하는 태스크를 생성하라.[`func dataTask(with: URLRequest, completionHandler: (Data?, URLResponse?, Error?) -> Void) -> URLSessionDataTask`](https://developer.apple.com/documentation/foundation/urlsession/1407613-datatask)

지정된 URL 요청 객체를 기반으로 URL의 내용을 반환하고 완료 시 처리기를 호출하는 태스크를 생성하라

[`protocol URLSessionDataDelegate`](https://developer.apple.com/documentation/foundation/urlsessiondatadelegate)

URL 세션 인스턴스가 대리자에게 데이터 및 업로드 태스크에 고유한 작업 수준 이벤트를 처리하도록 호출하는 메서드를 정의하는 프로토콜

