---
title: "지문 검색"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/23/2016
ms.openlocfilehash: 678ceaf122550c6561541533405fe3500d192110
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="scanning-for-fingerprints"></a>지문 검색

지문 인증을 사용 하도록 Xamarin.Android 응용 프로그램을 준비 하는 방법을 앞서 했으므로으로 돌아가겠습니다는 `FingerprintManager.Authenticate` 메서드를 Android 6.0 지문 인증에서 해당 위치에 논의 합니다. 워크플로의 지문 인증에 대 한 간략 한 개요는이 목록에 설명 되어 있습니다.

1. 호출 `FingerprintManager.Authenticate`전달 하는 `CryptoObject` 및 `FingerprintManager.AuthenticationCallback` 인스턴스. `CryptoObject` 지문 인증 결과 변조 되지 않도록 하는 데 사용 됩니다. 
2. 하위 클래스는 [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) 클래스입니다. 이 클래스의 인스턴스를 위해 제공 됩니다 `FingerprintManager` 인증 시작 될 때 지문입니다. 지문 스캐너 완료 되 면이 클래스에 콜백 메서드 중 하나를 호출 합니다.
3. 사용자가 장치 지문 검색 프로그램을 시작 하 고 사용자 상호 작용에 대 한 대기를 알 수 있도록 UI를 업데이트 하는 코드를 작성 합니다. 
4. 지문 스캐너 수행 되는 경우 Android 돌아갑니다 결과 응용 프로그램에는 메서드를 호출 하 여는 `FingerprintManager.AuthenticationCallback` 이전 단계에서 제공 된 인스턴스.
5. 응용 프로그램 지문 인증 결과 사용자에 게 알리는 적절 하 게 결과에 대응 합니다. 

다음 코드 조각은 지문에 대 한 검사를 시작 하는 활동에 있는 메서드의 한 예입니다.

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

이러한 매개 변수를 각각 살펴보겠습니다는 `Authenticate` 좀 더 자세히 메서드:

* 첫 번째 매개 변수는 _crypto_ 개체 지문 스캐너 지문 검색 결과 인증 하는 데 사용 합니다. 이 개체 `null`, 지문 결과 변조 무조건 아무것도 신뢰 응용 프로그램은이 경우. 것이 좋습니다는 `CryptoObject` 인스턴스화되고에 제공 되는 `FingerprintManager` null 대신 합니다. [CryptObject 만들기](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) 를 인스턴스화하는 방법을 자세히 설명 합니다는 `CryptoObject` 기반으로 한 `Cipher`합니다.
* 두 번째 매개 변수는 항상 0입니다. Android 설명서 플래그 집합으로이 식별 하며 나중에 사용 가능성이 예약 합니다. 
* 세 번째 매개 변수 `cancellationSignal` 지문 스캐너를 해제 하 고 현재 요청을 취소 하는 데 사용 하는 개체입니다. 이 한 [Android CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html),.NET framework에서 형식이 아닌 합니다.
* 네 번째 매개 변수는 필수 이며 해당 서브 클래스는 클래스는 `AuthenticationCallback` 추상 클래스입니다. 클라이언트에 신호를 보내이 클래스에 대 한 메서드 호출 될 때는 `FingerprintManager` 완료 하 고, 결과입니다. 구현에 대 한 이해 하는 많은 그대로 `AuthenticationCallback`에서 설명 합니다. [자체 섹션은](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)합니다.
* 다섯 번째 매개 변수는 선택적 `Handler` 인스턴스. 경우는 `Handler` 개체가 제공 되는 `FingerprintManager` ´ ֲ는 `Looper` 지문 하드웨어에서 메시지를 처리할 때 해당 개체에서 합니다. 일반적으로 하나 하지 않아도 제공는 `Handler`는 FingerprintManager ´ ֲ는 `Looper` 응용 프로그램에서 합니다.

## <a name="cancelling-a-fingerprint-scan"></a>지문 검색을 취소합니다.

시작 된 후 지문 검색을 취소 하는 사용자 (또는 응용 프로그램)에 대 한 할 수 있습니다. 이러한 상황에서 호출는 [ `IsCancelled` ](http://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) 메서드를는 [ `CancellationSignal` ](http://developer.android.com/reference/android/os/CancellationSignal.html) 에 게 제공한 `FingerprintManager.Authenticate` 지문 검사를 시작 하려면 호출 되었습니다.

관찰 한 했으므로 `Authenticate` 메서드를 자세히 더 중요 한 매개 변수 중 일부에 대해 살펴보겠습니다. 살펴보게 먼저 [인증 콜백에 응답](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md), 있는 설명 합니다 서브 클래스 하는 방법의 [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)에 반응 하는 Android 응용 프로그램을 사용 하도록 설정는 지문 검색 프로그램에서 제공 되는 결과입니다.




## <a name="related-links"></a>관련 링크

- [CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
