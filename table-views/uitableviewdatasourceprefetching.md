# UITableViewDataSourcePrefetching

테이블 뷰의 데이터 요구 사항에 대한 사전 경고를 제공하여 장기간 실행 중인 데이터 작업을 조기에 시작할 수 있는 프로토콜

## Declaration

```swift
protocol UITableViewDataSourcePrefetching
```

## Overview

`tableView(_:cellForRowAt:)` 데이터 소스 메서드가 호출 되기 전에 테이블 뷰의 데이터 소스와 함께 데이터 소스 객체를 사용하여 셀의 데이터 로드를 시작하라. 테이블 뷰로 데이터 소스 사전 가져오기를 지원하려면 다음 단계가 필요하다.

* 테이블 뷰 및 해당 정규 데이터 소스를 생성하라.
* `UITableViewDataSourcePrefetching` 프로토콜을 채택한 객체를 생성하고 테이블 뷰의 `prefetchDataSource` 프로퍼티에 할당하라.
* `tableView(_:prefetchRowsAt:)` 구현시 지정된 인덱스 경로에서 셀에 필요한 데이터의 비동기 로드를 시작하라.
* `tableView(_:cellForRowAt:)` 메서드로 테이블의 사전 설정된 데이터를 사용하여 셀을 표시하도록 준비하라.
* 테이블 뷰 `tableView(_:cancelPrefetchingForRowsAt:)` 메서드에 더 이상 데이터가 필요하지 않다고 알려지면 보류 중인 데이터 로드 작업을 취소하라.

> **Note**
>
> 테이블 뷰의 모든 셀에 대해 prefetch 메서드가 반드시 호출되는 것은 아니다. 데이터 로딩에 대한 제안된 접근방식에 대한 자세한 내용은 Loading Data Asynchronously 를 참고하라.

테이블 뷰 객체를 구성할 때 prefetch 데이터 소스를 해당 `prefetchDataSource` 프로퍼티에 할당하라. 테이블 뷰의 작동 방식에 대한 자세한 내용은 `UITableView` 를 참조하라.

## Loading Data Asynchronously

`tableView(_:prefetchRowsAt:)` 메서드는 테이블 뷰의 모든 셀에 대해 반드시 호출되는 것은 아니다. `tableView(_:cellForRowAt:)` 메서드의 구현은 다음과 같은 잠재적 상황에 대처할 수 있어야 한다.

* prefetch 요청을 통해 데이터가 로드되었으며 표시될 준비가 되었다.
* 데이터는 현재 사전 검색 중이지만 아직 사용할 수 없다.
* 데이터가 아직 요청되지 않았다.

이러한 모든 상황을 처리하는 한 가지 접근방식은 각 행에 대한 데이터를 로드하기 위해 `Operation` 을 사용하는 것이다. `Operation` 객체를 생성하여 prefetch 메서드에 저장한다. 데이터 소스 메서드는 작업과 결과를 검색하거나 존재하지 않는 경우 이를 생성할 수 있다. 비동기 프로그래밍 모델을 사용하여 원하는 동작을 수행하는 방법에 대한 추가 정보는 Concurrency Programming Guide를 참조하라.

