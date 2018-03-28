---
title: Face API를 사용 하 여 emotion 인식
description: Face API를 입력으로 이미지에는 얼굴 식 하며 감정 각 면 이미지에 대 한 조합에 대해 신뢰도 수준을 포함 하는 데이터를 반환 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램을 평가 하려면 emotion 인식 하도록 Face API를 사용 하는 방법을 설명 합니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 0fc69fb1283ea2afd95900348cdecec5d6514ae0
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2018
---
# <a name="emotion-recognition-using-the-face-api"></a>Face API를 사용 하 여 emotion 인식

_Face API를 입력으로 이미지에는 얼굴 식 하며 감정 각 면 이미지에 대 한 조합에 대해 신뢰도 수준을 포함 하는 데이터를 반환 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램을 평가 하려면 emotion 인식 하도록 Face API를 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Face API 얼굴 식에서 emotion 분노, 컨 템, disgust, 걱정, 중립 만족도 검색 하는 검색, 슬픔, 및 놀랄 수행할 수 있습니다. 이러한 감정은 보편적으로 및 문화 기본 동일한 얼굴 식을 통해 전달 됩니다. 얼굴 식에 대 한는 emotion 결과 반환 하는 데 뿐만 아니라 Face API 수도 반환에 대해 검색 된 경계 상자. Face API를 사용 하면 API 키를 얻어야 하는 참고 합니다. 다운로드할 수 있습니다 [Cognitive 서비스 시도](https://azure.microsoft.com/try/cognitive-services/?api=face-api)합니다.

Emotion 인식 클라이언트 라이브러리를 통해 및 REST API를 통해 수행할 수 있습니다. 이 문서에서는 emotion 인식을 통해 수행 된 [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/) NuGet에서 다운로드할 수 있는 클라이언트 라이브러리입니다.

Face API 비디오에서는 사용자의 얼굴 식을 인식 하도록도 사용할 수 있으며 해당 감정의 요약을 반환할 수 있습니다. 자세한 내용은 참조 [실시간에서 비디오를 분석 하는 방법을](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/)합니다.

Face API에 대 한 자세한 내용은 참조 [Face API](/azure/cognitive-services/face/overview/)합니다.

## <a name="performing-emotion-recognition"></a>Emotion 인식을 수행

Emotion 인식 Face API을 이미지 스트림으로 업로드 하 여 이루어집니다. 이미지 파일 크기를 4MB 보다 큰 수 해서는 안 되며 지원 되는 파일 형식은 JPEG, PNG, GIF, 및 BMP 있습니다.

다음 코드 예제에서는 emotion 인식 프로세스를 보여 줍니다.

```csharp
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;

var faceServiceClient = new FaceServiceClient(Constants.FaceApiKey, Constants.FaceEndpoint);
// e.g. var faceServiceClient = new FaceServiceClient("a3dbe2ed6a5a9231bb66f9a964d64a12", "https://westus.api.cognitive.microsoft.com/face/v1.0/detect");

var faceAttributes = new FaceAttributeType[] { FaceAttributeType.Emotion };
using (var photoStream = photo.GetStream())
{
    Face[] faces = await faceServiceClient.DetectAsync(photoStream, true, false, faceAttributes);
    if (faces.Any())
    {
        // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
        emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
    }
    // Store emotion as app rating
    ...
}
```

`FaceServiceClient` Face API 키 및에 인수로 전달 되는 끝점으로 emotion 인식을 수행 하려면 인스턴스를 만들어야 합니다는 `FaceServiceClient` 생성자입니다.

> [!NOTE]
> 동일한 지역으로 구독 키를 가져오는 데 사용 하 여 얼굴 API 호출에서 사용 해야 합니다. 예를 들어, 사용자 구독 키를 구입한 경우는 `westus` 영역 얼굴 감지 끝점 됩니다 `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`합니다.

`DetectAsync` 에 호출 되는 메서드는 `FaceServiceClient` 인스턴스를 이미지로 Face API에 업로드 한 `Stream`합니다. 이 작업을 호출 하는 경우 API 키 Face API에 전송 됩니다. 실패가 발생 올바른 API 키를 전송 하는 `Microsoft.ProjectOxford.Face.FaceAPIException` 잘못 된 API 키 제출 되었음을 나타내는 예외 메시지와 함께 throw 되 고 있습니다.

`DetectAsync` 메서드는 반환 된 `Face` 는 얼굴 인식 된 배열입니다. 일련의 선택적 글꼴 특성에서 지정 된와 함께 해당 위치를 나타내는 사각형을 포함 하는 면 반환 된 각는 `faceAttributes` 인수에는 `DetectAsync` 메서드. 없는 얼굴 감지 되 면 빈 `Face` 배열이 반환 됩니다.

Face API에서 결과 해석 하는 경우 검색 된 emotion으로 해석할지 점수가 가장 높은와 emotion 점수를 기본 형태로 변환 하는 대로 하나에 대 한 합계를 구할 합니다. 따라서 샘플 응용 프로그램 다음 스크린샷에서 같이 가장 큰 검색 된 글꼴에 대 한 가장 높은 점수를 인식 된 emotion 이미지에 표시 합니다.

![](emotion-recognition-images/emotion-recognition.png "Emotion 인식")

## <a name="summary"></a>요약

이 문서는 Xamarin.Forms 응용 프로그램을 평가 하려면 emotion 인식 하도록 Face API를 사용 하는 방법을 설명 합니다. Face API 얼굴 식을 입력으로 이미지의 하며 이미지의 각 면에 대해 감정의 신뢰성을 포함 하는 데이터를 반환 합니다.

## <a name="related-links"></a>관련 링크

- [API에 맞서게](/azure/cognitive-services/face/overview/)합니다.
- [Todo Cognitive 서비스 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/)
