---
title: 지문 검사
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/23/2016
ms.openlocfilehash: 372fe4c7844448e7fb3cbc768f16feb3a5cc7791
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671444"
---
# <a name="scanning-for-fingerprints"></a>지문 검사

지문 인증을 사용 하도록 Xamarin.Android 응용 프로그램을 준비 하는 방법을 살펴본 했으므로 예로 돌아가서,는 `FingerprintManager.Authenticate` 메서드를 Android 6.0 지문 인증에서 해당 위치에 논의 합니다. 워크플로의 지문 인증에 대 한 간략 한 개요는이 목록에 설명 되어 있습니다.

1. 호출 `FingerprintManager.Authenticate`전달 된 `CryptoObject` 및 `FingerprintManager.AuthenticationCallback` 인스턴스. `CryptoObject` 지문 인증 결과 사용 하 여 손상 되지 않았음을 확인 하는 데 사용 됩니다. 
2. 서브 클래스는 [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) 클래스입니다. 이 클래스의 인스턴스로 제공 될 `FingerprintManager` 때 인증 시작 지문입니다. 지문 스캐너를 완료 되 면이 클래스에서 콜백 메서드 중 하나를 호출 합니다.
3. 사용자가 장치 지문 스캐너를 시작한 사용자 상호 작용을 기다리고 있음을 알 수 있도록 UI를 업데이트 하는 코드를 작성 합니다. 
4. 지문 스캐너를 완료 되 면 Android는 결과 응용 프로그램에서 메서드를 호출 하 여는 `FingerprintManager.AuthenticationCallback` 이전 단계에서 제공 된 인스턴스.
5. 응용 프로그램 지문 인증 결과의 사용자를 적절 하 게 결과에 대응 합니다. 

다음 코드 조각은 다음과 같습니다. 지문 검사를 시작 하는 작업 메서드 예제

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

이러한 매개 변수의 각 살펴보겠습니다는 `Authenticate` 메서드를 조금 더 자세히:

* 첫 번째 매개 변수를 _crypto_ 지문 검색 결과 인증 하는 데 지문 스캐너를 사용 하 여 개체입니다. 이 개체 수 `null`, 지문 결과 변조 응용 프로그램은 무조건 신뢰 하지 않은 경우. 것이 좋습니다는 `CryptoObject` 인스턴스화되고에 제공 되는 `FingerprintManager` null이 아닌 합니다. [CryptObject 만들기](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) 인스턴스화하는 방법을 자세히 설명 합니다는 `CryptoObject` 기반을 `Cipher`입니다.
* 두 번째 매개 변수는 항상 0입니다. Android 설명서 플래그 집합으로이 식별 하며 나중에 사용 가능성이 예약 합니다. 
* 세 번째 매개 변수를 `cancellationSignal` 지문 스캐너를 해제 하 고 현재 요청을 취소 하는 데 사용 하는 개체입니다. 이 [Android CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html), 및.NET framework에서 형식이 아닙니다.
* 네 번째 매개 변수는 필수 이며 클래스 해당 서브 클래스는 `AuthenticationCallback` 추상 클래스입니다. 이 클래스의 메서드를 클라이언트에 알리기 위해 호출 될 때를 `FingerprintManager` 완료 란 무엇 이며 결과입니다. 구현에 대 한 이해 하는 합니다 `AuthenticationCallback`에서 다룰 [것이 고유한 섹션](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md).
* 다섯 번째 매개 변수는 선택적 `Handler` 인스턴스. 경우는 `Handler` 개체를 제공 합니다 `FingerprintManager` 사용할지는 `Looper` 지문 하드웨어에서 메시지를 처리할 때 해당 개체에서. 일반적으로 하나 않아도 제공을 `Handler`를 FingerprintManager 사용할지는 `Looper` 응용 프로그램에서.

## <a name="cancelling-a-fingerprint-scan"></a>지문 검색 취소

사용자 (또는 응용 프로그램)에 시작 된 후 지문 검색을 취소 해야 할 수도 있습니다. 이 이런 경우에 호출을 [ `IsCancelled` ](https://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) 메서드를 [ `CancellationSignal` ](https://developer.android.com/reference/android/os/CancellationSignal.html) 는 제공 된 `FingerprintManager.Authenticate` 지문 검사를 시작 하려면 호출 된 경우.

살펴본 했으므로 `Authenticate` 메서드를 일부 더 중요 한 매개 변수를 자세히 살펴보겠습니다. 먼저 살펴보겠습니다 [인증 콜백에 응답](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)를 논의 하는 서브 클래스 하는 방법을 [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html), 대응 하는 데 Android 응용 프로그램을 사용 하도록 설정 합니다 지문 스캐너를 제공한 결과입니다.




## <a name="related-links"></a>관련 링크

- [CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
