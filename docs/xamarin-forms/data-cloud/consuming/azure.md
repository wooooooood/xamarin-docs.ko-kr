---
title: "Azure 모바일 앱 사용"
description: "Azure 모바일 앱을 사용 하 여 확장 가능한 백 엔드 모바일 인증, 오프 라인 동기화 및 푸시 알림을 지 원하는 Azure 앱 서비스에서 호스트 된 앱을 개발할 수 있습니다. Node.js 백 엔드를 사용 하는 Azure 모바일 앱에 적용할 수만,이 문서에서는 쿼리, 삽입, 업데이트 및 Azure 모바일 앱 인스턴스 테이블에 저장 된 데이터를 삭제 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2B3EFD0A-2922-437D-B151-4B4DE46E2095
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 5b087700e3a5276e19454a8dafedb508758b7b71
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="consuming-an-azure-mobile-app"></a>Azure 모바일 앱 사용

_Azure 모바일 앱을 사용 하 여 확장 가능한 백 엔드 모바일 인증, 오프 라인 동기화 및 푸시 알림을 지 원하는 Azure 앱 서비스에서 호스트 된 앱을 개발할 수 있습니다. Node.js 백 엔드를 사용 하는 Azure 모바일 앱에 적용할 수만,이 문서에서는 쿼리, 삽입, 업데이트 및 Azure 모바일 앱 인스턴스 테이블에 저장 된 데이터를 삭제 하는 방법을 설명 합니다._

Xamarin.Forms가 사용할 수 있는 Azure 모바일 앱 인스턴스를 만드는 방법에 대 한 자세한 내용은 참조 하십시오. [Xamarin.Forms 앱 만들기](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/)합니다. 다음이 지침을 따랐다면 다운로드 가능한 샘플 응용 프로그램 하도록 구성할 수 있습니다 설정 하 여 Azure 모바일 앱 인스턴스를 사용할는 `Constants.ApplicationURL` Azure 모바일 앱 인스턴스의 URL로 합니다. 그런 다음 샘플 응용 프로그램을 실행할 때 해당 인스턴스에 연결 됩니다는 Azure 모바일 앱, 다음 스크린샷에 표시 된 것 처럼:

![](azure-images/portal.png "샘플 응용 프로그램")

Azure 모바일 앱에 대 한 액세스가 통해는 [Azure 모바일 클라이언트 SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), HTTPS를 통해 Xamarin.Forms 샘플 응용 프로그램을 Azure에서 모든 연결 하 고 있습니다.

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간에 보안 연결 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 적용 합니다. IOS 9에 대해 빌드된 앱에서 기본적으로 AT를 사용 하므로 모든 연결 AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 AT 옵트인 수 있습니다는 `HTTPS` 및 프로토콜을 인터넷 리소스에 대 한 통신을 보호 합니다. 응용 프로그램의 업데이트 하 여 이렇게 할 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="consuming-an-azure-mobile-app-instance"></a>Azure 모바일 앱 인스턴스 사용

[Azure 모바일 클라이언트 SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) 제공는 `MobileServiceClient` 다음 코드 예제에 나와 있는 것 처럼 Azure 모바일 앱 인스턴스에 액세스 하는 Xamarin.Forms 응용 프로그램에 사용 되는 클래스:

```csharp
IMobileServiceTable<TodoItem> todoTable;
MobileServiceClient client;

public TodoItemManager ()
{
  client = new MobileServiceClient (Constants.ApplicationURL);
  todoTable = client.GetTable<TodoItem> ();
}
```

경우는 `MobileServiceClient` 인스턴스가 만들어지고, Azure 모바일 앱 인스턴스를 식별 하는 응용 프로그램 URL을 지정 해야 합니다. 모바일 앱에 대 한 대시보드에서이 값을 가져올 수 있습니다는 [Microsoft Azure 포털](https://portal.azure.com/)합니다.

에 대 한 참조는 `TodoItem` 해당 테이블에 대해 작업을 수행 하려면 먼저 Azure 모바일 앱 인스턴스에 저장 된 테이블을 가져와야 합니다. 호출 하 여 이렇게는 `GetTable` 에서 메서드는 `MobileServiceClient` 인스턴스를 반환 하는 `IMobileServiceTable<TodoItem>` 참조 합니다.

### <a name="querying-data"></a>데이터 쿼리

호출 하 여 테이블의 내용을 검색할 수는 `IMobileServiceTable.ToEnumerableAsync` 메서드를 비동기적으로 쿼리를 실행 하 고 결과 반환 합니다. 데이터 일 수도 있습니다를 포함 하 여 서버 쪽 필터링을 `Where` 쿼리에 절. `Where` 절 다음 코드 예제에 나와 있는 것 처럼 행 필터링 조건자를 테이블에 대해 쿼리를 적용 합니다.

```csharp
public async Task<ObservableCollection<TodoItem>> GetTodoItemsAsync (bool syncItems = false)
{
  ...
  IEnumerable<TodoItem> items = await todoTable
              .Where (todoItem => !todoItem.Done)
              .ToEnumerableAsync ();

  return new ObservableCollection<TodoItem> (items);
}
```

모든 항목을 반환 하는이 쿼리는 `TodoItem` 갖는 테이블 `Done` 속성이 `false`합니다. 쿼리 결과에 저장할는 `ObservableCollection` 표시 합니다.

### <a name="inserting-data"></a>데이터 삽입

새 열이 자동으로 생성 될 Azure 모바일 앱 인스턴스 데이터를 삽입할 경우 필요에 따라 테이블, Azure 모바일 앱 인스턴스에서 해당 동적 스키마를 사용할 수를 제공 합니다. `IMobileServiceTable.InsertAsync` 메서드 다음 코드 예제에 나와 있는 것 처럼 지정된 된 테이블에 데이터의 새 행을 삽입를 사용 합니다.

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.InsertAsync (item);
  ...
}
```

삽입 요청을 만들 때 ID 해야 Azure 모바일 앱 인스턴스에 전달 되는 데이터에 지정할 수 없습니다. 삽입 요청 ID를 포함 하는 경우는 `MobileServiceInvalidOperationException` throw 됩니다.

이후에 `InsertAsync` 메서드가 완료 되 면 Azure 모바일 앱 인스턴스에 있는 데이터의 ID에 채워집니다는 `TodoItem` Xamarin.Forms 응용 프로그램에서 인스턴스.

### <a name="updating-data"></a>데이터 업데이트

새 열이 자동으로 생성 될 Azure 모바일 앱 인스턴스에 데이터를 업데이트할 때 필요에 따라 테이블, Azure 모바일 앱 인스턴스에서 해당 동적 스키마를 사용할 수를 제공 합니다. `IMobileServiceTable.UpdateAsync` 메서드 다음 코드 예제에 나와 있는 것 처럼 기존 데이터를 새 정보로 업데이트를 사용 합니다.

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.UpdateAsync (item);
  ...
}
```

업데이트 요청을 만들 때 Azure 모바일 앱 인스턴스 데이터를 업데이트할 수를 식별할 수 있도록 ID는 지정 되어야 합니다. 이 ID 값에 저장 되는 `TodoItem.ID` 속성입니다. 업데이트 요청 포함 되지 않은 경우 ID를 업데이트할 수 없으므로 데이터를 확인 하려면 Azure 모바일 앱 인스턴스에 대 한 있고 따라서는 `MobileServiceInvalidOperationException` throw 됩니다.

### <a name="deleting-data"></a>데이터 삭제

`IMobileServiceTable.DeleteAsync` 메서드 다음 코드 예제에 나와 있는 것 처럼, Azure 모바일 앱 테이블에서 데이터 삭제를 사용 합니다.

```csharp
public async Task DeleteTaskAsync (TodoItem item)
{
  ...
  await todoTable.DeleteAsync(item);
  ...
}
```

Delete 요청을 만들 때 Azure 모바일 앱 sinstance 삭제할 데이터를 식별할 수 있도록 ID는 지정 되어야 합니다. 이 ID 값에 저장 되는 `TodoItem.ID` 속성입니다. 삭제 될 데이터를 확인 하려면 Azure 모바일 앱 인스턴스에 대 한 방법이 있으면 삭제 요청 ID 들어 있으면 하므로 `MobileServiceInvalidOperationException` throw 됩니다.

## <a name="summary"></a>요약

이 문서에서는 사용 하는 방법의 [Azure 모바일 클라이언트 SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) 쿼리, 삽입, 업데이트 및 Azure 모바일 앱 인스턴스 테이블에 저장 된 데이터를 삭제 합니다. SDK는 제공 된 `MobileServiceClient` Xamarin.Forms 응용 프로그램에서 Azure 모바일 앱 인스턴스에 액세스 하기 위해 사용 되는 클래스입니다.


## <a name="related-links"></a>관련 링크

- [TodoAzure (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure/)
- [Xamarin.Forms 응용 프로그램 만들기](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
