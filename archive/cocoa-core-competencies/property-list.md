# Property list

프로퍼티 리스트는 파일 시스템에 저장하고 나중에 재구성할 수 있는 객체의 계층 구조를 나타낸 것이다. 프로퍼티 리스트를 통해 애플리케이션에서 적은 양의 데이터를 저장할 수 있는 경량 및 휴대용 방법을 제공한다. 특정 객체 타입으로 만들어진 데이터 계층은 실제로 객체그래프이다. 프로퍼티 리스트는 프로그램적으로 작성하기 쉽고, 영구적인 표현으로 직렬화하기가 더 쉽다. 애플리케이션은 나중에 정적 표현을 메모리로 다시 읽고 객체의 원래 계층을 재생성할 수 있다. Cocoa Foundation과 Core Foundation 모두 프로퍼티 리스트의 직렬화 및 역직렬화와 관련된 API를 가지고 있다.

### Property List Types and Objects

프로퍼티 리스트는 딕셔너리, 배열, 문자열, 숫자\(integer, float\), 날짜, 이진 데이터 및 불 값의 특정 타입 데이터로 구성된다. 딕셔너리와 배열은 컬렉션이기 때문에 특별한 타입이다. 그들은 다른 딕셔너리와 배열을 포함한 하나 또는 여러 데이터 타입을 포함할 수 있다. 객체의 계층적 둥지는 객체 그래프를 생성한다. 추상 데이터 타입은 다음 목록과 같이 컬렉션 객체 및 값 객체에 대한 Foundation 클래스, Core Foundation 타입 및 XML 요소를 가지고 있다.

| Abstract type | Foundation framework class | Core Foundation type | XML element |
| :--- | :--- | :--- | :--- |
| Array | NSArray | CFArrayRef | &lt;array&gt; |
| Dictionary | NSDictionary | CFDictionaryRef | &lt;dict&gt; |
| String | NSString | CFStringRef | &lt;string&gt; |
| Data | NSData | CFDataRef | &lt;data&gt; |
| Date | NSDate | CFDateRef | &lt;date&gt; |
| Integer | NSNumber \(intValue 32-bit\) NSNumber \(integerValue 64-bit\) | CFNumberRef \(kCFNumberSInt32Type\) CFNumberRef \(kCFNumberSInt64Type\) | &lt;integer&gt; |
| Floating-point value | NSNumber \(floatValue 32-bit\) NSNumber \(doubleValue 64-bit\) | CFNumberRef \(kCFNumberSFloat32Type\) CFNumberRef \(kCFNumberSFloat64Type\) | &lt;real&gt; |
| Boolean | NSNumber \(boolValue\) | CFBooleanRef | &lt;true/&gt; |

전체적으로 이러한 클래스의 인스턴스를 프로퍼티 리스트 객체라고 한다. 예를 들어 NSMutableDictionary 객체는 NSNumber, NSString 객체 등과 같이 프로퍼티 리스트 객체다. 프로퍼티 리스트가 유효하려면 객체 그래프의 모든 객체가 프로퍼티 리스트 객체여야 한다.

### Best Practices for Property Lists

프로퍼티 리스트를 XML과 바이너리 포멧으로 모두 쓸 수 있다. 바이너리 포멧은 XML 버전보다 훨씬 더 작고 따라서 더 효율적이다. 그것은 대부분의 경우에 권장된다. 그러나 필요한 경우 XML 프로퍼티 리스트를 수동으로 편집할 수 있다. 프로퍼티 리스트 파일은 plist 확장자명이 있다.

특히 객체의 가변성 설정이 있는 경우, 프로퍼티 리스트를 사용하여 객체의 크고 복잡한 그래프를 저장해서는 안된다. 또한 모델 객체와 같이 아키텍처에서 지원되지 않는 객체를 저장하는 데 프로퍼티 리스트를 사용할 수 없다. 이러한 경우에는 아카이빙을 사용하라. 프로퍼티 리스트는 NSData 객체가 포함될 수 있지만 프로퍼티 리스트의 데이터 객체를 사용하여 많은 양의 이진 데이터를 저장하지 않는 것이 가장 좋다.

### Property List Serialization

프로퍼티 리스트를 직렬화하고 다시 초기화하려면 NSPropertyListSerialization 클래스의 해당 클래스 메서드를 호출하거나 Core Foundation을 사용하는 경우 CFPropertyListRef 불투명 타입과 관련된 기능을 호출하라. Cocoa에서 직렬화된 출력은 NSData 객체 형식이다. 따라서 해당 클래스의 메서드\(예: `writeToFile:atomically:`\)를 사용하여 해당 데이터를 파일 시스템에 쓰고 적절한 NSData 클래스 팩토리 메서드를 사용하여 해당 데이터를 메모리로 다시 읽을 수 있다. 그런 다음 다시 초기화하면 프로퍼티 리스트에 대한 변경 옵션을 지정할 수 있다.

#### Prerequisite Articles

Object graph  
Collection

#### Related Articles

Object archiving  
Object mutability

#### Definitive Discussion

Property List Programming Guide

#### Sample Code Projects

People

