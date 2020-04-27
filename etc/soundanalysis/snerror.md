---
description: SoundAnalysis 프레임워크에서 발생한 에러.
---

# SNError

### Declaration

```swift
struct SNError
```

### Topics

#### Error Domain

* [`let SNErrorDomain: String`](https://developer.apple.com/documentation/soundanalysis/snerrordomain) SoundAnalysis 에러 도메인을 식별하는 문자열.
* [`static var errorDomain: String`](https://developer.apple.com/documentation/soundanalysis/snerror/3213744-errordomain) SoundAnalysis 에러 도메인을 식별하는 정적 접근 가능한 문자열.

#### Error Information

* [`var errorCode: Int`](https://developer.apple.com/documentation/soundanalysis/snerror/3213743-errorcode)  에러에 대한 코드.
* [`var errorUserInfo: [String : Any]`](https://developer.apple.com/documentation/soundanalysis/snerror/3213745-erroruserinfo) 에러에 대한 정보를 포함하는 딕셔너리.
* [`var localizedDescription: String`](https://developer.apple.com/documentation/soundanalysis/snerror/3213748-localizeddescription) 로컬라이즈되고 사람이 읽을 수 있는 에러에 대한 설명.

#### Errors

* [`enum SNError.Code`](https://developer.apple.com/documentation/soundanalysis/snerror/code) SoundAnalysis 프레임워크에서 생성된 열거형 에러 코드.
* [`static var invalidFormat: SNError.Code`](https://developer.apple.com/documentation/soundanalysis/snerror/3213746-invalidformat) 잘못된 오디오 형식을 나타내는 에러 코드.
* [`static var invalidModel: SNError.Code`](https://developer.apple.com/documentation/soundanalysis/snerror/3213747-invalidmodel) 잘못된 컴퓨터 학습 모델을 나타내는 에러 코드.
* [`static var invalidFile: SNError.Code`](https://developer.apple.com/documentation/soundanalysis/snerror/3362958-invalidfile) 잘못된 파일을 나타내는 에러 코드.
* [`static var operationFailed: SNError.Code`](https://developer.apple.com/documentation/soundanalysis/snerror/3213749-operationfailed) 분석 작업이 실패했음을 나타내는 에러 코드.
* [`static var unknownError: SNError.Code`](https://developer.apple.com/documentation/soundanalysis/snerror/3213750-unknownerror) 알 수 없는 에러가 발생함을 나타내는 에러 코드.

#### Operator Functions

* [`static func != (SNError, SNError) -> Bool`](https://developer.apple.com/documentation/soundanalysis/snerror/3213742) 두 개의 에러가 같지 않은지 여부를 나타내는 불 값을 반환한다.

