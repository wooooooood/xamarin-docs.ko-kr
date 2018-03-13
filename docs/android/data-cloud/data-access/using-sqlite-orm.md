---
title: "SQLite.NET를 사용 하 여"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: b9523d76c04dae97b74744fbe2bd6bc7022c3194
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="using-sqlitenet"></a>SQLite.NET를 사용 하 여

Xamarin에서 권장 하는 SQLite.NET 라이브러리는 쉽게 저장 하 고 Android 장치에서 로컬 SQLite 데이터베이스에 개체를 검색할 수 있는 매우 기본적인 ORM입니다. 개체 관계형 매핑을 ORM은 &ndash; 하면 저장 하 고 SQL 문을 작성 하지 않고도 데이터베이스에서 "개체"를 검색 하는 API입니다.

## <a name="using-sqlitenet"></a>SQLite.NET를 사용 하 여

Xamarin 앱에 SQLite.NET 라이브러리를 포함 하려면 추가 [SQLite.net PCL NuGet 패키지](https://www.nuget.org/packages/sqlite-net-pcl/) 사용 하 여 프로젝트에는 **SQLite net PCL** NuGet 패키지:

[![SQLite.NET NuGet 패키지](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet 패키지")](using-sqlite-orm-images/image1a.png#lightbox)

SQLite.NET 라이브러리를 사용할 수 있으면 데이터베이스에 액세스 하려면 사용 하려면 다음 세 가지 단계를 따르십시오.


1.  **사용 하 여 추가 문을** &ndash; 데이터 액세스는 필요한 C# 파일에 다음 문을 추가 합니다. 

    ```csharp
    using SQLite;
    ```

2.  **빈 데이터베이스를** &ndash; SQLiteConnection 클래스 생성자의 파일 경로 전달 하 여 데이터베이스 참조를 만들 수 있습니다. 파일이 이미 존재 하는지를 확인할 필요가 없습니다 &ndash; 기존 데이터베이스 파일을 열 수는 그렇지 않은 경우, 필요한 경우에 자동으로 생성 됩니다. `dbPath` 변수는이 문서 앞부분에서 설명한 규칙에 따라 결정 되어야 합니다.

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3.  **데이터 저장** &ndash; CreateTable 및 다음과 같은 Insert와 같이 해당 메서드를 호출 하 여 데이터베이스 명령이 실행 될 SQLiteConnection 개체를 만든 후:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4.  **데이터를 검색할** &ndash; 검색 하는 개체 (또는 개체 목록을) 다음 구문을 사용 됩니다.:

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>기본 데이터 액세스 예제

*DataAccess_Basic* Android에서 실행 하면 다음과 같이 표시 되는이 문서에 대 한 샘플 코드입니다. 코드는 SQLite.NET에서 단순 작업을 수행 하는 방법을 보여 줍니다. 하 고 응용 프로그램의 주 창에 텍스트로 결과를 보여 줍니다.


**Android**

![Android SQLite.NET 샘플](using-sqlite-orm-images/image3.png "Android SQLite.NET 샘플")

다음 코드 예제에서는 기본 데이터베이스 액세스를 캡슐화 SQLite.NET 라이브러리를 사용 하 여 전체 데이터베이스 상호 작용을 보여 줍니다.
표시 됩니다.

1.  데이터베이스 파일 만들기

2.  개체를 만들고 다음 저장 하 여 일부 데이터를 삽입 합니다.

3.  데이터 쿼리

이러한 네임 스페이스를 포함 해야 합니다.

```csharp
using SQLite; // from the github SQLite.cs class
```

마지막은 SQLite 프로젝트에 추가 했는지 필요 합니다. SQLite 데이터베이스 테이블 클래스에 특성을 추가 하 여 정의 됩니다 (의 `Stock` 클래스) CREATE TABLE 명령 대신 합니다.

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

사용 하 여 `[Table]` 테이블 이름 매개 변수 (이 경우 "Stock")에서 클래스와 동일한 이름으로 기본 데이터베이스 테이블 하면를 지정 하지 않고 특성입니다. 실제 테이블 이름 ORM 데이터 액세스 방법을 사용 하지 않고 데이터베이스에 대해 직접 SQL 쿼리를 작성할 경우에 유용 합니다. 마찬가지로 `[Column("_id")]` 특성은 선택적 이며 경우 없는 열에 추가할 클래스에 속성으로 같은 이름의 테이블 및 합니다.

## <a name="sqlite-attributes"></a>SQLite 특성

기본 데이터베이스에 저장 되는 방법을 제어 하려면 클래스에 적용할 수 있는 공통 특성은 다음과 같습니다.

-   **[PrimaryKey]**  &ndash; 기본 테이블의 기본 키를 나타낼 하는 정수 속성에이 특성을 적용할 수 있습니다. 복합 기본 키가 지원 되지 않습니다.

-   **[AutoIncrement]**  &ndash; 이 특성은 정수 속성의 값이 데이터베이스에 삽입 하는 각 새 개체에 대 한 자동 증가 되도록 하면 합니다.

-   **[Column(name)]**  &ndash; 선택적 제공 `name` 매개 변수는 기본 데이터베이스 열 이름 (속성과 동일)의 기본값을 재정의 합니다.

-   **[Table(name)]**  &ndash; 원본으로 사용 하는 SQLite 테이블에 저장 될 수 있는 것으로 클래스를 표시 합니다. 선택적 name 매개 변수를 지정 하는 기본 데이터베이스 테이블 이름 (클래스 이름과 동일)의 기본값을 덮어씁니다.

-   **[MaxLength(value)]**  &ndash; 데이터베이스 삽입 하려고 할 때 text 속성의 길이 제한 합니다. 사용 하는 코드가이 특성은 '때만 확인' 데이터베이스 삽입 또는 업데이트 작업을 수행 하려고 하는 대로 개체를 삽입 하기 전에 확인할 해야 합니다.

-   **[무시]**  &ndash; 하면 SQLite.NET이이 속성을 무시 합니다.
    이 데이터베이스에 저장할 수 없는 형식이 있는 속성 또는 속성을 자동으로 해결할 수 없는 모델 컬렉션 SQLite 되어야 하는 데 특히 유용 합니다.

-   **[고유]**  &ndash; 기본 데이터베이스 열에 값이 고유한 지 확인 합니다.


대부분의 이러한 특성은 선택 사항, SQLite 테이블 및 열 이름에 대 한 기본값을 사용 합니다. 항상 사용자 데이터에 선택 및 삭제 쿼리를 효율적으로 수행할 수 있도록 정수 기본 키를 지정 해야 합니다.

## <a name="more-complex-queries"></a>더 복잡 한 쿼리

다음 메서드를 `SQLiteConnection` 는 다른 데이터 작업을 수행 하는 데 사용할 수 있습니다.

-   **삽입** &ndash; 데이터베이스에 새 개체를 추가 합니다.

-   **가져오기&lt;T&gt;**  &ndash; 기본 키를 사용 하는 개체를 검색 하려고 시도 합니다.

-   **테이블&lt;T&gt;**  &ndash; 테이블의 모든 개체를 반환 합니다.

-   **삭제** &ndash; 기본 키를 사용 하 여 개체를 삭제 합니다.

-   **쿼리&lt;T&gt;**  &ndash; (개체로) 행의 수를 반환 하는 SQL 쿼리를 실행 합니다.

-   **실행** &ndash; 이 메서드를 사용 하 여 (아닌 `Query`) (예: INSERT, UPDATE 및 DELETE 명령) SQL에서 다시 행을 원하지 때.


### <a name="getting-an-object-by-the-primary-key"></a>기본 키로 개체를 가져오는

SQLite.Net 기본 키를 기반으로 하는 단일 개체를 검색 하는 Get 메서드를 제공 합니다.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Linq를 사용 하는 개체를 선택 합니다.

컬렉션을 반환 하는 메서드를 지원 `IEnumerable<T>` Linq를 사용 하 여 테이블의 내용을 정렬할 또는 쿼리할 수 있도록 합니다. 다음 코드는 문자 "A"로 시작 하는 모든 항목을 필터링 할 Linq를 사용 하는 예제를 보여 줍니다.

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>SQL을 사용 하는 개체를 선택 합니다.

SQLite.Net 데이터에 대 한 개체 기반 액세스를 제공할 수, 하는 경우에 해야 하는 경우가 Linq를 사용 하면 (또는 더 빠른 성능을 할 수 있습니다) 보다 더 복잡 한 쿼리를 수행 하 합니다. 다음 그림과 같이 쿼리 메서드를 사용 하면 SQL 명령을 사용할 수 있습니다.

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!NOTE]
> 직접 SQL 문을 작성 하는 경우 테이블 및 사용자 클래스 및 해당 특성에서 생성 된 데이터베이스의 열 이름에 대 한 종속성을 만듭니다. 코드에서 해당 이름을 변경 하면 모든 수동으로 작성 된 SQL 문을 업데이트 해야 합니다.

### <a name="deleting-an-object"></a>개체 삭제

다음과 같이 행을 삭제 하는 기본 키 사용 됩니다.

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

확인할 수 있습니다는 `rowcount` 행의 수 (이 경우 삭제 됨) 영향을 확인 합니다.

## <a name="using-sqlitenet-with-multiple-threads"></a>여러 스레드를 가진 SQLite.NET 사용

SQLite는 서로 다른 세 가지 스레딩 모드가 지원: *단일 스레드*, *다중 스레드*, 및 *직렬화*합니다. 아무런 제한 없이 여러 스레드에서 데이터베이스에 액세스 하려는 경우 SQLite 사용 하도록 구성할 수 있습니다는 **직렬화** 스레딩 모드입니다. 초기 응용 프로그램에이 모드를 설정 해야 (예를 들어 맨 앞에 `OnCreate` 메서드).

스레딩 모드를 변경 하려면 호출 `SqliteConnection.SetConfig`합니다. 이 줄의 코드에 대해 SQLite를 구성 하는 예를 들어 **직렬화** 모드: 

```csharp
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

Android 버전 SQLite의 제한이 몇 가지 추가 단계가 필요 합니다. 경우에 대 한 호출 `SqliteConnection.SetConfig` SQLite 예외와 같은 생성 `library used incorrectly`, 다음 해결 방법을 사용 해야 합니다. 

1.  링크는 네이티브를 **libsqlite.so** 라이브러리 있도록는 `sqlite3_shutdown` 및 `sqlite3_initialize` Api 앱에 사용할 수 있습니다.

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```


2.  시작 부분에는 `OnCreate` 메서드를 종료 SQLite에이 코드를 추가, 구성에 대 한 **직렬화** 모드 및 SQLite 다시 초기화 하십시오:

    ```csharp
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

이 해결 방법에 대해서도 작동는 `Mono.Data.Sqlite` 라이브러리입니다. 다중 스레딩 및 SQLite에 대 한 자세한 내용은 참조 [SQLite 및 다중 스레드](https://www.sqlite.org/threadsafe.html)합니다. 



## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android 데이터 레시피](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
