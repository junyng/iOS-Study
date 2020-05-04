# Bundle Programming Guide

## Introduction <a id="pageTitle"></a>

번들은 코드와 리소스를 캡슐화하는 데 사용되는 macOS와 iOS의 기본 기술이다. 번들은 복합 바이너리 파일을 만들 필요성을 완화하면서 필요한 리소스에 대해 알려진 위치를 제공하며 개발자 경험을 단순화한다. 대신 번들은 디렉토리와 파일을 사용하여 개발 중이나 배포 후 모두 쉽게 수정할 수 있는 좀 더 자연스러운 유형의 구성을 제공한다.

번들을 지원하기 위해 Cocoa와 Core Foundation은 번들의 컨텐츠에 접근하기 위한 프로그래밍 인터페이스를 제공한다. 번들은 체계적인 구조를 사용하기 때문에 모든 개발자가 번들의 근본적인 구성 원리를 이해하는 것이 중요하다. 이 문서는 번들의 작동 방식과 개발 중에 번들을 사용하여 리소스 파일에 접근하는 방법을 이해할 수 있는 기초를 제공한다.

### Organization of This Document

이 문서에는 다음 챕터가 수록되어 있다:

* [About Bundles](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/AboutBundles/AboutBundles.html#//apple_ref/doc/uid/10000123i-CH100-SW1) 번들과 패키지의 개념과 그것들이 시스템에 의해 어떻게 사용되는지를 소개한다.
* [Bundle Structures](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1) 표준 번들 유형의 구조와 내용을 기술한다.
* [Accessing a Bundle's Contents](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/AccessingaBundlesContents/AccessingaBundlesContents.html#//apple_ref/doc/uid/10000123i-CH104-SW1) Cocoa 및 Core Foundation 인터페이스를 사용하여 번들과 그 내용에 대한 정보를 얻는 방법을 보여준다.
* [Document Packages](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/DocumentPackages/DocumentPackages.html#//apple_ref/doc/uid/10000123i-CH106-SW1) 문서 패키지의 개념\(번들과 느슨하게 관련되어 있음\)과 사용 방법을 설명한다.

### See Also

이 문서의 정보는 모든 유형의 번들에 적용되지만, 보다 전문화된 유형의 번들\(예: 프레임워크 및 플러그인\)으로 작업할 경우 다음 문서도 참조해야 한다.

* [_Framework Programming Guide_](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPFrameworks/Frameworks.html#//apple_ref/doc/uid/10000183i) 사용자 정의 프레임워크 생성 및 사용에 대한 자세한 정보를 제공한다.
* [_Code Loading Programming Topics_](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/LoadingCode/LoadingCode.html#//apple_ref/doc/uid/10000052i) Objective-C를 사용한 플러그인 작성에 대한 정보를 제공한다.
* [_Plug-in Programming Topics_](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFPlugIns/CFPlugIns.html#//apple_ref/doc/uid/10000128i) C 언어를 사용한 플러그인 작성에 대한 정보를 제공하낟.



