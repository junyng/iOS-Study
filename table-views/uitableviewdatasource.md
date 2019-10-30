# UITableViewDataSource

데이터를 관리하고 테이블 뷰를 위한 셀을 제공하는데 사용하는 객체에서 채택한 메소드

## Declaration

```swift
protocol UITalbeViewDataSource
```

## Overview

테이블 뷰는 데이터의 표시만 관리하며, 데이터 자체는 관리하지 않는다. 데이터를 관리하려면 데이터 소스 객체, 즉 `UITableViewDataSource` 프로토콜을 구현하는 객체를 제공하라. 데이터 소스 객체는 테이블의 데이터 관련 요청에 응답한다. 또한 테이블의 데이터를 직접 관리하거나 앱의 다른 부분과 조정하여 해당 데이터를 관리한다. 데이터 소스 객체의 기타 책임이다.

* 테이블의 섹션과 행의 갯수를 보고한다.
* 테이블의 각 행에 셀을 제공한다.
* 섹션 헤더 및 푸터에 제목을 제공한다.
* 테이블의 인덱스 구성\(있는 경우\)
* 기본 데이터를 변경해야 하는 사용자 또는 테이블의 시작 업데이트에 응답한다.

이 프로토콜의 두 가지 메서드만 필요하며, 다음 예시 코드에 제시되어 있다.

```swift
// Return the number of rows for the table.     
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
   return 0
}

// Provide a cell object for each row.
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
   // Fetch a cell of the appropriate type.
   let cell = tableView.dequeueReusableCell(withIdentifier: "cellTypeIdentifier", for: indexPath)

   // Configure the cell’s contents.
   cell.textLabel!.text = "Cell text"

   return cell
}
```

이 프로토콜의 다른 메서드를 사용하여 테이블에 대한 특정 기능을 활성화하라. 예를 들어 행에 대해 swipe-to-delete 기능을 활성화하려면 `tableView(_:commit:forRowAt:)` 메서드를 구현해야 한다.

데이터 소스 객체를 사용하여 테이블 셀을 만들고 구성하는 메서드에 대한 자세한 내용은 Filling a Table with Data 를 참고하라.

## Specifying the Location of Rows and Sections

테이블 뷰는 `NSIndexPath` 객체의 로우 및 섹션 프로퍼티를 사용하여 셀 위치를 사용자에게 전달한다. 로우와 섹션 인덱스는 0을 기반으로 하기 때문에 첫 번째 구간은 인덱스 0, 두 번째 구간은 인덱스 1 등에 있다. 마찬가지로, 각 섹션의 첫 번째 로우는 인덱스 0에 있으며, 이는 열을 고유하게 식별하기 위해 섹션과 행 값을 모두 필요로 한다는 것을 의미한다. 테이블에 섹션이 없는 경우 행 값만 있으면 된다.

![tableview\_rows\_and\_sections](../.gitbook/assets/tableview_rows_and_sections.png)

