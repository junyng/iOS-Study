---
description: 객체에 다른 객체의 속성에 대한 변경 사항을 통지하라.
---

# Using Key-Value Observing in Swift

### Overview

Key-value 관찰은 다른 객체의 속성 변화에 대해 객체를 알리는 데 사용하는 코코아 프로그래밍 패턴이다. 이는 모델과 뷰 등 앱의 논리적으로 분리된 부분 간에 변경 사항을 전달하는 데 유용하다. `NSObject`에서 상속되는 클래스와 함께 관찰하는 Kay-value만 사용할 수 있다.

#### Annotate a Property for Key-Value Observing <a id="2990176"></a>

@objc 속성과 다이나믹 수식어를 모두 사용하여 key-value를 통해 관찰할 속성을 표시하라. 아래 예제에서는 관찰할 수 있는 속성 `myDate`가 포함된 `MyObjectToObserve` 클래스를 정의하라.

```swift
class MyObjectToObserve: NSObject {
    @objc dynamic var myDate = NSDate(timeIntervalSince1970: 0) // 1970
    func updateDate() {
        myDate = myDate.addingTimeInterval(Double(2 << 30)) // Adds about 68 years.
    }
}
```

#### Define an Observer <a id="2990177"></a>

관찰자 클래스는 인스턴스 하나 이상의 속성에 대한 변경에 대한 정보를 관리한다. 관찰자를 만들 때 관찰하고자 하는 키 경로를 사용하여 `observe(_:options:changeHandler:)` 메서드를 호출하여 관찰을 시작한다.

아래의 예에서 `\.objectToObserve.myDate` 키 경로는 `MyObjectToObserve`의 `myDate` 속성을 가리킨다.

```swift
class MyObserver: NSObject {
    @objc var objectToObserve: MyObjectToObserve
    var observation: NSKeyValueObservation?
    
    init(object: MyObjectToObserve) {
        objectToObserve = object
        super.init()
        
        observation = observe(
            \.objectToObserve.myDate,
            options: [.old, .new]
        ) { object, change in
            print("myDate changed from: \(change.oldValue!), updated to: \(change.newValue!)")
        }
    }
}
```

[`NSKeyValueObservedChange`](https://developer.apple.com/documentation/foundation/nskeyvalueobservedchange) 인스턴스의 이전 값 및 새 값 속성을 사용하여 관찰 중인 속성에 대해 변경된 내용을 확인하라.

속성이 어떻게 변경되었는지 알 필요가 없는 경우 `options` 매개 변수를 생략하라. `options` 매개 변수를 생략하면 새 속성 값과 이전 속성 값이 저장되므로 이전 값 및 새 값 속성은 `nil`이 된다.

#### Associate the Observer with the Property to Observe <a id="2990173"></a>

관찰할 속성을 관찰자의 이니셜라이저에 객체를 전달하여 해당 관찰자와 연결하라.

```swift
let observed = MyObjectToObserve()
let observer = MyObserver(object: observed)
```

#### Respond to a Property Change <a id="2990175"></a>

위와 같이 key-value 관찰을 사용하도록 설정된 객체는 관찰자에게 속성 변경에 대해 통지한다. 아래 예제는 `updateDate` 메서드를 호출하여 `myDate` 속성을 변경한다. 이 메서드는 관찰자의 변경 핸들러를 자동으로 트리거한다.

```swift
observed.updateDate() // Triggers the observer's change handler.
// Prints "myDate changed from: 1970-01-01 00:00:00 +0000, updated to: 2038-01-19 03:14:08 +0000"
```

위의 예는 날짜의 새 값과 이전 값을 모두 출력하여 속성 변화에 응답한다.

