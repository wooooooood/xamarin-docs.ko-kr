---
title: Xamarin.Forms 로컬 데이터베이스
description: Xamarin.Forms는 SQLite 데이터베이스 엔진을 사용하여 데이터베이스 기반 애플리케이션을 지원하기 때문에 공유 코드로 개체를 로드하고 저장할 수 있습니다. 이 문서에서는 Xamarin.Forms 애플리케이션이 SQLite.Net을 사용하여 데이터를 읽고 로컬 SQLite 데이터베이스에 데이터를 기록하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 321448453ebe38bd7d43665a3c8bade4fe0f68c2
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645249"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms 로컬 데이터베이스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

_Xamarin.Forms는 SQLite 데이터베이스 엔진을 사용하여 데이터베이스 기반 애플리케이션을 지원하기 때문에 공유 코드로 개체를 로드하고 저장할 수 있습니다. 이 문서에서는 Xamarin.Forms 애플리케이션이 SQLite.Net을 사용하여 데이터를 읽고 로컬 SQLite 데이터베이스에 데이터를 기록하는 방법을 설명합니다._

## <a name="overview"></a>개요

Xamarin.Forms 애플리케이션은 [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) 패키지를 사용하여 NuGet에서 제공되는 `SQLite` 클래스를 참조하여 데이터베이스 작업을 공유 코드에 통합할 수 있습니다. 데이터베이스 작업은 Xamarin.Forms 솔루션의.NET 표준 라이브러리 프로젝트에서 정의할 수 있습니다.

함께 제공되는 [애플리케이션 예제](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo)는 간단한 할 일 목록 애플리케이션입니다. 다음 스크린샷에서는 각 플랫폼에서 샘플이 어떻게 나타나는지를 보여 줍니다.

[![Xamarin.Forms 데이터베이스 예제 스크린샷](databases-images/todo-list-sml.png "TodoList 첫 페이지 스크린샷")](databases-images/todo-list.png#lightbox "TodoList 첫 페이지 스크린샷") [![Xamarin.Forms 데이터베이스 예제 스크린샷](databases-images/todo-list-sml.png "TodoList 첫 페이지 스크린샷")](databases-images/todo-list.png#lightbox "TodoList 첫 페이지 스크린샷")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>SQLite 사용

Xamarin.Forms .NET 표준 라이브러리에 SQLite 지원을 추가하려면 NuGet의 검색 기능을 사용하여 **sqlite-net-pcl**을 찾고 최신 패키지를 설치합니다.

![NuGet SQLite.NET PCL 패키지 추가](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet SQLite.NET PCL 패키지 추가")

비슷한 이름의 NuGet 패키지가 여러 개 있으며 정확한 패키지는 다음 특성을 포함합니다.

- **만든 사람:** Frank A. Krueger
- **ID:** sqlite-net-pcl
- **NuGet 링크:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> 패키지 이름에도 불구하고 .NET 표준 프로젝트에서도 **sqlite-net-pcl** NuGet 패키지를 사용합니다.

참조가 추가되었으면 데이터베이스를 저장하기 위해 로컬 파일 경로를 반환하는 `App` 클래스에 속성을 추가합니다.

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(
        Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "TodoSQLite.db3"));
    }
    return database;
  }
}
```

아래에 표시된 것처럼 `TodoItemDatabase` 생성자는 데이터베이스 파일에 대한 경로를 인수로 사용합니다.

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

데이터베이스를 싱글톤으로 노출하면 애플리케이션이 실행되는 동안 열린 상태로 유지되는 단일 데이터베이스 연결이 생성되므로 데이터베이스 작업이 수행될 때마다 데이터베이스 파일을 열거나 닫는 비용을 피할 수 있는 이점이 있습니다.

`TodoItemDatabase` 클래스의 나머지 부분에는 플랫폼 간 실행되는 SQLite 쿼리가 포함됩니다. 예제 쿼리 코드는 아래와 같습니다(구문에 대한 자세한 내용은 [SQLite.NET with Xamarin.iOS 사용](~/ios/data-cloud/data/using-sqlite-orm.md)에서 확인 가능).

```csharp
public Task<List<TodoItem>> GetItemsAsync()
{
  return database.Table<TodoItem>().ToListAsync();
}

public Task<List<TodoItem>> GetItemsNotDoneAsync()
{
  return database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
}

public Task<TodoItem> GetItemAsync(int id)
{
  return database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
}

public Task<int> SaveItemAsync(TodoItem item)
{
  if (item.ID != 0)
  {
    return database.UpdateAsync(item);
  }
  else {
    return database.InsertAsync(item);
  }
}

public Task<int> DeleteItemAsync(TodoItem item)
{
  return database.DeleteAsync(item);
}
```

> [!NOTE]
> 비동기 SQLite.Net API를 사용하면 데이터베이스 작업이 백그라운드 스레드로 전환된다는 이점이 있습니다. 또한 API에서 대신 처리하므로 동시성 처리 코드를 추가로 작성할 필요가 없습니다.

## <a name="summary"></a>요약

Xamarin.Forms는 SQLite 데이터베이스 엔진을 사용하여 데이터베이스 기반 애플리케이션을 지원하기 때문에 공유 코드로 개체를 로드하고 저장할 수 있습니다.

이 문서에서는 Xamarin.Forms를 사용하여 SQLite 데이터베이스에 **액세스**하는 것을 중점적으로 설명합니다. SQLite.Net 자체로 작업하는 방법은 [Android에서 SQLite.NET](~/android/data-cloud/data-access/using-sqlite-orm.md) 또는 [iOS에서 SQLite.NET](~/ios/data-cloud/data/using-sqlite-orm.md) 설명서를 참조하세요.

## <a name="related-links"></a>관련 링크

- [Todo 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)
- [Xamarin.Forms 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)

