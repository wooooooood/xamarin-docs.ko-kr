---
title: "지문 인증 시작"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: e3c986b03408dae98a5a79f257029c10909aeabd
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="getting-started-with-fingerprint-authentication"></a>지문 인증 시작

시작 하려면 먼저 살펴보겠습니다 지문 인증을 사용 하는 응용 프로그램 있도록 Xamarin.Android 프로젝트를 구성 하는 방법:

1. 업데이트 **AndroidManifest.xml** 지문 Api 필요로 하는 권한을 선언할 수 있습니다.
2. 에 대 한 참조는 `FingerprintManager`합니다.
3. 장치는 지문 검색 가능 인지 확인 합니다.

## <a name="requesting-permissions-in-the-application-manifest"></a>응용 프로그램에 대 한 권한을 요청 매니페스트

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Android 응용 프로그램 요청 해야 합니다는 `USE_FINGERPRINT` 매니페스트에 사용 권한입니다. 다음 스크린샷은 Visual Studio 2015에서 응용 프로그램에이 사용 권한을 추가 하는 방법을 보여 줍니다.

[![사용 하도록 설정 하면\_Android 매니페스트 화면에는 지문](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Android 응용 프로그램 요청 해야 합니다는 `USE_FINGERPRINT` 매니페스트에 사용 권한입니다. 다음 스크린 샷에서 Mac 용 Visual Studio에서 응용 프로그램에이 사용 권한을 추가 하는 방법을 보여 줍니다.

[![Android 응용 프로그램 화면에서 사용할 수 있도록 UseFingerprint](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>FingerprintManager의 인스턴스를 얻는

다음으로 응용 프로그램의 인스턴스를 가져올 해야는 `FingerprintManager` 또는 `FingerprintManagerCompat` 클래스입니다. 이전 버전의 Android와 호환 되도록 Android 응용 프로그램 호환성 API를 사용 해야의 Android 지원 v4 NuGet 패키지에서 찾을 수 있습니다. 다음 코드 조각에는 운영 체제에서 적절 한 개체를 가져오는 방법을 보여 줍니다. 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

이전 코드 조각에는 `context` 모든 Android은 `Android.Content.Context`합니다. 일반적으로 인증을 수행 된 활동입니다.

## <a name="checking-for-eligibility"></a>자격에 대 한 확인 하는 중

응용 프로그램 지문 인증을 사용할 수 있다는 것을 확인 하려면 몇 가지 검사를 수행 해야 합니다. 전체적으로 응용 프로그램이 확인할 부착 하는 데 사용 하는 5 개의 조건 있습니다.  
 

**API 수준 23** &ndash; 는 지문 Api API 수준 23 이상의 필요 합니다. `FingerprintManagerCompat` 클래스는 API 수준 검사 수에 대 한 줄 바꿈됩니다. 이러한 이유로 사용 하도록 권장 되는 **Android 지원 라이브러리 v4** 및 `FingerprintManagerCompat`; 여기에 이러한 검사 중 하나를 고려 합니다.

**하드웨어** &ndash; 응용 프로그램이 시작 될 때 처음으로, 지문 스캐너의 현재 상태에 대 한 확인 합니다.

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```
    
**장치는 보안** &ndash; 사용자 화면 잠금으로 보안이 설정 된 장치를 휴대 해야 합니다. 사용자가 화면 잠금을 사용 하 여 장치를 보호 하지 하는 경우 보안은 응용 프로그램에 중요 한 다음 사용자 사람과 알림을 받는 화면 잠금을 구성 해야 합니다. 다음 코드 조각은이 이전 requiste를 확인 하는 방법을 보여 줍니다.

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**지문을 등록** &ndash; 사용자 운영 체제에 등록 하는 하나 이상의 지문 있어야 합니다. 이 사용 권한 검사는 각 인증 시도 하기 전에 발생 합니다.

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**사용 권한** &ndash; 응용 프로그램이 응용 프로그램을 사용 하기 전에 사용자 로부터 권한을 요청 해야 합니다. Android 5.0 및 아래에 대 한 사용자는 앱을 설치 조건으로 사용 권한을 부여 합니다. Android 6.0 실행 시 사용 권한을 확인 하는 새 사용 권한 모델을 도입 했습니다. 이 코드 조각은 Android 6.0에 대 한 권한을 확인 하는 방법의 예입니다.

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
    // http://developer.android.com/training/permissions/requesting.html
}
```

응용 프로그램 지문 인증을 사용 하고자 하 때마다 3, 4 및 5 조건을 확인 해야 합니다. 응용 프로그램 처음으로 결과 저장 장치에서 실행 되는 처음 두 조건은 체크 인할 수 있습니다 (공유 기본 설정 예를 들어).

Android 6.0에서 사용 권한 요청 하는 방법에 대 한 자세한 내용은 Android 설명서를 참조 하십시오. [실행 시 권한 요청](http://developer.android.com/training/permissions/requesting.html)합니다.



## <a name="related-links"></a>관련 링크

- [컨텍스트](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [ContextCompat](https://developer.xamarin.com/api/type/Android.Support.V4.Content.ContextCompat/)
- [KeyguardManager](https://developer.xamarin.com/api/type/Android.App.KeyguardManager/)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [실행 시 권한 요청](http://developer.android.com/training/permissions/requesting.html)
