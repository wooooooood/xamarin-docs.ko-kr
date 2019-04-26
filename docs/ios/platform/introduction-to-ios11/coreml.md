---
title: Xamarin.iOS에서 CoreML 소개
description: 이 문서에는 iOS의 machine learning을 사용 하도록 설정 하는 CoreML을 설명 합니다. 이 문서는 CoreML 시작 하는 방법 및 비전 프레임 워크를 사용 하 여 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/30/2017
ms.openlocfilehash: 3a00a7256cace9cbcff3478d866646d48cfdc50b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61385079"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Xamarin.iOS에서 CoreML 소개

CoreML iOS machine learning은 – 앱 문제를 해결 하려면 이미지 인식에서 모든 종류의 작업을 수행 하려면 학습 된 기계 학습 모델을 활용할 수 있습니다.

이 소개에는 다음 내용을 다룹니다.

- [CoreML 시작](#coreml)
- [CoreML 비전 프레임 워크 사용](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>CoreML 시작

이러한 단계는 CoreML iOS 프로젝트를 추가 하는 방법을 설명 합니다. 참조 된 [Mars Habitat Pricer 샘플](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) 실용적인 예제입니다.

![Mars Habitat 가격 예측자 샘플 스크린 샷](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1. CoreML 모델 프로젝트에 추가

CoreML 모델 추가 (파일을를 **.mlmodel** 확장)에 **리소스** 프로젝트의 디렉터리입니다. 

모델 파일의 속성에서 해당 **빌드 작업** 로 설정 된 **CoreMLModel**합니다. 로 컴파일할 수는이 의미는 **.mlmodelc** 응용 프로그램을 빌드할 때 파일입니다.

### <a name="2-load-the-model"></a>2. 모델 로드

사용 하 여 모델을 로드 합니다 `MLModel.Create` 정적 메서드입니다.

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. 매개 변수를 설정 합니다.

시작 및 종료를 구현 하는 컨테이너 클래스를 사용 하 여 모델 매개 변수는 전달 `IMLFeatureProvider`합니다.

기능 공급자 클래스 문자열의 사전 처럼 동작 하 고 `MLFeatureValue`s, 각 기능 값 있는 간단한 문자열 또는 숫자, 배열 또는 데이터 또는 이미지를 포함 하는 픽셀 버퍼 수입니다.

단일 값 기능은 공급자에 대 한 코드 아래에 나와 있습니다.

```csharp
public class MyInput : NSObject, IMLFeatureProvider
{
  public double MyParam { get; set; }
  public NSSet<NSString> FeatureNames => new NSSet<NSString>(new NSString("myParam"));
  public MLFeatureValue GetFeatureValue(string featureName)
  {
    if (featureName == "myParam")
      return MLFeatureValue.FromDouble(MyParam);
    return MLFeatureValue.FromDouble(0); // default value
  }
```

다음과 같은 클래스를 사용 하 여, 입력된 매개 변수를 CoreML에서 인식 하는 방식으로 제공할 수 있습니다. 기능 이름 (같은 `myParam` 코드 예제에서) 모델에 필요한 것과 일치 해야 합니다.

### <a name="4-run-the-model"></a>4. 모델 실행

모델을 사용 하는 기능은 공급자가 인스턴스화될 수 및 매개 변수를 설정 해야 하는 `GetPrediction` 메서드를 호출 합니다.

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. 결과 추출 합니다.

예측 결과 `outFeatures` 인스턴스의 이기도 `IMLFeatureProvider`; output를 사용 하 여 값에 액세스할 수 있습니다 `GetFeatureValue` 의 각 출력 매개 변수 이름 (같은 `theResult`)이이 예제와 같이:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>CoreML 비전 프레임 워크 사용

CoreML 데도 사용할 수 있습니다 비전 프레임 워크와 함께에서 이미지, 도형 인식, 개체 id 및 기타 작업 등에서 작업을 수행 합니다.

다음 단계에서는 어떻게 CoreML 및 시각에서 함께 사용 합니다 [CoreMLVision 샘플](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)합니다. 샘플 결합 합니다 [사각형 인식](~/ios/platform/introduction-to-ios11/vision.md#rectangles) 사용 하 여 비전 프레임 워크에서는 _MNINSTClassifier_ 사진에서 필기 한 숫자를 식별 하는 CoreML 모델.

![숫자 3의 이미지 인식](coreml-images/vision3.png) ![숫자 5의 이미지 인식](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. 비전 CoreML 모델 만들기

CoreML 모델 _MNISTClassifier_ 로드 되어 다음에 래핑된를 `VNCoreMLModel` 그러면 모델 비전 작업에 사용할 수 있습니다. 이 코드는 또한 두 비전 요청을 만듭니다: 먼저 이미지의 사각형을 찾기에 다음 CoreML 모델을 사용 하 여 사각형을 처리 합니다.

```csharp
// Load the ML model
var bundle = NSBundle.MainBundle;
var assetPath = bundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
NSError mlErr, vnErr;
var mlModel = MLModel.Create(assetPath, out mlErr);
var model = VNCoreMLModel.FromMLModel(mlModel, out vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(model, HandleClassification);
```

클래스를 구현 해야 합니다 `HandleRectangles` 고 `HandleClassification` 3-4 단계 아래에 표시 된 Vision 요청에 대 한 메서드.

### <a name="2-start-the-vision-processing"></a>2. 비전 처리 시작

다음 코드는 요청 처리를 시작 합니다. 에 **CoreMLVision** 샘플에서이 코드는 사용자가 이미지를 선택한 후에 실행 합니다.

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

이 처리기에 전달 합니다 `ciImage` 비전 framework `VNDetectRectanglesRequest` 1 단계에서 생성 된 합니다.

### <a name="3-handle-the-results-of-vision-processing"></a>3. 비전 처리의 결과 처리 합니다.

사각형 검색이 완료 되 면 실행을 `HandleRectangles` 메서드 첫 번째 사각형을 추출 하려면 이미지를 자릅니다, 사각형이 이미지를 회색조로, 변환 및 분류에 대 한 CoreML 모델에 전달 합니다.

`request` 비전 요청의 세부 정보를 포함 하는이 메서드에 전달 된 매개 변수를 사용 하 고는 `GetResults<VNRectangleObservation>()` 메서드를 이미지에 있는 사각형의 목록을 반환 합니다. 첫 번째 사각형 `observations[0]` 추출 및 CoreML 모델에 전달 됩니다.

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] {ClassificationRequest}, out NSError err);
  });
}
```

합니다 `ClassificationRequest` 사용 하 여 1 단계에서 초기화 된는 `HandleClassification` 다음 단계에 정의 된 메서드.

### <a name="4-handle-the-coreml"></a>4. CoreML 처리

`request` CoreML 요청의 세부 정보를 포함 하는이 메서드에 전달 된 매개 변수를 사용 하 고는 `GetResults<VNClassificationObservation>()` 메서드를 신뢰 하 여 정렬 가능한 결과의 목록을 반환 합니다 (가장 높은 신뢰도 첫 번째):

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  // ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```

## <a name="samples"></a>샘플

하려는 CoreML 샘플 세 가지가 있습니다.

* 합니다 [Mars Habitat 가격 예측자 샘플](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) 단순 숫자 입력 및 출력 합니다.

* 합니다 [비전 및 CoreML 샘플](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/) 이미지 매개 변수를 받아들이고 비전 프레임 워크를 사용 하 여 자리를 인식 하는 CoreML 모델에 전달 되는 이미지의 사각형 영역을 식별 합니다.

* 마지막으로, 합니다 [CoreML 이미지 인식 샘플](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/) CoreML를 사용 하 여 사진에서 기능을 식별 합니다. 기본적으로 사용 하 여 더 작은 **SqueezeNet** 모델 (5MB) 하지만 쓰인 다운로드 하 고 더 큰 숫자를 통합할 수 있도록 **VGG16** 모델 (553 MB)입니다. 자세한 내용은 참조는 [샘플의 추가 정보](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md)합니다.

## <a name="related-links"></a>관련 링크

- [Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [CoreML 예제 (Mars Habitat) (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML 및 시각 (숫자 인식) (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [CoreML 이미지 인식 (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [Azure Custom Vision (샘플)를 사용 하 여 CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [CoreML (WWDC) (비디오)를 소개합니다.](https://developer.apple.com/videos/play/wwdc2017/703/)
