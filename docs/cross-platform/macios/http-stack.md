---
title: HttpClient 스택 및 iOS/macOS에 대 한 SSL/TLS 구현 선택기
description: 구현 선택기 SSL/TLS 및 HttpClient 스택의 Xamarin iOS, tvOS, 또는 macOS 앱에서 사용 됩니다 하는 SSL/TLS 및 HttpClient 구현을 결정 합니다.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 06/12/2017
ms.openlocfilehash: ba9eb6a062ce91db5f1597de6f9a2b01ad18a367
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-iosmacos"></a>HttpClient 스택 및 iOS/macOS에 대 한 SSL/TLS 구현 선택기

## <a name="httpclient-stack-selector"></a>HttpClient 스택 선택기

Xamarin.iOS, Xamarin.tvOS, 및 Xamarin.Mac에 사용할 수 있는:이 제어는 `HttpClient` 사용할 구현입니다. 기본적으로 작동 HttpClient 계속 `HttpWebRequest`, iOS, tvOS, 또는 macOS 네이티브 전송 방식을 사용 하는 구현에 필요에 따라 전환 이제 w (`NSUrlSession` 또는 `CFNetwork` 운영 체제에 따라). 장점은 작은 이진 파일 및 다운로드 속도 더, 단점은 실행할 비동기 작업에 대해 실행 되 고 이벤트 루프 필요 하다는 것입니다.

프로젝트를 참조 해야 합니다는 **System.Net.Http** 어셈블리입니다.

<a name="Selecting-a-HttpClient-Stack" />

### <a name="selecting-a-httpclient-stack"></a>HttpClient 스택 선택

응용 프로그램에서 사용 하 고 HttpClient를 조정 합니다.

1. 두 번 클릭는 **프로젝트 이름** 에 **솔루션 탐색기** 프로젝트 옵션을 엽니다.
2. 전환 하는 **빌드** 프로젝트에 대 한 설정 (예를 들어 **iOS 빌드** Xamarin.iOS 앱에 대 한).
3. **HttpClient 구현** 드롭다운을 선택 된 HttpClient 다음 중 하나로 입력: **관리**, **CFNetwork** 또는 **NSUrlSession**.

[![관리, CFNetwork, 또는 NSUrlSession HttpClient 구현을 선택합니다](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

<a name="Managed" />

### <a name="managed-default"></a>관리 되는 (기본값)

관리 되는 처리기에는 이전 버전의 Xamarin 선적 완전히 관리 되는 HttpClient 처리기가입니다.

#### <a name="pros"></a>전문가:

 - 기능이 호환 가능성이 가장 높은 Microsoft.NET 및 이전 Xamarin 버전을 사용 하 여 설정 합니다.

#### <a name="cons"></a>단점:

 - Apple Os와 완벽 하 게 통합 되지 않은 하 고 TLS 1.0으로 제한 됩니다.
 - 이 일반적으로 보다 훨씬 더 느리기 암호화 등의 작업에서 네이티브 Api입니다.
 - 따라서 배포 가능한 큰 앱을 만드는 관리 코드 더 필요 합니다.

<a name="CFNetwork" />

### <a name="cfnetwork"></a>CFNetwork

CFNetwork 기반 처리기는 네이티브 기반 `CFNetwork` iOS 6에서에서 사용할 수 있는 버전과 새 버전의 프레임 워크입니다.

#### <a name="pros"></a>전문가:

 - 더 나은 성능과 더 작은 실행 파일 크기에 대 한 네이티브 Api를 사용합니다.
 - TLS 1.2와 같은 최신 표준에 대 한 지원입니다.

#### <a name="cons"></a>단점:

 - IOS 6 이상 필요합니다.
 - WatchOS에 사용할 수 없습니다.
 - 일부 HttpClient 기능/옵션은 사용할 수 없습니다.

<a name="NSUrlSession" />

### <a name="nsurlsession"></a>NSUrlSession

NSURLSession 기반 처리기는 네이티브 기반 `NSURLSession` iOS 7에서에서 사용할 수 있는 버전과 새 버전의 프레임 워크입니다.

#### <a name="pros"></a>전문가:

 - 더 나은 성능과 더 작은 실행 파일 크기에 대 한 네이티브 Api를 사용합니다.
 - TLS 1.2와 같은 최신 표준에 대 한 지원.

#### <a name="cons"></a>단점:

 - IOS 7 이상 필요합니다.
 - 일부 HttpClient 기능/옵션은 사용할 수 없습니다.

### <a name="programmatically-setting-the-httpmessagehandler"></a>프로그래밍 방식으로 HttpMessageHandler 설정

위에 표시 된 프로젝트 전체 구성 외에 인스턴스화할 수 있습니다는 `HttpClient` 원하는 삽입 `HttpMessageHandler` 이러한 코드 조각에서와 같이 생성자를 통해:

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

이렇게 하면 다른 사용 하 여 `HttpMessageHandler` 에 선언 된 작업에서 **프로젝트 옵션** 대화 상자.

<a name="New-SSL-TLS-implementation-build-option" />
<a name="Selecting-a-SSL-TLS-implementation" />
<a name="Apple-TLS" />

## <a name="ssltls-implementation-build"></a>SSL/TLS 구현 빌드

SSL (Secure Socket Layer) 및 그 후속 TLS (전송 계층 보안), HTTP 및를 통해 다른 네트워크 연결에 대 한 지원을 제공 `System.Net.Security.SslStream`합니다. Xamarin.iOS, Xamarin.tvOS 또는 Xamarin.Mac의 `System.Net.Security.SslStream` 구현 모노에서 제공 하는 관리 되는 구현을 사용 하는 대신 Apple의 네이티브 SSL/TLS 구현을 호출 합니다. Apple의 native 구현 TLS 1.2를 지원합니다.

<a name="Mono" />

> [!WARNING]
> **모노/관리** TLS 공급자 v3 SSL 및 TLS v 1로 제한 됩니다. 이 TLS 공급자 사용 되지 않으며을 Xamarin.iOS 응용 프로그램에 대해 사용할 수 없습니다. 

<a name="App-Transport-Security" />

## <a name="app-transport-security"></a>앱의 전송 보안

Apple의 _앱 전송 보안_ (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간 보안 연결을 적용 합니다. 응용 프로그램 또는 소비 하는 라이브러리를 통해 직접 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 모든 인터넷 통신 보안 연결 모범 사례를 준수 AT에서 확인 합니다.

AT iOS 9, tvOS 9 및 OS X 10.11 (El Capitan) 용으로 작성 된 앱에서 기본적으로 활성화 되어 있으므로 이상 버전에서는 모든 연결을 사용 하 여 `NSUrlConnection`, `CFUrl` 또는 `NSUrlSession` AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.

HttpClient 스택 및 SSL/TLS 구현 선택 항목에 따라, AT와 함께 작동 하도록 앱에 수정 사항을 적용할 할 수 있습니다.

AT에 대 한 자세한 정보를 확인 하려면를 참조 하십시오 우리의 [앱 전송 보안 가이드](~/ios/app-fundamentals/ats.md)합니다.

## <a name="known-issues"></a>알려진 문제

이 섹션은 Xamarin.iOS에 TLS 지원이 알려진된 문제를 설명 합니다.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>프로젝트 "요청한 값 AppleTLS 찾을 수 없습니다." 오류와 함께 로드 하지 못했습니다.

Xamarin.iOS 9.8 도입 포함 된 몇 가지 새로운 설정은 **.csproj** Xamarin.iOS 응용 프로그램에 대 한 파일입니다. 이러한 변경 내용은 이전 버전의 Xamarin.iOS로 프로젝트를 열 때 문제가 발생할 수 있습니다. 다음 스크린 샷에서이 시나리오에 표시 될 수 있는 오류 메시지의 한 예입니다.

![프로젝트를 로드 하는 동안 오류의 스크린샷 요청한 값 레거시 찾을 수 없음](http-stack-images/tlserror-xs.png)

도입 하 여이 오류는 발생 된 `MtouchTlsProvider` Xamarin.iOS 9.8에서 프로젝트 파일을 설정 합니다. 없으면 Xamarin.iOS 9.8를 업데이트할 수 (또는 이상)는 해결 방법은 수동으로 편집 하는 **.csproj** 응용 프로그램 파일을 제거는 `MtouchTlsprovider` 요소를 한 다음 변경 된 프로젝트 파일을 저장 합니다.

다음 코드 조각은 상황의 한 예로 `MtouchTlsProvider` 설정을 보일 수 있습니다 내부와 같은 한 **.csproj** 파일:

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>관련 링크

- [TLS(전송 계층 보안)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [앱 전송 보안](~/ios/app-fundamentals/ats.md)
