# Value object

값 객체는 본질적으로 문자열, 숫자 또는 날짜와 같은 단순한 데이터 요소에 대한 객체지향 래퍼이다. Cocoa의 공통 값 클래스는 NSString, NSDate, NSNumber이다. 값 객체는 종종 사용자가 생성하는 다른 사용자 지정 객체의 속성이다.

값 객체가 해당 단순 스칼라 타입보다 풍부한 동작을 제공한다.\(char, NSTimeInterval, int, float, double\)

* NSArray 또는 NSDictionary의 인스턴스처럼 값 객체를 콜렉션에 넣을 수 있다.
* NSString 및 그 서브클래스 NSMutableString을 사용하여 광범위한 문자열 관련 작업을 수행할 수 있다. 예를 들어 문자열을 하나로 묶고, 문자열을 분리하고, 파일 경로를 작업하고, 문자의 경우를 변환하고, 서브스트링을 검색할 수 있다. 이 모든 것에서 문자열 객체는 유니코드로 처리된다.
* NSDate를 사용하면 NSCalendar 및 기타 관련 클래스와 연계하여 사용자가 선호하는 캘린더를 기준으로 두 인스턴스 사이의 월과 일수를 결정하는 것과 같이 시간대와 윤년 등의 변수를 고려한 복잡한 캘린더 계산을 수행할 수 있다.
* NSNumber 서브클래스 NSDecimalNumber를 사용하여 통화 기반 계산을 정확하게 수행할 수 있다.

### NSValue

NSValue는 C 또는 Objective-C 데이터 항목에 대한 간단한 컨테이너를 제공한다. 그것은 char, int, float, double과 같은 스칼라 타입과 포인터, 구조체, 객체 ID를 포함할 수 있다. 이러한 데이터 타입의 항목을 NSArray 및 NSSet 인스턴스와 같은 컬렉션에 추가할 수 있으며, 이러한 컬렉션의 요소는 객체가 되어야 한다. 이는 점, 크기, 사각형 구조\(예: NSPoint, CGSize, NSRect\)를 집합에 넣어야 하는 경우에 특히 유용하다.

