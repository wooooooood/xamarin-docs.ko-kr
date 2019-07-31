---
title: Microsoft Speech API를 사용 하 여 음성 인식
description: Microsoft Speech API는 음성된 언어를 처리 하는 알고리즘을 제공 하는 클라우드 기반 API입니다. 이 문서에는 오디오 Xamarin.Forms 응용 프로그램에서 텍스트를 변환 하려면 Microsoft Speech Recognition REST API를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 97997a527647ae972eadff47da8c1321d5d55daa
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655462"
---
# <a name="speech-recognition-using-the-microsoft-speech-api"></a>Microsoft Speech API를 사용 하 여 음성 인식

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Speech API는 음성된 언어를 처리 하는 알고리즘을 제공 하는 클라우드 기반 API입니다. 이 문서에는 오디오 Xamarin.Forms 응용 프로그램에서 텍스트를 변환 하려면 Microsoft Speech Recognition REST API를 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Microsoft Speech API에는 두 가지 구성 요소에 있습니다.

- 음성 텍스트 변환 하는 데는 음성 인식 API. 음성 인식 서비스 라이브러리, REST API 또는 클라이언트 라이브러리를 통해 수행할 수 있습니다.
- 텍스트를 음성 API 음성 텍스트 변환에 대 한 합니다. 텍스트 음성 변환 REST API를 통해 수행 됩니다.

이 문서에서는 REST API를 통해 음성 인식을 수행에 중점을 둡니다. 클라이언트 및 서비스 라이브러리를 지원 하지만 일부 결과 반환, REST API만 부분 결과 없이 단일 인식 결과 반환할 수 있습니다.

Microsoft Speech API를 사용 하는 API 키를 가져와야 합니다. Azure에서 가져올 수 있습니다 [포털](https://portal.azure.com/)합니다. 자세한 내용은 [Azure portal에서 Cognitive Services 계정을 만드는](/azure/cognitive-services/cognitive-services-apis-create-account)합니다.

Microsoft Speech API에 대 한 자세한 내용은 참조 하세요. [Microsoft Speech API 설명서](/azure/cognitive-services/speech/home/)합니다.

## <a name="authentication"></a>인증

Microsoft Speech REST API에 대 한 모든 요청에서 cognitive 서비스 토큰 서비스에서 가져올 수 있는 JSON 웹 토큰 (JWT) 액세스 토큰을 필요한 `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`합니다. 토큰 서비스에 POST 요청을 수행 하 여 토큰을 가져올 수 있습니다 지정 하는 `Ocp-Apim-Subscription-Key` 값으로 API 키가 포함 된 헤더입니다.

다음 코드 예제에서는 토큰 서비스에서 토큰 액세스를 요청 하는 방법을 보여 줍니다.

```csharp
public AuthenticationService(string apiKey)
{
    subscriptionKey = apiKey;
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
}
...
async Task<string> FetchTokenAsync(string fetchUri)
{
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";
    var result = await httpClient.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
}
```

Base64 텍스트를 반환된 하는 액세스 토큰을 만료 시간을 10 분에 있습니다. 따라서 샘플 응용 프로그램 9 분 마다 액세스 토큰을 갱신합니다.

각 Microsoft Speech REST API 액세스 토큰을 지정 해야로 호출을 `Authorization` 문자열을 접두사로 하는 헤더 `Bearer`다음 코드 예제에 나와 있는 것 처럼:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

유효한 액세스 토큰을 Microsoft Speech REST API에 전달할 오류 403 응답 오류가 발생 합니다.

## <a name="performing-speech-recognition"></a>음성 인식을 수행

POST 요청을 수행 하 여 음성 인식 이루어집니다 합니다 `recognition` API에 `https://speech.platform.bing.com/speech/recognition/`입니다. 단일 요청에 오디오를 10 초 이상 포함할 수 없습니다 및 총 요청 기간이 14 초를 초과할 수 없습니다.

오디오 콘텐츠 wav 형식 요청 POST 본문에 배치 되어야 합니다.

샘플 응용 프로그램에서을 `RecognizeSpeechAsync` 음성 인식 프로세스를 호출 하는 메서드:

```csharp
public async Task<SpeechResult> RecognizeSpeechAsync(string filename)
{
    ...

    // Read audio file to a stream
    var file = await PCLStorage.FileSystem.Current.LocalStorage.GetFileAsync(filename);
    var fileStream = await file.OpenAsync(PCLStorage.FileAccess.Read);

    // Send audio stream to Bing and deserialize the response
    string requestUri = GenerateRequestUri(Constants.SpeechRecognitionEndpoint);
    string accessToken = authenticationService.GetAccessToken();
    var response = await SendRequestAsync(fileStream, requestUri, accessToken, Constants.AudioContentType);
    var speechResult = JsonConvert.DeserializeObject<SpeechResult>(response);

    fileStream.Dispose();
    return speechResult;
}
```

오디오 wav PCM 데이터와 각 플랫폼별 프로젝트에 기록 됩니다 및 `RecognizeSpeechAsync` 메서드는 `PCLStorage` NuGet 패키지를 스트림으로 오디오 파일을 엽니다. URI에서 생성 되는 음성 인식 요청 및 액세스 토큰은 토큰 서비스에서 검색 됩니다. 음성 인식 요청에 게시 되는 `recognition` 결과 포함 하는 JSON 응답을 반환 하는 API입니다. 표시에 대 한 호출 메서드로 반환 되는 결과 사용 하 여 JSON 응답을 deserialize 됩니다.

### <a name="configuring-speech-recognition"></a>음성 인식을 구성합니다.

음성 인식 프로세스는 다음과 같은 HTTP 쿼리 매개 변수를 지정 하 여 구성할 수 있습니다.

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    // To build a request URL, you should follow:
    // https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest
    string requestUri = speechEndpoint;
    requestUri += @"dictation/cognitiveservices/v1?";
    requestUri += @"language=en-us";
    requestUri += @"&format=simple";
    System.Diagnostics.Debug.WriteLine(requestUri.ToString());
    return requestUri;
}
```

수행한 기본 구성의 `GenerateRequestUri` 방법은 오디오 콘텐츠는의 로캘을 설정 하는 것입니다. 지원 되는 로캘 목록은 [지원 되는 언어](/azure/cognitive-services/speech/api-reference-rest/supportedlanguages/)를 참조 하세요.

### <a name="sending-the-request"></a>요청을 보내기

`SendRequestAsync` 메서드 Microsoft Speech REST API에 POST 요청을 수행 하 고 응답을 반환 합니다.

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);
    var response = await httpClient.PostAsync(url, content);
    return await response.Content.ReadAsStringAsync();
}
```

이 메서드는 POST 요청이 작성합니다.

- 오디오 스트림에 배치를 `StreamContent` 스트림에 따라 HTTP 콘텐츠를 제공 하는 경우.
- 설정 된 `Content-Type` 요청 헤더 `audio/wav; codec="audio/pcm"; samplerate=16000`합니다.
- 액세스 토큰을 추가 합니다 `Authorization` 문자열을 접두사로 헤더 `Bearer`합니다.

그런 다음 POST 요청을 보낼 `recognition` API. 그런 다음 응답은 읽고 호출 메서드로 반환 됩니다.

`recognition` API에서 HTTP 상태 코드 200 (정상) 요청 올바른지 요청이 성공 했는지를 나타내는 응답에 요청 된 정보를 제공 하 고 응답을 보냅니다. 가능한 오류 응답의 목록을 참조 하세요 [문제 해결](/azure/cognitive-services/speech/troubleshooting)합니다.

### <a name="processing-the-response"></a>응답 처리

API 응답에 포함 되 고 인식된 된 텍스트를 사용 하 여 JSON 형식으로 반환 됩니다는 `name` 태그입니다. 다음 JSON 데이터는 일반적으로 성공적인 응답 메시지가 표시 됩니다.

```json
{  
   "RecognitionStatus":"Success",
   "DisplayText":"Go shopping tomorrow.",
   "Offset":16000000,
   "Duration":17100000
}
```

샘플 응용 프로그램에서 JSON 응답으로 deserialize 하는 `SpeechResult` 다음 스크린샷과에서 같이 표시에 대 한 호출 메서드로 반환 되는 결과 사용 하 여 인스턴스:

![](speech-recognition-images/speech-recognition.png "음성 인식")

## <a name="summary"></a>요약

이 문서에는 오디오 Xamarin.Forms 응용 프로그램에서 텍스트를 변환 하려면 Microsoft Speech REST API를 사용 하는 방법을 설명 합니다. 음성 인식 기능을 수행 하는 것 외에도 Microsoft Speech API를 음성된 단어로 텍스트를 변환할 수도 있습니다.

## <a name="related-links"></a>관련 링크

- [Microsoft Speech API 설명서](/azure/cognitive-services/speech/home/)합니다.
- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Todo Cognitive Services (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
