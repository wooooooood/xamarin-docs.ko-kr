---
title: RESTful 웹 서비스 사용
description: 응용 프로그램에 웹 서비스를 통합 하는 것은 일반적인 시나리오입니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 RESTful 웹 서비스를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
ms.openlocfilehash: cb569a425bf636a51dd6d132f6efa539e74443a0
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644572"
---
# <a name="consume-a-restful-web-service"></a>RESTful 웹 서비스 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)

_응용 프로그램에 웹 서비스를 통합 하는 것은 일반적인 시나리오입니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 RESTful 웹 서비스를 사용 하는 방법에 설명 합니다._

REST representational State Transfer ()는 웹 서비스 구축을 위한 아키텍처 스타일입니다. REST 요청 웹 페이지를 검색 하 고 서버에 데이터를 보내도록 웹 브라우저를 사용 하는 동일한 HTTP 동사를 사용 하 여 HTTP를 통해 수행 됩니다. 동사는 다음과 같습니다.

- **가져올** –이 작업은 웹 서비스에서 데이터를 검색 하려면 사용 합니다.
- **POST** –이 작업은 웹 서비스에서 데이터의 새 항목을 만드는 데 사용 합니다.
- **배치** –이 작업은 웹 서비스에서 데이터 항목을 업데이트 하는 데 사용 됩니다.
- **패치** –이 작업은 항목을 수정 해야 하는 방법에 대 한 지침의 집합을 설명 하 여 웹 서비스에서 데이터 항목을 업데이트 하려면 사용 합니다. 이 동사는 샘플 응용 프로그램에서 사용 되지 않습니다.
- **삭제** –이 작업은 웹 서비스에서 데이터 항목을 삭제 하려면 사용 합니다.

웹 서비스 REST를 준수 하는 Api는 RESTful Api를 호출 하 고를 사용 하 여 정의 됩니다.

- 기본 URI입니다.
- GET, POST, PUT, PATCH 또는 DELETE 같은 HTTP 메서드.
- 같은 개체 JSON (JavaScript Notation) 데이터에 대 한 미디어 유형입니다.

RESTful 웹 서비스는 일반적으로 클라이언트에 데이터를 반환할 JSON 메시지를 사용 합니다. JSON은 데이터를 보낼 때 생성 압축 된 페이로드를 발생 하는 대역폭 요구 사항을 감소는 텍스트 기반 데이터 교환 형식입니다. 샘플 응용 프로그램은 오픈 소스 [NewtonSoft JSON.NET 라이브러리](http://www.newtonsoft.com/json) serialize 하 고 메시지를 deserialize 합니다.

REST의 단순성 모바일 응용 프로그램에서 웹 서비스에 액세스 하기 위한 기본 방법은 있도록 비디오나 합니다.

샘플 응용 프로그램을 실행 하면 다음 스크린샷에 표시 된 것 처럼 로컬에서 호스팅된 REST 서비스에 연결 합니다.

![](rest-images/portal.png "샘플 응용 프로그램")

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결 하므로 중요 한 정보가 실수로 유출 방지 적용 합니다. ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되므로 모든 연결이 ATS 보안 요구 사항이 적용 됩니다. 연결에서 이러한 요구를 충족 하지 않는, 예외와 함께 실패 합니다.
>
>사용할 수 없는 경우의 ATS 옵트인 수 있습니다 합니다 **HTTPS** 프로토콜 및 인터넷 리소스에 대 한 통신을 보호 합니다. 이 앱을 업데이트 하 여 수행할 수 있습니다 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="consuming-the-web-service"></a>웹 서비스 사용

REST 서비스는 ASP.NET Core를 사용 하 여 작성 됩니다 하 고 다음 작업을 제공 합니다.

|작업|HTTP 메서드|상대 URI|매개 변수|
|--- |--- |--- |--- |
|할 일 항목의 목록 가져오기|가져오기|/api/todoitems/|
|할 일 항목을 새로 만들려면|올리기|/api/todoitems/|JSON 형식의 TodoItem|
|할 일 항목 업데이트|PUT|/api/todoitems/|JSON 형식의 TodoItem|
|할 일 항목 삭제|Delete|/api/todoitems/{id}|

대부분의 Uri 포함 합니다 `TodoItem` 경로에 대 한 ID입니다. 예를 들어, 삭제 하는 `TodoItem` ID가 갖는 `6bb8a868-dba1-4f1a-93b7-24ebce87e243`, 클라이언트가 삭제 요청을 보냅니다 `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243`합니다. 샘플 응용 프로그램에 사용 된 데이터 모델에 대 한 자세한 내용은 참조 하세요. [데이터 모델링은](~/xamarin-forms/data-cloud/web-services/introduction.md)합니다.

Web API 프레임 워크는 요청을 받으면 요청을 작업에 라우팅합니다. 이러한 작업은 단순한 공용 메서드는 `TodoItemsController` 클래스입니다. 다음 코드 예제에 나와 있는 요청에 대 한 응답으로 호출 하는 작업을 확인 하려면 라우팅 테이블을 사용 하는 프레임 워크:

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

라우팅 테이블에 경로 템플릿 및 라우팅 테이블에 경로 템플릿에 대 한 URI를 일치 시 키 려 고 Web API 프레임 워크는 HTTP 요청을 받으면 합니다. 일치 하는 경우 클라이언트는 404 (찾을 수 없음) 오류를 받습니다. 경로 찾을 수 없습니다. 일치 하는 경로가 없으면 웹 API 선택 컨트롤러 및 작업을 다음과 같이 합니다.

- "Controller"의 값에 Web API 컨트롤러를 찾으려면 추가 합니다 *{컨트롤러}* 변수입니다.
- 작업을 찾으려면 Web API HTTP 메서드를 찾아 특성으로 동일한 HTTP 메서드를 사용 하 여 데코레이팅된 컨트롤러 작업을 살펴봅니다.
- 합니다 *{id}* 자리 표시자 변수는 작업 매개 변수에 매핑됩니다.

REST 서비스는 기본 인증을 사용합니다. 자세한 내용은 참조 [RESTful 웹 서비스를 인증](~/xamarin-forms/data-cloud/authentication/rest.md)합니다. ASP.NET Web API 라우팅에 대 한 자세한 내용은 참조 하세요. [ASP.NET Web API에서 라우팅](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) ASP.NET 웹 사이트입니다. ASP.NET Core를 사용 하 여 REST 서비스를 구축 하는 방법에 대 한 자세한 내용은 참조 하세요. [네이티브 모바일 응용 프로그램에 대 한 백 엔드 서비스 만들기](/aspnet/core/mobile/native-mobile-backend/)합니다.

`HttpClient` 클래스는 HTTP를 통한 요청을 받고 보내는 데 사용 됩니다. HTTP 요청을 보내는 기능을 제공 하 고 리소스를 식별 하는 URI에서 HTTP 응답을 수신 합니다. 각 요청을 비동기 작업으로 전송 됩니다. 비동기 작업에 대 한 자세한 내용은 참조 하세요. [비동기 지원 개요](~/cross-platform/platform/async.md)합니다.

`HttpResponseMessage` 클래스는 HTTP 요청을 생성 된 후 웹 서비스에서 받은 HTTP 응답 메시지를 나타냅니다. 상태 코드, 헤더 및 모든 본문을 포함 하 여 응답에 대 한 정보를 포함 합니다. 합니다 `HttpContent` 클래스를 나타내는 HTTP 본문 및 콘텐츠 헤더와 같은 `Content-Type` 고 `Content-Encoding`입니다. 사용 하 여 콘텐츠를 읽을 수 있습니다 합니다 `ReadAs` 메서드를 같은 `ReadAsStringAsync` 및 `ReadAsByteArrayAsync`데이터의 형식에 따라 합니다.

### <a name="creating-the-httpclient-object"></a>HTTPClient 개체 만들기

`HttpClient` 클래스 수준에서 인스턴스는 선언으로 다음 코드 예제와 같이 HTTP 요청을 해야 응용 프로그램 개체에 대 한 존재 합니다.

```csharp
public class RestService : IRestService
{
  HttpClient _client;
  ...

  public RestService ()
  {
    _client = new HttpClient ();
  }
  ...
}
```

### <a name="retrieving-data"></a>데이터 검색

`HttpClient.GetAsync` 다음 코드 예제에 표시 된 대로 메서드는 URI로 지정 된 웹 서비스에 GET 요청을 보내고 다음 웹 서비스에서 응답을 수신 하는:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));
  ...
  var response = await _client.GetAsync (uri);
  if (response.IsSuccessStatusCode)
  {
      var content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

REST 서비스 HTTP 상태 코드를 보냅니다는 `HttpResponseMessage.IsSuccessStatusCode` 속성을 HTTP 요청의 성공 또는 실패 여부를 나타냅니다. 이 작업의 나머지 서비스는 요청이 성공 하 고 응답에서 요청된 된 정보는 응답에서 HTTP 상태 코드를 200 (정상)를 보냅니다.

HTTP 작업을 성공적으로 수행 되었으면 표시에 대 한 응답의 콘텐츠를 읽을 합니다. `HttpResponseMessage.Content` 속성은 HTTP 응답의 콘텐츠를 나타냅니다와 `HttpContent.ReadAsStringAsync` 메서드를 문자열로 된 HTTP 콘텐츠를 비동기적으로 씁니다. 이 콘텐츠를 JSON에서 변환 됩니다는 `List` 의 `TodoItem` 인스턴스.

### <a name="creating-data"></a>데이터 만들기

`HttpClient.PostAsync` 메서드는 URI로 지정 된 웹 서비스에 POST 요청을 보내는 데 사용 되 고 다음 코드 예제에 표시 된 대로 웹 서비스에서 응답을 받을 수:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  var uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));

  ...
  var json = JsonConvert.SerializeObject (item);
  var content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem)
  {
    response = await _client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully saved.");

  }
  ...
}
```

`TodoItem` 인스턴스 웹 서비스에 보낼 JSON 페이로드로 변환 됩니다. 이 페이로드에 사용 하 여 요청 되기 전에 웹 서비스에 전송 된 HTTP 콘텐츠 본문에 포함 된 `PostAsync` 메서드.

REST 서비스 HTTP 상태 코드를 보냅니다는 `HttpResponseMessage.IsSuccessStatusCode` 속성을 HTTP 요청의 성공 또는 실패 여부를 나타냅니다. 이 작업에 대 한 일반적인 응답 다음과 같습니다.

- **201 (만들어짐)** – 응답이 보내지기 전에 만들어지는 새 리소스를 요청 했습니다.
- **400 (잘못 된 요청)** – 요청 서버 인식할 수 없습니다.
- **409 (충돌)** – 요청 서버의 충돌이 발생 하지 수행 수 없습니다.

### <a name="updating-data"></a>데이터 업데이트

`HttpClient.PutAsync` 다음 코드 예제에 표시 된 대로 메서드는 URI로 지정 된 웹 서비스에 PUT 요청을 보내고 다음 웹 서비스에서 응답을 수신 하는:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await _client.PutAsync (uri, content);
  ...
}
```
작업을 `PutAsync` 메서드를 `PostAsync` 웹 서비스에서 데이터를 만들기 위한 사용 되는 메서드. 그러나 웹 서비스에서 전송 가능한 응답 다릅니다.

REST 서비스 HTTP 상태 코드를 보냅니다는 `HttpResponseMessage.IsSuccessStatusCode` 속성을 HTTP 요청의 성공 또는 실패 여부를 나타냅니다. 이 작업에 대 한 일반적인 응답 다음과 같습니다.

- **204 (내용 없음)** – 요청이 성공적으로 처리 되 고 응답 의도적으로 비어 있습니다.
- **400 (잘못 된 요청)** – 요청 서버 인식할 수 없습니다.
- **404 (찾을 수 없음)** – 서버에서 요청한 리소스가 존재 하지 않습니다.

### <a name="deleting-data"></a>데이터 삭제

`HttpClient.DeleteAsync` 다음 코드 예제 에서처럼 URI에 지정 된 웹 서비스를 삭제 요청을 보내고 다음 웹 서비스에서 응답을 수신 메서드는:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  var uri = new Uri (string.Format (Constants.TodoItemsUrl, id));
  ...
  var response = await _client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully deleted.");
  }
  ...
}
```

REST 서비스 HTTP 상태 코드를 보냅니다는 `HttpResponseMessage.IsSuccessStatusCode` 속성을 HTTP 요청의 성공 또는 실패 여부를 나타냅니다. 이 작업에 대 한 일반적인 응답 다음과 같습니다.

- **204 (내용 없음)** – 요청이 성공적으로 처리 되 고 응답 의도적으로 비어 있습니다.
- **400 (잘못 된 요청)** – 요청 서버 인식할 수 없습니다.
- **404 (찾을 수 없음)** – 서버에서 요청한 리소스가 존재 하지 않습니다.

## <a name="related-links"></a>관련 링크

- [네이티브 모바일 응용 프로그램에 대한 백 엔드 서비스 만들기](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
