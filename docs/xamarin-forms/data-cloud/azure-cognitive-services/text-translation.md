---
title: Translator API를 사용 하 여 텍스트 번역
description: Microsoft Translator API를 사용 하 여 REST API에서 음성 및 텍스트를 변환할 수 있습니다. 이 문서에서는 Microsoft Translator Text API를 사용 하 여 응용 프로그램에서 한 언어에서 다른 언어로 텍스트를 변환 하는 방법을 설명 합니다 Xamarin.Forms .
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f0f43f8f2113b6bd0a800ed3e0bd96b641575b1c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139284"
---
# <a name="text-translation-using-the-translator-api"></a>Translator API를 사용 하 여 텍스트 번역

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Translator API를 사용 하 여 REST API에서 음성 및 텍스트를 변환할 수 있습니다. 이 문서에서는 Microsoft Translator Text API를 사용 하 여 응용 프로그램에서 한 언어에서 다른 언어로 텍스트를 변환 하는 방법을 설명 합니다 Xamarin.Forms ._

## <a name="overview"></a>개요

Translator API에는 다음과 같은 두 가지 구성 요소가 있습니다.

- 텍스트 번역을 REST API 하 여 텍스트를 한 언어에서 다른 언어의 텍스트로 변환할 수 있습니다. API는 번역 전에 전송 된 텍스트의 언어를 자동으로 검색 합니다.
- 음성 번역은 한 언어에서 다른 언어로 텍스트 높여줄 음성 번역을 REST API 합니다. 또한 이 API는 텍스트에서 음성 변환 기능을 통합하여 번역된 텍스트를 다시 읽어줍니다.

이 문서에서는 Translator Text API를 사용 하 여 텍스트를 한 언어에서 다른 언어로 번역 하는 방법을 집중적으로 설명 합니다.

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다.

Translator Text API를 사용 하려면 API 키를 가져와야 합니다. [Microsoft Translator Text API에 등록 하는 방법](/azure/cognitive-services/translator/translator-text-how-to-signup/)에서 얻을 수 있습니다.

Microsoft Translator Text API에 대 한 자세한 내용은 [Translator Text API 설명서](/azure/cognitive-services/translator/)를 참조 하세요.

## <a name="authentication"></a>인증

Translator Text API에 대 한 모든 요청에는의 인식 서비스 토큰 서비스에서 가져올 수 있는 JWT (JSON Web Token) 액세스 토큰이 필요 `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` 합니다. 토큰 서비스에 대 한 POST 요청을 수행 하 고 `Ocp-Apim-Subscription-Key` API 키를 해당 값으로 포함 하는 헤더를 지정 하 여 토큰을 가져올 수 있습니다.

다음 코드 예제에서는 토큰 서비스에서 액세스 토큰을 요청 하는 방법을 보여 줍니다.

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

Base64 텍스트인 반환 된 액세스 토큰의 만료 시간은 10 분입니다. 따라서 샘플 응용 프로그램은 9 분 마다 액세스 토큰을 갱신 합니다.

`Authorization` `Bearer` 다음 코드 예제에 표시 된 것 처럼 각 Translator Text API 호출에서 문자열이 접두사로 지정 된 헤더로 액세스 토큰을 지정 해야 합니다.

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

인식 서비스 토큰 서비스에 대 한 자세한 내용은 [인증](/azure/cognitive-services/translator/reference/v3-0-reference#authentication)을 참조 하세요.

## <a name="performing-text-translation"></a>텍스트 번역 수행

에서 API에 대 한 GET 요청을 수행 하 여 텍스트 번역을 수행할 수 있습니다 `translate` `https://api.microsofttranslator.com/v2/http.svc/translate` . 예제 응용 프로그램에서 메서드는 `TranslateTextAsync` 텍스트 변환 프로세스를 호출 합니다.

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

`TranslateTextAsync`메서드는 요청 URI를 생성 하 고 토큰 서비스에서 액세스 토큰을 검색 합니다. 그런 다음 `translate` 결과를 포함 하는 XML 응답을 반환 하는 API로 텍스트 변환 요청을 보냅니다. XML 응답이 구문 분석 되 고 변환 결과가 표시를 위해 호출 메서드에 반환 됩니다.

텍스트 변환 REST Api에 대 한 자세한 내용은 [Translator Text API](/azure/cognitive-services/translator/reference/v3-0-reference)를 참조 하세요.

### <a name="configuring-text-translation"></a>텍스트 번역 구성

텍스트 번역 프로세스는 HTTP 쿼리 매개 변수를 지정 하 여 구성할 수 있습니다.

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

이 메서드는 번역할 텍스트와 텍스트를 변환할 언어를 설정 합니다. Microsoft Translator에서 지원 되는 언어 목록은 [microsoft Translator Text API에서 지원 되는 언어](/azure/cognitive-services/translator/languages/)를 참조 하세요.

> [!NOTE]
> 응용 프로그램에서 텍스트가 있는 언어를 알아야 하는 경우 `Detect` API를 호출 하 여 텍스트 문자열의 언어를 검색할 수 있습니다.

### <a name="sending-the-request"></a>요청을 보내는 중

`SendRequestAsync`메서드는 REST API 텍스트 번역에 GET 요청을 수행 하 고 응답을 반환 합니다.

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

이 메서드는 `Authorization` 헤더에 문자열을 접두사로 하는 액세스 토큰을 추가 하 여 GET 요청을 빌드합니다 `Bearer` . 그러면 GET 요청이 API로 전송 되 고 `translate` , 번역할 텍스트를 지정 하는 요청 URL과 텍스트를 변환할 언어가 사용 됩니다. 그런 다음 응답을 읽고 호출 메서드로 반환 합니다.

`translate`요청이 성공 했으며 요청 된 정보가 응답에 있음을 나타내는 경우 API는 응답에서 HTTP 상태 코드 200 (OK)를 전송 합니다. 가능한 오류 응답 목록은 [GET 변환](/azure/cognitive-services/translator/reference/v3-0-translate)의 응답 메시지를 참조 하세요.

### <a name="processing-the-response"></a>응답 처리

API 응답은 XML 형식으로 반환 됩니다. 다음 XML 데이터는 일반적인 성공적인 응답 메시지를 보여 줍니다.

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

예제 응용 프로그램에서 XML 응답은 인스턴스로 구문 분석 되 고 `XDocument` , 다음 스크린샷에 표시 된 것 처럼 표시 하기 위해 xml 루트 값이 호출 메서드로 반환 됩니다.

![](text-translation-images/text-translation.png "Text Translation to German")

## <a name="summary"></a>요약

이 문서에서는 Microsoft Translator Text API를 사용 하 여 응용 프로그램에서 한 언어의 텍스트를 다른 언어로 변환 하는 방법을 설명 Xamarin.Forms 했습니다. 텍스트를 번역 하는 것 외에도 Microsoft Translator API는 한 언어에서 다른 언어로 텍스트로 음성을 높여줄 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Translator Text API 설명서](/azure/cognitive-services/translator/)
- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Todo Cognitive Services (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Translator Text API](/azure/cognitive-services/translator/reference/v3-0-reference)
