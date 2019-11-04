# Object archiving

아카이빙은 관련 객체 그룹을 애플리케이션 간에 저장하거나 전송할 수 있는 형태로 변환하는 과정이다. 아카이빙의 결과 아카이브는 객체의 ID, 캡슐화된 값, 다른 객체와의 관계를 기록하는 바이트 스트림이다. 반대 과정인 언아카이빙은 아카이브를 가져와서 동일한 객체 네트워크를 재구성한다.

아카이빙의 주요 가치는 객체를 영구적으로 유지할 수 있는 일반적인 방법을 제공한다는 것이다. 애플리케이션은 객체 데이터를 특수 파일 형식으로 작성하는 대신, 자주 자신의 모델 객체를 파일로 쓸 수 있는 아카이브에 저장한다. 애플리케이션은 또한 일반적으로 객체 그래프로 알려진 객체 네트워크를 다른 애플리케이션으로 전송할 수 있다. 애플리케이션은 종종 복사 및 붙여넣기 같은 페이스트보드 작업을 위해 이것을 한다.

![](file:///Users/BLU/TIL/iOS/Cocoa-Core-Competencies/Images/archiving_2x.png?lastModify=1572841132)

해당 인스턴스를 아카이브에 포함하려면 클래스가 NSCoding 프로토콜을 채택하고 객체를 인코딩 및 디코딩하는 데 필요한 메서드를 구현해야 한다. Cocoa 아카이브는 Objective-C 객체, C 배열, 구조체, 문자열을 저장할 수 있다. 아카이브는 캡슐화된 데이터와 함께 객체의 타입을 저장하므로, 바이트 스트림에서 디코딩된 객체는 원래 스트림에 인코딩된 객체와 동일한 클래스이다.

### Keyed and Sequential Archivers

Foundation 프레임워크는 객체 네트워크의 아카이빙 및 언아카이빙을 위한 두 가지 클래스 셋을 제공한다. 여기에는 아카이빙 및 언아카이빙 프로세스를 시작하고 객체의 인스턴스 데이터를 인코딩 및 디코딩하는 메서드가 포함된다. 이러한 클래스의 객체를 archivers 및 unarchivers 라고 부른다.

* Keyed archivers 및 unarchivers \(NSKeyedArchiver, NSKeyedUnarchiver\). 이러한 객체는 인코딩 및 디코딩할 데이터의 식별자로 문자열 키를 사용한다. 이 객체들은 특히 새로운 애플리케이션에서 아카이빙 및 언아카이빙 객체에 선호되는 객체들이다.
* Sequential archivers 및 unarchivers \(NSArchiver, NSUnarchiver\). 구식 archiver는 객체 상태를 일정한 순서로 인코딩한다. uncrchiver는 객체 상태를 동일한 순서로 디코딩하기를 예상한다. 이들의 용도는 레거시 코드를 위한 것이다. 새로운 애플리케이션은 Keyed archives를 대신 사용해야 한다.

### Creating and Decoding Keyed Archives

애플리케이션은 NSKeyedArchiver의 클래스 메서드 `archiveRootObject:toFile:`를 호출하여 아카이브를 생성한다. 이 메서드의 첫 번째 매개변수는 객체 그래프의 루트 객체를 참조한다. 루트 객체를 시작으로, NSCoding 프로토콜을 준수하는 그래프의 각 객체는 아카이브로 인코딩할 수 있는 기회가 주어진다. 결과 바이트 스트림은 지정된 파일에 기록된다.

아카이브 디코딩은 반대 방향으로 진행된다. 애플리케이션은 NSKeyedUnarchiver 클래스 메서드 `unarchiveObjectWithFile:`를 호출한다. 아카이브 파일을 사용할 경우 이 메서드는 객체 그래프를 다시 생성하여 그래프에서 각 객체의 바이트 스트림안의 관련 데이터를 디코딩하고 객체를 재생성한다. 이 메서드는 루트 객체에 대한 참조를 반환하는 것으로 끝난다.

NSKeyedArchiver 클래스 메서드 `archivedDataWithRootObject:` 및 `unarchiveObjectWithData:`는 파일보다는 데이터 객체와 함께 작업한다는 점을 제외하면 위의 메서드와 동일하다.

#### Prerequisite Articles

Object graph  
Object encoding

#### Related Articles

Model object  
Property list  
Object life cycle

#### Definitive Discussion

Archives and Serializations Programming Guide

