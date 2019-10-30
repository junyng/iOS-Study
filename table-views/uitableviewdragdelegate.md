# UITableViewDragDelegate

테이블 뷰에서 드래그를 시작하기 위한 인터페이스

## Declaration

```swift
protocol UITableViewDragDelegate
```

## Overview

테이블 뷰에서 드래그를 시작하는데 사용하는 객체에서 이 프로토콜을 구현하라. 이 프로토콜의 유일한 메서드는 `tableView(_:itemsForBeginning:at:)` 이다. 그러나 테이블 뷰의 드래그 동작을 사용자 정의하는데 필요한 다른 메서드를 구현할 수 있다.

테이블 뷰의 `DragDelegate` 프로퍼티에 사용자 정의 델리게이트 객체를 할당하라.

