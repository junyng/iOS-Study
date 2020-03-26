---
description: '테이블 뷰의 행과 섹션을 삽입, 삭제 또는 선택하는 일련의 메서드 호출을 시작한다.'
---

# beginUpdates\(\)

### Declaration

```swift
func beginUpdates()
```

### Discussion

가능하면 이 메서드 대신 [`performBatchUpdates(_:completion:)`](https://developer.apple.com/documentation/uikit/uitableview/2887515-performbatchupdates) 메서드를 사용하라.

후속 삽입, 삭제 및 선택 작업 \(예를 들어, [`cellForRow(at:)`](https://developer.apple.com/documentation/uikit/uitableview/1614983-cellforrow) 및 [`indexPathsForVisibleRows`](https://developer.apple.com/documentation/uikit/uitableview/1614885-indexpathsforvisiblerows)\)을 동시에 애니메이션화하려면 이 메서드를 호출하라. 또한 셀을 다시 로드하지 않고 행 높이의 변화를 애니메이션화하기 위해 [`endUpdates()`](https://developer.apple.com/documentation/uikit/uitableview/1614890-endupdates) 메서드에 이어 이 메서드를 사용할 수 있다. 이 메서드 그룹은 [`endUpdates()`](https://developer.apple.com/documentation/uikit/uitableview/1614890-endupdates)의 호출로 마무리해야 한다. 이러한 메서드 쌍은 중첩될 수 있다. 이 블록 내에서 삽입, 삭제 및 선택 호출을 수행하지 않으면 행 수와 같은 테이블 특성이 유효하지 않을 수 있다. 그룹 내에서 [`reloadData()`](https://developer.apple.com/documentation/uikit/uitableview/1614862-reloaddata)를 호출해서는 안되며, 그룹 내에서 이 메서드를 호출할 경우 애니메이션을 직접 수행해야 한다.

### See Also

#### Performing Batch Updates to Rows and Sections

* [`func performBatchUpdates((() -> Void)?, completion: ((Bool) -> Void)?)`](https://developer.apple.com/documentation/uikit/uitableview/2887515-performbatchupdates)  여러 개의 삽입, 삭제, 리로드 작업을 그룹으로 애니메이션화 한다.
* [`func endUpdates()`](https://developer.apple.com/documentation/uikit/uitableview/1614890-endupdates)  테이블 뷰의 로우와 섹션을 삽입, 삭제, 선택 또는 리로드하는 일련의 메서드 호출을 완료한다.

