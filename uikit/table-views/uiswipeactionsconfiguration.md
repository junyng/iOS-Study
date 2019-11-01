# UISwipeActionsConfiguration

테이블의 행을 스와이프하면서 수행할 액션 집합

## Declaration

```swift
class UISwipeActionsConfiguration : NSObject
```

## Overview

`UISwipeActionsConfiguration` 객체를 생성해 사용자 정의 스와이프 액션을 테이블 뷰의 행과 연관지어라. 사용자는 행과 관련된 액션을 표시하기 위해 테이블 뷰에서 왼쪽 또는 오른쪽으로 수평으로 스와이프한다. 각 swipe-actions 객체에는 각 유형의 스와이프에 대해 표시할 작업 집합이 포함되어 있다.

테이블 뷰의 행에 커스텀 액션을 추가하기 위해 테이블 뷰 delegate 메서드`tableView(_:leadingSwipeActionsConfigurationForRowAt:)` 또는 `tableView(_:trailingSwipeActionsConfigurationForRowAt:)` 를 구현하라. 이 메서드에서 지정된 행에 대한 액션을 생성하고 반환하라. 테이블에는 액션 버튼이 표시되며 사용자가 액션 버튼을 누를 때 적절한 핸들러 블록이 실행된다.

