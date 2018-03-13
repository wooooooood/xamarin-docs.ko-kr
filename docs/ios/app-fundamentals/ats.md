---
title: "앱의 전송 보안"
description: "앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간 보안 연결을 적용합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: a4491f550369bbb8515635ecbb7c1c2b74de48cf
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="app-transport-security"></a>앱의 전송 보안

_앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간 보안 연결을 적용합니다._

이 문서에서는 소개 iOS 9 앱에 대해 앱 전송 보안을 적용 하는 보안 변경 내용 및 [Xamarin.iOS 프로젝트에 대 한 즉](#xamarinsupport)에 적용 됩니다는 [AT 구성 옵션](#config) 및 에서는 설명 하는 방법 [옵트아웃 AT의](#optout) AT 필요한 경우. AT 기본적으로 사용 하도록 설정 하기 때문에 안전 하지 않은 모든 인터넷 연결 (하지 않은 것을 명시적으로 허용 된) iOS 9 앱에서 예외가 발생 합니다.


## <a name="about-app-transport-security"></a>앱의 전송 보안에 대 한

위에서 설명 했 듯이 AT 보장 9 iOS 및 OS X El Capitan에서 모든 인터넷 통신이 모범 사례를 연결 보안을 직접 응용 프로그램 또는 해당 되는 라이브러리를 통해 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 사용 했습니다.

기존 앱에 대 한 구현에서 `HTTPS` 가능 하면 항상 프로토콜입니다. 사용 해야 새 Xamarin.iOS 앱에 대 한 `HTTPS` 인터넷 리소스와 통신할 때에 합니다. 또한 고급 API 통신 완전 보안으로 TLS 버전 1.2 사용 하 여 암호화 되어야 합니다.

사용한 연결은 [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) 또는 [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) AT 9 iOS 및 OS X 10.11 (El Capitan) 용으로 작성 된 앱에서 기본적으로 사용 합니다.

## <a name="default-ats-behavior"></a>기본 AT 동작

AT 9 iOS 및 OS X 10.11 (El Capitan)에 대 한 모든 연결을 사용 하 여 빌드한 앱에 기본적으로 활성화 되어 있으므로 [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) 또는 [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) 를 일으키는 됩니다 AT 보안 요구 사항입니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.

### <a name="ats-connection-requirements"></a>AT 연결 요구 사항

AT 모든 인터넷 연결에 대해 다음 요구 사항을 적용 합니다.

- 모든 연결 암호 완전 보안을 사용 해야 합니다. 허용 되는 암호를 아래 목록을 참조 하십시오.
- 보안 TLS (전송 계층) 프로토콜 버전 1.2 이상 이어야 합니다.
- 모든 인증서에 대 한 적어도 SHA256 지문 또는 사용 하 여 2048 비트 또는 큰 RSA 키 또는 256 비트 큰 Elliptic 곡선 (ECC) 키를 사용 합니다.

다시, AT를 iOS 9에에서 기본적으로 사용 하므로 이러한 요구 사항을 충족 하지 않는 연결을 시도 하면 예외가 throw 되 고 발생 합니다. 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>AT 호환 되는 암호

다음 완전 보안 암호화 유형 AT 인터넷 통신 보안에서 허용 됩니다.

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

IOS 인터넷 통신 클래스 사용에 대 한 자세한 내용은 Apple의를 참조 하십시오 [NSURLConnection 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755), [CFURL 참조](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) 또는 [NSURLSession 클래스 참조 ](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Xamarin.iOS에서 AT 지원

Xamarin.iOS 앱 라이브러리 또는 서비스를 사용 하는 인터넷에 연결 하면 AT 9 iOS 및 OS X El Capitan에 기본적으로 활성화 되므로 특정 작업을 수행 해야 하거나 연결 예외가 throw 됩니다.

기존 응용 프로그램에 대 한 Apple 제안 지 원하는 `HTTPS` 최대한 빨리 프로토콜입니다. 하거나 수 없는 경우 타사 지원 하지 않는 웹 서비스에 연결 하는 때문에 `HTTPS` 지 원하는 경우 또는 `HTTPS` 유용한 것, 있습니다 수 되지 않게 AT 합니다. 참조는 [AT Opting 아웃](#optout) 자세한 내용을 보려면 아래 섹션.

사용 해야 새 Xamarin.iOS 앱에 대 한 `HTTPS` 인터넷 리소스와 통신할 때에 합니다. 다시, 경우가 있을 수 있습니다 (예: 타사 웹 서비스를 사용 하 여) 여기서 이것이 불가능 하 고 되지 않게 하려면 AT의 필요 합니다.

또한 AT 상위 수준 API 통신을 완전 보안으로 TLS 버전 1.2 사용 하 여 암호화를 적용 합니다. 참조는 [AT 연결 요구 사항](#ATS-Connection-Requirements) 및 [호환 되는 암호를 AT](#ATS-Compatible-Ciphers) 자세한 내용은 위 섹션.

TLS에 잘 알고 수 ([Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security))는 SSL에 대 한 후속 ([Secure Socket Layer](https://en.wikipedia.org/wiki/Transport_Layer_Security))를 통해 보안을 적용 하려면 암호화 프로토콜의 컬렉션을 제공 하 고 네트워크 연결 합니다.

TLS 수준을 사용 하는 웹 서비스에 의해 제어 됩니다 이므로 응용 프로그램의 제어를 벗어나야 합니다. 둘 다는 `HttpClient` 및 `ModernHttpClient` 자동으로 가장 높은 수준의 서버에서 지원 되는 TLS 암호화를 사용 해야 합니다.

서버 큰지에 (특히 경우는 타사 서비스)에 말하는, 완전 보안을 사용 하지 않도록 설정 하거나 더 낮은 TLS 수준 선택 해야 할 수 있습니다. 참조는 [AT 옵션 구성](#Configuring-ATS-Options) 자세한 내용을 보려면 아래 섹션.

> [!IMPORTANT]
> **참고:** 앱 전송 보안을 사용 하 여 Xamarin 앱에 적용 되지 않습니다 **관리 HTTPClient 구현**합니다. CFNetwork를 사용 하 여 연결에 적용 됩니다 **HTTPClient 구현** 또는 **NSURLSession HTTPClient 구현** 만 합니다.

### <a name="setting-the-httpclient-implementation"></a>HTTPClient 구현 설정

IOS 앱에서 사용 하는 HTTPClient 구현을 설정 하려면는 **프로젝트** 에 **솔루션 탐색기** 열려는 **프로젝트 옵션**합니다. 로 이동 **iOS 빌드** 아래에서 원하는 클라이언트 유형을 선택 하 고는 **HttpClient 구현** 드롭다운:

![](ats-images/client01.png "IOS 빌드 옵션을 설정합니다.")


#### <a name="managed-handler"></a>관리 되는 처리기

관리 되는 처리기는 완전히 관리 되는 HttpClient 처리기는 이전 버전의 Xamarin.iOS와 배송 되 고 기본 처리기입니다.

전문가:

- Microsoft.NET 및 이전 버전의 Xamarin 가장 호환 됩니다.

단점:

- 하지 iOS (예: TLS 1.0만 표시 됩니다)와 통합 되어 완벽 하 게 합니다.
- 네이티브 Api 보다 훨씬 더 느리기 일반적으로
- 관리 코드 보다 필요 하며 큰 앱을 만듭니다.

#### <a name="cfnetwork-handler"></a>CFNetwork 처리기

기반 CFNetwork 처리기는 네이티브 기반 `CFNetwork` 프레임 워크입니다.

전문가:

- 더 나은 성능과 더 작은 실행 파일 크기에 대 한 네이티브 API를 사용합니다.
- TLS 1.2와 같은 최신 표준에 대 한 지원을 추가합니다.

단점:

- IOS 6 이상 필요합니다.
- WatchOS의 사용할 수 없습니다.
- 일부 HttpClient 기능 및 옵션은 사용할 수 없습니다.

#### <a name="nsurlsession-handler"></a>NSUrlSession 처리기

기반 NSUrlSession 처리기는 네이티브 기반 `NSUrlSession` API입니다.

전문가:

- 더 나은 성능과 더 작은 실행 파일 크기에 대 한 네이티브 API를 사용합니다.
- TLS 1.2와 같은 최신 표준에 대 한 지원을 추가합니다.

단점:

- IOS 7 이상 필요합니다.
- 일부 HttpClient 기능 및 옵션은 사용할 수 없습니다. 

## <a name="diagnosing-ats-issues"></a>AT 문제 진단

인터넷에 직접 또는 iOS 9의에서 웹 보기에서 연결을 시도할 때 형식에서 오류가 발생할 수 있습니다.

> 보안 되지 않으므로 앱 전송 보안 일반 텍스트 (http://www.-the-blocked-domain.com) HTTP 리소스 부하를 차단 했습니다. 앱의 Info.plist 파일을 통해 임시 예외를 구성할 수 있습니다.

IOS9, 앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간 보안 연결을 적용합니다. 또한 AT 필요 하므로 사용 하는 통신의 `HTTPS` 프로토콜 및 높은 수준의 API 통신을 암호화 완전 보안 TLS 버전 1.2 사용 합니다.

AT 9 iOS 및 OS X 10.11 (El Capitan)에 대 한 모든 연결을 사용 하 여 빌드한 앱에 기본적으로 활성화 되어 있으므로 `NSURLConnection`, `CFURL` 또는 `NSURLSession` AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.

Apple 제공는 [TLSTool 샘플 응용 프로그램](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) 컴파일할 수 있습니다 (또는 필요에 따라 Xamarin 및 C# 코드 변환) AT/TLS 문제를 진단 하는 데 사용 합니다. 참조 하십시오는 [AT Opting 아웃](#optout) 이 문제를 해결 하는 방법에 대 한 내용은 아래 섹션.


<a name="config" />

## <a name="configuring-ats-options"></a>AT 옵션 구성

응용 프로그램에서 특정 키에 대 한 값을 설정 하 여 다양 한 AT의 기능을 구성할 수 있습니다 **Info.plist** 파일입니다. 다음 키가 AT 제어에 사용할 수 있는 (_중첩 되어 표시 들여쓴_):

```csharp
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

각 키에는 다음 형식과 의미 합니다.

- **NSAppTransportSecurity** (`Dictionary`)-모든 설정 키를 포함 하 고 AT에 대 한 값입니다.
- **NSAllowsArbitraryLoads** (`Boolean`)-경우 `YES` AT 모든 도메인에 사용 되지 것입니다 **하지** 에 나열 된 `NSExceptionDomains`합니다. 나열 된 도메인에 대 한 지정 된 보안 설정이 사용 됩니다.
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean`)-경우 `YES` 웹 페이지 응용 프로그램의 나머지 부분에 대해 아직 사용 되 고 Apple 전송 보안 (AT) 보호 하는 동안 올바르게 로드를 허용 합니다.
- **NSExceptionDomains** (`Dictionary`)-도메인의 컬렉션을 AT 지정된 된 도메인에 사용 해야 하는 보안 설정 합니다.
- **< Domain-name-for-exception-as-string >** (`Dictionary`)-(예: 해당된 도메인에 대 한 예외 컬렉션. `www.xamarin.com`).
- **NSExceptionMinimumTLSVersion** (`String`)-하나로 최소 TLS 버전 `TLSv1.0`, `TLSv1.1` 또는 `TLSv1.2` (기본값인).
- **NSExceptionRequiresForwardSecrecy** (`Boolean`)-경우 `NO` 도메인 정방향 보안 포함 한 암호화가 사용 하지 않아도 됩니다. 기본값은 `YES`입니다.
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`)-경우 `NO` (기본값)와이 도메인의 모든 통신에 있어야는 `HTTPS` 프로토콜입니다.
- **NSRequiresCertificateTransparency** (`Boolean`)-경우 `YES` 도메인의 SSL Secure Sockets Layer ()에서 유효한 투명도 데이터를 포함 해야 합니다. 기본값은 `NO`입니다.
- **NSIncludesSubdomains** (`Boolean`)-경우 `YES` 이러한 설정은이 도메인의 모든 하위 도메인을 재정의 합니다. 기본값은 `NO`입니다.
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`)-The TLS 버전 도메인이 개발자의 제어를 벗어나야 타사 서비스는 때를 사용 합니다.
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`)-경우 `YES` 3rd 파티 도메인 완전 보안 필요 합니다.
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`)-경우 `YES` 는 AT 3rd 파티 도메인과 안전 하지 않은 통신을 허용 합니다.

<a name="optout" />

### <a name="opting-out-of-ats"></a>사용 하 고 아웃 AT

Apple 항상 사용 하도록 제안 하는 동안는 `HTTPS` 프로토콜 및 인터넷에 대 한 보안 통신 정보를 기준으로, 이것이 불가능 항상 않았을 수도 있습니다. 예를 들어, 한 타사 웹 서비스와 통신 하거나 응용 프로그램에서 광고를 제공 하는 인터넷을 사용 하는 경우.

응용 프로그램에 다음과 같이 변경 Xamarin.iOS 앱 안전 하지 않은 도메인에는 요청을 수행 해야 하는 경우 **Info.plist** 파일 AT에서 지정된 된 도메인에 게 적용 되는 보안 기본값을 사용할 수 없게 됩니다.

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

Mac 용 Visual Studio 안에서 두 번 클릭는 `Info.plist` 파일에 **솔루션 탐색기**, 전환할는 **소스** 위 키 추가:

[![](ats-images/ats01.png "Info.plist 파일의 소스 뷰")](ats-images/ats01.png#lightbox)


응용 프로그램을 로드 하 고 안전 하지 않은 사이트에서 웹 콘텐츠를 표시 하는 경우 다음 응용 프로그램을 추가 **Info.plist** 파일을 웹 페이지를 나머지에도 사용할 Apple 전송 보안 (AT) 보호 하는 동안 올바르게 로드 하려면 응용 프로그램:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

필요에 따라 변경할 수 있습니다는 다음 응용 프로그램에 **Info.plist** 완전히 AT를 사용 하지 않으려면 모든 도메인 및 인터넷 통신에 대 한 파일:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

Mac 용 Visual Studio 안에서 두 번 클릭는 `Info.plist` 파일에 **솔루션 탐색기**, 전환할는 **소스** 위 키 추가:

[![](ats-images/ats02.png "Info.plist 파일의 소스 뷰")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> **참고:** 해야 응용 프로그램에서 안전 하지 않은 웹 사이트에 대 한 연결을 필요한 경우 **항상** 도메인을 사용 하 여 예외 입력 `NSExceptionDomains` AT 사용 하 여 완전히 해제 하는 대신 `NSAllowsArbitraryLoads`합니다. `NSAllowsArbitraryLoads` 극단적인 응급 상황에만 사용 해야 합니다.




다시, AT를 사용 하지 않도록 설정 해야 _만_ 사용 불가능 하거나 비현실적는 보안 연결으로 전환 하는 경우 마지막 수단으로 사용할 수 있습니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서 앱 전송 보안 (AT) 도입 개이고 인터넷 보안 통신을 적용 하는 방법을 설명 합니다. 첫째, 우리 AT 필요 iOS 9에서 실행 중인 Xamarin.iOS 앱에 대 한 변경 내용을 다룹니다. 그런 다음 제어 AT 기능 및 옵션 다룬 합니다. 마지막으로, AT Xamarin.iOS 응용 프로그램에서 옵트아웃 다룹니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
