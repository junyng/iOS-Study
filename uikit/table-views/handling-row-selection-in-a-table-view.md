# Handling Row Selection in a Table View

사용자가 테이블 뷰 셀을 탭하는 시간을 감지하여 앱이 다음에 표시되는 작업을 수행할 수 있도록 하라.

## Overview

사용자가 테이블 뷰의 행을 탭할 때, 다른 뷰가 제자리로 슬라이드되거나 체크 표시가 있는 선택된 상태를 나타내는 행과 같은 종류의 동작이 주로 뒤따른다. 앱이 작업을 수행하려면 사용자가 행을 탭하는 시간을 알고 그에 따라 응답해야 한다.

## Configure Row Selection Behavior

테이블 뷰에서 행 선택 동작을 구성하기 위해 다음 프로퍼티를 사용하라

`allowsSelection`

테이블이 편집 모드가 아닐 경우 사용자가 행을 선택할 수 있는지 여부를 확인한다. 기본값은 `true`이다.

`allowsMultipleSelection`

테이블 뷰가 편집 모드가 아닐 경우 사용자가 하나 이상의 행을 선택할 수 있는지 여부를 확인한다. 기본값은 `false`이다.

`allowsSelectionDuringEditing`

테이블 뷰가 편집 모드에 있는 동안 사용자가 행을 선택할 수 있는지 여부를 결정한다. 기본값은 `false` 이다.

`allowsMultipleSelectionDuringEditing`

테이블 뷰가 편집 모드에 있는 동안 사용자가 하나 이상의 행을 선택할 수 있는지 여부를 결정한다. 기본값은 `false` 이다.

## Respond to Row Selections

사용자가 행을 탭할 때, 테이블 뷰는 delegate 메서드 `tableView(_:didSelectRowAt:)` 를 호출한다. 이 때 앱은 선택된 항목의 세부사항을 표시하는 등의 동작을 수행한다.

```swift
func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    let selectedTrail = trails[indexPath.row]

    if let viewController = storyboard?.instantiateViewController(identifier: "TrailViewController") as? TrailViewController {
        viewController.trail = selectedTrail
        navigationController?.pushViewController(viewController, animated: true)
    }
}
```

새로운 뷰 컨트롤러를 네비게이션 스택에 push하여 셀 선택에 응답하는 경우, 뷰 컨트롤러가 스택에서 pop 될 때 셀의 선택을 취소하라. `UITableViewController` 를 사용하여 테이블 뷰를 표시하는 경우, `clearsSelectionOnViewWillAppear` 프로퍼티를 `true`로 설정함으로써 행동을 취할 수 있다. 그렇지 않으면 뷰 컨트롤러의 `viewWillAppear(_:)` 메서드에서 선택 항목을 clear 할 수 있다.

```swift
override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    if let selectedIndexPath = tableView.indexPathForSelectedRow {
        tableView.deselectRow(at: selectedIndexPath, animated: animated)
    }
}
```

행에 detail 버튼 accessory 뷰가 표시되고 사용자가 이를 탭하는 경우 테이블 뷰는 "did select row" 메서드 대신에 `tableView(_:accessoryButtonTappedForRowWith:)` 메서드를 호출한다. accessory 뷰에 대한 자세한 내용은 Configuring the Cells for Your Table을 참조 하라.

## Select a Row Programmatically

가끔 행의 선택은 테이블 뷰의 탭이 아닌 앱 자체 내에서 발생한다. 예를 들어, 사용자가 주소록에 새로운 사람을 등록하고 연락처 목록을 리턴한다. 앱이 이 목록을 최근에 추가된 사람에게 스크롤한다. 이 상황에서 `selectRow(at:animated:scrollPosition:)` 메서드를 사용할 수 있다. 또한 행이 이미 선택되어 있다면 `scrollToNearestSelectedRow(at:animated:)` 메서드를 사용할 수 있다. 선택 없이 특정 행으로 스크롤하는 것을 원한다면 `scrollToRow(at:at:animated:)` 를 호출하라.

> **Note**
>
> 행을 누르는 것과 마찬가지로, 프로그래밍 방식으로 행을 선택하면 `tableView(_:didSelectRowAt:)` delegate 메서드를 호출한다.

## Manage Selection Lists

선택한 항목과 별도 목록을 유지하기 위해 셀 선택을 사용할 수 있다. 포함 목록은 하나 이상의 선택된 항목을 가질 수 있는 반면 독점 목록은 기껏해야 하나를 가진다.

앱에서 선택 목록을 제공할 때, 항목의 상태를 가리키는 셀의 선택 상태를 사용하지 않아야 한다. 대신에 체크마크 또는 accessory 뷰를 표시하라. 체크마크를 사용하여 상태를 표시하려면 delegate 메서드 `tableView(_:didSelectRowAt:)` 메서드를 실행한 다음 `deselectRow(at:animated:)` 메서드를 호출해 셀의 선택을 취소한다. 이어서 셀의 `accessoryType` 프로퍼티를 `UITableViewCell.AccessoryType.checkmark` 로 설정하라.

예를들어, 배낭여행자를 위한 앱 사용자가 패킹 리스트를 만들 수 있다. 여기에는 침낭이나 텐트와 같은 캠핑 장비 목록이 포함된다. 사용자가 해당 기어를 포장했음을 표시하기 위해 항목을 탭하면, 앱은 행을 선택 해제하고 항목의 데이터 모델을 업데이트하며 포장된 품목에 대한 체크마크를 행에 표시한다. 항목을 다시 누르면 항목이 포장되지 않은 것으로 표시되고 체크마크가 제거된다. 예제는 다음과 같다.

```swift
func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    // Unselect the row, and instead, show the state with a checkmark.
    tableView.deselectRow(at: indexPath, animated: false)

    guard let cell = tableView.cellForRow(at: indexPath) else { return }

    // Update the selected item to indicate whether the user packed it or not.
    let item = packingList[indexPath.row]
    let newItem = PackingItem(name: item.name, isPacked: !item.isPacked)
    packingList.remove(at: indexPath.row)
    packingList.insert(newItem, at: indexPath.row)

    // Show a check mark next to packed items.
    if newItem.isPacked {
        cell.accessoryType = .checkmark
    } else {
        cell.accessoryType = .none
    }
}
```

독점 목록을 관리하는것은 유사하다. 선택한 상태를 가리키기 위해 행을 선택을 취소하고 체크마크 또는 accessory 뷰를 표시한다. 하지만 포함 목록과는 달리 독점 목록을 한 번에 하나의 선택된 항목으로만 제한하라.

예를 들어, 배낭여행자 앱은 사용자가 쉽게, 보통 그리고 어려운 단계 중 단 하나의 레벨에 따라 등산로를 필터링할 수 있도록 할 수 있다. 이와 같은 독점 목록을 사용하여 앱은 이전 선택 항목에서 체크마크를 제거하고 현재 선택 항목에 대한 체크마크를 표시해야 한다. 앱은 또한 어느 항목이 현재 선택되었는지 기억해야 한다. 예제는 다음과 같다.

```swift
func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    // Unselect the row.
    tableView.deselectRow(at: indexPath, animated: false)

    // Did the user tap on a selected filter item? If so, do nothing.
    let selectedFilterRow = selectedFilters[indexPath.section]
    if selectedFilterRow == indexPath.row {
        return
    }

    // Remove the checkmark from the previously selected filter item.
    if let previousCell = tableView.cellForRow(at: IndexPath(row: selectedFilterRow, section: indexPath.section)) {
        previousCell.accessoryType = .none
    }

    // Mark the newly selected filter item with a checkmark.
    if let cell = tableView.cellForRow(at: indexPath) {
        cell.accessoryType = .checkmark
    }

    // Remember this selected filter item.
    selectedFilters[indexPath.section] = indexPath.row
}
```

