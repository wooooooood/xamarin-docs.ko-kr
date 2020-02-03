---
title: 텍스트 번역, Translator API를 사용 하 여
description: Microsoft Translator API는 음성 및 REST API를 통해 텍스트 번역을 사용할 수 있습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 다른 언어로 텍스트를 변환 하는 Microsoft Translator Text API를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 841b1d4abab5e4c09249174b221da20794771a86
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725570"
---
# <a name="text-translation-using-the-translator-api"></a>텍스트 번역, Translator API를 사용 하 여

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Translator API를 사용 하 여 REST API에서 음성 및 텍스트를 변환할 수 있습니다. 이 문서에서는 Xamarin.ios 응용 프로그램에서 Microsoft Translator Text API를 사용 하 여 한 언어에서 다른 언어로 텍스트를 변환 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Translator API의 두 구성 요소:

- 텍스트 번역 REST API를 한 언어에서 다른 언어의 텍스트로 변환 합니다. API는 자동으로 변환 하기 전에 전송 된 텍스트의 언어를 검색 합니다.
- 음성 번역 REST API를 다른 언어의 텍스트를 음성 언어에서를 기록 합니다. 또한 이 API는 텍스트에서 음성 변환 기능을 통합하여 번역된 텍스트를 다시 읽어줍니다.

이 문서는 Translator Text API를 사용 하 여 다른 언어로 텍스트를 번역에 중점을 둡니다.

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다.

Translator Text API를 사용 하는 API 키를 가져와야 합니다. [Microsoft Translator Text API에 등록 하는 방법](/azure/cognitive-services/translator/translator-text-how-to-signup/)에서 얻을 수 있습니다.

Microsoft Translator Text API에 대 한 자세한 내용은 [Translator Text API 설명서](/azure/cognitive-services/translator/)를 참조 하세요.

## <a name="authentication"></a>인증

Translator Text API에 대 한 모든 요청에는 `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`의 인식 서비스 토큰 서비스에서 가져올 수 있는 JSON Web Token (JWT) 액세스 토큰이 필요 합니다. 토큰 서비스에 대 한 POST 요청을 수행 하 고 API 키를 해당 값으로 포함 하는 `Ocp-Apim-Subscription-Key` 헤더를 지정 하 여 토큰을 가져올 수 있습니다.

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

다음 코드 예제에 표시 된 것 처럼 문자열 `Bearer`접두사가 붙은 `Authorization` 헤더로 각 Translator Text API 호출에 액세스 토큰을 지정 해야 합니다.

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

인식 서비스 토큰 서비스에 대 한 자세한 내용은 [인증](/azure/cognitive-services/translator/reference/v3-0-reference#authentication)을 참조 하세요.

## <a name="performing-text-translation"></a>텍스트 번역 수행

텍스트 변환은 `https://api.microsofttranslator.com/v2/http.svc/translate`에서 `translate` API에 대 한 GET 요청을 수행 하 여 달성할 수 있습니다. 샘플 응용 프로그램에서 `TranslateTextAsync` 메서드는 텍스트 변환 프로세스를 호출 합니다.

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

`TranslateTextAsync` 메서드는 요청 URI를 생성 하 고 토큰 서비스에서 액세스 토큰을 검색 합니다. 텍스트 번역 요청은 결과를 포함 하는 XML 응답을 반환 하는 `translate` API로 전송 됩니다. XML 응답을 구문 분석 및 변환 결과를 표시 하기 위해 호출 메서드로 반환 됩니다.

텍스트 변환 REST Api에 대 한 자세한 내용은 [Translator Text API](/azure/cognitive-services/translator/reference/v3-0-reference)를 참조 하세요.

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

이 메서드는 텍스트를 번역 및 텍스트를 변환할 언어를 설정 합니다. Microsoft Translator에서 지원 되는 언어 목록은 [microsoft Translator Text API에서 지원 되는 언어](/azure/cognitive-services/translator/languages/)를 참조 하세요.

> [!NOTE]
> 응용 프로그램에서 텍스트의 언어를 알고 있어야 하는 경우 `Detect` API를 호출 하 여 텍스트 문자열의 언어를 검색할 수 있습니다.

### <a name="sending-the-request"></a>요청을 보내기

`SendRequestAsync` 메서드는 텍스트 번역 REST API에 GET 요청을 수행 하 고 응답을 반환 합니다.

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

이 메서드는 문자열 `Bearer`앞에 있는 `Authorization` 헤더에 액세스 토큰을 추가 하 여 GET 요청을 작성 합니다. 그런 다음 GET 요청은 번역할 텍스트를 지정 하는 요청 URL과 텍스트를 변환할 언어를 사용 하 여 `translate` API로 전송 됩니다. 그런 다음 응답은 읽고 호출 메서드로 반환 됩니다.

요청이 성공 했으며 요청 된 정보가 응답에 있음을 나타내는 경우 `translate` API는 응답에서 HTTP 상태 코드 200 (OK)를 전송 합니다. 가능한 오류 응답 목록은 [GET 변환](/azure/cognitive-services/translator/reference/v3-0-translate)의 응답 메시지를 참조 하세요.

### <a name="processing-the-response"></a>응답 처리

API 응답은 XML 형식으로 반환 합니다. 다음 XML 데이터에는 일반적인 성공 응답 메시지를 보여 줍니다.

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

예제 응용 프로그램에서 XML 응답은 다음 스크린샷에 표시 된 것 처럼 표시 하기 위해 XML 루트 값을 호출 메서드로 반환 하는 `XDocument` 인스턴스로 구문 분석 됩니다.

![](text-translation-images/text-translation.png "Text Translation to German")

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms 응용 프로그램에서 다른 언어의 텍스트로 한 언어에서 텍스트를 번역에 Microsoft Translator Text API를 사용 하는 방법을 설명 합니다. 텍스트를 번역 하는 것 외에도 Microsoft Translator API 다른 언어의 텍스트에 사용 하는 음성 언어에서 촬영할 수도 있습니다.

## <a name="related-links"></a>관련 링크

- [Translator Text API 설명서](/azure/cognitive-services/translator/)
- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Todo Cognitive Services (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Translator Text API](/azure/cognitive-services/translator/reference/v3-0-reference)
