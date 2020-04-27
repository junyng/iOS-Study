---
description: '테이블 뷰의 행과 섹션을 삽입, 삭제, 선택 또는 리로드하는 일련의 메서드 호출을 포함한다.'
---

# endUpdates\(\)

### Declaration

```swift
func endUpdates()
```

### Discussion

가능하면 이 메서드 대신 [`performBatchUpdates(_:completion:)`](https://developer.apple.com/documentation/uikit/uitableview/2887515-performbatchupdates) 메서드를 사용하라.

[`beginUpdates()`](https://developer.apple.com/documentation/uikit/uitableview/1614908-beginupdates) 에서 시작하여 이 메서드를 호출해 작업으로 삽입, 삭제, 선택 및 로우와 섹션을 리로드하는 작업으로 구성된 일련의 메서드 호출을 브래킷화하라.

`endUpdates`를 호출할 때 `UITableView`는 작업을 동시에 애니메이션화한다. [`beginUpdates()`](https://developer.apple.com/documentation/uikit/uitableview/1614908-beginupdates) 및 `endUpdates` 호출은 중첩될 수 있다. 이 블록 내에서 삽입, 삭제, 선택 호출을 하지 않으면 로우 갯수와 같은 테이블 속성이 무효가 될 수 있다.

### See Also

#### Performing Batch Updates to Rows and Sections

* [`func performBatchUpdates((() -> Void)?, completion: ((Bool) -> Void)?)`](https://developer.apple.com/documentation/uikit/uitableview/2887515-performbatchupdates)  여러 개의 삽입, 삭제, 리로드 작업을 그룹으로 애니메이션화 한다.
* [`func endUpdates()`](https://developer.apple.com/documentation/uikit/uitableview/1614890-endupdates)  테이블 뷰의 로우와 섹션을 삽입, 삭제, 선택 또는 리로드하는 일련의 메서드 호출을 완료한다.

