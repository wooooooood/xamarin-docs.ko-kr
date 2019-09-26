---
title: Xamarin.ios의 데이터베이스
description: 이 문서에서는 키-값 코딩 및 키-값 관찰을 사용 하 여 SQLite 데이터베이스와 Xcode Interface Builder의 UI 요소 간 데이터 바인딩을 허용 하는 방법을 설명 합니다. SQLite.NET ORM을 사용 하 여 SQLite 데이터에 대 한 액세스를 제공 하는 방법도 다룹니다.
ms.prod: xamarin
ms.assetid: 44FAFDA8-612A-4E0F-8BB4-5C92A3F4D552
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 03886a53e4f737b1e874a756f8801e46c7de4d32
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "70769906"
---
# <a name="databases-in-xamarinmac"></a>Xamarin.ios의 데이터베이스

_이 문서에서는 키-값 코딩 및 키-값 관찰을 사용 하 여 SQLite 데이터베이스와 Xcode Interface Builder의 UI 요소 간 데이터 바인딩을 허용 하는 방법을 설명 합니다. SQLite.NET ORM을 사용 하 여 SQLite 데이터에 대 한 액세스를 제공 하는 방법도 다룹니다._

## <a name="overview"></a>개요

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용할 때 Xamarin.ios 또는 xamarin.ios 응용 프로그램이 액세스할 수 있는 것과 동일한 SQLite 데이터베이스에 액세스할 수 있습니다.

이 문서에서는 SQLite 데이터에 액세스 하는 두 가지 방법을 설명 합니다.

1. **직접 액세스** -SQLite 데이터베이스에 직접 액세스 하 여 Xcode의 Interface Builder에서 생성 된 UI 요소를 사용 하 여 키-값 코딩 및 데이터 바인딩에 대 한 데이터베이스의 데이터를 사용할 수 있습니다. Xamarin.ios 응용 프로그램에서 키-값 코딩 및 데이터 바인딩 기술을 사용 하 여 UI 요소를 채우고 사용 하기 위해 작성 하 고 유지 관리 해야 하는 코드의 양을 크게 줄일 수 있습니다. 또한 프런트 엔드 사용자 인터페이스 (_모델-뷰-컨트롤러_)에서 지원 데이터 (_데이터 모델_)를 추가로 분리 하 여 더 쉽게 유지 관리 하 고 더욱 유연한 응용 프로그램을 디자인할 수 있는 이점을 누릴 수 있습니다.
2. **SQLITE.NET orm** -오픈 소스 orm ( [SQLite.NET](http://www.sqlite.org) Object Relationship Manager)을 사용 하 여 SQLite 데이터베이스에서 데이터를 읽고 쓰는 데 필요한 코드의 양을 크게 줄일 수 있습니다. 그런 다음이 데이터를 사용 하 여 테이블 뷰와 같은 사용자 인터페이스 항목을 채울 수 있습니다.

[![실행 중인 앱의 예](databases-images/intro01.png "실행 중인 앱의 예")](databases-images/intro01-large.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램의 SQLite 데이터베이스로 키-값 코딩 및 데이터 바인딩을 사용 하는 기본 사항을 설명 합니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 가장 좋습니다. 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하 고,에서 사용할 주요 개념 및 기술을 설명 하 고 있습니다. 이 문서를 참조 하세요.

키-값 코딩 및 데이터 바인딩을 사용할 예정 이므로,이 설명서와 샘플 응용 프로그램에서 사용 되는 핵심 기술 및 개념을 설명 하는 대로 먼저 [데이터 바인딩과 키-값 코딩](~/mac/app-fundamentals/databinding.md) 을 통해 작업 해 보세요.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 특성 요소 Objective-C 개체 및 UI에 C# 클래스를 연결 하는 데 사용 합니다.

## <a name="direct-sqlite-access"></a>직접 SQLite 액세스

Xcode의 Interface Builder에서 UI 요소에 바인딩할 SQLite 데이터의 경우, 데이터를 쓰고 읽는 방법을 완전히 제어 하 여 먼저 SQLite 데이터베이스에 직접 액세스 하는 것이 좋습니다 (예: ORM 등의 기술을 사용 하는 대신).  데이터베이스에서

[데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 설명서에서 볼 수 있듯이 xamarin.ios 응용 프로그램에서 키-값 코딩 및 데이터 바인딩 기법을 사용 하면 작성 하 고 유지 관리 해야 하는 코드의 양을 크게 줄일 수 있습니다. UI 요소 작업 SQLite 데이터베이스에 직접 액세스 하는 것과 함께 사용 하면 해당 데이터베이스에서 데이터를 읽고 쓰는 데 필요한 코드의 양을 크게 줄일 수 있습니다.

이 문서에서는 데이터 바인딩과 키-값 코딩 문서에서 샘플 앱을 수정 하 여 SQLite 데이터베이스를 바인딩의 지원 원본으로 사용 합니다.

### <a name="including-sqlite-database-support"></a>SQLite 데이터베이스 지원 포함

계속 하기 전에 몇 가지에 대 한 참조를 포함 하 여 SQLite 데이터베이스 지원을 응용 프로그램에 추가 해야 합니다. Dll 파일.

다음을 수행합니다.

1. **Solution Pad**에서 **참조** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **참조 편집**을 선택 합니다.
2. **Mono. Sqlite** 및 system.xml 어셈블리를 모두 선택 **합니다.** 

    [![필요한 참조 추가](databases-images/reference01.png "필요한 참조 추가")](databases-images/reference01-large.png#lightbox)
3. **확인** 단추를 클릭 하 여 변경 내용을 저장 하 고 참조를 추가 합니다.

### <a name="modifying-the-data-model"></a>데이터 모델 수정

이제 응용 프로그램에 SQLite 데이터베이스에 직접 액세스 하기 위한 지원을 추가 했으므로 데이터 모델 개체를 수정 하 여 데이터베이스에서 데이터를 읽고 써야 하며 키-값 코딩 및 데이터 바인딩을 제공 해야 합니다. 샘플 응용 프로그램의 경우 **PersonModel.cs** 클래스를 편집 하 고 다음과 같이 만듭니다.

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

아래의 수정 사항을 자세히 살펴보겠습니다.

먼저 SQLite를 사용 하는 데 필요한 몇 가지 using 문을 추가 하 고 SQLite 데이터베이스에 대 한 마지막 연결을 저장 하는 변수를 추가 했습니다.

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection _conn = null;
```

사용자가 데이터 바인딩을 통해 UI의 내용을 수정할 때이 저장 된 연결을 사용 하 여 레코드의 변경 내용을 데이터베이스에 자동으로 저장 합니다.

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

**이름**, **직업** 또는 **ismanager** 속성에 대 한 모든 변경 내용은 이전에 데이터가 저장 된 경우 `_conn` (예: 변수가이 아닌 `null`경우) 데이터베이스에 전송 됩니다. 다음으로, 데이터베이스에서 사용자를 **만들고**, **업데이트**하 고, **로드** 하 고, **삭제** 하는 메서드를 살펴보겠습니다.

#### <a name="create-a-new-record"></a>새 레코드 만들기

SQLite 데이터베이스에 새 레코드를 만들기 위해 다음 코드가 추가 되었습니다.

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

를 `SQLiteCommand` 사용 하 여 데이터베이스에 새 레코드를 만듭니다. 을 호출 `SQLiteConnection` `CreateCommand`하 여 메서드로 전달 된 (conn)에서 새 명령을 가져옵니다. 다음으로, 실제 값에 대 한 매개 변수를 제공 하 여 실제로 새 레코드를 작성 하도록 SQL 명령을 설정 합니다.

```csharp
command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";
```

나중에 `Parameters.AddWithValue` `SQLiteCommand`에서 메서드를 사용 하 여 매개 변수의 값을 설정 합니다. 매개 변수를 사용 하 여 값 (예: 작은따옴표)이 SQLite로 전송 되기 전에 제대로 인코딩 되는지 확인 합니다. 예:

```csharp
command.Parameters.AddWithValue ("@COL1", ID);
```

마지막으로 사용자가 관리자이 고 해당 직원의 컬렉션을 포함할 수 있으므로 해당 사용자에 대해 메서드를 `Create` 재귀적으로 호출 하 여 데이터베이스에도 저장 합니다.

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

다음 코드는 SQLite 데이터베이스의 기존 레코드를 업데이트 하기 위해 추가 되었습니다.

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

위에서 **만들기** 와 마찬가지로 전달 `SQLiteConnection`된에서 `SQLiteCommand` 을 가져오고 SQL에서 레코드를 업데이트 (매개 변수 제공) 하도록 설정 합니다.

```csharp
command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";
```

매개 변수 값 (예: `command.Parameters.AddWithValue ("@COL1", ID);`)을 입력 하 고 자식 레코드에 대해 업데이트를 재귀적으로 호출 합니다.

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Update sub record
    Person.Update (conn);
}
```

#### <a name="loading-a-record"></a>레코드 로드

다음 코드는 SQLite 데이터베이스에서 기존 레코드를 로드 하기 위해 추가 되었습니다.

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

루틴은 부모 개체 (직원 개체를 로드 하는 관리자 개체)에서 재귀적으로 호출 될 수 있으므로 데이터베이스에 대 한 연결 열기 및 닫기를 처리 하는 특수 코드가 추가 되었습니다.

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

항상로 SQL을 설정 하 여 레코드를 검색 하 고 매개 변수를 사용 합니다.

```csharp
// Create new command
command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", id);
```

마지막으로 데이터 판독기를 사용 하 여 쿼리를 실행 하 고 레코드 필드를 반환 합니다 ( `PersonModel` 클래스의 인스턴스로 복사).

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

이 사용자가 관리자 인 경우에도 해당 `Load` 메서드를 재귀적으로 호출 하 여 모든 직원을 로드 해야 합니다.

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

SQLite 데이터베이스에서 기존 레코드를 삭제 하기 위해 다음 코드가 추가 되었습니다.

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

여기서는 매개 변수를 사용 하 여 관리자 레코드와 해당 관리자의 모든 직원 레코드를 삭제 하는 SQL을 제공 합니다.

```csharp
// Create new command
command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", ID);
```

레코드가 제거 된 후에는 `PersonModel` 클래스의 현재 인스턴스를 삭제 합니다.

```csharp
// Empty class
ID = "";
ManagerID = "";
Name = "";
Occupation = "";
isManager = false;
_people = new NSMutableArray();
```

### <a name="initializing-the-database"></a>데이터베이스 초기화

데이터 모델을 변경 하 여 데이터베이스에 대 한 읽기 및 쓰기를 지원 하기 위해 데이터베이스에 대 한 연결을 열고 첫 번째 실행에서 초기화 해야 합니다. **MainWindow.cs** 파일에 다음 코드를 추가 해 보겠습니다.

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

위의 코드를 좀 더 자세히 살펴보겠습니다. 먼저 새 데이터베이스 (이 예제에서는 사용자의 데스크톱)의 위치를 선택 하 고, 데이터베이스가 있는지 확인 하 고, 그렇지 않은 경우 만듭니다.

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "People.db3");

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
if (!exists)
    SqliteConnection.CreateFile (db);
```

그런 다음 위에서 만든 경로를 사용 하 여 데이터베이스에 대 한 연결을 설정 합니다.

```csharp
var conn = new SqliteConnection("Data Source=" + db);
```

그런 다음 필요한 데이터베이스에 모든 SQL 테이블을 만듭니다.

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

마지막으로, 데이터 모델 (`PersonModel`)을 사용 하 여 응용 프로그램을 처음 실행할 때 또는 데이터베이스가 없는 경우 데이터베이스에 대 한 기본 레코드 집합을 만듭니다.

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

응용 프로그램을 시작 하 고 주 창을 열면 위에서 추가한 코드를 사용 하 여 데이터베이스에 연결 합니다.

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get access to database
    DatabaseConnection = GetDatabaseConnection ();
}
```

### <a name="loading-bound-data"></a>바인딩된 데이터 로드

SQLite 데이터베이스에서 바인딩된 데이터에 직접 액세스 하기 위한 모든 구성 요소를 사용 하 여 응용 프로그램에서 제공 하는 다양 한 보기에서 데이터를 로드할 수 있으며 UI에 자동으로 표시 됩니다.

#### <a name="loading-a-single-record"></a>단일 레코드 로드

ID가 알고 있는 단일 레코드를 로드 하려면 다음 코드를 사용할 수 있습니다.

```csharp
Person = new PersonModel (Conn, "0");
```

#### <a name="loading-all-records"></a>모든 레코드 로드

관리자 인지 여부에 관계 없이 모든 사용자를 로드 하려면 다음 코드를 사용 합니다.

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

여기서는 `PersonModel` 클래스에 대 한 생성자의 오버 로드를 사용 하 여 사용자를 메모리로 로드 합니다.

```csharp
var person = new PersonModel (_conn, childID);
```

데이터 바인딩된 클래스를 호출 하 여 사람 컬렉션 `AddPerson (person)`에 사람을 추가 하는 것은 UI가 변경 내용을 인식 하 고 표시 하도록 합니다.

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}
```

#### <a name="loading-top-level-records-only"></a>최상위 레코드만 로드

개요 뷰에서 데이터를 표시 하는 등의 경우에만 관리자를 로드 하려면 다음 코드를 사용 합니다.

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

유일한 차이점은 관리자 `command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1"`만 로드 하는 SQL 문의 유일한 차이점 이지만, 그렇지 않으면 위의 섹션과 동일 하 게 작동 합니다.

<a name="Databases-and-ComboBoxes" />

### <a name="databases-and-comboboxes"></a>데이터베이스 및 comboboxes

MacOS에서 사용할 수 있는 메뉴 컨트롤 (예: 콤보 상자)은 내부 목록 (Interface Builder 미리 정의 되거나 코드를 통해 채워질 수 있음)에서 또는 고유한 사용자 지정 외부 데이터 원본을 제공 하 여 드롭다운 목록을 채우도록 설정할 수 있습니다. 자세한 내용은 [Menu 컨트롤 데이터 제공](~/mac/user-interface/standard-controls.md#Providing-Menu-Control-Data) 을 참조 하세요.

예를 들어 Interface Builder 위의 단순 바인딩 예제를 편집 하 고, 콤보 상자를 추가 하 고 라는 `EmployeeSelector`콘센트를 사용 하 여 노출 합니다.

[![콤보 상자 출구 노출](databases-images/combo01.png "콤보 상자 출구 노출")](databases-images/combo01-large.png#lightbox)

**특성 검사자**에서 **autocompletes** 를 선택 하 고 **데이터 원본** 속성을 사용 합니다.

![콤보 상자 특성 구성](databases-images/combo02.png "콤보 상자 특성 구성")

변경 내용을 저장 하 고 동기화 할 Mac용 Visual Studio로 돌아갑니다.

#### <a name="providing-combobox-data"></a>Combobox 데이터 제공

그런 다음 라는 `ComboBoxDataSource` 프로젝트에 새 클래스를 추가 하 고 다음과 같이 만듭니다.

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

이 예제에서는 모든 SQLite 데이터 원본에서 콤보 `NSComboBoxDataSource` 상자 항목을 제공할 수 있는 새을 만듭니다. 먼저, 다음 속성을 정의 합니다.

- `Conn`-SQLite 데이터베이스에 대 한 연결을 가져오거나 설정 합니다.
- `TableName`-테이블 이름을 가져오거나 설정 합니다.
- `IDField`-지정 된 테이블의 고유 ID를 제공 하는 필드를 가져오거나 설정 합니다. 기본값은 `ID`입니다.
- `DisplayField`-드롭다운 목록에 표시 되는 필드를 가져오거나 설정 합니다.
- `RecordCount`-지정 된 테이블의 레코드 수를 가져옵니다.

개체의 새 인스턴스를 만들 때 연결, 테이블 이름, 선택적으로 ID 필드 및 표시 필드를 전달 합니다.

```csharp
public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
{
    // Initialize
    this.Conn = conn;
    this.TableName = tableName;
    this.DisplayField = displayField;
}
```

메서드 `GetRecordCount` 는 지정 된 테이블의 레코드 수를 반환 합니다.

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

`TableName` ,`IDField` 또는 속성`DisplayField` 값이 변경 될 때마다 호출 됩니다.

메서드 `IDForIndex` 는 지정 된 드롭다운 목록 항목`IDField`인덱스에 있는 레코드의 고유 ID ()를 반환 합니다. 

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

메서드 `ValueForIndex` 는 지정 된 드롭다운 목록`DisplayField`인덱스에 있는 항목에 대 한 값 ()을 반환 합니다.

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

메서드 `IDForValue` 는 지정 된 값 (`DisplayField`)`IDField`에 대해 고유 ID ()를 반환 합니다.

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

는 `ItemCount` `TableName`, 또는속성이`DisplayField` 변경 될 때 계산 되는 목록에 있는 항목의 사전 계산 수를 반환 합니다. `IDField`

```csharp
public override nint ItemCount (NSComboBox comboBox)
{
    return RecordCount;
}
```

메서드 `ObjectValueForItem` 는 지정 된 드롭다운 목록`DisplayField`항목 인덱스에 대해 값 ()을 제공 합니다.

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

SQLite 명령의 `LIMIT` 및 `OFFSET` 문을 사용 하 여 필요한 레코드 하나를 제한 합니다.

메서드 `IndexOfItem` 는 지정 된 값 (`DisplayField`)의 드롭다운 항목 인덱스를 반환 합니다.

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

값을 찾을 `NSRange.NotFound` 수 없는 경우 값이 반환 되 고 드롭다운 목록에서 모든 항목이 선택 취소 됩니다.

메서드 `CompletedString` 는 부분적으로 형식화 된 항목에`DisplayField`대해 일치 하는 첫 번째 값 ()을 반환 합니다.

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

모든 부분을 함께 가져오려면를 편집 `SubviewSimpleBindingController` 하 고 다음과 같이 표시 합니다.

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

속성 `DataSource` 은 콤보 상자에 연결 된 `ComboBoxDataSource` (위에서 만든)의 바로 가기를 제공 합니다.

메서드 `LoadSelectedPerson` 는 지정 된 고유 ID에 대 한 사용자를 데이터베이스에서 로드 합니다.

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

메서드 재정의 `AwakeFromNib` 에서 먼저 사용자 지정 콤보 상자 데이터 원본의 인스턴스를 연결 합니다.

```csharp
EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");
```

다음으로, 지정 된 사용자를 프레젠테이션 하 고 로드 하는 데이터의 연결 된 고유 ID (`IDField`)를 찾아 사용자에 게 응답 합니다.

```csharp
EmployeeSelector.Changed += (sender, e) => {
    // Get ID
    var id = DataSource.IDForValue (EmployeeSelector.StringValue);
    LoadSelectedPerson (id);
};
```

사용자가 드롭다운 목록에서 새 항목을 선택 하는 경우 새 사용자도 로드 합니다.

```csharp
EmployeeSelector.SelectionChanged += (sender, e) => {
    // Get ID
    var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
    LoadSelectedPerson (id);
};
```

마지막으로 콤보 상자와 표시 된 사람을 목록의 첫 번째 항목에 자동으로 채웁니다.

```csharp
// Auto select the first person
EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
Person = new PersonModel (Conn, DataSource.IDForIndex(0));
```

## <a name="sqlitenet-orm"></a>ORM SQLite.NET

위에서 설명한 것 처럼 오픈 소스 ORM ( [SQLite.NET](http://www.sqlite.org) Object Relationship Manager)을 사용 하 여 SQLite 데이터베이스에서 데이터를 읽고 쓰는 데 필요한 코드의 양을 크게 줄일 수 있습니다. 키-값 코딩 및 데이터 바인딩이 개체에 적용 되는 몇 가지 요구 사항으로 인해 데이터를 바인딩할 때 가장 적합 한 경로가 아닐 수도 있습니다.

SQLite.Net 웹 사이트 _에 따라 "SQLite는 자체 포함 된 서버를 사용 하지 않는 구성의 트랜잭션 SQL database 엔진을 구현 하는 소프트웨어 라이브러리입니다. SQLite는 전 세계에서 가장 널리 배포 되는 데이터베이스 엔진입니다. SQLite의 소스 코드는 공용 도메인에 있습니다. "_

다음 섹션에서는 SQLite.Net를 사용 하 여 테이블 뷰에 대 한 데이터를 제공 하는 방법을 보여 줍니다.

### <a name="including-the-sqlitenet-nuget"></a>SQLite.net NuGet 포함

SQLite.NET는 응용 프로그램에 포함 하는 NuGet 패키지로 제공 됩니다. SQLite.NET를 사용 하 여 데이터베이스 지원을 추가 하기 전에이 패키지를 포함 해야 합니다.

패키지를 추가 하려면 다음을 수행 합니다.

1. **Solution Pad**에서 **패키지** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **패키지 추가** ...를 선택 합니다.
2. `SQLite.net` **검색 상자** 에를 입력 하 고 **sqlite-net** 항목을 선택 합니다.

    [![SQLite NuGet 패키지 추가](databases-images/nuget01.png "SQLite NuGet 패키지 추가")](databases-images/nuget01-large.png#lightbox)
3. **패키지 추가** 단추를 클릭 하 여 완료 합니다.

### <a name="creating-the-data-model"></a>데이터 모델 만들기

프로젝트에 새 클래스를 추가 하 고에서를 `OccupationModel`호출 해 보겠습니다. 이제 **OccupationModel.cs** 파일을 편집 하 고 다음과 같이 합니다.

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

먼저 SQLite.NET (`using Sqlite`)를 포함 한 다음,이 레코드가 저장 될 때 데이터베이스에 기록 되는 여러 속성을 노출 합니다. 기본 키로 사용 하는 첫 번째 속성은 다음과 같이 자동으로 증가 하도록 설정 합니다.

```csharp
[PrimaryKey, AutoIncrement]
public int ID { get; set; }
```

### <a name="initializing-the-database"></a>데이터베이스 초기화

데이터 모델을 변경 하 여 데이터베이스에 대 한 읽기 및 쓰기를 지원 하기 위해 데이터베이스에 대 한 연결을 열고 첫 번째 실행에서 초기화 해야 합니다. 다음 코드를 추가 해 보겠습니다.

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

먼저 데이터베이스 (이 경우 사용자의 데스크톱)의 경로를 가져오고 데이터베이스가 이미 있는지 확인 합니다.

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "Occupation.db3");
OccupationModel Occupation;

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
```

다음으로 위에서 만든 경로에서 데이터베이스에 대 한 연결을 설정 합니다.

```csharp
var conn = new SQLiteConnection (db);
```

마지막으로, 테이블을 만들고 몇 가지 기본 레코드를 추가 합니다.

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

예제 사용법으로 Xcode의 Interface builder에서 UI에 테이블 뷰를 추가 합니다. 코드를 통해`OccupationTable` C# 액세스할 수 있도록 콘센트 ()를 통해이 테이블 뷰를 노출 합니다.

[![테이블 뷰 유출 노출](databases-images/table01.png "테이블 뷰 유출 노출")](databases-images/table01-large.png#lightbox)

다음으로 사용자 지정 클래스를 추가 하 여이 테이블을 SQLite.NET 데이터베이스의 데이터로 채웁니다.

### <a name="creating-the-table-data-source"></a>테이블 데이터 원본 만들기

테이블에 대 한 데이터를 제공 하는 사용자 지정 데이터 원본을 만들어 보겠습니다. 먼저 이라는 `TableORMDatasource` 새 클래스를 추가 하 고 다음과 같이 만듭니다.

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

나중에이 클래스의 인스턴스를 만들 때 open SQLite.NET 데이터베이스 연결을 전달 합니다. 메서드 `LoadOccupations` 는 데이터베이스를 쿼리하고 `OccupationModel` 데이터 모델을 사용 하 여 검색 된 레코드를 메모리에 복사 합니다.

### <a name="creating-the-table-delegate"></a>테이블 대리자 만들기

SQLite.NET 데이터베이스에서 로드 한 정보를 표시 하는 사용자 지정 테이블 대리자가 필요한 최종 클래스입니다. 프로젝트에 새 `TableORMDelegate` 를 추가 하 고 다음과 같이 만듭니다.

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

여기서는 SQLite.NET 데이터베이스에서 로드 한 `Occupations` 데이터 원본의 컬렉션을 사용 하 여 `GetViewForItem` 메서드 재정의를 통해 테이블의 열을 채웁니다.

### <a name="populating-the-table"></a>테이블 채우기

모든 부분이 준비 되 면 메서드를 `AwakeFromNib` 재정의 하 고 다음과 같이 xib 파일에서 팽창 때 테이블을 채워야 합니다.

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

먼저 SQLite.NET 데이터베이스에 대 한 액세스 권한을 획득 하 고 아직 없는 경우이를 만들고 채웁니다. 다음으로 사용자 지정 테이블 데이터 원본의 새 인스턴스를 만들고 데이터베이스 연결을 전달 하 여 테이블에 연결 합니다. 마지막으로, 사용자 지정 테이블 대리자의 새 인스턴스를 만들고, 데이터 원본을 전달 하 고, 테이블에 연결 합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 SQLite 데이터베이스를 사용 하 여 데이터 바인딩 및 키-값 코딩 작업에 대해 자세히 살펴봅니다. 먼저 KVC (키-값 C# 코딩) 및 키-값 관찰 (KVO)을 사용 하 여 클래스를 목표 C로 노출 하는 방법을 살펴보았습니다. 다음으로 KVO 규격 클래스를 사용 하 고 데이터를 Xcode의 Interface Builder에 있는 UI 요소에 바인딩하는 방법을 보여 주었습니다. 또한이 문서에서는 SQLite.NET ORM을 통해 SQLite 데이터 작업을 수행 하 고 해당 데이터를 테이블 뷰에 표시 합니다.

## <a name="related-links"></a>관련 링크

- [MacDatabase (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macdatabase)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [표준 컨트롤](~/mac/user-interface/standard-controls.md)
- [테이블 보기](~/mac/user-interface/table-view.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [컬렉션 뷰](~/mac/user-interface/collection-view.md)
- [키-값 코딩 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Cocoa 바인딩 프로그래밍 항목 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Cocoa 바인딩 참조 소개](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS 휴먼 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
