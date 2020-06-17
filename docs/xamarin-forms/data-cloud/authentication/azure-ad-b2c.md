---
제목: "Azure Active Directory B2C를 사용 하 여 사용자 인증" 설명: "Azure Active Directory B2C 소비자 지향 웹 및 모바일 응용 프로그램을 위한 클라우드 id 관리 기능을 제공 합니다. 이 문서에서는 Azure Active Directory B2C를 사용 하 여 Microsoft 인증 라이브러리를 통해 id 관리를 모바일 응용 프로그램에 통합 하는 방법을 보여 줍니다. "
assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6: xamarin-forms author: davidbritch: dabritch:: 12/04/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="authenticate-users-with-azure-active-directory-b2c"></a>Azure Active Directory B2C로 사용자 인증

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azureadb2cauth)

_Azure Active Directory B2C는 소비자 지향 웹 및 모바일 응용 프로그램을 위한 클라우드 id 관리 기능을 제공 합니다. 이 문서에서는 Azure Active Directory B2C를 사용 하 여 Microsoft 인증 라이브러리를 통해 id 관리를 모바일 응용 프로그램에 통합 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

ADB2C (Azure Active Directory B2C)는 소비자 지향 응용 프로그램을 위한 id 관리 서비스입니다. 이를 통해 사용자는 기존 소셜 계정 또는 전자 메일, 사용자 이름 및 암호와 같은 사용자 지정 자격 증명을 사용 하 여 응용 프로그램에 로그인 할 수 있습니다. 사용자 지정 자격 증명 계정을 _로컬_ 계정 이라고 합니다.

Azure Active Directory B2C identity management service를 모바일 응용 프로그램에 통합 하는 프로세스는 다음과 같습니다.

1. Azure Active Directory B2C 테 넌 트를 만듭니다.
1. Azure Active Directory B2C 테 넌 트에 모바일 응용 프로그램을 등록 합니다.
1. 등록 및 로그인에 대 한 정책을 만들고 암호 사용자 흐름을 잊음.
1. MSAL (Microsoft 인증 라이브러리)을 사용 하 여 Azure Active Directory B2C 테 넌 트를 사용 하 여 인증 워크플로를 시작 합니다.

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다.

Azure Active Directory B2C는 Microsoft, GitHub, Facebook, Twitter 등을 비롯 한 여러 id 공급자를 지원 합니다. Azure Active Directory B2C 기능에 대 한 자세한 내용은 [Azure Active Directory B2C 설명서](/azure/active-directory-b2c/)를 참조 하세요.

Microsoft 인증 라이브러리는 여러 응용 프로그램 아키텍처 및 플랫폼을 지원 합니다. MSAL 기능에 대 한 자세한 내용은 GitHub의 [Microsoft 인증 라이브러리](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) 를 참조 하세요.

## <a name="configure-an-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C 테 넌 트 구성

샘플 프로젝트를 실행 하려면 Azure Active Directory B2C 테 넌 트를 만들어야 합니다. 자세한 내용은 [Azure Portal에서 Azure Active Directory B2C 테 넌 트 만들기](/azure/active-directory-b2c/active-directory-b2c-get-started/)를 참조 하세요.

테 넌 트를 만든 후에는 모바일 응용 프로그램을 구성 하기 위해 테 넌 트 **이름** 및 **테 넌 트 ID** 가 필요 합니다. 테 넌 트 ID 및 이름은 테 넌 트 URL을 만들 때 생성 된 도메인에 의해 정의 됩니다. 생성 된 테 넌 트 URL이 인 경우 테 넌 트 `https://contoso20190410tenant.onmicrosoft.com/` **ID** 는이 `contoso20190410tenant.onmicrosoft.com` 고 **테 넌 트 이름은** `contoso20190410tenant` 입니다. 상단 메뉴에서 **디렉터리 및 구독 필터** 를 클릭 하 여 Azure Portal에서 테 넌 트 도메인을 찾습니다. 다음 스크린샷은 Azure 디렉터리 및 구독 필터 단추와 테 넌 트 도메인을 보여 줍니다.

[![Azure 디렉터리 및 구독 필터 보기의 테 넌 트 이름](azure-ad-b2c-images/azure-tenant-name-cropped.png)](azure-ad-b2c-images/azure-tenant-name.png#lightbox)

샘플 프로젝트에서 **Constants.cs** 파일을 편집 하 여 `tenantName` 및 필드를 설정 `tenantId` 합니다. 다음 코드에서는 테 넌 트 도메인이 인 경우 이러한 값을 설정 하는 방법을 보여 줍니다 `https://contoso20190410tenant.onmicrosoft.com/` .이 값은 포털의 값으로 바꿉니다.

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    ...
}
```

## <a name="register-your-mobile-application-with-azure-active-directory-b2c"></a>Azure Active Directory B2C를 사용 하 여 모바일 응용 프로그램 등록

모바일 응용 프로그램을 연결 하 고 사용자를 인증 하려면 먼저 테 넌 트에 등록 해야 합니다. 등록 프로세스에서는 응용 프로그램에 고유한 **응용 프로그램 ID** 를 할당 하 고, 인증 후 응답을 응용 프로그램에 다시 전달 하는 **리디렉션 URL** 을 할당 합니다. 자세한 내용은 [Azure Active Directory B2C: 응용 프로그램 등록](/azure/active-directory-b2c/active-directory-b2c-app-registration/)을 참조 하세요. 응용 프로그램에 할당 된 응용 프로그램 ID를 알고 있어야 합니다 .이 **ID** 는 속성 보기에서 응용 프로그램 이름 뒤에 나열 됩니다. 다음 스크린샷은 응용 프로그램 ID를 찾을 수 있는 위치를 보여 줍니다.

[![Azure 응용 프로그램 속성 보기의 응용 프로그램 ID](azure-ad-b2c-images/azure-application-id-cropped.png)](azure-ad-b2c-images/azure-application-id.png#lightbox)

Microsoft 인증 라이브러리에서는 응용 프로그램에 대 한 **리디렉션 URL** 이 "msal" 텍스트와 접두사로 시작 하는 **응용 프로그램 ID** 가 되 고 그 뒤에 "auth" 라는 끝점이 필요 합니다. 응용 프로그램 ID가 "1234abcd" 이면 전체 URL은 이어야 합니다 `msal1234abcd://auth` . 응용 프로그램에서 **Native client** 설정을 사용 하도록 설정 했는지 확인 하 고 다음 스크린샷에 표시 된 것 처럼 응용 프로그램 ID를 사용 하 여 **사용자 지정 리디렉션 URI** 를 만들어야 합니다.

![Azure 응용 프로그램 속성 보기의 사용자 지정 리디렉션 URI](azure-ad-b2c-images/azure-redirect-uri.png)

이 URL은 Android **ApplicationManifest.xml** 및 iOS **info.plist**모두에서 나중에 사용 됩니다.

샘플 프로젝트에서 **Constants.cs** 파일을 편집 하 여 `clientId` 필드를 **응용 프로그램 ID**로 설정 합니다. 다음 코드는 응용 프로그램 ID가 인 경우이 값을 설정 하는 방법을 보여 줍니다 `1234abcd` .

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    static readonly string clientId = "1234abcd";
    ...
}
```

## <a name="create-sign-up-and-sign-in-policies-and-forgot-password-policies"></a>등록 및 로그인 정책 만들기 및 암호 정책 잊음

정책은 사용자가 계정을 만들거나 암호를 다시 설정 하는 등의 작업을 완료 하기 위해 진행 하는 환경입니다. 또한 정책은 사용자가 환경에서 반환 될 때 응용 프로그램에서 수신 하는 토큰의 콘텐츠를 지정 합니다. 계정 등록 및 로그인에 대 한 정책을 설정 하 고 암호를 재설정 해야 합니다. Azure에는 공통 정책의 생성을 간소화 하는 기본 제공 정책이 있습니다. 자세한 내용은 [Azure Active Directory B2C: 기본 제공 정책](/azure/active-directory-b2c/active-directory-b2c-reference-policies/)을 참조 하세요.

정책 설정을 완료 한 후에는 Azure Portal의 **사용자 흐름 (정책)** 보기에 두 개의 정책이 있어야 합니다. 다음 스크린샷은 Azure Portal의 두 가지 구성 된 정책을 보여 줍니다.

![Azure 사용자 흐름 (정책) 보기에서 구성 된 두 개의 정책](azure-ad-b2c-images/azure-application-policies.png)

샘플 프로젝트에서 **Constants.cs** 파일을 편집 하 여 `policySignin` 및 `policyPassword` 필드를 정책 설정 중에 선택한 이름을 반영 하도록 설정 합니다.

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

## <a name="use-the-microsoft-authentication-library-msal-for-authentication"></a>인증에 MSAL (Microsoft 인증 라이브러리) 사용

MSAL (Microsoft 인증 라이브러리) NuGet 패키지를 솔루션의 공유, .NET Standard 프로젝트 및 플랫폼 프로젝트에 추가 해야 합니다 Xamarin.Forms . MSAL에는 `PublicClientApplicationBuilder` 인터페이스를 준수 하는 개체를 생성 하는 클래스가 포함 되어 있습니다 `IPublicClientApplication` . MSAL `With` 은 절을 활용 하 여 생성자와 인증 메서드에 추가 매개 변수를 제공 합니다.

샘플 프로젝트에서 **app.xaml** 에 대 한 코드는 및 라는 정적 속성을 정의 `AuthenticationClient` `UIParent` 하 고 `AuthenticationClient` 생성자에서 개체를 인스턴스화합니다. `WithIosKeychainSecurityGroup`절은 iOS 응용 프로그램에 대 한 보안 그룹 이름을 제공 합니다. `WithB2CAuthority`절은 사용자를 인증 하는 데 사용 되는 기본 **인증 기관**또는 정책을 제공 합니다. `WithRedirectUri`절은 여러 uri를 지정 하는 경우 사용할 URI를 리디렉션하는 Azure Notification Hubs 인스턴스에 지시 합니다. 다음 예제에서는를 인스턴스화하는 방법을 보여 줍니다 `PublicClientApplication` .

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
            .WithRedirectUri($"msal{Constants.ClientId}://auth")
            .Build();

        MainPage = new NavigationPage(new LoginPage());
    }

    ...
```

> [!NOTE]
> Azure Notification Hubs 인스턴스에 하나의 리디렉션 URI만 정의 되어 있는 경우 해당 인스턴스는 절을 사용 하 여 `AuthenticationClient` 리디렉션 uri를 지정 하지 않고 작동할 수 있습니다 `WithRedirectUri` . 그러나 다른 클라이언트 또는 인증 방법을 지원 하기 위해 Azure 구성이 확장 될 경우에는 항상이 값을 지정 해야 합니다.

`OnAppearing`이전에 로그인 한 **LoginPage.xaml.cs** `AcquireTokenSilentAsync` 사용자에 대 한 인증 토큰을 새로 고치기 위해 LoginPage.xaml.cs 코드 뒤에 있는 이벤트 처리기가 호출 됩니다. 인증 프로세스는 성공 하는 `LogoutPage` 경우로 리디렉션되고 실패 시 아무 작업도 수행 하지 않습니다. 다음 예에서는의 자동 재인증 프로세스를 보여 줍니다 `OnAppearing` .

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

`OnLoginButtonClicked`로그인 단추를 클릭할 때 발생 하는 이벤트 처리기는를 호출 `AcquireTokenAsync` 합니다. MSAL 라이브러리가 자동으로 모바일 장치 브라우저를 열고 로그인 페이지로 이동 합니다. **권한**이라는 로그인 URL은 **Constants.cs** 파일에 정의 된 테 넌 트 이름과 정책의 조합입니다. 사용자가 암호 찾기 옵션을 선택 하면 예외를 사용 하 여 앱에 반환 되며 암호 찾기 환경을 시작 합니다. 다음 예에서는 인증 프로세스를 보여 줍니다.

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

`OnForgotPassword`메서드는 로그인 프로세스와 유사 하지만 사용자 지정 정책을 구현 합니다. `OnForgotPassword`는 `AcquireTokenAsync` 특정 **권한을**제공할 수 있는의 다른 오버 로드를 사용 합니다. 다음 예제에서는 토큰을 획득할 때 사용자 지정 **권한을** 제공 하는 방법을 보여 줍니다.

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

최종 인증 부분은 로그 아웃 프로세스입니다. `OnLogoutButtonClicked`메서드는 사용자가 로그 아웃 단추를 누를 때 호출 됩니다. 모든 계정을 반복 하 고 해당 토큰이 무효화 되었는지 확인 합니다. 다음 샘플에서는 로그 아웃 구현을 보여 줍니다.

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

IOS에서 Azure Active Directory B2C에 등록 된 사용자 지정 URL 구성표는 **info.plist**에 등록 되어 있어야 합니다. MSAL은 이전에 [Azure Active Directory B2C를 사용 하 여 모바일 응용 프로그램 등록](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c)에서 설명한 특정 패턴을 준수 하는 URL 체계를 기대 합니다. 다음 스크린샷은 **info.plist**의 사용자 지정 URL 스키마를 보여 줍니다.

!["IOS의 사용자 지정 URL 체계 등록"](azure-ad-b2c-images/customurl-ios.png)

MSAL에는 다음 스크린샷에 표시 된 것 처럼 **Entitilements**에 등록 된 iOS의 키 집합 자격도 필요 합니다.

!["IOS에서 응용 프로그램 자격 설정"](azure-ad-b2c-images/entitlements-ios.png)

Azure Active Directory B2C 권한 부여 요청을 완료 하면 등록 된 리디렉션 URL로 리디렉션됩니다. 사용자 지정 URL 체계는 iOS에서 모바일 응용 프로그램을 시작 하 고 URL을 시작 매개 변수로 전달 하 여 `OpenUrl` 응용 프로그램 클래스의 재정의에 의해 처리 되 `AppDelegate` 고, 환경에 대 한 제어를 msal로 반환 합니다. `OpenUrl`구현은 다음 코드 예제에 나와 있습니다.

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

Android에서 Azure Active Directory B2C에 등록 된 사용자 지정 URL 체계를 **AndroidManifest.xml**에 등록 해야 합니다. MSAL은 이전에 [Azure Active Directory B2C를 사용 하 여 모바일 응용 프로그램 등록](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c)에서 설명한 특정 패턴을 준수 하는 URL 체계를 기대 합니다. 다음 예에서는 **AndroidManifest.xml**의 사용자 지정 URL 스키마를 보여 줍니다.

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
        <!-- example -->
        <!-- <data android:scheme="msalaaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" android:host="auth" /> -->
        <data android:scheme="INSERT_URI_SCHEME_HERE" android:host="auth" />
      </intent-filter>
    </activity>"
  </application>
</manifest>
```

`MainActivity`클래스를 수정 하 여 `UIParent` 호출 중에 응용 프로그램에 개체를 제공 해야 합니다 `OnCreate` . Azure Active Directory B2C 권한 부여 요청이 완료 되 면 **AndroidManifest.xml**에서 등록 된 URL 체계로 리디렉션됩니다. 등록 된 URI 체계를 사용 하면 Android에서 `OnActivityResult` URL을 사용 하 여 메서드를 시작 매개 변수로 호출 합니다. 여기서 메서드는 메서드를 통해 처리 `SetAuthenticationContinuationEventArgs` 됩니다.

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

유니버설 Windows 플랫폼에서 MSAL을 사용 하는 데 필요한 추가 설치는 없습니다.

## <a name="run-the-project"></a>프로젝트 실행

가상 또는 물리적 장치에서 응용 프로그램을 실행 합니다. **로그인** 단추를 누르면 브라우저가 열리고 로그인 하거나 계정을 만들 수 있는 페이지로 이동 합니다. 로그인 프로세스를 완료 한 후에는 응용 프로그램의 로그 아웃 페이지로 돌아옵니다. 다음 스크린샷은 Android 및 iOS에서 실행 되는 사용자 로그인 화면을 보여 줍니다.

!["Android 및 iOS의 Azure ADB2C 로그인 화면"](azure-ad-b2c-images/login.png)

## <a name="related-links"></a>관련 링크

- [AzureADB2CAuth (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azureadb2cauth)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft 인증 라이브러리](https://www.nuget.org/packages/Microsoft.Identity.Client)
- [Microsoft 인증 라이브러리 설명서](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)
