# README

## Navigation

> 네비게이션은 유저에게 자연스럽고 친숙한 느낌이 들어야하며 인터페이스를 지배하거나 콘텐츠에 집중을 떼도록 하면 안됩니다. iOS에서는 세 가지 주요 네비게이션 방식이 있습니다.

### Hierarchical Navigation

목적 화면에 도달할 때까지 화면을 하나씩 선택하는 구조입니다. 다른 목적지로 가려면, 한 스텝 되돌아가거나 처음부터 다시 시작해 다른 선택을 해야합니다. 설정 및 메일에서 이 네비게이션 방식을 사용합니다.

![navigation\_hierarchical](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/navigation_hierarchical.png)

### Flat Navigation

여러 콘텐츠 항목간 화면 전환을 할 수 있습니다. 뮤직앱, 앱스토어 앱이 위의 네비게이션 방식을 사용합니다.

![navigation\_flat](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/navigation_flat.png)

### Content-Driven or Experience-Driven Navigation

콘텐츠를 통해서 자유롭게 이동합니다. 혹은 콘텐츠 자체가 네비게이션을 정의 합니다. 게임, books 앱 그리고 다른 몰입형 앱은 일반적으로 이 네비게이션 방식을 사용합니다.

![navigation\_experience\_driven](https://github.com/junyng/study-apple-docs/tree/c4b292b17da2edc8670232ab9689281024a64f04/.gitbook/assets/navigation_experience_driven.png)

몇몇 앱은 위의 세가지 방식을 결합하여 구성하기도 합니다. 예를 들어, 카테고리 화면을 나누기 위해 사용하는 플랫 네비게이션 방식과 각 화면별 계층 네비게이션을 결합해 사용합니다.

**명확한 경로를 제공해야 합니다.** 사용자들이 항상 앱의 어디에 있는지, 다음 목적지는 어떻게 가는지 알아야합니다. 네비게이션 방식에 관계 없이 컨텐츠를 통해 화면을 전환하는 경로는 논리적이고 예측 가능하며 따라 하기 쉽게 유지되어야 합니다. 일반적으로 하나의 화면 당 하나의 경로를 부여해야 합니다. 그러나 여러 화면을 볼 필요가 있는 경우, 모달 화면을 사용합니다.

**컨텐츠에 빠르고 쉽게 접근할 수 있는 정보 구조를 설계합니다.** 즉, 최소한의 탭, 스와이프, 화면으로 정보 구조를 구성합니다.

**터치 제스처를 사용해 유동적으로 생성합니다.** 최소한의 인터페이스의 마찰에 쉽게 이동할 수 있도록 합니다. 예를들어, 유저가 화면의 측면을 손가락으로 밀어 이전 화면으로 돌아가게 할 수 있습니다.

**표준 네비게이션 구성 요소를 사용합니다.** 가능한 표준 객체 라이브러에 포함된 컨트롤을 사용합니다. 사용자는 이러한 컨트롤에 익숙하기 때문에 어떻게 다루는지 직관적으로 알고 있습니다.

네비게이션 바를 사용하여 데이터 계층이 이동하게 합니다. 네비게이션 바의 제목은 계층구조의 현재 위치를 표시할 수 있으며, 뒤로가기 버튼을 누르면 이전 위치로 쉽게 돌아갈 수 있습니다.

**탭바를 사용하여 콘텐츠 카테고리를 보여줍니다.** 탭 바를 사용하면 현재 위치에 관계 없이 빠르게 카테고리 간에 전환할 수 있습니다.

**동일한 컨텐츠 유형의 여러 페이지가 있는 경우 페이지 컨트롤을 사용합니다.** 페이지 컨트롤은 사용 가능한 페이지 수와 현재 활성 페이지 수를 명확하게 전달합니다. 예를 들어, 기본앱 중 하나인 날씨 앱은 페이지 컨트롤을 사용하여 위치별 날씨 페이지를 표시합니다.

## 출처

### Apple documentation

### [Human Interface Guidelines - App Architecture](https://developer.apple.com/design/human-interface-guidelines/ios/app-architecture/navigation/)

### 부스트 코스

### [2. 회원가입 화면 구현 - 내비게이션 인터페이스란?](https://www.edwith.org/boostcourse-ios/lecture/16857/)

