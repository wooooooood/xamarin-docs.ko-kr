---
title: Azure Mobile Apps와 Azure Active Directory B2C 통합
description: Azure Active Directory B2C는 소비자 지향 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리 솔루션입니다. 이 문서에서는 Azure Active Directory B2C를 사용 하 여 Xamarin.Forms를 사용 하 여 인증 및 Azure Mobile Apps 인스턴스에 대 한 권한 부여를 제공 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2019
ms.openlocfilehash: 2f9cfceee4765dea946dfdac974ac2d56595ef94
ms.sourcegitcommit: 9aaae4f0af096cd136b3dcfbb9af591ba307dc25
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67398691"
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Azure Mobile Apps와 Azure Active Directory B2C 통합

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)

_Azure Active Directory B2C는 소비자 지향 응용 프로그램에 대 한 클라우드 id 관리를 제공합니다. 이 문서에서는 Xamarin.Forms 사용 하 여 Azure Mobile Apps 인스턴스로 id 관리를 통합 하려면 Azure Active Directory B2C를 사용 하는 방법에 설명 합니다._

## <a name="overview"></a>개요

Azure Mobile Apps를 사용 하면 Azure App Service에서 호스팅되는 확장 가능한 백 엔드를 사용 하 여 응용 프로그램을 개발할 수 있습니다. 모바일 인증, 오프 라인 동기화 및 푸시 지원 알림. Azure Mobile Apps에 대 한 자세한 내용은 참조 하세요. [Azure 모바일 앱을 소비](~/xamarin-forms/data-cloud/consuming/azure.md), 및 [Azure Mobile Apps를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure.md)합니다.

Azure Active Directory B2C는 소비자를 허용 하는 소비자 지향 응용 프로그램에 대 한 id 관리 서비스:

- 기존 소셜 계정 (Microsoft, Google, Facebook, Amazon, LinkedIn)으로 로그인 합니다.
- 새 자격 증명 (전자 메일 주소 및 암호 또는 사용자 이름 및 암호)을 만듭니다. 이러한 자격 증명 이라고 *로컬* 계정.

Azure Active Directory B2C에 대 한 자세한 내용은 참조 하세요. [Azure Active Directory B2C를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)합니다.

Azure 모바일 앱에 대 한 인증 워크플로 관리 하려면 azure Active Directory B2C는 사용할 수 있습니다. 이 방법은 클라우드의 id 관리를 구성 하 고 변경 내용을 응용 프로그램 코드를 수정 하지 않고 있습니다.


Azure Active Directory B2C를 사용 하 여 azure Mobile Apps에는 두 가지 인증 방법을 허용합니다.

- [클라이언트 관리](#client-managed-authentication) – Xamarin.Forms 모바일 응용 프로그램 Azure Active Directory B2C 테 넌 트를 사용 하 여 인증 프로세스를 시작 하 고 Azure Mobile Apps 인스턴스에 받은 인증 토큰을 전달 합니다.
- [서버 관리](#server-managed-authentication) – Azure Mobile Apps 인스턴스 사용 하 여 Azure Active Directory B2C 테 넌 트 웹 기반 워크플로 통해 인증 프로세스를 시작 합니다.

두 경우 모두 Azure Active Directory B2C 테 넌 트에서 인증 환경이 제공 됩니다. 샘플 응용 프로그램에서 다음 스크린샷과 같은 로그인 화면이 표시 됩니다.

![](azure-ad-b2c-mobile-app-images/screenshots.png "로그인 페이지")

이 예제에서는 로컬 계정으로 로그인을 보여주지만, Microsoft, Google, Facebook 및 기타 id 공급자를 사용할 수도 있습니다.

## <a name="setup"></a>설정

모두 워크플로에서 Azure Mobile Apps를 사용 하 여 Azure Active Directory B2C 테 넌 트를 통합 하기 위한 초기 프로세스는 다음과 같습니다.

1. Azure portal에서 Azure Mobile Apps 인스턴스를 만듭니다. 이 샘플 프로젝트와 유사한 시작 프로젝트를 생성합니다. 자세한 내용은 [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)합니다.
1. Azure Active Directory B2C 테 넌 트를 만듭니다. 자세한 내용은 [Azure Active Directory B2C를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)합니다.
1. Azure Mobile Apps 인스턴스와 Xamarin.Forms 응용 프로그램에서 인증을 사용 하도록 설정 합니다. 자세한 내용은 [Azure Mobile Apps를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure.md)합니다.

> [!NOTE]
> Microsoft 인증 라이브러리 (MSAL) 클라이언트 관리 인증 워크플로 사용 하는 경우 필요 합니다. MSAL은 장치의 웹 브라우저를 사용 하 여 인증 합니다. 브라우저에서 인증 완료 된 후 사용자는 사용자 지정 URL 구성표가 리디렉션됩니다. 응용 프로그램에는 다시 사용자 환경 제어 하 고 인증에 응답할 수 있도록 사용자 지정 URL 구성표를 등록 합니다. MSAL을 사용 하 여 Azure Active Directory B2C 테 넌 트와 통신 하는 방법에 대 한 자세한 내용은 참조 하세요. [Azure Active Directory B2C를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)합니다.

## <a name="client-managed-authentication"></a>클라이언트 관리 인증

클라이언트 관리 인증에서는 Xamarin.Forms 응용 프로그램 인증 흐름을 시작 하는 Azure Active Directory B2C 테 넌 트에 연결 합니다. Sign-on 후 Azure Active Directory B2C 테 넌 트에 Azure Mobile Apps 인스턴스에 제공 되는 id 토큰을 반환 합니다. 이렇게 하면 Xamarin.Forms 응용 프로그램을을 인증 된 사용자 권한이 필요한 작업을 수행할 수 있습니다.

### <a name="azure-active-directory-b2c-client-managed-tenant-configuration"></a>Azure Active Directory B2C 테 넌 트 클라이언트 관리 구성

클라이언트 관리 인증 워크플로에 대 한 Azure Active Directory B2C 테 넌 트를 다음과 같이 구성 해야 합니다.

- 설정할 **네이티브 클라이언트 포함** "예"로 합니다.
- 사용자 지정 리디렉션 URI를 설정 합니다. 앱 ID를 결합 하 고 뒤에 "msal"를 사용 하 여 MSAL 설명서 권장 ":/ / 인증 /"입니다. 자세한 내용은 [MSAL 클라이언트 응용 프로그램 리디렉션 URI](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Client-Applications#redirect-uri)합니다.

다음 스크린샷은 이러한 구성을 보여 줍니다.

[!["Azure Active Directory B2C 구성"](azure-ad-b2c-mobile-app-images/azure-redirect-uri-cropped.png)](azure-ad-b2c-mobile-app-images/azure-redirect-uri.png#lightbox "Azure Active Directory B2C 구성")

Azure Active Directory B2C 테 넌 트에서 사용 되는 로그인 정책은 같은 사용자 지정 URL 구성표를 회신 URL 설정 되도록 구성 되어야 합니다. 선택 하 여 수행할 수 있습니다 **사용자 흐름을 실행** 정책 설정에 액세스 하려면 Azure portal에서 합니다. 모바일 앱 구성 해야 하는 대로이 화면에 있는 메타 데이터 URL을 저장 합니다. 다음 스크린샷은이 구성 및 복사 해야 하는 URL을 보여 줍니다.

[!["Azure Active Directory B2C 정책"](azure-ad-b2c-mobile-app-images/client-flow-policies-cropped.png)](azure-ad-b2c-mobile-app-images/client-flow-policies.png#lightbox "Azure Active Directory B2C 정책 구성")

### <a name="azure-mobile-app-configuration"></a>Azure 모바일 앱 구성

클라이언트 관리 인증 워크플로에 대 한 Azure Mobile Apps 인스턴스를 다음과 같이 구성 합니다.

- App Service 인증 켜야 합니다.
- 요청이 인증 되지 않은 경우 수행할 동작을 설정 해야 **Azure Active Directory로 로그인**합니다.

다음 스크린샷은 이러한 구성을 보여 줍니다.

[!["Azure Mobile Apps 인증 구성"](azure-ad-b2c-mobile-app-images/ama-config-cropped.png)](azure-ad-b2c-mobile-app-images/ama-config.png#lightbox "Azure Mobile Apps 인증 구성")

Azure Active Directory B2C 테 넌 트를 사용 하 여 통신 하도록 Azure Mobile Apps 인스턴스를 구성 합니다.

- Azure Active Directory 구성을 클릭 하 고 사용 하도록 설정 **고급** Azure Active Directory 인증 공급자에 대 한 모드입니다.
- 설정할 **클라이언트 ID** 에 **응용 프로그램 ID** Azure Active Directory B2C 테 넌 트입니다.
- 설정 된 **발급자 Url** 테 넌 트 구성 하는 동안 이전에 복사한 정책의 메타 데이터 url입니다.

다음 스크린샷은 이러한 구성을 보여 줍니다.

![Azure Mobile Apps "고급" 구성](azure-ad-b2c-mobile-app-images/ama-advanced-config.png)

### <a name="signing-in"></a>로그인

다음 코드 예제에서는 클라이언트 관리 인증 워크플로 시작 하기 위해 키 메서드 호출을 보여 줍니다.

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...

    authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        string.Empty,
        UIBehavior.SelectAccount,
        string.Empty,
        App.UiParent);

    ...

    var payload = new JObject();
    if (authenticationResult != null && !string.IsNullOrWhiteSpace(authenticationResult.AccessToken))
    {
        payload["access_token"] = authenticationResult.AccessToken;
    }

    User = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        payload);
    success = true;

    ...
}
```

Microsoft 인증 라이브러리 (MSAL)는 Azure Active Directory B2C 테 넌 트를 사용 하 여 인증 하는 워크플로 시작 하는 데 사용 됩니다. 합니다 `AcquireTokenAsync` 장치의 웹 브라우저를 시작 하 고 참조를 통해 정책에 의해 지정 된 Azure Active Directory B2C 정책에 정의 된 인증 옵션을 표시 하는 메서드를 `Constants.AuthoritySignin` 상수입니다. 이 정책은 사용자의 로그인 및 등록 환경 및 응용 프로그램 인증이 성공 하면 수신 클레임을 정의 합니다.

결과 `AcquireTokenAsync` 메서드 호출을 `AuthenticationResult` 인스턴스. 인증이 성공 하는 경우는 `AuthenticationResult` 인스턴스에 로컬로 캐시 하는 액세스 토큰을 포함 됩니다. 인증이 성공한 경우의 `AuthenticationResult` 인스턴스 인증에 실패 한 이유를 나타내는 데이터를 포함 됩니다. MSAL을 사용 하 여 Azure Active Directory B2C 테 넌 트를 사용 하 여 통신 하는 방법에 대 한 자세한 내용은 [Azure Active Directory B2C를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)합니다.

경우는 `CurrentClient.LoginAsync` 메서드가 호출 되 면 Azure Mobile Apps 인스턴스 래핑됩니다 액세스 토큰을 받습니다는 `JObject`합니다. Azure Mobile Apps 인스턴스 자체 OAuth 2.0 인증 흐름을 시작 하지 않아도는 유효한 토큰 방법의 존재 합니다. 대신를 `CurrentClient.LoginAsync` 메서드가 반환 되는 `MobileServiceUser` 에 저장 되는 인스턴스는 `MobileServiceClient.CurrentUser` 속성. 이 속성은 제공 `UserId` 고 `MobileServiceAuthenticationToken` 속성입니다. 이러한 인증 된 사용자 및 만료 될 때까지 사용할 수 있는 토큰을 나타냅니다. Xamarin.Forms 응용 프로그램을 인증 된 사용자 권한이 필요한 작업을 수행할 수 있도록 Azure Mobile Apps 인스턴스에 대 한 모든 요청에서 인증 토큰이 포함 됩니다.

### <a name="signing-out"></a>로그 아웃

다음 코드 예제에서는 클라이언트에서 관리 하는 로그 아웃 프로세스를 호출 하는 방법을 보여 줍니다.

```csharp
public async Task<bool> LogoutAsync()
{
    ...

    IEnumerable<IAccount> accounts = await ADB2CClient.GetAccountsAsync();
    while (accounts.Any())
    {
        await ADB2CClient.RemoveAsync(accounts.First());
        accounts = await ADB2CClient.GetAccountsAsync();
    }
    User = null;

    ...
}
```

`CurrentClient.LogoutAsync` 메서드는 취소 Azure Mobile Apps 인스턴스를 사용 하 여 사용자 인증 및 모든 인증 토큰이 MSAL에 의해 만들어진 로컬 캐시에서 지워집니다.

## <a name="server-managed-authentication"></a>서버 관리 인증

서버 관리 인증 Xamarin.Forms 응용 프로그램에는 Azure Active Directory B2C 테 넌 트를 사용 하 여 B2C 정책에 정의 된 대로 로그인 페이지를 표시 하 여 OAuth 2.0 인증 흐름을 관리 하는 Azure Mobile Apps 인스턴스를 연결 합니다. 성공적인 로그온을 다음 Azure Mobile Apps 인스턴스 Xamarin.Forms 응용 프로그램을 인증 된 사용자 권한이 필요한 작업을 수행할 수 있도록 토큰을 반환 합니다.

### <a name="azure-active-directory-b2c-server-managed-tenant-configuration"></a>Azure Active Directory B2C 테 넌 트 서버 관리 구성

서버 관리 인증 워크플로에 대 한 Azure Active Directory B2C 테 넌 트를 다음과 같이 구성 해야 합니다.

- 암시적 흐름 허용 및 웹 앱/웹 API 포함 됩니다.
- Azure 모바일 앱의 주소로 회신 URL을 설정 하 고 그 다음 `/.auth/login/aad/callback`합니다.

다음 스크린샷은 이러한 구성을 보여 줍니다.

[!["Azure Active Directory B2C 구성"](azure-ad-b2c-mobile-app-images/server-flow-config-cropped.png)](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "Azure Active Directory B2C 구성")

Azure Active Directory B2C 테 넌 트에 사용 된 정책 구성 된 회신 URL을가지고 있어야 합니다. Azure 모바일 앱, 뒤의 주소로 회신 URL을 설정 하 여 이렇게 `/.auth/login/aad/callback`합니다. 또한 모바일 앱 구성 해야 하는 대로이 화면 위쪽에 있는 메타 데이터 URL을 저장할 해야 있습니다. 다음 스크린샷은이 구성 및 저장 해야 하는 메타 데이터 URL을 보여 줍니다.

!["Azure Active Directory B2C 정책 구성"](azure-ad-b2c-mobile-app-images/server-flow-policies.png)

### <a name="azure-mobile-apps-instance-configuration"></a>Azure Mobile Apps 인스턴스 구성

서버 관리 인증 워크플로에 대 한 Azure Mobile Apps 인스턴스를 다음과 같이 구성 해야 합니다.

- App Service 인증 켜야 합니다.
- 요청이 인증 되지 않은 경우 수행할 동작을 설정 해야 **Azure Active Directory로 로그인**합니다.

다음 스크린샷은 이러한 구성을 보여 줍니다.

[!["Azure Mobile Apps 인증 구성"](azure-ad-b2c-mobile-app-images/ama-config-cropped.png)](azure-ad-b2c-mobile-app-images/ama-config.png#lightbox "Azure Mobile Apps 인증 구성")

Azure Mobile Apps 인스턴스를 Azure Active Directory B2C 테 넌 트를 사용 하 여 통신도 구성 해야 합니다.

- Azure Active Directory 구성을 클릭 하 고 사용 하도록 설정 **고급** Azure Active Directory 인증 공급자에 대 한 모드입니다.
- 설정할 **클라이언트 ID** 에 **응용 프로그램 ID** Azure Active Directory B2C 테 넌 트입니다.
- 합니다 **발급자 Url** 테 넌 트 구성 하는 동안 이전에 복사한 정책의 메타 데이터 URL입니다.

다음 스크린샷은 이러한 구성을 보여 줍니다.

![Azure Mobile Apps "고급" 구성](azure-ad-b2c-mobile-app-images/ama-advanced-config.png)

### <a name="signing-in"></a>로그인

다음 코드 예제에는 서버 관리 인증 워크플로 시작 하는 방법을 보여 줍니다.

```csharp
public async Task<bool> AuthenticateAsync()
{
    ...

    MobileServiceUser user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        Constants.URLScheme);

    ...
}
```

경우는 `CurrentClient.LoginAsync` 메서드가 호출 되 면 Azure Mobile Apps 인스턴스 OAuth 2.0 인증 흐름을 시작 하는 연결 된 Azure Active Directory B2C 정책을 실행 합니다. 각 `AuthenticateAsync` 메서드는 플랫폼 마다 고유 합니다. 그러나 각 `AuthenticateAsync` 메서드 호출을 `CurrentClient.LoginAsync` 메서드 및 인증 프로세스에서 Azure Active Directory 테 넌 트를 사용할지를 지정 합니다. 자세한 내용은 [사용자가 로그인](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in)합니다.

`CurrentClient.LoginAsync` 메서드가 반환 되는 `MobileServiceUser` 에 저장 되는 인스턴스는 `CurrentClient.CurrentUser` 속성입니다. 이 속성은 제공 `UserId` 고 `MobileServiceAuthenticationToken` 속성입니다. 이러한 인증된 된 사용자 및 인증 토큰을 만료 될 때까지 사용할 수 있는 사용자를 나타냅니다. Xamarin.Forms 응용 프로그램에서 Azure Mobile Apps 인스턴스가 인증 된 사용자 권한이 필요한 작업을 수행할 수 있도록 Azure Mobile Apps 인스턴스에 대 한 모든 요청에서 인증 토큰이 포함 됩니다.

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


## <a name="related-links"></a>관련 링크

- [TodoAzureAuth ClientFlow (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [TodoAzureAuth ServerFlow (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Mobile Apps 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure Active Directory B2C 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Microsoft 인증 라이브러리 nuget 패키지](https://www.nuget.org/packages/Microsoft.Identity.Client)
- [Microsoft 인증 라이브러리 문서](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)
