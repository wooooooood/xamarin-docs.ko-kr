---
title: Xamarin.ios의 system.object
description: 이 문서에서는 Xamarin.ios 응용 프로그램의 SQLite 데이터에 액세스 하기 위해 System.web 및 Mono를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: 736d70aebcf861b5557d5f076a42ff0a3dcfc043
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569957"
---
# <a name="systemdata-in-xamarinios"></a>Xamarin.ios의 system.object

Xamarin.ios 8.10은 ADO.NET 공급자를 비롯 한 [system.object](xref:System.Data)에 대 한 지원을 추가 합니다. `Mono.Data.Sqlite.dll` 지원에는 다음 [어셈블리](~/cross-platform/internals/available-assemblies.md)를 추가 하는 기능이 포함 됩니다.

- `System.Data.dll`
- `System.Data.Service.Client.dll`
- `System.Transactions.dll`
- `Mono.Data.Tds.dll`
- `Mono.Data.Sqlite.dll`

<a name="Example"></a>

## <a name="example"></a>예제

다음 프로그램은에서 데이터베이스를 만들고 `Documents/mydb.db3` , 데이터베이스가 이전에 존재 하지 않는 경우 예제 데이터로 채워집니다. 그런 다음 출력을 기록 하 여 데이터베이스를 쿼리 합니다 `stderr` .

### <a name="add-references"></a>참조 추가

먼저 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 편집** ...을 선택 하 고 다음을 선택 합니다. `System.Data` `Mono.Data.Sqlite`

[![](system.data-images/edit-references-sml.png "Adding new references")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>샘플 코드

다음 코드에서는 테이블을 만들고 포함 된 SQL 명령을 사용 하 여 행을 삽입 하는 간단한 예를 보여 줍니다.

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
> 위의 코드 샘플에 설명 된 것 처럼 sql 명령에 문자열을 포함 하는 것은 코드를 [sql 삽입](https://en.wikipedia.org/wiki/SQL_injection)에 취약 하 게 만드는 것이 좋습니다.

### <a name="using-command-parameters"></a>Command 매개 변수 사용

다음 코드에서는 명령 매개 변수를 사용 하 여 사용자가 입력 한 텍스트를 데이터베이스에 안전 하 게 삽입 하는 방법을 보여 줍니다 (텍스트에 단일 아포스트로피와 같은 특수 SQL 문자가 포함 된 경우에도).

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

<a name="Missing_Functionality"></a>

## <a name="missing-functionality"></a>누락 된 기능

**System.object** 와 **Mono. Sqlite** 에는 일부 기능이 없습니다.

<a name="System.Data"></a>

### <a name="systemdata"></a>System.Data

**System.object** 에서 누락 된 기능은 다음과 같이 구성 됩니다.

- [시스템 CodeDom](xref:System.CodeDom) 을 필요로 하는 모든 항목 (예:  [TypedDataSetGenerator](xref:System.Data.TypedDataSetGenerator) )
- XML 구성 파일 지원 (예:  [System.web. DbProviderConfigurationHandler](xref:System.Data.Common.DbProviderConfigurationHandler) )
- [System.web. DbProviderFactories](xref:System.Data.Common.DbProviderFactories) (XML 구성 파일 지원에 따라 달라 짐)
- [System.Data.OleDb](xref:System.Data.OleDb)
- [System.Data.Odbc](xref:System.Data.Odbc)
- `System.EnterpriseServices.dll`종속성이에서 *제거* 되었으므로 `System.Data.dll` [SqlConnection. EnlistDistributedTransaction (ITransaction)](xref:System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction*) 메서드가 제거 됩니다.

<a name="Mono.Data.Sqlite"></a>

### <a name="monodatasqlite"></a>Mono. Sqlite

한편, **Mono. a l l. a l l. a l l. l** i 3.5 d. *runtime* `Mono.Data.Sqlite.dll` iOS 8은 SQLite 3.8.5와 함께 제공 됩니다. 즉, 두 버전 간에 몇 가지 사항이 변경 되었습니다.

이전 버전의 iOS에는 다음 버전의 SQLite가 제공 됩니다.

- **iOS 7** -버전 3.7.13.
- **iOS 6** -버전 3.7.13.
- **iOS 5** -버전 3.7.7.
- **iOS 4** -버전 3.6.22.

가장 일반적인 문제는 데이터베이스 스키마 쿼리와 관련이 있습니다. 예를 들어 런타임에 지정 된 테이블에 존재 하는 열을 결정 하는 것과 같습니다. 예를 들어 `Mono.Data.Sqlite.SqliteConnection.GetSchema` [DbConnection](xref:System.Data.Common.DbConnection.GetSchema) 및를 재정의 하 여 `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` [dbdatareader](xref:System.Data.Common.DbDataReader.GetSchemaTable)를 재정의 합니다. 즉, [DataTable](xref:System.Data.DataTable) 을 사용 하는 모든 항목이 작동 하지 않는 것 같습니다.

<a name="Data_Binding"></a>

## <a name="data-binding"></a>데이터 바인딩

지금은 데이터 바인딩이 지원 되지 않습니다.
