# UITableViewCell

테이블 뷰에서 단일 행의 시각적인 표현

## Declaration

```swift
class UITableViewCell : UIView
```

## Overview

`UITableViewCell` 객체는 단일 테이블 행의 내용을 관리하는 전문적인 뷰 타입이다. 주로 셀을 사용하여 앱의 사용자 지정 컨텐츠를 구성하고 표시한다. 그러나 `UITableViewCell` 은 테이블 관련 동작을 지원하기 위해 다음과 같은 몇 가지 특정 사용자 정의를 제공한다.

* 셀 선택 적용 또는 강조 표시
* detail 또는 disclosure 컨트롤과 같은 표준 accessory 뷰 추가
* 셀을 편집 가능한 상태로 설정
* 테이블에 시각적 계층 구조를 만들기 위해 셀의 내용을 삽입하라.

앱의 콘텐츠는 셀의 bounds 대부분을 차지하지만, 셀은 다른 콘텐츠의 공간을 만들기 위해 그 공간을 조정할 수 있다. 셀은 trailing edge에 accessory 뷰를 표시한다. 테이블을 편집 모드로 전환할 때, 셀은 컨텐츠 영역의 leading edge에 삭제 컨트롤을 추가하고, 선택적으로 reorder 컨트롤을 위해 accessory 뷰를 교환한다.

![tableviewcell](../.gitbook/assets/tableviewcell.png)

모든 테이블 뷰에는 콘텐츠 표시를 위한 셀 타입이 하나 이상 있어야 하며, 테이블에는 다양한 타입의 콘텐츠를 표시할 수 있는 셀 타입이 여러 개 있을 수 있다. 테이블 셀을 만드는 방법에 대한 자세한 내용은 Filling a Table with Data를 참조 하라.

## Configuring Your Cell's Content

스토리보드 파일에 셀의 컨텐츠 및 레이아웃을 구성하라. 테이블에는 기본적으로 하나의 셀 타입이 있지만 테이블의 프로토타입 셀 속성에서 값을 변경하여 더 추가할 수 있다. 셀의 내용을 구성하는 것 외에도 다음 속성을 구성하라.

* 식별자. 이 식별자\(재사용 식별자라고도 함\)를 사용하여 셀 생성
* 스타일. 표준 타입 중 하나를 선택하거나 사용자 지정 셀을 정의하라.
* 클래스. 사용자 지정 동작과 함께 `UITableViewCell` 하위 클래스를 지정하라.

표준 타입의 경우 `UITableViewCell` 은 컨텐츠를 표시하기 위한 뷰를 제공한다. 셀의 `textLabel`, `detailTextLabel` 및 `imageView` 프로퍼티에 값을 할당하기만 하면 된다. 다음 그림은 사용자가 제공하는 값이 셀의 내용 영역 내에 어떻게 배치되는지를 보여준다. 만약 셀에 대한 이미지를 제공하지 않는다면, 셀은 다른 컨텐츠 뷰에 여분의 공간을 분배한다.

![configure\_tableview\_cell](../.gitbook/assets/configure_tableview_cell.png)

셀 모양을 사용자 정의하는 방법에 대한 자세한 내용은 Configuring the Cells for Your Table을 참조 하라.

