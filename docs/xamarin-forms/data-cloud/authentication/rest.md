---
title: "RESTful 웹 서비스를 인증합니다."
description: "HTTP은 리소스에 대 한 액세스를 제어 하는 여러 가지 인증 메커니즘의 사용을 지원 합니다. 기본 인증을 올바른 자격 증명을 가진 클라이언트에만 리소스에 대 한 액세스를 제공 합니다. 이 문서에서는 RESTful 웹 서비스 리소스에 대 한 액세스를 보호 하기 위해 기본 인증을 사용 하는 방법을 보여줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 7aea74f95e8738cc415eaac3a5ac4f86b069d0f7
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="authenticating-a-restful-web-service"></a>RESTful 웹 서비스를 인증합니다.

_HTTP은 리소스에 대 한 액세스를 제어 하는 여러 가지 인증 메커니즘의 사용을 지원 합니다. 기본 인증을 올바른 자격 증명을 가진 클라이언트에만 리소스에 대 한 액세스를 제공 합니다. 이 문서에서는 RESTful 웹 서비스 리소스에 대 한 액세스를 보호 하기 위해 기본 인증을 사용 하는 방법을 보여줍니다._

함께 제공 된 Xamarin.Forms 샘플 응용 프로그램 웹 서비스에 대 한 읽기 전용 액세스를 제공 하는 Xamarin 호스팅되지 REST 서비스를 사용 합니다. 따라서 생성, 업데이트 및 데이터를 삭제 하는 작업은 응용 프로그램에서 사용 하는 데이터가 변경 하지 않습니다. 그러나 REST 서비스의 호스팅 가능한 버전은 영어로 *TodoRESTService* 는 서비스 설정에 대 한 지침 및 샘플 응용 프로그램 폴더를 있습니다 찾을 수 있습니다. 이 호스팅 가능 버전 REST 서비스의 전체 만들고, 데이터 업데이트, 읽기 및 삭제 액세스를 제공 합니다.

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간에 보안 연결 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 적용 합니다. IOS 9에 대해 빌드된 앱에서 기본적으로 AT를 사용 하므로 모든 연결 AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 AT 옵트인 수 있습니다는 `HTTPS` 및 프로토콜을 인터넷 리소스에 대 한 통신을 보호 합니다. 응용 프로그램의 업데이트 하 여 이렇게 할 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="authenticating-users-over-http"></a>HTTP를 통해 사용자를 인증합니다.

기본 인증에서 HTTP를 지 원하는 가장 간단한 인증 메커니즘은 많이 들며 클라이언트 사용자 이름 및 암호 암호화 되지 않은 base64 인코딩 텍스트로 전송 합니다. 다음과 같습니다.

- 웹 서비스와 보호 된 리소스에 대 한 요청 될 경우 HTTP 상태 코드가 401 (액세스가 거부 되었습니다) 요청을 거부 하 고 다음 다이어그램에 나와 있는 것 처럼 응답 Www-authenticate 헤더를 설정 합니다.

![](rest-images/basic-authentication-fail.png "기본 인증 실패")

- 웹 서비스와 보호 된 리소스에 대 한 요청을 수신 하는 경우의 `Authorization` 헤더 올바르게 설정 요청이 성공 했음을 나타내는 HTTP 상태 코드가 200 인 웹 서비스 응답 했으며 응답에 요청된 된 정보 인지 합니다. 이 시나리오는 다음 다이어그램에 표시 됩니다.

![](rest-images/basic-authentication-success.png "기본 인증 다음에 나오")

> [!NOTE]
> HTTPS 연결을 통해만 기본 인증을 사용 해야 합니다. HTTP 연결을 통해 사용할 경우의 <code>Authorization</code> 공격자에 의해 캡처된 HTTP 트래픽을 하는 경우에 쉽게 헤더를 디코딩할 수 있습니다.

## <a name="specifying-basic-authentication-in-a-web-request"></a>웹 요청에 지정 하 여 기본 인증

기본 인증의 사용은 다음과 같이 지정 됩니다.

1. "기본"에 추가 되는 문자열은 `Authorization` 요청 헤더입니다.
1. 사용자 이름 및 암호는 문자열에 "username:password"는 다음에 추가 하 고 인코딩된 base64 형식으로 결합 됩니다는 `Authorization` 요청 헤더입니다.

따라서 'XamarinUser'의 사용자 이름 및 암호 'XamarinPassword'를 사용 하 여 헤더가 됩니다.

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

`HttpClient` 클래스 설정할 수 있습니다는 `Authorization` 헤더 값에는 `HttpClient.DefaultRequestHeaders.Authorization` 속성입니다. 때문에 `HttpClient` 인스턴스가 여러 요청에 대해 존재 하는 `Authorization` 헤더만 설정 해야 한 번 때 대신 다음 코드 예제에 나와 있는 것 처럼 모든 요청을 만드는:

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    var authData = string.Format ("{0}:{1}", Constants.Username, Constants.Password);
    var authHeaderValue = Convert.ToBase64String (Encoding.UTF8.GetBytes (authData));

    client = new HttpClient ();
    ...
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue ("Basic", authHeaderValue);
  }
  ...
}
```

요청으로 서명 되는 웹 서비스 작업에 대 한 요청이 만들어질 때 다음의 `Authorization` 사용자 작업을 호출할 수 있는 권한이 있는지 여부를 나타내는 헤더입니다.

> [!NOTE]
> 되지만 샘플 REST 서비스는 상수로 자격 증명으로 저장, 게시 된 응용 프로그램에는 안전 하지 않은 형식으로 저장할 것인지 없습니다. [Xamarith.Auth](https://www.nuget.org/packages/Xamarin.Auth/) NuGet 자격 증명을 안전 하 게 저장 하기 위한 기능을 제공 합니다. 자세한 내용은 참조 [저장 장치에서 계정 정보를 검색 하 고](~/xamarin-forms/data-cloud/authentication/oauth.md)합니다.


## <a name="processing-the-authorization-header-server-side"></a>권한 부여 헤더 서버 쪽 처리

함께 제공 된 샘플 REST 서비스와 각 작업 데코레이팅합니다는 `[BasicAuthentication]` 특성입니다. 이 특성에 의해 구현 됩니다는 `BasicAuthenticationAttribute` 솔루션에서 클래스 및 구문 분석 하는 데 사용 되는 `Authorization` 헤더에 저장 된 값을 비교 하 여 base64 인코딩 자격 증명이 유효 하는 경우를 확인 하 고 *Web.config*. 이 방법은 샘플 서비스에 대 한 적합 한 이지만, 공용 웹 서비스에 대 한 확장 해야 합니다.

IIS에서 사용 하는 기본 인증 모듈에서 사용자가 Windows 자격 증명에 대해 인증 됩니다. 따라서 사용자는 서버의 도메인에 계정이 있어야 합니다. 그러나 기본 인증 모델 데이터베이스와 같은 외부 소스에 대해 사용자 계정에서 인증 된 사용자 지정 인증을 허용 하도록 구성할 수 있습니다. 자세한 내용은 참조 [ASP.NET Web API에서 기본 인증](http://www.asp.net/web-api/overview/security/basic-authentication) ASP.NET 웹 사이트에서.

> [!NOTE]
> 기본 인증 로그 아웃 관리 하도록 설계 되지 않았습니다. 따라서 세션을 종료 하는 로그 아웃에 대 한 표준 기본 인증 방법이입니다.

## <a name="summary"></a>요약

이 문서에는 기본 인증을 사용 하 여 Xamarin.Forms 응용 프로그램에서 발생 한 웹 요청을 추가 하는 방법을 살펴보았습니다는 `HttpClient` 클래스입니다. 기본 인증을 올바른 자격 증명을 가진 클라이언트에만 리소스에 대 한 액세스를 제공 합니다. 사용 하는 방법에 대 한 내용은 [Xamarin.Auth](https://www.nuget.org/packages/Xamarin.Auth/) Xamarin.Forms 응용 프로그램에서 인증 프로세스를 관리만 데이터에 액세스할 수 있는 동안 사용자가 백 엔드를 공유할 수 있도록 하려면 참조 [사용자 인증 Id 공급자와](~/xamarin-forms/data-cloud/authentication/oauth.md)합니다.


## <a name="related-links"></a>관련 링크

- [TodoREST (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [RESTful 웹 서비스 사용](~/xamarin-forms/data-cloud/consuming/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
