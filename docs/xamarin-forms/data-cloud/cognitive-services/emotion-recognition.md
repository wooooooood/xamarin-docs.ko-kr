---
title: Face API를 사용 하 여 emotion 인식
description: Face API를 입력으로 이미지에는 얼굴 식 하며 감정 각 면 이미지에 대 한 조합에 대해 신뢰도 수준을 포함 하는 데이터를 반환 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램을 평가 하려면 emotion 인식 하도록 Face API를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 4dc04cb077b894b255eb496b2cb2983626573897
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34049768"
---
# <a name="emotion-recognition-using-the-face-api"></a>Face API를 사용 하 여 emotion 인식

_Face API를 입력으로 이미지에는 얼굴 식 하며 감정 각 면 이미지에 대 한 조합에 대해 신뢰도 수준을 포함 하는 데이터를 반환 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램을 평가 하려면 emotion 인식 하도록 Face API를 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Face API 얼굴 식에서 emotion 분노, 컨 템, disgust, 걱정, 중립 만족도 검색 하는 검색, 슬픔, 및 놀랄 수행할 수 있습니다. 이러한 감정은 보편적으로 및 문화 기본 동일한 얼굴 식을 통해 전달 됩니다. 얼굴 식에 대 한는 emotion 결과 반환 하는 데 뿐만 아니라 Face API 수도 반환에 대해 검색 된 경계 상자. Face API를 사용 하면 API 키를 얻어야 하는 참고 합니다. 다운로드할 수 있습니다 [Cognitive 서비스 시도](https://azure.microsoft.com/try/cognitive-services/?api=face-api)합니다.

Emotion 인식 클라이언트 라이브러리를 통해 및 REST API를 통해 수행할 수 있습니다. 이 문서에서는 REST API를 통해 emotion 인식을 수행 합니다. REST API에 대 한 자세한 내용은 참조 [얼굴 REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)합니다.

Face API 비디오에서는 사용자의 얼굴 식을 인식 하도록도 사용할 수 있으며 해당 감정의 요약을 반환할 수 있습니다. 자세한 내용은 참조 [실시간에서 비디오를 분석 하는 방법을](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/)합니다.

Face API에 대 한 자세한 내용은 참조 [Face API](/azure/cognitive-services/face/overview/)합니다.

## <a name="authentication"></a>인증

Face API에 대 한 모든 요청 필요 하면 API 키의 값으로 지정 해야 하는 `Ocp-Apim-Subscription-Key` 헤더입니다. 다음 코드 예제에서는 API 키를 추가 하는 방법을 보여 줍니다.는 `Ocp-Apim-Subscription-Key` 요청 헤더의:

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Face API를 올바른 API 키를 전달 하는 오류 401 응답 오류가 발생 합니다.

## <a name="performing-emotion-recognition"></a>Emotion 인식을 수행

Emotion 인식 이미지가 포함 된 POST 요청 하 여 수행 되는 `detect` API에 `https://[location].api.cognitive.microsoft.com/face/v1.0`여기서 `[location]]` API 키를 가져오는 데 영역입니다. 선택적인 요청 매개 변수는.

- `returnFaceId` -검색 된 면 faceIds 반환할 것인지 합니다. 기본값은 `true`입니다.
- `returnFaceLandmarks` -검색 된 면 얼굴 이정표 반환할 것인지 합니다. 기본값은 `false`입니다.
- `returnFaceAttributes` – 특성에 맞서게 여부를 분석 하 고 지정 된 하나 이상의 반환 합니다. 지원 되 면 특성에는 `age`, `gender`, `headPose`, `smile`, `facialHair`, `glasses`, `emotion`, `hair`, `makeup`, `occlusion`, `accessories`, `blur`, `exposure`, 및 `noise`합니다. 글꼴 특성 분석 시간 및 계산에 대 한 추가 비용에 있는지 확인 합니다.

이미지 콘텐츠 URL 또는 이진 데이터로 POST 요청의 본문에 배치 되어야 합니다.

> [!NOTE]
> 지원 되는 이미지 파일 형식 JPEG, PNG, GIF, 및 BMP, 이며 허용 되는 파일 크기가 1KB에서 4mb입니다.

샘플 응용 프로그램에서 emotion 인식 프로세스가 호출 하 여 호출 됩니다는 `DetectAsync` 메서드:

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

이 메서드 호출에는 faceIds 되돌려야, 얼굴 이정표 안 반환 하 고 이미지의 emotion 분석 되어야 하는 이미지 데이터가 포함 된 스트림을 지정 합니다. 또한 결과의 배열의 형태로 돌아갑니다 지정 `Face` 개체입니다. 차례로 `DetectAsync` 메서드가 호출 하는 `detect` emotion 인식을 수행 하는 REST API:

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

이 메서드는 요청 URI를 생성 하 고 다음 요청을 보냅니다는 `detect` 를 통해 API는 `SendRequestAsync` 메서드.

> [!NOTE]
> 동일한 지역으로 구독 키를 가져오는 데 사용 하 여 얼굴 API 호출에서 사용 해야 합니다. 예를 들어, 사용자 구독 키를 구입한 경우는 `westus` 영역 얼굴 감지 끝점 됩니다 `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`합니다.

### <a name="sending-the-request"></a>요청을 보내기

`SendRequestAsync` 메서드 Face API에 POST 요청을 만들고으로 결과 반환는 `Face` 배열:

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

메서드를 POST 요청에 대 한 이미지 스트림을 래핑하고 작성 이미지 스트림을 통해 제공 되는 경우는 `StreamContent` 스트림에 따라 HTTP 콘텐츠를 제공 하는 인스턴스. 또는 이미지 URL을 통해 제공 되는 경우 메서드를 작성 POST 요청에 URL에 래핑하여는 `StringContent` 문자열에 따라 HTTP 콘텐츠를 제공 하는 인스턴스.

POST 요청에 전송 됩니다 `detect` API입니다. 응답 읽기, deserialize 및 호출 하는 메서드로 반환 합니다.

`detect` API에서 HTTP 상태 코드 200 (정상) 요청이 올바른지는 요청이 성공 했음을 나타내는 응답에 요청 된 정보를 제공 하 고 응답을 보냅니다. 목록이 가능한 오류 응답에 대 한 참조 [얼굴 REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)합니다.

### <a name="processing-the-response"></a>가 응답 처리

API 응답은 JSON 형식으로 반환 합니다. 다음 JSON 데이터 샘플 응용 프로그램에서 요청한 데이터를 제공 하는 일반적인 성공 응답 메시지를 보여 줍니다.

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

성공적인 응답 메시지 면 항목을 기준으로 내림차순으로 빈 응답 없음 얼굴 감지 이면 얼굴 사각형 크기의 배열으로 이루어져 있습니다. 글꼴 포함 일련의 선택적 글꼴 특성에서 지정 된 각 인식는 `returnFaceAttributes` 인수에는 `DetectAsync` 메서드.

배열으로 deserialize 하는 JSON 응답 예제 응용 프로그램에서는 `Face` 개체입니다. Face API에서 결과 해석 하는 경우 검색 된 emotion으로 해석할지 점수가 가장 높은와 emotion 점수를 기본 형태로 변환 하는 대로 하나에 대 한 합계를 구할 합니다. 따라서 샘플 응용 프로그램 이미지에는 가장 큰 검색 된 글꼴에 대 한 가장 높은 점수를 인식 된 emotion를 표시합니다. 다음 코드로 작업을 수행 합니다.

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

다음 스크린 샷에서 샘플 응용 프로그램에서 emotion 인식 프로세스의 결과 보여 줍니다.

![](emotion-recognition-images/emotion-recognition.png "Emotion 인식")

## <a name="summary"></a>요약

이 문서는 Xamarin.Forms 응용 프로그램을 평가 하려면 emotion 인식 하도록 Face API를 사용 하는 방법을 설명 합니다. Face API 얼굴 식을 입력으로 이미지의 하며 이미지의 각 면에 대해 감정의 신뢰성을 포함 하는 데이터를 반환 합니다.

## <a name="related-links"></a>관련 링크

- [API에 맞서게](/azure/cognitive-services/face/overview/)합니다.
- [Todo Cognitive 서비스 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Face REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
