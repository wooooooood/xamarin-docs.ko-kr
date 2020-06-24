---
title: 'Xamarin.Essentials: 보안 스토리지'
description: 이 문서에서는 간단한 키/값 쌍을 안전하게 저장하는 데 도움이 되는 Xamarin.Essentials의 SecureStorage 클래스에 관해 설명합니다. 클래스 사용 방법, 플랫폼 구현 관련 정보 및 제한 사항을 설명합니다.
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 061bc1cfe785ad080092ba21340f7d38bc499ed9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84801948"
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials: 보안 스토리지

**SecureStorage** 클래스는 간단한 키/값 쌍을 안전하게 저장하는 데 도움이 됩니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

**SecureStorage** 기능에 액세스하려면 다음과 같은 플랫폼 특정 설정이 필요합니다.

# <a name="android"></a>[Android](#tab/android)

> [!TIP]
> [앱에 대한 자동 백업](https://developer.android.com/guide/topics/data/autobackup)은 Android 6.0(API 레벨 23) 이상의 기능으로, 사용자의 앱 데이터(공유 기본 설정, 앱 내부 스토리지의 파일 및 기타 특정 파일)를 백업합니다. 앱을 다시 설치하거나 새 디바이스에 설치하면 데이터가 복원됩니다. 이는 백업된 공유 기본 설정을 이용하며 복원 시 암호를 해독할 수 없는 `SecureStorage`에 영향을 줄 수 있습니다. Xamarin.Essentials는 키를 제거해서 다시 설정할 수 있도록 하여 자동으로 이러한 경우를 처리하지만, 자동 백업을 사용하지 않도록 설정하면 추가 단계를 수행할 수 있습니다.

### <a name="enable-or-disable-backup"></a>백업 사용 또는 사용 안 함
`AndroidManifest.xml` 파일에서 `android:allowBackup` 설정을 false로 지정하여 전체 애플리케이션의 자동 백업을 사용하지 않을 수 있습니다. 이 방법은 다른 방법으로 데이터를 복원하려는 경우에만 권장됩니다.

```xml
<manifest ... >
    ...
    <application android:allowBackup="false" ... >
        ...
    </application>
</manifest>
```

### <a name="selective-backup"></a>선택적 백업
특정 콘텐츠가 백업되지 않도록 자동 백업을 구성할 수 있습니다. 사용자 지정 규칙 집합을 만들어 `SecureStore` 항목이 백업되지 않도록 제외할 수 있습니다.

1. **AndroidManifest.xml**에서 `android:fullBackupContent` 특성을 설정합니다.

    ```xml
    <application ...
        android:fullBackupContent="@xml/auto_backup_rules">
    </application>
    ```

2. **AndroidResource**의 빌드 작업으로 **Resources/xml** 디렉터리에 **auto_backup_rules.xml**이라는 새 XML 파일을 만듭니다. `SecureStorage`를 제외한 모든 공유 기본 설정을 포함하는 다음 콘텐츠를 설정합니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <full-backup-content>
        <include domain="sharedpref" path="."/>
        <exclude domain="sharedpref" path="${applicationId}.xamarinessentials.xml"/>
    </full-backup-content>
    ```

# <a name="ios"></a>[iOS](#tab/ios)

**iOS 시뮬레이터**에서 개발하는 경우 **키 체인** 자격을 사용하도록 설정하고 애플리케이션의 번들 식별자에 대한 키 체인 액세스 그룹을 추가합니다.

iOS 프로젝트에서 **Entitlements.plist**를 열고 **키 체인** 자격을 찾아 사용하도록 설정합니다. 그러면 애플리케이션 식별자가 그룹으로 자동 추가됩니다.

프로젝트 속성의 **iOS 번들 서명** 아래에서 **사용자 지정 자격**을 **Entitlements.plist**로 설정합니다.

> [!TIP]
> iOS 디바이스에 배포할 때는 이 자격이 필요하지 않으므로 제거해야 합니다.

# <a name="uwp"></a>[UWP](#tab/uwp)

추가 설정이 필요하지 않습니다.

-----

## <a name="using-secure-storage"></a>보안 스토리지 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

지정된 _키_의 값을 보안 스토리지에 저장하려면:

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

보안 스토리지에서 값을 검색하려면:

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
> 요청된 키와 연결된 값이 없는 경우 `GetAsync`는 `null`을 반환합니다.

특정 키를 제거하려면 다음을 호출합니다.

```csharp
SecureStorage.Remove("oauth_token");
```

모든 키를 제거하려면 다음을 호출합니다.

```csharp
SecureStorage.RemoveAll();
```

## <a name="platform-implementation-specifics"></a>플랫폼 구현 관련 정보

# <a name="android"></a>[Android](#tab/android)

[Android 키 저장소](https://developer.android.com/training/articles/keystore.html)를 사용하여 **[YOUR-APP-PACKAGE-ID].xamarinessentials**라는 파일 이름으로 [공유 기본 설정](https://developer.android.com/training/data-storage/shared-preferences.html)에 저장되기 전에 값을 암호화하는 데 사용되는 암호화 키를 저장합니다.  공유 기본 설정 파일에 사용되는 키(_값_에 대한 _키_인 암호화 키 아님)는 `SecureStorage` API로 전달되는 키의 _MD5 해시_입니다.

**API 레벨 23 이상**

최신 API 레벨에서 **AES** 키는 Android 키 저장소에서 가져오며, **AES/GCM/NoPadding** 암호화에 사용되어 값이 공유 기본 설정 파일에 저장되기 전에 암호화합니다.

**API 레벨 22 이하**

이전 API 레벨에서 Android 키 저장소는 **RSA** 키 저장만 지원합니다. 이 키는 **RSA/ECB/PKCS1Padding** 암호화와 함께 사용되어 **AES** 키(런타임에 임의로 생성됨)를 암호화하며, 아직 생성되지 않은 경우 공유 기본 설정 파일의 _SecureStorageKey_ 키 아래에 저장됩니다.

**SecureStorage**는 [기본 설정](preferences.md) API를 사용하며 [기본 설정](preferences.md#persistence) 문서에 설명된 것과 동일한 데이터 지속성을 따릅니다. 디바이스가 API 레벨 22 이하에서 API 레벨 23 이상으로 업그레이드되는 경우, 앱을 제거하거나 **RemoveAll**을 호출하지 않는 한 이 유형의 암호화가 계속 사용됩니다.

# <a name="ios"></a>[iOS](#tab/ios)

[KeyChain](xref:Security.SecKeyChain)을 사용하여 iOS 디바이스에 값을 안전하게 저장합니다.  값을 저장하는 데 사용된 `SecRecord`의 `Service` 값은 **[YOUR-APP-BUNDLE-ID].xamarinessentials**로 설정됩니다.

KeyChain 데이터가 iCloud와 동기화되어 애플리케이션을 제거해도 iCloud 및 사용자의 다른 디바이스에서 안전한 값이 제거되지 않는 경우도 있습니다.

# <a name="uwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider)를 사용하여 UWP 디바이스에서 값을 안전하게 암호화합니다.

암호화된 값은 `ApplicationData.Current.LocalSettings`의 컨테이너 안에 **[YOUR-APP-ID].xamarinessentials**라는 이름으로 저장됩니다.

**SecureStorage**는 [기본 설정](preferences.md) API를 사용하며 [기본 설정](preferences.md#persistence) 문서에 설명된 것과 동일한 데이터 지속성을 따릅니다. 또한 각 설정의 이름 길이가 최대 255자 이하로 제한된 `LocalSettings`을(를) 사용합니다. 각 설정의 크기는 최대 8K 바이트이며 각 복합 설정은 크기는 최대 64K 바이트까지 가능합니다.

-----

## <a name="limitations"></a>제한 사항

이 API는 소량 텍스트를 저장하기 위한 것입니다.  대량 텍스트를 저장하는 데 사용하려고 하면 성능이 느려질 수 있습니다.

## <a name="api"></a>API

- [SecureStorage 소스 코드](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/SecureStorage)
- [SecureStorage API 문서](xref:Xamarin.Essentials.SecureStorage)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Secure-Storage-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
