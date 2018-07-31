---
title: Xamarin.iOS에서 비전 프레임 워크
description: 이 문서에서는 iOS 11을 사용 하는 방법을 설명 Xamarin.iOS에서 비전 프레임 워크입니다. 사각형 검색에 설명 하므로 특히 및 얼굴 감지 합니다.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2017
ms.openlocfilehash: 4746de2f351e866fd72946b204f97e997c3e88c4
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350666"
---
# <a name="vision-framework-in-xamarinios"></a>Xamarin.iOS에서 비전 프레임 워크

비전 프레임 워크 수가 한 iOS 11 포함 하는 기능을 처리 하는 새 이미지를 추가 합니다.

- [사각형 검색](#rectangles)
- [얼굴 감지](#faces)
- Machine Learning 이미지 분석 (에서 설명한 [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- 바코드 검색
- 이미지 맞춤 분석
- 텍스트 검색
- 기간 검색
- 개체 검색 및 추적

![검색 하는 세 개의 사각형이 촬영](vision-images/found-rectangles-tiny.png) ![두 개의 얼굴 감지를 사용 하 여 촬영](vision-images/xamarin-home-faces-tiny.png)

사각형 감지 및 얼굴 감지 아래에서 자세히 설명 합니다.

<a name="rectangles" />

## <a name="rectangle-detection"></a>사각형 검색

합니다 [VisionRects 샘플](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) 이미지를 처리 하 고 검색 된 사각형을 그리는 방법을 보여 줍니다.

### <a name="1-initialize-the-vision-request"></a>1. 비전 요청을 초기화

`ViewDidLoad`를 만들기는 `VNDetectRectanglesRequest` 를 참조 하는 `HandleRectangles` 각 요청의 끝에서 호출할 메서드:

`MaximumObservations` 속성도 설정 해야, 그렇지 않으면 1로 기본 설정 됩니다 및 하나의 결과만 반환 됩니다.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. 비전 처리 시작

다음 코드는 요청 처리를 시작 합니다. 에 **VisionRects** 샘플에서이 코드는 사용자가 이미지를 선택한 후에 실행 합니다.

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

이 처리기에 전달 합니다 `ciImage` 비전 framework `VNDetectRectanglesRequest` 1 단계에서 생성 된 합니다.

### <a name="3-handle-the-results-of-vision-processing"></a>3. 비전 처리의 결과 처리 합니다.

사각형 검색이 완료 되 면 프레임 워크 실행을 `HandleRectangles` 메서드를 간략하게 설명 하는 다음과 같습니다.

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  bool atLeastOneValid = false;
  foreach (var o in observations){
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  if (!atLeastOneValid) return;
  // Show the pre-processed image
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. 결과 표시 합니다.

합니다 `OverlayRectangles` 의 메서드를 **VisionRectangles** 샘플에는 세 가지 기능:

- 원본 이미지를 렌더링합니다.
- 각각 검색 된 위치를 나타내는 사각형을 그리기 및
- CoreGraphics를 사용 하 여 각 사각형에 대 한 텍스트 레이블을 추가 합니다.

보기는 [샘플의 소스](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) 정확한 CoreGraphics 메서드에 대 한 합니다.

![검색 하는 세 개의 사각형이 촬영](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. 추가 처리

사각형 감지 된 종종 작업의 체인에서 첫 번째 단계 일 뿐와 같은 [CoreMLVision 예제](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision)사각형 필기 숫자를 구문 분석 하는 CoreML 모델에 전달 되는 위치입니다.


<a name="faces" />

## <a name="face-detection"></a>얼굴 감지

합니다 [VisionFaces 샘플](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) 비슷한 방식으로 작동 합니다 **VisionRectangles** 다른 비전 요청 클래스를 사용 하 여 샘플.

### <a name="1-initialize-the-vision-request"></a>1. 비전 요청을 초기화

`ViewDidLoad`, 만들기를 `VNDetectFaceRectanglesRequest` 를 참조 하는 `HandleRectangles` 메서드 각 요청의 끝에 호출 됩니다.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. 비전 처리 시작

다음 코드는 요청 처리를 시작 합니다. 에 **VisionFaces** 이 코드는 사용자가 이미지를 선택한 후에 실행 하는 샘플:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

이 처리기에 전달 합니다 `ciImage` 비전 framework `VNDetectFaceRectanglesRequest` 1 단계에서 생성 된 합니다.

### <a name="3-handle-the-results-of-vision-processing"></a>3. 비전 처리의 결과 처리 합니다.

얼굴 검색 완료 되 면 처리기를 실행 합니다 `HandleRectangles` 오류 처리를 수행 하 고 검색 된 얼굴 및 호출의 범위를 표시 하는 메서드는 `OverlayRectangles` 원래 그림의 경계 사각형을 그릴 수:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNFaceObservation>();
  // ... omitted error handling...
  var summary = "";
  var imageSize = InputImage.Extent.Size;
  bool atLeastOneValid = false;
  Console.WriteLine("Faces:");
  summary += "Faces:";
  foreach (var o in observations) {
    // Verify detected rectangle is valid. omitted
    var boundingBox = o.BoundingBox.Scaled(imageSize);
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  // Show the pre-processed image (on UI thread)
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. 결과 표시 합니다.

합니다 `OverlayRectangles` 의 메서드를 **VisionFaces** 샘플에는 세 가지 기능:

- 원본 이미지를 렌더링합니다.
- 검색의 각 얼굴에 대 한 사각형을 그리기 및
- CoreGraphics를 사용 하 여 각 얼굴에 대 한 텍스트 레이블을 추가 합니다.

보기는 [샘플의 소스](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) 정확한 CoreGraphics 메서드에 대 한 합니다.

![두 개의 얼굴 감지를 사용 하 여 촬영](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. 추가 처리

비전 프레임 워크는 눈 등 입 얼굴 특징을 검색 하려면 추가 기능을 포함 합니다. 사용 된 `VNDetectFaceLandmarksRequest` 형식을 반환 하는 `VNFaceObservation` 위의 3 단계와 이지만 추가 결과 `VNFaceLandmark` 데이터입니다.


## <a name="related-links"></a>관련 링크

- [비전 사각형 (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [비전 얼굴 (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [필터, Metal, 시각 및 더 (WWDC) (비디오) Core 이미지의 발전](https://developer.apple.com/videos/play/wwdc2017/510/)
