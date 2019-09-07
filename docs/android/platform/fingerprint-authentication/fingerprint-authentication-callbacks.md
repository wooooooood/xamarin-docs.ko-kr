---
title: 인증 콜백에 응답
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: 57e6ed2c01e382d7daee2933ac49c8282199a3fc
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758860"
---
# <a name="responding-to-authentication-callbacks"></a>인증 콜백에 응답

지문 스캐너는 자체 스레드에서 백그라운드에서 실행 되 고, 완료 되 면 UI 스레드에서의 `FingerprintManager.AuthenticationCallback` 메서드 하나를 호출 하 여 검사 결과를 보고 합니다. Android 응용 프로그램은 다음의 모든 메서드를 구현 하 여이 추상 클래스를 확장 하는 자체 처리기를 제공 해야 합니다.

- **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; 복구할 수 없는 오류가 발생 하는 경우 호출 됩니다. 응용 프로그램 또는 사용자가 상황을 해결 하기 위해 수행할 수 있는 작업은 없습니다. 단, 다시 시도할 수 있습니다.
- **`OnAuthenticationFailed()`** &ndash; 이 메서드는 지문이 검색 되었지만 장치에서 인식 되지 않을 때 호출 됩니다.
- **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; 스캐너를 통해 스와이프 하는 핑거와 같이 복구할 수 있는 오류가 있을 때 호출 됩니다.
- **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; 지문이 인식 될 때 호출 됩니다.

를 `CryptoObject` 호출할 `Authenticate`때가 사용 된 경우에서 `OnAuthenticationSuccessful`를 호출 `Cipher.DoFinal` 하는 것이 좋습니다.
`DoFinal`는 암호가 변조 되었거나 잘못 초기화 된 경우에 예외를 throw 합니다 .이 경우 지문 스캐너의 결과가 응용 프로그램 외부에서 변조 되었을 수 있습니다.

> [!NOTE]
> 콜백 클래스를 비교적 경량으로 유지 하 고 응용 프로그램 관련 논리를 사용 하지 않는 것이 좋습니다. 콜백은 Android 응용 프로그램과 지문 스캐너의 결과 간에 "트래픽 복사"로 작동 해야 합니다.

## <a name="a-sample-authentication-callback-handler"></a>샘플 인증 콜백 처리기

다음 클래스는 최소 `FingerprintManager.AuthenticationCallback` 구현에 대 한 예입니다. 

```csharp
class MyAuthCallbackSample : FingerprintManagerCompat.AuthenticationCallback
{
    // Can be any byte array, keep unique to application.
    static readonly byte[] SECRET_BYTES = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    // The TAG can be any string, this one is for demonstration.
    static readonly string TAG = "X:" + typeof (SimpleAuthCallbacks).Name;

    public MyAuthCallbackSample()
    {
    }

    public override void OnAuthenticationSucceeded(FingerprintManagerCompat.AuthenticationResult result)
    {
        if (result.CryptoObject.Cipher != null) 
        {
            try
            {
                // Calling DoFinal on the Cipher ensures that the encryption worked.
                byte[] doFinalResult = result.CryptoObject.Cipher.DoFinal(SECRET_BYTES);
    
                // No errors occurred, trust the results.              
            }
            catch (BadPaddingException bpe)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + bpe);
            }
            catch (IllegalBlockSizeException ibse)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + ibse);
            }
        }
        else
        {
            // No cipher used, assume that everything went well and trust the results.
        }
    }

    public override void OnAuthenticationError(int errMsgId, ICharSequence errString)
    {
        // Report the error to the user. Note that if the user canceled the scan,
        // this method will be called and the errMsgId will be FingerprintState.ErrorCanceled.
    }

    public override void OnAuthenticationFailed()
    {
        // Tell the user that the fingerprint was not recognized.
    }

    public override void OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)
    {
        // Notify the user that the scan failed and display the provided hint.
    }
}
```

`OnAuthenticationSucceeded`가 호출 될 때 `FingerprintManager` `Authentication` 가에 제공 되었는지 확인 합니다. `Cipher` 그럴 경우 암호화에 `DoFinal` 대해 메서드가 호출 됩니다. 그러면이 닫히고 `Cipher`원래 상태로 복원 됩니다. 암호화에 문제가 발생 하는 경우에서 `DoFinal` 예외를 throw 하 고 인증 시도를 실패 한 것으로 간주 해야 합니다.

`OnAuthenticationError` 및`OnAuthenticationHelp` 콜백은 각각 문제의 원인을 나타내는 정수를 수신 합니다. 다음 섹션에서는 각 가능한 도움말 또는 오류 코드에 대해 설명 합니다. 두 콜백은 지문 인증이 실패 했음을 &ndash; 응용 프로그램에 알리기 위해 비슷한 용도로 사용 됩니다. 차이점은 심각도입니다. `OnAuthenticationHelp`사용자가 복구할 수 없는 오류 (예: 지문을 너무 빨리 살짝 밀기) `OnAuthenticationError` 손상 된 지문 스캐너와 같은 심각한 오류가 발생 합니다.

는 `OnAuthenticationError` 메시지를 `CancellationSignal.Cancel()` 통해 지문 검사가 취소 될 때 호출 됩니다. 매개 변수의 값은 5 (`FingerprintState.ErrorCanceled`)입니다. `errMsgId` 요구 사항에 따라의 `AuthenticationCallbacks` 구현에서이 상황을 다른 오류와 다르게 처리할 수 있습니다. 

`OnAuthenticationFailed`는 지문을 성공적으로 스캔 했지만 장치에 등록 된 지문과 일치 하지 않을 때 호출 됩니다. 

## <a name="help-codes-and-error-message-ids"></a>도움말 코드 및 오류 메시지 Id 

FingerprintManager 클래스에 대 한 [Android SDK 설명서](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) 에는 오류 코드와 도움말 코드의 목록과 설명이 나와 있습니다. Xamarin.ios는 `Android.Hardware.Fingerprints.FingerprintState` 열거형을 사용 하 여 이러한 값을 나타냅니다.

- **`AcquiredGood`** &ndash; (값 0) 얻은 이미지가 양호 합니다.

- **`AcquiredImagerDirty`** &ndash; (값 3) 센서에서 의심 되거나 검색 된 것으로 인해 지문 이미지가 너무 소음이 발생 했습니다. 예를 들어 센서 (걸린 픽셀, swaths 등) `AcquiredInsufficient` 에 대 한 여러 또는 실제 검색 후이를 반환 하는 것이 합리적입니다. 사용자는이 반환 될 때 센서를 정리 하기 위한 조치를 취해야 합니다.

- **`AcquiredInsufficient`** (값 2) 지문 이미지는 검색 된 조건 (예: 마른 스킨) 또는 변경 가능한 센서 `AcquiredImagerDirty`(예: &ndash;

- **`AcquiredPartial`** &ndash; (값 1) 부분 지문 이미지만 검색 되었습니다. 등록 하는 동안 사용자에 게이 문제를 해결 하기 위해 수행 해야 하는 사항에 대 한 &ldquo;정보가 있어야 합니다. 예를 들어, 센서를 힘껏 누릅니다.&rdquo;

- **`AcquiredTooFast`** &ndash; (값 5) 빠른 동작으로 인해 지문 이미지가 불완전 합니다. 선형 배열 센서에는 대부분 적절 하지만, 인수를 획득 하는 동안 손가락을 이동한 경우에도이 문제가 발생할 수 있습니다. 사용자에 게 손가락을 느리게 이동 하거나 (선형), 계속 해 서 센서에서 손가락을 이동 하도록 요청 해야 합니다.

- **`AcquiredToSlow`** &ndash; (값 4) 동작 부족으로 인해 지문 이미지를 읽을 수 없습니다. 이는 살짝 밀기 동작이 필요한 선형 배열 센서에 가장 적합 합니다.

- **`ErrorCanceled`** &ndash; (값 5) 지문 센서를 사용할 수 없기 때문에 작업이 취소 되었습니다. 예를 들어 사용자가 전환 되거나 장치가 잠겨 있거나 다른 보류 중인 작업에서이를 방지 하거나 사용 하지 않도록 설정 하는 경우이 문제가 발생할 수 있습니다.

- **`ErrorHwUnavailable`** &ndash; (값 1) 하드웨어를 사용할 수 없습니다. 나중에 다시 시도하십시오.

- **`ErrorLockout`** &ndash; (값 7) 너무 많은 시도로 인해 API가 잠기기 때문에 작업이 취소 되었습니다.

- **`ErrorNoSpace`** &ndash; (값 4) 등록 등의 작업에 대해 오류 상태가 반환 되었습니다. 작업을 완료 하기 위해 남은 저장소 공간이 부족 하 여 작업을 완료할 수 없습니다.

- **`ErrorTimeout`** &ndash; (값 3) 현재 요청이 너무 오래 실행 되었을 때 오류 상태가 반환 됩니다. 이는 프로그램에서 지문 센서를 무기한 대기 하지 않도록 하기 위한 것입니다. 제한 시간은 플랫폼 및 센서 전용 이지만 일반적으로 30 초입니다.

- **`ErrorUnableToProcess`** &ndash; (값 2) 센서가 현재 이미지를 처리할 수 없는 경우 오류 상태가 반환 됩니다.

## <a name="related-links"></a>관련 링크

- [암호](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
