---
title: 지문 인증
description: 이 가이드에서는 Xamarin.Android 응용 프로그램에 Android 6.0에 도입 된 지문 인증을 추가 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1b28b16dfd92ef3a31201ef2e86681a425a58ab8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764120"
---
# <a name="fingerprint-authentication"></a>지문 인증

_이 가이드에서는 Xamarin.Android 응용 프로그램에 Android 6.0에 도입 된 지문 인증을 추가 하는 방법에 설명 합니다._


## <a name="fingerprint-authentication-overview"></a>지문 인증 개요

Android 장치에서 지문 스캐너의 도착 사용자 인증의 기존 사용자 이름/암호 메서드 하는 대신 응용 프로그램을 제공합니다. 사용자를 인증 하는 지문 사용 하면 사용자 이름 및 암호 보다 개입 수준이 낮습니다 보안을 통합 하는 응용 프로그램입니다.

FingerprintManager Api 지문 스캐너와 장치를 대상으로 하 고 API 수준 23 (Android 6.0)가 실행 되 고 이상. Api에서 발견 되는 `Android.Hardware.Fingerprints` 네임 스페이스입니다. Android 지원 라이브러리 v4 지문 Android의 이전 버전을 위한 Api의 버전을 제공 합니다. 호환성 Api에서 발견 되는 `Android.Support.v4.Hardware.Fingerprint` 네임 스페이스를 통해 배포 되는 [Xamarin.Android.Support.v4 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)합니다.

[FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (및 해당 지원 라이브러리 정수 [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html))는 하드웨어를 검사 하는 지문을 사용 하기 위한 기본 클래스입니다. 이 클래스는 자체 하드웨어와의 상호 작용을 관리 하는 시스템 수준 서비스는 Android SDK 래퍼입니다. 여기서는 지문 스캐너를 시작 하 고 스캐너에서 피드백에 대응 하기 위한입니다. 이 클래스는 세 멤버가 포함 된 매우 간단한 인터페이스를 있습니다.

* **`Authenticate`** &ndash; 이 메서드는 하드웨어 스캐너를 초기화 하 고 지문을 검색 하려면 사용자에 대 한 대기는 백그라운드에서 서비스를 시작 합니다.
* **`EnrolledFingerprints`** &ndash; 이 속성은 반환 `true` 는 사용자가 장치에서 하나 이상의 지문을 등록 하는 경우.
* **`HardwareDetected`** &ndash; 이 속성은 장치 지문 검색을 지원 하는지 확인 하는

`FingerprintManager.Authenticate` 메서드 지문 스캐너를 시작 하는 Android 응용 프로그램에서 사용 됩니다. 다음 코드 조각은 지원 라이브러리 호환성 Api를 사용 하 여 해당 파일을 호출 하는 방법의 예입니다.

```csharp
// context is any Android.Content.Context instance, typically the Activity 
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
fingerprintManager.Authenticate(FingerprintManager.CryptoObject crypto,
                                int flags,
                                CancellationSignal cancel,
                                FingerprintManagerCompat.AuthenticationCallback callback,
                                Handler handler
                               );
```

이 가이드를 사용 하는 방법에 설명 합니다는 `FingerprintManager` 지문 인증으로 Android 응용 프로그램을 향상 시키기 위해 Api입니다. 인스턴스화할 때 만드는 방법에 적용 됩니다는 `CryptoObject` 지문 스캐너에서 결과 보호 하기 위해. 응용 프로그램 하위 클래스를 해야 하는 방법을 검토 합니다 `FingerprintManager.AuthenticationCallback` 지문 스캐너에서 피드백에 응답 합니다. 마지막으로, 보겠습니다 지문 Android 장치 또는 에뮬레이터에 등록 하는 방법 및 사용 하는 방법을 **adb** 지문 검색을 시뮬레이션할 수 있습니다.

## <a name="requirements"></a>요구 사항

지문 인증에 필요한 Android 6.0 (API 수준 23) 또는 더 높은 대 한 지문 스캐너를 사용 하 여 장치입니다. 

지문을 인증 하는 각 사용자에 대 한 장치에서 이미 등록 해야 합니다. 이 암호, PIN, 살짝 패턴 또는 안 면 인식을 사용 하는 화면 잠금 설정을 해야 합니다. Android 에뮬레이터에서 지문 인증 기능 중 일부를 시뮬레이션 하는 것이 불가능 합니다.  이 두 항목에 대 한 자세한 내용은 참조 하십시오는 [지문 등록](enrolling-fingerprint.md) 섹션. 






## <a name="related-links"></a>관련 링크

- [지문 가이드 샘플 응용 프로그램](https://developer.xamarin.com/samples/monodroid/FingerprintGuide/)
- [지문 대화 샘플](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [런타임 시 권한 요청](http://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](http://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [지문 및 지불 API (비디오)](https://youtu.be/VOn7VrTRlA4)
