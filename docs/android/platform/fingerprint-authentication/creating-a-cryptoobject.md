---
title: CryptoObject 만들기
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 871058d1c128b37a0f2e77b43587139efb433de1
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "75487778"
---
# <a name="creating-a-cryptoobject"></a>CryptoObject 만들기

지문 인증 결과의 무결성은 애플리케이션에 중요합니다. 애플리케이션이 사용자의 신원을 파악하는 방법이기 때문입니다. 이론적으로 맬웨어는 지문 스캐너에서 반환된 결과를 가로채 조작할 수 있습니다. 이 섹션에서는 지문 결과의 유효성을 유지하는 한 가지 방법을 설명합니다. 

`FingerprintManager.CryptoObject`는 Java 암호화 API를 중심으로 하는 래퍼로, `FingerprintManager`가 인증 요청의 무결성을 보호하는 데 사용합니다. 일반적으로 `Javax.Crypto.Cipher` 개체는 지문 스캐너의 결과를 암호화하는 메커니즘입니다. `Cipher` 개체 자체는 애플리케이션이 Android 키 저장소 API를 사용하여 만든 키를 사용합니다.

이러한 클래스가 모두 함께 작동하는 방식을 이해하기 위해, 먼저 `CryptoObject`를 만드는 방법을 보여 주는 다음 코드를 살펴본 후 자세히 설명하겠습니다.

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

샘플 코드는 애플리케이션에서 만든 키를 사용하여 각 `CryptoObject`에 대한 새 `Cipher`를 만듭니다. 키는 `CryptoObjectHelper` 클래스의 시작 부분에 설정된 `KEY_NAME` 변수로 식별됩니다. `GetKey` 메서드는 Android 키 저장소 API를 사용하여 키를 시도하고 검색합니다. 키가 없는 경우 `CreateKey` 메서드는 애플리케이션에 대한 새 키를 만듭니다.

암호화는 `Cipher.GetInstance` 호출을 사용하여 인스턴스화되고 _변환_(데이터 암호화 및 암호 해독 방법을 암호에 알려주는 문자열 값)을 취합니다. `Cipher.Init`를 호출하면 애플리케이션의 키를 제공하여 암호 초기화를 완료합니다. 

Android가 키를 무효화할 수 있는 몇 가지 상황이 있습니다. 

- 디바이스에 새 지문이 등록되었습니다.
- 디바이스에 등록된 지문이 없습니다.
- 사용자가 화면 잠금을 사용하지 않도록 설정했습니다.
- 사용자가 화면 잠금(화면 잠금 유형 또는 사용된 PIN/패턴)을 변경했습니다.

이 경우 `Cipher.Init`는 [`KeyPermanentlyInvalidatedException`](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)를 throw합니다. 위의 샘플 코드에서는 해당 예외를 트래핑하고 키를 삭제한 다음 새 키를 만듭니다.

다음 섹션에서는 키를 만들어 디바이스에 저장하는 방법을 설명합니다.

## <a name="creating-a-secret-key"></a>비밀 키 만들기

`CryptoObjectHelper` 클래스는 Android [`KeyGenerator`](xref:Javax.Crypto.KeyGenerator)를 사용하여 키를 만들고 디바이스에 저장합니다. `KeyGenerator` 클래스는 키를 만들 수 있지만 만들 키의 유형에 대한 메타데이터가 필요합니다. 이 정보는 [`KeyGenParameterSpec`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) 클래스의 인스턴스에서 제공합니다. 

`KeyGenerator`는 `GetInstance` 팩터리 메서드를 사용하여 인스턴스화됩니다. 이 샘플 코드는 [_AES_](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)(_Advanced Encryption Standard_)를 암호화 알고리즘으로 사용합니다. AES는 데이터를 고정 크기의 블록으로 분할하고 각 블록을 암호화합니다.

그런 다음 `KeyGenParameterSpec.Builder`를 사용하여 `KeyGenParameterSpec`을 만듭니다. `KeyGenParameterSpec.Builder`은 만들 키에 대한 다음 정보를 래핑합니다.

- 키의 이름입니다.
- 키는 암호화 및 암호 해독 모두에 유효해야 합니다.
- 샘플 코드에서 `BLOCK_MODE`는 _암호화 블록 체인_(`KeyProperties.BlockModeCbc`)으로 설정됩니다. 즉, 각 블록이 이전 블록으로 XOR됩니다(각 블록 간 종속성을 생성). 
- `CryptoObjectHelper`는 [_PKCS7_](https://tools.ietf.org/html/rfc2315)(_공개 키 암호 표준 #7_)을 사용하여 크기가 모두 동일하도록 블록을 채울 바이트를 생성합니다.
- `SetUserAuthenticationRequired(true)`는 키를 사용하려면 사용자 인증이 필요함을 의미합니다.

`KeyGenParameterSpec`이 만들어진 후에는 키를 생성하여 디바이스에 안전하게 저장하는 `KeyGenerator`를 초기화하는 데 사용됩니다. 

## <a name="using-the-cryptoobjecthelper"></a>CryptoObjectHelper 사용

이제 샘플 코드에서 `CryptoObjectHelper` 클래스에 `CryptoWrapper`를 만들기 위한 논리를 상당 부분 캡슐화했으므로, 이 가이드의 시작 부분부터 다시 코드를 수행하여 `CryptoObjectHelper`를 사용하여 암호를 만들고 지문 스캐너를 시작하겠습니다. 

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

`CryptoObject`를 만드는 방법을 살펴보았으므로, 계속해서 `FingerprintManager.AuthenticationCallbacks`을 사용하여 지문 스캐너 서비스의 결과를 Android 애플리케이션으로 전송하는 방법을 살펴보겠습니다.

## <a name="related-links"></a>관련 링크

- [Cipher](xref:Javax.Crypto.Cipher)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [KeyGenerator](xref:Javax.Crypto.KeyGenerator)
- [KeyGenParameterSpec](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html)
- [KeyGenParameterSpec.Builder](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html)
- [KeyPermanentlyInvalidatedException](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)
- [KeyProperties](https://developer.android.com/reference/android/security/keystore/KeyProperties.html)
- [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [RFC 2315 - PCKS #7](https://tools.ietf.org/html/rfc2315)
