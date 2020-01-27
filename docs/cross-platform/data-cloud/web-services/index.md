---
title: 웹 서비스 소개
description: 이 가이드에서는 다양 한 웹 서비스 기술을 사용 하는 방법을 보여 줍니다. 여기서 다루는 항목에는 REST 서비스와의 통신, SOAP 서비스 및 Windows Communication Foundation 서비스가 포함 됩니다.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: ebd7cad9ef33a44dbc7aa469bb4e866bdfea2e61
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724783"
---
# <a name="introduction-to-web-services"></a>웹 서비스 소개

_이 가이드에서는 다양 한 웹 서비스 기술을 사용 하는 방법을 보여 줍니다. 여기서 다루는 항목에는 REST 서비스와의 통신, SOAP 서비스 및 Windows Communication Foundation 서비스가 포함 됩니다._

제대로 작동 하려면 많은 모바일 응용 프로그램이 클라우드에 종속 되므로 웹 서비스를 모바일 응용 프로그램에 통합 하는 것이 일반적인 시나리오입니다. Xamarin 플랫폼은 다양 한 웹 서비스 기술 사용을 지원 하 고 RESTful, ASMX 및 Windows Communication Foundation (WCF) 서비스 사용을 위한 내장 및 타사 지원을 포함 합니다.

Xamarin.ios를 사용 하는 고객의 경우 [Xamarin.ios 웹 서비스](~/xamarin-forms/data-cloud/index.yml) 설명서에서 이러한 각 기술을 사용 하는 전체 예제가 있습니다.

> [!IMPORTANT]
> IOS 9에서 앱 전송 보안 (ATS)은 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결을 적용 하 여 중요 한 정보가 실수로 공개 되는 것을 방지 합니다.
> 由于默认情况下构建适用于 iOS 9 应用程序中启用了 ATS，所有连接都将遵循 ATS 安全要求。 如果连接不能满足这些要求，它们将失败并出现异常。

`HTTPS` 프로토콜을 사용 하 고 인터넷 리소스에 대 한 보안 통신을 할 수 없는 경우 ATS를 옵트아웃 (opt out) 할 수 있습니다. 这可以通过更新应用程序的实现**Info.plist**文件。 有关详细信息请参阅[应用程序传输安全](~/ios/app-fundamentals/ats.md)。

## <a name="rest"></a>REST

具象状态传输 (REST) 是用于构建 web 服务的体系结构样式。 REST 请求是通过发出 HTTP 使用 web 浏览器使用检索 web 页面，并将数据发送到服务器的同一 HTTP 谓词。 谓词是：

- **获取**– 此操作用于从 web 服务中检索数据。
- **开机自检**– 此操作用于 web 服务上创建新项的数据。
- **放置**– 此操作用于更新 web 服务上的数据的项。
- **修补程序**– 此操作用于通过一组指令描述有关应如何修改此项更新 web 服务上的数据的项。 在示例应用程序不使用此谓词。
- **删除**– 此操作用于删除 web 服务上的数据的项。

遵守 REST 的 Api 被称为 RESTful Api，并使用定义的 web 服务：

- 一个基 URI。
- HTTP 方法，如 GET、 POST、 PUT、 PATCH 或 DELETE。
- 数据，如 JavaScript 对象表示法 (JSON) 媒体类型。

REST 的简单性已帮助使其用于访问移动应用程序中的 web 服务的主要方法。

## <a name="consuming-rest-services"></a>REST 서비스 사용

REST 서비스를 사용 하는 데 사용할 수 있는 라이브러리와 클래스에는 여러 가지가 있으며 다음 하위 섹션에서 설명 합니다. REST 서비스를 사용 하는 방법에 대 한 자세한 내용은 [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/rest.md)을 참조 하세요.

### <a name="httpclient"></a>HttpClient

[MICROSOFT Http 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Net.Http) 는 http를 통해 요청을 보내고 받는 데 사용 되는 `HttpClient` 클래스를 제공 합니다. URI로 식별 된 리소스에서 http 요청을 보내고 HTTP 응답을 받기 위한 기능을 제공 합니다. 每个请求将作为异步操作发送。 有关异步操作的详细信息，请参阅[异步支持概述](~/cross-platform/platform/async.md)。

`HttpResponseMessage`类表示 HTTP 响应消息进行 HTTP 请求之后收到来自 web 服务。 상태 코드, 헤더 및 본문을 포함 하 여 응답에 대 한 정보를 포함 합니다. 합니다 `HttpContent` 클래스를 나타내는 HTTP 본문 및 콘텐츠 헤더와 같은 `Content-Type` 고 `Content-Encoding`입니다. 可以读取内容，使用任一`ReadAs`方法，如`ReadAsStringAsync`和`ReadAsByteArrayAsync`根据数据的格式。

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

웹 서비스를 호출 하는 또 다른 옵션은 [서비스 스택](https://servicestack.net) 라이브러리입니다. 예를 들어 다음 코드에서는 서비스 스택의 `IServiceClient.GetAsync` 메서드를 사용 하 여 서비스 요청을 실행 하는 방법을 보여 줍니다.

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

RESTful web 服务通常使用 JSON 消息来将数据返回到客户端。 JSON은 압축 페이로드를 생성 하는 텍스트 기반 데이터 교환 형식으로, 데이터를 보낼 때 대역폭 요구 사항이 감소 합니다. 이 섹션에서는 JSON 및 POX (RESTful)에서의 응답을 사용 하는 메커니즘을 검사 합니다.

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

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

### <a name="servicestacktext"></a>ServiceStack.Text

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

ASMX는 SOAP (Simple Object Access Protocol)를 사용 하 여 메시지를 보내는 웹 서비스를 빌드하는 기능을 제공 합니다. SOAP 是一种独立于平台的和独立于语言的构建和访问 web 服务协议。 一个 ASMX 服务的使用者不需要完全了解平台、 对象模型中或用于实现服务的编程语言。 它们只需了解如何发送和接收 SOAP 消息。

SOAP 消息是包含以下元素的 XML 文档：

- 名为的根元素*信封*，它标识为 SOAP 消息的 XML 文档。
- 一个可选*标头*包含特定于应用程序的信息，例如身份验证数据的元素。 如果*标头*存在元素，则它必须是第一个子元素*信封*元素。
- 所需*正文*包含适用于接收方的 SOAP 消息的元素。
- 一个可选*容错*元素，用于指示错误消息。 如果*容错*元素不存在，必须为的子元素*正文*元素。

SOAP 可以在许多传输协议，包括 HTTP、 SMTP、 TCP 和 UDP 上操作。 但是，一个 ASMX 服务可以仅通过 HTTP 操作。 Xamarin 平台支持标准 SOAP 1.1 实现通过 HTTP，并且这包括对很多标准的 ASMX 服务配置的支持。

### <a name="generating-a-proxy"></a>프록시 생성

응용 프로그램에서 서비스에 연결할 수 있도록 하는 ASMX 서비스를 사용 하려면 *프록시가* 생성 되어야 합니다. 代理是通过使用以定义的方法和关联的服务配置的服务元数据构造的。 이 메타 데이터는 웹 서비스에 의해 생성 된 WSDL (웹 서비스 기술 언어) 문서로 노출 됩니다. 프록시는 웹 서비스에 대 한 웹 참조를 플랫폼별 프로젝트에 추가 하기 위해 Mac용 Visual Studio 또는 Visual Studio를 사용 하 여 빌드됩니다.

웹 서비스 URL은 `file:///` 경로 접두사를 통해 액세스할 수 있는 호스트 된 원격 원본 또는 로컬 파일 시스템 리소스 일 수 있습니다. 예를 들면 다음과 같습니다.

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "The web service URL can either be a hosted remote source or local file system resource accessible via the file path prefix")](images/add-webreference-dialog.png#lightbox)

이렇게 하면 프로젝트의 웹 또는 서비스 참조 폴더에 프록시가 생성 됩니다. 프록시는 생성 된 코드 이므로 수정 하면 안 됩니다.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>수동으로 프록시를 프로젝트에 추가

호환 되는 도구를 사용 하 여 생성 된 기존 프록시가 있는 경우이 출력은 프로젝트의 일부로 포함 될 때 사용 될 수 있습니다. Mac용 Visual Studio에서 **파일 추가** ...를 사용 하 여 프록시를 추가 하는 메뉴 옵션입니다. 또한이를 위해 **참조 추가 ...** *를 사용* 하 여 명시적으로 참조 해야 합니다. 대화 상자에서 추가합니다.

### <a name="consuming-the-proxy"></a>프록시 사용

生成的代理类提供用于使用 web 服务使用异步编程模型 (APM) 设计模式的方法。 在此模式中异步操作实现这两个方法名为*BeginOperationName*并*EndOperationName*，其中开始和结束异步操作。

*BeginOperationName*方法开始异步操作并返回一个对象，实现`IAsyncResult`接口。 在调用*BeginOperationName*，应用程序可以继续在调用线程上执行指令，同时异步操作在线程池线程。

每次调用*BeginOperationName*，应用程序还应调用*EndOperationName*来获取该操作的结果。 返回值*EndOperationName*同步 web 服务方法返回的类型相同。 다음 코드 예제는 이러한 예를 보여줍니다.

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

任务并行库 (TPL) 可以简化使用 APM begin/end 方法对通过封装中相同的异步操作的过程`Task`对象。 这种封装提供的多个重载`Task.Factory.FromAsync`方法。 이 메서드는 `TodoService.BeginGetTodoItems` 메서드가 완료 된 후 `BeginGetTodoItems` 대리자에 전달 되는 데이터가 없음을 나타내는 `null` 매개 변수를 사용 하 여 `TodoService.EndGetTodoItems` 메서드를 실행 하는 `Task`을 만듭니다. 最后的值`TaskCreationOptions`枚举指定应使用的创建和执行任务的默认行为。

APM에 대 한 자세한 내용은 MSDN의 [비동기 프로그래밍 모델](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) 및 [TPL 및 기존 .NET Framework 비동기 프로그래밍](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) 을 참조 하세요.

ASMX 서비스를 사용 하는 방법에 대 한 자세한 내용은 [asmx (ASP.NET Web service) 사용](~/xamarin-forms/data-cloud/web-services/asmx.md)을 참조 하세요.

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>WCF(Windows Communication Foundation)

WCF는 서비스 지향 응용 프로그램을 빌드하기 위한 Microsoft의 통합 프레임 워크입니다. 它允许开发人员构建安全、 可靠、 事务处理，且可互操作分布式应用程序。

WCF 描述了与各种不同的约定，其中包括以下服务：

- **数据协定**– 定义构成对其中一条消息的内容的基础的数据结构。
- **消息协定搭配**– 撰写邮件从现有数据协定。
- **错误协定**– 允许指定的自定义 SOAP 错误。
- **服务协定**– 指定服务支持的操作，这些消息所需的与每个操作进行交互。 它们还指定可以与每个服务上的操作相关联的任何自定义错误行为。

ASP.NET Web 服务 (ASMX) 和 WCF 之间的差异，但务必要了解 WCF 支持的相同功能提供的 ASMX 服务 – 通过 HTTP 的 SOAP 消息。

> [!IMPORTANT]
> WCF에 대 한 Xamarin 플랫폼 지원은 `BasicHttpBinding` 클래스를 사용 하 여 HTTP/HTTPS를 통해 텍스트로 인코딩된 SOAP 메시지로 제한 됩니다. 此外，WCF 支持需要使用工具仅在 Windows 环境以生成代理中可用。

### <a name="generating-a-proxy"></a>프록시 생성

一个*代理*必须生成以使用 WCF 服务，允许应用程序连接到服务。 代理是通过使用以定义的方法和关联的服务配置的服务元数据构造的。 此元数据中生成的 web 服务的 Web 服务描述语言 (WSDL) 文档的形式公开。 Visual Studio 2017의 Microsoft WCF Web Service Reference Provider를 사용 하 여 프록시를 빌드하여 웹 서비스에 대 한 서비스 참조를 .NET Standard 라이브러리에 추가할 수 있습니다.

创建的代理帐户在 Visual Studio 2017 中使用 Microsoft WCF Web Service Reference Provider 的替代方法是使用 ServiceModel Metadata Utility Tool (svcutil.exe)。 有关详细信息，请参阅[ServiceModel Metadata Utility Tool (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)。

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

使用绑定指定传输、 编码和协议详细信息所需的应用程序和服务彼此通信。 `BasicHttpBinding`指定将通过 HTTP 传输协议发送文本编码的 SOAP 消息。 指定终结点地址可以连接到 WCF 服务的不同实例应用程序，前提是有多个已发布的实例。

### <a name="consuming-the-proxy"></a>프록시 사용

生成的代理类提供用于使用 web 服务使用异步编程模型 (APM) 设计模式的方法。 在此模式下，异步操作实现这两个方法名为*BeginOperationName*并*EndOperationName*，其中开始和结束异步操作。

*BeginOperationName*方法开始异步操作并返回一个对象，实现`IAsyncResult`接口。 在调用*BeginOperationName*，应用程序可以继续在调用线程上执行指令，同时异步操作在线程池线程。

每次调用*BeginOperationName*，应用程序还应调用*EndOperationName*来获取该操作的结果。 返回值*EndOperationName*同步 web 服务方法返回的类型相同。 다음 코드 예제는 이러한 예를 보여줍니다.

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

任务并行库 (TPL) 可以简化使用 APM begin/end 方法对通过封装中相同的异步操作的过程`Task`对象。 这种封装提供的多个重载`Task.Factory.FromAsync`方法。 이 메서드는 `TodoServiceClient.BeginGetTodoItems` 메서드가 완료 된 후 `BeginGetTodoItems` 대리자에 전달 되는 데이터가 없음을 나타내는 `null` 매개 변수를 사용 하 여 `TodoServiceClient.EndGetTodoItems` 메서드를 실행 하는 `Task`을 만듭니다. 最后的值`TaskCreationOptions`枚举指定应使用的创建和执行任务的默认行为。

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
