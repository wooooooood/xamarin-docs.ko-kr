---
title: 지문 인증
description: 이 가이드에서는 Android 6.0에 도입된 지문 인증을 Xamarin.Android 애플리케이션에 추가하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 4a4b6ee7a123683a9d5a140c46c0b3542767ffa3
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027520"
---
# <a name="fingerprint-authentication"></a>지문 인증

_이 가이드에서는 Android 6.0에 도입된 지문 인증을 Xamarin.Android 애플리케이션에 추가하는 방법을 설명합니다._

## <a name="fingerprint-authentication-overview"></a>지문 인증 개요

Android 디바이스에서 지문 스캐너가 도착하면 애플리케이션에 사용자 인증의 기존 사용자 이름/암호 방법 대신 사용할 수 있는 방법이 제공됩니다. 지문을 사용하여 사용자를 인증하면 애플리케이션에서 사용자 이름 및 암호보다 파급력이 낮은 보안을 통합할 수 있습니다.

FingerprintManager API는 지문 스캐너를 사용하여 디바이스를 대상으로 하며 API 레벨 23(Android 6.0) 이상을 실행하고 있습니다. API는 `Android.Hardware.Fingerprints` 네임스페이스에 있습니다. Android 지원 라이브러리 v4는 이전 버전의 Android용 지문 API 버전을 제공합니다. 호환성 API는 `Android.Support.v4.Hardware.Fingerprint` 네임 스페이스에서 찾을 수 있으며, [Xamarin.Android.Support.v4 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)를 통해 배포됩니다.

[FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)(및 해당 지원 라이브러리 항목, [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html))는 지문 스캔 하드웨어를 사용하기 위한 기본 클래스입니다. 이 클래스는 하드웨어 자체와의 상호 작용을 관리하는 시스템 수준 서비스 주위의 Android SDK 래퍼입니다. 이는 지문 스캐너를 시작하고 스캐너에서 피드백에 응답하는 일을 담당합니다. 이 클래스에는 다음과 같은 세 개의 멤버만 있는 매우 간단한 인터페이스가 있습니다.

- **`Authenticate`** &ndash; 이 메서드는 하드웨어 스캐너를 초기화하고 백그라운드에서 서비스를 시작하여 사용자가 지문을 스캔할 때까지 대기합니다.
- **`EnrolledFingerprints`** &ndash; 사용자가 하나 이상의 지문을 디바이스에 등록한 경우 이 속성은 `true`를 반환합니다.
- **`HardwareDetected`** &ndash; 이 속성은 디바이스에서 지문 스캔을 지원하는지 여부를 확인하는 데 사용됩니다.

`FingerprintManager.Authenticate` 메서드는 Android 애플리케이션에서 지문 스캐너를 시작하는 데 사용됩니다. 다음 코드 조각은 지원 라이브러리 호환성 API를 사용하여 호출하는 방법의 예제입니다.

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

이 가이드에서는 `FingerprintManager` API를 사용하여 지문 인증으로 Android 애플리케이션을 개선하는 방법을 설명합니다. 지문 스캐너에서 결과를 보호하는 데 도움이 되는 `CryptoObject`를 인스턴스화하고 만드는 방법을 설명합니다. 애플리케이션이 `FingerprintManager.AuthenticationCallback`을 서브클래싱하고 지문 스캐너에서 피드백에 응답하는 방식을 살펴보겠습니다. 마지막으로 Android 디바이스 또는 에뮬레이터에서 지문을 등록하는 방법 및 **adb**를 사용하여 지문 스캔을 시뮬레이트하는 방법을 알아봅니다.

## <a name="requirements"></a>요구 사항

지문 인증에는 Android 6.0(API 수준 23) 이상 및 지문 스캐너가 있는 디바이스가 필요합니다. 

인증할 각 사용자의 디바이스에는 지문이 이미 등록되어 있어야 합니다. 여기에는 암호, PIN, 살짝 밀기 패턴 또는 얼굴 인식을 사용하는 화면 잠금을 설정하는 작업이 포함됩니다. Android Emulator에서 일부 지문 인증 기능을 시뮬레이트할 수 있습니다.  이러한 두 항목에 대한 자세한 내용은 [지문 등록](enrolling-fingerprint.md) 섹션을 참조하세요. 

## <a name="related-links"></a>관련 링크

- [지문 가이드 샘플 앱](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fingerprintguide)
- [지문 대화 상자 샘플](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [런타임에 권한 요청](https://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](https://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](xref:Android.Content.Context)
- [지문 및 지불 API(비디오)](https://youtu.be/VOn7VrTRlA4)
