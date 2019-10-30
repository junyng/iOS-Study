# UITableViewDropCoordinator

테이블 뷰를 사용해 사용자 정의 드랍 관련 작업을 조정하는 인터페이스

## Declaration

```swift
protocol UITableViewDropCoordinator
```

## Overview

직접 이 클래스의 인스턴스를 만들지 않는다. 테이블 뷰에 드랍이 발생하면 `UIKit` 은 이 클래스의 인스턴스를 생성하고 `tableView(_:performDropWith:)` 메서드에 전달한다. 객체를 사용하여 드랍된 항목이 원하는 위치에 애니메이션것을 테이블 뷰가 알 수 있도록 하라.

