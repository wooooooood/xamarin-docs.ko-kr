---
title: 로컬 SQLite.NET 데이터베이스에 데이터 저장
description: 이 문서에서는 로컬 SQLite.NET 데이터베이스에 데이터를 저장하는 방법을 설명합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 5BF901BD-FDE8-4B74-B4AB-418E81745A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 2cd4726566e73aece5d0deef90ad1feedefaa2d8
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "71249675"
---
# <a name="store-data-in-a-local-sqlitenet-database"></a>로컬 SQLite.NET 데이터베이스에 데이터 저장

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)

이 빠른 시작에서 다음과 같은 작업을 수행하는 방법을 알아봅니다.

- NuGet 패키지 관리자를 사용하여 프로젝트에 NuGet 패키지를 추가합니다.
- 데이터를 SQLite.NET 데이터베이스에 로컬로 저장합니다.

이 빠른 시작은 로컬 SQLite.NET 데이터베이스에 데이터를 저장하는 방법을 안내합니다. 최종 애플리케이션은 다음과 같습니다.

[![](database-images/screenshots1-sml.png "Notes Page")](database-images/screenshots1.png#lightbox "Notes Page")
[![](database-images/screenshots2-sml.png "Note Entry Page")](database-images/screenshots2.png#lightbox "Note Entry Page")

## <a name="prerequisites"></a>사전 요구 사항

이 빠른 시작을 시도하기 전에 [이전 빠른 시작](multi-page.md)을 성공적으로 완료해야 합니다. 또는 [이전 빠른 시작 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)을 다운로드하고 이 빠른 시작의 시작점으로 사용하세요.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio를 사용하여 앱 업데이트

1. Visual Studio를 시작하고 Notes 솔루션을 엽니다.

2. **솔루션 탐색기**에서 **Notes** 프로젝트를 선택하고, 마우스 오른쪽 단추로 클릭한 다음, **NuGet 패키지 관리...** 를 선택합니다.

    ![](database-images/vs/add-nuget-packages.png "Add NuGet Packages")    

3. **NuGet 패키지 관리자**에서 **찾아보기** 탭을 선택하고 **sqlite-net-pcl** NuGet 패키지를 검색하여 선택한 다음, **설치** 단추를 클릭하여 프로젝트에 추가합니다.

    ![](database-images/vs/add-package.png "Add Package")

    > [!NOTE]
    > 이름이 유사한 NuGet 패키지가 여러 개 있습니다. 올바른 패키지에는 이러한 특성이 있습니다.
    > - **작성자:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet 링크:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > 패키지 이름에도 불구하고 이 NuGet 패키지는 .NET 표준 패키지에서 사용할 수 있습니다.

    이 패키지는 데이터베이스 작업을 애플리케이션에 통합하는 데 사용됩니다.

4. **솔루션 탐색기**의 **Notes** 프로젝트에 있는 **Models** 폴더에서 **Note.cs**를 열고 기존 코드를 열고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using SQLite;

    namespace Notes.Models
    {
        public class Note
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    이 클래스는 애플리케이션의 각 메모에 대한 데이터를 저장하는 `Note` 모델을 정의합니다. `ID` 속성은 SQLite.NET 데이터베이스의 각 `Note` 인스턴스가 SQLite.NET에서 제공되는 고유 ID를 갖도록 `PrimaryKey` 및 `AutoIncrement` 특성과 함께 표시됩니다.

    **CTRL+S** 키를 눌러 변경 내용을 **Note.cs**에 저장하고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 애플리케이션을 빌드하려고 하면 후속 단계에서 수정될 오류가 발생합니다.

5. **솔루션 탐색기**에서 **Data**라는 새 폴더를 **Notes** 프로젝트에 추가합니다.

6. **솔루션 탐색기**의 **Notes** 프로젝트에서 **Data** 폴더에 **NoteDatabase**라는 새 클래스를 추가합니다.

7. **NoteDatabase.cs**에서 기존 코드를 다음 코드로 바꿉니다.

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;
    using Notes.Models;

    namespace Notes.Data
    {
        public class NoteDatabase
        {
            readonly SQLiteAsyncConnection _database;

            public NoteDatabase(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                return _database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                return _database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    return _database.UpdateAsync(note);
                }
                else
                {
                    return _database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                return _database.DeleteAsync(note);
            }
        }
    }
    ```

    이 클래스에는 데이터베이스를 만들고, 그 데이터베이스로부터 데이터를 읽고 쓰는 코드가 있습니다. 코드는 데이터베이스 작업을 백그라운드 스레드로 이동시키는 비동기 SQLite.NET API를 사용합니다. 또한 `NoteDatabase` 생성자는 데이터베이스 파일의 경로를 인수로 사용합니다. 이 경로는 다음 단계에서 `App` 클래스에 의해 제공됩니다.

    **CTRL+S** 키를 눌러 변경 내용을 **NoteDatabase.cs**에 저장하고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 애플리케이션을 빌드하려고 하면 후속 단계에서 수정될 오류가 발생합니다.

8. **솔루션 탐색기**의 **Notes** 프로젝트에서 **App.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Notes.Data;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

            public static NoteDatabase Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new NoteDatabase(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "Notes.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();
                MainPage = new NavigationPage(new NotesPage());
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

    이 코드는 새 `NoteDatabase` 인스턴스를 singleton으로 만들고 데이터베이스의 파일 이름을 `NoteDatabase` 생성자에 인수로 전달하는 `Database` 속성을 정의합니다. 데이터베이스를 싱글톤으로 노출하면 애플리케이션이 실행되는 동안 열린 상태로 유지되는 단일 데이터베이스 연결이 생성되므로 데이터베이스 작업이 수행될 때마다 데이터베이스 파일을 열거나 닫는 비용을 피할 수 있는 이점이 있습니다.

    **CTRL+S** 키를 눌러 변경 내용을 **App.xaml.cs**에 저장하고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 애플리케이션을 빌드하려고 하면 후속 단계에서 수정될 오류가 발생합니다.

9. **솔루션 탐색기**의 **Notes** 프로젝트에서 **NotesPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음 `OnAppearing` 메서드를 다음 코드로 바꿉니다.

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    이 코드는 [`ListView`](xref:Xamarin.Forms.ListView)를 데이터베이스에 저장된 메모로 채웁니다.

    **CTRL+S** 키를 눌러 변경 내용을 **NotesPage.xaml.cs**에 저장하고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 애플리케이션을 빌드하려고 하면 후속 단계에서 수정될 오류가 발생합니다.

10. **솔루션 탐색기**에서 **NoteEntryPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음 `OnSaveButtonClicked` 및 `OnDeleteButtonClicked` 메서드를 다음 코드로 바꿉니다.

      ```csharp
      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          await App.Database.SaveNoteAsync(note);
          await Navigation.PopAsync();
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);
          await Navigation.PopAsync();
      }
      ```    

      `NoteEntryPage`는 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)에서 단일 메모를 나타내는 `Note` 인스턴스를 저장합니다. `OnSaveButtonClicked` 이벤트 처리기가 실행되면 `Note` 인스턴스가 데이터베이스에 저장되고 애플리케이션이 이전 페이지로 다시 이동합니다. `OnDeleteButtonClicked` 이벤트 처리기가 실행되면 `Note` 인스턴스가 데이터베이스에서 삭제되고 애플리케이션이 이전 페이지로 다시 이동합니다.

      **CTRL+S** 키를 눌러 변경 내용을 **NoteEntryPage.xaml.cs**에 저장하고 파일을 닫습니다.

11. 각 플랫폼에서 프로젝트를 빌드하고 실행합니다. 자세한 내용은 [빠른 시작 빌드](single-page.md#building-the-quickstart)를 참조하세요.

    **NotesPage**에서 **+** 단추를 눌러 **NoteEntryPage**로 이동하고 노트를 입력합니다. 노트를 저장한 후 애플리케이션은 **NotesPage**로 다시 이동합니다.

    다양한 길이의 여러 노트를 입력하여 애플리케이션 동작을 관찰합니다.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Mac용 Visual Studio를 사용하여 앱 업데이트

1. Mac용 Visual Studio를 시작하고 Notes 프로젝트를 엽니다.

2. **Solution Pad**에서 **Notes** 프로젝트를 선택하고 마우스 오른쪽 단추를 클릭한 다음 **추가 > NuGet 패키지 추가...** 를 선택합니다.

    ![](database-images/vsmac/add-nuget-packages.png "Add NuGet Packages")    

3. **패키지 추가** 창에서 **sqlite-net-pcl** NuGet 패키지를 검색하여 선택한 다음, **패키지 추가** 단추를 클릭하여 프로젝트에 추가합니다.

    ![](database-images/vsmac/add-package.png "Add Package")

    > [!NOTE]
    > 이름이 유사한 NuGet 패키지가 여러 개 있습니다. 올바른 패키지에는 이러한 특성이 있습니다.
    > - **작성자:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet 링크:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > 패키지 이름에도 불구하고 이 NuGet 패키지는 .NET 표준 패키지에서 사용할 수 있습니다.

    이 패키지는 데이터베이스 작업을 애플리케이션에 통합하는 데 사용됩니다.

4. **Solution Pad**의 **Notes** 프로젝트에서 **Models** 폴더의 **Note.cs**를 열고 기존 코드를 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using SQLite;

    namespace Notes.Models
    {
        public class Note
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    이 클래스는 애플리케이션의 각 메모에 대한 데이터를 저장하는 `Note` 모델을 정의합니다. `ID` 속성은 SQLite.NET 데이터베이스의 각 `Note` 인스턴스가 SQLite.NET에서 제공되는 고유 ID를 갖도록 `PrimaryKey` 및 `AutoIncrement` 특성과 함께 표시됩니다.

    **파일 > 저장**을 선택하거나(또는 **&#8984; + S**를 눌러) 변경 내용을 **Note.cs**에 저장하고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 애플리케이션을 빌드하려고 하면 후속 단계에서 수정될 오류가 발생합니다.

5. **Solution Pad**에서 **Data**라는 새 폴더를 **Notes** 프로젝트에 추가합니다.

6. **Solution Pad**의 **Notes** 프로젝트에서 **Data** 폴더에 **NoteDatabase**라는 새 클래스를 추가합니다.

7. **NoteDatabase.cs**에서 기존 코드를 다음 코드로 바꿉니다.

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;
    using Notes.Models;

    namespace Notes.Data
    {
        public class NoteDatabase
        {
            readonly SQLiteAsyncConnection _database;

            public NoteDatabase(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Note>().Wait();
            }

            public Task<List<Note>> GetNotesAsync()
            {
                return _database.Table<Note>().ToListAsync();
            }

            public Task<Note> GetNoteAsync(int id)
            {
                return _database.Table<Note>()
                                .Where(i => i.ID == id)
                                .FirstOrDefaultAsync();
            }

            public Task<int> SaveNoteAsync(Note note)
            {
                if (note.ID != 0)
                {
                    return _database.UpdateAsync(note);
                }
                else
                {
                    return _database.InsertAsync(note);
                }
            }

            public Task<int> DeleteNoteAsync(Note note)
            {
                return _database.DeleteAsync(note);
            }
        }
    }
    ```

    이 클래스에는 데이터베이스를 만들고, 그 데이터베이스로부터 데이터를 읽고 쓰는 코드가 있습니다. 코드는 데이터베이스 작업을 백그라운드 스레드로 이동시키는 비동기 SQLite.NET API를 사용합니다. 또한 `NoteDatabase` 생성자는 데이터베이스 파일의 경로를 인수로 사용합니다. 이 경로는 다음 단계에서 `App` 클래스에 의해 제공됩니다.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **NoteDatabase.cs**에 저장하고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 애플리케이션을 빌드하려고 하면 후속 단계에서 수정될 오류가 발생합니다.

8. **Solution Pad**의 **Notes** 프로젝트에서 **App.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Notes.Data;

    namespace Notes
    {
        public partial class App : Application
        {
            static NoteDatabase database;

            public static NoteDatabase Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new NoteDatabase(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "Notes.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();
                MainPage = new NavigationPage(new NotesPage());
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

    이 코드는 새 `NoteDatabase` 인스턴스를 singleton으로 만들고 데이터베이스의 파일 이름을 `NoteDatabase` 생성자에 인수로 전달하는 `Database` 속성을 정의합니다. 데이터베이스를 싱글톤으로 노출하면 애플리케이션이 실행되는 동안 열린 상태로 유지되는 단일 데이터베이스 연결이 생성되므로 데이터베이스 작업이 수행될 때마다 데이터베이스 파일을 열거나 닫는 비용을 피할 수 있는 이점이 있습니다.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **App.xaml.cs**에 저장하고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 애플리케이션을 빌드하려고 하면 후속 단계에서 수정될 오류가 발생합니다.

9. **Solution Pad**의 **Notes** 프로젝트에서 **NotesPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음 `OnAppearing` 메서드를 다음 코드로 바꿉니다.

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    이 코드는 [`ListView`](xref:Xamarin.Forms.ListView)를 데이터베이스에 저장된 메모로 채웁니다.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **NotesPage.xaml.cs**에 저장하고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 애플리케이션을 빌드하려고 하면 후속 단계에서 수정될 오류가 발생합니다.

10. **Solution Pad**에서 **NoteEntryPage.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음 `OnSaveButtonClicked` 및 `OnDeleteButtonClicked` 메서드를 다음 코드로 바꿉니다.

      ```csharp
      async void OnSaveButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          note.Date = DateTime.UtcNow;
          await App.Database.SaveNoteAsync(note);
          await Navigation.PopAsync();
      }

      async void OnDeleteButtonClicked(object sender, EventArgs e)
      {
          var note = (Note)BindingContext;
          await App.Database.DeleteNoteAsync(note);
          await Navigation.PopAsync();
      }
      ```    

      `NoteEntryPage`는 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)에서 단일 메모를 나타내는 `Note` 인스턴스를 저장합니다. `OnSaveButtonClicked` 이벤트 처리기가 실행되면 `Note` 인스턴스가 데이터베이스에 저장되고 애플리케이션이 이전 페이지로 다시 이동합니다. `OnDeleteButtonClicked` 이벤트 처리기가 실행되면 `Note` 인스턴스가 데이터베이스에서 삭제되고 애플리케이션이 이전 페이지로 다시 이동합니다.

      **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **NoteEntryPage.xaml.cs**에 저장하고 파일을 닫습니다.

11. 각 플랫폼에서 프로젝트를 빌드하고 실행합니다. 자세한 내용은 [빠른 시작 빌드](single-page.md#building-the-quickstart)를 참조하세요.

    **NotesPage**에서 **+** 단추를 눌러 **NoteEntryPage**로 이동하고 노트를 입력합니다. 노트를 저장한 후 애플리케이션은 **NotesPage**로 다시 이동합니다.

    다양한 길이의 여러 노트를 입력하여 애플리케이션 동작을 관찰합니다.

::: zone-end

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서 다음과 같은 방법을 배웠습니다.

- NuGet 패키지 관리자를 사용하여 프로젝트에 NuGet 패키지를 추가합니다.
- 데이터를 SQLite.NET 데이터베이스에 로컬로 저장합니다.

XAML 스타일로 애플리케이션의 스타일을 지정하려면 다음 빠른 시작을 계속 진행하세요.

> [!div class="nextstepaction"]
> [다음](styling.md)

## <a name="related-links"></a>관련 링크

- [노트(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)
- [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)
