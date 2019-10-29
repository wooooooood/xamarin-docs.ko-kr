---
title: Xamarin.ios의 CoreML 소개
description: 이 문서에서는 iOS에서 기계 학습을 가능 하 게 하는 CoreML에 대해 설명 합니다. 이 문서에서는 CoreML를 시작 하는 방법과 비전 프레임 워크에서이를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/30/2017
ms.openlocfilehash: 4319d9ab07682795e8890779a65a0e2289f4501c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032208"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Xamarin.ios의 CoreML 소개

CoreML는 iOS에 기계 학습을 제공 합니다. 앱은 학습 된 기계 학습 모델을 활용 하 여 문제 해결부터 이미지 인식까지 모든 종류의 작업을 수행할 수 있습니다.

이 소개에서는 다음 내용을 다룹니다.

- [CoreML 시작](#coreml)
- [시력 프레임 워크에서 CoreML 사용](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>CoreML 시작

이러한 단계에서는 CoreML를 iOS 프로젝트에 추가 하는 방법을 설명 합니다. 실제 예는 [Mars Habitat Pricer 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/) 을 참조 하세요.

![Mars Habitat 가격 예측 샘플 스크린샷](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1. CoreML 모델을 프로젝트에 추가 합니다.

프로젝트의 **Resources** 디렉터리에 coreml 모델 (확장명이 **mlmodel** 인 파일)을 추가 합니다. 

모델 파일의 속성에서 해당 **빌드 작업** 은 **Coremlmodel**로 설정 됩니다. 즉, 응용 프로그램을 빌드할 때이 파일은 **mlmodelc** 파일로 컴파일됩니다.

### <a name="2-load-the-model"></a>2. 모델을 로드 합니다.

`MLModel.Create` 정적 메서드를 사용 하 여 모델을 로드 합니다.

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. 매개 변수 설정

모델 매개 변수는 `IMLFeatureProvider`을 구현 하는 컨테이너 클래스를 사용 하 여 전달 및 출력 됩니다.

기능 공급자 클래스는 문자열 및 `MLFeatureValue`s 사전 처럼 동작 합니다. 여기서 각 기능 값은 단순 문자열 또는 숫자, 배열 또는 데이터 또는 이미지를 포함 하는 픽셀 버퍼 일 수 있습니다.

단일 값 기능 공급자에 대 한 코드는 다음과 같습니다.

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

이 처럼 클래스를 사용 하 여 CoreML에서 인식 하는 방식으로 입력 매개 변수를 제공할 수 있습니다. 기능 이름 (예: 코드 예제의 `myParam`)은 모델에 필요한 내용과 일치 해야 합니다.

### <a name="4-run-the-model"></a>4. 모델 실행

모델을 사용 하려면 기능 공급자를 인스턴스화해야 하 고 매개 변수를 설정 해야 합니다. 그러면 `GetPrediction` 메서드가 호출 됩니다.

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. 결과 추출

예측 결과 `outFeatures`도 `IMLFeatureProvider`의 인스턴스입니다. 출력 값에는 다음 예제와 같이 각 출력 매개 변수의 이름 (예: `theResult`)과 함께 `GetFeatureValue`를 사용 하 여 액세스할 수 있습니다.

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>시력 프레임 워크에서 CoreML 사용

CoreML를 비전 프레임 워크와 함께 사용 하 여 이미지에 대 한 작업을 수행할 수도 있습니다 (예: 셰이프 인식, 개체 식별 및 기타 작업).

아래 단계에서는 [Coreml비전 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)에서 coreml 및 비전을 함께 사용 하는 방법을 설명 합니다. 이 샘플에서는 비전 프레임 워크의 [사각형 인식 기능](~/ios/platform/introduction-to-ios11/vision.md#rectangles) 을 _MNINSTClassifier_ coreml 모델과 결합 하 여 사진에서 필기 한 숫자를 식별 합니다.

![숫자 3의 이미지 인식](coreml-images/vision3.png) ![숫자 5의 이미지 인식](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. 시각 CoreML 모델 만들기

CoreML 모델 _MNISTClassifier_ 로드 된 후 모델을 비전 작업에 사용할 수 있도록 하는 `VNCoreMLModel`에 래핑됩니다. 또한이 코드는 두 개의 시각 요청을 만듭니다. 첫 번째는 이미지에서 사각형을 찾은 다음 CoreML 모델을 사용 하 여 사각형을 처리 하는 것입니다.

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

이 클래스는 아래 3 단계 및 4 단계에 표시 된 대로 비전 요청에 대 한 `HandleRectangles` 및 `HandleClassification` 메서드를 계속 구현 해야 합니다.

### <a name="2-start-the-vision-processing"></a>2. 비전 처리를 시작 합니다.

다음 코드는 요청 처리를 시작 합니다. **Coremlvision** 샘플에서이 코드는 사용자가 이미지를 선택한 후에 실행 됩니다.

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

이 처리기는 1 단계에서 만든 비전 프레임 워크 `VNDetectRectanglesRequest`에 `ciImage`를 전달 합니다.

### <a name="3-handle-the-results-of-vision-processing"></a>3. 비전 처리 결과를 처리 합니다.

사각형 검색이 완료 되 면 이미지를 잘라 첫 번째 사각형을 추출 하는 `HandleRectangles` 메서드를 실행 하 고, 사각형 이미지를 회색조로 변환 변환 하 고, 분류를 위한 CoreML 모델에 전달 합니다.

이 메서드에 전달 된 `request` 매개 변수에는 비전 요청에 대 한 세부 정보가 포함 되 고 `GetResults<VNRectangleObservation>()` 메서드를 사용 하 여 이미지에 있는 사각형의 목록을 반환 합니다. 첫 번째 사각형 `observations[0]` 추출 되어 CoreML 모델에 전달 됩니다.

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

다음 단계에서 정의 된 `HandleClassification` 메서드를 사용 하도록 1 단계에서 `ClassificationRequest` 초기화 되었습니다.

### <a name="4-handle-the-coreml"></a>4. CoreML를 처리 합니다.

이 메서드에 전달 된 `request` 매개 변수는 CoreML 요청의 세부 정보를 포함 하 고 `GetResults<VNClassificationObservation>()` 메서드를 사용 하 여 신뢰도에 따라 정렬 된 가능한 결과 목록을 반환 합니다 (최고 신뢰도 우선).

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

다음 세 가지 CoreML 샘플이 있습니다.

- [Mars Habitat Price 예측 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/) 은 간단한 숫자 입력 및 출력을 포함 합니다.

- [비전 & coreml 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision) 은 이미지 매개 변수를 수락 하 고, 비전 프레임 워크를 사용 하 여 이미지의 사각형 영역을 식별 하며,이는 단일 숫자를 인식 하는 coreml 모델에 전달 됩니다.

- 마지막으로 [Coreml 이미지 인식 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlimagerecognition) 은 coreml를 사용 하 여 사진의 기능을 식별 합니다. 기본적으로 더 작은 5MB ( **SqueezeNet** model)를 사용 하지만, 더 큰 **VGG16** 모델 (553MB)을 다운로드 하 여 통합할 수 있도록 작성 되었습니다. 자세한 내용은 [샘플의 추가](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md)정보를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [CoreML 예제 (Mars Habitat) (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer/)
- [CoreML 및 비전 (숫자 인식) (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlvision)
- [CoreML 이미지 인식 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlimagerecognition)
- [Azure Custom Vision을 사용 하는 CoreML (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-coremlazuremodel)
- [CoreML (WWDC) 소개 (비디오)](https://developer.apple.com/videos/play/wwdc2017/703/)
