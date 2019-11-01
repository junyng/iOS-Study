# UITableViewDropProposal

테이블 뷰에서 드랍 처리를 위한 제안하는 솔루션

## Declaration

```swift
class UITableViewDropProposal : UIDropProposal
```

## Overview

드랍 델리게이트 객체의 `tableView(_:dropSessionDidUpdate:withDestinationIndexPath:)` 메서드에서 클래스의 인스턴스를 생성하라. 테이블이 현재 지정된 위치에서 드랍을 처리하려는 방법을 알 수 있도록 drop proposal을 생성한다. 테이블 뷰는 사용자에게 적절한 시각적 피드백을 제공하기 위해 해당 정보를 사용한다.

