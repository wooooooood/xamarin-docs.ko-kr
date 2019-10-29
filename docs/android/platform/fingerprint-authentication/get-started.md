---
title: 지문 인증 시작
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/17/2018
ms.openlocfilehash: 746a096f93036e63b29bc917826259f88426cead
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020276"
---
# <a name="getting-started-with-fingerprint-authentication"></a>지문 인증 시작

시작 하려면 먼저 응용 프로그램에서 지문 인증을 사용할 수 있도록 Xamarin Android 프로젝트를 구성 하는 방법을 살펴보겠습니다.

1. **Androidmanifest .xml** 을 업데이트 하 여 지문 api에 필요한 사용 권한을 선언 합니다.
2. `FingerprintManager`에 대 한 참조를 가져옵니다.
3. 장치에서 지문 검사를 수행할 수 있는지 확인 합니다.

## <a name="requesting-permissions-in-the-application-manifest"></a>응용 프로그램 매니페스트에서 권한 요청

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Android 응용 프로그램은 매니페스트에서 `USE_FINGERPRINT` 권한을 요청 해야 합니다. 다음 스크린샷은 Visual Studio에서 응용 프로그램에이 권한을 추가 하는 방법을 보여 줍니다.

[Android 매니페스트 화면에서\_지문을 사용 하도록 설정![](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Android 응용 프로그램은 매니페스트에서 `USE_FINGERPRINT` 권한을 요청 해야 합니다. 다음 스크린샷은 Mac용 Visual Studio의 응용 프로그램에이 권한을 추가 하는 방법을 보여 줍니다.

[Android 응용 프로그램 화면에서 UseFingerprint을 사용 하도록 설정 하는![](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>FingerprintManager의 인스턴스 가져오기

그런 다음 응용 프로그램은 `FingerprintManager` 또는 `FingerprintManagerCompat` 클래스의 인스턴스를 가져와야 합니다. Android의 이전 버전과 호환 되려면 android 응용 프로그램이 Android Support v4 NuGet 패키지에 있는 호환성 API를 사용 해야 합니다. 다음 코드 조각은 운영 체제에서 적절 한 개체를 가져오는 방법을 보여 줍니다. 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

위의 코드 조각에서 `context`은 Android `Android.Content.Context`입니다. 일반적으로이 작업은 인증을 수행 하는 작업입니다.

## <a name="checking-for-eligibility"></a>자격 확인

응용 프로그램에서 여러 검사를 수행 하 여 지문 인증을 사용할 수 있는지 확인 해야 합니다. 전체에는 응용 프로그램에서 자격을 확인 하는 데 사용 하는 5 가지 조건이 있습니다.  

**Api 레벨 23** &ndash; 지문 Api에는 api 레벨 23 이상이 필요 합니다. `FingerprintManagerCompat` 클래스는 API 수준 검사를 래핑합니다. 이러한 이유로 **Android 지원 라이브러리 v4** 및 `FingerprintManagerCompat`를 사용 하는 것이 좋습니다. 이렇게 하면 이러한 검사 중 하나를 고려 합니다.

**하드웨어** &ndash; 응용 프로그램이 처음 시작 될 때 지문 스캐너가 있는지 확인 해야 합니다.

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```

**장치** 는 화면 잠금으로 보안이 설정 된 상태에서 사용자가 장치를 안전 하 게 보호 &ndash; 합니다. 사용자가 화면 잠금을 사용 하 여 장치를 보호 하지 않고 응용 프로그램에 보안이 중요 한 경우 화면 잠금을 구성 해야 한다는 알림이 사용자에 게 표시 되어야 합니다. 다음 코드 조각에서는이 requiste를 확인 하는 방법을 보여 줍니다.

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**등록 된 지문을** &ndash; 사용자에 게는 운영 체제에 등록 된 지문이 하나 이상 있어야 합니다. 이 권한 검사는 각 인증 시도 전에 발생 합니다.

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

응용 프로그램 &ndash; 사용 **권한은** 응용 프로그램을 사용 하기 전에 사용자의 권한을 요청 해야 합니다. Android 5.0 이하의 경우 사용자가 앱을 설치 하는 조건으로 사용 권한을 부여 합니다. Android 6.0에는 런타임에 사용 권한을 확인 하는 새 사용 권한 모델이 도입 되었습니다. 이 코드 조각은 Android 6.0에 대 한 사용 권한을 확인 하는 방법의 예입니다.

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

응용 프로그램에서 인증 옵션을 제공할 때마다 이러한 모든 조건을 확인 하면 사용자에 게 최상의 사용자 환경이 제공 됩니다. 장치 또는 운영 체제에 대 한 변경 또는 업그레이드는 지문 인증의 가용성에 영향을 줄 수 있습니다. 이러한 검사의 결과를 캐시 하도록 선택 하는 경우 업그레이드 시나리오를 사용할 수 있도록 해야 합니다.

Android 6.0에서 사용 권한을 요청 하는 방법에 대 한 자세한 내용은 [런타임 시 android 가이드 권한 요청](https://developer.android.com/training/permissions/requesting.html)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [측면](xref:Android.Content.Context)
- [KeyguardManager](xref:Android.App.KeyguardManager)
- [ContextCompat](https://developer.android.com/reference/android/support/v4/content/ContextCompat)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [런타임에 권한 요청](https://developer.android.com/training/permissions/requesting.html)
