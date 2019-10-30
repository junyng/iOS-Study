# UITableViewController

테이블 뷰 관리를 전문으로하는 뷰 컨트롤러

## Declaration

```swift
class UITableViewController : UIViewController
```

## Overview

인터페이스가 테이블 뷰로 구성되고 다른 콘텐츠가 거의 또는 전혀 없는 경우 `UITableViewController` 를 Subclass하라. 테이블 뷰 컨트롤러에서 테이블 뷰의 컨텐츠를 관리하고 변경 사항에 대응하는데 필요한 프로토콜을 이미 채택한다. 또한 `UITableViewController` 는 다음과 같은 동작을 구현한다.

* 스토리 보드나 nib 파일에 보관된 테이블 뷰를 자동으로 로드한다. 테이블 뷰를 `tableView` 프로퍼티로 접근하라.
* 데이터 소스와 테이블 뷰의 delegate를 자동으로 설정한다.
* `viewWillAppear(_:)` 메서드를 구현하고 첫 번째 화면에서는 테이블 뷰의 데이터를 자동으로 로드한다. 테이블 뷰가 표시될 때마다\(요청에 따라 애니메이션을 포함하거나 포함하지 않음\) 선택 항목을 지우고, `clearsSelectionOnViewWillAppear` 프로퍼티를 변경하여 이 동작을 비활성화 할 수 있다.
* `viewDidAppear(_:)` 메서드를 구현하고 처음 나타날 때 테이블 뷰의 스크롤 인디케이터를 자동으로 점멸한다.
* `setEditting(_:animated:)` 메서드를 구현하고 사용자가 네비게이션 바의 Edit\|Done 버튼을 탭할 때 테이블의 편집 모드를 자동으로 전환한다.
* 화면 키보드의 나타남과 사라짐에 맞게 테이블 뷰의 크기를 자동으로 조정한다.

관리할 각 테이블 뷰에 대해 `UITableViewController` 의 사용자 정의 하위 클래스를 생성하라. 테이블 뷰 컨트롤러를 초기화 할 때, 테이블 뷰의 스타일을 지정해야 한다.\(plain 또는 grouped\). 또한 테이블을 데이터로 채우는 데 필요한 데이터 소스와 델리게이트 메서드를 재정의해야 한다. `loadView()` 또는 다른 슈퍼클래스 메서드를 재정의 할 수 있다. 하지만 만약 그렇다면, 보통 첫 번째 메서드 호출처럼 슈퍼클래스 구현 메서드를 반드시 실행해야 한다.

