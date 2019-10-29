---
title: 웹 서비스 소개
description: 이 가이드에서는 다양 한 웹 서비스 기술을 사용 하는 방법을 보여 줍니다. 여기서 다루는 항목에는 REST 서비스와의 통신, SOAP 서비스 및 Windows Communication Foundation 서비스가 포함 됩니다.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 50302b0b9cf96d211c704ab9e68d1c61d11e807a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016578"
---
# <a name="introduction-to-web-services"></a>웹 서비스 소개

_이 가이드에서는 다양 한 웹 서비스 기술을 사용 하는 방법을 보여 줍니다. 여기서 다루는 항목에는 REST 서비스와의 통신, SOAP 서비스 및 Windows Communication Foundation 서비스가 포함 됩니다._

제대로 작동 하려면 많은 모바일 응용 프로그램이 클라우드에 종속 되므로 웹 서비스를 모바일 응용 프로그램에 통합 하는 것이 일반적인 시나리오입니다. Xamarin 플랫폼은 다양 한 웹 서비스 기술 사용을 지원 하 고 RESTful, ASMX 및 Windows Communication Foundation (WCF) 서비스 사용을 위한 내장 및 타사 지원을 포함 합니다.

Xamarin.ios를 사용 하는 고객의 경우 [Xamarin.ios 웹 서비스](~/xamarin-forms/data-cloud/index.yml) 설명서에서 이러한 각 기술을 사용 하는 전체 예제가 있습니다.

> [!IMPORTANT]
> IOS 9에서 앱 전송 보안 (ATS)은 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결을 적용 하 여 중요 한 정보가 실수로 공개 되는 것을 방지 합니다.
> ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되어 있으므로 모든 연결에 ATS 보안 요구 사항이 적용 됩니다. 연결이 이러한 요구 사항을 충족 하지 않으면 예외와 함께 실패 합니다.

`HTTPS` 프로토콜을 사용 하 고 인터넷 리소스에 대 한 보안 통신을 할 수 없는 경우 ATS를 옵트아웃 (opt out) 할 수 있습니다. 이는 앱의 **info.plist** 파일을 업데이트 하 여 수행할 수 있습니다. 자세한 내용은 [앱 전송 보안](~/ios/app-fundamentals/ats.md)을 참조 하세요.

## <a name="rest"></a>REST

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

REST의 단순성은 모바일 응용 프로그램에서 웹 서비스에 액세스 하는 기본 방법입니다.

## <a name="consuming-rest-services"></a>REST 서비스 사용

REST 서비스를 사용 하는 데 사용할 수 있는 라이브러리와 클래스에는 여러 가지가 있으며 다음 하위 섹션에서 설명 합니다. REST 서비스를 사용 하는 방법에 대 한 자세한 내용은 [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/rest.md)을 참조 하세요.

### <a name="httpclient"></a>HttpClient

[MICROSOFT Http 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Net.Http) 는 http를 통해 요청을 보내고 받는 데 사용 되는 `HttpClient` 클래스를 제공 합니다. URI로 식별 된 리소스에서 http 요청을 보내고 HTTP 응답을 받기 위한 기능을 제공 합니다. 각 요청은 비동기 작업으로 전송 됩니다. 비동기 작업에 대 한 자세한 내용은 [Async 지원 개요](~/cross-platform/platform/async.md)를 참조 하세요.

`HttpResponseMessage` 클래스는 HTTP 요청을 만든 후 웹 서비스에서 받은 HTTP 응답 메시지를 나타냅니다. 상태 코드, 헤더 및 본문을 포함 하 여 응답에 대 한 정보를 포함 합니다. 합니다 `HttpContent` 클래스를 나타내는 HTTP 본문 및 콘텐츠 헤더와 같은 `Content-Type` 고 `Content-Encoding`입니다. 데이터의 형식에 따라 `ReadAsStringAsync` 및 `ReadAsByteArrayAsync`와 같은 `ReadAs` 방법 중 하나를 사용 하 여 콘텐츠를 읽을 수 있습니다.

`HttpClient` 클래스에 대 한 자세한 내용은 [HTTPClient 개체 만들기](~/xamarin-forms/data-cloud/web-services/rest.md)를 참조 하세요.

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

`HTTPWebRequest`를 사용 하 여 웹 서비스를 호출 하는 방법은 다음과 같습니다.

- 특정 URI에 대 한 요청 인스턴스를 만듭니다.
- 요청 인스턴스에 다양 한 HTTP 속성을 설정 합니다.
- 요청에서 `HttpWebResponse`를 검색 하는 중입니다.
- 응답에서 데이터를 읽는 중입니다.

예를 들어 다음 코드는 의약품 웹 서비스의 미국 국가 라이브러리에서 데이터를 검색 합니다.

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"https://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
request.ContentType = "application/json";
request.Method = "GET";

using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
  if (response.StatusCode != HttpStatusCode.OK)
     Console.Out.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
  using (StreamReader reader = new StreamReader(response.GetResponseStream()))
  {
               var content = reader.ReadToEnd();
               if(string.IsNullOrWhiteSpace(content)) {
                       Console.Out.WriteLine("Response contained empty body...");
               }
               else {
                       Console.Out.WriteLine("Response Body: \r\n {0}", content);
               }

               Assert.NotNull(content);
  }
}
```

위의 예에서는 JSON으로 형식이 지정 된 데이터를 반환 하는 `HttpWebRequest`을 만듭니다. 데이터는 데이터를 읽기 위해 `StreamReader` 가져올 수 있는 `HttpWebResponse`으로 반환 됩니다.

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

REST 서비스를 사용 하는 또 다른 방법은 [RestSharp](http://restsharp.org/) 라이브러리를 사용 하는 것입니다. RestSharp는 원시 문자열 콘텐츠 또는 deserialize C# 된 개체로 결과를 검색 하는 지원을 포함 하 여 HTTP 요청을 캡슐화 합니다. 예를 들어 다음 코드는 의약품 웹 서비스의 미국 국가별 라이브러리를 요청 하 고 결과를 JSON 형식 문자열로 검색 합니다.

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm`은 `RestSharp.RestResponse.Content` 속성에서 원시 JSON 문자열을 사용 하 여 C# 개체로 변환 하는 메서드입니다. 웹 서비스에서 반환 된 데이터를 deserialize 하는 방법은이 문서의 뒷부분에 설명 되어 있습니다.

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

`HttpWebRequest`와 같이 Mono BCL (기본 클래스 라이브러리)에서 사용할 수 있는 클래스 외에도 RestSharp 등의 타사 C# 라이브러리를 사용 하 여 웹 서비스를 사용할 수도 있습니다. 예를 들어 iOS에서는 `NSUrlConnection` 및 `NSMutableUrlRequest` 클래스를 사용할 수 있습니다.

다음 코드 예제에서는 iOS 클래스를 사용 하 여 의약품 웹 서비스의 미국 국가 라이브러리를 호출 하는 방법을 보여 줍니다.

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("https://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
       NSUrlRequestCachePolicy.ReloadRevalidatingCacheData, 20);
request["Accept"] = "application/json";

var connectionDelegate = new RxTermNSURLConnectionDelegate();
var connection = new NSUrlConnection(request, connectionDelegate);
connection.Start();

public class RxTermNSURLConnectionDelegate : NSUrlConnectionDelegate
{
       StringBuilder _ResponseBuilder;
       public bool IsFinishedLoading { get; set; }
       public string ResponseContent { get; set; }

       public RxTermNSURLConnectionDelegate()
               : base()
       {
               _ResponseBuilder = new StringBuilder();
       }

       public override void ReceivedData(NSUrlConnection connection, NSData data)
       {
               if(data != null) {
                       _ResponseBuilder.Append(data.ToString());
               }
       }
       public override void FinishedLoading(NSUrlConnection connection)
       {
               IsFinishedLoading = true;
               ResponseContent = _ResponseBuilder.ToString();
       }
}
```

일반적으로 웹 서비스를 사용 하는 플랫폼별 클래스는 네이티브 코드가로 C#이식 되는 시나리오로 제한 되어야 합니다. 가능 하면 웹 서비스 액세스 코드는 플랫폼 간 공유를 위해 이식할 수 있어야 합니다.

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

웹 서비스를 호출 하는 또 다른 옵션은 [서비스 스택](https://www.servicestack.net/) 라이브러리입니다. 예를 들어 다음 코드에서는 서비스 스택의 `IServiceClient.GetAsync` 메서드를 사용 하 여 서비스 요청을 실행 하는 방법을 보여 줍니다.

```csharp
client.GetAsync<CustomersResponse>("",
          (response) => {
               foreach(var c in response.Customers) {
                       Console.WriteLine(c.CompanyName);
               }
       },
       (response, ex) => {
               Console.WriteLine(ex.Message);
       });
```

> [!IMPORTANT]
> ServiceStack 및 RestSharp와 같은 도구를 사용 하 여 REST 서비스를 쉽게 호출 하 고 사용할 수 있지만 표준 _DataContract_ serialization 규칙을 따르지 않는 XML 또는 JSON을 사용 하는 것은 어려울 수 있습니다. 필요한 경우 요청을 호출 하 고 아래에서 설명 하는 ServiceStack. 텍스트 라이브러리를 사용 하 여 적절 한 serialization을 명시적으로 처리 합니다.

<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>RESTful 데이터 소비

RESTful 웹 서비스는 일반적으로 JSON 메시지를 사용 하 여 클라이언트에 데이터를 반환 합니다. JSON은 압축 페이로드를 생성 하는 텍스트 기반 데이터 교환 형식으로, 데이터를 보낼 때 대역폭 요구 사항이 감소 합니다. 이 섹션에서는 JSON 및 POX (RESTful)에서의 응답을 사용 하는 메커니즘을 검사 합니다.

<a name="Using_System.JSON" />

### <a name="systemjson"></a>시스템 JSON

Xamarin 플랫폼은 기본 제공 되는 JSON에 대 한 지원을 제공 합니다. `JsonObject`를 사용 하 여 다음 코드 예제와 같이 결과를 검색할 수 있습니다.

```csharp
var obj = JsonObject.Parse(json);
var properties = obj["rxtermsProperties"];
term.BrandName = properties["brandName"];
term.DisplayName = properties["displayName"];
term.Synonym = properties["synonym"];
term.FullName = properties["fullName"];
term.FullGenericName = properties["fullGenericName"];
term.Strength = properties["strength"];
```

그러나 `System.Json` 도구는 전체 데이터를 메모리에 로드 하는 것을 알고 있어야 합니다.

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

[Newtonsoft.json JSON.NET 라이브러리](https://www.newtonsoft.com/json) 는 JSON 메시지를 serialize 및 deserialize 하는 데 널리 사용 되는 라이브러리입니다. 다음 코드 예제에서는 JSON.NET를 사용 하 여 JSON 메시지를 C# 개체로 deserialize 하는 방법을 보여 줍니다.

```csharp
var term = new RxTerm();
var properties = JObject.Parse(json)["rxtermsProperties"];
term.BrandName = properties["brandName"].Value<string>();
term.DisplayName = properties["displayName"].Value<string>();
term.Synonym = properties["synonym"].Value<string>();;
term.FullName = properties["fullName"].Value<string>();;
term.FullGenericName = properties["fullGenericName"].Value<string>();;
term.Strength = properties["strength"].Value<string>();
term.RxCUI = properties["rxcui"].Value<string>();
```

<a name="Using_ServiceStack.Text" />

### <a name="servicestacktext"></a>ServiceStack. 텍스트

ServiceStack. Text는 ServiceStack 라이브러리와 함께 작동 하도록 설계 된 JSON serialization 라이브러리입니다. 다음 코드 예제에서는 `ServiceStack.Text.JsonObject`를 사용 하 여 JSON을 구문 분석 하는 방법을 보여 줍니다.

```csharp
var result = JsonObject.Parse(json).Object("rxtermsProperties")
       .ConvertTo(x => new RxTerm {
               BrandName = x.Get("brandName"),
               DisplayName = x.Get("displayName"),
               Synonym = x.Get("synonym"),
               FullName = x.Get("fullName"),
               FullGenericName = x.Get("fullGenericName"),
               Strength = x.Get("strength"),
               RxTermDoseForm = x.Get("rxtermsDoseForm"),
               Route = x.Get("route"),
               RxCUI = x.Get("rxcui"),
               RxNormDoseForm = x.Get("rxnormDoseForm"),
       });
```

<a name="Using_System.Xml.Linq" />

### <a name="systemxmllinq"></a>System.Xml.Linq

XML 기반 REST 웹 서비스를 사용 하는 경우 다음 코드 예제에 나와 있는 것 처럼 LINQ to XML를 사용 하 여 XML C# 을 구문 분석 하 고 개체를 인라인으로 채울 수 있습니다.

```csharp
var doc = XDocument.Parse(xml);
var result = doc.Root.Descendants("rxtermsProperties")
.Select(x=> new RxTerm()
       {
               BrandName = x.Element("brandName").Value,
               DisplayName = x.Element("displayName").Value,
               Synonym = x.Element("synonym").Value,
               FullName = x.Element("fullName").Value,
               FullGenericName = x.Element("fullGenericName").Value,
               //bind more here...
               RxCUI = x.Element("rxcui").Value,
       });
```

<a name="asmx" />

## <a name="aspnet-web-service-asmx"></a>ASMX (ASP.NET Web Service)

ASMX는 SOAP (Simple Object Access Protocol)를 사용 하 여 메시지를 보내는 웹 서비스를 빌드하는 기능을 제공 합니다. SOAP는 웹 서비스를 빌드하고 액세스 하기 위한 플랫폼 독립적이 고 언어에 독립적인 프로토콜입니다. ASMX 서비스 소비자는 서비스를 구현 하는 데 사용 되는 플랫폼, 개체 모델 또는 프로그래밍 언어에 대해 알 필요가 없습니다. SOAP 메시지를 송수신 하는 방법을 이해 해야 합니다.

SOAP 메시지는 다음 요소를 포함 하는 XML 문서입니다.

- XML 문서를 SOAP 메시지로 식별 하는 *Envelope* 이라는 루트 요소입니다.
- 인증 데이터와 같은 응용 프로그램 관련 정보를 포함 하는 선택적 *헤더* 요소입니다. *Header* 요소가 있으면 *Envelope* 요소의 첫 번째 자식 요소 여야 합니다.
- 받는 사람에 대해 의도 된 SOAP 메시지를 포함 하는 필수 *본문* 요소입니다.
- 오류 메시지를 나타내는 데 사용 되는 선택적 *오류* 요소입니다. *Fault* 요소가 있으면 *Body* 요소의 자식 요소 여야 합니다.

SOAP는 HTTP, SMTP, TCP 및 UDP를 비롯 한 여러 전송 프로토콜을 통해 작동할 수 있습니다. 그러나 ASMX 서비스는 HTTP를 통해서만 작동할 수 있습니다. Xamarin 플랫폼은 HTTP를 통한 표준 SOAP 1.1 구현을 지원 하며, 여기에는 많은 표준 ASMX 서비스 구성에 대 한 지원이 포함 됩니다.

### <a name="generating-a-proxy"></a>프록시 생성

응용 프로그램에서 서비스에 연결할 수 있도록 하는 ASMX 서비스를 사용 하려면 *프록시가* 생성 되어야 합니다. 프록시는 메서드 및 관련 서비스 구성을 정의 하는 서비스 메타 데이터를 사용 하 여 생성 됩니다. 이 메타 데이터는 웹 서비스에 의해 생성 된 WSDL (웹 서비스 기술 언어) 문서로 노출 됩니다. 프록시는 웹 서비스에 대 한 웹 참조를 플랫폼별 프로젝트에 추가 하기 위해 Mac용 Visual Studio 또는 Visual Studio를 사용 하 여 빌드됩니다.

웹 서비스 URL은 `file:///` 경로 접두사를 통해 액세스할 수 있는 호스트 된 원격 원본 또는 로컬 파일 시스템 리소스 일 수 있습니다. 예를 들면 다음과 같습니다.

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "The web service URL can either be a hosted remote source or local file system resource accessible via the file path prefix")](images/add-webreference-dialog.png#lightbox)

이렇게 하면 프로젝트의 웹 또는 서비스 참조 폴더에 프록시가 생성 됩니다. 프록시는 생성 된 코드 이므로 수정 하면 안 됩니다.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>수동으로 프록시를 프로젝트에 추가

호환 되는 도구를 사용 하 여 생성 된 기존 프록시가 있는 경우이 출력은 프로젝트의 일부로 포함 될 때 사용 될 수 있습니다. Mac용 Visual Studio에서 **파일 추가** ...를 사용 하 여 프록시를 추가 하는 메뉴 옵션입니다. 또한이를 위해 **참조 추가 ...** *를 사용* 하 여 명시적으로 참조 해야 합니다. 대화.

### <a name="consuming-the-proxy"></a>프록시 사용

생성 된 프록시 클래스는 APM (비동기 프로그래밍 모델) 디자인 패턴을 사용 하는 웹 서비스를 사용 하는 메서드를 제공 합니다. 이 패턴에서 비동기 작업은 비동기 작업을 시작 하 고 종료 하는 *Beginoperationname* 및 *EndOperationName*라는 두 개의 메서드로 구현 됩니다.

*Beginoperationname* 메서드는 비동기 작업을 시작 하 고 `IAsyncResult` 인터페이스를 구현 하는 개체를 반환 합니다. *Beginoperationname*을 호출한 후에는 비동기 작업이 스레드 풀 스레드에서 수행 되는 동안 응용 프로그램에서 호출 스레드에 대 한 명령을 계속 실행할 수 있습니다.

*Beginoperationname*을 호출할 때마다 응용 프로그램은 *EndOperationName* 를 호출 하 여 작업의 결과를 가져와야 합니다. *EndOperationName* 의 반환 값은 동기 웹 서비스 메서드에서 반환 되는 형식과 동일 합니다. 다음 코드 예제는 이러한 예를 보여줍니다.

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

TPL (작업 병렬 라이브러리)은 동일한 `Task` 개체에 비동기 작업을 캡슐화 하 여 APM begin/end 메서드 쌍을 사용 하는 프로세스를 간소화할 수 있습니다. 이 캡슐화는 `Task.Factory.FromAsync` 메서드의 여러 오버 로드에서 제공 됩니다. 이 메서드는 `TodoService.BeginGetTodoItems` 메서드가 완료 된 후 `BeginGetTodoItems` 대리자에 전달 되는 데이터가 없음을 나타내는 `null` 매개 변수를 사용 하 여 `TodoService.EndGetTodoItems` 메서드를 실행 하는 `Task`을 만듭니다. 마지막으로 `TaskCreationOptions` 열거형의 값은 작업의 생성 및 실행에 대 한 기본 동작을 사용 하도록 지정 합니다.

APM에 대 한 자세한 내용은 MSDN의 [비동기 프로그래밍 모델](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) 및 [TPL 및 기존 .NET Framework 비동기 프로그래밍](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) 을 참조 하세요.

ASMX 서비스를 사용 하는 방법에 대 한 자세한 내용은 [asmx (ASP.NET Web service) 사용](~/xamarin-forms/data-cloud/web-services/asmx.md)을 참조 하세요.

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>WCF(Windows Communication Foundation)

WCF는 서비스 지향 응용 프로그램을 빌드하기 위한 Microsoft의 통합 프레임 워크입니다. 이를 통해 개발자는 안전 하 고 신뢰할 수 있으며 트랜잭션 된 분산 응용 프로그램을 빌드할 수 있습니다.

WCF는 다음과 같은 다양 한 계약을 포함 하는 서비스를 설명 합니다.

- **데이터 계약** – 메시지 내에서 콘텐츠의 기반을 형성 하는 데이터 구조를 정의 합니다.
- **메시지 계약** – 기존 데이터 계약에서 메시지를 작성 합니다.
- **오류 계약** – 사용자 지정 SOAP 오류를 지정할 수 있습니다.
- **서비스 계약** – 서비스에서 지 원하는 작업 및 각 작업과 상호 작용 하는 데 필요한 메시지를 지정 합니다. 또한 각 서비스에서 작업에 연결할 수 있는 사용자 지정 오류 동작도 지정 합니다.

ASP.NET 웹 서비스 (ASMX)와 WCF 간에는 차이점이 있지만, WCF는 ASMX에서 제공 하는 것과 동일한 기능 (HTTP를 통한 SOAP 메시지)을 지원 한다는 것을 이해 하는 것이 중요 합니다.

> [!IMPORTANT]
> WCF에 대 한 Xamarin 플랫폼 지원은 `BasicHttpBinding` 클래스를 사용 하 여 HTTP/HTTPS를 통해 텍스트로 인코딩된 SOAP 메시지로 제한 됩니다. 또한 WCF 지원에는 프록시를 생성 하는 Windows 환경 에서만 사용할 수 있는 도구를 사용 해야 합니다.

### <a name="generating-a-proxy"></a>프록시 생성

응용 프로그램에서 서비스에 연결할 수 있도록 하는 WCF 서비스를 사용 하려면 *프록시가* 생성 되어야 합니다. 프록시는 메서드 및 관련 서비스 구성을 정의 하는 서비스 메타 데이터를 사용 하 여 생성 됩니다. 이 메타 데이터는 웹 서비스에 의해 생성 된 WSDL (웹 서비스 기술 언어) 문서의 형식으로 노출 됩니다. Visual Studio 2017의 Microsoft WCF Web Service Reference Provider를 사용 하 여 프록시를 빌드하여 웹 서비스에 대 한 서비스 참조를 .NET Standard 라이브러리에 추가할 수 있습니다.

Visual Studio 2017에서 Microsoft WCF Web Service Reference Provider를 사용 하 여 프록시를 만드는 대안은 ServiceModel Metadata 유틸리티 도구 (svcutil.exe)를 사용 하는 것입니다. 자세한 내용은 [ServiceModel Metadata 유틸리티 도구 (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)를 참조 하세요.

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>프록시 구성

생성 된 프록시를 구성 하는 작업은 일반적으로 다음 예제와 같이 초기화 하는 동안 두 가지 구성 인수 (SOAP 1.1/ASMX 또는 WCF에 따라)를 사용 합니다 `EndpointAddress`.

```csharp
var binding = new BasicHttpBinding () {
       Name= "basicHttpBinding",
       MaxReceivedMessageSize = 67108864,
};

binding.ReaderQuotas = new System.Xml.XmlDictionaryReaderQuotas() {
       MaxArrayLength = 2147483646,
       MaxStringContentLength = 5242880,
};

var timeout = new TimeSpan(0,1,0);
binding.SendTimeout= timeout;
binding.OpenTimeout = timeout;
binding.ReceiveTimeout = timeout;

client = new Service1Client (binding, new EndpointAddress ("http://192.168.1.100/Service1.svc"));
```

바인딩은 응용 프로그램 및 서비스에서 서로 통신 하는 데 필요한 전송, 인코딩 및 프로토콜 세부 정보를 지정 하는 데 사용 됩니다. `BasicHttpBinding`는 HTTP 전송 프로토콜을 통해 텍스트 인코딩된 SOAP 메시지를 보내도록 지정 합니다. 끝점 주소를 지정 하면 게시 된 인스턴스가 여러 개 있는 경우 응용 프로그램에서 WCF 서비스의 다른 인스턴스에 연결할 수 있습니다.

### <a name="consuming-the-proxy"></a>프록시 사용

생성 된 프록시 클래스는 APM (비동기 프로그래밍 모델) 디자인 패턴을 사용 하는 웹 서비스를 사용 하는 메서드를 제공 합니다. 이 패턴에서 비동기 작업은 비동기 작업을 시작 하 고 종료 하는 *Beginoperationname* 및 *EndOperationName*라는 두 개의 메서드로 구현 됩니다.

*Beginoperationname* 메서드는 비동기 작업을 시작 하 고 `IAsyncResult` 인터페이스를 구현 하는 개체를 반환 합니다. *Beginoperationname*을 호출한 후에는 비동기 작업이 스레드 풀 스레드에서 수행 되는 동안 응용 프로그램에서 호출 스레드에 대 한 명령을 계속 실행할 수 있습니다.

*Beginoperationname*을 호출할 때마다 응용 프로그램은 *EndOperationName* 를 호출 하 여 작업의 결과를 가져와야 합니다. *EndOperationName* 의 반환 값은 동기 웹 서비스 메서드에서 반환 되는 형식과 동일 합니다. 다음 코드 예제는 이러한 예를 보여줍니다.

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

TPL (작업 병렬 라이브러리)은 동일한 `Task` 개체에 비동기 작업을 캡슐화 하 여 APM begin/end 메서드 쌍을 사용 하는 프로세스를 간소화할 수 있습니다. 이 캡슐화는 `Task.Factory.FromAsync` 메서드의 여러 오버 로드에서 제공 됩니다. 이 메서드는 `TodoServiceClient.BeginGetTodoItems` 메서드가 완료 된 후 `BeginGetTodoItems` 대리자에 전달 되는 데이터가 없음을 나타내는 `null` 매개 변수를 사용 하 여 `TodoServiceClient.EndGetTodoItems` 메서드를 실행 하는 `Task`을 만듭니다. 마지막으로 `TaskCreationOptions` 열거형의 값은 작업의 생성 및 실행에 대 한 기본 동작을 사용 하도록 지정 합니다.

APM에 대 한 자세한 내용은 MSDN의 [비동기 프로그래밍 모델](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) 및 [TPL 및 기존 .NET Framework 비동기 프로그래밍](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) 을 참조 하세요.

WCF 서비스를 사용 하는 방법에 대 한 자세한 내용은 [wcf (Windows Communication Foundation) 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/wcf.md)을 참조 하세요.

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>전송 보안 사용

WCF 서비스는 전송 수준 보안을 사용 하 여 메시지 가로채기를 방지할 수 있습니다. Xamarin 플랫폼은 SSL을 통해 전송 수준 보안을 사용 하는 바인딩을 지원 합니다. 그러나 스택에서 인증서의 유효성을 검사 해야 하는 경우가 있을 수 있으며이로 인해 예기치 않은 동작이 발생할 수 있습니다. 다음 코드 예제에서 보여 주는 것 처럼 서비스를 호출 하기 전에 `ServerCertificateValidationCallback` 대리자를 등록 하 여 유효성 검사를 재정의할 수 있습니다.

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

이는 서버 쪽 인증서 유효성 검사를 무시 하 고 전송 암호화를 유지 합니다. 그러나이 방법은 인증서와 관련 된 신뢰 문제를 효과적으로 무시 하므로 적절 하지 않을 수 있습니다. 자세한 내용은 [mono-project.com](https://www.mono-project.com)에서 [신뢰할 수 있는 루트 Respectfully 사용](https://www.mono-project.com/UsingTrustedRootsRespectfully) 을 참조 하세요.

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>클라이언트 자격 증명 보안 사용

WCF 서비스를 사용 하려면 서비스 클라이언트가 자격 증명을 사용 하 여 인증 해야 할 수도 있습니다. Xamarin 플랫폼은 클라이언트가 SOAP 메시지 봉투 내에서 자격 증명을 보낼 수 있는 WS-SECURITY 프로토콜을 지원 하지 않습니다. 그러나 Xamarin 플랫폼은 적절 한 `ClientCredentialType`지정 하 여 HTTP 기본 인증 자격 증명을 서버에 전송 하는 기능을 지원 합니다.

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

그런 다음 기본 인증 자격 증명을 지정할 수 있습니다.

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

위의 예제에서 "trampolines가 0 형식으로 실행 되었습니다." 라는 메시지가 나타나면 빌드에 `–aot “trampolines={number of trampolines}”` 인수를 추가 하 여 trampolines 형식의 수를 늘릴 수 있습니다. 자세한 내용은 [문제 해결](~/ios/troubleshooting/troubleshooting.md#trampolines)을 참조하세요.

REST 웹 서비스의 컨텍스트에서 HTTP 기본 인증에 대 한 자세한 내용은 [RESTful 웹 서비스 인증](~/xamarin-forms/data-cloud/authentication/rest.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Xamarin.ios의 웹 서비스](~/xamarin-forms/data-cloud/index.yml)
- [ServiceModel Metadata 유틸리티 도구 (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](https://msdn.microsoft.com/library/system.servicemodel.basichttpbinding.aspx)
