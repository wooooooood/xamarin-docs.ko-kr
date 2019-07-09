---
title: Windows Communication Foundation (WCF) 웹 서비스를 사용 합니다.
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 WCF 단순 개체 액세스 프로토콜 (SOAP) 서비스를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/28/2019
ms.openlocfilehash: c79dd6430d387d75acfa010e7f5ad01829f8a6f0
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67658940"
---
# <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>Windows Communication Foundation (WCF) 웹 서비스를 사용 합니다.

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)

_WCF는 서비스 지향 응용 프로그램을 빌드하기 위한 Microsoft의 통합 된 프레임 워크입니다. 개발자가 보안, 신뢰할 수 있는, 트랜잭션 및 상호 운용 가능한 분산된 응용 프로그램을 빌드할 수 있습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 WCF 단순 개체 액세스 프로토콜 (SOAP) 서비스를 사용 하는 방법에 설명 합니다._

WCF에서는 다양 한 포함 하 여 서로 다른 계약을 사용 하 여 서비스를 설명 합니다.

- **데이터 계약** – 메시지 내에서 콘텐츠에 대 한 기반을 형성 하는 데이터 구조를 정의 합니다.
- **메시지 계약** -기존 데이터 계약에서 메시지를 작성 합니다.
- **오류 계약** – 사용자 지정 SOAP 오류를 지정할 수 있도록 합니다.
- **서비스 계약** – 서비스를 지 원하는 작업을 지정 하 고 각 작업을 사용 하 여 상호 작용 하는 데 필요한 메시지입니다. 이러한 각 서비스에 대 한 작업을 사용 하 여 연결할 수 있는 모든 사용자 지정 오류 동작을 지정 합니다.

동일한 기능을 ASMX 제공 – HTTP 통한 SOAP 메시지는 WCF, ASP.NET 웹 서비스 (ASMX) 하며 WCF 지원 간의 차이점입니다. ASMX 서비스를 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [사용 ASP.NET 웹 서비스 (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md)합니다.

> [!IMPORTANT]
> HTTP/HTTPS를 사용 하 여 WCF에 대 한 Xamarin 플랫폼 지원 스타일러스가 텍스트 인코딩된 SOAP 메시지의 제한 된 `BasicHttpBinding` 클래스입니다.
>
> WCF 지원만 프록시를 생성 하 고 호스트를 TodoWCFService Windows 환경에서 사용할 수 있는 도구를 사용을 해야 합니다. 구축 하 고 iOS 앱을 테스트 하는 TodoWCFService Windows 컴퓨터에서 또는 Azure 웹 서비스로 배포 해야 합니다.
>
> Xamarin Forms 네이티브 앱은 일반적으로.NET Standard 클래스 라이브러리를 사용 하 여 코드를 공유 합니다. 그러나.NET Core 지원 하지 않습니다 현재 WCF 되므로 공유 프로젝트에는 레거시 이식 가능한 클래스 라이브러리 여야 합니다. .NET Core에서 WCF 지원에 대 한 자세한 내용은 [.NET Core와.NET Framework 서버 앱에 대해 선택할](/dotnet/standard/choosing-core-framework-server)합니다.

샘플 응용 프로그램 솔루션을 WCF 서비스를 로컬로 실행할 수 있으며 다음 스크린샷에 보이는 것 같습니다.

![](wcf-images/portal.png "샘플 응용 프로그램")

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결 하므로 중요 한 정보가 실수로 유출 방지 적용 합니다. ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되므로 모든 연결이 ATS 보안 요구 사항이 적용 됩니다. 연결에서 이러한 요구를 충족 하지 않는, 예외와 함께 실패 합니다.
>
> 사용할 수 없는 경우의 ATS 옵트인 수 있습니다는 `HTTPS` 프로토콜 및 인터넷 리소스에 대 한 통신을 보호 합니다. 이 앱을 업데이트 하 여 수행할 수 있습니다 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="consume-the-web-service"></a>웹 서비스 사용

WCF 서비스에는 다음 작업을 제공합니다.

|작업|설명|매개 변수|
|--- |--- |--- |
|GetTodoItems|할 일 항목의 목록 가져오기|
|CreateTodoItem|할 일 항목을 새로 만들려면|XML 직렬화 TodoItem|
|EditTodoItem|할 일 항목 업데이트|XML 직렬화 TodoItem|
|DeleteTodoItem|할 일 항목 삭제|XML 직렬화 TodoItem|

응용 프로그램에서 사용 되는 데이터 모델에 대 한 자세한 내용은 참조 하세요. [데이터 모델링은](~/xamarin-forms/data-cloud/web-services/introduction.md)합니다.

A *프록시* 응용 프로그램 서비스에 연결을 허용 하는 WCF 서비스를 사용 하 여 생성 해야 합니다. 프록시는 메서드 및 연결 된 서비스 구성을 정의 하는 서비스 메타 데이터를 사용 하 여 생성 됩니다. 이 메타 데이터는 웹 서비스에 의해 생성 되는 웹 서비스 설명 언어 (WSDL) 문서의 형태로 노출 됩니다. .NET Standard 라이브러리에 웹 서비스에 대 한 서비스 참조를 추가 하려면 Visual Studio 2017에서 Microsoft WCF Web Service Reference Provider 사용 하 여 프록시를 빌드할 수 있습니다. Microsoft WCF Web Service Reference Provider 사용 하 여 Visual Studio 2017에서 프록시를 만드는 대신 ServiceModel Metadata 유틸리티 도구 (svcutil.exe)를 사용 하는 것입니다. 자세한 내용은 [ServiceModel Metadata 유틸리티 도구 (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/)합니다.

생성 된 프록시 클래스의 비동기 프로그래밍 모델 (APM) 디자인 패턴을 사용 하는 웹 서비스 사용에 대 한 메서드를 제공 합니다. 이 패턴에서는 비동기 작업 이라는 두 가지 방법으로 구현 됩니다 *BeginOperationName* 하 고 *EndOperationName*, 시작 하며 비동기 작업을 종료 합니다.

합니다 *BeginOperationName* 메서드는 비동기 작업을 시작 하 고 구현 하는 개체를 반환 합니다.는 `IAsyncResult` 인터페이스입니다. 호출한 후 *BeginOperationName*, 응용 프로그램 수 동안 명령을 계속 실행할 때 호출 스레드에서 스레드 풀 스레드에서 비동기 작업을 수행 합니다.

호출할 때마다 *BeginOperationName*, 응용 프로그램도 호출 해야 *EndOperationName* 작업의 결과를 가져옵니다. 반환 값 *EndOperationName* 동기 웹 서비스 메서드의 반환 형식과 동일 합니다. 예를 들어 합니다 `EndGetTodoItems` 의 컬렉션을 반환 하는 메서드 `TodoItem` 인스턴스. 합니다 *EndOperationName* 메서드에 포함 됩니다는 `IAsyncResult` 에 대 한 해당 호출에서 반환한 인스턴스를 설정 해야 하는 매개 변수를 *BeginOperationName* 메서드.

작업 병렬 라이브러리 (TPL)과 같은 비동기 작업을 캡슐화 하 여 APM begin/end 메서드 쌍을 사용 하는 프로세스를 간소화할 수 `Task` 개체입니다. 여러 오버 로드에서 제공 하는이 캡슐화는 `TaskFactory.FromAsync` 메서드.

APM 대 한 자세한 내용은 참조 하세요 [비동기 프로그래밍 모델](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) 하 고 [TPL 및 일반적인.NET Framework 비동기 프로그래밍](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) MSDN에서.

### <a name="create-the-todoserviceclient-object"></a>TodoServiceClient 개체 만들기

생성된 된 프록시 클래스를 제공 합니다 `TodoServiceClient` HTTP를 통해 WCF 서비스와 통신 하는 데 사용 되는 클래스입니다. URI에서 비동기 작업 서비스 인스턴스를 식별 한 대로 웹 서비스 메서드를 호출 하는 것에 대 한 기능을 제공 합니다. 비동기 작업에 대 한 자세한 내용은 참조 하세요. [비동기 지원 개요](~/cross-platform/platform/async.md)합니다.

`TodoServiceClient` 클래스 수준에서 인스턴스는 선언으로 다음 코드 예제에 표시 된 대로 WCF 서비스를 사용 하도록 해야 응용 프로그램 개체에 대 한 존재 합니다.

```csharp
public class SoapService : ISoapService
{
  ITodoService todoService;
  ...

  public SoapService ()
  {
    todoService = new TodoServiceClient (
      new BasicHttpBinding (),
      new EndpointAddress (Constants.SoapUrl));
  }
  ...
}
```

`TodoServiceClient` 인스턴스가 바인딩 정보 및 끝점 주소를 구성 합니다. 전송, 인코딩 및 서로 통신 하는 응용 프로그램 및 서비스에 필요한 프로토콜 정보를 지정 하는 바인딩을 사용 됩니다. `BasicHttpBinding` 텍스트 인코딩된 SOAP 메시지는 HTTP 전송 프로토콜을 통해 전송 되도록 지정 합니다. 끝점 주소를 지정 하는 게시 된 인스턴스가 여러 개 있는 응용 프로그램을을 WCF 서비스의 다른 인스턴스에 연결할 수 있습니다.

서비스 참조를 구성 하는 방법에 대 한 자세한 내용은 참조 하세요. [서비스 참조 구성](~/cross-platform/data-cloud/web-services/index.md#wcf)합니다.

### <a name="create-data-transfer-objects"></a>데이터 전송 개체 만들기

샘플 응용 프로그램을 `TodoItem` 모델 데이터에는 클래스입니다. 저장 하는 `TodoItem` 생성 된 프록시를 먼저 변환 해야 하는 웹 서비스에 대 한 항목 `TodoItem` 형식입니다. 이 `ToWCFServiceTodoItem` 메서드를 다음 코드 예제 에서처럼:

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

이 메서드는 단순히 새 만듭니다 `TodoWCFService.TodoItem` 인스턴스로 이동한 각 속성에서 동일한 속성을 설정 합니다 `TodoItem` 인스턴스.

마찬가지로 데이터 웹 서비스에서 검색 되 면 변환 해야 생성 된 프록시에서 `TodoItem` 에 입력을 `TodoItem` 인스턴스. 사용 하 여 이렇게는 `FromWCFServiceTodoItem` 메서드를 다음 코드 예제 에서처럼:

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
{
  return new TodoItem
  {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

이 메서드는 단순히 데이터에서 생성 된 프록시 검색 `TodoItem` 입력 하 고 새로 만든에서 설정 `TodoItem` 인스턴스.

### <a name="retrieve-data"></a>데이터 검색

합니다 `TodoServiceClient.BeginGetTodoItems` 하 고 `TodoServiceClient.EndGetTodoItems` 메서드를 호출 하는 데 사용 됩니다는 `GetTodoItems` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 와 다음 코드 예제와 같이 개체:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems)
  {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` 메서드를 만듭니다를 `Task` 실행 하는 합니다 `TodoServiceClient.EndGetTodoItems` 메서드를 한 번를 `TodoServiceClient.BeginGetTodoItems` 메서드가 완료 되 면 사용 하 여를 `null` 데이터가 없는에 전달 되 고 있는지를 나타내는 매개 변수를 `BeginGetTodoItems` 대리자. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 태스크 실행에 대 한 기본 동작을 사용 해야 함을 지정 합니다.

`TodoServiceClient.EndGetTodoItems` 메서드가 반환 되는 `ObservableCollection` 의 `TodoWCFService.TodoItem` 변환 됩니다 하는 인스턴스를 `List` 의 `TodoItem` 표시에 대 한 인스턴스.

### <a name="create-data"></a>데이터 만들기

합니다 `TodoServiceClient.BeginCreateTodoItem` 하 고 `TodoServiceClient.EndCreateTodoItem` 메서드를 호출 하는 데 사용 됩니다는 `CreateTodoItem` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 와 다음 코드 예제와 같이 개체:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` 메서드를 만듭니다를 `Task` 실행 하는 합니다 `TodoServiceClient.EndCreateTodoItem` 메서드를 한 번를 `TodoServiceClient.BeginCreateTodoItem` 메서드가 완료 되 면 사용 하 여는 `todoItem` 매개 변수를 전달 되는 데이터는 `BeginCreateTodoItem` 대리자를 지정 합니다 `TodoItem` 웹 서비스를 통해 생성 됩니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 태스크 실행에 대 한 기본 동작을 사용 해야 함을 지정 합니다.

웹 서비스 throw를 `FaultException` 만들 수 없는 경우는 `TodoItem`, 응용 프로그램에서 처리 되는 합니다.

### <a name="update-data"></a>업데이트 데이터

합니다 `TodoServiceClient.BeginEditTodoItem` 하 고 `TodoServiceClient.EndEditTodoItem` 메서드를 호출 하는 데 사용 됩니다는 `EditTodoItem` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 와 다음 코드 예제와 같이 개체:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` 메서드를 만듭니다를 `Task` 실행 하는 합니다 `TodoServiceClient.EndEditTodoItem` 메서드를 한 번를 `TodoServiceClient.BeginCreateTodoItem` 메서드가 완료 되 면 사용 하 여는 `todoItem` 매개 변수를 전달 되는 데이터는 `BeginEditTodoItem` 대리자를 지정 합니다 `TodoItem` 웹 서비스에서 업데이트 해야 합니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 태스크 실행에 대 한 기본 동작을 사용 해야 함을 지정 합니다.

웹 서비스 throw를 `FaultException` 찾거나 업데이트 하지 못한 경우는 `TodoItem`, 응용 프로그램에서 처리 되는 합니다.

### <a name="delete-data"></a>데이터 삭제

합니다 `TodoServiceClient.BeginDeleteTodoItem` 하 고 `TodoServiceClient.EndDeleteTodoItem` 메서드를 호출 하는 데 사용 됩니다는 `DeleteTodoItem` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 와 다음 코드 예제와 같이 개체:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  ...
  await Task.Factory.FromAsync (
    todoService.BeginDeleteTodoItem,
    todoService.EndDeleteTodoItem,
    id,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` 메서드를 만듭니다를 `Task` 실행 하는 합니다 `TodoServiceClient.EndDeleteTodoItem` 메서드를 한 번를 `TodoServiceClient.BeginDeleteTodoItem` 메서드가 완료 되 면 사용 하 여는 `id` 매개 변수를 전달 되는 데이터는 `BeginDeleteTodoItem` 대리자를 지정 합니다 `TodoItem` 웹 서비스에 의해 삭제 됩니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 태스크 실행에 대 한 기본 동작을 사용 해야 함을 지정 합니다.

웹 서비스 throw를 `FaultException` 찾거나 삭제 하지 못한 경우는 `TodoItem`, 응용 프로그램에서 처리 되는 합니다.

## <a name="configure-remote-access-to-iis-express"></a>IIS Express에 대 한 원격 액세스 구성
Visual Studio 2017 또는 Visual Studio 2019에서 추가 구성이 없는 PC에서 UWP 응용 프로그램을 테스트할 수 있어야 합니다. Android 및 iOS 클라이언트를 테스트 합니다.이 섹션의 추가 단계가 필요할 수 있습니다. 참조 [iOS 시뮬레이터 및 Android 에뮬레이터에서에서 로컬 웹 서비스에 연결](~/cross-platform/deploy-test/connect-to-local-web-services.md) 자세한 내용은 합니다.

기본적으로 IIS Express는 요청에 응답 하도록 `localhost`합니다. 원격 장치 (예: Android 장치, iPhone 또는 시뮬레이터에도) 로컬 WCF 서비스에 액세스할을 수 없습니다. 로컬 네트워크에서 Windows 10 워크스테이션 IP 주소를 확인 해야 합니다. 이 예에서는 사용자의 워크스테이션에 IP 주소를 가정 `192.168.1.143`합니다. 다음 단계는 원격 연결을 허용 하 고 실제 또는 가상 장치에서 서비스에 연결 하려면 Windows 10 및 IIS Express를 구성 하는 방법을 설명 합니다.

1. **Windows 방화벽 예외가 추가**합니다. 서브넷에서 응용 프로그램은 WCF 서비스와 통신 하는 데 사용할 수 있는 Windows 방화벽을 통해 포트를 열어야 합니다. 방화벽에서 포트 49393를 여는 인바운드 규칙을 만듭니다. 관리 명령 프롬프트에서이 명령을 실행 합니다.
    ```
    netsh advfirewall firewall add rule name="TodoWCFService" dir=in protocol=tcp localport=49393 profile=private remoteip=localsubnet action=allow
    ```

1. **동의 원격 연결에 IIS Express를 구성**합니다. IIS Express에 대 한 구성 파일을 편집 하 여 IIS Express를 구성할 수 있습니다 **[솔루션 디렉터리]\.vs\config\applicationhost.config**합니다. 찾을 합니다 `site` 이름의 요소가 `TodoWCFService`합니다. 다음 XML과 유사한 같아야 합니다.

    ```xml
    <site name="TodoWCFService" id="2">
        <application path="/" applicationPool="Clr4IntegratedAppPool">
            <virtualDirectory path="/" physicalPath="C:\Users\tom\TodoWCF\TodoWCFService\TodoWCFService" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:49393:localhost" />
        </bindings>
    </site>
    ```

    두 개의 추가 해야 `binding` 열어야 요소 외부 트래픽 및 Android 에뮬레이터에 49393 포트입니다. 바인딩 사용을 `[IP address]:[port]:[hostname]` 어떻게 IIS Express는 요청에 응답을 지정 하는 형식입니다. 외부 요청으로 지정 해야 하는 호스트 이름을 갖습니다는 `binding`합니다. 다음 XML을 추가 하는 `bindings` 요소를 사용자 고유의 IP 주소를 사용 하 여 IP 주소를 대체 합니다.

    ```xml
    <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
    <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
    ```

    변경 후는 `bindings` 요소는 다음과 같이 표시 됩니다.

    ```xml
    <site name="TodoWCFService" id="2">
        <application path="/" applicationPool="Clr4IntegratedAppPool">
            <virtualDirectory path="/" physicalPath="C:\Users\tom\TodoWCF\TodoWCFService\TodoWCFService" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:49393:localhost" />
            <binding protocol="http" bindingInformation="*:49393:192.168.1.143" />
            <binding protocol="http" bindingInformation="*:49393:127.0.0.1" />
        </bindings>
    </site>
    ```

    >[!IMPORTANT]
    >기본적으로 IIS Express는 보안상의 이유로 외부 원본에서 연결 허용 하지 됩니다. 원격 장치에서의 연결을 사용 하도록 설정 하려면 실행 해야 IIS Express 관리자 권한으로 합니다. 이 작업을 수행 하는 가장 쉬운 방법은 관리자 권한으로 Visual Studio 2017을 실행 하는 것입니다. 이 시작 됩니다 IIS Express 관리자 권한으로는 TodoWCFService를 실행 하는 경우.

    이 단계 전체를 사용 하 여 TodoWCFService를 실행 하 고 서브넷에서 다른 장치에서 연결 수 있어야 합니다. 응용 프로그램을 실행 하 고 방문 하 여 테스트할 수 있습니다 `http://localhost:49393/TodoService.svc`합니다. 표시 되 면를 **잘못 된 요청** 해당 URL을 방문 하는 동안 오류가 발생 했습니다. 프로그램 `bindings` (요청이 IIS Express에 도달 하지만 거부 됩니다.) IIS Express 구성에서 잘못 된 것 같습니다. 다른 오류가 발생할 경우 응용 프로그램이 실행 되지 않거나 방화벽이 잘못 구성 되어 있는지 수 있습니다.

    실행 및 서비스 제공을 유지 하려면 IIS Express를 허용 하려면 기능을 해제 합니다 **편집 하며 계속 하기** 옵션 **프로젝트 속성 > 웹 > 디버거**합니다.

1. **서비스에 액세스를 사용 하 여 장치 엔드포인트 사용자 지정**합니다. 이 단계에서는 클라이언트 응용 프로그램을 실제 또는 에뮬레이트된 장치에서 실행 중인 WCF 서비스에 액세스를 구성 합니다.

    에뮬레이터는 호스트 컴퓨터에 직접 액세스 하지 못하도록 제한 하는 내부 프록시를 활용 하는 Android 에뮬레이터 `localhost` 주소입니다. 대신 주소 `10.0.2.2` 에뮬레이터에서 라우팅되는 `localhost` 내부 프록시를 통해 호스트 컴퓨터. 이러한 프록시 요청 해야 `127.0.0.1` 위의 단계에서이 호스트에 대 한 IIS Express 바인딩을 만든 이유는 요청 헤더에 호스트 이름으로 합니다.

    Mac에서 시뮬레이터를 실행 하는 iOS 빌드 호스트를 사용 하는 경우에 합니다 [원격 iOS 시뮬레이터에 대 한 Windows](~/tools/ios-simulator/index.md)합니다. 시뮬레이터에서 네트워크 요청 호스트 이름으로 로컬 네트워크에서 워크스테이션 IP 해야 (이 예제의 있기 `192.168.1.143`, 실제 IP 주소는 다를 수 있지만). 이 때문에 위의 단계에서이 호스트에 대 한 IIS Express 바인딩을 생성 합니다.

    확인 합니다 `SoapUrl` 의 속성을 **Constants.cs** TodoWCF (이식 가능) 프로젝트 파일에에서 있는 네트워크에 대 한 올바른 값:

    ```csharp
    public static string SoapUrl
    {
        get
        {
            var defaultUrl = "http://localhost:49393/TodoService.svc";

            if (Device.RuntimePlatform == Device.Android)
            {
                defaultUrl = "http://10.0.2.2:49393/TodoService.svc";
            }
            else if (Device.RuntimePlatform == Device.iOS)
            {
                defaultUrl = "http://192.168.1.143:49393/TodoService.svc";
            }

            return defaultUrl;
        }
    }
    ```

    구성한 후 합니다 **Constants.cs** 적절 한 끝점을 사용 하 여 실제 또는 가상 장치에서 Windows 10 워크스테이션에서 실행 중인 TodoWCFService에 연결할 수 있어야 합니다.

## <a name="related-links"></a>관련 링크

- [TodoWCF (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [방법: Windows Communication Foundation 클라이언트 만들기](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [ServiceModel Metadata 유틸리티 도구 (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
