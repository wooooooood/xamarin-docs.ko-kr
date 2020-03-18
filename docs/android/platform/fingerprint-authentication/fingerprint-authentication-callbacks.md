---
title: 인증 콜백에 응답
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: 8dc06740355bd95828e1a1bd8d9d15a2ef37e6b2
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027529"
---
# <a name="responding-to-authentication-callbacks"></a>인증 콜백에 응답

지문 스캐너는 자체 스레드로 백그라운드에서 실행됩니다. 실행이 완료되면 UI 스레드에서 `FingerprintManager.AuthenticationCallback`의 한 메서드를 호출하여 스캔 결과를 보고합니다. Android 애플리케이션은 이 추상 클래스를 확장하는 고유 처리기를 제공하여, 다음과 같은 메서드를 모두 구현해야 합니다.

- **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; 복구할 수 없는 오류가 있을 때 호출됩니다. 애플리케이션 또는 사용자가 재시도 외에는 상황을 해결하기 위해 할 수 있는 것이 없습니다.
- **`OnAuthenticationFailed()`** &ndash; 이 메서드는 지문이 검색되었지만 디바이스에서 인식되지 않을 때 호출됩니다.
- **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; 스캐너 위에서 손가락을 너무 빨리 움직였을 때와 같이 복구할 수 없는 오류가 있을 때 호출됩니다.
- **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; 지문이 인식되었을 때 호출됩니다.

`Authenticate`를 호출할 때 `CryptoObject`가 사용되었으면 `OnAuthenticationSuccessful`에서 `Cipher.DoFinal`을 호출하는 것이 좋습니다.
`DoFinal`은 암호화가 손상되었거나 부적절하게 초기화된 경우, 지문 스캐너의 결과가 애플리케이션 외부에서 손상되었을 수 있음을 나타내는 예외를 발생시킵니다.

> [!NOTE]
> 콜백 클래스는 비교적 가볍게 그리고 애플리케이션 특정 논리로부터 자유롭게 유지하는 것이 좋습니다. 콜백은 Android 애플리케이션과 지문 스캐너의 결과 사이에 "교통 경찰"과 같은 역할을 수행합니다.

## <a name="a-sample-authentication-callback-handler"></a>샘플 인증 콜백 처리기

다음 클래스는 최소한의 `FingerprintManager.AuthenticationCallback` 구현에 대한 예입니다. 

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

`OnAuthenticationSucceeded`는 `Authentication`이 호출되었을 때 `FingerprintManager`에 `Cipher`가 제공되었는지 확인합니다. 그렇다면 암호화에서 `DoFinal` 메서드가 호출됩니다. 그러면 `Cipher`가 닫히고 원래 상태로 복원됩니다. 암호화에 문제가 있으면 `DoFinal`이 예외를 throw하고 인증 시도가 실패한 것으로 간주됩니다.

`OnAuthenticationError` 및 `OnAuthenticationHelp` 콜백에는 각각 무엇이 문제인지를 나타내는 정수가 수신됩니다. 다음 섹션에서는 가능한 각 도움말 또는 오류 코드를 설명합니다. 두 콜백은 지문 인증이 실패했음을 애플리케이션에 알리는 비슷한 역할을 수행합니다. 차이점은 심각도에 있습니다. `OnAuthenticationHelp`는 지문을 너무 빨리 대는 경우와 같이 사용자가 복구할 수 있는 오류이고, `OnAuthenticationError`는 지문 스캐너 손상과 같은 보다 심각한 오류입니다.

`CancellationSignal.Cancel()` 메시지를 통해 지문 스캔이 취소되면 `OnAuthenticationError`가 호출됩니다. `errMsgId` 매개 변수에는 값 5(`FingerprintState.ErrorCanceled`)가 포함됩니다. 요구 사항에 따라 `AuthenticationCallbacks` 구현에서는 이 상황을 다른 오류들과 다르게 처리할 수 있습니다. 

지문이 성공적으로 스캔되었지만 디바이스에 등록된 지문과 일치하지 않으면 `OnAuthenticationFailed`가 호출됩니다. 

## <a name="help-codes-and-error-message-ids"></a>도움말 코드 및 오류 메시지 ID 

오류 코드 및 도움말 코드의 목록 및 설명은 FingerprintManager 클래스에 대한 [Android SDK 설명서](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD)에서 찾을 수 있습니다. Xamarin.Android는 이러한 값을 `Android.Hardware.Fingerprints.FingerprintState` 열거형으로 나타냅니다.

- **`AcquiredGood`** &ndash;(값 0) 가져온 이미지가 양호합니다.

- **`AcquiredImagerDirty`** &ndash;(값 3) 센서 먼지 감지 또는 의심에 따라 지문 이미지에 노이즈가 너무 많습니다. `AcquiredInsufficient`가 여러 번 발생하거나 센서에서 실제로 먼지가 감지된 다음에는(뭉친 픽셀, 띠 등)이를 반환하는 것이 합리적입니다. 사용자는 이 코드가 반환되었을 때 센서를 깨끗이 하기 위한 조치를 취해야 합니다.

- **`AcquiredInsufficient`** &ndash;(값 2) 감지된 조건(예: 건조한 피부) 또는 오염되었을 수 있는 센서로 인해 노이즈가 너무 많아서 지문 이미지를 처리할 수 없습니다(`AcquiredImagerDirty` 참조).

- **`AcquiredPartial`** &ndash;(값 1) 지문 이미지가 일부만 검색되었습니다. 등록 중 이 문제를 해결하기 위해 어떤 조치가 필요한지 사용자에게 알려야 합니다(예: &ldquo;센서를 확실하게 누르기&rdquo;).

- **`AcquiredTooFast`** &ndash;(값 5) 동작이 너무 빨라서 지문 이미지가 불완전합니다. 선형 배열 센서의 경우 대부분 적합하지만, 데이터 획득 중 손가락이 이동한 경우에도 이 조건이 발생할 수 있습니다. 사용자에게 손가락을 천천히 움직이거나(선형) 센서에 손가락을 더 길게 남기도록 요청해야 합니다.

- **`AcquiredToSlow`** &ndash;(값 4) 움직임이 부족하여 지문 이미지를 읽을 수 없습니다. 살짝 밀기 동작이 필요한 선형 배열 센서에 가장 적합합니다.

- **`ErrorCanceled`** &ndash;(값 5) 지문 센서를 사용할 수 없어서 작업이 취소되었습니다. 예를 들어 사용자가 전환되었거나 디바이스가 잠겼거나, 다른 보류 중인 작업으로 인해 기능이 방해되거나 사용하지 않도록 설정되었을 때 발생할 수 있습니다.

- **`ErrorHwUnavailable`** &ndash;(값 1) 하드웨어를 사용할 수 없습니다. 나중에 다시 시도하십시오.

- **`ErrorLockout`** &ndash;(값 7) 시도 횟수가 너무 많아서 API가 잠겼기 때문에 작업이 취소되었습니다.

- **`ErrorNoSpace`** &ndash;(값 4) 등록과 같은 작업에 대해 반환된 오류 상태입니다. 작업을 완료하기 위해 남은 스토리지가 충분하지 않기 때문에 작업을 완료할 수 없습니다.

- **`ErrorTimeout`** &ndash;(값 3) 현재 요청이 너무 오류 실행되었을 때 반환되는 오류 상태입니다. 이것은 프로그램이 지문 센서를 무한정 기다리지 않도록 방지하기 위한 것입니다. 시간 제한은 플랫폼 및 센서마다 다르지만 일반적으로 약 30초입니다.

- **`ErrorUnableToProcess`** &ndash;(값 2) 센서가 현재 이미지를 처리할 수 없을 때 반환되는 오류 상태입니다.

## <a name="related-links"></a>관련 링크

- [암호화](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
