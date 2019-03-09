---
title: 인증 콜백에 응답
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: c720a30a59eea8f1ed74033da8d1c045a1fb9109
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57666868"
---
# <a name="responding-to-authentication-callbacks"></a>인증 콜백에 응답

지문 스캐너를 자체 스레드에서 백그라운드로 실행 되 고 방법 중 하나를 호출 하 여 검색의 결과 보고 완료 되 면 `FingerprintManager.AuthenticationCallback` UI 스레드에서 합니다. Android 응용 프로그램을 모든 다음 메서드를 구현 하는이 추상 클래스를 확장 하는 고유한 처리기를 제공 해야 합니다.

* **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; 복구할 수 없는 오류가 있을 때 호출 됩니다. 더 많은 응용 프로그램이 나 사용자는 다시 시도 수 있는 점을 제외 하 고 문제를 해결 하기 위해 수행할 수 없습니다.
* **`OnAuthenticationFailed()`** &ndash; 이 메서드는 지문 검색 되었지만 장치에서 인식할 수 없는 경우에 호출 됩니다.
* **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; 스캐너를 통해 빠른를 누르거나 살짝 밀거나 되 손가락 등을 복구할 수 있는 오류가 있을 때 호출 됩니다.
* **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; 이 지문 인식 된 경우 호출 됩니다.

경우는 `CryptoObject` 호출할 때 사용한 `Authenticate`를 호출 하는 것이 좋습니다. `Cipher.DoFinal` 에서 `OnAuthenticationSuccessful`합니다.
`DoFinal` 지문 스캐너를 결과 손상 되었을 수 있는 응용 프로그램 외부에서 나타내는, 암호화 손상 되었거나 잘못 초기화 하는 경우 예외가 throw 됩니다.


> [!NOTE]
> 콜백 클래스 양이 비교적 적은 가중치를 유지 하 고 응용 프로그램별 논리의 해제 하는 것이 좋습니다. 콜백을 "교통 경찰" Android 응용 프로그램 및 결과 간의 지문 스캐너에서으로 작동 해야 합니다.

## <a name="a-sample-authentication-callback-handler"></a>샘플 인증 콜백 처리기

다음 클래스는 최소한의 예가 `FingerprintManager.AuthenticationCallback` 구현 합니다. 

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

`OnAuthenticationSucceeded` 확인 하는 `Cipher` 제공한 `FingerprintManager` 때 `Authentication` 호출 되었습니다. 그렇다면는 `DoFinal` 암호화 메서드를 호출 합니다. 프로그램이 종료는 `Cipher`를 원래 상태로 복원 합니다. 경우는 문제가 암호화를 사용 하 여 다음 `DoFinal` 예외를 throw 하 고 인증 시도가 실패 한 것으로 간주 됩니다.

합니다 `OnAuthenticationError` 및 `OnAuthenticationHelp` 콜백을 각 문제를 나타내는 정수를 수신 합니다. 다음 섹션에서는 각 가능한 도움말 또는 오류 코드를 설명합니다. 두 개의 콜백이 비슷한 용도로 &ndash; 알리는 응용 프로그램이 해당 지문 인증에 실패 했습니다. 어떻게 다른 지 심각도입니다. `OnAuthenticationHelp` 너무 빠릅니다; 지문이 살짝 같은 사용자 복구할 수 있는 오류 `OnAuthenticationError` 손상 된 지문 스캐너와 같은 심각한 오류가 더 있습니다.

유의 `OnAuthenticationError` 지문 검색을 통해 취소 되 면 호출 되는 `CancellationSignal.Cancel()` 메시지. 합니다 `errMsgId` 매개 변수 5 값이 포함 됩니다 (`FingerprintState.ErrorCanceled`). 구현 요구 사항에 따라는 `AuthenticationCallbacks` 이 이런 다른 오류를 다르게 처리할 수 있습니다. 

`OnAuthenticationFailed` 지문을 성공적으로 검색 한 장치를 사용 하 여 등록 된 모든 지문이 일치 하지 않는 경우 호출 됩니다. 

## <a name="help-codes-and-error-message-ids"></a>코드 및 오류 메시지 Id 

목록 및 오류 코드 및 도움말 코드 설명에서 확인할 수 있습니다 합니다 [Android SDK 설명서](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) FingerprintManager 클래스에 대 한 합니다. Xamarin.Android는 이러한 값을 나타내는 `Android.Hardware.Fingerprints.FingerprintState` 열거형:


-   **`AcquiredGood`** &ndash; (값 0) 이미지 획득 되었을 것입니다.


-   **`AcquiredImagerDirty`** &ndash; (값 3) 지문 이미지 센서에 더 트 바이크 의심 되는 또는 검색으로 인해 너무 소음이 많은 했습니다. 예를 들어 여러 후이 반환 하는 `AcquiredInsufficient` 또는 실제 센서 (예: 고정된 된 픽셀, swaths)에 더 트 바이크 검색 합니다. 사용자는이 반환 되 면 센서를 정리 하는 작업을 수행 해야 합니다.


-   **`AcquiredInsufficient`** &ndash; (값 2) 지문 이미지 검색된 조건 (예: dry 스킨) 또는 더티 수 있는 센서로 인해 처리 하는 데 너무 소음이 많은 되었습니다 (참조 `AcquiredImagerDirty`합니다.



-   **`AcquiredPartial`** &ndash; (값 1) 부분 지문 이미지만 검색 되었습니다. 등록 중 사용자에 예를 들어이 문제를 해결 하려면 수행 해야 할 항목에 전달 되어야 &ldquo;단단히 센서에서 키를 누릅니다.&rdquo;



-   **`AcquiredTooFast`** &ndash; (값 5) 지문 이미지 빠른 동작으로 인해 완료 되지 않았습니다. 선형 배열 센서에 대 한 적절 한 대부분을 하는 동안이 때도 발생할 수 있습니다 획득 하는 동안 손가락 이동 되었습니다. 사용자에서 느린 손가락을 이동 하 라는 메시지가 표시 됩니다 (선형) 하거나 더 센서에서 손가락을 둡니다.




-   **`AcquiredToSlow`** &ndash; (값 4) 지문 이미지 동작의 부족으로 인해 읽을 수 없습니다. 살짝 밀기 동작을 필요로 하는 선형 배열 센서에 가장 적합 합니다.



-   **`ErrorCanceled`** &ndash; (값 5) 지문 센서를 사용할 수 없기 때문에 작업이 취소 되었습니다. 예를 들어, 사용자 전환 됩니다, 장치가 잠긴 경우 또는 작업이 보류 중인 다른를 방해 하거나 비활성화 하는 경우 발생할 수 있습니다.



-   **`ErrorHwUnavailable`** &ndash; (값 1) 하드웨어를 사용할 수 없는 경우 나중에 다시 시도하십시오.




-   **`ErrorLockout`** &ndash; (값 7) API를 너무 많이 시도 하면 인해 잠그기 때문에 작업이 취소 되었습니다.




-   **`ErrorNoSpace`** &ndash; (값 4) 등록; 등의 작업에 대 한 반환 된 오류 상태 남은 작업을 완료할 충분 한 저장소가 없기 때문에 작업을 완료할 수 없습니다.



-   **`ErrorTimeout`** &ndash; (값 3) 현재 요청 너무 오래 실행 된 경우 반환 되는 오류 상태입니다. 이 프로그램 지문 센서를 무기한 대기 하지 못하도록 것입니다. 제한 시간 플랫폼과 관련 센서는 있지만 일반적으로 30 초 정도 있습니다.



-   **`ErrorUnableToProcess`** &ndash; (값 2) 센서 현재 이미지를 처리할 수 없을 때 반환 된 오류 상태입니다.



## <a name="related-links"></a>관련 링크

- [Cipher](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
