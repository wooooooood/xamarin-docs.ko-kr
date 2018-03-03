---
title: "Azure 모바일 앱과 오프 라인 데이터 동기화"
description: "오프 라인 동기화에는 사용자가 모바일 응용 프로그램, 보기, 추가, 또는 데이터 수정, 상호 작용할 수 있도록 있더라도 네트워크에 연결 하지 않습니다. 변경 내용이 로컬 데이터베이스에 저장 됩니다 및 장치가 온라인 상태 이면 되 면 변경 내용이 Azure 모바일 앱 인스턴스와 동기화 할 수 있습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에 오프 라인 동기화 기능을 추가 하는 방법에 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: DBB343B0-2709-4C20-A669-5522B9956D9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2017
ms.openlocfilehash: b7756c63901d3b4fbfea70587b3fdf8e5cf9df72
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="synchronizing-offline-data-with-azure-mobile-apps"></a>Azure 모바일 앱과 오프 라인 데이터 동기화

_오프 라인 동기화에는 사용자가 모바일 응용 프로그램, 보기, 추가, 또는 데이터 수정, 상호 작용할 수 있도록 있더라도 네트워크에 연결 하지 않습니다. 변경 내용이 로컬 데이터베이스에 저장 됩니다 및 장치가 온라인 상태 이면 되 면 변경 내용이 Azure 모바일 앱 인스턴스와 동기화 할 수 있습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에 오프 라인 동기화 기능을 추가 하는 방법에 설명 합니다._

## <a name="overview"></a>개요

[Azure 모바일 클라이언트 SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) 제공는 `IMobileServiceTable` Azure 모바일 앱 인스턴스에 저장 된 테이블에서 수행할 수 있는 작업을 나타내는 인터페이스입니다. 이러한 작업은 Azure 모바일 앱 인스턴스에 직접 연결 하 고 모바일 장치에 네트워크 연결이 없는 경우 실패 합니다.

오프 라인 동기화를 지원 하기 위해 Azure 모바일 클라이언트 SDK 지원 *테이블 동기화*를 통해 제공 되는 `IMobileServiceSyncTable` 인터페이스입니다. 이 인터페이스는 동일한 만들기, 읽기, 업데이트, 삭제 (CRUD) 작업으로 제공는 `IMobileServiceTable` 인터페이스, 하지만 작업에서 읽거나 쓸를 로컬 저장소입니다. 로컬 저장소에 대 한 호출 될 때까지 Azure 모바일 앱 인스턴스에서 새 데이터로 채워져 있지 *끌어오기* 데이터입니다. 호출할 때까지 데이터는 Azure 모바일 앱 인스턴스에 전송 되지 않고는 마찬가지로, *푸시* 로컬 변경 내용이 있습니다.

오프 라인 동기화에는 Azure 모바일 앱 인스턴스 및 사용자 지정 충돌 해결에 로컬 저장소에서와 동일한 레코드가 변경 될 때 충돌을 검색 하는 것에 대 한 지원이 포함 되어 있습니다. 충돌 로컬 저장소 또는 Azure 모바일 앱 인스턴스에서 처리할 수 있습니다.

오프 라인 동기화에 대 한 자세한 내용은 참조 [Azure 모바일 앱에서 데이터 동기화를 오프 라인](/azure/app-service-mobile/app-service-mobile-offline-data-sync/) 및 [Xamarin.Forms 모바일 앱에 대 한 오프 라인 동기화 사용](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-offline-data/)합니다.

## <a name="setup"></a>설정

Xamarin.Forms 응용 프로그램과 Azure 모바일 앱 인스턴스 간에 오프 라인 동기화를 통합 하기 위한 프로세스는 다음과 같습니다.

1. Azure 모바일 앱 인스턴스를 만듭니다. 자세한 내용은 참조 [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)합니다.
1. 추가 [Microsoft.Azure.Mobile.Client.SQLiteStore](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/) Xamarin.Forms 솔루션의 모든 프로젝트에 NuGet 패키지 합니다.
1. (선택 사항) Azure 모바일 앱 인스턴스 및 Xamarin.Forms 응용 프로그램에서 인증을 활성화 합니다. 자세한 내용은 참조 [Azure 모바일 앱과 사용자가 인증](~/xamarin-forms/data-cloud/authentication/azure.md)합니다.

다음 섹션 Microsoft.Azure.Mobile.Client.SQLiteStore NuGet 패키지를 사용 하는 유니버설 Windows 플랫폼 (UWP) 프로젝트를 구성 하기 위한 추가 설정 지침을 제공 합니다. 추가 설정 없이 iOS 및 Android Microsoft.Azure.Mobile.Client.SQLiteStore NuGet 패키지를 사용 해야 합니다.

### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

SQLite는 유니버설 Windows 플랫폼 (UWP)에 사용 하려면 다음이 단계를 수행 합니다.

1. 설치는 [유니버설 Windows 플랫폼에 대 한 SQLite](http://sqlite.org/2016/sqlite-uwp-3120200.vsix) 개발 환경에서 Visual Studio 확장 합니다.
1. Visual Studio에서 UWP 프로젝트에서 마우스 오른쪽 단추로 클릭 **참조 > 참조 추가**로 이동 **확장** 추가 **유니버설 Windows 플랫폼에 대 한 SQLite** 및  **유니버설 Windows 플랫폼 앱 용 visual c + + 2015 런타임** UWP 프로젝트에 대 한 확장입니다.

## <a name="initializing-the-local-store"></a>로컬 저장소를 초기화합니다.

모든 동기화 테이블 작업을 수행 하려면 로컬 저장소를 초기화 합니다. Xamarin.Forms 솔루션의 클래스 라이브러리 PCL (이식 가능한) 프로젝트에서이 작업을 수행 합니다.

```csharp
using Microsoft.WindowsAzure.MobileServices;
using Microsoft.WindowsAzure.MobileServices.SQLiteStore;
using Microsoft.WindowsAzure.MobileServices.Sync;

namespace TodoAzure
{
    public partial class TodoItemManager
    {
        static TodoItemManager defaultInstance = new TodoItemManager();
        IMobileServiceClient client;
        IMobileServiceSyncTable<TodoItem> todoTable;

        private TodoItemManager()
        {
            this.client = new MobileServiceClient(Constants.ApplicationURL);
            var store = new MobileServiceSQLiteStore("localstore.db");
            store.DefineTable<TodoItem>();
            this.client.SyncContext.InitializeAsync(store);
            this.todoTable = client.GetSyncTable<TodoItem>();
        }
        ...
  }
}
```

새 로컬 SQLite 데이터베이스 만들어집니다는 `MobileServiceSQLiteStore` 존재 하지 않는 있는 클래스입니다. 그런 다음 `DefineTable<T>` 메서드의 필드와 일치 하는 로컬 저장소의 테이블을 만듭니다는 `TodoItem` 있는 존재 하지 않는 입력 합니다.

A *동기화 컨텍스트* 과 연관 된 `MobileServiceClient` 인스턴스와 동기화 테이블에서 적용 된 변경 내용을 추적 합니다. 동기화 컨텍스트 만들기, 업데이트 및 삭제 (CUD) 작업의 순서 있는 목록을 유지 하는 큐는 나중에 보냅니다 Azure 모바일 앱 인스턴스를 유지 관리 합니다. `IMobileServiceSyncContext.InitializeAsync()` 메서드는 동기화 컨텍스트를 로컬 저장소를 연결할 때 사용 됩니다.

`todoTable` 필드는는 `IMobileServiceSyncTable`, 로컬 저장소를 사용 하는 모든 CRUD 작업 하므로 합니다.

## <a name="performing-synchronization"></a>동기화를 수행합니다.

로컬 저장소는 Azure 모바일 앱으로 동기화 됩니다 인스턴스에 `SyncAsync` 메서드가 호출 됩니다.

```csharp
public async Task SyncAsync()
{
  ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

  try
  {
    await this.client.SyncContext.PushAsync();

    // The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
    // Use a different query name for each unique query in your program.
    await this.todoTable.PullAsync("allTodoItems", this.todoTable.CreateQuery());
  }
  catch (MobileServicePushFailedException exc)
  {
    if (exc.PushResult != null)
    {
      syncErrors = exc.PushResult.Errors;
    }
  }

  // Simple error/conflict handling.
  if (syncErrors != null)
  {
    foreach (var error in syncErrors)
    {
      if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
      {
        // Update failed, revert to server's copy
        await error.CancelAndUpdateItemAsync(error.Result);
      }
      else
      {
        // Discard local change
        await error.CancelAndDiscardItemAsync();
      }

      Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
    }
  }
}
```

`IMobileServiceSyncTable.PushAsync` 메서드는 특정 테이블 보다는 동기화 컨텍스트에서 작동 하 고 마지막 푸시 이후의 모든 CUD 변경 내용을 보냅니다.

끌어오기 수행한는 `IMobileServiceSyncTable.PullAsync` 단일 테이블에서 메서드. 첫 번째 매개 변수는 `PullAsync` 메서드는 모바일 장치에 대해서만 사용 되는 쿼리 이름입니다. Azure 모바일 클라이언트 SDK 수행 이름 결과 null이 아닌 쿼리를 제공 하는 *증분 동기화*때마다 가져오기 작업을 반환 하는 위치 결과 얻으려면 최신, `updatedAt` 결과의 타임 스탬프를 로컬에 저장 됩니다 시스템 테이블입니다. 후속 끌어오기 작업만 레코드를 검색한 후 해당 타임 스탬프입니다. 또는 *전체 동기화* 전달 하 여 액세스가 가능 `null` 쿼리 이름으로 생성 된 각 끌어오기 작업에서 검색 되 고 모든 레코드입니다. 모든 동기화 작업에 따라 수신된 된 데이터는 로컬 저장소에 삽입 됩니다.

> [!NOTE]
> **참고**: 끌어오기를 로컬에서 업데이트 중인 테이블에 대해 실행 되는 경우 끌어오기는 먼저 밀어넣기 동기화 컨텍스트에서 실행할 합니다. 이미 대기 중인 변경 내용 및 Azure 모바일 앱 인스턴스에서 새 데이터 간의 충돌을 최소화 합니다.

`SyncAsync` 메서드에 로컬 저장소와 Azure 모바일 앱 인스턴스에 동일한 레코드가 변경 될 때 충돌을 처리 하는 기본 구현을 포함 됩니다. 로컬 저장소와 Azure 모바일 앱 인스턴스 데이터를 업데이트 하였습니다 충돌 하는 경우는 `SyncAsync` 메서드 Azure 모바일 앱 인스턴스에 저장 된 데이터에서 로컬 저장소의 데이터를 업데이트 합니다. 다른 충돌이 발생 하면는 `SyncAsync` 메서드 로컬 변경 내용을 취소 합니다. Azure 모바일 앱 인스턴스에서 삭제 삭제 하는 데이터에 대 한 로컬 변경 있는 시나리오를 처리 합니다.

프로덕션 응용 프로그램에서 개발자는 사용자 지정 작성 해야 `IMobileServiceSyncHandler` 해당 사용 사례에 적합 한 충돌 처리 구현 합니다. 자세한 내용은 참조 [낙관적 동시성 충돌 해결에 대 한 사용](https://azure.microsoft.com/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/#optimisticconcurrency) Azure 포털에서 및 [관리 되는 클라이언트 SDK의에서 오프 라인 지원에 심층](https://blogs.msdn.microsoft.com/azuremobile/2014/04/07/deep-dive-on-the-offline-support-in-the-managed-client-sdk/) on MSDN 블로그.

## <a name="purging-data"></a>데이터 삭제

로컬 저장소 테이블에에서 데이터를 지울 수 있습니다는 `IMobileServiceSyncTable.PurgeAsync` 메서드. 이 메서드는 더 이상 응용 프로그램에 필요한 오래 된 데이터를 제거 하는 등의 시나리오를 지원 합니다. 예를 들어 예제 응용 프로그램 표시만 `TodoItem` 인스턴스를 완료 하지 않았습니다. 따라서 완료 된 항목 로컬에 저장 해야 하는 더 이상 사용 되지 않습니다. 다음과 같이 로컬 저장소에서 완료 된 항목을 제거를 수행할 수 있습니다.

```csharp
await todoTable.PurgeAsync(todoTable.Where(item => item.Done));
```

에 대 한 호출 `PurgeAsync` 푸시 작업을 트리거합니다. 따라서 로컬 저장소에서 제거 하기 전에 로컬로 완료 상태로 표시 된 모든 항목이 Azure 모바일 앱 인스턴스에 전송 됩니다. 그러나 Azure 모바일 앱 인스턴스와 동기화 보류 중인 작업이 있으면 지우기 throw 됩니다는 `InvalidOperationException` 경우가 아니면는 `force` 로 설정 된 `true`합니다. 다른 방식을 확인 하는 것은 `IMobileServiceSyncContext.PendingOperations` 속성 보류 중인 Azure 모바일 앱 인스턴스에 푸시되 지 않은 하는 속성은 0 하는 경우 데이터베이스 제거가만 수행 하는 작업의 수를 반환 합니다.

> [!NOTE]
> **참고**: 호출 `PurgeAsync` 와 `force` 매개 변수 설정 `true` 보류 중인 변경 내용이 모두 손실 됩니다.

## <a name="initiating-synchronization"></a>동기화 시작

샘플 응용 프로그램에서의 `SyncAsync` 통해 메서드가 호출 되는 `TodoList.OnAppearing` 메서드:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  // Set syncItems to true to synchronize the data on startup when running in offline mode
  await RefreshItems(true, syncItems: true);
}
```

즉, 응용 프로그램이 시작 될 때 Azure 모바일 앱 인스턴스와 동기화 하려고 합니다.

새로 고치 데이터의 목록에 Windows 플랫폼에서 사용 하 여 끌어오기를 사용 하 여 iOS 및 Android에서 동기화에 시작할 수는 또한는 **동기화** 사용자 인터페이스에서 단추입니다. 자세한 내용은 참조 [새로 고침을 밀어넣을](~/xamarin-forms/user-interface/listview/interactivity.md#Pull_to_Refresh)합니다.

## <a name="summary"></a>요약

이 문서는 Xamarin.Forms 응용 프로그램에 오프 라인 동기화 기능을 추가 하는 방법을 설명 합니다. 오프 라인 동기화에는 사용자가 모바일 응용 프로그램, 보기, 추가, 또는 데이터 수정, 상호 작용할 수 있도록 있더라도 네트워크에 연결 하지 않습니다. 변경 내용이 로컬 데이터베이스에 저장 됩니다 및 장치가 온라인 상태 이면 되 면 변경 내용이 Azure 모바일 앱 인스턴스와 동기화 할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [TodoAzureAuthOfflineSync (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)
- [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure 모바일 앱을 사용한 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
