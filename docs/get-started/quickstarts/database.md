---
title: 로컬 SQLite.NET 데이터베이스에 데이터 저장
description: 이 문서에서는 로컬 SQLite.NET 데이터베이스에서 데이터를 저장 하는 방법에 설명 합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 5BF901BD-FDE8-4B74-B4AB-418E81745A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 5c3daf04c08e2109c46b24c198fef8e71fac2f3d
ms.sourcegitcommit: e7f27ba75cae5099ef053b819b84132a77d4f9e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2019
ms.locfileid: "58854992"
---
# <a name="store-data-in-a-local-sqlitenet-database"></a>로컬 SQLite.NET 데이터베이스에서 데이터를 저장 합니다.

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Database/)

이 빠른 시작에서는 배웁니다 방법:

- NuGet 패키지 관리자를 사용 하 여 NuGet 패키지를 프로젝트에 추가 합니다.
- SQLite.NET 데이터베이스에서 데이터를 로컬로 저장 합니다.

빠른 시작에서는 로컬 SQLite.NET 데이터베이스에서 데이터를 저장 하는 방법을 설명 합니다. 최종 애플리케이션은 다음과 같습니다.

[![](database-images/screenshots1-sml.png "페이지 정보")](database-images/screenshots1.png#lightbox "노트가")
[![](database-images/screenshots2-sml.png "항목 페이지를 참고")](database-images/screenshots2.png#lightbox "참고 항목 페이지")

### <a name="prerequisites"></a>전제 조건

성공적으로 완료 해야 합니다 [이전 빠른 시작](multi-page.md) 이 빠른 시작을 시도 하기 전에 합니다. 또는 다운로드 합니다 [이전 빠른 시작 샘플](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/MultiPage/) 이 빠른 시작에 대 한 시작 점으로 사용 합니다.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio를 사용하여 앱 업데이트

1. Visual Studio를 시작 하 고 정보 솔루션을 엽니다.

2. **솔루션 탐색기**를 선택 합니다 **메모** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **NuGet 패키지 관리...** :

    ![](database-images/vs/add-nuget-packages.png "NuGet 패키지 추가")    

3. **NuGet 패키지 관리자**에서 **찾아보기** 탭을 선택하고 **sqlite-net-pcl** NuGet 패키지를 검색하여 선택한 다음, **설치** 단추를 클릭하여 프로젝트에 추가합니다.

    ![](database-images/vs/add-package.png "패키지 추가")

    > [!NOTE]
    > 이름이 유사한 NuGet 패키지가 여러 개 있습니다. 올바른 패키지에는 이러한 특성이 있습니다.
    > - **작성자:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet 링크:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > 패키지 이름에도 불구하고 이 NuGet 패키지는 .NET 표준 패키지에서 사용할 수 있습니다.

    이 패키지는 데이터베이스 작업을 애플리케이션에 통합하는 데 사용됩니다.

4. **솔루션 탐색기**를 **정보** 프로젝트를 엽니다 **Note.cs** 에 **모델** 폴더 및 기존 코드를 코드를 다음과 같습니다.

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

    이 클래스 정의 `Note` 응용 프로그램에서 각 메모에 대 한 데이터를 저장 하는 모델입니다. 합니다 `ID` 속성으로 표시 됩니다 `PrimaryKey` 하 고 `AutoIncrement` 되도록 각 특성 `Note` SQLite.NET 데이터베이스에 인스턴스 SQLite.NET에서 제공 하는 고유 id를 갖습니다.

    변경 내용을 저장 **Note.cs** 키를 눌러 **CTRL + S**, 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

5. **솔루션 탐색기**, 라는 새 폴더 추가 **데이터** 에 **메모** 프로젝트.

6. **솔루션 탐색기**의 **메모** 프로젝트 라는 새 클래스를 추가 **NoteDatabase** 에 **데이터** 폴더.

7. **NoteDatabase.cs**, 기존 코드를 다음 코드로 바꿉니다.

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

    이 클래스에는 데이터베이스 만들기, 데이터를 읽고에서 데이터를 쓸 것에서 데이터를 삭제 하는 코드가 포함 됩니다. 코드는 데이터베이스 작업을 백그라운드 스레드로 이동시키는 비동기 SQLite.NET API를 사용합니다. 또한 `NoteDatabase` 생성자는 데이터베이스 파일의 경로를 인수로 사용합니다. 이 경로에서 제공 하는 `App` 다음 단계에는 클래스입니다.

    변경 내용을 저장 **NoteDatabase.cs** 키를 눌러 **CTRL + S**, 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

8. **솔루션 탐색기**의 **노트** 프로젝트를 두 번 클릭 **App.xaml.cs** 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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
            ...
        }
    }
    ```

    이 코드는 정의 `Database` 만드는 새 속성 `NoteDatabase` 데이터베이스의 파일 이름에 대 한 인수로 전달 단일 인스턴스로 `NoteDatabase` 생성자입니다. 데이터베이스를 싱글톤으로 노출하면 애플리케이션이 실행되는 동안 열린 상태로 유지되는 단일 데이터베이스 연결이 생성되므로 데이터베이스 작업이 수행될 때마다 데이터베이스 파일을 열거나 닫는 비용을 피할 수 있는 이점이 있습니다.

    **CTRL+S** 키를 눌러 변경 내용을 **App.xaml.cs**에 저장하고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

9. **솔루션 탐색기**의 **노트** 프로젝트를 두 번 클릭 **NotesPage.xaml.cs** 하 여 엽니다. 그런 다음 대체는 `OnAppearing` 메서드를 다음 코드로:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    이 코드는 [ `ListView` ](xref:Xamarin.Forms.ListView) 데이터베이스에 저장 된 모든 정보를 사용 하 여 합니다.

    변경 내용을 저장 **NotesPage.xaml.cs** 키를 눌러 **CTRL + S**, 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

10. **솔루션 탐색기**를 두 번 클릭 **NoteEntryPage.xaml.cs** 하 여 엽니다. 그런 다음 대체 합니다 `OnSaveButtonClicked` 고 `OnDeleteButtonClicked` 메서드를 다음 코드로:

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

      `NoteEntryPage` 저장을 `Note` 에서 단일 메모를 나타내는 인스턴스를 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 페이지의 합니다. 경우는 `OnSaveButtonClicked` 이벤트 처리기가 실행을 `Note` 인스턴스 데이터베이스에 저장 되 고 응용 프로그램의 이전 페이지로 다시 이동 합니다. 경우는 `OnDeleteButtonClicked` 이벤트 처리기가 실행을 `Note` 인스턴스가 데이터베이스에서 삭제 되 고 응용 프로그램의 이전 페이지로 다시 이동 합니다.

      변경 내용을 저장 **NoteEntryPage.xaml.cs** 키를 눌러 **CTRL + S**, 파일을 닫습니다.

11. 빌드 및 각 플랫폼에서 프로젝트를 실행 합니다. 자세한 내용은 [빠른 시작을 작성](single-page.md#building-the-quickstart)합니다.

    에 **NotesPage** 키를 누릅니다를 **+** 단추를를 **NoteEntryPage** 메모를 입력 합니다. 응용 프로그램 메모를 저장 합니다.이 다시로 탐색 한 후 합니다 **NotesPage**합니다.

    다양 한 응용 프로그램 동작을 관찰 하기 위해 다양 한 길이, 메모를 입력 합니다.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Mac용 Visual Studio를 사용하여 앱 업데이트

1. Mac 용 Visual Studio를 시작 하 고 정보 프로젝트를 엽니다.

2. 에 **Solution Pad**를 선택 합니다 **정보** 프로젝트, 마우스 오른쪽 단추로 **추가 > NuGet 패키지 추가...** :

    ![](database-images/vsmac/add-nuget-packages.png "NuGet 패키지 추가")    

3. **패키지 추가** 창에서 **sqlite-net-pcl** NuGet 패키지를 검색하여 선택한 다음, **패키지 추가** 단추를 클릭하여 프로젝트에 추가합니다.

    ![](database-images/vsmac/add-package.png "패키지 추가")

    > [!NOTE]
    > 이름이 유사한 NuGet 패키지가 여러 개 있습니다. 올바른 패키지에는 이러한 특성이 있습니다.
    > - **작성자:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet 링크:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > 패키지 이름에도 불구하고 이 NuGet 패키지는 .NET 표준 패키지에서 사용할 수 있습니다.

    이 패키지는 데이터베이스 작업을 애플리케이션에 통합하는 데 사용됩니다.

4. 에 **Solution Pad**의 **정보** 프로젝트를 엽니다 **Note.cs** 에 **모델** 폴더 및 다음을 사용 하 여 기존 코드 코드:

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

    이 클래스 정의 `Note` 응용 프로그램에서 각 메모에 대 한 데이터를 저장 하는 모델입니다. 합니다 `ID` 속성으로 표시 됩니다 `PrimaryKey` 하 고 `AutoIncrement` 되도록 각 특성 `Note` SQLite.NET 데이터베이스에 인스턴스 SQLite.NET에서 제공 하는 고유 id를 갖습니다.

    변경 내용을 저장 **Note.cs** 를 선택 하 여 **파일 > 저장** (또는 키를 눌러  **&#8984; + S**), 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

5. 에 **Solution Pad**, 라는 새 폴더를 추가 **데이터** 에 **메모** 프로젝트.

6. 에 **Solution Pad**의 **정보** 프로젝트 라는 새 클래스를 추가 **NoteDatabase** 에 **데이터** 폴더.

7. **NoteDatabase.cs**, 기존 코드를 다음 코드로 바꿉니다.

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

    이 클래스에는 데이터베이스 만들기, 데이터를 읽고에서 데이터를 쓸 것에서 데이터를 삭제 하는 코드가 포함 됩니다. 코드는 데이터베이스 작업을 백그라운드 스레드로 이동시키는 비동기 SQLite.NET API를 사용합니다. 또한 `NoteDatabase` 생성자는 데이터베이스 파일의 경로를 인수로 사용합니다. 이 경로에서 제공 하는 `App` 다음 단계에는 클래스입니다.

    변경 내용을 저장 **NoteDatabase.cs** 를 선택 하 여 **파일 > 저장** (또는 키를 눌러  **&#8984; + S**), 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

8. 에 **Solution Pad**의 **정보** 프로젝트를 두 번 클릭 **App.xaml.cs** 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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
            ...
        }
    }
    ```

    이 코드는 정의 `Database` 만드는 새 속성 `NoteDatabase` 데이터베이스의 파일 이름에 대 한 인수로 전달 단일 인스턴스로 `NoteDatabase` 생성자입니다. 데이터베이스를 싱글톤으로 노출하면 애플리케이션이 실행되는 동안 열린 상태로 유지되는 단일 데이터베이스 연결이 생성되므로 데이터베이스 작업이 수행될 때마다 데이터베이스 파일을 열거나 닫는 비용을 피할 수 있는 이점이 있습니다.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **App.xaml.cs**에 저장하고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

9. 에 **Solution Pad**의 **정보** 프로젝트를 두 번 클릭 **NotesPage.xaml.cs** 하 여 엽니다. 그런 다음 대체는 `OnAppearing` 메서드를 다음 코드로:

    ```csharp
    protected override async void OnAppearing()
    {
        base.OnAppearing();

        listView.ItemsSource = await App.Database.GetNotesAsync();
    }
    ```    

    이 코드는 [ `ListView` ](xref:Xamarin.Forms.ListView) 데이터베이스에 저장 된 모든 정보를 사용 하 여 합니다.

    변경 내용을 저장 **NotesPage.xaml.cs** 를 선택 하 여 **파일 > 저장** (또는 키를 눌러  **&#8984; + S**), 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

10. 에 **Solution Pad**를 두 번 클릭 **NoteEntryPage.xaml.cs** 하 여 엽니다. 그런 다음 대체 합니다 `OnSaveButtonClicked` 고 `OnDeleteButtonClicked` 메서드를 다음 코드로:

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

      `NoteEntryPage` 저장을 `Note` 에서 단일 메모를 나타내는 인스턴스를 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 페이지의 합니다. 경우는 `OnSaveButtonClicked` 이벤트 처리기가 실행을 `Note` 인스턴스 데이터베이스에 저장 되 고 응용 프로그램의 이전 페이지로 다시 이동 합니다. 경우는 `OnDeleteButtonClicked` 이벤트 처리기가 실행을 `Note` 인스턴스가 데이터베이스에서 삭제 되 고 응용 프로그램의 이전 페이지로 다시 이동 합니다.

      변경 내용을 저장 **NoteEntryPage.xaml.cs** 를 선택 하 여 **파일 > 저장** (또는 키를 눌러  **&#8984; + S**), 파일을 닫습니다.

11. 빌드 및 각 플랫폼에서 프로젝트를 실행 합니다. 자세한 내용은 [빠른 시작을 작성](single-page.md#building-the-quickstart)합니다.

    에 **NotesPage** 키를 누릅니다를 **+** 단추를를 **NoteEntryPage** 메모를 입력 합니다. 응용 프로그램 메모를 저장 합니다.이 다시로 탐색 한 후 합니다 **NotesPage**합니다.

    다양 한 응용 프로그램 동작을 관찰 하기 위해 다양 한 길이, 메모를 입력 합니다.

::: zone-end

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서 학습 하는 방법.

- NuGet 패키지 관리자를 사용 하 여 NuGet 패키지를 프로젝트에 추가 합니다.
- SQLite.NET 데이터베이스에서 데이터를 로컬로 저장 합니다.

XAML 스타일을 사용 하 여 응용 프로그램의 스타일, 다음 빠른 시작을 계속 진행 하세요.

> [!div class="nextstepaction"]
> [다음](styling.md)

## <a name="related-links"></a>관련 링크

- [노트(샘플)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Database/)
- [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)
