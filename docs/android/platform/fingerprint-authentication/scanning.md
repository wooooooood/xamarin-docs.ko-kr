---
title: 지문 스캔
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/23/2016
ms.openlocfilehash: 61edd0e4b532f18a8fc28502e5bb990703068776
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027487"
---
# <a name="scanning-for-fingerprints"></a>지문 스캔

지문 인증을 사용하도록 Xamarin.Android 애플리케이션을 준비하는 방법을 살펴보았으므로 이제 `FingerprintManager.Authenticate` 메서드로 돌아가서 Android 6.0 지문 인증에서 이 메서드의 위치에 대해 논의하겠습니다. 지문 인증의 워크플로에 대한 간략한 개요는 다음 목록에서 설명합니다.

1. `FingerprintManager.Authenticate`를 호출하고 `CryptoObject` 및 `FingerprintManager.AuthenticationCallback` 인스턴스를 전달합니다. `CryptoObject`는 지문 인증 결과가 변조되지 않았음을 확인하는 데 사용됩니다. 
2. [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) 클래스를 서브클래스화합니다. 이 클래스의 인스턴스는 지문 인증이 시작될 때 `FingerprintManager`에 제공됩니다. 지문 스캐너가 완료되면 이 클래스의 콜백 메서드 중 하나를 호출합니다.
3. 디바이스에서 지문 스캐너를 시작했고 사용자 상호 작용을 기다리는 중임을 사용자에게 알리도록 UI를 업데이트하는 코드를 작성합니다. 
4. 지문 스캐너가 완료되면 Android는 이전 단계에서 제공된 `FingerprintManager.AuthenticationCallback` 인스턴스에서 메서드를 호출하여 애플리케이션에 결과를 반환합니다.
5. 애플리케이션은 사용자에게 지문 인증 결과를 알리고 결과에 적절히 반응합니다. 

다음 코드 조각은 작업에서 지문을 스캔하기 시작하는 메서드의 예입니다.

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

`Authenticate` 메서드의 각 매개 변수를 좀 더 자세히 살펴보겠습니다.

- 첫 번째 매개 변수는 지문 스캐너가 지문 스캔 결과를 인증하는 데 사용할 _crypto_ 개체입니다. 이 개체는 `null`일 수 있으며, 이 경우 애플리케이션은 지문 결과가 위조되지 않았음을 무조건 신뢰해야 합니다. `CryptoObject`를 인스턴스화하고 null이 아닌 `FingerprintManager`에 제공하는 것이 좋습니다. [CryptObject 만들기](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md)에서는 `Cipher`를 기반으로 `CryptoObject`를 인스턴스화하는 방법에 대해 자세히 설명합니다.
- 두 번째 매개 변수는 항상 0입니다. Android 설명서에서는 이를 플래그 세트로 식별하며 나중에 사용하도록 예약되어 있을 가능성이 높습니다. 
- 세 번째 매개 변수인 `cancellationSignal`은 지문 스캐너를 끄고 현재 요청을 취소하는 데 사용되는 개체입니다. 이는 [Android CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)이며 .NET Framework의 형식이 아닙니다.
- 네 번째 매개 변수는 필수이며 `AuthenticationCallback` 추상 클래스를 서브클래스화하는 클래스입니다. 이 클래스의 메서드는 `FingerprintManager`가 완료되면 결과가 무엇인지 클라이언트에 알리기 위해 호출됩니다. `AuthenticationCallback` 구현을 이해하려면 자세한 설명이 필요하므로 [단독 섹션](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)에서 설명합니다.
- 다섯 번째 매개 변수는 선택적 `Handler` 인스턴스입니다. `Handler` 개체가 제공되는 경우 `FingerprintManager`는 지문 하드웨어에서 메시지를 처리할 때 해당 개체에서 `Looper`를 사용합니다. 일반적으로 `Handler`는 제공할 필요가 없으며, FingerprintManager는 애플리케이션에서 `Looper`를 사용합니다.

## <a name="cancelling-a-fingerprint-scan"></a>지문 스캔 취소

사용자(또는 애플리케이션)가 이미 시작된 지문 스캔을 취소해야 하는 경우가 있을 수 있습니다. 이 경우 지문 스캔을 시작하기 위해 호출되었을 때 `FingerprintManager.Authenticate`에 제공된 [`CancellationSignal`](https://developer.android.com/reference/android/os/CancellationSignal.html)에서 [`IsCancelled`](https://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) 메서드를 호출합니다.

`Authenticate` 메서드를 살펴보았으므로 이제 더 중요한 몇 가지 매개 변수를 좀 더 자세히 살펴보겠습니다. Android 애플리케이션이 지문 스캐너에서 제공한 결과에 응답할 수 있게 해주는 [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)을 서브클래스화하는 방법을 설명하는 [인증 콜백에 응답](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)을 살펴보겠습니다.

## <a name="related-links"></a>관련 링크

- [CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
