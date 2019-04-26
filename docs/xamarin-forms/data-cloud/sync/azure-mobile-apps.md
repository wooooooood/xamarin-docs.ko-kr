---
title: Azure Mobile Apps를 사용 하 여 오프 라인 데이터 동기화
description: 이 문서에서는 Azure Mobile Apps 백 엔드를 사용 하는 Xamarin.Forms 응용 프로그램에 오프 라인 동기화 기능을 추가 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: DBB343B0-2709-4C20-A669-5522B9956D9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2017
ms.openlocfilehash: 6080b4dc152558d6f532399cee7424670c588c28
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61319580"
---
# <a name="synchronizing-offline-data-with-azure-mobile-apps"></a>Azure Mobile Apps를 사용 하 여 오프 라인 데이터 동기화

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)

_오프 라인 동기화 하면 모바일 응용 프로그램, 보기, 추가, 또는 데이터 수정, 상호 작용할 수 없는 네트워크에 연결 하는 경우에 합니다. 변경 내용은 로컬 데이터베이스에 저장 됩니다 하 고 장치가 온라인 상태가 되 면 변경 내용을 Azure Mobile Apps 인스턴스와 동기화 될 수 있습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에 오프 라인 동기화 기능을 추가 하는 방법에 설명 합니다._

## <a name="overview"></a>개요

[Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) 제공 된 `IMobileServiceTable` Azure Mobile Apps 인스턴스에 저장 된 테이블에서 수행할 수 있는 작업을 나타내는 인터페이스입니다. 이러한 작업을 Azure Mobile Apps 인스턴스에 직접 연결 하 고 모바일 장치에 네트워크 연결이 없는 경우 실패 합니다.

Azure Mobile Client SDK는 오프 라인 동기화를 지원 하려면 다음을 지원 합니다. *테이블을 동기화*를 통해 제공 되는 `IMobileServiceSyncTable` 인터페이스입니다. 이 인터페이스는 동일한 만들기, 읽기, 업데이트, 삭제 (CRUD) 작업을 제공 합니다 `IMobileServiceTable` 인터페이스 되지만 작업에서 읽기 또는 쓰기는 로컬 저장소에 있습니다. 호출 될 때까지 로컬 저장소에서 Azure Mobile Apps 인스턴스를 새 데이터로 채워지지 *끌어오기* 데이터입니다. 호출 될 때까지 Azure Mobile Apps 인스턴스에 데이터 전송 되지 않습니다는 마찬가지로 *푸시* 로컬 변경 합니다.

오프 라인 동기화에는 Azure Mobile Apps 인스턴스 및 사용자 지정 충돌 해결 프로그램에서 로컬 저장소에 있으며 동일한 레코드가 변경 된 경우 충돌을 감지 하는 것에 대 한 지원이 포함 됩니다. Azure Mobile Apps 인스턴스 또는 로컬 저장소에서 충돌 하거나 처리할 수 있습니다.

오프 라인 동기화에 대 한 자세한 내용은 참조 하세요. [Azure Mobile Apps에서 오프 라인 데이터 동기화](/azure/app-service-mobile/app-service-mobile-offline-data-sync/) 하 고 [Xamarin.Forms 모바일 앱에 대 한 오프 라인 동기화 사용](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-offline-data/)합니다.

## <a name="setup"></a>설정

Xamarin.Forms 응용 프로그램 및 Azure Mobile Apps 인스턴스 간에 오프 라인 동기화를 통합 하기 위한 프로세스는 다음과 같습니다.

1. Azure Mobile Apps 인스턴스를 만듭니다. 자세한 내용은 [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)합니다.
1. 추가 된 [Microsoft.Azure.Mobile.Client.SQLiteStore](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/) Xamarin.Forms 솔루션의 모든 프로젝트에 NuGet 패키지.
1. (선택 사항) Azure Mobile Apps 인스턴스와 Xamarin.Forms 응용 프로그램에서 인증을 사용 하도록 설정 합니다. 자세한 내용은 [Azure Mobile Apps를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure.md)합니다.

다음 섹션에서는 Microsoft.Azure.Mobile.Client.SQLiteStore NuGet 패키지를 사용 하 여 유니버설 Windows 플랫폼 (UWP) 프로젝트 구성에 대 한 추가 설치 지침을 제공 합니다. 추가 설치 없이 iOS 및 Android에서 Microsoft.Azure.Mobile.Client.SQLiteStore NuGet 패키지를 사용 해야 합니다.

### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

SQLite에서 Windows 플랫폼 (UWP (유니버설)을 사용 하려면 다음이 단계를 수행 합니다.

1. 설치 합니다 [유니버설 Windows 플랫폼용 SQLite](http://sqlite.org/2016/sqlite-uwp-3120200.vsix) 개발 환경에서 Visual Studio 확장입니다.
1. Visual Studio에서 UWP 프로젝트에서 마우스 오른쪽 단추로 클릭 **참조 > 참조 추가**, 이동할 **확장** 추가 합니다 **유니버설 Windows 플랫폼용 SQLite** 및  **유니버설 Windows 플랫폼 앱 용 visual c + + 2015 런타임** UWP 프로젝트를 확장 합니다.

## <a name="initializing-the-local-store"></a>로컬 저장소를 초기화합니다.

모든 동기화 테이블 작업을 수행 하려면 로컬 저장소를 초기화 합니다. 이 작업은 Xamarin.Forms 솔루션의 이식 가능한 클래스 라이브러리 (PCL) 프로젝트에서 수행 됩니다.

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

새 로컬 SQLite 데이터베이스에서 만들어집니다는 `MobileServiceSQLiteStore` 클래스는 이미 존재 하지 않습니다. 그런 다음, `DefineTable<T>` 메서드는 필드와 일치 하는 로컬 저장소에 테이블을 만듭니다는 `TodoItem` 는 이미 존재 하지 않는 입력 합니다.

A *동기화 컨텍스트에* 연관 된를 `MobileServiceClient` 인스턴스와 동기화 테이블에 적용 된 변경 내용을 추적 합니다. 동기화 컨텍스트는 만들기, 업데이트 및 삭제 (CUD) 작업의 정렬된 된 목록을 유지 하는 큐는 나중에 보냅니다 Azure Mobile Apps 인스턴스를 유지 관리 합니다. `IMobileServiceSyncContext.InitializeAsync()` 메서드 동기화 컨텍스트를 사용 하 여 로컬 저장소를 연결할 때 사용 됩니다.

`todoTable` 필드는 `IMobileServiceSyncTable`, 이므로 모든 CRUD 작업에는 로컬 저장소를 사용 합니다.

## <a name="performing-synchronization"></a>동기화를 수행합니다.

로컬 저장소는 Azure Mobile Apps를 사용 하 여 동기화 된 경우 인스턴스는 `SyncAsync` 메서드:

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

`IMobileServiceSyncTable.PushAsync` 메서드는 특정 테이블 보다는 동기화 컨텍스트에에서 작동 하 고 마지막 푸시 이후로 모든 CUD 변경을 보냅니다.

끌어오기가 수행한는 `IMobileServiceSyncTable.PullAsync` 단일 테이블에서 메서드. 첫 번째 매개 변수는 `PullAsync` 메서드는 모바일 장치에만 사용 되는 쿼리 이름입니다. 수행 하는 Azure Mobile Client SDK 이름 결과 null이 아닌 쿼리를 제공 하는 *증분 동기화*때마다 가져오기 작업을 반환 하는 위치 결과 얻으려면 최신, `updatedAt` 로컬에 저장 된 결과의 타임 스탬프 시스템 테이블입니다. 후속 끌어오기 작업은 다음 레코드가 해당 timestamp 이후의 검색 합니다. 한편 *전체 동기화* 전달 하 여 수행할 수 있습니다 `null` 쿼리 이름으로 각 끌어오기 작업에서 검색 되는 모든 레코드의 결과입니다. 모든 동기화 작업에 다음 받은 데이터는 로컬 저장소에 삽입 됩니다.

> [!NOTE]
> 끌어오기가 로컬 업데이트를 보류 중인 테이블에 대해를 실행 하는 경우 끌어오기 동기화 컨텍스트에 푸시를 먼저 실행 됩니다. 이 이미 큐에 대기 중인 변경 내용과 Azure Mobile Apps 인스턴스에서 새 데이터 간에 충돌을 최소화 합니다.

`SyncAsync` 메서드는 모두 로컬 저장소에 Azure Mobile Apps 인스턴스를 동일한 레코드가 변경 될 때 충돌을 처리 하는 것에 대 한 기본 구현에도 포함 되어 있습니다. 로컬 저장소와 Azure Mobile Apps 인스턴스에서 데이터가 업데이트 된 경우 충돌을 `SyncAsync` 메서드 Azure Mobile Apps 인스턴스에 저장 된 데이터에서 로컬 저장소에 데이터를 업데이트 합니다. 다른 충돌이 발생 하면는 `SyncAsync` 메서드 로컬 변경 내용을 취소 합니다. Azure Mobile Apps 인스턴스에서 삭제 되는 데이터에 대 한 로컬 변경 있는 시나리오를 처리 합니다.

개발자가 프로덕션 응용 프로그램에서 사용자 지정 작성 해야 `IMobileServiceSyncHandler` 충돌 처리 구현 해당 사용 사례에 적합 합니다. 자세한 내용은 [충돌 해결을 위해 사용 하 여 낙관적 동시성](https://azure.microsoft.com/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/#optimisticconcurrency) Azure portal에서 및 [관리 되는 클라이언트 SDK의에서 오프 라인 지원에 대 한 심층 분석](https://blogs.msdn.microsoft.com/azuremobile/2014/04/07/deep-dive-on-the-offline-support-in-the-managed-client-sdk/) on MSDN 블로그.

## <a name="purging-data"></a>데이터를 제거합니다.

사용 하 여 데이터의 로컬 저장소의 테이블을 지울 수 있습니다는 `IMobileServiceSyncTable.PurgeAsync` 메서드. 이 메서드는 응용 프로그램이 더 이상 필요 하지 않는 오래 된 데이터를 제거 하는 등의 시나리오를 지원 합니다. 예를 들어, 샘플 응용 프로그램 표시만 `TodoItem` 완료 되지 않은 인스턴스가 있습니다. 따라서 완료 된 항목은 더 이상 로컬로 저장 해야 합니다. 다음과 같이 로컬 저장소에서 완료 된 항목 제거를 수행할 수 있습니다.

```csharp
await todoTable.PurgeAsync(todoTable.Where(item => item.Done));
```

에 대 한 호출 `PurgeAsync` 도 푸시 작업을 트리거합니다. 따라서 로컬 저장소에서 제거 하기 전에 로컬로 완료로 표시 되는 모든 항목 Azure Mobile Apps 인스턴스에 전송 됩니다. 그러나 Azure Mobile Apps 인스턴스와 동기화 보류 중인 작업이 없으면 지우기 시킵니다를 `InvalidOperationException` 하지 않는 한는 `force` 매개 변수는 설정 `true`. 대체 전략을 검토 하는 것은 `IMobileServiceSyncContext.PendingOperations` 보류 중인 Azure Mobile Apps 인스턴스에 푸시되 지 않은 속성이 0 이면 지우기만 수행 하는 작업의 수를 반환 하는 속성입니다.

> [!NOTE]
> 호출 `PurgeAsync` 사용 하 여 합니다 `force` 매개 변수 설정 `true` 보류 중인 변경 내용이 손실 됩니다.

## <a name="initiating-synchronization"></a>동기화 작업 시작

샘플 응용 프로그램에서 `SyncAsync` 메서드를 통해 호출 되는 `TodoList.OnAppearing` 메서드:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  // Set syncItems to true to synchronize the data on startup when running in offline mode
  await RefreshItems(true, syncItems: true);
}
```

즉, 응용 프로그램은 시작할 때 Azure Mobile Apps 인스턴스와 동기화 하려고 합니다.

또한 시작할 수 있습니다 iOS 및 Android에서 끌어오기를 사용 하 여 Windows 플랫폼에서 데이터를 목록에 새로 고침을 사용 하 여 합니다 **동기화** 사용자 인터페이스에서 단추입니다. 자세한 내용은 [고치려면 당김](~/xamarin-forms/user-interface/listview/interactivity.md#Pull_to_Refresh)합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms 응용 프로그램에 오프 라인 동기화 기능을 추가 하는 방법을 설명 합니다. 오프 라인 동기화 하면 모바일 응용 프로그램, 보기, 추가, 또는 데이터 수정, 상호 작용할 수 없는 네트워크에 연결 하는 경우에 합니다. 변경 내용은 로컬 데이터베이스에 저장 됩니다 하 고 장치가 온라인 상태가 되 면 변경 내용을 Azure Mobile Apps 인스턴스와 동기화 될 수 있습니다.


## <a name="related-links"></a>관련 링크

- [TodoAzureAuthOfflineSync (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)
- [Azure 모바일 앱 사용](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Mobile Apps 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
