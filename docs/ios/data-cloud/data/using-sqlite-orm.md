---
title: SQLite.NET를 사용 하 여 Xamarin.iOS를 사용 하 여
description: SQLite.NET PCL NuGet 라이브러리 Xamarin.iOS 앱에 대 한 간단한 데이터 액세스 메커니즘을 제공합니다. 이 문서는이 라이브러리를 사용 하는 방법의 개요를 제공 합니다.
ms.prod: xamarin
ms.assetid: 79813B09-42D7-47DD-AE71-A605E6B9EF24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/18/2018
ms.openlocfilehash: 370867b52ec09d0c3ad0f801b6a75c356d806734
ms.sourcegitcommit: 086edd9c44dfc0e77412e1ed5eda7318bbd1ce7c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58477397"
---
# <a name="using-sqlitenet-with-xamarinios"></a>SQLite.NET를 사용 하 여 Xamarin.iOS를 사용 하 여

Xamarin에서 권장 하는 SQLite.NET 라이브러리에 저장 하 고 iOS 장치에서 로컬 SQLite 데이터베이스의 개체를 검색할 수 있는 기본적인 ORM는 합니다.
ORM는 개체 관계형 매핑을 – 저장 하 고 SQL 문을 작성 하지 않고 데이터베이스에서 "개체"를 검색할 수 있는 API를 나타냅니다.

<a name="Usage"/>

## <a name="usage"></a>사용법

Xamarin 앱에서 SQLite.NET 라이브러리를 포함 하려면 다음 NuGet 패키지를 프로젝트에 추가:

- **패키지 이름:** sqlite-net-pcl
- **작성자:** Frank A. Krueger
- **ID:** sqlite-net-pcl
- **Url:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![SQLite.NET NuGet 패키지](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet 패키지")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> 다른 SQLite 패키지의 여러 가지 – 적절히 (아닐 상위 검색 결과)를 선택 해야 합니다.

SQLite.NET 라이브러리를 사용할 수 있으면 데이터베이스에 액세스 하려면 사용 하는 이러한 세 단계를 수행 합니다.

1. **추가 하 여 문을** -다음 문을 추가 합니다 C# 데이터 액세스 필요한 파일:

    ```csharp
    using SQLite;
    ```

1. **빈 데이터베이스 만들기** -SQLiteConnection 클래스 생성자를 사용 하는 파일 경로 전달 하 여 데이터베이스 참조를 만들 수 있습니다. 검사할 파일이 이미 있는 경우 자동으로 만들어집니다 필요한 그렇지 않은 경우 기존 데이터베이스 파일이 열립니다 필요가 없습니다.

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

    DbPath 변수는이 문서의 앞부분에서 설명한 규칙에 따라 결정 되어야 합니다.

1. **데이터 저장** -CreateTable 및 다음과 같은 Insert와 같은 해당 메서드를 호출 하 여 명령이 실행 될 데이터베이스 SQLiteConnection 개체를 만든 후:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

1. **데이터를 검색할** -검색할 개체 (또는 개체의 목록)에 다음 구문을 사용 합니다.

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>기본 데이터 액세스 예제

합니다 *DataAccess_Basic* iOS에서 실행 하는 경우이 문서에 대 한 샘플 코드가 다음과 같이 합니다. 코드는 간단한 SQLite.NET 작업을 수행 하는 방법을 보여 줍니다 하 고 응용 프로그램의 주 창에 텍스트로 결과 보여 줍니다.

**iOS**

 [![iOS SQLite.NET 샘플](using-sqlite-orm-images/image2-sml.png)](using-sqlite-orm-images/image2-sml.png#lightbox)

다음 코드 샘플에서는 SQLite.NET 라이브러리를 사용 하 여 기본 데이터베이스 액세스를 캡슐화 하는 전체 데이터베이스 상호 작용을 보여 줍니다. 표시 됩니다.

1.  데이터베이스 파일 만들기
1.  개체를 만들고 저장 하 여 일부 데이터 삽입
1.  데이터 쿼리

이러한 네임 스페이스를 포함 해야 합니다.

```csharp
using SQLite; // from the github SQLite.cs class
```

강조 표시 된 대로 프로젝트에 SQLite를 추가 하는 것이 그러려면 [여기](#Usage)합니다. SQLite 데이터베이스 테이블에는 클래스에 특성을 추가 하 여 정의 됩니다 (의 `Stock` 클래스)는 CREATE TABLE 명령 대신 합니다.

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

사용 하는 `[Table]` 테이블 이름 매개 변수는 기본 데이터베이스 테이블에 클래스와 동일한 이름 (이 경우 "Stock") 하면를 지정 하지 않고 특성입니다. 실제 테이블 이름은 데이터베이스에 대해 직접 SQL 쿼리를 작성 하지 않고 있습니다 ORM 데이터 액세스 메서드를 사용 하는 경우 중요 합니다. 마찬가지로 `[Column("_id")]` 특성은 선택적 경우 없는 열에 추가할 클래스의 속성과 동일한 이름 가진 테이블 및 합니다.

## <a name="sqlite-attributes"></a>SQLite 특성

기본 데이터베이스에 저장 되는 방식을 제어 하려면 클래스에 적용할 수 있는 공통 특성은 다음과 같습니다.

-  **[PrimaryKey]**  -이 특성을 기본 테이블의 기본 키로 강제로 하는 정수 속성에 적용할 수 있습니다. 복합 기본 키가 지원 되지 않습니다.
-  **[AutoIncrement]**  -이 특성에는 정수 속성의 값을 데이터베이스에 삽입 하는 각 새 개체에 대 한 자동 증가 하면 합니다.
-  **[Column(name)]**  – 선택적 제공 `name` 매개 변수는 기본 데이터베이스 열 이름 (속성과 동일)의 기본값을 재정의 됩니다.
-  **[Table(name)]**  – 클래스는 기본 SQLite 테이블에 저장할 수 있는 것으로 표시 합니다. 선택적 name 매개 변수를 지정 합니다. 기본 데이터베이스 테이블의 이름 (클래스 이름과 동일)의 기본값을 재정의 됩니다.
-  **[MaxLength(value)]**  – 데이터베이스 삽입을 수행 하려고 할 때 텍스트 속성의 길이 제한 합니다. 코드가 사용 유효성을 검사 해야이이 특성은 '때만 확인' 삽입 또는 업데이트 작업을 시도 하는 대로 개체를 삽입 하기 전에 합니다.
-  **[무시]**  –이 속성을 무시 하려면 SQLite.NET 발생 합니다. 데이터베이스에 저장할 수 없는 형식이 있는 속성 또는 자동으로 확인할 수 없는 모델 컬렉션 SQLite 되도록 하는 속성에 대 한 경우 특히 유용 합니다.
-  **[Unique]**  – 기본 데이터베이스 열에 값이 고유한 지 확인 합니다.


대부분의 이러한 특성은 선택 사항, SQLite 테이블 및 열 이름에 대 한 기본 값을 사용 합니다. 데이터에 선택 및 삭제 쿼리를 효율적으로 수행할 수 있도록 항상 정수 기본 키를 지정 해야 합니다.

## <a name="more-complex-queries"></a>더 복잡 한 쿼리

다음 메서드는 `SQLiteConnection` 다른 데이터 작업을 수행 하기 위해 사용할 수 있습니다.

-  **삽입** – 데이터베이스에 새 개체를 추가 합니다.
-  **가져올<T>**  – 기본 키를 사용 하 여 개체를 검색 하려고 시도 합니다.
-  **테이블<T>**  – 테이블의 모든 개체를 반환 합니다.
-  **삭제** – 기본 키를 사용 하 여 개체를 삭제 합니다.
-  **쿼리<T>**  -(개체로) 행 개수를 반환 하는 SQL 쿼리를 수행 합니다.
-  **실행할** –이 메서드를 사용 하 여 (아니라 `Query` ) 행 (예: INSERT, UPDATE 및 DELETE 명령) SQL에서 다시 감염 되지 않은 경우.


### <a name="getting-an-object-by-the-primary-key"></a>기본 키로 개체를 가져오는 중

SQLite.Net 해당 기본 키를 기반으로 하는 단일 개체를 검색 하는 Get 메서드를 제공 합니다.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>Linq를 사용 하 여 개체를 선택 합니다.

컬렉션을 반환 하는 메서드는 IEnumerable 지원<T> Linq를 사용 하 여 쿼리 또는 테이블의 내용을 정렬할 수 있도록 합니다. 다음 코드는 문자 "A"로 시작 하는 모든 항목을 필터링 하려면 Linq를 사용 하는 예제를 보여 줍니다.

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>SQL을 사용 하 여 개체를 선택 합니다.

SQLite.Net 데이터에 대 한 개체 기반 액세스를 제공할 수, 하는 경우에 Linq를 사용 하면 (또는 더 빠른 성능을 해야 할 수 있습니다) 보다 더 복잡 한 쿼리를 수행 해야 경우가 있습니다. 다음과 같이 쿼리 메서드를 사용 하 여 SQL 명령을 사용할 수 있습니다.

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!IMPORTANT]
> 직접 SQL 문을 작성 하는 경우 테이블 및 데이터베이스에 클래스 및 해당 특성에서 생성 된 열 이름에 대 한 종속성을 만듭니다. 코드에서 해당 이름을 변경 하는 경우 수동으로 작성 된 모든 SQL 문을 업데이트 해야 합니다.

### <a name="deleting-an-object"></a>개체 삭제

다음과 같이 기본 키 행을 삭제 하는 합니다.

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

확인할 수 있습니다는 `rowcount` 행 개수 (이 경우 삭제) 영향을 확인 합니다.

## <a name="using-sqlitenet-with-multiple-threads"></a>SQLite.NET를 사용 하 여 여러 스레드를 사용 하 여

SQLite는 서로 다른 3 가지 스레딩 모드를 지원합니다. *단일 스레드*하십시오 *다중 스레드*, 및 *직렬화*합니다. 아무런 제한 없이 여러 스레드에서 데이터베이스에 액세스 하려는 경우 SQLite를 사용 하 여 구성할 수 있습니다 합니다 **직렬화 됨** 모드를 스레딩 합니다. 응용 프로그램의 초기에이 모드를 설정 해야 (예를 들어 맨 앞에 `OnCreate` 메서드).

스레딩 모드를 변경 하려면 호출 `SqliteConnection.SetConfig` 에 `Mono.Data.Sqlite` 네임 스페이스입니다. 이 코드 줄에 대해 SQLite를 구성 하는 예를 들어 **직렬화 됨** 모드:

```csharp
using Mono.Data.Sqlite;
...
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
