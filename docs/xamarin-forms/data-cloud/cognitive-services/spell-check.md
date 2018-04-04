---
title: Bing 맞춤법 검사 API를 사용 하 여 맞춤법 검사
description: Bing 맞춤법 검사 맞춤법이 틀린된 단어에 대 한 추천 단어 인라인 상황별 맞춤법 텍스트에 대 한 검사 수행 합니다. 이 문서에서는 Bing 맞춤법 검사 REST API를 사용 하 여 Xamarin.Forms 응용 프로그램의 맞춤법 오류를 수정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 41bd79b22aa193dd5303847997bc07e8e8d12e58
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>Bing 맞춤법 검사 API를 사용 하 여 맞춤법 검사

_Bing 맞춤법 검사 맞춤법이 틀린된 단어에 대 한 추천 단어 인라인 상황별 맞춤법 텍스트에 대 한 검사 수행 합니다. 이 문서에서는 Bing 맞춤법 검사 REST API를 사용 하 여 Xamarin.Forms 응용 프로그램의 맞춤법 오류를 수정 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Bing 맞춤법 검사 REST API는 두 가지 작동 모드 및 API에 요청을 수행할 때는 모드를 지정 해야 합니다.

- `Spell` 짧은 텍스트 (최대 9 단어) 대/소문자 변경 하지 않고 수정합니다.
- `Proof` 긴 텍스트를 수정 하 고 대/소문자 수정 및 기본 문장 부호, 제공 적극적인 수정 사항을 표시 하지 않습니다.

Bing 맞춤법 검사 API를 사용 하면 API 키를 받아야 합니다. 다운로드할 수 있습니다 [Cognitive 서비스 시도](https://azure.microsoft.com/try/cognitive-services/)

Bing 맞춤법 검사 API에서 지 원하는 언어 목록은 참조 하십시오. [지원 되는 언어](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/)합니다. Bing 맞춤법 검사 API에 대 한 자세한 내용은 참조 [Bing 맞춤법 검사 설명서](/azure/cognitive-services/bing-spell-check/)합니다.

## <a name="authentication"></a>인증

Bing 맞춤법 검사 API에 대 한 모든 요청 필요 하면 API 키의 값으로 지정 해야 하는 `Ocp-Apim-Subscription-Key` 헤더입니다. 다음 코드 예제에서는 API 키를 추가 하는 방법을 보여 줍니다.는 `Ocp-Apim-Subscription-Key` 요청 헤더의:

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Bing 맞춤법 검사 API에 올바른 API 키를 전달 하는 오류 401 응답 오류가 발생 합니다.

## <a name="performing-spell-checking"></a>맞춤법 검사 수행

GET 또는 POST 요청을 수행 하 여 맞춤법 검사를 수행할 수 있습니다는 `SpellCheck` API에 `https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck`합니다. GET 요청을 만들 때는 맞춤법을 검사할 텍스트를 쿼리 매개 변수로 전송 됩니다. POST 요청을 만들 때는 맞춤법을 검사할 텍스트는 요청 본문에 전송 됩니다. GET 요청은 맞춤법 검사 1500 자의 쿼리 매개 변수 문자열 길이 제한으로 인해 제한 됩니다. 따라서 POST 요청은 짧은 문자열에는 맞춤법 검사 되는 하지 않는 한 일반적으로 해야 합니다.

샘플 응용 프로그램에서의 `SpellCheckTextAsync` 맞춤법 검사 프로세스를 호출 하는 메서드:

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

`SpellCheckTextAsync` 메서드 요청 URI를 생성 하 고 다음 요청을 보냅니다는 `SpellCheck` API는 결과 포함 하는 JSON 응답을 반환 합니다. JSON 응답에 표시 하기 위해 호출 하는 메서드에서 반환 되는 결과와 deserialize 됩니다.

### <a name="configuring-spell-checking"></a>맞춤법 검사 구성

HTTP 쿼리 매개 변수를 지정 하 여 맞춤법 검사 프로세스를 구성할 수 있습니다.

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

이 메서드는 텍스트를 선택 하면 맞춤법 고 맞춤법 검사 모드를 설정 합니다.

Bing 맞춤법 검사 REST API에 대 한 자세한 내용은 참조 [맞춤법 검사 API v7 참조](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)합니다.

### <a name="sending-the-request"></a>요청을 보내기

`SendRequestAsync` 메서드는 Bing 맞춤법 검사 REST API에 GET 요청 및 응답을 반환 합니다.

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

이 메서드는 GET 요청 API 키의 값으로 추가 하 여 작성 된 `Ocp-Apim-Subscription-Key` 헤더입니다. GET 요청에 전송 됩니다는 `SpellCheck` 변환 해야 하는 텍스트를 지정 하는 요청 URL과 맞춤법 검사 모드를 사용 하 여의 API입니다. 그런 다음 응답은 읽고 호출 하는 메서드로 반환 됩니다.

`SpellCheck` API에서 HTTP 상태 코드 200 (정상) 요청이 올바른지는 요청이 성공 했음을 나타내는 응답에 요청 된 정보를 제공 하 고 응답을 보냅니다. 응답 개체의 목록에 대 한 참조 [응답 개체](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects)합니다.

### <a name="processing-the-response"></a>가 응답 처리

API 응답은 JSON 형식으로 반환 합니다. 다음 JSON 데이터 철자가 틀린된 텍스트에 대 한 응답 메시지를 보여 줍니다. `Go shappin tommorow`:

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

`flaggedTokens` 배열 올바르게 입력 되지 또는 문법적으로 잘못 된 플래그가 지정 된 텍스트의 단어의 배열을 포함 합니다. 배열 없는 맞춤법 및 문법 오류를 발견 되 면에 비어 있게 됩니다. 배열 내의 태그는:

- `offset` – 플래그가 지정 된 단어에는 텍스트 문자열의 시작 부분에서 0부터 시작 오프셋입니다.
- `token` – 철자가 정확 하지 않았거나 문법적으로 올바른있지 않습니다 하는 텍스트 문자열에서 단어입니다.
- `type` -플래그를 지정 하는 단어를 발생 시킨 오류의 유형입니다. 가능한 값이 2 개- `RepeatedToken` 및 `UnknownToken`합니다.
- `suggestions` – 맞춤법 및 문법 오류를 해결 하는 단어의 배열입니다. 배열의 이루어져는 `suggestion` 및 `score`, 제안 된 수정 올바른지 신뢰 수준을 나타냅니다.

으로 deserialize 하는 JSON 응답 예제 응용 프로그램에서는 `SpellCheckResult` 표시 하기 위해 호출 하는 메서드에서 반환 되는 결과 함께 인스턴스. 다음 코드 예제는 방법을 `SpellCheckResult` 인스턴스 표시 하기 위해 처리 됩니다.

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

이 코드 반복는 `FlaggedTokens` 컬렉션 및 모든 철자가 잘못 된 대체 또는 문법적으로 잘못 된 단어의 첫 번째 제안 된 원본 텍스트입니다. 다음 스크린샷에서 전과 후 맞춤법 검사를 보여 줍니다.

![](spell-check-images/before-spell-check.png "맞춤법을 검사 하기 전에")

![](spell-check-images/after-spell-check.png "맞춤법을 검사 한 후")

## <a name="summary"></a>요약

이 문서를 Xamarin.Forms 응용 프로그램에서 맞춤법 검사 오류를 해결 하려면 Bing 맞춤법 검사 REST API를 사용 하는 방법을 설명 합니다. Bing 맞춤법 검사 맞춤법이 틀린된 단어에 대 한 추천 단어 인라인 상황별 맞춤법 텍스트에 대 한 검사 수행 합니다.

## <a name="related-links"></a>관련 링크

- [Bing 맞춤법 검사 설명서](/azure/cognitive-services/bing-spell-check/)
- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Todo Cognitive 서비스 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Bing 맞춤법 검사 API v7 참조](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
