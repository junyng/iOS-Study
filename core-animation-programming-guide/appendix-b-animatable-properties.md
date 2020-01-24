# Appendix B: Animatable Properties

`CALayer` 와 `CIFilter` 의 많은 속성이 애니메이션화될 수 있다. 이 부록에는 기본적으로 사용되는 애니메이션과 함께 이러한 속성이 나열되어 있다.

### CALayer Animatable Properties

Table B-1에는 애니메이션을 고려할 수 있는 `CALayer` 클래스의 속성이 나열되어 있다. 각 속성에 대해 표에는 애니메이션 객체의 타입도 나열되어 있다.

**Table B-1** Layer properties and their default animations

| Property | Default animation |
| :--- | :--- |
| [`anchorPoint`](https://developer.apple.com/documentation/quartzcore/calayer/1410817-anchorpoint) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`backgroundColor`](https://developer.apple.com/documentation/quartzcore/calayer/1410966-backgroundcolor) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`backgroundFilters`](https://developer.apple.com/documentation/quartzcore/calayer/1410827-backgroundfilters) | Uses the default implied [`CATransition`](https://developer.apple.com/documentation/quartzcore/catransition) object, described in [Table B-3](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW3). Sub-properties of the filters are animated using the default implied `CABasicAnimation` object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`borderColor`](https://developer.apple.com/documentation/quartzcore/calayer/1410903-bordercolor) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`borderWidth`](https://developer.apple.com/documentation/quartzcore/calayer/1410917-borderwidth) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`bounds`](https://developer.apple.com/documentation/quartzcore/calayer/1410915-bounds) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`compositingFilter`](https://developer.apple.com/documentation/quartzcore/calayer/1410748-compositingfilter) | Uses the default implied [`CATransition`](https://developer.apple.com/documentation/quartzcore/catransition) object, described in [Table B-3](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW3). Sub-properties of the filters are animated using the default implied `CABasicAnimation` object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`contents`](https://developer.apple.com/documentation/quartzcore/calayer/1410773-contents) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`contentsRect`](https://developer.apple.com/documentation/quartzcore/calayer/1410866-contentsrect) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`cornerRadius`](https://developer.apple.com/documentation/quartzcore/calayer/1410818-cornerradius) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`doubleSided`](https://developer.apple.com/documentation/quartzcore/calayer/1410924-isdoublesided) | There is no default implied animation. |
| [`filters`](https://developer.apple.com/documentation/quartzcore/calayer/1410901-filters) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). Sub-properties of the filters are animated using the default implied `CABasicAnimation` object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`frame`](https://developer.apple.com/documentation/quartzcore/calayer/1410779-frame) | This property is not animatable. You can achieve the same results by animating the [`bounds`](https://developer.apple.com/documentation/quartzcore/calayer/1410915-bounds) and [`position`](https://developer.apple.com/documentation/quartzcore/calayer/1410791-position) properties. |
| [`hidden`](https://developer.apple.com/documentation/quartzcore/calayer/1410838-ishidden) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`mask`](https://developer.apple.com/documentation/quartzcore/calayer/1410861-mask) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`masksToBounds`](https://developer.apple.com/documentation/quartzcore/calayer/1410896-maskstobounds) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`opacity`](https://developer.apple.com/documentation/quartzcore/calayer/1410933-opacity) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`position`](https://developer.apple.com/documentation/quartzcore/calayer/1410791-position) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`shadowColor`](https://developer.apple.com/documentation/quartzcore/calayer/1410829-shadowcolor) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`shadowOffset`](https://developer.apple.com/documentation/quartzcore/calayer/1410970-shadowoffset) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`shadowOpacity`](https://developer.apple.com/documentation/quartzcore/calayer/1410751-shadowopacity) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`shadowPath`](https://developer.apple.com/documentation/quartzcore/calayer/1410771-shadowpath) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`shadowRadius`](https://developer.apple.com/documentation/quartzcore/calayer/1410819-shadowradius) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`sublayers`](https://developer.apple.com/documentation/quartzcore/calayer/1410802-sublayers) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`sublayerTransform`](https://developer.apple.com/documentation/quartzcore/calayer/1410888-sublayertransform) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`transform`](https://developer.apple.com/documentation/quartzcore/calayer/1410836-transform) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |
| [`zPosition`](https://developer.apple.com/documentation/quartzcore/calayer/1410884-zposition) | Uses the default implied [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) object, described in [Table B-2](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/AnimatableProperties/AnimatableProperties.html#//apple_ref/doc/uid/TP40004514-CH11-SW2). |

Table B-2에는 기본 속성 기반 애니메이션의 애니메이션 특성이 나열되어 있다.

**Table B-2** Default Implied Basic Animation

| Description | Value |
| :--- | :--- |
| Class | [`CABasicAnimation`](https://developer.apple.com/documentation/quartzcore/cabasicanimation) |
| Duration | 0.25 seconds, or the duration of the current transaction |
| Key path | Set to the property name of the layer. |

Table B-3은 기본 변환 기반 애니메이션에 대한 애니메이션 객체 설정을 나열한다.

**Table B-3** Default Implied Transition

| Description | Value |
| :--- | :--- |
| Class | [`CATransition`](https://developer.apple.com/documentation/quartzcore/catransition) |
| Duration | 0.25 seconds, or the duration of the current transaction |
| Type | Fade \(`kCATransitionFade`\) |
| Start progress | `0.0` |
| End progress | `1.0` |

### CIFilter Animatable Properties

코어 애니메이션은 코어 이미지의 [`CIFilter`](https://developer.apple.com/documentation/coreimage/cifilter)클래스에 다음과 같은 애니메이션 특성을 추가한다. 이러한 속성은 OS X에서만 사용할 수 있다.

* [`name`](https://developer.apple.com/documentation/coreimage/cifilter/1437997-setname)
* [`enabled`](https://developer.apple.com/documentation/coreimage/cifilter/1438276-enabled)

이러한 추가에 대한 자세한 내용은 CIFilter _Core Animation Additions_를 참조하라.

