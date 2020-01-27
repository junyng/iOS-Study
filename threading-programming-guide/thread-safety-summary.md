# Thread Safety Summary

이 부록은 OS X와 iOS의 일부 핵심 프레임워크의 고수준의 쓰레드 안정성에 대해 설명한다. 이 부록의 정보를 변경될 수 있다.

### Cocoa

멀티 쓰레드의 코코아 사용에 대한 가이드라인은 다음과 같다:

* 불변의 객체는 일반적으로 쓰레드 세이프하다. 일단 그것들을 만들면, 이 객체들을 안전하게 쓰레드로 주고 받을 수 있다. 반면에, 변경 가능한 객체는 일반적으로 쓰레드 세이프하지 않다. 쓰레드된 애플리케이션에서 변경 가능한 객체를 사용하려면 애플리케이션이 적절하게 동기화되어야 한다. 자세한 내용은 [Mutable Versus Immutable](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/ThreadSafetySummary/ThreadSafetySummary.html#//apple_ref/doc/uid/20000736-126010)를 참조하라.
* "thread-unsafe"로 간주되는 많은 객체는 멀티 쓰레드에서 사용하는 것이 안전하지 않을 뿐이다. 이 객체들 중 많은 것들은 한 번에 하나의 쓰레드만 있는 한 어떤 쓰레드에서도 사용될 수 있다. 애플리케이션의 메인 쓰레드로 특별히 제한되는 객체는 이렇게 호출된다.
* 애플리케이션의 메인 쓰레드는 이벤트 처리를 담당한다. 다른 쓰레드가 이벤트 경로에 관여하는 경우 애플리케이션 키트가 계속 작동하지만, 작동이 순차적으로 발생할 수 있다.
* 쓰레드를 사용하여 뷰에 그리려면 [`NSView`](https://developer.apple.com/documentation/appkit/nsview)의 [`lockFocusIfCanDraw`](https://developer.apple.com/documentation/appkit/nsview/1483285-lockfocusifcandraw) 및 [`unlockFocus`](https://developer.apple.com/documentation/appkit/nsview/1483711-unlockfocus) 메서드 사이에 있는 모든 드로잉 코드를 브래킷하라.
* 코코아와 함께 POSIX 쓰레드를 사용하려면 먼저 코코아를 멀티 쓰레드 모드로 전환해야 한다. 자세한 내용은 [Using POSIX Threads in a Cocoa Application](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/20000738-125024)를 참조하라.

#### Foundation Framework Thread Safety

파운데이션 프레임워크가 쓰레드 세이프하고 애플리케이션 키트 프레임워크는 그렇지 않다는 오해가 있다. 불행하게도 이것은 총체적이고 다소 오해의 소지가 있다. 각 프레임워크에는 쓰레드에 안전한 영역과 쓰레드에 안전하지 않은 영역이 있다. 다음 섹션에서는 파운데이션 프레임워크의 일반적인 쓰레드 안전성에 대해 설명한다.

**Thread-Safe Classes and Functions**

다음과 같은 클래스와 함수는 일반적으로 쓰레드 세이프한 것으로 간주된다. 먼저 락을 획득하지 않고도 여러 쓰레드에서 동일한 인스턴스를 사용할 수 있다.

* [`NSArray`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)
* [`NSAssertionHandler`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAssertionHandler/Description.html#//apple_ref/occ/cl/NSAssertionHandler)
* [`NSAttributedString`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAttributedStrngClstr/Description.html#//apple_ref/occ/cl/NSAttributedString)
* [`NSBundle`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)
* [`NSCalendar`](https://developer.apple.com/documentation/foundation/nscalendar)
* [`NSCalendarDate`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCalendarDate/Description.html#//apple_ref/occ/cl/NSCalendarDate)
* [`NSCharacterSet`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCharacterSetClstr/Description.html#//apple_ref/occ/cl/NSCharacterSet)
* [`NSConditionLock`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSConditionLock/Description.html#//apple_ref/occ/cl/NSConditionLock)
* [`NSConnection`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSConnection/Description.html#//apple_ref/occ/cl/NSConnection)
* [`NSData`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)
* [`NSDate`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/cl/NSDate)
* [`NSDateFormatter`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateFormatter/Description.html#//apple_ref/occ/cl/NSDateFormatter)
* `NSDecimal` functions
* [`NSDecimalNumber`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDecimalNumber/Description.html#//apple_ref/occ/cl/NSDecimalNumber)
* [`NSDecimalNumberHandler`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDecimalNumberHandler/Description.html#//apple_ref/occ/cl/NSDecimalNumberHandler)
* [`NSDeserializer`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDeserializer/Description.html#//apple_ref/occ/cl/NSDeserializer)
* [`NSDictionary`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)
* [`NSDistantObject`](https://developer.apple.com/documentation/foundation/nsdistantobject)
* [`NSDistributedLock`](https://developer.apple.com/documentation/foundation/nsdistributedlock)
* [`NSDistributedNotificationCenter`](https://developer.apple.com/documentation/foundation/nsdistributednotificationcenter)
* [`NSException`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/cl/NSException)
* [`NSFileManager`](https://developer.apple.com/documentation/foundation/filemanager)
* [`NSFormatter`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSFormatter/Description.html#//apple_ref/occ/cl/NSFormatter)
* [`NSHost`](https://developer.apple.com/documentation/foundation/host)
* [`NSJSONSerialization`](https://developer.apple.com/documentation/foundation/nsjsonserialization)
* [`NSLock`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSLock/Description.html#//apple_ref/occ/cl/NSLock)
* [`NSLog`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSLog)/[`NSLogv`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSLogv)
* [`NSMethodSignature`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSMethodSignature/Description.html#//apple_ref/occ/cl/NSMethodSignature)
* [`NSNotification`](https://developer.apple.com/documentation/foundation/nsnotification)
* [`NSNotificationCenter`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationCenter/Description.html#//apple_ref/occ/cl/NSNotificationCenter)
* [`NSNumber`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/cl/NSNumber)
* [`NSNumberFormatter`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumberFormatter/Description.html#//apple_ref/occ/cl/NSNumberFormatter)
* [`NSObject`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)
* [`NSOrderedSet`](https://developer.apple.com/documentation/foundation/nsorderedset)
* [`NSPortCoder`](https://developer.apple.com/documentation/foundation/nsportcoder)
* [`NSPortMessage`](https://developer.apple.com/documentation/foundation/nsportmessage)
* [`NSPortNameServer`](https://developer.apple.com/documentation/foundation/nsportnameserver)
* [`NSProgress`](https://developer.apple.com/documentation/foundation/nsprogress)
* [`NSProtocolChecker`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSProtocolChecker/Description.html#//apple_ref/occ/cl/NSProtocolChecker)
* [`NSProxy`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSProxy/Description.html#//apple_ref/occ/cl/NSProxy)
* [`NSRecursiveLock`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRecursiveLock/Description.html#//apple_ref/occ/cl/NSRecursiveLock)
* [`NSSet`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/cl/NSSet)
* [`NSString`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)
* [`NSThread`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/cl/NSThread)
* [`NSTimer`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimer/Description.html#//apple_ref/occ/cl/NSTimer)
* [`NSTimeZone`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimeZoneClassCluster/Description.html#//apple_ref/occ/cl/NSTimeZone)
* [`NSUserDefaults`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSUserDefaults/Description.html#//apple_ref/occ/cl/NSUserDefaults)
* [`NSValue`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue)
* [`NSXMLParser`](https://developer.apple.com/documentation/foundation/xmlparser)
* Object allocation and retain count functions
* Zone and memory functions

**Thread-Unsafe Classes**

다음의 클래스와 함수는 일반적으로 쓰레드 세이프하지 않다. 대부분의 경우, 한 번에 하나의 쓰레드에서만 이 클래스를 사용하는 한, 어떤 쓰레드에서도 이 클래스를 사용할 수 있다. 추가적인 자세한 내용은 클래스 설명서를 참조하라.

* [`NSArchiver`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArchiver/Description.html#//apple_ref/occ/cl/NSArchiver)
* [`NSAutoreleasePool`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAutoreleasePool/Description.html#//apple_ref/occ/cl/NSAutoreleasePool)
* [`NSCoder`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCoder/Description.html#//apple_ref/occ/cl/NSCoder)
* [`NSCountedSet`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCountedSet/Description.html#//apple_ref/occ/cl/NSCountedSet)
* [`NSEnumerator`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSEnumerator/Description.html#//apple_ref/occ/cl/NSEnumerator)
* [`NSFileHandle`](https://developer.apple.com/documentation/foundation/filehandle)
* `NSHashTable` functions
* [`NSInvocation`](https://developer.apple.com/documentation/foundation/nsinvocation)
* `NSMapTable` functions
* [`NSMutableArray`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSMutableArray)
* [`NSMutableAttributedString`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAttributedStrngClstr/Description.html#//apple_ref/occ/cl/NSMutableAttributedString)
* [`NSMutableCharacterSet`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCharacterSetClstr/Description.html#//apple_ref/occ/cl/NSMutableCharacterSet)
* [`NSMutableData`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSMutableData)
* [`NSMutableDictionary`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSMutableDictionary)
* [`NSMutableOrderedSet`](https://developer.apple.com/documentation/foundation/nsmutableorderedset)
* [`NSMutableSet`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/cl/NSMutableSet)
* [`NSMutableString`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSMutableString)
* [`NSNotificationQueue`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationQueue/Description.html#//apple_ref/occ/cl/NSNotificationQueue)
* [`NSPipe`](https://developer.apple.com/documentation/foundation/nspipe)
* [`NSPort`](https://developer.apple.com/documentation/foundation/nsport)
* [`NSProcessInfo`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSProcessInfo/Description.html#//apple_ref/occ/cl/NSProcessInfo)
* [`NSRunLoop`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop)
* [`NSScanner`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/cl/NSScanner)
* [`NSSerializer`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSerializer/Description.html#//apple_ref/occ/cl/NSSerializer)
* [`NSTask`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTask/Description.html#//apple_ref/occ/cl/NSTask)
* [`NSUnarchiver`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSUnarchiver/Description.html#//apple_ref/occ/cl/NSUnarchiver)
* [`NSUndoManager`](https://developer.apple.com/documentation/foundation/undomanager)
* User name and home directory functions

`NSArchiver`, `NSCoder` 및 `NSEnumerator` 객체는 그 자체로 쓰레드 세이프하지만, 사용 중에 해당 객체가 감싼 데이터 객체를 변경하는 것은 안전하지 않기 때문에 여기에 나열된다. 예를 들어, 아카이버의 경우 보관 중인 객체 그래프를 변경하는 것은 안전하지 않다. 열거자의 경우, 어떤 쓰레드도 열거된 콜렉션을 변경하는 것은 안전하지 않다.

**Main Thread Only Classes**

다음 클래스는 애플리케이션의 메인 쓰레드에서만 사용해야 한다.

[NSAppleScript](https://developer.apple.com/documentation/foundation/nsapplescript)

**Mutable Versus Immutable**

불변의 객체는 일반적으로 쓰레드 세이프하다. 일단 그것을 만들면, 당신은 이러한 객체를 쓰레드로부터 안전하게 통과시킬 수 있다. 물론 불변의 객체를 사용할 때는 참조 카운트를 올바르게 사용해야 한다. 유지하지 않은 객체를 부적절하게 릴리스하면 나중에 예외를 발생시킬 수 있다.

변경가능한 객체는 일반적으로 쓰레드 세이프하지 않다. 쓰레드가 있는 애플리케이션에서 변경 가능한 객체를 사용하려면 애플리케이션이 락을 사용하여 해당 객체에 대한 접근을 동기화해야 한다. \(자세한 내용은 [Atomic Operations](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/ThreadSafety/ThreadSafety.html#//apple_ref/doc/uid/10000057i-CH8-SW2)을 참조하라\). 일반적으로 변경가능한에 관한 경우 콜렉션 클래스 \(예를 들어, [`NSMutableArray`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSMutableArray), [`NSMutableDictionary`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSMutableDictionary)\)는 안전하지 않다. 즉, 하나 이상의 쓰레드가 동일한 배열을 변경하는 경우 문제가 발생할 수 있다. 쓰레드의 안전성을 보장하기 위해 읽기와 쓰기가 발생하는 지점을 잠그고 있어야 한다.

어떤 메서드가 불변의 객체를 반환한다고 주장하더라도, 단순히 반환된 객체가 불변이라고 가정해서는 안된다. 메서드 구현에 따라 반환된 객체는 변경가능하거나 불변할 수 있다. 예를 들어, [`NSString`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)의 반환 타입을 사용하는 메서드는 실제로 구현으로 [`NSMutableString`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSMutableString)을 반환할 수 있다. 가지고 있는 객체가 불변의 것임을 보증하려면 불변의 사본을 만들어야 한다.

**Reentrancy**

재진입성은 동일한 객체 또는 다른 객체의 다른 작업에 대한 연산이 "콜 아웃"될 때만 가능하다. 객체를 리테인하고 릴리즈 하는 것은 때때로 간과되는 그런 "콜 아웃"의 하나이다.

다음 표에는 명시적으로 재진입된 파운데이션 프레임워크의 부분이 나열되어 있다. 다른 모든 클래스는 재진입할 수도 있고 그렇지 않을 수도 있으며, 향후 재진입할 수 도 있다. 재진입에 대한 완전한 분석이 수행된 적이 없으며 이 목록은 완전하지 않을 수 있다.

* Distributed Objects
* [`NSConditionLock`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSConditionLock/Description.html#//apple_ref/occ/cl/NSConditionLock)
* [`NSDistributedLock`](https://developer.apple.com/documentation/foundation/nsdistributedlock)
* [`NSLock`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSLock/Description.html#//apple_ref/occ/cl/NSLock)
* [`NSLog`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSLog)/[`NSLogv`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSLogv)
* [`NSNotificationCenter`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationCenter/Description.html#//apple_ref/occ/cl/NSNotificationCenter)
* [`NSRecursiveLock`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRecursiveLock/Description.html#//apple_ref/occ/cl/NSRecursiveLock)
* [`NSRunLoop`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop)
* [`NSUserDefaults`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSUserDefaults/Description.html#//apple_ref/occ/cl/NSUserDefaults)

**Class Initialization**

Objective-C 런타임 시스템은 클래스가 다른 메시지를 수신하기 전에 모든 클래스 객체에 initialize 메시지를 전송한다. 이것은 클래스를 사용하기 전에 런타임 환경을 설정할 수 있는 기회를 제공한다. 멀티 쓰레드 애플리케이션에서 런타임은 한 개의 쓰레드, 즉 첫번째 메시지를 클래스에 보내는 쓰레드만 `initialize` 메서드를 실행하도록 한다. 첫 번째 쓰레드가 아직 `initialize` 메서드에 있는 동안 두 번째 쓰레드가 클래스에 메시지를 보내려고 하면, 두 번째 쓰레드는 `initialize` 메서드가 실행을 마칠 때까지 차단된다. 한편, 첫 번째 쓰레드는 클래스에서 계속해서 다른 메서드를 호출할 수 있다. `initialize` 메서드는 두 번째 쓰레드 호출 메서드에 의존해서는 안된다. 그렇게 되면 두 쓰레드가 교착 상태가 된다.

OS X 10.1.x 버전 이하의 버그로 인해 쓰레드는 다른 쓰레드가 해당 클래스의 `initialize` 메서드를 완료하기 전에 클래스에 메시지를 보낼 수 있었다. 그러면 쓰레드는 완전히 초기화되지 않은 값에 접근할 수 있으며, 아마도 애플리케이션을 손상시킬 수 있다. 이 문제가 발생하면, 초기화된 후에야 값에 접근할 수 있도록 락을 도입하거나 멀티 쓰레드가 되기 전에 클래스를 강제로 초기화해야 한다.

**Autorelease Pools**

각 쓰레드는 자체 [`NSAutoreleasePool`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAutoreleasePool/Description.html#//apple_ref/occ/cl/NSAutoreleasePool) 객체의 스택을 유지한다. 코코아는 현재 쓰레드 스택에 항상 오토릴리즈 풀이 있을것으로 예상한다. 풀을 사용할 수 없으면 객체가 해제되지 않고 메모리가 누수된다. `NSAutoreleasePool` 객체는 애플리케이션 키트에 기반한 애플리케이션의 메인 쓰레드에서 자동으로 생성되고 파괴되지만, 보조 쓰레드\(및 파운데이션 전용 애플리케이션\)는 코코아를 사용하기 전에 자체적인 쓰레드를 생성해야 한다. 쓰레드가 수명이 길고 잠재적으로 많은 오토릴리즈된 객체를 생성할 수 있는 경우 애플리케이션 키트가 메인 쓰레드에 생성하는 것처럼 주기적으로 오토 릴리즈 풀을 파괴하고 생성해야 한다. 그렇지 않으면 오토릴리즈된 객체가 누적되어 메모리 공간이 커진다. 분리된 쓰레드가 코코아를 사용하지 않으면 오토릴리즈 풀을 만들 필요가 없다.

**Run Loops**

모든 쓰레드에는 하나의 런 루프가 있다. 그러나 각 런 루프와 따라서 각 쓰레드는 런 루프가 실행될 때 어떤 입력 소스가 수신되는지 결정하는 자체 입력 모드 세트를 가지고 있다. 하나의 런 루프에 정의된 입력 모드는 동일한 이름을 가질 수 있더라도 다른 런 루프에 정의된 입력 모드에 영향을 미치지 않는다.

애플리케이션이 애플리케이션 키트를 기반으로 하는 경우 메인 쓰레드의 런 루프가 자동으로 실행되지만 보조 쓰레드\(및 파운데이션 전용 애플리케이션\)는 런 루프를 직접 실행해야 한다. 분리된 쓰레드가 런 루프에 들어가지 않으면 분리 메서가 실행을 마치는 즉시 쓰레드가 종료된다.

약간의 외관에도 불구하고, [`NSRunLoop`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop) 클래스는 안전하지 않다. 이 클래스의 인스턴스 메서드를 소유한 쓰레드에서만 호출해야 한다.

#### Application Kit Framework Thread Safety

다음 섹션에서는 애플리케이션 키트 프레임워크의 일반적인 쓰레드 세이프에 대해 설명한다.

**Thread-Unsafe Classes**

다음 클래스와 함수는 일반적으로 안전하지 않다. 대부분의 경우, 한 번에 하나의 쓰레드에서만 이 클래스를 사용하는 한, 어떤 쓰레드에서도 이 클래스를 사용할 수 있다. 자세한 내용은 클래스 설명서를 참조하라.

* [`NSGraphicsContext`](https://developer.apple.com/documentation/appkit/nsgraphicscontext). For more information, see [NSGraphicsContext Restrictions](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/ThreadSafetySummary/ThreadSafetySummary.html#//apple_ref/doc/uid/10000057i-CH12-126712).
* [`NSImage`](https://developer.apple.com/documentation/appkit/nsimage). For more information, see [NSImage Restrictions](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/ThreadSafetySummary/ThreadSafetySummary.html#//apple_ref/doc/uid/10000057i-CH12-126728).
* [`NSResponder`](https://developer.apple.com/documentation/appkit/nsresponder)
* [`NSWindow`](https://developer.apple.com/documentation/appkit/nswindow) and all of its descendants. For more information, see [Window Restrictions](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/ThreadSafetySummary/ThreadSafetySummary.html#//apple_ref/doc/uid/10000057i-CH12-123364).

**Main Thread Only Classes**

다음 클래스는 애플리케이션의 메인 쓰레드에서만 사용해야 한다:

* [`NSCell`](https://developer.apple.com/documentation/appkit/nscell) 과 그 후손 클래스들.
* [`NSView`](https://developer.apple.com/documentation/appkit/nsview) 과 그 후손 클래스들. 더 자세한 정보는 [NSView Restrictions](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/ThreadSafetySummary/ThreadSafetySummary.html#//apple_ref/doc/uid/10000057i-CH12-123427)를 참조하라.

**Window Restrictions**

보조 쓰레드에 윈도우를 만들 수 있다. 애플리케이션 키트는 윈도우와 관련된 데이터 구조가 경쟁 조건을 피하기 위해 메인 쓰레드에 할당되지 않도록 한다. 많은 윈도우를 다루는 애플리케이션에서 윈도우 객체가 누수될 가능성이 있다.

보조 쓰레드에 모달 윈도우를 만들 수 있다. 메인 쓰레드가 모달 루프를 실행하는 동안 모달 루프를 실행하는 동안 애플리케이션 키트는 보조 쓰레드 호출을 차단한다.

**Event Handling Restrictions**

애플리케이션의 메인 쓰레드는 이벤트 처리를 담당한다. 메인 쓰레드는 [`NSApplication`](https://developer.apple.com/documentation/appkit/nsapplication)의 `run` 메서드에서 차단된 쓰레드로, 일반적으로 애플리케이션의 메인 함수에서 호출된다. 다른 쓰레드가 이벤트 경로에 관련될 경우 애플리케이션 키트가 계속 작동하지만, 작업이 순서에 따라 수행될 수 있다. 예를 들어, 두 개의 서로 다른 쓰레드가 주요 이벤트에 응답하는 경우, 키가 잘못된 상태로 수신될 수 있다.

`NSApplication`의 [`postEvent:atStart:`](https://developer.apple.com/documentation/appkit/nsapplication/1428710-postevent)메서드를 호출하여 보조 쓰레드에서 메인 쓰레드 이벤트 큐의 이벤트 큐에 게시할 수 있다. 단, 사용자 입력 이벤트에 관해서는 순서가 보장되지 않는다. 애플리케이션의 메인 쓰레드는 여전히 이벤트 큐에서 이벤트를 처리하는 역할을 한다.

**Drawing Restrictions**

애플리케이션 키트는 [`NSBezierPath`](https://developer.apple.com/documentation/appkit/nsbezierpath) 및 [`NSString`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)클래스를 포함하여 그래픽 함수화 클래스를 사용하여 그릴 때 일반적으로 쓰레드 세이프하다. 특정 클래스를 사용하기 위한 세부사항은 다음 섹션에 설명되어 있다. 그리기 및 쓰레드에 대한 자세한 내용은 [_Cocoa Drawing Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290)를 참조하라.

**NSView Restrictions**

[`NSView`](https://developer.apple.com/documentation/appkit/nsview)클래스는 일반적으로 쓰레드 세이프하지 않다. 애플리케이션의 기본 쓰레드에서만 `NSView` 객체에 대해 생성, 파괴, 크기 조정, 이동 및 기타 작업을 수행해야 한다. 보조 쓰레드에서 그리는 것 호출은 [`lockFocusIfCanDraw`](https://developer.apple.com/documentation/appkit/nsview/1483285-lockfocusifcandraw)및 [`unlockFocus`](https://developer.apple.com/documentation/appkit/nsview/1483711-unlockfocus)을 위한 호출과 함께 하는 한 쓰레드 세이프하다.

애플리케이션의 보조 쓰레드가 메인 쓰레드에 뷰의 일부를 다시 그리게 하려면 `display`, [`setNeedsDisplay:`](https://developer.apple.com/documentation/appkit/nsview/1483360-needsdisplay), [`setNeedsDisplayInRect:`](https://developer.apple.com/documentation/appkit/nsview/1483475-setneedsdisplayinrect) 또는 [`setViewsNeedDisplay:`](https://developer.apple.com/documentation/appkit/nswindow/1419609-viewsneeddisplay)와 같은 메서드를 사용하여 뷰의 일부를 다시 그려서는 안된다.

대신 메인 쓰레드에 메시지를 보내거나 [`performSelectorOnMainThread:withObject:waitUntilDone:`](https://developer.apple.com/documentation/objectivec/nsobject/1414900-performselector)를 사용하여 해당 메서드를 호출해야 한다.

뷰 시스템의 그래픽 상태\(gstate\)는 쓰레드 마다이다. 그래픽 상태를 사용하는 것은 단일 쓰레드 애플리케이션보다 더 나은 그리기 성능을 달성하는 방법이었지만 더 이상 그렇지 않다. 그래픽 상태를 잘못 사용하면 실제로 메인 쓰레드에 그리는 것보다 효율이 떨어지는 그리기 코드가 발생할 수 있다.

**NSGraphicsContext Restrictions**

[`NSGraphicsContext`](https://developer.apple.com/documentation/appkit/nsgraphicscontext)클래스는 기본 그래픽 시스템에서 제공하는 그리기 컨텍스트를 나타낸다. 각 `NSGraphicsContext`인스턴스는 좌표계, 클리핑, 현재 글꼴 등 독자적인 그래픽 상태를 가지고 있다. 클래스의 인스턴스는 각 [`NSWindow`](https://developer.apple.com/documentation/appkit/nswindow)인스턴스의 메인 쓰레드에 자동으로 생성된다. 보조 쓰레드에서 그리기를 수행하면 해당 쓰레드에 대해 새로운 `NSGraphicsContext`인스턴스가 생성된다.

보조 쓰레드에서 그리기를 하는 경우 수동으로 그리기 호출을 플러시해야 한다. 코코아는 보조 쓰레드에서 가져온 콘텐츠로 뷰를 자동으로 업데이트하지 않으므로 그리기를 마치면 `NSGraphicsContext`의 [`flushGraphics`](https://developer.apple.com/documentation/appkit/nsgraphicscontext/1527919-flushgraphics)메서드를 호출해야 한다. 애플리케이션이 메인 쓰레드에서만 콘텐츠를 그리는 경우 그리기 호출을 플러시할 필요가 없다 

**NSImage Restrictions**

하나의 쓰레드는 [`NSImage`](https://developer.apple.com/documentation/appkit/nsimage) 객체를 생성하여 이미지 버퍼에 그린 후 메인 쓰레드로 전달하여 그릴 수 있다. 기본 이미지 캐시는 모든 쓰레드 간에 공유된다. 이미지 및 캐싱 작동 방법에 대한 자세한 내용은 [_Cocoa Drawing Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290)를 참조하라.

#### Core Data Framework

코어 데이터 프레임워크는 일반적으로 쓰레딩을 지원하지만, 일부 사용 경고가 적용된다. 이러한 경고에 대한 자세한 내용은 [_Core Data Programming Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/index.html#//apple_ref/doc/uid/TP40001075)를 참조하라.

### Core Foundation

코어 파운데이션은 충분히 안전하므로, 주의 깊게 프로그래밍할 경우, 경쟁하는 쓰레드와 관련된 문제에 부딪쳐서는 안된다. 그것은 당신이 불변 객체를 쿼리하고, 보관하고, 해제하고, 전달할 때와 같은 일반적인 경우에 쓰레드처럼 안전하다. 둘 이상의 쓰레드에서 쿼리할 수 있는 중심 공유 객체도 안정적으로 쓰레드에 안전하다.

코코아처럼, 코어 파운데이션은 객체나 그 콘텐츠에 대한 변경가능에 관해서는 안전하지 않다. 예를 들어, 변경 가능한 데이터나 변경 가능한 배열 객체를 수정하는 것은 예상하는 바와 같이 쓰레드 세이프는 아니지만, 불변성 배열 내부의 객체를 수정하는 것도 아니다. 그 이유 중 하나는 이러한 상황에서 중요한 성능이다. 게다가 이 수준에서는 일반적으로 절대 쓰레드 안전을 달성할 수 없다. 예를 들어, 콜렉션에서 얻은 객체를 유지함으로써 발생하는 불확정적인 행동을 배제할 수 없다. 콜렉션 자체는 포함된 객체를 보관하라는 호출을 하기 전에 해제될 수 있다.

코어 파운데이션 객체가 여러 쓰레드에서 접근되고 변경가능할 경우 코드는 접근 지점에서 락을 사용하여 동시 접근으로부터 보호해야 한다. 예를 들어, 코어 파운데이션 배열의 객체를 열거하는 코드는 열거 블록 주변의 적절한 락 호출을 사용하여 배열을 변경하는 다른 것으로부터 보호해야 한다. 메인 쓰레드 프로세스 이벤트를 허용함으로써 보다 일관된 사용자 환경을 실현할 수 있다. 일단 수신되면, 이벤트는 필요에 따라 추가 처리를 위해 보조 쓰레드로 전송될 수 있다.



