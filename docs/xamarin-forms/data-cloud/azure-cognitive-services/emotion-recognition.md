---
제목: "Face API를 사용 하는 인식 된 Emotion 인식" 설명: "Face API는 이미지에서 얼굴 식을 입력으로 사용 하 고 이미지의 각 면에 대 한 감정을 집합에 대 한 신뢰도 수준을 포함 하는 데이터를 반환 합니다. 이 문서에서는 Face API를 사용 하 여 응용 프로그램을 평가 하는 emotion를 인식 하는 방법을 설명 Xamarin.Forms 합니다.
assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A: xamarin-forms author: davidbritch: dabritch:: 05/10/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="perceived-emotion-recognition-using-the-face-api"></a>Face API를 사용 하는 인식 된 Emotion 인식

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

이 Face API는 휴먼 coders의 인식 된 주석을 기반으로 하는 얼굴 식에서 분노, 경 멸, 혐오, emotion, 행복, 중립, sadness을 검색 하는 데 검색을 수행할 수 있습니다. 그러나 얼굴 식 만으로는 사용자의 내부 상태를 반드시 나타내는 것은 아닙니다.

얼굴 식에 대 한 emotion 결과를 반환 하는 것 외에도, 검색 Face API 된 면에 대 한 경계 상자를 반환할 수 있습니다.

Emotion 인식은 클라이언트 라이브러리를 통해, REST API을 통해 수행할 수 있습니다. 이 문서에서는 REST API를 통해 emotion 인식 작업을 수행 하는 방법을 집중적으로 설명 합니다. REST API에 대 한 자세한 내용은 [Face REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)를 참조 하세요.

Face API를 사용 하 여 비디오의 얼굴 얼굴을 인식 하 고 해당 감정을에 대 한 요약을 반환할 수도 있습니다. 자세한 내용은 [실시간으로 비디오를 분석 하는 방법](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/)을 참조 하세요.

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다.

Face API를 사용 하려면 API 키를 가져와야 합니다. 이는 [Try Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api)에서 얻을 수 있습니다.

Face API에 대 한 자세한 내용은 [Face API](/azure/cognitive-services/face/overview/)을 참조 하세요.

## <a name="authentication"></a>인증

Face API에 대 한 모든 요청에는 헤더의 값으로 지정 해야 하는 API 키가 필요 합니다 `Ocp-Apim-Subscription-Key` . 다음 코드 예제에서는 요청 헤더에 API 키를 추가 하는 방법을 보여 줍니다 `Ocp-Apim-Subscription-Key` .

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Face API에 유효한 API 키를 전달 하지 못하면 401 응답 오류가 발생 합니다.

## <a name="perform-emotion-recognition"></a>Emotion 인식 수행

Emotion 인식은에서 api에 이미지를 포함 하는 POST 요청을 수행 하 여 수행 됩니다 `detect` `https://[location].api.cognitive.microsoft.com/face/v1.0` `[location]]` . 여기서는 api 키를 가져오는 데 사용 된 지역입니다. 선택적 요청 매개 변수는 다음과 같습니다.

- `returnFaceId`– 검색 된 면의 faceIds 반환할지 여부입니다. 기본값은 `true`입니다.
- `returnFaceLandmarks`– 검색 된 면의 얼굴 랜드마크 반환할지 여부입니다. 기본값은 `false`입니다.
- `returnFaceAttributes`– 하나 이상의 지정 된 얼굴 특성을 분석 하 여 반환할지 여부입니다. 지원 되는 face 특성에는,,,,,,,,,,,,, 등이 `age` `gender` `headPose` `smile` `facialHair` `glasses` `emotion` `hair` `makeup` `occlusion` `accessories` `blur` `exposure` `noise` 있습니다. 얼굴 특성 분석에는 추가 계산 및 시간 비용이 있습니다.

이미지 콘텐츠는 POST 요청의 본문에 URL 또는 이진 데이터로 배치 되어야 합니다.

> [!NOTE]
> 지원 되는 이미지 파일 형식은 JPEG, PNG, GIF 및 BMP 이며 허용 되는 파일 크기는 1KB에서 4MB입니다.

샘플 응용 프로그램에서 메서드를 호출 하 여 emotion 인식 프로세스를 호출 합니다 `DetectAsync` .

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

이 메서드 호출은 이미지 데이터를 포함 하는 스트림을 지정 합니다 .이는 faceIds을 반환 해야 하 고 랜드마크은 반환 되지 않으며 이미지의 emotion를 분석 해야 함을 나타냅니다. 또한 결과가 개체의 배열로 반환 됨을 지정 합니다 `Face` . 그러면 `DetectAsync` 메서드는 `detect` emotion 인식을 수행 하는 REST API를 호출 합니다.

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

이 메서드는 요청 URI를 생성 한 다음 `detect` 메서드를 통해 요청을 API로 보냅니다 `SendRequestAsync` .

> [!NOTE]
> 구독 키를 가져오는 데 사용 하는 것 처럼 Face API 호출에서 동일한 지역을 사용 해야 합니다. 예를 들어 지역에서 구독 키를 가져온 경우 `westus` 얼굴 감지 끝점은 `https://westus.api.cognitive.microsoft.com/face/v1.0/detect` 입니다.

### <a name="send-the-request"></a>요청 보내기

`SendRequestAsync`메서드는 Face API에 대 한 POST 요청을 수행 하 고 결과를 배열로 반환 합니다 `Face` .

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

스트림을 통해 이미지를 제공 하는 경우 메서드는 `StreamContent` 스트림에 따라 HTTP 콘텐츠를 제공 하는 인스턴스에 이미지 스트림을 래핑하여 POST 요청을 빌드합니다. 또는 URL을 통해 이미지를 제공 하는 경우 메서드는 `StringContent` 문자열에 따라 HTTP 콘텐츠를 제공 하는 인스턴스에 url을 래핑하여 POST 요청을 빌드합니다.

그런 다음 POST 요청을 API로 보냅니다 `detect` . 응답을 읽고 deserialize 한 다음 호출 메서드로 반환 합니다.

`detect`요청이 성공 했으며 요청 된 정보가 응답에 있음을 나타내는 경우 API는 응답에서 HTTP 상태 코드 200 (OK)를 전송 합니다. 가능한 오류 응답 목록은 [Face REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)를 참조 하세요.

### <a name="process-the-response"></a>응답 처리

API 응답은 JSON 형식으로 반환 됩니다. 다음 JSON 데이터는 샘플 응용 프로그램에서 요청한 데이터를 제공 하는 일반적인 성공적인 응답 메시지를 보여 줍니다.

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

성공적인 응답 메시지는 얼굴 사각형 크기를 기준으로 내림차순으로 순위를 지정 하는 표면 항목의 배열로 구성 되 고, 빈 응답은 검색 된 얼굴 없음을 나타냅니다. 각 인식 된 면에는 메서드에 대 한 인수로 지정 되는 일련의 선택적 face 특성이 포함 됩니다 `returnFaceAttributes` `DetectAsync` .

샘플 응용 프로그램에서 JSON 응답은 개체의 배열로 deserialize 됩니다 `Face` . Face API에서 결과를 해석 하는 경우 검색 된 emotion은 점수가 합계로 정규화 되므로 점수가 가장 높은 emotion로 해석 됩니다. 따라서 샘플 응용 프로그램은 이미지에서 가장 큰 검색 된 표면에 대해 점수가 가장 높은 인식 된 emotion를 표시 합니다. 다음 코드로 이 작업을 수행합니다.

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

다음 스크린샷은 샘플 응용 프로그램에서 emotion 인식 프로세스의 결과를 보여줍니다.

![](emotion-recognition-images/emotion-recognition.png "Emotion Recognition")

## <a name="related-links"></a>관련 링크

- [Face API](/azure/cognitive-services/face/overview/).
- [Todo Cognitive Services (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [얼굴 REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
