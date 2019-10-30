# Adding Headers and Footers to Table Sections

헤더 및 푸터 뷰를 테이블 뷰 섹션에 추가하여 행 그룹을 시각적으로 구분하라.

## Overview

헤더 및 푸터 뷰를 섹션의 시작과 끝에 대한 시각적 마커로 사용한다. 헤더와 푸터 뷰는 선택 사항으로 원하는 만큼 많거나 적게 사용자 지정할 수 있다.

![tableview\_header\_footer](../.gitbook/assets/tableview_header_footer.png)

텍스트 라벨을 사용하여 기본 헤더 또는 푸터를 생성하려면 테이블의 데이터 소스 객체의 `tableView(_:titleForHeaderInSection:)` 또는 `tableView(_:titleForFooterInSection:)` 메서드를 오버라이드 하라. 테이블 뷰는 표준 헤더 또는 푸터를 생성해 지정된 위치에 있는 테이블에 삽입한다.

```swift
// Create a standard header that includes the returned text.
override func tableView(_ tableView: UITableView, titleForHeaderInSection 
                            section: Int) -> String? {
   return "Header \(section)"
}

// Create a standard footer that includes the returned text.
override func tableView(_ tableView: UITableView, titleForFooterInSection 
                            section: Int) -> String? {
   return "Footer \(section)"
}
```

> **Note**
>
> 헤더와 푸터는 일반적으로 단일 섹션에 적용되지만, 테이블의 `tableHeaderView` 또는 `tableFooterView` 를 사용하여 전체 테이블에 대해 단일 헤더 또는 푸터 뷰를 제공할 수도 있다. 전역 헤더는 테이블 내용 상단에 나타나고 전역 푸터는 하단에 나타난다.

## Customize the Header and Footer Views

사용자 정의 헤더와 푸터 뷰는 테이블의 섹션에 고유한 모양을 제공한다. 사용자 정의 헤더와 푸터를 사용하여 원하는 뷰를 지정하고 할당된 공간 내의 아무곳에나 배치하라. 테이블의 다른 섹션에 대해 다른 헤더 또는 푸터 뷰를 제공할 수도 있다.

사용자 정의 헤더 또는 푸터 뷰를 생성하려면:

1. `UITableViewHeaderFooterView` 객체를 사용해 헤더와 푸터의 모양을 정의한다.
2. `UITableViewHeaderFooterView` 객체를 테이블 뷰에 등록하라.
3. 테이블 뷰 delegate 객체의 `tableView(_:viewForHeaderInSection:)` 와 `tableView(_:viewForFooterInSection:)` 메서드를 구현하여 뷰를 생성하고 구성하라.

항상 `UITableViewHeaderFooterView` 객체를 헤더와 푸터에 대해 사용하라. 이 뷰는 셀이 사용하는 것과 동일한 재사용 모델을 지원하므로, 매번 뷰를 작성하는 대신 뷰를 재활용할 수 있다. 헤더 또는 푸터 뷰를 등록한 후 테이블 뷰의 `dequeueReusableHeaderFooterView(with:Identifier:)` 메서드를 사용해 해당 뷰 인스턴스를 요청하라. 재활용된 헤더 또는 푸터 뷰가 사용가능하면, 테이블 뷰는 새로운 뷰를 생성하기 전에 먼저 해당 헤더를 반환한다.

`UITableViewHeaderFooterView` 객체를 그대로 사용하여 해당 `contentView` 프로퍼티에 뷰 추가 또는 뷰를 하위 분류하고 추가할 수 있다. 스택 뷰 또는 오토레이아웃 제약 조건을 사용하여 컨텐츠 뷰 내부에 하위 뷰를 배치하라. 컨텐츠의 배경을 변경하려면 헤더-푸터 뷰의 `backgroundView` 프로퍼티를 수정하라. 다음 예제 코드는 생성 시 이미지 및 라벨 뷰를 배치하는 사용자 정의 헤더 뷰를 보여준다.

```swift
class MyCustomHeader: UITableViewHeaderFooterView {
    let title = UILabel()
    let image = UIImageView()

    override init(reuseIdentifier: String?) {
        super.init(reuseIdentifier: reuseIdentifier)
        configureContents()
    }

    func configureContents() {
        image.translatesAutoresizingMaskIntoConstraints = false
        title.translatesAutoresizingMaskIntoConstraints = false

        contentView.addSubview(image)
        contentView.addSubview(title)

        // Center the image vertically and place it near the leading
        // edge of the view. Constrain its width and height to 50 points.
        NSLayoutConstraint.activate([
            image.leadingAnchor.constraint(equalTo: contentView.layoutMarginsGuide.leadingAnchor),
            image.widthAnchor.constraint(equalToConstant: 50),
            image.heightAnchor.constraint(equalToConstant: 50),
            image.centerYAnchor.constraint(equalTo: contentView.centerYAnchor),

            // Center the label vertically, and use it to fill the remaining
            // space in the header view. 
            title.heightAnchor.constraint(equalToConstant: 30),
            title.leadingAnchor.constraint(equalTo: image.trailingAnchor, 
                   constant: 8),
            title.trailingAnchor.constraint(equalTo: 
                   contentView.layoutMarginsGuide.trailingAnchor),
            title.centerYAnchor.constraint(equalTo: contentView.centerYAnchor)
        ])
    }
}
```

테이블 뷰 구성의 일부로 헤더 뷰를 등록하라.

```swift
override func viewDidLoad() {
   super.viewDidLoad()

   // Register the custom header view.
   tableView.register(MyCustomHeader.self, 
       forHeaderFooterViewReuseIdentifier: “sectionHeader")
}
```

delegate의 `tableView(_:viewForHeaderInSection:)` 메서드에서 사용자 정의 뷰를 생성하고 구성하라. 다음 예제 코드는 등록된 사용자 정의 헤더를 삭제하고 제목과 이미지 프로퍼티를 구성한다.

```swift
override func tableView(_ tableView: UITableView, 
        viewForHeaderInSection section: Int) -> UIView? {
   let view = tableView.dequeueReusableHeaderFooterView(withIdentifier:
               "sectionHeader") as! MyCustomHeader
   view.title.text = sections[section]
   view.image.image = UIImage(named: sectionImages[section])

   return view
}
```

다음 이미지는 결과 헤더를 보여준다.

![tableview\_resulting\_headers](../.gitbook/assets/tableview_resulting_headers.png)

## Change the Height of Headers and Footers

테이블 뷰는 섹션 헤더와 푸터의 높이를 이를 나타내는 뷰와 별도로 추적한다. `UITableView` 는 두 가지 항목에 대해 기본 사이즈를 제공한다. 모든 헤더\(또는 모든 푸터\)의 높이가 동일한 경우 테이블 뷰 섹션을 사용하여 `sectionHeaderHeight` 및 `sectionFooterHeight` 프로퍼티 높이를 지정하라. 또는 테이블 뷰에서 제공하는 기본값을 사용하라.

헤더 또는 푸터 높이가 모두 동일하지 않거나 동적으로 변경할 수 있는 경우, delegate 객체의 `tableView(_:heightForHeaderInSection:)` 및 `tableView(_:heightForFooterInSection:)` 메서드를 사용하여 높이를 제공하라. 이러한 메서드를 구현할 때는 테이블의 모든 헤더와 푸터에 대한 값을 제공해야 한다. 테이블 뷰는 보이는 헤더와 푸터의 높이를 요구한다. 사용자가 스크롤할 때 테이블 뷰는 화면 밖으로 이동한 다음 다시 화면으로 이동하는 경우를 포함하여 나타나는 대로 각 높이에 대한 높이를 제공하도록 요청한다.

