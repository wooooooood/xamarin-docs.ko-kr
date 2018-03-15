---
title: "ASP.NET 웹 서비스 (ASMX)를 사용합니다."
description: "ASMX 웹 서비스는 SOAP Simple Object Access Protocol ()를 사용 하 여 메시지를 보내는 작성 하는 기능을 제공 합니다. SOAP는 빌드하고 웹 서비스에 액세스 하기 위한 플랫폼 독립적, 언어 독립적인 프로토콜입니다. 소비자는 ASMX 서비스의 플랫폼, 개체 모델 또는 서비스를 구현 하는 데 사용 되는 프로그래밍 언어에 대 한 어떠한 정보도 알 필요가 없습니다. SOAP 메시지를 송수신 하는 방법을 이해 하려면 하기만 합니다. 이 문서는 ASMX SOAP 서비스 Xamarin.Forms 응용 프로그램에서 사용 하는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: a095dbbb78ad1517791356ae0b7cbeaa94d1336f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>ASP.NET 웹 서비스 (ASMX)를 사용합니다.

_ASMX 웹 서비스는 SOAP Simple Object Access Protocol ()를 사용 하 여 메시지를 보내는 작성 하는 기능을 제공 합니다. SOAP는 빌드하고 웹 서비스에 액세스 하기 위한 플랫폼 독립적, 언어 독립적인 프로토콜입니다. 소비자는 ASMX 서비스의 플랫폼, 개체 모델 또는 서비스를 구현 하는 데 사용 되는 프로그래밍 언어에 대 한 어떠한 정보도 알 필요가 없습니다. SOAP 메시지를 송수신 하는 방법을 이해 하려면 하기만 합니다. 이 문서는 ASMX SOAP 서비스 Xamarin.Forms 응용 프로그램에서 사용 하는 방법을 보여 줍니다._

SOAP 메시지는 다음과 같은 요소를 포함 하는 XML 문서:

- 명명 된 루트 요소가 *봉투 (envelope)* SOAP 메시지가 XML 문서를 식별 하는 합니다.
- 선택적 *헤더* 인증 데이터 등의 응용 프로그램 관련 정보를 포함 하는 요소입니다. 경우는 *헤더* 요소가 있는지의 첫 번째 자식 요소 여야 합니다는 *봉투 (envelope)* 요소입니다.
- 필수 *본문* 수신자를 위한 SOAP 메시지를 포함 하는 요소입니다.
- 선택적 *오류* 오류 메시지를 나타내는 데 사용 되는 요소입니다. 경우는 *오류* 요소가 있는지의 자식 요소 여야 합니다.는 *본문* 요소입니다.

HTTP, SMTP, TCP 및 UDP를 포함 하 여 여러 전송 프로토콜을 통해 SOAP 작동할 수 있습니다. 그러나 HTTP를 통해는 ASMX 서비스 에서만 작동할 수 있습니다. Xamarin 플랫폼은 HTTP를 통해 표준 SOAP 1.1 구현을 지원 하 고 다양 한 표준 ASMX 서비스 구성에 대 한 지원을 포함 합니다.

샘플 응용 프로그램을 함께 제공 되는 추가 정보 파일에는 ASMX 서비스 설정에 대 한 지침을 찾을 수 있습니다. 그러나 예제 응용 프로그램을 실행 합니다 연결 데이터에 대 한 읽기 전용 액세스를 제공 하는 ASMX Xamarin 호스팅 서비스에 다음 스크린샷에 표시 된 것 처럼:

![](asmx-images/portal.png "샘플 응용 프로그램")

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간에 보안 연결 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 적용 합니다. IOS 9에 대해 빌드된 앱에서 기본적으로 AT를 사용 하므로 모든 연결 AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 AT 옵트인 수 있습니다는 `HTTPS` 및 프로토콜을 인터넷 리소스에 대 한 통신을 보호 합니다. 응용 프로그램의 업데이트 하 여 이렇게 할 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="consuming-the-web-service"></a>웹 서비스 사용

ASMX 서비스는 다음 작업을 제공합니다.

|작업|설명|매개 변수|
|--- |--- |--- |
|GetTodoItems|할 일 항목의 목록 가져오기|
|CreateTodoItem|새 할 일 항목을 만듭니다.|XML 직렬화 TodoItem|
|EditTodoItem|할 일 항목 업데이트|XML 직렬화 TodoItem|
|DeleteTodoItem|할 일 항목 삭제|XML 직렬화 TodoItem|

응용 프로그램에서 사용 되는 데이터 모델에 대 한 자세한 내용은 참조 [데이터 모델링](~/xamarin-forms/data-cloud/walkthrough.md)합니다.

> [!NOTE]
> 샘플 응용 프로그램 웹 서비스에 대 한 읽기 전용 액세스를 제공 하는 ASMX Xamarin 호스팅 서비스를 사용 합니다. 따라서 생성, 업데이트 및 데이터를 삭제 하는 작업은 응용 프로그램에서 사용 하는 데이터가 변경 하지 않습니다. 그러나 ASMX 서비스의 호스팅 가능한 버전은 영어로 **TodoASMXService** 함께 제공 된 샘플 응용 프로그램의 폴더입니다. 이 호스팅 가능 버전 전체 ASMX 서비스 허용의 만들기, 업데이트, 읽기 및 데이터에 대 한 액세스를 삭제 합니다.

A *프록시* 응용 프로그램이 서비스에 연결할 수 있는 ASMX 서비스를 사용할 생성 해야 합니다. 프록시는 메서드 및 연결 된 서비스 구성을 정의 하는 서비스 메타 데이터를 사용 하 여 구성 됩니다. 이 메타 데이터는 웹 서비스에 의해 생성 되는 WSDL 웹 서비스 설명 언어 () 문서의 형태로 표시 됩니다. 프록시는 플랫폼별 프로젝트에 웹 서비스에 대 한 웹 참조를 추가 하 여 작성 됩니다.

생성 된 프록시 클래스는 비동기 프로그래밍 모델 (APM) 디자인 패턴을 사용 하는 웹 서비스를 사용 하기 위한 메서드를 제공 합니다. 이 패턴에서는 비동기 작업을 라는 두 가지 방법으로 구현 *BeginOperationName* 및 *EndOperationName*를 시작 하 고 비동기 작업을 종료 합니다.

*BeginOperationName* 메서드는 비동기 작업을 시작 하 고 구현 하는 개체를 반환 합니다.는 `IAsyncResult` 인터페이스입니다. 호출한 후 *BeginOperationName*, 응용 프로그램 수 동안 명령을 계속 실행할 때 호출 스레드에서 비동기 작업은 스레드 풀 스레드에서 수행 됩니다.

각 호출에 대 한 *BeginOperationName*, 응용 프로그램 호출 또한 해야 *EndOperationName* 작업의 결과를 가져옵니다. 반환 값 *EndOperationName* 동기 웹 서비스 메서드에서 반환 되는 동일한 형식입니다. 예를 들어는 `EndGetTodoItems` 메서드 컬렉션을 반환 `TodoItem` 인스턴스. *EndOperationName* 메서드에 포함 됩니다는 `IAsyncResult` 해당 호출에서 반환한 인스턴스를 설정 해야 하는 매개 변수는 *BeginOperationName* 메서드.

TPL 작업 병렬 라이브러리 () 같은 비동기 작업을 캡슐화 하 여 한 APM begin/end 메서드 쌍을 사용 하는 프로세스를 간소화할 수 `Task` 개체입니다. 이 캡슐화는 여러 오버 로드에서 제공 되는 `TaskFactory.FromAsync` 메서드.

APM 하는 방법에 대 한 자세한 내용은 참조 [비동기 프로그래밍 모델](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) 및 [TPL 및 일반적인.NET Framework 비동기 프로그래밍](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) msdn 합니다.

### <a name="creating-the-todoservice-object"></a>TodoService 개체 만들기

생성 된 프록시 클래스를 제공는 `TodoService` HTTP를 통해 ASMX 서비스와 통신 하는 데 사용 되는 클래스입니다. URI에서 비동기 작업 서비스 인스턴스를 식별 한 대로 웹 서비스 메서드를 호출 하기 위한 기능을 제공 합니다. 비동기 작업에 대 한 자세한 내용은 참조 [비동기 지원 개요](~/cross-platform/platform/async.md)합니다.

`TodoService` 클래스 수준에서 인스턴스는 선언으로 다음 코드 예제에 나와 있는 것 처럼 ASMX 서비스를 사용 하는 응용 프로그램에서 필요로에 거주 하 고 개체:

```csharp
public class SoapService : ISoapService
{
  ASMXService.TodoService asmxService;
  ...

  public SoapService ()
  {
    asmxService = new ASMXService.TodoService (Constants.SoapUrl);
  }
  ...
}
```

`TodoService` 생성자 ASMX 서비스 인스턴스의 URL을 지정 하는 선택적 문자열 매개 변수를 사용 합니다. 이 통해 게시 된 인스턴스를 여러 개 있으면 ASMX 서비스의 다른 인스턴스에 연결 하도록 응용 프로그램.

### <a name="creating-data-transfer-objects"></a>데이터 전송 개체 만들기

예제 응용 프로그램 사용은 `TodoItem` 모델 데이터에는 클래스입니다. 저장 하는 `TodoItem` 생성 된 프록시를 먼저 변환 해야 하는 웹 서비스에서 항목 `TodoItem` 유형입니다. 이렇게는 `ToASMXServiceTodoItem` 메서드를 다음 코드 예제에 나와 있는 것 처럼:

```csharp
ASMXService.TodoItem ToASMXServiceTodoItem (TodoItem item)
{
  return new ASMXService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

이 메서드는 단순히 새 만듭니다 `ASMService.TodoItem` 인스턴스를 마우스에서 동일한 속성을 설정 하는 각 속성의 `TodoItem` 인스턴스.

마찬가지로, 웹 서비스에서 데이터를 검색 하는 경우 변환 해야 생성 된 프록시의 `TodoItem` 를 입력 한 `TodoItem` 인스턴스. 이 사용 하 여 수행 되는 `FromASMXServiceTodoItem` 메서드를 다음 코드 예제에 나와 있는 것 처럼:

```csharp
static TodoItem FromASMXServiceTodoItem (ASMXService.TodoItem item)
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

`TodoService.BeginGetTodoItems` 및 `TodoService.EndGetTodoItems` 메서드는 호출 하는 데 사용 되는 `GetTodoItems` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 다음 코드 예제와 같이 개체:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromASMXServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` 메서드를 만듭니다는 `Task` 를 실행 하는 `TodoService.EndGetTodoItems` 메서드를 한 번의 `TodoService.BeginGetTodoItems` 메서드가 완료 되 면와 `null` 나타내는 데이터가 없는에 전달 되는 매개 변수는 `BeginGetTodoItems` 위임 합니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 작업의 실행에 대 한 기본 동작을 사용 하도록 지정 합니다.

`TodoService.EndGetTodoItems` 메서드 배열을 반환 `ASMXService.TodoItem` 다음 변환 되는 인스턴스는 `List` 의 `TodoItem` 디스플레이 대 한 인스턴스.

### <a name="creating-data"></a>데이터 만들기

`TodoService.BeginCreateTodoItem` 및 `TodoService.EndCreateTodoItem` 메서드는 호출 하는 데 사용 되는 `CreateTodoItem` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 다음 코드 예제와 같이 개체:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` 메서드를 만듭니다는 `Task` 를 실행 하는 `TodoService.EndCreateTodoItem` 메서드를 한 번의 `TodoService.BeginCreateTodoItem` 메서드가 완료 되 면와 `todoItem` 매개 변수가 전달 되는 데이터는 `BeginCreateTodoItem` 대리자는 지정하려면`TodoItem` 웹 서비스를 통해 생성 됩니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 작업의 실행에 대 한 기본 동작을 사용 하도록 지정 합니다.

웹 서비스에서 throw 한 `SoapException` 만들 수 없는 경우는 `TodoItem`는 응용 프로그램에 의해 처리 됩니다.

### <a name="updating-data"></a>데이터 업데이트

`TodoService.BeginEditTodoItem` 및 `TodoService.EndEditTodoItem` 메서드는 호출 하는 데 사용 되는 `EditTodoItem` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 다음 코드 예제와 같이 개체:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` 메서드를 만듭니다는 `Task` 를 실행 하는 `TodoService.EndEditTodoItem` 메서드를 한 번의 `TodoService.BeginCreateTodoItem` 메서드가 완료 되 면와 `todoItem` 매개 변수가 전달 되는 데이터는 `BeginEditTodoItem` 대리자는 지정하려면`TodoItem` 웹 서비스에 의해 업데이트 해야 합니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 작업의 실행에 대 한 기본 동작을 사용 하도록 지정 합니다.

웹 서비스에서 throw 한 `SoapException` 찾을 수 없거나 업데이트할 수 없는 경우는 `TodoItem`는 응용 프로그램에 의해 처리 됩니다.

### <a name="deleting-data"></a>데이터 삭제

`TodoService.BeginDeleteTodoItem` 및 `TodoService.EndDeleteTodoItem` 메서드는 호출 하는 데 사용 되는 `DeleteTodoItem` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 다음 코드 예제와 같이 개체:

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

`Task.Factory.FromAsync` 메서드를 만듭니다는 `Task` 를 실행 하는 `TodoService.EndDeleteTodoItem` 메서드를 한 번의 `TodoService.BeginDeleteTodoItem` 메서드가 완료 되 면와 `id` 매개 변수가 전달 되는 데이터는 `BeginDeleteTodoItem` 대리자는 지정하려면`TodoItem` 웹 서비스에 의해 삭제 됩니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 작업의 실행에 대 한 기본 동작을 사용 하도록 지정 합니다.

웹 서비스에서 throw 한 `SoapException` 를 찾거나 삭제할 수 없는 경우는 `TodoItem`는 응용 프로그램에 의해 처리 됩니다.

## <a name="summary"></a>요약

이 문서는 ASMX 웹 서비스 Xamarin.Forms 응용 프로그램에서 사용 하는 방법을 보여 줍니다. ASMX 웹 서비스 메시지를 보내는 HTTP를 통한 SOAP을 사용 하 여 구축 하는 기능을 제공 합니다. 소비자는 ASMX 서비스의 플랫폼, 개체 모델 또는 서비스를 구현 하는 데 사용 되는 프로그래밍 언어에 대 한 어떠한 정보도 알 필요가 없습니다. SOAP 메시지를 송수신 하는 방법을 이해 하려면 하기만 합니다.


## <a name="related-links"></a>관련 링크

- [TodoASMX (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
