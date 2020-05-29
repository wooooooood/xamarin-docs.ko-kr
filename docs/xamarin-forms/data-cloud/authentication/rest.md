---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d62e533d127294c77c0779c20fd9c78ef2231200
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135722"
---
# <a name="authenticate-a-restful-web-service"></a>RESTful 웹 서비스 인증

_HTTP에서는 여러 인증 메커니즘을 사용 하 여 리소스에 대 한 액세스를 제어할 수 있습니다. 기본 인증은 올바른 자격 증명이 있는 클라이언트에만 리소스에 대 한 액세스를 제공 합니다. 이 문서에서는 기본 인증을 사용 하 여 RESTful 웹 서비스 리소스에 대 한 액세스를 보호 하는 방법을 보여 줍니다._

> [!NOTE]
> IOS 9 이상에서 ATS (App Transport Security)는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간 보안 연결을 적용 하 여 중요 한 정보가 실수로 공개 되는 것을 방지 합니다. ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되어 있으므로 모든 연결에 ATS 보안 요구 사항이 적용 됩니다. 연결이 이러한 요구 사항을 충족 하지 않으면 예외와 함께 실패 합니다.
> 프로토콜을 사용 하 여 `HTTPS` 인터넷 리소스에 대 한 보안 통신을 할 수 없는 경우 ATS를 옵트아웃 (opt out) 할 수 있습니다. 이는 앱의 **info.plist** 파일을 업데이트 하 여 수행할 수 있습니다. 자세한 내용은 [앱 전송 보안](~/ios/app-fundamentals/ats.md)을 참조 하세요.

## <a name="authenticating-users-over-http"></a>HTTP를 통한 사용자 인증

기본 인증은 HTTP에서 지원 되는 가장 간단한 인증 메커니즘으로, 사용자 이름 및 암호를 암호화 되지 않은 base64 인코딩 텍스트로 보내는 클라이언트를 포함 합니다. 다음과 같이 작동합니다.

- 웹 서비스는 보호 된 리소스에 대 한 요청을 수신 하는 경우 다음 다이어그램과 같이 HTTP 상태 코드 401 (액세스 거부 됨)를 사용 하 여 요청을 거부 하 고 WWW-인증 응답 헤더를 설정 합니다.

![](rest-images/basic-authentication-fail.png "Basic Authentication Failing")

- 웹 서비스가 헤더를 올바르게 설정 하 여 보호 된 리소스에 대 한 요청을 수신 하는 경우 `Authorization` 웹 서비스는 HTTP 상태 코드 200 (요청이 성공 했으며 요청 된 정보가 응답에 있음을 나타냄)를 사용 하 여 응답 합니다. 이 시나리오는 다음 다이어그램에 나와 있습니다.

![](rest-images/basic-authentication-success.png "Basic Authentication Succeeding")

> [!NOTE]
> 기본 인증은 HTTPS 연결을 통해서만 사용 해야 합니다. Http 연결을 통해 사용 되는 경우 `Authorization` http 트래픽이 공격자에 의해 캡처되는 경우 헤더를 쉽게 디코딩할 수 있습니다.

## <a name="specifying-basic-authentication-in-a-web-request"></a>웹 요청에서 기본 인증 지정

기본 인증의 사용은 다음과 같이 지정 됩니다.

1. "Basic" 문자열은 요청 헤더에 추가 됩니다 `Authorization` .
1. 사용자 이름 및 암호는 "username: password" 형식의 문자열로 결합 되며이는 base64 인코딩 후 요청 헤더에 추가 됩니다 `Authorization` .

따라서 ' XamarinUser '의 사용자 이름 및 ' XamarinPassword '의 암호를 사용 하면 헤더가 다음과 같이 됩니다.

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

`HttpClient`클래스는 `Authorization` 속성에 헤더 값을 설정할 수 있습니다 `HttpClient.DefaultRequestHeaders.Authorization` . `HttpClient`인스턴스가 여러 요청에 존재 하기 때문에 `Authorization` 다음 코드 예제에 표시 된 것 처럼 헤더는 모든 요청을 만들 때가 아니라 한 번만 설정 하면 됩니다.

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

그런 다음 웹 서비스 작업에 대 한 요청이 수행 되 면 헤더를 사용 하 여 요청을 서명 `Authorization` 하 고 사용자에 게 작업을 호출할 수 있는 권한이 있는지 여부를 나타냅니다.

> [!IMPORTANT]
> 이 코드는 자격 증명을 상수로 저장 하지만 게시 된 응용 프로그램에서는 안전 하지 않은 형식으로 저장 해서는 안 됩니다.

## <a name="processing-the-authorization-header-server-side"></a>권한 부여 헤더 서버 쪽 처리

REST 서비스는 각 작업을 특성으로 데코레이팅 해야 합니다 `[BasicAuthentication]` . 이 특성은 헤더를 구문 분석 하 고 base64 인코딩 자격 증명이 유효한 지를 확인 하는 데 사용 되며,이를 web.config `Authorization` 에 저장 된 값과 비교 하 여 확인 *합니다*. 이 접근 방식은 샘플 서비스에 적합 하지만 공용 웹 서비스를 위해 확장 해야 합니다.

IIS에서 사용 하는 기본 인증 모듈에서 사용자는 Windows 자격 증명을 사용 하 여 인증 됩니다. 따라서 사용자에 게는 서버 도메인에 대 한 계정이 있어야 합니다. 그러나 사용자 지정 인증을 허용 하도록 기본 인증 모델을 구성할 수 있습니다 .이 경우 사용자 계정은 데이터베이스와 같은 외부 원본에 대해 인증 됩니다. 자세한 내용은 ASP.NET 웹 사이트에서 [ASP.NET Web API의 기본 인증](https://www.asp.net/web-api/overview/security/basic-authentication) 을 참조 하세요.

> [!NOTE]
> 기본 인증은 로그 아웃을 관리 하도록 설계 되지 않았습니다. 따라서 로그 아웃에 대 한 표준 기본 인증 방법은 세션을 종료 하는 것입니다.

## <a name="related-links"></a>관련 링크

- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/web-services/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
