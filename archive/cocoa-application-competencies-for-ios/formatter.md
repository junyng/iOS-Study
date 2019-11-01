# Formatter

포맷터는 값의 문자열 표현을 자동으로 해당 값을 나타내는 객체로 자동 변환하는 객체이다. 예를 들어, [`NSNumberFormatter`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumberFormatter/Description.html#//apple_ref/occ/cl/NSNumberFormatter)객체가 문자열 "1.25"를 [`NSNumber`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/cl/NSNumber)객체 1.25로 변환할 수 있다. [`NSDateFormatter`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateFormatter/Description.html#//apple_ref/occ/cl/NSDateFormatter)객체는 2009년 12월 12일을 나타내는 `NSDate` 객체를 "11/22/2009"로 변환할 수 있다. 보시다시피, 변환은 문자열에서 값 객체, 값 객체에서 문자열로 양방향으로 작용한다. 포맷터의 추상 기본 클래스는 [`NSFormatter`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSFormatter/Description.html#//apple_ref/occ/cl/NSFormatter)이다. `NSFormatter`의 하위 클래스를 사용하여 다른 유형의 데이터에 대한 포맷터를 만들 수 있다. 심지어 애플리케이션의 데이터 모델에 의해 정의된 사용자 정의 데이터 유형도 가능하다.

![](../../.gitbook/assets/formatter.jpg)

![](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/formatter.jpg)

## Configuring and Applying a Formatter

날짜나 숫자 포맷터 객체를 만들 때 여러 가지 방법으로 구성할 수 있지만, 주요 특성은 포맷터 스타일과 로케일이다. `NSNumberFormatter` 객체에 소수, 통화, 백분율, 과학적 또는 "spell-out" 스타일을 부여할 수 있다. \(예를 들어, "25"에서 "twenty-five"\) `NSDateFormatter` 객체는 다양한 범위의 날짜와 시간 스타일을 모두 포함한다. 예를 들어 "11/22/2009" 부터 "Sunday, November 22, 2009 AD"

또한 [`NSLocale`](https://developer.apple.com/documentation/foundation/nslocale) 객체를 포맷터 객체에 적용하여 특정 로케일을 반영할 수 있다. 예를 들어, 미국 영어의 "1.02"는 프랑스어로 "1,02"가 될 것이다. 현재 로케일을 가져오려면 \(사용자가 설정한\), `NSLocale` 클래스 메서드 [`currentLocale`](https://developer.apple.com/documentation/foundation/nslocale/1409990-currentlocale)를 호출하라.

날짜 또는 숫자 형식을 구성한 후 사용자 인터페이스에서 가져온 문자열 \(일반적으로 텍스트 필드\)에 적용하여 값 객체를 얻어라. 아니면 `NSDate` 또는 `NSNumber` 객체에 적용하고 사용자 인터페이스 객체에 결과 문자열을 작성한다. 이러한 목적을 위한 메서드는 [`dateFromString:`](https://developer.apple.com/documentation/foundation/dateformatter/1409994-date), [`stringFromDate:`](https://developer.apple.com/documentation/foundation/nsdateformatter/1415810-stringfromdate), [`numberFromString:`](https://developer.apple.com/documentation/foundation/numberformatter/1408845-number), [`stringFromNumber:`](https://developer.apple.com/documentation/foundation/nsnumberformatter/1418046-stringfromnumber) 이다.

## In OS X, You Can Attach a Formatter to a Cell

OS X에서는 숫자 또는 날짜 포맷터 객체를 프로그래밍 방식 또는 인터페이스 빌더으로 셀 객체와 연결할 수 있다. 문자열과 숫자 또는 날짜 객체 사이의 변환은 자동으로 발생한다. 변환을 수행하기 위해 어떤 메서드도 호출할 필요가 없다. 셀 객체는 텍스트 필드 이외의 객체와 연관될 수 있다. 예를 들어, 테이블 뷰 또는 개요 뷰에서 셀에 포맷터 객체를 할당할 수 있다.

### Prerequisite Articles

\(None\)

### Related Articles

[Internationalization](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Internationalization.html#//apple_ref/doc/uid/TP40008195-CH23)

**Definitive Discussion**

\_\_[_Data Formatting Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/DataFormatting/DataFormatting.html#//apple_ref/doc/uid/10000029i)

**Sample Code Projects**

[DateCell](https://developer.apple.com/library/archive/samplecode/DateCell/Introduction/Intro.html#//apple_ref/doc/uid/DTS40008866)

