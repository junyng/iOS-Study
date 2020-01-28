# Using Autorelease Pool Blocks

오토릴리즈 풀 블록은 객체의 소유권을 포기할 수 있는 메커니즘을 제공하지만, 즉시 할당 취소될 가능성은 피할 수 있다. \(예를 들어, 메서드로부터 객체를 반환하는 경우\). 일반적으로, 당신만의 오토릴리즈 풀을 만들 필요가 없지만, 여러분이 해야 하거나 그렇게 하는 것이 유익할 수 있는 몇몇 상황들이 있다.

### 오토릴리즈 블록에 대하여

오토릴리즈 풀 블록은 다음 예제에서 설명한 것과 같이 `@autoreleasepool`을 사용하여 표시된다:

```objectivec
@autoreleasepool {
    // Code that creates autoreleased objects.
}
```

오토릴리즈 풀 블록의 끝에서, `autorelease` 메시지를 수신한 객체는 `release` 메시지를 전송한다—객체는 블록 내에서 `autorelease` 메시지를 보낼 때마다 `release` 메시지를 수신한다.

다른 코드 블록과 마찬가지로 오토릴리즈 풀 블록은 중첩될 수 있다:

```objectivec
@autoreleasepool {
    // . . .
    @autoreleasepool {
        // . . .
    }
    . . .
}
```

\(위의 코드는 정상적으로 볼 수는 없다. 일반적으로 한 소스파일의 오토릴리즈 풀 블록 내의 코드는 다른 오토릴리즈 풀 블록 내에 포함된 다른 소스 파일의 코드를 호출한다.\) 주어진 `autorelease` 메시지의 경우, 해당 `release` 메시지는 자동 `autorelease` 메시지가 전송된 오토릴리즈풀 끝에 전송된다.

Cocoa는 항상 오토릴리즈 풀 블록 내에서 코드가 실행될 것으로 예상한다. 그렇지 않으면 오토릴리즈된 객체가 해제되지 않고 애플리케이션이 메모리를 누수한다. \(오토릴리즈 풀 외부에서 `autorelease` 메시지를 보내는 경우 Cocoa가 적절한 오류 메시지를 기록한다.\) AppKit 및 UIKit 프레임워크는 오토릴리즈 풀 블록 내에서 각 이벤트 루프 반복 \(마우스 다운 이벤트 또는 탭 등\)을 처리한다. 따라서 일반적으로 오토릴리즈 풀 블록을 직접 만들거나 오토릴리즈 풀을 만들 때 사용되는 코드를 볼 필요가 있다. 그러나 오토릴리즈 풀 블록을 사용할 수 있는 경우가 세 가지 있다:

* 커맨드 라인 툴과 같이 UI 프레임워크를 기반으로 하지 않는 프로그램을 작성하는 경우.
* 많은 임시 객체를 만드는 루프를 작성할 경우. 다음 번 반복 전에 루프 내부의 오토릴리즈 풀 블록을 사용하여 해당 객체를 폐기할 수 있다. 루프에서 오토릴리즈 풀 블록을 사용하면 애플리케이션의 최대 메모리 공간을 줄일 수 있다.
* 만약 2차 쓰레드를 낳을 경우. 쓰레드가 실행되기 시작하면 즉시 오토릴리즈 풀 블록을 생성 해야 한다. 그렇지 않으면 애플리케이션에서 객체를 누수한다. \(자세한 내용은 [Autorelease Pool Blocks and Threads](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmAutoreleasePools.html#//apple_ref/doc/uid/20000047-1041876)를 참조하라.\)

### 로컬 오토릴리즈 풀 블록을 사용하여 메모리 사용량 감소

많은 프로그램들이 오토릴리즈된 임시 객체를 만든다. 이러한 객체는 블록이 끝날 때까지 프로그램의 메모리 사용량에 추가된다. 많은 상황에서 임시 객체를 현재의 이벤트 루프 반복이 끝날때까지 축적할 수 있도록 허용하면 과도한 오버헤드가 발생하지 않지만, 어떤 상황에서는 메모리 사용량에 실질적으로 더 많은 수의 임시 객체를 생성하여 더 빨리 폐기할 수 있다. 이러한 후자의 경우, 당신은 당신만의 오토릴리즈 풀 블록을 만들 수 있다. 블록 끝에서 임시 객체가 릴리즈되며, 이는 일반적으로 할당 해제를 초래하여 프로그램의 메모리 공간을 감소시킨다.

다음 예제에서는 `for` 루프에 오토릴리즈 풀 블록을 사용하는 방법을 보여준다:

```objectivec
NSArray *urls = <# An array of file URLs #>;
for (NSURL *url in urls) {
 
    @autoreleasepool {
        NSError *error;
        NSString *fileContents = [NSString stringWithContentsOfURL:url
                                         encoding:NSUTF8StringEncoding error:&error];
        /* Process the string, creating and autoreleasing more objects. */
    }
}
```

`for` 루프 프로세스는 한 번에 하나의 파일을 처리한다. 오토릴리즈 풀 블록 내부에 `autorelease` 메시지를 보낸 모든 객체 \(`fileContents` 같은\)는 블록 끝에 릴리즈된다.

오토릴리즈 풀 블록 후에는 블록 내에서 오토릴리즈된 모든 객체를 "폐기된 객체"로 간주해야 한다. 해당 객체에 메시지를 보내거나 메서드의 호출자에게 반환하지 말아라. 오토릴리즈 풀 블록을 벗어난 임시 객체를 사용해야 하는 경우, 블록 내의 객체에 `retain` 메시지를 보낸 다음 이후에 `autorelease`를 전송하면 된다.

```objectivec
– (id)findMatchingObject:(id)anObject {
 
    id match;
    while (match == nil) {
        @autoreleasepool {
 
            /* Do a search that creates a lot of temporary objects. */
            match = [self expensiveSearchForObject:anObject];
 
            if (match != nil) {
                [match retain]; /* Keep match around. */
            }
        }
    }
 
    return [match autorelease];   /* Let match go and return it. */
}
```

오토릴리즈 풀 내에서 `match` 하도록 `retain` 을 전송하면, 오토릴리즈 풀 블록이 `match` 수명을 연장하고, 루프 외부의 메시지를 수신하여 `findMatchingObject:` 의 호출자에게 반환될 수 있다.

### 오토릴리즈 풀 블록 및 쓰레드

Cocoa 애플리케이션의 각 쓰레드는 자체적인 오토릴리즈 풀 블록 스택을 유지한다. Foundation 전용 프로그램을 작성하거나 쓰레드를 분리한 경우 오토릴리즈 풀 블록을 생성해야 한다.

애플리케이션이나 쓰레드가 오래 지속되어 많은 오토릴리즈된 객체를 잠재적으로 생성할 경우 오토릴리즈 풀 블록 \(AppKit 및 UIKit이 메인쓰레드에서 하는 것과 같이\)을 사용해야 한다. 그렇지 않으면 오토릴리즈된 객체가 축적되고 메모리 사용량이 커진다. 분리된 쓰레드가 Cocoa 호출을 하지 않으면 오토릴리즈 풀 블록을 사용할 필요가 없다.

> 참고: `NSThread` 대신 POSIX 쓰레드 API를 사용하여 보조 쓰레드를 만들 경우, Cocoa가 멀티쓰레딩 모드에 있지 않으면 Cocoa를 사용할 수 없다. Cocoa는 첫 번째 `NSThread` 객체를 분리한 후에만 멀티쓰레딩 모드로 전환된다. 보조 POSIX 쓰레드에서 Cocoa를 사용하려면 먼저 애플리케이션이 즉시 종료될 수 있는 `NSThread` 객체를 하나 이상 분리해야 한다. `NSThread` 클래스 메서드 [`isMultiThreaded`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/clm/NSThread/isMultiThreaded) 를 사용하여 Cocoa가 멀티쓰레딩 모드인지 테스트할 수 있다.

