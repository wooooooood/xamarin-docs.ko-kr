---
title: Xamarin.ios의 비전 프레임 워크
description: 이 문서에서는 Xamarin.ios에서 iOS 11 비전 프레임 워크를 사용 하는 방법을 설명 합니다. 특히 사각형 검색 및 얼굴 감지에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 08/31/2017
ms.openlocfilehash: b0f6647ff92c8d8d0b8d2769c85aa24572d1464e
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285737"
---
# <a name="vision-framework-in-xamarinios"></a>Xamarin.ios의 비전 프레임 워크

비전 프레임 워크는 다음과 같은 다양 한 새 이미지 처리 기능을 iOS 11에 추가 합니다.

- [사각형 감지](#rectangles)
- [얼굴 감지](#faces)
- Machine Learning 이미지 분석 ( [Coreml](~/ios/platform/introduction-to-ios11/coreml.md)에서 설명)
- 바코드 검색
- 이미지 맞춤 분석
- 텍스트 검색
- 수평 검색
- 개체 검색 & 추적

![세 개의 사각형이 검색 된 사진](vision-images/found-rectangles-tiny.png) ![두 얼굴 검색 된 사진](vision-images/xamarin-home-faces-tiny.png)

사각형 검색 및 얼굴 감지 아래에서 자세히 설명 합니다.

<a name="rectangles" />

## <a name="rectangle-detection"></a>사각형 감지

[VisionRects 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionrectangles) 에서는 이미지를 처리 하 고 검색 된 사각형을 그리는 방법을 보여 줍니다.

### <a name="1-initialize-the-vision-request"></a>1. 비전 요청 초기화

에서 `ViewDidLoad`각 요청이 끝날 `VNDetectRectanglesRequest` 때 호출 될 `HandleRectangles` 메서드를 참조 하는을 만듭니다.

`MaximumObservations` 속성도 설정 해야 합니다. 그렇지 않으면 기본값이 1로 설정 되 고 단일 결과만 반환 됩니다.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. 비전 처리 시작

다음 코드는 요청 처리를 시작 합니다. **VisionRects** 샘플에서이 코드는 사용자가 이미지를 선택한 후에 실행 됩니다.

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

이 처리기는를 `ciImage` 1 단계에서 만든 `VNDetectRectanglesRequest` 비전 프레임 워크로 전달 합니다.

### <a name="3-handle-the-results-of-vision-processing"></a>3. 비전 처리의 결과를 처리 합니다.

사각형 검색이 완료 되 면 프레임 워크에서 `HandleRectangles` 메서드를 실행 합니다. 요약 정보는 아래와 같습니다.

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

**VisionRectangles 샘플** 의 메서드에는세가지함수가`OverlayRectangles` 있습니다.

- 소스 이미지 렌더링
- 각 항목을 검색 한 위치를 나타내는 사각형 그리기
- CoreGraphics를 사용 하 여 각 사각형에 대 한 텍스트 레이블 추가

정확한 CoreGraphics 메서드에 대 한 [샘플 소스](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionrectangles) 를 봅니다.

![세 개의 사각형이 검색 된 사진](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. 추가 처리

사각형 검색은 종종 작업 체인에서 첫 번째 단계입니다. [예를 들어 사각형이 CoreML](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision)모델에 전달 되어 필기 숫자를 구문 분석 합니다.


<a name="faces" />

## <a name="face-detection"></a>얼굴 감지

[VisionFaces 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionfaces) 은 다른 시각 요청 클래스를 사용 하 여 **VisionRectangles** 샘플과 비슷한 방식으로 작동 합니다.

### <a name="1-initialize-the-vision-request"></a>1. 비전 요청 초기화

에서 `ViewDidLoad`각 요청이 끝날 `VNDetectFaceRectanglesRequest` 때 호출 될 `HandleRectangles` 메서드를 참조 하는을 만듭니다.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. 비전 처리 시작

다음 코드는 요청 처리를 시작 합니다. **VisionFaces** 샘플에서이 코드는 사용자가 이미지를 선택한 후에 실행 됩니다.

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

이 처리기는를 `ciImage` 1 단계에서 만든 `VNDetectFaceRectanglesRequest` 비전 프레임 워크로 전달 합니다.

### <a name="3-handle-the-results-of-vision-processing"></a>3. 비전 처리의 결과를 처리 합니다.

얼굴 검색이 완료 되 면 처리기는 오류 처리를 수행 `HandleRectangles` 하 고 검색 된 면의 범위를 표시 하는 메서드를 실행 하 고를 `OverlayRectangles` 호출 하 여 원본 그림에 경계 사각형을 그립니다.

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

**VisionFaces 샘플** 의 메서드에는세가지함수가`OverlayRectangles` 있습니다.

- 소스 이미지 렌더링
- 검색 된 각 면에 대 한 사각형 그리기
- CoreGraphics를 사용 하 여 각 면에 대 한 텍스트 레이블 추가

정확한 CoreGraphics 메서드에 대 한 [샘플 소스](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionfaces) 를 봅니다.

![두 얼굴 검색 된 사진](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. 추가 처리

시각 프레임 워크에는 눈동자 및 입 등의 얼굴 기능을 검색 하는 추가 기능이 포함 되어 있습니다. 위의 3 단계에서와 같이 결과 `VNFaceObservation` 를 반환 하 고 추가 `VNFaceLandmark` 데이터를 사용 하 여 결과를 반환 하는 형식을사용합니다.`VNDetectFaceLandmarksRequest`


## <a name="related-links"></a>관련 링크

- [시각 사각형 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionrectangles)
- [시력 얼굴 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionfaces)
- [핵심 이미지의 고급 기능-필터, 금속, 비전 및 기타 (WWDC) (비디오)](https://developer.apple.com/videos/play/wwdc2017/510/)
