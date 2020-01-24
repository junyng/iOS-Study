# Appendix C: Key-Value Coding Extensions

코어 애니메이션은 [`CAAnimation`](https://developer.apple.com/documentation/quartzcore/caanimation) 및 [`CALayer`](https://developer.apple.com/documentation/quartzcore/calayer) 클래스에 관련된 N 프로토콜을 확장한다. 이 확장은 일부 키에 대한 기본값을 추가하고, 래핑 컨벤션을 확장하며 [`CGPoint`](https://developer.apple.com/documentation/coregraphics/cgpoint), [`CGRect`](https://developer.apple.com/documentation/coregraphics/cgrect), [`CGSize`](https://developer.apple.com/documentation/coregraphics/cgsize), 및 [`CATransform3D`](https://developer.apple.com/documentation/quartzcore/catransform3d) 타입에 대한 키패스 지원을 추가한다.

### Key-Value Coding Compliant Container Classes

CA 및 CA 클래스는 임의 키에 대한 값을 설정할 수 있는 키-값 코딩 호환 컨테이너 클래스이다. 키 `someKey`가 `CALayer` 클래스의 선언된 속성이 아니더라도 다음과 같이 값을 설정할 수 있다:

```objectivec
[theLayer setValue:[NSNumber numberWithInteger:50] forKey:@"someKey"];
```

다른 키 패스에 대한 값을 검색하는 것과 같이 임의 키 값을 검색할 수도 있다. 예를 들어, 이전에 설정된 일부 키 패스 값을 검색하려면 다음 코드를 사용한다.

```objectivec
someKeyValue=[theLayer valueForKey:@"someKey"];
```

> **OS X 참고:** 해당 클래스의 인스턴스에 대해 설정한 추가 키를 자동으로 아카이브하는 `CAAnimation` 및 `CALayer`클래스는 [`NSCoding`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intf/NSCoding)프로토콜을 지원한다.

### Default Value Support

코어 애니메이션은 키-값 코딩에 규칙을 추가하여 클래스가 설정된 값이 없는 키에 대해 기본값을 제공할 수 있다. [`CAAnimation`](https://developer.apple.com/documentation/quartzcore/caanimation)및 [`CALayer`](https://developer.apple.com/documentation/quartzcore/calayer)클래스는 `defaultValueForKey:`클래스 메서드를 사용하여 이 규칙을 지원한다.

키의 기본 값을 제공하려면 원하는 클래스의 하위 클래스를 생성하고 기본값인 `defaultValueForKey:` 메서드를 재정의하라. 이 메서드의 구현은 키 매개변수를 검사하고 적절한 기본값을 반환해야 한다. Listing C-1은 `masksToBounds` 속성에 대한 기본값을 제공하는 레이어 객체에 대한 `defaultValueForKey:` 메서드의 샘플 구현을 보여준다.

**Listing C-1** defaultValueForKey 구현 예:

```objectivec
+ (id)defaultValueForKey:(NSString *)key
{
    if ([key isEqualToString:@"masksToBounds"])
         return [NSNumber numberWithBool:YES];
 
    return [super defaultValueForKey:key];
}
```

### Wrapping Conventions

키의 데이터가 스칼라 값 또는 C 데이터 구조체로 구성될 때 해당 타입을 레이어에 할당하기 전에 객체에 감싸야 한다. 마찬가지로 해당 타입에 접근할 때 객체를 검색한 다음 해당 클래스에 대한 확장을 사용하여 해당 값을 풀어야 한다. Table C-1 은 일반적으로 사용되는 C 타입과 C 타입을 감싸는 데 사용하는 Objective-C 클래스를 나열한다.

**Table C-1** C 타입에 대한 래퍼 클래스

| C type | Wrapping class |
| :--- | :--- |
| [`CGPoint`](https://developer.apple.com/documentation/coregraphics/cgpoint) | [`NSValue`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue) |
| [`CGSize`](https://developer.apple.com/documentation/coregraphics/cgsize) | `NSValue` |
| [`CGRect`](https://developer.apple.com/documentation/coregraphics/cgrect) | `NSValue` |
| [`CATransform3D`](https://developer.apple.com/documentation/quartzcore/catransform3d) | `NSValue` |
| [`CGAffineTransform`](https://developer.apple.com/documentation/coregraphics/cgaffinetransform) | [`NSAffineTransform`](https://developer.apple.com/documentation/foundation/nsaffinetransform) \(OS X only\) |

### Key Path Support for Structures

[`CAAnimation`](https://developer.apple.com/documentation/quartzcore/caanimation) 및 [`CALayer`](https://developer.apple.com/documentation/quartzcore/calayer) 클래스를 사용하면 키 패스를 사용하여 선택한 데이터 구조체의 필드에 접근할 수 있다. 이 기능은 애니메이션으로 만들 데이터 구조체의 필드를 지정하는 편리한 방법이다. 이러한 규칙을 [`setValue:forKeyPath:`](https://developer.apple.com/documentation/objectivec/nsobject/1418139-setvalue)및 [`valueForKeyPath:`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/EOF/EOControl/Classes/NSObjectAdditions/Description.html#//apple_ref/occ/instm/NSObject/valueForKeyPath:)메서드와 함께 사용하여 해당 필드를 설정하고 가져올 수도 있다.

#### CATransform3D Key Paths

향상된 키 패스 지원을 사용하여 C 데이터 타입을 포함하는 속성의 특정 변환 값을 검색할 수 있다. 레이어 변환의 전체 키 패스를 지정하려면 문자열 값 `transform` 또는 하위 레이어 `sublayerTransform` 다음에 Table C-2의 필드 키 패스를 사용하라. 예를 들어, 레이어의 z축을 중심으로 회전 계수를 지정하려면 키 패스 `transform.rotation.z`를 지정하라.

**Table C-2** Transform field value key paths.

| Field Key Path | Description |
| :--- | :--- |
| `rotation.x` | Set to an `NSNumber` object whose value is the rotation, in radians, in the x axis. |
| `rotation.y` | Set to an `NSNumber` object whose value is the rotation, in radians, in the y axis. |
| `rotation.z` | Set to an `NSNumber` object whose value is the rotation, in radians, in the z axis. |
| `rotation` | Set to an `NSNumber` object whose value is the rotation, in radians, in the z axis. This field is identical to setting the `rotation.z` field. |
| `scale.x` | Set to an `NSNumber` object whose value is the scale factor for the x axis. |
| `scale.y` | Set to an `NSNumber` object whose value is the scale factor for the y axis. |
| `scale.z` | Set to an `NSNumber` object whose value is the scale factor for the z axis. |
| `scale` | Set to an `NSNumber` object whose value is the average of all three scale factors. |
| `translation.x` | Set to an `NSNumber` object whose value is the translation factor along the x axis. |
| `translation.y` | Set to an `NSNumber` object whose value is the translation factor along the y axis. |
| `translation.z` | Set to an `NSNumber` object whose value is the translation factor along the z axis. |
| translation | Set to an `NSValue` object containing an `NSSize` or `CGSize` data type. That data type indicates the amount to translate in the x and y axis. |

다음 예에서는 [`setValue:forKeyPath:`](https://developer.apple.com/documentation/objectivec/nsobject/1418139-setvalue)메서드를 사용하여 레이어를 수정할 수 있는 방법을 보여준다. 예제에서는 x 축의 변환 계수를 10 포인트로 설정하여 레이어가 표시된 축을 따라 해당 양만큼 이동하게 한다.

```objectivec
[myLayer setValue:[NSNumber numberWithFloat:10.0] forKeyPath:@"transform.translation.x"];
```

> **Note:** 키 패스를 사용하여 값을 설정하는 것은 Objective-C 속성을 사용하여 설정하는 것과 같지 않다. 속성 표기법을 사용하여 변환 값 설정할 수 없다. 이전 키 패스 문자열과 함께 [`setValue:forKeyPath:`](https://developer.apple.com/documentation/objectivec/nsobject/1418139-setvalue)메서드를 사용해야 한다.

#### CGPoint Key Paths

지정된 속성의 값이 [`CGPoint`](https://developer.apple.com/documentation/coregraphics/cgpoint)데이터 타입인 경우, 해당 속성에 Table C-3의 필드 이름 중 하나를 추가하여 해당 값을 가져오거나 설정할 수 있다. 예를 들어, 레이어 [`position`](https://developer.apple.com/documentation/quartzcore/calayer/1410791-position) 속성의 x 구성요소를 변경하려면 키 패스 `position.x`에 써라.

**Table C-3** CGPoint 데이터 구조체 필드

| Structure Field | Description |
| :--- | :--- |
| `x` | The x component of the point. |
| `y` | The y component of the point. |

#### CGSize Key Paths

지정된 속성의 값이 [`CGSize`](https://developer.apple.com/documentation/coregraphics/cgsize)데이터 타입인 경우, 해당 속성에 Table C-4의 필드 이름 중 하나를 추가하여 해당 값을 가져오거나 설정할 수 있다.

**Table C-4** CGSize data structure fields

| Structure Field | Description |
| :--- | :--- |


| `width` | The width component of the size. |
| :--- | :--- |


| `height` | The height component of the size. |
| :--- | :--- |


#### CGRect Key Paths

지정된 속성의 값이 [`CGRect`](https://developer.apple.com/documentation/coregraphics/cgrect)데이터 타입인 경우, 해당 값을 가져오거나 설정하려면 Table C-3의 필드 이름을 속성에 추가하라. 예를 들어, 레이어 [`bounds`](https://developer.apple.com/documentation/quartzcore/calayer/1410915-bounds)속성의 너비 구성요소를 변경하려면, 키 패스 `bounds.size.width`를 쓸 수 있다.

**Table C-5** CGRect data structure fields.

| Structure Field | Description |
| :--- | :--- |
| `origin` | The origin of the rectangle as a `CGPoint`. |
| `origin.x` | The x component of the rectangle origin. |
| `origin.y` | The y component of the rectangle origin. |
| `size` | The size of the rectangle as a `CGSize`. |
| `size.width` | The width component of the rectangle size. |
| `size.height` | The height component of the rectangle size. |

