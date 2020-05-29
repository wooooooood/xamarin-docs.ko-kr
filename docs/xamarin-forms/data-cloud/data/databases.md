---
title: Xamarin.Forms로컬 데이터베이스
description: Xamarin.Forms는 SQLite 데이터베이스 엔진을 사용 하 여 데이터베이스 기반 응용 프로그램을 지원 하므로 공유 코드에서 개체를 로드 하 고 저장할 수 있습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램이 SQLite.Net를 사용 하 여 로컬 SQLite 데이터베이스에서 데이터를 읽고 쓰는 방법을 설명 합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 04d813baae5796da68ea27389df33738af5cde3e
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84131003"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms로컬 데이터베이스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

SQLite 데이터베이스 엔진을 사용 하면 Xamarin.Forms 응용 프로그램에서 공유 코드에 데이터 개체를 로드 하 고 저장할 수 있습니다. 예제 응용 프로그램은 SQLite 데이터베이스 테이블을 사용 하 여 할 일 항목을 저장 합니다. 이 문서에서는 공유 코드에서 SQLite.Net를 사용 하 여 로컬 데이터베이스에 정보를 저장 하 고 검색 하는 방법을 설명 합니다.

[![IOS 및 Android에서 Todolist 앱의 스크린샷](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "IOS 및 Android의 Todolist 앱")

다음 단계를 수행 하 여 모바일 앱에 SQLite.NET를 통합 합니다.

1. [NuGet 패키지를 설치](#install-the-sqlite-nuget-package)합니다.
1. [상수를 구성](#configure-app-constants)합니다.
1. [데이터베이스 액세스 클래스를 만듭니다](#create-a-database-access-class).
1. [에서 데이터에 Xamarin.Forms 액세스 ](#access-data-in-xamarinforms)합니다.
1. [고급 구성](#advanced-configuration).

## <a name="install-the-sqlite-nuget-package"></a>SQLite NuGet 패키지 설치

NuGet 패키지 관리자를 사용 하 여 **sqlite-net-library** 를 검색 하 고 공유 코드 프로젝트에 최신 버전을 추가 합니다.

이름이 유사한 NuGet 패키지가 여러 개 있습니다. 올바른 패키지에는 이러한 특성이 있습니다.

- **만든 사람:** Frank A. Krueger
- **ID:** sqlite-net-pcl
- **NuGet 링크:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> 패키지 이름에도 불구하고 .NET 표준 프로젝트에서도 **sqlite-net-pcl** NuGet 패키지를 사용합니다.

## <a name="configure-app-constants"></a>앱 상수 구성

샘플 프로젝트에는 일반적인 구성 데이터를 제공 하는 **Constants.cs** 파일이 포함 되어 있습니다.

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

상수 파일은 `SQLiteOpenFlag` 데이터베이스 연결을 초기화 하는 데 사용 되는 기본 열거형 값을 지정 합니다. `SQLiteOpenFlag`열거형은 다음 값을 지원 합니다.

- `Create`: 연결에서 데이터베이스 파일이 없는 경우 자동으로 만듭니다.
- `FullMutex`: 연결이 직렬화 된 스레딩 모드로 열립니다.
- `NoMutex`: 연결이 다중 스레딩 모드로 열립니다.
- `PrivateCache`: 사용 하도록 설정 된 경우에도 연결에서 공유 캐시에 참여 하지 않습니다.
- `ReadWrite`: 연결에서 데이터를 읽고 쓸 수 있습니다.
- `SharedCache`: 연결을 사용 하는 경우 공유 캐시에 참여 합니다.
- `ProtectionComplete`: 장치가 잠겨 있는 동안 파일이 암호화 되어 액세스할 수 없습니다.
- `ProtectionCompleteUnlessOpen`: 파일이 열릴 때까지 암호화 되지만 사용자가 장치를 잠근 경우에도 액세스할 수 있습니다.
- `ProtectionCompleteUntilFirstUserAuthentication`: 사용자가 장치를 부팅 하 고 잠금을 해제할 때까지 파일이 암호화 됩니다.
- `ProtectionNone`: 데이터베이스 파일이 암호화 되지 않았습니다.

데이터베이스가 사용 되는 방식에 따라 다른 플래그를 지정 해야 할 수 있습니다. 에 대 한 자세한 내용은 `SQLiteOpenFlags` sqlite.org에서 [새 데이터베이스 연결 열기](https://www.sqlite.org/c3ref/open.html) 를 참조 하세요.

## <a name="create-a-database-access-class"></a>데이터베이스 액세스 클래스 만들기

데이터베이스 래퍼 클래스는 응용 프로그램의 나머지 부분에서 데이터 액세스 계층을 추상화 합니다. 이 클래스는 쿼리 논리를 중앙화 하 고 데이터베이스 초기화의 관리를 간소화 하 여 앱이 증가 함에 따라 데이터 작업을 쉽게 리팩터링 또는 확장할 수 있도록 합니다. Todo 앱은 `TodoItemDatabase` 이 용도로 클래스를 정의 합니다.

### <a name="lazy-initialization"></a>초기화 지연

는 `TodoItemDatabase` .net 클래스를 사용 하 여 `Lazy` 처음 액세스할 때까지 데이터베이스 초기화를 지연 합니다. 초기화 지연을 사용 하면 데이터베이스 로드 프로세스가 앱 시작을 지연 하지 않습니다. 자세한 내용은 [Lazy &lt; T &gt; 클래스](xref:System.Lazy`1)를 참조 하세요.

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

데이터베이스 연결은 응용 프로그램의 수명 동안 단일 데이터베이스 연결이 사용 되도록 하는 정적 필드입니다. 영구 정적 연결을 사용 하면 단일 앱 세션 중에 연결을 여러 번 열고 닫는 것 보다 성능이 향상 됩니다.

`InitializeAsync`메서드는 개체를 저장 하기 위한 테이블이 이미 존재 하는지 여부를 확인 하는 일을 담당 `TodoItem` 합니다. 이 메서드는 테이블이 없는 경우 자동으로 만듭니다.

### <a name="the-safefireandforget-extension-method"></a>SafeFireAndForget extension 메서드

`TodoItemDatabase`클래스가 인스턴스화되면 비동기 프로세스에 해당 하는 데이터베이스 연결을 초기화 해야 합니다. 단,

- 클래스 생성자는 비동기 일 수 없습니다.
- 대기 하지 않는 비동기 메서드는 예외를 throw 하지 않습니다.
- 메서드를 사용 하면 `Wait` 스레드가 차단 되 _고_ 숨깁니다 예외를 허용 합니다.

비동기 초기화를 시작 하기 위해 실행을 차단 하지 않고 예외를 catch 할 수 있는 기회를 갖도록 샘플 응용 프로그램은 라는 확장명 메서드를 사용 합니다 `SafeFireAndForget` . `SafeFireAndForget`확장 메서드는 클래스에 추가 기능을 제공 합니다 `Task` .

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

`SafeFireAndForget`메서드는 제공 된 개체의 비동기 실행을 기다립니다 하 `Task` 고, `Action` 예외가 throw 되 면 호출 되는를 연결할 수 있도록 합니다.

자세한 내용은 [작업 기반 비동기 패턴 (탭)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)을 참조 하세요.

### <a name="data-manipulation-methods"></a>데이터 조작 메서드

클래스에는 `TodoItemDatabase` 만들기, 읽기, 편집 및 삭제의 네 가지 데이터 조작 형식에 대 한 메서드가 포함 되어 있습니다. SQLite.NET 라이브러리는 SQL 문을 작성 하지 않고 개체를 저장 하 고 검색할 수 있는 간단한 ORM (개체 관계형 맵)을 제공 합니다.

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

## <a name="access-data-in-xamarinforms"></a>데이터 액세스Xamarin.Forms

Xamarin.Forms `App` 클래스는 클래스의 인스턴스를 노출 합니다 `TodoItemDatabase` .

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

이 속성을 사용 하면 Xamarin.Forms 구성 요소가 `Database` 사용자 상호 작용에 대 한 응답으로 인스턴스에서 데이터 검색 및 조작 메서드를 호출할 수 있습니다. 예를 들면 다음과 같습니다.

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

SQLite는이 문서 및 샘플 앱에 설명 된 것 보다 더 많은 기능을 갖춘 강력한 API를 제공 합니다. 다음 섹션에서는 확장성에 중요 한 기능을 다룹니다.

자세한 내용은 sqlite.org의 [SQLite 설명서](https://www.sqlite.org/docs.html) 를 참조 하세요.

### <a name="write-ahead-logging"></a>미리 쓰기 로깅

기본적으로 SQLite는 기존 롤백 저널을 사용 합니다. 변경 되지 않은 데이터베이스 콘텐츠의 복사본이 별도의 롤백 파일에 기록 된 후 변경 내용이 데이터베이스 파일에 직접 기록 됩니다. 롤백은 롤백 저널을 삭제할 때 발생 합니다.

WAL (미리 쓰기 로깅)은 먼저 별도의 WAL 파일에 변경 내용을 기록 합니다. WAL 모드에서 커밋은 WAL 파일에 추가 된 특수 레코드로, 단일 WAL 파일에서 여러 트랜잭션을 수행할 수 있습니다. WAL 파일은 _검사점_이라는 특수 한 작업으로 데이터베이스 파일에 다시 병합 됩니다.

읽기 및 쓰기 작업이 동시에 수행 될 수 있도록 판독기와 작성기가 서로를 차단 하지 않기 때문에 로컬 데이터베이스에 대 한 WAL 속도가 빨라질 수 있습니다. 그러나 WAL 모드는 _페이지 크기_를 변경할 수 없으며, 데이터베이스에 추가 파일 연결을 추가 하 고, 추가 _검사점_ 작업을 추가 합니다.

SQLite.NET에서 WAL을 사용 하도록 설정 하려면 `EnableWriteAheadLoggingAsync` 인스턴스에서 메서드를 호출 합니다 `SQLiteAsyncConnection` .

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

자세한 내용은 sqlite.org의 [SQLite 미리 쓰기 로깅](https://www.sqlite.org/wal.html) 을 참조 하세요.

### <a name="copying-a-database"></a>데이터베이스 복사

SQLite 데이터베이스를 복사 해야 하는 여러 가지 경우가 있습니다.

- 데이터베이스가 응용 프로그램과 함께 제공 되었지만 모바일 장치에서 쓰기 가능한 저장소로 복사 또는 이동 해야 합니다.
- 데이터베이스의 백업 또는 복사본을 만들어야 합니다.
- 데이터베이스 파일의 버전을 변경 하거나, 이동 하거나, 이름을 변경 해야 합니다.

일반적으로 데이터베이스 파일을 이동 하거나, 이름을 바꾸거나, 복사 하는 과정은 몇 가지 추가 고려 사항이 있는 다른 모든 파일 형식과 동일한 프로세스입니다.

- 데이터베이스 파일을 이동 하기 전에 모든 데이터베이스 연결을 닫아야 합니다.
- [미리 쓰기 로깅을](#write-ahead-logging)사용 하는 경우 SQLite는 공유 메모리 액세스 (.sm) 파일 및 (미리 쓰기 로그) (wal) 파일을 만듭니다. 이러한 파일에도 변경 내용을 적용 해야 합니다.

자세한 내용은 [ Xamarin.Forms 의 파일 처리 ](~/xamarin-forms/data-cloud/data/files.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Todo 샘플 응용 프로그램](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)
- [SQLite.NET NuGet 패키지](https://www.nuget.org/packages/sqlite-net-pcl/)
- [SQLite 설명서](https://www.sqlite.org/docs.html)
- [Android에서 SQLite 사용](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [IOS에서 SQLite 사용](~/ios/data-cloud/data/using-sqlite-orm.md)
- [작업 기반 비동기 패턴 (탭)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [Lazy &lt; T &gt; 클래스](xref:System.Lazy`1)
