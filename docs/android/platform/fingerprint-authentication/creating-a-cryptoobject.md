---
title: CryptoObject 만들기
ms.prod: xamarin
ms.assetid: 4D159B80-FFF4-4136-A7F0-F8A5C6B86838
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7328792e0d921beb09389d9a0400ea97766b2246
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756441"
---
# <a name="creating-a-cryptoobject"></a>CryptoObject 만들기

지문 인증 결과의 무결성은 응용 프로그램에서 사용자의 id &ndash; 를 인식 하는 방식으로 중요 합니다. 이론적으로 타사 맬웨어는 지문 스캐너에서 반환 된 결과를 가로채 조작할 수 있습니다. 이 섹션에서는 지문 결과의 유효성을 유지 하는 한 가지 방법을 설명 합니다. 

는 `FingerprintManager.CryptoObject` Java 암호화 api를 중심으로 하는 래퍼로,에서 인증 요청의 `FingerprintManager` 무결성을 보호 하는 데 사용 됩니다. 일반적으로 개체 `Javax.Crypto.Cipher` 는 지문 스캐너의 결과를 암호화 하는 메커니즘입니다. 개체 `Cipher` 자체는 Android 키 저장소 api를 사용 하 여 응용 프로그램에서 만든 키를 사용 합니다.

이러한 클래스가 모두 함께 작동 하는 방식을 이해 하기 위해 먼저을 만든 `CryptoObject`다음 자세히 설명 하는 다음 코드를 살펴보겠습니다.

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

샘플 코드는 응용 프로그램에서 만든 `Cipher` 키를 `CryptoObject`사용 하 여 각각에 대해 새를 만듭니다. 키는 `KEY_NAME` 클래스`CryptoObjectHelper` 의 시작 부분에 설정 된 변수로 식별 됩니다. 이 메서드 `GetKey` 는 Android 키 저장소 api를 사용 하 여 키를 시도 하 고 검색 합니다. 키가 없는 경우 메서드 `CreateKey` 는 응용 프로그램에 대 한 새 키를 만듭니다.

암호화는에 대 `Cipher.GetInstance`한 호출을 사용 하 여 인스턴스화되고, _변환을_ 수행 하 고, 데이터를 암호화 하 고 암호를 해독 하는 방법을 암호에 알려 주는 문자열 값입니다. 에 대 `Cipher.Init` 한 호출은 응용 프로그램의 키를 제공 하 여 암호화 초기화를 완료 합니다. 

Android에서 키를 무효화할 수 있는 몇 가지 상황이 있습니다. 

- 새 지문이 장치에 등록 되었습니다.
- 장치에 등록 된 지문이 없습니다.
- 사용자가 화면 잠금을 사용 하지 않도록 설정 했습니다.
- 사용자가 화면 잠금 (screenlock의 유형 또는 사용 된 PIN/패턴)을 변경 했습니다.

이 `Cipher.Init` 경우는 [`KeyPermanentlyInvalidatedException`](https://developer.android.com/reference/android/security/keystore/KeyPermanentlyInvalidatedException.html)을 throw 합니다. 위의 샘플 코드에서는 해당 예외를 트래핑 하 고 키를 삭제 한 다음 새 항목을 만듭니다.

다음 섹션에서는 키를 만들어 장치에 저장 하는 방법을 설명 합니다.

## <a name="creating-a-secret-key"></a>비밀 키 만들기

클래스 `CryptoObjectHelper` 는 Android [`KeyGenerator`](xref:Javax.Crypto.KeyGenerator) 를 사용 하 여 키를 만들고 장치에 저장 합니다. 클래스 `KeyGenerator` 는 키를 만들 수 있지만 만들 키 형식에 대 한 메타 데이터가 필요 합니다. 이 정보는 [`KeyGenParameterSpec`](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.html) 클래스의 인스턴스에서 제공 됩니다. 

는 `KeyGenerator` 팩터리 메서드를 `GetInstance` 사용 하 여 인스턴스화됩니다. 샘플 코드는_AES_( [_AES(Advanced Encryption Standard)_ ](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) )를 암호화 알고리즘으로 사용 합니다. AES는 데이터를 고정 크기의 블록으로 분할 하 고 각 블록을 암호화 합니다.

그런 다음를 `KeyGenParameterSpec` `KeyGenParameterSpec.Builder`사용 하 여를 만듭니다. 는 `KeyGenParameterSpec.Builder` 만들 키에 대 한 다음 정보를 래핑합니다.

- 키의 이름입니다.
- 키는 암호화 및 암호 해독 모두에 유효 해야 합니다.
- 샘플 코드 `BLOCK_MODE` 에서는 _Cipher block 체인화_ (`KeyProperties.BlockModeCbc`)로 설정 됩니다. 즉, 각 블록이 이전 블록으로 XORed (각 블록 간의 종속성을 생성 함). 
- 는 `CryptoObjectHelper` _PKCS7_( [_공개 키 암호화 표준 #7_](https://tools.ietf.org/html/rfc2315) )를 사용 하 여 블록이 모두 동일한 크기 인지 확인 하는 바이트를 생성 합니다.
- `SetUserAuthenticationRequired(true)`키를 사용 하려면 사용자 인증이 필요 함을 의미 합니다.

를 만든 후에는 키를 생성 하 여 장치 `KeyGenerator`에 안전 하 게 저장 하는을 초기화 하는 데 사용 됩니다. `KeyGenParameterSpec` 

## <a name="using-the-cryptoobjecthelper"></a>CryptoObjectHelper 사용

이제 샘플 코드가 `CryptoWrapper` `CryptoObjectHelper` 클래스에를 만들기 위한 논리를 크게 캡슐화 했으므로 `CryptoObjectHelper` 이 가이드의 시작 부분에서 코드를 다시 방문 하 고를 사용 하 여 암호를 만들고 지문 스캐너를 시작 하겠습니다. 

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

이제를 만드는 `CryptoObject`방법을 살펴보았으므로 이제를 사용 하 여 `FingerprintManager.AuthenticationCallbacks` 지문 스캐너 서비스의 결과를 Android 응용 프로그램으로 전송 하는 방법을 확인할 수 있습니다.

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
