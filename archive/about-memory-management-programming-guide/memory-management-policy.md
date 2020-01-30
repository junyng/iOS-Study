# Memory Management Policy

참조 카운트 환경에서 메모리 관리에 사용되는 기본 모델은 [`NSObject`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intf/NSObject) 프로토콜에 정의된 메서드와 표준 메서드 명명 규칙의 조합에 의해 제공된다. `NSObject` 클래스는 객체가 할당 해제될 때 자동으로 호출되는 메서드인 `dealloc`도 정의한다. 이 글은 Cocoa 프로그램에서 메모리를 올바르게 관리하기 위해 알아야 할 모든 기본 규칙을 설명하고, 정확한 사용 예를 몇 가지 제시한다.

### 기본 메모리 관리 규칙

메모리 관리 모델은 객체 소유에 기반을 두고 있다. 어떤 객체든 하나 이상의 소유자가 있을 수 있다. 객체에 적어도 하나의 소유자가 있는 한 객체는 계속 존재한다. 객체에 소유자가 없으면 런타임 시스템은 자동으로 객체를 파괴한다. 객체를 소유할 때와 소유하지 않을 때 명확히 하기 위해 Cocoa는 다음과 같은 정책을 설정한다:

* **당신은 생성한 객체를 소유한다.** 이름이 "alloc", "new", "copy" 또는 "mutableCopy" \(예를 들어, [`alloc`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/clm/NSObject/alloc), [`newObject`](https://developer.apple.com/documentation/appkit/nsobjectcontroller/1535921-newobject), 또는 [`mutableCopy`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/instm/NSObject/mutableCopy)\)로 시작하는 메서드를 사용하여 객체를 생성한다.
* **리테인을 사용하여 객체 소유권을 획득할 수 있다.** 획득한 객체는 일반적으로 그것이 받은 메서드 내에서 유효하게 유지될 것을 보증하며, 그 메서드는 또한 그 객체의 호출자에게 안전하게 반환할 수 있다. 두 가지 상황에서 `retain`을 사용한다: \(1\) 접근자 메서드 또는 init 메서드의 구현에서 저장할 객체의 소유권을 속성 값으로 가져온다. 그리고 \(2\) 다른 작업의 부작용으로 객체가 무효화되지 않도록 다음과 같이 한다. \([Avoid Causing Deallocation of Objects You’re Using](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/20000043-1000922) 참조\)
* **더 이상 필요하지 않을 때는 소유한 객체의 소유권을 포기해야 한다.** 당신은 객체에 대한 소유권을 [`release`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/release) 또는 [`autorelease`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/autorelease)메시지를 보내 포기한다. 따라서 Cocoa 용어에서, 객체의 소유권을 포기하는 것을 일반적으로 "releasing"이라고 한다.
* **소유하지 않은 객체의 소유권을 포기해서는 안된다.** 이것은 명백하게 언급된 이전의 정책 규칙 결과이다.

#### 간단한 예제

정책을 설명하려면 다음 코드 조각을 고려하라:

```objectivec
{
    Person *aPerson = [[Person alloc] init];
    // ...
    NSString *name = aPerson.fullName;
    // ...
    [aPerson release];
}
```

Person 객체는 `alloc` 메서드를 사용하여 생성되므로 더 이상 필요하지 않을 때 나중에 `release` 메시지를 보내게 된다. 사람의 이름은 소유 메서드를 사용하여 반환 받지 않으므로, `release` 메시지를 보내지 않는다. 그러나 이 예제는 `autorelease` 보다는 `release` 를 사용한다는 점에 주목하라.

#### 오토릴리즈를 사용하여 지연된 릴리즈 전송

일반적으로 메서드에서 객체를 반환할 때 지연된 `release` 메시지를 보내야 하는 경우 `autorelease`를 사용하라. 예를 들어, 다음과 같이 `fullName` 메서드를 구현할 수 있다:

```objectivec
- (NSString *)fullName {
    NSString *string = [[[NSString alloc] initWithFormat:@"%@ %@",
                                          self.firstName, self.lastName] autorelease];
    return string;
}
```

 `alloc`에 의해 반환된 문자열을 소유하라. 메모리 관리 규칙을 준수하려면 문자열에 대한 참조를 잃기 전에 문자열의 소유권을 포기해야 한다. 그러나 `release` 를 사용하면 문자열이 반환되기 전에 문자열이 할장 해제되고 \(그리고 메서드가 유효하지 않은 객체를 반환한다.\) `autorelease` 를사용하여 소유권을 포기함을 나타내지만, 메서드의 호출자가 반환된 문자열을 사용할 수 있도록 허용한 후 할당을 취소하라.

다음과 같이 `fullName` 메서드를 구현할 수 있다:

```objectivec
- (NSString *)fullName {
    NSString *string = [NSString stringWithFormat:@"%@ %@",
                                 self.firstName, self.lastName];
    return string;
}
```

기본 규칙을 따르면, `stringWithFormat:`이 반환한 문자열을 소유하지 않으므로, 메서드에서 문자열을 안전하게 반환할 수 있다.

이와는 대조적으로 다음과 같은 구현은 잘못되었다:

```objectivec
- (NSString *)fullName {
    NSString *string = [[NSString alloc] initWithFormat:@"%@ %@",
                                         self.firstName, self.lastName];
    return string;
}
```

명명 규칙에 따르면, `fullName` 메서드가 반환된 문자열을 소유한다는 것을 나타내는 것은 아무것도 없다. 따라서 호출자는 반환된 문자열을 릴리즈할 이유가 없으므로 누수가 발생한다.

#### 참조로 반환된 객체는 소유하지 않는다

Cocoa의 일부 메서드는 객체가 참조에 의해 반환된다고 명시한다. \(즉, `ClassName **` 또는 `id *` 의 인자를 취한다.\) 일반적인 패턴은 [`initWithContentsOfURL:options:error:`](https://developer.apple.com/documentation/foundation/nsdata/1407864-initwithcontentsofurl) \(`NSData`\) 및 [`initWithContentsOfFile:encoding:error:`](https://developer.apple.com/documentation/foundation/nsstring/1412610-init) \(`NSString`\) 에서 설명한 것처럼 오류에 대한 정보가 포함된 `NSError` 객체를 사용하는 것이다. 

이러한 경우 이미 기술한 것과 동일한 규칙이 적용된다. 이러한 메서드 중 하나를 호출할 때 `NSError` 객체를 만들지 않으므로 소유하지 마라. 따라서 이 예제에서 설명한 바와 같이 릴리즈 할 필요가 없다:

```objectivec
NSString *fileName = <#Get a file name#>;
NSError *error;
NSString *string = [[NSString alloc] initWithContentsOfFile:fileName
                        encoding:NSUTF8StringEncoding error:&error];
if (string == nil) {
    // Deal with error...
}
// ...
[string release];
```

### 객체 소유권을 포기하기 위한 dealloc 구현

`NSObject` 클래스는 객체에 소유자가 없고 해당 메모리가 회수될 때 자동으로 호출되는 메서드인 [`dealloc`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/instm/NSObject/dealloc)을 정의한다—Cocoa 용어에서 "freed" 또는 "deallocated" 라고 한다. `dealloc` 메서드의 역할은 객체 자신의 메모리를 해제하고 객체 인스턴스 변수의 소유권을 포함하여 보유한 모든 리소스를 처리한다.

다음 예제에서는 Person 클래스에 대해 `dealloc` 메서드를 구현하는 방법을 보여준다.

```objectivec
@interface Person : NSObject
@property (retain) NSString *firstName;
@property (retain) NSString *lastName;
@property (assign, readonly) NSString *fullName;
@end
 
@implementation Person
// ...
- (void)dealloc
    [_firstName release];
    [_lastName release];
    [super dealloc];
}
@end
```

> **중요**: 다른 객체의 `dealloc` 메서드를 직접 호출하지 마라.
>
> 당신은 당신의 구현이 끝날 때 슈퍼클래스의 구현을 호출해야 한다.
>
> 시스템 리소스 관리를 객체 수명에 연결해서는 안된다. 자세한 내용은 [Don’t Use dealloc to Manage Scarce Resources](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW13)을 참조하라.
>
> 애플리케이션이 종료되면, 객체가 `dealloc` 메시지를 보내지 않을지도 모른다. 왜냐하면 프로세스의 메모리는 자동으로 지워지기 때문이고, 모든 메모리 관리 메서드를 호출하는 것보다 운영 체제가 자원을 정리하도록 허용하는 것이 더 효율적이다.

### Core Foundation은 유사하지만 다른 규칙을 사용한다

Core Foundation 객체에 대한 유사한 메모리 관리 규칙이 있다 \([_Memory Management Programming Guide for Core Foundation_](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/CFMemoryMgmt.html#//apple_ref/doc/uid/10000127i) 참조\). 그러나 Cocoa와 Core Foundation의 명명 규칙은 다르다. 특히, Core Foundation의 생성 규칙 \([The Create Rule](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029) 참조\)은 Objective-C 객체를 반환하는 메서드에는 적용되지 않는다. 예를 들어, 다음 코드 조각에서는 `myInstance`의 소유권을 양도할 책임이 없다:

```objectivec
MyClass *myInstance = [MyClass createInstance];
```

