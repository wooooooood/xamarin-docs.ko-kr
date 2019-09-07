---
title: 지문 인증
description: 이 가이드에서는 Android 6.0에 도입 된 지문 인증을 Xamarin Android 응용 프로그램에 추가 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: a58242e89033d6cd2652495f9466379f63f498f0
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761242"
---
# <a name="fingerprint-authentication"></a>지문 인증

_이 가이드에서는 Android 6.0에 도입 된 지문 인증을 Xamarin Android 응용 프로그램에 추가 하는 방법을 설명 합니다._

## <a name="fingerprint-authentication-overview"></a>지문 인증 개요

Android 장치에서 지문 스캐너가 도착 하면 응용 프로그램에 사용자 인증의 기존 사용자 이름/암호 방법 대신 사용할 수 있는 방법이 제공 됩니다. 지문을 사용 하 여 사용자를 인증 하면 응용 프로그램에서 사용자 이름 및 암호 보다 개입 수준이 높은 보안을 통합할 수 있습니다.

FingerprintManager Api는 지문 스캐너를 사용 하 여 장치를 대상으로 하며 API 레벨 23 (Android 6.0) 이상을 실행 하 고 있습니다. Api는 `Android.Hardware.Fingerprints` 네임 스페이스에 있습니다. Android 지원 라이브러리 v4는 이전 버전의 Android 용 지문 Api 버전을 제공 합니다. 호환성 api는 `Android.Support.v4.Hardware.Fingerprint` 네임 스페이스에서 확인할 수 있으며, [Xamarin. 지원 v4 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)를 통해 배포 됩니다.

[FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (및 해당 지원 라이브러리 대응, [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html))은 지문 검사 하드웨어를 사용 하기 위한 기본 클래스입니다. 이 클래스는 하드웨어 자체와의 상호 작용을 관리 하는 시스템 수준 서비스 주위의 Android SDK 래퍼입니다. 이는 지문 스캐너를 시작 하 고 스캐너에서 피드백에 응답 하는 일을 담당 합니다. 이 클래스에는 다음과 같은 세 개의 멤버만 있는 매우 간단한 인터페이스가 있습니다.

- **`Authenticate`** &ndash; 이 메서드는 하드웨어 스캐너를 초기화 하 고 백그라운드에서 서비스를 시작 하 여 사용자가 지문을 검색할 때까지 대기 합니다.
- **`EnrolledFingerprints`** 사용자가 하나 이상의 `true` 지문을 장치에 등록 한 경우이 속성은를 반환 합니다. &ndash;
- **`HardwareDetected`** &ndash; 이 속성은 장치에서 지문 검색을 지원 하는지 여부를 확인 하는 데 사용 됩니다.

메서드 `FingerprintManager.Authenticate` 는 Android 응용 프로그램에서 지문 스캐너를 시작 하는 데 사용 됩니다. 다음 코드 조각은 지원 라이브러리 호환성 Api를 사용 하 여 호출 하는 방법의 예입니다.

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

이 가이드에서는 api를 `FingerprintManager` 사용 하 여 지문 인증으로 Android 응용 프로그램을 개선 하는 방법을 설명 합니다. 지문 스캐너에서 결과를 보호 하는 데 `CryptoObject` 도움이 되는를 인스턴스화하고 만드는 방법을 설명 합니다. 응용 프로그램이 지문 스캐너에서 피드백을 서브클래싱하 `FingerprintManager.AuthenticationCallback` 고 피드백에 응답 하는 방법을 살펴보겠습니다. 마지막으로 Android 장치 또는 에뮬레이터에서 지문을 등록 하는 방법과 **adb** 를 사용 하 여 지문 검색을 시뮬레이트하는 방법을 알아봅니다.

## <a name="requirements"></a>요구 사항

지문 인증에는 Android 6.0 (API 수준 23) 이상 및 지문 스캐너가 있는 장치가 필요 합니다. 

인증할 각 사용자의 장치에는 지문이 이미 등록 되어 있어야 합니다. 여기에는 암호, PIN, 살짝 밀기 패턴 또는 얼굴 인식을 사용 하는 화면 잠금을 설정 하는 작업이 포함 됩니다. Android Emulator에서 일부 지문 인증 기능을 시뮬레이션할 수 있습니다.  이러한 두 항목에 대 한 자세한 내용은 [지문 등록](enrolling-fingerprint.md) 섹션을 참조 하세요. 

## <a name="related-links"></a>관련 링크

- [지문 가이드 샘플 앱](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fingerprintguide)
- [지문 대화 상자 샘플](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [런타임에 권한 요청](https://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](https://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](xref:Android.Content.Context)
- [지문 및 지불 API (비디오)](https://youtu.be/VOn7VrTRlA4)
