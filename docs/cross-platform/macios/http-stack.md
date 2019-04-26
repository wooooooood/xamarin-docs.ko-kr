---
title: HttpClient 및 iOS/macOS에 대 한 SSL/TLS 구현 선택기
description: HttpClient 스택 및 SSL/TLS 구현 선택기 Xamarin iOS, tvOS, 또는 macOS 앱에서 사용할 SSL/TLS 및 HttpClient 구현 결정 합니다.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: fd48c7148aadd8d156544113e2d719295294bf40
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261287"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>IOS/macOS에 대 한 HttpClient 및 SSL/TLS 구현 선택기

합니다 **HttpClient 구현 선택 기가** Xamarin.iOS Xamarin.tvOS, 및 Xamarin.Mac 제어는 `HttpClient` 사용할 구현입니다. IOS, tvOS, 또는 macOS 기본 전송을 사용 하는 구현으로 전환할 수 있습니다 (`NSUrlSession` 또는 `CFNetwork`OS에 따라). 장점은은 TLS 1.2 지원의 작은 이진 이며 더 빠르게 다운로드 합니다. 단점은 실행할 비동기 작업을 실행 하는 이벤트 루프 필요 하다는 것입니다.

프로젝트를 참조 해야 합니다 **System.Net.Http** 어셈블리입니다.

> [!WARNING]
> **2018 년 4 월** 보안 강화로 인해 – 요구 사항, PCI 규정 준수를 포함 하 여 주요 클라우드 공급자 및 웹 서버는 TLS 버전 1.2 보다 이전 지원 중지 해야 합니다.  이전 버전의 Visual Studio 이전 버전의 TLS 사용 하 여 기본값으로 만든 Xamarin 프로젝트입니다.
>
> 앱이 이러한 서버 및 서비스를 사용 하 여 작업을 계속할 수 있도록 **Xamarin 프로젝트를 업데이트 해야 합니다 `NSUrlSession` 설정 아래에 표시 된 다음 다시 빌드하고 다시 앱을 배포** 사용자에 게 합니다.

### <a name="selecting-an-httpclient-stack"></a>HttpClient 스택 선택

조정 된 `HttpClient` 앱에서 사용 중:

1. 두 번 클릭 합니다 **프로젝트 이름** 에 **솔루션 탐색기** 프로젝트 옵션을 열려면 합니다.
2. 으로 전환 합니다 **빌드** 프로젝트에 대 한 설정 (예를 들어 **iOS 빌드** Xamarin.iOS 앱에 대 한).
3. **HttpClient 구현** 드롭다운을 `HttpClient` 다음 중 하나로 입력: **NSUrlSession** (권장) **CFNetwork**, 또는 **관리 되는**합니다.

[![관리 되는, CFNetwork, 또는 NSUrlSession 로부터 HttpClient 구현 선택](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> TLS 1.2 지원용는 `NSUrlSession` 옵션은 권장 됩니다.

### <a name="nsurlsession"></a>NSUrlSession

합니다 `NSURLSession`-기반된 처리기 네이티브 기반 `NSURLSession` iOS 7에서에서 사용할 수 있으며 최신 프레임 워크입니다. 
**권장된 설정입니다.**

#### <a name="pros"></a>전문가

- 네이티브 Api를 사용 하 여 더 나은 성능과 더 작은 실행 파일 크기에 대 한 합니다.
- TLS 1.2와 같은 최신 표준 지원 합니다.

#### <a name="cons"></a>단점

- IOS 7 이상 필요합니다.
- 일부 `HttpClient` 기능/옵션을 사용할 수 없습니다.

### <a name="cfnetwork"></a>CFNetwork

합니다 `CFNetwork`-기반된 처리기 네이티브 기반 `CFNetwork` iOS 6에서에서 사용할 수 있으며 최신 프레임 워크입니다.

#### <a name="pros"></a>전문가

- 네이티브 Api를 사용 하 여 더 나은 성능과 더 작은 실행 파일 크기에 대 한 합니다.
- TLS 1.2와 같은 최신 표준 지원 합니다.

#### <a name="cons"></a>단점

- IOS 6 이상 필요합니다.
- WatchOS에서 사용할 수 없습니다.
- 일부 HttpClient 기능/옵션이 제공 됩니다.

### <a name="managed"></a>관리

관리 되는 처리기는 완전히 관리 되는 HttpClient 처리기는 이전 버전의 Xamarin 사용 하 여 발송 되었습니다.

#### <a name="pros"></a>전문가

- 해당 기능이 가장 호환성이 뛰어난 Microsoft.NET 및 이전 Xamarin 버전을 사용 하 여 설정 합니다.

#### <a name="cons"></a>단점

- Apple Os와 완벽 하 게 통합 되지 않습니다 하 고 TLS 1.0으로 제한 됩니다. 웹 서버를 보호 하거나 나중에 클라우드 서비스에 연결할 수 없습니다.
- 이 일반적으로 보다 훨씬 더 느립니다 암호화 등 기본 Api입니다.
- 배포 가능한 큰 앱을 만들어서 관리 코드 더 필요 합니다.

### <a name="programmatically-setting-the-httpmessagehandler"></a>Httpmessagehandler 입니다를 프로그래밍 방식으로 설정

위에 표시 된 프로젝트 수준 구성 외에도 인스턴스화할 수도 있습니다는 `HttpClient` 하 고 원하는 삽입 `HttpMessageHandler` 이러한 코드 조각에서와 같이 생성자를 통해:

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

따라서 다른 데 `HttpMessageHandler` 에서 선언 된에서 합니다 **프로젝트 옵션** 대화 합니다.

## <a name="ssltls-implementation"></a>SSL/TLS 구현

SSL (Secure Socket Layer) 및 그 후속 버전인 TLS (Transport Layer Security)에 대해 지원을 제공 HTTP 통해 다른 네트워크 연결과 `System.Net.Security.SslStream`합니다. Xamarin.tvOS Xamarin.iOS, Xamarin.Mac의 `System.Net.Security.SslStream` 구현 Apple의 기본 SSL/TLS 구현 Mono가 제공한 관리 되는 구현을 사용 하는 대신 호출 됩니다. Apple의 기본 구현은 TLS 1.2를 지원합니다.

> [!WARNING]
> 예정된 Xamarin.Mac 4.8 릴리스는 macOS 10.9 이상만 지원합니다.
> 이전 버전의 Xamarin.Mac은 macOS 10.7 이상을 지원했지만 이러한 이전 macOS 버전에는 TLS 1.2를 지원할 수 있는 TLS 인프라가 충분하지 않습니다. macOS 10.7 또는 macOS 10.8을 대상으로 하려면 Xamarin.Mac 4.6 또는 이전 버전을 사용하세요.

## <a name="app-transport-security"></a>앱 전송 보안

Apple _앱 전송 보안_ ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결을 적용 합니다. 앱 또는 사용 하는 라이브러리를 통해 직접 중요 한 정보가 실수로 유출 방지 모든 인터넷 통신 보안 연결 모범 사례를 준수 하는지 ATS에서 확인 합니다.

ATS 9 iOS, tvOS 9 및 OS X 10.11 (El Capitan) 용으로 빌드된 앱에서 기본적으로 활성화 되어 있으므로 이상에서 사용 하 여 모든 연결 `NSUrlConnection`, `CFUrl` 또는 `NSUrlSession` ATS 보안 요구 사항이 적용 됩니다. 연결에서 이러한 요구를 충족 하지 않는, 예외와 함께 실패 합니다.

HttpClient 스택 및 SSL/TLS 구현 선택 항목에 따라 ATS와 함께 작동 하도록 앱을 수정 해야 할 수 있습니다.

ATS 대 한 자세한 정보를 확인 하려면 하세요 우리의 [앱 전송 보안 가이드](~/ios/app-fundamentals/ats.md)합니다.

## <a name="known-issues"></a>알려진 문제

이 섹션에서는 Xamarin.iOS의 TLS 지원의 알려진된 문제를 설명 합니다.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>프로젝트를 "요청한 값 AppleTLS를 찾을 수 없습니다" 오류와 함께 로드 하지 못했습니다.

9.8 Xamarin.iOS에는 포함 된 새 설정을 일부를 도입 합니다 **.csproj** Xamarin.iOS 응용 프로그램에 대 한 파일입니다. 이러한 변경 내용은 이전 버전의 Xamarin.iOS 사용 하 여 프로젝트를 열 때 문제가 발생할 수 있습니다. 다음 스크린샷은 다음과 같습니다.이 시나리오에서는 표시 될 수 있는 오류 메시지의 예

![프로젝트를 로드 하는 동안 오류 스크린샷 요청 값 레거시 찾을 수 없음](http-stack-images/tlserror-xs.png)

도입 하 여이 오류는 발생 된 `MtouchTlsProvider` Xamarin.iOS 9.8에서 프로젝트 파일을 설정 합니다. 없으면 Xamarin.iOS 9.8로 업데이트 수 (또는 이상)에 해결 방법은 수동으로 편집 하는 **.csproj** 응용 프로그램 파일을 제거는 `MtouchTlsprovider` 요소를 다음 변경 된 프로젝트 파일을 저장 합니다.

다음 코드 조각은 모양의 예는 `MtouchTlsProvider` 설정 표시 될 수 있습니다 내에서 같은 **.csproj** 파일:

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>관련 링크

- [TLS(전송 계층 보안)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [앱 전송 보안](~/ios/app-fundamentals/ats.md)
