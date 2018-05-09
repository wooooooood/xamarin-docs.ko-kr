---
title: 보안 저장소 Xamarin.Essentials
description: SecureStorage 클래스는 단순 키/값 쌍을 안전 하 게 저장 하는 데 도움이 됩니다.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
ms.technology: xamarin-crossplatform
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: 23b38721d1437edba611dc6575593b83e2d4a548
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-secure-storage"></a>보안 저장소 Xamarin.Essentials

![시험판 NuGet](~/media/shared/pre-release.png)

**SecureStorage** 클래스에 안전 하 게 단순 키/값 쌍을 저장 하는 데 도움이 됩니다.

## <a name="using-secure-storage"></a>보안 저장소를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

에 대 한 값을 저장 하는 지정 된 _키_ 보안 저장소에서:

```csharp
await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
```

에 보안 저장소에서 값을 검색 합니다.

```csharp
var oauthToken = await SecureStorage.GetAsync("oauth_token");
```

## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 사항

# <a name="androidtabandroid"></a>[Android](#tab/android)

[Android KeyStore](https://developer.android.com/training/articles/keystore.html) 에 저장 하기 전에 값을 암호화 하는 데 사용 하는 암호화 키를 저장 하는 데 사용 되는 [공유 기본 설정을](https://developer.android.com/training/data-storage/shared-preferences.html) 의 파일 이름을 가진 **.xamarinessentials [귀하가-응용 프로그램-패키지-ID]** .  공유 기본 설정 파일에 사용 된 키가 한 _MD5 해시_ 에 전달 된 키의는 `SecureStorage` API의 합니다.

## <a name="api-level-23-and-higher"></a>API 수준 23 이상

새 API 수준에는 **AES** 키는 Android 키 저장소에서 가져온 이며 함께 사용는 **AES/GCM/NoPadding** 공유 기본 설정 파일에 저장 되기 전에 값을 암호화 하는 암호입니다.

## <a name="api-level-22-and-lower"></a>API 수준 22 및 하 한

이전 API 수준에서 Android KeyStore만 저장할 수 있도록 지원 **RSA** 키와 함께 사용 되는 **RSA/ECB/PKCS1Padding** 암호를 암호화 하는 **AES** (임의로 키 런타임에 생성) 키 아래에 있는 공유 기본 설정 파일에 저장 하 고 _SecureStorageKey_아직 생성 되지 않은 한 경우, 합니다.

암호화 된 모든 값은 장치에서 앱을 제거할 때 제거 됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

[키 집합](https://developer.xamarin.com/api/type/Android.Security.KeyChain/) iOS 장치에서 값을 안전 하 게 저장 하는 데 사용 됩니다.  `SecRecord` 값을 저장 하는 데에 `Service` 값으로 설정 **[귀하가-응용 프로그램-번들-ID].xamarinessentials**합니다.

일부 경우에 키 집합 데이터를 iCloud와 동기화 됩니다 응용 프로그램 제거 수 제거 / 하지 안전한 값 iCloud와 사용자의 다른 장치에서 합니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/en-us/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) encryped 값 UWP 장치에서 안전 하 게 하는 데 사용 됩니다.

Encryped 값에 저장 됩니다 `ApplicationData.Current.LocalSettings`, 컨테이너의 이름으로 내부 **[귀하가 앱 ID].xamarinessentials**합니다.

응용 프로그램 제거는 _LocalSettings_, 및 암호화 된 모든 값도 제거 됩니다.

-----

## <a name="limitations"></a>제한 사항

이 API는 적은 양의 텍스트를 저장 하기 위한 것입니다.  성능이 많은 양의 텍스트를 저장 하려면 사용 하려는 경우 느릴 수 있습니다.

## <a name="api"></a>API

- [SecureStorage 소스 코드](https://github.com/xamarin/Essentials/tree/master/Essentials/SecureStorage)
- [SecureStorage API 설명서](xref:Xamarin.Essentials.SecureStorage)
