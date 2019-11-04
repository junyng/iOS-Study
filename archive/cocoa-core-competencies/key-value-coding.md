# Key-value coding

Key-value coding은 문자열 식별자를 사용하여 객체의 속성 및 관계에 간접적으로 접근하는 메커니즘이다. 그것은 Cocoa 프로그래밍에 특별한 몇 가지 메커니즘과 기술과 관련이 있으며, 그 중에서도 Core Data, 애플리케이션 스크립팅 가능성, 바인딩 기술, 선언된 프로퍼티의 언어 특징과 관련된다. \(스크립팅 가능성과 바인딩은 OS X의 Cocoa에만 해당된다.\) Key-value 코딩을 사용하여 프로그램 코드를 간소화할 수도 있다.

### Object Properties and KVC

Key-value coding\(또는 KVC\)는 프로퍼티의 일반적인 개념이다. 프로퍼티는 객체가 캡슐화하는 단위를 말한다. 프로퍼티는 다음 두 가지 일반적인 타입 중 하나가 될 수 있다: 속성\(예: 이름, 제목, 소계, 텍스트 색상\) 또는 다른 객체와의 관계. 관계는 일대일 또는 일대다가 될 수 있다. 일대다 관계의 값은 일반적으로 관계가 순서가 있는지 없는지에 따라 배열 또는 세트이다.

KVC는 문자열 식별자인 키를 통해 객체의 속성을 찾는다. 키는 일반적으로 객체에 의해 정의된 접근방식 또는 인스턴스 변수의 이름에 해당한다. 키는 특정 규칙을 준수해야 한다. 그것은 반드시 ASCII 인코딩되어야 하고 소문자로 시작되어야 하며 공백이 없어야 한다. 키 경로는 통과할 객체 프로퍼티의 순서를 지정하는 데 사용되는 도트구분 키의 문자열이다. 시퀀스에서 첫 번째 키의 프로퍼티는 특정 객체\(다음 다이어그램의 employee1\)에 상대적이다. 각 후속 키는 이전 프로퍼티의 값에 비례하여 평가된다.

![](file:///Users/BLU/TIL/iOS/Cocoa-Core-Competencies/Images/key_value_coding_2x.png?lastModify=1572840913)

### Making a Class KVC Compliant

`NSKeyValueCoding` 비공식 프로토콜은 KVC를 가능하게 한다. 두 가지 메서드 - `valueForKey:`와 `setValue:forKey:`는 키가 주어졌을 때 프로퍼티 값을 가져오고 설정하기 때문에 특히 중요하다. `NSObject`는 이러한 메서드의 기본 구현을 제공하며, 클래스가 key-value coding을 준수하는 경우 이 구현에 의존할 수 있다.

KVC를 준수하도록 하는 방법은 해당 프로퍼티가 속성인지, 일대일 관계인지 또는 일대다 관계인지에 따라 달라진다. 속성 및 일대일 관계의 경우 클래스는 지정된 선호도 순서로 다음 중 하나 이상을 구현해야 한다. \(키는 프로퍼티 키를 말한다.\)

1. 클래스는 이름 키를 가진 선언된 프로퍼티를 가지고 있다.
2. 키라는 이름의 수신 메서드를 구현하고, 프로퍼티가 변형 가능한 경우 `setKey:`를 설정한다.\(프로퍼티가 부울 속성인 경우 getter 접근 방법에는 `isKey`형식이 있다.\)
3. form key 또는 \_key의 인스턴스 변수를 선언한다.

KVC 규정 준수를 이행하는 것은 복잡한 절차이다. 이 절차가 무엇인지 자세히 알아보려면 key-value 코딩을 명확하게 설명하는 문서를 참조하라.

#### Prerequisite Articles

* Object modeling
* Accessor method

#### Related Articles

* Declared property

#### Definitive Discussion

* Key-Value Coding Programming Guide

