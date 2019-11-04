# Notification

## Notification

알림은 하나 이상의 관찰 대상에게 프로그램 내의 이벤트를 알리기 위해 발송된 메시지를 말한다. Cocoa의 알림 메커니즘은 브로드캐스트 모델을 따른다. 이는 프로그램 이벤트를 시작하거나 처리하는 객체가 해당 이벤트에 대해 알고자 하는 객체의 수에 관계없이 통신하는 방법이다. 이러한 알림 수신인\(관찰자\)는 이벤트에 대응하여 그들 자신의 모양, 행동 및 상태를 조정할 수 있다고 알려져 있다. 알림을 보내는 객체\(또는 게시\)는 이러한 관찰자가 무엇인지 알 필요가 없다. 따라서 알림은 프로그램에서 조정과 응집도를 얻기 위한 강력한 메커니즘이다. 이는 프로그램에서 객체 간의 강한 의존성의 필요성을 감소시킨다. \(이러한 의존성은 해당 객체의 재사용성을 감소시킨다.\) Foundation, AppKit 및 기타 Objective-C 프레임워크의 많은 클래스는 프로그램이 관찰하기 위해 등록할 수 있는 알림을 정의한다.

알림 메커니즘의 중심부는 알림 센터\(`NSNotificationCenter`\)로 알려진 프로세스별 싱글톤 객체이다. 객체가 알림을 게시하면 알림 센터로 이동하며, 알림을 위한 일종의 정보 교환소와 방송 센터 역할을 한다. 애플리케이션의 다른 곳에 있는 이벤트에 대해 알아야 하는 객체들은 해당 이벤트가 발생할 때 알림을 받고 싶다는 것을 알리기 위해 알림 센터에 등록한다. 알림 센터가 해당 관찰자에게 알림을 동기식으로 제공하더라도 알림 대기열\(`NSNotificationQueue`\)을 사용하여 비동기식으로 게시할 수 있다.

![](file:///Users/BLU/TIL/iOS/Cocoa-Core-Competencies/Images/notificationcenter_2x.png?lastModify=1572841108)

### The Notification Object

알림은 `NSNotification`클래스의 인스턴스로 표시된다. 알림 객체에는 고유한 이름, 게시 객체 및 \(선택적으로\) userInfo 딕셔너리라고 하는 보충 정보 딕셔너리 등 몇 가지 상태 비트가 포함되어 있다. 관심 있는 관찰자에게 알림이 전달되면 알림 객체는 알림을 처리하는 메서드의 인수로 전달된다.

### Observing a Notification

알림을 관찰하려면 싱글톤 `NSNotificationCenter` 인스턴스를 가져와 `addObserver:selector:name:object:` 메시지를 보내라. 일반적으로 이 등록 단계는 애플리케이션을 시작한 직후에 수행된다. `addObserver:selector:name:object:` 메서드의 두 번째 매개 변수는 알림을 처리하기 위해 구현하는 메서드를 식별하는 선택기이다. 메서드에는 다음과 같은 서명이 있어야 한다.

```text
- (void)myNotificationHandler:(NSNotification *)notif;
```

```text
NSString *AMMyNotification = @"AMMyNotication";
```

알림을 게시하려면 `postNotificationName:object:userInfo:`\(또는 유사한\) 메시지를 싱글톤 `NSNotificationCenter`객체로 전송하라. 이 메서드는 알림을 알림 센터에 보내기 전에 알림 객체를 생성한다.

알림을 게시하기 전에 알림의 이름으로 고유한 전역 문자열 상수를 정의하라. 사용하는 관습은 2개 또는 3개 문자로 된 애플리케이션 접두사를 사용한다.

### Posting a Notification

이 처리 메서드에서는 알림에서 정보를 추출하여 응답에 도움을 줄 수 있으며, 특히 userInfo 딕셔너리의 데이터\(있는 경우\)를 추출할 수 있다.



#### Prerequisite Articles

[Selector](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html#//apple_ref/doc/uid/TP40008195-CH48-SW1)  
[Message](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59-SW1)

**Related Articles**

[Delegation](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14-SW1)  
[Model-View-Controller](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html#//apple_ref/doc/uid/TP40008195-CH32-SW1)  
[Singleton](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Singleton.html#//apple_ref/doc/uid/TP40008195-CH49-SW1)

**Definitive Discussion**

_Notification Programming Topics_

**Sample Code Projects**

[HeadsUpUI](https://developer.apple.com/library/archive/samplecode/HeadsUpUI/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007998)

