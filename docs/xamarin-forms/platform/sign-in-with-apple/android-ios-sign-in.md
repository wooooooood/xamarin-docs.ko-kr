---
title: Xamarin에 Apple에서 로그인 사용
description: Xamarin. Forms 모바일 응용 프로그램에서 Apple에 로그인을 구현 하는 방법을 알아봅니다.
ms.prod: xamarin
ms.assetid: 2E47E7F2-93D4-4CA3-9E66-247466D25E4D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 09/10/2019
ms.openlocfilehash: 105088b612ffc35d18bdf800b48cc700ce6f4a48
ms.sourcegitcommit: 09bc69d7119a04684c9e804c5cb113b8b1bb7dfc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71206517"
---
# <a name="use-sign-in-with-apple-in-xamarinforms"></a>Xamarin에서 Apple에 로그인 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/Redth/Xamarin.AppleSignIn.Sample)

Apple로 로그인은 타사 인증 서비스를 사용 하는 iOS 13의 모든 새 응용 프로그램을 위한 것입니다. IOS와 Android의 구현 세부 정보는 매우 다릅니다. 이 가이드에서는 지금 Xamarin.ios에서이 작업을 수행 하는 방법을 안내 합니다.

이 가이드 및 샘플에서는 특정 플랫폼 서비스를 사용 하 여 Apple에서 로그인을 처리 합니다.

- Openid connect/OpenAuth를 사용 하 여 Azure Functions와 통신 하는 일반 웹 서비스를 사용 하는 Android
- ios는 iOS 13의 인증에 기본 API를 사용 하 고 iOS 12 이하의 일반 웹 서비스로 대체 합니다.

## <a name="a-sample-apple-sign-in-flow"></a>샘플 Apple 로그인 흐름

이 샘플에서는 Xamarin.ios 앱에서 Apple 로그인을 사용 하기 위한 독단적인 구현을 제공 합니다.

두 개의 Azure Functions를 사용 하 여 인증 흐름에 도움을 줍니다.

1. `applesignin_auth`-Apple 로그인 권한 부여 URL을 생성 하 고이에 리디렉션합니다.  모바일 앱 대신 서버 쪽에서이 작업을 수행 하므로를 `state` 캐시 하 고 Apple 서버에서 콜백을 보내는 경우 유효성을 검사할 수 있습니다.
2. `applesignin_callback`-Apple에서 POST 콜백을 처리 하 고 액세스 토큰 및 ID 토큰에 대 한 권한 부여 코드를 안전 하 게 교환 합니다.  마지막으로, 응용 프로그램의 URI 스키마로 다시 리디렉션되고 URL 조각에서 토큰을 다시 전달 합니다.

모바일 앱은 사용자가 선택한 사용자 지정 URI 체계 (이 경우 `xamarinformsapplesignin://`)를 처리 하기 위해 자신을 등록 하므로 함수는 `applesignin_callback` 토큰을 다시 해당 토큰에 릴레이할 수 있습니다.

사용자가 인증을 시작 하면 다음 단계가 수행 됩니다.

1. 모바일 앱은 및 `nonce` `state` 값을 생성 하 여 `applesignin_auth` Azure 함수에 전달 합니다.
2. Azure `applesignin_auth` 함수는 제공 `state` 된 및 `nonce`를 사용 하 여 Apple 로그인 권한 부여 URL을 생성 하 고 모바일 앱 브라우저를이 권한으로 리디렉션합니다.
3. 사용자가 Apple 서버에서 호스트 되는 Apple 로그인 권한 부여 페이지에서 자격 증명을 안전 하 게 입력 합니다.
4. Apple의 서버에서 apple 로그인 흐름이 완료 된 후 apple은 `redirect_uri` `applesignin_callback` Azure function이 될로 리디렉션합니다.
5. `applesignin_callback` 함수로 전송 되는 Apple의 요청은 올바른 `state` 가 반환 되 고 ID 토큰 클레임이 유효한 지 확인 하기 위해 유효성이 검사 됩니다.
6. Azure `applesignin_callback` 함수는 _액세스 토큰_, _새로 고침 토큰_및 _id 토큰_ (사용자 id, 이름 및 전자 메일에 대 한 클레임이 포함 됨)에 대해 Apple에서 `code` 게시 한을 교환 합니다.
7. 마지막 `applesignin_callback` 으로 Azure 함수는 토큰 (예: `xamarinformsapplesignin://#access_token=...&refresh_token=...&id_token=...`)을 사용 하`xamarinformsapplesignin://`여 uri 조각을 추가 하는 앱의 uri 체계 ()로 다시 리디렉션합니다.
8. 모바일 앱은 URI 조각을로 `AppleAccount` 구문 분석 하 고 수신 된 `nonce` 클레임이 흐름 시작 시 생성 `nonce` 된와 일치 하는지 확인 합니다.
9. 이제 모바일 앱이 인증 되었습니다.

## <a name="azure-functions"></a>Azure Functions

이 샘플에서는 Azure Functions을 사용 합니다. 또는 ASP.NET Core 컨트롤러나 유사한 웹 서버 솔루션이 동일한 기능을 제공할 수 있습니다.

### <a name="configuration"></a>Configuration

Azure Functions 사용 하는 경우 몇 가지 앱 설정을 구성 해야 합니다.

- `APPLE_SIGNIN_KEY_ID``KeyId` -이전 버전입니다.
- `APPLE_SIGNIN_TEAM_ID`-일반적으로 [멤버 자격 프로필](https://developer.apple.com/account/#/membership) 에 있는 _팀 ID_ 입니다.
- `APPLE_SIGNIN_SERVER_ID`: `ServerId` 이는 이전 버전의입니다.  앱 _번들 id_가 *아니라 사용자가* 만든 *서비스 id* 의 *식별자* 입니다.
- `APPLE_SIGNIN_APP_CALLBACK_URI`-를 사용 하 여 앱으로 다시 리디렉션하는 사용자 지정 URI 체계입니다.  이 샘플 `xamarinformsapplesignin://` 에서는을 사용 합니다.
- `APPLE_SIGNIN_REDIRECT_URI`- *Apple 로그인* 구성 섹션에서 *서비스 ID* 를 만들 때 설치 하는 *리디렉션 URL* 입니다.  테스트 하려면 다음과 같이 표시 될 수 있습니다.`http://local.test:7071/api/applesignin_callback`
- `APPLE_SIGNIN_P8_KEY`- `.p8` 파일의 텍스트 내용으로, `\n` 모든 줄바꿈 제거 되었으므로 하나의 긴 문자열이 됩니다.

### <a name="security-considerations"></a>보안 고려 사항

응용 프로그램 코드 내부에 P8 키를 저장 **하지 마세요** . 응용 프로그램 코드를 다운로드 하 고 디스어셈블 하는 것이 쉽습니다. 

또한를 사용 `WebView` 하 여 인증 흐름을 호스트 하 고, URL 탐색 이벤트를 가로채서 인증 코드를 가져오는 것이 좋지 않은 경우입니다. 현재는 토큰 교환을 처리 하기 위해 서버에 일부 코드를 호스팅하지 않고 iOS13 이상 장치에서 Apple을 사용 하 여 로그인을 처리 하는 완전히 안전한 방법이 없습니다. 서버에 권한 부여 url 생성 코드를 호스트 하는 것이 좋습니다. 이렇게 하면 Apple에서 서버에 대 한 게시 콜백을 발급할 때 상태를 캐시 하 고 유효성을 검사할 수 있습니다.

## <a name="a-cross-platform-sign-in-service"></a>플랫폼 간 로그인 서비스

Xamarin.ios DependencyService를 사용 하 여 iOS에서 플랫폼 서비스를 사용 하는 별도의 인증 서비스와 공유 인터페이스를 기반으로 하는 Android 및 기타 iOS 이외의 플랫폼을 위한 일반 웹 서비스를 만들 수 있습니다.

```csharp
public interface IAppleSignInService
{
    bool Callback(string url);

    Task<AppleAccount> SignInAsync();
}
```

IOS에서는 기본 Api가 사용 됩니다.

```csharp
public class AppleSignInServiceiOS : IAppleSignInService
{
#if __IOS__13
    AuthManager authManager;
#endif

    bool Is13 => UIDevice.CurrentDevice.CheckSystemVersion(13, 0);
    WebAppleSignInService webSignInService;

    public AppleSignInServiceiOS()
    {
        if (!Is13)
            webSignInService = new WebAppleSignInService();
    }

    public async Task<AppleAccount> SignInAsync()
    {
        // Fallback to web for older iOS versions
        if (!Is13)
            return await webSignInService.SignInAsync();

        AppleAccount appleAccount = default;

#if __IOS__13
        var provider = new ASAuthorizationAppleIdProvider();
        var req = provider.CreateRequest();

        authManager = new AuthManager(UIApplication.SharedApplication.KeyWindow);

        req.RequestedScopes = new[] { ASAuthorizationScope.FullName, ASAuthorizationScope.Email };
        var controller = new ASAuthorizationController(new[] { req });

        controller.Delegate = authManager;
        controller.PresentationContextProvider = authManager;

        controller.PerformRequests();

        var creds = await authManager.Credentials;

        if (creds == null)
            return null;

        appleAccount = new AppleAccount();
        appleAccount.IdToken = JwtToken.Decode(new NSString(creds.IdentityToken, NSStringEncoding.UTF8).ToString());
        appleAccount.Email = creds.Email;
        appleAccount.UserId = creds.User;
        appleAccount.Name = NSPersonNameComponentsFormatter.GetLocalizedString(creds.FullName, NSPersonNameComponentsFormatterStyle.Default, NSPersonNameComponentsFormatterOptions.Phonetic);
        appleAccount.RealUserStatus = creds.RealUserStatus.ToString();
#endif

        return appleAccount;
    }

    public bool Callback(string url) => true;
}

#if __IOS__13
class AuthManager : NSObject, IASAuthorizationControllerDelegate, IASAuthorizationControllerPresentationContextProviding
{
    public Task<ASAuthorizationAppleIdCredential> Credentials
        => tcsCredential?.Task;

    TaskCompletionSource<ASAuthorizationAppleIdCredential> tcsCredential;

    UIWindow presentingAnchor;

    public AuthManager(UIWindow presentingWindow)
    {
        tcsCredential = new TaskCompletionSource<ASAuthorizationAppleIdCredential>();
        presentingAnchor = presentingWindow;
    }

    public UIWindow GetPresentationAnchor(ASAuthorizationController controller)
        => presentingAnchor;

    [Export("authorizationController:didCompleteWithAuthorization:")]
    public void DidComplete(ASAuthorizationController controller, ASAuthorization authorization)
    {
        var creds = authorization.GetCredential<ASAuthorizationAppleIdCredential>();
        tcsCredential?.TrySetResult(creds);
    }

    [Export("authorizationController:didCompleteWithError:")]
    public void DidComplete(ASAuthorizationController controller, NSError error)
        => tcsCredential?.TrySetException(new Exception(error.LocalizedDescription));
}
#endif
```

컴파일 플래그 `__IOS__13` 는 iOS 13 뿐만 아니라 일반 웹 서비스로 대체 되는 레거시 버전에 대 한 지원을 제공 하는 데 사용 됩니다.

Android에서 Azure Functions를 사용 하는 제네릭 웹 서비스가 사용 됩니다.

```csharp
public class WebAppleSignInService : IAppleSignInService
{
    // IMPORTANT: This is what you register each native platform's url handler to be
    public const string CallbackUriScheme = "xamarinformsapplesignin";
    public const string InitialAuthUrl = "http://local.test:7071/api/applesignin_auth";

    string currentState;
    string currentNonce;

    TaskCompletionSource<AppleAccount> tcsAccount = null;

    public bool Callback(string url)
    {
        // Only handle the url with our callback uri scheme
        if (!url.StartsWith(CallbackUriScheme + "://"))
            return false;

        // Ensure we have a task waiting
        if (tcsAccount != null && !tcsAccount.Task.IsCompleted)
        {
            try
            {
                // Parse the account from the url the app opened with
                var account = AppleAccount.FromUrl(url);

                // IMPORTANT: Validate the nonce returned is the same as our originating request!!
                if (!account.IdToken.Nonce.Equals(currentNonce))
                    tcsAccount.TrySetException(new InvalidOperationException("Invalid or non-matching nonce returned"));

                // Set our account result
                tcsAccount.TrySetResult(account);
            }
            catch (Exception ex)
            {
                tcsAccount.TrySetException(ex);
            }
        }

        tcsAccount.TrySetResult(null);
        return false;
    }

    public async Task<AppleAccount> SignInAsync()
    {
        tcsAccount = new TaskCompletionSource<AppleAccount>();

        // Generate state and nonce which the server will use to initial the auth
        // with Apple.  The nonce should flow all the way back to us when our function
        // redirects to our app
        currentState = Util.GenerateState();
        currentNonce = Util.GenerateNonce();

        // Start the auth request on our function (which will redirect to apple)
        // inside a browser (either SFSafariViewController, Chrome Custom Tabs, or native browser)
        await Xamarin.Essentials.Browser.OpenAsync($"{InitialAuthUrl}?&state={currentState}&nonce={currentNonce}",
            Xamarin.Essentials.BrowserLaunchMode.SystemPreferred);

        return await tcsAccount.Task;
    }
}
```

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 사용 하기 위해 Apple에 로그인을 설정 하는 데 필요한 단계에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [XamarinFormsAppleSignIn (샘플)](https://github.com/Redth/Xamarin.AppleSignIn.Sample)
- [Apple 지침을 사용 하 여 로그인](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)
