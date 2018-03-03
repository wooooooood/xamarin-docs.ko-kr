---
title: "Graph API 액세스"
description: "Active Directory를 사용 하 여 Xamarin을 사용 하 여 Graph API를 쿼리할 수"
ms.topic: article
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 2b0e4a9466ab59ec27b957bf284a7d3895f6a4fc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="accessing-the-graph-api"></a>Graph API 액세스

_Active Directory를 사용 하 여 Xamarin을 사용 하 여 Graph API를 쿼리할 수_

Xamarin 응용 프로그램 내에서 Graph API를 사용 하려면 다음이 단계를 수행 합니다.

1. [Azure Active Directory에 등록](~/cross-platform/data-cloud/active-directory/get-started/register.md) 에 *windowsazure.com* 다음 포털
2. [서비스 구성](~/cross-platform/data-cloud/active-directory/get-started/configure.md)합니다.

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>3단계. 응용 프로그램에 Active Directory 인증 추가

응용 프로그램에 대 한 참조를 추가 **Azure Active Directory 인증 라이브러리 (Azure ADAL)** Mac.에 대 한 Visual Studio 또는 Visual Studio에서 NuGet 패키지 관리자를 사용 하 여
선택 했는지 확인 **시험판 패키지를 표시** 미리 보기 상태에서 이므로이 패키지를 포함 하도록 합니다.

> [!IMPORTANT]
> 참고: Azure ADAL 3.0 현재 미리 보기 이므로 최종 버전이 릴리스될 때마다 전에 주요 변경 내용 수입니다. 


![](graph-images/06.-adal-nuget-package.jpg "Azure Active Directory 인증 라이브러리 (Azure ADAL)에 대 한 참조를 추가 합니다.")

응용 프로그램에서 이제 인증 흐름에 필요한 다음과 같은 클래스 수준 변수를 추가 해야 합니다.

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

여기서 한 가지 `commonAuthority`합니다. 인증 끝점 경우 `common`, 앱은 **다중 테 넌 트**, 모든 사용자를 의미 하는 Active Directory 자격 증명으로 로그인을 사용할 수 있습니다. 인증을 받은 후 해당 사용자는 자신의 Active Directory의 컨텍스트에서 작동-즉, 자신의 Active Directory와 관련 된 세부 정보를 볼 수 있습니다.

### <a name="write-method-to-acquire-access-token"></a>액세스 토큰 획득을 메서드 작성

(Android)에 대 한 다음 코드는 인증을 시작 하 고 완료 시의 결과 할당 `authResult`합니다. IOS 및 Windows Phone 구현은 약간 다: 두 번째 매개 변수 (`Activity`) iOS 달라 지는 부재 및 Windows Phone 합니다.

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

위의 코드에서의 `AuthenticationContext` commonAuthority 사용 하 여 인증을 담당 합니다. 에 `AcquireTokenAsync` 메서드 매개 변수로 사용이 경우 액세스 되어야 하는 리소스를 `graphResourceUri`, `clientId`, 및 `returnUri`합니다. 앱으로 돌아갑니다.는 `returnUri` 인증을 완료 합니다. 그러나이 코드에 대 한 모든 플랫폼에 마지막 매개 변수 동일 하 게 유지 됩니다 `AuthorizationParameters`, 서로 다른 플랫폼에서 다르게 됩니다 및 인증 흐름을 관리를 담당 합니다.

Android 또는 iOS의 경우 전달 `this` 매개 변수를 `AuthorizationParameters(this)` 컨텍스트를 공유 하는, Windows에서 변수로 전달 될 매개 변수 없이 새 반면 `AuthorizationParameters()`합니다.

### <a name="handle-continuation-for-android"></a>Android에 대 한 연속 작업을 처리 합니다.

인증이 완료 되 면 여 흐름 응용 프로그램에 반환 해야 합니다. 다음 코드에서 처리 하는 Android의 경우는에 추가 해야 **MainActivity.cs**:


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);

    
}
```

### <a name="handle-continuation-for-windows-phone"></a>Windows Phone 대 한 연속 작업을 처리 합니다.

Windows Phone 대 한 수정 된 `OnActivated` 에서 메서드는 **App.xaml.cs** 파일는 아래 코드:

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

이제 응용 프로그램을 실행 하는 경우는 인증 대화 상자가 표시 됩니다.
인증이 성공 (이 경우 Graph API)에서 리소스에 액세스할 수 권한을 묻는 메시지가:

![](graph-images/08.-authentication-flow.jpg "인증 성공 시 이름을 묻는 경우에는 Graph API의 리소스에 액세스할 권한을")

인증이 성공 하 고 응용 프로그램에서 리소스에 액세스할 권한이 부여 된, 얻어야 하는 경우는 `AccessToken` 및 `RefreshToken` 콤보에서 `authResult`합니다. 이러한 토큰에는 다른 API 호출 및 원리를 자세히 파악할수록 Azure Active Directory와 권한 부여에 필요 합니다.

![](graph-images/07.-access-token-for-authentication.jpg "이러한 토큰에는 다른 API 호출 및 원리를 자세히 파악할수록 Azure Active Directory와 권한 부여에 필요")

예를 들어 아래 코드에서 Active Directory 사용자 목록을 가져올 수 있습니다. Azure AD에서 보호 된 웹 API와 웹 API URL을 대체할 수 있습니다.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

