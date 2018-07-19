---
title: Xamarin.Forms 로컬 데이터베이스
description: Xamarin.Forms는 로드 하 고 공유 코드에 개체를 저장 가능 하 게 하는 SQLite 데이터베이스 엔진을 사용 하 여 데이터베이스 기반 응용 프로그램을 지원 합니다. 이 문서에서는 어떻게 Xamarin.Forms 응용 프로그램 읽고 쓸 수 데이터를 로컬 SQLite 데이터베이스 SQLite.Net를 사용 하 여 설명 합니다.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: feec4993a0719a083d713e084552b18aead8ee42
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310142"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms 로컬 데이터베이스

_Xamarin.Forms는 로드 하 고 공유 코드에 개체를 저장 가능 하 게 하는 SQLite 데이터베이스 엔진을 사용 하 여 데이터베이스 기반 응용 프로그램을 지원 합니다. 이 문서에서는 어떻게 Xamarin.Forms 응용 프로그램 읽고 쓸 수 데이터를 로컬 SQLite 데이터베이스 SQLite.Net를 사용 하 여 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Forms 응용 프로그램이 사용할 수는 [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) 데이터베이스 작업을 통합 하는 패키지를 참조 하 여 코드를 공유는 `SQLite` NuGet에 함께 제공 되는 클래스입니다. Xamarin.Forms 솔루션의 표준.NET 라이브러리 프로젝트에서 데이터베이스 작업을 정의할 수 있습니다.

함께 제공 되 [샘플 응용 프로그램](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) 는 간단한 Todo 목록 응용 프로그램입니다. 다음 스크린샷에서 샘플 각 플랫폼에 어떻게 표시 되는지를 보여 줍니다.

[![Xamarin.Forms 데이터베이스 스크린 샷입니다](databases-images/todo-list-sml.png "TodoList 첫 번째 페이지 스크린샷")](databases-images/todo-list.png#lightbox "TodoList 첫 번째 페이지 스크린샷") [ ![ Xamarin.Forms 데이터베이스 스크린 샷입니다](databases-images/todo-list-sml.png "TodoList 첫 번째 페이지 스크린샷")](databases-images/todo-list.png#lightbox "TodoList 첫 번째 페이지 스크린샷")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>SQLite를 사용 하 여

SQLite 지원 Xamarin.Forms.NET 표준 라이브러리를 추가 하려면 사용 하 여 NuGet의 검색 기능 찾으려고 **sqlite net pcl** 최신 패키지를 설치 하 고:

![NuGet SQLite.NET PCL 패키지 추가](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet SQLite.NET PCL 패키지 추가")

NuGet 패키지는 이름이 비슷한 여러 가지, 올바른 패키지에 이러한 특성:

- **만든:** Frank A. Krueger
- **Id:** sqlite net pcl
- **NuGet 링크:** [sqlite net pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> 패키지 이름에도 불구 하 고 사용 하 여는 **sqlite net pcl** 표준.NET 프로젝트에도 NuGet 패키지 합니다.

참조가 추가 되 면 속성을 추가 하 고 `App` 데이터베이스를 저장 하기 위한 로컬 파일 경로 반환 하는 클래스:

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

`TodoItemDatabase` 생성자 인수로 데이터베이스 파일에 대 한 경로 사용 하는 다음과 같습니다.

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

응용 프로그램 하는 동안 열린 상태로 유지 되는 단일 항목은 단일 데이터베이스 연결이 만들어지면 데이터베이스에 노출 될 이점은, 되지 않도록 지출 될 때마다 데이터베이스 파일을 위에서 데이터베이스 작업을 수행 합니다.

나머지 부분에서는 `TodoItemDatabase` 클래스 플랫폼을 실행 하는 SQLite 쿼리를 포함 합니다. 예제 쿼리 코드는 다음과 같습니다 (구문에 대 한 자세한 내용은에서 확인할 수 있습니다는 [를 사용 하 여 SQLite.NET](~/cross-platform/app-fundamentals/index.md) 문서):

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
> 비동기 SQLite.Net API 사용 시의 이점은 작업을 백그라운드 스레드로 이동 해당 데이터베이스입니다. 또한 추가 동시성 처리 API의 처리를 사용 하기 때문에 코드를 작성할 필요가 없습니다 있습니다.

## <a name="summary"></a>요약

Xamarin.Forms는 로드 하 고 공유 코드에 개체를 저장 가능 하 게 하는 SQLite 데이터베이스 엔진을 사용 하 여 데이터베이스 기반 응용 프로그램을 지원 합니다.

이 문서에 초점을 맞춘 **액세스** Xamarin.Forms를 사용 하 여 SQLite 데이터베이스입니다. 작업 SQLite.Net 자체에 대 한 자세한 내용은 참조는 [SQLite.NET Android에서](~/android/data-cloud/data-access/using-sqlite-orm.md) 또는 [SQLite.NET iOS에서](~/ios/data-cloud/data/using-sqlite-orm.md) 설명서입니다.

## <a name="related-links"></a>관련 링크

- [Todo 샘플](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [데이터베이스 통합 문서](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
