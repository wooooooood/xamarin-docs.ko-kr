---
title: Android에서 SQLite.NET 사용
description: SQLite.NET PCL NuGet 라이브러리는 Xamarin Android 앱에 대 한 간단한 데이터 액세스 메커니즘을 제공 합니다.
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/18/2018
ms.openlocfilehash: 65c8466e2649c6d48cf5651f25d14c073dbcf5e3
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754426"
---
# <a name="using-sqlitenet-with-android"></a>Android에서 SQLite.NET 사용

Xamarin에서 권장 하는 SQLite.NET 라이브러리는 Android 장치의 로컬 SQLite 데이터베이스에서 개체를 쉽게 저장 하 고 검색할 수 있도록 하는 매우 기본적인 ORM입니다. ORM은 SQL 문을 작성 하지 &ndash; 않고도 데이터베이스에서 "개체"를 저장 하 고 검색할 수 있도록 하는 API의 개체 관계형 매핑을 나타냅니다.

Xamarin 앱에 SQLite.NET 라이브러리를 포함 하려면 다음 NuGet 패키지를 프로젝트에 추가 합니다.

- **패키지 이름:** sqlite-net-pcl
- **작성자:** Frank A. Krueger
- **ID:** sqlite-net-pcl
- **Url:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![SQLite.NET NuGet 패키지](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet 패키지")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> 사용할 수 있는 여러 SQLite 패키지가 있습니다. 올바른 항목을 선택 해야 합니다 (검색의 상위 결과가 아닐 수 있음).

SQLite.NET 라이브러리를 사용할 수 있게 되 면 다음 세 단계를 수행 하 여 데이터베이스에 액세스 합니다.

1. **Using 문 추가** 데이터 액세스가 필요한 C# 파일에 다음 문을 추가 합니다. &ndash;

    ```csharp
    using SQLite;
    ```

2. **빈 데이터베이스 만들기** &ndash; SQLiteConnection class 생성자의 파일 경로를 전달 하 여 데이터베이스 참조를 만들 수 있습니다. 파일이 이미 있는지 &ndash; 확인할 필요는 없습니다. 필요한 경우 자동으로 생성 되 고 그렇지 않으면 기존 데이터베이스 파일이 열립니다. 변수 `dbPath` 는이 문서의 앞부분에서 설명한 규칙에 따라 결정 해야 합니다.

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3. **데이터 저장** &ndash; SQLiteConnection 개체를 만든 후에는 CreateTable 등의 메서드를 호출 하 여 데이터베이스 명령을 실행 하 고 다음과 같이 삽입 합니다.

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4. **데이터 검색** &ndash; 개체 (또는 개체 목록)를 검색 하려면 다음 구문을 사용 합니다.

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>기본 데이터 액세스 샘플

이 문서에 대 한 *DataAccess_Basic* 샘플 코드는 Android에서 실행 하는 경우와 같습니다. 이 코드에서는 간단한 SQLite.NET 작업을 수행 하 고 결과를 응용 프로그램의 주 창에 텍스트로 표시 하는 방법을 보여 줍니다.

**Android**

![Android SQLite.NET 샘플](using-sqlite-orm-images/image3.png "Android SQLite.NET 샘플")

다음 코드 샘플에서는 SQLite.NET 라이브러리를 사용 하 여 기본 데이터베이스 액세스를 캡슐화 하는 전체 데이터베이스 상호 작용을 보여 줍니다.
다음을 보여 줍니다.

1. 데이터베이스 파일 만들기

2. 개체를 만든 다음 저장 하 여 일부 데이터 삽입

3. 데이터 쿼리

다음 네임 스페이스를 포함 해야 합니다.

```csharp
using SQLite; // from the github SQLite.cs class
```

마지막 항목을 사용 하려면 프로젝트에 SQLite를 추가 해야 합니다. SQLite 데이터베이스 테이블은 CREATE TABLE 명령 대신 클래스 ( `Stock` 클래스)에 특성을 추가 하 여 정의 됩니다.

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

테이블 이름 매개 변수를 지정 하지 않고 특성을사용하면기본데이터베이스테이블의이름이클래스(이경우"Stock")와동일하게됩니다.`[Table]` ORM 데이터 액세스 방법을 사용 하는 대신 데이터베이스에 대해 직접 SQL 쿼리를 작성 하는 경우에는 실제 테이블 이름이 중요 합니다. `[Column("_id")]` 마찬가지로 특성은 선택 사항이 며, 없는 경우에는 클래스의 속성과 이름이 같은 열이 테이블에 추가 됩니다.

## <a name="sqlite-attributes"></a>SQLite 특성

기본 데이터베이스에 저장 되는 방법을 제어 하기 위해 클래스에 적용할 수 있는 공통 특성은 다음과 같습니다.

- **[PrimaryKey]** &ndash; 이 특성을 정수 속성에 적용 하 여 기본 테이블의 기본 키로 지정할 수 있습니다. 복합 기본 키는 지원 되지 않습니다.

- **[AutoIncrement]** &ndash; 이 특성으로 인해 데이터베이스에 삽입 된 각 새 개체에 대해 정수 속성의 값이 자동으로 증가 합니다.

- **[열 (이름)]** &ndash; 선택적`name` 매개 변수를 제공 하면 기본 데이터베이스 열 이름 (속성과 동일)의 기본값이 재정의 됩니다.

- **[테이블 (이름)]** &ndash; 클래스를 기본 SQLite 테이블에 저장할 수 있는 것으로 표시 합니다. 선택적 name 매개 변수를 지정 하면 기본 데이터베이스 테이블 이름 (클래스 이름과 같음)의 기본값이 재정의 됩니다.

- **[MaxLength (값)]** &ndash; 데이터베이스 삽입을 시도할 때 텍스트 속성의 길이를 제한 합니다. 데이터베이스 삽입 또는 업데이트 작업을 시도 하는 경우이 특성이 ' 확인 ' 된 경우에만 코드를 사용 하 여이 작업의 유효성을 검사 해야 합니다.

- **[무시]** &ndash; SQLite.NET이이 속성을 무시 하도록 합니다.
    이는 데이터베이스에 저장할 수 없는 형식이 있는 속성 또는 SQLite에서 자동으로 해결할 수 없는 컬렉션을 모델링 하는 속성에 특히 유용 합니다.

- **[고유]** &ndash; 기본 데이터베이스 열의 값이 고유한 지 확인 합니다.

이러한 특성의 대부분은 선택 사항이 며, SQLite는 테이블 및 열 이름에 기본값을 사용 합니다. 데이터에 대해 선택 및 삭제 쿼리를 효율적으로 수행할 수 있도록 항상 정수 기본 키를 지정 해야 합니다.

## <a name="more-complex-queries"></a>더 복잡 한 쿼리

의 `SQLiteConnection` 다음 메서드를 사용 하 여 다른 데이터 작업을 수행할 수 있습니다.

- **삽입** &ndash; 데이터베이스에 새 개체를 추가 합니다.

- **기본 키를 사용 하 여 개체를 가져오려고 시도 합니다.&lt;&gt;**  &ndash;

- **테이블&lt;T&gt;는테이블 의 모든** 개체를반환합니다&ndash; .

- **삭제** &ndash; 기본 키를 사용 하 여 개체를 삭제 합니다.

- **쿼리&lt;T&gt;는여러 행을** 개체로반환하는SQL쿼리를수행합니다.&ndash;

- **실행** SQL에서 행을 반환 하지 `Query`않을 경우 (예: INSERT, UPDATE 및 DELETE 명령)이 아닌이 메서드를 사용 합니다. &ndash;

### <a name="getting-an-object-by-the-primary-key"></a>기본 키로 개체 가져오기

SQLite.Net는 기본 키를 기반으로 단일 개체를 검색 하는 Get 메서드를 제공 합니다.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Linq를 사용 하 여 개체 선택

Linq를 사용 하 여 `IEnumerable<T>` 테이블의 내용을 쿼리하거나 정렬할 수 있도록 컬렉션을 반환 하는 메서드를 지원 합니다. 다음 코드에서는 Linq를 사용 하 여 문자 "A"로 시작 하는 모든 항목을 필터링 하는 예제를 보여 줍니다.

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>SQL을 사용 하 여 개체 선택

SQLite.Net는 데이터에 대 한 개체 기반 액세스를 제공할 수 있지만 때로는 Linq에서 허용 하는 것 보다 더 복잡 한 쿼리를 수행 해야 할 수 있습니다 (또는 더 빠른 성능이 필요할 수 있음). 다음과 같이 쿼리 메서드를 사용 하 여 SQL 명령을 사용할 수 있습니다.

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!NOTE]
> SQL 문을 직접 작성할 때 클래스 및 해당 특성에서 생성 된 데이터베이스의 테이블 및 열 이름에 대 한 종속성을 만들 수 있습니다. 코드에서 이러한 이름을 변경 하는 경우 수동으로 작성 된 SQL 문을 업데이트 해야 합니다.

### <a name="deleting-an-object"></a>개체 삭제

기본 키는 다음과 같이 행을 삭제 하는 데 사용 됩니다.

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

에서 `rowcount` 영향을 받는 행의 수를 확인할 수 있습니다 (이 경우 삭제).

## <a name="using-sqlitenet-with-multiple-threads"></a>여러 스레드에 SQLite.NET 사용

SQLite는 다음과 같은 세 가지 스레딩 모드를 지원 합니다. *단일 스레드*, *다중 스레드*및 *serialize*됩니다. 제한 없이 여러 스레드에서 데이터베이스에 액세스 하려는 경우에는 **serialize** 된 스레딩 모드를 사용 하도록 SQLite를 구성할 수 있습니다. 응용 프로그램의 초기에이 모드를 설정 하는 것이 중요 합니다 (예: `OnCreate` 메서드의 시작).

스레딩 모드를 변경 하려면를 호출 `SqliteConnection.SetConfig`합니다. 예를 들어 다음 코드 줄은 **serialize** 된 모드에 대해 SQLite를 구성 합니다.

```csharp
using using Mono.Data.Sqlite;
...
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

SQLite의 Android 버전에는 몇 가지 추가 단계가 필요한 제한 사항이 있습니다. 에 대 한 호출이 `SqliteConnection.SetConfig` 와 `library used incorrectly`같은 SQLite 예외를 생성 하는 경우에는 다음 해결 방법을 사용 해야 합니다.

1. 앱에서 `sqlite3_shutdown` 및 `sqlite3_initialize` api를 사용할 수 있도록 네이티브 **libsqlite.so** 라이브러리에 연결 합니다.

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```

2. `OnCreate` 메서드의 시작 부분에서이 코드를 추가 하 여 sqlite를 종료 하 고, **serialize** 된 모드로 구성 하 고, sqlite를 다시 초기화 합니다.

    ```csharp
    using using Mono.Data.Sqlite;
    ...
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

이 해결 방법은 `Mono.Data.Sqlite` 라이브러리에도 적용 됩니다. SQLite 및 다중 스레딩에 대 한 자세한 내용은 [sqlite 및 다중 스레드](https://www.sqlite.org/threadsafe.html)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Xamarin 양식 데이터 액세스](~/xamarin-forms/data-cloud/data/databases.md)
