---
description: '다수의 삽입, 삭제, 리로드 및 이동 작업을 그룹으로 애니메이션화한다.'
---

# performBatchUpdates\(\_:completion:\)

### Declaration

```swift
func performBatchUpdates(_ updates: (() -> Void)?, 
              completion: ((Bool) -> Void)? = nil)
```

### Parameters

* **`updates`** 삽입, 삭제, 리로드 또는 이동 작업을 수행하는 블록이다. 테이블의 로우를 수정하는 것 외에도 테이블의 데이터 소스를 업데이트하여 변경사항을 반영하라. 이 블록에는 반환 값이 없으며 매개변수가 없다.
* **`completion`** 모든 작업이 완료되면 실행할 완료 핸들러 블록이다. 이 블록에는 반환 값이 없으며 다음 매개 변수가 사용된다. 
  * **`finished`** 애니메이션이 성공적으로 완료되었는지 여부를 나타내는 불 값. 애니메이션을 어떤 이유로 중단한 경우 이 파라미터 값은 `false`이다.

### Discussion

여러 개의 개별 애니메이션이 아닌 단일 애니메이션 작업으로 테이블 뷰를 여러 번 변경하려는 경우 이 메서드를 사용하라. `update` 매개 변수에 전달된 블록을 사용하여 수행할 모든 작업을 지정하라.

삭제는 배치 작업에 삽입하기 전에 처리된다. 즉, 삭제에 대한 인덱스는 배치 작업 전에 테이블 뷰 상태의 인덱스를 기준으로 처리되고, 삽입에 대한 인덱스는 배치 작업에서 모든 삭제 후 상태 인덱스에 비례하여 처리된다.

### See Also

#### Performing Batch Updates to Rows and Sections

* [`func beginUpdates()`](https://developer.apple.com/documentation/uikit/uitableview/1614908-beginupdates)  테이블 뷰의 로우와 섹션을 삽입, 삭제 또는 선택하는 일련의 메서드 호출을 시작한다.
* [`func endUpdates()`](https://developer.apple.com/documentation/uikit/uitableview/1614890-endupdates)  테이블 뷰의 로우와 섹션을 삽입, 삭제, 선택 또는 리로드하는 일련의 메서드 호출을 완료한다.



