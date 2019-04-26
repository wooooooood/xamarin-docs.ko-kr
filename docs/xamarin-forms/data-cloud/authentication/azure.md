---
title: Azure Mobile Apps 사용 하 여 사용자 인증
description: 이 문서에서는 Azure Mobile Apps를 사용 하 여 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: D50D6F56-8B19-44E7-81F3-E0E1C6E240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 428e536d6895ff16a928f8cc40a8a7976d087471
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61331324"
---
# <a name="authenticating-users-with-azure-mobile-apps"></a>Azure Mobile Apps 사용 하 여 사용자 인증

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)

_Azure Mobile Apps 인증 및 Facebook, Google, Microsoft, Twitter 및 Azure Active Directory를 포함 하 여 응용 프로그램 사용자 권한 부여를 지원 하기 위해 다양 한 외부 id 공급자를 사용 합니다. 사용 권한은 인증 된 사용자만 액세스를 제한 하려면 테이블에서 설정할 수 있습니다. 이 문서에서는 Azure Mobile Apps를 사용 하 여 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 방법에 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 Azure Mobile Apps를 보유 한 프로세스는 다음과 같습니다.

1. Id 공급자 사이트에서 Azure 모바일 앱을 등록 하 고 Mobile Apps 백 엔드에서 공급자가 생성 한 자격 증명을 설정 합니다. 자세한 내용은 [인증을 위한 앱 등록 및 App Services 구성](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#register-your-app-for-authentication-and-configure-app-services)합니다.
1. 인증 프로세스가 완료 되 면 Xamarin.Forms 응용 프로그램으로 다시 리디렉션될 수 인증 시스템을 통해 Xamarin.Forms 응용 프로그램에 대 한 새로운 URL 체계를 정의 합니다. 자세한 내용은 [외부 리디렉션 Url 허용에 앱 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#redirecturl)합니다.
1. 클라이언트를 인증 된만에 Azure Mobile Apps 백 엔드에 액세스를 제한 합니다. 자세한 내용은 [사용 권한을 인증 된 사용자로 제한](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#restrict-permissions-to-authenticated-users)합니다.
1. Xamarin.Forms 응용 프로그램에서 인증을 호출 합니다. 자세한 내용은 [이식 가능한 클래스 라이브러리에 인증 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-portable-class-library)를 [iOS 앱에 인증 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-ios-app)를 [Android 앱에 인증 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-android-app), 및 [ Windows 10 앱 프로젝트에 인증 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-windows-10-including-phone-app-projects)합니다.

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결 하므로 중요 한 정보가 실수로 유출 방지 적용 합니다. ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되므로 모든 연결이 ATS 보안 요구 사항이 적용 됩니다. 연결에서 이러한 요구를 충족 하지 않는, 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 ATS 옵트인 수 있습니다는 `HTTPS` 프로토콜 및 인터넷 리소스에 대 한 통신을 보호 합니다. 이 앱을 업데이트 하 여 수행할 수 있습니다 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

지금까지 모바일 응용 프로그램 id 공급자를 사용 하 여 인증을 수행 하는를 포함 된 웹 보기를 사용 했습니다. 이 다음과 같은 이유로 권장 이상:

- 웹 보기를 호스팅하는 응용 프로그램에는 사용자의 전체 인증 자격 증명, 권한 부여 뿐 아니라 응용 프로그램 의도 액세스할 수 있습니다. 이 응용 프로그램에 필요한 응용 프로그램의 공격 노출 영역을 향상 시키는 것 보다 더 강력한 자격 증명에 대 한 액세스를 최소 권한의 원칙을 위반 합니다.
- 호스트 응용 프로그램 수 사용자 이름 및 암호, 자동으로 폼을 제출 및 사용자 동의 무시 및 세션 쿠키를 복사 캡처하고 사용자로 인증 된 작업을 수행 하 합니다.
- 포함 된 웹 보기는 다른 응용 프로그램 또는 사용자는 첨자 사용자 환경을 간주 되는 모든 권한 부여 요청에 대 한 로그인 하도록 요구 되는 장치의 웹 브라우저를 사용 하 여 인증 상태를 공유 하지 마세요.
- 일부 권한 부여 끝점 검색 웹 보기에서 제공 되는 권한 부여 요청을 차단 하는 단계를 수행 합니다.

장치의 웹 브라우저를 사용 하 여 최신 버전의 해킹 하는 인증을 수행 하는 합니다 [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)합니다. 장치 브라우저를 사용 하 여 인증 요청에 대 한 사용자만 응용 프로그램에서 로그인 및 권한 부여 흐름의 전환율 개선 장치에 한 번씩 id 공급자 로그인에 필요한 응용 프로그램의 유용성이 향상 됩니다. 또한 장치 브라우저 응용 프로그램을 검사 하 고 콘텐츠는 웹 뷰의 있지만 브라우저에 표시 되지 않습니다의 콘텐츠를 수정할 수는 향상 된 보안을 제공 합니다.

## <a name="using-an-azure-mobile-apps-instance"></a>Azure Mobile Apps 인스턴스를 사용 하 여

[Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) 제공 된 `MobileServiceClient` Azure Mobile Apps 인스턴스에 액세스 하기 위해 Xamarin.Forms 응용 프로그램에서 사용 되는 클래스입니다.

샘플 응용 프로그램 사용자가 Google 계정 Xamarin.Forms 응용 프로그램에 로그인 할 수 있는 id 공급자로 Google을 사용 합니다. 이 문서를 id 공급자로 Google을 사용, 방법은 다른 id 공급자에 모두 적용 합니다.

<a name="logging-in" />

### <a name="logging-in-users"></a>사용자 로그인

다음 스크린샷에서 샘플 응용 프로그램에서 로그인 화면이 표시 됩니다.

![](azure-images/login.png "로그인 페이지")

Google을 id 공급자로 사용, Facebook, Microsoft, Twitter 및 Azure Active Directory를 포함 하 여, 다양 한 다른 id 공급자를 사용할 수 있습니다.

다음 코드 예제에서는 로그인 프로세스를 호출 하는 방법을 보여 줍니다.

```csharp
async void OnLoginButtonClicked(object sender, EventArgs e)
{
  ...
  if (App.Authenticator != null)
  {
    authenticated = await App.Authenticator.AuthenticateAsync();
  }
  ...
}
```

합니다 `App.Authenticator` 속성이 `IAuthenticate` 각 플랫폼별 프로젝트에 의해 설정 된 인스턴스. 합니다 `IAuthenticate` 인터페이스를 지정는 `AuthenticateAsync` 각 플랫폼 프로젝트에서 제공 해야 하는 작업입니다. 따라서 호출을 `App.Authenticator.AuthenticateAsync` 메서드가 실행을 `IAuthenticate.AuthenticateAsync` 플랫폼 프로젝트에서 메서드.

플랫폼의 모든 `IAuthenticate.AuthenticateAsync` 메서드를 호출 합니다 `MobileServiceClient.LoginAsync` 로그인 인터페이스 및 캐시 데이터를 표시 하는 방법입니다. 다음 코드 예제는 `LoginAsync` iOS 플랫폼용 메서드:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    UIApplication.SharedApplication.KeyWindow.RootViewController,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

다음 코드 예제는 `LoginAsync` Android 플랫폼에 대 한 메서드:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    this,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

다음 코드 예제는 `LoginAsync` 유니버설 Windows 플랫폼에 대 한 메서드:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

모든 플랫폼에서의 `MobileServiceAuthenticationProvider` 열거형은 인증 프로세스에서 사용 되는 id 공급자를 지정 하는 데 사용 됩니다. 경우는 `MobileServiceClient.LoginAsync` 메서드가 호출 되 면 선택한 공급자의 로그인 페이지를 표시 하 고 id 공급자를 사용 하 여 로그인이 성공한 후 인증 토큰을 생성 하 여 Azure Mobile Apps는 인증 흐름을 시작 합니다. `MobileServiceClient.LoginAsync` 메서드가 반환 되는 `MobileServiceUser` 에 저장 되는 인스턴스는 `MobileServiceClient.CurrentUser` 속성입니다. 이 속성은 제공 `UserId` 고 `MobileServiceAuthenticationToken` 속성입니다. 이러한 인증된 된 사용자 및 사용자에 대 한 인증 토큰을 나타냅니다. Xamarin.Forms 응용 프로그램에서 Azure 모바일 앱 인스턴스가 인증 된 사용자 권한이 필요한 작업을 수행할 수 있도록 Azure Mobile Apps 인스턴스에 대 한 모든 요청에서 인증 토큰이 포함 됩니다.

<a name="logging-out" />

### <a name="logging-out-users"></a>로그 아웃 사용자

다음 코드 예제에서는 로그 아웃 프로세스를 호출 하는 방법을 보여 줍니다.

```csharp
async void OnLogoutButtonClicked(object sender, EventArgs e)
{
  bool loggedOut = false;

  if (App.Authenticator != null)
  {
    loggedOut = await App.Authenticator.LogoutAsync ();
  }
  ...
}
```

합니다 `App.Authenticator` 속성이 `IAuthenticate` 각 platformproject로 설정 된 인스턴스. 합니다 `IAuthenticate` 인터페이스를 지정는 `LogoutAsync` 각 플랫폼 프로젝트에서 제공 해야 하는 작업입니다. 따라서 호출을 `App.Authenticator.LogoutAsync` 메서드가 실행을 `IAuthenticate.LogoutAsync` 플랫폼 프로젝트에서 메서드.

플랫폼의 모든 `IAuthenticate.LogoutAsync` 메서드를 호출 합니다 `MobileServiceClient.LogoutAsync` id 공급자를 사용 하 여에 로그인 한 사용자 인증을 해제 하는 방법입니다. 다음 코드 예제는 `LogoutAsync` iOS 플랫폼용 메서드:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  foreach (var cookie in NSHttpCookieStorage.SharedStorage.Cookies)
  {
    NSHttpCookieStorage.SharedStorage.DeleteCookie (cookie);
  }
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

다음 코드 예제는 `LogoutAsync` Android 플랫폼에 대 한 메서드:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  CookieManager.Instance.RemoveAllCookie();
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

다음 코드 예제는 `LogoutAsync` 유니버설 Windows 플랫폼에 대 한 메서드:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

경우는 `IAuthenticate.LogoutAsync` 메서드가 호출 되 면 id 공급자에서 설정한 모든 쿠키 취소 하기 전에 `MobileServiceClient.LogoutAsync` 메서드를 호출 하는 id 공급자를 사용 하 여에 로그인 한 사용자 인증을 해제 합니다.

## <a name="summary"></a>요약

이 문서에서는 Azure Mobile Apps를 사용 하 여 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 방법을 설명 합니다. Azure Mobile Apps 인증 및 Facebook, Google, Microsoft, Twitter 및 Azure Active Directory를 포함 하 여 응용 프로그램 사용자 권한 부여를 지원 하기 위해 다양 한 외부 id 공급자를 사용 합니다. `MobileServiceClient` 클래스는 Azure Mobile Apps 인스턴스에 대 한 액세스를 제어 하려면 Xamarin.Forms 응용 프로그램에서 사용 됩니다.


## <a name="related-links"></a>관련 링크

- [TodoAzureAuth (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)
- [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Xamarin.Forms 앱에 인증 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
