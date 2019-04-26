---
title: 전송 계층 보안 (TLS) 1.2
description: 이 문서에서는 설명 하는 방법을 Xamarin.iOS, Xamarin.Android 및 Xamarin.Mac 프로젝트에 대 한 TLS 1.2를 사용 하도록 설정 하려면. Mac 용 Visual Studio 2019 및 Visual Studio에서 그렇게 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 26870ae0e84a84a7b78f7766a8e134ecfc7b223e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61304810"
---
# <a name="transport-layer-security-tls-12"></a>전송 계층 보안 (TLS) 1.2

최신 버전을 사용 하 여 [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) 응용 프로그램 네트워크 통신은 보안을 확인 해야 합니다.

> [!WARNING]
> **2018 년 4 월** 보안 강화로 인해 – 요구 사항, PCI 규정 준수를 포함 하 여 주요 클라우드 공급자 및 웹 서버는 TLS 버전 1.2 보다 이전 지원 중지 해야 합니다.  이전 버전의 Visual Studio 이전 버전의 TLS 사용 하 여 기본값으로 만든 Xamarin 프로젝트입니다.
>
> 앱이 이러한 서버 및 서비스를 사용 하 여 작업을 계속할 수 있도록 **아래 설정을 사용 하 여 다시 작성 하 고 다시 앱을 배포 하 여 Xamarin 프로젝트를 업데이트 해야** 사용자에 게 합니다.

프로젝트 참조 해야 합니다는 **System.Net.Http** 어셈블리 아래와 같이 구성 합니다.

## <a name="update-xamarinandroid-to-tls-12"></a>Tls 1.2 업데이트 Xamarin.Android

업데이트를 **HttpClient 구현** 하 고 **SSL/TLS 구현** TLS 1.2 보안을 사용 하는 옵션입니다.

> [!NOTE]
> Android 5.0 이상이 필요합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

이러한 설정에서 찾을 수 있습니다 **프로젝트 속성 > Android 옵션** 클릭 하 여 **고급** 단추:

[![Visual Studio에서 TLS 및 HttpClient 구성](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이러한 설정에서 찾을 수 있습니다 **프로젝트 옵션 > 빌드 > Android 빌드** 탭:

[![Mac 용 Visual Studio에서 TLS 및 HttpClient 구성](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-xamarinios-to-tls-12"></a>Xamarin.iOS TLS 1.2 업데이트

업데이트를 **HttpClient 구현** TSL 1.2 보안을 사용 하는 옵션입니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

이 설정에서 찾을 수 있습니다 **프로젝트 속성 > iOS 빌드**:

[![Visual Studio에서 TLS 및 HttpClient 구성](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이 설정에서 찾을 수 있습니다 **프로젝트 옵션 > 빌드 > iOS 빌드** 탭:

[![Mac 용 Visual Studio에서 HttpClient 구성](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-xamarinmac-to-tls-12"></a>Update Xamarin.Mac to TLS 1.2

Mac 용 Visual Studio에서 업데이트에 Xamarin.Mac 앱에서 TLS 1.2를 사용 하도록 설정 합니다 **HttpClient 구현** 옵션 **프로젝트 옵션 > 빌드 > Mac 빌드**:

[![Mac 용 Visual Studio에서 HttpClient 구성](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

> [!WARNING]
> 예정된 Xamarin.Mac 4.8 릴리스는 macOS 10.9 이상만 지원합니다.
> 이전 버전의 Xamarin.Mac은 macOS 10.7 이상을 지원했지만 이러한 이전 macOS 버전에는 TLS 1.2를 지원할 수 있는 TLS 인프라가 충분하지 않습니다. macOS 10.7 또는 macOS 10.8을 대상으로 하려면 Xamarin.Mac 4.6 또는 이전 버전을 사용하세요.

## <a name="alternative-configuration-options"></a>대체 구성 옵션

이 섹션에서는 위에 표시 된 TLS 1.2 지원 구성에 대 한 대안을 설명 합니다.
만 응용 프로그램 개발자는 TLS 지원의 차이 사용 하 여 위험을 이해 하는 경우 이러한 대안을 고려해 야 합니다.

### <a name="httpclient-implementation"></a>HttpClient 구현

Xamarin 개발자가 코드에서 네이티브 네트워킹 클래스를 사용할 수 있게 되었습니다 항상, 그러나는 네트워킹 스택에서 결정 하는 옵션 에서도 사용 되는 `HttpClient` 클래스입니다. 네이티브 플랫폼의 속도 보안 이점이 있는 친숙 한.NET API를 제공 합니다.

옵션은 다음과 같습니다.

- **관리 되는 스택** – Mono 제공한 네트워크 기능 또는
- **네이티브 스택을** – 다양 한 네트워킹 기본 플랫폼 (Android, iOS 또는 macOS)에서 제공 하는 Api입니다.

하지만 관리 되는 스택 느려집니다 하 고 실행 파일 크기가 커질 수 있습니다 가장 높은 수준의 기존.NET 코드와의 호환성을 제공 합니다.

기본 옵션은 더 빠를 수 있습니다 및 향상 된 보안 (TLS 1.2 포함)에 있지만 모든 기능 및 옵션을 제공할 수 없습니다는 `HttpClient` 클래스입니다.

### <a name="ssltls-implementation-android"></a>SSL/TLS 구현 (Android)

Android 프로젝트 옵션을 선택할 수 있는 SSL/TLS 구현 지원:

- **Mono/관리** – Android에서 TLS 1.1
- **네이티브** – Android에서 TLS 1.2입니다.

하지만 새 Xamarin 프로젝트 기본값 (모든 프로젝트에 대 한 권장)는 TLS 1.2를 지 원하는 네이티브 구현, 호환성을 위해 필요 경우 전환할 수 있습니다 하 여 관리 코드로 다시 합니다.

> [!IMPORTANT]
> 합니다 **Mono/Managed** 옵션 [iOS 및 Mac에서 제거](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/) 프로젝트 옵션입니다.
>
> IOS 및 Mac 플랫폼에서 기본 옵션을 항상 사용 됩니다.

## <a name="platform-specific-details"></a>플랫폼 관련 세부 정보

위의 요약 Xamarin 프로젝트에서 SSL/TLS 및 HttpClient 구현에 대 한 프로젝트 수준 설정에 설명 합니다. HttpClient 구현 코드에서 동적으로 설정할 수도 있습니다. 자세한 내용은 다음 플랫폼별 가이드를 참조 하세요.

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS 및 Mac**](~/cross-platform/macios/http-stack.md)

## <a name="summary"></a>요약

가능한 경우 항상 응용 프로그램에서 전송 계층 보안 (TLS) 1.2를 사용 해야 합니다.
이 문서의 지침에 따라 기존 응용 프로그램의 설정을 업데이트 하 고 다시 작성 하 고, 고객에 게 다시 배포 해야 합니다.

## <a name="related-links"></a>관련 링크

- [앱 전송 보안](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin Cycle 9 (2017 년 2 월)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Mono 4.8 릴리스 정보-TLS 1.2 지원](https://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler, 및 WebRequestHandler 설명](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](https://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](xref:CoreFoundation.CFNetwork)
- [Foundation.NSUrlConnection](xref:Foundation.NSUrlConnection)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP 클라이언트 (샘플)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
