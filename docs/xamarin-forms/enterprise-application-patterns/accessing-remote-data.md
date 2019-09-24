---
title: 원격 데이터에 액세스
description: 이 장에서는 eShopOnContainers mobile app이 컨테이너 화 된 마이크로 서비스에서 데이터에 액세스 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9c793f4d5f0cda5bff2dedef5e4e5e5bdfca69e5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770810"
---
# <a name="accessing-remote-data"></a>원격 데이터에 액세스

많은 최신 웹 기반 솔루션은 웹 서버에서 호스트 되는 웹 서비스를 사용 하 여 원격 클라이언트 응용 프로그램에 대 한 기능을 제공 합니다. 웹 서비스에서 노출 하는 작업은 web API를 구성 합니다.

클라이언트 앱은 API에서 노출 하는 데이터 또는 작업이 어떻게 구현 되는지 몰라도 웹 API를 활용할 수 있어야 합니다. 이렇게 하려면 API가 클라이언트 앱 및 웹 서비스에서 사용할 데이터 형식 및 클라이언트 앱과 웹 서비스 간에 교환 되는 데이터의 구조에 동의할 수 있도록 하는 일반적인 표준에 따라 유럽 연합 합니다.

## <a name="introduction-to-representational-state-transfer"></a>Representational State Transfer 소개

REST (Representational State Transfer)는 하이퍼 미디어를 기반으로 분산 시스템을 구축 하기 위한 아키텍처 스타일입니다. REST 모델의 주요 이점은 개방형 표준을 기반으로 하며, 모델 또는이를 액세스 하는 클라이언트 앱의 구현을 특정 구현에 바인딩하지 않는다는 것입니다. 따라서 Microsoft ASP.NET Core MVC를 사용 하 여 REST 웹 서비스를 구현할 수 있으며, 클라이언트 앱은 HTTP 요청을 생성 하 고 HTTP 응답을 구문 분석할 수 있는 모든 언어 및 도구 집합을 사용 하 여 개발할 수 있습니다.

REST 모델은 탐색 체계를 사용 하 여 네트워크를 통해 개체 및 서비스를 나타냅니다 (리소스 라고 함). REST를 구현 하는 시스템은 일반적으로 HTTP 프로토콜을 사용 하 여 이러한 리소스에 액세스 하기 위한 요청을 전송 합니다. 이러한 시스템에서 클라이언트 앱은 리소스를 식별 하는 URI 형식으로 요청을 제출 하 고 해당 리소스에 대해 수행할 작업을 나타내는 HTTP 메서드 (예: GET, POST, PUT 또는 DELETE)를 제출 합니다. HTTP 요청의 본문에는 작업을 수행 하는 데 필요한 데이터가 포함 되어 있습니다.

> [!NOTE]
> REST는 상태 비저장 요청 모델을 정의 합니다. 따라서 HTTP 요청은 독립적 이어야 하 고 순서에 관계 없이 발생할 수 있습니다.

REST 요청의 응답은 표준 HTTP 상태 코드를 사용 합니다. 예를 들어 유효한 데이터를 반환 하는 요청은 HTTP 응답 코드 200 (OK)를 포함 해야 하는 반면, 지정 된 리소스를 찾거나 삭제 하지 못한 요청은 HTTP 상태 코드 404 (찾을 수 없음)을 포함 하는 응답을 반환 해야 합니다.

RESTful web API는 연결 된 리소스 집합을 노출 하 고, 앱이 해당 리소스를 조작 하 고 이러한 리소스를 쉽게 탐색할 수 있도록 하는 핵심 작업을 제공 합니다. 이러한 이유로 일반적인 RESTful web API를 구성 하는 Uri는 노출 되는 데이터를 기반으로 하며 HTTP에서 제공 하는 기능을 사용 하 여이 데이터에 대해 작동 합니다.

HTTP 요청에 클라이언트 앱에 포함 된 데이터와 웹 서버의 해당 응답 메시지는 미디어 유형 이라고 하는 다양 한 형식으로 표시 될 수 있습니다. 클라이언트 앱이 메시지 본문에 데이터를 반환 하는 요청을 보내면 요청 `Accept` 헤더에서 처리할 수 있는 미디어 형식을 지정할 수 있습니다. 웹 서버에서이 미디어 유형을 지 원하는 경우 메시지 본문에 있는 데이터의 형식을 지정 하 `Content-Type` 는 헤더를 포함 하는 응답으로 회신할 수 있습니다. 클라이언트 앱이 응답 메시지를 구문 분석 하 고 결과를 적절 하 게 해석 해야 합니다.

REST에 대 한 자세한 내용은 [api 디자인](/azure/architecture/best-practices/api-design/) 및 [api 구현](/azure/architecture/best-practices/api-implementation/)을 참조 하세요.

## <a name="consuming-restful-apis"></a>RESTful Api 사용

EShopOnContainers 모바일 앱은 MVVM (모델-뷰-ViewModel) 패턴을 사용 하며, 패턴의 모델 요소는 앱에서 사용 되는 도메인 엔터티를 나타냅니다. EShopOnContainers reference 응용 프로그램의 컨트롤러 및 리포지토리 클래스는 이러한 여러 모델 개체를 허용 하 고 반환 합니다. 따라서 모바일 앱과 컨테이너 화 된 마이크로 서비스 간에 전달 되는 모든 데이터를 저장 하는 Dto (데이터 전송 개체)로 사용 됩니다. Dto를 사용 하 여 웹 서비스에 데이터를 전달 하 고 데이터를 수신 하는 경우의 주요 혜택은 단일 원격 호출에서 더 많은 데이터를 전송 하 여 앱이 수행 해야 하는 원격 호출의 수를 줄일 수 있다는 것입니다.

### <a name="making-web-requests"></a>웹 요청 수행

EShopOnContainers 모바일 앱은 `HttpClient` 클래스를 사용 하 여 HTTP를 통해 요청을 수행 하며, JSON은 미디어 유형으로 사용 됩니다. 이 클래스는 URI로 식별 된 리소스에서 HTTP 요청을 비동기적으로 보내고 HTTP 응답을 받기 위한 기능을 제공 합니다. 클래스 `HttpResponseMessage` 는 http 요청이 수행 된 후 REST API에서 받은 http 응답 메시지를 나타냅니다. 상태 코드, 헤더 및 모든 본문을 포함 하 여 응답에 대 한 정보를 포함 합니다. 합니다 `HttpContent` 클래스를 나타내는 HTTP 본문 및 콘텐츠 헤더와 같은 `Content-Type` 고 `Content-Encoding`입니다. 데이터의 형식에 따라 `ReadAs` `ReadAsStringAsync` 및 `ReadAsByteArrayAsync`와 같은 메서드를 사용 하 여 콘텐츠를 읽을 수 있습니다.

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>GET 요청 만들기

`CatalogService` 클래스는 카탈로그 마이크로 서비스에서 데이터 검색 프로세스를 관리 하는 데 사용 됩니다. 클래스의 메서드에서 클래스는 `CatalogService` autofac 종속성 주입 컨테이너를 사용 하 여 `ICatalogService` 형식에 대 한 형식 매핑으로 등록 됩니다. `RegisterDependencies` `ViewModelLocator` 그런 다음 `CatalogViewModel` 클래스의 인스턴스를 만들 때 해당 생성자는 autofac가 `ICatalogService` 확인 하 고 `CatalogService` 클래스의 인스턴스를 반환 하는 형식을 허용 합니다. 종속성 주입에 대 한 자세한 내용은 [종속성 주입 소개](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)를 참조 하세요.

그림 10-1에서는 `CatalogView`에서 표시 하기 위해 카탈로그 마이크로 서비스에서 카탈로그 데이터를 읽는 클래스의 상호 작용을 보여 줍니다.

[카탈로그 마이크로 서비스에서 데이터 검색 ![(accessing-remote-data-images/catalogdata.png " ")]] (accessing-remote-data-images/catalogdata-large.png#lightbox "카탈로그 마이크로 서비스에서 데이터 검색")

**그림 10-1**: 카탈로그 마이크로 서비스에서 데이터 검색

을 탐색 하면 `CatalogViewModel` 클래스의 메서드가호출됩니다.`OnInitialize` `CatalogView` 이 메서드는 다음 코드 예제에서 보여 주는 것 처럼 카탈로그 마이크로 서비스에서 카탈로그 데이터를 검색 합니다.

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

이 메서드는 autofac `GetCatalogAsync` 에 `CatalogService` `CatalogViewModel` 의해에 삽입 된 인스턴스의 메서드를 호출 합니다. 다음 코드 예제는 `GetCatalogAsync` 메서드를 보여줍니다.

```csharp
public async Task<ObservableCollection<CatalogItem>> GetCatalogAsync()  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.CatalogEndpoint);  
    builder.Path = "api/v1/catalog/items";  
    string uri = builder.ToString();  

    CatalogRoot catalog = await _requestProvider.GetAsync<CatalogRoot>(uri);  
    ...  
    return catalog?.Data.ToObservableCollection();            
}
```

이 메서드는 요청을 보낼 리소스를 식별 하는 URI를 빌드하고, `RequestProvider` 클래스를 사용 하 여 리소스에서 GET HTTP 메서드를 호출한 다음 결과를 `CatalogViewModel`로 반환 합니다. 클래스 `RequestProvider` 에는 리소스를 식별 하는 URI 형식, 해당 리소스에 대해 수행할 작업을 나타내는 HTTP 메서드 및 작업을 수행 하는 데 필요한 데이터를 포함 하는 본문으로 요청을 전송 하는 기능이 포함 되어 있습니다. `RequestProvider` 클래스를에 `CatalogService class`삽입 하는 방법에 대 한 자세한 내용은 [종속성 주입 소개](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)를 참조 하세요.

다음 코드 예제에서는 `GetAsync` `RequestProvider` 클래스의 메서드를 보여 줍니다.

```csharp
public async Task<TResult> GetAsync<TResult>(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    HttpResponseMessage response = await httpClient.GetAsync(uri);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>   
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

이 메서드는 `CreateHttpClient` 적절 한 헤더 집합을 사용 하 여 `HttpClient` 클래스의 인스턴스를 반환 하는 메서드를 호출 합니다. 그런 다음 URI로 식별 되는 리소스에 비동기 GET 요청을 전송 하 고 응답을 `HttpResponseMessage` 인스턴스에 저장 합니다. 그런 다음 응답이 성공 HTTP 상태 코드를 포함 하지 않는 경우 예외를 throw 하는 메서드를호출합니다.`HandleResponse` 그런 다음 응답을 문자열로 읽어 JSON `CatalogRoot` 에서 개체로 변환 하 고 `CatalogService`로 반환 합니다.

메서드 `CreateHttpClient` 는 다음 코드 예제에서 표시 됩니다.

```csharp
private HttpClient CreateHttpClient(string token = "")  
{  
    var httpClient = new HttpClient();  
    httpClient.DefaultRequestHeaders.Accept.Add(  
        new MediaTypeWithQualityHeaderValue("application/json"));  

    if (!string.IsNullOrEmpty(token))  
    {  
        httpClient.DefaultRequestHeaders.Authorization =   
            new AuthenticationHeaderValue("Bearer", token);  
    }  
    return httpClient;  
}
```

이 메서드는 `HttpClient` 클래스의 새 인스턴스를 `HttpClient` 만들고 인스턴스에서 만든 요청의 `Accept` 헤더를로 `application/json`설정 합니다 .이 경우 JSON을 사용 하 여 응답의 콘텐츠 형식을 지정 해야 함을 나타냅니다. 그런 다음 `CreateHttpClient` 액세스 토큰이 메서드에 인수로 전달 된 경우 `HttpClient` 인스턴스에 의해 생성 된 모든 요청의 `Authorization` 헤더에 추가 되 고 문자열 `Bearer`을 접두사로 추가 합니다. 권한 부여에 대 한 자세한 내용은 [권한 부여](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)를 참조 하세요.

클래스`RequestProvider`의 `HttpClient.GetAsync` `CatalogController` 메서드가를`Items` 호출 하면 다음 코드 예제에 표시 된 대로 Catalog. API 프로젝트의 클래스에 있는 메서드가 호출 됩니다. `GetAsync`

```csharp
[HttpGet]  
[Route("[action]")]  
public async Task<IActionResult> Items(  
    [FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)  
{  
    var totalItems = await _catalogContext.CatalogItems  
        .LongCountAsync();  

    var itemsOnPage = await _catalogContext.CatalogItems  
        .OrderBy(c=>c.Name)  
        .Skip(pageSize * pageIndex)  
        .Take(pageSize)  
        .ToListAsync();  

    itemsOnPage = ComposePicUri(itemsOnPage);  
    var model = new PaginatedItemsViewModel<CatalogItem>(  
        pageIndex, pageSize, totalItems, itemsOnPage);             

    return Ok(model);  
}
```

이 메서드는 entityframework를 사용 하 여 SQL 데이터베이스에서 카탈로그 데이터를 검색 하 고, 성공 HTTP 상태 코드 및 JSON 형식 `CatalogItem` 인스턴스의 컬렉션을 포함 하는 응답 메시지로 반환 합니다.

#### <a name="making-a-post-request"></a>POST 요청 만들기

`BasketService` 클래스는 바구니 마이크로 서비스를 사용 하 여 데이터 검색 및 업데이트 프로세스를 관리 하는 데 사용 됩니다. 클래스의 메서드에서 클래스는 `BasketService` autofac 종속성 주입 컨테이너를 사용 하 여 `IBasketService` 형식에 대 한 형식 매핑으로 등록 됩니다. `RegisterDependencies` `ViewModelLocator` 그런 다음 `BasketViewModel` 클래스의 인스턴스를 만들 때 해당 생성자는 autofac가 `IBasketService` 확인 하 고 `BasketService` 클래스의 인스턴스를 반환 하는 형식을 허용 합니다. 종속성 주입에 대 한 자세한 내용은 [종속성 주입 소개](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)를 참조 하세요.

그림 10-2에서는 바구니 마이크로 서비스에 게 표시 되 `BasketView`는 바구니 데이터를 보내는 클래스의 상호 작용을 보여 줍니다.

바구니 마이크로 서비스 데이터 보내기 [ ![(accessing-remote-data-images/basketdata.png " ")]] (accessing-remote-data-images/basketdata-large.png#lightbox "바구니 마이크로 서비스 데이터 보내기")

**그림 10-2**: 바구니 마이크로 서비스 데이터 보내기

바구니 `ReCalculateTotalAsync` 에 항목이 추가 되 면 `BasketViewModel` 클래스의 메서드가 호출 됩니다. 이 메서드는 바구니에 있는 항목의 total 값을 업데이트 하 고 다음 코드 예제와 같이 바구니 데이터를 바구니 마이크로 서비스 보냅니다.

```csharp
private async Task ReCalculateTotalAsync()  
{  
    ...  
    await _basketService.UpdateBasketAsync(new CustomerBasket  
    {  
        BuyerId = userInfo.UserId,   
        Items = BasketItems.ToList()  
    }, authToken);  
}
```

이 메서드는 autofac `UpdateBasketAsync` 에 `BasketService` `BasketViewModel` 의해에 삽입 된 인스턴스의 메서드를 호출 합니다. 다음 메서드는 메서드를 `UpdateBasketAsync` 보여 줍니다.

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

이 메서드는 요청을 보낼 리소스를 식별 하는 URI를 빌드하고, `RequestProvider` 클래스를 사용 하 여 리소스에서 POST HTTP 메서드를 호출한 다음 결과를 `BasketViewModel`로 반환 합니다. 인증 프로세스 중에 IdentityServer에서 얻은 액세스 토큰은 바구니 마이크로 서비스 요청에 권한을 부여 하는 데 필요 합니다. 권한 부여에 대 한 자세한 내용은 [권한 부여](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)를 참조 하세요.

다음 코드 예제에서는 `PostAsync` `RequestProvider` 클래스의 메서드 중 하나를 보여 줍니다.

```csharp
public async Task<TResult> PostAsync<TResult>(  
    string uri, TResult data, string token = "", string header = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    ...  
    var content = new StringContent(JsonConvert.SerializeObject(data));  
    content.Headers.ContentType = new MediaTypeHeaderValue("application/json");  
    HttpResponseMessage response = await httpClient.PostAsync(uri, content);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>  
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

이 메서드는 `CreateHttpClient` 적절 한 헤더 집합을 사용 하 여 `HttpClient` 클래스의 인스턴스를 반환 하는 메서드를 호출 합니다. 그런 다음 URI에 의해 식별 되는 리소스에 비동기 POST 요청을 전송 하 고, serialize 된 바구니 데이터를 JSON 형식으로 보내고, 응답을 `HttpResponseMessage` 인스턴스에 저장 합니다. 그런 다음 응답이 성공 HTTP 상태 코드를 포함 하지 않는 경우 예외를 throw 하는 메서드를호출합니다.`HandleResponse` 그런 다음 응답을 문자열로 읽어 JSON `CustomerBasket` 에서 개체로 변환 하 고 `BasketService`로 반환 합니다. `CreateHttpClient` 메서드에 대 한 자세한 내용은 [GET 요청 만들기](#making_a_get_request)를 참조 하세요.

`HttpClient.PostAsync` `BasketController` 클래스의 메서드가를 호출 하`Post` 는 경우, 다음 코드 예제와 같이 바구니 프로젝트의 클래스에서 메서드가 호출 됩니다. `PostAsync` `RequestProvider`

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

이 메서드는 `RedisBasketRepository` 클래스의 인스턴스를 사용 하 여 바구니 데이터를 Redis cache에 보관 하 고, 성공 HTTP 상태 코드 및 JSON 형식의 `CustomerBasket` 인스턴스가 포함 된 응답 메시지로 반환 합니다.

#### <a name="making-a-delete-request"></a>삭제 요청 만들기

그림 10-3에서는 바구니 마이크로 서비스 바구니 데이터를 삭제 하는 클래스의 상호 작용을 `CheckoutView`보여 줍니다.

![](accessing-remote-data-images/checkoutdata.png "바구니의 데이터 삭제 마이크로 서비스")

**그림 10-3**: 바구니 마이크로 서비스에서 데이터 삭제

체크 아웃 프로세스를 호출 하 `CheckoutAsync` 는 경우 `CheckoutViewModel` 클래스의 메서드가 호출 됩니다. 이 메서드는 다음 코드 예제에서 보여 주는 것 처럼 장바구니를 지우기 전에 새 주문을 만듭니다.

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

이 메서드는 autofac `ClearBasketAsync` 에 `BasketService` `CheckoutViewModel` 의해에 삽입 된 인스턴스의 메서드를 호출 합니다. 다음 메서드는 메서드를 `ClearBasketAsync` 보여 줍니다.

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

이 메서드는 요청을 보낼 리소스를 식별 하는 URI를 빌드하고 `RequestProvider` 클래스를 사용 하 여 리소스에 대 한 DELETE HTTP 메서드를 호출 합니다. 인증 프로세스 중에 IdentityServer에서 얻은 액세스 토큰은 바구니 마이크로 서비스 요청에 권한을 부여 하는 데 필요 합니다. 권한 부여에 대 한 자세한 내용은 [권한 부여](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)를 참조 하세요.

다음 코드 예제에서는 `DeleteAsync` `RequestProvider` 클래스의 메서드를 보여 줍니다.

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

이 메서드는 `CreateHttpClient` 적절 한 헤더 집합을 사용 하 여 `HttpClient` 클래스의 인스턴스를 반환 하는 메서드를 호출 합니다. 그런 다음 URI로 식별 되는 리소스에 비동기 삭제 요청을 제출 합니다. `CreateHttpClient` 메서드에 대 한 자세한 내용은 [GET 요청 만들기](#making_a_get_request)를 참조 하세요.

`HttpClient.DeleteAsync` `BasketController` 클래스의 메서드가를 호출 하`Delete` 는 경우, 다음 코드 예제와 같이 바구니 프로젝트의 클래스에서 메서드가 호출 됩니다. `DeleteAsync` `RequestProvider`

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

이 메서드는 `RedisBasketRepository` 클래스의 인스턴스를 사용 하 여 Redis cache에서 바구니 데이터를 삭제 합니다.

## <a name="caching-data"></a>데이터 캐싱

자주 액세스 하는 데이터를 앱에 가까이 있는 빠른 저장소에 캐싱하여 앱 성능을 향상 시킬 수 있습니다. 빠른 저장소가 원래 원본 보다 더 가까운 앱에 있는 경우 캐싱은 데이터를 검색할 때 응답 시간을 크게 향상 시킬 수 있습니다.

가장 일반적인 형태의 캐싱은 캐시를 참조 하 여 앱에서 데이터를 검색 하는 읽기-캐싱입니다. 데이터가 캐시에 없으면 데이터 저장소에서 검색 되어 캐시에 추가 됩니다. 앱은 캐시 배제 패턴을 사용 하 여 읽기-읽기 캐싱을 구현할 수 있습니다. 이 패턴은 항목이 현재 캐시에 있는지 여부를 확인 합니다. 항목이 캐시에 없으면 데이터 저장소에서 읽어서 캐시에 추가 됩니다. 자세한 내용은 [캐시](/azure/architecture/patterns/cache-aside/) 배제 패턴을 참조 하세요.

> [!TIP]
> 자주 읽고 자주 변경 되지 않는 데이터를 캐시 합니다. 이 데이터는 앱에서 처음 검색할 때 필요할 때 캐시에 추가할 수 있습니다. 즉, 응용 프로그램은 데이터 저장소에서 한 번만 데이터를 인출 해야 하며, 그 후에는 캐시를 사용 하 여 후속 액세스를 만족할 수 있습니다.

EShopOnContainers reference 응용 프로그램과 같은 분산 응용 프로그램은 다음 캐시 중 하나 또는 둘 모두를 제공 해야 합니다.

- 여러 프로세스 또는 컴퓨터에서 액세스할 수 있는 공유 캐시.
- 응용 프로그램을 실행 하는 장치에 로컬로 데이터가 저장 되는 개인 캐시.

EShopOnContainers 모바일 앱은 앱의 인스턴스를 실행 하는 장치에 로컬로 데이터가 보관 되는 개인 캐시를 사용 합니다. EShopOnContainers reference 응용 프로그램 [에서 사용 하는 캐시에 대 한 자세한 내용은 .net 마이크로 서비스: 컨테이너화된 .NET 애플리케이션을 위한 아키텍처](https://aka.ms/microservicesebook)를 참조하세요.

> [!TIP]
> 캐시는 언제 든 지 사라질 수 있는 임시 데이터 저장소로 생각할 수 있습니다. 데이터가 원래 데이터 저장소 뿐만 아니라 캐시에 유지 되는지 확인 합니다. 캐시를 사용할 수 없게 되 면 데이터가 손실 될 가능성이 최소화 됩니다.

### <a name="managing-data-expiration"></a>데이터 만료 관리

캐시 된 데이터가 항상 원래 데이터와 일치 하는 것은 바람직하지 않습니다. 원래 데이터 저장소의 데이터는 캐시 된 후 변경 되어 캐시 된 데이터가 부실 해질 수 있습니다. 따라서 앱은 캐시의 데이터가 최대한 최신 상태 인지 확인 하는 데 도움이 되는 전략을 구현 해야 하지만 캐시의 데이터가 오래 된 경우 발생 하는 상황을 감지 하 고 처리할 수도 있습니다. 대부분의 캐싱 메커니즘을 사용 하면 데이터를 만료 하도록 캐시를 구성할 수 있으므로 데이터를 최신으로 유지 하는 기간을 줄일 수 있습니다.

> [!TIP]
> 캐시를 구성할 때 기본 만료 시간을 설정 합니다. 많은 캐시가 만료를 구현 하며,이는 데이터를 무효화 하 고 지정 된 기간 동안 액세스 하지 않은 경우 캐시에서 제거 합니다. 그러나 만료 기간을 선택할 때는 주의 해야 합니다. 너무 짧으면 데이터가 너무 빨리 만료 되 고 캐싱의 이점이 줄어듭니다. 너무 오래 된 경우 데이터 위험이 부실 합니다. 따라서 만료 시간은 데이터를 사용 하는 앱에 대 한 액세스 패턴과 일치 해야 합니다.

캐시 된 데이터가 만료 되 면 캐시에서 제거 해야 하며, 앱은 원래 데이터 저장소에서 데이터를 검색 하 여 캐시에 다시 저장 해야 합니다.

데이터가 너무 오래 지속 될 수 있는 경우 캐시가 채워질 수 있습니다. 따라서 제거 라고 하는 프로세스에서 일부 항목을 제거 하려면 캐시에 새 항목을 추가 하는 요청이 필요할 수 *있습니다.* 캐싱 서비스는 일반적으로 가장 최근에 사용 된 방식으로 데이터를 제거 합니다. 그러나 가장 최근에 사용한 것을 비롯 한 다른 제거 정책 및 선입 first (선입 out)가 있습니다. 자세한 내용은 [캐싱 지침](/azure/architecture/best-practices/caching/)을 참조 하세요.

<a name="caching_images" />

### <a name="caching-images"></a>이미지 캐싱

EShopOnContainers 모바일 앱은 캐시를 활용 하는 원격 제품 이미지를 사용 합니다. 이러한 이미지는 [`Image`](xref:Xamarin.Forms.Image) 컨트롤 `CachedImage` 및 [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) 라이브러리에서 제공 하는 컨트롤에 의해 표시 됩니다.

Xamarin Forms [`Image`](xref:Xamarin.Forms.Image) 컨트롤은 다운로드 된 이미지의 캐싱을 지원 합니다. 캐싱은 기본적으로 사용 하도록 설정 되며 24 시간 동안 로컬로 이미지를 저장 합니다. 또한 [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity) 속성을 사용 하 여 만료 시간을 구성할 수 있습니다. 자세한 내용은 [다운로드 한 이미지 캐싱](~/xamarin-forms/user-interface/images.md#downloaded-image-caching)을 참조 하세요.

FFImageLoading의 `CachedImage` 컨트롤은 xamarin.ios [`Image`](xref:Xamarin.Forms.Image) 컨트롤을 대체 하 여 보충 기능을 사용할 수 있는 추가 속성을 제공 합니다. 이 기능 중에서 컨트롤은 오류를 지원 하 고 이미지 자리 표시자를 로드 하는 동안 구성 가능한 캐싱을 제공 합니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱이의 컨트롤에서 사용 `CachedImage` [`ListView`](xref:Xamarin.Forms.ListView) `CatalogView`하는 데이터 `ProductTemplate`템플릿인의 컨트롤을 사용 하는 방법을 보여 줍니다.

```xaml
<ffimageloading:CachedImage
    Grid.Row="0"
    Source="{Binding PictureUri}"     
    Aspect="AspectFill">
    <ffimageloading:CachedImage.LoadingPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="default_campaign" />
            <On Platform="UWP" Value="Assets/default_campaign.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.LoadingPlaceholder>
    <ffimageloading:CachedImage.ErrorPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="noimage" />
            <On Platform="UWP" Value="Assets/noimage.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.ErrorPlaceholder>
</ffimageloading:CachedImage>
```

컨트롤 `CachedImage` 은 `LoadingPlaceholder` 및 속성을플랫폼별이미지로설정합니다.`ErrorPlaceholder` 속성은 `Source` 속성으로 지정 된 이미지를 검색 하는 동안 표시할 이미지를 지정 하 고 속성은 `ErrorPlaceholder` 이미지를 검색 하려고 할 때 오류가 발생 하는 경우 표시할 이미지를 지정 합니다. `LoadingPlaceholder` 속성에 `Source` 의해 지정 됩니다.

이름에서 알 때 컨트롤은 `CachedImage` `CacheDuration` 속성 값에 지정 된 시간 동안 장치의 원격 이미지를 캐시 합니다. 이 속성 값을 명시적으로 설정 하지 않으면 30 일의 기본값이 적용 됩니다.

## <a name="increasing-resilience"></a>복원 력 증대

원격 서비스 및 리소스와 통신 하는 모든 앱은 일시적인 오류에 중요 해야 합니다. 일시적인 오류에는 서비스에 대 한 네트워크 연결이 일시적으로 손실 되거나, 서비스를 일시적으로 사용할 수 없거나, 서비스가 사용 중일 때 발생 하는 시간 초과가 포함 됩니다. 이러한 오류는 자동으로 수정 되는 경우가 많으며, 적절 한 지연 후 작업이 반복 되 면 성공할 가능성이 높습니다.

일시적인 오류는 모든 예측 가능한 상황에서 철저 하 게 테스트 된 경우에도 앱의 인식 품질에 큰 영향을 줄 수 있습니다. 원격 서비스와 통신 하는 앱이 안정적으로 작동 하도록 하려면 다음을 모두 수행할 수 있어야 합니다.

- 오류가 발생 하는 경우 오류를 검색 하 고 오류가 일시적이 될 가능성이 있는지 확인 합니다.
- 오류가 일시적인 것일 가능성이 있는 것으로 확인 되 면 작업을 다시 시도 하 고 작업을 다시 시도한 횟수를 추적 합니다.
- 재시도 횟수, 각 시도 사이의 지연 시간 및 시도 실패 후 수행할 작업을 지정 하는 적절 한 재시도 전략을 사용 합니다.

이 일시적인 오류 처리는 재시도 패턴을 구현 하는 코드에서 원격 서비스에 액세스 하려는 모든 시도를 래핑하여 구현할 수 있습니다.

### <a name="retry-pattern"></a>재시도 패턴

응용 프로그램이 원격 서비스에 요청을 보내려고 할 때 오류를 검색 하는 경우 다음과 같은 방법으로 오류를 처리할 수 있습니다.

- 작업을 다시 시도 하 고 있습니다. 앱에서 실패 한 요청을 즉시 다시 시도할 수 있습니다.
- 지연 후 작업을 다시 시도 하는 중입니다. 앱은 요청을 다시 시도 하기 전에 적절 한 시간 동안 대기 해야 합니다.
- 작업을 취소 하 고 있습니다. 응용 프로그램에서 작업을 취소 하 고 예외를 보고 해야 합니다.

앱의 비즈니스 요구 사항에 맞게 다시 시도 전략을 조정 해야 합니다. 예를 들어 재시도 횟수 및 다시 시도 간격을 시도 중인 작업으로 최적화 하는 것이 중요 합니다. 작업이 사용자 상호 작용의 일부인 경우 재시도 간격은 짧고, 사용자가 응답을 기다릴 수 없도록 하기 위해 몇 번의 재시도만 시도 해야 합니다. 작업이 장기 실행 워크플로의 일부인 경우 워크플로를 취소 하거나 다시 시작 하는 데 비용이 많이 들고 시간이 오래 걸리는 경우에는 시도 사이에 대기 하 고 더 많은 시간을 다시 시도 하는 것이 적절 합니다.

> [!NOTE]
> 시도 간 지연 시간을 최소화 하 고 재시도 횟수를 최소화 하는 적극적인 재시도 전략은 거의 또는 대량으로 실행 되는 원격 서비스의 성능을 저하 시킬 수 있습니다. 또한 이러한 다시 시도 전략은 계속 해 서 실패 한 작업을 수행 하려고 하는 경우 앱의 응답성에 영향을 줄 수 있습니다.

한 번의 재시도 후에도 요청이 계속 실패 하는 경우 앱에서 더 많은 요청을 동일한 리소스로 이동 하 여 오류를 보고 하는 것이 더 좋습니다. 그런 다음 설정 된 기간이 지나면 응용 프로그램은 리소스에 대해 하나 이상의 요청을 만들어 성공 했는지 확인할 수 있습니다. 자세한 내용은 [회로 차단기 패턴](#circuit_breaker_pattern)을 참조 하세요.

> [!TIP]
> 무한 재시도 메커니즘을 구현 하지 마십시오. 유한 횟수의 재시도를 사용 하거나 [회로 차단기](/azure/architecture/patterns/circuit-breaker/) 패턴을 구현 하 여 서비스를 복구할 수 있도록 합니다.

EShopOnContainers 모바일 앱은 RESTful 웹 요청을 만들 때 현재 재시도 패턴을 구현 하지 않습니다. 그러나 [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) 라이브러리에서 제공하는 컨트롤 `CachedImage`은 이미지 로드를 다시 시도하여 일시적인 오류 처리를 지원합니다. 이미지 로드에 실패 하는 경우 추가 시도가 수행 됩니다. 시도 횟수는 `RetryCount` 속성으로 지정 되며, `RetryDelay` 속성에 의해 지정 된 지연 이후에 다시 시도 됩니다. 이러한 속성 값을 명시적으로 설정 하지 않으면 `RetryCount` 속성에 대해 3 개의 기본값이 적용 되 고 `RetryDelay` 속성에는 250ms 적용 됩니다. `CachedImage` 컨트롤에 대 한 자세한 내용은 [이미지 캐싱](#caching_images)을 참조 하세요.

EShopOnContainers reference 응용 프로그램은 재시도 패턴을 구현 합니다. 재시도 패턴을 `HttpClient` 클래스와 결합 하는 방법에 대 한 자세한 내용은 .net 마이크로 서비스를 참조 [하세요. 컨테이너화된 .NET 애플리케이션을 위한 아키텍처](https://aka.ms/microservicesebook)를 참조하세요.

다시 시도 패턴에 대 한 자세한 내용은 [재시도](/azure/architecture/patterns/retry/) 패턴을 참조 하세요.

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>회로 차단기 패턴

경우에 따라 수정 하는 데 시간이 오래 걸리는 예상 이벤트로 인해 오류가 발생할 수 있습니다. 이러한 오류는 전체 서비스 오류에 대 한 일부 연결 손실과의 범위를 지정할 수 있습니다. 이러한 상황에서는 응용 프로그램에서 성공할 가능성이 없는 작업을 다시 시도 하는 것은 무의미 하지 않으며, 대신 작업이 실패 한 것을 허용 하 고 그에 따라이 오류를 처리 해야 합니다.

회로 차단기 패턴은 응용 프로그램이 실패할 가능성이 있는 작업을 반복적으로 실행 하는 것을 방지할 수 있으며, 앱에서 오류가 해결 되었는지 여부도 검색할 수 있도록 합니다.

> [!NOTE]
> 회로 차단기 패턴의 목적은 재시도 패턴과 다릅니다. 재시도 패턴을 사용 하면 응용 프로그램이 성공 하다 고 가정 하에 작업을 다시 시도할 수 있습니다. 회로 차단기 패턴은 응용 프로그램이 실패할 가능성이 있는 작업을 수행할 수 없도록 합니다.

회로 차단기는 실패할 수 있는 작업에 대 한 프록시로 작동 합니다. 프록시는 발생 한 최근 오류 수를 모니터링 하 고,이 정보를 사용 하 여 작업을 계속 진행할 수 있는지 여부를 결정 하거나, 예외를 즉시 반환할 것인지 여부를 결정 합니다.

EShopOnContainers 모바일 앱은 현재 회로 차단기 패턴을 구현 하지 않습니다. 그러나 eShopOnContainers는 그렇지 않습니다. 자세한 내용은 .net 마이크로 서비스 [를 참조 하세요. 컨테이너화된 .NET 애플리케이션을 위한 아키텍처](https://aka.ms/microservicesebook)를 참조하세요.

> [!TIP]
> 재시도 및 회로 차단기 패턴을 결합 합니다. 앱은 재시도 패턴을 사용 하 여 회로 차단기를 통해 작업을 호출 하 여 재시도 및 회로 차단기 패턴을 결합할 수 있습니다. 그러나 다시 시도 논리는 회로 차단기에서 반환 된 모든 예외에 대 한 중요 한 것 이며 회로 차단기가 일시적인 오류가 아님을 나타내는 경우 다시 시도를 중단 합니다.

회로 차단기 패턴에 대 한 자세한 내용은 [회로 차단기](/azure/architecture/patterns/circuit-breaker/) 패턴을 참조 하세요.

## <a name="summary"></a>요약

많은 최신 웹 기반 솔루션은 웹 서버에서 호스트 되는 웹 서비스를 사용 하 여 원격 클라이언트 응용 프로그램에 대 한 기능을 제공 합니다. 웹 서비스에서 노출 하는 작업은 web API를 구성 하 고, 클라이언트 앱은 API에서 노출 하는 데이터 또는 작업이 구현 되는 방식을 몰라도 웹 API를 활용할 수 있어야 합니다.

자주 액세스 하는 데이터를 앱에 가까이 있는 빠른 저장소에 캐싱하여 앱 성능을 향상 시킬 수 있습니다. 앱은 캐시 배제 패턴을 사용 하 여 읽기-읽기 캐싱을 구현할 수 있습니다. 이 패턴은 항목이 현재 캐시에 있는지 여부를 확인 합니다. 항목이 캐시에 없으면 데이터 저장소에서 읽어서 캐시에 추가 됩니다.

웹 Api와 통신할 때 앱은 일시적인 오류를 인식 해야 합니다. 일시적인 오류에는 서비스에 대 한 네트워크 연결이 일시적으로 손실 되거나, 서비스를 일시적으로 사용할 수 없거나, 서비스가 사용 중일 때 발생 하는 시간 초과가 포함 됩니다. 이러한 오류는 자동으로 수정 되는 경우가 많으며, 적절 한 지연 후에 작업이 반복 되 면 성공할 가능성이 높습니다. 따라서 응용 프로그램은 일시적인 오류 처리 메커니즘을 구현 하는 코드에서 web API에 액세스 하려는 모든 시도를 래핑합니다.

## <a name="related-links"></a>관련 링크

- [다운로드 전자책 (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
