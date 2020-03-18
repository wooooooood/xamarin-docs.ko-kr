---
title: 지문 인증 시작
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/17/2018
ms.openlocfilehash: 746a096f93036e63b29bc917826259f88426cead
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020276"
---
# <a name="getting-started-with-fingerprint-authentication"></a>지문 인증 시작

시작을 위해 먼저 애플리케이션에서 지문 인증을 사용할 수 있도록 Xamarin.Android 프로젝트를 구성하는 방법을 살펴보겠습니다.

1. **AndroidManifest.xml**을 업데이트하여 지문 API에 필요한 권한을 선언합니다.
2. `FingerprintManager`에 대한 참조를 가져옵니다.
3. 디바이스에서 지문 검사를 수행할 수 있는지 확인합니다.

## <a name="requesting-permissions-in-the-application-manifest"></a>애플리케이션 매니페스트에서 권한 요청

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Android 애플리케이션은 매니페스트에서 `USE_FINGERPRINT` 권한을 요청해야 합니다. 다음 스크린샷은 Visual Studio에서 애플리케이션에 이 권한을 추가하는 방법을 보여줍니다.

[![Android 매니페스트 화면에서 USE\_FINGERPRINT 사용](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

Android 애플리케이션은 매니페스트에서 `USE_FINGERPRINT` 권한을 요청해야 합니다. 다음 스크린샷은 Mac용 Visual Studio에서 애플리케이션에 이 권한을 추가하는 방법을 보여줍니다.

[![Android 애플리케이션 화면에서 UseFingerprint 사용](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>FingerprintManager의 인스턴스 가져오기

다음으로, 애플리케이션이 `FingerprintManager` 또는 `FingerprintManagerCompat` 클래스의 인스턴스를 가져와야 합니다. 이전 버전의 Android와 호환되려면 Android 애플리케이션이 Android Support v4 NuGet 패키지에 있는 호환성 API를 사용해야 합니다. 다음 코드 조각은 운영 체제에서 적절한 개체를 가져오는 방법을 보여줍니다. 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

위의 코드 조각에서 `context`는 Android `Android.Content.Context`입니다. 일반적으로 이는 인증을 수행하는 작업입니다.

## <a name="checking-for-eligibility"></a>자격 확인

애플리케이션은 여러 검사를 수행하여 지문 인증을 사용할 수 있는지 확인해야 합니다. 애플리케이션이 자격을 확인하는 데 사용하는 조건은 총 5개가 있습니다.  

**API 수준 23** &ndash; 지문 API를 사용하려면 API 수준 23 이상이 필요합니다. `FingerprintManagerCompat` 클래스가 API 수준 검사를 자동으로 래핑합니다. 이러한 이유로 **Android 지원 라이브러리 v4** 및 `FingerprintManagerCompat`를 사용하는 것이 좋습니다. 이는 다음과 같은 검사 중 하나를 수행합니다.

**하드웨어** &ndash; 애플리케이션이 처음 시작될 때 지문 스캐너가 있는지 확인해야 합니다.

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```

**디바이스가 보호됨** &ndash; 사용자는 화면 잠금으로 디바이스를 보호해야 합니다. 사용자가 화면 잠금을 사용하여 디바이스를 보호하지 않은 경우 애플리케이션에 보안이 중요하다면 화면 잠금을 구성해야 한다는 알림이 사용자에게 표시되어야 합니다. 다음 코드 조각에서는 이러한 필수 조건을 확인하는 방법을 보여줍니다.

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**등록된 지문** &ndash; 사용자는 운영 체제에 하나 이상의 지문을 등록해야 합니다. 이 권한 검사는 각 인증 시도 전에 발생합니다.

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**권한** &ndash; 사용자가 애플리케이션을 사용하기 전에 애플리케이션이 사용자의 권한을 요청해야 합니다. Android 5.0 이하에서는 사용자가 앱을 설치하는 조건으로 사용자에게 권한을 부여합니다. Android 6.0에는 런타임에 권한을 확인하는 새 권한 모델이 도입되었습니다. 이 코드 조각은 Android 6.0에 대한 권한을 확인하는 방법의 예입니다.

```csharp
// The context is typically a reference to the current activity.
Android.Content.PM.Permission permissionResult = ContextCompat.CheckSelfPermission(context, Manifest.Permission.UseFingerprint);
if (permissionResult == Android.Content.PM.Permission.Granted)
{
    // Permission granted - go ahead and start the fingerprint scanner.
}
else
{
    // No permission. Go and ask for permissions and don't start the scanner. See
    // https://developer.android.com/training/permissions/requesting.html
}
```

애플리케이션에서 인증 옵션을 제공할 때마다 이러한 모든 조건을 확인하면 사용자에게 최상의 사용자 환경이 제공됩니다. 디바이스 또는 운영 체제에 대한 변경 또는 업그레이드는 지문 인증의 사용 가능 여부에 영향을 줄 수 있습니다. 이러한 검사의 결과를 캐시하도록 선택하는 경우 업그레이드 시나리오에 맞추어야 합니다.

Android 6.0에서 권한을 요청하는 방법에 대한 자세한 내용은 Android 가이드 [런타임에 권한 요청](https://developer.android.com/training/permissions/requesting.html)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [컨텍스트](xref:Android.Content.Context)
- [KeyguardManager](xref:Android.App.KeyguardManager)
- [ContextCompat](https://developer.android.com/reference/android/support/v4/content/ContextCompat)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [런타임에 권한 요청](https://developer.android.com/training/permissions/requesting.html)
