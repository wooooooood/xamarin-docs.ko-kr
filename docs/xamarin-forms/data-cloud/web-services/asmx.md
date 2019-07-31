---
title: ASMX (ASP.NET Web Service) 사용
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 ASMX SOAP 서비스를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2019
ms.openlocfilehash: 124cffaf827ebeb8109f3cd4ac4dfd2701e43f9c
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656653"
---
# <a name="consume-an-aspnet-web-service-asmx"></a>ASMX (ASP.NET Web Service) 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)

_ASMX는 SOAP Simple Object Access Protocol ()를 사용 하 여 메시지를 전송 하는 웹 서비스를 구축 하는 기능을 제공 합니다. SOAP는 빌드하고 웹 서비스에 액세스 하기 위한 플랫폼 독립적인 및 언어 독립적 프로토콜입니다. ASMX 서비스의 소비자는 플랫폼, 개체 모델 또는 서비스를 구현 하는 데 사용 되는 프로그래밍 언어에 대 한 알 필요가 없습니다. 만 SOAP 메시지를 수신 하는 방법을 이해 해야 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 ASMX SOAP 서비스를 사용 하는 방법에 설명 합니다._

SOAP 메시지는 다음 요소를 포함 하는 XML 문서:

- 라는 루트 요소가 *봉투 (envelope)* SOAP 메시지가 XML 문서를 식별 하는 합니다.
- 선택적인 *헤더* 인증 데이터와 같은 응용 프로그램 관련 정보를 포함 하는 요소입니다. 경우는 *머리글* 요소가의 첫 번째 자식 요소 여야 합니다 합니다 *봉투 (envelope)* 요소입니다.
- 필수 *본문* 수신자를 위한 SOAP 메시지를 포함 하는 요소입니다.
- 선택적인 *오류* 오류 메시지를 나타내는 데 사용 되는 요소입니다. 경우는 *오류* 요소가, 자식 요소 이어야 합니다는 *본문* 요소입니다.

SOAP는 HTTP, SMTP, TCP 및 UDP를 비롯 한 여러 전송 프로토콜을 통해 작동할 수 있습니다. 그러나 HTTP를 통한 ASMX 서비스만 작동할 수 있습니다. Xamarin 플랫폼은 HTTP를 통해 표준 SOAP 1.1 구현을 지원 하 고 다양 한 표준 ASMX 서비스 구성에 대 한 지원이 포함 됩니다.

이 샘플에는 물리적 또는 에뮬레이트된 장치에서 실행 되는 모바일 응용 프로그램 및 데이터 가져오기, 추가, 편집 및 삭제를 위한 메서드를 제공 하는 ASMX 서비스가 포함 되어 있습니다. 모바일 응용 프로그램을 실행 하면 다음 스크린샷에 표시 된 것 처럼 로컬로 호스트 된 ASMX 서비스에 연결 됩니다.

![](asmx-images/portal.png "샘플 응용 프로그램")

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결 하므로 중요 한 정보가 실수로 유출 방지 적용 합니다. ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되므로 모든 연결이 ATS 보안 요구 사항이 적용 됩니다. 연결에서 이러한 요구를 충족 하지 않는, 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 ATS 옵트인 수 있습니다는 `HTTPS` 프로토콜 및 인터넷 리소스에 대 한 통신을 보호 합니다. 이 앱을 업데이트 하 여 수행할 수 있습니다 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="consume-the-web-service"></a>웹 서비스 사용

ASMX 서비스는 다음 작업을 제공 합니다.

|작업|설명|매개 변수|
|--- |--- |--- |
|GetTodoItems|할 일 항목의 목록 가져오기|
|CreateTodoItem|할 일 항목을 새로 만들려면|XML 직렬화 TodoItem|
|EditTodoItem|할 일 항목 업데이트|XML 직렬화 TodoItem|
|DeleteTodoItem|할 일 항목 삭제|XML 직렬화 TodoItem|

응용 프로그램에서 사용 되는 데이터 모델에 대 한 자세한 내용은 참조 하세요. [데이터 모델링은](~/xamarin-forms/data-cloud/web-services/introduction.md)합니다.

## <a name="create-the-todoservice-proxy"></a>TodoService 프록시 만들기

이라는 `TodoService`프록시 클래스는를 확장 `SoapHttpClientProtocol` 하 고 HTTP를 통해 ASMX 서비스와 통신 하기 위한 메서드를 제공 합니다. 프록시는 Visual Studio 2019 또는 Visual Studio 2017의 각 플랫폼별 프로젝트에 웹 참조를 추가 하 여 생성 됩니다. 웹 참조는 서비스의 WSDL (웹 서비스 기술 언어) 문서에 정의 된 각 작업에 대 한 메서드 및 이벤트를 생성 합니다.

예를 들어 `GetTodoItems` 서비스 작업을 수행 하면 프록시의 `GetTodoItemsAsync` 메서드와 `GetTodoItemsCompleted` 이벤트가 생성 됩니다. 생성 된 메서드는 void 반환 형식을 가지 며 부모 `GetTodoItems` `SoapHttpClientProtocol` 클래스에서 작업을 호출 합니다. 호출 된 메서드가 서비스에서 응답을 받으면 `GetTodoItemsCompleted` 이벤트를 발생 시키고 이벤트의 `Result` 속성 내에서 응답 데이터를 제공 합니다.

## <a name="create-the-isoapservice-implementation"></a>ISoapService 구현 만들기

공유 플랫폼 간 프로젝트에서 서비스를 사용할 수 있도록 하기 위해이 샘플에서는 `ISoapService` [의 C#작업 비동기 프로그래밍 모델 ](/dotnet/csharp/programming-guide/concepts/async/)을 따르는 인터페이스를 정의 합니다. 각 플랫폼은 플랫폼 `ISoapService` 특정 프록시를 노출 하기 위해를 구현 합니다. 이 샘플에서는 `TaskCompletionSource` 개체를 사용 하 여 프록시를 태스크 비동기 인터페이스로 노출 합니다. 사용 `TaskCompletionSource` 에 대 한 자세한 내용은 아래 섹션의 각 작업 유형에 대 한 구현에서 찾을 수 있습니다.

샘플 `SoapService`은 다음과 같습니다.

1. 클래스 수준 `TodoService` 인스턴스로를 인스턴스화합니다.
1. 개체를 저장 `TodoItem` 하기 `Items` 위해 라는 컬렉션을 만듭니다.
1. 의 선택적 `Url` 속성에 대 한 사용자 지정 끝점을 지정 합니다.`TodoService`

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

이 메서드는 새 `ASMService.TodoItem` 인스턴스를 만들고 각 속성을 `TodoItem` 인스턴스의 동일한 속성으로 설정 합니다.

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

이 메서드는 프록시 생성 `TodoItem` 형식에서 데이터를 검색 하 고 새로 만든 `TodoItem` 인스턴스에 설정 합니다.

### <a name="retrieve-data"></a>데이터 검색

인터페이스 `ISoapService` `Task` 에서는 메서드가항목컬렉션과함께를반환해야합니다.`RefreshDataAsync` 그러나이 메서드 `TodoService.GetTodoItemsAsync` 는 void를 반환 합니다. 인터페이스 패턴을 충족 하려면를 호출 `GetTodoItemsAsync`하 고 `GetTodoItemsCompleted` 이벤트가 발생할 때까지 기다린 후 컬렉션을 채워야 합니다. 이렇게 하면 유효한 컬렉션을 UI로 반환할 수 있습니다.

`TaskCompletionSource`아래 예제에서는 새를 만들고, `RefreshDataAsync` 메서드에서 비동기 호출을 시작 하 고,에서 제공 하 `Task` 는을 `TaskCompletionSource`기다립니다 합니다. 이벤트 처리기가 호출 되 면 컬렉션을 `Items` 채우고가 `TaskCompletionSource`업데이트 됩니다. `TodoService_GetTodoItemsCompleted`

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

데이터를 만들거나 편집 하는 `ISoapService.SaveTodoItemAsync` 경우 메서드를 구현 해야 합니다. 이 메서드는 `TodoItem` 가 새 항목 인지 업데이트 된 항목 인지 검색 하 고 `todoService` 개체에 대해 적절 한 메서드를 호출 합니다. 및 이벤트 처리기도 구현 하 여 `todoService` 가 ASMX 서비스에서 응답을 받은 시기를 알 수 있도록 해야 합니다. 이러한 처리기는 동일한 작업을 수행 하기 때문에 단일 처리기로 결합 될 수 있습니다. `EditTodoItemCompleted` `CreateTodoItemCompleted` 다음 예제에서는 인터페이스 및 이벤트 처리기 구현 뿐만 `TaskCompletionSource` 아니라 비동기식으로 작동 하는 데 사용 되는 개체를 보여 줍니다.

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

데이터를 삭제 하려면 비슷한 구현이 필요 합니다. 을 정의 `ISoapService.DeleteTodoItemAsync` 하 고, 이벤트 처리기 및 메서드를 구현 합니다. `TaskCompletionSource`

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
