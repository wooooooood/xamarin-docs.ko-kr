---
ms.openlocfilehash: b11a5972c2aabace8a6991a82f5719f34450297d
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841509"
---
이 연습에서는 데이터 액세스 클래스를 **LocalDatabaseTutorial** 프로젝트에 추가합니다. 이 프로젝트는 여러 사람에 대한 데이터를 데이터베이스에 보존하는 데 사용됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **솔루션 탐색기**의 **LocalDatabaseTutorial** 프로젝트에서 `Person`이라는 새 클래스를 프로젝트에 추가합니다. 그런 다음, **Person.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Person
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Name { get; set; }
            public int Age { get; set; }
        }
    }
    ```

    이 코드는 애플리케이션의 각 개인에 대한 데이터를 저장하는 `Person` 클래스를 정의합니다. `ID` 속성은 데이터베이스의 각 `Person` 인스턴스가 SQLite.NET에서 제공되는 고유 ID를 갖도록 `PrimaryKey` 및 `AutoIncrement` 특성과 함께 표시됩니다.

1. **솔루션 탐색기**의 **LocalDatabaseTutorial** 프로젝트에서 `Database`이라는 새 클래스를 프로젝트에 추가합니다. 그런 다음, **Database.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Database
        {
            readonly SQLiteAsyncConnection _database;

            public Database(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Person>().Wait();
            }

            public Task<List<Person>> GetPeopleAsync()
            {
                return _database.Table<Person>().ToListAsync();
            }

            public Task<int> SavePersonAsync(Person person)
            {
                return _database.InsertAsync(person);
            }
        }
    }
    ```

    이 클래스에는 데이터베이스를 만들고, 그 데이터베이스로부터 데이터를 읽고 쓰는 코드가 있습니다. 코드는 데이터베이스 작업을 백그라운드 스레드로 이동시키는 비동기 SQLite.NET API를 사용합니다. 또한 `Database` 생성자는 데이터베이스 파일의 경로를 인수로 사용합니다. 이 경로는 다음 연습에서 `App` 클래스에 의해 제공됩니다.

1. **솔루션 탐색기**의 **LocalDatabaseTutorial** 프로젝트에서 **App.xaml**을 확장하고 **App.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **App.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace LocalDatabaseTutorial
    {
        public partial class App : Application
        {
            static Database database;

            public static Database Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new Database(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "people.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    이 코드는 새로운 `Database` 인스턴스를 싱글톤으로 생성하는 `Database` 속성을 정의합니다. 데이터베이스를 저장할 위치를 나타내는 로컬 파일 경로와 파일 이름이 인수로 `Database` 클래스 생성자에 전달됩니다.

    > [!IMPORTANT]
    > 데이터베이스를 싱글톤으로 노출하면 애플리케이션이 실행되는 동안 열린 상태로 유지되는 단일 데이터베이스 연결이 생성되므로 데이터베이스 작업이 수행될 때마다 데이터베이스 파일을 열거나 닫는 비용을 피할 수 있는 이점이 있습니다.

1. 오류가 없는지 확인하기 위해 솔루션을 빌드합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad**의 **LocalDatabaseTutorial** 프로젝트에서 `Person`이라는 새 클래스를 프로젝트에 추가합니다. 그런 다음, **Person.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Person
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Name { get; set; }
            public int Age { get; set; }
        }
    }
    ```

    이 코드는 애플리케이션의 각 개인에 대한 데이터를 저장하는 `Person` 클래스를 정의합니다. `ID` 속성은 데이터베이스의 각 `Person` 인스턴스가 SQLite.NET에서 제공되는 고유 ID를 갖도록 `PrimaryKey` 및 `AutoIncrement` 특성과 함께 표시됩니다.

1. **Solution Pad**의 **LocalDatabaseTutorial** 프로젝트에서 `Database`이라는 새 클래스를 프로젝트에 추가합니다. 그런 다음, **Database.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Database
        {
            readonly SQLiteAsyncConnection _database;

            public Database(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Person>().Wait();
            }

            public Task<List<Person>> GetPeopleAsync()
            {
                return _database.Table<Person>().ToListAsync();
            }

            public Task<int> SavePersonAsync(Person person)
            {
                return _database.InsertAsync(person);
            }
        }
    }
    ```

    이 클래스에는 데이터베이스를 만들고, 그 데이터베이스로부터 데이터를 읽고 쓰는 코드가 있습니다. 코드는 데이터베이스 작업을 백그라운드 스레드로 이동시키는 비동기 SQLite.NET API를 사용합니다. 또한 `Database` 생성자는 데이터베이스 파일의 경로를 인수로 사용합니다. 이 경로는 다음 연습에서 `App` 클래스에 의해 제공됩니다.

1. **Solution Pad**의 **LocalDatabaseTutorial** 프로젝트에서 **App.xaml**을 확장하고 **App.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음, **App.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace LocalDatabaseTutorial
    {
        public partial class App : Application
        {
            static Database database;

            public static Database Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new Database(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "people.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    이 코드는 새로운 `Database` 인스턴스를 싱글톤으로 생성하는 `Database` 속성을 정의합니다. 데이터베이스를 저장할 위치를 나타내는 로컬 파일 경로와 파일 이름이 인수로 `Database` 클래스 생성자에 전달됩니다.

    > [!IMPORTANT]
    > 데이터베이스를 싱글톤으로 노출하면 애플리케이션이 실행되는 동안 열린 상태로 유지되는 단일 데이터베이스 연결이 생성되므로 데이터베이스 작업이 수행될 때마다 데이터베이스 파일을 열거나 닫는 비용을 피할 수 있는 이점이 있습니다.

1. 오류가 없는지 확인하기 위해 솔루션을 빌드합니다.
