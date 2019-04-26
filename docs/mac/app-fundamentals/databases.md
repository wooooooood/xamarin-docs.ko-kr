---
title: Xamarin.Mac의 데이터베이스
description: 이 문서에서는 키-값 코딩 및 관찰 SQLite 데이터베이스와 Xcode의 Interface Builder에서 UI 요소 간의 데이터 바인딩을 허용 하는 키-값을 사용 하 여 설명 합니다. SQLite.NET ORM을 사용 하 여 SQLite 데이터에 액세스할 수 있도록 대해서도 설명 합니다.
ms.prod: xamarin
ms.assetid: 44FAFDA8-612A-4E0F-8BB4-5C92A3F4D552
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 465b4a34d54dbee92461669b16c3b8a13188bbde
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61378345"
---
# <a name="databases-in-xamarinmac"></a>Xamarin.Mac의 데이터베이스

_이 문서에서는 키-값 코딩 및 관찰 SQLite 데이터베이스와 Xcode의 Interface Builder에서 UI 요소 간의 데이터 바인딩을 허용 하는 키-값을 사용 하 여 설명 합니다. SQLite.NET ORM을 사용 하 여 SQLite 데이터에 액세스할 수 있도록 대해서도 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Mac 응용 프로그램에서 C# 및.NET을 사용 하 여 작업을 하는 경우는 Xamarin.iOS 또는 Xamarin.Android 응용 프로그램에 액세스할 수 있는 동일한 SQLite 데이터베이스에 액세스할을 수 있습니다.

이 문서에서 우리는 주제 SQLite 데이터에 액세스 하는 두 가지 방법:

1. **직접 액세스** -SQLite 데이터베이스에 직접 액세스 하 여 키-값 코딩에 대 한 데이터베이스의에서 데이터를 사용할 수 있으며 Xcode의 Interface Builder에서 UI 요소를 사용 하 여 데이터 바인딩이 생성 합니다. 키-값 코딩 및 바인딩 기술 Xamarin.Mac 응용 프로그램에서 데이터를 사용 하 여 작성 하 고 채우는 UI 요소를 사용 하 여 작업을 유지 해야 하는 코드의 양을 크게 줄일 수 있습니다. 추가 백업 데이터를 분리의 장점은 수도 있습니다 (_데이터 모델_)에 처음부터 사용자 인터페이스를 종료 합니다 (_모델-뷰-컨트롤러_)를 더 쉬워진 유지 관리를 보다 융통성 있는 응용 프로그램 디자인 합니다.
2. **SQLite.NET ORM** -오픈 소스를 사용 하 여 [SQLite.NET](http://www.sqlite.org) 개체 관계 관리자 (ORM) SQLite 데이터베이스에서 데이터를 읽고 하는 데 필요한 코드의 양을 크게 줄일 수 있습니다 것입니다. 그런 다음이 데이터 테이블 뷰 같은 사용자 인터페이스 항목을 채우는 데 사용할 수 있습니다.

[![실행 중인 앱의 예가](databases-images/intro01.png "실행 중인 앱의 예")](databases-images/intro01-large.png#lightbox)

이 문서에서는 키-값 코딩 및 SQLite 데이터베이스를 사용 하 여 Xamarin.Mac 응용 프로그램에서 데이터 바인딩을 사용 하는 기본 사항을 설명 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서를 사용 하는 기술을 설명 합니다.

키-값 코딩 및 데이터 바인딩을 사용 합니다, 이후 하세요 진행 합니다 [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 먼저으로 핵심 기술 및 개념을 다룹니다이 설명서에서 해당 샘플에 사용할 응용 프로그램입니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 특성 요소 Objective-C 개체 및 UI에 C# 클래스를 연결 하는 데 사용 합니다.

## <a name="direct-sqlite-access"></a>SQLite에 대 한 직접 액세스

Xcode의 Interface Builder에서 UI 요소에 바인딩할 수는 SQLite 데이터에 대 한 것이 가장 좋습니다 직접 (달리 ORM 같은 기술을 사용 하 여) SQLite 데이터베이스를 액세스, 총 방식 제어할 수 없으므로 데이터를 작성 하 고 읽기  데이터베이스입니다.

살펴본 것 처럼 합니다 [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 문서, 키-값 코딩 및 데이터 바인딩 기술 Xamarin.Mac 응용 프로그램에서 사용 하 여 줄일 수 있습니다 크게 작성 해야 하는 코드의 양 및 채우고 UI 요소를 사용 하는 유지 관리 합니다. SQLite 데이터베이스에 대 한 직접 액세스를 결합 하면 해당 데이터베이스에 데이터를 읽고 하는 데 필요한 코드의 양을 크게 줄일 수 있습니다.

이 문서에서는 샘플 앱을 백업 원본으로는 SQLite 데이터베이스를 사용 하 여 바인딩에 대 한 데이터 바인딩 및 키-값 코딩 문서를 수정할 예정입니다.

### <a name="including-sqlite-database-support"></a>포함 하 여 SQLite 데이터베이스 지원

계속 수 전에 몇 가지에 대 한 참조를 포함 하 여 응용 프로그램에 SQLite 데이터베이스 지원을 추가 해야 합니다. Dll 파일입니다.

다음을 수행합니다.

1. 에 **Solution Pad**를 마우스 오른쪽 단추로 클릭 합니다 **참조** 폴더를 선택 **참조 편집**합니다.
2. 둘 다 선택 합니다 **Mono.Data.Sqlite** 하 고 **System.Data** 어셈블리: 

    [![필요한 참조를 추가](databases-images/reference01.png "필요한 참조를 추가 합니다.")](databases-images/reference01-large.png#lightbox)
3. 클릭 합니다 **확인** 단추 변경 내용을 저장 하 고 참조를 추가 합니다.

### <a name="modifying-the-data-model"></a>데이터 모델을 수정

응용 프로그램에 SQLite 데이터베이스에 직접 액세스 하는 것에 대 한 지원을 추가 했지만, 이제는 읽기 및 데이터베이스에서 데이터 쓰기 (뿐만 키-값 코딩 및 데이터 바인딩 제공)은 데이터 모델 개체를 수정 해야 합니다. 이 샘플 응용 프로그램의 경우 편집할 수는 **PersonModel.cs** 클래스 및 다음과 비슷하게 표시:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _ID = "";
        private string _managerID = "";
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        private SqliteConnection _conn = null;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        [Export("ID")]
        public string ID {
            get { return _ID; }
            set {
                WillChangeValue ("ID");
                _ID = value;
                DidChangeValue ("ID");
            }
        }

        [Export("ManagerID")]
        public string ManagerID {
            get { return _managerID; }
            set {
                WillChangeValue ("ManagerID");
                _managerID = value;
                DidChangeValue ("ManagerID");
            }
        }

        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }

        public PersonModel (string id, string name, string occupation)
        {
            // Initialize
            this.ID = id;
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (SqliteConnection conn, string id)
        {
            // Load from database
            Load (conn, id);
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion

        #region SQLite Routines
        public void Create(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Create new record ID?
            if (ID == "") {
                ID = Guid.NewGuid ().ToString();
            }

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Save manager ID and create the sub record
                Person.ManagerID = ID;
                Person.Create (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Update(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Update sub record
                Person.Update (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Load(SqliteConnection conn, string id) {
            bool shouldClose = false;

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Is the database already open?
            if (conn.State != ConnectionState.Open) {
                shouldClose = true;
                conn.Open ();
            }

            // Execute query
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", id);

                using (var reader = command.ExecuteReader ()) {
                    while (reader.Read ()) {
                        // Pull values back into class
                        ID = (string)reader [0];
                        Name = (string)reader [1];
                        Occupation = (string)reader [2];
                        isManager = (bool)reader [3];
                        ManagerID = (string)reader [4];
                    }
                }
            }

            // Is this a manager?
            if (isManager) {
                // Yes, load children
                using (var command = conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

                    // Populate with data from the record
                    command.Parameters.AddWithValue ("@COL1", id);

                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Load child and add to collection
                            var childID = (string)reader [0];
                            var person = new PersonModel (conn, childID);
                            _people.Add (person);
                        }
                    }
                }
            }

            // Should we close the connection to the database
            if (shouldClose) {
                conn.Close ();
            }

            // Save last connection
            _conn = conn;
        }

        public void Delete(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Empty class
            ID = "";
            ManagerID = "";
            Name = "";
            Occupation = "";
            isManager = false;
            _people = new NSMutableArray();

            // Save last connection
            _conn = conn;
        }
        #endregion
    }
}
```

아래에서 자세히 수정에 대해를 살펴보겠습니다.

첫째, 여러 추가한 SQLite를 사용 하는 데 필요한 문을 사용 하 여 있으며 마지막 연결 SQLite 데이터베이스에 저장할 변수를 추가 했습니다.

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection _conn = null;
```

이 저장 된 연결 사용 하 여 사용자 데이터 바인딩을 통해 UI에 콘텐츠를 수정 하는 경우 변경 내용을 데이터베이스에 레코드를 자동으로 저장 하겠습니다.

```csharp
[Export("Name")]
public string Name {
    get { return _name; }
    set {
        WillChangeValue ("Name");
        _name = value;
        DidChangeValue ("Name");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("Occupation")]
public string Occupation {
    get { return _occupation; }
    set {
        WillChangeValue ("Occupation");
        _occupation = value;
        DidChangeValue ("Occupation");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}
```

변경 내용을 합니다 **이름**, **직업** 또는 **isManager** 속성 하기 전에 데이터가 있는 저장 된 경우 데이터베이스에 전송 됩니다 (예를 들어 경우는 `_conn` 변수가 없는 `null`). 다음으로를 추가 하는 방법에 살펴보겠습니다 **만들기**, **업데이트**, **부하** 하 고 **삭제** 데이터베이스에서 사용자.

#### <a name="create-a-new-record"></a>새 레코드 만들기

다음 코드는 SQLite 데이터베이스에서 새 레코드를 만드는 추가 되었습니다.

```csharp
public void Create(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Create new record ID?
    if (ID == "") {
        ID = Guid.NewGuid ().ToString();
    }

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Save manager ID and create the sub record
        Person.ManagerID = ID;
        Person.Create (conn);
    }

    // Save last connection
    _conn = conn;
}
```

사용 하는 것을 `SQLiteCommand` 데이터베이스에 새 레코드를 만들어야 합니다. 새 명령을 가져오는 합니다 `SQLiteConnection` 는 전달 된 메서드를 호출 하 여 (연결) `CreateCommand`합니다. 다음으로 설정 새 레코드를 실제로 쓸 SQL 명령 실제 값에 대 한 매개 변수를 제공 합니다.

```csharp
command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";
```

나중에 사용 하 여 매개 변수 값 설정 합니다 `Parameters.AddWithValue` 메서드는 `SQLiteCommand`합니다. 매개 변수를 사용 하 여 값 (예: 작은따옴표) SQLite에 보내지기 전에 제대로 인코딩 있는지 확인 합니다. 예제:

```csharp
command.Parameters.AddWithValue ("@COL1", ID);
```

마지막으로, 사용자는 관리자 및 해당 직원의 컬렉션을 사용할 수, 있으므로 재귀적으로 호출 된 `Create` 메서드는 물론 데이터베이스에 저장 하는 사람들에:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Save manager ID and create the sub record
    Person.ManagerID = ID;
    Person.Create (conn);
}
```

#### <a name="updating-a-record"></a>레코드 업데이트

다음 코드는 SQLite 데이터베이스의 기존 레코드 업데이트에 추가 되었습니다.

```csharp
public void Update(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Update sub record
        Person.Update (conn);
    }

    // Save last connection
    _conn = conn;
}
```

와 같은 **만들기** 얻게 위의 `SQLiteCommand` 전달 된에서 `SQLiteConnection`, (매개 변수를 제공) 하는 레코드를 업데이트 하는 SQL 설정:

```csharp
command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";
```

매개 변수 값을 입력 했습니다 (예: `command.Parameters.AddWithValue ("@COL1", ID);`) 다시 재귀적으로 호출 업데이트 모든 자식 레코드:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Update sub record
    Person.Update (conn);
}
```
#### <a name="loading-a-record"></a>레코드를 로드합니다.

다음 코드는 SQLite 데이터베이스에서 기존 레코드를 로드에 추가 되었습니다.

```csharp
public void Load(SqliteConnection conn, string id) {
    bool shouldClose = false;

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Is the database already open?
    if (conn.State != ConnectionState.Open) {
        shouldClose = true;
        conn.Open ();
    }

    // Execute query
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Pull values back into class
                ID = (string)reader [0];
                Name = (string)reader [1];
                Occupation = (string)reader [2];
                isManager = (bool)reader [3];
                ManagerID = (string)reader [4];
            }
        }
    }

    // Is this a manager?
    if (isManager) {
        // Yes, load children
        using (var command = conn.CreateCommand ()) {
            // Create new command
            command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

            // Populate with data from the record
            command.Parameters.AddWithValue ("@COL1", id);

            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Load child and add to collection
                    var childID = (string)reader [0];
                    var person = new PersonModel (conn, childID);
                    _people.Add (person);
                }
            }
        }
    }

    // Should we close the connection to the database
    if (shouldClose) {
        conn.Close ();
    }

    // Save last connection
    _conn = conn;
}
```

루틴 (예: 해당 직원 개체를 로드 하는 관리자 개체)는 부모 개체에서 재귀적으로 호출할 수 있습니다, 되므로 특수 코드 열기 및 닫기는 데이터베이스로 연결을 처리 하도록 추가 되었습니다.

```csharp
bool shouldClose = false;
...

// Is the database already open?
if (conn.State != ConnectionState.Open) {
    shouldClose = true;
    conn.Open ();
}
...

// Should we close the connection to the database
if (shouldClose) {
    conn.Close ();
}

```

늘 그렇듯이 레코드를 검색 하 여 매개 변수를 사용 하 여 SQL 설정 했습니다.

```csharp
// Create new command
command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", id);
```

마지막으로를 사용 하 여 데이터 판독기 쿼리가 실행 되 고 레코드 필드를 반환 (의 인스턴스로 복사 하는 `PersonModel` 클래스).

```csharp
using (var reader = command.ExecuteReader ()) {
    while (reader.Read ()) {
        // Pull values back into class
        ID = (string)reader [0];
        Name = (string)reader [1];
        Occupation = (string)reader [2];
        isManager = (bool)reader [3];
        ManagerID = (string)reader [4];
    }
}
```

또한 모든 직원을 로드 해야이 사용자가 관리자 인 경우 (재귀적으로 호출 하 여 다시 해당 `Load` 메서드).

```csharp
// Is this a manager?
if (isManager) {
    // Yes, load children
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Load child and add to collection
                var childID = (string)reader [0];
                var person = new PersonModel (conn, childID);
                _people.Add (person);
            }
        }
    }
}
```

#### <a name="deleting-a-record"></a>레코드 삭제

다음 코드는 SQLite 데이터베이스에서 기존 레코드를 삭제 하려면 추가 되었습니다.

```csharp
public void Delete(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Empty class
    ID = "";
    ManagerID = "";
    Name = "";
    Occupation = "";
    isManager = false;
    _people = new NSMutableArray();

    // Save last connection
    _conn = conn;
}
```

여기에 SQL 관리자 레코드와 해당 관리자 (매개 변수 사용)에서 모든 직원의 레코드를 삭제를 제공 합니다.

```csharp
// Create new command
command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", ID);
```

현재 인스턴스를 취소 레코드 제거 된 후에 `PersonModel` 클래스:

```csharp
// Empty class
ID = "";
ManagerID = "";
Name = "";
Occupation = "";
isManager = false;
_people = new NSMutableArray();
```

### <a name="initializing-the-database"></a>데이터베이스를 초기화합니다.

읽기 및 쓰기 데이터베이스를 지원 하기 위해 현재 위치에서 데이터 모델 변경 내용을 사용 하 여 데이터베이스에 대 한 연결을 열고 첫 번째 실행에서 초기화 해야 합니다. 다음 코드를 추가 해 보겠습니다 우리의 **MainWindow.cs** 파일:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection DatabaseConnection = null;
...

private SqliteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "People.db3");

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);
    if (!exists)
        SqliteConnection.CreateFile (db);

    // Create connection to the database
    var conn = new SqliteConnection("Data Source=" + db);

    // Set the structure of the database
    if (!exists) {
        var commands = new[] {
            "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
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

        // Build list of employees
        var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
        Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
        Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
        Craig.Create (conn);

        var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
        Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
        Larry.Create (conn);
    }

    // Return new connection
    return conn;
}
```

위의 코드에 대해 좀 더 자세히 살펴보겠습니다. 첫째, 새 데이터베이스 (이 예제에서 사용자의 데스크톱), 데이터베이스가 있는지 확인 하 고 그렇지 않은 경우 만듭니다 위치 선택:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "People.db3");

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
if (!exists)
    SqliteConnection.CreateFile (db);
```

다음으로, 위에서 만든 경로 사용 하 여 데이터베이스에 연결 설정:

```csharp
var conn = new SqliteConnection("Data Source=" + db);
```

그런 다음 모든 SQL 테이블 필요한 데이터베이스에 만듭니다.

```csharp
var commands = new[] {
    "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
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
```

마지막으로 사용 하 여 데이터 모델 (`PersonModel`) 응용 프로그램 실행은 첫 번째 데이터베이스 시간 또는 데이터베이스가 없는 경우에 대 한 레코드의 기본 집합을 만들려면:

```csharp
// Build list of employees
var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
Craig.Create (conn);

var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
Larry.Create (conn);
``` 

응용 프로그램이 시작 되 고 주 창을 시작, 우리가 위에 추가한 코드를 사용 하 여 데이터베이스에 연결:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get access to database
    DatabaseConnection = GetDatabaseConnection ();
}
```

### <a name="loading-bound-data"></a>바인딩된 데이터 로드

해당 위치에서 SQLite 데이터베이스에서 바인딩된 데이터를 직접 액세스 하기 위한 모든 구성 요소를 사용 하 여 응용 프로그램을 제공 하 고 UI에 자동으로 표시 되는 다양 한 보기의 데이터를 로드할 수 있습니다.

#### <a name="loading-a-single-record"></a>단일 레코드를 로드합니다.

여기서 ID는 단일 레코드 알고, 로드 하려면 다음 코드를 사용할 수 있습니다.

```csharp
Person = new PersonModel (Conn, "0");
```

#### <a name="loading-all-records"></a>모든 레코드를 로드합니다.

경우 관리자는 여부에 관계 없이 모든 사용자를 로드 하려면 다음 코드를 사용 합니다.

```csharp
// Load all employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People]";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();

```

여기에서 사용 하 여 생성자의 오버 로드는 `PersonModel` 사용자를 메모리로 로드 하는 클래스:

```csharp
var person = new PersonModel (_conn, childID);
```

사용자 컬렉션에 사용자를 추가할 데이터 바인딩된 클래스도 이라고 `AddPerson (person)`, 이렇게 하면 UI 변경 내용을 인식 하 고 표시 합니다.

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}
```

#### <a name="loading-top-level-records-only"></a>최상위 수준 레코드만 로드

유일한 관리자 (예: 개요 보기에서 데이터를 표시)를 로드 하려면 다음 코드를 사용 했습니다.

```csharp
// Load only managers employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();
```

SQL 문의 차이점 (관리자만 로드 `command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1"`) 위의 섹션과 그렇지 않은 경우 동일 하 게 작동 하지만 합니다.

<a name="Databases-and-ComboBoxes" />

### <a name="databases-and-comboboxes"></a>데이터베이스 및 콤보 상자

(하거나 실행할 수 있는 Interface Builder에서 미리 정의 된 코드를 통해 채울) 내부 목록에서 드롭다운 목록을 채우는 데 사용할 수 있습니다 (예: 콤보 상자) macOS 메뉴 컨트롤을 설정할 수 있습니다 하거나 사용자 고유의 사용자 지정, 외부 데이터 소스를 제공 합니다. 참조 [제공 메뉴 컨트롤 데이터](~/mac/user-interface/standard-controls.md#Providing-Menu-Control-Data) 대 한 자세한 내용은 합니다.

예를 들어, 콤보 상자를 추가 Interface Builder에서 위의 단순 바인딩 예제를 편집 하 고 라는 콘센트를 사용 하 여 노출 `EmployeeSelector`:

[![콤보 상자 콘센트 노출](databases-images/combo01.png "콤보 상자 콘센트를 노출 합니다.")](databases-images/combo01-large.png#lightbox)

에 **특성 검사기**를 확인 합니다 **자동 완성** 및 **데이터 원본을 사용 하 여** 속성:

![콤보 상자 특성 구성](databases-images/combo02.png "콤보 상자 특성 구성")

변경 내용을 저장 하 고 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

#### <a name="providing-combobox-data"></a>콤보 상자 데이터를 제공합니다.

다음으로 프로젝트에 새 클래스를 추가 `ComboBoxDataSource` 다음과 같을 수 있도록 합니다.

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public class ComboBoxDataSource : NSComboBoxDataSource
    {
        #region Private Variables
        private SqliteConnection _conn = null;
        private string _tableName = "";
        private string _IDField = "ID";
        private string _displayField = "";
        private nint _recordCount = 0;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        public string TableName {
            get { return _tableName; }
            set { 
                _tableName = value;
                _recordCount = GetRecordCount ();
            }
        }

        public string IDField {
            get { return _IDField; }
            set {
                _IDField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public string DisplayField {
            get { return _displayField; }
            set { 
                _displayField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public nint RecordCount {
            get { return _recordCount; }
        }
        #endregion

        #region Constructors
        public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.DisplayField = displayField;
        }

        public ComboBoxDataSource (SqliteConnection conn, string tableName, string idField, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.IDField = idField;
            this.DisplayField = displayField;
        }
        #endregion

        #region Private Methods
        private nint GetRecordCount ()
        {
            bool shouldClose = false;
            nint count = 0;

            // Has a Table, ID and display field been specified?
            if (TableName !="" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read count from query
                            var result = (long)reader [0];
                            count = (nint)result;
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return the number of records
            return count;
        }
        #endregion

        #region Public Methods
        public string IDForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string ValueForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string IDForValue (string value)
        {
            NSString result = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", value);

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            result = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return result;
        }
        #endregion 

        #region Override Methods
        public override nint ItemCount (NSComboBox comboBox)
        {
            return RecordCount;
        }

        public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public override nint IndexOfItem (NSComboBox comboBox, string value)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";
            nint index = NSRange.NotFound;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read () && !found) {
                            // Read the display field from the query
                            field = (string)reader [0];
                            ++index;

                            // Is this the value we are searching for?
                            if (value == field) {
                                // Yes, exit loop
                                found = true;
                            }
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return index;
        }

        public override string CompletedString (NSComboBox comboBox, string uncompletedString)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Escape search string
                uncompletedString = uncompletedString.Replace ("'", "");

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            field = (string)reader [0];
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return field;
        }
        #endregion
    }
}
```

이 예에서 만들었습니다 새 `NSComboBoxDataSource` SQLite 데이터 소스에서 콤보 상자 항목을 제공할 수는 있습니다. 먼저, 다음 속성을 정의합니다.

- `Conn` -SQLite 데이터베이스에 대 한 연결을 설정 하거나 가져옵니다.
- `TableName` -테이블 이름을 가져오거나 설정 합니다를 가져옵니다.
- `IDField` -지정된 된 테이블에 대 한 고유 ID를 제공 하는 필드를 설정 하거나 가져옵니다. 기본값은 `ID`입니다.
- `DisplayField` -드롭다운 목록에 표시 되는 필드를 설정 하거나 가져옵니다.
- `RecordCount` -지정된 된 테이블의 레코드 수를 가져옵니다.

개체의 새 인스턴스를 만들 때 연결, 테이블, 필요에 따라 ID 필드 이름과 표시 필드에 전달 합니다.

```csharp
public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
{
    // Initialize
    this.Conn = conn;
    this.TableName = tableName;
    this.DisplayField = displayField;
}
```

`GetRecordCount` 메서드는 지정된 된 테이블에서 레코드의 수를 반환 합니다.

```csharp
private nint GetRecordCount ()
{
    bool shouldClose = false;
    nint count = 0;

    // Has a Table, ID and display field been specified?
    if (TableName !="" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read count from query
                    var result = (long)reader [0];
                    count = (nint)result;
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return the number of records
    return count;
}
```

언제 든 지 라고 합니다 `TableName`, `IDField` 또는 `DisplayField` 속성 값이 변경 합니다.

합니다 `IDForIndex` 고유 ID를 반환 하는 메서드 (`IDField`) 지정된 드롭다운 목록 항목 인덱스에 있는 레코드에 대 한 합니다. 

```csharp
public string IDForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

합니다 `ValueForIndex` 메서드 값을 반환 합니다 (`DisplayField`) 지정된 드롭다운 목록 인덱스의 항목에 대 한 합니다.

```csharp
public string ValueForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

합니다 `IDForValue` 고유 ID를 반환 하는 메서드 (`IDField`) 지정 된 값 (`DisplayField`):

```csharp
public string IDForValue (string value)
{
    NSString result = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", value);

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    result = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return result;
}
```

`ItemCount` 계산 하는 경우 목록에서 항목의 사전 계산된 수를 반환 합니다 `TableName`, `IDField` 또는 `DisplayField` 속성이 변경 된:

```csharp
public override nint ItemCount (NSComboBox comboBox)
{
    return RecordCount;
}
```

합니다 `ObjectValueForItem` 값을 제공 하는 메서드 (`DisplayField`) 지정된 드롭다운 목록 항목 인덱스:

```csharp
public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

사용 하는 `LIMIT` 및 `OFFSET` 필요 함을에서는 하나의 레코드를 제한 하는 SQLite 명령에서 문.

합니다 `IndexOfItem` 메서드 반환 값의 드롭다운 항목 인덱스 (`DisplayField`) 지정 합니다.

```csharp
public override nint IndexOfItem (NSComboBox comboBox, string value)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";
    nint index = NSRange.NotFound;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read () && !found) {
                    // Read the display field from the query
                    field = (string)reader [0];
                    ++index;

                    // Is this the value we are searching for?
                    if (value == field) {
                        // Yes, exit loop
                        found = true;
                    }
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return index;
}
```

값을 찾을 수 없는 경우는 `NSRange.NotFound` 값이 반환 하 고 드롭다운 목록에서 모든 항목이 선택 취소 합니다.

`CompletedString` 첫 번째 일치 하는 값을 반환 하는 메서드 (`DisplayField`) 부분적으로 형식화 된 항목에 대 한 합니다.

```csharp
public override string CompletedString (NSComboBox comboBox, string uncompletedString)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Escape search string
        uncompletedString = uncompletedString.Replace ("'", "");

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    field = (string)reader [0];
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return field;
}
```

#### <a name="displaying-data-and-responding-to-events"></a>데이터를 표시 하 고 이벤트에 응답

모든 부분이 함께 표시 하려면 편집을 `SubviewSimpleBindingController` 수 있으며 다음과 같은 모양:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public partial class SubviewSimpleBindingController : AppKit.NSViewController
    {
        #region Private Variables
        private PersonModel _person = new PersonModel();
        private SqliteConnection Conn;
        #endregion

        #region Computed Properties
        //strongly typed view accessor
        public new SubviewSimpleBinding View {
            get {
                return (SubviewSimpleBinding)base.View;
            }
        }

        [Export("Person")]
        public PersonModel Person {
            get {return _person; }
            set {
                WillChangeValue ("Person");
                _person = value;
                DidChangeValue ("Person");
            }
        }

        public ComboBoxDataSource DataSource {
            get { return EmployeeSelector.DataSource as ComboBoxDataSource; }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public SubviewSimpleBindingController (IntPtr handle) : base (handle)
        {
            Initialize ();
        }

        // Called when created directly from a XIB file
        [Export ("initWithCoder:")]
        public SubviewSimpleBindingController (NSCoder coder) : base (coder)
        {
            Initialize ();
        }

        // Call to load from the XIB/NIB file
        public SubviewSimpleBindingController (SqliteConnection conn) : base ("SubviewSimpleBinding", NSBundle.MainBundle)
        {
            // Initialize
            this.Conn = conn;
            Initialize ();
        }

        // Shared initialization code
        void Initialize ()
        {
        }
        #endregion

        #region Private Methods
        private void LoadSelectedPerson (string id)
        {

            // Found?
            if (id != "") {
                // Yes, load requested record
                Person = new PersonModel (Conn, id);
            }
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Configure Employee selector dropdown
            EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");

            // Wireup events
            EmployeeSelector.Changed += (sender, e) => {
                // Get ID
                var id = DataSource.IDForValue (EmployeeSelector.StringValue);
                LoadSelectedPerson (id);
            };

            EmployeeSelector.SelectionChanged += (sender, e) => {
                // Get ID
                var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
                LoadSelectedPerson (id);
            };

            // Auto select the first person
            EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
            Person = new PersonModel (Conn, DataSource.IDForIndex(0));
    
        }
        #endregion
    }
}
```

합니다 `DataSource` 속성에 대 한 바로 가기를 제공 합니다 `ComboBoxDataSource` (위에서 만든) 콤보 상자에 연결 합니다.

`LoadSelectedPerson` 메서드는 데이터베이스에서 지정된 된 고유 ID에 대 한 사용자를 로드 합니다.

```csharp
private void LoadSelectedPerson (string id)
{

    // Found?
    if (id != "") {
        // Yes, load requested record
        Person = new PersonModel (Conn, id);
    }
}
```

에 `AwakeFromNib` 메서드 재정의 먼저 사용자 지정 콤보 상자의 데이터 원본 인스턴스에 연결:

```csharp
EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");
```

연결 된 고유 ID를 검색 하 여 콤보 상자는 텍스트 값을 편집 하는 사용자에 대응 어 (`IDField`) 데이터의 표시 하는 경우 지정 된 사용자를 로드 및 찾을:

```csharp
EmployeeSelector.Changed += (sender, e) => {
    // Get ID
    var id = DataSource.IDForValue (EmployeeSelector.StringValue);
    LoadSelectedPerson (id);
};
```

또한 사용자가 드롭다운 목록에서 새 항목을 선택 하면 새 사용자를 로드:

```csharp
EmployeeSelector.SelectionChanged += (sender, e) => {
    // Get ID
    var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
    LoadSelectedPerson (id);
};
```

마지막으로,에서는 자동으로 채우려면 콤보 상자 및 목록에서 첫 번째 항목을 사용 하 여 표시 된 사용자:

```csharp
// Auto select the first person
EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
Person = new PersonModel (Conn, DataSource.IDForIndex(0));
```

## <a name="sqlitenet-orm"></a>SQLite.NET ORM

오픈 소스를 사용 하 여 설명한 것 처럼 [SQLite.NET](http://www.sqlite.org) 개체 관계 관리자 (ORM) SQLite 데이터베이스에서 데이터를 읽고 하는 데 필요한 코드의 양을 크게 줄일 수 있습니다 것입니다. 다양 한 키-값 코딩 및 데이터 바인딩 개체에 배치 하는 요구 사항으로 인해 데이터를 바인딩할 때 수행할 최적의 경로 아닐 수 있습니다.

SQLite.Net 웹 사이트에 따라 _"SQLite는 자체 포함 된, 서버 리스, 0-구성, 트랜잭션 SQL 데이터베이스 엔진을 구현 하는 소프트웨어 라이브러리입니다. SQLite는 전 세계에서 가장 광범위 하 게 배포 된 데이터베이스 엔진입니다. SQLite에 대 한 소스 코드에 공용 도메인. "_

다음 섹션에서는 SQLite.Net를 사용 하 여 표 보기에 대 한 데이터를 제공 하는 방법을 살펴보겠습니다.

### <a name="including-the-sqlitenet-nuget"></a>SQLite.net NuGet을 포함 하 여

SQLite.NET 응용 프로그램에서 사용 하는 NuGet 패키지로 제공 됩니다. SQLite.NET를 사용 하 여 데이터베이스 지원을 추가할 수 있습니다, 전에이 패키지를 포함 해야 합니다.

패키지를 추가 하려면 다음을 수행 합니다.

1. 에 **Solution Pad**를 마우스 오른쪽 단추로 클릭 합니다 **패키지** 폴더를 선택 **패키지 추가...**
2. 입력 `SQLite.net` 에 **검색 상자** 선택 합니다 **sqlite net** 항목:

    [![SQLite NuGet 패키지 추가](databases-images/nuget01.png "SQLite NuGet 패키지 추가")](databases-images/nuget01-large.png#lightbox)
3. 클릭 합니다 **패키지 추가** 완료 단추입니다.

### <a name="creating-the-data-model"></a>데이터 모델 만들기

프로젝트에 새 클래스를 추가 하 고 호출 하겠습니다 `OccupationModel`합니다. 다음으로, 편집할를 **OccupationModel.cs** 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using SQLite;

namespace MacDatabase
{
    public class OccupationModel
    {
        #region Computed Properties
        [PrimaryKey, AutoIncrement]
        public int ID { get; set; }

        public string Name { get; set;}
        public string Description { get; set;}
        #endregion

        #region Constructors
        public OccupationModel ()
        {
        }

        public OccupationModel (string name, string description)
        {

            // Initialize
            this.Name = name;
            this.Description = description;

        }
        #endregion
    }
}
```

SQLite.NET 포함 먼저 (`using Sqlite`), 다음 몇 가지 속성을 노출 했습니다 각각 쓰입니다 데이터베이스로이 레코드를 저장할 때. 첫 번째 속성의 기본 키로 확인 하 고 다음과 같이 자동 증가를 설정 합니다.

```csharp
[PrimaryKey, AutoIncrement]
public int ID { get; set; }
```
### <a name="initializing-the-database"></a>데이터베이스를 초기화합니다.

읽기 및 쓰기 데이터베이스를 지원 하기 위해 현재 위치에서 데이터 모델 변경 내용을 사용 하 여 데이터베이스에 대 한 연결을 열고 첫 번째 실행에서 초기화 해야 합니다. 다음 코드를 추가 해 보겠습니다.

```csharp
using SQLite;
...

public SQLiteConnection Conn { get; set; }
...

private SQLiteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "Occupation.db3");
    OccupationModel Occupation;

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);

    // Create connection to database
    var conn = new SQLiteConnection (db);

    // Initially populate table?
    if (!exists) {
        // Yes, build table
        conn.CreateTable<OccupationModel> ();

        // Add occupations
        Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
        conn.Insert (Occupation);
    }

    return conn;
}
```

먼저 데이터베이스 (이 경우 사용자의 데스크톱)에 대 한 경로 가져옵니다 하 고 데이터베이스가 이미 있는 경우 참조:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "Occupation.db3");
OccupationModel Occupation;

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
```

다음으로, 위에서 만든 경로에 데이터베이스 연결을 설정 합니다.

```csharp
var conn = new SQLiteConnection (db);
```

마지막으로 테이블을 만들 하 고 일부 기본 레코드를 추가 합니다.

```csharp
// Yes, build table
conn.CreateTable<OccupationModel> ();

// Add occupations
Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
conn.Insert (Occupation);
```

### <a name="adding-a-table-view"></a>테이블 뷰 추가

사용 예제와 Xcode의 Interface builder에서 ui 테이블 뷰를 추가 하겠습니다. 이 테이블 뷰의 콘센트를 통해 노출 됩니다 (`OccupationTable`) C# 코드를 통해 액세스할 수 있도록 합니다.

[![테이블 보기 콘센트 노출](databases-images/table01.png "테이블 보기 콘센트를 노출 합니다.")](databases-images/table01-large.png#lightbox)

다음으로, SQLite.NET 데이터베이스에서 데이터를 사용 하 여이 테이블을 채우는 데 사용자 지정 클래스를 추가 하겠습니다.

### <a name="creating-the-table-data-source"></a>테이블 데이터 원본 만들기

테이블에 대 한 데이터를 제공 하는 사용자 지정 데이터 소스를 만들어 보겠습니다. 라는 새 클래스를 먼저 추가 `TableORMDatasource` 다음과 같을 수 있도록 합니다.

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDatasource : NSTableViewDataSource
    {
        #region Computed Properties
        public List<OccupationModel> Occupations { get; set;} = new List<OccupationModel>();
        public SQLiteConnection Conn { get; set; }
        #endregion

        #region Constructors
        public TableORMDatasource (SQLiteConnection conn)
        {
            // Initialize
            this.Conn = conn;
            LoadOccupations ();
        }
        #endregion

        #region Public Methods
        public void LoadOccupations() {

            // Get occupations from database
            var query = Conn.Table<OccupationModel> ();

            // Copy into table collection
            Occupations.Clear ();
            foreach (OccupationModel occupation in query) {
                Occupations.Add (occupation);
            }

        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Occupations.Count;
        }
        #endregion
    }
}
```

나중에이 클래스의 인스턴스를 만들 때이 열린 SQLite.NET 데이터베이스 연결에 전달 합니다. 합니다 `LoadOccupations` 메서드는 데이터베이스에 쿼리하고 찾은 레코드 메모리로 복사 (사용 하 여이 `OccupationModel` 데이터 모델).

### <a name="creating-the-table-delegate"></a>테이블 대리자 만들기

필요한 최종 클래스는 SQLite.NET 데이터베이스에서 로드 정보를 표시 하는 사용자 지정 테이블 대리자입니다. 새를 추가 해 보겠습니다 `TableORMDelegate` 프로젝트에 다음과 같을 수 있도록 합니다.

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDelegate : NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "OccCell";
        #endregion

        #region Private Variables
        private TableORMDatasource DataSource;
        #endregion

        #region Constructors
        public TableORMDelegate (TableORMDatasource dataSource)
        {
            // Initialize
            this.DataSource = dataSource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Occupation":
                view.StringValue = DataSource.Occupations [(int)row].Name;
                break;
            case "Description":
                view.StringValue = DataSource.Occupations [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

여기서 사용 하 여 데이터 원본의 `Occupations` 컬렉션 (즉 SQLite.NET 데이터베이스에서 로드)를 통해 테이블의 열에 맞게는 `GetViewForItem` 메서드 재정의 합니다.

### <a name="populating-the-table"></a>테이블 채우기

모든 위치에서 요소를 사용 하 여 보겠습니다 테이블 때 채울.xib 파일에서 재정의 하 여 팽창 됩니다는 `AwakeFromNib` 메서드와 보이게 하 여 다음과 같은:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get database connection
    Conn = GetDatabaseConnection ();

    // Create the Occupation Table Data Source and populate it
    var DataSource = new TableORMDatasource (Conn);

    // Populate the Product Table
    OccupationTable.DataSource = DataSource;
    OccupationTable.Delegate = new TableORMDelegate (DataSource);
}
```

먼저 것에 액세스할 SQLite.NET 데이터베이스를 만들고 이미 존재 하지 않는 경우 채우기 있습니다. 새 인스턴스를 만들겠습니다. 그런 다음 사용자 지정 테이블 데이터 원본에서의 데이터베이스 연결에서 전달 하 고 테이블에 연결 했습니다. 마지막으로,이 사용자 지정 테이블 대리자의 새 인스턴스를 만들고, 데이터 원본에 전달 하 고 테이블에 연결 합니다.

## <a name="summary"></a>요약

이 문서에서는 자세히 살펴보고 데이터 바인딩 및 Xamarin.Mac 응용 프로그램에서 SQLite 데이터베이스를 사용 하 여 키-값 코딩 작업을 수행 했습니다. 첫째,이 키-값 관찰 (KVO) 및 키-값 코딩 (KVC)를 사용 하 여 Objective-c를 C# 클래스를 노출 살펴보았습니다. 다음으로 KVO 호환 클래스를 사용 하는 방법에 알아보았습니다 하 고 데이터 Xcode의 Interface Builder에서 UI 요소에 바인딩합니다. 문서는 SQLite.NET ORM 통해 SQLite 데이터로 작업 하 고 해당 데이터를 테이블 보기로 표시에 대해서도 다룹니다.



## <a name="related-links"></a>관련 링크

- [MacDatabase (샘플)](https://developer.xamarin.com/samples/mac/MacDatabase/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [표준 컨트롤](~/mac/user-interface/standard-controls.md)
- [테이블 뷰](~/mac/user-interface/table-view.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [컬렉션 뷰](~/mac/user-interface/collection-view.md)
- [키-값 코딩 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Cocoa 바인딩 프로그래밍 항목 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Cocoa 바인딩 참조 소개](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS 휴먼 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
