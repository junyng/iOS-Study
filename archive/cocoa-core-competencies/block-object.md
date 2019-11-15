# Block object

블록 객체는 인수로 전달되고, 선택적으로 저장되며, 여러 스레드에서 사용할 수 있는 함수 식을 합성할 수 있도록 허용하는 C 수준의 구문 및 런타임 기능이다. 이 함수의 표현은 참조할 수 있고 지역 변수에 대한 접근을 보존할 수 있다. 다른 언어와 환경에서 블록 객체는 클로저 또는 람다라고 불린다. 값인 것처럼 전달할 수 있는 작업 단위\(코드 세그먼트\)를 만들려면 블록을 사용하라. 블록은 더 유연한 프로그래밍과 능력을 제공한다. 예를 들어, 콜백을 쓰거나 컬렉션의 모든 항목에 대해 작업을 수행하는 데 콜백을 사용할 수 있다.

### Declaring a Block

여러 상황에서 블록을 인라인으로 사용하여 선언할 필요가 없다. 그러나 선언 구문은 별표 포인터\(\*\) 대신 캐럿\(^\)을 사용한다는 점을 제외하면 함수 포인터의 표준 구문과 유사하다. 예를 들어, 다음과 같이 세 개의 매개변수가 필요한 블록을 참조하고  `float` 값을 반환하는 `aBlock`을 선언한다.

```objectivec
float (^aBlock)(const int*, int, float);
```

### Creating a Block

캐럿\(^\) 연산자를 사용하여 시작을 나타내고 세미콜론으로 블록 식의 끝을 표시하라. 다음 예제에서는 단순 블록을 선언하고 이전에 선언된 블록 변수 \(oneForm\)에 할당한다.

```objectivec
int (^oneFrom)(int);
 
oneFrom = ^(int anInt) {
    return anInt - 1;
};
```

닫히는 세미콜론은 표준 C의 라인 끝 마커로 필요하다.

만약 명시적으로 블록 식의 반환값을 선언하지 않는 다면, 자동으로 블록의 내용을 추론할 수 있다.

### Block-Mutable Variables

로컬로 둘러싸인 어휘 범위에 선언된 변수가 포함된 `__block` 저장 수식어를 사용하여 해당 변수가 블록의 참조에 의해 제공되어야 하며 변형가능해야 함을 나타낼 수 있다. 모든 변경은 둘러싸인 같은 어휘 범위 내에서 정의 등에 반영되어 있다.

### Using Blocks

만약 블록을 변수로 선언하면 함수처럼 사용할 수 있다. 다음 예는 9를 출력한다.

```objectivec
printf("%d\n", oneFrom(10));
```

그러나 일반적으로 함수나 메서드에 대한 인자로 블록을 통과시킨다. 이러한 경우블록을 한 라인으로 생성하는 경우가 많다.

다음 예제는 `NSSet` 객체가 로컬 변수에 의해 지정된 단어 포함 여부를 결정하고, 다른 로컬 변수\(찾은\) 값을 `YES` \(그리고 탐색을 중지\) 로 설정 한다. 예제에서는 `found`는  `__block` 변수로 선언된다.

```objectivec
__block BOOL found = NO;
NSSet *aSet = [NSSet setWithObjects: @"Alpha", @"Beta", @"Gamma", @"X", nil];
NSString *string = @"gamma";
 
[aSet enumerateObjectsUsingBlock:^(id obj, BOOL *stop) {
    if ([obj localizedCaseInsensitiveCompare:string] == NSOrderedSame) {
        *stop = YES;
        found = YES;
    }
}];
 
// At this point, found == YES
```

이 예제에서 블록은 메서드 인자 목록이 포함되어 있다. 블록은 또한 스택 로컬 변수를 사용한다.

### Comparison Operations

Cocoa 환경에서 블록을 사용하여 수행하는 보다 일반적인 작업 중 하나는 두 객체의 비교이다. \(예: 배열의 내용 정렬\) Objective-C 런타임은 이러한 비교를 위한 `NSComparator` 블록 타입을 정의한다.

### Prerequisite Articles

[Message](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59-SW1)

#### Related Articles

[Enumeration](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Enumeration.html#//apple_ref/doc/uid/TP40008195-CH17-SW1)

#### Definitive Discussion

[Working with Blocks](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithBlocks/WorkingwithBlocks.html#//apple_ref/doc/uid/TP40011210-CH8)

