# Fetching Website Data into Memory

URL 세션에서 데이터 태스크를 생성하여 데이터를 메모리로 직접 수신하라.

### Overview

원격 서버와의 작은 상호 작용을 위해서, [`URLSessionDataTask`](https://developer.apple.com/documentation/foundation/urlsessiondatatask) 클래스를 사용하여 응답 데이터를 메모리로 수신할 수 있다. \(데이터를 파일 시스템에 직접 저장하는 [`URLSessionDownloadTask`](https://developer.apple.com/documentation/foundation/urlsessiondownloadtask) 클래스를 사용하는 대신\). 데이터 태스크는 웹 서비스 엔드포인트를 호출하는 것과 같은 용도에 이상적이다.

URL 세션 인스턴스를 사용하여 태스크를 생성하라. 당신의 요구가 꽤 간단하다면, 당신은 [`URLSession`](https://developer.apple.com/documentation/foundation/urlsession) 클래스의 [`shared`](https://developer.apple.com/documentation/foundation/urlsession/1409000-shared) 인스턴스를 사용할 수 있다. 델리게이트 콜백을 통해 전송과 상호 작용하려면 공유 인스턴스를 사용하는 대신 세션을 생성하라. 세션을 만들 때 [`URLSessionConfiguration`](https://developer.apple.com/documentation/foundation/urlsessionconfiguration) 인스턴스를 사용하며, [`URLSessionDelegate`](https://developer.apple.com/documentation/foundation/urlsessiondelegate) 또는 그 하위 프로토콜 중 하나를 구현한 클래스도 전달한다. 세션을 재사용하여 여러 태스크를 작성할 수 있으므로 필요한 각 고유 설정에 대한 세션을 작성하고 프로퍼티로 저장한다.

> **참고**
>
> 필요한 것보다 더 많은 세션을 만들지 않도록 주의하라. 예를 들어, 유사하게 설정된 세션이 필요한 앱의 여러 부분이 있는 경우 하나의 세션을 생성하고 공유하라.

세션이 있으면 `dataTask()` 메서드 중 하나를 사용하여 데이터 태스크를 생성하라. 태스크는 일시 중단된 상태로 생성되며, [`resume()`](https://developer.apple.com/documentation/foundation/urlsessiontask/1411121-resume)를 호출하여 시작할 수 있다.

#### Receive Results with a Completion Handler <a id="2919375"></a>

데이터를 가져오는 가장 간단한 방법은 완료 핸들러를 사용하는 데이터 태스크를 생성하는 것이다. 이 배치를 통해 태스크는 사용자가 제공하는 완료 핸들러 블록에 서버의 리스폰스, 데이터 및 에러를 전송한다. Figure 1은 세션과 태스크 사이의 관계, 그리고 결과가 완료 핸들러에 어떻게 전달되는지를 보여준다.

**Figure 1** Creating a completion handler to receive results from a task

![](../.gitbook/assets/receive_results_from_task.png)

완료 핸들러를 사용하는 데이터 태스크를 생성하려면 URLSession의 [`dataTask(with:)`](https://developer.apple.com/documentation/foundation/urlsession/1411554-datatask) 메서드를 호출하라. 당신의 완료 핸들러는 세 가지 일을 해야 한다.

1. `error` 매개변수가 `nil` 인지 확인하라. 그렇지 않은 경우 전송 에러가 발생하여 에러를 처리하고 종료하라.
2. `response` 매개변수를 확인하여 상태 코드가 성공을 나타내며 MIME 타입이 예측 가능한 값인지 확인하라. 그렇지 않은 경우 서버 에러를 처리하고 종료하라.
3. 필요에 따라 `data` 인스턴스를 사용하라.

Listing 1은 URL 콘텐츠를 가져오기 위한 `startLoad()` 메서드를 보여준다. URLSession 클래스의 공유 인스턴스를 사용하여 결과를 완료 핸들러에 전달하는 데이터 태스크를 생성하는 것으로 시작한다. 이 핸들러는 로컬 및 서버 에러를 확인한 후 데이터를 문자열로 변환하여 WKWebView 아울렛을 채우는 데 사용한다. 물론, 데이터 모델로 파싱하는 것과 같이 당신의 앱에서 가져온 데이터에 다른 용도도 있을 수 있다.

**Listing 1** Creating a completion handler to receive data-loading results

```swift
func startLoad() {
    let url = URL(string: "https://www.example.com/")!
    let task = URLSession.shared.dataTask(with: url) { data, response, error in
        if let error = error {
            self.handleClientError(error)
            return
        }
        guard let httpResponse = response as? HTTPURLResponse,
            (200...299).contains(httpResponse.statusCode) else {
            self.handleServerError(response)
            return
        }
        if let mimeType = httpResponse.mimeType, mimeType == "text/html",
            let data = data,
            let string = String(data: data, encoding: .utf8) {
            DispatchQueue.main.async {
                self.webView.loadHTMLString(string, baseURL: url)
            }
        }
    }
    task.resume()
}
```

> **중요**
>
> 완료 핸들러는 태스크를 생성한 처리기와는 다른 Grand Central Dispatch 큐에서 호출된다. 따라서 데이터 또는 에러를 사용하여 웹 뷰 업데이트와 같은 UI를 업데이트하는 작업은 여기에 표시된 것처럼 메인 큐에 명시적으로 배치해야 한다.

#### Receive Transfer Details and Results with a Delegate <a id="2919376"></a>

데이터 태스크를 생성할 때 태스크와 활동에 대한 보다 높은 수준의 접근을 위해 완료 핸들러를 제공하는 대신 세션 델리게이트를 설정할 수 있다. Figure 2는 이 배치를 보여준다.

**Figure 2** Implementing a delegate to receive results from a task

![](../.gitbook/assets/receive_results_from_a_task.png)

이 접근 방식을 통해 데이터의 일부가 도착하는 대로 에러로 인해 전송이 완료되거나 실패할 때까지 [`URLSessionDataDelegate`](https://developer.apple.com/documentation/foundation/urlsessiondatadelegate)의 [`urlSession(_:dataTask:didReceive:)`](https://developer.apple.com/documentation/foundation/urlsessiondatadelegate/1411528-urlsession) 메서드에 제공된다. 델리게이트는 또한 전송이 진행됨에 따라 다른 종류의 이벤트도 받는다. 델리게이트 접근 방식을 사용할 때 URLSession 클래스의 간단한 공유 인스턴스를 사용하는 대신 고유한 URLSession 인스턴스를 생성해야 한다. 새로운 세션을 만들면 Listing 2에서와 같이 자신의 클래스를 세션의 델리게이트로 설정할 수 있다.

클래스가 하나 이상의 델리게이트 프로토콜을 구현한다고 선언하라. \([`URLSessionDelegate`](https://developer.apple.com/documentation/foundation/urlsessiondelegate), [`URLSessionTaskDelegate`](https://developer.apple.com/documentation/foundation/urlsessiontaskdelegate), [`URLSessionDataDelegate`](https://developer.apple.com/documentation/foundation/urlsessiondatadelegate) 및 [`URLSessionDownloadDelegate`](https://developer.apple.com/documentation/foundation/urlsessiondownloaddelegate)\) 그런 다음 이니셜라이저 [`init(configuration:delegate:delegateQueue:)`](https://developer.apple.com/documentation/foundation/urlsession/1411597-init) 를 통해 URL 세션 인스턴스를 생성하라. 이 이니셜라이저에 사용되는 설정 인스턴스를 사용자 정의할 수 있다. 예를 들어 [`waitsForConnectivity`](https://developer.apple.com/documentation/foundation/urlsessionconfiguration/2908812-waitsforconnectivity) 를 `true` 로 설정하는 것이 좋다. 이렇게 하면 세션은 필요한 연결을 사용할 수 없는 경우 즉시 실패하기보다는 적절한 연결을 기다린다.

**Listing 2** Creating a URLSession that uses a delegate

```swift
private lazy var session: URLSession = {
    let configuration = URLSessionConfiguration.default
    configuration.waitsForConnectivity = true
    return URLSession(configuration: configuration,
                      delegate: self, delegateQueue: nil)
}()
```

Listing 3은 이 세션을 사용하여 데이터 태스크를 시작하는 `startLoad()` 메서드를 보여주고, 수신된 데이터와 에러를 처리하기 위해 델리게이트 콜백을 사용한다. 이 목록은 세 개의 델리게이트 콜백을 구현한다:

* [`urlSession(_:dataTask:didReceive:completionHandler:)`](https://developer.apple.com/documentation/foundation/urlsessiondatadelegate/1410027-urlsession) 는 리스폰스에 성공한 HTTP 상태 코드가 있고 MIME 타입이 `text/html` 또는 `text/plain` 인지 확인한다. 다음 중 하나가 아닌 경우 태스크가 취소되고 그렇지 않으면 계속 진행할 수 있다.
* [`urlSession(_:dataTask:didReceive:)`](https://developer.apple.com/documentation/foundation/urlsessiondatadelegate/1411528-urlsession) 은 태스크가 수신하는 각 데이터 인스턴스를 가져와서 수신 데이터라는 버퍼에 추가한다.
* [`urlSession(_:task:didCompleteWithError:)`](https://developer.apple.com/documentation/foundation/urlsessiontaskdelegate/1411610-urlsession) 은 먼저 전송 레벨 에러가 발생했는지 확인한다. 에러가 없는 경우 `receivedData` 버퍼를 문자열로 변환하여 `webView` 의 내용으로 설정하려고 시도한다.

**Listing 3**  Using a delegate with a URL session data task

```swift
var receivedData: Data?

func startLoad() {
    loadButton.isEnabled = false
    let url = URL(string: "https://www.example.com/")!
    receivedData = Data()
    let task = session.dataTask(with: url)
    task.resume()
}

// delegate methods

func urlSession(_ session: URLSession, dataTask: URLSessionDataTask, didReceive response: URLResponse,
                completionHandler: @escaping (URLSession.ResponseDisposition) -> Void) {
    guard let response = response as? HTTPURLResponse,
        (200...299).contains(response.statusCode),
        let mimeType = response.mimeType,
        mimeType == "text/html" else {
        completionHandler(.cancel)
        return
    }
    completionHandler(.allow)
}

func urlSession(_ session: URLSession, dataTask: URLSessionDataTask, didReceive data: Data) {
    self.receivedData?.append(data)
}

func urlSession(_ session: URLSession, task: URLSessionTask, didCompleteWithError error: Error?) {
    DispatchQueue.main.async {
        self.loadButton.isEnabled = true
        if let error = error {
            handleClientError(error)
        } else if let receivedData = self.receivedData,
            let string = String(data: receivedData, encoding: .utf8) {
            self.webView.loadHTMLString(string, baseURL: task.currentRequest?.url)
        }
    }
}
```

다양한 델리게이트 프로토콜은 상기 코드에 제시된 것 이상으로 인증 문제 처리, 리다이렉션 및 기타 사례에 대한 메서드를 제공한다. URL 세션을 사용하여 URLSession 토론에서 전송 중에 발생할 수 있는 다양한 콜백을 설명한다.

### See Also

#### Essentials

[`class URLSession`](https://developer.apple.com/documentation/foundation/urlsession)

네트워크 데이터 전송에 관련된 태스크 그룹을 조정하는 객체이다.

[`class URLSessionTask`](https://developer.apple.com/documentation/foundation/urlsessiontask)

특정 리소스를 다운로드하는 것과 같은 태스크는 URL 세션에서 수행된다.

