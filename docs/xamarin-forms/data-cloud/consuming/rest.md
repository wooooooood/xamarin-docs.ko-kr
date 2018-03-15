---
title: "RESTful 웹 서비스 사용"
description: "응용 프로그램으로 통합 하는 웹 서비스는 일반적인 시나리오입니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 RESTful 웹 서비스를 사용 하는 방법을 보여줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 98c38001ea7751c419d4be5b0f68339b06ec656f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="consuming-a-restful-web-service"></a>RESTful 웹 서비스 사용

_응용 프로그램으로 통합 하는 웹 서비스는 일반적인 시나리오입니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 RESTful 웹 서비스를 사용 하는 방법을 보여줍니다._

REPRESENTATIONAL State Transfer ()은 웹 서비스를 구축 하기 위한 아키텍처는 스타일입니다. REST 요청은 HTTP를 통해 웹 페이지를 검색 하 고 서버에 데이터를 보내지 웹 브라우저를 사용 하는 동일한 HTTP 동사를 사용 하 여. 동사 됩니다.:

- **가져오기** -이 작업을 사용 하 여 웹 서비스에서 데이터를 검색 합니다.
- **POST** -이 작업은 웹 서비스에서 데이터의 새 항목을 만드는 데 사용 됩니다.
- **배치** -이 작업을 사용 하는 웹 서비스에서 데이터 항목을 업데이트 합니다.
- **패치** –이 작업을 사용 하는 항목을 수정 해야 하는 방법에 대 한 일련의 지침을 설명 하 여 웹 서비스에서 데이터 항목을 업데이트 합니다. 이 동사 샘플 응용 프로그램에서 사용 되지 않습니다.
- **삭제** -이 작업을 사용 하는 웹 서비스에서 데이터 항목을 삭제 합니다.

웹 서비스 Api REST를 준수 하는 RESTful Api 라고 하며 사용 하 여 정의 됩니다.

- 기본 URI입니다.
- GET, POST, PUT, PATCH 또는 DELETE와 같은 HTTP 메서드
- 같은 개체 JSON (JavaScript Notation) 데이터에 대 한 미디어 유형입니다.

RESTful 웹 서비스는 일반적으로 클라이언트에 데이터를 반환할 JSON 메시지를 사용 합니다. JSON은 그 결과 생성 압축 페이로드 데이터를 보낼 때 대역폭 요구 사항이 감소 하는 텍스트 기반 데이터 교환 형식입니다. 샘플 응용 프로그램에서 오픈 소스 [NewtonSoft JSON.NET 라이브러리](http://www.newtonsoft.com/json) serialize 하 고 메시지를 역직렬화 합니다.

모바일 응용 프로그램에서 웹 서비스에 액세스 하기 위한 기본 방법 확인 REST은 단순 하기 때문에 작업 수 있습니다.

샘플 응용 프로그램을 함께 제공 되는 추가 정보 파일에 REST 서비스를 설정에 대 한 지침을 찾을 수 있습니다. 그러나 예제 응용 프로그램을 실행 합니다 연결 데이터에 대 한 읽기 전용 액세스를 제공 하는 Xamarin 호스팅되지 REST 서비스에 다음 스크린샷에 표시 된 것 처럼:

![](rest-images/portal.png "샘플 응용 프로그램")

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간에 보안 연결 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 적용 합니다. IOS 9에 대해 빌드된 앱에서 기본적으로 AT를 사용 하므로 모든 연결 AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.
>
>사용할 수 없는 경우의 AT 옵트인 수 있습니다는 **HTTPS** 및 프로토콜을 인터넷 리소스에 대 한 통신을 보호 합니다. 응용 프로그램의 업데이트 하 여 이렇게 할 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="consuming-the-web-service"></a>웹 서비스 사용

REST 서비스는 ASP.NET Core를 사용 하 여 작성 된 하 고 다음 작업을 제공 합니다.

|작업|HTTP 메서드|상대 URI|매개 변수|
|--- |--- |--- |--- |
|할 일 항목의 목록 가져오기|가져오기|/api/todoitems/|
|새 할 일 항목을 만듭니다.|올리기|/api/todoitems/|JSON 형식의 TodoItem|
|할 일 항목 업데이트|PUT|/api/todoitems/|JSON 형식의 TodoItem|
|할 일 항목 삭제|Delete|/api/todoitems/{id}|

Uri의 대부분 포함는 `TodoItem` 경로에 대 한 ID입니다. 예를 들어, 삭제 하는 `TodoItem` ID가 갖는 `6bb8a868-dba1-4f1a-93b7-24ebce87e243`, 클라이언트는 DELETE 요청을 보냅니다 `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243`합니다. 샘플 응용 프로그램에서 사용 되는 데이터 모델에 대 한 자세한 내용은 참조 [데이터 모델링](~/xamarin-forms/data-cloud/walkthrough.md)합니다.

Web API 프레임 워크에서는 요청을 받으면 요청을 동작을 라우팅합니다. 이러한 작업은 단순히 공용 메서드는 `TodoItemsController` 클래스입니다. 프레임 워크는 라우팅 테이블을 사용 하 여 다음 코드 예제에 표시 된 요청에 대 한 응답으로 호출 하는 작업을 결정 하려면:

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

라우팅 테이블에는 경로 템플릿 및 Web API 프레임 워크는 HTTP 요청을 받으면 다시 일치 하는 라우팅 테이블에 경로 템플릿에 대 한 URI입니다. 일치 하는 경우 클라이언트가 404 (찾을 수 없음) 오류를 수신 경로 찾을 수 없습니다. 일치 하는 경로 있으면 웹 API 선택 컨트롤러 및 작업 다음과 같이 합니다.

- "컨트롤러"의 값에 Web API 컨트롤러를 찾으려면 추가 *{컨트롤러}* 변수입니다.
- 작업을 찾으려면 Web API HTTP 메서드 찾은 동일한 HTTP 메서드를 특성으로 데코레이팅되는 컨트롤러 작업 확인 합니다.
- *{id}* 작업 매개 변수 자리 표시자 변수 매핑됩니다.

나머지 서비스는 기본 인증을 사용합니다. 자세한 내용은 참조 [RESTful 웹 서비스를 인증](~/xamarin-forms/data-cloud/authentication/rest.md)합니다. ASP.NET Web API 라우팅에 대 한 자세한 내용은 참조 [ASP.NET Web API에서 라우팅](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) ASP.NET 웹 사이트에서. ASP.NET Core를 사용 하 여 REST 서비스를 작성 하는 방법에 대 한 자세한 내용은 참조 [네이티브 모바일 응용 프로그램에 대 한 백 엔드 서비스를 만드는](/aspnet/core/mobile/native-mobile-backend/)합니다.

> [!NOTE]
> 샘플 응용 프로그램 웹 서비스에 대 한 읽기 전용 액세스를 제공 하는 Xamarin 호스팅되지 REST 서비스를 사용 합니다. 따라서 생성, 업데이트 및 데이터를 삭제 하는 작업은 응용 프로그램에서 사용 하는 데이터가 변경 하지 않습니다. 그러나 REST 서비스의 호스팅 가능한 버전은 영어로 **TodoRESTService** 에 함께 제공 되는 폴더 [샘플 코드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)합니다.
> 전체 허용 REST 서비스를 직접 호스팅하는 경우 만들기, 업데이트, 읽기, 및 데이터에 대 한 액세스를 삭제 합니다.

`HttpClient` 클래스는 HTTP를 통한 요청을 받거나 보내기 위해 사용 됩니다. HTTP 요청을 보내기 위한 기능을 제공 하 고 리소스를 식별 하는 URI에서 HTTP 응답을 받기. 각 요청을 비동기 작업으로 전송 됩니다. 비동기 작업에 대 한 자세한 내용은 참조 [비동기 지원 개요](~/cross-platform/platform/async.md)합니다.

`HttpResponseMessage` 클래스는 HTTP 요청 수행 된 후 웹 서비스에서 받은 HTTP 응답 메시지를 나타냅니다. 상태 코드, 헤더 및 모든 본문을 포함 하 여 응답에 대 한 정보를 포함 합니다. `HttpContent` 클래스를 나타내는 HTTP 본문 및 콘텐츠 헤더와 같은 `Content-Type` 및 `Content-Encoding`합니다. 사용 하 여 콘텐츠를 읽을 수는 `ReadAs` 메서드 같은 `ReadAsStringAsync` 및 `ReadAsByteArrayAsync`데이터의 형식에 따라 합니다.

### <a name="creating-the-httpclient-object"></a>HTTPClient 개체 만들기

`HttpClient` 클래스 수준에서 인스턴스는 선언으로 다음 코드 예제와 같이 HTTP 요청을 만들기 위해 필요한 응용 프로그램에 대 한 거주 하 고 개체:

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    client = new HttpClient ();
    client.MaxResponseContentBufferSize = 256000;
  }
  ...
}
```

`HttpClient.MaxResponseContentBufferSize` 속성은 HTTP 응답 메시지의 콘텐츠를 읽을 때 버퍼링 할 바이트의 최대 수를 지정 하는 데 사용 합니다. 이 속성의 기본 크기는 최대 크기는 정수입니다. 따라서 속성 설정은 작은 값으로 안전 조치로, 웹 서비스의 응답으로 응용 프로그램을 허용 하는 데이터 양을 제한 합니다.

### <a name="retrieving-data"></a>데이터 검색

`HttpClient.GetAsync` 다음 코드 예제에 나와 있는 것 처럼 메서드는 URI로 지정 된 웹 서비스에 GET 요청을 보내고 다음 웹 서비스에서 응답을 받는를 사용 합니다.

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));
  ...
  var response = await client.GetAsync (uri);
  if (response.IsSuccessStatusCode) {
      var content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

REST 서비스 HTTP 상태 코드를 보냅니다는 `HttpResponseMessage.IsSuccessStatusCode` HTTP 요청의 성공 또는 실패를 나타내는 속성입니다. 이 작업의 나머지 서비스는 요청이 성공 하 고 응답에 요청 된 정보를 나타내는 응답에 HTTP 상태 코드를 200 (정상)를 보냅니다.

HTTP 작업에 성공 하면 표시에 대 한 응답의 콘텐츠를 읽을 합니다. `HttpResponseMessage.Content` 속성 HTTP 응답의 콘텐츠를 나타내는 및 `HttpContent.ReadAsStringAsync` 메서드를 문자열로 비동기적으로 HTTP 콘텐츠를 씁니다. 이 콘텐츠는 JSON을에서 변환 된 후 한 `List` 의 `TodoItem` 인스턴스.

### <a name="creating-data"></a>데이터 만들기

`HttpClient.PostAsync` 메서드는 URI로 지정 된 웹 서비스에 POST 요청을 보내는 데 사용 되 응답을 받을 웹 서비스에서 다음 코드 예제에 표시 된 대로 다음:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));

  ...
  var json = JsonConvert.SerializeObject (item);
  var content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem) {
    response = await client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully saved.");

  }
  ...
}
```

`TodoItem` 인스턴스는 웹 서비스에 보내기 위한 JSON 페이로드도 변환 됩니다. 이 페이로드에 요청은 먼저 웹 서비스에 보낼 수 있는 HTTP 콘텐츠 본문에 포함 된 `PostAsync` 메서드.

REST 서비스 HTTP 상태 코드를 보냅니다는 `HttpResponseMessage.IsSuccessStatusCode` HTTP 요청의 성공 또는 실패를 나타내는 속성입니다. 이 작업에 대 한 일반적인 응답은:

- **201 (만들어짐)** – 요청 응답이 보내지기 전에 만들어지는 새 리소스에서 발생 했습니다.
- **400 (잘못 된 요청)** – 요청이 서버에서 인식할 수 없는 합니다.
- **409 (충돌)** – 요청 서버에서 충돌이 발생 하지 수행 수 없습니다.

### <a name="updating-data"></a>데이터 업데이트

`HttpClient.PutAsync` 다음 코드 예제에 나와 있는 것 처럼 메서드는 URI로 지정 된 웹 서비스에 PUT 요청을 보내고 다음 웹 서비스에서 응답을 받는를 사용 합니다.

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await client.PutAsync (uri, content);
  ...
}
```
작업은 `PutAsync` 메서드는 `PostAsync` 웹 서비스에서 데이터를 만드는 데 사용 되는 메서드. 그러나 웹 서비스에서 보내는 찼을 다릅니다.

REST 서비스 HTTP 상태 코드를 보냅니다는 `HttpResponseMessage.IsSuccessStatusCode` HTTP 요청의 성공 또는 실패를 나타내는 속성입니다. 이 작업에 대 한 일반적인 응답은:

- **204 (콘텐츠 없음)** – 요청이 성공적으로 처리 되 고 응답은 비어 있습니다.
- **400 (잘못 된 요청)** – 요청이 서버에서 인식할 수 없는 합니다.
- **404 (찾을 수 없음)** -요청된 된 리소스는 서버에 존재 하지 않습니다.

### <a name="deleting-data"></a>데이터 삭제

`HttpClient.DeleteAsync` 다음 코드 예제에 나와 있는 것 처럼 메서드는 URI로 지정 된 웹 서비스에 DELETE 요청을 보내고 다음 웹 서비스에서 응답을 받는를 사용 합니다.

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/{0}
  var uri = new Uri (string.Format (Constants.RestUrl, id));
  ...
  var response = await client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully deleted.");
  }
  ...
}
```

REST 서비스 HTTP 상태 코드를 보냅니다는 `HttpResponseMessage.IsSuccessStatusCode` HTTP 요청의 성공 또는 실패를 나타내는 속성입니다. 이 작업에 대 한 일반적인 응답은:

- **204 (콘텐츠 없음)** – 요청이 성공적으로 처리 되 고 응답은 비어 있습니다.
- **400 (잘못 된 요청)** – 요청이 서버에서 인식할 수 없는 합니다.
- **404 (찾을 수 없음)** -요청된 된 리소스는 서버에 존재 하지 않습니다.

## <a name="summary"></a>요약

이 문서는 Xamarin.Forms 응용 프로그램에서 RESTful 웹 서비스를 사용 하는 방법을 검사를 사용 하는 `HttpClient` 클래스입니다. 모바일 응용 프로그램에서 웹 서비스에 액세스 하기 위한 기본 방법 확인 REST은 단순 하기 때문에 작업 수 있습니다.


## <a name="related-links"></a>관련 링크

- [네이티브 모바일 응용 프로그램에 대한 백 엔드 서비스 만들기](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
