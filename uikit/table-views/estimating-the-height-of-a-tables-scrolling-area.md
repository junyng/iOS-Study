# Estimating the Height of a Table's Scrolling Area

스크롤이 컨텐츠의 크기를 정확하게 반영할 수 있도록 테이블 뷰의 헤더, 푸터 및 행에 대한 높이 추정치를 제공하라.

## Overview

가능하면 테이블 뷰는 셀, 헤더 및 푸터의 높이 추정치를 사용하여 성능 및 스크롤 동작을 개선한다. 테이블 뷰가 화면에 나타나기 전에, 스크롤 관련 파라미터를 구성하기 위해 해당 정보가 필요하기 때문에, 테이블 뷰의 높이를 계산해야 한다. 항목에 대해 추정된 높이를 제공하지 않는 경우, 테이블 뷰는 앞에 있는 항목의 실제 높이를 계산해야 하며, 이는 비용이 많이 들 수 있다.

> **Important**
>
> 테이블 뷰에 자체 크기 조정 셀, 헤더 또는 푸터가 포함된 경우 해당 항목에 대해 추정된 높이를 제공해야 한다.

테이블 뷰는 표준 헤더, 푸터 및 행 스타일에 기초한 테이블 뷰 항목의 기본 높이 추정치를 제공한다. 테이블의 항목이 기본값보다 훨씬 짧거나 큰 경우 테이블의 추정치에 값을 할당하여 사용자 지정 추정치를 제공할 수 있다. 테이블의 `estimatedRowHeight`, `estimatedSectionHeaderHeight`, `estimatedSectionFooterHeight` 프로퍼티이다. 개별 항목의 높이가 서로 다른 경우, 다음과 같은 delegate 객체의 메서드를 사용하여 사용자 정의 추정치를 제공하라.

* `tableView(_:estimatedHeightForRowAt:)`
* `tableView(_:estimatedHeightForHeaderInSection:)`
* `tableView(_:estimatedHeightForFooterInSection:)`

헤더, 푸터, 행의 높이를 추정할 때 정밀도보다 속도가 더 중요하다. 테이블 뷰는 테이블의 모든 항목에 대한 추정치를 요청하므로 delegate 메서드로 장기간 실행되는 작업을 수행하면 안된다. 대신 스크롤에 유용할 정도로 가까운 추정치를 생성한다. 테이블 뷰는 추정치를 화면에 나타나는 실제 항목 높이로 대체한다.

아래의 예제 코드는 높이가 다른 테이블 행의 추정 높이를 계산한다. 첫 번째 행의 셀은 항상 여러 행의 텍스트를 포함하는 사용자 지정 스타일을 사용한다. 다른 모든 행은 테이블 뷰에서 제공하는 기본 스타일을 사용한다.

```swift
let cellMarginSize :CGFloat  = 4.0
override func tableView(_ tableView: UITableView, 
         estimatedHeightForRowAt indexPath: IndexPath) -> CGFloat {
   // Choose an appropriate default cell size.
   var cellSize = UITableView.automaticDimension

   // The first cell is always a title cell. Other cells use the Basic style.
   if indexPath.row == 0 {
      //Title cells consist of one large title row and two body text rows.
      let largeTitleFont = UIFont.preferredFont(forTextStyle: .largeTitle)
      let bodyFont = UIFont.preferredFont(forTextStyle: .body)

      // Get the height of a single line of text in each font.
      let largeTitleHeight = largeTitleFont.lineHeight + largeTitleFont.leading
      let bodyHeight = bodyFont.lineHeight + bodyFont.leading

      // Sum the line heights plus top and bottom margins to get the final height.
      let titleCellSize = largeTitleHeight + (bodyHeight * 2.0) + (cellMarginSize * 2)

      // Update the estimated cell size.
      cellSize = titleCellSize
   }

   return cellSize
}
```

테이블 뷰가 높이 추정치를 사용할 때 스크롤 뷰에서 상속된 `contentOffset` 및 `contentSize` 프로퍼티를 적극적으로 관리한다. 이러한 프로퍼티를 직접 읽거나 수정하려고 하면 안된다. 이 값은 `UITableView` 에만 의미가 있다.

