# Basics of Using Image I/O

Image I/O 프레임워크는 원본 \([`CGImageSourceRef`](https://developer.apple.com/documentation/imageio/cgimagesource)\) 에서 이미지 데이터를 읽고 대상 \([`CGImageDestinationRef`](https://developer.apple.com/documentation/imageio/cgimagedestinationref)\) 에 이미지 데이터를 쓰기 위한 불투명 데이터 타입을 제공한다. 표준 웹 포멧, 높은 동적 범위 이미지, 원시 카메라 데이터 등 다양한 이미지 포멧을 지원한다. Image I/O에는 다음과 같은 많은 다른 기능이 있다:

* Mac 플랫폼을 위한 가장 빠른 이미지 디코더 및 인코더
* 이미지를 점진적으로 로드하는 기능
* 이미지 메타데이터 지원
* 효율적인 캐싱

다음에서 이미지 원본 및 이미지 대상 객체를 작성할 수 있다:

* URLs. URL로 위치를 지정할 수 있는 이미지는 이미지 데이터의 공급자 또는 수신자 역할을 할 수 있다. Image I/O에서 URL은 Core Foundation 데이터 타입 [`CFURLRef`](https://developer.apple.com/documentation/corefoundation/cfurl)로 표시된다.
* Core Foundation 객체 [`CFDataRef`](https://developer.apple.com/documentation/corefoundation/cfdata) 및 [`CFMutableDataRef`](https://developer.apple.com/documentation/corefoundation/cfmutabledataref)\`\`
* Quartz 데이터 소비자 \([`CGDataConsumerRef`](https://developer.apple.com/documentation/coregraphics/cgdataconsumer)\) 및 데이터 공급자 \([`CGDataProviderRef`](https://developer.apple.com/documentation/coregraphics/cgdataprovider)\) 객체

### Using the Image I/O Framework in Your Application

Image I/O는 OS X의 Application Services 프레임워크와 iOS의 I/O 프레임워크에 위치한다. 프로그램에 프레임워크를 추가한 후 다음 문장을 포함하여 헤더 파일을 가져와라:

`#import <ImageIO/ImageIO.h>`  


### Supported Image Formats

Image I/O 프레임워크는 `JPEG`, `JPEG2000`, `RAW`, `TIFF`, `BMP`, `PNG`와 같은 대부분의 일반적인 이미지 파일 형식을 이해한다. 각 플랫폼에서 모든 형식이 지원되는 것은 아니다. Image I/O가 지원하는 최신 목록을 보려면 다음 함수를 호출하라:

* [`CGImageSourceCopyTypeIdentifiers`](https://developer.apple.com/documentation/imageio/1465383-cgimagesourcecopytypeidentifiers)는 Image I/O가 이미지 소스로 지원하는 균일한 유형 식별자\(UTI\)의 배열을 반환한다.
* [`CGImageDestinationCopyTypeIdentifiers`](https://developer.apple.com/documentation/imageio/1465316-cgimagedestinationcopytypeidenti)는 Image I/O가 지원하는 균일한 유형 식별자\(UTI\)의 배열을 이미지 대상으로 반환한다.

그런 다음 Listing 1-1에 표시된 것처럼 [`CFShow`](https://developer.apple.com/documentation/corefoundation/1541433-cfshow) 함수를 사용하여 Xcode의 디버거 콘솔로 배열을 출력할 수 있다. 이러한 함수에 의해 반환되는 배열의 문자열은 `com.apple.pict`, `public.jpeg`, `public.tiff` 등의 형태를 띈다. [Table 1-1](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/ImageIOGuide/imageio_basics/ikpg_basics.html#//apple_ref/doc/uid/TP40005462-CH216-SW3)은 많은 공통 이미지 파일 형식에 대한 `UTIs`를 나열한다. OS X 및 iOS는 대부분의 일반적인 이미지 파일 형식에 대한 상수를 정의한다. 상수의 전체 집합은 `UTCoreTypes.h` 헤더 파일에서 선언된다. 이미지 유형을 지정해야 할 때 이미지 소스\(`kCGImageSourceTypeIdentifierHint`\)의 힌트로 또는 이미지 대상의 이미지 유형으로 사용할 수 있다.

**Listing 1-1**  지원되는 UTIs 가져오기 및 출력하기

```text
CFArrayRef mySourceTypes = CGImageSourceCopyTypeIdentifiers();
CFShow(mySourceTypes);
CFArrayRef myDestinationTypes = CGImageDestinationCopyTypeIdentifiers();
CFShow(myDestinationTypes);
```

**Table 1-1** 공통 통일 유형 식별자 \(UTIs\) 및 이미지 콘텐츠 유형 상수

| Uniform type identifier | Image content type constant |
| :--- | :--- |
| public.image | `kUTTypeImage` |
| public.png | `kUTTypePNG` |
| public.jpeg | `kUTTypeJPEG` |
| public.jpeg-2000 \(OS X only\) | `kUTTypeJPEG2000` |
| public.tiff | `kUTTypeTIFF` |
| com.apple.pict \(OS X only\) | `kUTTypePICT` |
| com.compuserve.gif | `kUTTypeGIF` |

