# Document Packages

여러 가지 서로 다른 타입의 데이터 때문에 문서 파일 형식이 너무 복잡해져서 관리할 수 없는 경우 문서에 패키지 형식을 적용하는 것을 고려하라. 문서 패키지는 사용자에게 단일 문서라는 착각을 주지만 문서 데이터를 내부적으로 저장하는 방법에는 유연성을 제공한다. 특히 JPEG, GIF 또는 XML과 같은 여러 가지 유형의 표준 데이터 형식을 사용하는 경우 문서 패키지는 해당 데이터에 접근하고 관리하기가 훨씬 쉬워진다.

### Defining Your Document Directory Structure

애플은 문서 패키지에 대해 어떤 특정한 구조도 규정하지 않는다. 그러나 플랫 디렉터리 구조를 만들거나 최상위 `Resources` 하위 디렉터리에 파일을 배치하는 프레임워크 번들 구조를 사용하는 것이다. \(프레임워크의 번들 구조에 대한 자세한 내용은 [Framework Bundles](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW26)을 참조하라.\)

### Registering Your Document Type

문서를 패키지로 등록하려면 애플리케이션 정보 속성 목록\(`Info.plist`\) 파일에서 문서 유형 정보를 수정해야 한다. `CFBundleDocumentTypes` 키는 애플리케이션이 지원하는 문서 유형에 대한 정보를 저장한다. 각 문서 패키지 유형에 대해 적절한 값을 가진 키를 `LSTypeIsPackage` 포함한다. 이 키가 있으면 Finder 및 Launch Services가 지정된 파일 확장명을 가진 디렉터리를 패키지로 처리하도록 지시한다. `Info.plist`키에 대한 자세한 내용은 [_Information Property List Key Reference_](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009247)를 참조하라.

문서 패키지는 사용자가 숨길 수 있는 확장자를 항상 가지고 있어야 한다. 확장을 통해 파인더는 문서 디렉터리를 식별하고 패키지로 취급할 수 있다. 문서 패키지를 MIME 타입 또는 4바이트 OS 유형과 유형과 연관시켜서는 안된다.

### Creating a New Document Package

애플리케이션이 새 문서를 작성할 때 다음 두 가지 방법 중 하나를 사용하여 작성할 수 있다:

* [`NSFileWrapper`](https://developer.apple.com/documentation/foundation/filewrapper) 객체를 사용하여 문서 패키지를 작성하라.
* 수동으로 문서 패키지를 생성하라.

Cocoa 애플리케이션을 만드는 경우 NSFileWrapper 클래스는 문서 내용을 읽고 쓰기 위해 [`NSDocument`](https://developer.apple.com/documentation/appkit/nsdocument)의 기존 지원과 연결되기 때문에 문서 패키지를 만들 때 선호되는 방법이다. \(`NSFileWrapper` 클래스는 iOS 4.0 이상에서도 Foundation 프레임워크의 일부로 사용할 수 있다.\) 이 클래스의 사용 방법에 대한 자세한 내용은 [_NSFileWrapper Class Reference_](https://developer.apple.com/documentation/foundation/nsfilewrapper)를 참조하라.

상위 수준의 Objective-C 프레임워크\(AppKit 또는 UIKit 같은\)를 사용하지 않는 커맨드라인 또는 기타 유형의 애플리케이션을 작성하는 경우에도 문서 패키지를 수동으로 생성할 수있다. 문서 패키지를 만들 때 기억해야 할 중요한 것은 그것이 단지 디렉터리라는 것이다. 문서 패키지의 유형이 등록되는 한\([Registering Your Document Type](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/DocumentPackages/DocumentPackages.html#//apple_ref/doc/uid/10000123i-CH106-SW3)에 설명된 것과 같이\), 적절한 파일 이름 확장자로 디렉터리를 작성하기만 하면 된다. \(Finder는 디렉터리를 패키지로 취급하기 위해 파일 이름 확장자를 큐로 사용한다.\) 표준 BSD 파일 시스템 루틴을 사용하거나 Foundation 프레임워크에서 [`NSFileManager`](https://developer.apple.com/documentation/foundation/filemanager) 클래스를 사용하여 디렉터리를 만들 수 있다.\(그리고 디렉터리에 넣을 파일을 만들 수 있다.\)

### Accessing Your Document Contents

문서 패키지의 내용에 접근하는 방법은 여러 가지가 있다. 문서 패키지는 디렉터리이기 때문에 적절한 파일 시스템 루틴을 사용하여 문서의 내용에 접근할 수 있다. 문서 패키지에 번들 구조를 사용하는 경우 [`NSBundle`](https://developer.apple.com/library/archive/documentation/LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle) 또는 [`CFBundleRef`](https://developer.apple.com/documentation/corefoundation/cfbundle) 루틴도 사용할 수 있다. 번들 구조의 사용은 여러 로컬라이제이션을 저장하는 문서에 특히 적합하다.

문서 패키지가 플랫 디렉터리 구조를 사용하거나 고정된 내용 파일 집합을 포함하는 경우 `NSBundle` 또는 `CFBundleRef`를 사용하는 것보다 파일 시스템 루틴을 빠르고 쉽게 사용할 수 있다. 그러나 문서의 내용이 변동될 수 있는 경우 문서 내부의 파일의 동적 발견을 단순화하기 위해 번들 구조와 `NSBundle` 또는 `CFBundleRef`를 사용하는 것을 고려해야 한다.

Cocoa 애플리케이션을 작성하는 경우 NSDocument 하위 클래스가 문서 패키지의 내용을 로드하는 방식을 사용자 정의해야 한다. [`readFromData:ofType:error:`](https://developer.apple.com/documentation/appkit/nsdocument/1515198-readfromdata) 및 [`dataOfType:error:`](https://developer.apple.com/documentation/appkit/nsdocument/1515205-data) 데이터를 읽고 쓰는 메서드는 단일 파일 문서를 위한 것이다. 문서 패키지를 처리하려면, [`readFromFileWrapper:ofType:error:`](https://developer.apple.com/documentation/appkit/nsdocument/1515044-readfromfilewrapper) 와 [`fileWrapperOfType:error:`](https://developer.apple.com/documentation/appkit/nsdocument/1515089-filewrapperoftype) 메서드를 대신 사용해야 한다. NSDocument 하위 클래스에서 문서 데이터를 읽고 쓰는 메서드에 대한 내용은 _Document-Based Applications Overview_를 참조하라.

