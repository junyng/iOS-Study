# UILocalizedIndexedCollation

섹션 인덱스를 갖는 테이블 뷰를 위해 데이터를 조직화, 정렬, 로컬라이즈 하는 객체

## Declaration

```swift
class UILocalizedIndexedCollation : NSObject
```

## Overview

인덱싱된 테이블 뷰의 데이터를 정렬하거나 관리하기 위해 UILocalizedIndexedCollation 객체와 데이터 소스 객체를 사용하라. 인덱스는 테이블 뷰에서 순차적으로 내용을 갖는 이상적인 방법이다. 예를 들어, 연락처 앱은 연락처를 알파벳 순으로 정렬하고 빠르게 전화번호를 탐색해 인덱스를 표시한다. 테이블 뷰에서 collation 객체를 테이블 섹션 타이틀과 인덱스 타이틀의 소스로 사용하라. 또한 당신의 테이블의 각 섹션의 항목을 정렬하는데 사용된다.

섹션 인덱스에 대한 데이터를 준비하기 위해, 각 모델 객체 인덱싱 되기를 한 indexed-collation 객체를 만들고 각각의 인덱싱 모델 객체를 위해 `section(for:collationStringSelector)` 를 호출하라. 이 메서드는 이러한 각 객체가 표시되어야하는 섹션을 결정하고 섹션을 식별하는 정수를 반환한다. 그런 다음 테이블 뷰 컨트롤러는 각 객체를 해당 섹션의 로컬 배열에 넣는다. 각각의 섹션 배열에 대해서 컨트롤러는 `sortedArray(from:collationStringSelector:)` 메서드를 호출해 섹션의 모든 객체를 정렬한다. indexed-collation 객체는 다음 예제 코드와 같이 테이블 뷰 컨트롤러가 테이블 뷰에 횡단 색인 데이터를 제공하는 데 사용하는 데이터 저장소이다.

```swift
func tableView(tableView: UITableView!, titleForHeaderInSection section: Int) -> String! {
    let currentCollation = UILocalizedIndexedCollation.currentCollation() as UILocalizedIndexedCollation
    let sectionTitles = currentCollation.sectionTitles as NSArray
    return sectionTitles.objectAtIndex(section) as String
}

func sectionIndexTitlesForTableView(tableView: UITableView!) -> NSArray! {
    let currentCollation = UILocalizedIndexedCollation.currentCollation() as UILocalizedIndexedCollation
    return currentCollation.sectionIndexTitles as NSArray
}

func tableView(tableView: UITableView!, sectionForSectionIndexTitle title: String!, atIndex index: Int) -> Int {
    let currentCollation = UILocalizedIndexedCollation.currentCollation() as UILocalizedIndexedCollation
    return currentCollation.sectionForSectionIndexTitleAtIndex(index)
}
```

