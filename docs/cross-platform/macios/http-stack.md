---
title: IOS/macOS에 대 한 HttpClient 및 SSL/TLS 구현 선택기
description: HttpClient 스택 및 SSL/TLS 구현 선택기는 Xamarin iOS, tvOS 또는 macOS 앱에서 사용 되는 HttpClient 및 SSL/TLS 구현을 결정 합니다.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: f00a25bbb86e9ec57ef2290c1a7e37a8891e1064
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521874"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>IOS/macOS에 대 한 HttpClient 및 SSL/TLS 구현 선택기

Xamarin.ios, tvOS 및 xamarin.ios에 대 한 **httpclient 구현 선택기** 는 사용할 `HttpClient` 구현을 제어 합니다. IOS, tvOS 또는 macos 네이티브 전송 (`NSUrlSession` 또는 `CFNetwork`OS에 따라)을 사용 하는 구현으로 전환할 수 있습니다. 여기서는 TLS 1.2-지원, 작은 이진 파일 및 빠른 다운로드를 지원 합니다. 단점은 비동기 작업을 실행 하기 위해 이벤트 루프를 실행 해야 한다는 것입니다.

프로젝트는 **시스템 .net. Http** 어셈블리를 참조 해야 합니다.

> [!WARNING]
> **4 월, 2018** – PCI 규정 준수를 포함 하 여 보안 요구 사항이 증가 함에 따라 주요 클라우드 공급자 및 웹 서버는 1.2 보다 오래 된 TLS 버전 지원을 중지 해야 합니다. 이전 버전의 Visual Studio에서 만든 Xamarin 프로젝트는 이전 버전의 TLS를 사용 합니다.
>
> 앱이 이러한 서버 및 서비스를 계속 사용할 수 있도록 하려면  **`NSUrlSession` 아래 표시 된 설정으로 Xamarin 프로젝트를 업데이트 한 다음 사용자에 게 앱을 다시 빌드하고 다시 배포 해야** 합니다.

### <a name="selecting-an-httpclient-stack"></a>HttpClient 스택 선택

앱에서 사용 `HttpClient` 중인를 조정 하려면 다음을 수행 합니다.

1. **솔루션 탐색기** 에서 **프로젝트 이름을** 두 번 클릭 하 여 프로젝트 옵션을 엽니다.
2. 프로젝트에 대 한 **빌드** 설정으로 전환 합니다 (예: xamarin.ios 앱에 대 한 **ios 빌드** ).
3. **Httpclient 구현** 드롭다운에서 다음 중 하나로 `HttpClient` 형식을 선택 합니다. **NSUrlSession** (권장), **Cfnetwork**또는 **관리**.

[![관리 되는, CFNetwork 또는 NSUrlSession에서 HttpClient 구현 선택](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> TLS 1.2 지원의 경우 `NSUrlSession` 옵션을 선택 하는 것이 좋습니다.

### <a name="nsurlsession"></a>NSUrlSession

기반 처리기는 iOS 7 이상에서 사용할 수 `NSURLSession` 있는 네이티브 프레임 워크를 기반으로 합니다. `NSURLSession` 
**권장 설정입니다.**

#### <a name="pros"></a>전문가

- 더 나은 성능과 더 작은 실행 파일 크기를 위해 네이티브 Api를 사용 합니다.
- TLS 1.2와 같은 최신 표준에 대 한 지원.

#### <a name="cons"></a>단점

- IOS 7 이상이 필요 합니다.
- 일부 `HttpClient` 기능/옵션을 사용할 수 없습니다.

### <a name="cfnetwork"></a>CFNetwork

기반 처리기는 iOS 6 이상에서 사용할 수 `CFNetwork` 있는 네이티브 프레임 워크를 기반으로 합니다. `CFNetwork`

#### <a name="pros"></a>전문가

- 더 나은 성능과 더 작은 실행 파일 크기를 위해 네이티브 Api를 사용 합니다.
- TLS 1.2와 같은 최신 표준에 대 한 지원.

#### <a name="cons"></a>단점

- IOS 6 이상이 필요 합니다.
- WatchOS에서 사용할 수 없습니다.
- 일부 HttpClient 기능/옵션을 사용할 수 없습니다.

### <a name="managed"></a>관리

관리 되는 처리기는 이전 버전의 Xamarin과 함께 제공 되는 완전히 관리 되는 HttpClient 처리기입니다.

#### <a name="pros"></a>전문가

- Microsoft .NET 및 이전 Xamarin 버전과 호환 가능한 기능 집합을 포함 합니다.

#### <a name="cons"></a>단점

- Apple Os와 완전히 통합 되지 않으며 TLS 1.0로 제한 됩니다. 나중에 보안 웹 서버 또는 클라우드 서비스에 연결 하지 못할 수 있습니다.
- 일반적으로 기본 Api 보다 암호화와 같은 작업을 수행 하는 것이 훨씬 느립니다.
- 더 많은 관리 코드가 필요 하므로 더 큰 앱 배포를 만들 수 있습니다.

### <a name="programmatically-setting-the-httpmessagehandler"></a>프로그래밍 방식으로 HttpMessageHandler 설정

위에 표시 된 프로젝트 전체 구성 외에도, 다음 코드 조각과 같이를 인스턴스화하고 `HttpClient` 생성자를 통해 원하는 `HttpMessageHandler` 을 삽입할 수 있습니다.

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

이렇게 하면 **프로젝트 옵션** 대화 상자에서 선언 `HttpMessageHandler` 된 것과 다른를 사용할 수 있습니다.

## <a name="ssltls-implementation"></a>SSL/TLS 구현

SSL (Secure Socket Layer) 및 그 후속 TLS (전송 계층 보안)는을 통해 `System.Net.Security.SslStream`HTTP 및 기타 네트워크 연결에 대 한 지원을 제공 합니다. Xamarin.ios, tvOS 또는 xamarin.ios의 `System.Net.Security.SslStream` 구현은 Mono에서 제공 하는 관리 되는 구현을 사용 하는 대신 Apple의 네이티브 SSL/TLS 구현을 호출 합니다. Apple의 네이티브 구현에서는 TLS 1.2을 지원 합니다.

> [!WARNING]
> 예정된 Xamarin.Mac 4.8 릴리스는 macOS 10.9 이상만 지원합니다.
> 이전 버전의 Xamarin.Mac은 macOS 10.7 이상을 지원했지만 이러한 이전 macOS 버전에는 TLS 1.2를 지원할 수 있는 TLS 인프라가 충분하지 않습니다. macOS 10.7 또는 macOS 10.8을 대상으로 하려면 Xamarin.Mac 4.6 또는 이전 버전을 사용하세요.

## <a name="app-transport-security"></a>앱 전송 보안

Apple의 ATS ( _앱 전송 보안_ )는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간 보안 연결을 적용 합니다. ATS를 사용 하면 모든 인터넷 통신이 안전한 연결 모범 사례를 준수 하 여 앱 또는 사용 중인 라이브러리를 통해 직접 중요 한 정보가 노출 되지 않도록 방지할 수 있습니다.

ATS는 iOS 9, tvOS 9 및 OS X 10.11 (El Capitan) 이상으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되어 있으므로, 또는 `NSUrlConnection` `NSUrlSession` 를 `CFUrl` 사용 하는 모든 연결에는 ATS 보안 요구 사항이 적용 됩니다. 연결이 이러한 요구 사항을 충족 하지 않는 경우 예외가 발생 하 여 실패 합니다.

HttpClient 스택 및 SSL/TLS 구현 선택에 따라 ATS에서 제대로 작동 하도록 앱을 수정 해야 할 수 있습니다.

ATS에 대해 자세히 알아보려면 [앱 전송 보안 가이드](~/ios/app-fundamentals/ats.md)를 참조 하세요.

## <a name="known-issues"></a>알려진 문제

이 섹션에서는 Xamarin.ios에서의 TLS 지원과 관련 하 여 알려진 문제에 대해 설명 합니다.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>"요청 된 값이 없습니다." 라는 오류가 발생 하 여 프로젝트를 로드 하지 못했습니다.

Xamarin.ios 9.8에는 Xamarin.ios 응용 프로그램에 대 한 **.csproj** 파일이 포함 된 몇 가지 새 설정이 도입 되었습니다. 이전 버전의 Xamarin.ios를 사용 하 여 프로젝트를 열면 이러한 변경으로 인해 문제가 발생할 수 있습니다. 다음 스크린샷은이 시나리오에 표시 될 수 있는 오류 메시지의 예입니다.

![프로젝트를 로드 하는 동안 발생 한 오류 스크린샷, 요청 된 값 레거시를 찾을 수 없음](http-stack-images/tlserror-xs.png)

이 오류는 xamarin.ios 9.8의 프로젝트 파일에 `MtouchTlsProvider` 대 한 설정이 도입 되어 발생 합니다. Xamarin.ios 9.8 이상으로 업데이트할 수 없는 경우 해결 방법은 **.csproj** 파일 응용 프로그램을 수동으로 편집 하 고 `MtouchTlsprovider` 요소를 제거한 다음 변경 된 프로젝트 파일을 저장 하는 것입니다.

다음 코드 조각은 **.csproj** 파일 내에 설정 된 `MtouchTlsProvider` 것 처럼 보일 수 있는 방법의 예입니다.

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>관련 링크

- [TLS(전송 계층 보안)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [앱 전송 보안](~/ios/app-fundamentals/ats.md)
