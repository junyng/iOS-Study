# Changing a Layer’s Default Behavior

코어 애니메이션은 동작 객체를 사용하는 레이어에 대한 암묵적 애니메이션 동작을 구현한다. 액션 객체는 [`CAAction`](https://developer.apple.com/documentation/quartzcore/caaction)프로토콜을 준수하고 레이어에서 수행할 몇 가지 관련 동작을 정의하는 객체이다. 모든 `CAAnimation`객체는 프로토콜을 구현하며, 일반적으로 레이어 속성이 변경될 때마다 실행되도록 할당되는 객체들이다.

애니메이션 속성은 액션의 한 타입이지만 원하는 거의 모든 액션으로 동작을 정의할 수 있다. 그러나 그러기 위해서는 액션 객체를 정의하고 앱의 레이어 객체와 연결해야 한다.

### Custom Action Objects Adopt the CAAction Protocol

자신의 액션 객체를 만들려면 클래스 중 하나에서 `CAAction`프로토콜을 채택하고 [`runActionForKey:object:arguments:`](https://developer.apple.com/documentation/quartzcore/caaction/1410806-run) 메서드를 구현하라. 이 메서드에서는 사용 가능한 정보를 사용하여 레이어에서 수행하려는 액션을 수행한다. 이 메서드를 사용하여 레이어에 애니메이션 객체를 추가하거나 다른 작업을 수행할 수 있다.

액션 객체를 정의할 때 해당 액션을 트리거할 방법을 결정해야 한다. 액션의 트리거는 나중에 해당 액션을 등록하는 데 사용하는 키를 정의한다. 액션 객체는 다음 상황 중 어느 하나에 의해 트리거될 수 있다:

* 레이어 속성 중 하나의 값이 변경되었다. 이것은 단지 애니메이션 가능한 것이 아닌 레이어의 속성 중 하나일 수 있다. \(또한 당신이 레이어에 추가하는 사용자 지정 속성과 행동을 연관시킬 수도 있다.\) 이 작업을 식별하는 키는 속성의 이름이다.
* 레이어는 가시화되거나 레이어 계층에 추가되었다. 이 액션을 식별하는 키는 [`kCAOnOrderIn`](https://developer.apple.com/documentation/quartzcore/kcaonorderin)이다.
* 레이어 계층으로부터 레이어가 제거되었다. 이 액션을 식별하는 키는 [`kCAOnOrderOut`](https://developer.apple.com/documentation/quartzcore/kcaonorderout)이다.
* 레이어가 전환 애니메이션에 포함되려고 한다. 이 액션을 식별하는 키는 [`kCATransition`](https://developer.apple.com/documentation/quartzcore/kcatransition)이다.

### Action Objects Must Be Installed On a Layer to Have an Effect

액션을 수행하기 전에 레이어는 실행할 해당 액션 객체를 찾아야 한다. 레이어 관련 액션의 핵심은 수정할 속성의 이름 또는 액션을 식별하는 특수 문자열이다. 레이어에서 적절한 이벤트가 발생하면 레이어는 키와 관련된 액션 객체를 검색하는 [`actionForKey:`](https://developer.apple.com/documentation/quartzcore/calayer/1410844-action)메서드를 호출한다. 이 검색 중에 앱이 여러 지점에서 자신을 삽입하고 해당 키에 대한 관련 액션 객체를 제공할 수 있다.

코어 애니메이션은 다음 순서로 액션 객체를 검색한다:

1. 레이어는 델리게이트를 가지고 해당 델리게이트는 [`actionForLayer:forKey:`](https://developer.apple.com/documentation/quartzcore/calayerdelegate/2097264-action)메서드를 구현하고 레이어는 해당 메서드를 호출한다. 델리게이트는 다음 중 하나를 수행해야 한다:
   * 지정된 키에 대한 액션 객체를 반환하라.
   * 액션을 처리하지 않으면 `nil` 을 반환하고, 이 경우 검색이 계속된다.
   * [`NSNull`](https://developer.apple.com/documentation/foundation/nsnull)객체를 반환하라. 이 경우 검색이 즉시 종료된다.
2. 레이어는 레이어의 [`actions`](https://developer.apple.com/documentation/quartzcore/calayer/1410789-actions)딕셔너리에서 주어진 키를 찾는다.
3. 레이어는 키가 들어 있는 액션 딕셔너리를 [`style`](https://developer.apple.com/documentation/quartzcore/calayer/1410875-style)딕셔너리에서 찾는다. \(즉, `style` 딕셔너리에는 값이 딕셔너리이기도 한 `actions`키가 포함되어 있다. 레이어는 이 두번째 딕셔너리에서 주어진 키를 찾는다.\)
4. 레이어는 [`defaultActionForKey:`](https://developer.apple.com/documentation/quartzcore/calayer/1410954-defaultactionforkey)클래스 메서드를 호출한다.
5. 레이어는 코어 애니메이션에 의해 정의된 암시적 액션\(만약 있다면\)을 수행한다.

적절한 검색 지점 중 하나에서 액션 객체를 제공하는 경우 레이어는 검색을 중지하고 반환된 액션 객체를 실행한다. 액션 객체를 찾으면 해당 객체의 [`runActionForKey:object:arguments:`](https://developer.apple.com/documentation/quartzcore/caaction/1410806-run)을 호출한다. 지정된 키에 대해 정의한 액션이 `CAAnimation`클래스의 인스턴스인 경우 해당 메서드의 기본 구현을 사용하여 애니메이션을 수행할 수 있다. `CAAction` 프로토콜을 준수하는 사용자 정의 객체를 정의하는 경우, 해당 메서드를 구현하여 적절한 조치를 취해야 한다.

액션 객체를 설치하는 위치는 레이어를 수정하는 방법에 따라 다르다.

* 특정 상황에서만 적용할 수 있는 조치 또는 이미 델리게이트 객체를 사용하는 레이어에 대해 델리게이트를 제공하고 해당 `actionForLayer:forKey:`메서드를 구현하라.
* 일반적으로 델리게이트를 사용하지 않는 레이어 객체의 경우, 레이어의 `actions`딕셔너리에 액션을 추가한다.
* 레이어 객체에서 정의하는 사용자 지정 속성과 관련된 액션의 경우 레이어 `style`딕셔너리에 액션을 포함한다.
* 레이어의 동작에 기초적인 액션의 경우, 레이어를 하위 분류하고 [`defaultActionForKey:`](https://developer.apple.com/documentation/quartzcore/calayer/1410954-defaultactionforkey)를 재정의한다.

Listing 6-1은 액션 객체를 제공하는 데 사용되는 델리게이트 메서드의 구현을 보여준다. 이 경우 델리게이트는 레이어의 [`contents`](https://developer.apple.com/documentation/quartzcore/calayer/1410773-contents)속성에 대한 변경 사항을 찾고 전환 애니메이션을 사용하여 새로운 콘텐츠를 제자리로 교환한다.

**Listing 6-1**  Providing an action using a layer delegate object

```objectivec
- (id<CAAction>)actionForLayer:(CALayer *)theLayer
                        forKey:(NSString *)theKey {
    CATransition *theAnimation=nil;
 
    if ([theKey isEqualToString:@"contents"]) {
 
        theAnimation = [[CATransition alloc] init];
        theAnimation.duration = 1.0;
        theAnimation.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseIn];
        theAnimation.type = kCATransitionPush;
        theAnimation.subtype = kCATransitionFromRight;
    }
    return theAnimation;
}
```

### Disable Actions Temporarily Using the CATransaction Class

[`CATransaction`](https://developer.apple.com/documentation/quartzcore/catransaction)클래스를 사용하여 레이어 액션을 일시적으로 사용하지 않도록 설정할 수 있다. 레이어의 속성을 변경할 때, 코어 애니메이션은 일반적으로 변경을 애니메이션화하기 위해 암묵적인 트랜잭션 객체를 생성한다. 변경 내용을 애니메이션화하지 않으려면 명시적 트랜잭션을 생성하고 해당 [`kCATransactionDisableActions`](https://developer.apple.com/documentation/quartzcore/kcatransactiondisableactions)속성을 `true`로 설정하여 암시적 애니메이션을 비활성화할 수 있다. Listing 6-2는 레이어 트리에서 지정된 레이어를 제거할 때 애니메이션을 비활성화하는 코드 조각들을 보여준다.

**Listing 6-2**  Temporarily disabling a layer’s actions

```objectivec
[CATransaction begin];
[CATransaction setValue:(id)kCFBooleanTrue
                 forKey:kCATransactionDisableActions];
[aLayer removeFromSuperlayer];
[CATransaction commit];
```

애니메이션 동작을 관리하기 위해 트랜잭션 객체를 사용하는 방법에 대한 자세한 내용은 [Explicit Transactions Let You Change Animation Parameters](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AdvancedAnimationTricks/AdvancedAnimationTricks.html#//apple_ref/doc/uid/TP40004514-CH8-SW3)를 참조하라.

