# UITableViewDropPlaceholder

플레이스홀더 셀은 드랍 미리보기 매개 변수를 사용자 정의하는 것을 지원한다.

## Declaration

```swift
class UITableViewDropPlaceholder : UITableViewPlaceholder
```

## Overview

플레이스홀더 셀을 테이블에 삽입하려면 `UITableViewDropPlaceholder` 객체를 생성하고 `UITableViewDropCoordinator` 의 `drop(_:to:)` 메서드에 전달하라. 셀 내용을 비동기식으로 로드하는 동안 플레이스홀더 셀을 사용하여 임시 인터페이스를 표시하라. 예를 들어, 플레이스홀더 셀은 진행 인디에키터 또는 셀 콘텐츠를 아직 사용할 수 없다는 메시지를 표시할 수 있다. 플레이스홀더 객체에는 테이블에 표시할 임시 셀 재사용 식별자가 포함되어 있다. 또한 드랍 도중 사용자 정의 미리보기를 포함할 수 있다.

사용하는 셀은 플레이스홀더에 등록해야 한다. 스토리보드 파일에서 테이블 뷰 셀 객체를 테이블에 추가하고 모양을 구성하고 해당 클래스를 `UITableViewCell`\(또는 적절한 서브 클래스\)로 설정하라. 그리고 재사용 식별자를 할당하라. `UITableViewDropPlaceholder` 객체를 생성할 때 셀의 재사용 식별자를 `init(insertionIndexPath:reuseIdentifier:rowHeight:)`에 전달하라. 테이블 뷰는 플레이스홀더 객체의 정보를 사용하여 셀을 테이블에 삽입한다.

> **Important**
>
> 플레이스홀더 셀은 테이블 뷰의 임시적인 부분으로 되어 있다. 항상 가능한 한 빨리 실제 셀로 교체하거나 드랍을 취소하여 테이블에서 제거하라. `UITableViewDropPlaceholderContext` 객체의 메서드를 사용해 테이블로부터 플레이스홀더를 제거하라.

