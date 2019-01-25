---
title: Xamarin.iOS System.Data
description: 이 문서에는 System.Data 및 Mono.Data.Sqlite.dll Xamarin.iOS 응용 프로그램에서 SQLite 데이터 액세스를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/25/2015
ms.openlocfilehash: 014de47660f2c0ac8295495e417b3d5def135470
ms.sourcegitcommit: ee626f215de02707b7a94ba1d0fa1d75b22ab84f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54879318"
---
# <a name="systemdata-in-xamarinios"></a>Xamarin.iOS System.Data

에 대 한 지원을 추가 하는 Xamarin.iOS 8.10 [System.Data](xref:System.Data)등는 `Mono.Data.Sqlite.dll` ADO.NET 공급자입니다. 지원에 다음 추가 포함 [어셈블리](~/cross-platform/internals/available-assemblies.md):

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`

<a name="Example" />

## <a name="example"></a>예제

다음 프로그램에서 데이터베이스를 만듭니다 `Documents/mydb.db3`, 경우 데이터베이스가 이전에 존재 하지 하 고 샘플 데이터로 채워집니다. 에 쓴 출력을 사용 하 여 데이터베이스를 쿼리 한 다음 `stderr`합니다.

### <a name="add-references"></a>참조 추가

첫째, 마우스 오른쪽 단추로 클릭 합니다 **참조** 노드 선택 **참조 편집...**  선택한 `System.Data` 고 `Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "새 참조 추가")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>샘플 코드

다음 코드 예제에서는 테이블 만들기 및 포함 된 SQL 명령을 사용 하 여 행 삽입을 보여 줍니다.

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
> 것은 바람직하지 만들므로 코드에 취약 한 SQL 명령에서 문자열을 포함 하 고 위의 코드 샘플에서 설명 했 듯이 [SQL 주입](http://en.wikipedia.org/wiki/SQL_injection)합니다.


### <a name="using-command-parameters"></a>Command 매개 변수 사용

다음 코드 (경우에 단일 아포스트로피 같은 특수 SQL 문자를 포함 하는 텍스트) 데이터베이스에 사용자가 입력 한 텍스트를 안전 하 게 삽입 하려면 명령 매개 변수를 사용 하는 방법을 보여 줍니다.

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

둘 다 **System.Data** 하 고 **Mono.Data.Sqlite** 일부 기능이 누락 됩니다.

<a name="System.Data" />

### <a name="systemdata"></a>System.Data

누락 된 기능 **System.Data.dll** 이루어져 있습니다.

-  아무 것도 필요한 [System.CodeDom](xref:System.CodeDom) (예:  [System.Data.TypedDataSetGenerator](xref:System.Data.TypedDataSetGenerator) )
-  XML 구성 파일 (예: 지원  [System.Data.Common.DbProviderConfigurationHandler](xref:System.Data.Common.DbProviderConfigurationHandler) )
-   [System.Data.Common.DbProviderFactories](xref:System.Data.Common.DbProviderFactories) (XML 구성 파일 지원에 따라 다름)
-   [System.Data.OleDb](xref:System.Data.OleDb)
-   [System.Data.Odbc](xref:System.Data.Odbc)
-  합니다 `System.EnterpriseServices.dll` 종속성 되었습니다 *제거* 에서 `System.Data.dll` 제거에서 결과, 합니다 [SqlConnection.EnlistDistributedTransaction(ITransaction)](xref:System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction*) 메서드.


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

한편 **Mono.Data.Sqlite.dll** 소스 코드 변경 없이 발생 하지만 대신 다양 한 호스트 될 수 있습니다 *런타임* 이후에 발급 `Mono.Data.Sqlite.dll` SQLite 3.5를 바인딩합니다. 한편, 8, iOS 3.8.5 SQLite를 사용 하 여 제공 됩니다. 말할 필요도 없이 몇 가지 작업은 두 버전 간에 변경 되었습니다.

이전 버전 iOS의 다음 버전의 SQLite 사용 하 여 제공:

- **iOS 7** -3.7.13 버전입니다.
- **iOS 6** -3.7.13 버전입니다.
- **iOS 5** -3.7.7 버전입니다.
- **iOS 4** -3.6.22 버전입니다.

예를 들어 열과 같은 지정된 된 테이블에 존재 하는 런타임 시 결정 데이터베이스 스키마는 쿼리를 관련 된 것으로 표시 되는 가장 일반적인 문제 `Mono.Data.Sqlite.SqliteConnection.GetSchema` (재정의 [DbConnection.GetSchema](xref:System.Data.Common.DbConnection.GetSchema) 고 `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` (재정의 [DbDataReader.GetSchemaTable](xref:System.Data.Common.DbDataReader.GetSchemaTable)합니다. 즉,에 아무 것도 사용 하 여 보입니다 [DataTable](xref:System.Data.DataTable) 작동할 것입니다.

<a name="Data_Binding" />

## <a name="data-binding"></a>데이터 바인딩

이 이번에는 데이터 바인딩을 지원 되지 않습니다.

