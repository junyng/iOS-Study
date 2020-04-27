---
description: 앱에서 사용할 Core ML 모델을 얻는다.
---

# Getting a Core ML Model

### Overview

Core ML은 신경망, 트리 앙상블, 서포트 벡터 머신, 일반화 선형 모형 등 다양한 머신러닝 모델을 지원한다. Core ML은 Core ML 모델 포멧\(.mlmodel 파일 확장자를 가진 모델\)을 필요로 한다.

[Create ML](https://developer.apple.com/documentation/createml) 및 자체 데이터를 사용하여 이미지를 인식하거나 텍스트에서 의미를 추출하거나 숫자 값 간의 관계를 찾는 등의 작업을 수행할 수 있는 사용자 정의 모델을 훈련할 수 있다. 을 사용하여 훈련된 모델은 Core ML 모델 포멧이며 앱에서 사용할 준비가 되어 있다.

애플은 또한 이미 Core ML 모델 포멧으로 되어 있는 몇 가지 인기 있는 오픈 소스 [models](https://developer.apple.com/machine-learning/models/)을 제공한다. 이 모델을 다운로드하여 앱에서 사용하기 시작할 수 있다. 또한, 다양한 연구 그룹과 대학들은 그들의 모델과 훈련 데이터를 발표하는데, 이것은 Core ML 모델 포멧이 아닐 수도 있다. 이러한 모델을 앱에서 사용하려면 [Converting Trained Models to Core ML](https://developer.apple.com/documentation/coreml/converting_trained_models_to_core_ml)에 설명된 바와 같이 변환해야 한다.



### See Also

#### First Steps

* [Integrating a Core ML Model into Your App](https://developer.apple.com/documentation/coreml/integrating_a_core_ml_model_into_your_app) 앱에 간단한 모델을 추가하고 입력 데이터를 모델에 전달하고 모델의 예측을 처리한다.
* [Converting Trained Models to Core ML](https://developer.apple.com/documentation/coreml/converting_trained_models_to_core_ml) 써드파티 머신러닝 도구로 작성된 훈련된 모델을 Core ML 모델 포멧으로 변환한다.

