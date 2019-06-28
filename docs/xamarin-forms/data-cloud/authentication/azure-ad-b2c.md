---
title: Azure Active Directory B2C 사용 하 여 사용자 인증
description: Azure Active Directory B2C는 소비자 지향 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리를 제공합니다. 이 문서에서는 Azure Active Directory B2C를 사용 하 여 Microsoft 인증 라이브러리를 사용 하 여 모바일 응용 프로그램에 id 관리를 통합할 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2019
ms.openlocfilehash: 06f5716c8decb21de39fd46abe734b5fdcd6bd43
ms.sourcegitcommit: 0f78ec17210b915b43ddab75937de8063e472c70
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67417943"
---
# <a name="authenticate-users-with-azure-active-directory-b2c"></a>Azure Active Directory B2C 사용 하 여 사용자 인증

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)

_Azure Active Directory B2C는 소비자 지향 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리를 제공합니다. 이 문서에서는 Azure Active Directory B2C를 사용 하 여 Microsoft 인증 라이브러리를 사용 하 여 모바일 응용 프로그램에 id 관리를 통합할 방법을 보여 줍니다._

## <a name="overview"></a>개요

Azure Active Directory B2C는 소비자 지향 응용 프로그램에 대 한 id 관리 서비스 (ADB2C). 사용자를 기존 소셜 계정 또는 전자 메일 또는 사용자 이름 및 암호와 같은 사용자 지정 자격 증명을 사용 하 여 응용 프로그램에 로그인 할 수 있습니다. 사용자 지정 자격 증명 계정 이라고 _로컬_ 계정.

모바일 응용 프로그램에 Azure Active Directory B2C id 관리 서비스를 통합 하기 위한 프로세스는 다음과 같습니다.

1. Azure Active Directory B2C 테 넌 트 만들기
1. Azure Active Directory B2C 테 넌 트를 사용 하 여 모바일 응용 프로그램 등록
1. 등록 및 로그인 정책 만들기 및 사용자 흐름 암호 잊음
1. Azure Active Directory B2C 테 넌 트를 사용 하 여 인증 워크플로 시작 하려면 Microsoft 인증 라이브러리 (MSAL)를 사용 합니다.

> [!NOTE]
> Azure Active Directory B2C는 Microsoft, GitHub, Facebook, Twitter 등을 포함 한 여러 id 공급자를 지원 합니다. Azure Active Directory B2C 기능에 대 한 자세한 내용은 참조 하세요. [Azure Active Directory B2C 설명서](/azure/active-directory-b2c/)합니다.
>
> Microsoft 인증 라이브러리는 여러 응용 프로그램 아키텍처 및 플랫폼을 지원합니다. MSAL 기능에 대 한 자세한 내용은 [Microsoft 인증 라이브러리](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) github입니다.

## <a name="configure-an-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C 테 넌 트를 구성 합니다.

샘플 프로젝트를 실행 하려면 Azure Active Directory B2C 테 넌 트를 만들어야 합니다. 자세한 내용은 [Azure portal에서 Azure Active Directory B2C 테 넌 트를 만드는](/azure/active-directory-b2c/active-directory-b2c-get-started/)합니다.

테 넌 트를 만든 후 해야 합니다 **테 넌 트 이름** 하 고 **테 넌 트 ID** 모바일 응용 프로그램을 구성 합니다. 테 넌 트 ID와 이름을 테 넌 트 URL을 만들 때 생성 된 도메인에 의해 정의 됩니다. 생성 된 테 넌 트 URL이 `https://contoso20190410tenant.onmicrosoft.com/` 는 **테 넌 트 ID** 됩니다 `contoso20190410tenant.onmicrosoft.com` 및 **테 넌 트 이름** 는 `contoso20190410tenant`합니다. 클릭 하 여 Azure portal에서 테 넌 트 도메인을 찾을 합니다 **디렉터리 및 구독 필터** 최상위 메뉴에서. 다음 스크린샷에서 Azure 디렉터리 및 구독 필터 단추 및 테 넌 트 도메인을 보여 줍니다.

[![Azure 디렉터리 및 구독 필터 보기에서 테 넌 트 이름](azure-ad-b2c-images/azure-tenant-name-cropped.png)](azure-ad-b2c-images/azure-tenant-name.png#lightbox)

샘플 프로젝트에서 편집 합니다 **Constants.cs** 설정 하기 위해 파일을 `tenantName` 및 `tenantId` 필드. 다음 코드에서는 이러한 값을 설정 해야 하는 방법을 테 넌 트 도메인 인지 `https://contoso20190410tenant.onmicrosoft.com/`, 포털에서 값을 사용 하 여 이러한 값을 대체 합니다.

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    ...
}
```

## <a name="register-your-mobile-application-with-azure-active-directory-b2c"></a>Azure Active Directory B2C를 사용 하 여 모바일 응용 프로그램 등록

모바일 응용 프로그램을 연결 하 고 사용자를 인증 하기 전에 테 넌 트를 사용 하 여 등록 되어야 합니다. 등록 프로세스에 고유한 할당 **응용 프로그램 ID** 응용 프로그램에 및 **리디렉션 URL** 는 다시 인증 한 후 응용 프로그램에 대 한 응답을 보냅니다. 자세한 내용은 참조 하세요. [Azure Active Directory B2C: 응용 프로그램을 등록](/azure/active-directory-b2c/active-directory-b2c-app-registration/)합니다. 알아야 할 됩니다는 **응용 프로그램 ID** 속성 보기의 응용 프로그램 이름 뒤에 나열 된 프로그램에 할당 합니다. 다음 스크린샷은 응용 프로그램 ID를 찾을 수 있는 위치를 보여 줍니다.

[![Azure 응용 프로그램 속성 보기에서 응용 프로그램 ID](azure-ad-b2c-images/azure-application-id-cropped.png)](azure-ad-b2c-images/azure-application-id.png#lightbox)

Microsoft Authentication Library가 필요 합니다 **리디렉션 URL** 되도록 응용 프로그램에 대 한 프로그램 **응용 프로그램 ID** 붙어 "msal" 및 "auth" 라는 끝점 뒤에. 응용 프로그램 ID "은 1234abcd" 인 경우 전체 URL 이어야 합니다 `msal1234abcd://auth`합니다. 응용 프로그램에 사용할 수 있도록 해야 합니다 **네이티브 클라이언트** 설정 하 고 만들기를 **사용자 지정 리디렉션 URI** 다음 스크린샷에 표시 된 대로 응용 프로그램 ID를 사용 하 여:

![Azure 응용 프로그램 속성 보기에서 사용자 지정 리디렉션 URI](azure-ad-b2c-images/azure-redirect-uri.png)

URL 모두 Android에서 나중에 사용할 **ApplicationManifest.xml** 및 iOS **Info.plist**합니다.

샘플 프로젝트에서 편집 합니다 **Constants.cs** 설정 하기 위해 파일을 `clientId` 필드를 **응용 프로그램 ID**. 다음 코드에서는이 값을 설정 해야 하는 방법을 응용 프로그램 ID 인지 `1234abcd`:

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    static readonly string clientId = "1234abcd";
    ...
}
```

## <a name="create-sign-up-and-sign-in-policies-and-forgot-password-policies"></a>등록 및 로그인 정책을 만드는 것을 잊은 암호 정책

정책은은 사용자가 계정을 만들거나 암호를 재설정 하는 등의 작업을 완료 하려면 통과 하는 환경입니다. 정책에는 또한 사용자 환경에서 반환 될 때 응용 프로그램에서 받는 토큰의 내용을 지정 합니다. 등록 및 로그인 계정 모두에 대 한 정책을 설정 하 고 암호를 재설정 해야 합니다. Azure에는 일반적인 정책 생성을 간소화 하는 기본 제공 정책이 있습니다. 자세한 내용은 참조 하세요. [Azure Active Directory B2C: 기본 제공 정책](/azure/active-directory-b2c/active-directory-b2c-reference-policies/)합니다.

정책 설치를 완료 한 경우 두 정책에 있어야 합니다 **사용자 흐름 (정책)** Azure 포털에서 보기. 다음 스크린샷은 Azure portal에서 구성 된 두 개의 정책을 보여 줍니다.

![Azure 사용자 흐름 (정책)에서 두 개의 구성 된 정책 보기](azure-ad-b2c-images/azure-application-policies.png)

샘플 프로젝트에서 편집 합니다 **Constants.cs** 설정 하기 위해 파일을 `policySignin` 및 `policyPassword` 정책 설치 중에 선택한 이름을 반영 하도록 필드:

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    static readonly string clientId = "1234abcd";
    static readonly string policySignin = "B2C_1_signupsignin1";
    static readonly string policyPassword = "B2C_1_passwordreset";
    ...
}
```

## <a name="use-the-microsoft-authentication-library-msal-for-authentication"></a>Microsoft 인증 라이브러리 (MSAL)를 사용 하 여 인증

Microsoft 인증 라이브러리 (MSAL) NuGet 패키지 공유,.NET Standard 프로젝트 및 Xamarin.Forms 솔루션에 플랫폼 프로젝트에 추가 되어야 합니다. MSAL 포함을 `PublicClientApplicationBuilder` 준수 하는 개체를 생성 하는 클래스는 `IPublicClientApplication` 인터페이스입니다. MSAL 활용 `With` 절 생성자 및 인증 방법 추가 매개 변수를 제공 합니다.

샘플 프로젝트에 대 한 코드 숨김에 **App.xaml** 라는 정적 속성을 정의 `AuthenticationClient` 및 `UIParent`, 인스턴스화합니다를 `AuthenticationClient` 생성자에는 개체입니다. `WithIosKeychainSecurityGroup` 절에는 iOS 응용 프로그램 보안 그룹 이름을 제공 합니다. `WithB2CAuthority` 절은 기본 제공 **기관**, 또는 사용자를 인증 하는 데 사용할 수 있는 정책입니다. 다음 예제에서는 인스턴스화하는 방법을 보여 줍니다는 `PublicClientApplication`:

```csharp
public partial class App : Application
{
    public static IPublicClientApplication AuthenticationClient { get; private set; }

    public static object UIParent { get; set; } = null;

    public App()
    {
        InitializeComponent();

        AuthenticationClient = PublicClientApplicationBuilder.Create(Constants.ClientId)
            .WithIosKeychainSecurityGroup(Constants.IosKeychainSecurityGroups)
            .WithB2CAuthority(Constants.AuthoritySignin)
            .Build();

        MainPage = new NavigationPage(new LoginPage());
    }

    ...
```

합니다 `OnAppearing` 이벤트 처리기는 **LoginPage.xaml.cs** 호출 코드가 `AcquireTokenSilentAsync` 하기 전에 로그인 하는 사용자에 대 한 인증 토큰을 새로 고치려면 합니다. 인증 프로세스를 리디렉션하는 `LogoutPage` 성공 및 실패 한 경우 아무 작업도 수행 하지. 다음 예제에서는 자동 재인증 프로세스 `OnAppearing`:

```csharp
public partial class LoginPage : ContentPage
{
    ...

    protected override async void OnAppearing()
    {
        try
        {
            // Look for existing account
            IEnumerable<IAccount> accounts = await App.AuthenticationClient.GetAccountsAsync();

            AuthenticationResult result = await App.AuthenticationClient
                .AcquireTokenSilent(Constants.Scopes, accounts.FirstOrDefault())
                .ExecuteAsync();

            await Navigation.PushAsync(new LogoutPage(result));
        }
        catch
        {
            // Do nothing - the user isn't logged in
        }
        base.OnAppearing();
    }

    ...
}
```

합니다 `OnLoginButtonClicked` (로그인 단추를 클릭할 때 발생) 이벤트 처리기 호출 `AcquireTokenAsync`합니다. MSAL 라이브러리는 자동으로 모바일 장치 브라우저를 열고 하 고 로그인 페이지로 이동 합니다. 라는 로그인 URL을 **기관**, 테 넌 트 이름의 조합 및 정책에 정의 된 합니다 **Constants.cs** 파일입니다. 사용자가 선택 하는 경우는 암호 옵션을 찾기 시작 되는 예외를 사용 하 여 앱에 반환 되는 암호를 잊으셨나요 합니다. 다음 예제에서는 인증 프로세스를 보여 줍니다.

```csharp
public partial class LoginPage : ContentPage
{
    ...

    async void OnLoginButtonClicked(object sender, EventArgs e)
    {
        AuthenticationResult result;
        try
        {
            result = await App.AuthenticationClient
                .AcquireTokenInteractive(Constants.Scopes)
                .WithPrompt(Prompt.SelectAccount)
                .WithParentActivityOrWindow(App.UIParent)
                .ExecuteAsync();
    
            await Navigation.PushAsync(new LogoutPage(result));
        }
        catch (MsalException ex)
        {
            if (ex.Message != null && ex.Message.Contains("AADB2C90118"))
            {
                result = await OnForgotPassword();
                await Navigation.PushAsync(new LogoutPage(result));
            }
            else if (ex.ErrorCode != "authentication_canceled")
            {
                await DisplayAlert("An error has occurred", "Exception message: " + ex.Message, "Dismiss");
            }
        }
    }

    ...
}
```

`OnForgotPassword` 메서드는 로그인 프로세스와 유사 하지만 사용자 지정 정책을 구현 합니다. `OnForgotPassword` 다른 오버 로드를 사용 하 여 `AcquireTokenAsync`, 특정을 제공할 수 있습니다 **기관**합니다. 다음 예제에서는 사용자 지정을 제공 하는 방법을 보여 줍니다 **기관** 토큰을 확보 하는 경우:

```csharp
public partial class LoginPage : ContentPage
{
    ...
    async Task<AuthenticationResult> OnForgotPassword()
    {
        try
        {
            return await App.AuthenticationClient
                .AcquireTokenInteractive(Constants.Scopes)
                .WithPrompt(Prompt.SelectAccount)
                .WithParentActivityOrWindow(App.UIParent)
                .WithB2CAuthority(Constants.AuthorityPasswordReset)
                .ExecuteAsync();
        }
        catch (MsalException)
        {
            // Do nothing - ErrorCode will be displayed in OnLoginButtonClicked
            return null;
        }
    }
}
```

인증의 마지막 조각은 로그 아웃 프로세스 이며 `OnLogoutButtonClicked` 메서드 사용자가 로그 아웃 단추를 누를 때 호출 됩니다. 모든 계정을 통해 반복 하 고 해당 토큰을 무효화를 확인 합니다. 아래 샘플에는 로그 아웃 구현 방법을 보여 줍니다.

```csharp
public partial class LogoutPage : ContentPage
{
    ...
    async void OnLogoutButtonClicked(object sender, EventArgs e)
    {
        IEnumerable<IAccount> accounts = await App.AuthenticationClient.GetAccountsAsync();

        while (accounts.Any())
        {
            await App.AuthenticationClient.RemoveAsync(accounts.First());
            accounts = await App.AuthenticationClient.GetAccountsAsync();
        }

        await Navigation.PopAsync();
    }
}
```

### <a name="ios"></a>iOS

IOS에서 Azure Active Directory B2C를 사용 하 여 등록 된 사용자 지정 URL 구성표 등록 해야 합니다 **Info.plist**합니다. MSAL에서는 앞에서 설명한 특정 패턴에 맞게 URL 체계 [Azure Active Directory B2C를 사용 하 여 모바일 응용 프로그램을 등록](/docs/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c)합니다. 다음 스크린샷은의 사용자 지정 URL 체계 **Info.plist**합니다.

!["Ios 사용자 지정 URL 구성표 등록"](azure-ad-b2c-images/customurl-ios.png)

MSAL에 등록 된 iOS에서 키 집합 자격 필요 합니다 **Entitilements.plist**다음 스크린샷에 표시 된 것 처럼:

!["Ios 응용 프로그램 권한 부여 설정"](azure-ad-b2c-images/entitlements-ios.png)

Azure Active Directory B2C에는 권한 부여 요청 완료 되 면 등록 된 리디렉션 URL로 리디렉션합니다. 사용자 지정 URL 구성표 인해 iOS 모바일 응용 프로그램을 시작 하 고 URL에서 처리 되는 시작 매개 변수로 전달 합니다 `OpenUrl` 응용 프로그램의 재정의 `AppDelegate` 클래스 및 msal 환경의 반환 제어 합니다. `OpenUrl` 구현은 다음 코드 예제에 나와 있습니다.

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        ...
        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
            return base.OpenUrl(app, url, options);
        }
    }
}
```

### <a name="android"></a>Android

Android에서 Azure Active Directory B2C를 사용 하 여 등록 된 사용자 지정 URL 구성표에 등록 해야 합니다 **AndroidManifest.xml**합니다. MSAL에서는 앞에서 설명한 특정 패턴에 맞게 URL 체계 [Azure Active Directory B2C를 사용 하 여 모바일 응용 프로그램을 등록](/docs/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c)합니다. 다음 예제에서는 사용자 지정 URL 구성표에는 **AndroidManifest.xml**합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="com.xamarin.adb2cauthorization">
  <uses-sdk android:minSdkVersion="15" />
  <application android:label="ADB2CAuthorization">
    <activity android:name="microsoft.identity.client.BrowserTabActivity">
      <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="INSERT_URI_SCHEME_HERE" android:host="auth" />"
      </intent-filter>
    </activity>"
  </application>
</manifest>
```

`MainActivity` 클래스를 제공 하도록 수정 해야 합니다 `UIParent` 하는 동안 응용 프로그램에는 개체를 `OnCreate` 호출. 권한 부여 요청을 완료 하는 Azure Active Directory B2C의 등록 URL 구성표 리디렉션합니다 합니다 **AndroidManifest.xml**합니다. 등록 된 URI 체계 Android 호출의 결과 `OnActivityResult` 에서 처리 되는 시작 매개 변수로 URL 사용 하 여 메서드를 `SetAuthenticationContinuationEventArgs` 메서드.

```csharp
public class MainActivity : FormsAppCompatActivity
{
    protected override void OnCreate(Bundle bundle)
    {
        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        LoadApplication(new App());
        App.UIParent = this;
    }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
    }
}
```

### <a name="universal-windows-platform"></a>UWP

추가 설치 없이 유니버설 Windows 플랫폼에서 MSAL을 사용 해야 합니다.

## <a name="run-the-project"></a>프로젝트 실행

가상 또는 실제 장치에서 응용 프로그램을 실행 합니다. 탭의 **로그인** 단추는 브라우저를 열고 로그인 하거나 계정을 만들 수 있는 페이지로 이동 합니다. 로그인 프로세스를 완료 한 후 응용 프로그램의 로그 아웃 페이지를 반환 합니다. 다음 스크린샷은 Android 및 iOS를 실행 하는 화면에서 사용자 로그인을 보여 줍니다.

!["Azure ADB2C 로그인 화면에서 Android 및 iOS"](azure-ad-b2c-images/login.png)

## <a name="related-links"></a>관련 링크

- [AzureADB2CAuth (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft 인증 라이브러리](https://www.nuget.org/packages/Microsoft.Identity.Client)
- [Microsoft 인증 라이브러리 문서](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)
