---
title: Id 공급자를 사용 하는 AuthenticateUsers
description: 이 문서에서는 Xamarin.Auth를 사용 하 여 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: 3bc001c048129851a3604752fdfbd45d72d4c3d3
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760493"
---
# <a name="authenticate-users-with-an-identity-provider"></a>Id 공급자를 사용 하 여 사용자 인증

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)

_Xamarin.Auth는 사용자를 인증 하 고 해당 계정에 저장 된 플랫폼 간 SDK입니다. Google, Microsoft, Facebook 및 Twitter와 같은 id 공급자를 사용 하는 것에 대 한 지원을 제공 하는 OAuth 인증자를 포함 합니다. 이 문서에서는 Xamarin.Auth를 사용 하 여 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 방법에 설명 합니다._

OAuth 인증을 위한 개방형 표준인 이며 타사 리소스 소유자 id를 공유 하지 않고 해당 정보에 액세스할 수 권한을 부여 해야 하는 리소스 공급자에 알리기 위해 리소스 소유자를 사용 하도록 설정 합니다. 이 예제 권한 사용자의 id를 공유 하지 않고 해당 데이터에 액세스 하는 응용 프로그램에 부여 해야 하는 id 공급자 (예: Google, Microsoft, Facebook 또는 Twitter)에 알리기 위해 사용자를 가능 하 게 됩니다. 로그인에는 웹 사이트 또는 응용 프로그램이 자신의 암호를 노출 하지 않고 웹 사이트 및 id 공급자를 사용 하 여 응용 프로그램에 일반적으로 사용자에 대 한 방법으로 사용 됩니다.

OAuth id 공급자를 사용 하는 경우 인증 흐름의 대략적인 개요는 다음과 같습니다.

1. 응용 프로그램에는 브라우저는 id 공급자 URL 이동합니다.
1. Id 공급자는 사용자 인증을 처리 하 고 응용 프로그램에 권한 부여 코드를 반환 합니다.
1. 응용 프로그램은 id 공급자에서 액세스 토큰에 대 한 권한 부여 코드를 교환 합니다.
1. 응용 프로그램 Api 같은 기본적인 사용자 데이터를 요청에 대 한 API는 id 공급자에 액세스 하려면 액세스 토큰을 사용 합니다.

샘플 응용 프로그램 Xamarin.Auth를 사용 하 여 Google에 대 한 기본 인증 흐름을 구현 하는 방법에 설명 합니다. 이 항목의 id 공급자로 Google을 사용, 방법은 다른 id 공급자에 모두 적용 합니다. Google OAuth 2.0 끝점을 사용 하 여 인증에 대 한 자세한 내용은 참조 하세요. [Google Api 액세스를 사용 하 여 oauth 2.0](https://developers.google.com/identity/protocols/OAuth2) Google의 웹 사이트입니다.

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결 하므로 중요 한 정보가 실수로 유출 방지 적용 합니다. ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되므로 모든 연결이 ATS 보안 요구 사항이 적용 됩니다. 연결에서 이러한 요구를 충족 하지 않는, 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 ATS 옵트인 수 있습니다는 `HTTPS` 프로토콜 및 인터넷 리소스에 대 한 통신을 보호 합니다. 이 앱을 업데이트 하 여 수행할 수 있습니다 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="using-xamarinauth-to-authenticate-users"></a>Xamarin.Auth를 사용 하 여 사용자를 인증 합니다.

Xamarin.Auth id 공급자의 권한 부여 끝점과 상호 작용 하는 응용 프로그램에 대 한 두 가지 방법을 지원 합니다.

1. 포함 된 웹 보기를 사용합니다. 일반적으로 된이 하는 동안 다음과 같은 이유로 더 이상 권장 되지:

    - 웹 보기를 호스팅하는 응용 프로그램에는 사용자의 전체 인증 자격 증명, 응용 프로그램 의도 OAuth 권한 부여 뿐 아니라 액세스할 수 있습니다. 이 응용 프로그램에 필요한 응용 프로그램의 공격 노출 영역을 향상 시키는 것 보다 더 강력한 자격 증명에 대 한 액세스를 최소 권한의 원칙을 위반 합니다.
    - 호스트 응용 프로그램 수 사용자 이름 및 암호, 자동으로 폼을 제출 및 사용자 동의 무시 및 세션 쿠키를 복사 캡처하고 사용자로 인증 된 작업을 수행 하 합니다.
    - 포함 된 웹 보기는 다른 응용 프로그램 또는 사용자는 첨자 사용자 환경을 간주 되는 모든 권한 부여 요청에 대 한 로그인 하도록 요구 되는 장치의 웹 브라우저를 사용 하 여 인증 상태를 공유 하지 마세요.
    - 일부 권한 부여 끝점 검색 웹 보기에서 제공 되는 권한 부여 요청을 차단 하는 단계를 수행 합니다.

1. 권장 되는 장치의 웹 브라우저를 사용 합니다. 장치 브라우저를 사용 하 여 OAuth 요청에 대 한 사용자만 응용 프로그램에서 로그인 및 권한 부여 흐름의 전환율 개선 장치에 한 번씩 id 공급자 로그인에 필요한 응용 프로그램의 유용성이 향상 됩니다. 또한 장치 브라우저 응용 프로그램을 검사 하 고 콘텐츠는 웹 뷰의 있지만 브라우저에 표시 되지 않습니다의 콘텐츠를 수정할 수는 향상 된 보안을 제공 합니다. 이것이이 문서 및 샘플 응용 프로그램에서 취한 접근법입니다.

샘플 응용 프로그램 사용자를 인증 하 고 기본 데이터를 검색 하려면 Xamarin.Auth를 사용 하는 방법에 대 한 고급 개요는 다음 다이어그램에 표시 됩니다.

![](oauth-images/google-auth.png "Google 인증을 받지 Xamarin.Auth를 사용 하 여")

응용 프로그램은 Google로 인증 요청을 `OAuth2Authenticator` 클래스입니다. 액세스 토큰을 포함 하는 해당 로그인 페이지를 통해 Google을 사용 하 여 사용자가 성공적으로 인증 되 면 인증 응답을 반환 됩니다. 그런 다음 응용 프로그램 요청 Google을 사용 하 여 기본 사용자 데이터에 대 한는 `OAuth2Request` 요청에 포함 되 고 액세스 토큰을 사용 하 여 클래스입니다.

### <a name="setup"></a>설정

Xamarin.Forms 응용 프로그램을 사용 하 여 Google 로그인을 통합 하는 Google API 콘솔 프로젝트를 만들어야 합니다. 이렇게 하려면 다음을 수행합니다.

1. 로 이동 합니다 [Google API 콘솔](http://console.developers.google.com) 웹 사이트 및 Google 계정 자격 증명으로 로그인 합니다.
1. 프로젝트 드롭다운 목록에서 기존 프로젝트를 선택 하거나 새로 만듭니다.
1. "API Manager" 보충 기사에서 선택 **자격 증명**를 선택 하 고는 **OAuth 동의 화면 탭**합니다. 선택는 **전자 메일 주소**, 지정는 **사용자에 게 표시 되는 제품 이름**를 누릅니다 **저장**.
1. **자격 증명** 탭을 선택 합니다 **자격 증명을 만듭니다** 드롭 다운 목록에서 선택한 **OAuth 클라이언트 ID**.
1. 아래 **응용 프로그램 종류**, 모바일 응용 프로그램에서 실행 되는 플랫폼을 선택 (**iOS** 또는 **Android**).
1. 필요한 세부 정보를 입력 하 고 선택 합니다 **만들기** 단추입니다.

> [!NOTE]
> 클라이언트 ID 설정 된 Google Api에 액세스 하는 응용 프로그램 및 모바일 응용 프로그램에는 단일 플랫폼에 고유 합니다. 따라서 한 **OAuth 클라이언트 ID** 는 Google 로그인을 사용 하는 각 플랫폼에 대해 만들어야 합니다.

이러한 단계를 수행한 후 Xamarin.Auth google OAuth2 인증 흐름을 시작 하려면 사용할 수 있습니다.

### <a name="creating-and-configuring-an-authenticator"></a>만들기 및 인증자를 구성 합니다.

Xamarin.Auth의 `OAuth2Authenticator` 클래스는 OAuth 인증 흐름을 처리 하는 일을 담당 합니다. 다음 코드 예제에서는의 인스턴스화를 `OAuth2Authenticator` 장치의 웹 브라우저를 사용 하 여 인증을 수행할 때 클래스:

```csharp
var authenticator = new OAuth2Authenticator(
    clientId,
    null,
    Constants.Scope,
    new Uri(Constants.AuthorizeUrl),
    new Uri(redirectUri),
    new Uri(Constants.AccessTokenUrl),
    null,
    true);
```

`OAuth2Authenticator` 클래스에 필요한 다양 한 매개 변수는 다음과 같습니다.

- **클라이언트 ID** – 클라이언트는 요청 하는 프로젝트에서 검색할 수를 식별 합니다 [Google API 콘솔](http://console.developers.google.com)합니다.
- **클라이언트 비밀** –이 되어야 `null` 또는 `string.Empty`합니다.
- **범위** – 응용 프로그램에서 요청한 API 액세스를 식별 하 고 값을 사용자에 게 표시 되는 동의 화면에 알립니다. 범위에 대 한 자세한 내용은 참조 하세요. [API 권한 부여 요청](https://developers.google.com/+/web/api/rest/oauth) Google의 웹 사이트입니다.
- **URL 권한 부여** –에서 권한 부여 코드를 가져올 수는 있는 URL을 식별 합니다.
- **리디렉션 URL** – 응답을 보낼 URL을 식별 합니다. 이 매개 변수의 값에 표시 되는 값 중 하 나와 일치 해야 합니다는 **자격 증명** 에서 프로젝트에 대 한 탭의 [Google 개발자 콘솔](https://console.developers.google.com/)합니다.
- **AccessToken Url** –이 권한 부여 코드를 가져온 후 액세스 토큰을 요청 하는 데 사용 하는 URL을 식별 합니다.
- **GetUserNameAsync Func** – 선택적 `Func` 성공적으로 인증 되는 계정의 사용자 이름을 비동기적으로 검색 하는 데 수 있습니다.
- **네이티브 UI를 사용 하 여** – `boolean` 장치의 웹 브라우저를 사용 하 여 인증 요청을 수행 여부를 나타내는 값입니다.

### <a name="setup-authentication-event-handlers"></a>인증 이벤트 처리기를 설정 합니다.

사용자 인터페이스에 대 한 이벤트 처리기를 프레젠테이션하기 전에 `OAuth2Authenticator.Completed` 다음 코드 예제 에서처럼 이벤트 등록 되어야 합니다.

```csharp
authenticator.Completed += OnAuthCompleted;
```

이 이벤트는 사용자가 성공적으로 인증 하거나 로그인을 취소 하는 경우에 발생 합니다.

필요에 따라 이벤트 처리기는 `OAuth2Authenticator.Error` 이벤트를 등록할 수 있습니다.

### <a name="presenting-the-sign-in-user-interface"></a>로그인 사용자 인터페이스를 제공합니다.

로그인 사용자 인터페이스에서 각 플랫폼 프로젝트를 초기화 해야 Xamarin.Auth 로그인 발표자는를 사용 하 여 사용자에 게 표시할 수 있습니다. 다음 코드 예제에서 로그인 프레 젠 터를 초기화 하는 방법을 보여 줍니다는 `AppDelegate` iOS 프로젝트에 클래스:

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

다음 코드 예제에서 로그인 프레 젠 터를 초기화 하는 방법을 보여 줍니다는 `MainActivity` Android 프로젝트에 클래스:

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

그런 다음.NET Standard 라이브러리 프로젝트를 로그인 프레 젠 터를 다음과 같이 호출할 수 있습니다.

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

인수를 `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` 메서드는는 `OAuth2Authenticator` 인스턴스. 경우는 `Login` 메서드가 호출 되 면 로그인 사용자 인터페이스에서 다음 스크린샷에 표시 되는 장치의 웹 브라우저 탭에서 사용자에 게 표시 됩니다.

![](oauth-images/login.png "Google 로그인")

### <a name="processing-the-redirect-url"></a>리디렉션 URL을 처리합니다.

사용자 인증 프로세스 완료 된 후에 웹 브라우저 탭에서 컨트롤 응용 프로그램에 반환 됩니다. 사용자 지정 URL 구성표는 인증 프로세스 후를 검색 하 고 전송 된 후 사용자 지정 URL 처리에서 반환 되는 리디렉션 URL을 등록 하 여 수행 됩니다.

응용 프로그램을 사용 하 여 연결 하는 사용자 지정 URL 구성표를 선택할 때 응용 프로그램 이름을 기반으로 해당 제어 체계를 사용 해야 합니다. 이 iOS 및 android에서 패키지 이름에 번들 식별자 이름을 사용 하 여 URL 체계를 내릴 수 있도록를 역순으로 구현할 수 있습니다. 그러나 일부 id 공급자, Google 등 되돌릴 되며 URL 스키마로 사용 하는 도메인 이름을 기반으로 하는 클라이언트 식별자를 할당 합니다. 예를 들어 Google의 클라이언트 id를 만듭니다 `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`에서 URL 체계 됩니다 `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`합니다. 단일만 유의 `/` 체계 구성 요소 뒤에 나타날 수 있습니다. 따라서 사용자 지정 URL 구성표를 활용 하 여 리디렉션 URL의 전체 예제는 `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`합니다.

웹 브라우저의 사용자 지정 URL 구성표를 포함 하는 id 공급자 로부터 응답을 받으면 실패 하는 URL을 로드 하려고 합니다. 대신 사용자 지정 URL 구성표는 이벤트를 발생 시켜 운영 체제에 보고 됩니다. 등록 된 스키마를 운영 체제를 확인 하 고 있으면 하나 운영 체제는 체계를 등록 하는 응용 프로그램을 시작 리디렉션 URL을 보냅니다.

운영 체제를 사용 하 여 사용자 지정 URL 구성표 등록 및 스키마를 처리 하는 메커니즘 각 플랫폼에 따라 다릅니다.

#### <a name="ios"></a>iOS

사용자 지정 URL 구성표에 등록 된 ios의 경우 **Info.plist**다음 스크린샷에 표시 된 것 처럼:

![](oauth-images/info-plist.png "URL 구성표 등록")

**식별자** 아무것도 값 수 및 **역할** 값으로 설정 되어 있어야 **뷰어**합니다. 합니다 **Url 스키마** 로 시작 하는 값을 `com.googleusercontent.apps`에서 프로젝트에 대 한 iOS 클라이언트 id를 가져올 수 있습니다 [Google API 콘솔](http://console.developers.google.com)합니다.

Id 공급자에는 권한 부여 요청 완료 되 면 응용 프로그램의 리디렉션 URL로 리디렉션합니다. URL은 iOS 응용 프로그램을 시작 하면 사용자 지정 체계를 사용 하므로에서 처리 되는 시작 매개 변수로 URL에 전달 된 `OpenUrl` 응용 프로그램의 재정의 `AppDelegate` 다음 코드 예제에 나와 있는 클래스:

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    // Convert NSUrl to Uri
    var uri = new Uri(url.AbsoluteString);

    // Load redirectUrl page
    AuthenticationState.Authenticator.OnPageLoading(uri);

    return true;
}
```

합니다 `OpenUrl` 에서 받은 URL을 변환 하는 메서드는 `NSUrl` .net `Uri`, 리디렉션 URL을 처리 하기 전에 `OnPageLoading` 공용 메서드의 `OAuth2Authenticator` 개체입니다. 이렇게 하면 Xamarin.Auth 수신된 된 OAuth 데이터를 구문 분석 하 고 웹 브라우저 탭을 닫습니다.

#### <a name="android"></a>Android

Android 사용자 지정 URL 구성표를 지정 하 여 등록은 [ `IntentFilter` ](xref:Android.App.IntentFilterAttribute) 특성을 `Activity` 체계를 처리 하는. Id 공급자에는 권한 부여 요청 완료 되 면 응용 프로그램의 리디렉션 URL로 리디렉션합니다. Android 응용 프로그램을 시작 하면 사용자 지정 체계를 사용 하는 URL에 의해 처리 되는 위치는 시작 매개 변수로 URL에 전달 합니다 `OnCreate` 메서드의 `Activity` 사용자 지정 URL 체계 처리를 위해 등록 합니다. 다음 코드 예제에서는 사용자 지정 URL 체계를 처리 하는 샘플 응용 프로그램에서 클래스를 보여 줍니다.

```csharp
[Activity(Label = "CustomUrlSchemeInterceptorActivity", NoHistory = true, LaunchMode = LaunchMode.SingleTop )]
[IntentFilter(
    new[] { Intent.ActionView },
    Categories = new [] { Intent.CategoryDefault, Intent.CategoryBrowsable },
    DataSchemes = new [] { "<insert custom URL here>" },
    DataPath = "/oauth2redirect")]
public class CustomUrlSchemeInterceptorActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        // Convert Android.Net.Url to Uri
        var uri = new Uri(Intent.Data.ToString());

        // Load redirectUrl page
        AuthenticationState.Authenticator.OnPageLoading(uri);

        Finish();
    }
}
```

`DataSchemes` 의 속성을 [ `IntentFilter` ](xref:Android.App.IntentFilterAttribute) 에서 프로젝트에 Android 클라이언트 id에서 가져온 역방향된 클라이언트 식별자로 설정 되어야 합니다 [Google API 콘솔](http://console.developers.google.com)합니다.

합니다 `OnCreate` 에서 받은 URL을 변환 하는 메서드는 `Android.Net.Url` .net `Uri`, 리디렉션 URL을 처리 하기 전에 `OnPageLoading` 공용 메서드의 `OAuth2Authenticator` 개체입니다. 그러면 Xamarin.Auth를 웹 브라우저 탭을 닫고 수신된 된 OAuth 데이터를 구문 분석 합니다.

> [!IMPORTANT]
> Xamarin.Auth 사용 하 여 Android에는 `CustomTabs` 웹 브라우저 및 운영 체제와 통신 하는 API입니다. 그러나이 확실 하지는 `CustomTabs` 호환 브라우저가 사용자의 장치에 설치 됩니다.

### <a name="examining-the-oauth-response"></a>OAuth 응답을 검토

수신된 된 OAuth 데이터 구문 분석 후 Xamarin.Auth 시킵니다는 `OAuth2Authenticator.Completed` 이벤트입니다. 이 이벤트에 대 한 이벤트 처리기에서는 `AuthenticatorCompletedEventArgs.IsAuthenticated` 속성을 다음 코드 예제와 같이 인증이 성공 여부를 확인에 사용할 수 있습니다.

```csharp
async void OnAuthCompleted(object sender, AuthenticatorCompletedEventArgs e)
{
  ...
  if (e.IsAuthenticated)
  {
    ...
  }
}
```

인증에 성공 하면에서 수집 된 데이터의 수를 `AuthenticatorCompletedEventArgs.Account` 속성입니다. Id 공급자에서 제공 하는 API에는 데이터에 대 한 요청을 서명 하는 액세스 토큰을 포함 합니다.

### <a name="making-requests-for-data"></a>데이터에 대 한 요청

요청을 수행 하는 응용 프로그램은 액세스 토큰을 가져옵니다, 후는 `https://www.googleapis.com/oauth2/v2/userinfo` id 공급자의 기본 사용자 데이터를 요청에 API. Xamarin.Auth의이 요청이 수행 `OAuth2Request` 에서 검색 하는 계정을 사용 하 여 인증 된 요청을 나타내는 클래스는 `OAuth2Authenticator` 다음 코드 예제에 표시 된 대로 인스턴스:

```csharp
// UserInfoUrl = https://www.googleapis.com/oauth2/v2/userinfo
var request = new OAuth2Request ("GET", new Uri (Constants.UserInfoUrl), null, e.Account);
var response = await request.GetResponseAsync ();
if (response != null)
{
  string userJson = response.GetResponseText ();
  var user = JsonConvert.DeserializeObject<User> (userJson);
}
```

뿐만 아니라 HTTP 메서드 및 API URL을 `OAuth2Request` 인스턴스 수도 지정 합니다.는 `Account` 로 지정 된 URL로 요청을 서명 하는 액세스 토큰을 포함 하는 인스턴스는 `Constants.UserInfoUrl` 속성. Id 공급자 액세스 토큰이 유효한 것으로 인식 된 사용자 이름 및 전자 메일 주소를 포함 하 여 JSON 응답을으로 기본적인 사용자 데이터를 반환 합니다. JSON 응답은 다음 읽고 deserialize 된 `user` 변수입니다.

자세한 내용은 [Google API 호출](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) Google 개발자 포털에서 합니다.

### <a name="storing-and-retrieving-account-information-on-devices"></a>저장 하 고 장치에 대 한 계정 정보를 검색 합니다.

Xamarin.Auth 안전 하 게 저장 `Account` 응용 프로그램 수행 하지 않도록 항상 다시 사용자를 인증 하는 계정의 개체에에서 저장 합니다. `AccountStore` 클래스는 계정 정보를 저장 하는 일을 담당 하며 키 집합 서비스 iOS에서 지원 되 고 `KeyStore` android에서 클래스입니다.

다음 코드 예제에서는 어떻게는 `Account` 개체를 안전 하 게 저장 합니다.

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

저장 된 계정은 고유 하 게 식별 하 여 구성 된 계정 키를 사용 하 여 `Username` 속성과 서비스 ID를 계정 저장소에서 가져오는 계정 때 사용 되는 문자열입니다. 경우는 `Account` 이전에 저장, 호출 된 `Save` 메서드 다시 덮어씁니다.

`Account` 서비스를 호출 하 여 검색할 수에 대 한 개체는 `FindAccountsForService` 메서드를 다음 코드 예제 에서처럼:

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

`FindAccountsForService` 메서드가 반환 되는 `IEnumerable` 의 컬렉션 `Account` 개체와 일치 하는 계정으로 설정 되 고 컬렉션의 첫 번째 항목.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Auth를 사용 하 여 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하는 방법을 설명 합니다. Xamarin.Auth 제공 합니다 `OAuth2Authenticator` 고 `OAuth2Request` Xamarin.Forms 응용 프로그램에서 Google, Microsoft, Facebook 및 Twitter와 같은 id 공급자를 사용 하는 데 사용 되는 클래스입니다.

## <a name="related-links"></a>관련 링크

- [OAuthNativeFlow (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)
- [네이티브 앱에 대 한 OAuth 2.0](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Oauth 2.0을 사용 하 여 Google Api에 액세스 하기](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
