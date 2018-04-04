---
title: Microsoft 음성 API를 사용 하 여 음성 인식
description: Microsoft 음성 API는 통용된 언어를 처리 하기 위한 알고리즘을 제공 하는 클라우드 기반 API입니다. 이 문서에서는 Microsoft 음성 인식 REST API를 사용 하 여 오디오 Xamarin.Forms 응용 프로그램에서 텍스트를 변환 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 81e645749d239f8964047e92255e786c9b35409d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="speech-recognition-using-the-microsoft-speech-api"></a>Microsoft 음성 API를 사용 하 여 음성 인식

_Microsoft 음성 API는 통용된 언어를 처리 하기 위한 알고리즘을 제공 하는 클라우드 기반 API입니다. 이 문서에서는 Microsoft 음성 인식 REST API를 사용 하 여 오디오 Xamarin.Forms 응용 프로그램에서 텍스트를 변환 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Microsoft 음성 API의 두 구성 요소:

- 단어를 텍스트로 변환 된 음성 인식 API입니다. 음성 인식 REST API, 클라이언트 라이브러리 또는 서비스 라이브러리를 통해 수행할 수 있습니다.
- 한 개의 문자 음성 변환 API 텍스트 음성 변환에 대 한 합니다. REST API를 통해 개의 문자 음성 변환 변환이 수행 됩니다.

이 문서에서는 REST API를 통해 음성 인식을 수행 합니다. 클라이언트 및 서비스 라이브러리에서 부분 결과 반환를 지원 하지만 REST API만 일부 결과 없이 단일 인식 결과 반환할 수 있습니다.

Microsoft 음성 API를 사용 하면 API 키를 받아야 합니다. 다운로드할 수 있습니다 [Cognitive 서비스 시도](https://azure.microsoft.com/try/cognitive-services/)합니다.

Microsoft 음성 API에 대 한 자세한 내용은 참조 [Microsoft 음성 API 설명서](/azure/cognitive-services/speech/home/)합니다.

## <a name="authentication"></a>인증

Microsoft 음성 REST API에 대 한 모든 요청에서 cognitive 서비스 토큰 서비스에서 가져올 수 있는 JSON 웹 토큰 (JWT) 액세스 토큰이, 필요한 `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`합니다. 토큰 서비스에 POST 요청을 하 여 토큰을 가져올 수 있습니다를 지정 하는 `Ocp-Apim-Subscription-Key` 해당 값으로 API 키가 포함 된 헤더입니다.

다음 코드 예제에서는 토큰 토큰 서비스에서 액세스를 요청 하는 방법을 보여 줍니다.

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

Base64 텍스트는 반환 된 액세스 토큰 만료 시간을 10 분에 있습니다. 따라서 샘플 응용 프로그램 9 분 마다 액세스 토큰을 갱신합니다.

각 Microsoft 음성 REST API에 액세스 토큰을 지정 해야 라고는 `Authorization` 헤더 문자열을 접두사로 `Bearer`다음 코드 예제에 나온 것 처럼:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

유효한 액세스 토큰을 Microsoft 음성 REST API에 전달 하는 오류 403 응답 오류가 발생 합니다.

## <a name="performing-speech-recognition"></a>음성 인식을 수행

음성 인식은 POST 요청을 수행 하 여 달성는 `recognition` API에 `https://speech.platform.bing.com/speech/recognition/`합니다. 단일 요청에는 오디오의 10 초 이상 포함할 수 없습니다 및 총 요청 기간을 14 초 초과할 수 없습니다.

오디오 콘텐츠 wav 형태로 표시 요청은 게시물 본문에 배치 되어야 합니다.

샘플 응용 프로그램에서의 `RecognizeSpeechAsync` 음성 인식 프로세스를 호출 하는 메서드:

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

기록 하는 PCM 웨이브 데이터와 각 플랫폼별 프로젝트에 및 `RecognizeSpeechAsync` 메서드는 `PCLStorage` NuGet 패키지를 스트림으로 오디오 파일을 엽니다. 음성 인식 요청 URI에서 생성 되 고 액세스 토큰은 토큰 서비스에서 검색 됩니다. 음성 인식 요청에 게시 되는 `recognition` API는 결과 포함 하는 JSON 응답을 반환 합니다. JSON 응답에 표시 하기 위해 호출 하는 메서드에서 반환 되는 결과와 deserialize 됩니다.

### <a name="configuring-speech-recognition"></a>음성 인식을 구성

HTTP 쿼리 매개 변수를 지정 하 여 음성 인식 프로세스를 구성할 수 있습니다.

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    // To build a request URL, you should follow:
    // https://docs.microsoft.com/en-us/azure/cognitive-services/speech/getstarted/getstartedrest
    string requestUri = speechEndpoint;
    requestUri += @"dictation/cognitiveservices/v1?";
    requestUri += @"language=en-us";
    requestUri += @"&format=simple";
    System.Diagnostics.Debug.WriteLine(requestUri.ToString());
    return requestUri;
}
```

수행 하는 주요 구성은 `GenerateRequestUri` 방법은 오디오 콘텐츠의 로캘을 설정 하는 것입니다. 지원 되는 로캘 목록은 참조 [지원 되는 언어 ](/azure/cognitive-services/speech/api-reference-rest/supportedlanguages/)합니다.

### <a name="sending-the-request"></a>요청을 보내기

`SendRequestAsync` 메서드 Microsoft 음성 REST API에 POST 요청을 수행 하 고 응답을 반환 합니다.

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

이 메서드는 여 POST 요청을 작성합니다.

- 래핑에 오디오 스트림이 `StreamContent` 스트림에 따라 HTTP 콘텐츠를 제공 하는 인스턴스.
- 설정의 `Content-Type` 요청의 헤더 `audio/wav; codec="audio/pcm"; samplerate=16000`합니다.
- 액세스 토큰을 추가 `Authorization` 헤더로, 문자열을 접두사로 `Bearer`합니다.

POST 요청에 전송 됩니다 `recognition` API입니다. 그런 다음 응답은 읽고 호출 하는 메서드로 반환 됩니다.

`recognition` API에서 HTTP 상태 코드 200 (정상) 요청이 올바른지는 요청이 성공 했음을 나타내는 응답에 요청 된 정보를 제공 하 고 응답을 보냅니다. 목록이 가능한 오류 응답에 대 한 참조 [문제 해결](/azure/cognitive-services/speech/troubleshooting)합니다.

### <a name="processing-the-response"></a>가 응답 처리

API 응답은 인식 된 텍스트에 포함 되 고 JSON 형식으로 반환 된 `name` 태그입니다. 다음 JSON 데이터에는 일반적인 성공 응답 메시지를 보여 줍니다.

```json
{  
   "RecognitionStatus":"Success",
   "DisplayText":"Go shopping tomorrow.",
   "Offset":16000000,
   "Duration":17100000
}
```

으로 deserialize 하는 JSON 응답 예제 응용 프로그램에서는 `SpeechResult` 다음 스크린샷에 표시 된 것 처럼 호출 하는 메서드를 표시 하려면 반환 되는 결과 함께 인스턴스:

![](speech-recognition-images/speech-recognition.png "음성 인식")

## <a name="summary"></a>요약

이 문서는 Microsoft 음성 REST API를 사용 하 여 오디오 Xamarin.Forms 응용 프로그램에서 텍스트를 변환 하는 방법을 설명 합니다. 음성 인식 기능을 수행 하는 것 외에도 Microsoft 음성 API 음성 단어로 사용 하는 텍스트를 변환할 수도 있습니다.

## <a name="related-links"></a>관련 링크

- [Microsoft 음성 API 설명서](/azure/cognitive-services/speech/home/)합니다.
- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Todo Cognitive 서비스 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
