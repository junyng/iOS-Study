---
description: 사용자 정의 워크플로우 및 고급 사용 사례를 지원하려면 Core ML API를 직접 사용하라.
---

# Core ML API

### Overview

대부분의 경우, Xcode 프로젝트에 모델을 추가할 때 Xcode에 의해 자동으로 생성되는 동적으로 생성된 인터페이스와만 상호 작용한다. 사용자 정의 워크플로우 또는 고급 사용 사례를 지원해야 하는 경우 Core ML API를 직접 사용할 수 있다. 예를 들어, 입력 데이터를 사용자 정의 구조체로 비동기적으로 수집하면서 예측해야 하는 경우 `MLFeatureProvider` 프로토콜을 채택할 수 있다.

Core ML API를 직접 사용하려면:

* 앱의 클래스 또는 구조체에서 [`MLFeatureProvider`](https://developer.apple.com/documentation/coreml/mlfeatureprovider) 프로토콜을 채택한다.
* `MLFeatureProvider`와 함께 [`MLModel`](https://developer.apple.com/documentation/coreml/mlmodel) 메서드를 사용하라.



### Topics

#### Machine Learning Model

* [`class MLModel`](https://developer.apple.com/documentation/coreml/mlmodel) 기계 학습 모델의 모든 세부 정보를 캡슐화.
* [Downloading and Compiling a Model on the User's Device](https://developer.apple.com/documentation/coreml/core_ml_api/downloading_and_compiling_a_model_on_the_user_s_device)  앱 설치 후 Core ML 모델을 사용자 기기에 배포한다.
* [Making Predictions with a Sequence of Inputs](https://developer.apple.com/documentation/coreml/core_ml_api/making_predictions_with_a_sequence_of_inputs) 반복 신경망 모델을 통합하여 입력 순서를 처리한다.

#### Model Features

* [`class MLFeatureValue`](https://developer.apple.com/documentation/coreml/mlfeaturevalue) 기능의 값과 유형은 읽기 전용 인스턴스로 번들된다.
* [`protocol MLFeatureProvider`](https://developer.apple.com/documentation/coreml/mlfeatureprovider) 모델의 입력 또는 출력에 대한 값의 집합을 나타내는 인터페이스.
* [`class MLDictionaryFeatureProvider`](https://developer.apple.com/documentation/coreml/mldictionaryfeatureprovider) 지정된 데이터 딕셔너리를 위한 편의 래퍼.
* [`protocol MLBatchProvider`](https://developer.apple.com/documentation/coreml/mlbatchprovider) 피처 제공자의 집합을 나타내는 인터페이스.
* [`class MLArrayBatchProvider`](https://developer.apple.com/documentation/coreml/mlarraybatchprovider) 피처 제공자 배치를 위한 편의 래퍼.

#### Model Updates

* [`class MLUpdateTask`](https://developer.apple.com/documentation/coreml/mlupdatetask) 추가 훈련 데이터로 모델을 업데이트하는 태스크.
* [Personalizing a Model with On-Device Updates](https://developer.apple.com/documentation/coreml/core_ml_api/personalizing_a_model_with_on-device_updates) 라벨 데이터로 업데이트 작업을 실행하여 업데이트 가능한 Core ML 모델을 수정한다. 

#### Customization

* [Integrating Custom Layers](https://developer.apple.com/documentation/coreml/core_ml_api/integrating_custom_layers) 사용자 정의 신경망 레이어를 Core ML 앱에 통합한다.
* [Creating a Custom Layer](https://developer.apple.com/documentation/coreml/core_ml_api/creating_a_custom_layer) Core ML 모델을 위한 사용자 정의 레이어를 만들어라.
* [`protocol MLCustomLayer`](https://developer.apple.com/documentation/coreml/mlcustomlayer) 신경망 모델에서 사용자 정의 레이어의 동작을 정의하는 인터페이스.
* [`protocol MLCustomModel`](https://developer.apple.com/documentation/coreml/mlcustommodel) 사용자 정의 모델의 동작을 정의하는 인터페이스.

#### Errors

* [`struct MLModelError`](https://developer.apple.com/documentation/coreml/mlmodelerror) Core ML 모델 에러에 대한 정보.

