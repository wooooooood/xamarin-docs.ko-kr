---
title: Windows Communication Foundation (WCF) 웹 서비스 사용
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 WCF 단순 개체 액세스 프로토콜 (SOAP) 서비스를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 7e8acc6e8aaf8b8e0e8cec7d5d0f3e28cf60073a
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055597"
---
# <a name="consuming-a-windows-communication-foundation-wcf-web-service"></a>Windows Communication Foundation (WCF) 웹 서비스 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)

_WCF는 서비스 지향 응용 프로그램을 빌드하기 위한 Microsoft의 통합 된 프레임 워크입니다. 개발자가 보안, 신뢰할 수 있는, 트랜잭션 및 상호 운용 가능한 분산된 응용 프로그램을 빌드할 수 있습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 WCF 단순 개체 액세스 프로토콜 (SOAP) 서비스를 사용 하는 방법에 설명 합니다._

WCF에서는 다양 한 다음을 포함 하는 서로 다른 계약을 사용 하 여 서비스를 설명 합니다.

- **데이터 계약** – 메시지 내에서 콘텐츠에 대 한 기반을 형성 하는 데이터 구조를 정의 합니다.
- **메시지 계약** -기존 데이터 계약에서 메시지를 작성 합니다.
- **오류 계약** – 사용자 지정 SOAP 오류를 지정할 수 있도록 합니다.
- **서비스 계약** – 서비스를 지 원하는 작업을 지정 하 고 각 작업을 사용 하 여 상호 작용 하는 데 필요한 메시지입니다. 이러한 각 서비스에 대 한 작업을 사용 하 여 연결할 수 있는 모든 사용자 지정 오류 동작을 지정 합니다.

ASP.NET 웹 서비스 (ASMX)와 WCF 사이의 차이가 이지만 WCF와 동일한 기능을 ASMX 제공 – HTTP 통한 SOAP 메시지를 지원 하는지 이해 해야 합니다. ASMX 서비스를 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [사용 ASP.NET 웹 서비스 (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)합니다.

일반적으로 Xamarin 플랫폼에는 Silverlight 런타임과 함께 제공 되는 WCF의 동일한 클라이언트 쪽 하위 집합을 지원 합니다. WCF의 가장 일반적인 인코딩 및 프로토콜 구현은 여기에-텍스트 인코딩된 SOAP 메시지는 HTTP 통해 전송 프로토콜을 사용 하는 `BasicHttpBinding` 클래스입니다. 또한 WCF 지원만 프록시를 생성할 Windows 환경에서 사용할 수 있는 도구의 사용을 해야 합니다.

WCF 서비스를 설정 하는 방법은 샘플 응용 프로그램을 함께 제공 되는 추가 정보 파일에서 찾을 수 있습니다. 그러나 샘플 응용 프로그램을 실행 하는 경우는 연결 데이터에 대 한 읽기 전용 액세스를 제공 하는 Xamarin에서 호스팅되는 WCF 서비스에 다음 스크린샷에 표시 된 대로:

![](wcf-images/portal.png "샘플 응용 프로그램")

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결 하므로 중요 한 정보가 실수로 유출 방지 적용 합니다. ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되므로 모든 연결이 ATS 보안 요구 사항이 적용 됩니다. 연결에서 이러한 요구를 충족 하지 않는, 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 ATS 옵트인 수 있습니다는 `HTTPS` 프로토콜 및 인터넷 리소스에 대 한 통신을 보호 합니다. 이 앱을 업데이트 하 여 수행할 수 있습니다 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="consuming-the-web-service"></a>웹 서비스 사용

WCF 서비스에는 다음 작업을 제공합니다.

|작업|설명|매개 변수|
|--- |--- |--- |
|GetTodoItems|할 일 항목의 목록 가져오기|
|CreateTodoItem|할 일 항목을 새로 만들려면|XML 직렬화 TodoItem|
|EditTodoItem|할 일 항목 업데이트|XML 직렬화 TodoItem|
|DeleteTodoItem|할 일 항목 삭제|XML 직렬화 TodoItem|

응용 프로그램에서 사용 되는 데이터 모델에 대 한 자세한 내용은 참조 하세요. [데이터 모델링은](~/xamarin-forms/data-cloud/walkthrough.md)합니다.

> [!NOTE]
> 샘플 응용 프로그램 웹 서비스에 대 한 읽기 전용 액세스를 제공 하는 Xamarin에서 호스팅되는 WCF 서비스를 사용 합니다. 따라서 만들기, 업데이트 및 데이터를 삭제 하는 작업은 응용 프로그램에서 사용 된 데이터를 변경 하지 않습니다. 그러나 ASMX 서비스의 호스팅 가능한 버전은 영어로 합니다 **TodoWCFService** 함께 제공 되는 샘플 응용 프로그램 폴더입니다. 이 호스팅 가능 버전 전체 WCF 서비스 허용의 만들기, 업데이트, 읽기 및 삭제 데이터에 대 한 액세스.

A *프록시* 응용 프로그램 서비스에 연결을 허용 하는 WCF 서비스를 사용 하 여 생성 해야 합니다. 프록시는 메서드 및 연결 된 서비스 구성을 정의 하는 서비스 메타 데이터를 사용 하 여 생성 됩니다. 이 메타 데이터는 웹 서비스에 의해 생성 되는 웹 서비스 설명 언어 (WSDL) 문서의 형태로 노출 됩니다. .NET Standard 라이브러리에 웹 서비스에 대 한 서비스 참조를 추가 하려면 Visual Studio 2017에서 Microsoft WCF Web Service Reference Provider 사용 하 여 프록시를 빌드할 수 있습니다. Microsoft WCF Web Service Reference Provider 사용 하 여 Visual Studio 2017에서 프록시를 만드는 대신 ServiceModel Metadata 유틸리티 도구 (svcutil.exe)를 사용 하는 것입니다. 자세한 내용은 [ServiceModel Metadata 유틸리티 도구 (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/)합니다.

생성 된 프록시 클래스의 비동기 프로그래밍 모델 (APM) 디자인 패턴을 사용 하는 웹 서비스 사용에 대 한 메서드를 제공 합니다. 이 패턴에서는 비동기 작업 이라는 두 가지 방법으로 구현 됩니다 *BeginOperationName* 하 고 *EndOperationName*, 시작 하며 비동기 작업을 종료 합니다.

합니다 *BeginOperationName* 메서드는 비동기 작업을 시작 하 고 구현 하는 개체를 반환 합니다.는 `IAsyncResult` 인터페이스입니다. 호출한 후 *BeginOperationName*, 응용 프로그램 수 동안 명령을 계속 실행할 때 호출 스레드에서 스레드 풀 스레드에서 비동기 작업을 수행 합니다.

호출할 때마다 *BeginOperationName*, 응용 프로그램도 호출 해야 *EndOperationName* 작업의 결과를 가져옵니다. 반환 값 *EndOperationName* 동기 웹 서비스 메서드의 반환 형식과 동일 합니다. 예를 들어 합니다 `EndGetTodoItems` 의 컬렉션을 반환 하는 메서드 `TodoItem` 인스턴스. 합니다 *EndOperationName* 메서드에 포함 됩니다는 `IAsyncResult` 에 대 한 해당 호출에서 반환한 인스턴스를 설정 해야 하는 매개 변수를 *BeginOperationName* 메서드.

작업 병렬 라이브러리 (TPL)과 같은 비동기 작업을 캡슐화 하 여 APM begin/end 메서드 쌍을 사용 하는 프로세스를 간소화할 수 `Task` 개체입니다. 여러 오버 로드에서 제공 하는이 캡슐화는 `TaskFactory.FromAsync` 메서드.

APM 대 한 자세한 내용은 참조 하세요 [비동기 프로그래밍 모델](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) 하 고 [TPL 및 일반적인.NET Framework 비동기 프로그래밍](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) MSDN에서.

### <a name="creating-the-todoserviceclient-object"></a>TodoServiceClient 개체 만들기

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

### <a name="creating-data-transfer-objects"></a>데이터 전송 개체 만들기

샘플 응용 프로그램을 `TodoItem` 모델 데이터에는 클래스입니다. 저장 하는 `TodoItem` 생성 된 프록시를 먼저 변환 해야 하는 웹 서비스에 대 한 항목 `TodoItem` 형식입니다. 이 `ToWCFServiceTodoItem` 메서드를 다음 코드 예제 에서처럼:

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem {
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
  return new TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

이 메서드는 단순히 데이터에서 생성 된 프록시 검색 `TodoItem` 입력 하 고 새로 만든에서 설정 `TodoItem` 인스턴스.

### <a name="retrieving-data"></a>데이터 검색

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

  foreach (var item in todoItems) {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` 메서드를 만듭니다를 `Task` 실행 하는 합니다 `TodoServiceClient.EndGetTodoItems` 메서드를 한 번를 `TodoServiceClient.BeginGetTodoItems` 메서드가 완료 되 면 사용 하 여를 `null` 데이터가 없는에 전달 되 고 있는지를 나타내는 매개 변수를 `BeginGetTodoItems` 대리자. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 태스크 실행에 대 한 기본 동작을 사용 해야 함을 지정 합니다.

`TodoServiceClient.EndGetTodoItems` 메서드가 반환 되는 `ObservableCollection` 의 `TodoWCFService.TodoItem` 변환 됩니다 하는 인스턴스를 `List` 의 `TodoItem` 표시에 대 한 인스턴스.

### <a name="creating-data"></a>데이터 만들기

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

### <a name="updating-data"></a>데이터 업데이트

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

### <a name="deleting-data"></a>데이터 삭제

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

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms 응용 프로그램에서 WCF SOAP 서비스를 사용 하는 방법을 보여 줍니다. 일반적으로 Xamarin 플랫폼에는 Silverlight 런타임과 함께 제공 되는 WCF의 동일한 클라이언트 쪽 하위 집합을 지원 합니다. WCF의 가장 일반적인 인코딩 및 프로토콜 구현은 여기에-텍스트 인코딩된 SOAP 메시지는 HTTP 통해 전송 프로토콜을 사용 하는 `BasicHttpBinding` 클래스입니다. 또한 WCF 지원만 프록시를 생성할 Windows 환경에서 사용할 수 있는 도구의 사용을 해야 합니다.


## <a name="related-links"></a>관련 링크

- [TodoWCF (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
