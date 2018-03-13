---
title: "웹 서비스 소개"
description: "이 가이드에는 다른 웹 서비스 기술을 사용 하는 방법을 보여 줍니다. REST 서비스, SOAP 서비스 및 Windows Communication Foundation 서비스와 통신 포함 하는 주제를 다룹니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 48489ca7dc28dcc14a7810b15dc1ffa1fd4f7cf4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-web-services"></a>웹 서비스 소개

_이 가이드에는 다른 웹 서비스 기술을 사용 하는 방법을 보여 줍니다. REST 서비스, SOAP 서비스 및 Windows Communication Foundation 서비스와 통신 포함 하는 주제를 다룹니다._

많은 모바일 응용 프로그램에서 클라우드에 종속 되어 제대로 작동 하려면 및 되므로 모바일 응용 프로그램에 웹 서비스를 통합 하는 것은 일반적인 시나리오입니다. Xamarin 플랫폼 다른 웹 서비스 기술 소비를 지원 하며 RESTful, ASMX, 및 Windows Communication Foundation (WCF) 서비스를 사용 하기 위한 기본 제공 및 타사 지원이 포함 됩니다.

이 문서에서는 다음 내용을 다룹니다.

- [REST 서비스](#rest)
- [ASP.Net 웹 서비스 (ASMX)](#asmx)
- [WCF Services](#wcf)

Xamarin.Forms를 사용 하 여 고객에 대 한 전체 예는 각에 이러한 기술을 사용 하 여 [Xamarin.Forms 웹 서비스](~/xamarin-forms/data-cloud/index.md) 설명서입니다.

> [!IMPORTANT]
> **Xamarin.iOS에 대 한 참고:** 9 Ios에서는 앱 전송 보안 (AT) 적용 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간에 보안 연결 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 합니다. IOS 9에 대해 빌드된 앱에서 기본적으로 AT를 사용 하므로 모든 연결 AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.


있습니다 수 옵트아웃 AT의 사용할 수 없는 경우는 `HTTPS` 및 프로토콜을 인터넷 리소스에 대 한 통신을 보호 합니다. 응용 프로그램의 업데이트 하 여 이렇게 할 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.



<a name="rest" />

## <a name="rest"></a>REST

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

모바일 응용 프로그램에서 웹 서비스에 액세스 하기 위한 기본 방법 확인 REST은 단순 하기 때문에 작업 수 있습니다.

## <a name="consuming-rest-services"></a>REST 서비스 사용

라이브러리 및 REST 서비스를 사용 하는 데 사용할 수 있는 클래스는 여러 가지가 및 다음 하위 섹션에서 논의 합니다. REST 서비스를 사용 하는 방법에 대 한 자세한 내용은 참조 [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/consuming/rest.md)합니다.

### <a name="httpclient"></a>HttpClient

[Microsoft HTTP 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Net.Http) 제공는 `HttpClient` 보내고 HTTP를 통해 요청을 수신 하는 데 사용 되는 클래스입니다. HTTP 요청을 보내 및 URI로 식별 되는 리소스에서 HTTP 응답을 수신에 대 한 기능을 제공 합니다. 각 요청을 비동기 작업으로 전송 됩니다. 비동기 작업에 대 한 자세한 내용은 참조 [비동기 지원 개요](~/cross-platform/platform/async.md)합니다.

`HttpResponseMessage` 클래스는 HTTP 요청 수행 된 후 웹 서비스에서 받은 HTTP 응답 메시지를 나타냅니다. 상태 코드, 헤더 및 본문을 포함 하 여 응답에 대 한 정보를 포함 합니다. `HttpContent` 클래스를 나타내는 HTTP 본문 및 콘텐츠 헤더와 같은 `Content-Type` 및 `Content-Encoding`합니다. 사용 하 여 콘텐츠를 읽을 수는 `ReadAs` 메서드 같은 `ReadAsStringAsync` 및 `ReadAsByteArrayAsync`데이터의 형식에 따라 합니다.

에 대 한 자세한 내용은 `HttpClient` 클래스를 참조 하십시오. [HTTPClient 개체를 만드는](~/xamarin-forms/data-cloud/consuming/rest.md)합니다.

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

사용한 웹 서비스 호출 `HTTPWebRequest` 포함 됩니다.

-  특정 URI에 대 한 요청 인스턴스를 생성 합니다.
-  요청 인스턴스의 다양 한 HTTP 속성을 설정 합니다.
-  검색 한 `HttpWebResponse` 요청에서 합니다.
-  응답에서 데이터를 읽는 중입니다.

다음 코드는 미국에서 데이터를 검색 하는 예를 들어 National 치료제 라이브러리 웹 서비스:

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
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

위의 예에서는 만듭니다는 `HttpWebRequest` JSON으로 서식이 지정 된 데이터를 반환 합니다. 데이터는 `HttpWebResponse`, 올는 `StreamReader` 데이터를 읽을 얻을 수 있습니다.

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

또 다른 방법은 REST 서비스를 사용 하는 데 사용 하 여 [RestSharp](http://restsharp.org/) 라이브러리입니다. RestSharp 원시 문자열 내용이 나 C# 개체로 deserialize 된 결과 검색 하는 것에 대 한 지원을 비롯 하 여 HTTP 요청을 캡슐화 합니다. 예를 들어 다음 코드를 요청 하면 미국 National 치료제 라이브러리 웹 서비스 및 검색 결과 JSON으로 문자열 서식이 지정 됩니다.

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` 원시 JSON 문자열을 수행 하는 방법은 `RestSharp.RestResponse.Content` 속성 및 C# 개체로 변환 합니다. 웹 서비스에서 반환 된 데이터를 역직렬화 하는 동안이 문서의 뒷부분에 설명 합니다.

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

모노 자료에서 사용할 수 있는 클래스 뿐 아니라 클래스 라이브러리 (BCL)와 같은 `HttpWebRequest`, 및 제 3 자 C# 라이브러리 RestSharp, 같은 플랫폼별 클래스는 또한 사용할 수 웹 서비스를 사용 하도록 합니다. 예를 들어 iOS의 경우에 `NSUrlConnection` 및 `NSMutableUrlRequest` 클래스를 사용할 수 있습니다.

다음 코드 예제에서는 미국을 호출 하는 방법을 보여 줍니다. IOS 클래스를 사용 하 여 national 치료제 라이브러리 웹 서비스:

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
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

일반적으로 웹 서비스를 사용 하기 위한 플랫폼 특정 클래스 C#을 네이티브 코드를 이식 되는 시나리오로 제한 해야 합니다. 가능한 경우, 플랫폼 간 공유 수 있도록 웹 서비스 액세스 코드 이식 가능 해야 합니다.

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

웹 서비스 호출에 대 한 또 다른 옵션은는 [서비스 스택을](http://www.servicestack.net/) 라이브러리입니다. 예를 들어 다음 코드를 보여 줍니다 서비스 스택을 사용 하는 방법을 `IServiceClient.GetAsync` 메서드 서비스 요청을 발급 합니다.

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
> **참고:** ServiceStack 및 RestSharp 쉽게 호출 하 고 소비와 같은 도구 REST 서비스를 것은 때로는 특수 XML 또는 JSON은 표준에 맞지 않는 사용할 _DataContract_ serialization 규칙입니다. 필요한 경우에 요청을 호출 하 고 아래에 설명 된 ServiceStack.Text 라이브러리를 사용 하 여 명시적으로 적절 한 serialization을 처리 합니다.


<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>RESTful 데이터 사용

RESTful 웹 서비스는 일반적으로 클라이언트에 데이터를 반환할 JSON 메시지를 사용 합니다. JSON은 텍스트 기반 데이터 교환 형식 생성 페이로드를 압축 하는 데이터를 보낼 때 감소 대역폭 요구 사항이 발생 합니다. 이 섹션에서는 RESTful 응답에 JSON과 일반 이전 XML (POX)를 사용 하기 위한 메커니즘 검사 됩니다.

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

Xamarin 플랫폼 기본 JSON에 대 한 지원이 포함 되어 있습니다. 사용 하 여 한 `JsonObject`, 다음 코드 예제와 같이 결과 검색할 수 있습니다.

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

그러나 되기 잘 알고 있어야 하는 `System.Json` 도구를 메모리에 전체 데이터를 로드 합니다.

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

[NewtonSoft JSON.NET 라이브러리](http://www.newtonsoft.com/json) 는 널리 사용 되는 라이브러리 직렬화 및 JSON 메시지를 역직렬화 합니다. 다음 코드 예제에서는 C# 개체를 JSON 메시지를 deserialize 하려고 JSON.NET을 사용 하는 방법을 보여 줍니다.

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

### <a name="servicestacktext"></a>ServiceStack.Text

ServiceStack.Text는 JSON serialization 라이브러리 ServiceStack 라이브러리를 사용 하도록 설계 되었습니다. 다음 코드 예제를 사용 하 여 JSON을 구문 분석 하는 방법을 보여 줍니다는 `ServiceStack.Text.JsonObject`:

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

다음 코드 예제에서와 같이 LINQ to XML XML 기반 REST 웹 서비스를 사용 하 고 발생할 경우 XML을 구문 분석 하는 C# 개체 인라인 채웁니다 사용 수 있습니다.:

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

## <a name="aspnet-web-service-asmx"></a>ASP.NET 웹 서비스 (ASMX)

ASMX 웹 서비스는 SOAP Simple Object Access Protocol ()를 사용 하 여 메시지를 보내는 작성 하는 기능을 제공 합니다. SOAP는 빌드하고 웹 서비스에 액세스 하기 위한 플랫폼 독립적, 언어 독립적인 프로토콜입니다. 소비자는 ASMX 서비스의 플랫폼, 개체 모델 또는 서비스를 구현 하는 데 사용 되는 프로그래밍 언어에 대 한 어떠한 정보도 알 필요가 없습니다. SOAP 메시지를 송수신 하는 방법을 이해 하려면 하기만 합니다.

SOAP 메시지는 다음과 같은 요소를 포함 하는 XML 문서:

- 명명 된 루트 요소가 *봉투 (envelope)* SOAP 메시지가 XML 문서를 식별 하는 합니다.
- 선택적 *헤더* 인증 데이터 등의 응용 프로그램 관련 정보를 포함 하는 요소입니다. 경우는 *헤더* 요소가 있는지의 첫 번째 자식 요소 여야 합니다는 *봉투 (envelope)* 요소입니다.
- 필수 *본문* 수신자를 위한 SOAP 메시지를 포함 하는 요소입니다.
- 선택적 *오류* 오류 메시지를 나타내는 데 사용 되는 요소입니다. 경우는 *오류* 요소가 있는지의 자식 요소 여야 합니다.는 *본문* 요소입니다.

HTTP, SMTP, TCP 및 UDP를 포함 하 여 여러 전송 프로토콜을 통해 SOAP 작동할 수 있습니다. 그러나 HTTP를 통해는 ASMX 서비스 에서만 작동할 수 있습니다. Xamarin 플랫폼은 HTTP를 통해 표준 SOAP 1.1 구현을 지원 하 고 다양 한 표준 ASMX 서비스 구성에 대 한 지원을 포함 합니다.

### <a name="generating-a-proxy"></a>프록시 생성

A *프록시* 사용 하면 응용 프로그램이 서비스에 연결 하는 ASMX 서비스를 생성 해야 합니다. 프록시는 메서드 및 연결 된 서비스 구성을 정의 하는 서비스 메타 데이터를 사용 하 여 구성 됩니다. 이 메타 데이터는 웹 서비스에 의해 생성 되는 WSDL 웹 서비스 설명 언어 () 문서로 표시 됩니다. 프록시는 플랫폼별 프로젝트에 웹 서비스에 대 한 웹 참조를 추가 하려면 Mac 용 Visual Studio 또는 Visual Studio를 사용 하 여 작성 됩니다.

웹 서비스 URL 호스팅된 원격 소스 또는 로컬 파일 시스템 리소스를 통해 액세스할 수는 `file:///` 예를 들어 경로 접두사:

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "웹 서비스 URL은 호스팅된 원격 소스 또는 파일 경로 접두사를 통해 액세스할 수 있는 로컬 파일 시스템 리소스 수 있습니다.")](images/add-webreference-dialog.png#lightbox)

이 프로젝트의 웹 사이트 또는 서비스 참조 폴더에 프록시를 생성합니다. 프록시 생성 코드를 수정 하지 않아야 합니다.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>프로젝트에 프록시를 수동으로 추가합니다.

호환 되는 도구를 사용 하 여 생성 된 기존 프록시를 사용 하도록 설정한 경우에 프로젝트의 일부분으로 포함 하는 경우이 출력 이용할 수 있습니다. Mac 용 Visual Studio에서 사용 하 여 **파일 추가 중...** 프록시 추가할 메뉴 옵션입니다. 또한 필요한 *System.Web.Services.dll* 를 사용 하 여 명시적으로 참조할 수는 **참조 추가...** 대화 상자입니다.

### <a name="consuming-the-proxy"></a>프록시 사용

생성 된 프록시 클래스는 비동기 프로그래밍 모델 (APM) 디자인 패턴을 사용 하는 웹 서비스를 사용 하기 위한 메서드를 제공 합니다. 이 패턴에서는 비동기 작업을 라는 두 가지 방법으로 구현 *BeginOperationName* 및 *EndOperationName*를 시작 하 고 비동기 작업을 종료 합니다.

*BeginOperationName* 메서드는 비동기 작업을 시작 하 고 구현 하는 개체를 반환 합니다.는 `IAsyncResult` 인터페이스입니다. 호출한 후 *BeginOperationName*, 응용 프로그램 수 동안 명령을 계속 실행할 때 호출 스레드에서 비동기 작업은 스레드 풀 스레드에서 수행 됩니다.

각 호출에 대 한 *BeginOperationName*, 응용 프로그램 호출 또한 해야 *EndOperationName* 작업의 결과를 가져옵니다. 반환 값 *EndOperationName* 동기 웹 서비스 메서드에서 반환 되는 동일한 형식입니다. 다음 코드 예제에서는 이러한 예제를 보여 줍니다.

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

TPL 작업 병렬 라이브러리 () 같은 비동기 작업을 캡슐화 하 여 한 APM begin/end 메서드 쌍을 사용 하는 프로세스를 간소화할 수 `Task` 개체입니다. 이 캡슐화는 여러 오버 로드에서 제공 되는 `Task.Factory.FromAsync` 메서드. 이 메서드를 만듭니다는 `Task` 를 실행 하는 `TodoService.EndGetTodoItems` 메서드를 한 번의 `TodoService.BeginGetTodoItems` 메서드가 완료 되 면와 `null` 나타내는 데이터가 없는에 전달 되는 매개 변수는 `BeginGetTodoItems` 위임 합니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 작업의 실행에 대 한 기본 동작을 사용 하도록 지정 합니다.

APM 하는 방법에 대 한 자세한 내용은 참조 [비동기 프로그래밍 모델](https://msdn.microsoft.com/en-us/library/ms228963(v=vs.110).aspx) 및 [TPL 및 일반적인.NET Framework 비동기 프로그래밍](https://msdn.microsoft.com/en-us/library/dd997423(v=vs.110).aspx) msdn 합니다.

ASMX 서비스를 사용 하는 방법에 대 한 자세한 내용은 참조 [ASP.NET 웹 서비스 (ASMX)를 사용해](~/xamarin-forms/data-cloud/consuming/asmx.md)합니다.

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>WCF(Windows Communication Foundation)

WCF는 서비스 지향 응용 프로그램을 빌드하기 위한 Microsoft의 통합된 프레임 워크입니다. 개발자가 보안, 신뢰할 수 있는, 트랜잭션 및 상호 운용할 수 있는 분산된 응용 프로그램을 빌드할 수 있습니다.

WCF 서비스를 다음을 포함 하는 서로 다른 계약의 다양 한을 설명 합니다.

- **데이터 계약** – 메시지 내에서 콘텐츠에 대 한 기초를 형성 하는 데이터 구조를 정의 합니다.
- **메시지 계약과** -기존 데이터 계약에서 메시지를 작성 합니다.
- **오류 계약** – 허용 사용자 지정 SOAP 오류를 지정 해야 합니다.
- **서비스 계약** – 서비스는 지원 되는 작업을 지정 하 고 각 작업와 상호 작용 하는 데 필요한 메시지입니다. 또한 각 서비스에 대 한 작업에 연결 될 수 있는 모든 사용자 지정 오류 동작을 지정 합니다.

ASP.NET 웹 서비스 (ASMX) 및 WCF에 대 한 차이점은 있지만 WCF와 동일한 기능을 ASMX 제공 – HTTP 통해 SOAP 메시지를 지원 하는지 이해 하는 중요 합니다.

일반적으로 Xamarin 플랫폼 Silverlight 런타임과 함께 제공 되는 WCF의 동일한 클라이언트 하위 집합을 지원 합니다. WCF의 가장 일반적인 인코딩 및 프로토콜 구현을 여기에-텍스트 인코딩 SOAP 메시지의 HTTP 통해 전송 프로토콜을 사용 하는 `BasicHttpBinding` 클래스. 또한 WCF 지원 하려면 프록시를 생성 하는 Windows 환경에만 사용할 수 있는 도구를 사용 합니다.

Xamarin 플랫폼을 사용 하 여 WCF를 사용 하는 방법에 대 한 자세한 내용은 웹으로 서비스는 `BasicHttpBinding` 클래스를 참조 하십시오. [WCF를 사용 하는 연습-](walkthrough-working-with-wcf.md)합니다.

### <a name="generating-a-proxy"></a>프록시 생성

A *프록시* 를 응용 프로그램이 서비스에 연결할 수 있는 WCF 서비스를 생성 해야 합니다. 프록시는 메서드 및 연결 된 서비스 구성을 정의 하는 서비스 메타 데이터를 사용 하 여 구성 됩니다. 이 메타 데이터는 웹 서비스에 의해 생성 되는 WSDL 웹 서비스 설명 언어 () 문서의 형태로 표시 됩니다. .NET 표준 라이브러리에는 웹 서비스에 대 한 서비스 참조를 추가 하려면 Visual Studio 2017에 Microsoft WCF 웹 서비스 참조 공급자를 사용 하 여 프록시를 빌드할 수 있습니다.

Microsoft WCF 웹 서비스 참조 공급자를 사용 하 여 Visual Studio 2017에 프록시를 만드는 대신 ServiceModel Metadata 유틸리티 도구 (svcutil.exe)를 사용 하는 것입니다. 자세한 내용은 참조 [ServiceModel Metadata 유틸리티 도구 (Svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)합니다.

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>프록시 구성

생성 된 프록시를 구성 합니다. 일반적으로 걸립니다 (SOAP 1.1/ASMX 또는 WCF)에 따라 두 개의 구성 인수 초기화 하는 동안:는 `EndpointAddress` 및/또는 아래 예에 나와 있는 것 처럼 관련된 바인딩 정보:

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

전송, 인코딩 및 응용 프로그램과 서비스에서 상호 통신에 필요한 프로토콜 정보를 지정 하는 바인딩은 사용 됩니다. `BasicHttpBinding` 지정 텍스트 인코딩된 SOAP 메시지를 HTTP 전송 프로토콜을 통해 전송 됩니다. 끝점 주소를 지정 하면 게시 된 인스턴스를 여러 개 있으면는 응용을 WCF 서비스의 다른 인스턴스에 연결할 수 있습니다.

### <a name="consuming-the-proxy"></a>프록시 사용

생성 된 프록시 클래스는 비동기 프로그래밍 모델 (APM) 디자인 패턴을 사용 하는 웹 서비스를 사용 하기 위한 메서드를 제공 합니다. 이 패턴에서는 비동기 작업을 라는 두 가지 방법으로 구현 *BeginOperationName* 및 *EndOperationName*를 시작 하 고 비동기 작업을 종료 합니다.

*BeginOperationName* 메서드는 비동기 작업을 시작 하 고 구현 하는 개체를 반환 합니다.는 `IAsyncResult` 인터페이스입니다. 호출한 후 *BeginOperationName*, 응용 프로그램 수 동안 명령을 계속 실행할 때 호출 스레드에서 비동기 작업은 스레드 풀 스레드에서 수행 됩니다.

각 호출에 대 한 *BeginOperationName*, 응용 프로그램 호출 또한 해야 *EndOperationName* 작업의 결과를 가져옵니다. 반환 값 *EndOperationName* 동기 웹 서비스 메서드에서 반환 되는 동일한 형식입니다. 다음 코드 예제에서는 이러한 예제를 보여 줍니다.

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

TPL 작업 병렬 라이브러리 () 같은 비동기 작업을 캡슐화 하 여 한 APM begin/end 메서드 쌍을 사용 하는 프로세스를 간소화할 수 `Task` 개체입니다. 이 캡슐화는 여러 오버 로드에서 제공 되는 `Task.Factory.FromAsync` 메서드. 이 메서드를 만듭니다는 `Task` 를 실행 하는 `TodoServiceClient.EndGetTodoItems` 메서드를 한 번의 `TodoServiceClient.BeginGetTodoItems` 메서드가 완료 되 면와 `null` 나타내는 데이터가 없는에 전달 되는 매개 변수는 `BeginGetTodoItems` 위임 합니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 작업의 실행에 대 한 기본 동작을 사용 하도록 지정 합니다.

APM 하는 방법에 대 한 자세한 내용은 참조 [비동기 프로그래밍 모델](https://msdn.microsoft.com/en-us/library/ms228963(v=vs.110).aspx) 및 [TPL 및 일반적인.NET Framework 비동기 프로그래밍](https://msdn.microsoft.com/en-us/library/dd997423(v=vs.110).aspx) msdn 합니다.

WCF 서비스를 사용 하는 방법에 대 한 자세한 내용은 참조 [Windows Communication Foundation (WCF) 웹 서비스 사용](~/xamarin-forms/data-cloud/consuming/wcf.md)합니다.

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>전송 보안을 사용 하 여

WCF 서비스 메시지 가로채기를 방지 하기 위해 전송 수준 보안을 사용할 수 있습니다. Xamarin 플랫폼 SSL을 사용 하 여 전송 수준 보안을 사용 하는 바인딩을 지원 합니다. 그러나 스택의 예기치 않은 동작이 발생 되는 인증서의 유효성을 검사 해야 할 수 있는 경우가 있을 수 있습니다. 유효성 검사를 등록 하 여 재정의할 수 있습니다는 `ServerCertificateValidationCallback` 대리자 다음 코드 예제에서와 같이 서비스를 호출 하기 전에:

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

서버 쪽 인증서의 유효성 검사를 무시 하는 동안 전송 암호화를 유지 합니다. 그러나이 방법은 효과적으로 인증서에 연결 된 신뢰 문제를 무시 하 고 적합할 수 있습니다. 자세한 내용은 참조 [를 사용 하 여 신뢰할 수 있는 루트 Respectfully](http://www.mono-project.com/UsingTrustedRootsRespectfully) 에 [모노 project.com](http://www.mono-project.com)합니다.

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>클라이언트 자격 증명 보안을 사용 하 여

WCF 서비스는 서비스 클라이언트가 자격 증명을 사용 하 여 인증 하도록 요구할 수도 있습니다. Xamarin 플랫폼 클라이언트가 SOAP 메시지 봉투 내의 자격 증명을 보낼 수 있도록 하는 Ws-security 프로토콜을 지원 하지 않습니다. 그러나 Xamarin 플랫폼에서는 적절 한을 지정 하 여 서버에 HTTP 기본 인증 자격 증명을 보낼 수 있는 기능을 지원 `ClientCredentialType`:

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

그런 다음 기본 인증 자격 증명을 지정할 수 있습니다.

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

위의 예에서 "형식의 trampolines 부족" 메시지 발생할 경우 있습니다 수를 늘릴 수 유형 0 trampolines의 추가 하 여는 `–aot “trampolines={number of trampolines}”` 빌드에 대 한 인수입니다. 자세한 내용은 참조 [문제 해결](~/ios/troubleshooting/troubleshooting.md#trampolines)합니다.

HTTP 기본 인증에 대 한 자세한 내용은 참조에 REST 웹 서비스의 컨텍스트 하지만에 [RESTful 웹 서비스를 인증](~/xamarin-forms/data-cloud/authentication/rest.md)합니다.

## <a name="summary"></a>요약

이 가이드에는 다른 웹 서비스 기술을 사용 하는 방법을 보여 줍니다. REST 서비스, SOAP 서비스 및 Windows Communication Foundation 서비스와 통신 포함 하는 주제를 다룹니다.

## <a name="related-links"></a>관련 링크

- [웹 서비스 예제](https://developer.xamarin.com/samples/mobile/WebServices/WebServiceSamples/)
- [Xamarin.Forms에 웹 서비스](~/xamarin-forms/data-cloud/index.md)
- [ServiceModel Metadata 유틸리티 도구 (svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](http://msdn.microsoft.com/en-us/library/system.servicemodel.basichttpbinding.aspx)
