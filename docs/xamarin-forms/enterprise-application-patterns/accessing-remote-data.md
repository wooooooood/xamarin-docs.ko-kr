---
title: 원격 데이터 액세스
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: f1e15d31f11c6845ad61882996f01fb16e80ed95
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="accessing-remote-data"></a>원격 데이터 액세스

많은 최신 웹 기반 솔루션을 통해 응용 프로그램 원격 클라이언트에 대 한 기능을 제공 하도록 웹 서버에 의해 호스팅되는 웹 서비스를 사용 합니다. 웹 서비스를 노출 하는 작업은 웹 API를 구성 합니다.

클라이언트 응용 프로그램 데이터 또는 API가 노출 하는 작업의 구현 방법을 몰라도 web API를 활용 하 여 수 있어야 합니다. 이 API 관련 하 여 클라이언트 응용 프로그램 및 웹 서비스 간에 교환 되는 데이터의 구조를 사용 하는 데이터 형식에 동의 하도록 클라이언트 응용 프로그램 및 웹 서비스를 활성화 하는 일반적인 표준 해야 합니다.

## <a name="introduction-to-representational-state-transfer"></a>Representational State Transfer 소개

REPRESENTATIONAL State Transfer ()은 하이퍼미디어를 기반으로 하는 분산된 시스템을 구축 하기 위한 아키텍처는 스타일입니다. 나머지 모델의 주요 이점은는 개방형 표준에 따라 모델 또는 다른 특정 구현에 액세스 하는 클라이언트 응용 프로그램의 구현이 바인딩하지 않는입니다. 따라서 Microsoft ASP.NET Core MVC를 사용 하 여 REST 웹 서비스를 구현할 수 있습니다 및 모든 언어와 HTTP 요청을 생성 하 고 HTTP 응답을 구문 분석할 수 있는 도구 집합을 사용 하 여 클라이언트 앱을 개발할 수 없습니다.

나머지 모델 리소스 라고 하는 네트워크를 통해 개체 및 서비스를 나타내기 위해 탐색 체계를 사용 합니다. 이러한 리소스에 액세스 하는 요청을 전송 하는 데 HTTP 프로토콜을 사용 하는 REST를 일반적으로 구현 하는 시스템입니다. 이러한 시스템에서는 클라이언트 응용 프로그램 리소스를 식별 하는 URI와 해당 리소스에 대해 수행할 연산을 지정 하는 HTTP 메서드 (예: GET, POST, PUT 또는 DELETE)의 형태로 요청을 제출 합니다. 작업을 수행 하는 데 필요한 모든 데이터를 포함 하는 HTTP 요청의 본문입니다.

> [!NOTE]
> REST는 상태 비저장 요청 모델을 정의합니다. 따라서 HTTP 요청 독립적 이어야 하며 순서에 관계 없이 발생할 수 있습니다.

나머지에서 응답 요청을 사용 하면 표준 HTTP 상태 코드를 사용 합니다. 예를 들어 찾기 또는 지정된 된 리소스를 삭제 하는 요청 HTTP 상태 코드 404 (찾을 수 없음)를 포함 하는 응답을 반환 해야 하는 동안 올바른 데이터를 반환 하는 요청의 HTTP 응답 코드 200 (정상)에 포함 되어야 합니다.

API는 RESTful 웹 연결 된 리소스 집합을 노출 하 고 앱이 이러한 리소스를 조작 하 고 쉽게 간을 탐색할 수 있도록 하는 핵심 작업을 제공 합니다. 이러한 이유로 일반 RESTful 웹 API를 구성 하는 Uri는을 향하고, 노출 된 데이터 및 사용 하는 기능을 제공 하 여이 데이터에서 작동 하는 HTTP입니다.

에 다양 한 형식의 미디어 유형으로는 HTTP 요청 및 웹 서버에서 해당 응답 메시지에 클라이언트 앱에서 포함 된 데이터를 제공할 수 수 없습니다. 처리할 수 있는 미디어 종류를 지정할 수 있는 클라이언트 응용 프로그램 메시지의 본문에 데이터를 반환 하는 요청을 보내면는 `Accept` 요청 헤더입니다. 웹 서버에서이 미디어 유형의 지 원하는 경우 포함 하는 응답으로 응답할 수 있습니다는 `Content-Type` 메시지의 본문에 있는 데이터의 형식을 지정 하는 헤더입니다. 하는 응답 메시지를 구문 분석 하 고 메시지 본문에 결과 적절 하 게 해석 하는 클라이언트 응용 프로그램의 책임입니다.

REST에 대 한 자세한 내용은 참조 [API 디자인](/azure/architecture/best-practices/api-design/) 및 [API 구현](/azure/architecture/best-practices/api-implementation/)합니다.

## <a name="consuming-restful-apis"></a>RESTful Api를 사용합니다.

EShopOnContainers 모바일 응용 프로그램 모델-뷰-MVVM () 패턴 및 도메인 엔터티는 응용 프로그램에서 사용 되는 패턴 나타내며의 모델 요소를 사용 합니다. EShopOnContainers 참조 응용 프로그램의 컨트롤러 및 리포지토리 클래스 받아들이고 여러 이러한 모델 개체를 반환 합니다. 모바일 앱과 컨테이너 화 된 microservices 간에 전달 되는 모든 데이터를 포함 하는 데이터 전송 개체 (Dto)으로 사용 됩니다. 주요 장점은 Dto를 사용 하 여 데이터를 전달 하 고 웹 서비스에서 데이터를 수신 하는 단일 원격 호출에 더 많은 데이터를 전송 하 여 응용 프로그램 수를 줄일 수 수행 되어야 하는 원격 호출의입니다.

### <a name="making-web-requests"></a>웹 요청 수행

EShopOnContainers 모바일 앱 사용 하 여는 `HttpClient` json을 미디어 유형을 사용 하 고 HTTP를 통해 요청을 만들기 위해 클래스입니다. 이 클래스는 비동기적으로 HTTP 요청을 전송에 대 한 기능을 제공 하 고 리소스를 식별 하는 URI에서 HTTP 응답을 받기. `HttpResponseMessage` 클래스 HTTP 요청이 수행 된 후에 REST API에서 받은 HTTP 응답 메시지를 나타냅니다. 상태 코드, 헤더 및 모든 본문을 포함 하 여 응답에 대 한 정보를 포함 합니다. `HttpContent` 클래스를 나타내는 HTTP 본문 및 콘텐츠 헤더와 같은 `Content-Type` 및 `Content-Encoding`합니다. 사용 하 여 콘텐츠를 읽을 수는 `ReadAs` 메서드 같은 `ReadAsStringAsync` 및 `ReadAsByteArrayAsync`데이터의 형식에 따라 합니다.

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>GET 요청을 만드는

`CatalogService` 클래스 카탈로그 마이크로 서비스에서 데이터 검색 프로세스를 관리 하는 데 사용 됩니다. 에 `RegisterDependencies` 에서 메서드는 `ViewModelLocator` 클래스는 `CatalogService` 클래스에 대 한 형식 매핑을으로 등록 됩니다는 `ICatalogService` Autofac 종속성 주입 컨테이너 형식입니다. 다음의 인스턴스는 `CatalogViewModel` 허용 생성자 클래스를 만들는 `ICatalogService` 입력의 인스턴스를 반환 Autofac 확인 되 면는 `CatalogService` 클래스. 종속성 주입 하는 방법에 대 한 자세한 내용은 참조 [종속성 주입 소개](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)합니다.

그림 10-1에서 표시 하기 위한 카탈로그 마이크로 서비스에서 카탈로그 데이터를 읽는 클래스와의 상호 작용을 보여 줍니다.는 `CatalogView`합니다.

[![](accessing-remote-data-images/catalogdata.png "카탈로그 마이크로 서비스에서 데이터를 검색할")](accessing-remote-data-images/catalogdata-large.png#lightbox "카탈로그 마이크로 서비스에서 데이터 검색")

**그림 10-1**: 카탈로그 마이크로 서비스에서 데이터 검색

경우는 `CatalogView` 탐색 되는 `OnInitialize` 에서 메서드는 `CatalogViewModel` 클래스 라고 합니다. 이 메서드는 다음 코드 예제에서와 같이 카탈로그 마이크로 서비스에서 카탈로그 데이터를 검색 합니다.

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

이 메서드를 호출는 `GetCatalogAsync` 의 메서드는 `CatalogService` 에 삽입 된 인스턴스는 `CatalogViewModel` Autofac 여 합니다. 다음 코드 예제는 `GetCatalogAsync` 메서드:

```csharp
public async Task<ObservableCollection<CatalogItem>> GetCatalogAsync()  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.CatalogEndpoint);  
    builder.Path = "api/v1/catalog/items";  
    string uri = builder.ToString();  

    CatalogRoot catalog = await _requestProvider.GetAsync<CatalogRoot>(uri);  
    ...  
    return catalog?.Data.ToObservableCollection();            
}
```

이 메서드, 요청을 받게 될 리소스를 식별 하는 URI를 만들고 사용 하 여 `RequestProvider` 결과를 반환 하기 전에 리소스에 대해 GET HTTP 메서드를 호출 하는 클래스는 `CatalogViewModel`합니다. `RequestProvider` 작업을 수행 하는 데 필요한 모든 데이터를 포함 하는 본문 및 클래스 리소스에 해당 리소스에 수행할 작업을 나타내는 HTTP 메서드를 식별 하는 URI의 형태로 요청을 제출 하는 기능이 포함 되어 있습니다. 방법에 대 한 내용은 `RequestProvider` 클래스에 삽입 되는 `CatalogService class`, 참조 [종속성 주입 소개](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)합니다.

다음 코드 예제는 `GetAsync` 에서 메서드는 `RequestProvider` 클래스:

```csharp
public async Task<TResult> GetAsync<TResult>(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    HttpResponseMessage response = await httpClient.GetAsync(uri);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>   
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

이 메서드를 호출는 `CreateHttpClient` 의 인스턴스를 반환 하는 `HttpClient` 적절 한 헤더 설정 클래스입니다. 다음에 저장 되는 응답과 함께 URI로 식별 되는 리소스에는 비동기 GET 요청을 제출는 `HttpResponseMessage` 인스턴스. `HandleResponse` 메서드가 호출 되 응답 성공 HTTP 상태 코드에 포함 되지 않은 경우는 예외를 throw 합니다. 다음 응답은 JSON에서 변환 된 문자열로 읽습니다는 `CatalogRoot` 개체를 반환 하는 `CatalogService`합니다.

`CreateHttpClient` 메서드는 다음 코드 예제에 표시 됩니다.

```csharp
private HttpClient CreateHttpClient(string token = "")  
{  
    var httpClient = new HttpClient();  
    httpClient.DefaultRequestHeaders.Accept.Add(  
        new MediaTypeWithQualityHeaderValue("application/json"));  

    if (!string.IsNullOrEmpty(token))  
    {  
        httpClient.DefaultRequestHeaders.Authorization =   
            new AuthenticationHeaderValue("Bearer", token);  
    }  
    return httpClient;  
}
```

이 메서드는의 새 인스턴스를 만듭니다는 `HttpClient` 클래스 및 설정의 `Accept` 하 여 만든 모든 요청 헤더는 `HttpClient` 인스턴스를 `application/json`, JSON을 사용 하 여 형식이 지정에 대 한 모든 응답의 콘텐츠를 필요 함을 나타냅니다. 액세스 토큰에 대 한 인수로 전달 된 경우에 다음의 `CreateHttpClient` 에 추가 된 메서드를는 `Authorization` 하 여 만든 모든 요청 헤더는 `HttpClient` 문자열을 접두사로 인스턴스 `Bearer`합니다. 권한 부여에 대 한 자세한 내용은 참조 [권한 부여](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)합니다.

경우는 `GetAsync` 에서 메서드는 `RequestProvider` 클래스가 호출 `HttpClient.GetAsync`, `Items` 에서 메서드는 `CatalogController` Catalog.API 프로젝트에서 클래스를 호출한 다음 코드 예제에 나와 있는:

```csharp
[HttpGet]  
[Route("[action]")]  
public async Task<IActionResult> Items(  
    [FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)  
{  
    var totalItems = await _catalogContext.CatalogItems  
        .LongCountAsync();  

    var itemsOnPage = await _catalogContext.CatalogItems  
        .OrderBy(c=>c.Name)  
        .Skip(pageSize * pageIndex)  
        .Take(pageSize)  
        .ToListAsync();  

    itemsOnPage = ComposePicUri(itemsOnPage);  
    var model = new PaginatedItemsViewModel<CatalogItem>(  
        pageIndex, pageSize, totalItems, itemsOnPage);             

    return Ok(model);  
}
```

이 메서드는 EntityFramework를 사용 하 여 SQL 데이터베이스에서 카탈로그 데이터를 검색 하 고 HTTP 상태 코드, 성공 포함 된 응답 메시지로 반환 하 고 JSON의 컬렉션 형식 `CatalogItem` 인스턴스.

#### <a name="making-a-post-request"></a>POST 요청을 만드는

`BasketService` 클래스는 데이터 검색을 관리 하 고 장바구니 마이크로 서비스와 프로세스를 업데이트 하는 데 사용 됩니다. 에 `RegisterDependencies` 에서 메서드는 `ViewModelLocator` 클래스는 `BasketService` 클래스에 대 한 형식 매핑을으로 등록 됩니다는 `IBasketService` Autofac 종속성 주입 컨테이너 형식입니다. 다음의 인스턴스는 `BasketViewModel` 허용 생성자 클래스를 만들는 `IBasketService` 입력의 인스턴스를 반환 Autofac 확인 되 면는 `BasketService `클래스. 종속성 주입 하는 방법에 대 한 자세한 내용은 참조 [종속성 주입 소개](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection)합니다.

그림 10-2에 표시 된 바구니 데이터를 전송 하는 클래스와의 상호 작용을 보여 줍니다.는 `BasketView`, 바구니 마이크로 서비스를 합니다.

[![](accessing-remote-data-images/basketdata.png "데이터 바구니 마이크로 서비스를 보내는")](accessing-remote-data-images/basketdata-large.png#lightbox "바구니 마이크로 서비스에 데이터 보내기")

**그림 10-2**: 바구니 마이크로 서비스에 데이터 보내기

시장 바구니에 항목 추가 될 때는 `ReCalculateTotalAsync` 에서 메서드는 `BasketViewModel` 클래스 라고 합니다. 이 메서드는은 시장 바구니에 있는 항목의 합계 값을 업데이트 하 고 다음 코드 예제에서와 같이 바구니 데이터 바구니 마이크로 서비스를 보냅니다.

```csharp
private async Task ReCalculateTotalAsync()  
{  
    ...  
    await _basketService.UpdateBasketAsync(new CustomerBasket  
    {  
        BuyerId = userInfo.UserId,   
        Items = BasketItems.ToList()  
    }, authToken);  
}
```

이 메서드를 호출는 `UpdateBasketAsync` 의 메서드는 `BasketService` 에 삽입 된 인스턴스는 `BasketViewModel` Autofac 여 합니다. 에서는 다음 메서드는 `UpdateBasketAsync` 메서드:

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

이 메서드, 요청을 받게 될 리소스를 식별 하는 URI를 만들고 사용 하 여 `RequestProvider` 결과를 반환 하기 전에 리소스에서 POST HTTP 메서드를 호출 하는 클래스는 `BasketViewModel`합니다. 액세스 토큰을 인증 프로세스 동안 IdentityServer에서 얻은 참고는 바구니 마이크로 서비스에 대 한 요청에 권한을 부여 해야 합니다. 권한 부여에 대 한 자세한 내용은 참조 [권한 부여](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)합니다.

다음 코드 예제에서는 중 하나는 `PostAsync` 의 메서드는 `RequestProvider` 클래스:

```csharp
public async Task<TResult> PostAsync<TResult>(  
    string uri, TResult data, string token = "", string header = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    ...  
    var content = new StringContent(JsonConvert.SerializeObject(data));  
    content.Headers.ContentType = new MediaTypeHeaderValue("application/json");  
    HttpResponseMessage response = await httpClient.PostAsync(uri, content);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>  
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

이 메서드를 호출는 `CreateHttpClient` 의 인스턴스를 반환 하는 `HttpClient` 적절 한 헤더 설정 클래스입니다. 다음 JSON 형식으로 저장 되 고 응답에 전송 되는 바구니 serialize 된 데이터와는 URI로 식별 되는 리소스에는 비동기 POST 요청을 제출는 `HttpResponseMessage` 인스턴스. `HandleResponse` 메서드가 호출 되 응답 성공 HTTP 상태 코드에 포함 되지 않은 경우는 예외를 throw 합니다. 그런 다음 응답에 JSON에서 변환 된 문자열로 읽기는 `CustomerBasket` 개체를 반환 하는 `BasketService`합니다. 에 대 한 자세한 내용은 `CreateHttpClient` 메서드를 참조 [GET 요청을 만드는](#making_a_get_request)합니다.

경우는 `PostAsync` 에서 메서드는 `RequestProvider` 클래스가 호출 `HttpClient.PostAsync`, `Post` 에서 메서드는 `BasketController` Basket.API 프로젝트에서 클래스를 호출한 다음 코드 예제에 나와 있는:

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

이 방법은 사용의 인스턴스는 `RedisBasketRepository` 클래스 Redis cache 바구니 데이터를 유지 하 고 성공 HTTP 상태 코드 및 JSON을 포함 하는 응답 메시지에 형식이 지정 된 반환 `CustomerBasket` 인스턴스.

#### <a name="making-a-delete-request"></a>DELETE 요청을 만드는

그림 10-3에 대 한 바구니 데이터 바구니 마이크로 서비스를 삭제 하는 클래스의 상호 작용을 보여 줍니다.는 `CheckoutView`합니다.

![](accessing-remote-data-images/checkoutdata.png "바구니 마이크로 서비스에서 데이터 삭제")

**그림 10-3**: 바구니 마이크로 서비스에서 데이터 삭제

체크 아웃 프로세스를 호출 하면는 `CheckoutAsync` 에서 메서드는 `CheckoutViewModel` 클래스 라고 합니다. 이 메서드는 다음 코드 예제에서와 같이 시장 바구니 지우기 전에 새로운 순서를 만듭니다.

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

이 메서드를 호출는 `ClearBasketAsync` 의 메서드는 `BasketService` 에 삽입 된 인스턴스는 `CheckoutViewModel` Autofac 여 합니다. 에서는 다음 메서드는 `ClearBasketAsync` 메서드:

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

이 메서드, 요청이 전송 되는 리소스를 식별 하는 URI을 만들고 사용 하 여 `RequestProvider` 클래스 리소스에서 DELETE HTTP 메서드를 호출 합니다. 액세스 토큰을 인증 프로세스 동안 IdentityServer에서 얻은 참고는 바구니 마이크로 서비스에 대 한 요청에 권한을 부여 해야 합니다. 권한 부여에 대 한 자세한 내용은 참조 [권한 부여](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization)합니다.

다음 코드 예제는 `DeleteAsync` 에서 메서드는 `RequestProvider` 클래스:

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

이 메서드를 호출는 `CreateHttpClient` 의 인스턴스를 반환 하는 `HttpClient` 적절 한 헤더 설정 클래스입니다. 다음 URI로 식별 되는 리소스에 비동기 DELETE 요청을 제출 합니다. 에 대 한 자세한 내용은 `CreateHttpClient` 메서드를 참조 [GET 요청을 만드는](#making_a_get_request)합니다.

경우는 `DeleteAsync` 에서 메서드는 `RequestProvider` 클래스가 호출 `HttpClient.DeleteAsync`, `Delete` 에서 메서드는 `BasketController` Basket.API 프로젝트에서 클래스를 호출한 다음 코드 예제에 나와 있는:

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

이 방법은 사용의 인스턴스는 `RedisBasketRepository` 클래스 Redis cache에서 바구니 데이터를 삭제 합니다.

## <a name="caching-data"></a>데이터 캐싱

근접 있는 빠른 저장소에 자주 액세스 하는 데이터를 캐시 하 여 응용 프로그램의 성능이 향상 될 수 있습니다는 앱입니다. 빠른 저장소 원래 소스 응용 프로그램에 가깝게 위치 경우 캐싱 크게 향상 시킬 수 응답 데이터를 검색할 때 제한 시간이 있습니다.

캐시의 가장 일반적인 형태는 read-through 캐싱 응용 프로그램 캐시를 참조 하 여 데이터를 검색 합니다. 데이터를 캐시에 없는 데이터 저장소에서 검색 있으며 캐시에 추가 합니다. 앱은 read-through 캐싱을 캐시 배제 패턴을 구현할 수 있습니다. 이 패턴 항목이 캐시에 현재 인지 여부를 결정 합니다. 항목을 캐시에 없는 경우 데이터 저장소에서 읽기 및 캐시에 추가 했습니다. 자세한 내용은 참조는 [캐시 배제](/azure/architecture/patterns/cache-aside/) 패턴입니다.

> [!TIP]
> 자주 읽혀집니다 및 자주 변경 하는 데이터를 캐시 합니다. 이 데이터는 필요할 때 처음으로 응용 프로그램에서 검색 된 캐시에 추가할 수 있습니다. 즉, 응용 프로그램이 데이터 저장소에서 데이터를 한 번만를 가져올 해야 고 후속 액세스 하는 캐시를 사용 하 여 충족 될 수 있습니다.

분산된 응용 프로그램은 eShopOnContainers 참조 응용 프로그램을 같은 제공 해야 하나 또는 둘 다 다음 캐시 중:

-   여러 프로세스 또는 컴퓨터에서 액세스할 수 있는 공유 캐시 합니다.
-   응용 프로그램을 실행 하는 장치에서 데이터 로컬로 하 게 유지 하 되 개인 캐시 합니다.

EShopOnContainers 모바일 응용 프로그램 데이터는 앱의 인스턴스를 실행 하는 장치에서 로컬로 유지 되는 개인 캐시를 사용 합니다. EShopOnContainers 참조 응용 프로그램에 사용 된 캐시에 대 한 정보를 참조 하십시오. [.NET Microservices:.NET 응용 프로그램의 컨테이너 화 된 아키텍처](https://aka.ms/microservicesebook)합니다.

> [!TIP]
> 언제 든 지 사라질 수 있는 임시 데이터 저장소로 캐시 생각 됩니다. 데이터 캐시를 비롯 하 여 원래 데이터 저장소에 유지 되도록 보장 합니다. 캐시를 사용할 수 없게 하는 경우에 다음의 경우 데이터 손실 가능성 최소화 됩니다.

### <a name="managing-data-expiration"></a>데이터 만료 관리

하지 한 될 캐시 된 데이터는 항상 원래 데이터와 일치 하는 것이 유용 합니다. 원래 데이터 저장소의 데이터는 것은 캐시 된 경우, 캐시 된 데이터가 유효 하지 않게 할 후 변경 될 수 있습니다. 따라서 응용 프로그램 해야 캐시의 데이터를 최대한으로 최신 인지 확인 하는 데 도움이 되는 전략을 구현 하지만 수 검색 있고 캐시의 데이터 사본이 부실한 때 발생 하는 상황을 처리 합니다. 가장 캐싱 메커니즘 데이터를 만료 하도록 구성 캐시를 사용 하 고 따라서 데이터가 만료 되었을 기간을 줄입니다.

> [!TIP]
> 기본 만료를 설정 합니다. 캐시를 구성할 때 시간이 있습니다. 많은 캐시 데이터를 무효화 하 고 지정 된 기간에 대 한 액세스 되지 않으면 캐시에서 제거 하는 만료를 구현 합니다. 그러나 주의 해야 만료 기간을 선택할 때. 너무 짧은 만들어질 경우 데이터가 너무 빨리 만료 되 고 캐싱의 효율성은 저하 됩니다. 만들어진 경우에 너무 오래 부실 데이터 위험 합니다. 따라서 만료 시간에 데이터를 사용 하는 앱에 대 한 액세스 패턴을 일치 해야 합니다.

캐시 된 데이터가 만료는 캐시에서 제거 해야 하 고 응용 프로그램을 검색 해야 하는 경우 데이터를 원래 데이터에서에서 저장 하 고 캐시에 다시 놓습니다.

것도 가능 데이터는 너무 오랫동안 유지 하도록 허용 된 경우 캐시 채울 수 있습니다. 따라서 요청을 캐시에 새 항목을 추가 해야 할 수 라고 하는 프로세스에서 일부 항목을 삭제 *eviction*합니다. 캐싱 서비스는 일반적으로 가장 최근에 사용한으로 데이터를 제거합니다. 그러나 가장 최근에 사용한, 및 선입 선출 포함 하 여 다른 제거 정책 있습니다. 자세한 내용은 참조 [캐싱 지침](/azure/architecture/best-practices/caching/)합니다.

<a name="caching_images" />

### <a name="caching-images"></a>이미지 캐싱

EShopOnContainers 모바일 앱을 캐시할을 활용 하는 원격 제품 이미지를 사용 합니다. 이러한 이미지도 표시 됩니다는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 컨트롤을 및 `CachedImage` 컨트롤에서 제공 되는 [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) 라이브러리.

Xamarin.Forms는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 컨트롤이 다운로드 되는 이미지의 캐싱을 지원 합니다. 캐싱은 기본적으로 사용 하도록 설정 하 고 24 시간 동안 로컬로 이미지를 저장 합니다. 또한 된 만료 시간을 구성할 수 있습니다는 [ `CacheValidity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) 속성입니다. 자세한 내용은 참조 [다운로드 한 이미지 캐싱](~/xamarin-forms/user-interface/images.md#Image_Caching)합니다.

FFImageLoading의 `CachedImage` 컨트롤은 Xamarin.Forms는에 대 한 대체 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 보충 기능을 사용 하는 추가 속성을 제공 하는 컨트롤입니다. 이 기능을 적절 한 컨트롤 오류를 지원 하 고 이미지 자리 표시자를 로드 하는 동안 구성 가능한 캐싱을 제공 합니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱 사용 하는 방법을 보여 줍니다.는 `CachedImage` 컨트롤에 `ProductTemplate`되는 데이터 서식 파일에서 사용 하는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 컨트롤에 `CatalogView`:

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

`CachedImage` 설정 제어는 `LoadingPlaceholder` 및 `ErrorPlaceholder` 플랫폼 특정 이미지에는 속성입니다. `LoadingPlaceholder` 속성으로 지정 된 이미지는 동안 표시할 이미지를 지정 된 `Source` 속성을 검색 및 `ErrorPlaceholder` 이미지를 검색 하려고 할 때 오류가 발생 하는 경우 표시할 이미지를 지정 하는 속성 에 지정 된는 `Source` 속성입니다.

이름에서 알 수 있듯이 `CachedImage` 컨트롤의 값에 의해 지정 된 시간에 대 한 장치에서 원격 이미지에서 캐시는 `CacheDuration` 속성입니다. 이 속성 값이 명시적으로 설정 되지 않은 경우 기본값인 30 일 동안 적용 됩니다.

## <a name="increasing-resilience"></a>탄력성 증가

원격 서비스와 리소스와 통신 하는 모든 앱은 일시적인 오류를 주의 해야 합니다. 일시적인 오류는 쿼리에 일시적으로 서비스에 네트워크 연결이 손실, 서비스 또는 서비스를 사용 중인 경우 발생 하는 시간 제한의 일시적으로 사용할 수 있습니다. 이러한 오류는 자체적으로 수정, 종종 되며 동작 적합 한 지연 후 반복 되 면 성공할 수 있습니다.

일시적인 오류 철저 하 게 예측 가능한 모든 환경에서 테스트 된 경우에 응용 프로그램의 체감된 품질에 큰 영향을 미칠 수 있습니다. 원격 서비스와 통신 하는 응용 프로그램을 안정적으로 작동 되도록는 다음과 같은 작업을 수행 하는 것이 수 있어야 합니다.

-   발생 하 고 오류가 발생 하는 일시적인 것으로 가능성이 결정 될 때 오류를 검색 합니다.
-   오류 일시적일 수와 한 후 작업을 다시 시도 횟수를 추적 것으로 예상 되어 있다고 판단할 경우 작업을 다시 시도 합니다.
-   시도가 실패 한 후 수행 하는 각 시도 작업 간 지연, 다시 시도 횟수를 지정 하는 적절 한 다시 시도 전략을 사용 합니다.

원격 서비스 재시도 패턴을 구현 하는 코드에 액세스 하려는 모든 시도 래핑하고이 일시적인 오류 처리를 구현할 수 있습니다.

### <a name="retry-pattern"></a>패턴을 다시 시도 하십시오.

원격 서비스에는 요청을 보내려고 시도할 때 오류를 감지 하는 응용 프로그램, 다음과 같은 방법으로 오류를 처리할 수 있습니다.:

-   작업을 다시 시도 합니다. 응용 프로그램에 즉시 결함이 있는 요청을 다시 수 없습니다.
-   잠시 후 작업을 다시 시도 합니다. 응용 프로그램 요청을 다시 시도 하기 전에 적절 한 기간에 대해 기다려야 합니다.
-   작업을 취소 합니다. 응용 프로그램 작업을 취소 하 고 예외를 보고 해야 합니다.

응용 프로그램의 비즈니스 요구 사항에 맞게 다시 시도 전략을 조절 해야 합니다. 예를 들어는 재시도 횟수를 최적화 하 고 다시 시도 간격을 시도 하 고 작업을 하는 작업을 해야 합니다. 작업이 사용자 상호 작용의 일부 이면 장 /만 몇 번 다시 시도 되지 않도록 하기 위해 응답을 대기 하는 사용자가 다시 시도 간격 이어야 합니다. 작업의 일부인 경우 장기 실행 워크플로 여기서 취소 하거나 다시 시작은 비용이 많이 드는 또는 시간이 많이 걸리는, 적절 한 번 더 다시 시도 및 더 이상 시도 간에 대기 하는 합니다.

> [!NOTE]
> 적극적인 다시 시도 전략 횟수와 많은 수의 재시도 최소 간격을 두고 용량에 근접 했거나 실행 되는 원격 서비스로 저하 시킬 수 있습니다. 또한 다시 시도 전략도 영향을 줄 수는 앱의 응답성 지속적으로 실패 한 작업을 수행 하려고 시도 하는 경우.

요청 된 수의 재시도 후 계속 실패 하는 경우 앱 같은 리소스를 추가 요청을 방지 하 고 오류가 보고 하는 것이 좋습니다. 그런 다음 정해진된 기간 이후에 응용 프로그램을 수행할 수 하나 이상의 요청 성공 고 리소스. 자세한 내용은 참조 [회로 차단기 패턴](#circuit_breaker_pattern)합니다.

> [!TIP]
> 무한 재시도 메커니즘을 구현 하지 않습니다. 한정 된 수의 재시도 사용 하거나 구현는 [회로 차단기](/azure/architecture/patterns/circuit-breaker/) 서비스가 복구할 수 있도록 하는 패턴입니다.

RESTful 웹 요청을 만들 때 eShopOnContainers 모바일 앱 재시도 패턴을 현재 구현 하지 않습니다. 그러나는 `CachedImage` 에서 제공 하는 컨트롤의 [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) 라이브러리 이미지 로드를 다시 시도 하 여 일시적인 오류 처리를 지원 합니다. 이미지 로드 실패 하면 더 이상 시도 되지 것입니다. 지정 된 시도 횟수는 `RetryCount` 속성을 다시 시도 횟수로 지정 된 지연 후 발생 합니다는 `RetryDelay` 속성입니다. 이러한 속성 값이 설정 명시적으로 아닌 값에 적용 되는 – 3에 대 한 기본은 `RetryCount` 속성과 대 한 증가가 `RetryDelay` 속성입니다. 에 대 한 자세한 내용은 `CachedImage` 제어, 참조 [이미지 캐싱](#caching_images)합니다.

EShopOnContainers 참조 응용 프로그램은 재시도 패턴을 구현 합니다. 를 비롯 한 자세한 내용은 사용 하 여 재시도 패턴을 결합 하는 방법에 대 한 설명은 `HttpClient` 클래스를 참조 하십시오. [.NET Microservices:.NET 응용 프로그램의 컨테이너 화 된 아키텍처](https://aka.ms/microservicesebook)합니다.

재시도 패턴에 대 한 자세한 내용은 참조는 [을 다시 시도](/azure/architecture/patterns/retry/) 패턴입니다.

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>회로 차단기 패턴

경우에 따라 오류를 해결 하는 데 걸리는 예상된 이벤트로 인해 발생할 수 있습니다. 이러한 오류는 서비스의 전체 오류 연결 부분 손실이에서 달라질 수 있습니다. 이 이러한 경우에는 응용 프로그램 성공 하 고 대신가 허용 작업이 실패 했습니다 하 고 적절 하 게이 오류를 처리할 가능성이 있는 작업을 다시 시도 하는 데 무의미 합니다.

회로 차단기 패턴이 반복적으로 응용 프로그램에서 오류 해결 되었음을 검색할를 활성화 하는 동안 실패할 가능성이 있는 작업을 실행 하는 응용 프로그램을 방지할 수 있습니다.

> [!NOTE]
> 회로 차단기 패턴의 목적은 재시도 패턴 간에 차이가 있습니다. 재시도 패턴에는 앱을 성공할 것 이란 가정 하에서 작업을 다시 시도 수 있습니다. 회로 차단기 패턴에서 실패할 가능성이 하는 작업을 수행 합니다. 응용 프로그램을 방지 합니다.

회로 차단기 실패할 수 있는 작업에 대 한 프록시 역할을 합니다. 프록시 발생 한 최근 실패 수를 모니터링 하 고이 정보를 사용 하 여 작업을 계속 하려면 또는 예외를 즉시 반환할 수 있도록 할지 여부를 결정 해야 합니다.

EShopOnContainers 모바일 앱 현재 회로 차단기 패턴을 구현 하지 않습니다. 그러나는 eShopOnContainers 다음과 같은 작업을 수행 하지 않습니다. 자세한 내용은 참조 [.NET Microservices:.NET 응용 프로그램의 컨테이너 화 된 아키텍처](https://aka.ms/microservicesebook)합니다.

> [!TIP]
> 다시 시도 및 회로 차단기 패턴을 결합 합니다. 앱은 재시도 패턴을 사용 하 여 회로 차단기를 통해 작업을 호출 하 여 다시 시도 및 회로 차단기 패턴을 결합할 수 있습니다. 그러나 재시도 논리 회로 차단기에서 반환 되는 모든 예외에 민감한와 회로 차단기 오류가 일시적이 지를 나타내는 경우 재시도 중단 합니다.

회로 차단기 패턴에 대 한 자세한 내용은 참조는 [회로 차단기](/azure/architecture/patterns/circuit-breaker/) 패턴입니다.

## <a name="summary"></a>요약

많은 최신 웹 기반 솔루션을 통해 응용 프로그램 원격 클라이언트에 대 한 기능을 제공 하도록 웹 서버에 의해 호스팅되는 웹 서비스를 사용 합니다. Web API를 구성 하는 웹 서비스를 노출 하는 작업 및 클라이언트 응용 프로그램 데이터 또는 API가 노출 하는 작업의 구현 방법을 몰라도 web API를 활용 하 여 수 있어야 합니다.

근접 있는 빠른 저장소에 자주 액세스 하는 데이터를 캐시 하 여 응용 프로그램의 성능이 향상 될 수 있습니다는 앱입니다. 앱은 read-through 캐싱을 캐시 배제 패턴을 구현할 수 있습니다. 이 패턴 항목이 캐시에 현재 인지 여부를 결정 합니다. 항목을 캐시에 없는 경우 데이터 저장소에서 읽기 및 캐시에 추가 했습니다.

웹 Api 통신할 때 앱이 일시적 오류에 민감한 이어야 합니다. 일시적인 오류는 쿼리에 일시적으로 서비스에 네트워크 연결이 손실, 서비스 또는 서비스를 사용 중인 경우 발생 하는 시간 제한의 일시적으로 사용할 수 있습니다. 이러한 오류는 자체적으로 수정, 종종 되며 동작이 적합 한 지연 시간 이후 반복은, 하는 경우 다음 성공할 수 있습니다. 따라서 앱을 일시적인 오류 처리 메커니즘을 구현 하는 코드의 웹 API에 액세스 하려는 모든 시도 래핑해야 합니다.


## <a name="related-links"></a>관련 링크

- [EBook (2mb PDF)를 다운로드 합니다.](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
