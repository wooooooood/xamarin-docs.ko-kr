---
title: 지문 인증 시작
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/17/2018
ms.openlocfilehash: 731aeaf0ad89a44211072962bf9891851a44ffcc
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667726"
---
# <a name="getting-started-with-fingerprint-authentication"></a>지문 인증 시작

시작 하려면 먼저 살펴봅니다 응용 프로그램에서 지문 인증을 사용할 수 있도록 Xamarin.Android 프로젝트를 구성 하는 방법.

1. 업데이트 **AndroidManifest.xml** 지문 Api를 필요로 하는 권한을 선언 합니다.
2. 에 대 한 참조를 `FingerprintManager`입니다.
3. 장치는 지문 검색 가능 인지 확인 합니다.

## <a name="requesting-permissions-in-the-application-manifest"></a>응용 프로그램에 필요한 권한을 요청 매니페스트

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Android 응용 프로그램을 요청 해야 합니다는 `USE_FINGERPRINT` 매니페스트에서 권한. 다음 스크린샷은 Visual Studio 2015에서 응용 프로그램에이 권한을 추가 하는 방법을 보여 줍니다.

[![사용 하도록 설정 하면\_Android 매니페스트 화면에서 지문](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Android 응용 프로그램을 요청 해야 합니다는 `USE_FINGERPRINT` 매니페스트에서 권한. 다음 스크린샷은 Mac 용 Visual Studio에서 응용 프로그램에이 권한을 추가 하는 방법을 보여 줍니다.

[![Android 응용 프로그램 화면에서 UseFingerprint를 사용 하도록 설정](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>FingerprintManager의 인스턴스를 시작합니다.

다음으로, 응용 프로그램의 인스턴스를 가져와야 합니다 `FingerprintManager` 또는 `FingerprintManagerCompat` 클래스입니다. 와 호환 되도록 이전 버전의 Android, Android 응용 프로그램 호환성 API를 사용 해야 Android 지원 v4 NuGet 패키지에서 찾을 수 있습니다. 다음 코드 조각에 운영 체제에서 적절 한 개체를 가져오는 방법을 보여 줍니다. 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

이전 코드 조각에는 `context` 는 모든 Android `Android.Content.Context`합니다. 일반적으로 인증을 수행 하는 작업입니다.

## <a name="checking-for-eligibility"></a>자격 확인

응용 프로그램 지문 인증을 사용할 수 있는지 확인 하려면 몇 가지 검사를 수행 해야 합니다. 전체적으로 응용 프로그램 자격을 확인 하는 데 사용 하는 5 개의 조건  

**API 레벨 23** &ndash; 지문 Api API 레벨 23 이상이 필요 합니다. `FingerprintManagerCompat` 클래스는 API 수준 검사를 래핑합니다. 이러한 이유로를 사용 합니다 **Android 지원 라이브러리 v4** 및 `FingerprintManagerCompat`;이 이러한 검사 중 하나를 고려 합니다.

**하드웨어** &ndash; 응용 프로그램을 처음 시작 되 면 지문 스캐너의 존재 여부 확인 해야 합니다.

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```

**장치가 보안 된** &ndash; 사용자 화면 잠금을 사용 하 여 보안 장치에 있어야 합니다. 사용자가 화면 잠금 사용 하 여 장치를 보호 하지 보안 응용 프로그램에 중요 한 경우 다음 사용자 알려야 화면 잠금을 설정 해야 합니다. 다음 코드 조각은이 필수 구성 확인 하는 방법을 보여 줍니다.

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**지문 등록** &ndash; 사용자 운영 체제를 사용 하 여 등록 하는 하나 이상의 지문 있어야 합니다. 이 권한 검사는 각 인증 시도 하기 전에 발생 합니다.

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**사용 권한** &ndash; 응용 프로그램이 응용 프로그램을 사용 하기 전에 사용자 로부터 권한을 요청 해야 합니다. Android 5.0 및 낮은 경우 사용자 앱을 설치 하는 조건으로 사용 권한을 부여 합니다. Android 6.0 런타임 시 사용 권한을 확인 하는 새 권한 모델을 도입 했습니다. 이 코드 조각은 다음과 같습니다. Android 6.0에 대 한 권한을 확인 하는 방법의 예

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

인증 옵션은 사용자가 최상의 사용자 환경을 확인 될 때마다 응용 프로그램 제공 하는 이러한 조건을 모두 확인 합니다. 변경 하거나 해당 장치 또는 운영 체제 업그레이드 지문 인증의 가용성 영향을 줄 수 있습니다. 이러한 검사의 결과 캐시 하려는 경우에 업그레이드 시나리오에 대 한 제공 해야 합니다.

Android 6.0에 대 한 권한 요청 하는 방법에 대 한 자세한 내용은 Android 가이드를 참조 하십시오 [런타임 시 권한 요청](https://developer.android.com/training/permissions/requesting.html)합니다.

## <a name="related-links"></a>관련 링크

- [컨텍스트](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [ContextCompat](https://developer.xamarin.com/api/type/Android.Support.V4.Content.ContextCompat/)
- [KeyguardManager](https://developer.xamarin.com/api/type/Android.App.KeyguardManager/)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [런타임 시 권한 요청](https://developer.android.com/training/permissions/requesting.html)
