---
title: ASMX(ASP.NET 웹 서비스) 사용
description: 이 문서에서는 응용 프로그램에서 ASMX SOAP 서비스를 사용 하는 방법을 보여 줍니다 Xamarin.Forms .
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1f7a0d04d1e7b6abc9931c05c0e46ef49f8ba09c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138465"
---
# <a name="consume-an-aspnet-web-service-asmx"></a>ASMX(ASP.NET 웹 서비스) 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)

_ASMX는 SOAP (Simple Object Access Protocol)를 사용 하 여 메시지를 보내는 웹 서비스를 빌드하는 기능을 제공 합니다. SOAP는 웹 서비스를 빌드하고 액세스 하기 위한 플랫폼 독립적이 고 언어에 독립적인 프로토콜입니다. ASMX 서비스 소비자는 서비스를 구현 하는 데 사용 되는 플랫폼, 개체 모델 또는 프로그래밍 언어에 대해 알 필요가 없습니다. SOAP 메시지를 송수신 하는 방법을 이해 해야 합니다. 이 문서에서는 응용 프로그램에서 ASMX SOAP 서비스를 사용 하는 방법을 보여 줍니다 Xamarin.Forms ._

SOAP 메시지는 다음 요소를 포함 하는 XML 문서입니다.

- XML 문서를 SOAP 메시지로 식별 하는 *Envelope* 이라는 루트 요소입니다.
- 인증 데이터와 같은 응용 프로그램 관련 정보를 포함 하는 선택적 *헤더* 요소입니다. *Header* 요소가 있으면 *Envelope* 요소의 첫 번째 자식 요소 여야 합니다.
- 받는 사람에 대해 의도 된 SOAP 메시지를 포함 하는 필수 *본문* 요소입니다.
- 오류 메시지를 나타내는 데 사용 되는 선택적 *오류* 요소입니다. *Fault* 요소가 있으면 *Body* 요소의 자식 요소 여야 합니다.

SOAP는 HTTP, SMTP, TCP 및 UDP를 비롯 한 여러 전송 프로토콜을 통해 작동할 수 있습니다. 그러나 ASMX 서비스는 HTTP를 통해서만 작동할 수 있습니다. Xamarin 플랫폼은 HTTP를 통한 표준 SOAP 1.1 구현을 지원 하며, 여기에는 많은 표준 ASMX 서비스 구성에 대 한 지원이 포함 됩니다.

이 샘플에는 물리적 또는 에뮬레이트된 장치에서 실행 되는 모바일 응용 프로그램 및 데이터 가져오기, 추가, 편집 및 삭제를 위한 메서드를 제공 하는 ASMX 서비스가 포함 되어 있습니다. 모바일 응용 프로그램을 실행 하면 다음 스크린샷에 표시 된 것 처럼 로컬로 호스트 된 ASMX 서비스에 연결 됩니다.

![](asmx-images/portal.png "Sample Application")

> [!NOTE]
> IOS 9 이상에서 ATS (App Transport Security)는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간 보안 연결을 적용 하 여 중요 한 정보가 실수로 공개 되는 것을 방지 합니다. ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되어 있으므로 모든 연결에 ATS 보안 요구 사항이 적용 됩니다. 연결이 이러한 요구 사항을 충족 하지 않으면 예외와 함께 실패 합니다.
> 프로토콜을 사용 하 여 `HTTPS` 인터넷 리소스에 대 한 보안 통신을 할 수 없는 경우 ATS를 옵트아웃 (opt out) 할 수 있습니다. 이는 앱의 **info.plist** 파일을 업데이트 하 여 수행할 수 있습니다. 자세한 내용은 [앱 전송 보안](~/ios/app-fundamentals/ats.md)을 참조 하세요.

## <a name="consume-the-web-service"></a>웹 서비스 사용

ASMX 서비스는 다음과 같은 작업을 제공 합니다.

|작업(Operation)|Description|매개 변수|
|--- |--- |--- |
|GetTodoItems|할 일 항목의 목록 가져오기|
|CreateTodoItem|새 할 일 항목 만들기|XML 직렬화 된 TodoItem|
|EditTodoItem|할 일 항목 업데이트|XML 직렬화 된 TodoItem|
|DeleteTodoItem|할 일 항목 삭제|XML 직렬화 된 TodoItem|

응용 프로그램에서 사용 되는 데이터 모델에 대 한 자세한 내용은 [데이터 모델링](~/xamarin-forms/data-cloud/web-services/introduction.md)을 참조 하세요.

## <a name="create-the-todoservice-proxy"></a>TodoService 프록시 만들기

이라는 프록시 클래스는 `TodoService` 를 확장 `SoapHttpClientProtocol` 하 고 HTTP를 통해 ASMX 서비스와 통신 하기 위한 메서드를 제공 합니다. 프록시는 Visual Studio 2019 또는 Visual Studio 2017의 각 플랫폼별 프로젝트에 웹 참조를 추가 하 여 생성 됩니다. 웹 참조는 서비스의 WSDL (웹 서비스 기술 언어) 문서에 정의 된 각 작업에 대 한 메서드 및 이벤트를 생성 합니다.

예를 들어 `GetTodoItems` 서비스 작업을 수행 하면 `GetTodoItemsAsync` 프록시의 메서드와 이벤트가 생성 `GetTodoItemsCompleted` 됩니다. 생성 된 메서드는 void 반환 형식을 가지 며 `GetTodoItems` 부모 클래스에서 작업을 호출 합니다 `SoapHttpClientProtocol` . 호출 된 메서드가 서비스에서 응답을 받으면 이벤트를 발생 `GetTodoItemsCompleted` 시키고 이벤트의 속성 내에서 응답 데이터를 제공 합니다 `Result` .

## <a name="create-the-isoapservice-implementation"></a>ISoapService 구현 만들기

공유 플랫폼 간 프로젝트에서 서비스를 사용할 수 있도록 하기 위해이 샘플에서는 `ISoapService` [c #의 작업 비동기 프로그래밍 모델](/dotnet/csharp/programming-guide/concepts/async/)을 따르는 인터페이스를 정의 합니다. 각 플랫폼은 `ISoapService` 플랫폼 특정 프록시를 노출 하기 위해를 구현 합니다. 이 샘플에서는 `TaskCompletionSource` 개체를 사용 하 여 프록시를 태스크 비동기 인터페이스로 노출 합니다. 사용에 대 한 자세한 내용은 `TaskCompletionSource` 아래 섹션의 각 작업 유형에 대 한 구현에서 찾을 수 있습니다.

샘플은 `SoapService` 다음과 같습니다.

1. `TodoService`클래스 수준 인스턴스로를 인스턴스화합니다.
1. `Items`개체를 저장 하기 위해 라는 컬렉션을 만듭니다. `TodoItem`
1. 의 선택적 속성에 대 한 사용자 지정 끝점을 지정 합니다. `Url``TodoService`

```csharp
public class SoapService : ISoapService
{
    ASMXService.TodoService todoService;
    public List<TodoItem> Items { get; private set; } = new List<TodoItem>();

    public SoapService ()
    {
        todoService = new ASMXService.TodoService ();
        todoService.Url = Constants.SoapUrl;
        ...
    }
}
```

### <a name="create-data-transfer-objects"></a>데이터 전송 개체 만들기

샘플 응용 프로그램은 클래스를 사용 하 여 `TodoItem` 데이터를 모델링 합니다. `TodoItem`웹 서비스에 항목을 저장 하려면 먼저 프록시 생성 형식으로 변환 해야 합니다 `TodoItem` . 이 `ToASMXServiceTodoItem` 작업은 다음 코드 예제와 같이 메서드로 수행 됩니다.

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

이 메서드는 새 `ASMService.TodoItem` 인스턴스를 만들고 각 속성을 인스턴스의 동일한 속성으로 설정 합니다 `TodoItem` .

마찬가지로 웹 서비스에서 데이터를 검색 하는 경우 프록시 생성 형식에서 인스턴스로 변환 되어야 합니다 `TodoItem` `TodoItem` . 이 `FromASMXServiceTodoItem` 작업은 다음 코드 예제와 같이 메서드를 사용 하 여 수행 됩니다.

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

이 메서드는 프록시 생성 형식에서 데이터를 검색 `TodoItem` 하 고 새로 만든 인스턴스에 설정 합니다 `TodoItem` .

### <a name="retrieve-data"></a>데이터 검색

`ISoapService`인터페이스에서는 `RefreshDataAsync` 메서드가 항목 컬렉션과 함께를 반환 해야 합니다 `Task` . 그러나이 `TodoService.GetTodoItemsAsync` 메서드는 void를 반환 합니다. 인터페이스 패턴을 충족 하려면를 호출 하 `GetTodoItemsAsync` `GetTodoItemsCompleted` 고 이벤트가 발생할 때까지 기다린 후 컬렉션을 채워야 합니다. 이렇게 하면 유효한 컬렉션을 UI로 반환할 수 있습니다.

아래 예제에서는 새를 만들고 `TaskCompletionSource` , 메서드에서 비동기 호출을 시작 하 `RefreshDataAsync` 고,에서 제공 하는을 기다립니다 합니다 `Task` `TaskCompletionSource` . `TodoService_GetTodoItemsCompleted`이벤트 처리기가 호출 되 면 컬렉션을 채우고 `Items` 가 업데이트 됩니다 `TaskCompletionSource` .

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> getRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.GetTodoItemsCompleted += TodoService_GetTodoItemsCompleted;
    }

    public async Task<List<TodoItem>> RefreshDataAsync()
    {
        getRequestComplete = new TaskCompletionSource<bool>();
        todoService.GetTodoItemsAsync();
        await getRequestComplete.Task;
        return Items;
    }

    private void TodoService_GetTodoItemsCompleted(object sender, ASMXService.GetTodoItemsCompletedEventArgs e)
    {
        try
        {
            getRequestComplete = getRequestComplete ?? new TaskCompletionSource<bool>();

            Items = new List<TodoItem>();
            foreach (var item in e.Result)
            {
                Items.Add(FromASMXServiceTodoItem(item));
            }
            getRequestComplete?.TrySetResult(true);
        }
        catch (Exception ex)
        {
            Debug.WriteLine(@"\t\tERROR {0}", ex.Message);
        }
    }

    ...
}
```

자세한 내용은 [비동기 프로그래밍 모델](/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm) 및 [TPL 및 기존 .NET Framework 비동기 프로그래밍](/dotnet/standard/parallel-programming/tpl-and-traditional-async-programming)을 참조 하세요.

### <a name="create-or-edit-data"></a>데이터 만들기 또는 편집

데이터를 만들거나 편집 하는 경우 메서드를 구현 해야 합니다 `ISoapService.SaveTodoItemAsync` . 이 메서드는 `TodoItem` 가 새 항목 인지 업데이트 된 항목 인지 검색 하 고 개체에 대해 적절 한 메서드를 호출 합니다 `todoService` . `CreateTodoItemCompleted`및 `EditTodoItemCompleted` 이벤트 처리기도 구현 하 여 `todoService` 가 ASMX 서비스에서 응답을 받은 시기를 알 수 있도록 해야 합니다. 이러한 처리기는 동일한 작업을 수행 하기 때문에 단일 처리기로 결합 될 수 있습니다. 다음 예제에서는 인터페이스 및 이벤트 처리기 구현 뿐만 아니라 `TaskCompletionSource` 비동기식으로 작동 하는 데 사용 되는 개체를 보여 줍니다.

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> saveRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.CreateTodoItemCompleted += TodoService_SaveTodoItemCompleted;
        todoService.EditTodoItemCompleted += TodoService_SaveTodoItemCompleted;
    }

    public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
    {
        try
        {
            var todoItem = ToASMXServiceTodoItem(item);
            saveRequestComplete = new TaskCompletionSource<bool>();
            if (isNewItem)
            {
                todoService.CreateTodoItemAsync(todoItem);
            }
            else
            {
                todoService.EditTodoItemAsync(todoItem);
            }
            await saveRequestComplete.Task;
        }
        catch (SoapException se)
        {
            Debug.WriteLine("\t\t{0}", se.Message);
        }
        catch (Exception ex)
        {
            Debug.WriteLine("\t\tERROR {0}", ex.Message);
        }
    }

    private void TodoService_SaveTodoItemCompleted(object sender, System.ComponentModel.AsyncCompletedEventArgs e)
    {
        saveRequestComplete?.TrySetResult(true);
    }

    ...
}
```

### <a name="delete-data"></a>데이터 삭제

데이터를 삭제 하려면 비슷한 구현이 필요 합니다. 을 정의 `TaskCompletionSource` 하 고, 이벤트 처리기 및 메서드를 구현 합니다 `ISoapService.DeleteTodoItemAsync` .

```csharp
public class SoapService : ISoapService
{
    TaskCompletionSource<bool> deleteRequestComplete = null;
    ...

    public SoapService()
    {
        ...
        todoService.DeleteTodoItemCompleted += TodoService_DeleteTodoItemCompleted;
    }

    public async Task DeleteTodoItemAsync (string id)
    {
        try
        {
            deleteRequestComplete = new TaskCompletionSource<bool>();
            todoService.DeleteTodoItemAsync(id);
            await deleteRequestComplete.Task;
        }
        catch (SoapException se)
        {
            Debug.WriteLine("\t\t{0}", se.Message);
        }
        catch (Exception ex)
        {
            Debug.WriteLine("\t\tERROR {0}", ex.Message);
        }
    }

    private void TodoService_DeleteTodoItemCompleted(object sender, System.ComponentModel.AsyncCompletedEventArgs e)
    {
        deleteRequestComplete?.TrySetResult(true);
    }

    ...
}
```

## <a name="test-the-web-service"></a>웹 서비스 테스트

로컬로 호스팅된 서비스를 사용 하 여 물리적 장치 또는 에뮬레이트된 장치를 테스트 하려면 사용자 지정 IIS 구성, 끝점 주소 및 방화벽 규칙이 준비 되어 있어야 합니다. 테스트 환경을 설정 하는 방법에 대 한 자세한 내용은 IIS Express에 대 한 [원격 액세스 구성](wcf.md#configure-remote-access-to-iis-express)을 참조 하세요. WCF와 ASMX 테스트의 유일한 차이점은 TodoService의 포트 번호입니다.

## <a name="related-links"></a>관련 링크

- [TodoASMX (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)
- [IAsyncResult](https://docs.microsoft.com/dotnet/api/system.iasyncresult)
