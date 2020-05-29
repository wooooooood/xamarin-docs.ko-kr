---
제목: "RESTful 웹 서비스 사용" 설명: "웹 서비스를 응용 프로그램에 통합 하는 것은 일반적인 시나리오입니다. 이 문서에서는 응용 프로그램에서 RESTful 웹 서비스를 사용 하는 방법을 보여 줍니다 Xamarin.Forms . "
assetid: B540910C-9C51-416A-AAB9-057BF76489C3: xamarin-forms author: davidbritch: dabritch:: 05/28/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="consume-a-restful-web-service"></a>RESTful 웹 서비스 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)

_웹 서비스를 응용 프로그램에 통합 하는 것은 일반적인 시나리오입니다. 이 문서에서는 응용 프로그램에서 RESTful 웹 서비스를 사용 하는 방법을 보여 줍니다 Xamarin.Forms ._

REST (Representational State Transfer)는 웹 서비스를 빌드하기 위한 아키텍처 스타일입니다. REST 요청은 웹 브라우저에서 웹 페이지를 검색 하 고 서버에 데이터를 보내는 데 사용 하는 것과 동일한 HTTP 동사를 사용 하 여 HTTP를 통해 수행 됩니다. 동사는 다음과 같습니다.

- **GET** –이 작업은 웹 서비스에서 데이터를 검색 하는 데 사용 됩니다.
- **POST** –이 작업은 웹 서비스에서 데이터의 새 항목을 만드는 데 사용 됩니다.
- **PUT** –이 작업은 웹 서비스의 데이터 항목을 업데이트 하는 데 사용 됩니다.
- **PATCH** –이 작업은 항목을 수정 하는 방법에 대 한 지침 집합을 설명 하 여 웹 서비스에서 데이터 항목을 업데이트 하는 데 사용 됩니다. 이 동사는 샘플 응용 프로그램에서 사용 되지 않습니다.
- **삭제** –이 작업은 웹 서비스의 데이터 항목을 삭제 하는 데 사용 됩니다.

REST를 준수 하는 웹 서비스 Api는 RESTful Api 라고 하며 다음을 사용 하 여 정의 됩니다.

- 기본 URI입니다.
- GET, POST, PUT, PATCH 또는 DELETE와 같은 HTTP 메서드입니다.
- JavaScript Object Notation (JSON)와 같은 데이터에 대 한 미디어 유형입니다.

RESTful 웹 서비스는 일반적으로 JSON 메시지를 사용 하 여 클라이언트에 데이터를 반환 합니다. JSON은 압축 페이로드를 생성 하는 텍스트 기반 데이터 교환 형식으로, 데이터를 보낼 때 대역폭 요구 사항이 감소 합니다. 샘플 응용 프로그램은 open source [newtonsoft.json JSON.NET 라이브러리](https://www.newtonsoft.com/json) 를 사용 하 여 메시지를 serialize 및 deserialize 합니다.

REST의 단순성은 모바일 응용 프로그램에서 웹 서비스에 액세스 하는 기본 방법입니다.

샘플 응용 프로그램을 실행 하면 다음 스크린샷에 표시 된 것 처럼 로컬에서 호스팅된 REST 서비스에 연결 합니다.

![](rest-images/portal.png "Sample Application")

> [!NOTE]
> IOS 9 이상에서 ATS (App Transport Security)는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간 보안 연결을 적용 하 여 중요 한 정보가 실수로 공개 되는 것을 방지 합니다. ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되어 있으므로 모든 연결에 ATS 보안 요구 사항이 적용 됩니다. 연결이 이러한 요구 사항을 충족 하지 않으면 예외와 함께 실패 합니다.
>
>**HTTPS** 프로토콜을 사용 하 고 인터넷 리소스에 대 한 보안 통신을 사용할 수 없는 경우 ATS를 옵트아웃 (opt out) 할 수 있습니다. 이는 앱의 **info.plist** 파일을 업데이트 하 여 수행할 수 있습니다. 자세한 내용은 [앱 전송 보안](~/ios/app-fundamentals/ats.md)을 참조 하세요.

## <a name="consuming-the-web-service"></a>웹 서비스 사용

REST 서비스는 ASP.NET Core를 사용 하 여 작성 되며 다음 작업을 제공 합니다.

|작업(Operation)|HTTP 메서드|상대 URI|매개 변수|
|--- |--- |--- |--- |
|할 일 항목의 목록 가져오기|GET|/api/todoitems/|
|새 할 일 항목 만들기|POST|/api/todoitems/|JSON 형식의 TodoItem|
|할 일 항목 업데이트|PUT|/api/todoitems/|JSON 형식의 TodoItem|
|할 일 항목 삭제|Delete|/api/todoitems/{id}|

대부분의 Uri는 `TodoItem` 경로에 ID를 포함 합니다. 예를 들어 ID가 인를 삭제 하기 위해 `TodoItem` `6bb8a868-dba1-4f1a-93b7-24ebce87e243` 클라이언트는에 삭제 요청을 보냅니다 `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243` . 예제 응용 프로그램에서 사용 되는 데이터 모델에 대 한 자세한 내용은 [데이터 모델링](~/xamarin-forms/data-cloud/web-services/introduction.md)을 참조 하세요.

웹 API 프레임 워크에서 요청을 받으면 요청을 작업으로 라우팅합니다. 이러한 작업은 단순히 클래스의 public 메서드 `TodoItemsController` 입니다. 프레임 워크는 라우팅 테이블을 사용 하 여 요청에 대 한 응답으로 호출할 작업을 결정 합니다 .이 작업은 다음 코드 예제에 나와 있습니다.

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

라우팅 테이블에는 경로 템플릿이 포함 되어 있으며, Web API 프레임 워크는 HTTP 요청을 받으면 라우팅 테이블의 경로 템플릿과 URI를 일치 시 키 려 고 시도 합니다. 일치 하는 경로를 찾을 수 없는 경우 클라이언트에서 404 (찾을 수 없음) 오류가 수신 됩니다. 일치 하는 경로가 있는 경우 Web API는 다음과 같이 컨트롤러 및 작업을 선택 합니다.

- 웹 API는 컨트롤러를 찾기 위해 *{controller}* 변수 값에 "controller"를 추가 합니다.
- 작업을 찾기 위해 Web API는 HTTP 메서드를 살펴보고 특성과 동일한 HTTP 메서드로 데코레이팅된 컨트롤러 작업을 살펴봅니다.
- *{Id}* 자리 표시자 변수가 동작 매개 변수에 매핑됩니다.

REST 서비스는 기본 인증을 사용 합니다. 자세한 내용은 [RESTful 웹 서비스 인증을](~/xamarin-forms/data-cloud/authentication/rest.md)참조 하세요. ASP.NET Web API 라우팅에 대 한 자세한 내용은 ASP.NET 웹 사이트의 [ASP.NET Web API 라우팅](https://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) 을 참조 하세요. ASP.NET Core를 사용 하 여 REST 서비스를 빌드하는 방법에 대 한 자세한 내용은 [네이티브 모바일 응용 프로그램을 위한 백 엔드 서비스 만들기](/aspnet/core/mobile/native-mobile-backend/)를 참조 하세요.

`HttpClient`클래스는 HTTP를 통해 요청을 보내고 받는 데 사용 됩니다. URI 식별 리소스에서 http 요청을 보내고 HTTP 응답을 받기 위한 기능을 제공 합니다. 각 요청은 비동기 작업으로 전송 됩니다. 비동기 작업에 대 한 자세한 내용은 [Async 지원 개요](~/cross-platform/platform/async.md)를 참조 하세요.

`HttpResponseMessage`클래스는 http 요청을 만든 후 웹 서비스에서 받은 http 응답 메시지를 나타냅니다. 상태 코드, 헤더 및 모든 본문을 포함 하 여 응답에 대 한 정보를 포함 합니다. 합니다 `HttpContent` 클래스를 나타내는 HTTP 본문 및 콘텐츠 헤더와 같은 `Content-Type` 고 `Content-Encoding`입니다. `ReadAs` `ReadAsStringAsync` 데이터의 형식에 따라 및와 같은 메서드를 사용 하 여 콘텐츠를 읽을 수 있습니다 `ReadAsByteArrayAsync` .

### <a name="creating-the-httpclient-object"></a>HTTPClient 개체 만들기

`HttpClient`인스턴스는 다음 코드 예제와 같이 응용 프로그램에서 HTTP 요청을 수행 해야 하는 경우에만 개체를 사용할 수 있도록 클래스 수준에서 선언 됩니다.

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    client = new HttpClient ();
  }
  ...
}
```

### <a name="retrieving-data"></a>데이터 검색

`HttpClient.GetAsync`메서드는 다음 코드 예제와 같이 URI로 지정 된 웹 서비스에 GET 요청을 보낸 다음 웹 서비스에서 응답을 수신 하는 데 사용 됩니다.

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  Uri uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));
  ...
  HttpResponseMessage response = await client.GetAsync (uri);
  if (response.IsSuccessStatusCode)
  {
      string content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

REST 서비스는 `HttpResponseMessage.IsSuccessStatusCode` http 요청 성공 또는 실패 여부를 나타내는 http 상태 코드를 속성에 보냅니다. 이 작업의 경우 REST 서비스는 응답에 HTTP 상태 코드 200 (OK)를 보냅니다 .이는 요청이 성공 했 고 요청 된 정보가 응답에 있음을 나타냅니다.

HTTP 작업에 성공 하면 응답 콘텐츠를 표시 하기 위해 읽습니다. `HttpResponseMessage.Content`속성은 http 응답의 내용을 나타내고 `HttpContent.ReadAsStringAsync` 메서드는 http 콘텐츠를 문자열에 비동기적으로 씁니다. 그런 다음이 콘텐츠는 JSON에서 인스턴스의로 변환 됩니다 `List` `TodoItem` .

> [!WARNING]
> 메서드를 사용 하 여 `ReadAsStringAsync` 큰 응답을 검색 하면 성능에 부정적인 영향을 줄 수 있습니다. 이러한 경우에는 응답을 완전히 버퍼링 하지 않도록 응답을 직접 deserialize 해야 합니다.

### <a name="creating-data"></a>데이터 만들기

`HttpClient.PostAsync`메서드는 다음 코드 예제에 표시 된 것 처럼 URI로 지정 된 웹 서비스에 POST 요청을 보낸 다음 웹 서비스에서 응답을 수신 하는 데 사용 됩니다.

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  Uri uri = new Uri (string.Format (Constants.TodoItemsUrl, string.Empty));

  ...
  string json = JsonConvert.SerializeObject (item);
  StringContent content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem)
  {
    response = await client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully saved.");
  }
  ...
}
```

`TodoItem`인스턴스는 웹 서비스로 보내기 위해 JSON 페이로드로 변환 됩니다. 그런 다음 메서드를 사용 하 여 요청을 수행 하기 전에 웹 서비스로 전송 되는 HTTP 콘텐츠의 본문에이 페이로드를 포함 `PostAsync` 합니다.

REST 서비스는 `HttpResponseMessage.IsSuccessStatusCode` http 요청 성공 또는 실패 여부를 나타내는 http 상태 코드를 속성에 보냅니다. 이 작업에 대 한 일반적인 응답은 다음과 같습니다.

- **201 (만듦)** – 응답이 전송 되기 전에 새 리소스가 생성 된 요청입니다.
- **400 (잘못 된 요청)** -서버에서 요청을 인식할 수 없습니다.
- **409 (충돌)** -서버에 대 한 충돌로 인해 요청을 수행할 수 없습니다.

### <a name="updating-data"></a>데이터 업데이트

`HttpClient.PutAsync`메서드는 다음 코드 예제와 같이 URI로 지정 된 웹 서비스에 PUT 요청을 보낸 다음 웹 서비스에서 응답을 수신 하는 데 사용 됩니다.

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await client.PutAsync (uri, content);
  ...
}
```

메서드의 작업은 `PutAsync` `PostAsync` 웹 서비스에서 데이터를 만드는 데 사용 되는 메서드와 동일 합니다. 그러나 웹 서비스에서 전송 되는 가능한 응답은 서로 다릅니다.

REST 서비스는 `HttpResponseMessage.IsSuccessStatusCode` http 요청 성공 또는 실패 여부를 나타내는 http 상태 코드를 속성에 보냅니다. 이 작업에 대 한 일반적인 응답은 다음과 같습니다.

- **204 (콘텐츠 없음)** – 요청이 성공적으로 처리 되 고 응답은 의도적으로 비어 있습니다.
- **400 (잘못 된 요청)** -서버에서 요청을 인식할 수 없습니다.
- **404 (찾을 수 없음)** – 요청 된 리소스가 서버에 없습니다.

### <a name="deleting-data"></a>데이터 삭제

`HttpClient.DeleteAsync`메서드는 다음 코드 예제와 같이 URI로 지정 된 웹 서비스에 삭제 요청을 보낸 다음 웹 서비스에서 응답을 수신 하는 데 사용 됩니다.

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  Uri uri = new Uri (string.Format (Constants.TodoItemsUrl, id));
  ...
  HttpResponseMessage response = await client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode)
  {
    Debug.WriteLine (@"\tTodoItem successfully deleted.");
  }
  ...
}
```

REST 서비스는 `HttpResponseMessage.IsSuccessStatusCode` http 요청 성공 또는 실패 여부를 나타내는 http 상태 코드를 속성에 보냅니다. 이 작업에 대 한 일반적인 응답은 다음과 같습니다.

- **204 (콘텐츠 없음)** – 요청이 성공적으로 처리 되 고 응답은 의도적으로 비어 있습니다.
- **400 (잘못 된 요청)** -서버에서 요청을 인식할 수 없습니다.
- **404 (찾을 수 없음)** – 요청 된 리소스가 서버에 없습니다.

## <a name="related-links"></a>관련 링크

- [네이티브 모바일 응용 프로그램에 대 한 백 엔드 서비스 만들기](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
