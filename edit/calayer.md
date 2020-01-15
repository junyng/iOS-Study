---
description: 이미지 기반 콘텐츠를 관리하고 해당 콘텐츠에서 애니메이션을 수행할 수 있는 객체.
---

# CALayer

### Declaration

```swift
class CALayer : NSObject
```

### Overview

레이어는 뷰에 대한 백킹 스토어를 제공하는 데 자주 사용되지만 콘텐츠를 표시하는 뷰 없이 사용할 수도 있다. 레이어의 주요 업무는 당신이 제공하는 시각적 콘텐츠를 관리하는 것이지만 레이어 자체는 배경색, 테두리, 그림자와 같이 설정할 수 있는 시각적 속성을 가지고 있다. 또한 레이어는 시각적 콘텐츠를 관리하는 것 외에도, 해당 콘텐츠를 화면에 표시하는 데 사용되는 콘텐츠의 기하학적 구조\(위치, 크기, 변환 등\)에 대한 정보를 유지한다. 레이어의 특성을 수정하는 것은 레이어의 내용이 기하학에서 애니메이션을 시작하는 방법이다. 레이어 객체는 레이어의 타이밍 정보를 정의하는 [`CAMediaTiming`](https://developer.apple.com/documentation/quartzcore/camediatiming) 프로토콜을 채택함으로써 레이어의 지속시간과 페이싱을 캡슐화한다.

레이어 객체가 뷰에 의해 작성된 경우 뷰는 일반적으로 레이어 대리자로 자동으로 지정되며 해당 관계를 변경해서는 안된다. 자신을 만드는 레이어의 경우 [`delegate`](https://developer.apple.com/documentation/quartzcore/calayer/1410984-delegate) 객체를 할당하고 해당 객체를 사용하여 레이어의 내용을 동적으로 제공하고 다른 작업을 수행할 수 있다. 레이어는 또한 서브뷰의 레이아웃을 별도로 관리하기 위해 레이아웃 관리자 객체\([`layoutManager`](https://developer.apple.com/documentation/quartzcore/calayer/1410749-layoutmanager) 속성에 할당됨\)을 가질 수 있다.

