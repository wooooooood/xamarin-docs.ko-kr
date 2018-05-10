---
title: Transport Layer Security (TLS) 1.2
description: Android, iOS 및 Mac에서 Xamarin 프로젝트에 대 한 TLS 1.2를 사용 하도록 설정
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 8e27801a9feb8cf7ba1534f88479dbf7259c3e85
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="transport-layer-security-tls-12"></a>Transport Layer Security (TLS) 1.2

최신 버전을 사용 하 여 [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) 응용 프로그램 네트워크 통신은 보안을 확인 해야 합니다.

> [!WARNING]
> **년 4 월 2018** 보안 향상된으로 인해 – PCI 준수를 포함 하는 요구 사항을 주요 클라우드 공급자 및 웹 서버 지원을 TLS 버전 1.2 보다 이전 버전이 중지 해야 합니다.  이전 버전의 TLS 사용 하도록 기본 Visual Studio의 이전 버전에서 만든 Xamarin 프로젝트.
>
> 응용 프로그램으로 계속 작업 하려면 이러한 서버 및 서비스를 보장 하기 위해 **아래 설정을 사용 하 여 한 후 다시 빌드하고 다시 앱을 배포 하도록 Xamarin 프로젝트를 업데이트 해야** 사용자에 게 있습니다.

프로젝트를 참조 해야 합니다는 **System.Net.Http** 어셈블리 아래와 같이 구성 합니다.

## <a name="update-android-to-tls-12"></a>Tls 1.2 업데이트 Android

업데이트는 **HttpClient 구현** 및 **SSL/TLS 구현** TLS 1.2 보안을 사용 하는 옵션입니다.

> [!NOTE]
> Android 5.0 이상이 필요합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

이러한 설정을 찾을 수 있습니다 **프로젝트 속성 > Android 옵션** 다음를 클릭 하 고 **고급** 단추:

[![Visual Studio에서 TLS 및 HttpClient 구성](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이러한 설정을 찾을 수 있습니다 **프로젝트 옵션 > 빌드 > Android 빌드** 탭:

[![Mac 용 Visual Studio에서 TLS 및 HttpClient 구성](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-ios-to-tls-12"></a>TLS 1.2 업데이트 iOS

업데이트는 **HttpClient 구현** TSL 1.2 보안을 사용 하는 옵션입니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

이 설정을 확인할 수 있습니다 **프로젝트 속성 > iOS 빌드**:

[![Visual Studio에서 TLS 및 HttpClient 구성](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이 설정을 확인할 수 있습니다 **프로젝트 옵션 > 빌드 > iOS 빌드** 탭:

[![Mac 용 Visual Studio에서 HttpClient 구성](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-macos-to-tls-12-in-visual-studio-for-mac"></a>Mac 용 Visual Studio에서 TLS 1.2를 macOS 업데이트

업데이트는 **HttpClient 구현** 옵션 **프로젝트 옵션 > 빌드 > Mac 빌드** TSL 1.2 보안을 사용 하는 탭:

[![Mac 용 Visual Studio에서 HttpClient 구성](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

## <a name="alternative-configuration-options"></a>대체 구성 옵션

이 섹션에서는 위에 표시 된 TLS 1.2 지원 구성에 대 한 대안을 설명 합니다.
만 응용 프로그램 개발자는 서로 다른 수준의 TLS 지원이 장단점을 이해 하는 경우 이러한 대안을 고려해 야 합니다.

### <a name="httpclient-implementation"></a>HttpClient 구현

그러나 Xamarin 개발자 코드의 기본 네트워킹 클래스를 사용할 수 있었던 항상, 즉는 네트워킹 스택을 결정 하는 옵션으로도 사용 되는 `HttpClient` 클래스입니다. 네이티브 플랫폼의 속도 보안 이점이 있는 친숙 한.NET API를 제공 합니다.

옵션은 다음과 같습니다.

- **관리 되는 스택** – 모노에서 제공한 네트워크 기능 또는
- **네이티브 스택을** – 다양 한 네트워킹 기본 플랫폼 (Android, iOS 또는 macOS)에서 제공 된 Api입니다.

관리 되는 스택 느려질 수 있고 더 큰 실행 파일 크기에 있지만 가장 높은 수준의 기존.NET 코드와의 호환성을 제공 합니다.

기본 옵션을 더 빠를 수 있습니다 및 향상 된 보안 (TLS 1.2 포함) 들에 게 모든 기능 및의 옵션을 제공 하지 않을 수 있습니다는 `HttpClient` 클래스입니다.

### <a name="ssltls-implementation-android"></a>SSL/TLS 구현 (Android)

Android 프로젝트 옵션을 사용 하는 SSL/TLS 구현을 지원 하기 위해 선택할 수 있습니다.

- **모노/관리** – Android에서 TLS 1.1
- **네이티브** – Android에서 TLS 1.2입니다.

하지만 새 Xamarin 프로젝트 (이것은 모든 프로젝트에 대 한 권장)는 TLS 1.2를 지 원하는 네이티브 구현으로 기본, 호환성을 위해 필요 전환할 수 있습니다는 관리 되는 코드로 경우.

> [!IMPORTANT]
> **모노/관리** 옵션 [iOS 및 Mac에서 제거](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/) 프로젝트 옵션입니다.
>
> 기본 옵션은 iOS 및 Mac 플랫폼용에서 항상 사용 됩니다.

## <a name="platform-specific-details"></a>플랫폼 관련 세부 정보

위의 요약 Xamarin 프로젝트에서 SSL/TLS 및 HttpClient 구현에 대 한 프로젝트 수준의 설정에 설명 합니다. HttpClient 구현 코드에서 동적으로 설정할 수도 있습니다. 자세한 내용은 이러한 플랫폼 관련 가이드를 참조 하십시오.

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS 및 Mac**](~/cross-platform/macios/http-stack.md)


## <a name="summary"></a>요약

가능한 경우 응용 프로그램 보안 TLS (전송 계층) 1.2를 사용 해야 합니다.
이 문서의 지침에 따라 기존 응용 프로그램의 설정을 업데이트 한 다음 다시 빌드를 고객에 게 다시 배포 합니다.

## <a name="related-links"></a>관련 링크

- [앱 전송 보안](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin 주기 9 (2017 년 2 월)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [4.8 모노 릴리스 정보-TLS 1.2 지원](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler, 및 설명 webrequesthandler가 아닌 경우](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP 클라이언트 (샘플)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
