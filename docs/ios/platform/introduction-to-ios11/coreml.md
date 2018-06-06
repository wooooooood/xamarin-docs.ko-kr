---
title: Xamarin.iOS에 CoreML 소개
description: 이 문서에서는 기계 학습에서 iOS 매핑함으로써 CoreML를 설명 합니다. 이 문서에서는 CoreML 시작 하는 방법과 비전 framework와 함께 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: b893fe5e56cc2d43a71870ffbbd20f0b8c6cfd18
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787498"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Xamarin.iOS에 CoreML 소개

_IOS 11에서 모바일 앱에 대 한 학습 하는 컴퓨터_

CoreML 제공 iOS에 대 한 기계 학습 – 응용 프로그램 문제 해결에 이미지 인식에서 모든 종류의 작업을 수행 하려면 학습 된 기계 학습 모델을 이용할 수 있습니다.

이 소개는 다음 내용을 설명 합니다.

- [CoreML 시작](#coreml)
- [CoreML 비전 framework와 함께 사용 하 여](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>CoreML 시작

이러한 단계에서는 CoreML iOS 프로젝트에 추가 하는 방법에 설명 합니다. 참조는 [Mars 우리가 Pricer 샘플](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) 실제 예에 대 한 합니다.

![Mars 우리가 가격 예측자 샘플 스크린 샷](coreml-images/marspricer-heading.png)

### <a name="1-add-the-model-to-the-project"></a>1. 프로젝트에 모델 추가

컴파일된 모델 추가 (사용 하 여 디렉터리의 **.modelc** 확장)에 **리소스** 프로젝트의 디렉터리입니다. 디렉터리의 내용을 해야 빌드 작업이 **BundleResource**:

![리소스 폴더에는 컴파일된 모델이 있어야 합니다.](coreml-images/resources-modelc.png)

[샘플](https://developer.xamarin.com/samples/monotouch/ios11/) Xcode 9에서 컴파일된 또는 터미널 명령을 사용 하 여 수동으로 모델 사용:

```csharp
xcrun coremlcompiler compile {model.mlmodel} {outputFolder}
```

> [!NOTE]
> **.model** 파일 _해야_ 로 컴파일할 수 **.modelc** CoreML에서 사용 하기 전에

### <a name="2-load-the-model"></a>2. 모델 로드

모델을 사용 하기 전에 사용 하 여 로드는 `MLModel.FromUrl` 정적 메서드입니다.

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.FromUrl(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. 매개 변수 설정

시작 및 종료를 구현 하는 컨테이너 클래스를 사용 하 여 모델 매개 변수는 전달 `IMLFeatureProvider`합니다.

문자열의 사전 처럼 작동 합니다. 기능 공급자 클래스 및 `MLFeatureValue`s, 각 기능 값을 간단한 문자열 또는 숫자, 배열 또는 데이터 또는 이미지를 포함 하는 픽셀 버퍼 될 수 있는 합니다.

단일 값 기능은 공급자에 대 한 코드는 다음과 같습니다.

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

다음과 같은 클래스를 사용 하 여 입력된 매개 변수를 CoreML에서 인식 하는 방식으로 제공할 수 있습니다. 기능 이름 (같은 `myParam` 코드 예제의) 모델에서 예상 하는 일치 해야 합니다.

### <a name="4-run-the-model"></a>4. 모델을 실행

모델을 사용 하 여 실행 하려면 인스턴스화할 기능 공급자 및 매개 변수를 다음 하는 `GetPrediction` 메서드를 호출할 수 있습니다.

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. 결과 추출 합니다.

예측 결과 `outFeatures` 인스턴스의 이기도 `IMLFeatureProvider`출력;를 사용 하 여 값에 액세스할 수 있습니다 `GetFeatureValue` 각 출력 매개 변수 이름으로 (같은 `theResult`)이이 예제와 같이,:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>CoreML 비전 Framework와 함께 사용 하 여

또한 CoreML 모양 인식, 개체 id 및 기타 작업 등의 이미지에 대 한 작업을 수행 하려면 비전 프레임 워크와 함께에서 사용할 수도 있습니다.

다음 단계에서는 CoreML 및 비전에서 어떻게 사용 되는지를 함께 [CoreMLVision 샘플](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)합니다. 샘플 결합는 [사각형 인식](~/ios/platform/introduction-to-ios11/vision.md#rectangles) 와 비전 프레임 워크에서는 _MNINSTClassifier_ CoreML 사진에 더 이상 필기 숫자가 식별 하는 모델입니다.

![숫자 3의 이미지 인식](coreml-images/vision3.png) ![숫자 5의 이미지 인식](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. 비전 CoreML 모델 만들기

CoreML 모델 _MNISTClassifier_ 로드 되 고 래핑됩니다는 `VNCoreMLModel` 해줍니다 모델 비전 태스크에 사용할 수 있습니다. 이 코드는 또한 두 비전 요청을 만듭니다: 먼저 사각형, 이미지에서 발견 하 고 다음에 대 한 사각형 CoreML 모델 처리:

```csharp
// Load the ML model
var assetPath = NSBundle.MainBundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
var mlModel = MLModel.FromUrl(assetPath, out NSError mlErr);
var vModel = VNCoreMLModel.FromMLModel(mlModel, out NSError vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(vModel, HandleClassification);
```

클래스를 구현 해야는 `HandleRectangles` 및 `HandleClassification` 3 및 4 아래 단계에 표시 된 Vision 요청에 대 한 메서드.

### <a name="2-start-the-vision-processing"></a>2. 비전 처리를 시작

다음 코드에서 요청을 처리를 시작 합니다. 에 **CoreMLVision** 샘플에이 코드는 사용자가 이미지를 선택한 후에 실행:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

이 처리기에 전달 된 `ciImage` 비전 프레임 워크에 `VNDetectRectanglesRequest` 1 단계에서 만든 합니다.

### <a name="3-handle-the-results-of-vision-processing"></a>3. 비전 처리 결과 처리 합니다.

실행 사각형 검색이 완료 되 면는 `HandleRectangles` 메서드 첫 번째 사각형을 추출 하는 이미지를 자릅니다 사각형 이미지를 회색조로, 변환 및 분류에 대 한 CoreML 모델에 전달 합니다.

`request` 비전 요청의 세부 정보를 포함 하는이 메서드에 전달 된 매개 변수를 사용 하 고는 `GetResults<VNRectangleObservation>()` 메서드, 사각형에 이미지의 목록을 반환 합니다. 첫 번째 사각형 `observations[0]` 추출 하 고 CoreML 모델에 전달 됩니다.

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] { ClassificationRequest }, out NSError err);
  });
}
```

`ClassificationRequest` 단계 1 사용 하도록 초기화 된는 `HandleClassification` 다음 단계에 정의 된 메서드.

### <a name="4-handle-the-coreml"></a>4. CoreML 처리

`request` CoreML 요청의 세부 정보를 포함 하는이 메서드에 전달 된 매개 변수를 사용 하 고는 `GetResults<VNClassificationObservation>()` 신뢰도로 정렬 가능한 결과 목록이 반환 메서드를 (가장 높은 신뢰도 첫 번째):

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```



## <a name="samples"></a>샘플

찾으려고 CoreML 샘플 3 개 있습니다.

* [Mars 우리가 가격 예측자 샘플](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) 에 단순 숫자 입력 및 출력이 있습니다.

* [비전 CoreML 샘플](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/) 비전 프레임 워크를 사용 하 여 단일 숫자를 인식 하는 CoreML 모델에 전달 되는 이미지, 사각형 영역을 식별 하 고 이미지 매개 변수를 허용 합니다.

* 마지막으로 [CoreML 이미지 인식 샘플](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/) CoreML를 사용 하 여 사진의 기능을 식별 합니다. 기본적으로 사용 하 여 더 작은 숫자 **SqueezeNet** 다운로드 하 고 더 큰 숫자를 통합할 수 있도록 하지만 모델 (5MB)를 기록 된은 **VGG16** 모델 (553 MB)입니다. 자세한 내용은 참조는 [샘플의 추가 정보](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md)합니다.


## <a name="related-links"></a>관련 링크

- [기계 학습 (Apple)](https://developer.apple.com/machine-learning/)
- [CoreML 예제 (Mars 우리가) (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML 및 비전 (번호 인식) (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [CoreML 이미지 인식 (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [CoreML Azure 사용자 지정 시력이 (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [소개 CoreML (WWDC) (비디오)](https://developer.apple.com/videos/play/wwdc2017/703/)
