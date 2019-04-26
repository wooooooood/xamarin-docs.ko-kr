---
title: 웹 서비스 소개
description: 이 가이드에는 다른 웹 서비스 기술을 사용 하는 방법을 보여 줍니다. 다룹니다 REST 서비스, SOAP 서비스 및 Windows Communication Foundation 서비스와 통신 합니다.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: afebe7f491855844e18bf054d665cf8d54e8f353
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61183897"
---
# <a name="introduction-to-web-services"></a>웹 서비스 소개

_이 가이드에는 다른 웹 서비스 기술을 사용 하는 방법을 보여 줍니다. 다룹니다 REST 서비스, SOAP 서비스 및 Windows Communication Foundation 서비스와 통신 합니다._

제대로 작동 하려면 많은 모바일 응용 프로그램 클라우드에 종속 되어 있으므로 모바일 응용 프로그램에 웹 서비스를 통합 하는 것은 일반적인 시나리오. Xamarin 플랫폼을 다른 웹 서비스 기술 사용을 지원 하며 RESTful, ASMX, 및 Windows Communication Foundation (WCF) 서비스를 사용 하기 위한 기본 제공 및 타사 지원 합니다.

Xamarin.Forms를 사용 하 여 고객에 게는 이러한 기술이 각각 사용 하 여 전체 예제는 [Xamarin.Forms 웹 서비스](~/xamarin-forms/data-cloud/index.md) 설명서.

> [!IMPORTANT]
> IOS 9 앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결 하므로 중요 한 정보가 실수로 유출 방지 적용 합니다.
> ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되므로 모든 연결이 ATS 보안 요구 사항이 적용 됩니다. 연결에서 이러한 요구를 충족 하지 않는, 예외와 함께 실패 합니다.

있습니다 수 옵트아웃 ATS 사용할 수 없는 경우는 `HTTPS` 프로토콜 및 인터넷 리소스에 대 한 통신을 보호 합니다. 이 앱을 업데이트 하 여 수행할 수 있습니다 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="rest"></a>REST

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

REST의 단순성 모바일 응용 프로그램에서 웹 서비스에 액세스 하기 위한 기본 방법은 있도록 비디오나 합니다.

## <a name="consuming-rest-services"></a>REST 서비스를 사용합니다.

다양 한 라이브러리 및 REST 서비스를 사용 하는 데 사용할 수 있는 클래스 및 다음 하위 섹션에 설명 합니다. REST 서비스를 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/consuming/rest.md)합니다.

### <a name="httpclient"></a>HttpClient

[Microsoft HTTP 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Net.Http) 제공 된 `HttpClient` 보내고 HTTP를 통해 요청을 수신 하는 데 사용 되는 클래스입니다. 보내는 HTTP 요청 및 URI로 식별 되는 리소스에서 받는 HTTP 응답에 대 한 기능을 제공 합니다. 각 요청을 비동기 작업으로 전송 됩니다. 비동기 작업에 대 한 자세한 내용은 참조 하세요. [비동기 지원 개요](~/cross-platform/platform/async.md)합니다.

`HttpResponseMessage` 클래스는 HTTP 요청을 생성 된 후 웹 서비스에서 받은 HTTP 응답 메시지를 나타냅니다. 상태 코드, 헤더 및 본문을 포함 하 여 응답에 대 한 정보를 포함 합니다. 합니다 `HttpContent` 클래스를 나타내는 HTTP 본문 및 콘텐츠 헤더와 같은 `Content-Type` 고 `Content-Encoding`입니다. 사용 하 여 콘텐츠를 읽을 수 있습니다 합니다 `ReadAs` 메서드를 같은 `ReadAsStringAsync` 및 `ReadAsByteArrayAsync`데이터의 형식에 따라 합니다.

에 대 한 자세한 내용은 합니다 `HttpClient` 클래스를 참조 하십시오 [HTTPClient 개체를 만드는](~/xamarin-forms/data-cloud/consuming/rest.md)합니다.

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

사용 하 여 웹 서비스 호출 `HTTPWebRequest` 포함 됩니다.

-  특정 URI에 대 한 요청 인스턴스를 만드는 중입니다.
-  요청 인스턴스의 다양 한 HTTP 속성을 설정 합니다.
-  검색을 `HttpWebResponse` 요청에서 합니다.
-  응답에서 데이터를 읽는 중입니다.

다음 코드는 미국에서 데이터를 검색 하는 예를 들어, National 의학 라이브러리의 웹 서비스:

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

위의 예제에서는 만듭니다는 `HttpWebRequest` JSON으로 서식이 지정 된 데이터가 반환 됩니다. 데이터가 반환 되는 `HttpWebResponse`, 올를 `StreamReader` 읽을 데이터를 가져올 수 있습니다.

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

REST 서비스를 사용 하는 다른 방법은 사용 하 여 [RestSharp](http://restsharp.org/) 라이브러리입니다. RestSharp 캡슐화 HTTP 요청을 원시 문자열 콘텐츠 또는 deserialize 된 결과 검색 하는 것에 대 한 지원을 비롯 하 여 C# 개체입니다. 예를 들어, 다음 코드를 요청 하는 미국 National 의학 라이브러리의 웹 서비스 및 검색 결과 JSON 형식 문자열.

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` 원시 JSON 문자열을 사용 하는 메서드입니다를 `RestSharp.RestResponse.Content` 속성으로 변환 하 고는 C# 개체입니다. 웹 서비스에서 반환 된 데이터를 역직렬화 하는 동안이 문서의 뒷부분에 설명 되어 있습니다.

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

Mono 자료에서 사용할 수 있는 클래스 외에도 클래스 라이브러리 (BCL)와 같은 `HttpWebRequest`, 및 타사 C# 라이브러리 RestSharp와 같은 플랫폼 관련 클래스도 사용할 수 있습니다 웹 서비스를 사용 합니다. 예를 들어 iOS에에서는 `NSUrlConnection` 및 `NSMutableUrlRequest` 클래스를 사용할 수 있습니다.

다음 코드 예제에서는 미국을 호출 하는 방법을 보여 줍니다. National 의학 라이브러리의 웹 서비스를 iOS 클래스를 사용 하 여:

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

일반적으로 웹 서비스 사용에 대 한 플랫폼 관련 클래스를 네이티브 코드를 이식 되는 시나리오를 제한 해야 C#입니다. 가능한 경우 공유 플랫폼 간 될 수 있도록 웹 서비스 액세스 코드 이식 가능 해야 합니다.

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

웹 서비스를 호출 하는 또 다른 옵션은는 [서비스 스택을](http://www.servicestack.net/) 라이브러리입니다. 예를 들어, 다음 코드에서는 서비스 스택 `IServiceClient.GetAsync` 서비스 요청을 실행 하는 방법.

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
> XML 또는 JSON 표준에 맞지 않는 사용할 간단한 경우가 ServiceStack 및 RestSharp 쉽게를 호출 하 여 소비와 같은 도구 REST 서비스를 하는 동안 _DataContract_ serialization 규칙입니다. 필요한 경우 요청을 호출 하 고 아래 설명 된 ServiceStack.Text 라이브러리를 사용 하 여 명시적으로 적절 한 serialization을 처리 합니다.


<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>RESTful 데이터 사용

RESTful 웹 서비스는 일반적으로 클라이언트에 데이터를 반환할 JSON 메시지를 사용 합니다. JSON은 텍스트 기반 데이터 교환 형식 생성 페이로드를 압축 하는 데이터를 보낼 때 감소 대역폭 요구 사항이 발생 합니다. 이 섹션에서는 RESTful 응답 JSON 및 일반 이전 XML (POX)를 사용 하기 위한 메커니즘 검사 됩니다.

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

Xamarin 플랫폼을 기본적으로 JSON에 대 한 지원을 제공합니다. 사용 하 여를 `JsonObject`, 다음 코드 예제와 같이 결과 검색할 수 있습니다.

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

그러나 것을 알아야 하는 `System.Json` 도구는 전체 데이터를 메모리로 로드 합니다.

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

합니다 [NewtonSoft JSON.NET 라이브러리](http://www.newtonsoft.com/json) 직렬화 및 역직렬화 JSON 메시지에 대 한 널리 사용 되는 라이브러리입니다. 다음 코드 예제에는 JSON 메시지를 deserialize 하는 데 JSON.NET을 사용 하는 방법을 보여 줍니다는 C# 개체.

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

ServiceStack.Text는 JSON 직렬화 라이브러리 ServiceStack 라이브러리와 함께 작동 하도록 설계 되었습니다. 다음 코드 예제를 사용 하 여 JSON을 구문 분석 하는 방법을 보여 줍니다는 `ServiceStack.Text.JsonObject`:

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

XML 기반 REST 웹 서비스를 사용 하는 경우 LINQ to XML 수 XML을 구문 분석 하 고 채우는 C# 다음 코드 예제에서 설명한 것 처럼 인라인 개체:

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

ASMX는 SOAP Simple Object Access Protocol ()를 사용 하 여 메시지를 전송 하는 웹 서비스를 구축 하는 기능을 제공 합니다. SOAP는 빌드하고 웹 서비스에 액세스 하기 위한 플랫폼 독립적인 및 언어 독립적 프로토콜입니다. ASMX 서비스의 소비자는 플랫폼, 개체 모델 또는 서비스를 구현 하는 데 사용 되는 프로그래밍 언어에 대 한 알 필요가 없습니다. 만 SOAP 메시지를 수신 하는 방법을 이해 해야 합니다.

SOAP 메시지는 다음 요소를 포함 하는 XML 문서:

- 라는 루트 요소가 *봉투 (envelope)* SOAP 메시지가 XML 문서를 식별 하는 합니다.
- 선택적인 *헤더* 인증 데이터와 같은 응용 프로그램 관련 정보를 포함 하는 요소입니다. 경우는 *머리글* 요소가의 첫 번째 자식 요소 여야 합니다 합니다 *봉투 (envelope)* 요소입니다.
- 필수 *본문* 수신자를 위한 SOAP 메시지를 포함 하는 요소입니다.
- 선택적인 *오류* 오류 메시지를 나타내는 데 사용 되는 요소입니다. 경우는 *오류* 요소가, 자식 요소 이어야 합니다는 *본문* 요소입니다.

SOAP는 HTTP, SMTP, TCP 및 UDP를 비롯 한 여러 전송 프로토콜을 통해 작동할 수 있습니다. 그러나 HTTP를 통한 ASMX 서비스만 작동할 수 있습니다. Xamarin 플랫폼은 HTTP를 통해 표준 SOAP 1.1 구현을 지원 하 고 다양 한 표준 ASMX 서비스 구성에 대 한 지원이 포함 됩니다.

### <a name="generating-a-proxy"></a>프록시 생성

A *프록시* 응용 프로그램 서비스에 연결을 허용 하는 ASMX 서비스를 사용 하 여 생성 해야 합니다. 프록시는 메서드 및 연결 된 서비스 구성을 정의 하는 서비스 메타 데이터를 사용 하 여 생성 됩니다. 이 메타 데이터는 웹 서비스에 의해 생성 되는 웹 서비스 설명 언어 (WSDL) 문서로 표시 됩니다. 프록시는 Mac 용 Visual Studio 또는 Visual Studio를 사용 하 여 플랫폼별 프로젝트에 웹 서비스에 대 한 웹 참조를 추가 하 여 작성 됩니다.

웹 서비스 URL을 호스 티 드 원격 소스 또는 로컬 파일 시스템 리소스를 통해 액세스할 수는 `file:///` 예를 들어 경로 접두사:

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "웹 서비스 URL을 호스 티 드 원격 소스 또는 파일 경로 접두사를 통해 액세스할 수 있는 로컬 파일 시스템 리소스 수 있습니다.")](images/add-webreference-dialog.png#lightbox)

이 프로젝트의 웹 사이트 또는 서비스 참조 폴더에 프록시를 생성합니다. 프록시 생성 코드를 수정 하지 않아야 합니다.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>프로젝트에 프록시를 수동으로 추가

기존 프록시의 호환 도구를 사용 하 여 생성 된 경우에 프로젝트의 일부분으로 포함 하는 경우이 출력 사용할 수 있습니다. Mac 용 Visual Studio에서 사용 된 **파일을 추가 하는 중...** 프록시에 추가할 메뉴 옵션입니다. 그러려면 뿐만 *System.Web.Services.dll* 를 사용 하 여 명시적으로 참조할 수는 **참조 추가...** 대화 상자입니다.

### <a name="consuming-the-proxy"></a>프록시를 사용합니다.

생성 된 프록시 클래스는 비동기 프로그래밍 모델 (APM) 디자인 패턴을 사용 하는 웹 서비스에 대 한 메서드를 제공 합니다. 이 패턴에서는 비동기 작업 이라는 두 가지 방법으로 구현 됩니다 *BeginOperationName* 하 고 *EndOperationName*를 시작 하며 비동기 작업을 종료 합니다.

합니다 *BeginOperationName* 메서드는 비동기 작업을 시작 하 고 구현 하는 개체를 반환 합니다.는 `IAsyncResult` 인터페이스입니다. 호출한 후 *BeginOperationName*, 응용 프로그램 수 동안 명령을 계속 실행할 때 호출 스레드에서 스레드 풀 스레드에서 비동기 작업을 수행 합니다.

호출할 때마다 *BeginOperationName*, 응용 프로그램도 호출 해야 *EndOperationName* 작업의 결과를 가져옵니다. 반환 값 *EndOperationName* 동기 웹 서비스 메서드의 반환 형식과 동일 합니다. 다음 코드 예제에서는이 예제를 보여 줍니다.

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

작업 병렬 라이브러리 (TPL)과 같은 비동기 작업을 캡슐화 하 여 APM begin/end 메서드 쌍을 사용 하는 프로세스를 간소화할 수 `Task` 개체입니다. 여러 오버 로드에서 제공 하는이 캡슐화는 `Task.Factory.FromAsync` 메서드. 이 메서드를 만듭니다를 `Task` 실행 하는 합니다 `TodoService.EndGetTodoItems` 메서드를 한 번를 `TodoService.BeginGetTodoItems` 메서드가 완료 되 면 사용 하 여를 `null` 데이터가 없는에 전달 되 고 있는지를 나타내는 매개 변수는 `BeginGetTodoItems` 대리자. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 태스크 실행에 대 한 기본 동작을 사용 해야 함을 지정 합니다.

APM에 대 한 자세한 내용은 참조 하세요. [비동기 프로그래밍 모델](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) 하 고 [TPL 및 일반적인.NET Framework 비동기 프로그래밍](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) MSDN에서.

ASMX 서비스를 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [ASP.NET 웹 서비스 (ASMX) 소비](~/xamarin-forms/data-cloud/consuming/asmx.md)합니다.

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>WCF(Windows Communication Foundation)

WCF는 서비스 지향 응용 프로그램을 빌드하기 위한 Microsoft의 통합 된 프레임 워크입니다. 개발자가 보안, 신뢰할 수 있는, 트랜잭션 및 상호 운용 가능한 분산된 응용 프로그램을 빌드할 수 있습니다.

WCF에서는 다양 한 다음을 포함 하는 서로 다른 계약을 사용 하 여 서비스를 설명 합니다.

- **데이터 계약** – 메시지 내에서 콘텐츠에 대 한 기반을 형성 하는 데이터 구조를 정의 합니다.
- **메시지 계약** -기존 데이터 계약에서 메시지를 작성 합니다.
- **오류 계약** – 사용자 지정 SOAP 오류를 지정할 수 있도록 합니다.
- **서비스 계약** – 서비스를 지 원하는 작업을 지정 하 고 각 작업을 사용 하 여 상호 작용 하는 데 필요한 메시지입니다. 이러한 각 서비스에 대 한 작업을 사용 하 여 연결할 수 있는 모든 사용자 지정 오류 동작을 지정 합니다.

ASP.NET 웹 서비스 (ASMX)와 WCF 사이의 차이가 이지만 WCF와 동일한 기능을 ASMX 제공 – HTTP 통한 SOAP 메시지를 지원 하는지 이해 해야 합니다.

> [!IMPORTANT]
> HTTP/HTTPS를 사용 하 여 WCF에 대 한 Xamarin 플랫폼 지원 스타일러스가 텍스트 인코딩된 SOAP 메시지의 제한 된 `BasicHttpBinding` 클래스입니다. 또한 WCF 지원만 프록시를 생성할 Windows 환경에서 사용할 수 있는 도구의 사용을 해야 합니다.

### <a name="generating-a-proxy"></a>프록시 생성

A *프록시* 응용 프로그램 서비스에 연결을 허용 하는 WCF 서비스를 사용 하 여 생성 해야 합니다. 프록시는 메서드 및 연결 된 서비스 구성을 정의 하는 서비스 메타 데이터를 사용 하 여 생성 됩니다. 이 메타 데이터는 웹 서비스에 의해 생성 되는 웹 서비스 설명 언어 (WSDL) 문서의 형태로 노출 됩니다. .NET Standard 라이브러리를 웹 서비스에 대 한 서비스 참조를 추가 하려면 Visual Studio 2017에서 Microsoft WCF Web Service Reference Provider 사용 하 여 프록시를 빌드할 수 있습니다.

Microsoft WCF Web Service Reference Provider 사용 하 여 Visual Studio 2017에서 프록시를 만드는 대신 ServiceModel Metadata 유틸리티 도구 (svcutil.exe)를 사용 하는 것입니다. 자세한 내용은 [ServiceModel Metadata 유틸리티 도구 (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)합니다.

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>프록시 구성

생성 된 프록시를 구성 합니다. 일반적으로 걸립니다 (SOAP 1.1/ASMX 또는 WCF)에 따라 두 개의 구성 인수 초기화 하는 동안:는 `EndpointAddress` 및/또는 연결 된 바인딩 정보를 아래 예와에서 같이:

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

전송, 인코딩 및 서로 통신 하는 응용 프로그램 및 서비스에 필요한 프로토콜 정보를 지정 하는 바인딩을 사용 됩니다. `BasicHttpBinding` 텍스트 인코딩된 SOAP 메시지는 HTTP 전송 프로토콜을 통해 전송 되도록 지정 합니다. 끝점 주소를 지정 하는 게시 된 인스턴스가 여러 개 있는 응용 프로그램을을 WCF 서비스의 다른 인스턴스에 연결할 수 있습니다.

### <a name="consuming-the-proxy"></a>프록시를 사용합니다.

생성 된 프록시 클래스의 비동기 프로그래밍 모델 (APM) 디자인 패턴을 사용 하는 웹 서비스 사용에 대 한 메서드를 제공 합니다. 이 패턴에서는 비동기 작업 이라는 두 가지 방법으로 구현 됩니다 *BeginOperationName* 하 고 *EndOperationName*, 시작 하며 비동기 작업을 종료 합니다.

합니다 *BeginOperationName* 메서드는 비동기 작업을 시작 하 고 구현 하는 개체를 반환 합니다.는 `IAsyncResult` 인터페이스입니다. 호출한 후 *BeginOperationName*, 응용 프로그램 수 동안 명령을 계속 실행할 때 호출 스레드에서 스레드 풀 스레드에서 비동기 작업을 수행 합니다.

호출할 때마다 *BeginOperationName*, 응용 프로그램도 호출 해야 *EndOperationName* 작업의 결과를 가져옵니다. 반환 값 *EndOperationName* 동기 웹 서비스 메서드의 반환 형식과 동일 합니다. 다음 코드 예제에서는이 예제를 보여 줍니다.

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

작업 병렬 라이브러리 (TPL)과 같은 비동기 작업을 캡슐화 하 여 APM begin/end 메서드 쌍을 사용 하는 프로세스를 간소화할 수 `Task` 개체입니다. 여러 오버 로드에서 제공 하는이 캡슐화는 `Task.Factory.FromAsync` 메서드. 이 메서드를 만듭니다를 `Task` 실행 하는 합니다 `TodoServiceClient.EndGetTodoItems` 메서드를 한 번를 `TodoServiceClient.BeginGetTodoItems` 메서드가 완료 되 면 사용 하 여를 `null` 데이터가 없는에 전달 되 고 있는지를 나타내는 매개 변수는 `BeginGetTodoItems` 대리자. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 태스크 실행에 대 한 기본 동작을 사용 해야 함을 지정 합니다.

APM에 대 한 자세한 내용은 참조 하세요. [비동기 프로그래밍 모델](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) 하 고 [TPL 및 일반적인.NET Framework 비동기 프로그래밍](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) MSDN에서.

WCF 서비스 사용에 대 한 자세한 내용은 참조 하세요. [Windows Communication Foundation (WCF) 웹 서비스 사용](~/xamarin-forms/data-cloud/consuming/wcf.md)합니다.

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>전송 보안을 사용 하 여

WCF 서비스 메시지 가로채기를 방지 하려면 전송 수준 보안을 사용할 수 있습니다. Xamarin 플랫폼에는 SSL을 사용 하 여 전송 수준 보안을 사용 하는 바인딩을 지원 합니다. 그러나 예기치 않은 동작이 발생 되는 인증서의 유효성을 검사 하는 스택을 해야 하는 경우가 있을 수 있습니다. 등록 하 여 유효성 검사를 재정의할 수 있습니다는 `ServerCertificateValidationCallback` 대리자 다음 코드 예제에서 설명한 것 처럼 서비스를 호출 하기 전에:

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

서버 쪽 인증서 유효성 검사를 무시 하는 동안 전송 암호화를 유지 합니다. 그러나이 방법은 효과적으로 인증서에 연결 된 신뢰 문제를 무시 하 고 적절 하지 않을 합니다. 자세한 내용은 참조 하세요. [를 사용 하 여 신뢰할 수 있는 루트 Respectfully](https://www.mono-project.com/UsingTrustedRootsRespectfully) 온 [mono project.com](https://www.mono-project.com)합니다.

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>클라이언트 자격 증명 보안을 사용 하 여

WCF 서비스는 서비스 클라이언트 자격 증명을 사용 하 여 인증을 요구할 수도 있습니다. Xamarin 플랫폼에는 클라이언트가 SOAP 메시지 봉투 안에서 자격 증명을 보낼 수 있도록 하는 Ws-security 프로토콜을 지원 하지 않습니다. 그러나 Xamarin 플랫폼을 적절 한을 지정 하 여 서버에 HTTP 기본 인증 자격 증명을 보내고 지 `ClientCredentialType`:

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

그런 다음 기본 인증 자격 증명을 지정할 수 있습니다.

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

위의 예제에서 "0 형식의 trampolines 부족" 메시지가 발생할 경우 늘릴 수 있습니다 trampolines 유형 0의 수를 추가 하 여는 `–aot “trampolines={number of trampolines}”` 빌드에 대 한 인수입니다. 자세한 내용은 [문제 해결](~/ios/troubleshooting/troubleshooting.md#trampolines)을 참조하세요.

HTTP 기본 인증에 대 한 자세한 내용은 참조에 있지만 컨텍스트에서 REST 웹 서비스의 [RESTful 웹 서비스를 인증](~/xamarin-forms/data-cloud/authentication/rest.md)합니다.

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms에서 웹 서비스](~/xamarin-forms/data-cloud/index.md)
- [ServiceModel Metadata 유틸리티 도구 (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](https://msdn.microsoft.com/library/system.servicemodel.basichttpbinding.aspx)
