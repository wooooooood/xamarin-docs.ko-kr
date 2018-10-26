---
title: CryptoObject 만들기
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 1658934bedce11a42701eb023a42fc9e617b654d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113770"
---
# <a name="creating-a-cryptoobject"></a>CryptoObject 만들기

지문 인증 결과의 무결성이 응용 프로그램에 중요 한 &ndash; 것이 응용 프로그램에서 사용자의 id를 인식 하는 방법입니다. 타사 맬웨어를 가로채 고 지문 스캐너를 반환한 결과 사용 하 여 조작에 대 한 이론적으로 불가능 합니다. 이 섹션에서는 지문 결과의 유효성을 유지 하기 위한 방법 중 하나를 설명 합니다. 

합니다 `FingerprintManager.CryptoObject` Java cryptography Api 래퍼는 및에서 사용 되는 `FingerprintManager` 인증 요청의 무결성을 보호 하기. 일반적으로 `Javax.Crypto.Cipher` 개체가 결과 지문 스캐너를 암호화 하는 메커니즘입니다. `Cipher` 개체 자체 Android 키 저장소 Api를 사용 하 여 응용 프로그램에서 만들어지는 키가 사용 됩니다.

이러한 클래스는 모두 함께 작동 하는 방법을 이해 하려면 먼저 살펴보겠습니다를 만드는 방법을 보여 주는 다음 코드를 `CryptoObject`를 자세히 설명 합니다.

```csharp
public class CryptoObjectHelper
{
    // This can be key name you want. Should be unique for the app.
    static readonly string KEY_NAME = "com.xamarin.android.sample.fingerprint_authentication_key";

    // We always use this keystore on Android.
    static readonly string KEYSTORE_NAME = "AndroidKeyStore";

    // Should be no need to change these values.
    static readonly string KEY_ALGORITHM = KeyProperties.KeyAlgorithmAes;
    static readonly string BLOCK_MODE = KeyProperties.BlockModeCbc;
    static readonly string ENCRYPTION_PADDING = KeyProperties.EncryptionPaddingPkcs7;
    static readonly string TRANSFORMATION = KEY_ALGORITHM + "/" +
                                            BLOCK_MODE + "/" +
                                            ENCRYPTION_PADDING;
    readonly KeyStore _keystore;

    public CryptoObjectHelper()
    {
        _keystore = KeyStore.GetInstance(KEYSTORE_NAME);
        _keystore.Load(null);
    }

    public FingerprintManagerCompat.CryptoObject BuildCryptoObject()
    {
        Cipher cipher = CreateCipher();
        return new FingerprintManagerCompat.CryptoObject(cipher);
    }

    Cipher CreateCipher(bool retry = true)
    {
        IKey key = GetKey();
        Cipher cipher = Cipher.GetInstance(TRANSFORMATION);
        try
        {
            cipher.Init(CipherMode.EncryptMode | CipherMode.DecryptMode, key);
        } catch(KeyPermanentlyInvalidatedException e)
        {
            _keystore.DeleteEntry(KEY_NAME);
            if(retry)
            {
                CreateCipher(false);
            } else
            {
                throw new Exception("Could not create the cipher for fingerprint authentication.", e);
            }
        }
        return cipher;
    }

    IKey GetKey()
    {
        IKey secretKey;
        if(!_keystore.IsKeyEntry(KEY_NAME))
        {
            CreateKey();
        }

        secretKey = _keystore.GetKey(KEY_NAME, null);
        return secretKey;
    }

    void CreateKey()
    {
        KeyGenerator keyGen = KeyGenerator.GetInstance(KeyProperties.KeyAlgorithmAes, KEYSTORE_NAME);
        KeyGenParameterSpec keyGenSpec =
            new KeyGenParameterSpec.Builder(KEY_NAME, KeyStorePurpose.Encrypt | KeyStorePurpose.Decrypt)
                .SetBlockModes(BLOCK_MODE)
                .SetEncryptionPaddings(ENCRYPTION_PADDING)
                .SetUserAuthenticationRequired(true)
                .Build();
        keyGen.Init(keyGenSpec);
        keyGen.GenerateKey();
    }
}
```

샘플 코드는 새 만들어집니다 `Cipher` 각각에 대해 `CryptoObject`, 응용 프로그램에서 생성 된 키를 사용 하 여 합니다. 키로 식별 되는 `KEY_NAME` 의 시작 부분에 설정 된 변수는 `CryptoObjectHelper` 클래스입니다. 메서드가 `GetKey` 시도 되며 Android 키 저장소 Api를 사용 하 여 키를 검색 합니다. 키 없으면, 다음 메서드 `CreateKey` 응용 프로그램에 대 한 새 키를 만듭니다.

암호에 대 한 호출을 사용 하 여 인스턴스화될 `Cipher.GetInstance`해를 _변환_ (암호 데이터 암호화 및 해독 하는 방법을 설명 하는 문자열 값). 에 대 한 호출 `Cipher.Init` 응용 프로그램에서 키를 제공 하 여 암호의 초기화를 완료 합니다. 

Android 키를 무효화 될 수 있는 경우도 있음을 알 것: 

* 새 지문이 장치를 사용 하 여 등록 되었습니다.
* 지문을 장치를 등록 하지 있습니다.
* 사용자는 화면 잠금을 비활성화 합니다.
* 사용자가 화면 잠금 (의 screenlock 또는 PIN/사용 된 패턴의 형식)으로 변경 합니다.

이 경우 `Cipher.Init` 시킵니다를 [ `KeyPermanentlyInvalidatedException` ](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)합니다. 위의 샘플 코드는 해당 예외를 트래핑, 키 삭제 및 새로 만든

다음 섹션에서는 키를 만들고 장치에 저장 하는 방법을 설명 합니다.

## <a name="creating-a-secret-key"></a>비밀 키 만들기

`CryptoObjectHelper` 클래스에서는 Android [ `KeyGenerator` ](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/) 키 만들기 및 장치에 저장 합니다. `KeyGenerator` 클래스 키를 만들 수를 요구 하지만 만들 키의 형식에 대 한 일부 메타 데이터입니다. 이 정보는 인스턴스의에서 제공 합니다 [ `KeyGenParameterSpec` ](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) 클래스입니다. 

A `KeyGenerator` 사용 하 여 인스턴스화됩니다는 `GetInstance` 팩터리 메서드입니다. 샘플 코드를 사용 하 여 [ _Advanced Encryption Standard_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) 암호화 알고리즘으로 합니다. AES는 고정된 크기의 블록에 데이터를 나누는 각 해당 블록을 암호화 합니다.

다음으로 `KeyGenParameterSpec` 사용 하 여 만들어집니다는 `KeyGenParameterSpec.Builder`합니다. `KeyGenParameterSpec.Builder` 만들어야 하는 키에 대 한 다음 정보를 래핑합니다.

* 키의 이름입니다.
* 키 암호화와 해독 모두에 대해 유효 해야 합니다.
* 샘플 코드에서는 `BLOCK_MODE` 로 설정 된 _Cipher Block Chaining_ (`KeyProperties.BlockModeCbc`), 각 블록은 이전 블록 (각 블록 사이의 종속성 만들기)를 사용 하 여 스트림은 임을 의미 합니다. 
* 합니다 `CryptoObjectHelper` 사용 하 여 [ _공개 키 암호화 표준 #7_ ](https://tools.ietf.org/html/rfc2315) (_PKCS7_) 되는지 확인 하는 동일한 크기의 모든 블록은 채울 바이트를 생성 하려면 .
* `SetUserAuthenticationRequired(true)` 의미 키를 사용 하기 전에 사용자 인증이 필요 합니다.

한 번 합니다 `KeyGenParameterSpec` 는 초기화에 사용 만들어지면를 `KeyGenerator`, 키 생성 되 고 안전 하 게 장치에 저장 됩니다. 

## <a name="using-the-cryptoobjecthelper"></a>CryptoObjectHelper를 사용 하 여

샘플 코드를 만들기 위한 논리의 대부분 캡슐화가 했으므로 `CryptoWrapper` 에 `CryptoObjectHelper` 클래스, 보겠습니다이 가이드의 시작 부분에서 코드를 다시 방문 하 고 사용을 `CryptoObjectHelper` 암호를 만들고 지문 스캐너를 시작 하: 

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */
    
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    // Using the Support Library classes for maximum reach
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthCallbacks is a C# class defined elsewhere in code.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Here is where the CryptoObjectHelper builds the CryptoObject. 
    fingerprintManager.Authenticate(cryptohelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

만드는 방법을 살펴본 했으므로 `CryptoObject`, 참조 이동할 수 있습니다. 방법을 `FingerprintManager.AuthenticationCallbacks` Android 응용 프로그램에 지문 스캐너 서비스의 결과 전송 하는 데 사용 됩니다.



## <a name="related-links"></a>관련 링크

- [암호화](https://developer.xamarin.com/api/type/Javax.Crypto.Cipher/)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)
- [KeyGenParameterSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](http://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315-PCKS #7](https://tools.ietf.org/html/rfc2315)
