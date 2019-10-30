# Table Views

커스텀 가능한 행의 단일 열에 데이터를 표시한다.

## Overview

테이블 뷰에는 행과 섹션으로 분할된 수직 스크롤 컨텐츠의 단일 열이 표시된다. 표의 각 행에는 앱과 관련된 정보가 하나씩 표시된다. 섹션을 통해 관련 행을 그룹화할 수 있다. 예를 들어, 연락처 앱은 테이블을 사용하여 사용자의 연락처 이름을 표시한다.

![table\_views](../.gitbook/assets/table_views.png)

테이블 뷰는 다음과 같은 여러 객체간의 공동 작업이다.

* Cells. 셀은 내용을 시각적으로 표현한다. UIKit에서 제공하는 기본 셀을 사용하거나 앱의 요구에 맞는 커스텀 셀을 정의할 수 있다.
* Table view controller. 일반적으로 테이블 뷰를 관리하려면 `UITableViewController` 객체를 사용하라. 다른 뷰 컨트롤러도 사용할 수 있지만 테이블 관련 기능이 작동하려면 테이블 뷰 컨트롤러가 필요하다.
* Data source object. 이 객체는 `UITableViewDataSource` 프로토콜을 채택하고 테이블에 대한 데이터를 제공한다.
* Delegate object. 이 객체는 `UITableViewDelegate` 프로토콜을 채택하고 테이블 내용과의 사용자 상호작용을 관리한다. 

## Topics

### Essentials

* class UITableView

  > 단일 열에 정렬된 행을 사용하여 데이터를 표시하는 뷰

### Data

* Filling a Table with Data

  > 데이터 소스 객체를 사용하여 테이블에 사용할 셀을 동적으로 생성 및 구성하거나 스토리보드에서 정적 방식으로 제공하라.

* protocol UITableViewDataSource

  > 데이터를 관리하고 테이블 뷰를 위한 셀을 제공하는데 사용되는 객체에서 채택되는 메서드

* protocol UITableViewDataSourcePrefetching

  > 테이블 뷰의 데이터 요구 사항에 대한 사전 경고를 제공하여 장기간 실행 중인 데이터 작업을 조기에 시작할 수 있는 프로토콜

* protocol UILocalizedIndexCollation

  > 섹션 인덱스가 있는 테이블 뷰의 데이터를 구성, 정렬 및 로컬화 하는 객체

* protocol UIDataSourceTranslating

  > 데이터 원본 객체 관리를 위한 고급 인터페이스

* class UIRefreshControl

  > 스크롤 뷰 내용을 새로 고치는 작업을 시작할 수 있는 표준 컨트롤

### Table Management

* Estimating the Height of a Table's Scrolling Area

  > 스크롤이 컨텐츠의 크기를 정확하게 반영할 수 있도록 테이블 뷰의 헤더, 푸터 및 행에 대한 높이 추정치를 제공하라.

* class UITableViewController

  > 테이블 뷰 관리를 담당하는 뷰 컨트롤러

* protocol UITableViewDelegate

  > 선택 관리, 헤더 및 푸터 구성, 셀 삭제 및 순서 변경, 테이블 뷰에서 기타 작업을 수행하는 메서드

* class UITableViewFocusUpdateContext

  > 특정 포커스 업데이트와 관련된 정보를 한 뷰에서 다른 뷰로 제공하는 컨텍스트 객체

### Cells, Headers, and Footers

* Configuring the Cells for Your Table

  > 스토리보드에서 하나 이상의 프로토타입 셀을 정의하여 테이블 행의 모양과 내용을 정의하라.

* Creating Self-Sizing Table View Cells

  > 동적 타입을 지원하는 테이블 뷰 셀을 만들고 시스템 간격 제약 사항을 사용하여 주변 텍스트 레이블의 간격을 조정하라.

* Adding Headers and Footers to Table Sections

  > 헤더 및 푸터 뷰를 테이블 뷰 섹션에 추가하여 행 그룹을 시각적으로 구분하라.

* class UITableViewCell

  > 테이블 뷰에서 단일 행의 시각적 표현

* class UITableViewHeaderFooterView

  > 해당 섹션에 대한 추가 정보를 표시하기 위해 테이블 섹션의 맨 위 또는 맨 아래에 배치하는 재사용 가능한 뷰

### Row Actions

* class UISwipeActionsConfiguration

  > 테이블 행을 스와이프할 때 수행할 액션 세트

* class UIContextualAction

  > 사용자가 테이블 행을 바꿀 때 표시되는 작업

### Selection Management

* Hanling Row Selection in a Table View

  > 사용자가 테이블 뷰 셀을 탭하는 시간을 감지하여 앱이 다음에 표시되는 작업을 수행할 수 있도록 하라.

* Selecting Multiple Items with a Two-Finger Pan Gesture

  > 테이블 및 컬렉션 뷰에서 다중 선택 제스처를 사용하여 여러 항목의 사용자 선택을 가속화하라.

### Drag and Drop

* Supporting Drag and Drop in Table Views

  > 테이블 뷰에서 드래그와 드랍을 다루는 것을 시작한다.

* protocol UITableViewDragDelegate

  > 테이블 뷰에서 드래그를 시작하기 위한 인터페이스

* protocol UITableViewDropDelegate

  > 테이블 뷰에서 드랍을 하기 위한 인터페이스

* protocol UITableViewDropCoordinator

  > 테이블 뷰를 사용하여 커스텀 드랍 관련 작업을 조정하는 인터페이스

* protocol UITableViewDropItem

  > 테이블 뷰로 삭제되는 항목과 관련된 데이터

* class UITableViewDropProposal

  > 테이블 뷰에서 드롭 처리를 위한 제안 솔루션

### Placeholder Cells

* protocol UITableViewDropPlaceholderContext

  > 드롭 작업 중에 테이블에 추가한 플레이스홀더 셀을 추적하기 위한 객체

* class UITableViewDropPlaceholder

  > 드롭 미리 보기 커스텀 매개 변수를 지원하는 플레이스홀더 셀

* class UITableViewPlaceholder

  > 테이블로 삽입되는 플레이스홀더 셀에 대한 정보를 포함하는 객체

## See Also

### Container Views

* Collection Views

  > 구성 가능하고 사용자 지정성이 높은 레이아웃을 사용하여 중첩된 뷰를 표시

* class UIStackView

  > 열 또는 행에 뷰 집합을 배치하기 위한 간소화된 인터페이스

* class UIScrollView

  > 포함된 뷰를 스크롤하고 확대할 수 있는 뷰

## 출처

### Apple documentation

[**UITableView**](https://developer.apple.com/documentation/uikit/uitableview)

### 부스트 코스

**\[3.기상정보 애플리케이션 - 1\) 테이블뷰란?\(Table View Programming Guide for iOS**\)\]\([https://www.edwith.org/boostcourse-ios/lecture/16860/\)\*\*](https://www.edwith.org/boostcourse-ios/lecture/16860/%29**)

