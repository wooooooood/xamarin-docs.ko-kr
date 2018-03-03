---
title: "Azure 모바일 앱을 사용한 사용자 인증"
description: "Azure 모바일 앱 인증 및 Facebook, Google, Microsoft, Twitter 및 Azure Active Directory를 포함 한 응용 프로그램 사용자 권한 부여를 지원 하기 위해 다양 한 외부 id 공급자를 사용 합니다. 권한은 인증 된 사용자만 액세스를 제한 하려면 테이블에 설정할 수 있습니다. 이 문서에서는 Azure 모바일 앱을 사용 하 여 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: D50D6F56-8B19-44E7-81F3-E0E1C6E240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 823dcdfdaca79045a407b62ec7e75079ee25d72f
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="authenticating-users-with-azure-mobile-apps"></a>Azure 모바일 앱을 사용한 사용자 인증

_Azure 모바일 앱 인증 및 Facebook, Google, Microsoft, Twitter 및 Azure Active Directory를 포함 한 응용 프로그램 사용자 권한 부여를 지원 하기 위해 다양 한 외부 id 공급자를 사용 합니다. 권한은 인증 된 사용자만 액세스를 제한 하려면 테이블에 설정할 수 있습니다. 이 문서에서는 Azure 모바일 앱을 사용 하 여 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 Azure 모바일 앱을 보유 한 프로세스는 다음과 같습니다.

1. Id 공급자의 사이트에서 Azure 모바일 앱을 등록 하 고 모바일 앱 백 엔드에 공급자가 생성 한 자격 증명을 설정 합니다. 자세한 내용은 참조 [인증에 대 한 앱을 등록 하 고 응용 프로그램 서비스 구성](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#register-your-app-for-authentication-and-configure-app-services)합니다.
1. 인증 프로세스가 완료 되 면 Xamarin.Forms 응용 프로그램으로 다시 리디렉션할 인증 시스템을 통해 Xamarin.Forms 응용 프로그램에 대 한 새 URL 체계를 정의 합니다. 자세한 내용은 참조 [외부 리디렉션 Url 허용에 앱 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#redirecturl)합니다.
1. 클라이언트를 인증 된만에 Azure 모바일 앱 백 엔드에 액세스를 제한 합니다. 자세한 내용은 참조 [인증 된 사용자에 게 사용 권한을 제한](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#restrict-permissions-to-authenticated-users)합니다.
1. Xamarin.Forms 응용 프로그램에서 인증을 호출 합니다. 자세한 내용은 참조 [이식 가능한 클래스 라이브러리에 인증 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-portable-class-library), [iOS 앱에 인증 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-ios-app), [Android 앱에 인증 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-android-app), 및 [ Windows 10 앱 프로젝트에 인증 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-windows-10-including-phone-app-projects)합니다.

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간에 보안 연결 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 적용 합니다. IOS 9에 대해 빌드된 앱에서 기본적으로 AT를 사용 하므로 모든 연결 AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 AT 옵트인 수 있습니다는 `HTTPS` 및 프로토콜을 인터넷 리소스에 대 한 통신을 보호 합니다. 응용 프로그램의 업데이트 하 여 이렇게 할 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

지금까지, 모바일 응용 프로그램 id 공급자와 인증을 수행 하도록 포함 된 웹 보기를 사용 했습니다. 이 다음과 같은 이유로 더 이상 권장 됩니다.

- 웹 보기를 호스팅하는 응용 프로그램 사용자의 전체 인증 자격 증명을 응용 프로그램에 대 한 원래 인증 권한 뿐 아니라 액세스할 수 있습니다. 이 방법은 응용 프로그램에 필요한 응용 프로그램의 공격 노출 영역을 향상 시키는 것 보다 더 강력한 자격 증명에 액세스할 때 최소 권한의 원칙을 위반 합니다.
- 호스트 응용 프로그램 수 사용자 이름 및 암호를 캡처, 자동으로 양식을 전송할 사용자 동의 무시 하 고 세션 쿠키 및 사용자로 인증 된 작업을 수행 하는 데 사용할 합니다.
- 포함 된 웹 보기 밑 사용자 환경을 간주 되는 모든 권한 부여 요청에 대 한 로그인에 사용자 장치의 웹 브라우저 또는 다른 응용 프로그램 인증 상태를 공유 하지 않습니다.
- 일부 권한 부여 끝점 감지 웹 보기에서 제공 하는 권한 부여 요청을 차단 하는 단계를 수행 합니다.

대신 사용 하는 최신 버전의 해킹 하는 인증을 수행 하는 장치의 웹 브라우저를 사용 하 여 [Azure 모바일 클라이언트 SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)합니다. 장치 브라우저를 사용 하 여 인증 요청에 대 한 사용자만의 응용 프로그램에서 로그인 및 권한 부여 흐름 변환율 향상 장치에 한 번씩 id 공급자에 로그인 해야 할 때 응용 프로그램의 유용성이 향상 됩니다. 장치 브라우저 응용 프로그램은 검사 하 고 브라우저에 표시 하는 콘텐츠 하지는 웹 보기의 콘텐츠를 수정할 수 향상 된 보안도 제공 합니다.

## <a name="using-an-azure-mobile-apps-instance"></a>Azure 모바일 응용 프로그램 인스턴스를 사용 하 여

[Azure 모바일 클라이언트 SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) 제공는 `MobileServiceClient` Azure 모바일 앱 인스턴스에 액세스 하는 Xamarin.Forms 응용 프로그램에서 사용 되는 클래스입니다.

예제 응용 프로그램 id 공급자로 Google Xamarin.Forms 응용 프로그램에 로그인 하는 계정 가진 사용자가 Google을 사용 합니다. Google이이 문서에서 id 공급자로으로 사용 되지만 방법은 다른 id 공급자에 모두 적용 합니다.

<a name="logging-in" />

### <a name="logging-in-users"></a>사용자 로그인

다음 스크린샷에서 샘플 응용 프로그램에서 로그인 화면을 보여 줍니다.

![](azure-images/login.png "로그인 페이지")

Google을 id 공급자로 사용, Facebook, Microsoft, Twitter 및 Azure Active Directory를 포함 하 여, 기타 id 공급자의 다양 한 사용할 수 있습니다.

다음 코드 예제에서는 로그인 프로세스는 호출 하는 방법을 보여 줍니다.

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

`App.Authenticator` 속성은 한 `IAuthenticate` 각 플랫폼별 프로젝트에 의해 설정 된 인스턴스. `IAuthenticate` 인터페이스 지정는 `AuthenticateAsync` 각 플랫폼 프로젝트에서 제공 되어야 하는 작업입니다. 따라서 호출 하는 `App.Authenticator.AuthenticateAsync` 메서드가 실행 되는 `IAuthenticate.AuthenticateAsync` 플랫폼 프로젝트에서 메서드.

플랫폼의 모든 `IAuthenticate.AuthenticateAsync` 메서드 호출에서 `MobileServiceClient.LoginAsync` 메서드 로그인 인터페이스 및 캐시 데이터를 표시 합니다. 다음 코드 예제는 `LoginAsync` iOS 플랫폼에 대 한 메서드:

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

모든 플랫폼에서의 `MobileServiceAuthenticationProvider` 열거형 인증 프로세스에서 사용 되는 id 공급자를 지정 하는 데 사용 됩니다. 경우는 `MobileServiceClient.LoginAsync` 메서드가 호출 되 면 선택한 공급자의 로그인 페이지를 표시 하 고 id 공급자와 성공적으로 로그인 한 후 인증 토큰을 생성 하 여 Azure 모바일 앱 인증 흐름을 시작 합니다. `MobileServiceClient.LoginAsync` 메서드가 반환 되는 `MobileServiceUser` 에 저장 되는 인스턴스에 `MobileServiceClient.CurrentUser` 속성입니다. 이 속성은 제공 `UserId` 및 `MobileServiceAuthenticationToken` 속성입니다. 이러한 인증된 된 사용자 및 사용자에 대 한 인증 토큰을 나타냅니다. 인증 토큰 Xamarin.Forms 응용 프로그램에서 인증 된 사용자 권한이 있어야 하는 Azure 모바일 앱 인스턴스 작업을 수행할 수 있도록 하는 Azure 모바일 앱 인스턴스에 대 한 모든 요청에 포함 됩니다.

<a name="logging-out" />

### <a name="logging-out-users"></a>사용자가 로깅

다음 코드 예제에서는 로그 아웃 프로세스는 호출 하는 방법을 보여 줍니다.

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

`App.Authenticator` 속성은 한 `IAuthenticate` 각 platformproject로 설정 된 인스턴스. `IAuthenticate` 인터페이스 지정는 `LogoutAsync` 각 플랫폼 프로젝트에서 제공 되어야 하는 작업입니다. 따라서 호출 하는 `App.Authenticator.LogoutAsync` 메서드가 실행 되는 `IAuthenticate.LogoutAsync` 플랫폼 프로젝트에서 메서드.

플랫폼의 모든 `IAuthenticate.LogoutAsync` 메서드 호출에서 `MobileServiceClient.LogoutAsync` 메서드를 id 공급자와에 로그인 한 사용자의 인증을 해제 합니다. 다음 코드 예제는 `LogoutAsync` iOS 플랫폼에 대 한 메서드:

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

경우는 `IAuthenticate.LogoutAsync` 메서드가 호출 되 면 id 공급자에 의해 설정 된 모든 쿠키의 선택을 취소 하기 전에 `MobileServiceClient.LogoutAsync` 메서드를 호출 하는 로그인 한 사용자 id 공급자와의 인증을 해제 합니다.

## <a name="summary"></a>요약

이 문서는 Azure 모바일 앱을 사용 하 여 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 방법을 설명 합니다. Azure 모바일 앱 인증 및 Facebook, Google, Microsoft, Twitter 및 Azure Active Directory를 포함 한 응용 프로그램 사용자 권한 부여를 지원 하기 위해 다양 한 외부 id 공급자를 사용 합니다. `MobileServiceClient` 클래스는 Azure 모바일 앱 인스턴스에 대 한 액세스를 제어 하는 Xamarin.Forms 응용 프로그램에서 사용 됩니다.


## <a name="related-links"></a>관련 링크

- [TodoAzureAuth (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)
- [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Xamarin.Forms 앱에 인증 추가](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
