---
title: 텍스트 번역, Translator API를 사용 하 여
description: Microsoft Translator API는 음성 및 REST API를 통해 텍스트 번역을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 다른 언어로 텍스트를 변환 하는 Microsoft Translator Text API를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 5739246ec7804b58d900ec790f427dab37504b1f
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655026"
---
# <a name="text-translation-using-the-translator-api"></a>텍스트 번역, Translator API를 사용 하 여

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Translator API는 음성 및 REST API를 통해 텍스트 번역을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 다른 언어로 텍스트를 변환 하는 Microsoft Translator Text API를 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Translator API의 두 구성 요소:

- 텍스트 번역 REST API를 한 언어에서 다른 언어의 텍스트로 변환 합니다. API는 자동으로 변환 하기 전에 전송 된 텍스트의 언어를 검색 합니다.
- 음성 번역 REST API를 다른 언어의 텍스트를 음성 언어에서를 기록 합니다. API는 또한 번역된 된 텍스트를 다시 사용 하는 텍스트 음성 변환 기능을 통합 합니다.

이 문서는 Translator Text API를 사용 하 여 다른 언어로 텍스트를 번역에 중점을 둡니다.

Translator Text API를 사용 하는 API 키를 가져와야 합니다. 가져올 수 있습니다 [Microsoft Translator Text API에 대 한 등록 방법](/azure/cognitive-services/translator/translator-text-how-to-signup/)합니다.

Microsoft Translator Text API에 대 한 자세한 내용은 참조 하세요. [Translator 텍스트 API 설명서](/azure/cognitive-services/translator/)합니다.

## <a name="authentication"></a>인증

Translator Text API에 대 한 모든 요청에서 cognitive 서비스 토큰 서비스에서 가져올 수 있는 JSON 웹 토큰 (JWT) 액세스 토큰을 필요한 `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`합니다. 토큰 서비스에 POST 요청을 수행 하 여 토큰을 가져올 수 있습니다 지정 하는 `Ocp-Apim-Subscription-Key` 값으로 API 키가 포함 된 헤더입니다.

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

각 Translator Text API 액세스 토큰을 지정 해야로 호출을 `Authorization` 문자열을 접두사로 하는 헤더 `Bearer`다음 코드 예제에 나와 있는 것 처럼:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Cognitive 서비스 토큰 서비스에 대 한 자세한 내용은 참조 하세요. [인증 토큰 API](http://docs.microsofttranslator.com/oauth-token.html)합니다.

## <a name="performing-text-translation"></a>텍스트 번역 수행

GET 요청을 수행 하 여 텍스트 번역을 수행할 수 있습니다 합니다 `translate` API에 `https://api.microsofttranslator.com/v2/http.svc/translate`입니다. 샘플 응용 프로그램에서을 `TranslateTextAsync` 텍스트 변환 프로세스를 호출 하는 메서드:

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

`TranslateTextAsync` 메서드 요청 URI를 생성 하 고 토큰 서비스에서 액세스 토큰을 검색 합니다. 텍스트 번역 요청에 전송 됩니다는 `translate` 결과가 포함 된 XML 응답을 반환 하는 API입니다. XML 응답을 구문 분석 및 변환 결과를 표시 하기 위해 호출 메서드로 반환 됩니다.

텍스트 번역 REST Api에 대 한 자세한 내용은 참조 하세요. [Microsoft Translator Text API](http://docs.microsofttranslator.com/text-translate.html)합니다.

### <a name="configuring-text-translation"></a>텍스트 변환 구성

HTTP 쿼리 매개 변수를 지정 하 여 텍스트 번역 프로세스를 구성할 수 있습니다.

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

이 메서드는 텍스트를 번역 및 텍스트를 변환할 언어를 설정 합니다. Microsoft Translator를 지 원하는 언어 목록은 참조 하세요 [Microsoft Translator Text API에서 지원 되는 언어](/azure/cognitive-services/translator/languages/)합니다.

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

액세스 토큰을 추가 하 여 GET 요청을 작성 하는이 메서드는 `Authorization` 문자열을 접두사로 헤더 `Bearer`합니다. 다음 GET 요청을 보낼는 `translate` 변환할 텍스트를 지정 하는 요청 URL과 텍스트를 변환 하는 언어를 사용 하 여 API입니다. 그런 다음 응답은 읽고 호출 메서드로 반환 됩니다.

`translate` API에서 HTTP 상태 코드 200 (정상) 요청 올바른지 요청이 성공 했는지를 나타내는 응답에 요청 된 정보를 제공 하 고 응답을 보냅니다. 가능한 오류 응답의 목록을 응답 메시지를 참조 하세요 [번역 가져오기](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate)합니다.

### <a name="processing-the-response"></a>응답 처리

API 응답은 XML 형식으로 반환 합니다. 다음 XML 데이터에는 일반적인 성공 응답 메시지를 보여 줍니다.

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

샘플 응용 프로그램에서 XML 응답으로 구문 분석 되는 `XDocument` 다음 스크린샷과에서 같이 표시에 대 한 호출 메서드로 반환 되는 XML 루트 값을 사용 하 여 인스턴스:

![](text-translation-images/text-translation.png "독일어로 텍스트 번역")

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms 응용 프로그램에서 다른 언어의 텍스트로 한 언어에서 텍스트를 번역에 Microsoft Translator Text API를 사용 하는 방법을 설명 합니다. 텍스트를 번역 하는 것 외에도 Microsoft Translator API 다른 언어의 텍스트에 사용 하는 음성 언어에서 촬영할 수도 있습니다.

## <a name="related-links"></a>관련 링크

- [Translator Text API 설명서](/azure/cognitive-services/translator/)합니다.
- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Todo Cognitive Services (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Microsoft Translator Text API](http://docs.microsofttranslator.com/text-translate.html)합니다.
