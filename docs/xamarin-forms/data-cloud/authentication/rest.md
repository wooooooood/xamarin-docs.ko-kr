---
title: RESTful 웹 서비스를 인증합니다.
description: 기본 인증 자격 증명이 있는 클라이언트만를 리소스에 대 한 액세스를 제공 합니다. 이 문서에서는 RESTful 웹 서비스 리소스에 대 한 액세스를 보호 하기 위해 기본 인증을 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
ms.openlocfilehash: d3f07a72ee26d6be4fafa72137dc9b6c3a724e00
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61330659"
---
# <a name="authenticating-a-restful-web-service"></a>RESTful 웹 서비스를 인증합니다.

_HTTP은 리소스에 대 한 액세스를 제어 하는 몇 가지 인증 메커니즘 사용을 지원 합니다. 기본 인증 자격 증명이 있는 클라이언트만를 리소스에 대 한 액세스를 제공 합니다. 이 문서에서는 RESTful 웹 서비스 리소스에 대 한 액세스를 보호 하기 위해 기본 인증을 사용 하는 방법에 설명 합니다._

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결 하므로 중요 한 정보가 실수로 유출 방지 적용 합니다. ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되므로 모든 연결이 ATS 보안 요구 사항이 적용 됩니다. 연결에서 이러한 요구를 충족 하지 않는, 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 ATS 옵트인 수 있습니다는 `HTTPS` 프로토콜 및 인터넷 리소스에 대 한 통신을 보호 합니다. 이 앱을 업데이트 하 여 수행할 수 있습니다 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="authenticating-users-over-http"></a>HTTP를 통해 사용자를 인증합니다.

기본 인증 하는 HTTP를 지 원하는 가장 간단한 인증 메커니즘 및 암호화 되지 않은 base64로 인코딩된 텍스트 사용자 이름 및 암호를 보내는 클라이언트를 포함 합니다. 다음과 같이 작동합니다.

- 웹 서비스에 보호 된 리소스에 대 한 요청을 수신 하는 경우 HTTP 상태 코드 401 (액세스가 거부 되었습니다)을 사용 하 여 요청을 거부 하 고 다음 다이어그램과에서 같이 Www-authenticate 응답 헤더를 설정 합니다.

![](rest-images/basic-authentication-fail.png "기본 인증 실패")

- 웹 서비스를 사용 하 여 보호 된 리소스에 대 한 요청을 수신 하는 경우는 `Authorization` 헤더가 올바르게 설정 요청이 성공 했는지 여부를 나타내는 HTTP 상태 코드 200 사용 하 여 웹 서비스 응답 및 응답에서 요청 된 정보를 인지 합니다. 이 시나리오는 다음 다이어그램에 표시 됩니다.

![](rest-images/basic-authentication-success.png "기본 인증 성공")

> [!NOTE]
> HTTPS 연결을 통해만 기본 인증을 사용 해야 합니다. HTTP 연결을 통해 사용 하는 경우는 <code>Authorization</code> 공격자에 의해 캡처된 HTTP 트래픽을 하는 경우에 쉽게 헤더를 디코딩할 수 있습니다.

## <a name="specifying-basic-authentication-in-a-web-request"></a>웹 요청에 지정 기본 인증

기본 인증의 사용은 다음과 같이 지정 됩니다.

1. "기본"에 추가 되는 문자열을 `Authorization` 요청의 헤더입니다.
1. Username 및 password는 문자열에 "username:password" base64 인코딩되며에 추가 되는 형식을 사용 하 여 결합 됩니다는 `Authorization` 요청의 헤더입니다.

따라서 'XamarinUser'의 사용자 이름 및 'XamarinPassword'의 암호를 사용 하 여 헤더가 됩니다.

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

`HttpClient` 클래스에 설정할 수는 `Authorization` 헤더 값에는 `HttpClient.DefaultRequestHeaders.Authorization` 속성. 때문에 `HttpClient` 인스턴스가 여러 개의 요청 전반를 `Authorization` 헤더만 설정 해야 번 경우 대신 다음 코드 예제와 같이 모든 요청:

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

요청으로 서명 되어 웹 서비스 작업에는 요청이 만들어질 때 다음을 `Authorization` 사용자 작업을 호출할 수 있는 권한이 있는지 여부를 나타내는 헤더입니다.

> [!NOTE]
> 이 코드는 상수로 자격 증명을 저장, 하는 동안 이러한 해야 저장 되지 게시 된 응용 프로그램에서 안전 하지 않은 형식으로. 합니다 [Xamarith.Auth](https://www.nuget.org/packages/Xamarin.Auth/) NuGet 자격 증명을 안전 하 게 저장 하는 기능을 제공 합니다. 자세한 내용은 참조 [저장 및 장치에 대 한 계정 정보를 검색](~/xamarin-forms/data-cloud/authentication/oauth.md)합니다.

## <a name="processing-the-authorization-header-server-side"></a>권한 부여 헤더 서버 쪽 처리

REST 서비스에는 각 작업을 데코 레이트 합니다는 `[BasicAuthentication]` 특성입니다. 이 특성 구문 분석 하는 데 사용 되는 `Authorization` 헤더에 저장 된 값을 비교 하 여 base64로 인코딩된 자격 증명이 올바른 경우 확인 하 고 *Web.config*합니다. 이 접근 방식은 샘플 서비스에 대 한 적합 한, 공용 웹 서비스에 대 한 확장이 필요 합니다.

IIS에서 사용 하는 기본 인증 모듈에서 사용자는 Windows 자격 증명에 대해 인증 됩니다. 따라서 사용자는 서버의 도메인에 계정이 있어야 합니다. 그러나 사용자 계정 데이터베이스와 같은 외부 원본에 대해 인증 됩니다 여기서 사용자 지정 인증을 허용 하는 기본 인증 모델을 구성할 수 있습니다. 자세한 내용은 참조 [ASP.NET Web API에서 기본 인증](http://www.asp.net/web-api/overview/security/basic-authentication) ASP.NET 웹 사이트입니다.

> [!NOTE]
> 기본 인증 로그 아웃을 관리 하도록 설계 되지 않았습니다. 따라서 로그 아웃 하기 위한 표준 기본 인증 방법은 세션을 종료 하는 것입니다.

## <a name="related-links"></a>관련 링크

- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/consuming/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
