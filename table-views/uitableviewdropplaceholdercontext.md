# UITableViewDropPlaceholderContext

드랍 작업 중에 테이블에 추가한 placeholder 셀을 추적하기 위한 객체

## Declaration

```swift
protocol UITableViewDropPlaceholderContext
```

## Overview

이 클래스의 인스턴스를 직접 만들지 않는다. 대신 드랍 코디네이터 객체에서 `drop(_:to:)` 메서드를 호출한다. 이 메서드는 플레이스홀더 셀을 테이블에 삽입하고 해당 플레이스홀더를 관리하기 위해 `UITableViewDropPlaceholderContext` 객체를 반환한다. 실제 데이터와 플레이스홀더 셀을 교환할 준비가 된 경우, 콘텍스트 객체의 `commitInsertion(dataSourceUpdates:)` 메서드를 호출하라. 교체를 제공하지 않고 플레이스홀더 셀을 제거하려면 `deletePlaceholder()` 를 대신 호출하라.

