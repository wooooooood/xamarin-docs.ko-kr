---
title: 인증 및 권한 부여
description: 이 장에서는 eShopOnContainers mobile 앱이 컨테이너 화 된 마이크로 서비스에 대 한 인증 및 권한 부여를 수행 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: 45008e127286d14ef62c5212976bfd3a8aac651f
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529180"
---
# <a name="authentication-and-authorization"></a>인증 및 권한 부여

인증은 사용자의 이름 및 암호와 같은 id 자격 증명을 가져오고 기관에 대해 해당 자격 증명의 유효성을 검사 하는 프로세스입니다. 자격 증명이 유효 하면 자격 증명을 제출한 엔터티가 인증 된 id로 간주 됩니다. Id가 인증 되 면 권한 부여 프로세스에서 해당 id에 지정 된 리소스에 대 한 액세스 권한이 있는지 여부를 확인 합니다.

인증 및 권한 부여를 ASP.NET Core Id 사용, Microsoft, Google 등의 외부 인증 공급자를 사용 하는 것을 포함 하 여 ASP.NET MVC 웹 응용 프로그램과 통신 하는 Xamarin Forms 앱에 통합 하는 방법에는 여러 가지가 있습니다. Facebook, Twitter 및 인증 미들웨어가 있습니다. EShopOnContainers 모바일 앱은 IdentityServer 4를 사용 하는 컨테이너 화 된 identity 마이크로 서비스를 사용 하 여 인증 및 권한 부여를 수행 합니다. 모바일 앱은 사용자를 인증 하거나 리소스에 액세스 하기 위해 IdentityServer에서 보안 토큰을 요청 합니다. IdentityServer 사용자를 대신 하 여 토큰을 발급 하려면 사용자가 IdentityServer에 로그인 해야 합니다. 그러나 IdentityServer는 인증을 위한 사용자 인터페이스 또는 데이터베이스를 제공 하지 않습니다. 따라서 eShopOnContainers 참조 응용 프로그램에서 ASP.NET Core Identity가이 목적으로 사용 됩니다.

## <a name="authentication"></a>인증

응용 프로그램에서 현재 사용자의 id를 알고 있어야 하는 경우 인증이 필요 합니다. 사용자를 식별 하는 ASP.NET Core의 기본 메커니즘은 개발자가 구성한 데이터 저장소에 사용자 정보를 저장 하는 ASP.NET Core Id 멤버 자격 시스템입니다. 일반적으로 사용자 지정 저장소나 타사 패키지를 사용 하 여 Azure storage, Azure Cosmos DB 또는 기타 위치에 id 정보를 저장할 수는 있지만이 데이터 저장소는 일반적으로 EntityFramework 저장소입니다.

로컬 사용자 데이터 저장소를 사용 하 고 쿠키를 통해 요청 간에 id 정보를 유지 하는 인증 시나리오의 경우 (ASP.NET MVC 웹 응용 프로그램에서 일반적으로) ASP.NET Core Identity가 적합 한 솔루션입니다. 그러나 쿠키는 항상 데이터를 유지 하 고 전송 하는 자연 스러운 수단이 아닙니다. 예를 들어 모바일 앱에서 액세스 하는 RESTful 끝점을 노출 하는 ASP.NET Core 웹 응용 프로그램은이 시나리오에서 쿠키를 사용할 수 없으므로 일반적으로 전달자 토큰 인증을 사용 해야 합니다. 그러나 전달자 토큰은 모바일 앱에서 수행 된 웹 요청의 권한 부여 헤더에 쉽게 검색 하 고 포함할 수 있습니다.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>IdentityServer 4를 사용 하 여 전달자 토큰 발급

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) 는 로컬 ASP.NET Core id 사용자에 대 한 보안 토큰 발급을 비롯 한 여러 인증 및 권한 부여 시나리오에 사용할 수 있는 ASP.NET Core에 대 한 오픈 소스 openid connect Connect 및 OAuth 2.0 프레임 워크입니다.

> [!NOTE]
> Openid connect Connect와 OAuth 2.0은 매우 비슷하며 책임이 서로 다릅니다.

Openid connect Connect는 OAuth 2.0 프로토콜을 기반으로 하는 인증 계층입니다. OAuth 2는 응용 프로그램이 보안 토큰 서비스에서 액세스 토큰을 요청 하 고 Api와 통신 하는 데 사용할 수 있는 프로토콜입니다. 이 위임은 인증 및 권한 부여를 중앙 집중화할 수 있으므로 클라이언트 응용 프로그램 및 Api의 복잡성을 줄입니다.

Openid connect Connect와 OAuth 2.0의 조합은 인증 및 API 액세스의 두 가지 기본적인 보안 문제를 결합 하 고, IdentityServer 4는 이러한 프로토콜의 구현입니다.

EShopOnContainers reference 응용 프로그램과 같은 직접 클라이언트-마이크로 서비스 통신을 사용 하는 응용 프로그램에서 STS (보안 토큰 서비스) 역할을 하는 전용 인증 마이크로 서비스를 사용 하 여 사용자를 인증할 수 있습니다 (그림 참조). 9-1. 클라이언트-마이크로 서비스 간 직접 통신에 대 한 자세한 내용은 [클라이언트와 마이크로 서비스 간 통신](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices)을 참조 하세요.

![](authentication-and-authorization-images/authentication.png "전용 인증 마이크로 서비스 인증")

**그림 9-1:** 전용 인증 마이크로 서비스 인증

EShopOnContainers 모바일 앱은 IdentityServer 4를 사용 하 여 인증을 수행 하 고 Api에 대 한 액세스를 제어 하는 id 마이크로 서비스와 통신 합니다. 따라서 모바일 앱은 사용자를 인증 하거나 리소스에 액세스 하기 위해 IdentityServer의 토큰을 요청 합니다.

- IdentityServer를 사용 하 여 사용자를 인증 하는 것은 인증 프로세스의 결과를 나타내는 *id* 토큰을 요청 하는 모바일 앱에 의해 수행 됩니다. 최소한의 경우 사용자에 대 한 식별자와 사용자가 인증 하는 방법 및 시기에 대 한 정보를 포함 합니다. 추가 id 데이터를 포함할 수도 있습니다.
- IdentityServer를 사용 하 여 리소스에 액세스 하는 것은 API 리소스에 대 한 액세스를 허용 하는 *액세스* 토큰을 요청 하는 모바일 앱에 의해 구현 됩니다. 클라이언트는 액세스 토큰을 요청 하 고 API에 전달 합니다. 액세스 토큰에는 클라이언트 및 사용자 (있는 경우)에 대 한 정보가 포함 됩니다. 그런 다음 Api는 해당 정보를 사용 하 여 해당 데이터에 대 한 액세스 권한을 부여 합니다.

> [!NOTE]
> 클라이언트는 IdentityServer에 등록 해야 토큰을 요청할 수 있습니다.

### <a name="adding-identityserver-to-a-web-application"></a>웹 응용 프로그램에 IdentityServer 추가

ASP.NET Core 웹 응용 프로그램에서 IdentityServer 4를 사용 하려면 웹 응용 프로그램의 Visual Studio 솔루션에 추가 해야 합니다. 자세한 내용은 IdentityServer 설명서에서 [개요](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) 를 참조 하세요.

IdentityServer가 웹 응용 프로그램의 Visual Studio 솔루션에 포함 되 면 웹 응용 프로그램의 HTTP 요청 처리 파이프라인에 추가 해야 Openid connect Connect 및 OAuth 2.0 끝점에 대 한 요청을 처리할 수 있습니다. 이는 다음 코드 예제 `Configure` 에서 보여 주는 것 처럼 웹 `Startup` 응용 프로그램의 클래스에서 메서드를 통해 얻을 수 있습니다.

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

웹 응용 프로그램의 HTTP 요청 처리 파이프라인에서 순서가 중요 합니다. 따라서 로그인 화면을 구현 하는 UI 프레임 워크 앞에 IdentityServer를 파이프라인에 추가 해야 합니다.

### <a name="configuring-identityserver"></a>IdentityServer 구성

EShopOnContainers reference 응용 프로그램의 다음 `ConfigureServices` 코드 예제에서 설명한 것 처럼 `Startup` `services.AddIdentityServer` 메서드를 호출 하 여 웹 응용 프로그램의 클래스에 있는 메서드에서 IdentityServer을 구성 해야 합니다.

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

`services.AddIdentityServer` 메서드를 호출한 후에는 다음을 구성 하기 위해 추가 흐름 api가 호출 됩니다.

- 서명에 사용 되는 자격 증명입니다.
- 사용자가 액세스를 요청할 수 있는 API 및 id 리소스입니다.
- 요청 토큰에 연결 되는 클라이언트입니다.
- ASP.NET Core Id입니다.

> [!TIP]
> IdentityServer 4 구성을 동적으로 로드 합니다. IdentityServer 4의 Api를 사용 하면 구성 개체의 메모리 내 목록에서 IdentityServer를 구성할 수 있습니다. EShopOnContainers reference 응용 프로그램에서 이러한 메모리 내 컬렉션은 응용 프로그램에 하드 코딩 됩니다. 그러나 프로덕션 시나리오에서는 구성 파일이 나 데이터베이스에서 동적으로 로드할 수 있습니다.

ASP.NET Core Identity를 사용 하도록 IdentityServer를 구성 하는 방법에 대 한 자세한 내용은 IdentityServer 설명서에서 [ASP.NET Core Identity 사용](https://identityserver4.readthedocs.io/en/latest/quickstarts/8_aspnet_identity.html) 을 참조 하세요.

#### <a name="configuring-api-resources"></a>API 리소스 구성

API 리소스를 구성할 때 메서드에 `AddInMemoryApiResources` 는 `IEnumerable<ApiResource>` 컬렉션이 필요 합니다. 다음 코드 예제에서는 eShopOnContainers reference `GetApis` 응용 프로그램에서이 컬렉션을 제공 하는 메서드를 보여 줍니다.

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

이 메서드는 IdentityServer가 주문 및 바구니 Api를 보호 하도록 지정 합니다. 따라서 IdentityServer 관리 액세스 토큰은 이러한 Api를 호출할 때 필요 합니다. `ApiResource` 형식에 대 한 자세한 내용은 IdentityServer 4 설명서의 [API 리소스](https://identityserver4.readthedocs.io/en/latest/reference/api_resource.html) 를 참조 하세요.

#### <a name="configuring-identity-resources"></a>Id 리소스 구성

Id 리소스를 구성할 때 메서드에 `AddInMemoryIdentityResources` 는 `IEnumerable<IdentityResource>` 컬렉션이 필요 합니다. Id 리소스는 사용자 ID, 이름, 전자 메일 주소 등의 데이터입니다. 각 id 리소스는 고유한 이름을 가지 며 임의의 클레임 유형을 할당할 수 있으며,이는 사용자의 id 토큰에 포함 됩니다. 다음 코드 예제에서는 eShopOnContainers reference `GetResources` 응용 프로그램에서이 컬렉션을 제공 하는 메서드를 보여 줍니다.

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

Openid connect Connect 사양은 일부 [표준 id 리소스](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims)를 지정 합니다. 최소 요구 사항은 사용자의 고유 ID를 내보내기 위한 지원이 제공 된다는 것입니다. 이는 id 리소스를 `IdentityResources.OpenId` 노출 하 여 이루어집니다.

> [!NOTE]
> 클래스 `IdentityResources` 는 openid connect Connect 사양 (openid connect, 메일, 프로필, 전화, 주소)에 정의 된 모든 범위를 지원 합니다.

또한 IdentityServer는 사용자 지정 id 리소스의 정의를 지원 합니다. 자세한 내용은 IdentityServer 설명서의 [사용자 지정 id 리소스 정의](http://docs.identityserver.io/en/latest/topics/resources.html#defining-custom-identity-resources) 를 참조 하세요. `IdentityResource` 형식에 대 한 자세한 내용은 IdentityServer 4 설명서의 [id 리소스](https://identityserver4.readthedocs.io/en/latest/reference/identity_resource.html) 를 참조 하세요.

#### <a name="configuring-clients"></a>클라이언트 구성

클라이언트는 IdentityServer에서 토큰을 요청할 수 있는 응용 프로그램입니다. 일반적으로 각 클라이언트에 대해 다음 설정을 최소한으로 정의 해야 합니다.

- 고유한 클라이언트 ID입니다.
- 토큰 서비스와의 허용 되는 상호 작용 (권한 유형 이라고 함)입니다.
- Id 및 액세스 토큰이 전송 되는 위치 (리디렉션 URI)입니다.
- 클라이언트에서 액세스할 수 있는 리소스의 목록 (범위 라고 함)입니다.

클라이언트를 구성할 때 메서드에 `AddInMemoryClients` 는 `IEnumerable<Client>` 컬렉션이 필요 합니다. 다음 코드 예제에서는 eShopOnContainers reference 응용 프로그램에서이 컬렉션을 제공 하 `GetClients` 는 메서드의 eShopOnContainers mobile 앱에 대 한 구성을 보여 줍니다.

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

이 구성은 다음 속성에 대 한 데이터를 지정 합니다.

- `ClientId`: 클라이언트에 대 한 고유 ID입니다.
- `ClientName`: 로깅 및 동의 화면에 사용 되는 클라이언트 표시 이름입니다.
- `AllowedGrantTypes`: 클라이언트가 IdentityServer와 상호 작용 하는 방법을 지정 합니다. 자세한 내용은 [인증 흐름 구성](#configuring_the_authentication_flow)을 참조 하세요.
- `ClientSecrets`: 토큰 끝점에서 토큰을 요청할 때 사용 되는 클라이언트 암호 자격 증명을 지정 합니다.
- `RedirectUris`: 토큰 또는 권한 부여 코드를 반환할 허용 되는 Uri를 지정 합니다.
- `RequireConsent`: 동의 화면이 필요한 지 여부를 지정 합니다.
- `RequirePkce`: 인증 코드를 사용 하는 클라이언트가 증명 키를 보내야 하는지 여부를 지정 합니다.
- `PostLogoutRedirectUris`: 로그 아웃 후에 리디렉션할 허용 되는 Uri를 지정 합니다.
- `AllowedCorsOrigins`: IdentityServer가 원본에서 크로스-원본 호출을 허용할 수 있도록 클라이언트의 원본을 지정 합니다.
- `AllowedScopes`: 클라이언트에서 액세스할 수 있는 리소스를 지정 합니다. 기본적으로 클라이언트에는 모든 리소스에 대 한 액세스 권한이 없습니다.
- `AllowOfflineAccess`: 클라이언트에서 새로 고침 토큰을 요청할 수 있는지 여부를 지정 합니다.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>인증 흐름 구성

클라이언트와 IdentityServer 간의 인증 흐름은 `Client.AllowedGrantTypes` 속성에서 grant 유형을 지정 하 여 구성할 수 있습니다. Openid connect Connect 및 OAuth 2.0 사양은 다음을 비롯 한 다양 한 인증 흐름을 정의 합니다.

- 명시적. 이 흐름은 브라우저 기반 응용 프로그램에 최적화 되어 있으며 사용자 인증 전용 이거나 인증 및 액세스 토큰 요청에 사용 되어야 합니다. 모든 토큰은 브라우저를 통해 전송 되므로 새로 고침 토큰과 같은 고급 기능을 사용할 수 없습니다.
- 인증 코드입니다. 이 흐름은 클라이언트 인증을 지 원하는 한편 브라우저 전면 채널과 달리 백 채널에서 토큰을 검색 하는 기능을 제공 합니다.
- 혼합. 이 흐름은 암시적 및 권한 부여 코드 권한 유형을 조합한 것입니다. Id 토큰은 브라우저 채널을 통해 전송 되 고 인증 코드와 같은 다른 아티팩트와 함께 서명 된 프로토콜 응답을 포함 합니다. 응답의 유효성 검사를 성공적으로 수행한 후에는 백 채널을 사용 하 여 액세스 및 새로 고침 토큰을 검색 해야 합니다.

> [!TIP]
> 하이브리드 인증 흐름을 사용 합니다. 하이브리드 인증 흐름은 브라우저 채널에 적용 되는 여러 공격을 완화 하 고, 액세스 토큰을 검색 하 고 토큰을 새로 고치는 네이티브 응용 프로그램에 권장 되는 흐름입니다.

인증 흐름에 대 한 자세한 내용은 IdentityServer 4 설명서의 [Grant Types](https://identityserver4.readthedocs.io/en/latest/topics/grant_types.html) 를 참조 하십시오.

### <a name="performing-authentication"></a>인증 수행

IdentityServer 사용자를 대신 하 여 토큰을 발급 하려면 사용자가 IdentityServer에 로그인 해야 합니다. 그러나 IdentityServer는 인증을 위한 사용자 인터페이스 또는 데이터베이스를 제공 하지 않습니다. 따라서 eShopOnContainers 참조 응용 프로그램에서 ASP.NET Core Identity가이 목적으로 사용 됩니다.

EShopOnContainers 모바일 앱은 그림 9-2에 나와 있는 하이브리드 인증 흐름을 사용 하 여 IdentityServer를 사용 하 여 인증 합니다.

![](authentication-and-authorization-images/sign-in.png "로그인 프로세스에 대 한 개략적인 개요")

**그림 9-2:** 로그인 프로세스에 대 한 개략적인 개요

에 `<base endpoint>:5105/connect/authorize`대 한 로그인 요청이 발생 합니다. 성공적인 인증 후 IdentityServer는 인증 코드와 id 토큰을 포함 하는 인증 응답을 반환 합니다. 그런 다음 인증 코드는 액세스, `<base endpoint>:5105/connect/token`id 및 새로 고침 토큰을 사용 하 여 응답 하는에 전송 됩니다.

EShopOnContainers 모바일 앱은에 `<base endpoint>:5105/connect/endsession`요청을 전송 하 여 추가 매개 변수를 사용 하 여 IdentityServer를 로그 아웃 합니다. 로그 아웃 발생 후 IdentityServer는 post 로그 아웃 리디렉션 URI를 모바일 앱으로 다시 전송 하 여 응답 합니다. 그림 9-3에서는이 프로세스를 보여 줍니다.

![](authentication-and-authorization-images/sign-out.png "로그 아웃 프로세스에 대 한 개략적인 개요")

**그림 9-3:** 로그 아웃 프로세스에 대 한 개략적인 개요

EShopOnContainers 모바일 앱에서 IdentityServer와의 통신은 `IdentityService` `IIdentityService` 인터페이스를 구현 하는 클래스에 의해 수행 됩니다. 이 인터페이스는 구현 하는 클래스에서, `CreateAuthorizationRequest` `CreateLogoutRequest`및 `GetTokenAsync` 메서드를 제공 하도록 지정 합니다.

#### <a name="signing-in"></a>로그인

사용자가에서 `SignInCommand` **로그인** 단추 `LoginView`를 누르면 `LoginViewModel` 클래스의가 실행 `SignInAsync` 되어 메서드가 실행 됩니다. 다음 코드 예제에서는 이 메서드를 보여줍니다.

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

이 메서드는 `CreateAuthorizationRequest` `IdentityService` 클래스의 메서드를 호출 합니다 .이 메서드는 다음 코드 예제에 나와 있습니다.

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.IdentityCallback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

이 메서드는 필수 매개 변수를 사용 하 여 IdentityServer의 [authorization 끝점](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)에 대 한 URI를 만듭니다. 권한 부여 끝점 `/connect/authorize` 은 사용자 설정으로 노출 된 기본 끝점의 포트 5105에 있습니다. 사용자 설정에 대 한 자세한 내용은 [구성 관리](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)를 참조 하세요.

> [!NOTE]
> EShopOnContainers 모바일 앱의 공격 노출 영역을 OAuth에 대 한 코드 교환 (PKCE) 확장을 구현 하 여 줄일 수 있습니다. PKCE는 권한 부여 코드가 가로채 면 사용 되지 않도록 보호 합니다. 이는 비밀 검증 도구를 생성 하는 클라이언트, 권한 부여 요청에 전달 되는 해시 및 인증 코드를 교환 때 해시 되지 않은 형태로 제공 됩니다. PKCE에 대 한 자세한 내용은 Internet 엔지니어링 작업 Force 웹 사이트에서 [OAuth 공용 클라이언트의 코드 교환에 대 한 증명 키](https://tools.ietf.org/html/rfc7636) 를 참조 하세요.

반환 된 URI는 `LoginUrl` `LoginViewModel` 클래스의 속성에 저장 됩니다. `IsLogin` [속성이이면`WebView`](xref:Xamarin.Forms.WebView) 의이 표시 됩니다. `LoginView` `true` 데이터 `WebView` 는속성`LoginUrl` 을 클래스의 속성에 IdentityServer `LoginUrl` 속성이 IdentityServer의 인증 끝점으로 설정 된 경우 로그인 요청을 호출 합니다. `LoginViewModel` [`Source`](xref:Xamarin.Forms.WebView.Source) IdentityServer이 요청을 수신 하 고 사용자가 인증 되지 않은 `WebView` 경우는 그림 9-4에 표시 된 구성 된 로그인 페이지로 리디렉션됩니다.

![](authentication-and-authorization-images/login.png "웹 보기에서 표시 하는 로그인 페이지")

**그림 9-4:** 웹 보기에서 표시 하는 로그인 페이지

로그인이 완료 [`WebView`](xref:Xamarin.Forms.WebView) 되 면이 반환 URI로 리디렉션됩니다. 이 `WebView` 탐색은 `NavigateAsync` `LoginViewModel` 클래스의 메서드를 실행 합니다 .이는 다음 코드 예제에 나와 있습니다.

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

이 메서드는 반환 URI에 포함 된 인증 응답을 구문 분석 하 고, 유효한 인증 코드가 있으면 IdentityServer의 [토큰 끝점](https://identityserver4.readthedocs.io/en/latest/endpoints/token.html)에 요청 하 여 권한 부여 코드, pkce 비밀 검증 도구를 전달 합니다. 및 기타 필수 매개 변수입니다. 토큰 끝점 `/connect/token` 은 사용자 설정으로 노출 된 기본 끝점의 포트 5105에 있습니다. 사용자 설정에 대 한 자세한 내용은 [구성 관리](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)를 참조 하세요.

> [!TIP]
> 반환 Uri의 유효성을 검사 합니다. EShopOnContainers 모바일 앱은 반환 URI의 유효성을 검사 하지 않지만, 가장 좋은 방법은 반환 URI가 알려진 위치를 참조 하는지 확인 하 여 열린 리디렉션 공격을 방지 하는 것입니다.

토큰 끝점이 유효한 인증 코드 및 PKCE 비밀 검증 도구를 수신 하면 액세스 토큰, id 토큰 및 새로 고침 토큰을 사용 하 여 응답 합니다. 그러면 API 리소스에 대 한 액세스를 허용 하는 액세스 토큰 및 id 토큰이 응용 프로그램 설정으로 저장 되 고 페이지 탐색이 수행 됩니다. 따라서 eShopOnContainers 모바일 앱에 대 한 전반적인 효과는 다음과 같습니다. 사용자가 IdentityServer를 사용 하 여 성공적으로 인증할 수 있는 경우 `MainView` 페이지 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) `CatalogView` 를 탐색 합니다. 선택 된 탭으로

페이지 탐색에 대 한 자세한 내용은 [탐색](~/xamarin-forms/enterprise-application-patterns/navigation.md)을 참조 하세요. 탐색에서 뷰 모델 [`WebView`](xref:Xamarin.Forms.WebView) 메서드를 실행 하는 방법에 대 한 자세한 내용은 [동작을 사용 하 여 탐색 호출](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors)을 참조 하세요. 응용 프로그램 설정에 대 한 자세한 내용은 [구성 관리](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)를 참조 하세요.

> [!NOTE]
> EShopOnContainers는 앱이에서 `SettingsView`모의 서비스를 사용 하도록 구성 된 경우에도 모의 로그인을 허용 합니다. 이 모드에서 앱은 IdentityServer와 통신 하지 않고 대신 사용자가 자격 증명을 사용 하 여 로그인 할 수 있도록 허용 합니다.

#### <a name="signing-out"></a>로그 아웃

사용자가에서 **로그 아웃** 단추 `ProfileView` `LogoutCommand` 를 누르면 `ProfileViewModel` 클래스의가 실행 `LogoutAsync` 되어 메서드가 실행 됩니다. 이 메서드는 페이지로의 `LoginView` 페이지 탐색을 수행 하 여로 `true` 설정 된 `LogoutParameter` 인스턴스를 매개 변수로 전달 합니다. 페이지를 탐색 하는 동안 매개 변수를 전달 하는 방법에 대 한 자세한 내용은 [탐색 하는 동안 매개 변수 전달](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation)

뷰가 생성 되 고 탐색 `InitializeAsync` 되 면 뷰의 연결 된 뷰 모델의 메서드가 실행 되며,이 메서드는 다음 코드 예제에 표시 된 `LoginViewModel` 클래스의 `Logout` 메서드를 실행 합니다.

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

이 메서드는 `IdentityService` 클래스 `CreateLogoutRequest` 에서 메서드를 호출 하 여 응용 프로그램 설정에서 검색 된 id 토큰을 매개 변수로 전달 합니다. 응용 프로그램 설정에 대 한 자세한 내용은 [구성 관리](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)를 참조 하세요. 다음 코드 예제는 `CreateLogoutRequest` 메서드를 보여줍니다.

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

이 메서드는 필수 매개 변수를 사용 하 여 IdentityServer의 [end session 끝점](https://identityserver4.readthedocs.io/en/latest/endpoints/endsession.html#refendsession)에 대 한 URI를 만듭니다. End session 끝점은 `/connect/endsession` 사용자 설정으로 노출 된 기본 끝점의 포트 5105에 있습니다. 사용자 설정에 대 한 자세한 내용은 [구성 관리](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)를 참조 하세요.

반환 된 URI는 `LoginUrl` `LoginViewModel` 클래스의 속성에 저장 됩니다. 속성이 인 `true`동안의는 [`WebView`](xref:Xamarin.Forms.WebView) 표시됩니다.`LoginView` `IsLogin` 데이터 `WebView` 는 속성 `LoginUrl` [`Source`](xref:Xamarin.Forms.WebView.Source) `LoginUrl` 을 클래스의속성에바인딩한다음,속성이IdentityServer의endsessionendpoint로설정된경우IdentityServer에대한로그아웃요청을만듭니다.`LoginViewModel` 사용자가 로그인 하는 경우 IdentityServer가이 요청을 받으면 로그 아웃이 발생 합니다. 인증은 ASP.NET Core에서 쿠키 인증 미들웨어에 의해 관리 되는 쿠키를 사용 하 여 추적 됩니다. 따라서 IdentityServer에서 로그 아웃 하면 인증 쿠키가 제거 되 고 사후 로그 아웃 리디렉션 URI가 클라이언트에 다시 전송 됩니다.

모바일 앱 [`WebView`](xref:Xamarin.Forms.WebView) 에서가 사후 로그 아웃 리디렉션 URI로 리디렉션됩니다. 이 `WebView` 탐색은 `NavigateAsync` `LoginViewModel` 클래스의 메서드를 실행 합니다 .이는 다음 코드 예제에 나와 있습니다.

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

이 메서드는 응용 프로그램 설정에서 id 토큰과 액세스 `IsLogin` 토큰을 모두 지우고 속성을로 `false`설정 하 여 `LoginView` 페이지의이 [`WebView`](xref:Xamarin.Forms.WebView) 보이지 않게 합니다. 마지막으로 속성은 다음에 사용자가 로그인을 시작할 때 준비 하는 데 필요한 매개 변수를 사용 하 여 IdentityServer의 [authorization 끝점](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)URI로 설정 됩니다. `LoginUrl`

페이지 탐색에 대 한 자세한 내용은 [탐색](~/xamarin-forms/enterprise-application-patterns/navigation.md)을 참조 하세요. 탐색에서 뷰 모델 [`WebView`](xref:Xamarin.Forms.WebView) 메서드를 실행 하는 방법에 대 한 자세한 내용은 [동작을 사용 하 여 탐색 호출](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors)을 참조 하세요. 응용 프로그램 설정에 대 한 자세한 내용은 [구성 관리](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)를 참조 하세요.

> [!NOTE]
> 또한 eShopOnContainers를 사용 하면 앱이 SettingsView에서 모의 서비스를 사용 하도록 구성 된 경우 모의 로그 아웃을 사용할 수 있습니다. 이 모드에서 앱은 IdentityServer와 통신 하지 않고 응용 프로그램 설정에서 저장 된 토큰을 모두 지웁니다.

<a name="authorization" />

## <a name="authorization"></a>Authorization

인증 후에는 ASP.NET Core 웹 Api에서 액세스 권한을 부여 해야 하는 경우가 많습니다 .이 경우 서비스에서 일부 인증 된 사용자에 게 Api를 사용할 수 있지만 all은 허용 되지 않습니다.

컨트롤러 또는 작업에 권한 부여 특성을 적용 하 여 ASP.NET Core MVC 경로에 대 한 액세스를 제한할 수 있습니다 .이는 다음 코드 예제에 표시 된 것 처럼 컨트롤러 또는 작업에 대 한 액세스를 인증 된 사용자로 제한 합니다.

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

권한이 없는 사용자가 `Authorize` 특성으로 표시 된 컨트롤러나 작업에 액세스 하려고 하면 MVC 프레임 워크에서 401 (권한 없음) HTTP 상태 코드를 반환 합니다.

> [!NOTE]
> `Authorize` 특성에 매개 변수를 지정 하 여 API를 특정 사용자로 제한할 수 있습니다. 자세한 내용은 [권한 부여](/aspnet/core/security/authorization/introduction/)를 참조 하세요.

IdentityServer는 제어 권한 부여를 제공 하는 액세스 토큰을 권한 부여 워크플로에 통합할 수 있습니다. 이 방법은 그림 9-5에 나와 있습니다.

![](authentication-and-authorization-images/authorization.png "액세스 토큰에의 한 권한 부여")

**그림 9-5:** 액세스 토큰에의 한 권한 부여

EShopOnContainers 모바일 앱은 id 마이크로 서비스와 통신 하 고 인증 프로세스의 일부로 액세스 토큰을 요청 합니다. 그런 다음 액세스 토큰은 주문 및 바구니 마이크로 서비스에 의해 노출 되는 Api에 액세스 요청의 일부로 전달 됩니다. 액세스 토큰에는 클라이언트 및 사용자에 대 한 정보가 포함 됩니다. 그런 다음 Api는 해당 정보를 사용 하 여 해당 데이터에 대 한 액세스 권한을 부여 합니다. Api를 보호 하도록 IdentityServer을 구성 하는 방법에 대 한 자세한 내용은 [Api 리소스 구성](#configuring-api-resources)을 참조 하세요.

### <a name="configuring-identityserver-to-perform-authorization"></a>권한 부여를 수행 하도록 IdentityServer 구성

IdentityServer를 사용 하 여 권한 부여를 수행 하려면 해당 권한 부여 미들웨어를 웹 응용 프로그램의 HTTP 요청 파이프라인에 추가 해야 합니다. 미들웨어는 `ConfigureAuth` 메서드에서`Configure` 호출 되는 웹 응용 프로그램의 `Startup` 클래스에 있는 메서드에서 추가 되며 eShopOnContainers reference 응용 프로그램의 다음 코드 예제에서 보여 줍니다.

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

이 메서드는 유효한 액세스 토큰을 사용 하 여 API에만 액세스할 수 있도록 합니다. 미들웨어는 들어오는 토큰의 유효성을 검사 하 여 신뢰할 수 있는 발급자 로부터 보내고 토큰을 수신 하는 API와 토큰을 사용할 수 있는지 확인 합니다. 따라서 주문 또는 바구니 컨트롤러를 검색 하면 액세스 토큰이 필요 함을 나타내는 401 (권한 없음) HTTP 상태 코드가 반환 됩니다.

> [!NOTE]
> `app.UseMvc()` 또는`app.UseMvcWithDefaultRoute()`를 사용 하 여 MVC를 추가 하기 전에 IdentityServer의 인증 미들웨어를 웹 응용 프로그램의 HTTP 요청 파이프라인에 추가 해야 합니다.

### <a name="making-access-requests-to-apis"></a>Api에 대 한 액세스 요청 만들기

주문 및 바구니 마이크로 서비스를 요청 하는 경우 다음 코드 예제에 표시 된 것 처럼 인증 프로세스 중에 IdentityServer에서 얻은 액세스 토큰을 요청에 포함 해야 합니다.

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

액세스 토큰은 응용 프로그램 설정으로 저장 되며 플랫폼별 저장소에서 검색 되 고 `GetOrderAsync` `OrderService` 클래스의 메서드에 대 한 호출에 포함 됩니다.

마찬가지로, 다음 코드 예제와 같이 IdentityServer protected API에 데이터를 보낼 때 액세스 토큰을 포함 해야 합니다.

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

액세스 토큰은 플랫폼별 저장소에서 검색 되 고 `UpdateBasketAsync` `BasketService` 클래스의 메서드에 대 한 호출에 포함 됩니다.

EShopOnContainers 모바일 앱의 `HttpClient` 클래스는클래스를사용하여eShopOnContainersreference응용프로그램에서노출하는RESTfulapi에대한요청을수행합니다.`RequestProvider` 권한 부여가 필요한 주문 및 바구니 Api에 대 한 요청을 만들 때 유효한 액세스 토큰을 요청에 포함 해야 합니다. 이렇게 하려면 다음 코드 예제에서 보여 주는 것 처럼 `HttpClient` 인스턴스의 헤더에 액세스 토큰을 추가 합니다.

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

클래스의 속성은 `DefaultRequestHeaders` 각 요청과 함께 전송 되는 헤더를 노출 하 고 액세스 `Authorization` 토큰은 문자열이 `Bearer`접두사로 추가 된 헤더에 추가 됩니다. `HttpClient` 요청이 RESTful api로 전송 되 면 `Authorization` 헤더의 값이 추출 되 고 유효성을 검사 하 여 신뢰할 수 있는 발급자 로부터 전송 되 고 사용자에 게이를 수신 하는 api를 호출할 수 있는 권한이 있는지 여부를 확인 하는 데 사용 됩니다.

EShopOnContainers 모바일 앱이 웹 요청을 만드는 방법에 대 한 자세한 내용은 [원격 데이터 액세스](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md)를 참조 하세요.

## <a name="summary"></a>요약

ASP.NET MVC 웹 응용 프로그램과 통신 하는 Xamarin. Forms 앱에 인증 및 권한 부여를 통합 하는 방법에는 여러 가지가 있습니다. EShopOnContainers 모바일 앱은 IdentityServer 4를 사용 하는 컨테이너 화 된 identity 마이크로 서비스를 사용 하 여 인증 및 권한 부여를 수행 합니다. IdentityServer는 ASP.NET Core Id와 통합 되어 전달자 토큰 인증을 수행 하는 ASP.NET Core에 대 한 오픈 소스 Openid connect Connect 및 OAuth 2.0 프레임 워크입니다.

모바일 앱은 사용자를 인증 하거나 리소스에 액세스 하기 위해 IdentityServer에서 보안 토큰을 요청 합니다. 리소스에 액세스할 때 권한 부여를 필요로 하는 Api에 대 한 요청에 액세스 토큰을 포함 해야 합니다. IdentityServer의 미들웨어는 들어오는 액세스 토큰의 유효성을 검사 하 여 신뢰할 수 있는 발급자 로부터 보내고이를 수신 하는 API에서 사용할 수 있는지 확인 합니다.


## <a name="related-links"></a>관련 링크

- [다운로드 전자책 (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
