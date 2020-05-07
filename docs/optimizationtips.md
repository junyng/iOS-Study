# OptimizationTips

## Writing High-Performance Swift Code

* Enabling Optimizations
* Whole Module Optimizations \(WMO\)
* Reducing Dynamic Dispatch
  * 동적 디스패치
  * 조언: 선언문을 재정의할 필요가 없다는 것을 알 때 'final'을 사용하라.
  * 조언: 파일 외부에서 선언에 접근할 필요가 없는 경우 'private' 및 'fileprivate'을 사용하라.
  * 조언: WMO가 활성화된 경우, 모듈 외부에서 선언에 접근할 필요가 없는 경우 'internal'을 사용하라.
* Using Container Types Efficiently
  * 조언: 배열에서 값 타입 사용
  * 조언: NSArray 브릿징이 필요하지 않은 경우 참조 타입과 함께 ContinuousArray 사용
  * 조언: 객체 재할당 대신 mutation 사용
* Unchecked operations
  * 조언: 오버플로우가 발생할 수 없다는 것을 증명할 수 있는 경우 unchecked integer 사용
* Generics
  * 조언: 동일한 모듈에 제네릭 선언을 배치하라.
* The cost of large Swift values
  * 조언: 큰 값에 대해 copy-on-write 사용
* Unsafe code
  * 조언: 관리되지 않은 참조를 사용하여 참조 카운팅 오버헤드를 방지하라.
* Protocols
  * 조언: 클래스만 만족하는 프로토콜을 클래스 프로토콜로 표시하라.
* The Cost of Let/Var when Captured by Escaping Closures
  * 조언: 실제로 클로저가 탈출하지 않는 경우 var를 inout으로 통과시켜라.
* Unsupported Optimization Attributes
* Footnotes

다음 문서는 고성능 스위프트 코드를 작성하기 위한 다양한 팁과 요령을 모은 것이다. 본 문서의 대상은 컴파일러와 표준 라이브러리 개발자들이다.

이 문서의 일부 팁은 Swift 프로그램의 품질을 향상시키고 오류 발생을 줄이고 코드를 더 쉽게 읽을 수 있도록 하는 데 도움이 될 수 있다. final-classes와 class-protocols를 명시적으로 표시하는 것은 두 가지 명백한 예시이다. 그러나 이 문서에 설명된 일부 팁은 원칙이 없고, 뒤틀려 있으며 컴파일러 또는 언어의 특정 일시적 제한을 해결하게 된다. 이 문서의 많은 권고사항은 프로그램 런타임, 이진 크기, 코드 가독성 등과 같은 것들에 대한 트레이드오프와 함께 제공된다.

### Enabling Optimizations

가장 먼저 해야 할 일은 최적화를 가능하게 하는 것이다. Swift는 다음과 같은 세 가지 최적화 수준을 제공한다:

* `-Onone`: 이것은 정상적인 개발을 위한 것이다. 이는 최소 최적화 수행 및 모든 디버그 정보를 보존한다.
* `-O`: 이것은 대부분의 생산 코드에 대한 것이다. 컴파일러는 방출된 코드의 종류와 양을 획기적으로 변경할 수 있는 적극적인 최적화를 수행한다. 디버그 정보는 방출되지만 손실될 것이다.
* `-Osize`: 이것은 컴파일러에서 성능보다 코드 크기를 우선시하는 특수한 최적화 모드이다.

Xcode UI에서는 다음과 같이 현재 최적화 수준을 수정할 수 있다:

프로젝트 탐색기에서 프로젝트 아이콘을 선택하여 프로젝트 편집기를 들어가라. 프로젝트 편집기에서 "Project" 헤더 아래의 아이콘을 선택하여 프로젝트 설정 편집기로 들어간다. 거기서부터 "Build Settings" 헤더의 "Optimization Level" 필드를 변경해 프로젝트의 모든 대상에 최적화 설정을 적용할 수 있다.

사용자 정의 최적화 수준을 특정 대상에 적용하려면 프로젝트 편집이긔 "Targets" 헤더에서 해당 대상을 선택하고 "Build Settings" 헤더 아래의 "Optimization Level" 필드를 재정의하라.

UI에서 특정 최적화 수준을 사용할 수 없는 경우 "Optimization Level" 드롭다운의  `Other...`를 선택하여 해당 플래그를 수동으로 지정할 수 있다.

### Whole Module Optimizations \(WMO\)

기본적으로 Swift는 각 파일을 개별적으로 컴파일한다. 이를 통해 Xcode는 여러 파일을 매우 빠르게 병렬로 컴파일 할 수 있다. 그러나 각 파일을 별도로 컴파일하여 특정 컴파일러 최적화를 방지한다. 스위프트는 또한 프로그램 전체를 하나의 파일처럼 컴파일 할 수 있고 프로그램을 하나의 컴파일 단위처럼 최적화할 수 있다. 이 모드는 `swiftc` 커맨드 라인 플래그 `-whole-module-optimization` 를 사용하여 활성화된다. 이 모드에서 컴파일된 프로그램은 컴파일하는 데 시간이 오래 걸리지만 더 빨리 실행될 수 있다.

이 모드는 Xcode 빌드 설정 'Whole Module Optimization'을 사용하여 활성화할 수 있다.

참고: 아래 섹션에서는 간결함을 위해 'WMO'라는 약어로 'Whole Module Optimization'을 참조할 것이다.

### Reducing Dynamic Dispatch

기본적으로 Swift는 Objective-C와 같은 매우 역동적인 언어다. Swift는 Objective-C와 달리 프로그래머에게 이러한 역동성을 제거하거나 감소시킴으로써 필요할 때 런타임 성능을 향상시킬 수 있는 능력을 제공한다. 이 섹션에서는 이러한 작업을 수행하는 데 사용할 수 있는 언어 구성의 몇 가지 예를 살펴본다.

#### 동적 디스패치

클래스는 기본적으로 메서드 및 속성 접근에 동적 디스패치를 사용한다. 따라서 다음 코드 조각에서 `a.aProperty`, `a.doSomething()` 및 `a.doSomethingElse()`는 모두 동적 디스패치를 통해 호출된다.

```swift
class A {
  var aProperty: [Int]
  func doSomething() { ... }
  dynamic doSomethingElse() { ... }
}

class B: A {
  override var aProperty {
    get { ... }
    set { ... }
  }

  override func doSomething() { ... }
}

func usingAnA(_ a: A) {
  a.doSomething()
  a.aProperty = ...
}
```

Swift에서 동적 디스패치는 vtable을 통한 간접 호출로 기본 설정된다 \[1\]. 선언에 dynamic 키워드를 붙이면 Swift는 대신 Objective-C 메시지 전송을 통해 호출한다. 두 경우 모두 간접 호출 자체의 오버헤드 외에도 많은 컴파일러 최적화 \[2\] 를 방지하므로 직접 함수 호출보다 속도가 느리다. 성능을 최적화할 코드에서, 종종 이러한 동적 디스패치 동작을 제한하기를 원할 것이다.

#### 조언: 선언문을 재정의할 필요가 없다는 것을 알 때 'final'을 사용하라.

`final` 키워드는 오버라이드 될 수 없는 클래스, 메서드 또는 프로퍼티 선언에 대한 제한이다. 이는 컴파일러가 간접 호출 대신 직접 함수 호출을 내보낼 수 있음을 의미한다. 예를 들어, 다음 `C.array1` 및 `D.array1`은 직접 접근된다 \[3\]. 반대로, `D.array2`는 vtable을 통해 호출될 것이다:

```swift
final class C {
  // 클래스 'C'의 어떤 선언도 재정의할 수 없다.
  var array1: [Int]
  func doSomething() { ... }
}

class D {
  final var array1: [Int] // 'array1' 계산 프로퍼티로 재정의할 수 없다.
  var array2: [Int]      // 'array2' 계산 프로퍼티로 재정의할 수 있다.
}

func usingC(_ c: C) {
   c.array1[i] = ... // 동적 디스패치를 거치지 않고 C.array에 직접 접근할 수 있다.
   c.doSomething() = ... // 가상 디스패치를 거치지 않고 C.doSomething에 직접 호출할 수 있다.
}

func usingD(_ d: D) {
   d.array1[i] = ... // 동적 디스패치를 거치지 않고도 D.array1에 직접 접근할 수 있다.
   d.array2[i] = ... // 동적 디스패치를 통해 D.array2에 접근할 것이다.
}
```

#### 조언: 파일 외부에서 선언에 접근할 필요가 없는 경우 'private' 및 'fileprivate'을 사용하라.

선언문에 `private` 또는 `fileprivate` 키워드를 적용하면 선언문이 선언된 파일에 대한 가시성이 제한된다. 이를 통해 컴파일러는 잠재적으로 재정의될 수 있는 다른 모든 선언을 확인할 수 있다. 따라서 그러한 선언이 없는 경우 컴파일러는 자동으로 `final` 키워드를 추론하고 그에 따라 메서드와 필드 접근에 대한 간접 호출을 제거할 수 있다. 예를 들어, `e.doSomething()` 및 `f.myPrivateVar`는 `E`, `F`가 동일한 파일에 재정의 선언이 없다고 가정하여 직접 접근할 수 있다:

```swift
private class E {
  func doSomething() { ... }
}

class F {
  fileprivate var myPrivateVar: Int
}

func usingE(_ e: E) {
  e.doSomething() // 이 클래스를 선언하는 하위 클래스가 파일에 없다.
                  // 컴파일러에서 doSomething()에 대한 가상 호출을 제거할 수 있다.
                  // 그리고 E의 doSomething 메서드에 직접 호출한다.
}

func usingF(_ f: F) -> Int {
  return f.myPrivateVar
}
```

#### 조언: WMO가 활성화된 경우, 모듈 외부에서 선언에 접근할 필요가 없는 경우 'internal'을 사용하라.

WMO \(위의 섹션 참조\)는 컴파일러로 하여금 모듈의 소스를 한꺼번에 컴파일하게 한다. 이를 통해 옵티마이저는 개별 선언을 컴파일할 때 모듈의 넓은 가시성을 가질 수 있다. internal 선언은 현재 모듈 외부에서 볼 수 없으므로, 옵티마이저는 모든 잠재적으로 재정의된 선언을 자동으로 발견하여 final을 추론할 수 있다.

참고: Swift에서 기본 접근 제어 수준을 어쨋든 internal이므로 Whole Module Optimization을 활성화함으로써 더 이상의 작업없이 추가 반가상화를 얻을 수 있다.

### Using Container Types Efficiently

Swift 표준 라이브러리에서 제공하는 중요한 기능은 일반 컨테이너 배열과 딕셔너리이다. 이 섹션에서는 이러한 타입의 사용 방법을 성능적으로 설명한다.

#### 조언: 배열에서 값 타입 사용

Swift에서 타입은 값 타입 \(구조체, 열거형, 튜플\)과 참조 타입 \(클래스\)의 두 가지 범주로 나뉠 수 있다. 중요한 차이점은 값 타입이 NSArray 안에 포함될 수 없다는 것이다. 따라서 값 타입을 사용할 때 옵티마이저는 배열이 NSArray 백업될 가능성을 처리하는 데 필요한 배열의 오버헤드 대부분을 제거할 수 있다.

또한 참조 타입과 대조적으로 값 타입은 참조 타입을 재귀적으로 포함하는 경우에만 참조 계산이 필요하다. 참조 타입 없이 값 타입을 사용하면 추가 리테인을 피할 수 있으며, 배열 내부의 트래픽을 해제할 수 있다.

```swift
// 여기에서 클래스를 사용하지 마라.
struct PhonebookEntry {
  var name: String
  var number: [Int]
}

var a: [PhonebookEntry]
```

큰 값 타입을 사용하는 것과 참조 타입을 사용하는 것 사이에는 트레이드 오프가 있다는 것을 기억하라. 어떤 경우에는 큰 값 타입을 복사하고 이동하는 오버헤드가 브릿징 제거와 리테인/릴리즈 오버헤드 비용보다 클 것이다.

#### NSArray 브릿징이 필요하지 않은 경우 참조 타입과 함께 ContinuousArray 사용

참조 타입 배열이 필요하며 배열을 NSArray로 브릿지할 필요가 없는 경우 Array 대신 ContiguousArray를 사용하라:

```swift
class C { ... }
var a: ContiguousArray<C> = [C(...), C(...), ..., C(...)]
```

#### 조언: 객체 재할당 대신 mutation 사용하라

Swift의 모든 표준 라이브러리 컨테이너는 COW \(copy-on-write\) \[4\]를 사용하여 명시적 사본 대신 복사를 수행하는 값 타입이다. 대부분의 경우, 컴파일러는 깊은 복사를 수행하는 대신 컨테이너를 리테인함으로써 불필요한 사본들을 유용하게 쓴다. 이 작업은 컨테이너의 레퍼런스 카운트가 1보다 크고 컨테이너가 변형된 경우에만 기본 컨테이너를 복사하는 방식으로 수행된다. 예를 들어, `d`가 `c`에 할당될 때 복사가 발생하지 않지만, `d`가 `2`를 추가하여 구조적 변이를 겪을 때 `d`는 복사되고 `2`는 `d`에 추가된다:

```swift
var c: [Int] = [ ... ]
var d = c        // 여기에서는 복사가 일어나지 않을 것이다.
d.append(2)      // 여기에서 복사가 발생한다.
```

때로는 사용자가 주의하지 않으면 COW가 예상치 못한 복사본을 추가로 도입할 수 있다. 이것의 예는 함수에서 객체 재할당을 통해 변형을 수행하려고 시도하는 것이다. Swift에서는 모든 파라미터가 +1로 전달된다. 다시 말하면, 파라미터는 callsite 전에 유지된 후 호출자의 끝에서 릴리즈된다. 이것은 다음과 같은 함수를 작성하면 다음과 같이 함을 의미한다:

```swift
func append_one(_ a: [Int]) -> [Int] {
  a.append(1)
  return a
}

var a = [1, 2, 3]
a = append_one(a)
```

`a`는 할당으로 인해 `append_one` 이후 아무런 용도가 없음에도 불구하고 복사될 수 있다 \[5\]. 이는 `inout` 파라미터를 통해 방지할 수 있다:

```swift
func append_one_in_place(a: inout [Int]) {
  a.append(1)
}

var a = [1, 2, 3]
append_one_in_place(&a)
```

### Unchecked operations

Swift는 정규 산술 수행 시 오버플로를 확인하여 정수 오버플로 버그를 제거한다. 이러한 점검은 메모리 안전 문제가 발생할 수 없다는 것을 아는 고성능 코드에서는 적절하지 않다.

#### 조언: 오버플로우가 발생할 수 없다는 것을 증명할 수 있는 경우 unchecked integer 사용

성능에 중요한 코드에서는 안전하다는 것을 알고 있으면 오버플로우 검사를 회피할 수 있다.

```swift
a: [Int]
b: [Int]
c: [Int]

// 전제 조건: a[i], b[i]: a[i] + b[i]는 오버플로우 되지 않는다!
for i in 0 ... n {
  c[i] = a[i] &+ b[i]
}
```

### Generics

Swift는 제네릭 타입의 사용을 통해 매우 강력한 추상화 메커니즘을 제공한다. Swift 컴파일러는 `T`에 대해 `MySwiftFunc<T>`를 수행할 수 있는 구체적인 코드 한 블록을 내보낸다. 생성된 코드는 함수 포인터 테이블과 `T`를 포함하는 박스를 추가 파라미터로 취한다. `MySwiftFunc<Int>`와 `MySwiftFunc<String>` 간의 동작 차이는 다른 테이블의 함수 포인터와 박스에 의해 제공된 크기 추상화를 통과시킴으로써 설명된다. 제네릭의 예제:

```swift
class MySwiftFunc<T> { ... }

MySwiftFunc<Int> X    // Will emit code that works with Int...
MySwiftFunc<String> Y // ... as well as String.
```

최적화가 활성화되면 Swift 컴파일러는 그러한 코드의 각 호출에 대해 살펴보고 호출에 사용된 구체타입\(즉, 제네릭 타입이 아님\)을 확인하려고 시도한다. 제네릭 함수의 정의가 옵티마이저에 가시화되고 구체 타입이 알려지면 Swift 컴파일러는 특정 타입에 특화된 일반 함수 버전을 내보낸다. _specialization_으로 불리는 이 과정은 제네릭과 관련된 오버헤드를 제거할 수 있게 한다. 제네릭의 몇 가지 더 많은 예제:

```swift
class MyStack<T> {
  func push(_ element: T) { ... }
  func pop() -> T { ... }
}

func myAlgorithm<T>(_ a: [T], length: Int) { ... }

// 컴파일러는 MyStack<Int> 코드를 전문화할 수 있다.
var stackOfInts: MyStack<Int>
// ints 스택 사용
for i in ... {
  stack.push(...)
  stack.pop(...)
}

var arrayOfInts: [Int]
// 컴파일러는 대상인 'myAlgorithm'의 특수 버전을 내보낼 수 있다.
// [Int]' types.
myAlgorithm(arrayOfInts, arrayOfInts.length)
```

#### 조언: 동일한 모듈에 제네릭 선언을 배치하라.

옵티마이저는 일반 선언의 정의가 현재 모듈에서 보이는 경우에만 전문화를 수행할 수 있다. 이는 선언이 `-whole-module-optimization` 플래그를 사용하지 않는 한 일반 호출과 동일한 파일에 있는 경우에만 발생할 수 있다. 표준 라이브러리의 특수한 경우임을 참고하라. 표준 라이브러리의 정의는 모든 모듈에서 볼 수 있으며 전문화에 사용할 수 있다.

### The cost of large Swift values

Swift에서는 값이 데이터의 고유한 복사본을 보관한다. 값 타입을 사용하는 데는 여러 가지 장점이 있는데, 이는 값이 독립적인 상태를 갖도록 하는 것과 같다. 값을 복사하면\(할당, 초기와 및 전달인자 전달 효과\) 프로그램에서 값의 새 복사본을 생성한다. 일부 큰 값의 경우 이러한 복사본은 시간이 많이 소요될 수 있으며 프로그램의 성능에 영향을 줄 수 있다.

"값" 노드를 사용하여 트리를 정의하는 아래 예제를 고려하라. 트리 노드에는 프로토콜을 사용하는 다른 노드가 포함되어 있다. 컴퓨터 그래픽에서 장면은 종종 값으로 표현될 수 있는 다른 엔티티 및 변환으로 구성되므로 이 예제는 다소 현실적이다.

```swift
protocol P {}
struct Node: P {
  var left, right: P?
}

struct Tree {
  var node: P?
  init() { ... }
}
```

트리를 복사 \(전달인자 전달, 초기화 또는 할당\)할 때 전체 트리를 복사해야 한다. 트리의 경우, 이은 malloc/free 에 대한 많은 호출과 상당한 참조 카운팅 오버헤드를 요구한다.

그러나 우리는 그 값의 의미론이 남아 있는 한 그 값이 메모리에 복사되어도 별로 신경 쓰지 않는다.

#### 조언: 실제로 클로저가 탈출하지 않는 경우 var를 inout으로 통과시켜라.

큰 값을 복사하는 비용을 없애려면 copy-on-write 동작을 채택하라. copy-on-write를 구현하는 가장 쉬운 방법은 배열과 같은 존재하는 데이터 구조체의 copy-on-write를 구성하는 것이다. Swift의 배열은 값이지만 배열의 내용은 copy-on-write 특성을 특징으로 하기 때문에 배열이 전달인자로 전달될 때마다 복사되지 않는다.

우리의 트리 예제에서 우리는 트리 콘텐츠를 배열로 래핑하여 복사하는 비용을 없앤다. 이 간단한 변화는 우리의 트리 데이터 구조체의 성능에 큰 영향을 미치며, 전달인자로써 배열 전달 비용은 트리의 크기에 따라 O\(n\)에서 O\(1\)로 떨어진다.

```swift
struct Tree: P {
  var node: [P?]
  init() {
    node = [thing]
  }
}
```

COW 의미론에는 배열을 사용하는 두 가지 분명한 단점이 있다. 첫 번째 문제는 배열이 값 래퍼의 맥락에서 "append"와 "count" 같은 메서드를 노출한다는 것이다. 이러한 메서드는 참조 래퍼의 사용을 어색하게 만들 수 있다. 사용하지 않는 API를 숨기는 래퍼 구조체를 만들고 옵티마이저가 이 오버헤드를 제거하면 이 문제를 해결할 수 있지만 이 래퍼는 두 번째 문제를 해결하지 못할 것이다. 두 번째 문제는 배열이 프로그램 안정성 및 Objective-C와의 상호작용을 보장하기 위한 코드를 가지고 있다는 것이다. Swift는 인덱싱된 접근이 배열 경계 내에 있는지, 배열 저장소를 확장해야 하는 경우 값을 저장할 때 신속하게 확인한다. 이러한 런타임 점검은 일을 더디게 할 수 있다.

배열을 사용할 수 있는 대안은 배열을 값 래퍼로 대체하기 위한 전용 copy-on-write 데이터 구조체를 구현하는 것이다. 아래의 예제는 이러한 데이터 구조체를 구성하는 방법을 보여준다:

```swift
final class Ref<T> {
  var val: T
  init(_ v: T) {val = v}
}

struct Box<T> {
    var ref: Ref<T>
    init(_ x: T) { ref = Ref(x) }

    var value: T {
        get { return ref.val }
        set {
          if !isKnownUniquelyReferenced(&ref) {
            ref = Ref(newValue)
            return
          }
          ref.val = newValue
        }
    }
}
```

`Box` 타입은 위의 코드 샘플에서 배열을 대체할 수 있다.

### Unsafe code

Swift 클래스는 항상 참조 카운트된다. Swift 컴파일러는 객체에 접근할 때마다 참조 카운트를 증가시키는 코드를 삽입한다. 예를 들어, 클래스를 사용하여 구현된 연결 리스트를 검색하는 문제를 고려하라. 리스트를 스캔하는 것은 참조를 한 노드에서 다음 노드로 이동하여 수행된다: `elem = elem.next`. 우리가 움직일 때마다 참소 Swift는 `next` 객체의 참조 카운트를 증가시키고 이전 객체의 참조 카운트를 감소시킨다. 이러한 참조 카운트 작업은 비용이 많이 들고 Swift 클래스를 사용할 때 피할 수 없다.

```swift
final class Node {
 var next: Node?
 var data: Int
 ...
}
```

#### 조언: 관리되지 않은 참조를 사용하여 참조 카운팅 오버헤드를 방지하라.

참고로, `Unmanaged._withUnsafeGuaranteedRef`은 public API가 아니며 앞으로 사라질 것이다. 따라서 앞으로 변경할 수 없는 코드로 사용하지 마라.

성능 임계 코드에서 관리되지 않은 참조를 사용하도록 선택할 수 있다. `Unmanaged<T>` 구조체는 개발자가 특정 참조에 대한 자동 참조 카운팅을 비활성화할 수 있다.

이 경우 `Unmanaged` 구조체 인스턴스가 `Unmanaged` 사용 기간 동안 보유한 있는 인스턴스에 대한 다른 참조가 있는지 확인해야 하며\(자세한 내용은 [Unmanaged.swift](https://github.com/apple/swift/blob/master/stdlib/public/core/Unmanaged.swift) 참조\) 인스턴스가 계속 유지되도록 하라.

```swift
// The call to ``withExtendedLifetime(Head)`` makes sure that the lifetime of
// Head is guaranteed to extend over the region of code that uses Unmanaged
// references. Because there exists a reference to Head for the duration
// of the scope and we don't modify the list of ``Node``s there also exist a
// reference through the chain of ``Head.next``, ``Head.next.next``, ...
// instances.

withExtendedLifetime(Head) {

  // Create an Unmanaged reference.
  var Ref: Unmanaged<Node> = Unmanaged.passUnretained(Head)

  // Use the unmanaged reference in a call/variable access. The use of
  // _withUnsafeGuaranteedRef allows the compiler to remove the ultimate
  // retain/release across the call/access.

  while let Next = Ref._withUnsafeGuaranteedRef { $0.next } {
    ...
    Ref = Unmanaged.passUnretained(Next)
  }
}
```

### Protocols

#### 조언: 클래스만 만족하는 프로토콜을 클래스 프로토콜로 표시하라.

Swift는 프로토콜 채택을 클래스로만 제한할 수 있다. 클래스 전용으로 프로토콜을 표시하는 한 가지 장점은 컴파일러가 클래스만 프로토콜을 만족하는 지식을 기반으로 프로그램을 최적화할 수 있다는 것이다. 예를 들어, ARC 메모리 관리 시스템은 클래스를 다루고 있다는 것을 알면 쉽게 리테인\(객체의 참조 카운트를 증가\)할 수 있다. 이러한 지식이 없으면 구조체가 프로토콜을 충족할 수 있으며 정해지지 않은 구조체를 리테인 또는 릴리즈할 준비가 되어 있다고 컴파일러는 가정하고 이는 비용이 많이 들 수 있다.

프로토콜의 채택을 클래스로 제한하는 것이 이치에 맞다면, 프로토콜을 클래스 전용 프로토콜로 표시하여 더 나은 런타임 성능을 얻어라.

```swift
protocol Pingable: AnyObject { func ping() -> Int }
```

조언: 실제로 클로저가 탈출하지 않는 경우 var를 inout으로 통과시켜라.

let/var의 구별은 단지 언어 의미에 관한 것이라고 생각할 수 있지만, 성능 고려사항도 있다. 클로저에 대한 바인딩을 만들 때마다 컴파일러가 이스케이핑 클로저를 강제로 내보낸다는 것을 기억하라. 예를 들어:

```swift
let f: () -> () = { ... } // Escaping closure
// Contrasted with:
({ ... })() // Non Escaping closure
x.map { ... } // Non Escaping closure
```

이스케이핑 클로저에 의해 var가 캡처되면 컴파일러는 클로저 creator/closure가 값에 모두 읽기/쓰기 할 수 있도록 var를 저장할 힙 박스를 할당해야 한다. 여기에는 캡처된 바인딩의 기본 타입이 사소한 상황도 포함된다. 이와는 대조적으로 let은 값으로 캡처된다. 이와 같이 컴파일러는 박스의 필요 없이 값의 복사본을 클로져의 저장소에 직접 보관한다.

#### 조언: 실제로 클로저가 탈출하지 않는 경우 var를 inout으로 통과시켜라.

표현성 목적으로 이스케이핑 클로저를 사용하지만 실제로 로컬에서 클로저를 사요하는 경우 캡처 대신 inout 파라미터로 통과하라. inout은 힙 박스가 변수에 할당되지 않도록 하고 힙 박스의 리테인/릴리즈 트래픽이 전달되지 않도록 한다.

### Unsupported Optimization Attributes

일부 강조된 타입 속성은 옵티마이저 지시사항으로 사용된다. 개발자는 이러한 속성을 실험하고 다음 불완전한 문서에 대한 메타 버그 보고서를 포함하여 버그 보서 및 기타 피드백을 다시 전송할 수 있다:

[:ref:\`UnsupportedOptimizationAttributes\`](https://github.com/apple/swift/blob/master/docs/OptimizationTips.rst#id6). 이러한 속성은 지원되는 언어 기능이 아니다. Swift Evolution에 의해 검토되지 않았으며 컴파일러 릴리즈간에 변경될 가능성이 있다.

### Footnotes

| [\[1\]](https://github.com/apple/swift/blob/master/docs/OptimizationTips.rst#id1) | 가상 메서드 테이블 또는 'vtable'은 타입 메서드의 주소를 포함하는 인스턴스가 참조하는 타입별 테이블이다. 동적 디스패치는 먼저 객체에서 테이블을 올려다본 다음 테이블에서 메서드를 찾아보는 방식으로 진행된다. |
| :--- | :--- |


| [\[2\]](https://github.com/apple/swift/blob/master/docs/OptimizationTips.rst#id2) | 이것은 컴파일러가 호출되는 정확한 기능을 모르기 때문이다. |
| :--- | :--- |


| [\[3\]](https://github.com/apple/swift/blob/master/docs/OptimizationTips.rst#id3) | 즉, 클래스 필드의 직접 로드 또는 함수에 대한 직접 호출. |
| :--- | :--- |


| [\[4\]](https://github.com/apple/swift/blob/master/docs/OptimizationTips.rst#id4) | 원본 복사본에 대한 수정, 그렇지 않으면 포인터가 주어질 경우에만 복사본이 만들어지는 최적화 기법. |
| :--- | :--- |


| [\[5\]](https://github.com/apple/swift/blob/master/docs/OptimizationTips.rst#id5) | 특정 경우 옵티마이저는 인라이닝을 통해 가능하고 ARC 최적화는 리테인, 릴리즈를 제거하여 복사본이 발생하지 않도록 한다. |
| :--- | :--- |


