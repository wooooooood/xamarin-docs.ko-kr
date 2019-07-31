---
title: TLS (Transport Layer Security) 1.2
description: 이 문서에서는 Xamarin.ios, Xamarin Android 및 Xamarin.ios 프로젝트용 TLS 1.2을 사용 하도록 설정 하는 방법을 설명 합니다. Visual Studio 2019 및 Mac용 Visual Studio에서이 작업을 수행 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 639a62316d718534677b0ae86f9e5b57791c23e1
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649223"
---
# <a name="transport-layer-security-tls-12"></a>TLS (Transport Layer Security) 1.2

최신 버전의 [TLS ( _Transport Layer Security_ )](https://en.wikipedia.org/wiki/Transport_Layer_Security) 를 사용 하는 것은 응용 프로그램 네트워크 통신의 보안을 유지 하는 데 중요 합니다.

> [!WARNING]
> **4 월, 2018** – PCI 규정 준수를 포함 하 여 보안 요구 사항이 증가 함에 따라 주요 클라우드 공급자 및 웹 서버는 1.2 보다 오래 된 TLS 버전 지원을 중지 해야 합니다.  이전 버전의 Visual Studio에서 만든 Xamarin 프로젝트는 이전 버전의 TLS를 사용 합니다.
>
> 앱이 이러한 서버 및 서비스와 계속 작동 하는지 확인 하려면 **Xamarin 프로젝트를 업데이트 하 여 아래 설정을 사용 하 고 앱을 다시 빌드하고 다시 배포** 하 여 사용자에 게 배포 해야 합니다.

프로젝트는 **System .net. Http** 어셈블리를 참조 하 고 아래와 같이 구성 해야 합니다.

## <a name="update-xamarinandroid-to-tls-12"></a>Xamarin Android를 TLS 1.2로 업데이트 합니다.

**Httpclient 구현** 및 **SSL/tls 구현** 옵션을 업데이트 하 여 TLS 1.2 보안을 사용 하도록 설정 합니다.

> [!NOTE]
> Android 5.0 이상이 필요 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

이러한 설정은 **프로젝트 속성에서 Android 옵션 >** 다음 **고급** 단추를 클릭 하 여 찾을 수 있습니다.

[![Visual Studio에서 HttpClient 및 TLS 구성](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이러한 설정은 **프로젝트 옵션 > 빌드 > Android 빌드** 탭에서 찾을 수 있습니다.

[![Mac용 Visual Studio에서 HttpClient 및 TLS 구성](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-xamarinios-to-tls-12"></a>Xamarin.ios를 TLS 1.2로 업데이트 합니다.

**Httpclient 구현** 옵션을 업데이트 하 여 TSL 1.2 보안을 사용 하도록 설정 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

이 설정은 **프로젝트 속성 > IOS 빌드**에서 찾을 수 있습니다.

[![Visual Studio에서 HttpClient 및 TLS 구성](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이 설정은 **프로젝트 옵션 > 빌드 > IOS 빌드** 탭에서 찾을 수 있습니다.

[![Mac용 Visual Studio에서 HttpClient 구성](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-xamarinmac-to-tls-12"></a>Xamarin.ios를 TLS 1.2로 업데이트 합니다.

Mac용 Visual Studio에서 Xamarin.ios 앱에 TLS 1.2을 사용 하도록 설정 하려면 Project 옵션에서 **Httpclient 구현** 옵션을 업데이트 **> Mac 빌드 >** 합니다.

[![Mac용 Visual Studio에서 HttpClient 구성](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

> [!WARNING]
> 예정된 Xamarin.Mac 4.8 릴리스는 macOS 10.9 이상만 지원합니다.
> 이전 버전의 Xamarin.Mac은 macOS 10.7 이상을 지원했지만 이러한 이전 macOS 버전에는 TLS 1.2를 지원할 수 있는 TLS 인프라가 충분하지 않습니다. macOS 10.7 또는 macOS 10.8을 대상으로 하려면 Xamarin.Mac 4.6 또는 이전 버전을 사용하세요.

## <a name="alternative-configuration-options"></a>대체 구성 옵션

이 섹션에서는 위에 표시 된 TLS 1.2 지원 구성의 대안을 설명 합니다.
응용 프로그램 개발자는 다양 한 수준의 TLS 지원을 사용할 경우의 위험을 이해 하는 경우에만 이러한 대안을 고려해 야 합니다.

### <a name="httpclient-implementation"></a>HttpClient 구현

Xamarin 개발자는 항상 네이티브 네트워킹 클래스를 코드에 사용할 수 있지만 `HttpClient` 클래스에서 사용 되는 네트워킹 스택을 결정 하는 옵션도 있습니다. 이는 네이티브 플랫폼의 속도와 보안상 이점이 있는 친숙 한 .NET API를 제공 합니다.

다음 옵션을 사용할 수 있습니다.

- **관리 되는 스택** – Mono에서 제공 되는 네트워크 기능 또는
- **Native stack** – 기본 플랫폼 (Android, IOS 또는 macos)에서 제공 하는 다양 한 네트워킹 api입니다.

관리 되는 스택은 기존 .NET 코드와 가장 높은 수준의 호환성을 제공 하지만 속도가 느려지고 실행 파일 크기가 커질 수 있습니다.

기본 옵션은 더 빠르고 보안 (TLS 1.2 포함)을 사용할 수 있지만 `HttpClient` 클래스의 일부 기능과 옵션은 제공 하지 않을 수 있습니다.

### <a name="ssltls-implementation-android"></a>SSL/TLS 구현 (Android)

Android 프로젝트 옵션을 통해 지원 되는 SSL/TLS 구현도 선택할 수 있습니다.

- **Mono/Managed** – Android에서 TLS 1.1
- **Native** – Android에서 TLS 1.2.

새 Xamarin 프로젝트는 기본적으로 모든 프로젝트에 대해 권장 되는 TLS 1.2을 지 원하는 네이티브 구현에 대 한 기본 구현 이지만, 호환성을 위해 필요한 경우 관리 코드로 다시 전환할 수 있습니다.

> [!IMPORTANT]
> **Mono/Managed** 옵션이 [iOS 및 Mac 프로젝트 옵션에서 제거](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_10/xamarin.ios_10.8.md) 되었습니다.
>
> 기본 옵션은 iOS 및 Mac 플랫폼에서 항상 사용 됩니다.

## <a name="platform-specific-details"></a>플랫폼별 세부 정보

위의 요약은 Xamarin 프로젝트의 HttpClient 및 SSL/TLS 구현에 대 한 프로젝트 수준 설정을 설명 합니다. HttpClient 구현은 코드에서 동적으로 설정할 수도 있습니다. 자세한 내용은 다음 플랫폼별 가이드를 참조 하세요.

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS 및 Mac**](~/cross-platform/macios/http-stack.md)

## <a name="summary"></a>요약

응용 프로그램은 가능 하면 TLS (Transport Layer Security) 1.2를 사용 해야 합니다.
이 문서의 지침에 따라 기존 응용 프로그램의 설정을 업데이트 한 후 고객에 게 다시 빌드하고 다시 배포 해야 합니다.

## <a name="related-links"></a>관련 링크

- [앱 전송 보안](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin Cycle 9 (2 월 2017 일)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (위키백과)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Mono 4.8 릴리스 정보-TLS 1.2 지원](https://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler 및 WebRequestHandler 설명](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
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
- [HTTP 클라이언트 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/httpclient/)
