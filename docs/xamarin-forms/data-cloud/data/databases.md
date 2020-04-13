---
title: Xamarin.Forms 로컬 데이터베이스
description: Xamarin.Forms는 SQLite 데이터베이스 엔진을 사용하여 데이터베이스 기반 애플리케이션을 지원하기 때문에 공유 코드로 개체를 로드하고 저장할 수 있습니다. 이 문서에서는 Xamarin.Forms 애플리케이션이 SQLite.Net을 사용하여 데이터를 읽고 로컬 SQLite 데이터베이스에 데이터를 기록하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 12/05/2019
ms.openlocfilehash: 2369afc693940d83971a43877da363e2c66b31b2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80992395"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms 로컬 데이터베이스

[![샘플](~/media/shared/download.png) 다운로드 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

SQLite 데이터베이스 엔진을 사용하면 Xamarin.Forms 응용 프로그램이 공유 코드에서 데이터 개체를 로드하고 저장할 수 있습니다. 샘플 응용 프로그램은 SQLite 데이터베이스 테이블을 사용하여 할 일 항목을 저장합니다. 이 문서에서는 공유 코드의 SQLite.Net 사용하여 로컬 데이터베이스에 정보를 저장하고 검색하는 방법을 설명합니다.

[![iOS 및 안드로이드에서 Todolist 앱의 스크린 샷](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "iOS 및 안드로이드에 대한 Todolist 앱")

다음 단계에 따라 SQLite.NET 모바일 앱에 통합합니다.

1. [NuGet 패키지를 설치합니다.](#install-the-sqlite-nuget-package)
1. [상수를 구성합니다.](#configure-app-constants)
1. [데이터베이스 액세스 클래스를 만듭니다.](#create-a-database-access-class)
1. [Xamarin.Forms .](#access-data-in-xamarinforms)
1. [고급 구성](#advanced-configuration).

## <a name="install-the-sqlite-nuget-package"></a>SQLite NuGet 패키지 설치

NuGet 패키지 관리자를 사용하여 **sqlite-net-pcl을** 검색하고 공유 코드 프로젝트에 최신 버전을 추가합니다.

이름이 유사한 NuGet 패키지가 여러 개 있습니다. 올바른 패키지에는 이러한 특성이 있습니다.

- **만든 사람:** Frank A. Krueger
- **ID:** sqlite-그물 pcl
- **NuGet 링크:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> 패키지 이름에도 불구하고 .NET 표준 프로젝트에서도 **sqlite-net-pcl** NuGet 패키지를 사용합니다.

## <a name="configure-app-constants"></a>앱 상수 구성

샘플 프로젝트에는 공통 구성 데이터를 제공하는 **Constants.cs** 파일이 포함되어 있습니다.

```csharp
public static class Constants
{
    public const string DatabaseFilename = "TodoSQLite.db3";

    public const SQLite.SQLiteOpenFlags Flags =
        // open the database in read/write mode
        SQLite.SQLiteOpenFlags.ReadWrite |
        // create the database if it doesn't exist
        SQLite.SQLiteOpenFlags.Create |
        // enable multi-threaded database access
        SQLite.SQLiteOpenFlags.SharedCache;

    public static string DatabasePath
    {
        get
        {
            var basePath = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
            return Path.Combine(basePath, DatabaseFilename);
        }
    }
}
```

상수 파일은 데이터베이스 `SQLiteOpenFlag` 연결을 초기화하는 데 사용되는 기본 열거형 값을 지정합니다. 열거형은 `SQLiteOpenFlag` 다음 값을 지원합니다.

- `Create`: 데이터베이스 파일이 없으면 연결이 자동으로 데이터베이스 파일을 만듭니다.
- `FullMutex`: 연결이 직렬화된 스레딩 모드에서 열립니다.
- `NoMutex`: 멀티 스레딩 모드에서 연결이 열립니다.
- `PrivateCache`: 연결이 활성화되어 있더라도 공유 캐시에 참여하지 않습니다.
- `ReadWrite`: 연결이 데이터를 읽고 쓸 수 있습니다.
- `SharedCache`: 연결이 활성화된 경우 공유 캐시에 참여합니다.
- `ProtectionComplete`: 장치가 잠겨 있는 동안 파일이 암호화되어 액세스할 수 없습니다.
- `ProtectionCompleteUnlessOpen`: 파일이 열릴 때까지 암호화되지만 사용자가 장치를 잠그더라도 액세스할 수 있습니다.
- `ProtectionCompleteUntilFirstUserAuthentication`: 사용자가 장치를 부팅하고 잠금을 해제할 때까지 파일이 암호화됩니다.
- `ProtectionNone`: 데이터베이스 파일이 암호화되지 않습니다.

데이터베이스사용 방식에 따라 다른 플래그를 지정해야 할 수 있습니다. 자세한 `SQLiteOpenFlags`내용은 sqlite.org [새 데이터베이스 연결 열기를](https://www.sqlite.org/c3ref/open.html) 참조하십시오.

## <a name="create-a-database-access-class"></a>데이터베이스 액세스 클래스 만들기

데이터베이스 래퍼 클래스는 앱의 나머지 부분에서 데이터 액세스 계층을 추상화합니다. 이 클래스는 쿼리 논리를 중앙 집중화하고 데이터베이스 초기화 관리를 단순화하여 앱이 증가함에 따라 데이터 작업을 리팩터링하거나 확장하는 것이 더 쉽습니다. Todo 앱은 이 `TodoItemDatabase` 목적을 위해 클래스를 정의합니다.

### <a name="lazy-initialization"></a>초기화 지연

.NET `TodoItemDatabase` `Lazy` 클래스를 사용하여 데이터베이스가 처음 액세스할 때까지 데이터베이스의 초기화를 지연시킵니다. 지연 초기화를 사용하면 데이터베이스 로드 프로세스가 앱 시작이 지연되지 않습니다. 자세한 내용은 [지연&lt;T&gt; 클래스](xref:System.Lazy`1)를 참조하십시오.

```csharp
public class TodoItemDatabase
{
    static readonly Lazy<SQLiteAsyncConnection> lazyInitializer = new Lazy<SQLiteAsyncConnection>(() =>
    {
        return new SQLiteAsyncConnection(Constants.DatabasePath, Constants.Flags);
    });

    static SQLiteAsyncConnection Database => lazyInitializer.Value;
    static bool initialized = false;

    public TodoItemDatabase()
    {
        InitializeAsync().SafeFireAndForget(false);
    }

    async Task InitializeAsync()
    {
        if (!initialized)
        {
            if (!Database.TableMappings.Any(m => m.MappedType.Name == typeof(TodoItem).Name))
            {
                await Database.CreateTablesAsync(CreateFlags.None, typeof(TodoItem)).ConfigureAwait(false);
                initialized = true;
            }
        }
    }

    //...
}
```

데이터베이스 연결은 앱의 수명 동안 단일 데이터베이스 연결이 사용되는 정적 필드입니다. 지속적이고 정적 연결을 사용하면 단일 앱 세션 동안 연결을 여러 번 열고 닫는 것보다 더 나은 성능을 제공합니다.

메서드는 `InitializeAsync` 개체를 저장하기 `TodoItem` 위해 테이블이 이미 있는지 확인합니다. 이 메서드가 존재하지 않는 경우 테이블을 자동으로 만듭니다.

### <a name="the-safefireandforget-extension-method"></a>세이프파이어앤포겟 익스텐션 방법

클래스가 `TodoItemDatabase` 인스턴스화되면 비동기 프로세스인 데이터베이스 연결을 초기화해야 합니다. 단,

- 클래스 생성자는 비동기일 수 없습니다.
- 기다려지지 않는 비동기 메서드는 예외를 throw하지 않습니다.
- 메서드를 `Wait` 사용하면 스레드가 _차단되고_ 예외가 삼켜집니다.

비동기 초기화를 시작하고 실행을 차단하지 않고 예외를 catch할 수 있는 기회를 얻으려면 샘플 응용 `SafeFireAndForget`프로그램에서 는 에서 확장 메서드를 사용합니다. 확장 `SafeFireAndForget` 메서드는 `Task` 클래스에 추가 기능을 제공합니다.

```csharp
public static class TaskExtensions
{
    // NOTE: Async void is intentional here. This provides a way
    // to call an async method from the constructor while
    // communicating intent to fire and forget, and allow
    // handling of exceptions
    public static async void SafeFireAndForget(this Task task,
        bool returnToCallingContext,
        Action<Exception> onException = null)
    {
        try
        {
            await task.ConfigureAwait(returnToCallingContext);
        }

        // if the provided action is not null, catch and
        // pass the thrown exception
        catch (Exception ex) when (onException != null)
        {
            onException(ex);
        }
    }
}
```

메서드는 `SafeFireAndForget` 제공된 `Task` 개체의 비동기 실행을 기다리고 예외가 throw된 경우 `Action` 호출되는 호출된 을 연결할 수 있습니다.

자세한 내용은 [작업 기반 비동기 패턴(TAP)을](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)참조하십시오.

### <a name="data-manipulation-methods"></a>데이터 조작 방법

클래스에는 `TodoItemDatabase` 생성, 읽기, 편집 및 삭제의 네 가지 유형의 데이터 조작에 대한 메서드가 포함되어 있습니다. SQLite.NET 라이브러리는 SQL 문을 작성하지 않고 개체를 저장하고 검색할 수 있는 간단한 ORM(개체 관계도 맵)을 제공합니다.

```csharp
public class TodoItemDatabase {

    // ...

    public Task<List<TodoItem>> GetItemsAsync()
    {
        return Database.Table<TodoItem>().ToListAsync();
    }

    public Task<List<TodoItem>> GetItemsNotDoneAsync()
    {
        // SQL queries are also possible
        return Database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
    }

    public Task<TodoItem> GetItemAsync(int id)
    {
        return Database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
    }

    public Task<int> SaveItemAsync(TodoItem item)
    {
        if (item.ID != 0)
        {
            return Database.UpdateAsync(item);
        }
        else
        {
            return Database.InsertAsync(item);
        }
    }

    public Task<int> DeleteItemAsync(TodoItem item)
    {
        return Database.DeleteAsync(item);
    }
}
```

## <a name="access-data-in-xamarinforms"></a>Xamarin.Forms의 데이터에 액세스

Xamarin.Forms `App` 클래스는 클래스의 인스턴스를 `TodoItemDatabase` 노출합니다.

```csharp
static TodoItemDatabase database;
public static TodoItemDatabase Database
{
    get
    {
        if (database == null)
        {
            database = new TodoItemDatabase();
        }
        return database;
    }
}
```

이 속성을 사용 하려면 Xamarin.Forms 구성 요소는 사용자 `Database` 상호 작용에 대 한 응답으로 인스턴스에서 데이터 검색 및 조작 메서드를 호출할 수 있습니다. 다음은 그 예입니다.

```csharp
var saveButton = new Button { Text = "Save" };
saveButton.Clicked += async (sender, e) =>
{
    var todoItem = (TodoItem)BindingContext;
    await App.Database.SaveItemAsync(todoItem);
    await Navigation.PopAsync();
};
```

## <a name="advanced-configuration"></a>고급 구성

SQLite는 이 문서와 샘플 앱에서 다루는 것보다 더 많은 기능을 갖춘 강력한 API를 제공합니다. 다음 섹션에서는 확장성에 중요한 기능을 다룹니다.

자세한 내용은 sqlite.org [SQLite 문서를](https://www.sqlite.org/docs.html) 참조하십시오.

### <a name="write-ahead-logging"></a>미리 쓰기 로깅

기본적으로 SQLite는 기존 롤백 저널을 사용합니다. 변경되지 않은 데이터베이스 콘텐츠의 복사본은 별도의 롤백 파일에 기록된 다음 변경 내용을 데이터베이스 파일에 직접 기록합니다. COMMIT는 롤백 저널이 삭제될 때 발생합니다.

WAL(미리 쓰기)은 먼저 별도의 WAL 파일에 변경 내용을 씁니다. WAL 모드에서 COMMIT는 WAL 파일에 추가된 특수 레코드로, 단일 WAL 파일에서 여러 트랜잭션이 발생할 수 있습니다. WAL 파일은 _검사점이라는_특수 작업에서 데이터베이스 파일로 다시 병합됩니다.

판독기와 작성기가 서로 를 차단하지 않으므로 읽기 및 쓰기 작업이 동시에 수행되므로 로컬 데이터베이스의 경우 WAL이 더 빠를 수 있습니다. 그러나 WAL 모드는 _페이지 크기를_변경할 수 없으며 데이터베이스에 추가 파일 연결을 추가하고 추가 검사점 작업을 _추가합니다._

SQLite.NET WAL을 사용하려면 `EnableWriteAheadLoggingAsync` 인스턴스에서 `SQLiteAsyncConnection` 메서드를 호출합니다.

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

자세한 내용은 sqlite.org [SQLite 쓰기 미리 로깅을](https://www.sqlite.org/wal.html) 참조하십시오.

### <a name="copying-a-database"></a>데이터베이스 복사

SQLite 데이터베이스를 복사해야 하는 경우는 다음과 같습니다.

- 데이터베이스가 응용 프로그램과 함께 제공되었지만 모바일 장치에서 쓰기 가능한 저장소로 복사하거나 이동해야 합니다.
- 데이터베이스의 백업 또는 복사본을 만들어야 합니다.
- 데이터베이스 파일의 버전을 지정, 이동 또는 이름을 바여야 합니다.

일반적으로 데이터베이스 파일을 이동, 이름 바꾸기 또는 복사하는 것은 다음과 같은 몇 가지 추가 고려 사항이 있는 다른 파일 형식과 동일한 프로세스입니다.

- 데이터베이스 파일을 이동하기 전에 모든 데이터베이스 연결을 닫아야 합니다.
- [미리 쓰기 로깅을](#write-ahead-logging)사용하는 경우 SQLite는 공유 메모리 액세스(.shm) 파일과 (미리 쓰기 로그) (.wal) 파일을 만듭니다. 이러한 파일에 도 변경 내용을 적용 해야 합니다.

자세한 내용은 [Xamarin.Forms의 파일 처리를](~/xamarin-forms/data-cloud/data/files.md)참조하십시오.

## <a name="related-links"></a>관련 링크

- [토도 샘플 응용 프로그램](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)
- [SQLite.NET NuGet 패키지](https://www.nuget.org/packages/sqlite-net-pcl/)
- [SQLite 문서](https://www.sqlite.org/docs.html)
- [안드로이드와 SQLite를 사용하여](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [iOS와 함께 SQLite 사용](~/ios/data-cloud/data/using-sqlite-orm.md)
- [작업 기반 비동기 패턴(TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [게으른&lt;&gt; T 클래스](xref:System.Lazy`1)
