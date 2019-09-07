---
title: Android 용 HttpClient 스택 및 SSL/TLS 구현 선택기
description: HttpClient 스택 및 SSL/TLS 구현 선택기는 Xamarin Android 앱에서 사용 되는 HttpClient 및 SSL/TLS 구현을 결정 합니다.
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/20/2018
ms.openlocfilehash: 3b74acee34c367814fbd2a948fe490f4225aee00
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755388"
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>Android 용 HttpClient 스택 및 SSL/TLS 구현 선택기

HttpClient 스택 및 SSL/TLS 구현 선택기는 Xamarin Android 앱에서 사용 되는 HttpClient 및 SSL/TLS 구현을 결정 합니다.

프로젝트는 **시스템 .net. Http** 어셈블리를 참조 해야 합니다.

> [!WARNING]
> **4 월, 2018** – PCI 규정 준수를 포함 하 여 보안 요구 사항이 증가 함에 따라 주요 클라우드 공급자 및 웹 서버는 1.2 보다 오래 된 TLS 버전 지원을 중지 해야 합니다. 이전 버전의 Visual Studio에서 만든 Xamarin 프로젝트는 이전 버전의 TLS를 사용 합니다.
>
> 앱이 이러한 서버 및 서비스와 함께 계속 작동 하도록 하려면 **아래에 `Android HttpClient` 표시 된 및 `Native TLS 1.2` 설정을 사용 하 여 Xamarin 프로젝트를 업데이트 한 다음 사용자에 게 앱을 다시 빌드하고 다시 배포 해야** 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin.ios HttpClient 구성은 **Android 옵션 > 프로젝트 옵션**에 있으며 **고급 옵션** 단추를 클릭 합니다.

다음은 TLS 1.2 지원에 권장 되는 설정입니다.

[![Visual Studio Android 옵션](http-stack-images/android-win-sml.png)](http-stack-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Xamarin.ios HttpClient 구성은 **프로젝트 옵션에서 Android 빌드 설정 > 빌드 >** 하 고 **일반** 탭을 클릭 합니다.

다음은 TLS 1.2 지원에 권장 되는 설정입니다.

[![Android 옵션 Mac용 Visual Studio](http-stack-images/android-mac-sml.png)](http-stack-images/android-mac.png#lightbox)

-----

## <a name="alternative-configuration-options"></a>대체 구성 옵션

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler는 관리 코드에서 모든 항목을 구현 하는 대신 네이티브 Java/OS 코드에 위임 하는 새 처리기입니다.
**이 옵션은 권장 되는 옵션입니다.**

#### <a name="pros"></a>전문가

- 더 나은 성능과 더 작은 실행 파일 크기를 위해 기본 API를 사용 합니다.
- 최신 표준 (예:)에 대 한 지원 TLS 1.2.

#### <a name="cons"></a>단점

- Android 4.1 이상이 필요 합니다.
- 일부 HttpClient 기능/옵션을 사용할 수 없습니다.

### <a name="managed-httpclienthandler"></a>관리 됨 (HttpClientHandler)

관리 되는 처리기는 이전 Xamarin Android 버전과 함께 제공 되는 완전히 관리 되는 HttpClient 처리기입니다.

#### <a name="pros"></a>전문가

- MS .NET 및 이전 Xamarin 버전에서 가장 호환 되는 (기능)입니다.

#### <a name="cons"></a>단점

- OS와 완전히 통합 되지 않습니다 (예: TLS 1.0로 제한 됨).
- 일반적으로 훨씬 느립니다 (예: 암호화)를 기본 API로 합니다.
- 더 큰 응용 프로그램을 만드는 관리 코드가 더 필요 합니다.

### <a name="choosing-a-handler"></a>처리기 선택

`AndroidClientHandler` 와`HttpClientHandler` 중에서 선택 하는 것은 응용 프로그램의 요구 사항에 따라 달라 집니다. `AndroidClientHandler`는 최신 보안 지원 (예:)을 위해 권장 됩니다.

- TLS 1.2 이상 지원이 필요 합니다.
- 앱이 Android 4.1 (API 16) 이상을 대상으로 합니다.
- 에 대 한 `HttpClient`TLS 1.2 + 지원이 필요 합니다.
- 에 대 한 `WebClient`TLS 1.2 + 지원은 필요 하지 않습니다.

`HttpClientHandler`는 TLS 1.2 이상 지원이 필요 하지만 Android 4.1 이전 버전의 Android를 지원 해야 하는 경우에 적합 합니다. 에 대 한 TLS 1.2 + 지원이 필요한 경우에 `WebClient`도이 옵션을 선택 하는 것이 좋습니다.

Xamarin Android 8.3 `HttpClientHandler` 부터는 기본 TLS 공급자로 보링 SSL (`btls`)로 기본 설정 됩니다. 보링 SSL TLS 공급자는 다음과 같은 이점을 제공 합니다.

- TLS 1.2 +를 지원 합니다.
- 모든 Android 버전을 지원 합니다.
- `HttpClient` 및`WebClient`에 대 한 TLS 1.2 + 지원 기능을 제공 합니다.

디스패처에 TLS 공급자로 보링 SSL을 사용할 경우의 단점은 결과 APK의 크기를 늘릴 수 있다는 것입니다 (지원 되는 ABI 당 추가 APK 크기의 1MB를 추가).

Xamarin Android 8.3부터 기본 TLS 공급자는 지루한 SSL (`btls`)입니다. 보링 ssl을 사용 하지 않으려는 경우 `$(AndroidTlsProvider)` 속성을로 `legacy` 설정 하 여 관리 되는 관리 SSL 구현으로 되돌릴 수 있습니다. 빌드 속성 설정에 대 한 자세한 내용은 [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md)를 참조 하세요.

### <a name="programatically-using-androidclienthandler"></a>프로그래밍 방식으로`AndroidClientHandler`

는 `Xamarin.Android.Net.AndroidClientHandler`xamarin.ios 전용 구현입니다.`HttpMessageHandler`
이 클래스의 인스턴스는 모든 HTTP 연결 `java.net.URLConnection` 에 대해 네이티브 구현을 사용 합니다. 이를 통해 이론적으로 HTTP 성능과 더 작은 APK 크기가 증가 합니다.

이 코드 조각은 `HttpClient` 클래스의 단일 인스턴스에 대해를 명시적으로 수행 하는 방법의 예입니다.

```csharp
// Android 4.1 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
> 기본 Android 장치는 TLS 1.2 (ie)를 지원 해야 합니다. Android 4.1 이상). TLS 1.2에 대 한 공식적인 지원은 Android 5.0 이상에서 제공 됩니다. 그러나 일부 장치는 Android 4.1 이상에서 TLS 1.2를 지원 합니다.

## <a name="ssltls-implementation-build-option"></a>SSL/TLS 구현 빌드 옵션

이 프로젝트 옵션은 모든 웹 요청 ( `HttpClient` 및 `WebRequest`)에서 사용할 기본 TLS 라이브러리를 제어 합니다. 기본적으로 TLS 1.2는 다음과 같이 선택 됩니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio의 TLS/SSL 구현 콤보 상자](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Mac용 Visual Studio의 TLS/SSL 구현 콤보 상자](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png#lightbox)

-----

예를 들어:

```csharp
var client = new HttpClient();
```

Httpclient 구현이 관리로 설정 **되** 고 tls 구현이 **Native TLS 1.2 +** 로 설정 된 경우 개체는 `client` 해당 관리 되 `HttpClientHandler` 는 및 tls 1.2 (BoringSSL 라이브러리에서 제공)을 자동으로 사용 합니다. HTTP 요청.

그러나 **httpclient 구현이** 로 `AndroidHttpClient`설정 된 경우 모든 `HttpClient` 개체는 기본 Java 클래스 `java.net.URLConnection` 를 사용 하며 **TLS/SSL 구현** 값의 영향을 받지 않습니다. `WebRequest`개체는 BoringSSL 라이브러리를 사용 합니다.

## <a name="other-ways-to-control-ssltls-configuration"></a>SSL/TLS 구성을 제어 하는 다른 방법

Xamarin Android 응용 프로그램에서 TLS 설정을 제어할 수 있는 방법에는 세 가지가 있습니다.

1. 프로젝트 옵션에서 HttpClient 구현 및 기본 TLS 라이브러리를 선택 합니다.
2. 를 사용 `Xamarin.Android.Net.AndroidClientHandler`하 여 프로그래밍 방식으로
3. 환경 변수를 선언 합니다 (선택 사항).

세 가지 옵션 중 하나를 사용 하는 것이 좋습니다. Android 프로젝트 옵션을 사용 하 여 전체 `HttpMessageHandler` 앱에 대 한 기본 및 TLS를 선언 하는 것입니다. 그런 다음 필요한 경우 프로그래밍 방식으로 `Xamarin.Android.Net.AndroidClientHandler` 개체를 인스턴스화합니다. 이러한 옵션은 위에 설명 되어 있습니다.

환경 변수 &ndash; &ndash; 를 사용 하는 세 번째 옵션은 아래에 설명 되어 있습니다.

### <a name="declare-environment-variables"></a>환경 변수 선언

Xamarin에서 TLS를 사용 하는 것과 관련 된 두 가지 환경 변수는 다음과 같습니다.

- `XA_HTTP_CLIENT_HANDLER_TYPE`이 환경 변수는 응용 프로그램 `HttpMessageHandler` 에서 사용할 기본값을 선언 합니다. &ndash; 예를 들어:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

- `XA_TLS_PROVIDER`이 환경 변수는 `legacy`사용할 `default` TLS 라이브러리, 또는 (이 변수를 생략 하는 것과 동일)를 선언 합니다. `btls` &ndash;

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

이 환경 변수는 프로젝트에 _환경 파일_ 을 추가 하 여 설정 합니다. 환경 파일은 **Androidenvironment**의 빌드 작업을 사용 하는 Unix 형식의 일반 텍스트 파일입니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio의 AndroidEnvironment 빌드 작업 스크린샷](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Mac용 Visual Studio의 AndroidEnvironment 빌드 작업 스크린샷](http-stack-images/tls03-xs.png)

-----

환경 변수 및 Xamarin.ios에 대 한 자세한 내용은 [Xamarin Android 환경](~/android/deploy-test/environment.md) 가이드를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [TLS(전송 계층 보안)](~/cross-platform/app-fundamentals/transport-layer-security.md)
