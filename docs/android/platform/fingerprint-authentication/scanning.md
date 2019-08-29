---
title: 지문을 검색 하는 중
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/23/2016
ms.openlocfilehash: 15afd5b1812e0423097e889cd8c2558ca01a8074
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119749"
---
# <a name="scanning-for-fingerprints"></a>지문을 검색 하는 중

이제 Xamarin android 응용 프로그램을 준비 하 여 지문 인증을 사용 하는 방법을 살펴보았으므로 이제 `FingerprintManager.Authenticate` 메서드로 돌아가서 Android 6.0 지문 인증에서 그 자리에 대해 논의 하겠습니다. 지문 인증에 대 한 워크플로에 대 한 간략 한 개요는 다음 목록에서 설명 합니다.

1. `CryptoObject` `FingerprintManager.Authenticate` 및`FingerprintManager.AuthenticationCallback` 인스턴스를 전달 하 여를 호출 합니다. 는 `CryptoObject` 지문 인증 결과가 변조 되지 않았는지 확인 하는 데 사용 됩니다. 
2. [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) 클래스의 하위 클래스입니다. 이 클래스의 인스턴스는 지문 인증이 시작 `FingerprintManager` 될 때 제공 됩니다. 지문 스캐너가 완료 되 면이 클래스의 콜백 메서드 중 하나를 호출 합니다.
3. UI를 업데이트 하는 코드를 작성 하 여 장치에서 지문 스캐너가 시작 되 고 사용자 조작이 대기 중임을 사용자에 게 알립니다. 
4. 지문 스캐너가 완료 되 면 Android는 이전 단계에서 제공 된 `FingerprintManager.AuthenticationCallback` 인스턴스에 대해 메서드를 호출 하 여 응용 프로그램에 결과를 반환 합니다.
5. 응용 프로그램은 사용자에 게 지문 인증 결과를 알리고 적절 한 결과에 반응 하 게 됩니다. 

다음 코드 조각은 작업에서 지문을 검색 하기 시작 하는 메서드의 예입니다.

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

- 첫 번째 매개 변수는 지문 스캔 결과를 인증 하는 데 도움이 되는 지문 스캐너가 사용할 _암호화_ 개체입니다. 이 개체는 일 `null`수 있습니다 .이 경우 응용 프로그램은 지문 결과와 훼손 되지 않은 것을 무조건 신뢰 해야 합니다. 을 `CryptoObject` 인스턴스화하고 null이 아닌에 제공 `FingerprintManager` 하는 것이 좋습니다. [Cryptobject를 만들면](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) 에 기반 `CryptoObject` `Cipher`하 여를 인스턴스화하는 방법에 대해 자세히 설명 합니다.
- 두 번째 매개 변수는 항상 0입니다. Android 설명서는이를 플래그 집합으로 식별 하 고 나중에 사용 하도록 예약 되어 있을 가능성이 높습니다. 
- 세 번째 매개 변수 `cancellationSignal` 는 지문 스캐너를 끄고 현재 요청을 취소 하는 데 사용 되는 개체입니다. 이는 .NET framework의 형식이 아닌 [Android CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html).
- 네 번째 매개 변수는 필수 이며 `AuthenticationCallback` 추상 클래스를 서브클래싱하는 클래스입니다. 이 클래스의 메서드는 `FingerprintManager` 가 완료 되 고 결과가 무엇 인지를 클라이언트에 알리기 위해 호출 됩니다. 을 구현 `AuthenticationCallback`하는 방법에 대 한 이해를 돕기 위해 많은 내용이 있으므로 [자체 섹션](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)에서 설명 합니다.
- 다섯 번째 매개 변수는 선택적 `Handler` 인스턴스입니다. 개체를 제공 하는 경우는 `FingerprintManager` 지문 하드웨어에서 `Looper` 메시지를 처리할 때 해당 개체의를 사용 합니다. `Handler` 일반적으로는 `Handler`를 제공 하지 않아도 되며, FingerprintManager는 응용 프로그램에서를 `Looper` 사용 합니다.

## <a name="cancelling-a-fingerprint-scan"></a>지문 검사 취소

사용자 (또는 응용 프로그램)가 시작 된 후 지문 검색을 취소 해야 할 수도 있습니다. 이 경우 지문 검사를 시작 [`IsCancelled`](https://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) 하기 위해 호출 [`CancellationSignal`](https://developer.android.com/reference/android/os/CancellationSignal.html) 될 때에 `FingerprintManager.Authenticate` 대해 제공 된에서 메서드를 호출 합니다.

이제 `Authenticate` 메서드를 살펴보았으므로 이제 더 중요 한 매개 변수 중 일부를 좀 더 자세히 살펴보겠습니다. 먼저 [인증 콜백에 응답](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)하 여 Android 응용 프로그램이 지문 스캐너에서 제공 하는 결과에 반응할 수 있도록 [FingerprintManager 콜백을](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)서브 클래스 하는 방법을 설명 합니다.




## <a name="related-links"></a>관련 링크

- [CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
