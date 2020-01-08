---
title: CryptoObject 만들기
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 871058d1c128b37a0f2e77b43587139efb433de1
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75487778"
---
# <a name="creating-a-cryptoobject"></a>CryptoObject 만들기

지문 인증 결과의 무결성은 응용 프로그램에서 사용자의 id를 아는 방법 &ndash; 응용 프로그램에 중요 합니다. 이론적으로 타사 맬웨어는 지문 스캐너에서 반환 된 결과를 가로채 조작할 수 있습니다. 이 섹션에서는 지문 결과의 유효성을 유지 하는 한 가지 방법을 설명 합니다. 

`FingerprintManager.CryptoObject`은 Java 암호화 Api를 중심으로 하는 래퍼로, `FingerprintManager`에서 인증 요청의 무결성을 보호 하는 데 사용 됩니다. 일반적으로 `Javax.Crypto.Cipher` 개체는 지문 스캐너의 결과를 암호화 하는 메커니즘입니다. `Cipher` 개체 자체는 Android 키 저장소 Api를 사용 하 여 응용 프로그램에서 만든 키를 사용 합니다.

이러한 클래스가 모두 함께 작동 하는 방식을 이해 하려면 먼저 `CryptoObject`를 만든 다음 보다 자세히 설명 하는 다음 코드를 살펴보겠습니다.

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
            cipher.Init(CipherMode.EncryptMode, key);
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

샘플 코드는 응용 프로그램에서 만든 키를 사용 하 여 각 `CryptoObject`에 대 한 새 `Cipher`를 만듭니다. 키는 `CryptoObjectHelper` 클래스의 시작 부분에 설정 된 `KEY_NAME` 변수로 식별 됩니다. `GetKey` 메서드는 Android 키 저장소 Api를 사용 하 여 키를 시도 하 고 검색 합니다. 키가 없는 경우 메서드 `CreateKey`는 응용 프로그램에 대 한 새 키를 만듭니다.

암호화는 `Cipher.GetInstance`에 대 한 호출을 사용 하 여 인스턴스화되고, _변환을_ 수행 하 고, 데이터를 암호화 하 고 암호 해독 하는 방법을 암호에 알려 주는 문자열 값입니다. `Cipher.Init`를 호출 하면 응용 프로그램의 키를 제공 하 여 암호 초기화를 완료 합니다. 

Android에서 키를 무효화할 수 있는 몇 가지 상황이 있습니다. 

- 새 지문이 장치에 등록 되었습니다.
- 장치에 등록 된 지문이 없습니다.
- 사용자가 화면 잠금을 사용 하지 않도록 설정 했습니다.
- 사용자가 화면 잠금 (screenlock의 유형 또는 사용 된 PIN/패턴)을 변경 했습니다.

이 경우 `Cipher.Init`에서 [`KeyPermanentlyInvalidatedException`](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)를 throw 합니다. 위의 샘플 코드에서는 해당 예외를 트래핑 하 고 키를 삭제 한 다음 새 항목을 만듭니다.

다음 섹션에서는 키를 만들어 장치에 저장 하는 방법을 설명 합니다.

## <a name="creating-a-secret-key"></a>비밀 키 만들기

`CryptoObjectHelper` 클래스는 Android [`KeyGenerator`](xref:Javax.Crypto.KeyGenerator) 를 사용 하 여 키를 만들고 장치에 저장 합니다. `KeyGenerator` 클래스는 키를 만들 수 있지만 만들 키 형식에 대 한 메타 데이터가 필요 합니다. 이 정보는 [`KeyGenParameterSpec`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) 클래스의 인스턴스에서 제공 됩니다. 

`KeyGenerator` `GetInstance` factory 메서드를 사용 하 여 인스턴스화됩니다. 샘플 코드는_AES_( [_AES(Advanced Encryption Standard)_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) )를 암호화 알고리즘으로 사용 합니다. AES는 데이터를 고정 크기의 블록으로 분할 하 고 각 블록을 암호화 합니다.

그런 다음 `KeyGenParameterSpec.Builder`를 사용 하 여 `KeyGenParameterSpec`를 만듭니다. `KeyGenParameterSpec.Builder`은 만들 키에 대 한 다음 정보를 래핑합니다.

- 키의 이름입니다.
- 키는 암호화 및 암호 해독 모두에 유효 해야 합니다.
- 샘플 코드에서 `BLOCK_MODE`은`KeyProperties.BlockModeCbc`( _암호화 블록 체인_ )로 설정 됩니다. 즉, 각 블록은 이전 블록 (각 블록 간의 종속성 만들기)으로 XORed 됩니다. 
- `CryptoObjectHelper`는_PKCS7_( [_공개 키 암호화 표준 #7_](https://tools.ietf.org/html/rfc2315) )를 사용 하 여 블록이 모두 동일한 크기 인지 확인 하기 위해 블록을 채울 바이트를 생성 합니다.
- `SetUserAuthenticationRequired(true)` 키를 사용 하려면 사용자 인증이 필요 함을 의미 합니다.

`KeyGenParameterSpec` 만들어진 후에는 키를 생성 하 여 장치에 안전 하 게 저장 하는 `KeyGenerator`를 초기화 하는 데 사용 됩니다. 

## <a name="using-the-cryptoobjecthelper"></a>CryptoObjectHelper 사용

이제 샘플 코드에서 `CryptoObjectHelper` 클래스에 `CryptoWrapper`을 만들기 위한 논리를 많이 캡슐화 했으므로이 가이드의 시작 부분부터 코드를 다시 방문 하 고 `CryptoObjectHelper`를 사용 하 여 암호를 만들고 지문 스캐너를 시작 하겠습니다. 

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

`CryptoObject`를 만드는 방법을 살펴보았으므로 이제를 사용 하 여 지문 스캐너 서비스의 결과를 Android 응용 프로그램으로 전송 하는 데 `FingerprintManager.AuthenticationCallbacks`를 사용 하는 방법을 확인할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [암호](xref:Javax.Crypto.Cipher)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](xref:Javax.Crypto.KeyGenerator)
- [KeyGenParameterSpec](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](https://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315-PCKS #7](https://tools.ietf.org/html/rfc2315)
