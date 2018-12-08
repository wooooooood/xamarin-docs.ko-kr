---
title: Bing Spell Check API를 사용 하 여 맞춤법 검사
description: Bing Spell Check에 맞춤법이 틀린된 단어에 대 한 인라인 제안 제공 텍스트를 확인 하는 상황에 맞는 맞춤법이 수행 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 맞춤법 오류를 해결 하려면 Bing Spell Check REST API를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 08ac86674e4f10d6bd17d765de2bcdf7c2d3f901
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061761"
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>Bing Spell Check API를 사용 하 여 맞춤법 검사

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)

_Bing Spell Check에 맞춤법이 틀린된 단어에 대 한 인라인 제안 제공 텍스트를 확인 하는 상황에 맞는 맞춤법이 수행 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 맞춤법 오류를 해결 하려면 Bing Spell Check REST API를 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Bing Spell Check REST API에 두 가지 작동 모드 및 API에 요청할 때 모드를 지정 해야 합니다.

- `Spell` 짧은 텍스트 (최대 9 개의 단어) 대/소문자 변경 하지 않고 수정합니다.
- `Proof` 긴 텍스트를 수정, 대/소문자 수정 및 기본 문장 부호, 제공 및 적극적으로 수정 하지 않습니다.

Bing Spell Check API를 사용 하는 API 키를 가져와야 합니다. 가져올 수 있습니다 [Cognitive 서비스 시도](https://azure.microsoft.com/try/cognitive-services/)

Bing Spell Check API를 지 원하는 언어 목록은 참조 하세요 [지원 되는 언어](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/)합니다. Bing Spell Check API에 대 한 자세한 내용은 참조 하세요. [Bing 맞춤법 검사 설명서](/azure/cognitive-services/bing-spell-check/)합니다.

## <a name="authentication"></a>인증

Bing Spell Check API에 대 한 모든 요청 값으로 지정 해야 하는 API 키가 필요 합니다 `Ocp-Apim-Subscription-Key` 헤더입니다. 다음 코드 예제에는 API 키를 추가 하는 방법을 보여 줍니다는 `Ocp-Apim-Subscription-Key` 요청 헤더:

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Bing Spell Check API를 유효한 API 키를 전달 하는 오류 401 응답 오류가 발생 합니다.

## <a name="performing-spell-checking"></a>맞춤법 검사 수행

GET 또는 POST 요청을 수행 하 여 맞춤법 검사를 수행할 수 있습니다 합니다 `SpellCheck` API에 `https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck`입니다. 에 GET 요청을 수행할 때 맞춤법을 검사할 텍스트 쿼리 매개 변수로 전송 됩니다. 에 POST 요청을 수행할 때 맞춤법을 검사할 텍스트는 요청 본문에 전송 됩니다. GET 요청은 맞춤법 검색 쿼리 매개 변수 문자열 길이 제한으로 인해 1,500 자 이내로 제한 됩니다. 따라서 POST 요청은 짧은 문자열에는 맞춤법 검사 되는 경우가 아니면 일반적으로 설정 안 됩니다.

샘플 응용 프로그램에서을 `SpellCheckTextAsync` 맞춤법 검색 프로세스를 호출 하는 메서드:

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

`SpellCheckTextAsync` 메서드는 요청 URI를 생성 하 고 요청을 보냅니다는 `SpellCheck` 결과 포함 하는 JSON 응답을 반환 하는 API입니다. 표시에 대 한 호출 메서드로 반환 되는 결과 사용 하 여 JSON 응답을 deserialize 됩니다.

### <a name="configuring-spell-checking"></a>맞춤법 검사 구성

HTTP 쿼리 매개 변수를 지정 하 여 맞춤법 검색 프로세스를 구성할 수 있습니다.

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

이 메서드는 텍스트 옵션을 선택 하는 맞춤법 및 맞춤법 검사 모드를 설정 합니다.

Bing Spell Check REST API에 대 한 자세한 내용은 참조 하세요. [Spell Check API v7 참조](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)합니다.

### <a name="sending-the-request"></a>요청을 보내기

`SendRequestAsync` 메서드는 Bing Spell Check REST API에 GET 요청을 수행 하 고 응답을 반환 합니다.

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

이 메서드는 GET 요청을 전송 합니다 `SpellCheck` 변환할 텍스트를 지정 하는 요청 URL과 맞춤법 검사 모드를 사용 하 여 API. 그런 다음 응답은 읽고 호출 메서드로 반환 됩니다.

`SpellCheck` API에서 HTTP 상태 코드 200 (정상) 요청 올바른지 요청이 성공 했는지를 나타내는 응답에 요청 된 정보를 제공 하 고 응답을 보냅니다. 응답 개체의 목록을 참조 하세요 [응답 개체](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects)합니다.

### <a name="processing-the-response"></a>응답 처리

API 응답은 JSON 형식으로 반환 됩니다. 철자가 잘못 된 텍스트에 대 한 응답 메시지를 표시 하는 다음 JSON 데이터를 `Go shappin tommorow`:

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

`flaggedTokens` 배열 되 맞춤법이 잘못 또는 문법적으로 잘못 된 플래그가 지정 된 텍스트의 단어의 배열을 포함 합니다. 배열은 맞춤법 또는 문법 오류가 없는지 없으면 비어 있게 됩니다. 배열 내에서 태그를 다음과 같습니다.

- `offset` – 플래그가 지정 된 단어를 텍스트 문자열의 시작 부분에서 0부터 시작 오프셋입니다.
- `token` – 철자가 정확 하지 않거나 문법적으로 올바르지 않습니다 하는 텍스트 문자열의 단어입니다.
- `type` – 플래그가 지정 단어를 발생 시킨 오류 유형입니다. 두 개의 가능한 값 – `RepeatedToken` 고 `UnknownToken`입니다.
- `suggestions` – 맞춤법 또는 문법 오류를 수정 하는 단어의 배열입니다. 배열 이루어집니다를 `suggestion` 및 `score`, 제안 된 수정 사항이 올바른지 신뢰 수준을 나타냅니다.

샘플 응용 프로그램에서 JSON 응답으로 deserialize 하는 `SpellCheckResult` 인스턴스를 표시 하기 위해 호출 하는 메서드에서 반환 되는 결과입니다. 다음 코드 예제에서는 하는 방법을 `SpellCheckResult` 인스턴스 표시 하기 위해 처리 됩니다.

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

이 코드를 반복 합니다 `FlaggedTokens` 컬렉션 및 모든 철자가 잘못 된 대체 또는 첫 번째 제안을 사용 하 여 원본 텍스트의 문법적으로 잘못 된 단어입니다. 다음 스크린샷은 spell check 전후 보여 줍니다.

![](spell-check-images/before-spell-check.png "맞춤법 검사 하기 전에")

![](spell-check-images/after-spell-check.png "맞춤법 검사 후")

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms 응용 프로그램에서 맞춤법 오류를 해결 하려면 Bing Spell Check REST API를 사용 하는 방법을 설명 합니다. Bing Spell Check에 맞춤법이 틀린된 단어에 대 한 인라인 제안 제공 텍스트를 확인 하는 상황에 맞는 맞춤법이 수행 합니다.

## <a name="related-links"></a>관련 링크

- [Bing Spell Check 설명서](/azure/cognitive-services/bing-spell-check/)
- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Todo Cognitive Services (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Bing Spell Check API v7 참조](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
