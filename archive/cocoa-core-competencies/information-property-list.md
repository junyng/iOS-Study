# Information property list

정보 프로퍼티 목록은 애플리케이션 또는 기타 번들 실행 파일의 구성 상세 내역을 지정하는 구조화된 텍스트다. 운영체제는 런타임에 정보 프로퍼티 목록에서 데이터를 추출하여 적절한 방법으로 처리한다. 예를 들어, 홈 스크린이나 파인더, iOS 및 OS X에서\(각각\) 애플리케이션을 나열할 때 설치된 애플리케이션의 정보 프로퍼티 목록에서 이러한 이름을 얻는다.

정보 프로퍼티 목록의 내용은 루트 노드가 항상 딕셔너리인 XML의 특별한 폼으로 구성된다. 딕셔너리는 일련의 키-값 쌍 또는 프로퍼티가 포함되어 있다. 여기서 키는 키 요소이고 값은 값 데이터 타입을 나타내는 요소이다.

![](file:///Users/BLU/TIL/iOS/Cocoa-Core-Competencies/Images/plist_2x.png?lastModify=1572840779)

키-값 쌍의 예시이다.

```text
<key>CFBundleDisplayName</key><string>Mail</string>
```

정보 프로퍼티 목록 파일의 이름은 Info.plist 여야 하며 그림과 같이 소문자 또는 대문자로 되어 있어야 한다. 파일의 텍스트는 유니코드 UTF-8로 인코딩되어 있다. 애플리케이션이나 다른 번들을 만들 때, 파일의 번들은 특정 위치에 저장된다. 애플리케이션이나 다른 번들을 만들 때, 파일은 번들의 특정 위치에 저장된다.

정보 프로퍼티 목록에 지정된 프로퍼티에는 표시 이름, 번들 식별자, 번들 아이콘, 번들 버전, 지원되는 플랫폼 및 문서 유형이 포함된다.

Xcode\(기본 개발 응용프로그램\)를 사용하여 애플리케이션 또는 다른 번들에 대한 프로젝트를 생성할 때 Xcode는 프로젝트의 리소스 폴더에 ProjectName-Info.plist라는 파일을 생성한다. \(프로젝트를 작성할 때 도구는 이 파일을 Info.plist로 번들에 복사한다.\) Xcode에서 ProjectName-Info.plist의 일부 프로퍼티를 구성하지만 추가 프로퍼티를 지정해야 하는 경우가 많다. 직접 파일을 선택하여 Xcode의 편집기의 정보 프로퍼티 목록을 편집할 수 있다. target inspector의 프로퍼티 창에서 일부 프로퍼티를 편집할 수도 있다.



#### Prerequisite Articles

[Property list](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/PropertyList.html#//apple_ref/doc/uid/TP40008195-CH44-SW1)  
[Bundle](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Bundle.html#//apple_ref/doc/uid/TP40008195-CH4-SW1)

**Related Articles**

\(None\)

**Definitive Discussion**

_Information Property List Key Reference_

