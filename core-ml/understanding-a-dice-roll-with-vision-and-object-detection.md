---
description: 카메라 프레임에 표시된 주사위 위치 및 값을 검출하고 주사위 검출 모델을 활용하여 롤의 끝을 결정한다.
---

# Understanding a Dice Roll with Vision and Object Detection

### Overview

이 샘플 앱은 [Create ML](https://developer.apple.com/documentation/createml)로 훈련된 물체 감지 모델을 사용하여 주사위가 평평한 표면으로 굴러갈 때 주사위 상단과 그 값을 인식한다. 

[Vision](https://developer.apple.com/documentation/vision)을 통해 카메라 프레임에서 객체 탐지 모델을 실행한 후, 모델은 롤이 종료된 시점과 주사위 값이 표시되는 값을 식별하기 위해 결과를 해석한다.

> Note
>
> 샘플 코드 프로젝트는 [228: Creating Great Apps Using Core ML and ARKit](https://developer.apple.com/videos/play/wwdc19/228/)과 관련이 있다.

#### Configure the Sample Code Project <a id="3375268"></a>

Xcode에서 샘플 코드 프로젝트를 실행하기 전에 다음 사항에 유의하라:

* iOS 13 이상을 사용하는 물리적 장치에서 이 샘플 코드 프로젝트를 실행해야 한다. 그 프로젝트는 시뮬레이터와는 작동하지 않는다.
* 그 모델은 흰 주사위, 검정색 주사위를 가장 잘 다룬다. 그것은 다른 색을 사용하는 주사위에서는 다르게 수행될 수 있다.

#### Add Inputs to the Request <a id="3375269"></a>

Vision에서 iOS 13부터 [`MLFeatureProvider`](https://developer.apple.com/documentation/coreml/mlfeatureprovider) 객체를 모델에 부착하여 영상 이외의 입력을 모델에 제공할 수 있다. 이것은 기본값과 다른 임계값을 지정하고자 할 때 객체 탐지에 유용하다.

아래와 같이 피처 제공자는 객체 탐지 모델에 `iouThreshold` 및 `confidenceThreshold`에 대한 값을 제공할 수 있다.

```swift
class ThresholdProvider: MLFeatureProvider {
    /// The actual values to provide as input
    ///
    /// Create ML Defaults are 0.45 for IOU and 0.25 for confidence.
    /// Here the IOU threshold is relaxed a little bit because there are
    /// sometimes multiple overlapping boxes per die.
    /// Technically, relaxing the IOU threshold means
    /// non-maximum-suppression (NMS) becomes stricter (fewer boxes are shown).
    /// The confidence threshold can also be relaxed slightly because
    /// objects look very consistent and are easily detected on a homogeneous
    /// background.
    open var values = [
        "iouThreshold": MLFeatureValue(double: 0.3),
        "confidenceThreshold": MLFeatureValue(double: 0.2)
    ]

    /// The feature names the provider has, per the MLFeatureProvider protocol
    var featureNames: Set<String> {
        return Set(values.keys)
    }

    /// The actual values for the features the provider can provide
    func featureValue(for featureName: String) -> MLFeatureValue? {
        return values[featureName]
    }
}
```

이 임계값 제공자를 [`VNCoreMLModel`](https://developer.apple.com/documentation/vision/vncoremlmodel)와 함께 사용하려면 다음 예에서 보는 대로 [`VNCoreMLModel`](https://developer.apple.com/documentation/vision/vncoremlmodel)의 속성에 [`featureProvider`](https://developer.apple.com/documentation/vision/vncoremlmodel/3131933-featureprovider) 할당하라.

#### Set Up a Vision Request to Handle Camera Frames <a id="3375270"></a>

단순화를 위해 [`ARSession`](https://developer.apple.com/documentation/arkit/arsession)에 나오는 카메라 프레임을 사용할 수 있다.

이러한 프레임에 디텍터를 실행하려면 아래 예제와 같이 먼저 모델과 함께 [`VNCoreMLRequest`](https://developer.apple.com/documentation/vision/vncoremlrequest) 요청을 설정하라.

```swift
guard let detector = try? VNCoreMLModel(for: DiceDetector().model) else {
    print("Failed to load detector!")
    return
}

// Use a threshold provider to specify custom thresholds for the object detector.
detector.featureProvider = ThresholdProvider()

diceDetectionRequest = VNCoreMLRequest(model: detector) { [weak self] request, error in
    self?.detectionRequestHandler(request: request, error: error)
}
// .scaleFill results in a slight skew but the model was trained accordingly
// see https://developer.apple.com/documentation/vision/vnimagecropandscaleoption for more information
diceDetectionRequest.imageCropAndScaleOption = .scaleFill
```

#### Pass Camera Frames to the Object Detector to Predict Dice Locations <a id="3375271"></a>

카메라에서 [`VNImageRequestHandler`](https://developer.apple.com/documentation/vision/vnimagerequesthandler) 객체를 사용하여 예측할 수 있도록 프레임을 [`VNCoreMLRequest`](https://developer.apple.com/documentation/vision/vncoremlrequest)에 전달하라. [`VNImageRequestHandler`](https://developer.apple.com/documentation/vision/vnimagerequesthandler) 객체는 모든 예측에 대해 모델 출력의 후처리뿐만 아니라 이미지 크기 조정 및 전처리를 처리한다.

카메라 프레임을 모델에 전달하려면 먼저 장치의 물리적 방향에 해당하는 이미지 방향을 찾아야 한다. 장치의 방향이 변경되면 영상의 가로 세로 비율도 변경될 수 있다. 탐지된 객체의 바운딩 박스를 원래 이미지로 다시 확장해야 하기 때문에 크기를 추적해야 한다.

```swift
// The frame is always oriented based on the camera sensor,
// so in most cases Vision needs to rotate it for the model to work as expected.
let orientation = UIDevice.current.orientation

// The image captured by the camera
let image = frame.capturedImage

let imageOrientation: CGImagePropertyOrientation
switch orientation {
case .portrait:
    imageOrientation = .right
case .portraitUpsideDown:
    imageOrientation = .left
case .landscapeLeft:
    imageOrientation = .up
case .landscapeRight:
    imageOrientation = .down
case .unknown:
    print("The device orientation is unknown, the predictions may be affected")
    fallthrough
default:
    // By default keep the last orientation
    // This applies for faceUp and faceDown
    imageOrientation = self.lastOrientation
}

// For object detection, keeping track of the image buffer size
// to know how to draw bounding boxes based on relative values.
if self.bufferSize == nil || self.lastOrientation != imageOrientation {
    self.lastOrientation = imageOrientation
    let pixelBufferWidth = CVPixelBufferGetWidth(image)
    let pixelBufferHeight = CVPixelBufferGetHeight(image)
    if [.up, .down].contains(imageOrientation) {
        self.bufferSize = CGSize(width: pixelBufferWidth,
                                 height: pixelBufferHeight)
    } else {
        self.bufferSize = CGSize(width: pixelBufferHeight,
                                 height: pixelBufferWidth)
    }
}

```

마지막으로 객체 디텍터를 사용하여 예측하기 위해 카메라의 이미지와 현재 방향에 대한 정보를 사용하여 [`VNImageRequestHandler`](https://developer.apple.com/documentation/vision/vnimagerequesthandler)를 호출하라.

```swift

// Invoke a VNRequestHandler with that image
let handler = VNImageRequestHandler(cvPixelBuffer: image, orientation: imageOrientation, options: [:])

do {
    try handler.perform([self.diceDetectionRequest])
} catch {
    print("CoreML request failed with error: \(error.localizedDescription)")
}
```

이제 앱이 모델에 _input_ 데이터를 제공하는 것을 처리하므로 모델의 _output_을 해석해야 한다.

#### Draw Bounding Boxes to Understand Your Model’s Behavior <a id="3375272"></a>

각 객체와 해당 텍스트 라벨 주위에 바운딩 박스를 그려서 디텍터가 얼마나 잘 작동하는지 더 잘 이해할 수 있다. 주사위 검출 모델은 주사위 상단을 검출해 각 주사위 상면에 나타난 핍의 수에 따라 라벨을 붙인다.

바운딩 박스를 그리려면 [Recognizing Objects in Live Capture](https://developer.apple.com/documentation/vision/recognizing_objects_in_live_capture)를 참조하라.

#### Determine When a Roll Has Ended <a id="3375273"></a>

주사위 게임을 할 때, 사용자들은 롤의 결과를 알고 싶어한다. 앱은 주사위의 위치와 값이 안정되기를 기다리는 것으로 롤이 끝났다고 판단한다.

종료 롤의 요구 사항을 다음 조건과 두 개의 연속 카메라 프레임 간의 비교로 정의할 수 있다:

* 검출된 주사위의 수는 같아야 한다.
* 검출된 각 다이에 대해:
  * 바운딩 박스가 움직여서는 안된다.
  * 식별된 클래스가 일치해야 한다.

이러한 제약 조건을 바탕으로 현재 및 이전 [`VNRecognizedObjectObservation`](https://developer.apple.com/documentation/vision/vnrecognizedobjectobservation) 객체를 기준으로 롤링이 종료되었는지 여부를 앱에 알려주는 기능을 만들 수 있다.

```swift
/// Determines if a roll has ended with the current dice values O(n^2)
///
/// - parameter observations: The object detection observations from the model
/// - returns: True if the roll has ended
func hasRollEnded(observations: [VNRecognizedObjectObservation]) -> Bool {
    // First check if same number of dice were detected
    if lastObservations.count != observations.count {
        lastObservations = observations
        return false
    }
    var matches = 0
    for newObservation in observations {
        for oldObservation in lastObservations {
            // If the labels don't match, skip it
            // Or if the IOU is less than 85%, consider this box different
            // Either it's a different die or the same die has moved
            if newObservation.labels.first?.identifier == oldObservation.labels.first?.identifier &&
                intersectionOverUnion(oldObservation.boundingBox, newObservation.boundingBox) > 0.85 {
                matches += 1
            }
        }
    }
    lastObservations = observations
    return matches == observations.count
}
```

이제 모든 예측\(모든 새로운 카메라 프레임을 의미\)에 대해 롤이 끝났는지 확인할 수 있다.

#### Display the Dice Values <a id="3375274"></a>

롤이 종료되면 화면에 정보를 표시하거나 게임 설정에서 다른 동작을 트리거할 수 있다. 이 샘플 앱은 화면에서 인식된 값 목록을 보여 주며, 가장 왼쪽에서 가장 오른쪽에 있는 [`VNRecognizedObjectObservation`](https://developer.apple.com/documentation/vision/vnrecognizedobjectobservation)으로 정렬된다. 각 관측치의 바운딩 박스 좌표에 따라 표면에 주사위가 있는 위치를 기준으로 값을 정렬한다. 앱은 바운딩 박스의 `centerX` 속성을 기준으로 관측치를 오름차순으로 정렬하여 이 작업을 수행한다.

```swift
var sortableDiceValues = [(value: Int, xPosition: CGFloat)]()

for observation in observations {
    // Select only the label with the highest confidence.
    guard let topLabelObservation = observation.labels.first else {
        print("Object observation has no labels")
        continue
    }

    if let intValue = Int(topLabelObservation.identifier) {
        sortableDiceValues.append((value: intValue, xPosition: observation.boundingBox.midX))
    }
}

let diceValues = sortableDiceValues.sorted { $0.xPosition < $1.xPosition }.map { $0.value }
```

### 

### See Also

#### Object Recognition

* [Recognizing Objects in Live Capture](https://developer.apple.com/documentation/vision/recognizing_objects_in_live_capture) Vision 알고리즘을 적용하여 실시간 비디오에서 객체를 식별하라.
* [`class VNRecognizedObjectObservation`](https://developer.apple.com/documentation/vision/vnrecognizedobjectobservation) 인식된 객체를 분류하는 분류 라벨 배열을 사용하여 탐지된 객체 관찰.

