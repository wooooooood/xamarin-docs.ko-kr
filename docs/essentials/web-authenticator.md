---
title: 'Xamarin.Essentials: 웹 인증자'
description: 이 문서에서는 Xamarin.Essentials의 WebAuthenticator 클래스에 대해 설명합니다. 이 클래스를 사용하면 앱에 대한 콜백을 수신 대기하는 브라우저 기반 인증 흐름을 시작할 수 있습니다.
ms.assetid: 3D95371E-5D59-440E-8D31-F3C04E493DC1
author: redth
ms.author: jodick
ms.date: 03/26/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6b094ddd7334da777d39d403eb06d72558c96ed2
ms.sourcegitcommit: 82eabb0eaa4a674897aa6d5e64efb91fd580c330
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86100193"
---
# <a name="xamarinessentials-web-authenticator"></a>Xamarin.Essentials: 웹 인증자

**WebAuthenticator** 클래스를 사용하면 앱에 등록된 특정 URL에 대한 콜백을 수신 대기하는 브라우저 기반 흐름을 시작할 수 있습니다.

## <a name="overview"></a>개요

많은 앱에서 사용자 인증 추가가 필요하며, 이는 흔히 사용자가 기존 Microsoft, Facebook, Google 그리고 이제 Apple 로그인 계정에 로그인할 수 있게 된다는 의미입니다.

[MSAL(Microsoft 인증 라이브러리)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview)은 앱에 인증을 추가하는 뛰어난 턴 키 솔루션을 제공합니다. 클라이언트 NuGet 패키지에서 Xamarin 앱을 지원하기도 합니다.

자체 웹 서비스를 인증에 사용하는 데 관심이 있는 경우 **WebAuthenticator**를 사용하여 클라이언트 쪽 기능을 구현할 수 있습니다.

## <a name="why-use-a-server-back-end"></a>서버 백 엔드를 사용하는 이유

보안을 강화하기 위해 많은 인증 공급자가 명시적 또는 2단계 인증 흐름만 제공하도록 전환했습니다. 이는 인증 흐름을 완료하기 위해 공급자로부터 _'클라이언트 암호'_ 가 필요함을 의미합니다. 아쉽게도 모바일 앱은 암호를 저장하기에 적합한 위치가 아니며, 모바일 앱의 코드, 이진 파일 또는 기타 위치에 저장된 모든 것은 일반적으로 안전하지 않은 것으로 간주됩니다.

여기에서 모범 사례는 모바일 앱과 인증 공급자 간의 중간 계층으로 웹 백 엔드를 사용하는 것입니다.

> [!IMPORTANT]
> 인증 흐름에서 웹 백 엔드를 활용하지 않는 이전의 모바일 전용 인증 라이브러리 및 패턴은 내재적으로 클라이언트 암호 저장을 위한 보안이 불충분하기 때문에 사용하지 않는 것이 좋습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

**WebAuthenticator** 기능에 액세스하려면 다음 플랫폼 관련 설정이 필요합니다.

# <a name="android"></a>[Android](#tab/android)

Android에서는 콜백 URI를 처리하기 위한 의도 필터 설정이 필요합니다. 이 작업은 `WebAuthenticatorCallbackActivity` 클래스를 서브클래싱하여 쉽게 수행할 수 있습니다.

> [!NOTE]
> [Android 앱 링크](https://developer.android.com/training/app-links/)를 구현하여 콜백 URI를 처리하고 이 애플리케이션만 콜백 URI를 처리하도록 등록될 수 있게 하는 것이 좋습니다.

```csharp
const string CALLBACK_SCHEME = "myapp";

[Activity(NoHistory = true, LaunchMode = LaunchMode.SingleTop)]
[IntentFilter(new[] { Android.Content.Intent.ActionView },
    Categories = new[] { Android.Content.Intent.CategoryDefault, Android.Content.Intent.CategoryBrowsable },
    DataScheme = CALLBACK_SCHEME)]
public class WebAuthenticationCallbackActivity : Xamarin.Essentials.WebAuthenticatorCallbackActivity
{
}
```

또한 `MainActivity`의 `OnResume` 재정의에서 Essentials로 콜백해야 합니다.

```csharp
protected override void OnResume()
{
    base.OnResume();

    Xamarin.Essentials.Platform.OnResume();
}
```

# <a name="ios"></a>[iOS](#tab/ios)

iOS에서는 Info.plist에 앱의 콜백 URI 패턴을 추가해야 합니다.

> [!NOTE]
> [범용 앱 링크](https://developer.apple.com/documentation/uikit/inter-process_communication/allowing_apps_and_websites_to_link_to_your_content)를 사용하여 앱의 콜백 URI를 모범 사례로 등록하는 것이 좋습니다.

또한 `AppDelegate`의 `OpenUrl` 메서드를 재정의하여 Essentials로 콜백해야 합니다.

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    if (Xamarin.Essentials.Platform.OpenUrl(app, url, options))
        return true;

    return base.OpenUrl(app, url, options);
}
```

# <a name="uwp"></a>[UWP](#tab/uwp)

UWP의 경우 `Package.appxmanifest` 파일에서 콜백 URI를 선언해야 합니다.

```xml
    <Extensions>
        <uap:Extension Category="windows.protocol">
            <uap:Protocol Name="myapp">
                <uap:DisplayName>My App</uap:DisplayName>
            </uap:Protocol>
        </uap:Extension>
    </Extensions>
```

-----

## <a name="using-webauthenticator"></a>WebAuthenticator 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

이 API는 다음 두 매개 변수를 사용하는 단일 메서드 `AuthenticateAsync`로 주로 구성됩니다. 웹 브라우저 흐름을 시작하는 데 사용해야 하는 URL과 흐름이 최종적으로 콜백하고 앱이 처리할 수 있도록 등록된 URI.

결과는 콜백 URI에서 구문 분석된 모든 쿼리 매개 변수를 포함하는 `WebAuthenticatorResult`입니다.

```csharp
var authResult = await WebAuthenticator.AuthenticateAsync(
        new Uri("https://mysite.com/mobileauth/Microsoft"),
        new Uri("myapp://"));

var accessToken = authResult?.AccessToken;
```

`WebAuthenticator` API는 브라우저에서 URL을 시작하고 콜백이 수신될 때까지 대기합니다.

![일반적인 웹 인증 흐름](images/web-authenticator.png)

사용자가 어느 지점에서든 흐름을 취소하면 `null` 결과가 반환됩니다.

## <a name="platform-differences"></a>플랫폼 간 차이점

# <a name="android"></a>[Android](#tab/android)

사용자 지정 탭이 사용 가능한 경우 항상 사용되며, 그렇지 않으면 URL에 대한 의도가 시작됩니다.

# <a name="ios"></a>[iOS](#tab/ios)

iOS 12 이상에서는 `ASWebAuthenticationSession`이 사용됩니다.  iOS 11에서는 `SFAuthenticationSession`이 사용됩니다.  이전 iOS 버전에서는 사용 가능한 경우 `SFSafariViewController`가 사용되며, 그렇지 않으면 Safari가 사용됩니다.

# <a name="uwp"></a>[UWP](#tab/uwp)

UWP에서는 `WebAuthenticationBroker`가 지원되는 경우 사용되고, 그렇지 않으면 시스템 브라우저가 사용됩니다.

-----

## <a name="apple-sign-in"></a>Apple 로그인

[Apple 검토 지침](https://developer.apple.com/app-store/review/guidelines/#sign-in-with-apple)에 따라 앱에서 소셜 로그인 서비스를 사용하여 인증하는 경우에는 Apple 로그인도 옵션으로 제공해야 합니다.

앱에 Apple 로그인을 추가하려면 먼저 [Apple 로그인을 사용하도록 앱을 구성](https://docs.microsoft.com/xamarin/ios/platform/ios13/sign-in)해야 합니다.

iOS 13 이상에서는 `AppleSignInAuthenticator.AuthenticateAsync()` 메서드를 호출하는 것이 좋습니다. 이렇게 하면 내부에서 네이티브 Apple 로그인 API를 사용하여 사용자가 이러한 디바이스에서 최상의 환경을 얻을 수 있습니다. 다음과 같이 런타임에 적절한 API를 사용하는 공유 코드를 작성할 수 있습니다.

```csharp
var scheme = "..."; // Apple, Microsoft, Google, Facebook, etc.
WebAuthenticatorResult r = null;

if (scheme.Equals("Apple")
    && DeviceInfo.Platform == DevicePlatform.iOS
    && DeviceInfo.Version.Major >= 13)
{
    // Use Native Apple Sign In API's
    r = await AppleSignInAuthenticator.AuthenticateAsync();
}
else
{
    // Web Authentication flow
    var authUrl = new Uri(authenticationUrl + scheme);
    var callbackUrl = new Uri("xamarinessentials://");

    r = await WebAuthenticator.AuthenticateAsync(authUrl, callbackUrl);
}

var accessToken = r?.AccessToken;
```

> [!TIP]
> iOS 13이 아닌 디바이스에서는 이 코드가 웹 인증 흐름을 시작하며, Android 및 UWP 디바이스에서 Apple 로그인을 사용하도록 설정하는 데에도 사용될 수 있습니다.
> iOS 시뮬레이터에서 iCloud 계정에 로그인하여 Apple 로그인을 테스트할 수 있습니다.

-----

## <a name="aspnet-core-server-back-end"></a>ASP.NET Core 서버 백 엔드

모든 웹 백 엔드 서비스에서 `WebAuthenticator` API를 사용할 수 있습니다.  ASP.NET Core 앱에서 사용하려면 먼저 다음 단계를 사용하여 웹앱을 구성해야 합니다.

1. ASP.NET Core 웹앱에 원하는 [외부 소셜 인증 공급자](https://docs.microsoft.com/aspnet/core/security/authentication/social/?view=aspnetcore-3.1&tabs=visual-studio)를 설치합니다.
2. `.AddAuthentication()` 호출에서 기본 인증 체계를 `CookieAuthenticationDefaults.AuthenticationScheme`으로 설정합니다.
3. Startup.cs `.AddAuthentication()` 호출에서 `.AddCookie()`를 사용합니다.
4. `.SaveTokens = true;`를 사용하여 모든 공급자를 구성해야 합니다.

> [!TIP]
> Apple 로그인을 포함하려는 경우 `AspNet.Security.OAuth.Apple` NuGet 패키지를 사용할 수 있습니다.  Essentials GitHub 리포지토리에서 전체 [Startup.cs 샘플](https://github.com/xamarin/Essentials/blob/develop/Samples/Sample.Server.WebAuthenticator/Startup.cs#L32-L60)을 볼 수 있습니다.

### <a name="add-a-custom-mobile-auth-controller"></a>사용자 지정 모바일 인증 컨트롤러 추가

모바일 인증 흐름에서는 사용자가 선택한 공급자(예: 앱의 로그인 화면에서 "Microsoft" 단추 클릭)로 직접 흐름을 시작하는 것이 좋습니다.  또한 특정 콜백 URI에서 애플리케이션으로 관련 정보를 반환하여 인증 흐름을 종료할 수 있어야 하는 것도 중요합니다.

이를 위해 사용자 지정 API 컨트롤러를 사용합니다.

```csharp
[Route("mobileauth")]
[ApiController]
public class AuthController : ControllerBase
{
    const string callbackScheme = "myapp";

    [HttpGet("{scheme}")] // eg: Microsoft, Facebook, Apple, etc
    public async Task Get([FromRoute]string scheme)
    {
        // 1. Initiate authentication flow with the scheme (provider)
        // 2. When the provider calls back to this URL
        //    a. Parse out the result
        //    b. Build the app callback URL
        //    c. Redirect back to the app
    }
}
```

이 컨트롤러의 목적은 앱이 요청하는 체계(공급자)를 유추하고 해당 소셜 공급자를 사용하여 인증 흐름을 시작하는 것입니다. 공급자가 웹 백 엔드로 콜백되면 컨트롤러는 결과를 구문 분석하고 매개 변수를 사용하여 앱의 콜백 URI로 리디렉션합니다.

경우에 따라 공급자의 `access_token`과 같은 데이터를 앱으로 다시 반환해야 할 수 있는데, 이 작업은 콜백 URI의 쿼리 매개 변수를 통해 수행할 수 있습니다. 또는 서버에서 고유한 ID를 만들고 앱에 자체 토큰을 다시 전달할 수도 있습니다. 어떤 작업을 수행할지, 어떤 방법을 사용할지는 개발자가 결정합니다.

Essentials 리포지토리에서 [전체 컨트롤러 샘플](https://github.com/xamarin/Essentials/blob/develop/Samples/Sample.Server.WebAuthenticator/Controllers/MobileAuthController.cs)을 확인하세요.

-----
## <a name="api"></a>API

- [WebAuthenticator 소스 코드](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/WebAuthenticator)
- [WebAuthenticator API 설명서](xref:Xamarin.Essentials.WebAuthenticator)
- [ASP.NET Core 서버 샘플](https://github.com/xamarin/Essentials/blob/develop/Samples/Sample.Server.WebAuthenticator/)
