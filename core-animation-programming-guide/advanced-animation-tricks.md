# Advanced Animation Tricks

속성 기반 또는 키프레임 애니메이션을 구성하여 더 많은 작업을 수행할 수 있는 여러 가지 방법이 있다. 여러 애니메이션을 함께 또는 순차적으로 수행해야 하는 앱은 보다 고급스런 동작을 사용하여 애니메이션의 타이밍을 동기화하거나 함께 묶을 수 있다. 당신은 또한 시각적 전환과 다른 흥미로운 애니메이션 효과를 만들기 위해 다른 유형의 애니메이션 객체를 사용할 수 있다.

### Transition Animations Support Changes to Layer Visibility

이름에서 알 수 있듯이, 전환 애니메이션 객체는 레이어에 대해 애니메이션화된 시각적 전환을 생성한다. 전환 객체의 가장 일반적인 용도는 한 레이어의 외관과 다른 레이어의 소멸을 조정된 방식으로 애니메이션화하는 것이다. 애니메이션이 레이어의 한 속성을 변경하는 속성 기반 애니메이션과 달리, 전환 애니메이션은 레이어의 캐시된 이미지를 조작하여 속성만 변경해도 어렵거나 불가능할 시각적 효과를 만들어 낸다. 표준 유형의 전환을 통해 드러내기, 밀어내기, 이동, 크로스페이딩을 수행할 수 있다. OS X에서는 코어 이미지 필터를 사용하여 와이프, 페이지 컬, 잔물결 또는 사용자 정의 효과와 같은 유형의 효과를 사용하는 전환을 만들 수도 있다.

전환 애니메이션을 수행하려면 [`CATransition`](https://developer.apple.com/documentation/quartzcore/catransition)객체를 생성하여 전환과 관련된 레이어에 추가하라. 전환 객체를 사용하여 수행할 전환 유형과 전환 애니메이션의 시작점과 끝점을 지정하라. 또한 전체 전환 애니메이션을 사용할 필요는 없다. 전환 객체를 사용하면 애니메이팅할 때 사용할 시작 및 종료 진행률 값을 지정할 수 있다. 이러한 값들은 애니메이션의 중간 지점에서 시작하거나 종료하는 것과 같은 것들을 가능하게 한다.

Listing 5-1은 두 뷰 사이에 애니메이션 밀어내기 전환을 만드는 데 사용되는 코드를 보여준다. 이 예에서 `myView1`과 `myView2`는 모두 동일한 상위 뷰에 동일한 위치에 있지만 현재 `myView1`만 볼 수 있다. 밀어내기 전환으로 인해 `myView1`이 왼쪽으로 미끄러져 사라지고 숨겨질 때까지 희미해진다. `myView2`는 오른쪽에서 미끄러져 눈에 띄게 된다. 두 뷰의 숨겨진 속성을 업데이트하면 애니메이션 끝에서 두 뷰의 가시성이 올바르게 표시된다.

**Listing 5-1**  Animating a transition between two views in iOS

```objectivec
CATransition* transition = [CATransition animation];
transition.startProgress = 0;
transition.endProgress = 1.0;
transition.type = kCATransitionPush;
transition.subtype = kCATransitionFromRight;
transition.duration = 1.0;
 
// Add the transition animation to both layers
[myView1.layer addAnimation:transition forKey:@"transition"];
[myView2.layer addAnimation:transition forKey:@"transition"];
 
// Finally, change the visibility of the layers.
myView1.hidden = YES;
myView2.hidden = NO;
```

두 레이어가 같은 전환에 포함되어 있을 때 두 레이어에 대해 동일한 전환 객체를 사용할 수 있다. 동일한 전환 객체를 사용하면 작성하기 위한 필요한 코드도 단순화된다. 그러나 다른 전환 객체를 사용할 수 있으며 각 레이어의 전환 매개 변수가 다른 경우 반드시 그렇게 해야 한다.

Listing 5-2는 OS X에 대한 전환 효과를 구현하기 위해 코어 이미지 필터를 사용하는 방법을 보여준다. 원하는 매개 변수로 필터를 구성한 후 전환 객체의 [`filter`](https://developer.apple.com/documentation/quartzcore/catransition/1412506-filter)속성에 할당한다. 그 후, 애니메이션을 적용하는 프로세스는 다른 유형의 애니메이션 객체와 동일하다.

**Listing 5-2**  Using a Core Image filter to animate a transition on OS X

```objectivec
// Create the Core Image filter, setting several key parameters.
CIFilter* aFilter = [CIFilter filterWithName:@"CIBarsSwipeTransition"];
[aFilter setValue:[NSNumber numberWithFloat:3.14] forKey:@"inputAngle"];
[aFilter setValue:[NSNumber numberWithFloat:30.0] forKey:@"inputWidth"];
[aFilter setValue:[NSNumber numberWithFloat:10.0] forKey:@"inputBarOffset"];
 
// Create the transition object
CATransition* transition = [CATransition animation];
transition.startProgress = 0;
transition.endProgress = 1.0;
transition.filter = aFilter;
transition.duration = 1.0;
 
[self.imageView2 setHidden:NO];
[self.imageView.layer addAnimation:transition forKey:@"transition"];
[self.imageView2.layer addAnimation:transition forKey:@"transition"];
[self.imageView setHidden:YES];
```

> **참고**: 애니메이션에서 코어 이미지 필터를 사용할 때 가장 까다로운 부분은 필터를 구성하는 것이다. 예를 들어, 바 스와이프의 경우 너무 높거나 너무 낮은 입력 각도를 지정하면 마치 전환이 이루어지지 않는 것처럼 보일 수 있다. 예상한 애니메이션을 볼 수 없는 경우 필터 매개 변수를 다른 값으로 조정하여 결과가 변경되는지 확인하라.

### Customizing the Timing of an Animation

타이밍은 애니메이션의 중요한 부분이며, 코어 애니메이션에서는 [`CAMediaTiming`](https://developer.apple.com/documentation/quartzcore/camediatiming)프로토콜의 메서드와 속성을 통해 애니메이션에 대한 정확한 타이밍 정보를 지정한다. 두 개의 핵심 애니메이션 클래스가 이 프로토콜을 채택한다. `CAAnimation`클래스는 애니메이션 객체에 타이밍 정보를 지정할 수 있도록 이 정보를 채택한다. 또한 이러한 애니메이션을 감싸는 암묵적 트랜잭션 객체는 일반적으로 우선되는 기본 타이밍 정보를 제공하지만, `CALayer`는 암시적 애니메이션에 대해 일부 타이밍 관련 기능을 구성할 수 있도록 이 기능을 채택한다.

타이밍과 애니메이션을 생각할 때, 레이어 객체가 시간과 어떻게 작용하는지 이해하는 것이 중요하다. 각 레이어는 애니메이션 타이밍을 관리하기 위해 사용하는 고유의 로컬 타임을 가지고 있다. 일반적으로 두 개의 서로 다른 레이어의 로컬 시간은 각 레이어에 대해 동일한 시간 값을 지정할 수 있을 정도로 가까우며 사용자는 아무것도 알아차리지 못할 수 있다. 그러나 레이어의 로컬 시간은 부모 레이어 또는 자체 타이밍 매개변수에 의해 수정될 수 있다. 예를 들어, 레이어의 [`speed`](https://developer.apple.com/documentation/quartzcore/camediatiming/1427647-speed) 속성을 변경하면 해당 레이어\(및 하위레이어\)의 애니메이션 기간이 비례적으로 변경 된다.

시간 값이 주어진 레이어에 적합한지 확인하기 위해 CALayer 클래스는 c 및 c 를 정의한다. 이러한 메서드를 사용하여 고정 시간 값을 레이어의 로컬 시간으로 변환하거나 시간 값을 한 레이어에서 다른 레이어로 변환할 수 있다. 이 메서드에서는 레이어의 로컬 시간에 영향을 미칠 수 있는 미디어 타이밍 속성을 고려하고 다른 레이어와 함께 사용할 수 있는 값을 반환한다. Listing 5-3은 레이어에 대한 현재 현지 시간을 얻기 위해 정기적으로 사용해야 하는 예를 보여준다. [`CACurrentMediaTime`](https://developer.apple.com/documentation/quartzcore/1395996-cacurrentmediatime) 함수는 컴퓨터의 현재 클럭 시간을 반환하는 편의 기능으로, 이 메서드는 레이어의 로컬 시간으로 가져가고 변환한다.

**Listing 5-3** Getting a layer's current local time

```objectivec
CFTimeInterval localLayerTime = [myLayer convertTime:CACurrentMediaTime() fromLayer:nil];
```

레이어의 로컬 시간에 시간 값을 가지면 해당 값을 사용하여 애니메이션 객체 또는 레이어의 타이밍 관련 속성을 업데이트할 수 있다. 이러한 타이밍 속성을 사용하면 다음을 포함하여 몇 가지 흥미로운 애니메이션 동작을 얻을 수 있다:

* [`beginTime`](https://developer.apple.com/documentation/quartzcore/camediatiming/1427654-begintime)속성을 사용하여 시작 시간을 설정하라. 일반적으로 애니메이션은 다음 업데이트 주기 동안 시작된다. `beginTime` 매개 변수를 사용하여 애니메이션 시작 시간을 몇 초 지연할 수 있다. 두 애니메이션을 함께 체인하는 방법은 애니메이션의 시작 시간을 다른 애니메이션의 종료 시간과 일치하도록 설정하는 것이다. 애니메이션 시작을 지연하는 경우 [`fillMode`](https://developer.apple.com/documentation/quartzcore/camediatiming/1427656-fillmode)속성을 [`kCAFillModeBackwards`](https://developer.apple.com/documentation/quartzcore/camediatimingfillmode/1427660-backwards)로 설정할 수도 있다. 이 채우기 모드는 레이어 트리의 레이어 객체가 다른 값을 포함하더라도 레이어가 애니메이션의 시작 값을 표시하게 한다. 이 채우기 모드가 없으면 애니메이션이 실행되기 전에 최종 값으로 점프하는 것을 볼 수 있다. 다른 채우기 모드도 사용할 수 있다.
* [`autoreverses`](https://developer.apple.com/documentation/quartzcore/camediatiming/1427645-autoreverses)속성은 애니메이션이 지정된 기간 동안 실행된 다음 애니메이션의 시작 값으로 돌아가도록 한다. 이 속성을 [`repeatCount`](https://developer.apple.com/documentation/quartzcore/camediatiming/1427666-repeatcount)속성과 결합하여 시작 값과 끝 값 사이에서 앞뒤로 애니메이션을 할 수 있다. 애니메이션을 자동 검색하는 경우 반복 횟수를 정수\(예: 1.0\)로 설정하면 애니메이션이 시작 값에 정지하게 된다. 절반 단계를 \(예: 1.5의 반복 횟수\)를 추가하면 애니메이션이 종료 값에 정지한다.
* 그룹 애니메이션이 포함된 [`timeOffset`](https://developer.apple.com/documentation/quartzcore/camediatiming/1427650-timeoffset)속성을 사용하여 다른 애니메이션보다 나중에 일부 애니메이션을 시작하라.

### Pausing and Resuming Animations

애니메이션을 일시 중지하려면 레이어들이 [`CAMediaTiming`](https://developer.apple.com/documentation/quartzcore/camediatiming)프로토콜을 채택하고 레이어 애니메이션 속도를 0.0으로 설정한다는 사실을 활용할 수 있다. 속도를 0으로 설정하면 값이 0이 아닌 값으로 다시 변경될 때까지 애니메이션이 일시 중지된다. Listing 5-4는 애니메이션을 나중에 일시 중지하거나 다시 시작하는 간단한 예를 보여준다.

**Listing 5-4** Pausing and resuming a layer's animations

```objectivec
-(void)pauseLayer:(CALayer*)layer {
   CFTimeInterval pausedTime = [layer convertTime:CACurrentMediaTime() fromLayer:nil];
   layer.speed = 0.0;
   layer.timeOffset = pausedTime;
}
 
-(void)resumeLayer:(CALayer*)layer {
   CFTimeInterval pausedTime = [layer timeOffset];
   layer.speed = 1.0;
   layer.timeOffset = 0.0;
   layer.beginTime = 0.0;
   CFTimeInterval timeSincePause = [layer convertTime:CACurrentMediaTime() fromLayer:nil] - pausedTime;
   layer.beginTime = timeSincePause;
}
```

### Explicit Transactions Let You Change Animation Parameters

레이어로 변경하는 모든 사항은 트랜잭션의 일부여야 한다. [`CATransaction`](https://developer.apple.com/documentation/quartzcore/catransaction)클래스는 적절한 시간에 애니메이션의 생성 및 그룹화 및 실행을 관리한다. 대부분의 경우, 당신은 자신의 트랜잭션을 만들 필요가 없다. 코어 애니메이션은 명시적 또는 암묵적 애니메이션을 레이어 중 하나에 추가할 때마다 암묵적 트랜잭션을 자동으로 생성한다. 하지만 명시적 트랜잭션을 만들어서 그 애니메이션을 보다 정확하게 관리할 수도 있다.

`CATransaction` 클래스의 메서드를 사용하여 트랜잭션을 만들고 관리한다. 새로운 트랜잭션 호출을 시작\(그리고 암묵적으로 생성\)하려면 [`begin`](https://developer.apple.com/documentation/quartzcore/catransaction/1448282-begin)클래스 메서드를 시작하고, 트랜잭션을 종료하려면 [`commit`](https://developer.apple.com/documentation/quartzcore/catransaction/1448255-commit)클래스 메서드를 호출한다. 그 사이에는 트랜잭션의 일부가 되고 싶은 변경사항이 있다. 예를 들어, 레이어의 두 속성을 변경하려면 Listing 5-5에서 코드를 사용할 수 있다.

**Listing 5-5**  Creating an explicit transaction

```objectivec
[CATransaction begin];
theLayer.zPosition=200.0;
theLayer.opacity=0.0;
[CATransaction commit];
```

트랜잭션을 사용하는 주요 이유 중 하나는 명시적 트랜잭션의 범위 내에서 기간, 타이밍 함수 및 기타 매개변수를 변경할 수 있기 때문이다. 또한 전체 트랜잭션에 완료 블록을 할당하여 애니메이션 그룹이 완료되면 앱에 알림을 보낼 수 있다. 애니메이션 파라미터를 변경하려면 [`setValue:forKey:`](https://developer.apple.com/documentation/quartzcore/catransaction/1448278-setvalue)메서드를 사용하여 트랜잭션 딕셔너리에서 적절한 키를 수정해야 한다. 예를 들어 기본 기간을 10초로 변경하려면 Listing 5-6에 나와 있는 것처럼 [`kCATransactionAnimationDuration`](https://developer.apple.com/documentation/quartzcore/kcatransactionanimationduration)키를 변경하라.

**Listing 5-6**  Changing the default duration of animations

```objectivec
[CATransaction begin];
[CATransaction setValue:[NSNumber numberWithFloat:10.0f]
                 forKey:kCATransactionAnimationDuration];
// Perform the animations
[CATransaction commit];
```

다른 애니메이션 세트에 대해 다른 기본값을 제공할 상황에서 트랜잭션을 중첩할 수 있다. 다른 트랜잭션에 중첩하려면 `begin` 클래스 메서드를 다시 호출하라. 각 `begin`호출은 `commit`메소드에 대응하는 호출에 의해 매칭되어야 한다. 가장 바깥쪽 트랜잭션에 대한 변경 사항을 커밋한 후에야 코어 애니메이션은 관련 애니메이션을 시작한다.

Listing 5-7은 한 트랜잭션이 다른 트랜잭션 안에 중첩된 예를 보여준다. 이 예에서 내부 트랜잭션과 동일한 애니메이션 매개변수를 변경하지만 다른 값을 사용한다.

**Listing 5-7**  Nesting explicit transactions

```objectivec
[CATransaction begin]; // Outer transaction
 
// Change the animation duration to two seconds
[CATransaction setValue:[NSNumber numberWithFloat:2.0f]
                forKey:kCATransactionAnimationDuration];
// Move the layer to a new position
theLayer.position = CGPointMake(0.0,0.0);
 
[CATransaction begin]; // Inner transaction
// Change the animation duration to five seconds
[CATransaction setValue:[NSNumber numberWithFloat:5.0f]
                 forKey:kCATransactionAnimationDuration];
 
// Change the zPosition and opacity
theLayer.zPosition=200.0;
theLayer.opacity=0.0;
 
[CATransaction commit]; // Inner transaction
 
[CATransaction commit]; // Outer transaction
```

### Adding Perspective to Your Animations

앱은 3개의 공간적 차원으로 레이어를 조작할 수 있지만, 단순성을 위해 코어 애니메이션은 평행 투영을 사용하여 레이어를 표시하며, 이는 기본적으로 장면을 2차원 평면으로 평면화한다. 이 기본 동작은 [`zPosition`](https://developer.apple.com/documentation/quartzcore/calayer/1410884-zposition)값이 다른 동일한 크기의 레이어가 z축에서 멀리 떨어져 있더라도 동일한 크기로 나타나게 한다. 그런 장면을 3차원으로 보는 관점은 사라진다. 그러나, 레이어의 변환 매트릭스를 원근 정보를 포함하도록 수정함으로써 그 동작을 변경할 수 있다.

장면\(scene\)의 원근법을 수정할 때는, 보고 있는 레이어를 포함하는 슈퍼 레이어의 서브 레이어 [`sublayerTransform`](https://developer.apple.com/documentation/quartzcore/calayer/1410888-sublayertransform) 행렬을 수정할 필요가 있다. 슈퍼 레이어를 수정하면 모든 하위 레이어에 동일한 원근법 정보를 적용하여 작성해야 하는 코드를 단순화한다. 또한 서로 다른 평면에서 겹치는 형제 하위 레이어에 투시법을 정확하게 적용하도록 한다.

Listing 5-8은 상위 레이어에 대한 단순한 투시 변환을 생성하는 방법을 보여준다. 이 경우 사용자 지정 eyePosition 변수는 레이어를 뷰의 z축을 따라 상대적인 거리를 지정한다. 일반적으로 예상된 방식으로 레이어 방향을 유지하기 위해 eyePosition에 양의 값을 지정한다. 값이 클수록 장면\(scene\)이 더 평평해지고 값이 작으면 레이어 사이에 시각적 차이가 더 극적으로 발생한다.

**Listing 5-8** Adding a perspective transform to a parent layer

```objectivec
CATransform3D perspective = CATransform3DIdentity;
perspective.m34 = -1.0/eyePosition;
 
// Apply the transform to a parent layer.
myParentLayer.sublayerTransform = perspective;
```

상위 레이어를 구성한 경우 하위 레이어의 `zPosition`속성을 변경하고 눈 위치와의 상대적 거리에 따라 크기가 어떻게 변하는지 관찰할 수 있다.

