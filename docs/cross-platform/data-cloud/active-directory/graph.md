---
title: Graph API에 액세스
description: 이 문서에서는 Xamarin을 사용 하 여 빌드된 모바일 응용 프로그램에 Azure Active Directory 인증을 추가 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: 96e0991bb0805e61dfbf91e8479cbf1c9943f212
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70287753"
---
# <a name="accessing-the-graph-api"></a>Graph API에 액세스

Xamarin 응용 프로그램 내에서 Graph API를 사용 하려면 다음 단계를 수행 합니다.

1. *Windowsazure.com* 포털에서 [Azure Active Directory 등록](~/cross-platform/data-cloud/active-directory/get-started/register.md)
2. [서비스를 구성](~/cross-platform/data-cloud/active-directory/get-started/configure.md)합니다.

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>3단계. 앱에 Active Directory 인증 추가

응용 프로그램에서 Visual Studio의 NuGet 패키지 관리자 또는 Mac용 Visual Studio를 사용 하 여 **AZURE ADAL (Azure Active Directory Authentication Library)** 에 대 한 참조를 추가 합니다.
미리 보기 상태 이므로이 패키지를 포함 하려면 **시험판 패키지 표시** 를 선택 해야 합니다.

> [!IMPORTANT]
> 참고: Azure ADAL 3.0은 현재 미리 보기로 제공 되며 최종 버전을 출시 하기 전에 주요 변경 내용이 있을 수 있습니다. 


![](graph-images/06.-adal-nuget-package.jpg "Azure Active Directory 인증 라이브러리에 대 한 참조 추가 (Azure ADAL)")

이제 응용 프로그램에서 인증 흐름에 필요한 다음과 같은 클래스 수준 변수를 추가 해야 합니다.

```csharp
//Client ID
public static string clientId = "25927d3c-.....-63f2304b90de";
public static string commonAuthority = "https://login.windows.net/common"
//Redirect URI
public static Uri returnUri = new Uri("http://xam-demo-redirect");
//Graph URI if you've given permission to Azure Active Directory
const string graphResourceUri = "https://graph.windows.net";
public static string graphApiVersion = "2013-11-08";
//AuthenticationResult will hold the result after authentication completes
AuthenticationResult authResult = null;
```

한 가지 주의할 점은 `commonAuthority`입니다. 인증 끝점이 인 경우 앱 `common`은 **다중 테 넌 트**로 사용 됩니다. 즉, 사용자가 Active Directory 자격 증명으로 로그인을 사용할 수 있습니다. 인증 후 해당 사용자는 자신의 Active Directory의 컨텍스트에서 작업 하 게 됩니다. 즉, Active Directory와 관련 된 세부 정보를 볼 수 있습니다.

### <a name="write-method-to-acquire-access-token"></a>액세스 토큰을 획득 하는 Write 메서드

다음 코드 (Android의 경우)는 인증을 시작 하 고 완료 되 면에서 `authResult`결과를 할당 합니다. Ios 및 Windows Phone 구현은 약간 다릅니다. 두 번째 매개 변수 (`Activity`)는 ios에서 다르며 Windows Phone에는 없습니다.

```csharp
public static async Task<AuthenticationResult> GetAccessToken
            (string serviceResourceId, Activity activity)
{
    authContext = new AuthenticationContext(Authority);
    if (authContext.TokenCache.ReadItems().Count() > 0)
        authContext = new AuthenticationContext(authContext.TokenCache.ReadItems().First().Authority);
    var authResult = await authContext.AcquireTokenAsync(serviceResourceId, clientId, returnUri, new AuthorizationParameters(activity));
    return authResult;
}  
```

위의 코드 `AuthenticationContext` 에서은 commonAuthority를 사용 하 여 인증을 담당 합니다. 이 클래스에 `AcquireTokenAsync` 는 액세스 해야 하는 리소스 (이 `clientId`경우 `graphResourceUri`, 및 `returnUri`)로 매개 변수를 사용 하는 메서드가 있습니다. 인증이 완료 `returnUri` 되 면 앱이로 돌아갑니다. 이 코드는 모든 플랫폼에서 동일 하 게 유지 되지만 마지막 매개 변수 `AuthorizationParameters`는 플랫폼 마다 다르며 인증 흐름을 관리 하는 일을 담당 합니다.

Android 또는 iOS의 경우에는 매개 변수를 `this` 로 `AuthorizationParameters(this)` 전달 하 여 컨텍스트를 공유 하는 반면 Windows에서는 매개 변수 없이 새 `AuthorizationParameters()`로 전달 됩니다.

### <a name="handle-continuation-for-android"></a>Android에 대 한 연속 처리

인증이 완료 된 후에는 흐름이 앱으로 돌아옵니다. Android의 경우 **MainActivity.cs**에 추가 해야 하는 다음 코드에 의해 처리 됩니다.


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
```

### <a name="handle-continuation-for-windows-phone"></a>Windows Phone에 대 한 연속 처리

Windows Phone 아래 코드를 `OnActivated` 사용 하 여 **App.xaml.cs** 파일에서 메서드를 수정 합니다.

```csharp
protected override void OnActivated(IActivatedEventArgs args)
{
#if WINDOWS_PHONE_APP
  if (args is IWebAuthenticationBrokerContinuationEventArgs)
  {
     WebAuthenticationBrokerContinuationHelper.SetWebAuthenticationBrokerContinuationEventArgs(args as IWebAuthenticationBrokerContinuationEventArgs);
  }
#endif
  base.OnActivated(args);
}
```

이제 응용 프로그램을 실행 하면 인증 대화 상자가 표시 됩니다.
인증에 성공 하면 리소스에 액세스할 수 있는 권한을 요청 합니다 (이 경우 Graph API).

![](graph-images/08.-authentication-flow.jpg "인증에 성공 하면 해당 사례에서 리소스에 대 한 액세스 권한을 요청 하 게 됩니다 Graph API")

인증에 성공 하 고 리소스에 액세스 하도록 앱에 권한을 부여한 경우에서 `AccessToken` `authResult`및 `RefreshToken` 콤보를 가져와야 합니다. 이러한 토큰은 추가 API 호출에 필요 하며 백그라운드에서 Azure Active Directory 권한 부여를 위해 필요 합니다.

![](graph-images/07.-access-token-for-authentication.jpg "이러한 토큰은 추가 API 호출에 필요 하며 백그라운드에서 Azure Active Directory 권한 부여를 위해 필요 합니다.")

예를 들어 아래 코드를 사용 하 여 Active Directory에서 사용자 목록을 가져올 수 있습니다. Web API URL을 Azure AD로 보호 되는 Web API로 바꿀 수 있습니다.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

