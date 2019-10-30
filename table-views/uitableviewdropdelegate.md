# UITableViewDropDelegate

테이블 뷰에서 드랍을 다루는 인터페이스

## Declaration

```swift
protocol UITableViewDropDelegate
```

## Overview

테이블 뷰에 드랍된 데이터를 통합하기 위해 객체에 이 프로토콜을 구현하라. 이 프로토콜에 필요한 유일한 메서드는 `tableView(_:performDropWith:)` 메서드이지만, 테이블 뷰의 드랍 동작을 사용자 정의하는데 필요한 다른 메서드를 구현할 수 있다.

테이블 뷰의 `dropDelegate` 프로퍼티에 사용자 정의 델리게이트 객체를 할당하라.

