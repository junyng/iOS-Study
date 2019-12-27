# Uploading Data to a Website

앱에서 서버로 데이터를 Post 한다.

### Overview

많은 앱들은 이미지나 문서와 같은 파일 업로드를 받아들이는 서버나 JSON과 같은 구조화된 데이터를 받아들이는 웹 서비스 API 엔드포인트를 사용하는 서버들과 함께 작동한다. 앱에서 데이터를 업로드하려면 [`URLSession`](https://developer.apple.com/documentation/foundation/urlsession) 인스턴스를 사용하여 [`URLSessionUploadTask`](https://developer.apple.com/documentation/foundation/urlsessionuploadtask) 인스턴스를 생성하라. 업로드 태스크는 업로드 수행 방법을 자세히 설명하는 [`URLRequest`](https://developer.apple.com/documentation/foundation/urlrequest) 인스턴스를 사용한다.

#### Prepare Your Data for Upload <a id="2923847"></a>

업로드할 데이터는 Listing 1의 경우와 같이 파일, 스트림 또는 데이터의 콘텐츠일 수 있다.

많은 웹 서비스 엔드 포인트는 배열 및 딕셔너리와 같은 [`Encodable`](https://developer.apple.com/documentation/swift/encodable) 을 준수한 [`JSONEncoder`](https://developer.apple.com/documentation/foundation/jsonencoder) 클래스를 사용하여 JSON 형식 데이터를 작성한다. Listing 1에서 보듯이, [`Codable`](https://developer.apple.com/documentation/swift/codable) 을 준수하는 구조체를 선언하고 이 타입의 인스턴스를 생성하고 JSONEncoder를 사용하여 해당 인스턴스를 JSON 데이터로 인코딩하여 업로드할 수 있다.

**Listing 1** Preparing JSON data for upload

```swift
struct Order: Codable {
    let customerId: String
    let items: [String]
}

// ...

let order = Order(customerId: "12345",
                  items: ["Cheese pizza", "Diet soda"])
guard let uploadData = try? JSONEncoder().encode(order) else {
    return
}
```

JPEG 또는 PNG 데이터로 이미지를 인코딩하거나 UTF-8과 같은 인코딩을 사용하여 문자열을 데이터로 변환하는 것과 같은 데이터 인스턴스를 생성하는 다른 많은 방법이 있다.

#### Configure an Upload Request <a id="2923848"></a>

업로드 태스크에는 URLRequest 인스턴스가 필요하다. Listing 2와 같이, 리퀘스트의 [`httpMethod`](https://developer.apple.com/documentation/foundation/urlrequest/2011415-httpmethod) 프로퍼티를 "POST" 또는 "PUT"으로 설정한다. Content-Length 헤더를 제외하고 제공할 HTTP 헤더의 값을 설정하려면 [`setValue(_:forHTTPHeaderField:)`](https://developer.apple.com/documentation/foundation/urlrequest/2011447-setvalue) 메서드를 사용하라. 세션은 데이터 크기에서 자동으로 콘텐츠 길이를 계산한다.

**Listing 2** Configuring a URL request

```swift
let url = URL(string: "https://example.com/post")!
var request = URLRequest(url: url)
request.httpMethod = "POST"
request.setValue("application/json", forHTTPHeaderField: "Content-Type")
```

#### Create and Start an Upload Task <a id="2923849"></a>

업로드를 시작하려면 URLSession 인스턴스에서 [`uploadTask(with:from:completionHandler:)`](https://developer.apple.com/documentation/foundation/urlsession/1411518-uploadtask) 를 호출하여 업로드하는 URLSessionTask 인스턴스를 생성하고 리퀘스트와 이전에 설정한 데이터 인스턴스를 전달하라. 태스크는 일시 중단된 상태에서 시작되므로 태스크의 [`resume()`](https://developer.apple.com/documentation/foundation/urlsessiontask/1411121-resume) 을 호출하여 네트워크 로딩 프로세스를 시작하라. Listing 3은 공유 URLSession 인스턴스를 사용하며, 그 결과를 완료 핸들러로 수신한다.

**Listing 3** Starting an upload task

```swift
let task = URLSession.shared.uploadTask(with: request, from: uploadData) { data, response, error in
    if let error = error {
        print ("error: \(error)")
        return
    }
    guard let response = response as? HTTPURLResponse,
        (200...299).contains(response.statusCode) else {
        print ("server error")
        return
    }
    if let mimeType = response.mimeType,
        mimeType == "application/json",
        let data = data,
        let dataString = String(data: data, encoding: .utf8) {
        print ("got data: \(dataString)")
    }
}
task.resume()
```

#### Alternatively, Upload by Setting a Delegate <a id="2936057"></a>

완료 핸들러 접근 방식의 대안으로, 설정한 세션에 델리게이트를 설정한 다음[`uploadTask(with:from:)`](https://developer.apple.com/documentation/foundation/urlsession/1409763-uploadtask) 로 업로드 태스크를 생성할 수 있다. 이 시나리오에서는 [`URLSessionDelegate`](https://developer.apple.com/documentation/foundation/urlsessiondelegate) 및 [`URLSessionTaskDelegate`](https://developer.apple.com/documentation/foundation/urlsessiontaskdelegate) 프로토콜의 메서드를 구현하라. 이러한 메서드는 서버 리스폰스와 모든 데이터 또는 전송 에러를 수신한다.

### See Also

#### Uploading

[Uploading Streams of Data](https://developer.apple.com/documentation/foundation/url_loading_system/uploading_streams_of_data)

Send a stream of data to a server.

