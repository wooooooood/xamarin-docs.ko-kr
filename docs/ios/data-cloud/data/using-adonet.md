---
title: "ADO.NET을 사용 하 여"
ms.topic: article
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 45d7b3e844501f8aa97d75f2fc8af27a961290b3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="using-adonet"></a>ADO.NET을 사용 하 여

Xamarin은 iOS의 경우 익숙한 ADO.NET와 유사한 구문을 사용 하 여 노출에 사용할 수 있는 SQLite 데이터베이스에 대 한 기본 제공 지원. 와 같은 SQLite에서 처리 하는 SQL 문을 작성 해야 이러한 Api를 사용 하 여 `CREATE TABLE`, `INSERT` 및 `SELECT` 문.

## <a name="assembly-references"></a>어셈블리 참조

SQLite 추가 해야 하는 ADO.NET 통해 액세스를 사용 하려면 `System.Data` 및 `Mono.Data.Sqlite` (샘플에 대 한 Mac 및 Visual Studio 용 Visual Studio에서) 다음 그림과 같이 iOS 프로젝트에 대 한 참조:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](using-adonet-images/image4.png "Mac 용 Visual Studio에서 어셈블리 참조")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Visual Studio에서 어셈블리 참조")

-----

마우스 오른쪽 단추로 클릭 **참조 > 참조 편집...**  다음 필요한 어셈블리를 선택 하려면 클릭 합니다.

## <a name="about-monodatasqlite"></a>Mono.Data.Sqlite에 대 한

사용 하 여는 `Mono.Data.Sqlite.SqliteConnection` 빈 데이터베이스 파일을 만들 클래스를 인스턴스화하는 다음 `SqliteCommand` 데이터베이스에 대해 SQL 명령을 실행 하기 위해 사용할 수 있는 개체입니다.


1. **빈 데이터베이스를 만드는** -호출의 `CreateFile` 을 올바른 메서드 (ie. 쓰기 가능한) 파일 경로입니다. 이 메서드를 호출 하기 전에 파일이 이미 있는지 여부를,의 위에 (새) 데이터베이스를 만들 수는 그렇지 않은 경우 및 이전 파일에 데이터가 손실 됩니다 확인 해야:

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
> **참고:** dbPath 변수는이 문서 앞부분에서 설명한 규칙에 따라 결정 되어야 합니다.

2. **데이터베이스 연결 만들기** -SQLite 데이터베이스 파일이 생성 된 데이터에 액세스 하는 연결 개체를 만들 수 있습니다. 연결이 연결 문자열의 형식로 구성 되며 `Data Source=file_path`여기 표시 된 것 처럼:

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    앞서 언급 했 듯이 연결 안 다시 사용 되는 다른 스레드 간에 됩니다. 의문이 있는 경우 필요에 따라 연결을 만들고; 완료 되 면 닫습니다 하지만 너무 필요한 것 보다이 더 자주 수행 하에 주의 해야 합니다.
    
3. **만들기 및 Database 명령을 실행** 에 대해 임의 SQL 명령을 실행 하는 연결 상태-합니다. 아래 코드에 실행 되 고 CREATE TABLE 문을 보여 줍니다.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

SQL 데이터베이스에 대해 직접 실행할 때 이미 존재 하는 테이블을 만들려고 하는 등의 잘못 된 요청을 하지 일반 예방 조치를 취해야 합니다. 한 추적 데이터베이스의 구조 "SQLite 오류 테이블 [항목] 이미 있습니다."와 같은 한 SqliteException 발생 하지 않도록 합니다.

## <a name="basic-data-access"></a>기본 데이터 액세스

*DataAccess_Basic* iOS를 실행 하면 다음과 같이 표시 되는이 문서에 대 한 샘플 코드:

 ![](using-adonet-images/image9.png "iOS ADO.NET 샘플")

아래 코드 SQLite에서 단순 작업을 수행 하는 방법을 보여 줍니다. 및 응용 프로그램의 주 창에 텍스트로 결과를 보여 줍니다.

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

이러한 작업은 일반적으로 여러 위치에 나타나는 코드 전체에서 예를 들어 응용 프로그램을 처음 시작할 때 데이터베이스 파일 및 테이블을 만들 및 응용 프로그램에서 개별 화면에서 데이터 읽기 및 쓰기를 수행할 수 있습니다. 아래 예제에서는이 예제에 대 한 단일 메서드로 그룹화 되어 있습니다.

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

SQLite에서는 데이터에 대해 실행할 SQL 명령 임의의 허용 하므로 무엇이 든 생성, 삽입, 업데이트, 삭제 또는 SELECT 문 원하는 수행할 수 있습니다. Sqlite 웹 사이트에서 SQLite에서 지 원하는 SQL 명령에 대 한 읽을 수 있습니다. SQL 문은 SqliteCommand 개체에서 세 가지 방법 중 하나를 사용 하 여 실행 됩니다.

-  **ExecuteNonQuery** – 테이블 만들기 또는 데이터 삽입을 위해 일반적으로 사용 합니다. 일부 작업에 대 한 반환 값은 영향을 받는 행의 수가 고 그렇지 않으면-1입니다.
-  **ExecuteReader** -행의 컬렉션으로 반환 되어야 하는 경우 사용 되는 `SqlDataReader` 합니다.
-  **ExecuteScalar** – 단일 값 (예를 들어 집계)을 검색 합니다.


### <a name="executenonquery"></a>EXECUTENONQUERY

INSERT, UPDATE 및 DELETE 문에 영향을 받는 행 수를 반환 합니다. 다른 모든 SQL 문에-1을 반환 합니다.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

다음 메서드는 SELECT 문에서 WHERE 절을 보여 줍니다. 코드는 완전 한 SQL 문이 만들어 때문에 문자열 앞뒤에 따옴표 (') 등의 예약 된 문자를 이스케이프할 주의 해야 합니다.

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

ExecuteReader 메서드 SqliteDataReader 개체를 반환합니다. 이 예제에 표시 된 읽기 메서드 외에 다른 유용한 속성과 다음과 같습니다.

-  **RowsAffected** – 쿼리에 의해 영향을 받는 행의 수입니다.
-  **HasRows** – 모든 행이 반환 된 여부.


### <a name="executescalar"></a>EXECUTESCALAR

(집계) 같은 단일 값을 반환 하는 SELECT 문에 대해이 사용 합니다.

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` 메서드의 반환 형식이 `object` -데이터베이스 쿼리에 따라 결과 캐스팅 해야 합니다. COUNT 쿼리에서 정수 또는 문자열에서 단일 열 선택 쿼리 될 수 있습니다. 이 다른 판독기 개체 또는 영향을 받는 행 수의 개수를 반환 하는 다른 Execute 메서드에 note 합니다.


## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS 데이터 레시피](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
