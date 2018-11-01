---
title: ADO.NET을 사용 하 여 android
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/08/2018
ms.openlocfilehash: 7ecf7244fb2ccbe0e4163c89941f9de5138ba713
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674953"
---
# <a name="using-adonet-with-android"></a>ADO.NET을 사용 하 여 android

Xamarin은 Android에서 사용할 수 있고 친숙 한 ADO.NET 같은 구문을 사용 하 여 노출 될 수 있습니다는 SQLite 데이터베이스에 대 한 기본 제공 지원 합니다. SQLite에서와 같은 처리 되는 SQL 문을 작성 해야 이러한 Api를 사용 하 여 `CREATE TABLE`, `INSERT` 및 `SELECT` 문입니다.

## <a name="assembly-references"></a>어셈블리 참조

SQLite를 추가 해야 하는 ADO.NET 통한 액세스를 사용 하려면 `System.Data` 및 `Mono.Data.Sqlite` 다음과 같이 Android 프로젝트에 대 한 참조:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows) 

![Visual Studio에서 android 참조](using-adonet-images/image7.png "Visual Studio에서 Android 참조") 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos) 

![Mac 용 Visual Studio의 android 참조](using-adonet-images/image5.png "Android Mac 용 Visual Studio에서 참조 합니다.") 

-----


마우스 오른쪽 단추로 클릭 **참조 > 참조를 편집 하는 중...**  다음 클릭 하 여 필요한 어셈블리를 선택 합니다.

## <a name="about-monodatasqlite"></a>Mono.Data.Sqlite에 대 한

사용 하 여는 `Mono.Data.Sqlite.SqliteConnection` 빈 데이터베이스 파일을 만드는 클래스를 인스턴스화하는 차례로 `SqliteCommand` 에서는 데이터베이스에 대해 SQL 명령을 실행 하는 데 사용할 수 있는 개체입니다.

**빈 데이터베이스 만들기** &ndash; 호출을 `CreateFile` 유효한 (즉, 쓰기 가능) 파일 경로 사용 하 여 메서드. 여부를이 메서드를 호출 하기 전에 파일이 이미, 그렇지 않으면 (새) 데이터베이스를 이전 버전의 맨 위에 만들어지고 이전 파일에 데이터가 손실 됩니다 확인 해야 합니다.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` `dbPath` 변수는이 문서의 앞부분에서 설명한 규칙에 따라 결정 되어야 합니다.

**데이터베이스 연결 만들기** &ndash; SQLite 데이터베이스 파일을 만든 후 데이터에 액세스 하는 연결 개체를 만들 수 있습니다. 연결 형식으로 사용 하는 연결 문자열을 사용 하 여 생성 된 `Data Source=file_path`다음과 같이 합니다.

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

앞에서 설명한 대로 연결 되어서는 안 다시 사용 되는 다른 스레드에서 합니다. 확실 하지 않은의 경우 필요에 따라 연결 만들고; 완료 되 면 닫습니다 하지만 너무 필요한 것 보다이 더 자주 수행 하는 주의 해야 합니다.

**만들기 및 데이터베이스 명령을 실행** &ndash; 연결 되 면에 대해 임의의 SQL 명령을 실행할 수 있습니다. 아래 코드는 `CREATE TABLE` 실행 중인 문의 합니다.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

SQL 데이터베이스에 대해 직접 실행 하는 경우 이미 존재 하는 테이블을 만들려고 시도 같은 잘못 된 요청을 하지는 일반 예방 조치를 취해야 합니다. 한 추적 데이터베이스의 구조는 발생 하지는 `SqliteException` 와 같은 **SQLite 오류 테이블 [항목] 이미**합니다.

## <a name="basic-data-access"></a>기본 데이터 액세스

합니다 *DataAccess_Basic* Android에서 실행 하는 경우이 문서에 대 한 샘플 코드가 다음과 같이 합니다.

![Android ADO.NET 샘플](using-adonet-images/image8.png "Android ADO.NET 샘플")

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

SQLite에서는 임의의 SQL 명령 데이터에 대해 실행 되도록 하므로 어떤를 수행할 수 있습니다 `CREATE`, `INSERT`, `UPDATE`, `DELETE`, 또는 `SELECT` 문을 선택 합니다. SQLite 웹 사이트에서 SQLite에서 지 원하는 SQL 명령에 대 한 읽을 수 있습니다. 세 가지 방법 중 하나를 사용 하 여 SQL 문이 실행 되는 `SqliteCommand` 개체:

-   **ExecuteNonQuery** &ndash; 테이블 만들기 또는 데이터 삽입을 위해 일반적으로 사용 합니다. 일부 작업에 대 한 반환 값은 영향을 받는 행 수,이-1이 고, 그렇지 합니다.

-   **ExecuteReader** &ndash; 행의 컬렉션으로 반환 되어야 하는 경우에 사용 된 `SqlDataReader`합니다.

-   **ExecuteScalar** &ndash; 단일 값 (예를 들어 집계)를 검색 합니다.


### <a name="executenonquery"></a>EXECUTENONQUERY

`INSERT`를 `UPDATE`, 및 `DELETE` 문의 영향을 받는 행 수를 반환 합니다. 다른 모든 SQL 문은-1을 반환 합니다.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

에서는 다음 메서드를 `WHERE` 절을 `SELECT` 문입니다.
코드는 완전 한 SQL 문이 선별 하기 때문에 문자열 주위에 따옴표 (')와 같은 예약된 문자 이스케이프에 주의 해야 합니다.

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

`ExecuteReader` 메서드는 `SqliteDataReader` 개체를 반환합니다. 이외에 `Read` 예에 표시 된 기타 유용한 속성을 포함 하는 메서드:

-   **RowsAffected** &ndash; 쿼리에 의해 영향을 받는 행 수입니다.

-   **HasRows** &ndash; 모든 행이 반환 여부.


### <a name="executescalar"></a>EXECUTESCALAR

이 사용 하 여 `SELECT` (집계) 같은 단일 값을 반환 하는 문입니다.

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

합니다 `ExecuteScalar` 메서드의 반환 형식이 `object` &ndash; 데이터베이스 쿼리에 따라 결과 캐스팅 해야 합니다. 정수 표시 될 수는 `COUNT` 쿼리 또는 단일 열에서 문자열 `SELECT` 쿼리 합니다. 이 다른 여러 `Execute` 판독기 개체 또는 영향을 받는 행의 개수를 반환 하는 메서드.



## <a name="related-links"></a>관련 링크

- [DataAccess Basic (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 고급 (샘플)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android 데이터 레시피](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms 데이터 액세스](~/xamarin-forms/app-fundamentals/databases.md)
