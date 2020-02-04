# Resource Programming Guide

## 리소에 대해 <a id="pageTitle"></a>

컴퓨터 프로그램에 적용되는 리소은 프로그램의 실행 코드와 함께 제공되는 데이터 파일이다. 리소스는 복잡한 데이터 집합 또는 그래픽 콘텐츠의 생성을 코드 외부로 이동하여 보다 적절한 도구로 작성해야 하는 코드를 단순화한다. 예를 들어, 코드를 사용하여 픽셀별로 영상 픽셀을 만드는 것보다 영상 편집기에서 영상 픽셀을 만드는 것이 훨씬 효율적이고 실용적이다. 리소스 이용하려면, 당신의 코드는 런타임에 그것을 로드하고 사용하기만 하면 된다.

당신의 코드를 단순화하는 것 외에도, 리소스는 지화 과정의 모든 애플리케이션에 대한 친숙한 부분이다. 애플리케이션에서 하드 코딩 문자열과 기타 사용자가 볼 수 있는 콘텐츠 대신 해당 콘텐츠를 외부 리소스 파일에 저장할 수 있다. 애플리케이션 지역화는 지원되는 각 언어에 대해 각 리소스 파일의 새 버전을 생성하는 간단한 프로세스가 된다. OS X와 iOS 모두에서 사용되는 번들 메커니즘은 지역화된 리소스를 구성하고 사용자 선호 언어에 맞는 리소스 파일의 로딩을 용이하게 할 수 있는 방법을 제공한다.

이 문서는 OS X와 iOS에서 지원되는 리소스 유형과 코드에 있는 리소스를 사용하는 방법에 대한 정보를 제공한다. 이 문서는 리소스 생성 프로세스에 초점을 맞추지 않는다. 대부분의 리소스는 `/Developer/Applications` 디렉토리에 제공된 개발자 도구 또는 타사 애플리케이션을 사용하여 생성된다. 또한 이 문서는 애플리케이션에서 리소스를 사용하는 것을 언급하고 있지만, 이 정보는 프레임워크와 플러그인을 포함한 다른 유형의 번들 실행 파일에도 적용된다.

이 문서를 읽기 전에 애플리케이션 번들에 의해 부과되는 조직 구조를 숙지해야 한다. 이 구조를 이해하면 애플리케이션이 사용하는 리소스 파일을 쉽게 구성하고 찾을 수 있다. 번들 구조에 대한 자세한 내용은 [_Bundle Programming Guide_](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html#//apple_ref/doc/uid/10000123i)을 참조하라.

### g한눈에

애플리케이션에는 여러 종류의 리소스가 포함될 수 있지만, iOS와 OS X에 의해 직접 지원되는 리소스가 몇 개 있다.

#### 애플리케이션의 유저 인터페이스를 저장하는 Nib 파일

Nib 파일은 iOS 및 Mac 앱을 만드는 데 사용되는 중요한 리소스 유형이다. _nib 파일_은 런타임에 재생성하려는 동결건조 객체 집합을 기본적으로 포함하는 데이터 아카이브이다.  Nib 파일은 미리 구성된 윈도우, 뷰 및 기타 시각 지향적인 객체를 저장하는 데 가장 일반적으로 사용되지만 컨트롤러와 같은 비시각적인 객체도 저장할 수 있다.

Xcode의 인터페이스 빌더를 통해 nib 파일을 편집할 수 있으며, 이 파일은 객체 조립을 위한 그래픽 편집기를 제공한다. 애플리케이션에 nib 파일을 로드하면 nib-loading 코드는 파일의 각 객체를 인스턴스화하여 인터페이스 빌더에서 지정한 상태로 복원한다. 따라서 인터페이스 빌더에서 볼 수 있는 것은 런타임에 애플리케이션에서 얻을 수 있는 것이다.

> **관련 챕터:** [Nib Files](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/LoadingResources/CocoaNibs/CocoaNibs.html#//apple_ref/doc/uid/10000051i-CH4-SW8)

#### 지역화 가능한 텍스트를 포함하는 문자열 리소스

텍스트는 대부분의 사용자에서 중요한 부분이지만 지역화 변경의 영향을 가장 많이 받는 리소스이기도 하다. iOS와 OS X는 하드 코딩 텍스트를 코드로 변환하는 대신 사용자가 읽을 수 있는 문자열 파일\(UTF-16 인코딩에서\)에 사용자가 볼 수 있는 텍스트의 저장 기능을 지원한다. \(복수 "strings"의 사용은 의도적이며 해당 유형의 파일에 사용되는 `.strings` 파일 이름 확장명으로 인해 발생한다.\) 스트링 파일은 당신이 당신의 코드를 한번 쓴 다음 쉽게 변경할 수 있는 리소스파일로부터 적절한 지역화된 텍스트를 로드할 수 있게 함으로써 국제화 및 지역화 과정을 크게 단순화한다.

코어 파운데이션 및 파운데이션 프레임워크는 문자열 파일에서 텍스트를 로드하는 기능을 제공한다. 또한 이러한 설비를 사용하는 애플리케이션은 Xcode와 함께 제공되는 툴을 활용하여 개발 프로세스 전반에 걸쳐 이러한 리소스 파일을 생성하고 유지할 수 있다.

> **관련 챕터:** [String Resources](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/LoadingResources/Strings/Strings.html#//apple_ref/doc/uid/10000051i-CH6-SW1)

#### 미리 렌더링된 콘텐츠를 나타내는 이미지, 사운드 및 동영상

iOS 및 Mac 앱에서 이미지, 사운드 및 동영상 리소스는 중요한 역할을 한다. 이미지는 각 운영체제에서 사용되는 고유한 시각적 스타일을 만드는 역할을 한다. 또한 복잡한 시각적 요소에 대한 그리기 코드를 단순화하는 데 도움이 된다. 사운드 및 동영상 파일은 유사한 방식으로 애플리케이션의 전반적인 사용자 환경을 향상시키는 동시에 해당 환경을 만드는 데 필요한 코드를 단순화한다. 두 운영체제는 애플리케이션에서 이러한 유형의 리소스를 로드하고 표시하는 광범위한 리소스를 제공한다.

> **관련 챕터:** [Image, Sound, and Video Resources](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/LoadingResources/ImageSoundResources/ImageSoundResources.html#//apple_ref/doc/uid/10000051i-CH7-SW1)

#### 프로퍼티 리스트 및 데이터 파일은 코드로부터 데이터를 분리한다

프로퍼티 리스트 파일은 문자열, 숫자, 불, 날짜 및 원시 데이터 값을 저장하는 데 사용되는 구조화된 파일이다. 파일의 데이터 항목은 배열과 딕셔너리 구조를 사용하여 구성되며, 대부분의 항목은 고유한 키와 연관된다. 시스템은 프로퍼티 리스트를 사용하여 간단한 데이터 세트를 저장한다. 예를 들어, 거의 모든 애플리케이션에서 발견되는 `Info.plist` 파일은 프로퍼티 리스트 파일의 예다. 또한 프로퍼티 리스트 파일을 사용하여 데이터 스토리지 요구 사항을 단순화할 수 있다. 

프로퍼티 리스트 외에도 OS X는 특정 용도로 특수하게 구조화된 파일을 지원한다. 예를 들어, AppleScript 데이터와 사용자 도움말은 특수 포맷 데이터 파일을 사용하여 저장된다. 또한 사용자 정의 데이터 파일을 직접 만들 수도 있다.

> **관련 챕터:** [Data Resource Files](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/LoadingResources/DataResourceFiles/DataResourceFiles.html#//apple_ref/doc/uid/10000051i-CH8-SW2)

#### iOS는 장치-특정 리소스를 지원한다

iOS 4.0 이상에서는 특정 유형의 장치에서만 개별 리소스 파일을 사용할 수 있는 것으로 표시할 수 있다. 이 기능은 유니버셜 애플리케이션에 대해 작성해야 하는 코드를 단순화한다. iPhone용 리소스 파일의 한 버전과 iPad용 파일의 다른 버전을 로드하기 위해 별도의 코드 경로를 만드는 대신 번들로딩 루틴이 올바른 파일을 선택할 수 있다. 당신이 해야할 일은 리소스 파일을 적절하게 이름 짓는 것이다.

리소스 파일을 특정 장치와 연결하려면 사용자 정의 수정자 문자열을 해당 파일 이름에 추가한다. 이 수정자 문자열을 포함하면 다음 형식의 파일 이름이 생성된다:

_&lt;basename&gt;&lt;device&gt;_._&lt;filename\_extension&gt;_  
_&lt;basename&gt;_ 문자열은 리소스 파일의 원래 이름을 나타낸다. 또한 코드에서 파일에 접근할 때 사용하는 이름을 나타낸다. 마찬가지로 _&lt;filename\_extension&gt;_ 문자열은 파일의 유형을 식별하는 데 사용되는 표준 파일 이름 확장자이다. _&lt;device&gt;_ 문자열은 다음과 같은 값 중 하나가 될 수 있는 대소문자를 구분하는 문자열이다:

* `~ipad` - 리소스는 iPad 장치에만 로드해야 한다.
* `~iphone` - 리소스는 iPhone 또는 iPod 터치 장치에만 로드해야 한다.

모든 유형의 리소스 파일에 장치 수정자를 적용할 수 있다. 예를 들어 `MyImage.png` 라는 이미지가 있다고 가정하라. iPad와 iPhone용 이미지의 다른 버전을 지정하려면 `MyImage~ipad.png` 및 `MyImage~iphone.png`라는 이름을 가진 리소스 파일을 생성하여 번들 모두에 포함하라. 이미지를 로드하려면 코드에서 리소스를 `MyImage.png` 라고 계속 언급하고 시스템이 다음과 같이 적절한 버전을 선택하도록 하라:

```objectivec
UIImage* anImage = [UIImage imageNamed:@"MyImage.png"];
```

iPhone 또는 iPod 터치 장치에서 시스템은 `MyImage~ipad.png` 리소스 파일을 로드하고 iPad에서는 `MyImage~iphone.png` 리소스 파일을 로드한다. 장치의 특정 리소스 버전이 발견되지 않으면 시스템은 원래 파일 이름을 가진 리소스를 찾는 것으로 되돌아간다. 이전 예에서는 `MyImage.png` 라는 이미지가 된다.

### 참고

다음 애플 개발자 문서는 _Resource Programming Guide_와 개념적으로 관련되어 있다:

* [_Bundle Programming Guide_](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html#//apple_ref/doc/uid/10000123i) 실행 코드 및 리소스를 저장하기 위해 애플리케이션에서 사용하는 번들 구조를 설명한다.
* [_Internationalization and Localization Guide_](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i) 애플리케이션\(및 그 리소스\)를 다른 언어로 번역하기 위해 준비하는 과정을 기술한다.
* [_Xcode Overview_](https://developer.apple.com/library/archive/documentation/ToolsLanguages/Conceptual/Xcode_Overview/index.html#//apple_ref/doc/uid/TP40010215) 에서 Build a User Interface in Xcode는 nib 파일 리소스를 편집하기 위한 도구를 설명한다.
* [_Property List Programming Guide_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/PropertyLists/Introduction/Introduction.html#//apple_ref/doc/uid/10000048i) Cocoa 애플리케이션에 프로퍼티 리스트 리소스 파일을 로드하기 위한 시설에 대해 설명한다.
* [_Property List Programming Topics for Core Foundation_](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFPropertyLists/CFPropertyLists.html#//apple_ref/doc/uid/10000130i) C 기반 애플리케이션에 프로퍼티 리스트 파일을 로드하기 위해 사용 중인 시설에 대해 설명한다.

