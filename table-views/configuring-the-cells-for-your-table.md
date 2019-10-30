# Configuring the Cells for Your Table

스토리보드에서 하나 이상의 프로토타입 셀을 정의하여 테이블 행의 모양과 내용을 지정하라.

## Overview

셀은 테이블의 행을 시각적으로 표현한다. 대부분의 테이블에서 한 두가지 유형의 셀만 제공한다. 가장 중요한 정보가 눈에 띄도록 셀을 설계하라. 셀의 뷰 및 뷰 구성을 신중하게 선택하여 수행하라.

설계시 셀의 모양을 스토리보드 파일에 지정한다. Xcode는 각 테이블마다 하나의 프로토타입 셀을 제공하며, 필요에 따라 더 많은 프로토타입 셀을 추가할 수 있다. 프로토타입 셀은 셀의 모양을 본뜬 역할을 한다. 이는 표시하려는 뷰와 셀의 내용 영역 내에 나열된 뷰를 포함한다. 런타임에 테이블의 데이터 소스 객체는 프로토타입에서 실제 셀을 생성하여 앱의 데이터로 구성한다.

![prototype\_cells](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/prototype_cells.png)

셀 모양 설계 방법에 대한 팁은 Human Interface Guidelines: Tables 를 참조하라.

## Assign a Reuse Identifier to Each Cell

재사용 식별자를 사용하면 테이블의 셀을 생성하고 재활용할 수 있다. 재사용 식별자는 테이블의 각 프로토타입 셀에 할당하는 문자열이다. 스토리보드에서 프로토타입 셀을 선택하고 식별자 속성에 비어 있지 않은 값을 할당하라. 테이블 뷰의 각 셀에는 고유한 재사용 식별자가 있어야 한다.

런타임에 셀 객체가 필요한 경우 테이블 뷰의 `dequeueReusableCell(with:Identifier:for:)` 메서드를 호출하여 원하는 셀의 재사용 식별자를 전달하라. 대기열에 요청된 유형의 셀이 포함된 경우 테이블 뷰는 해당 셀을 반환한다. 그렇지 않다면, 그것은 스토리보드의 프로토타입 셀을 사용하여 새로운 셀을 만든다. 셀을 재사용하면 스크롤하는 경우와 같이 중요한 시간에 메모리 할당을 최소화하여 성능을 향상시킨다.

```swift
var cell = tableView.dequeueReusableCell(withIdentifier: “myCellType”, for: indexPath)
```

## Configure a Cell with a Built-In Style

셀을 구성하는 가장 간단한 방법은 `UITableViewCell` 에서 제공하는 내장 스타일 중 하나를 사용하는 것이다. 이러한 스타일을 그대로 사용하므로 셀을 관리하기 위해 사용자 정의 하위 클래스를 제공할 필요가 없다. 각 스타일은 셀의 내용 영역 내에 있는 라벨의 위치를 결정하는 스타일과 함께 한 개 또는 두 개의 라벨을 포함한다. 대부분의 스타일은 또한 셀 내용의 앞쪽 가장자리에 있는 이미지를 포함한다.

표준 스타일 중 하나로 프로토타입 셀을 구성하려면 스토리보드에서 셀을 선택하고 셀의 스타일 속성을 사용자 정의 이외의 값으로 설정하라.

![tableview\_configure\_cell](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/tableview_configure_cell.png)

`tableView(_:cellForRowAt:)` 메서드에서 `UITableViewCell` 의 `textLabel`, `detailTextLabel` 및 `imageView` 프로퍼티를 사용하여 셀 내용을 구성하라. 이러한 프로퍼티는 뷰를 포함하지만, 셀 객체는 스타일이 해당 내용은 지원하는 경우에만 뷰를 할당한다. 예를 들어, 기본 셀 스타일은 상세 문자열을 지원하지 않으므로 `detailTextLabel` 프로퍼티는 해당 스타일에 대해 0이 된다. 다음 예제 코드는 기본 셀 스타일을 사용하는 셀을 구성하는 방법을 보여준다.

```swift
override func tableView(_ tableView: UITableView, 
             cellForRowAt indexPath: IndexPath) -> UITableViewCell {
   // Reuse or create a cell. 
   let cell = tableView.dequeueReusableCell(withIdentifier: "basicStyle", for: indexPath)

   // For a standard cell, use the UITableViewCell properties.
   cell.textLabel!.text = "Title text"
   cell.imageView!.image = UIImage(named: "bunny")
   return cell
}
```

## Configure a Cell with Custom Views

표준 스타일 이외의 모양은 사용자 정의 셀 스타일을 사용하라. 사용자 지정 셀을 사용하여 셀에서 원하는 뷰, 구성, 크기, 위치를 지정하라. 라벨과 이미지 같은 정적인 뷰는 셀에 가장 적합한 콘텐츠를 만든다. 컨트롤과 같은 사용자 상호작용이 필요한 뷰는 피하라. 셀에 스크롤 뷰, 테이블 뷰, 콜렉션 뷰 또는 기타 복잡한 컨테이너 뷰를 포함하지 말아야 한다. 셀에 스택 뷰를 포함할 수 있지만 스택 뷰의 항목 수를 최소화하여 성능을 향상시킬 수 있다.

사용자 정의 셀을 구성하려면 테이블의 프로토타입 셀로 뷰를 끌어다 놓아라. 다음 그림은 뷰에 대한 사용자 정의 레이아웃과 형식을 갖춘 셀을 보여준다. 셀의 내용 영역 내에 위치시키기 위해 제약을 사용한다. 제약 조건을 설정할 때 "Constrain to margins" 옵션을 사용하여 셀의 내용 영역 간의 간격을 유지하라.

![tableview\_configure\_cell\_with\_custom\_views](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/tableview_configure_cell_with_custom_views.png)

사용자 지정 셀의 경우, 셀의 뷰에 접근하기 위해 `UITableViewCell` 하위 클래스를 정의해야 한다. 하위 클래스에 아울렛을 추가하고 해당 아울렛을 프로토타입 셀의 해당 뷰에 연결하라.

```swift
class FoodCell: UITableViewCell {
    @IBOutlet var name : UILabel?
    @IBOutlet var plantDescription : UILabel?
    @IBOutlet var picture : UIImageView?
}
```

`tableView(_:cellForRowAt:)` 데이터 소스 메서드에서 셀의 아울렛 값을 사용해 모든 뷰에 할당하라.

```swift
override func tableView(_ tableView: UITableView, 
             cellForRowAt indexPath: IndexPath) -> UITableViewCell {

   // Reuse or create a cell of the appropriate type.
   let cell = tableView.dequeueReusableCell(withIdentifier: "foodCellType", 
                         for: indexPath) as! FoodCell

   // Fetch the data for the row.
   let theFood = foods[indexPath.row]

   // Configure the cell’s contents with data from the fetched object.
   cell.name?.text = theFood.name
   cell.plantDescription?.text = theFood.description
   cell.picture?.image = theFood.picture

   return cell
}
```

## Change the Height of Rows

테이블 뷰는 행의 높이를 나타내는 셀과 별도로 추적한다. `UITableView` 는 행의 기본 크기를 제공하지만 테이블 뷰의 행 높이 속성에 사용자 지정 값을 할당하여 기본 높이를 재정의할 수 있다. 모든 행 높이가 동일한 경우 항상 이 속성을 사용하라. 이렇게 하는 것이 delegate 객체에서 높이 값을 반환하는 것 보다 더 효율적이다.

행 높이가 모두 동일하지 않거나 동적으로 변경할 수 있는 경우, delegate 객체의 `tableView(_:heightForRowAt:)` 메서드를 사용하여 높이를 지정하라. 이 메서드를 구현할 때 테이블의 모든 행에 대한 값을 제공해야 한다. 다음 예제 코드는 각 섹션의 첫 번째 행에 대해 사용자 정의 높이를 반환하고 다른 모든 행에 대해 기본 높이를 사용하는 방법을 보여준다.

```swift
override func tableView(_ tableView: UITableView, 
           heightForRowAt indexPath: IndexPath) -> CGFloat {
   // Make the first row larger to accommodate a custom cell.
  if indexPath.row == 0 {
      return 80
   }

   // Use the default size for all other rows.
   return UITableView.automaticDimension
}
```

테이블 뷰는 보이는 행의 높이만 요청한다. 사용자가 스크롤할 때 테이블 뷰는 화면 밖으로 이동한 다음 다시 화면으로 이동하는 경우를 포함하여 나타나는 각 행에 대한 높이를 제공하도록 요청한다.

## Restore Your Cell's Original Appearance Before Reuse

셀이 화면 밖으로 이동하면 테이블 뷰는 셀의 뷰 계층에서 해당 셀을 제거하고 내부적으로 관리되는 재활용 큐에 배치된다. 테이블 뷰의 `dequeReusbleCell(withIdentifier:for:)` 메서드를 호출하여 새로운 셀을 요청할때, 테이블 뷰는 먼저 재활용 큐에서 셀을 반환한다. 큐가 비어 있으면, 테이블 뷰가 스토리보드에서 새로운 셀을 인스턴스화한다.

사용자 지정 셀의 뷰 모양을 변경하는 경우, 셀 서브클래스의 `prepareForReuse()` 메서드를 실행하라. 구현에서 셀의 모양을 원래의 상태로 되돌려라. 예를 들어, 셀에서 뷰의 알파 속성을 변경하는 경우 해당 속성을 원래 값으로 되돌려라. 셀을 표시하도록 구성할 때 레이블 텍스트를 지우거나 이미지를 nil으로 설정하거나 `tableView(_:cellForRowAt:)` 메서드를 사용하여 수정할 필요가 없다.

## Add an Accessory View to Your Cell

accessory 뷰는 옵셔널이고 셀의 후행 가장자리에 나타나는 시스템 정의 뷰이다. accessory 뷰를 사용하여 사용자에게 표준 셀 동작을 전달한다. 예를 들어, 행을 누르면 행에 대한 자세한 정보가 표시된다는 것을 사용자에게 알리기 위해 상세 버튼을 추가한다.

accessory 뷰를 구성하기

* 스토리보드에서 셀의 accessory 속성을 사용하여 원하는 accessory 뷰를 선택하라.
* 코드에서 셀 `accessoryType` 프로퍼티 값을 변경하라.

사용자는 터치할 때 accessory 뷰가 특정한 동작을 할 것으로 예상한다. 이러한 동작을 구현하는 방법에 대한 자세한 내용은 `UITableViewCell.AccessoryType` 을 참조하라.

