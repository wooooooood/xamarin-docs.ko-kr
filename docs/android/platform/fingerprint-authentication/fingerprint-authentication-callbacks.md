---
title: "인증 콜백을에 응답"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: 371ffae8e14a630cb548f4a9ee2bf0bd06f7284c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="responding-to-authentication-callbacks"></a>인증 콜백을에 응답

지문 스캐너 자체 스레드에서 백그라운드로 실행 되 고 완료 되 면 방법 중 하나를 호출 하 여 검사 결과 보고 합니다 `FingerprintManager.AuthenticationCallback` UI 스레드에서 합니다. Android 응용 프로그램에 다음과 같은 모든 메서드를 구현 하는이 추상 클래스를 확장 하는 자체 처리기를 제공 해야 합니다.

* **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; 복구할 수 없는 오류가 있을 때 호출 됩니다. 일 수 있는 다시 시도 제외 하 고 상황을 해결 하는 것이 할 수 있는 응용 프로그램이 나 사용자는 없습니다.
* **`OnAuthenticationFailed()`** &ndash; 이 메서드는 지문 검색 되었지만 장치에서 인식 되지 때 호출 됩니다.
* **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; 스캐너를 통해 빠른를 살짝 되 고 손가락 같은 복구할 수 있는 오류가 있을 때 호출 됩니다.
* **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; 이 인식 된 지문 때 호출 됩니다.

경우는 `CryptoObject` 를 호출할 때 사용한 `Authenticate`를 호출 하는 것이 좋습니다. `Cipher.DoFinal` 에서 `OnAuthenticationSuccessful`합니다.
`DoFinal` 암호화 손상 되었거나 잘못 초기화 하는 경우 예외를 throw 하 고는 지문 스캐너의 결과 손상 되었을 수 있는 응용 프로그램 외부의 있음을 나타냅니다.


> [!NOTE]
> **참고:** 응용 프로그램별 논리의를 콜백 클래스 양이 비교적 적은 가중치를 유지 하는 것이 좋습니다. 콜백은 한 "트래픽 개의 cop" Android 응용 프로그램 및 결과 간의 지문 스캐너에서로 작동 해야 합니다.

## <a name="a-sample-authentication-callback-handler"></a>샘플 인증 콜백 처리기

다음 클래스는 최소한의의 예시 `FingerprintManager.AuthenticationCallback` 구현: 

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

`OnAuthenticationSucceeded` 확인 하는 `Cipher` 에 제공 되었음을 `FingerprintManager` 때 `Authentication` 호출 되었습니다. 이 경우는 `DoFinal` 암호화 메서드를 호출 합니다. 닫습니다는 `Cipher`를 원래 상태로 복원 합니다. 경우 문제가 발생는 암호화 된 다음 `DoFinal` 예외를 throw 하 고 인증 시도가 실패 한 것으로 고려해 야 합니다.

`OnAuthenticationError` 및 `OnAuthenticationHelp` 콜백을 각 문제를 나타내는 정수를 수신 합니다. 다음 섹션에는 각각의 가능한 도움말 또는 오류 코드 설명합니다. 두 개의 콜백을 비슷한 용도로 &ndash; 지문 인증이 실패 했음을 응용 프로그램에 알리기 위해. 서로 어떻게 심각도입니다. `OnAuthenticationHelp` 비교 하 여 너무 빠른; 넘기기가 같은 사용자 복구할 수 있는 오류 `OnAuthenticationError` 가 손상 된 지문 스캐너 같은 심각한 오류가 더 합니다.

`OnAuthenticationError` 지문 검사를 통해 취소 될 때 호출 됩니다는 `CancellationSignal.Cancel()` 메시지입니다. `errMsgId` 매개 변수는 5의 값을 갖습니다 (`FingerprintState.ErrorCanceled`). 구현 요구 사항에 따라는 `AuthenticationCallbacks` 이 상황을 다른 오류를 다르게 처리 될 수 있습니다. 

`OnAuthenticationFailed` 비교 하 여 성공적으로 검색 한 했지만 등록 된 장치는 지문와 일치 하지 않습니다 때 호출 됩니다. 

## <a name="help-codes-and-error-message-ids"></a>도움말 코드 및 오류 메시지 Id 

목록 및 도움말 코드 및 오류 코드 설명에서 확인할 수 있습니다는 [Android SDK 설명서](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) FingerprintManager 클래스에 대 한 합니다. Xamarin.Android 사용 하 여 이러한 값을 나타내는 `Android.Hardware.Fingerprints.FingerprintState` enum:


-   **`AcquiredGood`** &ndash; (값 0) 획득 이미지 좋은 했습니다.


-   **`AcquiredImagerDirty`** &ndash; (값 3) 지문 이미지는 센서에 의심 되는 또는 검색 된 흙 때문에 지나치게 노이즈가 합니다. 예를 들어이 다중 후이 반환 `AcquiredInsufficient` 또는 흙 (고정된 된 픽셀, swaths 등)의 센서 대의 실제 검색 합니다. 사용자는이 값이 반환 하는 경우에 센서를 청소 하려면 작업을 수행 해야 합니다.


-   **`AcquiredInsufficient`** &ndash; (값 2) 지문 이미지는 검색된 조건 (예: 건조 스킨) 또는 가능한 더티 센서 발생 하 여 프로세스에 지나치게 노이즈가 (참조 `AcquiredImagerDirty`합니다.



-   **`AcquiredPartial`** &ndash; (값 1) 부분 지문 이미지만 검색 되었습니다. 등록 중에 예:이 문제를 해결 하려면 실행 해야 할 사용자를 알릴 &ldquo;모뎀과 센서에 키를 누릅니다.&rdquo;



-   **`AcquiredTooFast`** &ndash; (값 5) 지문 이미지 빠른 동작으로 인해 완료 되지 않았습니다. 선형 배열 센서에 대 한 적절 한 대부분을 하는 동안 손가락 취득 하는 동안 이동 된 경우 발생할 수이 있습니다. 느린 손가락을 이동 하 라는 메시지가 표시 합니다 (선형) 하거나 더 긴 센서에 손가락을 그대로 적용 합니다.




-   **`AcquiredToSlow`** &ndash; (값 4) 지문 이미지 동작의 부족으로 인해 읽을 수 없습니다. 이 살짝 동작을 필요로 하는 선형 배열 센서에 가장 적합 합니다.



-   **`ErrorCanceled`** &ndash; (값 5) 지문 센서를 사용할 수 없기 때문에 작업이 취소 되었습니다. 예를 들어이 사용자로 전환, 장치가 잠겨, 또는 대기 중인 다른를 방해 하거나 비활성화 때 발생할 수 있습니다.



-   **`ErrorHwUnavailable`** &ndash; (값 1) 하드웨어를 사용할 수 없는 경우 나중에 다시 시도 하십시오.




-   **`ErrorLockout`** &ndash; (값 7) API를 너무 많이 시도 인해 잠그기 때문에 작업이 취소 되었습니다.




-   **`ErrorNoSpace`** &ndash; (값 4) 등록; 작업에 대 한 반환 된 오류 상태 작업을 완료할 때까지 남은 충분 한 저장소가 없기 때문에 작업을 완료할 수 없습니다.



-   **`ErrorTimeout`** &ndash; (값 3) 현재 요청이 너무 오래 실행 된 경우 반환 되는 오류 상태입니다. 이 프로그램 지문 센서를 무기한 대기 하지 않도록 설정 됩니다. 제한 시간 플랫폼 및 세션 특정 않으며는 일반적으로 약 30 초입니다.



-   **`ErrorUnableToProcess`** &ndash; (값 2) 센서 현재 이미지를 처리할 수 없는 경우 반환 된 오류 상태입니다.



## <a name="related-links"></a>관련 링크

- [Cipher](https://docs.oracle.com/javase/7/docshttps://developer.xamarin.com/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
