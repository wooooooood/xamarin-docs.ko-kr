---
title: Bing Spell Check API 사용 하 여 맞춤법 검사
description: Bing Spell Check는 텍스트에 대 한 상황별 맞춤법 검사를 수행 하 고 철자가 틀린 단어에 대 한 인라인 제안을 제공 합니다. 이 문서에서는 Bing Spell Check REST API를 사용 하 여 응용 프로그램에서 맞춤법 오류를 수정 하는 방법을 설명 합니다 Xamarin.Forms .
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1703f0049408381a86da73fb28696ef8708cc790
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139297"
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>Bing Spell Check API 사용 하 여 맞춤법 검사

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Bing Spell Check는 텍스트에 대 한 상황별 맞춤법 검사를 수행 하 고 철자가 틀린 단어에 대 한 인라인 제안을 제공 합니다. 이 문서에서는 Bing Spell Check REST API를 사용 하 여 응용 프로그램에서 맞춤법 오류를 수정 하는 방법을 설명 합니다 Xamarin.Forms ._

## <a name="overview"></a>개요

Bing Spell Check REST API에는 두 가지 운영 모드가 있으며 API에 요청을 수행할 때 모드를 지정 해야 합니다.

- `Spell`대/소문자를 변경 하지 않고 짧은 텍스트 (최대 9 개 단어)를 수정 합니다.
- `Proof`긴 텍스트를 수정 하 고, 대/소문자를 수정 하 고, 기본 문장 부호를 제공 하며, 적극적으로 수정

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다.

Bing Spell Check API를 사용 하려면 API 키를 가져와야 합니다. 이는 [Try Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) 에서 얻을 수 있습니다.

Bing Spell Check API에서 지 원하는 언어 목록은 [지원 되는 언어](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/)를 참조 하세요. Bing Spell Check API에 대 한 자세한 내용은 [Bing Spell Check 설명서](/azure/cognitive-services/bing-spell-check/)를 참조 하세요.

## <a name="authentication"></a>인증

Bing Spell Check API에 대 한 모든 요청에는 헤더의 값으로 지정 해야 하는 API 키가 필요 합니다 `Ocp-Apim-Subscription-Key` . 다음 코드 예제에서는 요청 헤더에 API 키를 추가 하는 방법을 보여 줍니다 `Ocp-Apim-Subscription-Key` .

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Bing Spell Check API에 유효한 API 키를 전달 하지 못하면 401 응답 오류가 발생 합니다.

## <a name="performing-spell-checking"></a>맞춤법 검사 수행

에서 API에 대 한 GET 또는 POST 요청을 수행 하 여 맞춤법 검사를 수행할 수 있습니다 `SpellCheck` `https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck` . GET 요청을 수행 하는 경우 맞춤법을 검사할 텍스트가 쿼리 매개 변수로 전송 됩니다. POST 요청을 수행 하는 경우 맞춤법 검사를 할 텍스트가 요청 본문에 전송 됩니다. GET 요청은 쿼리 매개 변수 문자열 길이 제한으로 인해 1500 자로 제한 됩니다. 따라서 짧은 문자열의 맞춤법을 검사 하지 않는 한 POST 요청은 일반적으로 수행 됩니다.

예제 응용 프로그램에서 메서드는 `SpellCheckTextAsync` 맞춤법 검사 프로세스를 호출 합니다.

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

`SpellCheckTextAsync`메서드는 요청 URI를 생성 한 다음 API로 요청을 보냅니다 `SpellCheck` .이는 결과를 포함 하는 JSON 응답을 반환 합니다. JSON 응답은 deserialize 되며, 결과는 표시를 위해 호출 메서드로 반환 됩니다.

### <a name="configuring-spell-checking"></a>맞춤법 검사 구성

맞춤법 검사 프로세스는 HTTP 쿼리 매개 변수를 지정 하 여 구성할 수 있습니다.

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

이 메서드는 텍스트를 맞춤법 검사로 설정 하 고 맞춤법 검사 모드를 설정 합니다.

REST API Bing Spell Check에 대 한 자세한 내용은 [Spell Check API v7 reference](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)를 참조 하세요.

### <a name="sending-the-request"></a>요청을 보내는 중

`SendRequestAsync`메서드는 Bing Spell Check REST API에 GET 요청을 수행 하 고 응답을 반환 합니다.

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

이 메서드는 `SpellCheck` 변환할 텍스트를 지정 하는 요청 URL과 맞춤법 검사 모드를 사용 하 여 GET 요청을 API로 보냅니다. 그런 다음 응답을 읽고 호출 메서드로 반환 합니다.

`SpellCheck`요청이 성공 했으며 요청 된 정보가 응답에 있음을 나타내는 경우 API는 응답에서 HTTP 상태 코드 200 (OK)를 전송 합니다. 응답 개체 목록은 [응답 개체](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects)를 참조 하세요.

### <a name="processing-the-response"></a>응답 처리

API 응답은 JSON 형식으로 반환 됩니다. 다음 JSON 데이터는 철자가 잘못 된 텍스트에 대 한 응답 메시지를 표시 합니다 `Go shappin tommorow` .

```json
{  
   "_type":"SpellCheck",
   "flaggedTokens":[  
      {  
         "offset":3,
         "token":"shappin",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"shopping",
               "score":1
            }
         ]
      },
      {  
         "offset":11,
         "token":"tommorow",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"tomorrow",
               "score":1
            }
         ]
      }
   ],
   "correctionType":"High"
}
```

배열에 잘못 된 `flaggedTokens` 철자의 플래그가 지정 되었거나 문법적으로 잘못 된 텍스트의 단어 배열이 포함 되어 있습니다. 철자 또는 문법 오류가 없으면 배열이 비어 있습니다. 배열 내의 태그는 다음과 같습니다.

- `offset`– 텍스트 문자열의 시작 부분부터 플래그가 지정 된 단어로의 0부터 시작 하는 오프셋입니다.
- `token`– 텍스트 문자열에서 철자가 잘못 되었거나 문법적으로 잘못 된 단어입니다.
- `type`– 단어에 플래그가 지정 된 오류의 형식입니다. 및의 두 가지 값을 사용할 수 있습니다 `RepeatedToken` `UnknownToken` .
- `suggestions`– 철자 또는 문법 오류를 수정 하는 단어의 배열입니다. 배열은 및로 구성 되며 제안 된 `suggestion` `score` 수정이 정확한 것으로 확신 하는 수준을 나타냅니다.

샘플 응용 프로그램에서 JSON 응답은 인스턴스로 deserialize 되며 `SpellCheckResult` 결과는 표시를 위해 호출 메서드로 반환 됩니다. 다음 코드 예제에서는 `SpellCheckResult` 인스턴스를 표시 하기 위해 처리 하는 방법을 보여 줍니다.

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

이 코드는 컬렉션을 반복 `FlaggedTokens` 하 고 소스 텍스트에서 철자가 잘못 되거나 문법적으로 잘못 된 단어를 첫 번째 제안으로 바꿉니다. 다음 스크린샷는 맞춤법 검사 전후에 표시 됩니다.

![](spell-check-images/before-spell-check.png "Before Spell Check")

![](spell-check-images/after-spell-check.png "After Spell Check")

> [!NOTE]
> 위의 예제에서는 `Replace` 편의상를 사용 하지만 많은 양의 텍스트에서 잘못 된 토큰을 바꿀 수 있습니다. API는 업데이트를 `offset` 수행 하기 위해 원본 텍스트에서 올바른 위치를 식별 하기 위해 프로덕션 앱에서 사용 해야 하는 값을 제공 합니다.

## <a name="summary"></a>요약

이 문서에서는 Bing Spell Check REST API를 사용 하 여 응용 프로그램에서 맞춤법 오류를 수정 하는 방법을 설명 Xamarin.Forms 했습니다. Bing Spell Check는 텍스트에 대 한 상황별 맞춤법 검사를 수행 하 고 철자가 틀린 단어에 대 한 인라인 제안을 제공 합니다.

## <a name="related-links"></a>관련 링크

- [Bing Spell Check 설명서](/azure/cognitive-services/bing-spell-check/)
- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Todo Cognitive Services (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Bing Spell Check API v7 참조](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
