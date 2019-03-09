---
title: Azure 모바일 앱 사용
description: 이 기사를만 적용할 수 있는 Azure Mobile apps Node.js 백 엔드를 사용 하는 쿼리, 삽입, 업데이트 및 Azure Mobile Apps 인스턴스 테이블에 저장 된 데이터를 삭제 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2B3EFD0A-2922-437D-B151-4B4DE46E2095
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 8bd77ec4975fae3cc7245c5adc2b5ef18568b9e1
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667585"
---
# <a name="consuming-an-azure-mobile-app"></a>Azure 모바일 앱 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure/)

_Azure Mobile Apps를 사용 하면 모바일 인증, 오프 라인 동기화 및 푸시 알림 지원을 통해 Azure App Service에서 호스트 되는 확장 가능한 백 엔드를 사용 하 여 앱을 개발할 수 있습니다. 이 기사를만 적용할 수 있는 Azure Mobile apps Node.js 백 엔드를 사용 하는 쿼리, 삽입, 업데이트 및 Azure Mobile Apps 인스턴스 테이블에 저장 된 데이터를 삭제 하는 방법을 설명 합니다._

> [!NOTE]
> 월 30 일에 시작, 모든 새 Azure Mobile Apps 만들어집니다 TLS 1.2를 사용 하 여 기본적으로. 또한 하는 것이 좋습니다 기존 Azure Mobile Apps는 TLS 1.2를 사용 하도록 다시 구성할 수 있습니다. Azure 모바일 앱에서 TLS 1.2를 적용 하는 방법에 대 한 자세한 내용은 [적용 TLS 버전](/azure/app-service/app-service-web-tutorial-custom-ssl#enforce-tls-versions)합니다. TLS 1.2를 사용 하도록 Xamarin 프로젝트를 구성 하는 방법에 대 한 자세한 내용은 [전송 계층 보안 (TLS) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md)합니다.

Xamarin.Forms에서 사용할 수 있는 Azure Mobile Apps 인스턴스를 만드는 방법에 대 한 자세한 내용은 [Xamarin.Forms 앱 만들기](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/)합니다. 이 단계를 따라 다운로드 가능한 샘플 응용 프로그램을 구성할 수 있습니다 설정 하 여 Azure Mobile Apps 인스턴스를 사용 하 여 `Constants.ApplicationURL` Azure Mobile Apps 인스턴스의 URL로 합니다. 그런 다음 샘플 응용 프로그램이 실행 될 때이 Azure Mobile Apps 인스턴스에 연결할 다음 스크린샷에 표시 된 대로:

![](azure-images/portal.png "샘플 응용 프로그램")

통해 Azure Mobile Apps에 대 한 액세스는 [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), Xamarin.Forms 샘플 응용 프로그램이 Azure에서 모든 연결이 HTTPS를 통해 이루어집니다.

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 ATS ()는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결 하므로 중요 한 정보가 실수로 유출 방지 적용 합니다. ATS는 iOS 9 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되므로 모든 연결이 ATS 보안 요구 사항이 적용 됩니다. 연결에서 이러한 요구를 충족 하지 않는, 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 ATS 옵트인 수 있습니다는 `HTTPS` 프로토콜 및 인터넷 리소스에 대 한 통신을 보호 합니다. 이 앱을 업데이트 하 여 수행할 수 있습니다 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="consuming-an-azure-mobile-app-instance"></a>Azure 모바일 앱 인스턴스를 사용합니다.

합니다 [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) 제공 된 `MobileServiceClient` 다음 코드 예제에 표시 된 대로 Azure Mobile Apps 인스턴스에 액세스 하기 위해 Xamarin.Forms 응용 프로그램에서 사용 되는 클래스:

```csharp
IMobileServiceTable<TodoItem> todoTable;
MobileServiceClient client;

public TodoItemManager ()
{
  client = new MobileServiceClient (Constants.ApplicationURL);
  todoTable = client.GetTable<TodoItem> ();
}
```

경우는 `MobileServiceClient` 인스턴스가 만들어지고, Azure Mobile Apps 인스턴스를 식별 하는 응용 프로그램 URL을 지정 해야 합니다. 이 값에 모바일 앱의 대시보드에서 가져올 수 있습니다 합니다 [Microsoft Azure Portal](https://portal.azure.com/)합니다.

에 대 한 참조는 `TodoItem` 해당 테이블에 대해 작업을 수행 하려면 Azure Mobile Apps 인스턴스에 저장 된 테이블을 가져와야 합니다. 호출 하 여 이렇게 합니다 `GetTable` 메서드를 `MobileServiceClient` 인스턴스를 반환 하는 `IMobileServiceTable<TodoItem>` 참조.

### <a name="querying-data"></a>데이터 쿼리

테이블의 콘텐츠를 호출 하 여 검색할 수는 `IMobileServiceTable.ToEnumerableAsync` 메서드를 비동기적으로 쿼리를 평가 하 고 결과 반환 합니다. 데이터 일 수도 있습니다를 포함 하 여 서버 쪽 필터링을 `Where` 쿼리 절. `Where` 절 다음 코드 예제에 나와 있는 것 처럼 행 필터링 조건자를 테이블에 대해 쿼리를 적용 합니다.

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

모든 항목을 반환 하는이 쿼리는 `TodoItem` 테이블로 `Done` 속성과 같으면 `false`합니다. 쿼리 결과에 저장할는 `ObservableCollection` 표시 합니다.

### <a name="inserting-data"></a>데이터 삽입

새 열은 자동으로 생성 될 Azure Mobile Apps 인스턴스에 데이터를 삽입할 때 필요에 따라 테이블에서 Azure Mobile Apps 인스턴스에서 동적 스키마를 사용을 제공 합니다. `IMobileServiceTable.InsertAsync` 다음 코드 예제에 표시 된 대로 지정된 된 테이블에 새 데이터 행을 삽입할 메서드는:

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.InsertAsync (item);
  ...
}
```

삽입 요청을 만들 때 ID는 Azure Mobile Apps 인스턴스에 전달 되는 데이터에 지정 되어야 합니다. 삽입 요청 ID를 포함 하는 경우는 `MobileServiceInvalidOperationException` throw 됩니다.

후 합니다 `InsertAsync` 메서드가 완료 되 면 Azure Mobile Apps 인스턴스에 있는 데이터의 ID에 채워집니다는 `TodoItem` Xamarin.Forms 응용 프로그램의 인스턴스.

### <a name="updating-data"></a>데이터 업데이트

새 열은 자동으로 생성 될 Azure Mobile Apps 인스턴스에 데이터를 업데이트할 때 필요에 따라 테이블에서 Azure Mobile Apps 인스턴스에서 동적 스키마를 사용을 제공 합니다. `IMobileServiceTable.UpdateAsync` 메서드를 사용 하는 새 정보를 사용 하 여 기존 데이터를 업데이트한 다음 코드 예제 에서처럼:

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.UpdateAsync (item);
  ...
}
```

업데이트 요청을 만들 때 Azure Mobile Apps 인스턴스를 업데이트할 데이터를 식별할 수 있도록 ID는 지정 되어야 합니다. 이 ID 값에 저장 되는 `TodoItem.ID` 속성입니다. 업데이트 요청 포함 되지 않은 경우 ID를 업데이트할 수 없으므로 데이터를 확인 하려면 Azure Mobile Apps 인스턴스에 대 한 이므로 `MobileServiceInvalidOperationException` throw 됩니다.

### <a name="deleting-data"></a>데이터 삭제

`IMobileServiceTable.DeleteAsync` 다음 코드 예제에 표시 된 대로 메서드는 Azure Mobile Apps 테이블에서 데이터 삭제를 사용 합니다.

```csharp
public async Task DeleteTaskAsync (TodoItem item)
{
  ...
  await todoTable.DeleteAsync(item);
  ...
}
```

에 delete 요청을 수행할 때 Azure Mobile App sinstance 삭제할 데이터를 식별할 수 있도록 ID는 지정 되어야 합니다. 이 ID 값에 저장 되는 `TodoItem.ID` 속성입니다. 삭제할 데이터를 확인 하려면 Azure Mobile Apps 인스턴스에 대 한 방법이 있으면 삭제 요청 ID가 없으면 등을 `MobileServiceInvalidOperationException` throw 됩니다.

## <a name="summary"></a>요약

이 문서에 사용 하는 방법을 설명 합니다 [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) 를 쿼리, 삽입, 업데이트 및 Azure 모바일 앱 인스턴스에서 테이블에 저장 된 데이터를 삭제 합니다. SDK는 제공 된 `MobileServiceClient` Azure Mobile Apps 인스턴스에 액세스 하기 위해 Xamarin.Forms 응용 프로그램에서 사용 되는 클래스입니다.


## <a name="related-links"></a>관련 링크

- [TodoAzure (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure/)
- [Xamarin.Forms 앱 만들기](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
