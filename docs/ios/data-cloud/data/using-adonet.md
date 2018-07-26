---
title: ADO.NET을 사용 하 여 Xamarin.iOS를 사용 하 여
description: 이 문서에서는 ADO.NET SQLite Xamarin.iOS 응용 프로그램에 액세스 하려면 메서드를 사용 하는 방법을 설명 합니다. 어셈블리 참조, Mono.Data.Sqlite, 및 BasicDataAccess 샘플에 설명 합니다.
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 83f6059c405b2156270f4359cbba33177861af02
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241240"
---
# <a name="using-adonet-with-xamarinios"></a>ADO.NET을 사용 하 여 Xamarin.iOS를 사용 하 여

Xamarin은 ios의 경우 익숙한 ADO.NET 같은 구문을 사용 하 여 노출 제공 되는 SQLite 데이터베이스에 대 한 기본 제공 지원 합니다. SQLite에서와 같은 처리 되는 SQL 문을 작성 해야 이러한 Api를 사용 하 여 `CREATE TABLE`, `INSERT` 및 `SELECT` 문입니다.

## <a name="assembly-references"></a>어셈블리 참조

SQLite를 추가 해야 하는 ADO.NET 통한 액세스를 사용 하려면 `System.Data` 고 `Mono.Data.Sqlite` (Visual Studio 및 Mac 용 Visual Studio에서 샘플)에 대 한 다음과 같은 iOS 프로젝트에 대 한 참조:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](using-adonet-images/image4.png "Mac 용 Visual Studio에서 어셈블리 참조")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Visual Studio에서 어셈블리 참조")

-----

마우스 오른쪽 단추로 클릭 **참조 > 참조를 편집 하는 중...**  다음 클릭 하 여 필요한 어셈블리를 선택 합니다.

## <a name="about-monodatasqlite"></a>Mono.Data.Sqlite에 대 한

사용 하 여는 `Mono.Data.Sqlite.SqliteConnection` 빈 데이터베이스 파일을 만드는 클래스를 인스턴스화하는 차례로 `SqliteCommand` 에서는 데이터베이스에 대해 SQL 명령을 실행 하는 데 사용할 수 있는 개체입니다.


1. **빈 데이터베이스 만들기** -호출을 `CreateFile` 유효한 메서드 (ie. 쓰기 가능한) 파일 경로입니다. 여부를이 메서드를 호출 하기 전에 파일이 이미, 그렇지 않으면 (새) 데이터베이스를 이전 버전의 맨 위에 만들어지고 이전 파일에 데이터가 손실 됩니다 확인 해야 합니다.

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > `dbPath` 변수는이 문서의 앞부분에서 설명한 규칙에 따라 결정 되어야 합니다.

2. **데이터베이스 연결 만들기** -SQLite 데이터베이스 파일을 만든 후 데이터에 액세스 하는 연결 개체를 만들 수 있습니다. 연결 형식으로 사용 하는 연결 문자열을 사용 하 여 생성 된 `Data Source=file_path`다음과 같이 합니다.

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    앞에서 설명한 대로 연결 되어서는 안 다시 사용 되는 다른 스레드에서 합니다. 확실 하지 않은의 경우 필요에 따라 연결 만들고; 완료 되 면 닫습니다 하지만 너무 필요한 것 보다이 더 자주 수행 하는 주의 해야 합니다.
    
3. **만들기 및 데이터베이스 명령을 실행** -있으면 연결에 대해 임의의 SQL 명령을 실행할 수 있습니다. 아래 코드에는 실행 되는 CREATE TABLE 문을 보여 줍니다.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

SQL 데이터베이스에 대해 직접 실행 하는 경우 이미 존재 하는 테이블을 만들려고 시도 같은 잘못 된 요청을 하지는 일반 예방 조치를 취해야 합니다. "SQLite 오류 테이블 [항목] 이미 있습니다."와 같은 SqliteException를 발생 하지 않도록 유지 데이터베이스의 구조를 추적 합니다.

## <a name="basic-data-access"></a>기본 데이터 액세스

합니다 *DataAccess_Basic* iOS에서 실행 하는 경우이 문서에 대 한 샘플 코드가 다음과 같이 합니다.

 ![](using-adonet-images/image9.png "iOS ADO.NET 샘플")

아래 코드는 간단한 SQLite 작업을 수행 하는 방법을 보여 줍니다 하 고 응용 프로그램의 주 창에 텍스트로 결과 보여 줍니다.

이러한 네임 스페이스를 포함 해야 합니다.

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

다음 코드 예제에서는 전체 데이터베이스 상호 작용을 보여 줍니다.

1.  데이터베이스 파일 만들기
2.  일부 데이터를 삽입합니다.
3.  데이터 쿼리

이러한 작업은 일반적으로 여러 위치에 나타나는 코드 전체에서 예를 들어 응용 프로그램을 처음 시작할 때 데이터베이스 파일 및 테이블 만들기 및 앱에서 개별 화면에서 데이터 읽기 및 쓰기를 수행할 수 있습니다. 아래 예제에서는이 예제에 대 한 단일 메서드로 그룹화 되어 있습니다.

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

SQLite는 데이터에 대해 실행할 SQL 명령 임의의 허용 하므로 모든 만들기, 삽입, 업데이트, 삭제 또는 SELECT 문 원하는 수행할 수 있습니다. Sqlite 웹 사이트에서 SQLite에서 지 원하는 SQL 명령에 대 한 읽을 수 있습니다. SQL 문은 SqliteCommand 개체의 세 가지 방법 중 하나를 사용 하 여 실행 됩니다.

-  **ExecuteNonQuery** -테이블 만들기 또는 데이터 삽입을 위해 일반적으로 사용 합니다. 일부 작업에 대 한 반환 값은 영향을 받는 행 수,이-1이 고, 그렇지 합니다.
-  **ExecuteReader** -행의 컬렉션으로 반환 되어야 하는 경우에 사용 된 `SqlDataReader` 합니다.
-  **ExecuteScalar** – 단일 값 (예를 들어 집계)를 검색 합니다.


### <a name="executenonquery"></a>EXECUTENONQUERY

INSERT, UPDATE 및 DELETE 문에 영향을 받는 행 수를 반환 합니다. 다른 모든 SQL 문은-1을 반환 합니다.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

다음 메서드는 SELECT 문에서 WHERE 절을 보여 줍니다. 코드는 완전 한 SQL 문이 선별 하기 때문에 문자열 주위에 따옴표 (')와 같은 예약된 문자 이스케이프에 주의 해야 합니다.

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

ExecuteReader 메서드 SqliteDataReader 개체를 반환합니다. 예제 에서처럼 Read 메서드를 외에도 다른 유용한 속성 포함 됩니다.

-  **RowsAffected** – 쿼리에 의해 영향을 받는 행 수입니다.
-  **HasRows** – 모든 행이 반환 여부를 선택 합니다.


### <a name="executescalar"></a>EXECUTESCALAR

(집계) 같은 단일 값을 반환 하는 SELECT 문을 사용 합니다.

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

합니다 `ExecuteScalar` 메서드의 반환 형식은 `object` -데이터베이스 쿼리에 따라 결과 캐스팅 해야 합니다. COUNT 쿼리에서 정수 또는 단일 열 선택 쿼리에서 문자열로 될 수 있습니다. 이 다른 판독기 개체 또는 영향을 받는 행의 개수를 반환 하는 다른 Execute 메서드에 note 합니다.


## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS 데이터 레시피](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
