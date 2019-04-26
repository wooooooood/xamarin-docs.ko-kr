---
title: Graph API에 액세스
description: 이 문서에서는 Xamarin으로 빌드된 모바일 응용 프로그램에 Azure Active Directory 인증을 추가 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c43dfa79831f22e55490b27c3c360602ae717627
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61190029"
---
# <a name="accessing-the-graph-api"></a>Graph API에 액세스

Xamarin 응용 프로그램 내에서 Graph API를 사용 하려면 다음이 단계를 수행 합니다.

1. [Azure Active Directory를 사용 하 여 등록](~/cross-platform/data-cloud/active-directory/get-started/register.md) 에 *windowsazure.com* 다음 포털
2. [서비스 구성](~/cross-platform/data-cloud/active-directory/get-started/configure.md)합니다.

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>3단계. 앱에 Active Directory 인증 추가

응용 프로그램에 대 한 참조를 추가 **Azure Active Directory 인증 라이브러리 (Azure ADAL)** mac 용 Visual Studio 또는 Visual Studio에서 NuGet 패키지 관리자를 사용 하 여
선택 했는지 **시험판 패키지 표시** 미리 보기 상태에서 이므로이 패키지를 포함 합니다.

> [!IMPORTANT]
> 참고: Azure ADAL 3.0은 현재 미리 보기 및 최종 버전 해제 되기 전에 주요 변경 내용 수입니다. 


![](graph-images/06.-adal-nuget-package.jpg "Azure Active Directory 인증 라이브러리 (Azure ADAL)에 대 한 참조를 추가 합니다.")

응용 프로그램에 인증 흐름에 필요한 다음 클래스 수준 변수를 추가 하려면 이제 해야 합니다.

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

여기서 한 가지 `commonAuthority`합니다. 인증 끝점의 경우 `common`를 통해 앱이 **다중 테 넌 트**, 즉, 모든 사용자가 Active Directory 자격 증명을 사용 하 여 로그인을 사용할 수 있습니다. 인증 후 사용자가 자신의 Active Directory의 컨텍스트에서 작동 합니다. – 즉, 자신의 Active Directory와 관련 된 세부 정보 표시 됩니다.

### <a name="write-method-to-acquire-access-token"></a>액세스 토큰을 획득 하는 메서드를 작성 합니다.

(Android)에 대 한 다음 코드는 인증을 시작 하 고 완료 시의 결과 할당 `authResult`합니다. IOS 및 Windows Phone 구현 마다 약간 다: 두 번째 매개 변수 (`Activity`)은 iOS에서 서로 다르며 없기 Windows Phone.

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

위의 코드에서의 `AuthenticationContext` commonAuthority 사용 하 여 인증을 담당 합니다. 에 `AcquireTokenAsync` 이 경우에 액세스 해야 하는 리소스와 매개 변수를 사용 하는 메서드를 `graphResourceUri`를 `clientId`, 및 `returnUri`합니다. 앱 다시 돌아갈 것은 `returnUri` 인증이 완료 되는 경우. 그러나이 코드는 모든 플랫폼, 마지막 매개 변수를 그대로 유지 됩니다 `AuthorizationParameters`, 다양 한 플랫폼에서 다르게 됩니다 및 인증 흐름을 제어 하기 위해 일을 담당 합니다.

Android 또는 iOS의 경우 전달 `this` 매개 변수를 `AuthorizationParameters(this)` 컨텍스트를 공유 하는, Windows에서 변수로 전달 된 매개 변수 없이 새 반면 `AuthorizationParameters()`합니다.

### <a name="handle-continuation-for-android"></a>Android에 대 한 연속 작업을 처리 합니다.

인증 완료 되 면 흐름은 앱에 반환 해야 합니다. 다음 코드에서 처리 하는 Android의 경우는 추가할지 **MainActivity.cs**:


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);

    
}
```

### <a name="handle-continuation-for-windows-phone"></a>Windows Phone 대 한 연속 작업을 처리 합니다.

Windows Phone 대 한 수정 합니다 `OnActivated` 에서 메서드를 **App.xaml.cs** 파일을 아래 코드:

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

이제 응용 프로그램을 실행 하는 경우 인증 대화 상자를 표시 됩니다.
인증이 성공 하면 (이 경우 Graph API)에서 리소스에 액세스 권한을 요청 합니다.

![](graph-images/08.-authentication-flow.jpg "인증이 성공 하면이 경우 Graph API의에서 리소스에 액세스할 수 있는 권한이 요청")

인증이 성공 하는 리소스에 액세스 하려면 앱을 승인한를 가져와야 하는 경우는 `AccessToken` 하 고 `RefreshToken` 콤보에서 `authResult`합니다. 이러한 토큰은 백그라운드에서 Azure Active Directory를 사용 하 여 권한 부여 및 추가 API 호출에 필요 합니다.

![](graph-images/07.-access-token-for-authentication.jpg "이러한 토큰은 API 호출에 추가 하 고 백그라운드에서 Azure Active Directory를 사용 하 여 권한 부여 필요")

예를 들어 아래 코드를 사용 하면 Active Directory에서 사용자 목록을 가져올 수 있습니다. Azure AD에서 보호 되는 Web API를 사용 하 여 Web API URL을 바꿀 수 있습니다.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

