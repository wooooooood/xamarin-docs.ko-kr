---
title: Transport Layer Security (TLS)
description: "Android, iOS 및 Mac에서 Xamarin 프로젝트에 대 한 TLS 1.2를 사용 하도록 설정"
ms.topic: article
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/10/2017
ms.openlocfilehash: 5237ed35116e5f8983df579d0ab68363996fb06f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="transport-layer-security-tls"></a>Transport Layer Security (TLS)

_Android, iOS 및 Mac에서 Xamarin 프로젝트에 대 한 TLS 1.2를 사용 하도록 설정_

최신 버전을 사용 하 여 [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) 응용 프로그램 네트워크 통신은 보안을 확인 해야 합니다.

> [!NOTE]
> Xamarin 이후 해제 [2017 년 2 월](https://releases.xamarin.com/stable-release-cycle-9/) 기본적으로 새 프로젝트에서 TLS 1.2를 사용 합니다.

TLS 1.2 지원 제공 됩니다.

* 4.8 모노 (포함 [TLS 1.2 지원](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support))
* Xamarin.iOS
* Xamarin.Mac
* Xamarin.Android (Android 5.0 이상이 필요)

프로젝트를 참조 해야 합니다는 **System.Net.Http** 어셈블리입니다. 

## <a name="updating-to-tls-12"></a>Tls 1.2 업데이트

이 섹션에서는 업데이트할 수 있도록 일부의 Xamarin 프로젝트에서 네트워킹을 위한 구성 옵션에 설명 프로그램 _기존_ 더 안전한 프로토콜을 활용 하는 앱입니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

이러한 설정을 찾을 수 있습니다 **프로젝트 옵션 > Android 옵션** 다음를 클릭 하 고 **고급** 단추: 

[![Visual Studio에서 TLS 및 HttpClient 구성](transport-layer-security-images/properties-vs-sml.png)](transport-layer-security-images/properties-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
이러한 설정을 찾을 수 있습니다 **프로젝트 속성 > 빌드 옵션 > 고급** 탭:

[![Mac 용 Xamarin Studio 및 Visual Studio의 TLS 및 HttpClient 구성](transport-layer-security-images/properties-xs-sml.png)](transport-layer-security-images/properties-xs.png)

-----


### <a name="httpclient-implementation"></a>HttpClient 구현

그러나 Xamarin 개발자 코드의 기본 네트워킹 클래스를 사용할 수 있었던 항상, 즉는 네트워킹 스택을 결정 하는 옵션으로도 사용 되는 `HttpClient` 클래스입니다. 네이티브 플랫폼의 속도 보안 이점이 있는 친숙 한.NET API를 제공 합니다.

옵션은 다음과 같습니다.

- **관리 되는 스택** – 모노에서 제공한 네트워크 기능 또는
- **네이티브 스택을** – 다양 한 네트워킹 기본 플랫폼 (Android, iOS 또는 macOS)에서 제공 된 Api입니다.

관리 되는 스택 느려질 수 있고 더 큰 실행 파일 크기에 있지만 가장 높은 수준의 기존.NET 코드와의 호환성을 제공 합니다.

기본 옵션을 더 빠를 수 있습니다 및 향상 된 보안 (TLS 1.2 포함) 들에 게 모든 기능 및의 옵션을 제공 하지 않을 수 있습니다는 `HttpClient` 클래스입니다.


### <a name="ssltls-implementation"></a>SSL/TLS 구현

프로젝트 옵션을 사용 하는 SSL/TLS 구현을 지원 하기 위해 선택할 수 있습니다.

- **모노/관리** – Android에서 TLS 1.1, iOS 및 macOS에서 TLS 1.0입니다.
- **네이티브** – Android, iOS 및 macOS에서 TLS 1.2입니다.

하지만 새 Xamarin 프로젝트 (이것은 모든 프로젝트에 대 한 권장)는 TLS 1.2를 지 원하는 네이티브 구현으로 기본, 호환성을 위해 필요 전환할 수 있습니다는 관리 되는 코드로 경우.

> [!IMPORTANT]
> **모노/관리** 옵션에서 제거 될 예정는 [향후 릴리스에서](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/)합니다.
>
> 기본 옵션을 사용 하는 것이 좋습니다.

# <a name="platform-specific-details"></a>플랫폼 관련 세부 정보

위의 요약 Xamarin 프로젝트에서 SSL/TLS 및 HttpClient 구현에 대 한 프로젝트 수준의 설정에 설명 합니다. HttpClient 구현 코드에서 동적으로 설정할 수도 있습니다 및 iOS에는 두 가지 기본 옵션을 선택 합니다.

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS 및 Mac**](~/cross-platform/macios/http-stack.md)


# <a name="summary"></a>요약

가능한 경우 응용 프로그램 보안 TLS (전송 계층) 1.2를 사용 해야 합니다.
하지만 새로운 앱 이제 기본적으로이 구성이 문서의 지침에 따라 기존 응용 프로그램의 설정을 업데이트 해야 할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [앱의 전송 보안](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin 주기 9 (2017 년 2 월)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [4.8 모노 릴리스 정보-TLS 1.2 지원](http://www.mono-project.com/docs/about-monohttps://developer.xamarin.com/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler, 및 설명 webrequesthandler가 아닌 경우](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/en-us/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP 클라이언트 (샘플)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
