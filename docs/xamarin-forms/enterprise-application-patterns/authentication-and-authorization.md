---
title: 인증 및 권한 부여
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: 9c6f3ae19b3e1b89220cbdf0985f4bdf789f2209
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="authentication-and-authorization"></a>인증 및 권한 부여

인증은 사용자 로부터 id 자격 증명 이름 및 암호 등을 가져오기 고 기관에 대해 해당 자격 증명 유효성을 검사 하는 과정입니다. 자격 증명이 유효한 자격 증명을 제출 하는 엔터티 인증된 된 id로 간주 됩니다. Id가 인증 되 면 권한 부여 프로세스는 해당 id가 지정된 된 리소스에 액세스할 수 있는지 여부를 결정 합니다.

ASP.NET Core Id, Google, Microsoft와 같은 외부 인증 공급자를 사용 하는 등 ASP.NET MVC 웹 응용 프로그램와 통신 하는 Xamarin.Forms 앱에 인증 및 권한 부여를 통합 하는 방법은 여러 가지가 Facebook 또는 Twitter 및 인증 미들웨어입니다. EShopOnContainers 모바일 앱을 인증 및 권한 부여 IdentityServer 4를 사용 하는 컨테이너 화 된 identity 마이크로 서비스를 수행 합니다. 모바일 앱 또는 사용자를 인증에 대 한 리소스에 액세스 하기 위한 IdentityServer에서 보안 토큰을 요청 합니다. 사용자 대신 토큰을 발급 하려면 IdentityServer에 대 한 사용자 해야 로그인 IdentityServer 합니다. 그러나 IdentityServer 인증에 대 한 사용자 인터페이스 또는 데이터베이스를 제공 하지 않습니다. 따라서 eShopOnContainers 참조 응용 프로그램에서 ASP.NET Core Id는이 목적을 위해 사용 됩니다.

## <a name="authentication"></a>인증

응용 프로그램이 현재 사용자의 id를 알고 있어야 하는 경우 인증이 필요 합니다. ASP.NET Core 사용자를 식별 하기 위한 기본 메커니즘은 개발자가 구성한 데이터 저장소에 사용자 정보를 저장 하는 ASP.NET Core Id 멤버 자격 시스템입니다. 일반적으로 사용자 지정 저장소 또는 타사 패키지를 Azure 저장소, Azure Cosmos DB 또는 다른 위치에 id 정보를 저장 하려면 사용할 수 있지만이 데이터 저장소는 EntityFramework 저장소가 됩니다.

로컬 사용자 데이터 저장소를 구성 하는 인증 시나리오에서 사용 하 고 쿠키 (그대로 ASP.NET MVC 웹 응용 프로그램에서 일반적인)를 통해 요청 간에 id 정보를 유지 하는, ASP.NET Core Id 적합 한 솔루션입니다. 그러나 쿠키는 하지 항상 유지 하 고 데이터를 전송 하는 자연 스러운 방법. 예를 들어, 모바일 앱에서 액세스할 수 있는 RESTful 끝점을 노출 하는 ASP.NET Core 웹 응용 프로그램을 일반적으로이 시나리오에서 쿠키를 사용할 수 있으므로 전달자 토큰 인증을 사용 하도록 할 수 있습니다. 그러나 전달자 토큰 수 쉽게 검색 하 고 모바일 앱에서 만든 웹 요청 권한 부여 헤더에 포함 되어 있습니다.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>IdentityServer 4를 사용 하 여 전달자 토큰을 발급

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) 로컬 ASP.NET Core Id 사용자에 대 한 보안 토큰을 발행 비롯 한 대부분의 인증 및 권한 부여 시나리오에 사용할 수 있는 ASP.NET Core 오픈 소스 OAuth 2.0 및 OpenID Connect 프레임 워크입니다.

> [!NOTE]
> OAuth 2.0 및 OpenID Connect는 서로 다른 책임 않고 매우 비슷합니다.

OpenID Connect는 OAuth 2.0 프로토콜을 맨 위에 인증 계층입니다. OAuth 2는 응용 프로그램 보안 토큰 서비스에서 액세스 토큰을 요청 하 고 통신 하는 api 사용을 허용 하는 프로토콜입니다. 이 위임 인증 및 권한 부여를 집중 시킬 수 있으므로 클라이언트 응용 프로그램과 Api의 복잡성을 줄일 수 있습니다.

API 액세스 및 인증의 두 가지 기본 보안 문제를 결합 하는 OAuth 2.0 및 OpenID Connect의 조합 및 IdentityServer 4는 이러한 프로토콜의 구현입니다.

EShopOnContainers 참조 응용 프로그램 등의 직접 클라이언트 마이크로 서비스 통신을 사용 하는 응용 프로그램으로 서비스 STS (보안 토큰) 역할 전용된 인증 마이크로 서비스 사용할 수 사용자를 인증 그림에 나와 있는 것 처럼 9-1입니다. 직접 클라이언트 마이크로 서비스 통신에 대 한 자세한 내용은 참조 [클라이언트 간의 통신 및 Microservices](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices)합니다.

![](authentication-and-authorization-images/authentication.png "전용된 인증 마이크로 서비스에서 인증")

**그림 9-1:** 전용된 인증 마이크로 서비스에서 인증

EShopOnContainers 모바일 앱 인증을 수행 하 고 Api에 대 한 액세스 제어를 IdentityServer 4를 사용 하 여 identity 마이크로 서비스와 통신 합니다. 따라서 모바일 앱 사용자 인증을 위한 또는 리소스에 액세스 하기 위한에 토큰에서 IdentityServer, 요청:

-   모바일 응용 프로그램 요청에 의해 이루어집니다 IdentityServer와 사용자를 인증 한 *identity* 토큰을 인증 프로세스의 결과 나타냅니다. 에 필요한 최소 사용자 및 사용자 인증 방법 및 시기에 대 한 정보에 대 한는 식별자를 포함 합니다. 추가 id 데이터를 포함할 수도 있습니다.
-   모바일 응용 프로그램 요청에 의해 이루어집니다 IdentityServer 사용 하 여 리소스에 액세스 하는 *액세스* API 리소스에 액세스할 수 있도록 하는 토큰입니다. 클라이언트는 액세스 토큰을 요청을 API에 전달 합니다. 액세스 토큰 (있는 경우)는 클라이언트와 사용자에 대 한 정보를 포함 합니다. Api 정보를 사용 하 여 자신의 데이터에 액세스 권한을 부여 합니다.

> [!NOTE]
> 클라이언트 토큰을 요청할 수 전에 IdentityServer 등록 되어야 합니다.

### <a name="adding-identityserver-to-a-web-application"></a>IdentityServer 웹 응용 프로그램에 추가

IdentityServer 4를 사용 하도록 ASP.NET Core 웹 응용 프로그램에서 웹 응용 프로그램의 Visual Studio 솔루션에 추가 해야 합니다. 자세한 내용은 참조 [설정과 개요](https://identityserver4.readthedocs.io/en/release/quickstarts/0_overview.html) IdentityServer 설명서에 있습니다.

IdentityServer를 웹 응용 프로그램의 Visual Studio 솔루션에 포함 되 면 추가 해야 처리 파이프라인을 웹 응용 프로그램의 HTTP 요청에 OAuth 2.0 및 OpenID Connect 끝점에 대 한 요청을 서비스할 수 있도록 합니다. 이 구현는 `Configure` 웹 응용 프로그램에서 메서드 `Startup` 다음 코드 예제에서와 같이 클래스:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

웹 응용 프로그램의 HTTP 요청 처리 파이프라인에서에서 중요 합니다. 따라서 IdentityServer 로그인 화면을 구현 하는 UI 프레임 워크 전에 파이프라인에 추가 되어야 합니다.

### <a name="configuring-identityserver"></a>IdentityServer 구성

IdentityServer에서 구성 해야는 `ConfigureServices` 웹 응용 프로그램에서 메서드 `Startup` 호출 하 여 클래스는 `services.AddIdentityServer` eShopOnContainers 참조 응용 프로그램에서 다음 코드 예제에서와 같이 메서드:

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

호출한 후의 `services.AddIdentityServer` 메서드를 추가 fluent Api에서 다음 구성 하 라고 합니다.

-   서명에 사용 되는 자격 증명입니다.
-   사용자가 요청할 수 있는 API 및 identity 리소스에 대 한 액세스.
-   클라이언트가 연결 요청 토큰입니다.
-   ASP.NET Core Identity.

>💡 **팁**: IdentityServer 4 구성을 동적으로 로드 합니다. IdentityServer 4 Api IdentityServer 구성 개체의 메모리 내 목록에서 구성할 수 있습니다. EShopOnContainers 참조 응용 프로그램 이러한 메모리 내 컬렉션은 응용 프로그램에 하드 코드 되어 있습니다. 그러나 프로덕션 시나리오에서 이러한 로드할 수 동적으로 데이터베이스 또는 구성 파일에서.

IdentityServer ASP.NET Core Id를 사용 하도록 구성 하는 방법에 대 한 정보를 참조 하십시오. [ASP.NET Core Id를 사용 하 여](https://identityserver4.readthedocs.io/en/release/quickstarts/6_aspnet_identity.html) IdentityServer 설명서에 있습니다.

#### <a name="configuring-api-resources"></a>API 리소스를 구성합니다.

API 리소스를 구성 하는 경우는 `AddInMemoryApiResources` 메서드에서 예상는 `IEnumerable<ApiResource>` 컬렉션입니다. 다음 코드 예제는 `GetApis` 메서드는 eShopOnContainers에서이 컬렉션을 제공 하는 응용 프로그램을 참조 합니다.

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

이 메서드는 주문 및 바구니 Api IdentityServer 보호 해야 지정 합니다. 따라서 IdentityServer 관리 되는 액세스 이러한 Api 호출 하는 경우 토큰 있으면 됩니다. 에 대 한 자세한 내용은 `ApiResource` 입력을 참조 하십시오. [API 리소스](https://identityserver4.readthedocs.io/en/release/reference/api_resource.html#refapiresource) IdentityServer 4 설명서에 있습니다.

#### <a name="configuring-identity-resources"></a>Identity 리소스 구성

Identity 리소스를 구성 하는 경우는 `AddInMemoryIdentityResources` 메서드에서 예상는 `IEnumerable<IdentityResource>` 컬렉션입니다. Identity 리소스는 사용자 ID, 이름 또는 전자 메일 주소 등의 데이터입니다. 각 id 리소스에는 고유한 이름이 하며 임의의 클레임 유형을 지정할 수 있습니다에 사용자 id 토큰에 포함 됩니다. 다음 코드 예제는 `GetResources` 메서드는 eShopOnContainers에서이 컬렉션을 제공 하는 응용 프로그램을 참조 합니다.

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

OpenID Connect 사양에 지정 일부 [표준 id 리소스](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims)합니다. 최소 요구 사항을 표시 하 고 사용자에 대 한 고유 ID에 대 한 지원입니다. 노출 하 여 이렇게는 `IdentityResources.OpenId` id 리소스입니다.

> [!NOTE]
> `IdentityResources` 클래스 (openid, 전자 메일, 프로필, 전화 및 주소) OpenID Connect 사양에 정의 된 범위에서 모두 지원 합니다.

IdentityServer는 사용자 지정 id 리소스를 정의할 수 있습니다. 자세한 내용은 참조 [사용자 지정 id 리소스 정의](https://identityserver4.readthedocs.io/en/release/topics/resources.html#defining-custom-identity-resources) IdentityServer 설명서에 있습니다. 에 대 한 자세한 내용은 `IdentityResource` 입력을 참조 하십시오. [Id 리소스](https://identityserver4.readthedocs.io/en/release/reference/identity_resource.html) IdentityServer 4 설명서에 있습니다.

#### <a name="configuring-clients"></a>클라이언트 구성

클라이언트는 IdentityServer에서 토큰을 요청할 수 있는 응용 프로그램입니다. 일반적으로 다음 설정이 최소한 각 클라이언트에 대해 정의 되어야 합니다.

-   고유한 클라이언트 id입니다.
-   토큰 서비스 (권한 유형을 라고도 함)와 상호 작용 허용된 합니다.
-   Id 및 액세스 토큰에 전송 되는 위치 위치 (리디렉션 URI 라고 함).
-   클라이언트에 대 한 액세스를 허용 되는 리소스의 목록을 (범위 라고도 함).

클라이언트를 구성 하는 경우는 `AddInMemoryClients` 메서드에서 예상는 `IEnumerable<Client>` 컬렉션입니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱에 대 한 구성을 보여 줍니다.는 `GetClients` 메서드는 eShopOnContainers에서이 컬렉션을 제공 하는 응용 프로그램을 참조 합니다.

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

이 구성은 다음과 같은 속성에 대 한 데이터를 지정합니다.

-   `ClientId`클라이언트에 대 한: 고유 ID입니다.
-   `ClientName`: 클라이언트 표시 로깅 및 동의 화면에 사용 되는 이름입니다.
-   `AllowedGrantTypes`: 클라이언트를 IdentityServer와 상호 작용 하려고 하는 방법을 지정 합니다. 자세한 내용은 참조 [인증 흐름을 구성](#configuring_the_authentication_flow)합니다.
-   `ClientSecrets`: 토큰 끝점에서 토큰을 요청할 때 사용 되는 클라이언트 보안 자격 증명을 지정 합니다.
-   `RedirectUris`: 허용 된 Uri를 반환할 토큰 또는 권한 부여 코드를 지정 합니다.
-   `RequireConsent`: 동의 화면 필요한 지 여부를 지정 합니다.
-   `RequirePkce`: 권한 부여 코드를 사용 하는 클라이언트 증명 키를 전송 해야 하는지 여부를 지정 합니다.
-   `PostLogoutRedirectUris`: 로그 아웃 한 후에 리디렉션할 허용 된 Uri를 지정 합니다.
-   `AllowedCorsOrigins`: IdentityServer 원점 으로부터의 크로스-원본 호출을 허용할 수 있도록 클라이언트의 원본을 지정 합니다.
-   `AllowedScopes`: 클라이언트에 대 한 액세스는 리소스를 지정 합니다. 기본적으로 클라이언트는 모든 리소스에 액세스할 수 없습니다에 있습니다.
-   `AllowOfflineAccess`: 클라이언트 새로 고침 토큰을 요청할 수 있는지 여부를 지정 합니다.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>인증 흐름을 구성합니다.

인증 흐름에서 부여 유형을 지정 하 여 클라이언트와 IdentityServer 구성할 수는 `Client.AllowedGrantTypes` 속성입니다. OpenID Connect 및 OAuth 2.0 사양에는 다양 한 인증 흐름을 포함 하 여 정의 합니다.

-   암시적입니다. 이 흐름 브라우저 기반 응용 프로그램에 대해 최적화 되 고 인증 전용 사용자 또는 인증 및 액세스 토큰 요청에 대 한 사용 해야 합니다. 모든 토큰 브라우저를 통해 전송 되며 따라서 고급 기능 처럼 새로 고침 토큰은 허용 되지 않습니다.
-   권한 부여 코드입니다. 이 흐름 클라이언트 인증을 지원 하면서 브라우저 전면 채널 달리 백 채널에서 토큰을 검색 하는 기능을 제공 합니다.
-   혼합. 이 흐름은 암시적의 조합 및 인증 코드 권한 유형입니다. Id 토큰 브라우저 채널을 통해 전송 되 고 권한 부여 코드와 같은 다른 아티팩트를 함께 프로토콜 서명 된 응답을 포함 합니다. 응답의 유효성 검사 성공 후 액세스를 가져오고 새로 고침 토큰 백 채널을 사용 해야 합니다.

> [!TIP]
> 혼합 인증 흐름을 사용 합니다. 혼합 인증 흐름은 많은 브라우저 채널에 적용 되는 공격을 완화 하는 되며 액세스 토큰을 검색 합니다 (및 수 있는 새로 고침 토큰)을 하는 네이티브 응용 프로그램에 대 한 권장된 흐름.

인증 흐름에 대 한 자세한 내용은 참조 [부여 유형을](https://identityserver4.readthedocs.io/en/release/topics/grant_types.html) IdentityServer 4 설명서에 있습니다.

### <a name="performing-authentication"></a>인증을 수행합니다.

사용자 대신 토큰을 발급 하려면 IdentityServer에 대 한 사용자 해야 로그인 IdentityServer 합니다. 그러나 IdentityServer 인증에 대 한 사용자 인터페이스 또는 데이터베이스를 제공 하지 않습니다. 따라서 eShopOnContainers 참조 응용 프로그램에서 ASP.NET Core Id는이 목적을 위해 사용 됩니다.

EShopOnContainers 모바일 앱의 인증을 받은 IdentityServer 그림 9-2에 표시 된 하이브리드 인증 흐름을 사용 합니다.

![](authentication-and-authorization-images/sign-in.png "로그인 프로세스의 한 높은 수준의 개요")

**그림 9-2:** 로그인 프로세스의 한 높은 수준의 개요

에 대 한 로그인 요청이 `<base endpoint>:5105/connect/authorize`합니다. 성공적으로 인증 IdentityServer id 토큰 및 인증 코드를 포함 하는 인증 응답을 반환 합니다. 인증 코드에 전송 됩니다 `<base endpoint>:5105/connect/token`, access, id 및 새로 고침 토큰을 사용 하 여 응답입니다.

eShopOnContainers 모바일 앱 기호 출력 IdentityServer의 요청을 전송 하 여 `<base endpoint>:5105/connect/endsession`, 추가 매개 변수를 사용 합니다. 로그 아웃 발생 한 후 IdentityServer 후 로그 아웃 리디렉션 URI 모바일 앱으로 다시 전송 하 여 응답 합니다. 그림 9-3에서는이 프로세스를 보여 줍니다.

![](authentication-and-authorization-images/sign-out.png "로그 아웃 프로세스의 한 높은 수준의 개요")

**그림 9-3:** sign-out 프로세스의 한 높은 수준의 개요

EShopOnContainers 모바일 앱 IdentityServer와 통신 하 여 수행 되는 `IdentityService` 클래스를 구현 하는 `IIdentityService` 인터페이스입니다. 이 인터페이스를 구현 하는 클래스를 제공 하는 지정 `CreateAuthorizationRequest`, `CreateLogoutRequest`, 및 `GetTokenAsync` 메서드.

#### <a name="signing-in"></a>에 로그인

사용자가 누를 때는 **로그인** 단추는 `LoginView`, `SignInCommand` 에 `LoginViewModel` 클래스 차례로 실행 하는 실행 되는 `SignInAsync` 메서드. 다음 코드 예제에서는이 메서드를 보여 줍니다.

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

이 메서드 호출의 `CreateAuthorizationRequest` 에서 메서드는 `IdentityService` 다음 코드 예제에 표시 된 클래스:

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.IdentityCallback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

이 메서드가 IdentityServer의에 대 한 URI를 만드는 [권한 부여 끝점](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), 필수 매개 변수를 사용 합니다. 권한 부여 끝점에는 `/connect/authorize` 에 5105 사용자 설정으로 노출 하는 기본 끝점의 포트입니다. 사용자 설정에 대 한 자세한 내용은 참조 [구성 관리](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)합니다.

> [!NOTE]
> OAuth 코드 Exchange (PKCE) 확장에 대 한 원본 증명 키를 구현 하 여 eShopOnContainers 모바일 응용 프로그램의 공격 노출 영역이 줄어듭니다. PKCE을 가로채는 경우 사용 하는 인증 코드를 보호 합니다. 이 클라이언트 권한 부여 요청에서 전달 되는 해시 비밀 검증 도구를 생성 하 여 수행 하 고 제공 된 해시 되지 않은 경우 권한 부여 코드를 사용 중입니다. PKCE에 대 한 자세한 내용은 참조 [OAuth 공용 클라이언트에서 코드 Exchange에 대 한 원본 증명 키](https://tools.ietf.org/html/rfc7636) Internet Engineering Task Force 웹 사이트에 있습니다.

반환된 된 URI에 저장 되는 `LoginUrl` 의 속성은 `LoginViewModel` 클래스. 경우는 `IsLogin` 속성은 `true`, [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) 에 `LoginView` 표시 됩니다. `WebView` 데이터 바인딩합니다 해당 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) 속성을는 `LoginUrl` 의 속성은 `LoginViewModel` 클래스 하 고 있으므로 IdentityServer 로그인 요청 하면 때는 `LoginUrl` 속성이로 설정 되어 IdentityServer의 권한 부여 끝점입니다. IdentityServer이이 요청을 수신 하 고 사용자 인증 되지 않은 경우는 `WebView` 그림 9-4와 같이 구성 된 로그인 페이지로 이동 합니다.

![](authentication-and-authorization-images/login.png "WebView에 표시 된 로그인 페이지")

**그림 9-4:** WebView에 표시 된 로그인 페이지

로그인이 완료 되 면는 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) 반환 된 URI로 이동 합니다. 이 `WebView` 탐색 하면는 `NavigateAsync` 에서 메서드는 `LoginViewModel` 실행 될 다음 코드 예제에 표시 된 클래스:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

이 메서드는 반환 URI에 포함 된 인증 응답을 구문 분석 하 고 있는 유효한 권한 부여 코드를 있는 요청을 IdentityServer의 [토큰 끝점](https://identityserver4.readthedocs.io/en/release/endpoints/token.html), 권한 부여 코드를 전달에서 PKCE 비밀 검증 도구, 오류 코드 및 기타 필수 매개 변수입니다. 토큰 끝점에는 `/connect/token` 에 5105 사용자 설정으로 노출 하는 기본 끝점의 포트입니다. 사용자 설정에 대 한 자세한 내용은 참조 [구성 관리](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)합니다.

>💡 **팁**: 유효성 검사 Uri를 반환 합니다. EShopOnContainers 모바일 앱 반환 URI의 유효성을 검사 하지 않습니다, 있지만 오픈 리디렉션 공격을 방지 하기 위해 알려진된 위치에 반환 하는 URI가 참조 하는 유효성을 검사 하는 것이 좋습니다.

토큰 끝점 유효한 권한 부여 코드와 PKCE 비밀 검증 도구를 수신 하는 경우 액세스 토큰, id 토큰 및 새로 고침 토큰을 사용 하 여 응답 합니다. 액세스 토큰 (API 리소스에 액세스할 수 있음) 및 id 토큰, 응용 프로그램 설정으로 저장 한 다음 됩니다 하 고 페이지 탐색 수행 됩니다. 따라서 eShopOnContainers 모바일 앱에서이 따라가이:으로 이동할 사용자가을 IdentityServer와 성공적으로 인증 수, 하는 `MainView` 는 페이지는 [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 표시 하는 `CatalogView` 선택한 탭으로 합니다.

페이지 탐색에 대 한 정보를 참조 하십시오. [탐색](~/xamarin-forms/enterprise-application-patterns/navigation.md)합니다. 방법에 대 한 내용은 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) 탐색 참조를 실행 하는 보기 모델 방법으로 인해 [호출 탐색 동작을 사용 하 여](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors)합니다. 응용 프로그램 설정에 대 한 정보를 참조 하십시오. [구성 관리](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)합니다.

> [!NOTE]
> eShopOnContainers 모의 로그인 응용 프로그램에서 모의 서비스를 사용 하도록 구성 된 경우 수도 있습니다는 `SettingsView`합니다. 이 모드에서는 응용 프로그램에 로그인 자격 증명을 사용 하 여 사용자를 대신 허용 IdentityServer와 통신 하지 않습니다.

#### <a name="signing-out"></a>서명 아웃

사용자가 누를 때는 **로그 아웃** 단추는 `ProfileView`, `LogoutCommand` 에 `ProfileViewModel` 클래스 차례로 실행 하는 실행 되는 `LogoutAsync` 메서드. 이 메서드는 페이지 탐색을 수행한는 `LoginView` 전달 페이지는 `LogoutParameter` 인스턴스로 설정 `true` 매개 변수로 합니다. 페이지 탐색 하는 동안 매개 변수 전달에 대 한 자세한 내용은 참조 [탐색 하는 동안 매개 변수 전달](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation)합니다.

뷰를 만들고 탐색 하는 `InitializeAsync` 보기의 관련된 보기 모델의 메서드를 실행 하는 다음 실행에서 `Logout` 의 메서드는 `LoginViewModel` 다음 코드 예제에 표시 된 클래스:

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

이 메서드를 호출는 `CreateLogoutRequest` 에서 메서드는 `IdentityService` 매개 변수로 응용 프로그램 설정에서 id 토큰을 전달 하는 클래스를 검색 합니다. 응용 프로그램 설정에 대 한 자세한 내용은 참조 [구성 관리](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)합니다. 다음 코드 예제는 `CreateLogoutRequest` 메서드:

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

이 메서드가 만드는 IdentityServer의 URI [세션 끝점 종료](https://identityserver4.readthedocs.io/en/release/endpoints/endsession.html#refendsession), 필수 매개 변수를 사용 합니다. 최종 세션 끝점은 `/connect/endsession` 에 5105 사용자 설정으로 노출 하는 기본 끝점의 포트입니다. 사용자 설정에 대 한 자세한 내용은 참조 [구성 관리](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)합니다.

반환된 된 URI에 저장 되는 `LoginUrl` 의 속성은 `LoginViewModel` 클래스. 반면는 `IsLogin` 속성은 `true`, [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) 에 `LoginView` 표시 됩니다. `WebView` 데이터 바인딩합니다 해당 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) 속성을는 `LoginUrl` 의 속성은 `LoginViewModel` 클래스와 IdentityServer를 로그 아웃 요청을 하면 게 때는 `LoginUrl` 속성이로 설정 되어 IdentityServer의 끝 세션 끝점입니다. 사용자가 로그인 하 IdentityServer이이 요청을 수신 하는 경우 로그 아웃 발생 합니다. 인증 쿠키 인증 미들웨어에서 ASP.NET Core에서 관리 하는 쿠키와 함께 추적 됩니다. 따라서 로그 아웃 IdentityServer 인증 쿠키를 제거를 한 후 로그 아웃 리디렉션 URI를 다시 클라이언트에 보냅니다.

모바일 앱에서의 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) 사후 로그 아웃 리디렉션 URI로 이동 합니다. 이 `WebView` 탐색 하면는 `NavigateAsync` 에서 메서드는 `LoginViewModel` 실행 될 다음 코드 예제에 표시 된 클래스:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

이 메서드 id 토큰 및의 액세스 토큰을 응용 프로그램 설정 모두 지우고 설정는 `IsLogin` 속성을 `false`,으로 구독이 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) 에 `LoginView` 보이지 않는 되도록 페이지 . 마지막으로 `LoginUrl` IdentityServer URI의 속성이 [권한 부여 끝점](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), 필수 매개 변수가 된 다음에 준비 과정에서 사용자가 시작할에 로그인 합니다.

페이지 탐색에 대 한 정보를 참조 하십시오. [탐색](~/xamarin-forms/enterprise-application-patterns/navigation.md)합니다. 방법에 대 한 내용은 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) 탐색 참조를 실행 하는 보기 모델 방법으로 인해 [호출 탐색 동작을 사용 하 여](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors)합니다. 응용 프로그램 설정에 대 한 정보를 참조 하십시오. [구성 관리](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)합니다.

> [!NOTE]
> eShopOnContainers 수도 있습니다는 모의 로그 아웃 응용 프로그램의 SettingsView에서 모의 서비스를 사용 하도록 구성 된 경우. 이 모드에서는 앱 IdentityServer와 통신 하지 않는 하 고 대신 응용 프로그램 설정에서 저장 된 모든 토큰을 지웁니다.

<a name="authorization" />

## <a name="authorization"></a>권한 부여

ASP.NET Core 웹 Api 액세스 권한을 부여 해야 할 필요가 인증 후에 모두 그렇지는 않지만 일부 인증 된 사용자에 게 사용할 수 있는 서비스를 Api를 수 있습니다.

ASP.NET Core MVC 경로에 대 한 액세스를 제한 컨트롤러에 권한 부여 특성을 적용 하 여 수행할 수 있습니다 또는 컨트롤러에 대 한 액세스를 제한 하는 동작 또는 동작을 인증 된 사용자, 다음 코드 예제에 나와 있는 것 처럼:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

권한이 없는 사용자가 컨트롤러나로 표시 된 작업에 액세스 하려고 하는 경우는 `Authorize` 특성, MVC 프레임 워크 401 (권한 없음된) HTTP 상태 코드를 반환 합니다.

> [!NOTE]
> 매개 변수에서 지정할 수는 `Authorize` 특정 사용자에 게는 API를 제한 하는 특성입니다. 자세한 내용은 참조 [권한 부여](/aspnet/core/security/authorization/introduction/)합니다.

액세스 토큰 권한 부여 제어 제공 되도록 IdentityServer 권한 부여 워크플로로 통합할 수 있습니다. 이 방법은 그림 9-5에 표시 됩니다.

![](authentication-and-authorization-images/authorization.png "액세스 토큰으로 권한 부여")

**그림 9-5:** 액세스 토큰으로 권한 부여

EShopOnContainers 모바일 앱 identity 마이크로 서비스와 통신 하 고 인증 프로세스의 일부로 액세스 토큰을 요청 합니다. 다음 액세스 토큰은 액세스 요청의 일부로 순서 지정 및 바구니 microservices에서 노출 된 Api에 전달 됩니다. 액세스 토큰에서 클라이언트와 사용자에 대 한 정보를 포함 합니다. Api 정보를 사용 하 여 자신의 데이터에 액세스 권한을 부여 합니다. IdentityServer Api 보호를 구성 하는 방법에 대 한 정보를 참조 하십시오. [API 리소스 구성](#configuring-api-resources)합니다.

### <a name="configuring-identityserver-to-perform-authorization"></a>권한 부여를 수행 하는 IdentityServer 구성

IdentityServer 사용한 권한 부여를 수행 하려면 해당 인증 미들웨어를 웹 응용 프로그램의 HTTP 요청 파이프라인에 추가 합니다. 미들웨어에 추가 되는 `ConfigureAuth` 웹 응용 프로그램에서 메서드 `Startup` 클래스에서 호출 되는 `Configure` 메서드를 하며 eShopOnContainers 참조 응용 프로그램에서 다음 코드 예제에 나와:

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

이 메서드는 유효한 액세스 토큰으로 API만 액세스할 수 있는지 확인 합니다. 신뢰할 수 있는 발급자 로부터 전송 게 들어오는 토큰의 유효성을 검사 하 고 수신 하는 API에서 사용 되는 토큰이 올바른지 유효성을 검사 하는 미들웨어입니다. 따라서 순서 지정 또는 바구니 컨트롤러를 찾아 401 (권한 없음된) HTTP 상태 코드를 액세스 토큰 필수 항목 임을 나타내는 반환 됩니다.

> [!NOTE]
> MVC를 추가 하기 전에 IdentityServer의 인증 미들웨어를 웹 응용 프로그램의 HTTP 요청 파이프라인에 추가 해야 `app.UseMvc()` 또는 `app.UseMvcWithDefaultRoute()`합니다.

### <a name="making-access-requests-to-apis"></a>Api에 대 한 액세스 요청 만들기

에 대 한 요청에서 순서 지정 및 바구니 microservices, 액세스 토큰을 인증 프로세스 동안 IdentityServer에서 가져올 때 다음 코드 예제와 같이 요청을에 포함 되어야 합니다.

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

액세스 토큰 응용 프로그램 설정 대로 저장 하 고 플랫폼 특정 저장소에서 검색할 이며에 대 한 호출에 포함 된 `GetOrderAsync` 에서 메서드는 `OrderService` 클래스입니다.

마찬가지로, 액세스 토큰이 포함 되어야 합니다 API를 보호 하는 데이터는 IdentityServer를 보낼 경우 다음 코드 예제에 나와 있는 것 처럼:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

액세스 토큰은 플랫폼별 저장소에서 검색 되 고에 대 한 호출에 포함 된 `UpdateBasketAsync` 에서 메서드는 `BasketService` 클래스입니다.

`RequestProvider` 클래스, eShopOnContainers 모바일 앱에서 사용 하 여는 `HttpClient` 클래스를 eShopOnContainers 참조 응용 프로그램에서 노출 하는 RESTful Api에 요청 합니다. 만들기 순서 지정 및 바구니 Api 권한 부여를 요구 하기 위해 요청 하면 유효한 액세스 토큰이 요청에 포함 되어 있어야 합니다. 헤더에 액세스 토큰을 추가 하 여 이렇게는 `HttpClient` 다음 코드 예제에서와 같이 인스턴스:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`DefaultRequestHeaders` 속성은 `HttpClient` 클래스 각 요청과 함께 전송 되는 헤더를 노출 하 고 액세스 토큰에 추가 되는 `Authorization` 헤더 문자열을 접두사로 `Bearer`합니다. RESTful API의 값에 요청이 전송 된 때의 `Authorization` 헤더에서 추출 하 고 신뢰할 수 있는 발급자 로부터 전송한 및 사용자에 게는 API를 호출 하는 권한이 있는지 여부를 결정 하는 데 사용 하는 수신 확인 하기 위해 검사 됩니다.

EShopOnContainers 모바일 앱 웹 요청을 사용 하는 방법에 대 한 자세한 내용은 참조 [원격 데이터 액세스](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md)합니다.

## <a name="summary"></a>요약

ASP.NET MVC 웹 응용 프로그램와 통신 하는 Xamarin.Forms 앱에 인증 및 권한 부여를 통합 하는 방법은 여러 가지가 있습니다. EShopOnContainers 모바일 앱을 인증 및 권한 부여 IdentityServer 4를 사용 하는 컨테이너 화 된 identity 마이크로 서비스를 수행 합니다. IdentityServer는 전달자 토큰 인증을 수행 하도록 ASP.NET Core Id와 통합 되는 ASP.NET Core에 대 한 OAuth 2.0 및 OpenID Connect에서 프레임 워크는 오픈 소스입니다.

모바일 앱 또는 사용자를 인증에 대 한 리소스에 액세스 하기 위한 IdentityServer에서 보안 토큰을 요청 합니다. 리소스를 액세스할 때 액세스 토큰에는 권한 부여가 필요한 Api에 요청에 포함 되어야 합니다. IdentityServer의 미들웨어 신뢰할 수 있는 발급자 로부터 보낸 사실을 수신 하는 API에서 사용 되는 유효한 지 확인 하는 들어오는 액세스 토큰의 유효성을 검사 합니다.


## <a name="related-links"></a>관련 링크

- [EBook (2mb PDF)를 다운로드 합니다.](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
