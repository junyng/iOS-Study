---
description: 써드 파티 머신 러닝 도구로 만든 훈련된 모델을 Core ML 모델 포멧으로 변환하라.
---

# Converting Trained Models to Core ML

### Overview

지원되는 써드-파티 머신 러닝 프레임워크를 사용하여 모델을 만들고 훈련하는 경우, [MXNet converter](https://github.com/apache/incubator-mxnet/tree/master/tools/coreml) 또는 [TensorFlow converter](https://github.com/tf-coreml/tf-coreml) 등 Core ML 도구 또는 타사 변환 도구를 사용하여 모델을 Core ML 모델 포멧으로 변환할 수 있다. 그렇지 않으면, 변환 도구를 직접 만들어야 한다.

#### Use Core ML Tools <a id="2953556"></a>

[Core ML Tools](https://pypi.python.org/pypi/coremltools)은 다양한 모델 유형을 Core ML 모델 포멧으로 변환하는 파이썬 패키지이다. [목록 1](https://developer.apple.com/documentation/coreml/converting_trained_models_to_core_ml#2953557)은 지원되는 모델 및 써드-파티 프레임워크가 나열되어 있다.

**목록 1** Core ML 도구가 지원하는 모델 및 타사 프레임워크

<table>
  <thead>
    <tr>
      <th style="text-align:left">Model type</th>
      <th style="text-align:left">Supported models</th>
      <th style="text-align:left">Supported frameworks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Neural networks</td>
      <td style="text-align:left">Feedforward, convolutional, recurrent</td>
      <td style="text-align:left">
        <p>Caffe v1</p>
        <p>Keras 1.2.2+</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Tree ensembles</td>
      <td style="text-align:left">Random forests, boosted trees, decision trees</td>
      <td style="text-align:left">
        <p>scikit-learn 0.18</p>
        <p>XGBoost 0.6</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Support vector machines</td>
      <td style="text-align:left">Scalar regression, multiclass classification</td>
      <td style="text-align:left">
        <p>scikit-learn 0.18</p>
        <p>LIBSVM 3.22</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Generalized linear models</td>
      <td style="text-align:left">Linear regression, logistic regression</td>
      <td style="text-align:left">scikit-learn 0.18</td>
    </tr>
    <tr>
      <td style="text-align:left">Feature engineering</td>
      <td style="text-align:left">Sparse vectorization, dense vectorization, categorical processing</td>
      <td
      style="text-align:left">scikit-learn 0.18</td>
    </tr>
    <tr>
      <td style="text-align:left">Pipeline models</td>
      <td style="text-align:left">Sequentially chained models</td>
      <td style="text-align:left">scikit-learn 0.18</td>
    </tr>
  </tbody>
</table>

#### Convert Your Model <a id="2891985"></a>

모델의 타사 프레임워크에 해당하는 Core ML 변환기를 사용하여 모델을 변환하라. 변환기의  메서드를 호출하고 결과 모델을 Core ML 모델 포멧\(.mlmodel\)에 저장한다.

예를 들어, Caffe를 사용하여 모델을 만든 경우 Caffe 모델\(`.caffemodel`\)을 `coremltools.converters.caffe.convert` 메서드로 전달하라.

```text
import coremltools
coreml_model = coremltools.converters.caffe.convert('my_caffe_model.caffemodel')
```

이제 결과 모델을 Core ML 모델 포멧으로 저장하라.

```text
coremltools.utils.save_spec(coreml_model, 'my_model.mlmodel')
```

모델에 따라 입력, 출력 및 레이블을 업데이트하거나 이미지 이름, 유형 및 포멧을 선언해야 할 수도 있다. 변환 도구는 사용 가능한 옵션이 도구별로 다르기 때문에 더 많은 문서와 함께 꾸려진다. Core ML 도구에 대한 자세한 정보는 [Package Documentation](https://apple.github.io/coremltools/)를 참조하라.

#### Alternatively, Write a Custom Conversion Tool <a id="2903105"></a>

위에 나열된 도구에서 지원되는 포멧이 아닌 모델을 변환해야 할 때 자체 변환 도구를 만들 수 있다.

자신의 변환 도구를 작성하는 것은 모델의 입력, 출력 및 아키텍처 표현을 Core ML 모델 포멧으로 변환하는 것을 포함한다. 모델 아키텍처의 각 계층과 다른 계층과의 연결을 정의하여 이 작업을 수행하라. [Core ML Tools](https://pypi.python.org/pypi/coremltools)에서 제공하는 변환 도구를 예로 사용하라; 이들은 타사 프레임워크에서 생성된 다양한 모델 유형이 Core ML 모델 포멧으로 변환되는 방법을 설명한다.

> **Note**
>
> Core ML 모델 포멧은 일련의 프로토콜 버퍼 파일로 정의되며, [Core ML Model Specification](https://apple.github.io/coremltools/coremlspecification/)에 자세히 설명되어 있다.

### 

### See Also

#### First Steps

* [Getting a Core ML Model](https://developer.apple.com/documentation/coreml/getting_a_core_ml_model) 앱에서 사용할 Core ML 모델을 얻어라.
* [Integrating a Core ML Model into Your App](https://developer.apple.com/documentation/coreml/integrating_a_core_ml_model_into_your_app) 앱에 간단한 모델을 추가하고 입력 데이터를 모델에 전달하고 모델의 예측을 처리한다.

