---
title: HttpClient 스택 및 Android에 대 한 SSL/TLS 구현 선택기
description: HttpClient 스택 및 SSL/TLS 구현 선택기 Xamarin.Android 앱에서 사용 됩니다 하는 SSL/TLS 및 HttpClient 구현을 결정 합니다.
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/20/2018
ms.openlocfilehash: 765c51346ac63a00838fec52bde87b38091e2dd9
ms.sourcegitcommit: a4c2a63ba76b839cda99e4474e7ab46fe307cd39
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34689476"
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>HttpClient 스택 및 Android에 대 한 SSL/TLS 구현 선택기

HttpClient 스택 및 SSL/TLS 구현 선택기 Xamarin.Android 앱에서 사용 됩니다 하는 SSL/TLS 및 HttpClient 구현을 결정 합니다.

프로젝트를 참조 해야 합니다는 **System.Net.Http** 어셈블리입니다.

> [!WARNING]
> **년 4 월 2018** 보안 향상된으로 인해 – PCI 준수를 포함 하는 요구 사항을 주요 클라우드 공급자 및 웹 서버 지원을 TLS 버전 1.2 보다 이전 버전이 중지 해야 합니다.  이전 버전의 TLS 사용 하도록 기본 Visual Studio의 이전 버전에서 만든 Xamarin 프로젝트.
>
> 응용 프로그램으로 계속 작업 하려면 이러한 서버 및 서비스를 보장 하기 위해 **Xamarin 프로젝트를 업데이트 해야는 `Android HttpClient` 및 `Native TLS 1.2` 설정 아래 표시 된 다음 다시 빌드하고 다시 배포 앱** 하 여 사용자입니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin.Android HttpClient 구성은 **프로젝트 옵션 > Android 옵션**, 클릭는 **고급 옵션** 단추입니다.

TLS 1.2 지원에 대 한 권장된 설정은 다음과 같습니다.

[![Visual Studio의 Android 옵션](http-stack-images/android-win-sml.png)](http-stack-images/android-win.png#lightbox)


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Xamarin.Android HttpClient 구성은 **프로젝트 옵션 > 빌드 > Android 빌드** 설정과 클릭은 **일반** 탭 합니다.

TLS 1.2 지원에 대 한 권장된 설정은 다음과 같습니다.

[![Mac Android 옵션에 대 한 visual Studio](http-stack-images/android-mac-sml.png)](http-stack-images/android-mac.png#lightbox)

-----

## <a name="alternative-configuration-options"></a>대체 구성 옵션

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler는 관리 코드에서 모든 항목을 구현 하는 대신 네이티브 Java/OS 코드에 위임 하는 새 처리기입니다.
**이것이 권장 되는 옵션입니다.**

#### <a name="pros"></a>전문가

- 더 나은 성능과 더 작은 실행 파일 크기에 대 한 네이티브 API를 사용 합니다.
- 예: 최신 표준에 대 한 지원. TLS 1.2.

#### <a name="cons"></a>단점

- Android 5.0 이상이 필요합니다.
- 일부 HttpClient 기능/옵션은 사용할 수 없습니다.

### <a name="managed-httpclienthandler"></a>관리 되는 (HttpClientHandler)

관리 되는 처리기가 이전 Xamarin.Android 버전과 선적 HttpClient 완전히 관리 되는 처리기.

#### <a name="pros"></a>전문가

- 가장 (기능) 방식으로 호환 MS.NET 및 이전 Xamarin 버전 않습니다.

#### <a name="cons"></a>단점

- 하지 완벽 하 게 통합 되어 운영 체제 (예:. TLS 1.0 제한) 됩니다.
- (예: 일반적으로 훨씬 더 느리기 되었기. 암호화) 네이티브 API 보다 합니다.
- 더 큰 응용 프로그램을 만드는 관리 코드 더 필요 합니다.



### <a name="choosing-a-handler"></a>처리기를 선택합니다.

간의 선택 `AndroidClientHandler` 및 `HttpClientHandler` 응용 프로그램의 필요에 따라 달라 집니다. `AndroidClientHandler` 좋습니다 최신 보안 지원에 대 한 예입니다.

-   TLS 1.2 + 지원 해야 합니다.
-   앱은 Android 5.0 (API 21)를 대상으로 이상.
-   TLS 1.2 + 필요한에 대 한 지원 `HttpClient`합니다.
-   필요 하지 않습니다 TLS 1.2 +에 대 한 지원 `WebClient`합니다.

`HttpClientHandler` TLS 1.2 + 해야 할 경우에 적합 한 선택은 지원 하지만 Android 5.0 이전 Android 버전을 지원 해야 합니다. 것도 좋은 선택 TLS 1.2 + 해야 할 경우에 대 한 지원 `WebClient`합니다.

Xamarin.Android 8.3 부터는 `HttpClientHandler` 지루한 SSL에는 기본값 (`btls`) 기본 TLS 공급자로 합니다. SSL TLS 지루한 공급자에는 다음과 같은 이점이 있습니다.

-   TLS 1.2 +을 지원합니다.
-   모든 Android 버전을 지원합니다.
-   TLS 1.2 + 모두에 대 한 지원 제공 `HttpClient` 및 `WebClient`합니다.

지루한 SSL을 사용 하 여 기본 TLS 공급자로 단점은 결과 APK (지원 되는 ABI 당 추가 APK 크기의 약 1MB 추가)의 크기를 늘릴 수 있는 것입니다.

Xamarin.Android 8.3부터, 기본 TLS 공급자는 SSL 지루한 (`btls`). 설정 하 여 기록 관리 되는 SSL 구현으로 되돌릴 수 지루한 SSL을 사용 하지 않을 경우는 `$(AndroidTlsProvider)` 속성을 `legacy` (빌드 속성을 설정 하는 방법에 대 한 자세한 내용은 참조 [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md)).


### <a name="programatically-using-androidclienthandler"></a>프로그래밍 방식으로 사용 하 여 `AndroidClientHandler`

`Xamarin.Android.Net.AndroidClientHandler` 는 `HttpMessageHandler` Xamarin.Android에 대 한 구체적으로 구현 합니다.
이 클래스의 인스턴스는 네이티브 ´ ֲ `java.net.URLConnection` 모든 HTTP 연결에 대 한 구현 합니다. 이론적으로 HTTP 성능 및 더 작은 APK 크기 증가 제공 합니다.

이 코드 조각은의 단일 인스턴스를 위해 명시적으로 하는 방법의 예는 `HttpClient` 클래스:

```csharp
// Android 5.0 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
> 기본 Android 장치 (즉, TLS 1.2를 지원 해야 합니다. Android 5.0 이상)


## <a name="ssltls-implementation-build-option"></a>SSL/TLS 구현 빌드 옵션

이 프로젝트 옵션은 모든 웹 요청에서 사용 될 기본 TLS 라이브러리 제어 둘 다 `HttpClient` 및 `WebRequest`합니다. 기본적으로 TLS 1.2 선택 됩니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio에서 TLS/SSL 구현 콤보 상자](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Mac 용 Visual Studio에서 TLS/SSL 구현 콤보 상자](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png#lightbox)

-----

예를 들어:

```csharp
var client = new HttpClient();
```

HttpClient 구현으로 설정 된 경우 **관리** 있고 TLS 구현은로 설정 되었다면 **네이티브 TLS 1.2 +**, 하면 `client` 개체는 자동으로 사용 하 여 관리 되는 `HttpClientHandler` 및 TLS 1.2 (BoringSSL 라이브러리에서 제공)의 HTTP 요청에 대 한 합니다.

그러나 경우는 **HttpClient 구현** 로 설정 된 `AndroidHttpClient`, 다음 모든 `HttpClient` 개체는 기본 Java 클래스를 사용 합니다 `java.net.URLConnection` 영향을 받지 됩니다는 **TLS/SSL 구현을** 값입니다. `WebRequest` 개체는 BoringSSL 라이브러리를 사용 합니다.

## <a name="other-ways-to-control-ssltls-configuration"></a>SSL/TLS 구성 제어 하는 다른 방법

세 가지 방법으로 Xamarin.Android 응용 프로그램이 TLS 설정을 제어할 수 있습니다.

1. 프로젝트 옵션에서 HttpClient 구현 및 기본 TLS 라이브러리를 선택 합니다.
2. 프로그래밍 방식으로 사용 하 여 `Xamarin.Android.Net.AndroidClientHandler`합니다.
3. (선택 사항) 환경 변수를 선언 합니다.

세 가지 선택 항목의 기본 선언 하려면 Xamarin.Android 프로젝트 옵션을 사용 하는 권장된 방법입니다 `HttpMessageHandler` 및 전체 응용 프로그램을 위한 TLS 합니다. 필요한 경우 프로그래밍 방식으로 인스턴스화합니다 그런 다음 `Xamarin.Android.Net.AndroidClientHandler` 개체입니다. 이러한 옵션은 위에 설명 되어 있습니다.

세 번째 옵션 &ndash; 환경 변수를 사용 하 여 &ndash; 방법에 대해 설명 합니다.

### <a name="declare-environment-variables"></a>환경 변수를 선언 합니다.

Xamarin.Android에 TLS의 사용과 관련 된 두 개의 환경 변수 가지가 있습니다.

- `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; 이 환경 변수 선언에서 기본 `HttpMessageHandler` 응용 프로그램에서 사용할 합니다. 예를 들어:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

- `XA_TLS_PROVIDER` &ndash; 이 환경 변수는 TLS 라이브러리 사용 되며 하거나 선언 `btls`, `legacy`, 또는 `default` (되이 변수를 생략 하는 것과 동일):

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

이 환경 변수를 추가 하 여 설정 되는 _환경 파일_ 프로젝트에 있습니다. 환경 파일은 빌드 작업으로 Unix 형식 일반 텍스트 파일 **AndroidEnvironment**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Visual Studio에서 AndroidEnvironment 빌드 작업의 스크린 샷](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![AndroidEnvironment의 스크린 샷 빌드 Mac.에 대 한 Visual Studio에서 작업](http-stack-images/tls03-xs.png)

-----

참조 하십시오는 [Xamarin.Android 환경](~/android/deploy-test/environment.md) 환경 변수 및 Xamarin.Android에 대 한 자세한 내용은 가이드입니다.


## <a name="related-links"></a>관련 링크

- [TLS(전송 계층 보안)](~/cross-platform/app-fundamentals/transport-layer-security.md)
