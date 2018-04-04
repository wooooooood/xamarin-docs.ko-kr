---
title: 로컬 데이터베이스
description: Xamarin.Forms는 로드 하 고 공유 코드에 개체를 저장 가능 하 게 하는 SQLite 데이터베이스 엔진을 사용 하 여 데이터베이스 기반 응용 프로그램을 지원 합니다. 이 문서에서는 어떻게 Xamarin.Forms 응용 프로그램 읽고 쓸 수 데이터를 로컬 SQLite 데이터베이스 SQLite.Net를 사용 하 여 설명 합니다.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2017
ms.openlocfilehash: 95c5f482e1bf3e55fa4c6fef18b1dbe6274f33e8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="local-databases"></a>로컬 데이터베이스

_Xamarin.Forms는 로드 하 고 공유 코드에 개체를 저장 가능 하 게 하는 SQLite 데이터베이스 엔진을 사용 하 여 데이터베이스 기반 응용 프로그램을 지원 합니다. 이 문서에서는 어떻게 Xamarin.Forms 응용 프로그램 읽고 쓸 수 데이터를 로컬 SQLite 데이터베이스 SQLite.Net를 사용 하 여 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Forms 응용 프로그램이 사용할 수는 [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) 데이터베이스 작업을 통합 하는 패키지를 참조 하 여 코드를 공유는 `SQLite` NuGet에 함께 제공 되는 클래스입니다. 데이터베이스 작업에 데이터베이스를 저장할 경로 반환 하는 플랫폼별 프로젝트를 사용 하 여 Xamarin.Forms 솔루션의 클래스 라이브러리 PCL (이식 가능한) 프로젝트에 정의할 수 있습니다.

함께 제공 되 [샘플 응용 프로그램](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) 는 간단한 Todo 목록 응용 프로그램입니다. 다음 스크린샷에서 샘플 각 플랫폼에 어떻게 표시 되는지를 보여 줍니다.

[![Xamarin.Forms 데이터베이스 스크린 샷입니다](databases-images/todo-list-sml.png "TodoList 첫 번째 페이지 스크린샷")](databases-images/todo-list.png#lightbox "TodoList 첫 번째 페이지 스크린샷") [ ![ Xamarin.Forms 데이터베이스 스크린 샷입니다](databases-images/todo-list-sml.png "TodoList 첫 번째 페이지 스크린샷")](databases-images/todo-list.png#lightbox "TodoList 첫 번째 페이지 스크린샷")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>SQLite를 사용 하 여

이 섹션에서는 SQLite.Net NuGet는 Xamarin.Forms 솔루션 패키지를 추가, 데이터베이스 작업을 수행 하 여 사용할 메서드를 작성 하는 방법을 보여 줍니다.는 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) 각 플랫폼에 데이터베이스를 저장할 위치를 결정 합니다.

<a name="XamarinForms_PCL_Project" />

### <a name="xamarinsforms-pcl-project"></a>Xamarins.Forms PCL Project

SQLite 지원 Xamarin.Forms PCL 프로젝트를 추가 하려면 사용 하 여 NuGet의 검색 기능 찾으려고 **sqlite net pcl** 패키지를 설치 하 고:

![](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet SQLite.NET PCL 패키지 추가")

NuGet 패키지는 이름이 비슷한 여러 가지, 올바른 패키지에 이러한 특성:

- **만든:** Frank A. Krueger
- **Id:** sqlite-net-pcl
- **NuGet link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

참조가 추가 되 면 데이터베이스 파일의 위치를 확인 하는 플랫폼 특정 기능을 추상화 하는 인터페이스를 작성 합니다. 샘플에 사용 되는 인터페이스에는 하나의 메서드를 정의 합니다.

```csharp
public interface IFileHelper
{
  string GetLocalFilePath(string filename);
}
```

사용 하 여 인터페이스 정의 되 면는 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) 로컬 파일 경로 (참고는이 인터페이스는 아직 구현 되지)를 구현을 가져옵니다. 다음 코드의 구현을 가져옵니다는 `App.Database` 속성:

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(DependencyService.Get<IFileHelper>().GetLocalFilePath("TodoSQLite.db3"));
    }
    return database;
  }
}
```

`TodoItemDatabase` 생성자는 다음과 같습니다.

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

이 방법은 응용 프로그램이 실행 되지 않도록 열기 및 데이터베이스 작업이 수행 될 때마다 데이터베이스 파일을 닫는 동안 열린 상태로 유지 되는 단일 데이터베이스 연결을 만듭니다.

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

모든는 모든 플랫폼에서 공유를 PCL 프로젝트에 데이터 액세스 코드를 작성 합니다. 데이터베이스에 대 한 로컬 파일 경로 가져오는 다음 섹션에 설명 된 대로 플랫폼별 코드를 필요 합니다.

<a name="PCL_iOS" />

### <a name="ios-project"></a>iOS 프로젝트

IOS 응용 프로그램을 구성 하려면 같은 NuGet 패키지를 사용 하 여 iOS 프로젝트 추가 *NuGet* 창:

![](databases-images/vsmac-sqlite-nuget.png "NuGet SQLite.NET PCL 패키지 추가")

필요한 유일한 코드는는 `IFileHelper` 데이터 파일 경로 결정 하는 구현 합니다. 다음 코드에서 SQLite 데이터베이스 파일을 배치는 **라이브러리/데이터베이스** 샌드박스 응용 프로그램의 내 폴더. 참조는 [iOS 파일 시스템 작업](~/ios/app-fundamentals/file-system.md) 저장소에 사용할 수 있는 다른 디렉터리에 대 한 자세한 내용은 설명서입니다.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.iOS
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            string docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
            string libFolder = Path.Combine(docFolder, "..", "Library", "Databases");

            if (!Directory.Exists(libFolder))
            {
                Directory.CreateDirectory(libFolder);
            }

            return Path.Combine(libFolder, filename);
        }
    }
}
```

코드를 포함 하는 `assembly:Dependency` 이 구현 여부를 검색할 수 있도록 특성은 `DependencyService`합니다.

<a name="PCL_Android" />

### <a name="android-project"></a>Android 프로젝트

Android 응용 프로그램을 구성 하려면 같은 NuGet 패키지를 사용 하 여 Android 프로젝트에 추가 된 *NuGet* 창:

![](databases-images/vsmac-sqlite-nuget.png "NuGet SQLite.NET PCL 패키지 추가")

이 참조를 추가한 후에 필요한 유일한 코드를는 `IFileHelper` 데이터 파일 경로 결정 하는 구현 합니다.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.Droid
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            string path = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
            return Path.Combine(path, filename);
        }
    }
}
```

<a name="PCL_UWP" />

### <a name="windows-10-universal-windows-platform-uwp"></a>Windows 10 UWP(유니버설 Windows 플랫폼)

UWP 응용 프로그램을 구성 하려면 사용 하 여 UWP 프로젝트를 같은 NuGet 패키지에 추가 된 *NuGet* 창:

![](databases-images/vs2017-sqlite-uwp-nuget.png "NuGet SQLite.NET PCL 패키지 추가")

참조가 추가 되 면 구현는 `IFileHelper` 플랫폼 관련을 사용 하 여 인터페이스 `Windows.Storage` 데이터 파일 경로 확인 하는 API입니다.

```csharp
using Windows.Storage;
...

[assembly: Dependency(typeof(FileHelper))]
namespace Todo.UWP
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            return Path.Combine(ApplicationData.Current.LocalFolder.Path, filename);
        }
    }
}

```

## <a name="summary"></a>요약

Xamarin.Forms는 로드 하 고 공유 코드에 개체를 저장 가능 하 게 하는 SQLite 데이터베이스 엔진을 사용 하 여 데이터베이스 기반 응용 프로그램을 지원 합니다.

이 문서에 초점을 맞춘 **액세스** Xamarin.Forms를 사용 하 여 SQLite 데이터베이스입니다. 작업 SQLite.Net 자체에 대 한 자세한 내용은 참조는 [데이터 액세스:를 사용 하 여 SQLite.NET](~/cross-platform/app-fundamentals/index.md) 설명서입니다. 모든 플랫폼; 대부분 SQLite.Net 코드는 공유 가능 만 SQLite 데이터베이스 파일의 위치를 구성 하려면 플랫폼별 기능 합니다.


## <a name="related-links"></a>관련 링크

- [Todo 샘플](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [데이터베이스 통합 문서](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
