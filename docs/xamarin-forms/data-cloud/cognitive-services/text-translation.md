---
title: 변환기 API를 사용 하 여 텍스트 번역
description: 음성 및 REST API를 통해 텍스트 변환할 Microsoft 변환기 API는 사용할 수 있습니다. 이 문서에서는 Microsoft 변환기 텍스트 API Xamarin.Forms 응용 프로그램에서 다른 한 언어에서 텍스트를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 5c1001335fb030f9a91ec72456042316864ccf5c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="text-translation-using-the-translator-api"></a>변환기 API를 사용 하 여 텍스트 번역

_음성 및 REST API를 통해 텍스트 변환할 Microsoft 변환기 API는 사용할 수 있습니다. 이 문서에서는 Microsoft 변환기 텍스트 API Xamarin.Forms 응용 프로그램에서 다른 한 언어에서 텍스트를 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

변환기 API의 두 구성 요소:

- 텍스트 번역 한 언어에서 다른 언어의 텍스트를 변환 하는 REST API입니다. API는 자동으로 변환 하기 전에 전송 하는 텍스트의 언어를 검색 합니다.
- 음성의 변환 음성 한 언어에서 다른 언어의 텍스트로 변환 하도록 하는 REST API입니다. 또한 API로 언급 해야 할 번역된 된 텍스트를 다시 텍스트 음성 변환 기능을 통합 합니다.

이 문서 변환기 텍스트 API를 사용 하 여 다른 하 한 언어에서 텍스트를 번역에 중점을 둡니다.

변환기 텍스트 API를 사용 하면 API 키를 받아야 합니다. 다운로드할 수 있습니다 [Microsoft 변환기 텍스트 API에 대 한 등록 하는 방법을](/azure/cognitive-services/translator/translator-text-how-to-signup/)합니다.

Microsoft 변환기 텍스트 API에 대 한 자세한 내용은 참조 [변환기 텍스트 API 설명서](/azure/cognitive-services/translator/)합니다.

## <a name="authentication"></a>인증

변환기 텍스트 API에 대 한 모든 요청에서 cognitive 서비스 토큰 서비스에서 가져올 수 있는 JSON 웹 토큰 (JWT) 액세스 토큰이, 필요한 `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`합니다. 토큰 서비스에 POST 요청을 하 여 토큰을 가져올 수 있습니다를 지정 하는 `Ocp-Apim-Subscription-Key` 해당 값으로 API 키가 포함 된 헤더입니다.

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

각 변환기 텍스트 API에 액세스 토큰을 지정 해야 라고는 `Authorization` 헤더 문자열을 접두사로 `Bearer`다음 코드 예제에 나온 것 처럼:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Cognitive 서비스 토큰 서비스에 대 한 자세한 내용은 참조 [인증 토큰 API](http://docs.microsofttranslator.com/oauth-token.html)합니다.

## <a name="performing-text-translation"></a>텍스트 변환 수행

텍스트 번역에 대 한 GET 요청을 만들어 수행할 수 있습니다는 `translate` API에 `https://api.microsofttranslator.com/v2/http.svc/translate`합니다. 샘플 응용 프로그램에서의 `TranslateTextAsync` 텍스트 변환 프로세스를 호출 하는 메서드:

```csharp
public async Task<string> TranslateTextAsync(string text)
{
  ...
  string requestUri = GenerateRequestUri(Constants.TextTranslatorEndpoint, text, "en", "de");
  string accessToken = authenticationService.GetAccessToken();
  var response = await SendRequestAsync(requestUri, accessToken);
  var xml = XDocument.Parse(response);
  return xml.Root.Value;
}
```

`TranslateTextAsync` 메서드 요청 URI를 생성 하 고 토큰 서비스에서 액세스 토큰을 가져옵니다. 텍스트 변환 요청에 전송 됩니다는 `translate` API는 결과 포함 하는 XML 응답을 반환 합니다. XML 응답을 구문 분석 되 고 변환 결과를 표시 하기 위해 호출 메서드에 반환 됩니다.

텍스트 번역 REST Api에 대 한 자세한 내용은 참조 [Microsoft 변환기 텍스트 API](http://docs.microsofttranslator.com/text-translate.html)합니다.

### <a name="configuring-text-translation"></a>텍스트 변환 구성

HTTP 쿼리 매개 변수를 지정 하 여 텍스트 변환 프로세스를 구성할 수 있습니다.

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

이 메서드는 변환 해야 하는 텍스트와 텍스트를 변환 하는 언어를 설정 합니다. Microsoft Translator에서 지 원하는 언어 목록은 참조 [Microsoft 변환기 텍스트 API에서 지원 되는 언어](/azure/cognitive-services/translator/languages/)합니다.

> [!NOTE]
> 응용 프로그램에서는 텍스트는 언어를 알아야 하는 경우는 `Detect` 텍스트 문자열의 언어를 검색 하도록 API를 호출할 수 있습니다.

### <a name="sending-the-request"></a>요청을 보내기

`SendRequestAsync` 메서드 텍스트 번역 REST API에 GET 요청을 수행 하 고 응답을 반환 합니다.

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

이 메서드는 액세스 토큰을 추가 하 여 GET 요청을 작성 된 `Authorization` 헤더로, 문자열을 접두사로 `Bearer`합니다. GET 요청에 전송 됩니다는 `translate` 변환 해야 하는 텍스트를 지정 하는 요청 URL과 텍스트를 변환 하는 언어를 사용 하 여의 API입니다. 그런 다음 응답은 읽고 호출 하는 메서드로 반환 됩니다.

`translate` API에서 HTTP 상태 코드 200 (정상) 요청이 올바른지는 요청이 성공 했음을 나타내는 응답에 요청 된 정보를 제공 하 고 응답을 보냅니다. 목록이 가능한 오류 응답에 대 한 참조에서 응답 메시지 [번역 가져오기](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate)합니다.

### <a name="processing-the-response"></a>가 응답 처리

API 응답 XML 형식으로 반환 됩니다. 다음 XML 데이터에는 일반적인 성공 응답 메시지를 보여 줍니다.

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

샘플 응용 프로그램에서 XML 파일에 대 한 응답으로 구문 분석 되는 `XDocument` 인스턴스에서 다음 스크린샷에 표시 된 것 처럼 호출 하는 메서드로 표시 하기 위해 반환 되는 XML 루트 값:

![](text-translation-images/text-translation.png "독일어로 텍스트 번역")

## <a name="summary"></a>요약

이 문서에는 API를 사용 하 여 Microsoft 변환기 텍스트 Xamarin.Forms 응용 프로그램에서 다른 언어의 텍스트를 한 언어에서 변환 하는 방법을 설명 했습니다. 텍스트를 번역 하는 것 외에도 Microsoft 변환기 API 다른 언어의 텍스트에 한 언어에서 음성을 수도 적을 수 있습니다.

## <a name="related-links"></a>관련 링크

- [변환기 텍스트 API 설명서](/azure/cognitive-services/translator/)합니다.
- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Todo Cognitive 서비스 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft Translator 텍스트 API](http://docs.microsofttranslator.com/text-translate.html)합니다.
