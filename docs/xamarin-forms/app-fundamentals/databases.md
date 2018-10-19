---
title: Xamarin.Forms 로컬 데이터베이스
description: Xamarin.Forms는 로드 하 고 공유 코드에서 개체를 저장할 수 있도록 하는 SQLite 데이터베이스 엔진을 사용 하 여 데이터베이스 기반 응용 프로그램을 지원 합니다. 이 문서에서는 어떻게 Xamarin.Forms 응용 프로그램 수 데이터 읽기 및 쓰기 SQLite.Net를 사용 하 여 로컬 SQLite 데이터베이스를 설명 합니다.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 05c77c4c47841a897d0d1de5c3ba004db524ea2d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2018
ms.locfileid: "36310142"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms 로컬 데이터베이스

_Xamarin.Forms는 로드 하 고 공유 코드에서 개체를 저장할 수 있도록 하는 SQLite 데이터베이스 엔진을 사용 하 여 데이터베이스 기반 응용 프로그램을 지원 합니다. 이 문서에서는 어떻게 Xamarin.Forms 응용 프로그램 수 데이터 읽기 및 쓰기 SQLite.Net를 사용 하 여 로컬 SQLite 데이터베이스를 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Forms 응용 프로그램에서 사용할 수는 [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) 데이터베이스 작업을 통합 하는 패키지를 참조 하 여 코드를 공유 합니다 `SQLite` NuGet에서 제공 되는 클래스입니다. Xamarin.Forms 솔루션의.NET Standard 라이브러리 프로젝트에서 데이터베이스 작업을 정의할 수 있습니다.

와 함께 [샘플 응용 프로그램](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) 는 간단한 할 일 목록 응용 프로그램입니다. 다음 스크린샷에서 각 플랫폼에서 샘플을 표시 하는 방법을 보여 줍니다.

[![Xamarin.Forms 데이터베이스 예제 스크린샷](databases-images/todo-list-sml.png "TodoList 첫 번째 페이지 스크린샷")](databases-images/todo-list.png#lightbox "TodoList 첫 번째 페이지 스크린샷") [ ![ Xamarin.Forms 데이터베이스 예제 스크린샷](databases-images/todo-list-sml.png "TodoList 첫 번째 페이지 스크린샷")](databases-images/todo-list.png#lightbox "TodoList 첫 번째 페이지 스크린샷")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>SQLite 사용

Xamarin.Forms.NET Standard 라이브러리에 SQLite 지원을 추가 하려면 사용 하 여 NuGet의 검색 기능 찾으려고 **sqlite-net-pcl** 최신 패키지를 설치 하 고:

![NuGet SQLite.NET PCL 패키지 추가](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet SQLite.NET PCL 패키지 추가")

비슷한 이름의 NuGet 패키지의 여러 가지, 이러한 특성이 올바른 패키지:

- **만든:** Frank A. Krueger
- **Id:** sqlite-net-pcl
- **NuGet 링크:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> 패키지 이름에도 불구 하 고 사용 합니다 **sqlite-net-pcl** .NET Standard 프로젝트에도 NuGet 패키지.

참조를 추가한 후 속성을 추가 하 여 `App` 데이터베이스 저장 로컬 파일 경로 반환 하는 클래스:

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

`TodoItemDatabase` 생성자 인수로 서 데이터베이스 파일의 경로 사용 하는 다음과 같습니다.

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

이점은 단일 단일 데이터베이스의 연결이 생성 되는 데이터베이스를 노출 하는 동안 응용 프로그램 열기 유지 되는 실행, 수행 되므로 데이터베이스 작업을 열고 될 때마다 데이터베이스 파일을 닫는의 비용을 방지 합니다.

나머지는 `TodoItemDatabase` 클래스 플랫폼 간 실행 되는 SQLite 쿼리를 포함 합니다. 예제 쿼리 코드는 다음과 같습니다 (구문에 대 한 자세한 내용은에서 찾을 수 있습니다 [Xamarin.iOS와 함께 사용 하 여 SQLite.NET](~/ios/data-cloud/data/using-sqlite-orm.md)합니다.

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
> 비동기 SQLite.Net API를 사용 하 여의 장점은 작업을 백그라운드 스레드로 이동 하는 데이터베이스입니다. 또한 동시성 처리의 API를 담당 하기 때문에 코드를 작성할 필요가 없습니다 있습니다.

## <a name="summary"></a>요약

Xamarin.Forms는 로드 하 고 공유 코드에서 개체를 저장할 수 있도록 하는 SQLite 데이터베이스 엔진을 사용 하 여 데이터베이스 기반 응용 프로그램을 지원 합니다.

이 문서에 초점을 맞춘 **액세스** Xamarin.Forms를 사용 하 여 SQLite 데이터베이스입니다. SQLite.Net 자체를 사용 하 여 작업에 대 한 자세한 내용은 참조는 [Android에서 SQLite.NET](~/android/data-cloud/data-access/using-sqlite-orm.md) 또는 [iOS에서 SQLite.NET](~/ios/data-cloud/data/using-sqlite-orm.md) 설명서.

## <a name="related-links"></a>관련 링크

- [할 일 샘플](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)

