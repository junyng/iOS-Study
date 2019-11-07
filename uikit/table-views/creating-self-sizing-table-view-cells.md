# Creating Self-Sizing Table View Cells

동적 타입을 지원하는 테이블 뷰 셀을 만들고 시스템 간격 제약 조건을 사용하여 주변 텍스트 라벨의 간격을 조정하라.

## Overview

이 샘플 코드 프로젝트는 동적 타입을 지원하는 자체 사이징 테이블 뷰 셀을 만드는 방법을 보여준다. 동적 타입은 사용자가 셀에 표시되는 텍스트 크기를 제어할 수 있도록 하기 때문에, 셀이 텍스트 크기에 따라 크기가 조정되는 것이 중요하다.

또한 이 프로젝트에서는 자동 레이아웃 제약을 사용하여 텍스트 크기에 따라 주변 텍스트 라벨의 간격을 자동으로 조정하는 방법을 보여준다. 자동 간격을 입증하기 위해 셀은 두개의 `UIFont` 객체: 헤드라인 라벨, 본문 라벨이 표시된다.

## Add Dynamic Type Support

동적 타입에 대한 지원을 추가하기 위해 셀은 각 라벨에 크기를 가진 글꼴을 할당한다. 헤드라인 라벨의 경우, `headline` 텍스트 스타일이 있는 선호되는 글꼴이 사용된다. 선호하는 글꼴은 시스템 글꼴로 다른 크기로 확장할 수 있다. 초기 텍스트 크기는 `headline` 텍스트 스타일에 대한 글꼴 메트릭에 의해 결정된다.

```swift
headlineLabel.font = UIFont.preferredFont(forTextStyle: .headline)headlineLabel.adjustsFontForContentSizeCategory = true
```

본문 라벨의 경우 사용자 지정 글꼴이 사용된다. 그러나 사용자 지정 글꼴로 동적 타입을 지원하려면 특정 텍스트 스타일에 대한 글꼴 메트릭을 사용하는 글꼴 버전을 생성해야 한다. 그러나 사용자 정의 글꼴로 동적 타입을 지원하려면 특정 텍스트 스타일에 대한 글꼴 메트릭을 채택하는 글꼴 버전을 생성해야 한다. 본문 라벨의 경우, 본문 텍스트 스타일과 함께 Palatino 커스텀 글꼴을 사용한다.

```swift
guard let palatino = UIFont(name: "Palatino", size: 18) else {    fatalError("""        Failed to load the "Palatino" font.        Since this font is included with all versions of iOS that support Dynamic Type, verify that the spelling and casing is correct.        """    )}bodyLabel.font = UIFontMetrics(forTextStyle: .body).scaledFont(for: palatino)bodyLabel.adjustsFontForContentSizeCategory = true
```

동적 타입의 효과를 보기 전에 `adjustsFontForContentSizeCategory` 프로퍼티는 각 라벨에서 `true` 로 설정해야 한다. 이 프로퍼티는 사용자가 선호하는 텍스트 크기를 변경할 때 라벨에 글꼴의 텍스트 크기를 자동으로 조정하도록 지시한다. 자세한 내용은 Scaling Fonts Automatically를 참조하라.

## Use Auto Layout Constraints to Adjust Cell Size and Spacing

이 때 두 라벨은 자동으로 텍스트 크기를 조정할 수 있다. 그러나 셀은 크기를 조절할 수 없다. 오토레이아웃 제약은 크기를 조정하고 셀 `contentView` 및 셀에 포함된 레이블에 간격을 주기 위해서 필요하다.

## Set the Horizontal Position for Each Label

두 라벨의 너비는 셀의 컨텐츠 뷰의 너비를 채우도록 확장되어야 한다. 헤드라인 라벨은 본문 라벨 위에 표시되어야 한다. 이를 위해 라벨 너비를 정의하는 제약조건부터 각 라벨에 오토레이아웃 제약조건을 추가한다. 헤드라인 레이블의 경우 컨텐츠 뷰의 leading 및 trailing 마진 사이의 공간을 채우도록 라벨에 제약 조건이 추가된다. 본문 라벨의 경우 leading, trailing을 헤드라인 라벨의 leading, trailing 앵커와 동일하게 설정하는 제약조건이 추가된다.

```swift
headlineLabel.leadingAnchor.constraint(equalTo: contentView.layoutMarginsGuide.leadingAnchor).isActive = trueheadlineLabel.trailingAnchor.constraint(equalTo: contentView.layoutMarginsGuide.trailingAnchor).isActive = truebodyLabel.leadingAnchor.constraint(equalTo: headlineLabel.leadingAnchor).isActive = truebodyLabel.trailingAnchor.constraint(equalTo: headlineLabel.trailingAnchor).isActive = true
```

헤드라인 라벨의 leading, trailing 앵커와 동일한 본문 라벨의 leading, trailing 앵커를 설정하면 각 라벨의 왼쪽과 오른쪽 가장자리가 항상 동일한 위치에 있는지 확인할 수 있다. 이 접근방식은 추가적인 장점이 있다. 헤드라인 라벨의 왼쪽 또는 오른쪽 가장자리를 조정하면 본문 라벨에 변경 사항이 자동으로 적용된다. 라벨이 두 개뿐일 때 이것은 사소한 것처럼 보일 수 있지만, 다른 라벨의 동일한 앵커와 동일한 라벨의 leading, trailing 앵커를 설정하면 가장자리를 정렬해야 하는 라벨이 더 많을 때 시간을 절약할 수 있다.

## Set the Vertical Position for Each Label

수평 포지셔닝을 시행하면 각 라벨의 수직 위치를 설정할 수 있다. 오토레이아웃 제약조건은 헤드라인 텍스트를 본문 텍스트 위에 배치하면서 수직 정렬을 배치하는데 다시 사용된다.

수직 위치를 설정할 수 있는 한 가지 방법은 일정한 값을 기준으로 두 라벨 사이의 거리를 정의하는 제약조건을 추가하는 것이다. 그러나 일정한 값에 의존하는 것의 문제는 텍스트 크기가 바뀔 때마다 값을 조정해야 한다는 것이다. 그렇지 않으면 텍스트가 산산이 흩어져 보이거나 비좁아 보이므로 읽기 어렵게 만들어 진다. iOS 11 이상에서는 시스템 간격 제약 조건을 사용하여 상수 값에 의존하고 설정하는 것을 피할 수 있다.

시스템 간격 제약조건은 두 UI 요소 사이의 거리를 제약조건을 생성할 때 사용되는 앵커에 의해 제공된 정보를 기반으로한 값으로 설정한다. 예를 들어, 시스템 간격 제약조건은 다른 라벨의 `lastBaselineAnchor` \(이 라벨에서 가장 위쪽에 있는 텍스트의 기준선\) 아래에 시스템에 의해 정의된 거리에서 라벨의 첫 번째 `firstBaselineAnchor`\(이 라벨에서 가장 아래쪽에 있는 텍스트의 기준선\)를 배치할 수 있다. 제약조건은 제약조건의 상수 값을 조정하지 않고 텍스트 크기에 관계없이 두 라벨 사이에 적절한 간격을 항상 적용하도록 한다.

샘플 코드 프로젝트의 경우 셀은 시스템 간격 제약 조건을 사용한다.

1. 셀의 콘텐츠 뷰 상단과 헤드라인 라벨 사이의 간격을 설정하라.
2. 본문 라벨과 셀 컨텐츠 뷰 하단 사이의 간격을 설정하라.
3. 헤드라인과 본문 라벨 사이의 간격을 설정하라.

```swift
headlineLabel.firstBaselineAnchor.constraintEqualToSystemSpacingBelow(contentView.layoutMarginsGuide.topAnchor, multiplier: 1).isActive = truecontentView.layoutMarginsGuide.bottomAnchor.constraintEqualToSystemSpacingBelow(bodyLabel.lastBaselineAnchor, multiplier: 1).isActive = truebodyLabel.firstBaselineAnchor.constraintEqualToSystemSpacingBelow(headlineLabel.lastBaselineAnchor, multiplier: 1).isActive = true
```

시스템 간격 제약조건이 있는 상태에서 시스템은 텍스트 크기에 따라 두 라벨을 둘러싼 간격을 자동으로 조정한다.

![tableview\_system\_spacing\_constraints](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/tableview_system_spacing_constraints.png)

## Test with Accessibility Inspector

샘플 앱이 다른 텍스트 크기에 어떻게 반응하는지 테스트하려면 시뮬레이터에서 앱을 실행하고 Accessibility Inspector를 사용하여 텍스트 크기를 변경하라. Inspector를 사용하면 앱과 설정 앱 사이를 전환하지 않고도 텍스트 크기가 다른 앱의 인터페이스를 테스트 할 수 있다.

Accessibility Inspector 를 사용하려면 다음 단계를 수행하라.

1. Xcode를 시작하고 앱을 실행한다.
2. Xcode 메뉴바에서 **Xcode &gt; Open Developer Tool &gt; Accessibility Inspector**  inspector를 시작하라.
3. Accessibility Inspector의 왼쪽 상단 모서리에서 Simulator as the target를 선택하라.
4. Settings icon을 클릭하라.
5. 폰트 사이즈 슬라이더를 이동하여 앱에 표시되는 텍스트 크기를 변경하라.

