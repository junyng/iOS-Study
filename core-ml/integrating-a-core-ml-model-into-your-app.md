---
description: 앱에 간단한 모델을 추가하고 입력 데이터를 모델에 전달하고 모델의 예측을 처리한다.
---

# Integrating a Core ML Model into Your App

### Overview

이 샘플 앱은 화성의 서식지 가격을 예측하기 위해 훈련된 모델인 `MarsHabitatPricer.mlmodel`을 사용한다.

#### Add a Model to Your Xcode Project <a id="3570591"></a>

모델을 프로젝트 네비게이터로 끌어 Xcode 프로젝트에 추가하라.

모델 유형 및 모델의 예상 입력 및 출력을 포함한 모델에 대한 정보를 Xcode에서 모델을 열면 볼 수 있다. 이 표본에서 입력은 태양 전지판과 온실의 수와 서식지의 로트 크기\(에이커 단위\)이다. 출력은 서식지의 예상 가격이다.

#### Create the Model in Code <a id="3570592"></a>

Xcode는 또한 모델의 입력과 출력에 대한 정보를 사용하여 모델에 대한 사용자 정의 프로그래밍 인터페이스를 자동으로 생성하며, 이 인터페이스를 사용하여 코드의 모델과 상호 작용한다. `MarsHabitatPricer.mlmodel`의 경우, Xcode는 모델\(`MarsHabitatPricer`\), 모델의 입력 \(`MarsHabitatPricerInput`\), 모델의 출력\(`MarsHabitatPricerOutput`\)을 나타내는 인터페이스를 생성한다.

생성된 `MarsHabitatPricer` 클래스의 이니셜라이저를 사용하여 모델을 생성하라:

```swift
let model = MarsHabitatPricer()
```

#### Get Input Values to Pass to the Model <a id="3570593"></a>

이 샘플 앱은 UIPickerView를 사용하여 사용자로부터 모델의 입력 값을 얻는다:

```swift
func selectedRow(for feature: Feature) -> Int {
    return pickerView.selectedRow(inComponent: feature.rawValue)
}

let solarPanels = pickerDataSource.value(for: selectedRow(for: .solarPanels), feature: .solarPanels)
let greenhouses = pickerDataSource.value(for: selectedRow(for: .greenhouses), feature: .greenhouses)
let size = pickerDataSource.value(for: selectedRow(for: .size), feature: .size)
```

#### Use the Model to Make Predictions <a id="3570594"></a>

`MarsHabitatPricer` 클래스에는 모델의 입력 값\(이 경우, 태양 전지판의 수, 온실의 수, 서식지의 크기\) 에서 가격을 예측하는 데 사용되는 메서드를 가지고 있다. 이 메서드의 결과는 `MarsHabitatPricerOutput` 인스턴스이다.

```swift
guard let marsHabitatPricerOutput = try? model.prediction(solarPanels: solarPanels, greenhouses: greenhouses, size: size) else {
    fatalError("Unexpected runtime error.")
}
```

`marsHabitatPricerOutput`의 `price` 속성에 접근하여 예상 가격을 얻고 결과를 앱의 UI에 표시하라.

```swift
let price = marsHabitatPricerOutput.price
priceLabel.text = priceFormatter.string(for: price)
```

> **Note**
>
> 생성된 `prediction(solarPanels:greenhouses:size:)` 메서드는 에러를 throw 할 수 있다. Core ML로 작업할 때 가장 일반적인 유형의 에러는 입력 데이터의 세부 정보가 모델이 예상하는 세부 정보와 일치하지 않을 때\(예: 잘못된 형식의 이미지\) 발생한다.

#### Build and Run a Core ML App <a id="3570595"></a>

Xcode는 Core ML 모델을 기기에서 실행하도록 최적화된 리소스로 컴파일한다. 이 모델의 최적화된 표현은 앱 번들에 포함되어 있으며, 앱이 장치에서 실행되는 동안 예측하는 데 사용되는 것이다.

### See Also

#### First Steps

* [Getting a Core ML Model](https://developer.apple.com/documentation/coreml/getting_a_core_ml_model) 앱에서 사용할 Core ML 모델을 얻는다.
* [Converting Trained Models to Core ML](https://developer.apple.com/documentation/coreml/converting_trained_models_to_core_ml) 써드파티 머신 러닝 도구로 만든 훈련된 모델을 Core ML 모델 포멧으로 변환하라.

