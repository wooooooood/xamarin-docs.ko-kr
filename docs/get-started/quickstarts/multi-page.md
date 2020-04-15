---
title: 다중 페이지 Xamarin.Forms 애플리케이션에서 탐색 수행
description: 이 문서에서는 단일 노트 저장이 가능한 단일 페이지 애플리케이션을 여러 노트의 저장이 가능한 다중 페이지 애플리케이션으로 전환하는 방법을 설명합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 9DC3B3D6-6CBC-4705-BE80-3D86A9E65F92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 9ce02b4c6412eab1f4b1003b262573c59940286c
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "68653792"
---
# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>다중 페이지 Xamarin.Forms 애플리케이션에서 탐색 수행

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)

이 빠른 시작에서 다음과 같은 작업을 수행하는 방법을 알아봅니다.

- Xamarin.Forms 솔루션에 페이지를 추가합니다.
- 페이지 간 탐색을 수행합니다.
- 데이터 바인딩을 사용하여 사용자 인터페이스 요소와 데이터 원본 간 데이터 동기화를 수행합니다.

이 빠른 시작에서는 단일 노트 저장이 가능한 단일 페이지 플랫폼 간 Xamarin.Forms 애플리케이션을 여러 노트의 저장이 가능한 다중 페이지 애플리케이션으로 전환하는 방법을 안내합니다. 최종 애플리케이션은 다음과 같습니다.

[![](multi-page-images/screenshots1-sml.png "Notes Page")](multi-page-images/screenshots1.png#lightbox "Notes Page")
[![](multi-page-images/screenshots2-sml.png "Note Entry Page")](multi-page-images/screenshots2.png#lightbox "Note Entry Page")

### <a name="prerequisites"></a>사전 요구 사항

이 빠른 시작을 시도하기 전에 [이전 빠른 시작](single-page.md)을 성공적으로 완료해야 합니다. 또는 [이전 빠른 시작 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/)을 다운로드하고 이 빠른 시작의 시작점으로 사용하세요.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio를 사용하여 앱 업데이트

1. Visual Studio를 실행합니다. 시작 창에서 최근의 프로젝트/솔루션 목록 가운데 **Notes** 솔루션을 클릭하거나 **프로젝트 또는 솔루션 열기**를 클릭하고 **프로젝트/솔루션 열기** 대화 상자에서 Notes 프로젝트에 대한 솔루션 파일을 선택합니다.

    ![](multi-page-images/vs/open-solution.png "Open Project")

2. **솔루션 탐색기**에서 **Notes** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가 > 새 폴더**를 선택합니다.

    ![](multi-page-images/vs/add-new-item.png "Add New Item")

3. **솔루션 탐색기**에서 새 폴더의 이름을 **Models**로 지정합니다.

    ![](multi-page-images/vs/name-folder.png "Models Folder")

4. **솔루션 탐색기**에서 **Models** 폴더를 마우스 오른쪽 단추로 클릭하고 **추가 > 새 항목...** 을 선택합니다.

    ![](multi-page-images/vs/add-new-models-file.png "Add New File")

5. **새 항목 추가** 대화 상자에서 **Visual C# 항목 > Class**를 선택하고 새 파일에 **Note**라는 이름을 지정한 뒤 **추가** 단추를 클릭합니다.

    ![](multi-page-images/vs/add-note-class.png "Add Note Class")

    그러면 **Notes** 프로젝트의 **Models** 폴더에 **Note**라는 클래스가 추가됩니다.

6. **Note.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;

    namespace Notes.Models
    {
        public class Note
        {
            public string Filename { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    이 클래스는 애플리케이션의 각 노트에 대한 데이터를 저장하는 `Note` 모델을 정의합니다.    

    **CTRL+S** 키를 눌러 변경 내용을 **Note.cs**에 저장하고 파일을 닫습니다.

7. **솔루션 탐색기**에서 **Notes** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가 > 새 항목...** 을 선택합니다. **새 항목 추가** 대화 상자에서 **Visual C# 항목 > Xamarin.Forms > 콘텐츠 페이지**를 선택하고 새 파일의 이름을 **NoteEntryPage**로 지정한 뒤 **추가** 단추를 클릭합니다.

    ![](multi-page-images/vs/add-note-entry-page.png "Add Xamarin.Forms ContentPage")

    그러면 **NoteEntryPage**라는 새 페이지가 프로젝트의 루트 폴더에 추가됩니다. 이 페이지는 애플리케이션의 두 번째 페이지입니다.

8. **NoteEntryPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.NoteEntryPage"
                   Title="Note Entry">
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <Grid>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="*" />
                      <ColumnDefinition Width="*" />
                  </Grid.ColumnDefinitions>
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      이 코드는 텍스트 입력을 위한 하나의 [`Editor`](xref:Xamarin.Forms.Editor)와 파일을 저장 또는 삭제하도록 애플리케이션에 지시하는 두 개의 [`Button`](xref:Xamarin.Forms.Button) 인스턴스로 구성된 페이지의 사용자 인터페이스를 선언적으로 정의합니다. 두 개의 `Button` 인스턴스가 [`Grid`](xref:Xamarin.Forms.Grid)에 가로로 배치되고 `Editor` 및 `Grid`가 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 세로로 배치됩니다. 또한 `Editor`는 데이터 바인딩을 사용하여 `Note` 모델의 `Text` 속성에 바인딩합니다. 데이터 바인딩에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [데이터 바인딩](deepdive.md#data-binding)을 참조하세요.

      **CTRL+S**를 눌러 변경 내용을 **NoteEntryPage.xaml**에 저장하고 파일을 닫습니다.

9. **NoteEntryPage.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

      ```csharp
      using System;
      using System.IO;
      using Xamarin.Forms;
      using Notes.Models;

      namespace Notes
      {
          public partial class NoteEntryPage : ContentPage
          {
              public NoteEntryPage()
              {
                  InitializeComponent();
              }

              async void OnSaveButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  await Navigation.PopAsync();
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  await Navigation.PopAsync();
              }
          }
      }
      ```

      이 코드는 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)에서 단일 노트를 나타내는 `Note` 인스턴스를 저장합니다. **저장** [`Button`](xref:Xamarin.Forms.Button)을 누르면 `OnSaveButtonClicked` 이벤트 처리기가 실행되어 이름이 임의로 지정된 새 파일에 `Editor`의 콘텐츠가 저장되며, 노트가 업데이트 중이라면 기존 파일에 저장됩니다. 어느 쪽이든 파일은 애플리케이션의 로컬 애플리케이션 데이터 폴더에 저장됩니다. 그런 다음에는 메서드가 이전 페이지로 다시 이동합니다. **삭제** `Button`을 누르면 `OnDeleteButtonClicked` 이벤트 처리기가 실행되어 파일을 삭제하고(있는 경우) 이전 페이지로 다시 이동합니다. 탐색에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [탐색](deepdive.md#navigation)을 참조하세요.

      **CTRL+S** 키를 눌러 변경 내용을 **NoteEntryPage.xaml.cs**에 저장하고 파일을 닫습니다.

      > [!WARNING]
      > 이 시점에서 애플리케이션을 빌드하려고 하면 후속 단계에서 수정될 오류가 발생합니다.

10. **솔루션 탐색기**에서 **Notes** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가 > 새 항목...** 을 선택합니다. **새 항목 추가** 대화 상자에서 **Visual C# 항목 > Xamarin.Forms > 콘텐츠 페이지**를 선택하고 새 파일의 이름을 **NotesPage**로 지정한 뒤 **추가** 단추를 클릭합니다.

      그러면 **NotesPage**라는 페이지가 프로젝트의 루트 폴더에 추가됩니다. 이 페이지는 애플리케이션의 루트 페이지가 됩니다.

11. **NotesPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>
        <ListView x:Name="listView"
                  Margin="20"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage>
    ```

    이 코드는 [`ListView`](xref:Xamarin.Forms.ListView)와 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)으로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. `ListView`는 데이터 바인딩을 사용하여 애플리케이션에서 검색된 노트를 표시하며, 노트를 선택하게 되면 `NoteEntryPage`로 이동하여 노트를 수정할 수 있습니다. 또는 `ToolbarItem`을 눌러 새 노트를 만들 수도 있습니다. 데이터 바인딩에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [데이터 바인딩](deepdive.md#data-binding)을 참조하세요.

    **CTRL+S**를 눌러 변경 내용을 **NotesPage.xaml**에 저장하고 파일을 닫습니다.

12. **NotesPage.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Xamarin.Forms;
    using Notes.Models;

    namespace Notes
    {
        public partial class NotesPage : ContentPage
        {
            public NotesPage()
            {
                InitializeComponent();
            }

            protected override void OnAppearing()
            {
                base.OnAppearing();

                var notes = new List<Note>();

                var files = Directory.EnumerateFiles(App.FolderPath, "*.notes.txt");
                foreach (var filename in files)
                {
                    notes.Add(new Note
                    {
                        Filename = filename,
                        Text = File.ReadAllText(filename),
                        Date = File.GetCreationTime(filename)
                    });
                }

                listView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnNoteAddedClicked(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new NoteEntryPage
                {
                    BindingContext = new Note()
                });
            }

            async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
            {
                if (e.SelectedItem != null)
                {
                    await Navigation.PushAsync(new NoteEntryPage
                    {
                        BindingContext = e.SelectedItem as Note
                    });
                }
            }
        }
    }
    ```    

    이 코드는 `NotesPage`의 기능을 정의합니다. 페이지가 표시되면 `OnAppearing` 메서드가 실행되며, 그러면 [`ListView`](xref:Xamarin.Forms.ListView)가 로컬 애플리케이션 데이터 폴더에서 검색된 모든 노트로 채워집니다. [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)을 누르면 `OnNoteAddedClicked` 이벤트 처리기가 실행됩니다. 이 메서드는 `NoteEntryPage`로 이동해 새 `Note` 인스턴스로 연결되는 `NoteEntryPage`의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 설정합니다. `ListView`의 항목을 선택하면 `OnListViewItemSelected` 이벤트 처리기가 실행됩니다. 이 메서드는 `NoteEntryPage`로 이동해 선택된 `Note` 인스턴스로 연결되는 `NoteEntryPage`의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 설정합니다. 탐색에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [탐색](deepdive.md#navigation)을 참조하세요.

    **CTRL+S** 키를 눌러 변경 내용을 **NotesPage.xaml.cs**에 저장하고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 애플리케이션을 빌드하려고 하면 후속 단계에서 수정될 오류가 발생합니다.

13. **솔루션 탐색기**에서 **App.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            public static string FolderPath { get; private set; }

            public App()
            {
                InitializeComponent();
                FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
                MainPage = new NavigationPage(new NotesPage());
            }
            // ...
        }
    }
    ```

    이 코드는 `System.IO` 네임스페이스에 대해 네임스페이스 선언을 추가하고 `string` 형식의 정적 `FolderPath` 속성에 대해 선언을 추가합니다. `FolderPath` 속성은 노트 데이터가 저장될 디바이스에 경로를 저장하는 데 사용됩니다. 또한 이 코드는 `App` 생성자의 `FolderPath` 속성을 초기화하고 [`MainPage`](xref:Xamarin.Forms.Application.MainPage) 속성이 `NotesPage`의 인스턴스를 호스팅하는 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)가 되도록 초기화합니다. 탐색에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [탐색](deepdive.md#navigation)을 참조하세요.

    **CTRL+S** 키를 눌러 변경 내용을 **App.xaml.cs**에 저장하고 파일을 닫습니다.

14. **솔루션 탐색기**의 **Notes** 프로젝트에서 **MainPage.xaml**을 마우스 오른쪽 단추로 클릭하고 **삭제**를 선택합니다. 표시되는 대화 상자에서 **확인** 단추를 눌러 하드 디스크의 파일을 제거합니다.

    그러면 더 이상 사용되지 않는 페이지가 제거됩니다.

15. 각 플랫폼에서 프로젝트를 빌드하고 실행합니다. 자세한 내용은 [빠른 시작 빌드](single-page.md#building-the-quickstart)를 참조하세요.

    **NotesPage**에서 **+** 단추를 눌러 **NoteEntryPage**로 이동하고 노트를 입력합니다. 노트를 저장한 후 애플리케이션은 **NotesPage**로 다시 이동합니다.

    다양한 길이의 여러 노트를 입력하여 애플리케이션 동작을 관찰합니다.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Mac용 Visual Studio를 사용하여 앱 업데이트

1. Mac용 Visual Studio. 시작 창에서 **열기**를 클릭하고, 대화 상자에서 Notes 프로젝트에 대한 솔루션 파일을 선택합니다.

    ![](multi-page-images/vsmac/open-solution.png "Open Solution")

2. **Solution Pad**에서 **Notes** 프로젝트를 선택해 마우스 오른쪽 단추로 클릭하고 **추가 > 새 폴더**를 선택합니다.

    ![](multi-page-images/vsmac/add-new-folder.png "Add New Folder")

3. **Solution Pad**에서 새 폴더의 이름을 **Models**로 지정합니다.

    ![](multi-page-images/vsmac/name-folder.png "Models Folder")

4. **Solution Pad**에서 **Models** 폴더를 선택해 마우스 오른쪽 단추로 클릭하고 **추가 > 새 파일...** 을 선택합니다.

    ![](multi-page-images/vsmac/add-new-models-file.png "Add New File")

5. **새 파일** 대화 상자에서 **General > Empty Class**를 선택하고, 새 파일에 **Note**라는 이름을 지정하고 **새로 만들기** 단추를 클릭합니다.

    ![](multi-page-images/vsmac/add-note-class.png "Add Note Class")

    그러면 **Notes** 프로젝트의 **Models** 폴더에 **Note**라는 클래스가 추가됩니다.

6. **Note.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;

    namespace Notes.Models
    {
        public class Note
        {
            public string Filename { get; set; }
            public string Text { get; set; }
            public DateTime Date { get; set; }
        }
    }
    ```

    이 클래스는 애플리케이션의 각 노트에 대한 데이터를 저장하는 `Note` 모델을 정의합니다.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **Note.cs**에 저장하고 파일을 닫습니다.

7. **Solution Pad**에서 **Notes** 프로젝트를 선택해 마우스 오른쪽 단추로 클릭하고 **추가 > 새 파일...** 을 선택합니다. **새 파일** 대화 상자에서 **Forms > Forms ContentPage XAML**을 선택하고, 새 파일에 **NoteEntryPage**라는 이름을 지정하고 **새로 만들기** 단추를 클릭합니다.

    ![](multi-page-images/vsmac/add-note-entry-page.png "Add Xamarin.Forms ContentPage")

    그러면 **NoteEntryPage**라는 새 페이지가 프로젝트의 루트 폴더에 추가됩니다. 이 페이지는 애플리케이션의 두 번째 페이지입니다.

8. **NoteEntryPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

      ```xaml
      <?xml version="1.0" encoding="UTF-8"?>
      <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                   x:Class="Notes.NoteEntryPage"
                   Title="Note Entry">
          <StackLayout Margin="20">
              <Editor Placeholder="Enter your note"
                      Text="{Binding Text}"
                      HeightRequest="100" />
              <Grid>
                  <Grid.ColumnDefinitions>
                      <ColumnDefinition Width="*" />
                      <ColumnDefinition Width="*" />
                  </Grid.ColumnDefinitions>
                  <Button Text="Save"
                          Clicked="OnSaveButtonClicked" />
                  <Button Grid.Column="1"
                          Text="Delete"
                          Clicked="OnDeleteButtonClicked"/>
              </Grid>
          </StackLayout>
      </ContentPage>
      ```

      이 코드는 텍스트 입력을 위한 하나의 [`Editor`](xref:Xamarin.Forms.Editor)와 파일을 저장 또는 삭제하도록 애플리케이션에 지시하는 두 개의 [`Button`](xref:Xamarin.Forms.Button) 인스턴스로 구성된 페이지의 사용자 인터페이스를 선언적으로 정의합니다. 두 개의 `Button` 인스턴스가 [`Grid`](xref:Xamarin.Forms.Grid)에 가로로 배치되고 `Editor` 및 `Grid`가 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 세로로 배치됩니다. 또한 `Editor`는 데이터 바인딩을 사용하여 `Note` 모델의 `Text` 속성에 바인딩합니다. 데이터 바인딩에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [데이터 바인딩](deepdive.md#data-binding)을 참조하세요.

      **파일 > 저장**을 선택하여(또는 **&#8984; + S**를 누름)하여 변경 내용을 **NoteEntryPage.xaml**에 저장하고 파일을 닫습니다.

9. **NoteEntryPage.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

      ```csharp
      using System;
      using System.IO;
      using Xamarin.Forms;
      using Notes.Models;

      namespace Notes
      {
          public partial class NoteEntryPage : ContentPage
          {
              public NoteEntryPage()
              {
                  InitializeComponent();
              }

              async void OnSaveButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (string.IsNullOrWhiteSpace(note.Filename))
                  {
                      // Save
                      var filename = Path.Combine(App.FolderPath, $"{Path.GetRandomFileName()}.notes.txt");
                      File.WriteAllText(filename, note.Text);
                  }
                  else
                  {
                      // Update
                      File.WriteAllText(note.Filename, note.Text);
                  }

                  await Navigation.PopAsync();
              }

              async void OnDeleteButtonClicked(object sender, EventArgs e)
              {
                  var note = (Note)BindingContext;

                  if (File.Exists(note.Filename))
                  {
                      File.Delete(note.Filename);
                  }

                  await Navigation.PopAsync();
              }
          }
      }
      ```

      이 코드는 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)에서 단일 노트를 나타내는 `Note` 인스턴스를 저장합니다. **저장** [`Button`](xref:Xamarin.Forms.Button)을 누르면 `OnSaveButtonClicked` 이벤트 처리기가 실행되어 이름이 임의로 지정된 새 파일에 `Editor`의 콘텐츠가 저장되며, 노트가 업데이트 중이라면 기존 파일에 저장됩니다. 어느 쪽이든 파일은 애플리케이션의 로컬 애플리케이션 데이터 폴더에 저장됩니다. 그런 다음에는 메서드가 이전 페이지로 다시 이동합니다. **삭제** `Button`을 누르면 `OnDeleteButtonClicked` 이벤트 처리기가 실행되어 파일을 삭제하고(있는 경우) 이전 페이지로 다시 이동합니다. 탐색에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [탐색](deepdive.md#navigation)을 참조하세요.

      **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름) 변경 내용을 **NoteEntryPage.xaml.cs**에 저장하고 파일을 닫습니다.

      > [!WARNING]
      > 이 시점에서 애플리케이션을 빌드하려고 하면 후속 단계에서 수정될 오류가 발생합니다.

10. **Solution Pad**에서 **Notes** 프로젝트를 선택해 마우스 오른쪽 단추로 클릭하고 **추가 > 새 파일...** 을 선택합니다. **새 파일** 대화 상자에서 **Forms > Forms ContentPage XAML**을 선택하고, 새 파일에 **NotesPage**라는 이름을 지정하고 **새로 만들기** 단추를 클릭합니다.

      그러면 **NotesPage**라는 페이지가 프로젝트의 루트 폴더에 추가됩니다. 이 페이지는 애플리케이션의 루트 페이지가 됩니다.

11. **NotesPage.xaml**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>
        <ListView x:Name="listView"
                  Margin="20"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage>
    ```

    이 코드는 [`ListView`](xref:Xamarin.Forms.ListView)와 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)으로 구성된 페이지에 대한 사용자 인터페이스를 선언적으로 정의합니다. `ListView`는 데이터 바인딩을 사용하여 애플리케이션에서 검색된 노트를 표시하며, 노트를 선택하게 되면 `NoteEntryPage`로 이동하여 노트를 수정할 수 있습니다. 또는 `ToolbarItem`을 눌러 새 노트를 만들 수도 있습니다. 데이터 바인딩에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [데이터 바인딩](deepdive.md#data-binding)을 참조하세요.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **NotesPage.xaml**에 저장하고 파일을 닫습니다.

12. **NotesPage.xaml.cs**에서 템플릿 코드를 모두 제거하고 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Xamarin.Forms;
    using Notes.Models;

    namespace Notes
    {
        public partial class NotesPage : ContentPage
        {
            public NotesPage()
            {
                InitializeComponent();
            }

            protected override void OnAppearing()
            {
                base.OnAppearing();

                var notes = new List<Note>();

                var files = Directory.EnumerateFiles(App.FolderPath, "*.notes.txt");
                foreach (var filename in files)
                {
                    notes.Add(new Note
                    {
                        Filename = filename,
                        Text = File.ReadAllText(filename),
                        Date = File.GetCreationTime(filename)
                    });
                }

                listView.ItemsSource = notes
                    .OrderBy(d => d.Date)
                    .ToList();
            }

            async void OnNoteAddedClicked(object sender, EventArgs e)
            {
                await Navigation.PushAsync(new NoteEntryPage
                {
                    BindingContext = new Note()
                });
            }

            async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
            {
                if (e.SelectedItem != null)
                {
                    await Navigation.PushAsync(new NoteEntryPage
                    {
                        BindingContext = e.SelectedItem as Note
                    });
                }
            }
        }
    }
    ```    

    이 코드는 `NotesPage`의 기능을 정의합니다. 페이지가 표시되면 `OnAppearing` 메서드가 실행되며, 그러면 [`ListView`](xref:Xamarin.Forms.ListView)가 로컬 애플리케이션 데이터 폴더에서 검색된 모든 노트로 채워집니다. [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)을 누르면 `OnNoteAddedClicked` 이벤트 처리기가 실행됩니다. 이 메서드는 `NoteEntryPage`로 이동해 새 `Note` 인스턴스로 연결되는 `NoteEntryPage`의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 설정합니다. `ListView`의 항목을 선택하면 `OnListViewItemSelected` 이벤트 처리기가 실행됩니다. 이 메서드는 `NoteEntryPage`로 이동해 선택된 `Note` 인스턴스로 연결되는 `NoteEntryPage`의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 설정합니다. 탐색에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [탐색](deepdive.md#navigation)을 참조하세요.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **NotesPage.xaml.cs**에 저장하고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 애플리케이션을 빌드하려고 하면 후속 단계에서 수정될 오류가 발생합니다.

13. **Solution Pad**에서 **App.xaml.cs**를 두 번 클릭하여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class App : Application
        {
            public static string FolderPath { get; private set; }

            public App()
            {
                InitializeComponent();
                FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
                MainPage = new NavigationPage(new NotesPage());
            }
            // ...
        }
    }
    ```

    이 코드는 `System.IO` 네임스페이스에 대해 네임스페이스 선언을 추가하고 `string` 형식의 정적 `FolderPath` 속성에 대해 선언을 추가합니다. `FolderPath` 속성은 노트 데이터가 저장될 디바이스에 경로를 저장하는 데 사용됩니다. 또한 이 코드는 `App` 생성자의 `FolderPath` 속성을 초기화하고 [`MainPage`](xref:Xamarin.Forms.Application.MainPage) 속성이 `NotesPage`의 인스턴스를 호스팅하는 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)가 되도록 초기화합니다. 탐색에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [탐색](deepdive.md#navigation)을 참조하세요.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **App.xaml.cs**에 저장하고 파일을 닫습니다.

14. **Solution Pad**의 **Notes** 프로젝트에서 **MainPage.xaml**을 마우스 오른쪽 단추로 클릭하고 **제거**를 선택합니다. 표시되는 대화 상자에서 **삭제** 단추를 눌러 하드 디스크의 파일을 제거합니다.

    그러면 더 이상 사용되지 않는 페이지가 제거됩니다.

15. 각 플랫폼에서 프로젝트를 빌드하고 실행합니다. 자세한 내용은 [빠른 시작 빌드](single-page.md#building-the-quickstart)를 참조하세요.

    **NotesPage**에서 **+** 단추를 눌러 **NoteEntryPage**로 이동하고 노트를 입력합니다. 노트를 저장한 후 애플리케이션은 **NotesPage**로 다시 이동합니다.

    다양한 길이의 여러 노트를 입력하여 애플리케이션 동작을 관찰합니다.

::: zone-end

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 다음과 같은 방법을 배웠습니다.

- Xamarin.Forms 솔루션에 페이지를 추가합니다.
- 페이지 간 탐색을 수행합니다.
- 데이터 바인딩을 사용하여 사용자 인터페이스 요소와 데이터 원본 간 데이터 동기화를 수행합니다.

로컬 SQLite.NET 데이터베이스에 데이터를 저장할 수 있도록 애플리케이션을 수정하려면 다음 빠른 시작 안내를 계속 진행하세요.

> [!div class="nextstepaction"]
> [다음](database.md)

## <a name="related-links"></a>관련 링크

- [노트(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)
- [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)
