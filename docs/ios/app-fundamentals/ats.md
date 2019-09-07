---
title: Xamarin.ios의 앱 전송 보안
description: 'ATS (app Transport Security)는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결을 적용 합니다.'
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/13/2017
ms.openlocfilehash: 039a73b45f93525631635a9a73bf153c7938bc92
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766672"
---
# <a name="app-transport-security-in-xamarinios"></a>Xamarin.ios의 앱 전송 보안

_ATS (app Transport Security)는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결을 적용 합니다._

이 문서에서는 앱 전송 보안이 iOS 9 앱에서 적용 하는 보안 변경 내용 및이에 해당 하는 [ATS 구성 옵션](#config) 을 [설명](#xamarinsupport)하 고 [ATS를 옵트아웃](#optout) 하는 방법을 설명 합니다. 필요한 경우 ATS. ATS은 기본적으로 사용 하도록 설정 되어 있으므로 안전 하지 않은 모든 인터넷 연결이 iOS 9 앱에서 예외를 발생 시킵니다 (명시적으로 허용 하지 않는 한).

## <a name="about-app-transport-security"></a>앱 전송 보안 정보

위에서 설명한 것 처럼 ATS는 iOS 9 및 OS X El Capitan의 모든 인터넷 통신이 안전한 연결 모범 사례를 준수 하 여 앱 또는 해당 앱이 있는 라이브러리를 통해 직접 중요 한 정보가 노출 되지 않도록 방지 합니다. 필요로.

기존 앱의 경우 가능 하면 `HTTPS` 항상 프로토콜을 구현 합니다. 새 xamarin.ios 앱의 경우 인터넷 리소스와 통신할 때 `HTTPS` 독점적으로를 사용 해야 합니다. 또한 높은 수준의 API 통신은 TLS 버전 1.2을 사용 하 여 암호화 해야 합니다.

[NSUrlConnection](xref:Foundation.NSUrlConnection), [Cfurl](xref:CoreFoundation.CFUrl) 또는 [NSUrlSession](xref:Foundation.NSUrlSession) 를 사용 하 여 만든 모든 연결은 iOS 9 및 OS X 10.11 (El Capitan) 용으로 빌드된 앱에서 기본적으로 ATS을 사용 합니다.

## <a name="default-ats-behavior"></a>기본 ATS 동작

ATS는 iOS 9 및 OS X 10.11 (El Capitan) 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되어 있으므로 [NSUrlConnection](xref:Foundation.NSUrlConnection), [Cfurl](xref:CoreFoundation.CFUrl) 또는 [NSUrlSession](xref:Foundation.NSUrlSession) 를 사용 하는 모든 연결에는 ATS 보안 요구 사항이 적용 됩니다. 연결이 이러한 요구 사항을 충족 하지 않는 경우에는 예외와 함께 실패 합니다.

### <a name="ats-connection-requirements"></a>ATS 연결 요구 사항

ATS는 모든 인터넷 연결에 대해 다음과 같은 요구 사항을 적용 합니다.

- 모든 연결 암호화는 전달 보안을 사용 해야 합니다. 아래에서 허용 되는 암호화 목록을 참조 하십시오.
- TLS (전송 계층 보안) 프로토콜은 버전 1.2 이상 이어야 합니다.
- 2048 비트 이상 RSA 키가 있는 SHA256 지문을 하나 이상 사용 하거나 모든 인증서에 256 비트 이상 ECC (타원 Curve) 키를 사용 해야 합니다.

ATS가 iOS 9에서 기본적으로 사용 하도록 설정 되어 있기 때문에 이러한 요구 사항을 충족 하지 않는 연결을 시도 하면 예외가 throw 됩니다.

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>ATS 호환 암호화

ATS 보안 인터넷 통신에서 다음과 같은 전달 보안 암호화 유형을 허용 합니다.

- `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA`
- `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`

IOS 인터넷 통신 클래스를 사용 하는 방법에 대 한 자세한 내용은 Apple의 [NSURLConnection 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755), [Cfurl 참조](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) 또는 [NSURLSession 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435)를 참조 하세요.

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Xamarin.ios에서 ATS 지원

ATS는 iOS 9 및 OS X El Capitan에서 기본적으로 사용 하도록 설정 되어 있기 때문에 Xamarin.ios 앱 또는 사용 중인 라이브러리나 서비스에서 인터넷에 연결 하는 경우 몇 가지 작업을 수행 해야 합니다. 그렇지 않으면 연결 시 예외가 throw 됩니다.

기존 앱의 경우 Apple은 `HTTPS` 가능한 한 빨리 프로토콜을 지 원하는 것으로 제안 합니다. 지원 `HTTPS` 하지 않는 타사 웹 서비스에 연결 하거나 지원 `HTTPS` 하지 않는 타사 웹 서비스에 연결 하는 것이 불가능 한 경우 ATS에서 옵트아웃 (opt out) 할 수 있습니다. 자세한 내용은 아래의 [ATS에서 옵트아웃 (옵트아웃)](#optout) 섹션을 참조 하세요.

새 xamarin.ios 앱의 경우 인터넷 리소스와 통신할 때 독점적 `HTTPS` 으로를 사용 해야 합니다. 이는 가능 하지 않으며 ATS 옵트아웃 (opt out)을 수행 해야 하는 상황이 발생할 수 있습니다 (예: 타사 웹 서비스 사용).

또한 ATS는 TLS 버전 1.2을 사용 하 여 높은 수준의 API 통신을 전달 하는 보안을 적용 합니다. 자세한 내용은 위의 [ATS Connection 요구 사항](#ats-connection-requirements) 및 [ATS Compatible 암호](#ats-compatible-ciphers) 섹션을 참조 하세요.

TLS ([전송 계층 보안](https://en.wikipedia.org/wiki/Transport_Layer_Security))에 익숙하지 않을 수 있지만 SSL ([Secure Socket layer](https://en.wikipedia.org/wiki/Transport_Layer_Security))의 후속 작업 이며 네트워크 연결을 통해 보안을 적용 하기 위한 암호화 프로토콜의 컬렉션을 제공 합니다.

TLS 수준은 사용 하는 웹 서비스에 의해 제어 되므로 응용 프로그램의 컨트롤 외부에 있습니다. 및는 모두 서버에서 지 원하는 가장 높은 수준의 TLS 암호화를 자동으로 사용 합니다.`ModernHttpClient` `HttpClient`

통신 하는 서버 (특히 타사 서비스인 경우)에 따라 전달 보안을 사용 하지 않도록 설정 하거나 낮은 TLS 수준을 선택 해야 할 수 있습니다. 자세한 내용은 아래의 [ATS 옵션 구성](#configuring-ats-options) 섹션을 참조 하세요.

> [!IMPORTANT]
> 앱 전송 보안은 **관리 되는 HTTPClient 구현을**사용 하는 Xamarin 앱에는 적용 되지 않습니다. 이는 CFNetwork **Httpclient 구현을** 사용 하는 연결 또는 **NSURLSession httpclient 구현** 에만 적용 됩니다.

### <a name="setting-the-httpclient-implementation"></a>HTTPClient 구현 설정

IOS 앱에서 사용 하는 HTTPClient 구현을 설정 하려면 **솔루션 탐색기** 에서 **프로젝트** 를 두 번 클릭 하 여 **프로젝트 옵션**을 엽니다. **IOS 빌드** 로 이동 하 여 **httpclient 구현** 드롭다운에서 원하는 클라이언트 형식을 선택 합니다.

![](ats-images/client01.png "IOS 빌드 옵션 설정")

#### <a name="managed-handler"></a>관리 되는 처리기

관리 되는 처리기는 이전 버전의 Xamarin.ios와 함께 제공 되는 완전히 관리 되는 HttpClient 처리기 이며 기본 처리기입니다.

들

- 이는 Microsoft .NET 및 이전 버전의 Xamarin과 가장 호환 됩니다.

단점

- IOS와 완전히 통합 되지 않습니다 (예: TLS 1.0로 제한 됨).
- 일반적으로 기본 Api 보다 훨씬 느립니다.
- 더 많은 관리 코드가 필요 하 고 더 큰 앱이 생성 됩니다.

#### <a name="cfnetwork-handler"></a>CFNetwork Handler

Cfnetwork 기반 처리기는 네이티브 `CFNetwork` 프레임 워크를 기반으로 합니다.

들

- 는 더 나은 성능과 더 작은 실행 파일 크기를 위해 기본 API를 사용 합니다.
- TLS 1.2와 같은 최신 표준에 대 한 지원을 추가 합니다.

단점

- IOS 6 이상이 필요 합니다.
- WatchOS에서 사용할 수 없습니다.
- 일부 HttpClient 기능 및 옵션을 사용할 수 없습니다.

#### <a name="nsurlsession-handler"></a>NSUrlSession 처리기

NSUrlSession 기반 처리기는 네이티브 `NSUrlSession` API를 기반으로 합니다.

들

- 는 더 나은 성능과 더 작은 실행 파일 크기를 위해 기본 API를 사용 합니다.
- TLS 1.2와 같은 최신 표준에 대 한 지원을 추가 합니다.

단점

- IOS 7 이상이 필요 합니다.
- 일부 HttpClient 기능 및 옵션을 사용할 수 없습니다.

## <a name="diagnosing-ats-issues"></a>ATS 문제 진단

직접 또는 iOS 9의 웹 보기에서 인터넷에 연결 하려고 할 때 다음과 같은 형식으로 오류가 발생할 수 있습니다.

> 앱 전송 보안이 안전 하지 않으므로 일반 텍스트 HTTP http://www.-the-blocked-domain.com) (리소스 로드)를 차단 했습니다. 임시 예외는 앱의 info.plist 파일을 통해 구성할 수 있습니다.

IOS9에서 ATS (App Transport Security)는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간 보안 연결을 적용 합니다. 또한 ATS를 사용 하려면 프로토콜 및 `HTTPS` 높은 수준의 API 통신을 사용 하는 통신이 TLS 버전 1.2을 사용 하 여 암호화 되어야 합니다.

ATS는 iOS 9 및 OS X 10.11 (El Capitan) 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되므로 또는 `NSURLConnection` `NSURLSession` 를 `CFURL` 사용 하는 모든 연결에는 ATS 보안 요구 사항이 적용 됩니다. 연결이 이러한 요구 사항을 충족 하지 않는 경우에는 예외와 함께 실패 합니다.

또한 Apple은 컴파일되어 ATS/TLS 문제를 진단 하는 데 사용할 수 있는 [TLSTool 샘플 앱](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) (또는 선택적으로 Xamarin 및 C#로 트랜스 코딩)을 제공 합니다. 이 문제를 해결 하는 방법에 대 한 자세한 내용은 아래 [ATS의 옵트아웃 (옵트아웃)](#optout) 섹션을 참조 하세요.

<a name="config" />

## <a name="configuring-ats-options"></a>ATS 옵션 구성

앱의 **info.plist** 파일에서 특정 키에 대 한 값을 설정 하 여 ATS의 여러 기능을 구성할 수 있습니다. 다음 키는 ATS 제어 하는 데 사용할 수 있습니다 (_중첩 된 방법을 표시 하도록 들여쓰기_됨).

```
NSAppTransportSecurity
    NSAllowsArbitraryLoads
    NSAllowsArbitraryLoadsInWebContent
    NSExceptionDomains
    <domain-name-for-exception-as-string>
        NSExceptionMinimumTLSVersion
        NSExceptionRequiresForwardSecrecy
        NSExceptionAllowsInsecureHTTPLoads
        NSRequiresCertificateTransparency
        NSIncludesSubdomains
        NSThirdPartyExceptionMinimumTLSVersion
        NSThirdPartyExceptionRequiresForwardSecrecy
        NSThirdPartyExceptionAllowsInsecureHTTPLoads
```

각 키의 형식과 의미는 다음과 같습니다.

- **Nsapp로 보안** (`Dictionary`)-ATS에 대 한 모든 설정 키 및 값을 포함 합니다.
- **NSAllowsArbitraryLoads** (`Boolean` `NSExceptionDomains`) `YES` -ATS가에 나열 **되지 않은** 모든 도메인에 대해 사용 하지 않도록 설정 됩니다. 나열 된 도메인에 대해 지정 된 보안 설정이 사용 됩니다.
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean` )`YES` -앱의 나머지 부분에 대해 ATS (Apple Transport Security) 보호를 사용 하는 동안 웹 페이지를 올바르게 로드할 수 있습니다.
- **NSExceptionDomains** (`Dictionary`)-지정 된 도메인에 대해 ATS가 사용 해야 하는 보안 설정 및 도메인의 컬렉션입니다.
- `Dictionary`  **도메인이름-예외-문자열>()-지정된도메인에대한\<** 예외 컬렉션 (예: `www.xamarin.com`).
- **NSExceptionMinimumTLSVersion** (`String`)-최소 TLS 버전 `TLSv1.0` `TLSv1.1` `TLSv1.2` (기본값)입니다.
- **NSExceptionRequiresForwardSecrecy** (`Boolean`)-도메인이 `NO` 전달 보안과 함께 암호를 사용할 필요가 없는 경우 기본값은 `YES`입니다.
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`) `HTTPS` -(기본값)이 도메인과의 모든 통신은 프로토콜에 있어야 합니다. `NO`
- **NSRequiresCertificateTransparency** (`Boolean`)-도메인 `YES` 의 SSL(Secure Sockets Layer) (SSL)에 유효한 투명도 데이터가 포함 되어야 하는 경우 기본값은 `NO`입니다.
- **NSIncludesSubdomains** (`Boolean`)-이러한 `YES` 설정이이 도메인의 모든 하위 도메인을 재정의 하는 경우 기본값은 `NO`입니다.
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`)-도메인이 개발자의 제어를 벗어난 타사 서비스인 경우 사용 되는 TLS 버전입니다.
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`)-타사 `YES` 도메인에 전달 보안이 필요한 경우
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`)-ATS `YES` 에서 타사 도메인과 안전 하지 않은 통신을 허용 하면입니다.

<a name="optout" />

### <a name="opting-out-of-ats"></a>ATS의 옵트아웃

Apple에서는 `HTTPS` 프로토콜을 사용 하 여 인터넷 기반 정보에 대 한 안전한 통신을 제안 하지만 항상 가능 하지는 않을 수 있습니다. 예를 들어 타사 웹 서비스와 통신 하거나 앱에서 인터넷에 배달 된 광고를 사용 하는 경우입니다.

Xamarin.ios 앱이 보안 되지 않은 도메인에 대 한 요청을 수행 해야 하는 경우 앱의 **info.plist** 파일을 다음과 같이 변경 하면 ATS가 지정 된 도메인에 대해 적용 하는 보안 기본값을 사용 하지 않도록 설정 됩니다.

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>www.the-domain-name.com</key>
        <dict>
            <key>NSExceptionMinimumTLSVersion</key>
            <string>TLSv1.0</string>
            <key>NSExceptionRequiresForwardSecrecy</key>
            <false/>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <true/>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>
```

Mac용 Visual Studio 내에서 `Info.plist` **솔루션 탐색기**파일을 두 번 클릭 하 고 **원본** 뷰로 전환 하 여 위의 키를 추가 합니다.

[![](ats-images/ats01.png "Info.plist 파일의 소스 뷰입니다.")](ats-images/ats01.png#lightbox)

앱이 보안 되지 않은 사이트에서 웹 콘텐츠를 로드 하 고 표시 해야 하는 경우 앱의 **info.plist** 파일에 다음을 추가 하 여 앱의 나머지 부분에 대해 ATS (Apple Transport Security) 보호를 사용 하도록 설정 하는 동안 웹 페이지를 올바르게 로드할 수 있도록 합니다.

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

필요에 따라 앱의 **info.plist** 파일을 다음과 같이 변경 하 여 모든 도메인 및 인터넷 통신에 대해 ATS를 완전히 사용 하지 않도록 설정할 수 있습니다.

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

Mac용 Visual Studio 내에서 `Info.plist` **솔루션 탐색기**파일을 두 번 클릭 하 고 **원본** 뷰로 전환 하 여 위의 키를 추가 합니다.

[![](ats-images/ats02.png "Info.plist 파일의 소스 뷰입니다.")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> 응용 프로그램에 보안 되지 않은 웹 사이트에 대 한 연결이 필요한 경우에는를 사용 하 여 ATS `NSExceptionDomains` off를 완전히 사용 `NSAllowsArbitraryLoads`하는 대신를 사용 하 여 도메인을 항상 예외로 입력 해야 합니다. `NSAllowsArbitraryLoads` 극단적인 비상 시에 사용 해야 합니다.

ATS를 사용 하지 않도록 설정 하는 것은 보안 연결로 전환 하는 것이 불가능 하거나 불가능 한 경우 마지막 수단 _으로만_ 사용 해야 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 ATS (앱 전송 보안)를 소개 하 고 인터넷과의 보안 통신을 적용 하는 방법에 대해 설명 했습니다. 먼저 iOS 9에서 실행 되는 Xamarin.ios 앱에 필요한 변경 내용에 대해 설명 ATS. 그런 다음 ATS 기능 및 옵션 제어에 대해 설명 했습니다. 마지막으로 Xamarin.ios 앱에서 ATS를 옵트아웃 하는 방법에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
