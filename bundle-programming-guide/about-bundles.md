# About Bundles

번들은 macOS와 iOS에서 소프트웨어를 제공하는 편리한 방법이다. 번들은 최종 사용자를 위한 단순화된 인터페이스를 제공함과 동시에 개발에 대한 지원을 제공한다. 이 장에서는 번들에 대한 소개를 제공하며, macOS와 iOS에서 번들이 수행하는 역할에 대해 설명한다.

### Bundles and Packages

번들과 패키지는 때때로 상호 교환적으로 언급되기는 하지만, 그것들은 실제로 매우 뚜렷한 개념을 나타낸다.

* _package_는 파인더가 사용자에게 단일 파일인 것처럼 표시하는 임의의 디렉토리다.
* _bundle_은 실행 가능한 코드와 그 코드에 사용되는 리소스를 보유하는 표준화된 계층 구조를 가진 디렉리다.

패키지는 macOS를 사용하기 쉽게 만드는 근본적인 추상화 중 하나를 제공한다. 컴퓨터의 응용 프로그램이나 플러그인을 보면 실제로 보고 있는 것은 디렉토리다. 패키지 디렉토리 안에는 응용 프로그램 또는 플러그인을 실행하는 데 필요한 코드와 리소스 파일이 있다. 그러나 패키지 디렉토리와 상호 작용하면 파인더는 이 디렉리를 단일 파일처럼 처리한다. 이러한 동작은 일상적인 사용자가 패키지의 내용에 부정적인 영향을 미칠 수 있는 변경을 하는 것을 방지한다. 예를 들어, 응용프로그램이 올바르게 실행되지 않도록 하는 리소스 또는 코드 모듈을 재배치하거나 삭제하지 못하도록 한다.

> 참고: 기본적으로 패키지는 불투명한 파일로 처리되지만 사용자가 컨텐츠를 보고 수정할 수 있다. 패키지 디렉토리의 상황별 메뉴에는 Show Package Contents 명령이 있다. 이 명령을 선택하면 패키지 디렉토리의 최상위 수준으로 설정된 새 파인더 창이 표시된다. 사용자는 이 창을 사용하여 패키지의 디렉토리 구조를 탐색하고 일반 디렉토리 계층 구조인 것처럼 변경할 수 있다.

패키지는 사용자 경험을 향상시키기 위해 있는 반면, 번들은 개발자들이 그들의 코드를 패키지화하는 것을 돕고 운영체제가 그 코드에 접근하는 것을 돕는 데 더 많이 맞춰져 있다. 번들은 소프트웨어와 관련된 코드와 리소스를 구성하기 위한 기본 구조를 정의한다. 이러한 구조의 존재는 지역화 등 중요한 기능을 용이하게 하는 데도 도움이 된다. 번들의 정확한 구조는 애플리케이션, 프레임워크 또는 플러그인을 생성하느냐에 따라 달라진다. 대상 플랫폼과 플러그인 유형 등 다른 요인에 따라 달라지기도 한다.

번들과 패키지가 서로 교환될 수 있다고 여겨지는 이유는 많은 종류의 번들도 패키지가 되기 때문이다. 예를 들어, 애플리케이션과 로드 가능한 번들은 대개 시스템에 의해 불투명한 디렉토리로 취급되기 때문에 패키지이다. 그러나 모든 번들이 패키지인 것은 아니며 그 반대의 경우도 마찬가지이다.

#### How the System Identifies Bundles and Packages

다음 조건 중 하나라도 참일 경우 Finder는 디렉토리를 패키지로 간주한다:

* 디렉토리는 알려진 파일 이름 확장자 `.app`, `.bundle`, `.framework`, `.plugin`, `.kext` 등을 가지고 있다.
* 디렉토리에 다른 응용 프로그램 클레임이 패키지 유형을 나타내는 확장명이 있다. [Document Packages](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/DocumentPackages/DocumentPackages.html#//apple_ref/doc/uid/10000123i-CH106-SW1)를 참조하라.
* 디렉터리에 패키지 비트가 설정되어 있다.

패키지를 지정하는 바람직한 방법은 패키지 디렉터리에 알려진 파일 이름 확장명을 제공하는 것이다. 대부분의 경우 Xcode는 올바른 확장을 적용하는 템플릿을 제공하여 이를 처리한다. 적절한 유형의 Xcode 프로젝트를 만드는 것 밖에 하지 않는다.

대부분의 번들도 패키지다. 예를 들어, 응용 프로그램과 플러그인은 일반적으로 Finder에 의해 단일 파일로 표시된다. 그러나, 이것은 모든 번들 타입에 해당되지 않는다. 특히 프레임워크는 링크와 런타임 사용을 목적으로 단일 유닛을 취급되는 번들의 일종이지만 프레임워크 디렉토리는 투명하여 개발자가 헤더 파일 및 포함된 기타 리소스를 볼 수 있다.

#### About Bundle Display Names

디스플레이 이름은 사용자에게 번들과 패키지가 Finder에 의존하는 클라이언트를 손상시키지 않고 어떻게 표시되는지에 대한 제어권을 제공한다. 사용자가 자유롭게 파일 이름을 바꿀 수 있는 반면, 응용 프로그램 또는 프레임워크의 이름을 변경하면 응용 프로그램 또는 프레임워크를 이름으로 참조하는 관련 코드 모듈이 중단될 수 있다. 따라서 사용자의 번들의 이름을 변경할 때, 그 변화는 표면적으로만 나타난다. Finder는 파일 시스템에서 번들 이름을 변경하지 않고 별도의 문자열\(display name이라고 함\)을 번들과 연결하여 그 문자열을 대신 표시한다.

디스플레이 이름은 사용자에게만 표시하기 위한 것이다. 코드의 디렉터리를 열거나 접근하기 위해 디스플레이 이름을 사용하지 않지만, 사용자에게 디렉터리 이름을 표시할 때 이 이름을 사용하라. 기본적으로 번들의 디스플레이 이름은 번들 이름 자체와 동일하다. 그러나 시스템은 다음과 같은 경우 기본 디스플레이 이름을 변경할 수 있다:

* 번들이 애플리케이션인 경우 대부분의 경우 Finder는 `.app` 확장자를 숨긴다.
* 번들이 지역화된 디스플레이 이름을 지원하는 경우\(사용자가 번들 이름을 명시적으로 변경하지 않은 경우\), Finder는 사용자의 현재 언어 설정과 일치하는 이름을 표시한다.

Finder는 대부분의 경우 애플리케이션의 `.app` 확장자를 숨기지만 혼동을 방지하기 위해 표시할 수 있다. 예를 들어, 사용자가 애플리케이션의 이름을 변경하고 새 이름에 다른 파일 이름 확장자가 포함된 경우 Finder는 확장자 `.app`을 표시하여 번들이 애플리케이션임을 명확히 한다. 예를 들어, 체스 애플리케이션에 `.mov` 확장자를 추가하려면 Finder가 `Chess.mov.app`을 표시하여 사용자가 `Chess.mov`가 QuickTime 파일이라고 생각하는 것을 방지한다.

디스플레이 이름 및 지역화된 번들 이름 지정에 대한 자세한 내용은 [_File System Overview_](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPFileSystem/BPFileSystem.html#//apple_ref/doc/uid/10000185i)을 참조하라.

### The Advantages of Bundles

번들은 개발자들에게 다음과 같은 이점을 제공한다:

* 번들은 파일 시스템의 디렉터리 계층 구조이기 때문에 번들에는 파일이 들어 있을 뿐이다. 따라서 다른 유형의 파일을 열 때와 동일한 파일 기반 인터페이스를 모두 사용하여 번들 리소스를 열 수 있다.
* 번들 디렉토리 구조를 통해 여러 지역화 리소스 지원도 용이하다. 새로운 지역화된 리소스를 쉽게 추가하거나 원하지 않는 리소스를 제거할 수 있다.
* 번들은 HFS, HFS+, AFP와 같은 여러 포크 형식과 UFS, SMB, NFS와 같은 단일 포크 형식을 포함한 많은 다른 형식의 볼륨에 상주할 수 있다.
* 사용자는 번들을 Finder에서 드래그하여 설치, 재배치 및 제거할 수 있다.
* 패키지로도 사용되는 번들
* 번들은 복수의 칩 아키텍처\(PowerPC, Intel\) 및 상이한 주소 공간 요구 사항\(32bit/64bit\)을 지원할 수 있다. 또한 전문화된 실행 파일\(예를 들어, 특정 벡터 명령 집합에 최적화된 라이브러리\)의 포함을 지원할 수 있다.
* 실행가능한 대부분의 코드를 번들로 묶을 수 있다. 애플리케이션, 프레임워크\(공유 라이브러리\) 및 플러그인은 모두 번들 모델을 지원한다. 정적 라이브러리, 동적 라이브러리, 셸 스크립트 및 UNIX 커맨드 라인 툴은 번들 구조를 사용하지 않는다.
* 번들 애플리케이션은 서버에서 직접 실행될 수 있다. 로컬 시스템에 특별한 공유 라이브러리, 확장 및 리소스를 설치할 필요는 없다.

### Types of Bundles

모든 번들이 동일한 기본 기능을 지원하지만, 의도하는 용도를 정의하는 번들을 정의하고 생성하는 방법에는 차이가 있다:

* **Application** - 애플리케이션 번들은 실행 가능한 프로세스와 관련된 코드와 리소스를 관리한다. 이 번들의 정확한 구조는 당신이 목표로 하는 플랫폼\(iOS 또는 macOS\)에 달려 있다. 애플리케이션 번들의 구조에 대한 자세한 내용은 [Application Bundles](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW13)을 참조하라.
* **Frameworks** - 프레임워크 번들은 동적 공유 라이브러리 및 헤더 파일과 같은 관련 리소스를 관리한다. 애플리케이션은 하나 이상의 프레임워크에 연결하여 포함된 코드를 이용할 수 있다. 프레임워크 번들의 구조에 대한 자세한 내용은 [Anatomy of a Framework Bundle](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW28)을 참조하라.
* **Plug-Ins** - macOS는 많은 시스템 기능에 대한 플러그인을 지원한다. 플러그인은 애플리케이션이 사용자 정의 코드 모듈을 동적으로 로드하는 방법이다. 다음 목록은 개발할 수 있는 몇 가지 주요 플러그인 유형을 보여준다:
  * **Custom plug-ins**는 자신의 목적을 위하여 정의하는 플러그인이다. [Anatomy of a Loadable Bundle](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW32)를 참조하라.
  * **Image Unit plug-ins**는 Core Image 기술에 사용자 정의 이미지 처리 동작을 추가한다. [_Image Unit Tutorial_](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/ImageUnitTutorial/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004531)를 참조하라.
  * **Interface Builder plug-ins**에는 인터페이스 빌더 라이브러리 창에 통합할 사용자 정의 객체가 포함되어 있다.
  * **Preference Pane plug-ins**는 시스템 환경설정 애플리케이션에 통합할 사용자 정의 환경설정을 정의한다. [_Preference Pane Programming Guide_](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/PreferencePanes/PreferencePanes.html#//apple_ref/doc/uid/10000110i)를 참조하라.
  * **Quartz Composer plug-ins**은 Quartz Composer 애플리케이션의 사용자 정의 패치를 정의한다. [_Quartz Composer Custom Patch Programming Guide_](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/QuartzComposer_Patch_PlugIn_ProgGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004787)를 참조하라.
  * **Quick Look plug-ins**은 Quick Look을 사용하여 사용자 정의 문서 유형을 표시하도록 지원된다. [_Quick Look Programming Guide_](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/Quicklook_Programming_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40005020)를 참조하라.
  * **Spotlight plug-ins**는 사용자 정의 문서 유형 인덱스를 지원하여 해당 문서를 사용자가 검색할 수 있도록 한다. [_Spotlight Importer Programming Guide_](https://developer.apple.com/library/archive/documentation/Carbon/Conceptual/MDImporters/MDImporters.html#//apple_ref/doc/uid/TP40001267)를 참조하라.
  * **WebKit plug-ins**는 일반적인 웹 브라우저에서 지원하는 콘텐츠 유형을 확장한다.
  * **Widgets**는 새로운 HTML 기반 애플리케이션을 대시보드에 추가한다.

문서 형식은 번들 구조를 활용하여 내용을 구성할 수 있지만, 일반적으로 가장 순수한 의미에서 문서는 번들로 간주되지 않는다. 디렉터리로 구현되어 불투명한 유형으로 취급되는 문서는 내부 형식에 관계없이 문서 패키지로 간주된다. 문서 패키지에 대한 자세한 내용은 [Document Packages](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/DocumentPackages/DocumentPackages.html#//apple_ref/doc/uid/10000123i-CH106-SW1)을 참조하라.

### Creating a Bundle

대부분의 경우, 번들 또는 패키지를 수동으로 생성하지 마라. Xcode 프로젝트를 생성하거나 기존 프로젝트에 대상을 추가하면 Xcode는 필요할 때 필요한 번들 구조를 자동으로 생성한다. 예를 들어 애플리케이션, 프레임워크 및 로드 가능한 번들 대상은 모두 연관된 번들 구조를 가지고 있다. 이러한 대상 중 하나를 만들면 Xcode가 자동으로 해당 번들을 생성한다.

> **참고**: 일부 Xcode 타겟\(셸 도구 정적 라이브러리 같은\)은 번들 또는 패키지를 생성하지 않는다. 이는 정상이며 이러한 대상 유형에 대해 특별히 번들을 만들 필요가 없다. 이러한 타겟에 대해 생성된 결과 바이너리들은 그대로 사용하도록 되어 있다.

파일 만들기\(Xcode 대신\)를 사용하여 프로젝트를 만들면 번들을 만드는 마법이 없다. 하나의 번들은 잘 정의된 구조와 번들 디렉터리 이름의 끝에 추가된 특정 파일 이름 확장자를 가진 파일 시스템의 디렉터리일 뿐이다. 최상위 번들 디렉터리를 만들고 번들 내용을 적절하게 구성하기만 하면 번들에 접근하기 위한 프로그램적인 지원 기능을 사용하여 해당 콘텐츠에 접근할 수 있다. 번들 디렉터리를 구성하는 방법에 대한 자세한 내용은 [Bundle Structures](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1)을 참조하라.

### Programmatic Support for Accessing Bundles

번들을 참조하거나 자체 번들로 제공되는 프로그램은 Cocoa 및 Core Foundation의 인터페이스를 활용하여 번들의 콘텐츠에 접근할 수 있다. 이러한 인터페이스를 사용하여 번들 리소스를 찾고 번들의 구성에 대한 정보를 얻으며 실행 가능한 코드를 로드할 수 있다. Objective-C 애플리케이션에서 [`NSBundle`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle) 클래스를 사용하여 번들 정보를 가져오고 관리한다. C 기반 애플리케이션의 경우 [`CFBundleRef`](https://developer.apple.com/documentation/corefoundation/cfbundle) 불투명 타입과 관련된 기능을 사용하여 번들을 관리할 수 있다.

> **참고**: 다른 많은 Core Foundation 및 Cocoa 타입과 달리, `NSBundle`과 `CFBundleRef`는 toll-free bridged 데이터 타입이 아니므로 서로 교환하여 사용할 수 없다. 그러나, 어느 객체에서든 번들 경로 정보를 추출하여 다른 객체를 만드는 데 사용할 수 있다.

번들에 접근하기 위해 Cocoa 및 Core Foundation 프로그래밍적인 지원 사용 방법에 대한 내용은 [Accessing a Bundle's Contents](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/AccessingaBundlesContents/AccessingaBundlesContents.html#//apple_ref/doc/uid/10000123i-CH104-SW1)을 참조하라.

### Guidelines for Using Bundles

번들은 macOS와 iOS에서 소프트웨어를 위한 선호되는 조직 메커니즘이다. 번들 구조를 사용하면 실행 가능한 코드와 해당 코드를 한 장소와 조직적으로 지원할 리소스를 그룹화할 수 있다. 다음 가이드라인은 번들을 사용하는 방법에 대한 몇 가지 추가 조언을 제공한다:

* 항상 번들에 정보-속성 목록\(`Info.plist`\) 파일을 포함하라. 번들 유형에 권장되는 키를 포함해야 한다. 이 파일에 포함할 수 있는 모든 키 목록은 [_Runtime Configuration Guidelines_](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPRuntimeConfig/000-Introduction/introduction.html#//apple_ref/doc/uid/10000170i)을 참조하라.
* 특정 리소스 파일 없이 애플리케이션을 실행할 수 없는 경우, 해당 파일을 애플리케이션 번들 안에 포함하라. 애플리케이션에는 항상 작동하는 데 필요한 모든 이미지, 문자열 파일, 로컬라이즈 가능한 리소스 및 플러그인이 포함되어야 한다. 비임계 리소스는 가능할 때마다 애플리케이션 번들 내에 유사하게 저장해야 하지만 필요한 경우 번들 밖에 배치할 수 있다. 번들 구조 프로그래밍에 대한 자세한 내용은 [Application Bundles](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW13)을 참조하라.
* C++ 코드를 로드할 계획인 경우, `extern "C"`로 로드할 기호를 표시할 수 있다. `NSBundle`이나 Core Foundation `CFBundleRef` 함수는 C++ 이름 mangling 컨벤션에 대해 알지 못하므로 기호를 이렇게 표시하면 나중에 식별하기가 훨씬 쉬워진다.
* `NSBundle` 클래스를 사용하여 코드 조각 관리자\(CFM\) 코드를 로드할 수 없다. CFM 기반 코드를 로드해야 하는 경우 [`CFBundleRef`](https://developer.apple.com/documentation/corefoundation/cfbundle) 또는 [`CFPlugInRef`](https://developer.apple.com/documentation/corefoundation/cfplugin) 불투명 타입의 함수를 사용해야 한다. 이 기술을 사용하여 Mach-O 실행 파일에서 CFM 기반 플러그인을 로드할 수 있다.
* 항상 [`NSBundle`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle) 클래스 \(`CFBundleRef` 불투명 타입과 관련된 함수와는 반대로\)를 사용하여 Java 코드가 포함된 번들을 로드 해야한다.
* Objective-C 코드가 포함된 번들을 로드할 때, `NSBundle` 클래스 또는 MacOS v10.5 이상에서 `CFBundleRef` 불투명 타입과 관련된 함수를 사용할 수 있다. 하지만 각각의 동작에는 차이가 있다. Core Foundation 함수를 사용하여 플러그인 또는 기타 로드 가능한 번들을 로드하는 경우\(프레임 또는 동적 공유 라이브러리와는 달리\), 함수는 개인적으로 번들을 적재하고 그 기호들을 즉시 바인딩한다. `NSBundle`을 사용하면 번들이 전체적으로 로드되고 기호가 느리게 바인딩된다. 또한 NSBundle 클래스를 사용하여 로드된 번들은 [`NSBundleDidLoadNotification`](https://developer.apple.com/documentation/foundation/bundle/1416137-didloadnotification) 노티피케이션의 생성을 일으키지만 Core Foundation 함수를 사용하여 로드된 번들은 그렇지 않다.

