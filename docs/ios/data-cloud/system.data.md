---
title: System.Data
ms.topic: article
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 145a0692ed9761944eec4c7ece1f098a584f2d54
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="systemdata"></a>System.Data

에 대 한 지원을 추가 하는 Xamarin.iOS 8.10 [System.Data](https://developer.xamarin.com/api/namespace/System.Data/)를 포함 하 여는 `Mono.Data.Sqlite.dll` ADO.NET 공급자입니다. 지원 됩니다. 다음의 추가 [어셈블리](~/cross-platform/internals/available-assemblies.md):

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`


<a name="Example" />

## <a name="example"></a>예제

다음 프로그램에서 데이터베이스를 만듭니다. `Documents/mydb.db3`, 하는 경우는 이전에 데이터베이스가 없습니다 것 및 샘플 데이터로 채워집니다. 데이터베이스를 쿼리 한 다음의 출력에 기록 `stderr`합니다.

### <a name="add-references"></a>참조 추가

첫째, 마우스 오른쪽 단추로 클릭는 **참조** 노드 선택 **참조 편집...**  다음 선택 `System.Data` 및 `Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "새 참조 추가")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>샘플 코드

다음 코드에서는 테이블 만들기 및 포함 된 SQL 명령을 사용 하 여 행 삽입의 간단한 예를 보여 줍니다.

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;

class Demo {
    static void Main (string [] args)
    {
        var connection = GetConnection ();
        using (var cmd = connection.CreateCommand ()) {
            connection.Open ();
            cmd.CommandText = "SELECT * FROM People";
            using (var reader = cmd.ExecuteReader ()) {
                while (reader.Read ()) {
                    Console.Error.Write ("(Row ");
                    Write (reader, 0);
                    for (nint i = 1; i < reader.FieldCount; ++i) {
                        Console.Error.Write(" ");
                        Write (reader, i);
                    }
                    Console.Error.WriteLine(")");
                }
            }
            connection.Close ();
        }
    }

    static SqliteConnection GetConnection()
    {
        var documents = Environment.GetFolderPath (
                Environment.SpecialFolder.Personal);
        string db = Path.Combine (documents, "mydb.db3");
        bool exists = File.Exists (db);
        if (!exists)
            SqliteConnection.CreateFile (db);
        var conn = new SqliteConnection("Data Source=" + db);
        if (!exists) {
            var commands = new[] {
            "CREATE TABLE People (PersonID INTEGER NOT NULL, FirstName ntext, LastName ntext)",
            // WARNING: never insert user-entered data with embedded parameter values
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (1, 'First', 'Last')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (2, 'Dewey', 'Cheatem')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (3, 'And', 'How')",
            };
            conn.Open ();
            foreach (var cmd in commands) {
                using (var c = conn.CreateCommand()) {
                    c.CommandText = cmd;
                    c.CommandType = CommandType.Text;
                    c.ExecuteNonQuery ();
                }
            }
            conn.Close ();
        }
        return conn;
    }

    static void Write(SqliteDataReader reader, int index)
    {
        Console.Error.Write("({0} '{1}')",
                reader.GetName(index),
                reader [index]);
    }
}
```

> [!IMPORTANT]
> 위의 코드 예제에서 설명 했 듯이 것은 코드에 노출 하면 문자열 SQL 명령에 포함할 적절 [SQL 주입](http://en.wikipedia.org/wiki/SQL_injection)합니다.


### <a name="using-command-parameters"></a>명령 매개 변수를 사용 하 여

다음 코드에는 명령 매개 변수 (있는 경우에 텍스트 단일 아포스트로피와 같은 특수 SQL 문자가) 데이터베이스에 안전 하 게 사용자가 입력 한 텍스트 삽입을 사용 하는 방법을 보여 줍니다.

```csharp
// user input from Textbox control
var fname = fnameTextbox.Text;
var lname = lnameTextbox.Text;
// use command parameters to safely insert into database
using (var addCmd = conn.CreateCommand ()) {
    addCmd.CommandText = "INSERT INTO [People] (PersonID, FirstName, LastName) VALUES (@COL1, @COL2, @COL3)";
    addCmd.CommandType = System.Data.CommandType.Text;
    addCmd.AddParameterWithValue ("@COL1", 1);
    addCmd.AddParameterWithValue ("@COL2", fname);
    addCmd.AddParameterWithValue ("@COL3", lname);
    addCmd.ExecuteNonQuery ();
}
```

<a name="Missing_Functionality" />

## <a name="missing-functionality"></a>누락 된 기능

둘 다 **System.Data** 및 **Mono.Data.Sqlite** 일부 기능이 없습니다.

<a name="System.Data" />

### <a name="systemdata"></a>System.Data

누락 된 기능 **System.Data.dll** 이루어져 있습니다.

-  어느 것에 필요한 [System.CodeDom](https://developer.xamarin.com/api/namespace/System.CodeDom/) (예:  [System.Data.TypedDataSetGenerator](https://developer.xamarin.com/api/type/System.Data.TypedDataSetGenerator/) )
-  XML 구성 파일 (예: 지원  [System.Data.Common.DbProviderConfigurationHandler](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderConfigurationHandler/) )
-   [System.Data.Common.DbProviderFactories](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderFactories/) (XML 구성 파일 지원에 따라 다름)
-   [System.Data.OleDb](https://developer.xamarin.com/api/namespace/System.Data.OleDb/)
-   [System.Data.Odbc](https://developer.xamarin.com/api/namespace/System.Data.Odbc/)
-  `System.EnterpriseServices.dll` 종속성은 *제거* 에서 `System.Data.dll` 로 제거 하 고는 [SqlConnection.EnlistDistributedTransaction(ITransaction)](https://developer.xamarin.com/api/member/System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction/(System.EnterpriseServices.ITransaction)) 메서드.


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

한편, **Mono.Data.Sqlite.dll** 소스 코드 변경 없이 발생 하지만 대신 다양 한 호스트를 수 있습니다 *런타임* 이후에 발급 `Mono.Data.Sqlite.dll` SQLite 3.5에 바인딩합니다. 8, iOS는 한편, SQLite 3.8.5 함께 제공 됩니다. 말할 살펴보겠지만 일부의 원인 두 버전 간에 변경 되었습니다.

이전 버전 iOS의 다음 버전의 SQLite 함께 제공:

- **iOS 7** -3.7.13 버전입니다.
- **iOS 6** -3.7.13 버전입니다.
- **iOS 5** -3.7.7 버전입니다.
- **iOS 4** -3.6.22 버전입니다.

예를 들어 열와 같은 지정된 된 테이블에 존재 하는 런타임 시 결정 데이터베이스 스키마는 쿼리 하 나와 관련 되어야 하는 가장 일반적인 문제 표시 `Mono.Data.Sqlite.SqliteConnection.GetSchema` (재정의 [DbConnection.GetSchema](https://developer.xamarin.com/api/member/System.Data.Common.DbConnection.GetSchema/)) 및 `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` 드 ( 재정의 [DbDataReader.GetSchemaTable](https://developer.xamarin.com/api/member/System.Data.Common.DbDataReader.GetSchemaTable/)). 즉, 모두를 사용 하 여 보이는 [DataTable](https://developer.xamarin.com/api/type/System.Data.DataTable/) 작동 되지 않습니다.

<a name="Data_Binding" />

## <a name="data-binding"></a>데이터 바인딩

이 이번에는 데이터 바인딩을 지원 되지 않습니다.

