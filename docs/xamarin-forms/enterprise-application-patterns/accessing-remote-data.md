---
title: 원격 데이터에 액세스
description: 이 장에서 eShopOnContainers 모바일 앱 컨테이너 화 된 마이크로 서비스에서 데이터에 액세스 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 3a46b939fa87cd6535c9f86c46981c098542e7c9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105482"
---
# <a name="accessing-remote-data"></a>원격 데이터에 액세스

많은 최신 웹 기반 솔루션을 통해 응용 프로그램 원격 클라이언트에 대 한 기능을 제공 하기 위해 웹 서버에서 호스팅되는 웹 서비스를 사용 합니다. 웹 서비스를 노출 하는 작업을 웹 API를 구성 합니다.

클라이언트 앱 데이터 또는 API를 노출 하는 작업이 구현 하는 방법을 알 필요 없이 web API를 사용할 수 있어야 합니다. 이렇게 하려면 API를 사용 및 클라이언트 앱 및 웹 서비스 간에 교환 되는 데이터 구조의 데이터 형식에 동의 하도록 클라이언트 앱 및 웹 서비스를 사용할 수 있는 공통 표준을 적용 하 합니다.

## <a name="introduction-to-representational-state-transfer"></a>Representational State Transfer 소개

REST representational State Transfer ()는 하이퍼미디어 기반 분산된 시스템 구축에 아키텍처 스타일입니다. REST 모델의 주요 이점은 개방형 표준에 따라이 모델의 특정 구현에 액세스 하는 클라이언트 앱 구현에 바인딩되지 않습니다 하는 합니다. 따라서 Microsoft ASP.NET Core MVC를 사용 하 여 REST 웹 서비스를 구현할 수 있습니다 하 고 모든 언어 및 HTTP 요청을 생성 하 고 HTTP 응답을 구문 분석할 수 있는 도구 집합을 사용 하 여 클라이언트 앱을 개발할 수 있습니다.

REST 모델 탐색 체계를 사용 하 여 리소스 라고 하는 네트워크를 통해 개체 및 서비스를 나타냅니다. 일반적으로 REST를 구현 하는 시스템에 이러한 리소스에 액세스 하는 요청을 전송할 HTTP 프로토콜을 사용 합니다. 이러한 시스템에서는 클라이언트 앱 리소스를 식별 하는 URI 및 해당 리소스에 대해 수행할 작업을 나타내는 HTTP 메서드 (예: GET, POST, PUT 또는 DELETE) 형식으로 요청을 제출 합니다. HTTP 요청의 본문에는 작업을 수행 하는 데 필요한 모든 데이터가 포함 됩니다.

> [!NOTE]
> REST는 상태 비저장 요청 모델을 정의합니다. 따라서 HTTP 요청 독립적 이어야 하 고 임의 순서로 발생할 수 있습니다.

REST의 응답을 요청 하면 표준 HTTP 상태 코드를 사용 합니다. 예를 들어, 찾기 또는 지정된 된 리소스를 삭제 하지 못한 요청 HTTP 상태 코드 404 (찾을 수 없음)를 포함 하는 응답을 반환 해야 하는 동안 유효한 데이터를 반환 하는 요청의 HTTP 응답 코드 200 (정상)를 포함 해야 합니다.

RESTful 웹 API는 연결 된 리소스 집합을 노출 하 고 앱이 해당 리소스를 조작 하 고 사이 쉽게 탐색할 수 있도록 하는 핵심 작업을 제공 합니다. 따라서 일반적인 RESTful 웹 API를 구성 하는 Uri는 표시 하는 데이터 및 사용 하는 기능이 제공 하 여이 데이터에서 작동 하는 HTTP입니다.

다양 한 미디어 유형으로 알려진 형식에에서는 HTTP 요청 및 웹 서버에서 해당 응답 메시지에 클라이언트 앱에서 포함 된 데이터를 제공할 수 있습니다. 처리할 수 미디어 종류를 지정할 수 있는 클라이언트 앱을 메시지의 본문에 데이터를 반환 하는 요청을 보내면는 `Accept` 요청의 헤더입니다. 웹 서버에서이 미디어 형식은 지 원하는 경우를 포함 하는 응답으로 회신할 수 있습니다는 `Content-Type` 메시지의 본문에는 데이터의 형식을 지정 하는 헤더입니다. 응답 메시지를 구문 분석 하 고 메시지 본문에 결과 적절 하 게 해석 하도록 클라이언트 앱에서 수행 됩니다.

REST에 대 한 자세한 내용은 참조 하세요. [API 디자인](/azure/architecture/best-practices/api-design/) 하 고 [API 구현](/azure/architecture/best-practices/api-implementation/)합니다.

## <a name="consuming-restful-apis"></a>RESTful Api를 사용합니다.

EShopOnContainers 모바일 앱-Model-view-viewmodel (MVVM) 패턴 및 앱에서 사용 되는 도메인 엔터티 패턴 나타내는 모델 요소를 사용 합니다. EShopOnContainers 참조 응용 프로그램의 컨트롤러 및 리포지토리 클래스를 받아들이고 이러한 모델 개체의 대부분을 반환 합니다. 따라서 모바일 앱 및 컨테이너 화 된 마이크로 서비스 간에 전달 되는 모든 데이터가 저장 된 데이터 전송 개체 (Dto)으로 사용 됩니다. 주요 장점은 Dto를 사용 하 여 데이터를 전달 하 고 웹 서비스에서 데이터를 수신 하는 단일 원격 호출에 더 많은 데이터를 전송 하 여 앱 수를 줄일 수 해야 하는 원격 호출의 경우

### <a name="making-web-requests"></a>웹 요청 수행

모바일 앱에서 사용 하는 eShopOnContainers는 `HttpClient` 미디어 유형으로 사용 되는 JSON 사용 하 여 HTTP를 통해 요청을 수행 하는 클래스입니다. 이 클래스는 비동기적으로 HTTP 요청을 보내는 기능을 제공 하 고 리소스를 식별 하는 URI에서 HTTP 응답을 수신 합니다. `HttpResponseMessage` 클래스는 HTTP 요청을 생성 된 후에 REST API에서 받은 HTTP 응답 메시지를 나타냅니다. 상태 코드, 헤더 및 모든 본문을 포함 하 여 응답에 대 한 정보를 포함 합니다. 합니다 `HttpContent` 클래스를 나타내는 HTTP 본문 및 콘텐츠 헤더와 같은 `Content-Type` 고 `Content-Encoding`입니다. 사용 하 여 콘텐츠를 읽을 수 있습니다는 `ReadAs` 메서드를 같은 `ReadAsStringAsync` 및 `ReadAsByteArrayAsync`데이터의 형식에 따라 합니다.

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>GET 요청

`CatalogService` 클래스 카탈로그 마이크로 서비스에서 데이터 검색 프로세스를 관리 하는 데 사용 됩니다. 에 `RegisterDependencies` 에서 메서드를 `ViewModelLocator` 클래스는 `CatalogService` 클래스에 대 한 형식 매핑을으로 등록 됩니다는 `ICatalogService` Autofac 종속성 주입 컨테이너를 사용 하 여 형식입니다. 그런 다음 인스턴스가 `CatalogViewModel` 해당 생성자는 클래스를 만들는 `ICatalogService` 형식으로의 인스턴스를 반환 하는 Autofac 확인 되 면를 `CatalogService` 클래스입니다. 종속성 주입에 대 한 자세한 내용은 참조 하세요. [종속성 주입 소개](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)합니다.

그림 10-1에서 표시 하는 것에 대 한 카탈로그 마이크로 서비스에서 카탈로그 데이터를 읽는 클래스의 상호 작용을 보여 합니다 `CatalogView`합니다.

[![](accessing-remote-data-images/catalogdata.png "카탈로그 마이크로 서비스에서 데이터를 검색할")](accessing-remote-data-images/catalogdata-large.png#lightbox "카탈로그 마이크로 서비스에서 데이터 검색")

**그림 10-1**: 카탈로그 마이크로 서비스에서 데이터 검색

경우는 `CatalogView` 을 탐색 하면를 `OnInitialize` 에서 메서드를 `CatalogViewModel` 클래스 라고 합니다. 이 메서드에 다음 코드 예제에서 설명한 것 처럼 카탈로그 마이크로 서비스에서 카탈로그 데이터를 검색 합니다.

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

이 메서드를 호출 합니다 `GetCatalogAsync` 메서드를 `CatalogService` 에 삽입 된 인스턴스는 `CatalogViewModel` Autofac에서. 다음 코드 예제는 `GetCatalogAsync` 메서드:

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

이 메서드를는 요청을 보낼 리소스를 식별 하는 URI을 만들고 사용 하는 `RequestProvider` 결과를 반환 하기 전에 리소스에 대해 GET HTTP 메서드를 호출 하는 클래스는 `CatalogViewModel`합니다. `RequestProvider` 클래스 리소스에 해당 리소스에 대해 수행할 작업을 나타내는 HTTP 메서드를 식별 하는 URI의 형식으로 요청을 제출 하는 기능을 포함 하 고 작업을 수행 하는 데 필요한 모든 데이터를 포함 하는 본문을 합니다. 하는 방법에 대 한 자세한 `RequestProvider` 클래스에 주입 됩니다 합니다 `CatalogService class`를 참조 하세요 [종속성 주입 소개](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)합니다.

다음 코드 예제는 `GetAsync` 의 메서드는 `RequestProvider` 클래스:

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

이 메서드를 호출 합니다 `CreateHttpClient` 의 인스턴스를 반환 하는 메서드는 `HttpClient` 적절 한 헤더 집합을 사용 하 여 클래스입니다. 다음에 저장 되 고 응답을 사용 하 여 URI로 식별 되는 비동기 GET 요청을 제출 하는 `HttpResponseMessage` 인스턴스. `HandleResponse` 메서드는 호출 예외를 throw 하는 응답에는 성공적인 HTTP 상태 코드를 포함 하지 않습니다. 다음 응답은 JSON에서 변환 된 문자열로 읽습니다를 `CatalogRoot` 개체를 반환 합니다 `CatalogService`합니다.

`CreateHttpClient` 메서드는 다음 코드 예제에 표시 됩니다.

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

이 메서드는의 새 인스턴스를 만듭니다는 `HttpClient` 클래스 집합과 합니다 `Accept` 수행한 모든 요청의 헤더를 `HttpClient` 인스턴스를 `application/json`, JSON을 사용 하 여 서식이 지정 될 모든 응답의 콘텐츠를 필요로 하는 것을 나타내는입니다. 액세스 토큰에 인수로 전달 된 경우는 `CreateHttpClient` 메서드를 추가할는 `Authorization` 수행한 모든 요청의 헤더를 `HttpClient` 인스턴스, 문자열을 접두사로 `Bearer`. 권한 부여에 대 한 자세한 내용은 참조 하세요. [권한 부여](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)합니다.

경우는 `GetAsync` 에서 메서드를 `RequestProvider` 클래스 `HttpClient.GetAsync`의 `Items` 에서 메서드는 `CatalogController` Catalog.API 프로젝트에서 클래스 호출을 다음 코드 예제에 나와 있는:

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

이 메서드 EntityFramework를 사용 하 여 SQL database에서 카탈로그 데이터를 검색 하 여 성공적인 HTTP 상태 코드를 포함 하는 응답 메시지로 반환 하 고 JSON의 컬렉션 형식 `CatalogItem` 인스턴스.

#### <a name="making-a-post-request"></a>POST 요청

`BasketService` 클래스는 데이터 검색을 관리 하 고 장바구니 마이크로 서비스를 사용 하 여 프로세스를 업데이트 하는 데 사용 됩니다. 에 `RegisterDependencies` 에서 메서드를 `ViewModelLocator` 클래스는 `BasketService` 클래스에 대 한 형식 매핑을으로 등록 됩니다는 `IBasketService` Autofac 종속성 주입 컨테이너를 사용 하 여 형식입니다. 그런 다음 인스턴스가 `BasketViewModel` 해당 생성자는 클래스를 만들는 `IBasketService` 형식으로의 인스턴스를 반환 하는 Autofac 확인 되 면를 `BasketService `클래스입니다. 종속성 주입에 대 한 자세한 내용은 참조 하세요. [종속성 주입 소개](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)합니다.

그림 10-2 바구니에서 표시할 데이터를 전송 하는 클래스의 상호 작용을 보여 합니다 `BasketView`, 장바구니 마이크로 서비스.

[![](accessing-remote-data-images/basketdata.png "장바구니 마이크로 서비스에 데이터 보내기")](accessing-remote-data-images/basketdata-large.png#lightbox "장바구니 마이크로 서비스에 데이터 보내기")

**그림 10-2**: 장바구니 마이크로 서비스에 데이터 보내기

시장 바구니에 항목 추가 되 면 합니다 `ReCalculateTotalAsync` 의 메서드는 `BasketViewModel` 클래스 라고 합니다. 이 메서드는 시장 바구니에 있는 항목의 총 값을 업데이트 하 고 다음 코드 예제에서와 같이 장바구니 마이크로 서비스에 바구니 데이터를 보냅니다.

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

이 메서드를 호출 합니다 `UpdateBasketAsync` 메서드를 `BasketService` 에 삽입 된 인스턴스는 `BasketViewModel` Autofac에서. 에서는 다음 메서드는 `UpdateBasketAsync` 메서드:

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

이 메서드를는 요청을 보낼 리소스를 식별 하는 URI을 만들고 사용 하는 `RequestProvider` 결과를 반환 하기 전에 리소스에서 POST HTTP 메서드를 호출 하는 클래스는 `BasketViewModel`합니다. 액세스 토큰을 인증 프로세스 동안 IdentityServer에서 얻은 장바구니 마이크로 서비스에 대 한 요청 권한을 부여 해야 합니다. 권한 부여에 대 한 자세한 내용은 참조 하세요. [권한 부여](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)합니다.

다음 코드 예제 중 하나를 표시 합니다 `PostAsync` 의 메서드는 `RequestProvider` 클래스:

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

이 메서드를 호출 합니다 `CreateHttpClient` 의 인스턴스를 반환 하는 메서드는 `HttpClient` 적절 한 헤더 집합을 사용 하 여 클래스입니다. 다음 JSON 형식으로 저장 되는 응답에 전송 되는 바구니 serialize 된 데이터를 사용 하 여 URI로 식별 되는 비동기 POST 요청을 제출 하는 `HttpResponseMessage` 인스턴스. `HandleResponse` 메서드는 호출 예외를 throw 하는 응답에는 성공적인 HTTP 상태 코드를 포함 하지 않습니다. 그런 다음 응답을 JSON에서 변환 된 문자열을로 읽는 `CustomerBasket` 개체를 반환 합니다 `BasketService`합니다. 에 대 한 자세한 내용은 합니다 `CreateHttpClient` 메서드를 참조 하세요 [가져오기 요청을 만드는](#making_a_get_request)합니다.

경우는 `PostAsync` 에서 메서드를 `RequestProvider` 클래스 `HttpClient.PostAsync`의 `Post` 에서 메서드는 `BasketController` Basket.API 프로젝트에서 클래스 호출을 다음 코드 예제에 나와 있는:

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

인스턴스를 사용 하 여이 메서드는 `RedisBasketRepository` Redis 캐시로 바구니 데이터를 유지 하기 위해 클래스 및 형식이 지정 된 HTTP 상태 코드, 성공 및 JSON을 포함 하는 응답 메시지를 사용자에 게 돌려보냅니다 `CustomerBasket` 인스턴스.

#### <a name="making-a-delete-request"></a>삭제 요청

그림 10-3 바구니 데이터 장바구니 마이크로 서비스를 삭제 하는 클래스의 상호 작용을 보여 합니다 `CheckoutView`합니다.

![](accessing-remote-data-images/checkoutdata.png "장바구니 마이크로 서비스에서 데이터 삭제")

**그림 10-3**: 장바구니 마이크로 서비스에서 데이터 삭제

체크 아웃 프로세스를 호출 하면 합니다 `CheckoutAsync` 의 메서드는 `CheckoutViewModel` 클래스 라고 합니다. 이 메서드는 다음 코드 예제에서 설명한 것 처럼 시장 바구니를 지우기 전에 새 순서를 만듭니다.

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

이 메서드를 호출 합니다 `ClearBasketAsync` 메서드를 `BasketService` 에 삽입 된 인스턴스는 `CheckoutViewModel` Autofac에서. 에서는 다음 메서드는 `ClearBasketAsync` 메서드:

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

이 메서드는 요청으로 전송 될 리소스를 식별 하는 URI을 만들고 사용 하 여 `RequestProvider` 리소스에서 DELETE HTTP 메서드를 호출 하는 클래스입니다. 액세스 토큰을 인증 프로세스 동안 IdentityServer에서 얻은 장바구니 마이크로 서비스에 대 한 요청 권한을 부여 해야 합니다. 권한 부여에 대 한 자세한 내용은 참조 하세요. [권한 부여](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)합니다.

다음 코드 예제는 `DeleteAsync` 의 메서드는 `RequestProvider` 클래스:

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

이 메서드를 호출 합니다 `CreateHttpClient` 의 인스턴스를 반환 하는 메서드는 `HttpClient` 적절 한 헤더 집합을 사용 하 여 클래스입니다. 다음 URI로 식별 되는 리소스를 비동기 삭제 요청을 제출 합니다. 에 대 한 자세한 내용은 합니다 `CreateHttpClient` 메서드를 참조 하세요 [가져오기 요청을 만드는](#making_a_get_request)합니다.

경우는 `DeleteAsync` 에서 메서드를 `RequestProvider` 클래스 `HttpClient.DeleteAsync`의 `Delete` 에서 메서드는 `BasketController` Basket.API 프로젝트에서 클래스 호출을 다음 코드 예제에 나와 있는:

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

인스턴스를 사용 하 여이 메서드는 `RedisBasketRepository` 클래스 Redis cache에서 basket 데이터를 삭제 합니다.

## <a name="caching-data"></a>데이터 캐싱

빠른 저장소에 가까이 있는 자주 액세스 하는 데이터를 캐시 하 여 앱의 성능을 향상 될 수 있습니다 앱. 빠른 저장소 원본 보다 응용 프로그램에 더 가까이 있는 경우 캐싱 크게 향상 시킬 수 응답 데이터를 검색할 때 시간입니다.

Caching에 대 한 가장 일반적인 형태의 read-through 캐싱의 경우 앱은 캐시를 참조 하 여 데이터를 검색 합니다. 데이터 캐시에 없는 경우에 데이터 저장소에서 검색 하 고 캐시에 추가 합니다. 앱은 캐시 배제 패턴을 사용 하 여 read-through 캐싱을 구현할 수 있습니다. 이 패턴의 항목이 캐시에 현재 인지 여부를 결정 합니다. 항목이 캐시에 없는 경우 데이터 저장소에서 읽은 후 캐시에 추가 합니다. 자세한 내용은 참조는 [캐시 배제](/azure/architecture/patterns/cache-aside/) 패턴입니다.

> [!TIP]
> 자주 읽고 자주 변경 하는 데이터를 캐시 합니다. 이 데이터는 필요할 때 처음으로 앱에서 검색 된 캐시에 추가할 수 있습니다. 즉, 앱 데이터 저장소에서 한 번만 데이터를 인출 해야 이후 액세스는 캐시를 사용 하 여 충족할 수 있습니다.

배포 응용 프로그램에 eShopOnContainers 참조 응용 프로그램 같은 제공 해야 다음 캐시 중 하나 또는 모두:

-   공유 캐시를 여러 프로세스 또는 컴퓨터에서 액세스할 수 있습니다.
-   앱을 실행 하는 장치에서 데이터 로컬로 하 게 유지 하는 개인 캐시 합니다.

EShopOnContainers 모바일 앱 데이터를 앱의 인스턴스를 실행 하는 장치에서 로컬로 유지 되는 전용 캐시를 사용 합니다. EShopOnContainers 참조 응용 프로그램에서 사용 되는 캐시에 대 한 정보를 참조 하세요 [.NET 마이크로 서비스: 컨테이너 화 된.NET 응용 프로그램에 대 한 아키텍처](https://aka.ms/microservicesebook)합니다.

> [!TIP]
> 언제 든 지 사라질 수 있는 임시 데이터 저장소로 캐시 생각 합니다. 데이터 캐시를 비롯 하 여 원래 데이터 저장소에 유지 되도록 합니다. 캐시를 사용할 수 없게 하는 경우에 다음 데이터 손실 가능성 최소화 됩니다.

### <a name="managing-data-expiration"></a>데이터 만료 관리

캐시 된 데이터가 항상 원래 데이터와 일치 될 것 이라는 예상 실용적이 지 않습니다. 원래 데이터 저장소의 데이터는 것은 캐싱 후, 유효 하지 않게 하는 캐시 된 데이터 변경 될 수 있습니다. 따라서 앱 해야 캐시의 데이터를 최대한으로 최신 상태 인지 확인 하는 데 도움이 되는 전략을 구현할 수 검색을 캐시의 데이터가 오래 된 때 발생 하는 상황을 처리 합니다. 대부분의 캐싱 메커니즘 데이터를 만료 하도록 캐시를 사용 하 고 따라서 데이터가 만료 되었을 기간을 줄입니다.

> [!TIP]
> 기본 만료를 설정 합니다. 캐시를 구성 하는 경우 시간입니다. 많은 캐시 데이터를 무효화 하 고 지정 된 기간 동안 액세스 하지 않는 경우 캐시에서 제거 하는 만료를 구현 합니다. 그러나 만료 기간을 선택할 때 주의 기울여 해야 합니다. 너무 짧은, 수행 되는 경우 데이터는 너무 빨리 만료 됩니다 및 캐싱의 이점은 줄어듭니다. 수행 되는 경우 너무 오래 부실 데이터 위험 합니다. 따라서 만료 시간에 데이터를 사용 하는 앱에 대 한 액세스 패턴을 일치 해야 합니다.

캐시 된 데이터가 만료 되 고 캐시에서 제거 해야 앱을 검색 해야 하는 경우 원래 데이터의 데이터를 저장 하 고 캐시에 다시 배치 합니다.

것도 가능 데이터를 오랜 기간 동안 유지 하도록 허용 된 경우 캐시 채울 수 있습니다. 따라서 캐시에 새 항목을 추가 하는 요청 해야 할 수 있습니다 라는 프로세스에서 일부 항목을 제거 *제거*합니다. 캐싱 서비스는 일반적으로 가장 최근에 사용한 단위로 데이터를 제거합니다. 그러나 가장 최근에 사용한, 및 선입 선출 비롯 한 다른 제거 정책에 있습니다. 자세한 내용은 [캐싱 지침](/azure/architecture/best-practices/caching/)합니다.

<a name="caching_images" />

### <a name="caching-images"></a>이미지 캐싱

EShopOnContainers 모바일 앱을 캐시할 활용 하는 원격 제품 이미지를 사용 합니다. 이러한 이미지에서 표시 됩니다는 [ `Image` ](xref:Xamarin.Forms.Image) 컨트롤 및 `CachedImage` 에서 제공 하는 컨트롤을 [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) 라이브러리입니다.

Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image) 컨트롤이 다운로드 되는 이미지의 캐싱을 지원 합니다. 캐싱은 기본적으로 활성화 되 고 24 시간 동안 로컬 이미지를 저장 합니다. 만료 시간을 사용 하 여 구성할 수 있습니다 또한 합니다 [ `CacheValidity` ](xref:Xamarin.Forms.UriImageSource.CacheValidity) 속성입니다. 자세한 내용은 [다운로드 한 이미지 캐싱](~/xamarin-forms/user-interface/images.md#downloaded-image-caching)합니다.

FFImageLoading의 `CachedImage` 컨트롤을 사용 하면 Xamarin.Forms에 대 한 대체가 [ `Image` ](xref:Xamarin.Forms.Image) 컨트롤에 보조 기능을 사용 하는 추가 속성을 제공 합니다. 이 기능을 간에 컨트롤, 오류를 지원 하 고 자리 표시자 이미지를 로드 하는 동안 구성 가능한 캐싱을 제공 합니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱을 사용 하는 방법을 보여 줍니다.는 `CachedImage` 에서 제어를 `ProductTemplate`는에서 사용 하는 데이터 템플릿입니다를 [ `ListView` ](xref:Xamarin.Forms.ListView) 에서 제어할는 `CatalogView`:

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

`CachedImage` 집합을 제어 합니다 `LoadingPlaceholder` 및 `ErrorPlaceholder` 플랫폼별 이미지 속성입니다. `LoadingPlaceholder` 속성에서 지정한 이미지 하는 동안 표시 될 이미지를 지정 합니다 `Source` 속성을 검색 및 `ErrorPlaceholder` 이미지를 검색 하려고 할 때 오류가 발생 하는 경우 표시할 이미지를 지정 하는 속성 지정 된 된 `Source` 속성입니다.

이름에서 알 수 있듯이 합니다 `CachedImage` 컨트롤의 값으로 지정 된 시간에 대 한 장치에서 원격 이미지에서 캐시를 `CacheDuration` 속성입니다. 이 속성 값이 명시적으로 설정 되지 않은 경우 기본값인 30 일 동안 적용 됩니다.

## <a name="increasing-resilience"></a>복원 력 증가

원격 서비스 및 리소스와 통신 하는 모든 앱은 일시적인 오류에 민감해 야 합니다. 일시적인 오류는 서비스에 대 한 네트워크 연결 끊김, 서비스 또는 서비스 사용량이 많을 때 발생 하는 시간 제한의 일시적인 사용 중단을 포함 합니다. 이러한 오류는 자체적으로 수정, 종종 이므로 잠시 후 동작을 반복 하는 경우 성공할 가능성이 있습니다.

일시적인 오류는 철저 하 게 예측 가능한 모든 환경에서 테스트 된 경우에 응용 프로그램의 체감된 품질에 엄청난 영향을 줄을 수 있습니다. 원격 서비스와 통신 하는 앱을 안정적으로 작동 하도록 다음의 모든 작업을 수행 하 수 있어야 합니다.

-   오류는 일시적일 가능성이 있는지 확인 하 고 발생할 때 오류를 감지 합니다.
-   오류는 일시적일 수 고를 추적할 수 있는 작업을 시도한 횟수를 결정 하는 경우 작업을 다시 시도 합니다.
-   다시 시도 횟수, 각 시도 및 작업을 실패 한 시도 후 간격 수를 지정 하는 적절 한 재시도 전략을 사용 합니다.

이 일시적인 오류 처리 재시도 패턴을 구현 하는 코드의 원격 서비스에 액세스 하려는 모든 시도 래핑하여 구현할 수 있습니다.

### <a name="retry-pattern"></a>재시도 패턴

원격 서비스에 요청을 전송 하려고 할 때 오류를 감지 하는 앱을 하는 경우 다음 방법 중 하나로 오류 처리할 수 있습니다.:

-   작업을 다시 시도 합니다. 앱 수 다시 시도 실패 한 요청 즉시 합니다.
-   잠시 후 작업을 다시 시도 합니다. 앱에서 요청을 다시 시도 하기 전에 적절 한 기간 기다려야 합니다.
-   작업을 취소 합니다. 응용 프로그램 작업을 취소 하 고 예외를 보고 해야 합니다.

다시 시도 전략은 앱의 비즈니스 요구 사항에 맞게 튜닝 해야 합니다. 예를 들어, 재시도 횟수를 최적화 하 고 다시 시도 간격을 시도 하 고 작업에 중요 한 것입니다. 작업이 사용자 조작의 일부 이면 재시도 간격이 짧아야 하 고만 몇 번 다시 시도 되지 않도록 하기 위해 응답을 대기 하는 사용자가 있어야 합니다. 작업의 일부인 경우 장기 실행 워크플로 취소 또는 워크플로 다시 시작 비용이 많이 들거나 시간이 많이 소요 됩니다 하는 위치를 더 이상 시도 간에 대기 하 고 여러 번 재시도를 적절 한 것입니다.

> [!NOTE]
> 많은 수의 재시도 시도 간 지연 시간을 최소화 한 적극적인 다시 시도 전략에는 용량에 근접 했거나 실행 하는 원격 서비스를 저하 시킬 수 있습니다. 또한 이러한 재시도 전략도 영향을 줄 수 앱의 응답성을 지속적으로 실패 한 작업을 수행 하려고 시도 하는 경우.

요청 된 횟수의 재시도 후 계속 실패 하면, 동일한 리소스로 이동 하는 추가 요청을 차단 하 고 오류를 보고 하는 앱에 대 한 것이 좋습니다. 그런 다음 설정된 기간 후 앱에 요청할 수 이상의 성공적인 지 확인할 리소스. 자세한 내용은 [회로 차단기 패턴](#circuit_breaker_pattern)합니다.

> [!TIP]
> 무한 재시도 메커니즘을 구현 하지 않습니다. 한정 된 수의 재시도 사용 하거나 구현 합니다 [회로 차단기](/azure/architecture/patterns/circuit-breaker/) 서비스 복구를 허용 하는 패턴입니다.

RESTful 웹 요청을 생성할 때 eShopOnContainers 모바일 앱을 다시 시도 패턴을 현재 구현 하지 않습니다. 그러나 합니다 `CachedImage` 에서 제공 하는 컨트롤을 [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) 라이브러리 이미지 로드를 다시 시도 하 여 일시적인 오류 처리를 지원 합니다. 이미지 로드에 실패 하면 더 이상의 시도가 됩니다. 지정 된 횟수를 `RetryCount` 속성을 다시 시도 하 여 지정 된 지연 후에 발생 합니다는 `RetryDelay` 속성. 이러한 속성 값 설정 되어 있지 않으면 명시적으로, 해당 기본 값에 적용 되는 – 3 경우 합니다 `RetryCount` 속성과 250ms는 `RetryDelay` 속성입니다. 에 대 한 자세한 내용은 합니다 `CachedImage` 제어를 참조 하십시오 [이미지 캐싱](#caching_images)합니다.

EShopOnContainers 참조 응용 프로그램에 다시 시도 패턴을 구현지 않습니다. 자세한 내용은 사용 하 여 다시 시도 패턴을 결합 하는 방법에 대 한 설명을 비롯 하 여 `HttpClient` 클래스를 참조 하십시오 [.NET 마이크로 서비스: 컨테이너 화 된.NET 응용 프로그램에 대 한 아키텍처](https://aka.ms/microservicesebook)합니다.

다시 시도 패턴에 대 한 자세한 내용은 참조는 [다시 시도](/azure/architecture/patterns/retry/) 패턴.

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>회로 차단기 패턴

상황에 따라 오류를 해결 하는 데 걸리는 예상 된 이벤트로 인해 발생할 수 있습니다. 이러한 오류는 서비스의 전체 오류에는 부분적 연결 손실에서 까지입니다. 이러한 상황에서는 성공 및 대신 해야 작업에 실패 했음을 받아들이고 그에 따라이 오류를 처리 되지 않는 작업을 다시 시도 하는 앱에 대 한 무의미 합니다.

회로 차단기 패턴을 반복적으로 오류가 해결 되었는지 여부를 검색 하는 앱을 사용 하도록 설정 하는 동안 실패할 가능성이 있는 작업을 실행 하려는 앱을 방지할 수 있습니다.

> [!NOTE]
> 회로 차단기 패턴의 목적은 재시도 패턴과 다릅니다. 다시 시도 패턴에는 성공 한다는 기대 하는 작업을 다시 시도 하는 앱 수 있습니다. 회로 차단기 패턴은 실패할 가능성이 있는 작업을 수행 앱을 방지 합니다.

회로 차단기는 실패할 수 있는 작업에 대 한 프록시로 작동 합니다. 프록시는 발생 한 최근 오류 수를 모니터링 하 고이 정보를 사용 하 여 작업을 계속 하려면 또는 예외를 즉시 반환할 수 있도록 할지 여부를 결정 해야 합니다.

EShopOnContainers 모바일 앱은 현재 회로 차단기 패턴을 구현 하지 않습니다. 그러나 eShopOnContainers 다음과 같은 작업을 수행 하지 않습니다. 자세한 내용은 [.NET 마이크로 서비스: 컨테이너 화 된.NET 응용 프로그램에 대 한 아키텍처](https://aka.ms/microservicesebook)합니다.

> [!TIP]
> 다시 시도 및 회로 차단기 패턴을 결합 합니다. 앱 다시 시도 패턴을 사용 하 여 회로 차단기를 통해 작업을 호출 하 여 다시 시도 및 회로 차단기 패턴을 결합할 수 있습니다. 그러나 재시도 논리는 회로 차단기에 의해 반환 되는 모든 예외에 중요할 수 하 고 회로 차단 기가 오류가 일시적 임을 나타내는 경우 재시도 중단 해야 합니다.

회로 차단기 패턴에 대 한 자세한 내용은 참조는 [회로 차단기](/azure/architecture/patterns/circuit-breaker/) 패턴입니다.

## <a name="summary"></a>요약

많은 최신 웹 기반 솔루션을 통해 응용 프로그램 원격 클라이언트에 대 한 기능을 제공 하기 위해 웹 서버에서 호스팅되는 웹 서비스를 사용 합니다. 웹 API를 구성 하는 웹 서비스를 노출 하는 작업 및 클라이언트 앱 데이터 또는 API를 노출 하는 작업이 구현 하는 방법을 알 필요 없이 web API를 사용할 수 있어야 합니다.

빠른 저장소에 가까이 있는 자주 액세스 하는 데이터를 캐시 하 여 앱의 성능을 향상 될 수 있습니다 앱. 앱은 캐시 배제 패턴을 사용 하 여 read-through 캐싱을 구현할 수 있습니다. 이 패턴의 항목이 캐시에 현재 인지 여부를 결정 합니다. 항목이 캐시에 없는 경우 데이터 저장소에서 읽은 후 캐시에 추가 합니다.

Web Api와 통신 하는 경우 앱 일시적인 오류에 민감해 야 합니다. 일시적인 오류는 서비스에 대 한 네트워크 연결 끊김, 서비스 또는 서비스 사용량이 많을 때 발생 하는 시간 제한의 일시적인 사용 중단을 포함 합니다. 이러한 오류는 자체적으로 수정, 자주 하는 경우 동작을 반복 하 여 적절 한 시간 후에 다음 성공할 가능성이 있습니다. 따라서 앱은 일시적인 오류 처리 메커니즘을 구현 하는 코드에서 web API에 액세스 하려는 모든 시도 래핑해야 합니다.


## <a name="related-links"></a>관련 링크

- [2mb PDF 전자책 다운로드](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
