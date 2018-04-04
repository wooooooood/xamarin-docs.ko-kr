---
title: Id 공급자를 통해 사용자 인증
description: Xamarin.Auth는 계정을 저장 하 고 사용자를 인증 하기 위해 플랫폼 SDK 사용 되며 Google, Microsoft, Facebook 및 Twitter와 같은 id 공급자를 사용 하기 위한 지원을 제공 하는 OAuth 인증자를 포함 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하기 Xamarin.Auth를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: 26e85a37cfd36b5d4f045273548efafccca79e1a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="authenticating-users-with-an-identity-provider"></a>Id 공급자를 통해 사용자 인증

_Xamarin.Auth는 계정을 저장 하 고 사용자를 인증 하기 위해 플랫폼 SDK 사용 되며 Google, Microsoft, Facebook 및 Twitter와 같은 id 공급자를 사용 하기 위한 지원을 제공 하는 OAuth 인증자를 포함 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하기 Xamarin.Auth를 사용 하는 방법을 설명 합니다._

OAuth 인증을 위한 개방형 표준인 이며 리소스 소유자 id를 공유 하지 않고 해당 정보에 액세스 하는 제 3 자에 해당 사용 권한을 부여 해야 리소스 공급자에 알리기 위해 리소스 소유자 수 있습니다. 이러한 예제를 권한이 사용자의 id를 공유 하지 않고 해당 데이터에 액세스 하는 응용 프로그램에 부여 해야 하는 id 공급자 (예: Google, Microsoft, Facebook 또는 Twitter)에 알리기 위해 사용자를 가능 하 게 합니다. 일반적으로 로그인 하는 id 공급자를 사용 하 여 응용 프로그램 및 웹 사이트에 있지만 웹 사이트 또는 응용 프로그램에 자신의 암호를 노출 하지 않고 사용자에 대 한 방법으로 사용 됩니다.

상위 수준 개요를 OAuth id 공급자를 사용할 때는 인증 흐름은 다음과 같습니다.

1. 응용 프로그램 id 공급자 URL 브라우저에서 탐색 합니다.
1. Id 공급자는 사용자 인증을 처리 하 고 응용 프로그램에 권한 부여 코드를 반환 합니다.
1. 응용 프로그램은 id 공급자에서 액세스 토큰에 대 한 권한 부여 코드를 교환 합니다.
1. 응용 프로그램에 id 공급자의 기본 사용자 데이터를 요청에 대 한 API와 같은 Api에 액세스 하려면 액세스 토큰을 사용 합니다.

샘플 응용 프로그램에 Google에 대 한 기본 인증 흐름을 구현 하기 Xamarin.Auth를 사용 하는 방법을 보여 줍니다. Google을 사용 하 여이 항목의 id 공급자로, 방법은 다른 id 공급자에 모두 적용 합니다. Google OAuth 2.0 끝점을 사용 하 여 인증 하는 방법에 대 한 자세한 내용은 참조 [Google Api 액세스를 사용 하 여 oauth 2.0](https://developers.google.com/identity/protocols/OAuth2) Google의 웹 사이트에 있습니다.

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간에 보안 연결 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 적용 합니다. IOS 9에 대해 빌드된 앱에서 기본적으로 AT를 사용 하므로 모든 연결 AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 AT 옵트인 수 있습니다는 `HTTPS` 및 프로토콜을 인터넷 리소스에 대 한 통신을 보호 합니다. 응용 프로그램의 업데이트 하 여 이렇게 할 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="using-xamarinauth-to-authenticate-users"></a>Xamarin.Auth를 사용 하 여 사용자를 인증 합니다.

Xamarin.Auth 응용 프로그램이 id 공급자의 권한 부여 끝점와 상호 작용 하기 위한 두 가지 방법을 지원 합니다.

1. 포함 된 웹 보기를 사용 합니다. 이 방식은 일반적 이었습니다, 것이 더 이상 좋습니다 이유는 다음과 같습니다.

    - 웹 보기를 호스팅하는 응용 프로그램에는 응용 프로그램에 사용 하는 OAuth 인증 권한 뿐 아니라 사용자의 전체 인증 자격 증명을 액세스할 수 있습니다. 이 방법은 응용 프로그램에 필요한 응용 프로그램의 공격 노출 영역을 향상 시키는 것 보다 더 강력한 자격 증명에 액세스할 때 최소 권한의 원칙을 위반 합니다.
    - 호스트 응용 프로그램 수 사용자 이름 및 암호를 캡처, 자동으로 양식을 전송할 사용자 동의 무시 하 고 세션 쿠키 및 사용자로 인증 된 작업을 수행 하는 데 사용할 합니다.
    - 포함 된 웹 보기 밑 사용자 환경을 간주 되는 모든 권한 부여 요청에 대 한 로그인에 사용자 장치의 웹 브라우저 또는 다른 응용 프로그램 인증 상태를 공유 하지 않습니다.
    - 일부 권한 부여 끝점 감지 웹 보기에서 제공 하는 권한 부여 요청을 차단 하는 단계를 수행 합니다.

1. 권장 되는 방법인 장치의 웹 브라우저를 사용 합니다. 장치 브라우저를 사용 하 여 OAuth 요청에 대 한 사용자만의 응용 프로그램에서 로그인 및 권한 부여 흐름 변환율 향상 장치에 한 번씩 id 공급자에 로그인 해야 할 때 응용 프로그램의 유용성이 향상 됩니다. 장치 브라우저 응용 프로그램은 검사 하 고 브라우저에 표시 하는 콘텐츠 하지는 웹 보기의 콘텐츠를 수정할 수 향상 된 보안도 제공 합니다. 이 문서 및 예제 응용 프로그램에서 사용 하는 접근 방식입니다.

샘플 응용 프로그램 사용자를 인증 하 고 기본 데이터를 검색 하려면 Xamarin.Auth을 사용 하는 방법에 대해 개략적 다음 다이어그램에 표시 됩니다.

![](oauth-images/google-auth.png "Xamarin.Auth를 사용 하 여 google 인증")

응용 프로그램이 사용 하 여 Google에 인증을 요청 하는 `OAuth2Authenticator` 클래스입니다. 액세스 토큰을 포함 하는 로그인 페이지를 통해 google 사용자가 성공적으로 인증 되 면 인증 응답 반환 됩니다. 그런 다음 응용 프로그램 요청 Google을 사용 하 여 기본 사용자 데이터에 대 한 보냅니다는 `OAuth2Request` 요청에 포함 되 고 액세스 토큰으로 클래스입니다.

### <a name="setup"></a>설정

Google 로그인 Xamarin.Forms 응용 프로그램과 통합 하는 Google API 콘솔 프로젝트를 만들어야 합니다. 이렇게 하려면 다음을 수행합니다.

1. 이동 하 여 [Google API 콘솔](http://console.developers.google.com) 웹 사이트 및 Google 계정 자격 증명을 사용 하 여 로그인 합니다.
1. 프로젝트 드롭다운 목록에서 기존 프로젝트를 선택 하거나 새로 만듭니다.
1. "API Manager"에서 세로 막대에서 선택 **자격 증명**을 선택한 후는 **OAuth 동의 화면 탭**합니다. 선택 된 **전자 메일 주소**, 지정는 **사용자에 게 표시 하는 제품 이름**, 한 키를 누릅니다 **저장**합니다.
1. 에 **자격 증명** 탭을는 **자격 증명을 만들** 드롭 다운 목록에서 선택한 **OAuth 클라이언트 ID**합니다.
1. 아래 **응용 프로그램 종류**, 모바일 응용 프로그램에서 실행 하는 플랫폼을 선택 (**iOS** 또는 **Android**).
1. 필요한 세부 정보를 입력 하 고 선택 된 **만들기** 단추입니다.

> [!NOTE]
> 클라이언트 ID 활성화 된 Google Api에 액세스 하는 응용 프로그램 및 모바일 응용 프로그램에는 단일 플랫폼에 고유 합니다. 따라서 한 **OAuth 클라이언트 ID** ´ ֲ Google 로그인 하는 각 플랫폼에 대해 만들어야 합니다.

다음이 단계를 수행한 후 Xamarin.Auth google는 OAuth2 인증 흐름을 시작 하려면 사용할 수 있습니다.

### <a name="creating-and-configuring-an-authenticator"></a>만들기 및 인증자를 구성 합니다.

Xamarin.Auth의 `OAuth2Authenticator` 클래스는 OAuth 인증 흐름 처리를 담당 합니다. 다음 코드 예제에서는의 인스턴스화는 `OAuth2Authenticator` 장치의 웹 브라우저를 사용 하 여 인증을 수행할 때 클래스:

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

- **클라이언트 ID** –이 식별, 요청을 수행 하 고 프로젝트에서 검색할 수 있는 클라이언트는 [Google API 콘솔](http://console.developers.google.com)합니다.
- **클라이언트 암호** –이 되어야 `null` 또는 `string.Empty`합니다.
- **범위** –이 응용 프로그램에 의해 요청 된 API 액세스를 식별 하 고 값 사용자에 게 표시 된 동의 화면에 알립니다. 범위에 대 한 자세한 내용은 참조 [API 권한 부여 요청](https://developers.google.com/+/web/api/rest/oauth) Google의 웹 사이트에 있습니다.
- **URL 권한 부여** – URL 권한 부여 코드에서 얻을 수 있습니다 위치를 식별 합니다.
- **리디렉션 URL** –이 응답을 보낼 수는 URL을 식별 합니다. 이 매개 변수 값에 표시 되는 값 중 하 나와 일치 해야 합니다는 **자격 증명** 탭에서 프로젝트에 [Google 개발자 콘솔](https://console.developers.google.com/)합니다.
- **AccessToken Url** –이 권한 부여 코드를 가져온 후 액세스 토큰을 요청 하는 데 사용 하는 URL을 식별 합니다.
- **GetUserNameAsync Func** – 선택적는 `Func` 성공적으로 인증 된는 계정의 사용자 이름을 비동기적으로 검색 하는 데 사용 될 됩니다.
- **네이티브 UI를 사용 하 여** –는 `boolean` 인증 요청을 수행 하는 장치의 웹 브라우저를 사용할지 여부를 나타내는 값입니다.

### <a name="setup-authentication-event-handlers"></a>인증 이벤트 처리기를 설정 합니다.

사용자 인터페이스에 대 한 이벤트 처리기를 제공 하기 전에 `OAuth2Authenticator.Completed` 다음 코드 예제와 같이 이벤트 등록 되어야 합니다.

```csharp
authenticator.Completed += OnAuthCompleted;
```

이 이벤트는 사용자가 성공적으로 인증 하거나 로그인을 취소 하는 경우에 발생 합니다.

필요에 따라에 대 한 이벤트 처리기는 `OAuth2Authenticator.Error` 이벤트를 등록할 수 있습니다.

### <a name="presenting-the-sign-in-user-interface"></a>로그인 사용자 인터페이스를 제공합니다.

각 플랫폼 프로젝트에서 초기화 해야 Xamarin.Auth 로그인 발표자는를 사용 하 여 사용자에 게 로그인 사용자 인터페이스를 표시할 수 있습니다. 다음 코드 예제에서는에서 로그인 발표자 초기화 하는 방법을 보여 줍니다.는 `AppDelegate` iOS 프로젝트에 클래스:

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

다음 코드 예제에서는에서 로그인 발표자 초기화 하는 방법을 보여 줍니다.는 `MainActivity` Android 프로젝트의 클래스:

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

클래스 라이브러리 PCL (이식 가능한) 프로젝트 로그인 발표자 다음과 같이 호출할 수 있습니다.

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

인수는 `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` 메서드는는 `OAuth2Authenticator` 인스턴스. 경우는 `Login` 메서드가 호출 되 면 다음 스크린샷에 표시 되는 장치의 웹 브라우저에서 로그인 사용자 인터페이스는 탭에 사용자에 게 제공 합니다.

![](oauth-images/login.png "Google 로그인")

### <a name="processing-the-redirect-url"></a>리디렉션 URL을 처리합니다.

인증 프로세스를 완료 하는 사용자, 웹 브라우저 탭에서 응용 프로그램에 제어가 돌아옵니다. 이 사용자 지정 URL 체계는 인증 프로세스 후를 검색 하 고 전송 된 후 사용자 지정 URL을 처리에서 반환 되는 리디렉션 URL에 대 한 등록 하 여 달성 됩니다.

응용 프로그램에 연결할 사용자 지정 URL 구성표를 선택할 때는 응용 프로그램 이름을 기반으로 해당 제어 하는 체계를 사용 해야 합니다. URL 구성표를 변경 하거나 반전 하거나를 iOS 및 android에서는 패키지 이름에서 번들 식별자 이름을 사용 하 여이 작업을 수행할 수 있습니다. 그러나, Google 등의 일부 id 공급자는 다음 반대로 변경 하 고 URL 스키마로 사용 되는 도메인 이름을 기반으로 하는 클라이언트 식별자를 할당 합니다. 예를 들어, Google의 클라이언트 id를 만드는 경우 `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`, URL 체계 됩니다 `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`합니다. 단일만 참고 `/` 스키마 구성 요소 뒤에 나타날 수 있습니다. 따라서 사용자 지정 URL 체계를 사용 하 여 리디렉션 URL의 완전 한 예제는 `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`합니다.

웹 브라우저의 사용자 지정 URL 구성표를 포함 하는 id 공급자에서 응답을 받으면 실패 URL을 로드 하려고 합니다. 대신, 사용자 지정 URL 체계는 이벤트가 발생 하 여 운영 체제에 보고 됩니다. 운영 체제는 다음 등록 된 구성표에 대 한 확인를 운영 체제는 구성표를 등록 하는 응용 프로그램을 시작 되며 리디렉션 URL을 보냅니다가 있는 경우.

운영 체제와 사용자 지정 URL 체계를 등록 하 고 체계를 처리 하기 위한 메커니즘은 각 플랫폼에 관련이 있습니다.

#### <a name="ios"></a>iOS

IOS 사용자 지정 URL 구성표는에 등록 **Info.plist**다음 스크린샷에 표시 된 것 처럼:

![](oauth-images/info-plist.png "URL 구성표 등록")

**식별자** 값 아무 것도 가능 및 **역할** 값으로 설정 되어 있어야 **뷰어**합니다. **Url 스키마** 로 시작 하는 값 `com.googleusercontent.apps`를 받을 수 있는 프로젝트에 대 한 iOS 클라이언트 id에서 [Google API 콘솔](http://console.developers.google.com)합니다.

권한 부여 요청을 완료 하는 id 공급자 응용 프로그램의 리디렉션 URL로 리디렉션합니다. URL에 응용 프로그램을 시작 하는 iOS 사용자 지정 체계를 사용 하므로에서 처리 되는 시작 매개 변수로 URL에 전달 된 `OpenUrl` 응용 프로그램의 재정의 `AppDelegate` 다음 코드 예제에 표시 된 클래스:

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

`OpenUrl` 메서드 변환에서 받은 URL는 `NSUrl` .net `Uri`와 리디렉션 URL을 처리 하기 전에 `OnPageLoading` 공용 메서드의 `OAuth2Authenticator` 개체입니다. 이렇게 하면 Xamarin.Auth 수신된 된 OAuth 데이터를 구문 분석할와 웹 브라우저 탭을 닫습니다.

#### <a name="android"></a>Android

Android 사용자 지정 URL 구성표는 지정 하 여 등록는 [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) 특성에 `Activity` 체계에서 처리할지를 합니다. 권한 부여 요청을 완료 하는 id 공급자 응용 프로그램의 리디렉션 URL로 리디렉션합니다. 응용 프로그램을 시작 하는 Android에서 발생 하는 사용자 지정 체계를 사용 하는 URL을 URL에 매개 변수로 전달 하는 시작에서 처리 되는 `OnCreate` 의 메서드는 `Activity` 사용자 지정 URL 체계 처리를 위해 등록 합니다. 다음 코드 예제에서는 사용자 지정 URL 체계를 처리 하는 샘플 응용 프로그램에서 클래스를 보여 줍니다.

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

`DataSchemes` 의 속성은 [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) 하에 프로젝트에 대 한 Android 클라이언트 id에서 가져온 역방향된 클라이언트 식별자로 설정 해야 [Google API 콘솔](http://console.developers.google.com)합니다.

`OnCreate` 메서드 변환에서 받은 URL는 `Android.Net.Url` .net `Uri`와 리디렉션 URL을 처리 하기 전에 `OnPageLoading` 공용 메서드의 `OAuth2Authenticator` 개체입니다. 이렇게 하면 Xamarin.Auth를 웹 브라우저 탭을 닫고 수신된 된 OAuth 데이터를 구문 분석 됩니다.

> [!IMPORTANT]
> Android에서는 Xamarin.Auth 사용는 `CustomTabs` 웹 브라우저 및 운영 체제와 통신 하는 API입니다. 그러나이 보장 하지 않습니다는 `CustomTabs` 호환 브라우저 사용자의 장치에 설치 됩니다.

### <a name="examining-the-oauth-response"></a>OAuth 응답 검사

수신된 된 OAuth 데이터를 구문 분석 한 후 Xamarin.Auth 시킵니다는 `OAuth2Authenticator.Completed` 이벤트입니다. 이 이벤트에 대 한 이벤트 처리기에는 `AuthenticatorCompletedEventArgs.IsAuthenticated` 다음 코드 예제에 표시 된 대로 인증 성공 여부를 식별 하려면 속성을 사용할 수 있습니다.

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

성공적으로 인증에서 수집한 데이터를 사용할 수는 `AuthenticatorCompletedEventArgs.Account` 속성입니다. 데이터는 id 공급자에서 제공 되는 API에 대 한 요청을 서명 하는 데 사용할 수 있는 액세스 토큰에 포함 됩니다.

### <a name="making-requests-for-data"></a>데이터에 대 한 요청을 수행합니다.

요청을 수행 하는 데 사용 된 응용 프로그램 액세스 토큰을 얻고 나면는 `https://www.googleapis.com/oauth2/v2/userinfo` API 기본 사용자 데이터는 id 공급자를 요청 합니다. 이 요청은 Xamarin.Auth의 `OAuth2Request` 에서 검색 하는 계정을 사용 하 여 인증 된 요청을 나타내는 클래스는 `OAuth2Authenticator` 다음 코드 예제와 같이 인스턴스:

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

HTTP 메서드 및 API URL의 `OAuth2Request` 인스턴스도 지정는 `Account` 하 여 지정 된 URL로 요청에 서명 하는 액세스 토큰을 포함 하는 인스턴스는 `Constants.UserInfoUrl` 속성. Id 공급자는 다음을 포함 하 여 사용자의 이름 및 전자 메일 주소를 액세스 토큰이 유효한 것으로 인식 하 여 JSON 응답 메시지로 기본 사용자 데이터를 반환 합니다. JSON 응답은 다음 읽고으로 deserialize 하는 `user` 변수입니다.

자세한 내용은 참조 [Google API를 호출](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) Google 개발자 포털에 있습니다.

### <a name="storing-and-retrieving-account-information-on-devices"></a>저장 하 고 장치에서 계정 정보를 검색 합니다.

Xamarin.Auth 안전 하 게 저장 `Account` 개체는 계정에 저장할 응용 프로그램 사용자를 다시 인증 하려면 항상 보유 하지 않습니다. `AccountStore` 클래스는를 계정 정보를 저장 하 고 ios에서는 키 집합 서비스에 의해 지원 됩니다 및 `KeyStore` Android에는 클래스입니다.

다음 코드 예제와 어떻게는 `Account` 개체가 안전 하 게 저장 됩니다.

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

저장 된 계정을 고유 하 게 식별 계정으로 구성 하는 키를 사용 하 여 `Username` 속성과 서비스 ID로, 저장소 계정에서에서 계정 인출 때 사용 되는 문자열입니다. 경우는 `Account` 이전 저장, 호출 된 `Save` 메서드 다시 덮어씁니다.

`Account` 서비스를 호출 하 여 검색할 수에 대 한 개체는 `FindAccountsForService` 메서드를 다음 코드 예제에 나와 있는 것 처럼:

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

`FindAccountsForService` 메서드가 반환 되는 `IEnumerable` 의 컬렉션 `Account` 개체와 일치 하는 계정으로 설정 되 고 컬렉션의 첫 번째 항목입니다.

## <a name="summary"></a>요약

이 문서 Xamarin.Auth Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리 하려면 사용 하는 방법을 설명 합니다. Xamarin.Auth 제공는 `OAuth2Authenticator` 및 `OAuth2Request` Google, Microsoft, Facebook 및 Twitter와 같은 id 공급자를 사용 하는 Xamarin.Forms 응용 프로그램에서 사용 되는 클래스입니다.


## <a name="related-links"></a>관련 링크

- [OAuthNativeFlow (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/OAuthNativeFlow/)
- [네이티브 응용 프로그램에 대 한 OAuth 2.0](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Oauth 2.0을 사용 하 여 Google Api에 액세스 하려면](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
