---
title: iOS 시뮬레이터 및 Android Emulator에서 로컬 웹 서비스에 연결
description: 이 문서에서는 iOS 시뮬레이터 또는 Android Emulator에서 실행되는 Xamarin 모바일 애플리케이션이 로컬로 실행되는 ASP.NET Core 웹 서비스를 사용하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: FD8FE199-898B-4841-8041-CC9CA1A00917
author: davidbritch
ms.author: dabritch
ms.date: 10/16/2019
ms.openlocfilehash: 0a2bd469477ce6e2aca03e1d4cf279bb5a7a16f9
ms.sourcegitcommit: 94fa3bf464a2ee5ac4b6056691d264b8210b1192
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72526814"
---
# <a name="connect-to-local-web-services-from-ios-simulators-and-android-emulators"></a>iOS 시뮬레이터 및 Android Emulator에서 로컬 웹 서비스에 연결

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest/)

많은 모바일 애플리케이션이 웹 서비스를 사용합니다. 개발 단계에서는 로컬로 웹 서비스를 배포하고 iOS 시뮬레이터 또는 Android Emulator에서 실행되는 모바일 애플리케이션에서 웹 서비스를 사용하는 것이 일반적입니다. 따라서 웹 서비스를 호스트된 엔드포인트에 배포하지 않아도 되며, 모바일 애플리케이션 및 웹 서비스를 둘 다 로컬로 실행하기 때문에 디버깅도 간편해집니다.

iOS 시뮬레이터 또는 Android Emulator에서 실행되는 모바일 애플리케이션은 다음과 같이 로컬로 실행되고 HTTP를 통해 노출되는 ASP.NET Core 웹 서비스를 사용할 수 있습니다.

- iOS 시뮬레이터에서 실행 중인 애플리케이션은 머신 IP 주소 또는 `localhost` 호스트 이름을 통해 로컬 HTTP 웹 서비스에 연결할 수 있습니다. 예를 들어, `/api/todoitems/` 상대 URI를 통해 GET 작업을 노출하는 로컬 HTTP 웹 서비스가 있는 경우, iOS 시뮬레이터에서 실행 중인 애플리케이션은 `http://localhost:<port>/api/todoitems/`로 GET 요청을 전송하여 해당 작업을 사용할 수 있습니다.
- Android Emulator에서 실행 중인 애플리케이션은 호스트 루프백 인터페이스의 별칭(개발 머신의 `127.0.0.1`)인 `10.0.2.2` 주소를 통해 로컬 HTTP 웹 서비스에 연결할 수 있습니다. 예를 들어, `/api/todoitems/` 상대 URI를 통해 GET 작업을 노출하는 로컬 HTTP 웹 서비스가 있는 경우, Android Emulator에서 실행 중인 애플리케이션은 `http://10.0.2.2:<port>/api/todoitems/`로 GET 요청을 전송하여 해당 작업을 사용할 수 있습니다.

그러나 iOS 시뮬레이터 또는 Android Emulator에서 실행 중인 애플리케이션이 HTTPS를 통해 노출되는 로컬 웹 서비스를 사용하려면 추가 작업이 필요합니다. 이 시나리오에서 해당 프로세스는 다음과 같습니다.

1. 머신에 자체 서명된 개발 인증서를 만듭니다. 자세한 내용은 [개발 인증서 만들기](#create-a-development-certificate)를 참조하세요.
1. 디버그 빌드를 위해 적절한 `HttpClient` 네트워크 스택을 사용하도록 프로젝트를 구성합니다. 자세한 내용은 [프로젝트 구성](#configure-your-project)을 참조하세요.
1. 로컬 머신의 주소를 지정합니다. 자세한 내용은 [로컬 머신 주소 지정](#specify-the-local-machine-address)을 참조하세요.
1. 로컬 개발 인증서 보안 검사를 무시합니다. 자세한 내용은 [인증서 보안 검사 무시](#bypass-the-certificate-security-check)를 참조하세요.

각 항목을 차례차례 설명하겠습니다.

## <a name="create-a-development-certificate"></a>개발 인증서 만들기

.NET Core SDK를 설치하면 로컬 사용자 인증서 저장소에 ASP.NET Core HTTPS 개발 인증서가 설치됩니다. 그러나 인증서가 설치되었지만 신뢰할 수는 없습니다. 인증서를 신뢰하려면 다음 일회성 단계를 수행하여 dotnet `dev-certs` 도구를 실행합니다.

```console
dotnet dev-certs https --trust
```

다음 명령은 `dev-certs` 도구에 대한 도움말을 제공합니다.

```console
dotnet dev-certs https --help
```

또는 HTTPS를 사용하는 ASP.NET Core 2.1(이상) 프로젝트를 실행할 때 Visual Studio는 개발 인증서가 누락되었는지 검색하고, 누락된 경우 설치한 후 신뢰할 것을 요구합니다.

> [!NOTE]
> ASP.NET Core HTTPS 개발 인증서가 자체 서명됩니다.

머신에서 로컬 HTTPS를 사용하도록 설정하는 방법에 대한 자세한 내용은 [로컬 HTTPS 사용](/aspnet/core/getting-started#enable-local-https)을 참조하세요.

## <a name="configure-your-project"></a>프로젝트 구성

iOS 및 Android에서 실행되는 Xamarin 애플리케이션은 `HttpClient` 클래스에서 사용되는 네트워킹 스택을 관리형 네트워크 스택 또는 네이티브 네트워크 스택 중에서 선택할 수 있도록 합니다. 관리형 스택은 기존 .NET 코드와의 높은 수준의 호환성을 제공하지만 TLS 1.0으로 제한되며 속도가 더 느려지고 실행 파일 크기가 더 커질 수 있습니다. 네이티브 스택은 더 빠르고 더 나은 보안을 제공할 수 있지만 `HttpClient` 클래스의 모든 기능을 제공하지는 못할 수 있습니다.

### <a name="ios"></a>iOS

iOS에서 실행되는 Xamarin 애플리케이션은 관리형 네트워크 스택이나 네이티브 `CFNetwork` 또는 `NSUrlSession` 네트워크 스택을 사용할 수 있습니다. 기본적으로 새 iOS 플랫폼 프로젝트는 TLS 1.2를 지원하려는 경우에는 `NSUrlSession` 네트워크 스택을 사용하고, 성능을 높이고 실행 파일 크기를 줄이려는 경우에는 네이티브 API를 사용합니다.

그러나 애플리케이션이 로컬로 실행되는 보안 웹 서비스에 연결해야 하는 경우 개발자 테스트를 위해 관리형 네트워크 스택을 사용하기가 더 쉽습니다. 따라서 관리형 네트워크 스택을 사용하려면 디버그 시뮬레이터 빌드 프로필을 설정하고, `NSUrlSession` 네트워크 스택을 사용하려면 릴리스 빌드 프로필을 설정하는 것이 좋습니다. 각 네트워크 스택은 프로젝트 옵션에서 프로그래밍 방식을 사용하거나 선택기를 통해 설정할 수 있습니다. 자세한 내용은 [iOS/macOS용 HttpClient 및 SSL/TLS 구현 선택기](~/cross-platform/macios/http-stack.md)를 참조하세요.

### <a name="android"></a>Android

Android에서 실행되는 Xamarin 애플리케이션은 관리형 `HttpClient` 네트워크 스택이나 네이티브 `AndroidClientHandler` 네트워크 스택을 사용할 수 있습니다. 기본적으로 새 Android 플랫폼 프로젝트는 TLS 1.2를 지원하려는 경우에는 `AndroidClientHandler` 네트워크 스택을 사용하고, 성능을 높이고 실행 파일 크기를 줄이려는 경우에는 네이티브 API를 사용합니다. Android 네트워크 스택에 대한 자세한 내용은 [Android용 HttpClient 스택 및 SSL/TLS 구현 선택기](~/android/app-fundamentals/http-stack.md)를 참조하세요.

## <a name="specify-the-local-machine-address"></a>로컬 머신 주소 지정

iOS 시뮬레이터 및 Android Emulator는 로컬 머신에서 실행되는 보안 웹 서비스에 액세스할 수 있도록 합니다. 그러나 각 경우의 로컬 머신 주소가 다릅니다.

### <a name="ios"></a>iOS

iOS 시뮬레이터는 호스트 머신 네트워크를 사용합니다. 따라서 시뮬레이터에서 실행 중인 애플리케이션은 머신 IP 주소 또는 `localhost` 호스트 이름을 통해 로컬 머신에서 실행되는 웹 서비스에 연결할 수 있습니다. 예를 들어, `/api/todoitems/` 상대 URI를 통해 GET 작업을 노출하는 로컬 보안 웹 서비스가 있는 경우, iOS 시뮬레이터에서 실행 중인 애플리케이션은 `https://localhost:<port>/api/todoitems/`로 GET 요청을 전송하여 해당 작업을 사용할 수 있습니다.

> [!NOTE]
> Windows의 iOS 시뮬레이터에서 모바일 애플리케이션을 실행하는 경우 애플리케이션이 [Windows용 원격 iOS 시뮬레이터](~/tools/ios-simulator/index.md)에 표시됩니다. 그러나 애플리케이션은 페어링된 Mac에서 실행됩니다. 따라서 Mac에서 실행되는 iOS 애플리케이션의 경우 Windows에서 실행되는 웹 서비스에 localhost 방식으로 액세스할 수 없습니다.

### <a name="android"></a>Android

Android Emulator의 각 인스턴스는 개발 머신 네트워크 인터페이스에서 격리되며 가상 라우터 뒤에서 실행됩니다. 따라서 에뮬레이트된 디바이스에서는 개발 머신 또는 네트워크의 다른 에뮬레이터 인스턴스를 볼 수 없습니다.

그러나 각 에뮬레이터용 가상 라우터는 `10.0.2.2` 주소가 호스트 루프백 인터페이스의 별칭인 미리 할당된 주소(개발 머신의 127.0.0.1)를 포함하는 특정 네트워크 공간을 관리합니다. 따라서 `/api/todoitems/` 상대 URI를 통해 GET 작업을 노출하는 로컬 보안 웹 서비스가 있는 경우, Android Emulator에서 실행 중인 애플리케이션은 `https://10.0.2.2:<port>/api/todoitems/`로 GET 요청을 전송하여 해당 작업을 사용할 수 있습니다.

### <a name="xamarinforms-example"></a>Xamarin.Forms 예제

Xamarin.Forms 애플리케이션에서 [`Device`](xref:Xamarin.Forms.Device) 클래스를 사용하여 애플리케이션이 실행되는 플랫폼을 검색할 수 있습니다. 로컬 보안 웹 서비스에 액세스할 수 있도록 하는 해당 호스트 이름을 다음과 같이 설정할 수 있습니다.

```csharp
public static string BaseAddress =
    Device.RuntimePlatform == Device.Android ? "https://10.0.2.2:5001" : "https://localhost:5001";
public static string TodoItemsUrl = $"{BaseAddress}/api/todoitems/";
```

## <a name="bypass-the-certificate-security-check"></a>인증서 보안 검사 무시

iOS 시뮬레이터 또는 Android Emulator에서 실행되는 애플리케이션에서 로컬 보안 웹 서비스를 호출하려고 하면 각 플랫폼에서 관리형 네트워크 스택을 사용하더라도 `HttpRequestException`이 throw됩니다. 로컬 HTTPS 개발 인증서가 자체 서명되며, 자체 서명된 인증서는 iOS 또는 Android에서 신뢰되지 않기 때문입니다.

따라서 애플리케이션이 로컬 보안 웹 서비스를 사용하는 경우 SSL 오류를 무시해야 합니다. 현재 이를 수행하는 메커니즘은 iOS와 Android가 서로 다릅니다.

### <a name="ios"></a>iOS

관리형 네트워크 스택을 사용할 때 `ServicePointManager.ServerCertificateValidationCallback` 속성을 로컬 HTTPS 개발 인증서의 인증서 보안 검사 결과를 무시하는 콜백으로 설정하면 로컬 보안 웹 서비스의 경우 iOS에서 SSL 오류를 무시할 수 있습니다.

```csharp
#if DEBUG
    System.Net.ServicePointManager.ServerCertificateValidationCallback += (sender, certificate, chain, sslPolicyErrors) =>
    {
        if (certificate.Issuer.Equals("CN=localhost"))
            return true;
        return sslPolicyErrors == System.Net.Security.SslPolicyErrors.None;
    };
#endif
```

이 코드 예제에서 유효성 검사를 진행한 인증서가 `localhost` 인증서가 아닌 경우 서버 인증서 유효성 검사 결과가 반환됩니다. 이 인증서의 경우 유효성 검사 결과가 무시되고 인증서가 유효하다는 것을 나타내는 `true`가 반환됩니다. 이 코드는 `LoadApplication(new App())` 메서드 호출 전에 iOS의 `AppDelegate.FinishedLaunching` 메서드에 추가해야 합니다.

> [!NOTE]
> iOS의 네이티브 네트워크 스택은 `ServerCertificateValidationCallback`에 연결되지 않습니다.

### <a name="android"></a>Android

관리형 네트워크 스택과 네이티브 `AndroidClientHandler` 네트워크 스택을 모두 사용할 때 `HttpClientHandler` 객체에서 `ServerCertificateCustomValidationCallback` 속성을 로컬 HTTPS 개발 인증서의 인증서 보안 검사 결과를 무시하는 콜백으로 설정하면 로컬 보안 웹 서비스의 경우 Android에서 SSL 오류를 무시할 수 있습니다.

```csharp
public HttpClientHandler GetInsecureHandler()
{
    var handler = new HttpClientHandler();
    handler.ServerCertificateCustomValidationCallback = (message, cert, chain, errors) =>
    {
        if (cert.Issuer.Equals("CN=localhost"))
            return true;
        return errors == System.Net.Security.SslPolicyErrors.None;
    };
    return handler;
}
```

이 코드 예제에서 유효성 검사를 진행한 인증서가 `localhost` 인증서가 아닌 경우 서버 인증서 유효성 검사 결과가 반환됩니다. 이 인증서의 경우 유효성 검사 결과가 무시되고 인증서가 유효하다는 것을 나타내는 `true`가 반환됩니다. 결과 `HttpClient` 개체는 `HttpClientHandler` 생성자에 인수로 전달되어야 합니다.

## <a name="related-links"></a>관련 링크

- [TodoREST(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest/)
- [로컬 HTTPS 사용](/aspnet/core/getting-started#enable-local-https)
- [iOS/macOS용 HttpClient 및 SSL/TLS 구현 선택기](~/cross-platform/macios/http-stack.md)
- [Android용 HttpClient 스택 및 SSL/TLS 구현 선택기](~/android/app-fundamentals/http-stack.md)
