---
title: 'Xamarin.Essentials: 보안 저장소'
description: 이 문서는 간단한 키/값 쌍을 안전 하 게 저장 하는 데 도움이 됩니다 Xamarin.Essentials SecureStorage 클래스를 설명 합니다. 클래스, 플랫폼별 구현 및 제한 사항을 사용 하는 방법을 설명 합니다.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: 2dfdb7051b269e73c68290a557849b9ae606c165
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353297"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials: 보안 저장소

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **SecureStorage** 클래스에 안전 하 게 간단한 키/값 쌍을 저장 하는 데 도움이 됩니다.

## <a name="getting-started"></a>시작

액세스 하는 **SecureStorage** , 다음 플랫폼별 설정을 기능은 필요:

# <a name="androidtabandroid"></a>[Android](#tab/android)

추가 설정이 필요 없습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

IOS 시뮬레이터를 개발 하는 경우 사용 하도록 설정 합니다 **키 집합** 자격 응용 프로그램의 번들 식별자에 대 한 키 집합 액세스 그룹을 추가 합니다.

열기는 **Entitlements.plist** 찾을 및 iOS 프로젝트를 **Keychain** 자격 사용 하도록 설정 하 고 합니다. 응용 프로그램의 식별자를 그룹으로 자동으로 추가 됩니다.

프로젝트 속성에서 아래 **iOS 번들 서명** 설정 된 **사용자 지정 자격** 에 **Entitlements.plist**.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

추가 설정이 필요 없습니다.

-----

## <a name="using-secure-storage"></a>보안 저장소를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

에 대 한 값을 저장 하는 주어진 _키_ 보안 저장소에서:

```csharp
try
{
  await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

보안 저장소에서 값을 검색 합니다.

```csharp
try
{
  var oauthToken = await SecureStorage.GetAsync("oauth_token");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

> [!NOTE]
> 요청된 된 키와 연결 된 값이 없을 경우 `GetAsync` 돌아갑니다 `null`합니다.

특정 키를 제거 하려면 다음을 호출 합니다.

```csharp
SecureStorage.Remove("oauth_token");
```

모든 키를 제거 하려면 다음을 호출 합니다.

```csharp
SecureStorage.RemoveAll();
```


## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 정보

# <a name="androidtabandroid"></a>[Android](#tab/android)

[Android KeyStore](https://developer.android.com/training/articles/keystore.html) 에 저장 되기 전에 값을 암호화 하는 데 암호화 키를 저장 하는 데 사용 되는 [공유 기본 설정](https://developer.android.com/training/data-storage/shared-preferences.html) 의 파일 이름을 가진 **[YOUR-앱-패키지-ID].xamarinessentials** .  공유 기본 설정 파일에서 사용 된 키가 한 _MD5 해시_ 에 전달 된 키의는 `SecureStorage` Api.

## <a name="api-level-23-and-higher"></a>API 수준 23 이상

최신 API 수준에는 **AES** Android 키 저장소에서 키를 가져와 사용한는 **AES/GCM/NoPadding** 공유 기본 설정 파일에 저장 되기 전에 값을 암호화 하는 암호입니다.

## <a name="api-level-22-and-lower"></a>API 레벨 22 및 하 한

이전 API 수준에서 Android 키 저장소만 지원 저장 **RSA** 와 함께 사용 되며 키를 **PKCS1Padding/RSA/ECB** 암호를 암호화 하는 **AES** (임의로 키 런타임에 생성) 키 아래에 있는 공유 기본 설정 파일에 저장 _SecureStorageKey_아직 생성 되지 않은 한 경우.

**SecureStorage** 사용 합니다 [기본 설정](preferences.md) API와 동일한 데이터 지 속성에 설명 된 다음과 같이 [기본 설정](preferences.md#persistence) 설명서. 이 유형의 암호화 앱은 제거 하지 않는 한 사용할 계속 장치 API 레벨 22 이하로에서 API 수준 23 이상으로 업그레이드를 하는 경우 또는 **RemoveAll** 라고 합니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

[키 집합](https://developer.xamarin.com/api/type/Security.SecKeyChain/) iOS 장치에서 안전 하 게 값을 저장 하는 데 사용 됩니다.  합니다 `SecRecord` 값을 저장 하는 데에 `Service` 값으로 설정 **[YOUR-앱-번들-ID].xamarinessentials**합니다.

일부 경우 KeyChain 데이터와 iCloud에 동기화 됩니다 없으며 응용 프로그램 제거 제거할 수 있습니다 하지 보안 값 iCloud 및 사용자의 다른 장치에서.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) UWP 장치에서 안전 하 게 암호화 된 값으로 사용 됩니다.

암호화 된 값에 저장 됩니다 `ApplicationData.Current.LocalSettings`, 라는 이름의 컨테이너 안에 **[YOUR-앱-ID].xamarinessentials**합니다.

**SecureStorage** 사용 합니다 [기본 설정](preferences.md) API와 동일한 데이터 지 속성에 설명 된 다음과 같이 [기본 설정](preferences.md#persistence) 설명서.

-----

## <a name="limitations"></a>제한 사항

이 API는 적은 양의 텍스트를 저장 하기 위한 것입니다.  성능을 사용 하 여 많은 양의 텍스트를 저장 하려는 경우에 느려질 수 있습니다.

## <a name="api"></a>API

- [SecureStorage 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [SecureStorage API 설명서](xref:Xamarin.Essentials.SecureStorage)
