---
title: Azure 모바일 앱에 Azure Active Directory B2C 통합
description: Azure Active Directory B2C 연결 소비자 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리 솔루션입니다. 이 문서에서는 Xamarin.Forms를 사용한 인증 및 Azure 모바일 앱 인스턴스에 대 한 권한 부여를 제공 하려면 Azure Active Directory B2C를 사용 하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: cafc1e78779dc393fa0409daa08b3daa8948a1ee
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Azure 모바일 앱에 Azure Active Directory B2C 통합

_Azure Active Directory B2C 연결 소비자 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리 솔루션입니다. 이 문서에서는 Xamarin.Forms를 사용한 인증 및 Azure 모바일 앱 인스턴스에 대 한 권한 부여를 제공 하려면 Azure Active Directory B2C를 사용 하는 방법을 보여줍니다._

![](~/media/shared/preview.png "이 API는 현재 시험판 버전")

> [!NOTE]
> [Microsoft 인증 라이브러리](https://www.nuget.org/packages/Microsoft.Identity.Client) 미리 보기 상태, 하지만 프로덕션 환경에서 사용 하기에 적합 합니다. 그러나 있을 수 있습니다 수의 주요 변경 내용 API, 내부 캐시 형식 및 응용 프로그램에 영향을 줄 수 있는 라이브러리의 다른 메커니즘입니다.

## <a name="overview"></a>개요

Azure 모바일 앱을 사용 하 여 확장 가능한 백 엔드 모바일 인증, 오프 라인 동기화 및 푸시 알림을 지 원하는 Azure 앱 서비스에서 호스트 된 응용 프로그램을 개발할 수 있습니다. Azure 모바일 앱에 대 한 자세한 내용은 참조 [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md), 및 [Azure 모바일 앱과 사용자가 인증](~/xamarin-forms/data-cloud/authentication/azure.md)합니다.

Azure Active Directory B2C은 소비자가에 로그인 하 여 응용 프로그램에 있는 소비자 용 응용 프로그램에 대 한 id 관리 서비스입니다.

- 기존 공유 계정 (Microsoft, Google, Facebook, Amazon, LinkedIn)를 사용합니다.
- 새 자격 증명 (전자 메일 주소 및 암호 또는 사용자 이름 및 암호)을 만듭니다. 이러한 자격 증명 라고 *로컬* 계정.

Azure Active Directory B2C에 대 한 자세한 내용은 참조 [Azure Active Directory B2C 있는 사용자를 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)합니다.

Azure Active Directory B2C Azure 모바일 앱에 대 한 인증 워크플로 관리 데 사용할 수 있습니다. 이 방법을 사용 id 관리 환경을 클라우드에 완전히 정의 하 고 모바일 응용 프로그램 코드를 변경 하지 않고 수정할 수 있습니다.

Azure Active Directory B2C 테 넌 트 Azure 모바일 앱 인스턴스와 통합할 때 사용할 수 있는 인증 워크플로가 두 가지가 있습니다.

- [클라이언트에서 관리 하는](#client_managed) –이 Azure Active Directory B2C 테 넌 트의 인증 프로세스가 Xamarin.Forms 모바일 응용 프로그램 시작에 접근 하 고 Azure 모바일 앱 인스턴스를 받은 인증 토큰을 전달 합니다.
- [서버 관리](#server_managed) -이 방법은 Azure 모바일 앱의 인스턴스를 사용 하 여 Azure Active Directory B2C 테 넌 트 웹 기반 워크플로 통해 인증 프로세스를 시작 합니다.

두 경우 모두에서 인증 환경은 Azure Active Directory B2C 테 넌 트 별로 제공 됩니다. 예제 응용 프로그램에서는이 인해 다음 스크린샷에 표시 된 로그인 화면:

![](azure-ad-b2c-mobile-app-images/screenshots.png "로그인 페이지")

로그인 소셜 id 공급자와 또는 로컬 계정을 사용 하 여 일반적으로 허용 됩니다. Microsoft, Google, 및 Facebook이 예에서 소셜 id 공급자로 사용 되지만, 기타 id 공급자 사용할 수도 있습니다.

## <a name="setup"></a>설정

사용 되는 인증 워크플로 관계 없이 Azure 모바일 앱 인스턴스와 Azure Active Directory B2C 테 넌 트를 통합 하기 위한 초기 프로세스는 다음과 같습니다.

1. Azure 모바일 앱 인스턴스를 만듭니다. 자세한 내용은 참조 [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)합니다.
1. Azure 모바일 앱 인스턴스 및 Xamarin.Forms 응용 프로그램에서 인증을 활성화 합니다. 자세한 내용은 참조 [Azure 모바일 앱과 사용자가 인증](~/xamarin-forms/data-cloud/authentication/azure.md)합니다.
1. Azure Active Directory B2C 테 넌 트를 만듭니다. 자세한 내용은 참조 [Azure Active Directory B2C 있는 사용자를 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)합니다.

Note 인증 클라이언트에서 관리 하는 워크플로 사용 하는 경우 Microsoft 인증 라이브러리 (MSAL)가 필요 합니다. MSAL 인증을 수행 하는 장치의 웹 브라우저를 사용 합니다. 사용자만 로그인 할 장치, 응용 프로그램에서 흐름의 로그인 및 권한 부여 변환율 향상 되 면 필요에 따라 응용 프로그램의 유용성이 향상 됩니다. 또한 장치 브라우저 보안 향상된을 제공합니다. 인증 프로세스를 완료 하는 사용자, 웹 브라우저 탭에서 응용 프로그램에 제어가 돌아옵니다. 이 사용자 지정 URL 체계는 인증 프로세스 후를 검색 하 고 전송 된 후 사용자 지정 URL을 처리에서 반환 되는 리디렉션 URL에 대 한 등록 하 여 달성 됩니다. MSAL를 사용 하 여 Azure Active Directory B2C 테 넌 트와 통신 하는 방법에 대 한 자세한 내용은 참조 [Azure Active Directory B2C 있는 사용자를 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)합니다.

<a name="client_managed" />

## <a name="client-managed-authentication"></a>클라이언트에서 관리 하는 인증

인증 클라이언트에서 관리 하는 Xamarin.Forms 모바일 응용 프로그램 인증 흐름을 시작 하려면 Azure Active Directory B2C 테 넌 트를 연결 합니다. 로그온에 성공 Azure Active Directory B2C 후 테 넌 트 다음에 로그인 시 Azure 모바일 앱 인스턴스를 제공 하는 id 토큰을 반환 합니다. 따라서 Xamarin.Forms 응용 프로그램을에서 인증 된 사용자 권한이 필요로 하는 Azure 모바일 앱 인스턴스 작업을 수행할 수 있습니다.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Azure Active Directory B2C 테 넌 트 구성

클라이언트에서 관리 하는 인증 워크플로에 대 한 Azure Active Directory B2C 테 넌 트를 다음과 같이 구성 합니다.

- 네이티브 클라이언트를 포함 합니다.
- 다음 모바일 응용 프로그램을 고유 하 게 식별 하는 URL 체계에 사용자 지정 리디렉션 URI 설정 `://auth/`합니다. 사용자 지정 URL 구성표를 선택 하는 방법에 대 한 자세한 내용은 참조 [네이티브 응용 프로그램 리디렉션 URI 선택](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri)합니다.

다음 스크린샷은이 구성을 보여 줍니다.

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Azure Active Directory B2C 구성")](azure-ad-b2c-mobile-app-images/client-flow-config.png#lightbox "Azure Active Directory B2C 구성")

테 넌 트에 동일한 사용자 지정 URL 체계 회신 URL 설정 되도록도 구성 해야 Azure Active Directory B2C에 사용 되는 정책 이어서 `://auth/`합니다. 다음 스크린샷은이 구성을 보여 줍니다.

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Azure Active Directory B2C 정책")

### <a name="azure-mobile-app-configuration"></a>Azure 모바일 앱 구성

클라이언트에서 관리 하는 인증 워크플로에 대 한 Azure 모바일 앱 인스턴스를 다음과 같이 구성 합니다.

- 앱 서비스 인증을 설정 해야 합니다.
- 요청이 인증 되지 않은 경우 수행할 동작을 설정 해야 **Azure Active Directory로 로그인**합니다.

다음 스크린샷은이 구성을 보여 줍니다.

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Azure 모바일 앱 구성")

Azure 모바일 앱 인스턴스도 Azure Active Directory B2C 테 넌 트와 통신 하도록 구성 해야 합니다. 이 사용 하 여 수행할 수 있습니다 **고급** Azure Active Directory 인증 공급자에 대 한 모드와는 **클라이언트 ID** 되는 **응용 프로그램 ID** azure Active Directory B2C 테 넌 트, 및 **발급자 Url** Azure Active Directory B2C 정책에 대 한 메타 데이터 끝점 되 고 있습니다. 다음 스크린샷은이 구성을 보여 줍니다.

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "고급 구성 하는 azure 모바일 앱")

### <a name="signing-in"></a>로그인

다음 코드 예제에서는 인증 클라이언트에서 관리 하는 워크플로 시작 하는 방법을 보여 줍니다.

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

Microsoft 인증 라이브러리 (MSAL)와 Azure Active Directory B2C 테 넌 트 인증 워크플로 시작 하는 데 사용 됩니다. `AcquireTokenAsync` 장치의 웹 브라우저를 시작 하 고 정책을 통해 참조에 의해 지정 된 Azure Active Directory B2C 정책에 정의 된 인증 옵션을 표시 하는 메서드는 `Constants.Authority` 상수입니다. 이 정책은 등록 하는 동안 및 로그인을 소비자에 게 전송 되는 환경 및 클레임을 응용 프로그램 등록 또는 로그인에 성공적으로 받게 됩니다 정의 합니다.

결과 `AcquireTokenAsync` 메서드 호출은 `AuthenticationResult` 인스턴스. 인증이 성공 하는 경우는 `AuthenticationResult` 인스턴스 id 토큰을 로컬로 캐시에 포함 됩니다. 인증이 성공 하는 경우는 `AuthenticationResult` 인스턴스 인증에 실패 한 이유를 나타내는 데이터를 포함 됩니다. Azure Active Directory B2C 테 넌 트와 통신 하려면 MSAL를 사용 하는 방법에 대 한 정보를 참조 하십시오. [Azure Active Directory B2C 있는 사용자를 인증](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)합니다.

경우는 `MobileServiceClient.LoginAsync` 메서드가 호출 되 면 Azure 모바일 앱 인스턴스 수신 id 토큰에 래핑되는 `JObject`합니다. Azure 모바일 앱 인스턴스는 자체 OAuth 2.0 인증 흐름을 시작 하려면 필요 없는 유효한 토큰 의미의 존재 합니다. 대신,는 `MobileServiceClient.LoginAsync` 메서드가 반환 되는 `MobileServiceUser` 에 저장 되는 인스턴스에 `MobileServiceClient.CurrentUser` 속성입니다. 이 속성은 제공 `UserId` 및 `MobileServiceAuthenticationToken` 속성입니다. 이러한 인증된 된 사용자 및 만료 될 때까지 사용할 수 있는 사용자에 대 한 인증 토큰을 나타냅니다. 인증 토큰 Xamarin.Forms 응용 프로그램에서 인증 된 사용자 권한이 있어야 하는 Azure 모바일 앱 인스턴스 작업을 수행할 수 있도록 하는 Azure 모바일 앱 인스턴스에 대 한 모든 요청에 포함 됩니다.

### <a name="signing-out"></a>로그 아웃

다음 코드 예제에서는 클라이언트에서 관리 하는 로그 아웃 프로세스는 호출 하는 방법을 보여 줍니다.

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

`MobileServiceClient.LogoutAsync` 메서드 취소 Azure 모바일 앱 인스턴스를 사용 하 여 사용자를 인증 하 고 모든 인증 토큰이 MSAL에서 만든 로컬 캐시에서 지워집니다.

<a name="server_managed" />

## <a name="server-managed-authentication"></a>인증 서버 관리

서버 관리 인증 Xamarin.Forms 응용 프로그램 B2C 정책에 정의 된 대로 로그인 페이지를 표시 하 여 OAuth 2.0 인증 흐름을 관리 하는 Azure Active Directory B2C 테 넌 트를 사용 하 여 Azure 모바일 앱 인스턴스를 연결 합니다. 성공적인 로그온을 따라 Azure 모바일 앱 인스턴스 Xamarin.Forms 응용 프로그램에서 인증 된 사용자 권한이 있어야 하는 Azure 모바일 앱 인스턴스 작업을 수행할 수 있는 토큰을 반환 합니다.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Azure Active Directory B2C 테 넌 트 구성

서버 관리 되는 인증 워크플로에 대 한 Azure Active Directory B2C 테 넌 트를 다음과 같이 구성 합니다.

- 암시적 흐름을 허용 및 웹 앱/웹 API에 포함 됩니다.
- Azure 모바일 앱의 주소로 회신 URL을 설정 하며, 그 `/.auth/login/aad/callback`합니다.

다음 스크린샷은이 구성을 보여 줍니다.

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Azure Active Directory B2C 구성")](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "Azure Active Directory B2C 구성")

Azure 모바일 앱의 주소로 회신 URL 설정 되도록 테 넌 트도 구성 해야 Azure Active Directory B2C에 사용 되는 정책 이어서 `/.auth/login/aad/callback`합니다. 다음 스크린샷은이 구성을 보여 줍니다.

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Azure Active Directory B2C 정책")

### <a name="azure-mobile-apps-instance-configuration"></a>Azure 모바일 앱 인스턴스 구성

서버 관리 되는 인증 워크플로에 대 한 Azure 모바일 앱 인스턴스를 다음과 같이 구성 합니다.

- 앱 서비스 인증을 설정 해야 합니다.
- 요청이 인증 되지 않은 경우 수행할 동작을 설정 해야 **Azure Active Directory로 로그인**합니다.

다음 스크린샷은이 구성을 보여 줍니다.

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Azure 모바일 앱 구성")

Azure 모바일 앱 인스턴스도 Azure Active Directory B2C 테 넌 트와 통신 하도록 구성 해야 합니다. 이 사용 하 여 수행할 수 있습니다 **고급** Azure Active Directory 인증 공급자에 대 한 모드와는 **클라이언트 ID** 되는 **응용 프로그램 ID** azure Active Directory B2C 테 넌 트, 및 **발급자 Url** Azure Active Directory B2C 정책에 대 한 메타 데이터 끝점 되 고 있습니다. 다음 스크린샷은이 구성을 보여 줍니다.

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "고급 구성 하는 azure 모바일 앱")

### <a name="signing-in"></a>로그인

다음 코드 예제에서는 인증 서버 관리 워크플로 시작 하는 방법을 보여 줍니다.

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

경우는 `MobileServiceClient.LoginAsync` 메서드가 호출 되 면 Azure 모바일 앱 인스턴스 OAuth 2.0 인증 흐름을 시작 하 여 연결 된 Azure Active Directory B2C 정책을 실행 합니다. 각 `AuthenticateAsync` 메서드는 플랫폼 마다 고유 합니다. 그러나 각 `AuthenticateAsync` 메서드는 `MobileServiceClient.LoginAsync` 메서드는 인증 프로세스에서 Azure Active Directory 테 넌 트 사용될지를 지정 합니다. 자세한 내용은 참조 [사용자 로그인](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in)합니다.

`MobileServiceClient.LoginAsync` 메서드가 반환 되는 `MobileServiceUser` 에 저장 되는 인스턴스에 `MobileServiceClient.CurrentUser` 속성입니다. 이 속성은 제공 `UserId` 및 `MobileServiceAuthenticationToken` 속성입니다. 이러한 인증된 된 사용자 및 만료 될 때까지 사용할 수 있는 사용자에 대 한 인증 토큰을 나타냅니다. 인증 토큰 Xamarin.Forms 응용 프로그램에서 인증 된 사용자 권한이 있어야 하는 Azure 모바일 앱 인스턴스 작업을 수행할 수 있도록 하는 Azure 모바일 앱 인스턴스에 대 한 모든 요청에 포함 됩니다.

### <a name="signing-out"></a>로그 아웃

다음 코드 예제에서는 서버 관리 되는 로그 아웃 프로세스는 호출 하는 방법을 보여 줍니다.

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
    ...
}
```

`MobileServiceClient.LogoutAsync` 메서드 취소 Azure 모바일 앱 인스턴스를 사용 하 여 사용자를 인증 합니다. 자세한 내용은 참조 [사용자 로그 아웃](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out)합니다.

## <a name="summary"></a>요약

이 문서에 Xamarin.Forms를 사용한 인증 및 Azure 모바일 앱 인스턴스에 대 한 권한 부여를 제공 하려면 Azure Active Directory B2C를 사용 하는 방법을 보여 줍니다. Azure Active Directory B2C 연결 소비자 웹 및 모바일 응용 프로그램에 대 한 클라우드 id 관리 솔루션입니다.


## <a name="related-links"></a>관련 링크

- [TodoAzureAuth ServerFlow (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure 모바일 앱을 사용한 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure Active Directory B2C 있는 사용자를 인증합니다.](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Microsoft 인증 라이브러리](https://www.nuget.org/packages/Microsoft.Identity.Client)
