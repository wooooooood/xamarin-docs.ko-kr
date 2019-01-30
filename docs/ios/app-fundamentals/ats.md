---
title: Xamarin.iOS 앱 전송 보안
description: '앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결을 적용합니다.'
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2017
ms.openlocfilehash: f9308d3a746a5a0a43cf47cc5ea809c0f82bbe7b
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233824"
---
# <a name="app-transport-security-in-xamarinios"></a>Xamarin.iOS 앱 전송 보안

_앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결을 적용합니다._

이 문서에서는 iOS 9 앱에 앱 전송 보안을 적용 하는 보안 변경 내용을 소개 하 고 [Xamarin.iOS 프로젝트에 대 한 의미](#xamarinsupport), 적용 됩니다는 [ATS 구성 옵션](#config) 및 에서는 설명 하는 방법 [옵트아웃 ATS](#optout) ATS 필요한 경우. ATS는 기본적으로 사용 하도록 설정 하기 때문에 모든 안전 하지 않은 인터넷 연결 (하지 않는 한 명시적으로 허용한 것) iOS 9 앱의 경우 예외가 발생 합니다.


## <a name="about-app-transport-security"></a>앱 전송 보안에 대 한

위에서 설명한 대로 ATS 하면 9 iOS 및 OS X El Capitan에서 모든 인터넷 통신 보안 연결 모범 사례에 맞게 앱 또는 해당 라이브러리를 통해 직접 중요 한 정보가 실수로 유출 방지 사용 합니다.

기존 앱에 대 한 구현 된 `HTTPS` 가능한 프로토콜입니다. 새 Xamarin.iOS 앱에 대 한 사용할지 `HTTPS` 인터넷 리소스와 통신 하는 경우에 합니다. 또한 상위 수준 API 통신 전달 완전 보안을 사용 하 여 TLS 버전 1.2 사용 하 여 암호화 되어야 합니다.

사용한 연결은 [NSUrlConnection](xref:Foundation.NSUrlConnection)를 [CFUrl](xref:CoreFoundation.CFUrl) 또는 [NSUrlSession](xref:Foundation.NSUrlSession) ATS 9 iOS 및 OS X 10.11 (El Capitan) 용으로 빌드된 앱에서 기본적으로 사용 됩니다.

## <a name="default-ats-behavior"></a>기본 ATS 동작

ATS는 모든 연결을 사용 하 여 iOS 9 및 OS X 10.11 (El Capitan) 용으로 빌드된 앱에서 기본적으로 활성화 되어 있으므로 [NSUrlConnection](xref:Foundation.NSUrlConnection)를 [CFUrl](xref:CoreFoundation.CFUrl) 하거나 [NSUrlSession](xref:Foundation.NSUrlSession) 적용 됩니다 ATS 보안 요구 사항입니다. 연결에 이러한 요구 사항에 맞지 않는 경우 예외가 발생 하 여 실패 합니다.

### <a name="ats-connection-requirements"></a>ATS 연결 요구 사항

ATS는 모든 인터넷 연결에 대해 다음 요구 사항을 적용 됩니다.

- 모든 연결 암호화 전달 완전 보안을 사용 해야 합니다. 허용 된 암호화 아래 목록을 참조 하세요.
- 전송 계층 보안 (TLS) 프로토콜 버전 1.2 이상 이어야 합니다.
- 하나 이상의 SHA256 지문 2048 비트 또는 큰 RSA 키 또는 256 비트 또는 큰 Elliptic Curve (ECC) 키를 사용 하 여 모든 인증서에 대 한 사용 되어야 합니다.

마찬가지로 ATS가 iOS 9에서에서 기본적으로 사용 하도록 설정 되므로 이러한 요구 사항을 충족 하지 않는 연결을 시도 하면 예외가 throw 되 발생 합니다. 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>ATS 호환 암호화

ATS 인터넷 통신을 보호 하 여 전달 완전 보안 암호화 형식이 허용 됩니다.

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

IOS 인터넷 통신 클래스를 사용 하 여 작업에 대 한 자세한 내용은 Apple의를 참조 하세요 [NSURLConnection 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755)하십시오 [CFURL 참조](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) 또는 [NSURLSession 클래스 참조 ](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>ATS Xamarin.iOS에서 지 원하는

Xamarin.iOS 앱 라이브러리 또는 서비스를 사용 하는 인터넷에 연결 하면 ATS 9 iOS 및 OS X El Capitan에서 기본적으로 설정 됩니다 때문에 일부 작업을 수행 해야 하거나 연결 예외가 throw 됩니다.

기존 앱을 Apple에서 제안 지원는 `HTTPS` 최대한 빨리 프로토콜입니다. 그렇게 할 수 없으면 하거나 타사를 지원 하지 않는 웹 서비스에 연결 된 상태 이므로 `HTTPS` 지 원하는 경우 또는 `HTTPS` 것은, 있습니다 수 옵트아웃 ATS 합니다. 참조 된 [Opting 옵트아웃 ATS](#optout) 대 한 자세한 내용은 아래 섹션입니다.

새 Xamarin.iOS 앱에 대 한 사용할지 `HTTPS` 인터넷 리소스와 통신 하는 경우에 합니다. 마찬가지로 경우가 있을 수 있습니다 (예: 타사 웹 서비스를 사용 하 여) 여기서 이것이 불가능 하 고 옵트아웃 ATS 해야 합니다.

또한 ATS는 높은 수준의 API 통신을 전달 완전 보안을 사용 하 여 TLS 버전 1.2 사용 하 여 암호화를 적용 합니다. 참조를 [ATS 연결 요구 사항을](#ATS-Connection-Requirements) 하 고 [ATS 호환 암호화](#ATS-Compatible-Ciphers) 대 한 자세한 내용은 위의 섹션입니다.

TLS를 사용 하 여 친숙 하지 않을 때 ([Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)) SSL로의 후속 작업 ([Secure Socket Layer](https://en.wikipedia.org/wiki/Transport_Layer_Security))를 통해 보안을 적용 하는 암호화 프로토콜의 컬렉션을 제공 하 고 네트워크 연결입니다.

TLS 수준을 사용 하는 웹 서비스에 의해 제어 됩니다 이므로 앱의 컨트롤 외부에서. 모두를 `HttpClient` 하며 `ModernHttpClient` 자동으로 가장 높은 수준의 서버에서 지원 되는 TLS 암호화를 사용 해야 합니다.

서버 따라에 말하는 (특히 경우가 타사 추가 서비스)에 전달 완전 보안을 사용 하지 않도록 설정 하거나 더 낮은 TLS 수준 선택 해야 할 수 있습니다. 참조 된 [ATS 옵션 구성](#Configuring-ATS-Options) 대 한 자세한 내용은 아래 섹션입니다.

> [!IMPORTANT]
> 앱 전송 보안을 사용 하 여 Xamarin 앱에 적용 되지 않습니다 **관리 되는 HTTPClient 구현**합니다. CFNetwork를 사용 하 여 연결에 적용 됩니다 **HTTPClient 구현** 하거나 **NSURLSession HTTPClient 구현** 만 합니다.

### <a name="setting-the-httpclient-implementation"></a>HTTPClient 구현 설정

IOS 앱에서 사용 하는 HTTPClient 구현으로 설정 하려면 두 번 클릭 합니다 **프로젝트** 에 **솔루션 탐색기** 열려는 **프로젝트 옵션**. 이동할 **iOS 빌드** 아래에서 원하는 클라이언트 유형을 선택 합니다 **HttpClient 구현** 드롭다운:

![](ats-images/client01.png "IOS 빌드 옵션 설정")


#### <a name="managed-handler"></a>Managed Handler

관리 되는 처리기는 완전히 관리 되는 HttpClient 처리기 기본 처리기 이며 이전 버전의 Xamarin.iOS 사용 하 여 발송 되었습니다.

전문가:

- Microsoft.NET 및 이전 버전의 Xamarin 사용 하 여 가장 호환 됩니다.

단점:

- 없습니다 (예: TLS 1.0 지원 되는) iOS와 통합 되어 완벽 하 게 합니다.
- 것은 일반적으로 네이티브 Api 보다 훨씬 느립니다.
- 관리 되는 코드가 더 필요 하 고 더 큰 앱을 만듭니다.

#### <a name="cfnetwork-handler"></a>CFNetwork Handler

CFNetwork 기반 처리기를 기반으로 네이티브 `CFNetwork` 프레임 워크입니다.

전문가:

- 더 나은 성능과 더 작은 실행 파일 크기에 대 한 네이티브 API를 사용합니다.
- TLS 1.2와 같은 최신 표준에 대 한 지원을 추가합니다.

단점:

- IOS 6 이상 필요합니다.
- WatchOS의 사용할 수 없습니다.
- HttpClient 기능 및 옵션 일부 제공 되지 않습니다.

#### <a name="nsurlsession-handler"></a>NSUrlSession 처리기

NSUrlSession 기반 처리기를 기반으로 네이티브 `NSUrlSession` API.

전문가:

- 더 나은 성능과 더 작은 실행 파일 크기에 대 한 네이티브 API를 사용합니다.
- TLS 1.2와 같은 최신 표준에 대 한 지원을 추가합니다.

단점:

- IOS 7 이상 필요합니다.
- HttpClient 기능 및 옵션 일부 제공 되지 않습니다. 

## <a name="diagnosing-ats-issues"></a>ATS 문제 진단

직접 또는 ios 9 웹 보기에서 인터넷에 연결을 시도할 때 오류가 발생할 수 있습니다는 형식:

> 앱 전송 보안을 일반 텍스트로 HTTP 차단한 (http://www.-the-blocked-domain.com) 리소스 로드를 안전 하지 않습니다. 앱의 Info.plist 파일을 통해 임시 예외를 구성할 수 있습니다.

IOS9, 앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결을 적용합니다. 또한 ATS 필요 사용 하 여 통신을 `HTTPS` 프로토콜 및 전달 완전 보안을 사용 하 여 TLS 버전 1.2 사용 하 여 암호화 될 높은 수준의 API 통신 합니다.

ATS는 모든 연결을 사용 하 여 iOS 9 및 OS X 10.11 (El Capitan) 용으로 빌드된 앱에서 기본적으로 활성화 되어 있으므로 `NSURLConnection`하십시오 `CFURL` 또는 `NSURLSession` ATS 보안 요구 사항이 적용 됩니다. 연결에 이러한 요구 사항에 맞지 않는 경우 예외가 발생 하 여 실패 합니다.

Apple에서는 합니다 [TLSTool 샘플 앱](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) 컴파일할 수 있습니다 (또는 Xamarin으로 트랜스 코딩할 필요에 따라 및 C#) AT/TLS 문제를 진단 하는 데 사용 합니다. 참조 하십시오 합니다 [Opting 옵트아웃 ATS](#optout) 이 문제를 해결 하는 방법에 대 한 내용은 아래 섹션입니다.


<a name="config" />

## <a name="configuring-ats-options"></a>ATS 옵션 구성

앱의 특정 키에 대 한 값을 설정 하 여 다양 한 ATS의 기능을 구성할 수 있습니다 **Info.plist** 파일입니다. 다음 키는 ATS를 제어할 수 있습니다 (_중첩 된 방식을 표시할 들여쓰기_):

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
- **NSAllowsArbitraryLoads** (`Boolean`)- `YES` ATS 모든 도메인에 대 한 비활성화 됩니다 **하지** 에 나열 된 `NSExceptionDomains`합니다. 나열 된 도메인에 대 한 지정 된 보안 설정이 사용 됩니다.
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean`)- `YES` 웹 페이지 앱의 나머지 부분에 대 한 Apple 전송 보안 (ATS) 보호를 사용 하도록 계속 하는 동안 올바르게 로드를 사용 하면 됩니다.
- **NSExceptionDomains** (`Dictionary`)-도메인의 컬렉션을 지정된 된 도메인에 대 한 ATS 사용 해야 하는 보안 설정 합니다.
- **< Domain-name-for-exception-as-string >** (`Dictionary`)-(예: 지정 된 도메인에 대 한 예외 컬렉션 `www.xamarin.com`).
- **NSExceptionMinimumTLSVersion** (`String`)-최소 TLS 버전 중 하나로 `TLSv1.0`합니다 `TLSv1.1` 또는 `TLSv1.2` (기본값인).
- **NSExceptionRequiresForwardSecrecy** (`Boolean`)- `NO` 도메인 정방향 보안을 사용 하 여 암호화를 사용할 필요가 없습니다. 기본값은 `YES`입니다.
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`)- `NO` (기본값)이이 도메인을 사용 하 여 모든 통신에 있어야 합니다 `HTTPS` 프로토콜입니다.
- **NSRequiresCertificateTransparency** (`Boolean`)- `YES` 도메인의 보안 소켓 레이어 (SSL) 유효한 투명도 데이터를 포함 해야 합니다. 기본값은 `NO`입니다.
- **NSIncludesSubdomains** (`Boolean`)- `YES` 이러한 설정은이 도메인의 모든 하위 도메인을 재정의 합니다. 기본값은 `NO`입니다.
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`)-TLS 버전 도메인을 개발자의 컨트롤 외부의 타사 서비스를 사용 합니다.
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`)- `YES` 3rd 파티 도메인 전달 완전 보안 필요 합니다.
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`)- `YES` 는 ATS는 타사 파티 도메인을 사용 하 여 안전 하지 않은 통신을 허용 됩니다.

<a name="optout" />

### <a name="opting-out-of-ats"></a>ATS의 항목을 선택 하면 스케일 아웃

Apple 항상 사용 하도록 제안 하는 동안는 `HTTPS` 프로토콜 및 인터넷으로 보안 통신 정보를 기준으로,이 항상 가능한 것은 하는 경우가 있을 수 있습니다. 예를 들어는 타사 웹 서비스와 통신 하거나 앱에서 광고를 제공 하는 인터넷을 사용 하는 경우.

Xamarin.iOS 앱에 안전 하지 않은 도메인에 요청 해야, 앱의 다음 변경 **Info.plist** 파일 지정된 된 도메인에 대 한 ATS 적용 하는 보안 기본값을 사용 하지 않도록 설정 됩니다.

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

Mac 용 Visual Studio 안에서 두 번 클릭 합니다 `Info.plist` 파일를 **솔루션 탐색기**, 전환할를 **원본** 보기를 위의 키를 추가:

[![](ats-images/ats01.png "Info.plist 파일의 소스 보기")](ats-images/ats01.png#lightbox)


앱을 로드 하 고 안전 하지 않은 사이트에서 웹 콘텐츠를 표시 하는 경우 다음 앱을 추가 **Info.plist** 웹 페이지가 제대로 Apple 전송 보안 (ATS) 보호 아직 나머지에 사용 되는 동안 로드 하도록 허용 하는 파일 앱:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

응용 프로그램에 다음과 같이 변경할 수는 필요에 따라 **Info.plist** 파일을 모든 도메인 및 인터넷 통신에 대 한 ATS를 완전히 사용 하지 않도록 설정 합니다.

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

Mac 용 Visual Studio 안에서 두 번 클릭 합니다 `Info.plist` 파일를 **솔루션 탐색기**, 전환할를 **원본** 보기를 위의 키를 추가:

[![](ats-images/ats02.png "Info.plist 파일의 소스 보기")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> 응용 프로그램 안전 하지 않은 웹 사이트에 연결을 요구 하는 경우 **항상** 사용 하 여 예외도 도메인을 입력 `NSExceptionDomains` ATS 사용 하 여 완전히 해제 하는 대신 `NSAllowsArbitraryLoads`합니다. `NSAllowsArbitraryLoads` 극단적인 비상 시에 사용 해야 합니다.




마찬가지로 ATS를 사용 하지 않도록 설정 해야 _만_ 보안 연결으로 전환 중 하나를 사용할 수 없거나 적절 하지 않은 경우 마지막 수단으로 사용할 수 있습니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 앱 전송 보안 ATS ()를 도입 있고 인터넷을 사용 하 여 보안 통신 강제 적용 하는 방법을 설명 합니다. 먼저 iOS 9에서 실행 되는 Xamarin.iOS 앱에 대 한 ATS 필요한 변경 내용에 설명 했습니다. 그런 다음 앞서 설명한 ATS 기능과 옵션을 제어 합니다. 마지막으로, Xamarin.iOS 앱의 ATS 옵트아웃 다룹니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
