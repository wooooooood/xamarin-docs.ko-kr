---
title: Xamarin.iOS 비전 프레임 워크
description: 이 문서에서는 11 iOS를 사용 하는 방법을 설명 Xamarin.iOS에 비전 프레임 워크입니다. 사각형 검색에 설명 특히 검색 직면 하 고 있습니다.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2016
ms.openlocfilehash: c44c4b3ab12c1ba448f1befb6f831f5ad9119f18
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787421"
---
# <a name="vision-framework-in-xamarinios"></a>Xamarin.iOS 비전 프레임 워크

비전 프레임 워크 다양 한 기능을 포함 하 여 11 iOS 처리 하는 새 이미지를 추가 합니다.

- [사각형 검색](#rectangles)
- [얼굴 감지](#faces)
- 기계 학습 이미지 분석 (에 설명 된 [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- 바코드 검색
- 이미지 맞춤 분석
- 텍스트 검색
- 마감일 검색
- 개체 검색 및 추적

![검색 하는 세 개의 사각형이 촬영](vision-images/found-rectangles-tiny.png) ![검색 하는 두 개의 얼굴 사진 촬영](vision-images/xamarin-home-faces-tiny.png)

사각형 검색 및 얼굴 감지 아래에서 자세히 설명 되어 있습니다.

<a name="rectangles" />

## <a name="rectangle-detection"></a>사각형 검색

[VisionRects 샘플](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) 이미지를 처리 하 고 검색 된 사각형에 그리기 하는 방법을 보여 줍니다.

### <a name="1-initialize-the-vision-request"></a>1. 비전 요청을 초기화

`ViewDidLoad`, 만들는 `VNDetectRectanglesRequest` 참조 하는 `HandleRectangles` 각 요청이 끝날 때 호출할 메서드:

`MaximumObservations` 속성 설정 해야, 그렇지 않으면 값은 기본적으로 1 및 단일 결과만 반환 됩니다.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. 비전 처리를 시작

다음 코드에서 요청을 처리를 시작 합니다. 에 **VisionRects** 샘플에이 코드는 사용자가 이미지를 선택한 후에 실행:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

이 처리기에 전달 된 `ciImage` 비전 프레임 워크에 `VNDetectRectanglesRequest` 1 단계에서 만든 합니다.

### <a name="3-handle-the-results-of-vision-processing"></a>3. 비전 처리 결과 처리 합니다.

프레임 워크가 실행 하는 사각형 검색이 완료 되 면는 `HandleRectangles` 메서드를 요약은 다음과 같습니다.

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

### <a name="4-display-the-results"></a>4. 결과 표시

`OverlayRectangles` 에서 메서드는 **VisionRectangles** 샘플의 세 가지 기능은:

- 소스 이미지 렌더링
- 각 검색 되었지만 나타내려면 사각형 그리기 및
- CoreGraphics를 사용 하 여 각 사각형에 대 한 텍스트 레이블을 추가 합니다.

보기는 [샘플의 소스](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) 정확한 CoreGraphics 메서드에 대 한 합니다.

![검색 하는 세 개의 사각형이 촬영](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. 추가 처리

사각형 검색은 종종 작업의 체인의 첫 번째 단계 일 뿐와 같은 [CoreMLVision 예제](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision)사각형 필기 자릿수의 구문 분석을 CoreML 모델에 전달 되는 위치입니다.


<a name="faces" />

## <a name="face-detection"></a>얼굴 감지

[VisionFaces 샘플](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) 에서 비슷한 방식으로 작동 하는 **VisionRectangles** 샘플에서 다른 비전 요청 클래스를 사용 하 여 합니다.

### <a name="1-initialize-the-vision-request"></a>1. 비전 요청을 초기화

`ViewDidLoad`, 만들는 `VNDetectFaceRectanglesRequest` 참조 하는 `HandleRectangles` 각 요청이 끝날 때 호출할 메서드.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. 비전 처리를 시작

다음 코드에서 요청을 처리를 시작 합니다. 에 **VisionFaces** 이 코드는 사용자가 이미지를 선택한 후에 실행 하는 샘플:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

이 처리기에 전달 된 `ciImage` 비전 프레임 워크에 `VNDetectFaceRectanglesRequest` 1 단계에서 만든 합니다.

### <a name="3-handle-the-results-of-vision-processing"></a>3. 비전 처리 결과 처리 합니다.

처리기가 실행 얼굴 검색이 완료 되 면는 `HandleRectangles` 오류 처리를 수행 하 고 검색 된 면 및 호출의 범위를 표시 하는 메서드는 `OverlayRectangles` 원본 그림에 경계 사각형을 그리는:

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

### <a name="4-display-the-results"></a>4. 결과 표시

`OverlayRectangles` 에서 메서드는 **VisionFaces** 샘플의 세 가지 기능은:

- 소스 이미지 렌더링
- 각 면 감지에 대 한 사각형 그리기 및
- 각 면 CoreGraphics를 사용 하 여에 대 한 텍스트 레이블을 추가 합니다.

보기는 [샘플의 소스](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) 정확한 CoreGraphics 메서드에 대 한 합니다.

![검색 하는 두 개의 얼굴 사진 촬영](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. 추가 처리

비전 framework는 눈 및 입 같은 얼굴 기능을 검색 하는 추가 기능을 포함 합니다. 사용 하 여는 `VNDetectFaceLandmarksRequest` 을 반환 하는 형식 `VNFaceObservation` 추가 실행 되지만, 위의 3 단계 결과 `VNFaceLandmark` 데이터입니다.


## <a name="related-links"></a>관련 링크

- [비전 사각형 (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [비전 면 (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Core 이미지-필터, 금속, 비전과 더 (WWDC) (비디오)의 고급 기능](https://developer.apple.com/videos/play/wwdc2017/510/)
