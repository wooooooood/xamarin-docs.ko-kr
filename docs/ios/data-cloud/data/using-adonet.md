---
title: Xamarin.ios에서 ADO.NET 사용
description: 이 문서에서는 ADO.NET 응용 프로그램에서 SQLite에 액세스 하는 방법으로를 사용 하는 방법을 설명 합니다. 어셈블리 참조, Mono. BasicDataAccess 및 샘플에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: a9df85a405bc086f86dae73fea615581bf9d28d0
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767383"
---
# <a name="using-adonet-with-xamarinios"></a>Xamarin.ios에서 ADO.NET 사용

Xamarin은 iOS에서 사용할 수 있는 SQLite 데이터베이스를 기본적으로 지원 하며, 친숙 한 ADO.NET와 유사한 구문을 사용 하 여 노출 됩니다. 이러한 api를 사용 하려면 `CREATE TABLE`, `INSERT` 및 `SELECT` 문과 같이 SQLite에 의해 처리 되는 SQL 문을 작성 해야 합니다.

## <a name="assembly-references"></a>어셈블리 참조

ADO.NET를 통해 액세스 SQLite를 사용 하려면 다음과 `System.Data` 같이 `Mono.Data.Sqlite` iOS 프로젝트에 대 한 참조를 추가 해야 합니다 (Mac용 Visual Studio 및 Visual Studio의 샘플).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 ![](using-adonet-images/image4.png "Mac용 Visual Studio의 어셈블리 참조")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  ![](using-adonet-images/image6.png "Visual Studio의 어셈블리 참조")

-----

참조 > 마우스 오른쪽 단추로 클릭 하 고 참조 **편집** ...을 클릭 하 여 필요한 어셈블리를 선택 합니다.

## <a name="about-monodatasqlite"></a>Mono 정보. Sqlite

여기서는 `Mono.Data.Sqlite.SqliteConnection` 클래스를 사용 하 여 빈 데이터베이스 파일을 만든 다음 데이터베이스에 `SqliteCommand` 대해 SQL 명령을 실행 하는 데 사용할 수 있는 개체를 인스턴스화합니다.

1. **빈 데이터베이스 만들기** -유효한 (쓰기 `CreateFile` 가능) 파일 경로를 사용 하 여 메서드를 호출 합니다. 이 메서드를 호출 하기 전에 파일이 이미 있는지 여부를 확인 해야 합니다. 그렇지 않으면 새 (비어 있는) 데이터베이스가 이전 데이터베이스의 맨 위에 생성 되며 이전 파일의 데이터가 손실 됩니다.

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > 변수 `dbPath` 는이 문서의 앞부분에서 설명한 규칙에 따라 결정 해야 합니다.

2. **데이터베이스 연결 만들기** -SQLite 데이터베이스 파일이 생성 된 후에는 데이터에 액세스 하는 연결 개체를 만들 수 있습니다. 연결은 다음과 같이의 `Data Source=file_path`형식을 사용 하는 연결 문자열을 사용 하 여 생성 됩니다.

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    앞에서 설명한 것 처럼 여러 스레드에서 연결이 다시 사용 되어서는 안 됩니다. 확실 하지 않은 경우 필요에 따라 연결을 만들고 완료 되 면 닫습니다. 그러나 필요한 것 보다 더 자주이 작업을 수행 하는 것에 유의 해야 합니다.

3. **데이터베이스 명령 만들기 및 실행** -연결 된 후에는 임의의 SQL 명령을 실행할 수 있습니다. 아래 코드는 실행 중인 CREATE TABLE 문을 보여 줍니다.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

데이터베이스에 대해 직접 SQL을 실행할 때는 이미 존재 하는 테이블을 만들려고 하는 것과 같이 잘못 된 요청을 수행 하지 않는 일반적인 예방 조치를 취해야 합니다. "SQLite error table [Items]가 이미 존재 합니다."와 같은 SqliteException을 발생 시 키 지 않도록 데이터베이스 구조를 계속 추적 합니다.

## <a name="basic-data-access"></a>기본 데이터 액세스

IOS에서 실행 되는 경우이 문서의 *DataAccess_Basic* 샘플 코드는 다음과 같습니다.

 ![](using-adonet-images/image9.png "iOS ADO.NET 샘플")

아래 코드에서는 간단한 SQLite 작업을 수행 하 고 결과를 응용 프로그램의 주 창에 텍스트로 표시 하는 방법을 보여 줍니다.

다음 네임 스페이스를 포함 해야 합니다.

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

다음 코드 샘플에서는 전체 데이터베이스 상호 작용을 보여 줍니다.

1. 데이터베이스 파일 만들기
2. 일부 데이터 삽입
3. 데이터 쿼리

이러한 작업은 일반적으로 코드 전체의 여러 위치에 표시 됩니다. 예를 들어 응용 프로그램이 처음 시작 될 때 데이터베이스 파일 및 테이블을 만들고 앱의 개별 화면에서 데이터 읽기 및 쓰기를 수행할 수 있습니다. 아래 예제에서는이 예제에서 단일 메서드로 그룹화 되었습니다.

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}
```

## <a name="more-complex-queries"></a>더 복잡 한 쿼리

SQLite는 임의의 SQL 명령을 데이터에 대해 실행할 수 있으므로 원하는 모든 CREATE, INSERT, UPDATE, DELETE 또는 SELECT 문을 수행할 수 있습니다. Sqlite 웹 사이트의 SQLite에서 지원 되는 SQL 명령에 대해 알아볼 수 있습니다. SqliteCommand 개체의 세 가지 메서드 중 하나를 사용 하 여 SQL 문을 실행 합니다.

- **ExecuteNonQuery** – 일반적으로 테이블을 만들거나 데이터를 삽입 하는 데 사용 됩니다. 일부 작업의 반환 값은 영향을 받는 행의 수입니다. 그렇지 않으면-1입니다.
- **ExecuteReader** – 행 컬렉션을로 `SqlDataReader` 반환 해야 할 때 사용 됩니다.
- **ExecuteScalar** – 단일 값 (예: 집계)을 검색 합니다.

### <a name="executenonquery"></a>EXECUTENONQUERY

INSERT, UPDATE 및 DELETE 문에서는 영향을 받는 행의 수를 반환 합니다. 다른 모든 SQL 문은-1을 반환 합니다.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

다음 메서드는 SELECT 문의 WHERE 절을 보여 줍니다. 코드는 전체 SQL 문을 작성 하므로 문자열 주위에 따옴표 (')와 같은 예약 된 문자를 이스케이프 처리 해야 합니다.

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

ExecuteReader 메서드는 SqliteDataReader 개체를 반환 합니다. 예제에 표시 된 읽기 메서드 외에도 다른 유용한 속성은 다음과 같습니다.

- **Rowsaffected** – 쿼리의 영향을 받는 행의 수입니다.
- **Hasrows** – 행이 반환 되었는지 여부입니다.

### <a name="executescalar"></a>EXECUTESCALAR

단일 값 (예: 집계)을 반환 하는 SELECT 문에 대해이 값을 사용 합니다.

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

메서드의 반환 형식은입니다 `object` . 데이터베이스 쿼리에 따라 결과를 캐스팅 해야 합니다. `ExecuteScalar` 결과는 COUNT 쿼리의 정수 이거나 단일 열 SELECT 쿼리의 문자열 일 수 있습니다. 이는 판독기 개체 또는 영향을 받는 행 수의 수를 반환 하는 다른 Execute 메서드와는 다릅니다.

## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS 데이터 조리법](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin 양식 데이터 액세스](~/xamarin-forms/data-cloud/data/databases.md)
