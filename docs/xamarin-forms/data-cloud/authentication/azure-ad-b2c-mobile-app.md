---
title: Azure Mobile Apps와 Azure Active Directory B2C 통합
description: Azure Active Directory B2C는 소비자 지향 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리 솔루션입니다. 이 문서에서는 Azure Active Directory B2C를 사용 하 여 Xamarin.Forms를 사용 하 여 인증 및 Azure Mobile Apps 인스턴스에 대 한 권한 부여를 제공 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 135977329e2a190dd4c611937f6b8a664f135f5c
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53051946"
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Azure Mobile Apps와 Azure Active Directory B2C 통합

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)

_Azure Active Directory B2C는 소비자 지향 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리 솔루션입니다. 이 문서에서는 Azure Active Directory B2C를 사용 하 여 Xamarin.Forms를 사용 하 여 인증 및 Azure Mobile Apps 인스턴스에 대 한 권한 부여를 제공 하는 방법에 설명 합니다._

![](~/media/shared/preview.png "이 API는 현재 시험판")

> [!NOTE]
> 합니다 [Microsoft 인증 라이브러리](https://www.nuget.org/packages/Microsoft.Identity.Client) 는 아직 미리 보기, 이지만 프로덕션 환경에 적합 합니다. 그러나 있을 수 있습니다 수의 주요 변경 내용 API, 내부 캐시 형식 및 응용 프로그램에 영향을 줄 수 있는 라이브러리의 다른 메커니즘입니다.

## <a name="overview"></a>개요

Azure Mobile Apps를 사용 하면 모바일 인증, 오프 라인 동기화 및 푸시 알림 지원을 통해 Azure App Service에서 호스트 되는 확장 가능한 백 엔드를 사용 하 여 응용 프로그램을 개발할 수 있습니다. Azure Mobile Apps에 대 한 자세한 내용은 참조 하세요. [Azure 모바일 앱을 소비](~/xamarin-forms/data-cloud/consuming/azure.md), 및 [Azure Mobile Apps를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure.md)합니다.

Azure Active Directory B2C는 소비자가에 로그인 하 여 응용 프로그램을 허용 하는 소비자 지향 응용 프로그램에 대 한 id 관리 서비스:

- 기존 소셜 계정 (Microsoft, Google, Facebook, Amazon, LinkedIn)을 사용합니다.
- 새 자격 증명 (전자 메일 주소 및 암호 또는 사용자 이름 및 암호)를 만드는 중입니다. 이러한 자격 증명 이라고 *로컬* 계정.

Azure Active Directory B2C에 대 한 자세한 내용은 참조 하세요. [Azure Active Directory B2C를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)합니다.

Azure 모바일 앱에 대 한 인증 워크플로 관리 하려면 azure Active Directory B2C는 사용할 수 있습니다. 이 방법을 사용 하 여 id 관리 환경을 클라우드에서 완전히 정의 및 모바일 응용 프로그램 코드를 변경 하지 않고 수정할 수 있습니다.

사용할 수 있는 Azure Mobile Apps 인스턴스를 사용 하 여 Azure Active Directory B2C 테 넌 트를 통합 하는 경우 두 가지 인증 워크플로 가지가 있습니다.

- [클라이언트 관리](#client_managed) –이 Azure Active Directory B2C 테 넌 트를 사용 하 여 인증 프로세스가 Xamarin.Forms 모바일 응용 프로그램 시작에 접근 하 고 Azure Mobile Apps 인스턴스에 받은 인증 토큰을 전달 합니다.
- [서버 관리](#server_managed) -이 방법은 Azure Mobile Apps 인스턴스를 사용 하 여 Azure Active Directory B2C 테 넌 트 웹 기반 워크플로 통해 인증 프로세스를 시작 합니다.

두 경우 모두 Azure Active Directory B2C 테 넌 트에서 인증 환경이 제공 됩니다. 샘플 응용 프로그램에서이 인해 다음 스크린샷에서 표시 된 로그인 화면에서:

![](azure-ad-b2c-mobile-app-images/screenshots.png "로그인 페이지")

로그인 또는 로컬 계정을 소셜 id 공급자를 사용 하 여, 허용 됩니다. Microsoft, Google 및 Facebook은이 예제에서 소셜 id 공급자로 사용 되지만, 다른 id 공급자를 사용할 수도 있습니다.

## <a name="setup"></a>설정

인증 워크플로에 사용에 관계 없이 Azure Mobile Apps 인스턴스를 사용 하 여 Azure Active Directory B2C 테 넌 트를 통합 하기 위한 초기 프로세스는 다음과 같습니다.

1. Azure Mobile Apps 인스턴스를 만듭니다. 자세한 내용은 [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)합니다.
1. Azure Mobile Apps 인스턴스와 Xamarin.Forms 응용 프로그램에서 인증을 사용 하도록 설정 합니다. 자세한 내용은 [Azure Mobile Apps를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure.md)합니다.
1. Azure Active Directory B2C 테 넌 트를 만듭니다. 자세한 내용은 [Azure Active Directory B2C를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)합니다.

클라이언트 관리 인증 워크플로 사용 하는 경우 (MSAL (Microsoft 인증 라이브러리)를 해야 note 합니다. MSAL은 장치의 웹 브라우저를 사용 하 여 인증을 수행 합니다. 사용자 로그인 장치별로 응용 프로그램의 흐름 로그인 및 권한 부여의 전환율을 개선 한 후에 필요한 응용 프로그램의 유용성이 향상 됩니다. 또한 장치 브라우저 향상 된 보안을 제공합니다. 사용자 인증 프로세스 완료 된 후에 웹 브라우저 탭에서 컨트롤 응용 프로그램에 반환 됩니다. 사용자 지정 URL 구성표는 인증 프로세스 후를 검색 하 고 전송 된 후 사용자 지정 URL 처리에서 반환 되는 리디렉션 URL을 등록 하 여 수행 됩니다. MSAL을 사용 하 여 Azure Active Directory B2C 테 넌 트와 통신 하는 방법에 대 한 자세한 내용은 참조 하세요. [Azure Active Directory B2C를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)합니다.

<a name="client_managed" />

## <a name="client-managed-authentication"></a>클라이언트 관리 인증

클라이언트 관리 인증 Xamarin.Forms 모바일 응용 프로그램 인증 흐름을 시작 하는 Azure Active Directory B2C 테 넌 트를 연결 합니다. 면 성공적인 로그인-Azure Active Directory B2C 테 넌 트 한 다음 로그인 시 Azure Mobile Apps 인스턴스에 제공 되는 id 토큰을 반환 합니다. 이렇게 하면 Xamarin.Forms 응용 프로그램을에서 Azure Mobile Apps 인스턴스가 인증 된 사용자 권한이 필요한 작업을 수행할 수 있습니다.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Azure Active Directory B2C 테 넌 트 구성

클라이언트 관리 인증 워크플로에 대 한 Azure Active Directory B2C 테 넌 트를 다음과 같이 구성 해야 합니다.

- 네이티브 클라이언트를 포함 합니다.
- 뒤에 모바일 응용 프로그램을 고유 하 게 식별 하는 URL 구성표를 사용자 지정 리디렉션 URI로 `://auth/`합니다. 사용자 지정 URL 구성표를 선택 하는 방법에 대 한 자세한 내용은 참조 하세요. [네이티브 앱 리디렉션 URI 선택](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri)합니다.

다음 스크린샷은 이러한 구성을 보여 줍니다.

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Azure Active Directory B2C 구성을")](azure-ad-b2c-mobile-app-images/client-flow-config.png#lightbox "Azure Active Directory B2C 구성")

뒤에 동일한 사용자 지정 URL 구성표를 회신 URL 설정 되도록 테 넌 트도 구성 해야 Azure Active Directory B2C에서 사용 된 정책 `://auth/`합니다. 다음 스크린샷은 이러한 구성을 보여 줍니다.

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Azure Active Directory B2C 정책")

### <a name="azure-mobile-app-configuration"></a>Azure 모바일 앱 구성

클라이언트 관리 인증 워크플로에 대 한 Azure Mobile Apps 인스턴스를 다음과 같이 구성 해야 합니다.

- App Service 인증 켜야 합니다.
- 요청이 인증 되지 않으면 수행할 동작을 설정 해야 **Azure Active Directory로 로그인**합니다.

다음 스크린샷은 이러한 구성을 보여 줍니다.

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Azure 모바일 앱 구성")

Azure Mobile Apps 인스턴스를 Azure Active Directory B2C 테 넌 트를 사용 하 여 통신에 구성 되어야 합니다. 사용 하 여이 작업을 수행할 수 있습니다 **고급** Azure Active Directory 인증 공급자에 대 한 모드와는 **클라이언트 ID** 되는 **응용 프로그램 ID** azure Active Directory B2C 테 넌 트, 및 **발급자 Url** 되는 Azure Active Directory B2C 정책에 대 한 메타 데이터 끝점입니다. 다음 스크린샷은 이러한 구성을 보여 줍니다.

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Azure 모바일 앱 고급 구성")

### <a name="signing-in"></a>로그인

다음 코드 예제에서는 클라이언트 관리 인증 워크플로 시작 하는 방법을 보여 줍니다.

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);

    ...
    var payload = new JObject();
    payload["access_token"] = authenticationResult.IdToken;

    User = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        payload);
    ...
}
```

Microsoft 인증 라이브러리 (MSAL)는 Azure Active Directory B2C 테 넌 트를 사용 하 여 인증 하는 워크플로 시작 하는 데 사용 됩니다. 합니다 `AcquireTokenAsync` 장치의 웹 브라우저를 시작 하 고 참조를 통해 정책에 의해 지정 된 Azure Active Directory B2C 정책에 정의 된 인증 옵션을 표시 하는 메서드를 `Constants.Authority` 상수입니다. 이 정책은 로그인 및 등록 하는 동안 소비자가 경험한 환경 및 응용 프로그램 등록 또는 로그인 성공적으로 수신 됩니다 클레임을 정의 합니다.

결과 `AcquireTokenAsync` 메서드 호출을 `AuthenticationResult` 인스턴스. 인증이 성공 하는 경우는 `AuthenticationResult` 인스턴스에 로컬로 캐시 하는 id 토큰에 포함 됩니다. 인증이 성공한 경우의 `AuthenticationResult` 인스턴스 인증에 실패 한 이유를 나타내는 데이터를 포함 됩니다. MSAL을 사용 하 여 Azure Active Directory B2C 테 넌 트를 사용 하 여 통신 하는 방법에 대 한 자세한 내용은 [Azure Active Directory B2C를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)합니다.

경우는 `MobileServiceClient.LoginAsync` 메서드가 호출 되 면 Azure Mobile Apps 인스턴스에 수신에 래핑된 id 토큰을 `JObject`합니다. Azure Mobile Apps 인스턴스 자체 OAuth 2.0 인증 흐름을 시작 하지 않아도는 유효한 토큰 방법의 존재 합니다. 대신를 `MobileServiceClient.LoginAsync` 메서드가 반환 되는 `MobileServiceUser` 에 저장 되는 인스턴스는 `MobileServiceClient.CurrentUser` 속성. 이 속성은 제공 `UserId` 고 `MobileServiceAuthenticationToken` 속성입니다. 이러한 인증된 된 사용자 및 인증 토큰을 만료 될 때까지 사용할 수 있는 사용자를 나타냅니다. Xamarin.Forms 응용 프로그램에서 Azure Mobile Apps 인스턴스가 인증 된 사용자 권한이 필요한 작업을 수행할 수 있도록 Azure Mobile Apps 인스턴스에 대 한 모든 요청에서 인증 토큰이 포함 됩니다.

### <a name="signing-out"></a>로그 아웃

다음 코드 예제에서는 클라이언트에서 관리 하는 로그 아웃 프로세스를 호출 하는 방법을 보여 줍니다.

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();

    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

`MobileServiceClient.LogoutAsync` 메서드는 취소 Azure Mobile Apps 인스턴스를 사용 하 여 사용자 인증 및 모든 인증 토큰이 MSAL에 의해 만들어진 로컬 캐시에서 지워집니다.

<a name="server_managed" />

## <a name="server-managed-authentication"></a>서버 관리 인증

서버 관리 인증 Xamarin.Forms 응용 프로그램에는 Azure Active Directory B2C 테 넌 트를 사용 하 여 B2C 정책에 정의 된 대로 로그인 페이지를 표시 하 여 OAuth 2.0 인증 흐름을 관리 하는 Azure Mobile Apps 인스턴스를 연결 합니다. 성공적인 로그온을 다음 Azure Mobile Apps 인스턴스 Xamarin.Forms 응용 프로그램에서 Azure Mobile Apps 인스턴스가 인증 된 사용자 권한이 필요한 작업을 수행할 수 있도록 토큰을 반환 합니다.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Azure Active Directory B2C 테 넌 트 구성

서버 관리 인증 워크플로에 대 한 Azure Active Directory B2C 테 넌 트를 다음과 같이 구성 해야 합니다.

- 암시적 흐름 허용 및 웹 앱/웹 API 포함 됩니다.
- Azure 모바일 앱의 주소로 회신 URL을 설정 하 고 그 다음 `/.auth/login/aad/callback`합니다.

다음 스크린샷은 이러한 구성을 보여 줍니다.

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Azure Active Directory B2C 구성을")](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "Azure Active Directory B2C 구성")

회신 URL을 Azure 모바일 앱의 주소로 설정 되도록 테 넌 트도 구성 해야 Azure Active Directory B2C에서 사용 된 정책 뒤 `/.auth/login/aad/callback`합니다. 다음 스크린샷은 이러한 구성을 보여 줍니다.

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Azure Active Directory B2C 정책")

### <a name="azure-mobile-apps-instance-configuration"></a>Azure Mobile Apps 인스턴스 구성

서버 관리 인증 워크플로에 대 한 Azure Mobile Apps 인스턴스를 다음과 같이 구성 해야 합니다.

- App Service 인증 켜야 합니다.
- 요청이 인증 되지 않으면 수행할 동작을 설정 해야 **Azure Active Directory로 로그인**합니다.

다음 스크린샷은 이러한 구성을 보여 줍니다.

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Azure 모바일 앱 구성")

Azure Mobile Apps 인스턴스를 Azure Active Directory B2C 테 넌 트를 사용 하 여 통신에 구성 되어야 합니다. 사용 하 여이 작업을 수행할 수 있습니다 **고급** Azure Active Directory 인증 공급자에 대 한 모드와는 **클라이언트 ID** 되는 **응용 프로그램 ID** azure Active Directory B2C 테 넌 트, 및 **발급자 Url** 되는 Azure Active Directory B2C 정책에 대 한 메타 데이터 끝점입니다. 다음 스크린샷은 이러한 구성을 보여 줍니다.

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Azure 모바일 앱 고급 구성")

### <a name="signing-in"></a>로그인

다음 코드 예제에서는 서버 관리 인증 워크플로 시작 하는 방법을 보여 줍니다.

```csharp
public async Task<bool> AuthenticateAsync()
{
    ...
    MobileServiceUser user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        UIApplication.SharedApplication.KeyWindow.RootViewController,
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        Constants.URLScheme);
    ...
}
```

경우는 `MobileServiceClient.LoginAsync` 메서드가 호출 되 면 Azure Mobile Apps 인스턴스 OAuth 2.0 인증 흐름을 시작 하는 연결 된 Azure Active Directory B2C 정책을 실행 합니다. 각 `AuthenticateAsync` 메서드는 플랫폼 마다 고유 합니다. 그러나 각 `AuthenticateAsync` 메서드는 `MobileServiceClient.LoginAsync` 메서드 및 인증 프로세스에서 Azure Active Directory 테 넌 트를 사용할지를 지정 합니다. 자세한 내용은 [사용자가 로그인](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in)합니다.

`MobileServiceClient.LoginAsync` 메서드가 반환 되는 `MobileServiceUser` 에 저장 되는 인스턴스는 `MobileServiceClient.CurrentUser` 속성입니다. 이 속성은 제공 `UserId` 고 `MobileServiceAuthenticationToken` 속성입니다. 이러한 인증된 된 사용자 및 인증 토큰을 만료 될 때까지 사용할 수 있는 사용자를 나타냅니다. Xamarin.Forms 응용 프로그램에서 Azure Mobile Apps 인스턴스가 인증 된 사용자 권한이 필요한 작업을 수행할 수 있도록 Azure Mobile Apps 인스턴스에 대 한 모든 요청에서 인증 토큰이 포함 됩니다.

### <a name="signing-out"></a>로그 아웃

다음 코드 예제에서는 로그 아웃 프로세스 서버 관리를 호출 하는 방법을 보여 줍니다.

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
    ...
}
```

`MobileServiceClient.LogoutAsync` 메서드 취소 Azure Mobile Apps 인스턴스를 사용 하 여 사용자를 인증 합니다. 자세한 내용은 [사용자 아웃 로깅](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out)합니다.

## <a name="summary"></a>요약

이 문서에서는 Azure Active Directory B2C를 사용 하 여 Xamarin.Forms를 사용 하 여 인증 및 Azure Mobile Apps 인스턴스에 대 한 권한 부여를 제공 하는 방법을 보여 줍니다. Azure Active Directory B2C는 소비자 지향 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리 솔루션입니다.


## <a name="related-links"></a>관련 링크

- [TodoAzureAuth ServerFlow (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Mobile Apps 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure Active Directory B2C 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Microsoft 인증 라이브러리](https://www.nuget.org/packages/Microsoft.Identity.Client)
