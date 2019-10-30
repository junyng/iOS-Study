# UITableViewHeaderFooterView

해당 섹션에 대한 추가 정보를 표시하기 위해 테이블 섹션의 맨 위 또는 맨 아래에 배치하는 재사용 가능한 뷰

## Declaration

```swift
class UITableViewHeaderFooterView : UIView
```

## Overview

`UITableViewHeaderFooterView` 객체를 통해 테이블 섹션의 헤더 및 푸터 컨텐츠를 효율적으로 관리하라. 헤더-푸터 뷰는 서브클래스 또는 그대로 사용할 수 있는 재사용 가능한 뷰이다. 표준 모양의 헤더나 푸터를 만들어내기 위해 `textLabel` 프로퍼티에 나타낼 텍스트를 구성하라. \(만약 테이블 뷰에 그룹 스타일을 구성하였다면 `detailTextLabel` 프로퍼티 또한 구성할 수 있다.\) 좀 더 사용자 정의 모양을 생성하려면 `contentView` 프로퍼티에 뷰로 subviews를 추가한다. 또한 선택적으로 배경 뷰를 `backgroundView` 프로퍼티를에 할당할 수 있다.

헤더-푸터 뷰를 재사용하는 것을 장려하기 위해서 테이블 뷰 메서드 `register(_:forHeaderFooterViewReuseIdentifier:)` 또는 `register(_:forHeaderFooterViewReuseIdentifier:)` 메서드를 호출 및 등록하라. 이 메서드는 재활용 뷰\(가능한 경우\)를 반환하거나 등록했던 정보를 사용하여 새로운 뷰를 생성한다. delegate 객체 `tableView(_:viewForHeaderInSection:)` 또는 `tableView(_:viewForFooterInSection:)` 메서드에서 뷰를 생성하기 위해 테이블 뷰의 `dequeueReusableHeaderFooterView(withIdentifier:)` 메서드를 호출한다. 이 메서드는 재활용 뷰\(가능한 경우\)를 반환하거나 등록했던 정보를 사용하여 새로운 뷰를 생성한다.

사용자 정의 헤더-푸터 뷰를 생성하는 단순한 대안으로 데이터 소스 객체의 `tableView(_:titleForHeaderInSection:)` 와 `tableView(_:titleForFooter)` 를 구현하는 것이다. 이러한 메서드를 구현하면 테이블 뷰는 표준 헤더나 푸터 뷰를 생성하고 제공하는 텍스트를 표시한다.

