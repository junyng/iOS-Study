# UIRefreshControl

스크롤 뷰의 내용 리프레쉬를 시작하는 표준 컨트롤

## Declaration

```swift
class UIRefreshControl : UIControl
```

## Overview

`UIRefreshControl` 객체는 테이블 뷰와 콜렉션 뷰를 포함하여 `UIScrollView` 객체에 붙일 수 있는 표준 컨트롤이다. 스크롤 가능한 뷰에 이 컨트롤을 추가하여 사용자에게 내용을 새로 고치는 표준 방법을 제공하라. 사용자가 스크롤 가능한 내용 영역의 상단을 아래로 끌면 스크롤 뷰에서 새로 고침 컨트롤이 표시되고 프로그래스바가 애니메이션을 시작하고 앱에 알린다. 이 노티피케이션을 사용하여 내용을 업데이트하고 리프레쉬 컨트롤을 해제하라.

![refresh\_control](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/refresh_control.png)

리프레쉬 컨트롤은 `UIControl` 의 target-action 메커니즘을 사용하여 컨텐츠를 업데이트할 시기를 알려준다. 활성화시 리프레쉬 컨트롤은 구성 시간에 제공한 액션 메서드를 호출한다. 액션 메서드를 추가할 때, 다음 예제 코드와 같이 `valueChanged` 이벤트를 수신하도록 구성하라. 액션 메서드를 사용하여 내용을 업데이트하고, 완료되면 리프레쉬 컨트롤의 `endRefreshing()` 메서드를 호출하라.

```swift
func configureRefreshControl () {
   // Add the refresh control to your UIScrollView object.
   myScrollingView.refreshControl = UIRefreshControl()
   myScrollingView.refreshControl?.addTarget(self, action:
                                      #selector(handleRefreshControl),
                                      for: .valueChanged)
}

@objc func handleRefreshControl() {
   // Update your content…

   // Dismiss the refresh control.
   DispatchQueue.main.async {
      self.myScrollingView.refreshControl?.endRefreshing()
   }
}
```

> **Note**
>
> `UITableViewController` 는 연관된 테이블의 새로 고침 동작을 관리하기 위한 `refresh Control` 프로퍼티도 포함되어 있다.

