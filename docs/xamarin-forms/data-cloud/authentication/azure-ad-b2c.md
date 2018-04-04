---
title: Azure Active Directory B2C 있는 사용자를 인증합니다.
description: Azure Active Directory B2C 연결 소비자 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리 솔루션입니다. 이 문서에서는 Microsoft 인증 라이브러리와 Azure Active Directory B2C 소비자 id 관리를 모바일 응용 프로그램 통합을 사용 하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: f17a6ad012aff81674db943b7d65e65ba77dca52
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Azure Active Directory B2C 있는 사용자를 인증합니다.

_Azure Active Directory B2C 연결 소비자 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리 솔루션입니다. 이 문서에서는 Microsoft 인증 라이브러리와 Azure Active Directory B2C 소비자 id 관리를 모바일 응용 프로그램 통합을 사용 하는 방법을 보여줍니다._

![](~/media/shared/preview.png "이 API는 현재 시험판 버전")

> [!NOTE]
> [Microsoft 인증 라이브러리](https://www.nuget.org/packages/Microsoft.Identity.Client) 미리 보기 상태, 하지만 프로덕션 환경에서 사용 하기에 적합 합니다. 그러나 있을 수 있습니다 수의 주요 변경 내용 API, 내부 캐시 형식 및 응용 프로그램에 영향을 줄 수 있는 라이브러리의 다른 메커니즘입니다.

## <a name="overview"></a>개요

Azure Active Directory B2C은 소비자가에 로그인 하 여 응용 프로그램에 있는 소비자 용 응용 프로그램에 대 한 id 관리 서비스입니다.

- 기존 공유 계정 (Microsoft, Google, Facebook, Amazon, LinkedIn)를 사용합니다.
- 새 자격 증명 (전자 메일 주소 및 암호 또는 사용자 이름 및 암호)을 만듭니다. 이러한 자격 증명 라고 *로컬* 계정.

Azure Active Directory B2C id 관리 서비스로 모바일 응용 프로그램으로 통합 하기 위한 프로세스는 다음과 같습니다.

1. Azure Active Directory B2C 테 넌 트를 만듭니다. 자세한 내용은 참조 [Azure 포털에서 Azure Active Directory B2C 테 넌 트를 만들기](/azure/active-directory-b2c/active-directory-b2c-get-started/)합니다.
1. Azure Active Directory B2C 테 넌 트와 모바일 응용 프로그램을 등록 합니다. 등록 프로세스에서 할당 한 **응용 프로그램 ID** 고유 하 게 식별 하는 응용 프로그램 및 **리디렉션 URL** 직접 응용 프로그램에 응답을 다시 사용할 수 있는 합니다. 자세한 내용은 참조 [Azure Active Directory B2C: 응용 프로그램 등록](/azure/active-directory-b2c/active-directory-b2c-app-registration/)합니다.
1. 등록 및 로그인 정책을 만듭니다. 이 정책은 소비자를 등록 및 로그인 하는 동안 통과 하는 환경을 정의 합니다를 응용 프로그램을 수신할 토큰의 내용을 성공적으로에 지정 등록 또는 로그인 합니다. 자세한 내용은 참조 [Azure Active Directory B2C: 기본 제공 정책](/azure/active-directory-b2c/active-directory-b2c-reference-policies/)합니다.
1. 사용 하 여 [Microsoft 인증 라이브러리](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) Azure Active Directory B2C 테 넌 트와 인증 하는 워크플로 시작 하려면 모바일 응용 프로그램에서 합니다.

> [!NOTE]
> 모바일 응용 프로그램에 Azure Active Directory B2C id 관리를 통합, 뿐만 아니라 모바일 응용 프로그램에 Azure Active Directory id 관리를 통합 하 MSAL도 사용할 수 있습니다. Azure Active directory에서 모바일 응용 프로그램을 등록 하 여이 작업을 수행할 수 있습니다는 [응용 프로그램 등록 포털](https://apps.dev.microsoft.com/)합니다. 등록 프로세스에서 할당 한 **응용 프로그램 ID** MSAL를 사용 하는 경우 지정할 수 있는 사용자 응용 프로그램을 고유 하 게 식별 합니다. 자세한 내용은 참조 [v2.0 끝점과 응용 프로그램을 등록 하는 방법을](/azure/active-directory/develop/active-directory-v2-app-registration/), 및 [인증 Your 모바일 앱 사용 하 여 Microsoft 인증 라이브러리](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) Xamarin 블로그.

MSAL 인증을 수행 하는 장치의 웹 브라우저를 사용 합니다. 사용자만 로그인 할 장치, 응용 프로그램에서 흐름의 로그인 및 권한 부여 변환율 향상 되 면 필요에 따라 응용 프로그램의 유용성이 향상 됩니다. 또한 장치 브라우저 보안 향상된을 제공합니다. 인증 프로세스를 완료 하는 사용자, 웹 브라우저 탭에서 응용 프로그램에 제어가 돌아옵니다. 이 사용자 지정 URL 체계는 인증 프로세스 후를 검색 하 고 전송 된 후 사용자 지정 URL을 처리에서 반환 되는 리디렉션 URL에 대 한 등록 하 여 달성 됩니다. 사용자 지정 URL 구성표를 선택 하는 방법에 대 한 자세한 내용은 참조 [네이티브 응용 프로그램 리디렉션 URI 선택](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/)합니다.

> [!NOTE]
> 운영 체제와 사용자 지정 URL 체계를 등록 하 고 체계를 처리 하기 위한 메커니즘은 각 플랫폼에 관련이 있습니다.

Azure Active Directory B2C 테 넌 트에 전송 된 각 요청에 지정 된 *정책*합니다. 정책 등록 또는 로그인 시와 같은 소비자 identity 경험에 설명 합니다. 예를 들어 등록 정책에서 다음 설정을 통해 구성 될 Azure Active Directory B2C 테 넌 트의 동작 허용:

- 소비자 응용 프로그램에 로그인 하는 데 사용할 수 있는 계정 유형입니다.
- 데이터를 수집 하려면 소비자에서 등록 하는 동안 합니다.
- 다단계 인증 합니다.
- 등록 페이지 콘텐츠입니다.
- 모바일 응용 프로그램 정책이 실행 된 때를 수신 하는 토큰 클레임입니다.

Azure Active Directory 테 넌 트 필요에 따라 응용 프로그램에서 사용할 수 있는 다양 한 유형의 여러 정책이 포함 될 수 있습니다. 또한 정책은 정의 하 고 코드를 변경 하지 않고 소비자 identity 경험을 수정할 수 있도록 응용 프로그램 간에 재사용할 수 있습니다. 정책에 대 한 자세한 내용은 참조 [Azure Active Directory B2C: 기본 제공 정책](/azure/active-directory-b2c/active-directory-b2c-reference-policies/)합니다.

## <a name="setup"></a>설정

Microsoft 인증 라이브러리 (MSAL) NuGet 라이브러리는 Xamarin.Forms 솔루션에서 플랫폼 프로젝트와 PCL 이식 가능한 클래스 라이브러리 () 프로젝트에 추가 되어야 합니다. 다음 섹션에서는 MSAL 모바일 응용 프로그램에서 Azure Active Directory B2C 테 넌 트와 통신 하는 데 사용 되는 추가 설정 지침을 제공 합니다.

### <a name="portable-class-library"></a>이식 가능한 클래스 라이브러리

MSAL Windows Phone 8.1을 지원 하지 않습니다 하 고 있으므로 PCLs MSAL를 사용 하는 됩니다이 대상을 제거 해야 합니다. 이 변경 내용 대상 PCLs Profile7를 사용 하 여 수행할 수 있습니다. PCL에 대한 자세한 내용은 [이식 가능한 클래스 라이브러리 소개](~/cross-platform/app-fundamentals/pcl.md)를 참조하세요.

### <a name="ios"></a>iOS

Ios, Azure Active Directory B2C에 등록 된 사용자 지정 URL 체계에 등록 해야 **Info.plist**다음 스크린샷에 표시 된 것 처럼:

![](azure-ad-b2c-images/customurl-ios.png "Ios 사용자 지정 URL 구성표를 등록 하는 중")

Azure Active Directory B2C에 권한 부여 요청 완료 되 면 등록 된 리디렉션 URL로 리디렉션합니다. URL에 모바일 응용 프로그램을 시작 하는 iOS 사용자 지정 체계를 사용 하므로에서 처리 되는 시작 매개 변수로 URL에 전달 된 `OpenUrl` 응용 프로그램의 재정의 `AppDelegate` 다음 코드에 표시 된 클래스 예:

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
            return true;
        }
    }
}
```

코드는 `OpenURL` 메서드를 사용 하면 대화형 부분의 인증 워크플로가 종료 된 후 제어가 MSAL를 반환 합니다.

### <a name="android"></a>Android

Android에서는 Azure Active Directory B2C에 등록 된 사용자 지정 URL 체계에 등록 해야 **AndroidManifest.xml**를 추가 하 여는 `<activity>` 기존 내부 요소 `<application>` 요소입니다. `<activity>` 요소를 지정는 `IntentFilter` 에 `Activity` 체계를 처리 하 고 다음 예제에 표시 됩니다.

```xml
<application ...>
  <activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
      <action android:name="android.intent.action.VIEW" />
      <category android:name="android.intent.category.DEFAULT" />
      <category android:name="android.intent.category.BROWSABLE" />
      <data android:scheme="INSERT_URL_SCHEME_HERE" android:host="auth" />
    </intent-filter>
  </activity>
</application>
```

Azure Active Directory B2C에 권한 부여 요청 완료 되 면 등록 된 리디렉션 URL로 리디렉션합니다. URL이 모바일 응용 프로그램을 시작 하는 Android에서 발생 하는 사용자 지정 체계를 사용 하므로에서 처리 되는 시작 매개 변수로 URL에 전달 된 `microsoft.identity.client.BrowserTabActivity`합니다. `data android:scheme` 등록 된 Azure Active Directory B2C 응용 프로그램의 사용자 지정 URL 구성표 속성을 설정 해야 합니다.

또한는 `MainActivity` 다음 코드 예제와 같이 클래스 수정할 수 있어야 합니다.

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            global::Xamarin.Forms.Forms.Init(this, bundle);
            Microsoft.WindowsAzure.MobileServices.CurrentPlatform.Init();
            LoadApplication(new App());
            App.UiParent = new UIParent(this);
        }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
        }
    }
}

```

`OnCreate` 메서드를 할당 하 여 수정 되는 `UIParent` 인스턴스는 `App.UiParent` 속성. 이렇게 하면 인증 흐름 현재 활동의 컨텍스트에서 발생 하 합니다.

코드는 `OnActivityResult` 메서드를 사용 하면 대화형 부분의 인증 워크플로가 종료 된 후 제어가 MSAL를 반환 합니다.

### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

유니버설 Windows 플랫폼에서 추가 설정 없이 MSAL 사용 하려면 필요 합니다.

## <a name="initialization"></a>초기화

멤버를 사용 하는 Microsoft 인증 라이브러리는 `PublicClientApplication` 인증 하는 워크플로 시작 하는 클래스입니다. 샘플 응용 프로그램 선언 하 고 초기화는 `public` 이라는이 유형의 속성 `ADB2CClient`에 `AuthenticationProvider` 클래스입니다. 다음 코드 예제에서는이 속성은 초기화 하는 방법을 보여 줍니다.

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

Azure Active Directory B2C 테 넌 트와 등록 된 모바일 응용 프로그램 등록 프로세스에서 할당 한 **응용 프로그램 ID**합니다. 이 ID를 지정 해야 합니다는 `PublicClientApplication` 생성자와 함께 `Authority` 실행할 기본 URL 및 Azure Active Directory B2C 정책을 구성 하는 상수입니다.

## <a name="signing-in"></a>로그인

다음 스크린샷에서 샘플 응용 프로그램에서 로그인 화면을 보여 줍니다.

![](azure-ad-b2c-images/login.png "로그인 페이지")

로그인 소셜 id 공급자와 또는 로컬 계정을 사용 하 여 일반적으로 허용 됩니다. 위에 나와 있는 것 처럼 소셜 id 공급자로 사용 되지만 Microsoft, Google, 및 Facebook, 기타 id 공급자 사용할 수도 있습니다.

다음 코드 예제에서는 로그인 프로세스는 호출 하는 방법을 보여 줍니다.

```csharp
using Microsoft.Identity.Client;

public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);
    ...
}

```

`AcquireTokenAsync` 장치의 웹 브라우저를 시작 하 고 정책을 통해 참조에 의해 지정 된 Azure Active Directory B2C 정책에 정의 된 인증 옵션을 표시 하는 메서드는 `Constants.Authority` 상수입니다. 이 정책은 등록 하는 동안 및 로그인을 소비자에 게 전송 되는 환경 및 클레임을 응용 프로그램 등록 또는 로그인에 성공적으로 받게 됩니다 정의 합니다.

결과 `AcquireTokenAsync` 메서드 호출은 `AuthenticationResult` 인스턴스. 인증이 성공 하는 경우는 `AuthenticationResult` 인스턴스 id 토큰을 로컬로 캐시에 포함 됩니다. 인증이 성공 하는 경우는 `AuthenticationResult` 인스턴스 인증에 실패 한 이유를 나타내는 데이터를 포함 됩니다.

샘플 응용 프로그램 인증이 성공 하는 경우에 `TodoList` 페이지를 탐색 합니다.

## <a name="silent-re-authentication"></a>자동 다시 인증

경우는 `LoginPage` 샘플에서 응용 프로그램이 표시 인증 사용자 인터페이스를 표시 하지 않고 사용자 토큰을 검색 하려고 시도 합니다. 이 통해는 `AcquireTokenSilentAsync` 메서드를 다음 코드 예제에서와 같이:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult;

    if (useSilent)
    {
        authenticationResult = await ADB2CClient.AcquireTokenSilentAsync(
            Constants.Scopes,
            GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
            Constants.Authority,
            false);
    }
    ...
}
```

`AcquireTokenSilentAsync` 메서드 사용자 로그인을 요구 하지 않고 캐시에서 사용자 토큰을 검색 하려고 시도 합니다. 이 처리 시나리오에 적합 한 토큰을 이미 있을 수 캐시에 이전 세션에서 합니다. 토큰을 가져오는 데 시도가 성공 하는 경우는 `TodoList` 페이지를 탐색 합니다. 토큰을 가져오는 데 시도가 실패할 경우 아무 일도 발생 하 고 사용자는 새 인증 워크플로 시작 하도록 선택 해야 합니다.

## <a name="signing-out"></a>로그 아웃

다음 코드 예제에서는 로그 아웃 프로세스는 호출 하는 방법을 보여 줍니다.

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

로컬 캐시에서 모든 인증 토큰을 지웁니다.

## <a name="summary"></a>요약

이 문서에는 Microsoft 인증 라이브러리 (MSAL) 및 Azure Active Directory B2C 소비자 id 관리를 모바일 응용 프로그램 통합을 사용 하는 방법을 보여 줍니다. Azure Active Directory B2C 연결 소비자 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리 솔루션입니다.


## <a name="related-links"></a>관련 링크

- [AzureADB2CAuth (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft 인증 라이브러리](https://www.nuget.org/packages/Microsoft.Identity.Client)
