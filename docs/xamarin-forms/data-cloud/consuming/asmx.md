---
title: ASP.NET 웹 서비스 (ASMX) 사용
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 ASMX SOAP 서비스를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 1fa2fee36619a2a443d84b861b2473a1f616ed0e
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055143"
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>ASP.NET 웹 서비스 (ASMX) 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)

_ASMX는 SOAP Simple Object Access Protocol ()를 사용 하 여 메시지를 전송 하는 웹 서비스를 구축 하는 기능을 제공 합니다. SOAP는 빌드하고 웹 서비스에 액세스 하기 위한 플랫폼 독립적인 및 언어 독립적 프로토콜입니다. ASMX 서비스의 소비자는 플랫폼, 개체 모델 또는 서비스를 구현 하는 데 사용 되는 프로그래밍 언어에 대 한 알 필요가 없습니다. 만 SOAP 메시지를 수신 하는 방법을 이해 해야 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 ASMX SOAP 서비스를 사용 하는 방법에 설명 합니다._

SOAP 메시지는 다음 요소를 포함 하는 XML 문서:

- 라는 루트 요소가 *봉투 (envelope)* SOAP 메시지가 XML 문서를 식별 하는 합니다.
- 선택적인 *헤더* 인증 데이터와 같은 응용 프로그램 관련 정보를 포함 하는 요소입니다. 경우는 *머리글* 요소가의 첫 번째 자식 요소 여야 합니다 합니다 *봉투 (envelope)* 요소입니다.
- 필수 *본문* 수신자를 위한 SOAP 메시지를 포함 하는 요소입니다.
- 선택적인 *오류* 오류 메시지를 나타내는 데 사용 되는 요소입니다. 경우는 *오류* 요소가, 자식 요소 이어야 합니다는 *본문* 요소입니다.

SOAP는 HTTP, SMTP, TCP 및 UDP를 비롯 한 여러 전송 프로토콜을 통해 작동할 수 있습니다. 그러나 HTTP를 통한 ASMX 서비스만 작동할 수 있습니다. Xamarin 플랫폼은 HTTP를 통해 표준 SOAP 1.1 구현을 지원 하 고 다양 한 표준 ASMX 서비스 구성에 대 한 지원이 포함 됩니다.

ASMX 서비스를 설정 하는 방법은 샘플 응용 프로그램을 함께 제공 되는 추가 정보 파일에서 찾을 수 있습니다. 그러나 샘플 응용 프로그램을 실행 하는 서비스에 연결 ASMX Xamarin 호스 데이터에 대 한 읽기 전용 액세스를 제공 하는 다음 스크린샷에 표시 된 대로:

![](asmx-images/portal.png "샘플 응용 프로그램")

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결 하므로 중요 한 정보가 실수로 유출 방지 적용 합니다. ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되므로 모든 연결이 ATS 보안 요구 사항이 적용 됩니다. 연결에서 이러한 요구를 충족 하지 않는, 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 ATS 옵트인 수 있습니다는 `HTTPS` 프로토콜 및 인터넷 리소스에 대 한 통신을 보호 합니다. 이 앱을 업데이트 하 여 수행할 수 있습니다 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="consuming-the-web-service"></a>웹 서비스 사용

ASMX 서비스는 다음 작업을 제공 합니다.

|작업|설명|매개 변수|
|--- |--- |--- |
|GetTodoItems|할 일 항목의 목록 가져오기|
|CreateTodoItem|할 일 항목을 새로 만들려면|XML 직렬화 TodoItem|
|EditTodoItem|할 일 항목 업데이트|XML 직렬화 TodoItem|
|DeleteTodoItem|할 일 항목 삭제|XML 직렬화 TodoItem|

응용 프로그램에서 사용 되는 데이터 모델에 대 한 자세한 내용은 참조 하세요. [데이터 모델링은](~/xamarin-forms/data-cloud/walkthrough.md)합니다.

> [!NOTE]
> 샘플 응용 프로그램 웹 서비스에 대 한 읽기 전용 액세스를 제공 하는 Xamarin에서 호스팅되는 ASMX 서비스를 사용 합니다. 따라서 만들기, 업데이트 및 데이터를 삭제 하는 작업은 응용 프로그램에서 사용 된 데이터를 변경 하지 않습니다. 그러나 ASMX 서비스의 호스팅 가능한 버전은 영어로 합니다 **TodoASMXService** 함께 제공 되는 샘플 응용 프로그램 폴더입니다. 이 호스팅 가능 버전 전체 ASMX 서비스 허용의 만들기, 업데이트, 읽기 및 삭제 데이터에 대 한 액세스.

A *프록시* 응용 프로그램 서비스에 연결을 허용 하는 ASMX 서비스를 사용 하도록 생성 해야 합니다. 프록시는 메서드 및 연결 된 서비스 구성을 정의 하는 서비스 메타 데이터를 사용 하 여 생성 됩니다. 이 메타 데이터는 웹 서비스에 의해 생성 되는 웹 서비스 설명 언어 (WSDL) 문서의 형태로 노출 됩니다. 프록시는 플랫폼별 프로젝트에 웹 서비스에 대 한 웹 참조를 추가 하 여 빌드됩니다.

생성 된 프록시 클래스는 비동기 프로그래밍 모델 (APM) 디자인 패턴을 사용 하는 웹 서비스에 대 한 메서드를 제공 합니다. 이 패턴에서는 비동기 작업 이라는 두 가지 방법으로 구현 됩니다 *BeginOperationName* 하 고 *EndOperationName*를 시작 하며 비동기 작업을 종료 합니다.

합니다 *BeginOperationName* 메서드는 비동기 작업을 시작 하 고 구현 하는 개체를 반환 합니다.는 `IAsyncResult` 인터페이스입니다. 호출한 후 *BeginOperationName*, 응용 프로그램 수 동안 명령을 계속 실행할 때 호출 스레드에서 스레드 풀 스레드에서 비동기 작업을 수행 합니다.

호출할 때마다 *BeginOperationName*, 응용 프로그램도 호출 해야 *EndOperationName* 작업의 결과를 가져옵니다. 반환 값 *EndOperationName* 동기 웹 서비스 메서드의 반환 형식과 동일 합니다. 예를 들어 합니다 `EndGetTodoItems` 의 컬렉션을 반환 하는 메서드 `TodoItem` 인스턴스. 합니다 *EndOperationName* 메서드에 포함 됩니다는 `IAsyncResult` 에 대 한 해당 호출에서 반환한 인스턴스를 설정 해야 하는 매개 변수를 *BeginOperationName* 메서드.

작업 병렬 라이브러리 (TPL)과 같은 비동기 작업을 캡슐화 하 여 APM begin/end 메서드 쌍을 사용 하는 프로세스를 간소화할 수 `Task` 개체입니다. 여러 오버 로드에서 제공 하는이 캡슐화는 `TaskFactory.FromAsync` 메서드.

APM 대 한 자세한 내용은 참조 하세요 [비동기 프로그래밍 모델](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) 하 고 [TPL 및 일반적인.NET Framework 비동기 프로그래밍](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) MSDN에서.

### <a name="creating-the-todoservice-object"></a>TodoService 개체 만들기

생성된 된 프록시 클래스를 제공 합니다 `TodoService` HTTP를 통한 ASMX 서비스와 통신 하는 데 사용 되는 클래스입니다. URI에서 비동기 작업 서비스 인스턴스를 식별 한 대로 웹 서비스 메서드를 호출 하는 것에 대 한 기능을 제공 합니다. 비동기 작업에 대 한 자세한 내용은 참조 하세요. [비동기 지원 개요](~/cross-platform/platform/async.md)합니다.

`TodoService` 클래스 수준에서 인스턴스는 선언으로 다음 코드 예제에 표시 된 대로 응용 프로그램 해야 ASMX 서비스를 사용 하는 개체에 대 한 상주 합니다.

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

`TodoService` 생성자는 ASMX 서비스 인스턴스의 URL을 지정 하는 선택적 문자열 매개 변수를 사용 합니다. 이 통해 게시 된 인스턴스가 여러 개는 ASMX 서비스의 다른 인스턴스에 연결 하려면 응용 프로그램.

### <a name="creating-data-transfer-objects"></a>데이터 전송 개체 만들기

샘플 응용 프로그램을 `TodoItem` 모델 데이터에는 클래스입니다. 저장 하는 `TodoItem` 생성 된 프록시를 먼저 변환 해야 하는 웹 서비스에 대 한 항목 `TodoItem` 형식입니다. 이 `ToASMXServiceTodoItem` 메서드를 다음 코드 예제 에서처럼:

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

이 메서드는 단순히 새 만듭니다 `ASMService.TodoItem` 인스턴스로 이동한 각 속성에서 동일한 속성을 설정 합니다 `TodoItem` 인스턴스.

마찬가지로 데이터 웹 서비스에서 검색 되 면 변환 해야 생성 된 프록시에서 `TodoItem` 에 입력을 `TodoItem` 인스턴스. 사용 하 여 이렇게는 `FromASMXServiceTodoItem` 메서드를 다음 코드 예제 에서처럼:

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

이 메서드는 단순히 데이터에서 생성 된 프록시 검색 `TodoItem` 입력 하 고 새로 만든에서 설정 `TodoItem` 인스턴스.

### <a name="retrieving-data"></a>데이터 검색

합니다 `TodoService.BeginGetTodoItems` 하 고 `TodoService.EndGetTodoItems` 메서드를 호출 하는 데 사용 됩니다는 `GetTodoItems` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 와 다음 코드 예제와 같이 개체:

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

`Task.Factory.FromAsync` 메서드를 만듭니다를 `Task` 실행 하는 합니다 `TodoService.EndGetTodoItems` 메서드를 한 번를 `TodoService.BeginGetTodoItems` 메서드가 완료 되 면 사용 하 여를 `null` 데이터가 없는에 전달 되 고 있는지를 나타내는 매개 변수를 `BeginGetTodoItems` 대리자. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 태스크 실행에 대 한 기본 동작을 사용 해야 함을 지정 합니다.

합니다 `TodoService.EndGetTodoItems` 메서드는 배열을 반환 `ASMXService.TodoItem` 변환 됩니다 하는 인스턴스를 `List` 의 `TodoItem` 표시에 대 한 인스턴스.

### <a name="creating-data"></a>데이터 만들기

합니다 `TodoService.BeginCreateTodoItem` 하 고 `TodoService.EndCreateTodoItem` 메서드를 호출 하는 데 사용 됩니다는 `CreateTodoItem` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 와 다음 코드 예제와 같이 개체:

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

`Task.Factory.FromAsync` 메서드를 만듭니다를 `Task` 실행 하는 합니다 `TodoService.EndCreateTodoItem` 메서드를 한 번를 `TodoService.BeginCreateTodoItem` 메서드가 완료 되 면 사용 하 여는 `todoItem` 매개 변수를 전달 되는 데이터는 `BeginCreateTodoItem` 대리자를 지정 합니다 `TodoItem` 웹 서비스를 통해 생성 됩니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 태스크 실행에 대 한 기본 동작을 사용 해야 함을 지정 합니다.

웹 서비스 throw를 `SoapException` 만들 수 없는 경우는 `TodoItem`, 응용 프로그램에서 처리 되는 합니다.

### <a name="updating-data"></a>데이터 업데이트

합니다 `TodoService.BeginEditTodoItem` 하 고 `TodoService.EndEditTodoItem` 메서드를 호출 하는 데 사용 됩니다는 `EditTodoItem` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 와 다음 코드 예제와 같이 개체:

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

`Task.Factory.FromAsync` 메서드를 만듭니다를 `Task` 실행 하는 합니다 `TodoService.EndEditTodoItem` 메서드를 한 번를 `TodoService.BeginCreateTodoItem` 메서드가 완료 되 면 사용 하 여는 `todoItem` 매개 변수를 전달 되는 데이터는 `BeginEditTodoItem` 대리자를 지정 합니다 `TodoItem` 웹 서비스에서 업데이트 해야 합니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 태스크 실행에 대 한 기본 동작을 사용 해야 함을 지정 합니다.

웹 서비스 throw를 `SoapException` 찾거나 업데이트 하지 못한 경우는 `TodoItem`, 응용 프로그램에서 처리 되는 합니다.

### <a name="deleting-data"></a>데이터 삭제

합니다 `TodoService.BeginDeleteTodoItem` 하 고 `TodoService.EndDeleteTodoItem` 메서드를 호출 하는 데 사용 됩니다는 `DeleteTodoItem` 웹 서비스에서 제공 하는 작업입니다. 이러한 비동기 메서드에 캡슐화 되는 `Task` 와 다음 코드 예제와 같이 개체:

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

`Task.Factory.FromAsync` 메서드를 만듭니다를 `Task` 실행 하는 합니다 `TodoService.EndDeleteTodoItem` 메서드를 한 번를 `TodoService.BeginDeleteTodoItem` 메서드가 완료 되 면 사용 하 여는 `id` 매개 변수를 전달 되는 데이터는 `BeginDeleteTodoItem` 대리자를 지정 합니다 `TodoItem` 웹 서비스에 의해 삭제 됩니다. 마지막으로, 값은 `TaskCreationOptions` 열거형을 만들고 태스크 실행에 대 한 기본 동작을 사용 해야 함을 지정 합니다.

웹 서비스 throw를 `SoapException` 찾거나 삭제 하지 못한 경우는 `TodoItem`, 응용 프로그램에서 처리 되는 합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms 응용 프로그램에서 ASMX 웹 서비스를 사용 하는 방법을 보여 줍니다. ASMX는 SOAP를 사용 하 여 HTTP를 통해 메시지를 전송 하는 웹 서비스를 구축 하는 기능을 제공 합니다. ASMX 서비스의 소비자는 플랫폼, 개체 모델 또는 서비스를 구현 하는 데 사용 되는 프로그래밍 언어에 대 한 알 필요가 없습니다. 만 SOAP 메시지를 수신 하는 방법을 이해 해야 합니다.


## <a name="related-links"></a>관련 링크

- [TodoASMX (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
