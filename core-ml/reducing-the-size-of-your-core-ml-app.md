---
description: 애플리케이션 번들 내의 Core ML 모델에 사용되는 스토리지를 줄여라.
---

# Reducing the Size of Your Core ML App

### Overview

앱에서 기계 학습 모델을 번들링하는 것은 Core ML로 시작하는 가장 쉬운 방법이다. 모델이 더욱 발전함에 따라, 그것들은 더 커져서 상당한 저장 공간을 차지할 수 있다. 신경망 기반 모델의 경우, 무게 파라미터에 대한 낮은 정밀도 표현을 사용하여 footprint를 줄이는 것을 고려하라. 모델이 반 정밀도를 사용할 수 있는 신경망이 아니거나 앱의 크기를 더 줄여야 할 경우, 앱으로 모델을 번들링하는 대신 사용자의 장치에 모델을 다운로드하고 컴파일하기 위한 기능을 추가하라.

#### Convert to a Half-Precision Model <a id="2936383"></a>

[Core ML Tools](https://apple.github.io/coremltools/index.html)는 신경망 모델의 부동 소수점 가중치를 전체 정밀도에서 반 정밀도 값으로 변환하는 변환 기능을 제공한다 \(표현에 사용되는 비트 수를 32개에서 16개로 줄임\). 이러한 변환 유형은 네트워크의 크기를 현저히 줄일 수 있으며, 대부분 네트워크 내의 연결 가중치에서 발생한다.

**Listing 1** Core ML Tools를 사용하여 모델을 낮은 정확도로 변환

```text
# Load a model, lower its precision, and then save the smaller model.
model_spec = coremltools.utils.load_spec('./exampleModel.mlmodel')
model_fp16_spec = coremltools.utils.convert_neural_network_spec_weights_to_fp16(model_spec)
coremltools.utils.save_spec(model_fp16_spec, 'exampleModelFP16.mlmodel')
```

신경망을 내장한 신경망이나 파이프라인 모델을 반 정확도로만 변환할 수 있다. 모델의 모든 완전한 정밀도 weight 파라미터는 반정밀도로 변환되어야 한다.

반정밀 부동 소수점 값을 사용하면 부동 소수점 값의 정확도가 떨어질 뿐만 아니라 가능한 값의 범위도 줄어든다. 이 옵션을 사용자에게 배포하기 전에 모델의 동작이 저하되지 않았는지 확인하라.

반 정밀도로 변환된 모델은 iOS 11.2, macOS 10.13.2, tvOS 11.2 또는 watchOS 4.2 이상의 OS 버전을 필요로 한다.

#### Convert to a Lower Precision Model <a id="3030045"></a>

Core ML Tools 2.0은 모델의 정확도를 8, 4, 2, 1비트로 낮추기 위해 새로운 유틸리티를 도입했다. 도구에는 원래 모델과 낮은 정확도 모델 간의 동작 차이를 측정하는 함수가 포함되어 있다. 이러한 유틸리티 사용에 대한 자세한 내용은 [Core ML Tools Neural Network Quantization](https://apple.github.io/coremltools/generated/coremltools.models.neural_network.quantization_utils.html) 문서를 참조하라.

낮은 정확도 모델은 iOS 12, macOS 10.14, tvOS 12 또는 watchOS 5 이상의 OS 버전을 필요로 한다.

#### Download and Compile a Model <a id="2936384"></a>

앱 크기를 줄이는 또 다른 옵션은 앱이 모델을 사용자의 장치에 다운로드하여 백그라운드에서 컴파일하도록 하는 것이다. 예를 들어, 사용자가 앱이 지원하는 모델의 일부만 사용하는 경우 가능한 모든 모델을 앱과 함께 번들화할 필요는 없다. 대신, 모델은 사용자 행동에 따라 나중에 다운로드할 수 있다. [Downloading and Compiling a Model on the User's Device](https://developer.apple.com/documentation/coreml/core_ml_api/downloading_and_compiling_a_model_on_the_user_s_device)를 참조하라.

