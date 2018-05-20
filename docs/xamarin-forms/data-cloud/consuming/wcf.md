---
title: Windows Communication Foundation (WCF) 웹 서비스 사용
description: WCF는 서비스 지향 응용 프로그램을 빌드하기 위한 Microsoft의 통합된 프레임 워크입니다. 개발자가 보안, 신뢰할 수 있는, 트랜잭션 및 상호 운용할 수 있는 분산된 응용 프로그램을 빌드할 수 있습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서는 WCF SOAP Simple Object Access Protocol () 서비스를 사용 하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 23cdc1871511fa75ba2686213d135822ca0fb971
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2018
---
# <a name="consuming-a-windows-communication-foundation-wcf-web-service"></a>Windows Communication Foundation (WCF) 웹 서비스 사용

_WCF는 서비스 지향 응용 프로그램을 빌드하기 위한 Microsoft의 통합된 프레임 워크입니다. 개발자가 보안, 신뢰할 수 있는, 트랜잭션 및 상호 운용할 수 있는 분산된 응용 프로그램을 빌드할 수 있습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서는 WCF SOAP Simple Object Access Protocol () 서비스를 사용 하는 방법을 보여줍니다._

WCF 서비스를 다음을 포함 하는 서로 다른 계약의 다양 한을 설명 합니다.

- **데이터 계약** – 메시지 내에서 콘텐츠에 대 한 기초를 형성 하는 데이터 구조를 정의 합니다.
- **메시지 계약과** -기존 데이터 계약에서 메시지를 작성 합니다.
- **오류 계약** – 허용 사용자 지정 SOAP 오류를 지정 해야 합니다.
- **서비스 계약** – 서비스는 지원 되는 작업을 지정 하 고 각 작업와 상호 작용 하는 데 필요한 메시지입니다. 또한 각 서비스에 대 한 작업에 연결 될 수 있는 모든 사용자 지정 오류 동작을 지정 합니다.

ASP.NET 웹 서비스 (ASMX) 및 WCF에 대 한 차이점은 있지만 WCF와 동일한 기능을 ASMX 제공 – HTTP 통해 SOAP 메시지를 지원 하는지 이해 하는 중요 합니다. ASMX 서비스를 사용 하는 방법에 대 한 자세한 내용은 참조 [를 사용해 ASP.NET 웹 서비스 (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)합니다.

일반적으로 Xamarin 플랫폼 Silverlight 런타임과 함께 제공 되는 WCF의 동일한 클라이언트 하위 집합을 지원 합니다. WCF의 가장 일반적인 인코딩 및 프로토콜 구현을 여기에-텍스트 인코딩 SOAP 메시지의 HTTP 통해 전송 프로토콜을 사용 하는 `BasicHttpBinding` 클래스. 또한 WCF 지원 하려면 프록시를 생성 하는 Windows 환경에만 사용할 수 있는 도구를 사용 합니다.

샘플 응용 프로그램을 함께 제공 되는 추가 정보 파일에는 WCF 서비스를 설정에 대 한 지침을 찾을 수 있습니다. 그러나 샘플 응용 프로그램을 실행할 때 것은 연결할 데이터에 대 한 읽기 전용 액세스를 제공 하는 Xamarin에서 호스팅되는 WCF 서비스 다음 스크린샷에 표시 된 것 처럼:

![](wcf-images/portal.png "샘플 응용 프로그램")

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간에 보안 연결 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 적용 합니다. IOS 9에 대해 빌드된 앱에서 기본적으로 AT를 사용 하므로 모든 연결 AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 AT 옵트인 수 있습니다는 `HTTPS` 및 프로토콜을 인터넷 리소스에 대 한 통신을 보호 합니다. 응용 프로그램의 업데이트 하 여 이렇게 할 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="consuming-the-web-service"></a>웹 서비스 사용

WCF 서비스는 다음 작업을 제공합니다.

|작업|설명|매개 변수|
|--- |--- |--- |
|GetTodoItems|할 일 항목의 목록 가져오기|
|CreateTodoItem|새 할 일 항목을 만듭니다.|XML 직렬화 TodoItem|
|EditTodoItem|할 일 항목 업데이트|XML 직렬화 TodoItem|
|DeleteTodoItem|할 일 항목 삭제|XML 직렬화 TodoItem|

응용 프로그램에서 사용 되는 데이터 모델에 대 한 자세한 내용은 참조 [데이터 모델링](~/xamarin-forms/data-cloud/walkthrough.md)합니다.

> [!NOTE]
> 샘플 응용 프로그램 웹 서비스에 대 한 읽기 전용 액세스를 제공 하는 Xamarin에서 호스팅되는 WCF 서비스를 사용 합니다. 따라서 생성, 업데이트 및 데이터를 삭제 하는 작업은 응용 프로그램에서 사용 하는 데이터가 변경 하지 않습니다. 그러나 ASMX 서비스의 호스팅 가능한 버전은 영어로 **TodoWCFService** 함께 제공 된 샘플 응용 프로그램의 폴더입니다. 이 호스팅 가능 버전 전체 WCF 서비스 사용의 만들기, 업데이트, 읽기 및 데이터에 대 한 액세스를 삭제 합니다.

A *프록시* 를 응용 프로그램이 서비스에 연결할 수 있는 WCF 서비스를 생성 해야 합니다. 프록시는 메서드 및 연결 된 서비스 구성을 정의 하는 서비스 메타 데이터를 사용 하 여 구성 됩니다. 이 메타 데이터는 웹 서비스에 의해 생성 되는 WSDL 웹 서비스 설명 언어 () 문서의 형태로 표시 됩니다. 프록시 웹 서비스에 대 한 서비스 참조를 라이브러리에 추가 하는 표준.NET에서 Visual Studio 2017 Microsoft WCF 웹 서비스 참조 공급자를 사용 하 여 작성할 수 있습니다. Microsoft WCF 웹 서비스 참조 공급자를 사용 하 여 Visual Studio 2017에 프록시를 만드는 대신 ServiceModel Metadata 유틸리티 도구 (svcutil.exe)를 사용 하는 것입니다. 자세한 내용은 참조 [ServiceModel Metadata 유틸리티 도구 (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/)합니다.

생성 된 프록시 클래스는 비동기 프로그래밍 모델 (APM) 디자인 패턴을 사용 하는 웹 서비스를 사용 하기 위한 메서드를 제공 합니다. 이 패턴에서는 비동기 작업을 라는 두 가지 방법으로 구현 *BeginOperationName* 및 *EndOperationName*를 시작 하 고 비동기 작업을 종료 합니다.

*BeginOperationName* 메서드는 비동기 작업을 시작 하 고 구현 하는 개체를 반환 합니다.는 `IAsyncResult` 인터페이스입니다. 호출한 후 *BeginOperationName*, 응용 프로그램 수 동안 명령을 계속 실행할 때 호출 스레드에서 비동기 작업은 스레드 풀 스레드에서 수행 됩니다.

각 호출에 대 한 *BeginOperationName*, 응용 프로그램 호출 또한 해야 *EndOperationName* 작업의 결과를 가져옵니다. 반환 값 *EndOperationName* 동기 웹 서비스 메서드에서 반환 되는 동일한 형식입니다. 예를 들어는 `EndGetTodoItems` 메서드 컬렉션을 반환 `TodoItem` 인스턴스. *EndOperationName* 메서드에 포함 됩니다는 `IAsyncResult` 해당 호출에서 반환한 인스턴스를 설정 해야 하는 매개 변수는 *BeginOperationName* 메서드.

TPL 작업 병렬 라이브러리 () 같은 비동기 작업을 캡슐화 하 여 한 APM begin/end 메서드 쌍을 사용 하는 프로세스를 간소화할 수 `Task` 개체입니다. 이 캡슐화는 여러 오버 로드에서 제공 되는 `TaskFactory.FromAsync` 메서드.

APM 하는 방법에 대 한 자세한 내용은 참조 [비동기 프로그래밍 모델](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) 및 [TPL 및 일반적인.NET Framework 비동기 프로그래밍](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) msdn 합니다.

### <a name="creating-the-todoserviceclient-object"></a>TodoServiceClient 개체 만들기

생성 된 프록시 클래스를 제공는 `TodoServiceClient` HTTP를 통해 WCF 서비스와 통신 하는 데 사용 되는 클래스입니다. URI에서 비동기 작업 서비스 인스턴스를 식별 한 대로 웹 서비스 메서드를 호출 하기 위한 기능을 제공 합니다. 비동기 작업에 대 한 자세한 내용은 참조 [비동기 지원 개요](~/cross-platform/platform/async.md)합니다.

`TodoServiceClient` 클래스 수준에서 인스턴스는 선언으로 다음 코드 예제에 표시 된 대로 WCF 서비스를 사용 하는 응용 프로그램에서 필요로에 거주 하 고 개체:

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

`TodoServiceClient` 바인딩 정보 및 끝점 주소와 인스턴스가 구성 되어 있습니다. 전송, 인코딩 및 응용 프로그램과 서비스에서 상호 통신에 필요한 프로토콜 정보를 지정 하는 바인딩은 사용 됩니다. `BasicHttpBinding` 지정 텍스트 인코딩된 SOAP 메시지를 HTTP 전송 프로토콜을 통해 전송 됩니다. 끝점 주소를 지정 하면 게시 된 인스턴스를 여러 개 있으면는 응용을 WCF 서비스의 다른 인스턴스에 연결할 수 있습니다.

서비스 참조를 구성 하는 방법에 대 한 자세한 내용은 참조 [서비스 참조 구성](~/cross-platform/data-cloud/web-services/index.md#wcf)합니다.

### <a name="creating-data-transfer-objects"></a>데이터 전송 개체 만들기

예제 응용 프로그램 사용은 `TodoItem` 모델 데이터에는 클래스입니다. 저장 하는 `TodoItem` 생성 된 프록시를 먼저 변환 해야 하는 웹 서비스에서 항목 `TodoItem` 유형입니다. 이렇게는 `ToWCFServiceTodoItem` 메서드를 다음 코드 예제에 나와 있는 것 처럼:

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

이 메서드는 단순히 새 만듭니다 `TodoWCFService.TodoItem` 인스턴스를 마우스에서 동일한 속성을 설정 하는 각 속성의 `TodoItem` 인스턴스.

마찬가지로, 웹 서비스에서 데이터를 검색 하는 경우 변환 해야 생성 된 프록시의 `TodoItem` 를 입력 한 `TodoItem` 인스턴스. 이 사용 하 여 수행 되는 `FromWCFServiceTodoItem` 메서드를 다음 코드 예제에 나와 있는 것 처럼:

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

이 메서드는 단순히 데이터에서 생성 된 프록시 검색 `TodoItem` 입력 하 고 새로 만들어진 설정 `TodoItem` 인스턴스.

### <a name="retrieving-data"></a>데이터 검색

`TodoServiceClient.BeginGetTodoItems` 및 `TodoServiceClient.EndGetTodoItems` 메서드는 호출 하는 데 사용 되는 `GetTodoItems` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 다음 코드 예제와 같이 개체:

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

`Task.Factory.FromAsync` 메서드를 만듭니다는 `Task` 를 실행 하는 `TodoServiceClient.EndGetTodoItems` 메서드를 한 번의 `TodoServiceClient.BeginGetTodoItems` 메서드가 완료 되 면와 `null` 나타내는 데이터가 없는에 전달 되는 매개 변수는 `BeginGetTodoItems` 위임 합니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 작업의 실행에 대 한 기본 동작을 사용 하도록 지정 합니다.

`TodoServiceClient.EndGetTodoItems` 메서드가 반환 되는 `ObservableCollection` 의 `TodoWCFService.TodoItem` 다음 변환 되는 인스턴스는 `List` 의 `TodoItem` 디스플레이 대 한 인스턴스.

### <a name="creating-data"></a>데이터 만들기

`TodoServiceClient.BeginCreateTodoItem` 및 `TodoServiceClient.EndCreateTodoItem` 메서드는 호출 하는 데 사용 되는 `CreateTodoItem` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 다음 코드 예제와 같이 개체:

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

`Task.Factory.FromAsync` 메서드를 만듭니다는 `Task` 를 실행 하는 `TodoServiceClient.EndCreateTodoItem` 메서드를 한 번의 `TodoServiceClient.BeginCreateTodoItem` 메서드가 완료 되 면와 `todoItem` 매개 변수가 전달 되는 데이터는 `BeginCreateTodoItem` 대리자는 지정하려면`TodoItem` 웹 서비스를 통해 생성 됩니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 작업의 실행에 대 한 기본 동작을 사용 하도록 지정 합니다.

웹 서비스에서 throw 한 `FaultException` 만들 수 없는 경우는 `TodoItem`는 응용 프로그램에 의해 처리 됩니다.

### <a name="updating-data"></a>데이터 업데이트

`TodoServiceClient.BeginEditTodoItem` 및 `TodoServiceClient.EndEditTodoItem` 메서드는 호출 하는 데 사용 되는 `EditTodoItem` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 다음 코드 예제와 같이 개체:

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

`Task.Factory.FromAsync` 메서드를 만듭니다는 `Task` 를 실행 하는 `TodoServiceClient.EndEditTodoItem` 메서드를 한 번의 `TodoServiceClient.BeginCreateTodoItem` 메서드가 완료 되 면와 `todoItem` 매개 변수가 전달 되는 데이터는 `BeginEditTodoItem` 대리자는 지정하려면`TodoItem` 웹 서비스에 의해 업데이트 해야 합니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 작업의 실행에 대 한 기본 동작을 사용 하도록 지정 합니다.

웹 서비스에서 throw 한 `FaultException` 찾을 수 없거나 업데이트할 수 없는 경우는 `TodoItem`는 응용 프로그램에 의해 처리 됩니다.

### <a name="deleting-data"></a>데이터 삭제

`TodoServiceClient.BeginDeleteTodoItem` 및 `TodoServiceClient.EndDeleteTodoItem` 메서드는 호출 하는 데 사용 되는 `DeleteTodoItem` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 다음 코드 예제와 같이 개체:

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

`Task.Factory.FromAsync` 메서드를 만듭니다는 `Task` 를 실행 하는 `TodoServiceClient.EndDeleteTodoItem` 메서드를 한 번의 `TodoServiceClient.BeginDeleteTodoItem` 메서드가 완료 되 면와 `id` 매개 변수가 전달 되는 데이터는 `BeginDeleteTodoItem` 대리자는 지정하려면`TodoItem` 웹 서비스에 의해 삭제 됩니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 작업의 실행에 대 한 기본 동작을 사용 하도록 지정 합니다.

웹 서비스에서 throw 한 `FaultException` 를 찾거나 삭제할 수 없는 경우는 `TodoItem`는 응용 프로그램에 의해 처리 됩니다.

## <a name="summary"></a>요약

이 문서에는 WCF SOAP 서비스 Xamarin.Forms 응용 프로그램에서 사용 하는 방법을 보여 줍니다. 일반적으로 Xamarin 플랫폼 Silverlight 런타임과 함께 제공 되는 WCF의 동일한 클라이언트 하위 집합을 지원 합니다. WCF의 가장 일반적인 인코딩 및 프로토콜 구현을 여기에-텍스트 인코딩 SOAP 메시지의 HTTP 통해 전송 프로토콜을 사용 하는 `BasicHttpBinding` 클래스. 또한 WCF 지원 하려면 프록시를 생성 하는 Windows 환경에만 사용할 수 있는 도구를 사용 합니다.


## <a name="related-links"></a>관련 링크

- [TodoWCF (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
