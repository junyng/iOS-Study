# Filling a Table with Data

데이터 소스 객체를 사용하여 테이블에 사용할 셀을 동적으로 생성 및 구성하거나 스토리보드에서 정적 방식으로 제공하라.

## Overview

테이블 뷰는 인터페이스의 데이터 기반 요소 입니다. 데이터 소스 객체를 사용하여 앱의 데이터와 해당 데이터의 각 부분을 화면에 렌더링하는데 필요한 뷰를 제공하라. 즉, 데이터 소스는 `UITableViewDataSource` 프로토콜을 채택하는 객체이다. 테이블 뷰는 화면에서 뷰를 정렬하고 데이터 소스 객체와 함께 작동하여 해당 데이터를 최신 상태로 유지한다.

테이블 뷰에서 데이터를 행과 섹션으로 편성한다. 행에 개별 데이터 항목이 표시되고 섹션이 관련 행을 함께 그룹화한다. 섹션은 필요하지 않지만, 이미 계층화된 데이터를 구성하는 좋은 방법이다. 예를 들어, 연락처 앱은 각 연락처의 이름을 일렬로 표시하고, 행을 그 사람의 성의 첫 글자를 기준으로 한 섹션으로 그룹화한다.

![contacts](../.gitbook/assets/contacts.png)

## Provide the Numbers of Rows and Sections

화면에 나타나기 전에 테이블 뷰는 행과 섹션의 총 개수를 지정하도록 요청한다. 데이터 소스 객체는 다음 두 가지 방법을 사용하여 이 정보를 제공한다.

```swift
func numberOfSections(in tableView: UITableView) -> Int  // Optional 
func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int
```

이러한 메서드를 구현할 때 행 및 섹션 수를 가능한 한 빨리 반환하라. 이렇게 하려면 행과 섹션 정보를 쉽게 검색할 수 있도록 데이터를 구성해야 할 수 있다. 예를 들어, 배열을 사용하여 테이블의 데이터를 관리하는 것을 고려하라. 배열이 테이블 뷰 자체와 일치하기 때문에 행과 섹션을 모두 관리하는데 좋은 도구이다.

아래 예제 코드는 다중 섹션 테이블에서 행 및 섹션 수를 반환하는 데이터 소스 메서드의 구현을 보여준다. 이 테이블에서는 각 행에 문자열이 표시되므로 구현에는 각 섹션에 대한 문자열이 저장된다. 섹션을 관리하기 위해 구현에서는 배열의 배열\(계층적 데이터\)를 사용한다. 섹션 수를 가져오기 위해 데이터 소스는 계층 데이터 배열의 항목 수를 반환한다. 특정 섹션의 행 수를 가져오기 위해 데이터 소스는 각 하위 배열의 항목 수를 반환한다.

```swift
var hierarchicalData = [[String]]() 

override func numberOfSections(in tableView: UITableView) -> Int {
   return hierarchicalData.count
}

override func tableView(_ tableView: UITableView, 
                        numberOfRowsInSection section: Int) -> Int {
   return hierarchicalData[section].count
}
```

### Define the Appearance of Rows

셀을 사용하여 스토리보드 파일에서 테이블 행의 모양을 정의하라. 셀은 테이블 행의 템플릿처럼 작동하는 `UITableViewCell` 객체다. 셀은 뷰이며 내용을 표시하는데 필요한 모든 서브 뷰를 포함할 수 있다. 레이블, 이미지 뷰 및 기타 뷰를 내용 영역에 추가하고 제약 조건을 사용하여 해당 뷰를 정렬할 수 있다.

앱 인터페이스에 테이블 뷰를 추가하면 구성할 수 있는 프로토타입 셀이 하나 포함되어 있다. 프로토타입 셀을 추가하려면 테이블 뷰를 선택하고 프로토타입 셀 특성을 업데이트하라. 각 셀은 모양을 정의하는 스타일을 가지고 있다. UIKit에서 제공하는 표준 스타일 중 하나를 선택하거나 사용자 정의 스타일을 정의할 수 있다.

다음 그림은 각각 표준 셀 스타일 중 하나를 사용하는 두 개의 프로토타입 셀이 있는 테이블을 보여준다.

![tableview\_prototype\_cell](../.gitbook/assets/tableview_prototype_cell.png)

스토리보드 파일에서 각 프로토타입 셀에 대해 다음 작업을 수행하라.

* 셀 스타일을 사용자 정의로 설정하거나 표준 셀 스타일 중 하나로 설정하라.
* 셀의 식별자 속성에 비어 있지 않은 문자열을 할당하라.
* 사용자 정의 셀의 경우 셀에 뷰 및 제약 조건을 추가하라.
* Identity inspector 에서 사용자 지정 셀의 클래스를 지정하라.

사용자 정의 뷰를 사용하여 셀을 생성할 때, `UITableViewCell` 하위 클래스를 정의하여 해당 뷰를 관리하라. 하위 클래스에서 앱의 데이터를 표시하는 사용자 정의 뷰에 대한 아울렛을 추가하고 해당 아울렛을 스토리보드 파일의 실제 뷰에 연결하라. 런타임에 셀을 구성하려면 아울렛이 필요하다.

셀 모양을 구성하는 방법에 대한 자세한 내용은 Configuring the Cells for Your Table을 참고하라.

## Create and Configure Cells for Each Row

테이블 뷰가 화면에 나타나기 전에, 데이터 소스 객체에게 테이블의 보이는 부분 또는 근처에 있는 행에 셀을 제공하도록 요청한다. 데이터 소스 객체 `tableView(_:cellForRowAt:)` 메서드가 빠르게 응답해야 한다. 다음 패턴으로 이 메서드를 구현하라.

1. `dequeueReusableCell(withIdentifier:for:)` 메서드를 호출하여 셀 객체를 받는다.
2. 앱의 사용자 정의 데이터로 셀 뷰를 구성하라.
3. 셀을 테이블 뷰로 리턴하라.

표준 셀 스타일의 경우 `UITableViewCell` 에는 구성해야하는 뷰가 포함된 속성이 포함되어 있다. 사용자 지정 셀의 경우 설계시 셀에 뷰를 추가하고 셀에 접근하기 위해 아울렛을 추가한다.

아래의 예제 코드는 단일 텍스트 라벨을 포함하는 셀을 구성하는 데이터 소스 메서드의 버전을 보여준다. 셀은 표준 셀 스타일 중 하나인 기본 스타일을 사용한다. 기본 스타일 셀의 경우 `UITableViewCell`의 `textLabel` 속성은 데이터로 구성하는 레이블 뷰를 포함한다.

```swift
override func tableView(_ tableView: UITableView,
                        cellForRowAt indexPath: IndexPath) -> UITableViewCell {
   // Ask for a cell of the appropriate type.
   let cell = tableView.dequeueReusableCell(withIdentifier: "basicStyleCell", for: indexPath)

   // Configure the cell’s contents with the row and section number.
   // The Basic cell style guarantees a label view is present in textLabel.
   cell.textLabel!.text = "Row \(indexPath.row)"
   return cell
}
```

테이블 뷰는 테이블의 각 행에 대해 셀을 생성하도록 요청하지 않는다. 대신, 테이블 뷰는 테이블의 보이는 부분이나 근처에 있는 셀에 대해서만 질문하면서 셀을 느슨하게 관리한다.

셀을 게으르게 만들면 테이블이 사용하는 메모리의 양을 줄일 수 있다. 그러나 이것은 또한 데이터 소스 객체가 빨리 셀을 생성해야 한다는 것을 의미한다. `tableView(_:cellForRowAt:)` 메서드를 사용하여 테이블 데이터를 로드하거나 장시간 작업을 수행하면 안된다.

> **Note**
>
> 표준 셀 스타일을 사용하는 것 외에도 원하는 뷰를 포함하는 사용자 지정 셀을 정의할 수 있다. 셀 구성에 대한 자세한 내용은 Configuring the Cells for Your Table을 참조하라.

## Prefetch Data to Improve Performance

테이블 뷰의 스크롤링 성능은 매우 중요하다. 테이블의 데이터를 가져오는데 비용이 많이 드는 작업\(예를 들어 데이터베이스에서 가져오기, 사전 설정 데이터 소스 객체를 사용하기\)을 하는 해당 객체는 `UITableViewDataSourcePrefetching` 프로토콜을 채택한다. 이는 뷰를 스크롤하기 전에 데이터를 비동기적으로 로드하기 시작한다.

사전 설정 데이터 소스를 구현하는 방법에 대한 자세한 내용은 `UITableViewDataSourcePrefetching`을 참조 하라.

## Specify Data Statically in the Storyboard

정적 테이블을 사용하여 프로토타입 제작 중 또는 테이블 내용이 변경되지 않을때 시간을 절약하라. 정적 테이블을 사용하여 스토리보드 파일의 맨 앞에 테이블의 모든 데이터를 지정하며, 데이터 소스 객체를 구현하지 말아야 한다. 런타임에 UIKit은 스토리보드에서 해당 데이터를 로드하고 관리한다. 런타임에 정적 테이블의 데이터를 변경할 수 없으므로 전송 앱에서 데이터를 조금씩 사용하라.

스토리보드 파일에서의 정적 테이블 구성

1. `UITableViewController` 객체를 스토리보드에 추가하라.
2. 테이블 뷰 컨트롤러의 테이블 뷰를 선택하라.
3. 테이블 뷰의 내용 속성\(Attribute inspector\)의 특성을 정적 셀로 변경하라.
4. 테이블 뷰의 섹션 특성을 사용하여 테이블의 섹션 수를 지정하라.
5. 각 섹션의 행 특성을 원하는 행 수로 설정하라.
6. 원하는 뷰 및 콘텐츠로 각 셀을 구성하라.

> **Important**
>
> 정적 데이터가 있는 테이블 뷰에서는 해당 데이터를 관리하기 위해 `UITableViewController` 객체가 필요하다.

나중에 테이블 뷰의 내용을 업데이트할 가능성이 있는 경우 정적 데이터를 사용하지 말도록 하라. 테이블 뷰에 정적 데이터가 들어 있는 `UITableViewController` 에 데이터 소스 객체를 할당하는 것은 프로그래머의 오류이다.

