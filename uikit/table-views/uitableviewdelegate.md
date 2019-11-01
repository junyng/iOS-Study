# UITableViewDelegate

선택 관리, 헤더 및 푸터 섹션 구성, 셀 삭제 및 순서 변경, 테이블 뷰에서 기타 작업 수행에 대한 메서드들

## Declaration

```swift
protocol UITableViewDelegate
```

## Overview

이 프로토콜의 메서드를 사용하여 다음 기능을 관리하라.

* 커스텀 헤더 및 푸터 뷰 생성 및 관리하라.
* 로우, 헤더 및 푸터의 사용자 지정 높이를 지정하라.
* 더 나은 스크롤 지원을 위해 높이 추정치를 제공하라.
* 로우 내용을 삽입하라.
* 로우 선택에 응답하라.
* 테이블 로우의 스와이프 및 기타 작업에 응답하라.
* 테이블 내용 편집을 지원하라.

테이블 뷰는 `NSIndexPath` 객체를 사용하여 로우 및 섹션을 지정한다. 로우 및 섹션 인덱스를 해석하는 방법에 대한 자세한 내용은 Specifying the Location of Rows and Sections를 참조 하라.

