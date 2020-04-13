---
title: RESTful 웹 서비스 인증
description: 기본 인증은 올바른 자격 증명을 가진 클라이언트에만 리소스에 대한 액세스를 제공합니다. 이 문서에서는 기본 인증을 사용하여 RESTful 웹 서비스 리소스에 대한 액세스를 보호하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
ms.openlocfilehash: 65606b72c3944809a09b8c70b9cdbb5524e0f856
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388592"
---
# <a name="authenticate-a-restful-web-service"></a>RESTful 웹 서비스 인증

_HTTP는 리소스에 대한 액세스를 제어하기 위해 여러 인증 메커니즘의 사용을 지원합니다. 기본 인증은 올바른 자격 증명을 가진 클라이언트에만 리소스에 대한 액세스를 제공합니다. 이 문서에서는 기본 인증을 사용하여 RESTful 웹 서비스 리소스에 대한 액세스를 보호하는 방법을 보여 줍니다._

> [!NOTE]
> iOS 9 이상에서 ATS(앱 전송 보안)는 인터넷 리소스(예: 앱의 백 엔드 서버)와 앱 간의 보안 연결을 적용하여 중요한 정보가 실수로 노출되는 것을 방지합니다. ATS는 iOS 9용으로 빌드된 앱에서 기본적으로 활성화되므로 모든 연결에는 ATS 보안 요구 사항이 적용됩니다. 연결이 이러한 요구 사항을 충족하지 않으면 예외로 실패합니다.
> 프로토콜을 사용할 수 없는 경우 ATS를 `HTTPS` 옵트아웃하고 인터넷 리소스에 대한 보안 통신을 사용할 수 없습니다. 이 응용 프로그램의 **Info.plist** 파일을 업데이트 하 여 수행할 수 있습니다. 자세한 내용은 [앱 전송 보안을](~/ios/app-fundamentals/ats.md)참조하십시오.

## <a name="authenticating-users-over-http"></a>HTTP를 통해 사용자 인증

기본 인증은 HTTP에서 지원하는 가장 간단한 인증 메커니즘이며 암호화되지 않은 base64 인코딩된 텍스트로 사용자 이름과 암호를 보내는 클라이언트와 관련이 있습니다. 다음과 같이 작동합니다.

- 웹 서비스가 보호된 리소스에 대한 요청을 받는 경우 HTTP 상태 코드 401(액세스 거부됨)으로 요청을 거부하고 다음 다이어그램과 같이 WWW-인증 응답 헤더를 설정합니다.

![](rest-images/basic-authentication-fail.png "Basic Authentication Failing")

- 웹 서비스가 `Authorization` 헤더를 올바르게 설정한 상태에서 보호된 리소스에 대한 요청을 받으면 웹 서비스는 HTTP 상태 코드 200으로 응답하여 요청이 성공했으며 요청된 정보가 응답중임을 나타냅니다. 이 시나리오는 다음 다이어그램에 나와 있습니다.

![](rest-images/basic-authentication-success.png "Basic Authentication Succeeding")

> [!NOTE]
> 기본 인증은 HTTPS 연결을 통해서만 사용해야 합니다. HTTP 연결을 통해 사용하면 `Authorization` 공격자가 HTTP 트래픽을 캡처한 경우 헤더를 쉽게 디코딩할 수 있습니다.

## <a name="specifying-basic-authentication-in-a-web-request"></a>웹 요청에서 기본 인증 지정

기본 인증의 사용은 다음과 같이 지정됩니다.

1. "Basic" 문자열이 요청의 `Authorization` 헤더에 추가됩니다.
1. 사용자 이름과 암호는 "username:password" 형식의 문자열로 결합된 다음 base64를 인코딩하고 `Authorization` 요청의 헤더에 추가합니다.

따라서 'XamarinUser'의 사용자 이름과 'XamarinPassword'의 암호를 사용하면 헤더가 됩니다.

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

클래스는 `HttpClient` `HttpClient.DefaultRequestHeaders.Authorization` 속성에 `Authorization` 헤더 값을 설정할 수 있습니다. 인스턴스는 `HttpClient` 여러 요청에 걸쳐 `Authorization` 존재하기 때문에 다음 코드 예제와 같이 모든 요청을 수행할 때가 아니라 헤더를 한 번만 설정하면 됩니다.

```csharp
public class RestService : IRestService
{
  HttpClient _client;
  ...

  public RestService ()
  {
    var authData = string.Format ("{0}:{1}", Constants.Username, Constants.Password);
    var authHeaderValue = Convert.ToBase64String (Encoding.UTF8.GetBytes (authData));

    _client = new HttpClient ();
    _client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue ("Basic", authHeaderValue);
  }
  ...
}
```

그런 다음 웹 서비스 작업에 대한 요청이 수행되면 `Authorization` 요청이 헤더로 서명되어 사용자가 작업을 호출할 수 있는 권한이 있는지 여부를 나타냅니다.

> [!IMPORTANT]
> 이 코드는 자격 증명을 상수로 저장하지만 게시된 응용 프로그램에서 안전하지 않은 형식으로 저장해서는 안 됩니다.

## <a name="processing-the-authorization-header-server-side"></a>권한 부여 헤더 서버 측 처리

REST 서비스는 각 작업을 `[BasicAuthentication]` 특성으로 장식해야 합니다. 이 특성은 `Authorization` 헤더를 구문 분석하고 base64 인코딩된 자격 증명이 *Web.config에*저장된 값과 비교하여 유효한지 확인하는 데 사용됩니다. 이 방법은 샘플 서비스에 적합하지만 공용 웹 서비스에 대해 확장해야 합니다.

IIS에서 사용하는 기본 인증 모듈에서 사용자는 Windows 자격 증명에 대해 인증됩니다. 따라서 사용자는 서버의 도메인에 계정이 있어야 합니다. 그러나 기본 인증 모델은 데이터베이스와 같은 외부 소스에 대해 사용자 계정이 인증되는 사용자 지정 인증을 허용하도록 구성할 수 있습니다. 자세한 내용은 ASP.NET 웹 [사이트의 ASP.NET 웹 API의 기본 인증을](https://www.asp.net/web-api/overview/security/basic-authentication) 참조하십시오.

> [!NOTE]
> 기본 인증은 로그아웃을 관리하도록 설계되지 않았습니다. 따라서 로그아웃에 대 한 표준 기본 인증 방법은 세션을 종료 하는 것입니다.

## <a name="related-links"></a>관련 링크

- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
