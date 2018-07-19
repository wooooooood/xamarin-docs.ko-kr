---
title: CryptoObject 만들기
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: f7a8ab7a43c0a3258cf6e737b0d235cbe7a1c747
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765716"
---
# <a name="creating-a-cryptoobject"></a>CryptoObject 만들기

지문 인증 결과의 무결성이 응용 프로그램에 중요 한 &ndash; 것이 응용 프로그램 사용자의 id를 알고 하는 방법입니다. 타사 맬웨어를 가로채 고 지문 검색 프로그램에서 반환 된 결과 변조할 이론적으로 같습니다. 이 섹션에서는 지문 결과의 유효성을 유지 하기 위한 방법 중 하나를 설명 합니다. 

`FingerprintManager.CryptoObject` Java cryptography Api에 대 한 래퍼로 및에서 사용 되는 `FingerprintManager` 인증 요청의 무결성을 보호 합니다. 일반적으로 `Javax.Crypto.Cipher` 개체는 지문 스캐너의 결과 암호화 하기 위한 메커니즘입니다. `Cipher` 개체 자체 Android 키 저장소 Api를 사용 하 여 응용 프로그램에 의해 만들어진 키가 사용 됩니다.

이러한 모든 클래스가 함께 작동 하는 방법을 이해 하려면 먼저 살펴보겠습니다를 만드는 방법을 보여 주는 다음 코드는 `CryptoObject`, 다음 보다 자세히 설명:

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

예제 코드에서는 새 만듭니다 `Cipher` 각 `CryptoObject`, 응용 프로그램에 의해 생성 된 키를 사용 하 여 합니다. 키로 식별 되는 `KEY_NAME` 의 시작 부분에 설정 된 변수는 `CryptoObjectHelper` 클래스입니다. 메서드가 `GetKey` 시도 하 고 Android 키 저장소 Api를 사용 하 여 키를 검색 합니다. 키가 없는 경우 다음 메서드 `CreateKey` 응용 프로그램에 대 한 새 키를 만듭니다.

암호화에 대 한 호출으로 인스턴스화된 `Cipher.GetInstance`해는 _변환_ (암호화 데이터 암호화 및 해독 하는 방법을 설명 하는 문자열 값). 에 대 한 호출 `Cipher.Init` 응용 프로그램에서 키를 제공 하는 암호의 초기화를 완료 합니다. 

일부 경우에는 Android 키를 무효화 될 수 있다는 것은: 

* 새 지 문은 등록 된 장치.
* 지문을 장치를 등록 하지 있습니다.
* 사용자가 화면 잠금을 사용할 수 없습니다.
* 사용자가 화면 잠금 (의 screenlock 또는 PIN/패턴 사용의 형식)으로 변경 합니다.

이 경우 `Cipher.Init` throw 됩니다는 [ `KeyPermanentlyInvalidatedException` ](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)합니다. 위의 샘플 코드는 해당 예외를 트래핑는, 키를 삭제 하 고 새 구성표를 만들어야 합니다.

다음 섹션에는 키를 만들어 장치에 저장 하는 방법을 설명 합니다.

## <a name="creating-a-secret-key"></a>비밀 키를 만드는 중

`CryptoObjectHelper` 클래스 사용은 Android [ `KeyGenerator` ](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/) 는 키를 만들고 장치에 저장 합니다. `KeyGenerator` 클래스 키를 만들 수, 프로그램이 만들려는 키의 형식에 대 한 일부 메타 데이터입니다. 인스턴스에서이 정보는 제공 된 [ `KeyGenParameterSpec` ](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) 클래스입니다. 

A `KeyGenerator` 사용 하 여 인스턴스화됩니다는 `GetInstance` 팩터리 메서드입니다. 사용 하 여 샘플 코드는 [ _고급 암호화 표준_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (_AES_) 암호화 알고리즘으로 합니다. AES는 데이터를 나누고 고정된 된 크기의 블록으로를 각 해당 블록을 암호화 합니다.

다음으로 `KeyGenParameterSpec` 사용 하 여 만들어집니다는 `KeyGenParameterSpec.Builder`합니다. `KeyGenParameterSpec.Builder` 만들어야 하는 키에 대 한 다음 정보를 래핑합니다.

* 키의 이름입니다.
* 키 암호화 및 암호 해독 모두에 대해 유효 해야 합니다.
* 샘플 코드에서는 `BLOCK_MODE` 로 설정 된 _암호 블록 체인_ (`KeyProperties.BlockModeCbc`), 각 블록은 이전 블록 (각 블록 간의 종속성 만들기)와 스트림은 의미 합니다. 
* `CryptoObjectHelper` 사용 하 여 [ _공개 키 암호화 표준 #7_ ](https://tools.ietf.org/html/rfc2315) (_PKCS7_) 같은 크기의 모든 되는지 되도록 블록을 채울 됩니다 (바이트)를 생성 하려면 .
* `SetUserAuthenticationRequired(true)` 해당 사용자 인증을 해야 키를 사용할 수를 의미 합니다.

한 번의 `KeyGenParameterSpec` 은 초기화 사용을 만든는 `KeyGenerator`, 키를 생성 되 고 안전 하 게 장치에 저장 됩니다. 

## <a name="using-the-cryptoobjecthelper"></a>CryptoObjectHelper를 사용 하 여

만들기 위한 논리의 대부분 예제 코드 캡슐화가 했으므로 `CryptoWrapper` 에 `CryptoObjectHelper` 클래스, 보겠습니다이 가이드의 시작 부분에서 코드를 다시 확인 하 고 사용 하는 `CryptoObjectHelper` 암호를 만들고 지문 스캐너를 시작: 

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

만드는 방법을 살펴보았습니다 했으므로 `CryptoObject`, 참조로 이동할 수 있습니다. 방법을 `FingerprintManager.AuthenticationCallbacks` Android 응용 프로그램을 지문 스캐너 서비스의 결과 전송 하는 데 사용 됩니다.



## <a name="related-links"></a>관련 링크

- [Cipher](https://developer.xamarin.com/api/type/Javax.Crypto.Cipher/)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](https://developer.xamarin.com/api/type/Javax.Crypto.KeyGenerator/)
- [KeyGenParameterSpec](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](http://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](http://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](http://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315-PCKS #7](https://tools.ietf.org/html/rfc2315)
