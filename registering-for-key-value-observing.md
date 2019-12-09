# Registering for Key-Value Observing

키-값 관찰은 객체가 다른 객체의 특정 특성에 대한 변경사항을 통지받을 수 있도록 하는 메커니즘이다.

* [addObserver:forKeyPath:options:context:](https://developer.apple.com/documentation/objectivec/nsobject/1412787-addobserver) 메서드를 사용하여 관찰된 객체에 관찰자를 등록하라.
* 변경 통지 메시지를 수락하기 위해 관찰자 내부에 [observeValueForKeyPath:ofObject:change:context:](https://developer.apple.com/documentation/objectivec/nsobject/1416553-observevalueforkeypath) 를 구현한다.
* 메시지를 더 이상 수신하지 않아야 할 때 [removeObserver:forKeyPath:](https://developer.apple.com/documentation/objectivec/nsobject/1408054-removeobserver) 메서드를 사용한다. 최소한 관찰자가 메모리에서 해제되기 전에 이 메서드를 호출하라.

> **중요**: 모든 클래스가 모든 프로퍼티에 대해 KVO를 준수하는 것은 아니다. [KVO Compliance](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/KeyValueObserving/Articles/KVOCompliance.html#//apple_ref/doc/uid/20002178-BAJEAIEE)에 설명된 단계에 따라 자신의 클래스가 KVO를 준수하는지 확인할 수 있다. 일반적으로 애플이 제공하는 프레임워크의 프로퍼티는 다음과 같이 문서화된 경우에만 KVO를 준수한다.

### Registering as an Observer

관측 객체는 먼저 `addObserver:forKeyPath:options:context:` message 를 전송하여 관찰된 객체에 자신을 등록하고 관찰한 프로퍼티의 경로를 전달한다. 관찰자는 알림의 측면을 관리하기 위해 옵션 매개 변수 및 컨텍스트 포인터를 추가로 지정한다.

#### Options

옵션 매개변수 \(옵션 상수 비트 연산 `OR`\)는 알림에 제공된 변경 딕셔너리의 내용에 모두 영향을 미치며, 알림이 생성되는 방식이다. `NSKeyValueObservingOptionOld` 옵션을 지정하여 변경하기 전에 관찰된 프로퍼티의 값을 수신하도록 선택한다. `NSKeyValueObservingOptionNew` 옵션을 사용하여 프로퍼의 새로운 값을 요청하라. 이러한 옵션의 비트 연산 OR 을 사용하여 이전 값과 새로운 값을 모두 수신한다.

관찰된 객체에 `NSKeyValueObservingOptionInitial` 옵션과 함께 즉시 변경 알림 \(이전에`addObserver:forKeyPath:options:context:` returns\)을 보내도록 초기에 지시한다. 추가적인 일회성 알림을 사용하여 관찰자 프로퍼티의 초기 값을 설정할 수 있다.

`NSKeyValueObservingOptionPrior` 옵션을 포함하여 관찰된 객체에서 프로퍼티 변경 바로 전에 알림을 보내도록 지시한다. 변경 딕셔너리는 `NSKeyValueChangeNotificationIsPriorKey` 를 `NSNumber` 래핑 `YES` 값에 포함시킴으로써 변경 전 통지를 나타낸다. 그 키는 달리 존재하지 않는다. 관찰자 자신의 KVO 규정 준수가 관찰된 프로퍼티에 의존하는 프로퍼티 중 하나에 대해 -`willChange…`   메서드 중 하나를 호출하도록 요구하는 경우 변경 전 통지를 사용할 수 있다. 통상적인 변경 후 통지는 너무 늦게 와서 시간 내에 -`willChange…` 를 호출할 수 없다.

#### Context

`addObserver:forKeyPath:options:context:` 메시지의 컨텍스트 포인터에는 해당 변경 알림에서 관찰자에게 다시 전달될 임의 데이터가 포함되어 있다. 변경 통지의 원점을 결정하기 위해 `NULL` 을 지정하고 키 경로 문자열에 전적으로 의존할 수 있지만, 이 접근방식은 다른 이유로 인해 슈퍼클래스 또한 동일한 키 경로를 관찰하고 있는 객체에 문제를 일으킬 수 있다.

더 안전하고 확장 가능한 접근법은 당신이 받은 통지가 슈퍼클래스가 아닌 당신의 관찰자에게 운명지어진다는 것을 확실히 하기 위해 컨텍스트를 사용하는 것이다.

고유하게 이름 지어진 정적 변수의 주소가 좋은 컨텍스트를 만든다. 슈퍼 클래스 또는 하위 클래스에서 유사한 방식으로 선택한 컨테스트가 겹치지 않는다. 전체 클래스에 대해 단일 컨텍스트를 선택하고 알림 메시지의 키 경로 문자열을 사용하여 변경된 내용을 확인한다. 또는 관찰된 각 키 경로에 대해 고유한 컨텍스트를 만들 수 있으며, 이 컨텍스트는 문자열 비교의 필요성을 완전히 우회하여 보다 효율적인 알림 구문 분석으로 이어질 수 있다. Listing 1은 이 방법으로 선택한 `balance` 및 `interestRate` 프로퍼티에 대한 예제 컨텍스트를 보여준다.

**Listing 1**  Creating context pointers

```objectivec
static void *PersonAccountBalanceContext = &PersonAccountBalanceContext;
static void *PersonAccountInterestRateContext = &PersonAccountInterestRateContext;
```

Listing 2의 예는 주어진 컨텍스트 포인터를 사용하여 `Account` 인스턴스 `balance` 와 `interestRate` 프로퍼티에 대해 개인 인스턴스가 관찰자로 등록하는 방법을 보여준다.

**Listing 2**  Registering the inspector as an observer of the balance and interestRate properties

```objectivec
- (void)registerAsObserverForAccount:(Account*)account {
    [account addObserver:self
              forKeyPath:@"balance"
                 options:(NSKeyValueObservingOptionNew |
                          NSKeyValueObservingOptionOld)
                 context:PersonAccountBalanceContext];
 
    [account addObserver:self
              forKeyPath:@"interestRate"
                 options:(NSKeyValueObservingOptionNew |
                          NSKeyValueObservingOptionOld)
                  context:PersonAccountInterestRateContext];
}
```

> 참고: 키-값 관찰 `addObserver:forKeyPath:options:context:` 메서드는 관찰하는 객체 또는 컨텍스트에 강한 참조를 유지하지 않는다. 관찰 및 관찰 대상 및 컨텍스트에 대한 강한 참조를 필요에 따라 유지하라.

### Receiving Notification of a Change

객체의 관찰된 프로퍼티 값이 변경되는 경우, 관찰자는 `observeValueForKeyPath:ofObject:change:context:` 메시지를 받는다. 모든 관찰자는 반드시 이 메서드를 실행해야 한다.

관측 객체는 관련 객체로서 알림을 트리거한 키 경로, 변경사항에 대한 세부사항을 포함한 딕셔너리 및 관찰자가 이 키 경로에 등록되었을 때 제공된 컨텍스트 포인터를 제공한다.

변경 딕셔너리 항목 `NSKeyValueChangeKindKey` 는 발생한 변경의 유형에 대한 정보를 제공한다. 관찰된 객체의 값이 변경된 경우, `NSKeyValueChangeKindKey` 항목이 `NSKeyValueChangeSetting` 를 반환한다. 관찰자가 등록되었을 때 지정한 옵션에 따라 변경 딕셔너리의 `NSKeyValueChangeOldKey` 및 `NSKeyValueChangeNewKey` 항목을 변경 전/후의 프로퍼티 값을 포함한다. 프로퍼티가 객체인 경우 값을 직접 제공한다. 프로퍼티가 스칼라 또는 C 구조체인 경우 값은 NSValue 객체에 래핑되어 있다. \(키-값 코딩과 마찬가지로\)

관측된 프로퍼티가 일대다 관계인 경우, `NSKeyValueChangeKindKey` 항목은 또한 관계에 있는 객체가 각각 `NSKeyValueChangeInsertion`, `NSKeyValueChangeRemove` 또는 `NSKeyValueChangeReplacement`를 반환하여 삽입, 제거 또는 대체되었는지 여부를 나타낸다.

`NSKeyValueChangeIndexesKey`의 변경 딕셔너리 항목은 변경된 관계의 인덱스를 지정하는 `NSIndexSet` 객체다. `NSKeyValueObservingOptionNew` 또는 `NSKeyValueObservingOptionOld`가 관찰자가 등록될 때 옵션으로 지정된 경우 변경 딕셔너리의 `NSKeyValueChangeOldKey` 및 `NSKeyValueChangeNewKey` 항목은 변경 전후에 관련된 객체의 값을 포함하는 배열이다.

Listing 3의 예제에서는 Person 관찰자를 위한 `surveyValueForKeyPath:ofObject:change:context:context:` 구현을 보여준다. 이 값은 Listing 2 에 등록된 프로퍼티 `balance` 및 `interestRate` 의 이전 값과 새로운 값을 기록한다.

**Listing 3**  Implementation of observeValueForKeyPath:ofObject:change:context:

```objectivec
- (void)observeValueForKeyPath:(NSString *)keyPath
                      ofObject:(id)object
                        change:(NSDictionary *)change
                       context:(void *)context {
 
    if (context == PersonAccountBalanceContext) {
        // Do something with the balance…
 
    } else if (context == PersonAccountInterestRateContext) {
        // Do something with the interest rate…
 
    } else {
        // Any unrecognized context must belong to super
        [super observeValueForKeyPath:keyPath
                             ofObject:object
                               change:change
                               context:context];
    }
}
```

관찰자를 등록할 때 `NULL` 컨텍스트를 지정한 경우, 관찰 중인 주요 경로와 알림 키 경로를 비교하여 변경된 내용을 확인하라. 관찰된 모든 키 경로에 대해 단일 컨텍스트를 사용한 경우 먼저 알림의 컨텍스트에 대해 테스트하고 일치 항목을 찾은 다음 키 경로 문자열 비교를 사용하여 구체적으로 변경된 내용을 확인하라. 여기서 설명한 것처럼 각 키 경로에 대해 고유한 컨텍스트를 제공한 경우 일련의 단순 포인터 비교는 이 관찰자에 대한 알림이 있는지 여부와 변경된 키 경로를 동시에 알려준다.

어떤 경우에도, 관찰자는 컨텍스트를 인식하지 못하는 경우\(또는 간단한 경우, 키 경로 중 하나\) 항상 슈퍼클래스의 `observeValueForKeyPath:ofObject:change:context:` 구현을 호출한다. 왜냐하면 이것은 슈퍼클래스가 알림에도 등록되었다는 것을 의미하기 때문이다.

> 참고: 알림이 클래스 계층의 상단으로 전파되는 경우, `NSObject`는 프로그래밍 오류로 인해 `NSInternalInconsistencyException` 을 실행한다. 이는 하위 클래스가 등록한 알림을 사용하지 못했기 때문이다.

### Removing an Object as an Observer

관찰된 객체에 `removeObserver:forKeyPath:context:` 메시지를 보냄으로 키-값 관찰자를 제거하고, 관찰 객체, 키 경로 및 컨텍스트를 지정한다. Listing 4의 예는 `Person` 이 `balance`와 `interestRate` 의 관찰자로서 그 자신을 제거하는 것을 보여준다.

**Listing 4**  Removing the inspector as an observer of balance and interestRate

```objectivec
- (void)unregisterAsObserverForAccount:(Account*)account {
    [account removeObserver:self
                 forKeyPath:@"balance"
                    context:PersonAccountBalanceContext];
 
    [account removeObserver:self
                 forKeyPath:@"interestRate"
                    context:PersonAccountInterestRateContext];
}
```

`removeObserver:forKeyPath:context:` 메시지를 수신한 후 관찰 객체는 더 이상 지정된 키 경로 및 객체에 대한 어떤 `observeValueForKeyPath:ofObject:change:context:` 메시지도 수신하지 않는다.

관찰자를 제거할 때 몇 가지 사항을 유념하라.

* 만약 이미 `NSRangeException`의 하나의 결과로 등록되지 않은 경우 관찰자로 제거를 요청한다. `removeObserver:forKeyPath:context:`에 해당하는 호출이 `addObserver:forKeyPath:options:context:`에 정확히 한 번 호출 하거나, 앱에서 실행 불가능한 경우 `removeObserver:forKeyPath:context:context:` 호출을 try/catch 블록 안에 넣어 잠재적인 예외를 처리하라.
* 관찰자는 소멸되었을 때 자동으로 자신을 제거하지 않는다. 관찰된 객체는 관찰자의 상태를 의식하지 않고 계속 알림을 보낸다. 그러나 다른 메시지와 마찬가지로 릴리즈된 객체에 전송된 변경 알림을 메모리 접근 예외를 트리거한다. 그러므로 당신은 관찰자들이 메모리에서 사라지기 전에 그들 자신을 제거하도록 보장하라.
* 프로토콜은 어떤 객체가 관찰자인지 또는 관찰되고 있는지 물을 수 있는 방법을 제공하지 않는다. 릴리즈 관련 오류를 방지하려면 코드를 구성하라. 일반적인 패턴은 관찰자가 초기화되는 동안 관찰자로 등록하는 것이다. \(예를들어, init 또는 viewDidLoad 에서\) 그리고 해제 중\(일반적으로 `dealloc` 중에\) 등록을 취소하고, 메시지 추가 및 제거를 적절하게 페어링하고, 메모리가 해제되기 전에 관찰자가 등록을 취소하라.

