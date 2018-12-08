---
title: Face API를 사용 하 여 감정 인식
description: Face API 얼굴 표현을 입력으로 이미지에서는 및의 이미지에 있는 각 얼굴의 감정 집합 간에 신뢰 수준을 포함 하는 데이터를 반환 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램을 평가 하려면 감정 인식 하도록 Face API를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: d703de90378991d262a4b056b9ebc98d183e3fb8
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056492"
---
# <a name="emotion-recognition-using-the-face-api"></a>Face API를 사용 하 여 감정 인식

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)

_Face API 얼굴 표현을 입력으로 이미지에서는 및의 이미지에 있는 각 얼굴의 감정 집합 간에 신뢰 수준을 포함 하는 데이터를 반환 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램을 평가 하려면 감정 인식 하도록 Face API를 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Face API 얼굴 식에 분노, 경 멸, 혐오, 공포, 행복, 중립 검색할 감정 감지, 슬픔 및 놀람을 수행할 수 있습니다. 이러한 감정은 보편적으로 및 문화 기본 동일한 표정을 통해 전달 됩니다. 식의 얼굴 감정을 결과 반환, 뿐만 아니라 Face API 수도 반환 검색 된 얼굴에 대 한 경계 상자입니다. Face API를 사용 하려면 API 키를 받아야 하는 참고 합니다. 가져올 수 있습니다 [Cognitive Services 시도](https://azure.microsoft.com/try/cognitive-services/?api=face-api)합니다.

감정 인식 클라이언트 라이브러리 및 REST API를 통해 수행할 수 있습니다. 이 문서에서는 REST API를 통해 감정 인식을 수행에 중점을 둡니다. REST API에 대 한 자세한 내용은 참조 하세요. [Face REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)합니다.

Face API 비디오에서 사람의 얼굴 표정을 인식 하도 사용할 수 있으며 한 사람의 감정 요약해 서를 반환할 수 있습니다. 자세한 내용은 [실시간 비디오 분석 방법](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/)합니다.

Face API에 대 한 자세한 내용은 참조 하세요. [Face API](/azure/cognitive-services/face/overview/)합니다.

## <a name="authentication"></a>인증

Face API에 대 한 모든 요청 값으로 지정 해야 하는 API 키가 필요 합니다 `Ocp-Apim-Subscription-Key` 헤더입니다. 다음 코드 예제에는 API 키를 추가 하는 방법을 보여 줍니다는 `Ocp-Apim-Subscription-Key` 요청 헤더:

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Face API에 유효한 API 키를 전달 하는 오류 401 응답 오류가 발생 합니다.

## <a name="performing-emotion-recognition"></a>감정 인식을 수행

감정 인식 이미지가 포함 된 POST 요청을 만들어 수행 됩니다 합니다 `detect` API `https://[location].api.cognitive.microsoft.com/face/v1.0`여기서 `[location]]` API 키를 가져오는 데 사용할 영역입니다. 선택적 요청 매개 변수를 다음과 같습니다.

- `returnFaceId` -검색 된 얼굴 faceIds를 반환할지 여부를 합니다. 기본값은 `true`입니다.
- `returnFaceLandmarks` – 중 검색 된 얼굴 랜드마크를 반환할지 여부를 합니다. 기본값은 `false`입니다.
- `returnFaceAttributes` – 분석 하 고 하나 이상의 지정 된 반환 것인지 얼굴 특성입니다. 지원 되는 얼굴 특성 포함 `age`, `gender`, `headPose`, `smile`, `facialHair`를 `glasses`, `emotion`, `hair`, `makeup`를 `occlusion`, `accessories`, `blur`하십시오 `exposure`, 및 `noise`합니다. 얼굴 특성 분석에 추가 계산 시간 및 비용을 참고 합니다.

이미지 콘텐츠 URL 또는 이진 데이터를 POST 요청의 본문에 배치 되어야 합니다.

> [!NOTE]
> 지원 되는 이미지 파일은 JPEG, PNG, GIF 및 BMP, 및 허용 되는 파일 크기가 1KB에서 4mb입니다.

샘플 응용 프로그램에서 감정 인식 프로세스를 호출한 호출 된 `DetectAsync` 메서드:

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

이 메서드 호출은 faceIds 반환 되어야 하는, 해당 얼굴 랜드마크 반환 하지 않아야 하 고 이미지의 감정 분석 해야 하도록 이미지 데이터를 포함 하는 스트림을 지정 합니다. 또한 결과 배열로 반환될지 지정 `Face` 개체입니다. 차례로 합니다 `DetectAsync` 메서드를 호출 하는 `detect` 감정 인식을 수행 하는 REST API:

```csharp
public async Task<Face[]> DetectAsync(Stream imageStream, bool returnFaceId, bool returnFaceLandmarks, IEnumerable<FaceAttributeType> returnFaceAttributes)
{
  var requestUrl =
    $"{Constants.FaceEndpoint}/detect?returnFaceId={returnFaceId}" +
    "&returnFaceLandmarks={returnFaceLandmarks}" +
    "&returnFaceAttributes={GetAttributeString(returnFaceAttributes)}";
  return await SendRequestAsync<Stream, Face[]>(HttpMethod.Post, requestUrl, imageStream);
}
```

이 메서드는 요청 URI를 생성 하 고 요청을 보냅니다 합니다 `detect` 를 통해 API를 `SendRequestAsync` 메서드.

> [!NOTE]
> 동일한 지역으로 구독 키를 가져오는 데 사용 하 여 Face API 호출에 사용 해야 합니다. 예를 들어에서 구독 키를 구입한 경우 합니다 `westus` 얼굴 검색 끝점 수는 지역 `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`합니다.

### <a name="sending-the-request"></a>요청을 보내기

합니다 `SendRequestAsync` Face API에 POST 요청 메서드와으로 결과 반환을 `Face` 배열:

```csharp
async Task<TResponse> SendRequestAsync<TRequest, TResponse>(HttpMethod httpMethod, string requestUrl, TRequest requestBody)
{
  var request = new HttpRequestMessage(httpMethod, Constants.FaceEndpoint);
  request.RequestUri = new Uri(requestUrl);
  if (requestBody != null)
  {
    if (requestBody is Stream)
    {
      request.Content = new StreamContent(requestBody as Stream);
      request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
    }
    else
    {
      // If the image is supplied via a URL
      request.Content = new StringContent(JsonConvert.SerializeObject(requestBody, s_settings), Encoding.UTF8, "application/json");
    }
  }

  HttpResponseMessage responseMessage = await _client.SendAsync(request);
  if (responseMessage.IsSuccessStatusCode)
  {
    string responseContent = null;
    if (responseMessage.Content != null)
    {
      responseContent = await responseMessage.Content.ReadAsStringAsync();
    }
    if (!string.IsNullOrWhiteSpace(responseContent))
    {
      return JsonConvert.DeserializeObject<TResponse>(responseContent, s_settings);
    }
    return default(TResponse);
  }
  else
  {
    ...
  }
  return default(TResponse);
}
```

이미지 스트림을 래핑하여 메서드 POST 요청에는 빌드 이미지를 스트림을 통해 제공 하면를 `StreamContent` 스트림에 따라 HTTP 콘텐츠를 제공 하는 경우. 또는 이미지 URL을 통해 제공 되 면, 메서드 작성 POST 요청 URL에 래핑하여는 `StringContent` 문자열을 기반으로 HTTP 콘텐츠를 제공 하는 경우.

그런 다음 POST 요청을 보낼 `detect` API. 응답을 deserialize 읽고 호출 메서드로 반환 합니다.

`detect` API에서 HTTP 상태 코드 200 (정상) 요청 올바른지 요청이 성공 했는지를 나타내는 응답에 요청 된 정보를 제공 하 고 응답을 보냅니다. 가능한 오류 응답의 목록을 참조 하세요 [Face REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)합니다.

### <a name="processing-the-response"></a>응답 처리

API 응답은 JSON 형식으로 반환 됩니다. 다음 JSON 데이터에는 샘플 응용 프로그램에서 요청한 데이터를 제공 하는 일반적인 성공 응답 메시지를 보여 줍니다.

```json
[  
   {  
      "faceId":"8a1a80fe-1027-48cf-a7f0-e61c0f005051",
      "faceRectangle":{  
         "top":192,
         "left":164,
         "width":339,
         "height":339
      },
      "faceAttributes":{  
         "emotion":{  
            "anger":0.0,
            "contempt":0.0,
            "disgust":0.0,
            "fear":0.0,
            "happiness":1.0,
            "neutral":0.0,
            "sadness":0.0,
            "surprise":0.0
         }
      }
   }
]
```

성공적인 응답 메시지의 내림차순으로 빈 응답은 검색 된 얼굴이 없음을 나타냄 얼굴 사각형 크기에 따라 순위를 지정 하는 얼굴 항목 배열을 이루어져 있습니다. 각 얼굴에 의해 지정 되는 선택적 얼굴 특성을 인식 합니다 `returnFaceAttributes` 인수는 `DetectAsync` 메서드.

샘플 응용 프로그램에서 JSON 응답의 배열으로 deserialize `Face` 개체입니다. Face API의 결과 해석 하는 경우 검색 된 감정을으로 해석할지 점수가 가장 높은 감정 점수는 정규화 된로 하나에 대 한 합계를 합니다. 따라서 샘플 응용 프로그램 이미지의 최대 검색 된 얼굴에 대 한 점수가 가장 높은 인식된 emotion을 표시합니다. 이 작업은 다음 코드를 사용 하 여 수행 됩니다.

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

다음 스크린샷은 샘플 응용 프로그램에서 감정 인식 프로세스의 결과 보여 줍니다.

![](emotion-recognition-images/emotion-recognition.png "감정 인식")

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms 응용 프로그램을 평가 하려면 감정 인식 하도록 Face API를 사용 하는 방법을 설명 합니다. Face API 얼굴 표현을 입력으로 이미지에서는 및의 이미지에 있는 각 얼굴의 감정 집합 간에 신뢰도 포함 하는 데이터를 반환 합니다.

## <a name="related-links"></a>관련 링크

- [Face API](/azure/cognitive-services/face/overview/)합니다.
- [Todo Cognitive Services (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Face REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
