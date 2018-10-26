---
title: HttpClient 스택 및 Android에 대 한 SSL/TLS 구현 선택기
description: HttpClient 스택 및 SSL/TLS 구현 선택기에는 Xamarin.Android 앱에 사용할 SSL/TLS 및 HttpClient 구현을 결정 합니다.
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/20/2018
ms.openlocfilehash: 47c9ddf3f1a61b0ec7e2a8ed993ad665267993fd
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114277"
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>HttpClient 스택 및 Android에 대 한 SSL/TLS 구현 선택기

HttpClient 스택 및 SSL/TLS 구현 선택기에는 Xamarin.Android 앱에 사용할 SSL/TLS 및 HttpClient 구현을 결정 합니다.

프로젝트를 참조 해야 합니다 **System.Net.Http** 어셈블리입니다.

> [!WARNING]
> **2018 년 4 월** 보안 강화로 인해 – 요구 사항, PCI 규정 준수를 포함 하 여 주요 클라우드 공급자 및 웹 서버는 TLS 버전 1.2 보다 이전 지원 중지 해야 합니다.  이전 버전의 Visual Studio 이전 버전의 TLS 사용 하 여 기본값으로 만든 Xamarin 프로젝트입니다.
>
> 앱이 이러한 서버 및 서비스를 사용 하 여 작업을 계속할 수 있도록 **Xamarin 프로젝트를 업데이트 해야 합니다 `Android HttpClient` 및 `Native TLS 1.2` 설정 아래에 표시 된 다음 다시 빌드하고 다시 배포 앱** 를 프로그램 사용자입니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin.Android HttpClient 구성이 **프로젝트 옵션 > Android 옵션**를 클릭 합니다 **고급 옵션** 단추입니다.

이러한 TLS 1.2 지원에 대 한 권장 설정은 다음과 같습니다.

[![Visual Studio Android 옵션](http-stack-images/android-win-sml.png)](http-stack-images/android-win.png#lightbox)


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Xamarin.Android HttpClient 구성이 **프로젝트 옵션 > 빌드 > Android 빌드** 설정과 클릭 합니다 **일반** 탭 합니다.

이러한 TLS 1.2 지원에 대 한 권장 설정은 다음과 같습니다.

[![Android 옵션 Mac 용 visual Studio](http-stack-images/android-mac-sml.png)](http-stack-images/android-mac.png#lightbox)

-----

## <a name="alternative-configuration-options"></a>대체 구성 옵션

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler은 모든 관리 코드에서 구현 하는 대신 네이티브 Java/OS 코드에 위임 하는 새 처리기입니다.
**권장 되는 옵션입니다.**

#### <a name="pros"></a>전문가

- 더 나은 성능과 더 작은 실행 파일 크기에 대 한 기본 API를 사용 합니다.
- 예를 들어 최신 표준을 지원 합니다. TLS 1.2.

#### <a name="cons"></a>단점

- Android 5.0 이상이 필요합니다.
- 일부 HttpClient 기능/옵션이 제공 됩니다.

### <a name="managed-httpclienthandler"></a>관리 (HttpClientHandler)

관리 되는 처리기가 이전 Xamarin.Android 버전과 반송 하는 완전히 관리 되는 HttpClient 처리기입니다.

#### <a name="pros"></a>전문가

- 것이 가장 호환 (기능) MS.NET 및 이전 Xamarin 버전입니다.

#### <a name="cons"></a>단점

- 이 완전히 통합 되지는 않았습니다 OS를 사용 하 여 (예: 제한 된 TLS 1.0)입니다.
- (예: 일반적으로 매우 느려지는 것 암호화) 네이티브 API 보다 합니다.
- 더 큰 응용 프로그램을 만드는 관리 코드 더 필요 합니다.



### <a name="choosing-a-handler"></a>처리기를 선택합니다.

간의 선택 `AndroidClientHandler` 고 `HttpClientHandler` 응용 프로그램의 필요에 따라 달라 집니다. `AndroidClientHandler` 것이 좋습니다는 최신 보안 지원에 대 한 예입니다.

-   TLS 1.2 이상 지원 해야 합니다.
-   앱을 대상으로 Android 5.0 (API 21) 이상.
-   TLS 1.2 이상 해야 지원 `HttpClient`합니다.
-   필요가 없는 TLS 1.2 이상에 대 한 지원 `WebClient`합니다.

`HttpClientHandler` TLS 1.2 이상 필요한 경우 적합 한 선택은 지원 하지만 Android 5.0 이전 버전의 Android 지원 해야 합니다. 것도 적합 한 TLS 1.2 이상 필요한 경우에 대 한 지원 `WebClient`합니다.

Xamarin.Android 8.3 부터는 `HttpClientHandler` Boring SSL 기본값으로 (`btls`) 기본 TLS 공급자입니다. Boring SSL TLS 공급자는 다음과 같은 이점을 제공합니다.

-   TLS 1.2 이상 지원합니다.
-   모든 Android 버전을 지원합니다.
-   TLS 1.2 이상 둘 다에 대 한 지원 제공 `HttpClient` 고 `WebClient`입니다.

Boring SSL을 사용 하 여 기본 TLS 공급자의 단점은 (약 1MB 지원 되는 ABI 당 추가 APK 크기의 추가) 생성 된 APK의 크기를 늘릴 수 있는 것입니다.

Xamarin.Android 8.3 부터는 기본 TLS 공급자는 Boring SSL (`btls`). 설정 하 여 기록 관리 되는 SSL 구현에 되돌릴 수 Boring SSL을 사용 하지 않을 경우 합니다 `$(AndroidTlsProvider)` 속성을 `legacy` (빌드 속성을 설정 하는 방법에 대 한 자세한 내용은 참조 하세요. [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md)).


### <a name="programatically-using-androidclienthandler"></a>프로그래밍 방식으로 사용 하 여 `AndroidClientHandler`

`Xamarin.Android.Net.AndroidClientHandler` 되는 `HttpMessageHandler` Xamarin.Android에 맞게 구현 합니다.
이 클래스의 인스턴스는 네이티브를 사용 하 여 `java.net.URLConnection` 모든 HTTP 연결에 대 한 구현 합니다. HTTP 성능과 APK 크기가 작은 증가 하는 이론적으로 제공 됩니다.

이 코드의 단일 인스턴스에 대해 명시적으로 하는 방법의 한 예로 `HttpClient` 클래스:

```csharp
// Android 5.0 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
> 기본 Android 장치가 TLS 1.2 (즉를 지원 해야 합니다. Android 5.0 이상)


## <a name="ssltls-implementation-build-option"></a>SSL/TLS 구현 빌드 옵션

이 프로젝트 옵션 제어 모든 웹 요청에서 사용할 기본 TLS 라이브러리 둘 다 `HttpClient` 고 `WebRequest`입니다. 기본적으로 TLS 1.2 선택 됩니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio에서 TLS/SSL 구현 콤보 상자](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Mac 용 Visual Studio에서 TLS/SSL 구현 콤보 상자](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png#lightbox)

-----

예를 들어:

```csharp
var client = new HttpClient();
```

HttpClient 구현으로 설정 된 경우 **Managed** TLS 구현으로 설정 되었습니다 **네이티브 TLS 1.2 이상**, 해당 `client` 개체는 자동으로 사용 하 여 관리 되는 `HttpClientHandler` 및 TLS 1.2 (BoringSSL 라이브러리에서 제공 됨) 해당 HTTP 요청에 대 한 합니다.

그러나 경우는 **HttpClient 구현** 로 설정 된 `AndroidHttpClient`, 다음 모든 `HttpClient` 개체는 기본 Java 클래스를 사용 하 여 `java.net.URLConnection` 영향을 받지 됩니다는 **TLS/SSL 구현을** 값입니다. `WebRequest` 개체는 BoringSSL 라이브러리를 사용 합니다.

## <a name="other-ways-to-control-ssltls-configuration"></a>SSL/TLS 구성을 제어 하는 다른 방법

세 가지 방법으로 Xamarin.Android 응용 프로그램을 TLS 설정을 제어할 수 있습니다.

1. 프로젝트 옵션에서 HttpClient 구현 및 기본 TLS 라이브러리를 선택 합니다.
2. 프로그래밍 방식으로 사용 하 여 `Xamarin.Android.Net.AndroidClientHandler`입니다.
3. (선택 사항) 환경 변수를 선언 합니다.

세 가지 선택 사항, 권장 되는 방법은 Xamarin.Android 프로젝트 옵션 기본값을 선언 하는 데 `HttpMessageHandler` 및 전체 앱에 대 한 TLS 합니다. 필요한 경우 프로그래밍 방식으로 인스턴스화합니다 차례로 `Xamarin.Android.Net.AndroidClientHandler` 개체입니다. 이러한 옵션은 위에 설명 되어 있습니다.

세 번째 옵션인 &ndash; 환경 변수를 사용 하 여 &ndash; 에 대해 설명 합니다.

### <a name="declare-environment-variables"></a>환경 변수를 선언 합니다.

Xamarin.Android에는 TLS 사용에 관련 된 두 개의 환경 변수는

- `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; 이 환경 변수 선언 기본 `HttpMessageHandler` 응용 프로그램이 사용할 됩니다. 예를 들어:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

- `XA_TLS_PROVIDER` &ndash; 이 환경 변수는 TLS 라이브러리 사용 되며 하거나 선언 `btls`, `legacy`, 또는 `default` (같습니다으로이 변수를 생략).

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

이 환경 변수를 추가 하 여 설정 된 _환경 파일_ 프로젝트에. 환경 파일 빌드 작업으로는 Unix 형식의 일반 텍스트 파일은 **AndroidEnvironment**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio에서 AndroidEnvironment 빌드 작업의 스크린샷.](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![AndroidEnvironment 스크린샷 빌드 mac 용 Visual Studio에서 작업](http-stack-images/tls03-xs.png)

-----

참조 하십시오 합니다 [Xamarin.Android 환경](~/android/deploy-test/environment.md) 환경 변수 및 Xamarin.Android에 대 한 자세한 가이드입니다.


## <a name="related-links"></a>관련 링크

- [TLS(전송 계층 보안)](~/cross-platform/app-fundamentals/transport-layer-security.md)
