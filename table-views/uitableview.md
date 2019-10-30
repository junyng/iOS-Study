# UITableView

단일 열에 정렬된 행을 사용하여 데이터를 표시하는 뷰

## Declaration

```swift
class UITableView : UIScrollView
```

## Overview

iOS의 테이블 뷰는 수직으로 스크롤되는 콘텐츠의 단일 열이 행으로 구분되어 표시된다. 테이블의 각 행에는 앱 컨텐츠의 한 부분이 포함되어 있다. 예를 들어, 연락처 앱은 각 연락처의 이름을 별도의 행에 표시하며, 설정 앱의 메인 페이지에는 사용 가능한 설정 그룹이 표시된다. 하나의 긴 행 목록을 표시하도록 테이블을 구성하거나 관련 행을 섹션으로 그룹화하여 내용을 쉽게 탐색할 수 있다.

![uitableview](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/uitableview.png)

테이블은 데이터가 고도로 구조화되거나 계층적으로 조직된 앱에 의해 일반적으로 사용된다. 계층적 데이터를 포함하는 앱은 종종 네비게이션 뷰 컨트롤러와 함께 테이블을 사용하므로 계층의 다른 레벨 간의 탐색이 용이하다. 예를 들어 Settings 앱은 테이블과 네비게이션 컨트롤러를 사용하여 시스템 설정을 구성한다.

`UITableView` 는 테이블의 기본 모양을 관리하지만, 앱은 실제 컨텐츠를 표시하는 셀\(`UITableViewCell` 객체\)을 제공한다. 표준 셀 구성은 텍스트와 이미지의 간단한 조합을 표시하지만 원하는 콘텐츠를 표시하는 사용자 정의 셀을 정의할 수 있다. 또한, 셀 그룹에 대한 추가 정보를 제공하기 위해 헤더 및 푸터 뷰를 제공할 수 있다.

## Adding a Table View to Your Interface

인터페이스에 테이블 뷰를 추가하려면 테이블 뷰 컨트롤러\(`UITableViewController`\) 객체를 스토리보드로 끌어다 놓아라. Xcode는 사용자가 구성 및 사용할 수 있는 뷰 컨트롤러와 테이블 뷰를 모두 포함하는 새로운 씬을 생성한다.

테이블 뷰는 데이터 기반이며, 일반적으로 데이터 소스 원본 객체에서 해당 데이터를 가져온다. 데이터 소스 객체는 앱의 데이터를 관리하고 테이블의 셀을 만들고 구성하는 역할을 한다. 테이블의 내용이 변경되지 않으면 스토리보드 파일에서 해당 내용을 구성할 수 있다.

테이블 데이터를 지정하는 방법에 대한 정보는 Filling a Table with Data를 참고하라.

## Saving and Restoring the Table's Current State

테이블 뷰는 UIKit 앱 복원을 지원한다. 테이블 데이터를 저장 및 복원하려면 테이블 뷰의 `restorationIdentifier` 프로퍼티에 비어 있지 않은 값을 할당하라. 상위 뷰 컨트롤러가 저장되면 테이블 뷰에서 현재 선택되고 표시되는 행의 인덱스 경로를 자동으로 저장한다. 만약 테이블의 데이터 소스 객체가 `UIDataSourceModelAssociation` 프로토콜을 채택하는 경우 테이블은 인덱스 경로 대신 해당 항목에 대해 제공하는 고유한 ID가 저장된다.

앱의 상태 정보를 저장 및 복원하는 방법에 대한 정보는 Preserving Your App's UI Across Launches를 참고하라.

