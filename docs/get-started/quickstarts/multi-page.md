---
title: 다중 페이지 Xamarin.Forms 응용 프로그램에서 탐색을 수행 합니다.
description: 이 문서에서는 다중 페이지 응용 프로그램으로, 여러 정보를 저장할 수 있는 단일 메모를 저장할 수 있는 단일 페이지 응용 프로그램을 설정 하는 방법에 설명 합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 9DC3B3D6-6CBC-4705-BE80-3D86A9E65F92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/02/2019
ms.openlocfilehash: 48b1c93afa22bf069a2b47b6c9a259641e8ed70f
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55293337"
---
# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>다중 페이지 Xamarin.Forms 응용 프로그램에서 탐색을 수행 합니다.

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/MultiPage/)

이 빠른 시작에서는 배웁니다 방법:

- Xamarin.Forms 솔루션에 추가 페이지를 추가 합니다.
- 페이지 간의 탐색을 수행 합니다.
- 데이터 바인딩을 사용 하 여 사용자 인터페이스 요소와 해당 데이터 원본 간에 데이터를 동기화 합니다.

빠른 시작은 단일 페이지 플랫폼 간 Xamarin.Forms 응용 프로그램을 다중 페이지 응용 프로그램으로, 여러 정보를 저장할 수 있는 단일 메모를 저장할 수 있는 설정 하는 방법을 안내 합니다. 최종 애플리케이션은 다음과 같습니다.

[![](multi-page-images/screenshots1-sml.png "페이지 정보")](multi-page-images/screenshots1.png#lightbox "노트가")
[![](multi-page-images/screenshots2-sml.png "항목 페이지를 참고")](multi-page-images/screenshots2.png#lightbox "참고 항목 페이지")

### <a name="prerequisites"></a>전제 조건

성공적으로 완료 해야 합니다 [이전 빠른 시작](single-page.md) 이 빠른 시작을 시도 하기 전에 합니다. 또는 다운로드 합니다 [이전 빠른 시작 샘플](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/) 이 빠른 시작에 대 한 시작 점으로 사용 합니다.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio를 사용하여 앱 업데이트

1. Visual Studio를 실행합니다. 시작 페이지에서 클릭 **열려 있는 프로젝트 / 솔루션**, 및를 **프로젝트 열기** 정보 프로젝트에 대 한 솔루션 파일을 선택 하는 대화 상자:

    ![](multi-page-images/vs/open-solution.png "프로젝트 열기")

2. **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 **노트** 프로젝트를 마우스 **추가 > 새 폴더**:

    ![](multi-page-images/vs/add-new-item.png "새 항목 추가")

3. **솔루션 탐색기**에 새 폴더 이름을 **모델**:

    ![](multi-page-images/vs/name-folder.png "Models 폴더")

4. **솔루션 탐색기**를 선택 합니다 **모델** 폴더를 마우스 오른쪽 단추로 **추가 > 새 항목...** :

    ![](multi-page-images/vs/add-new-models-file.png "새 파일 추가")

5. 에 **새 항목 추가** 대화 상자에서 **Visual C# 항목 > 클래스**, 새 파일의 이름을 **참고**를 클릭 하 고는 **추가** 단추:

    ![](multi-page-images/vs/add-note-class.png "참고 클래스 추가")

    라는 클래스를 추가 하는이 **참고** 에 **모델** 폴더를 **메모** 프로젝트.

6. **Note.cs**, 모든 템플릿 코드를 제거 하 고 다음 코드로 바꿉니다.

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

    이 클래스 정의 `Note` 응용 프로그램에서 각 메모에 대 한 데이터를 저장 하는 모델입니다.    

    변경 내용을 저장 **Note.cs** 키를 눌러 **CTRL + S**, 파일을 닫습니다.

7. **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 **노트** 프로젝트를 마우스 **추가 > 새 항목...** . 에 **새 항목 추가** 대화 상자에서 **Visual C# 항목 > Xamarin.Forms > 콘텐츠 페이지**, 새 파일의 이름을 **NoteEntryPage**를 클릭 하 고는  **추가** 단추:

    ![](multi-page-images/vs/add-note-entry-page.png "Xamarin.Forms ContentPage 추가")

    이 명명 된 새 페이지 추가 **NoteEntryPage** 프로젝트의 루트 폴더에 있습니다. 이 페이지는 응용 프로그램에서 두 번째 페이지를 수 있습니다.

8. **NoteEntryPage.xaml**, 모든 템플릿 코드를 제거 하 고 다음 코드로 바꿉니다.

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

      구성 된 페이지에 대 한 사용자 인터페이스를 선언적으로 정의 하는이 코드는 [ `Editor` ](xref:Xamarin.Forms.Editor) 텍스트 입력 및 2에 대 한 [ `Button` ](xref:Xamarin.Forms.Button) 저장 또는 삭제 하 고 응용 프로그램이 있는 인스턴스를 파일입니다. 두 `Button` 인스턴스는 가로로 배치를 [ `Grid` ](xref:Xamarin.Forms.Grid)를 사용 하 여는 `Editor` 및 `Grid` 에 세로로 배치 되는 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). 또한를 `Editor` 바인딩할 데이터 바인딩을 사용 하는 `Text` 의 속성은 `Note` 모델. 데이터 바인딩에 대 한 자세한 내용은 참조 하세요. [증상](deepdive.md#data-binding) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

      변경 내용을 저장 **NoteEntryPage.xaml** 키를 눌러 **CTRL + S**, 파일을 닫습니다.

9. **NoteEntryPage.xaml.cs**, 모든 템플릿 코드를 제거 하 고 다음 코드로 바꿉니다.

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

      이 코드에서는 저장을 `Note` 에서 단일 메모를 나타내는 인스턴스를 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 페이지의 합니다. 경우는 **저장** [ `Button` ](xref:Xamarin.Forms.Button) 눌러져 합니다 `OnSaveButtonClicked` 의 콘텐츠를 저장 하거나 이벤트 처리기가 실행을 `Editor` 임의로 생성 된 파일 이름 사용 하 여 새 파일에 또는 메모를 업데이트 하는 경우 기존 파일입니다. 두 경우 모두 파일은 응용 프로그램에 대 한 로컬 응용 프로그램 데이터 폴더에 저장 됩니다. 다음 메서드는 이전 페이지로 다시 이동합니다. 경우는 **삭제** `Button` 눌러져는 `OnDeleteButtonClicked` 존재 하 고 이전 페이지로 다시 이동 된 파일을 삭제 하는 이벤트 처리기가 실행 합니다. 탐색에 대 한 자세한 내용은 참조 하세요. [탐색](deepdive.md#navigation) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

      변경 내용을 저장 **NoteEntryPage.xaml.cs** 키를 눌러 **CTRL + S**, 파일을 닫습니다.

      > [!WARNING]
      > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

10. **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 **노트** 프로젝트를 마우스 **추가 > 새 항목...** . 에 **새 항목 추가** 대화 상자에서 **Visual C# 항목 > Xamarin.Forms > 콘텐츠 페이지**, 새 파일의 이름을 **NotesPage**를 클릭 하 고는 **추가**  단추입니다.

      이 라는 페이지를 추가 합니다 **NotesPage** 프로젝트의 루트 폴더에 있습니다. 이 페이지에는 응용 프로그램의 루트 페이지 수 있습니다.

11. **NotesPage.xaml**, 모든 템플릿 코드를 제거 하 고 다음 코드로 바꿉니다.

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

    구성 된 페이지에 대 한 사용자 인터페이스를 선언적으로 정의 하는이 코드는 [ `ListView` ](xref:Xamarin.Forms.ListView) 와 [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem)합니다. 합니다 `ListView` 응용 프로그램에서 검색 되는 모든 메모를 표시 하려면 바인딩 및 메모를 선택 하는 데이터 이동은 `NoteEntryPage` 메모를 수정할 수 있습니다. 또는 키를 눌러 새 메모를 만들 수 있습니다는 `ToolbarItem`합니다. 데이터 바인딩에 대 한 자세한 내용은 참조 하세요. [증상](deepdive.md#data-binding) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    변경 내용을 저장 **NotesPage.xaml** 키를 눌러 **CTRL + S**, 파일을 닫습니다.

12. **NotesPage.xaml.cs**, 모든 템플릿 코드를 제거 하 고 다음 코드로 바꿉니다.

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

    이 코드에 대 한 기능을 정의 합니다 `NotesPage`합니다. 페이지가 표시 되는 `OnAppearing` 메서드를 실행 하는 채웁니다 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 로컬 응용 프로그램 데이터 폴더에서 검색 된 모든 정보를 사용 하 여 합니다. 경우는 [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) 눌러져는 `OnNoteAddedClicked` 이벤트 처리기가 실행 됩니다. 이 메서드를 탐색 합니다 `NoteEntryPage`설정, 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 의 `NoteEntryPage` 새 `Note` 인스턴스. 항목이 `ListView` 을 선택는 `OnListViewItemSelected` 이벤트 처리기가 실행 됩니다. 이 메서드를 탐색 합니다 `NoteEntryPage`설정, 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 의 `NoteEntryPage` 하 여 선택한 `Note` 인스턴스. 탐색에 대 한 자세한 내용은 참조 하세요. [탐색](deepdive.md#navigation) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    변경 내용을 저장 **NotesPage.xaml.cs** 키를 눌러 **CTRL + S**, 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

13. **솔루션 탐색기**를 두 번 클릭 **App.xaml.cs** 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
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
            ...
        }
    }
    ```

    이 코드에 대 한 네임 스페이스 선언을 추가 합니다 `System.IO` 네임 스페이스 정적에 대 한 선언을 추가 하 고 `FolderPath` 형식의 속성 `string`합니다. `FolderPath` 속성을 사용 하는 장치에서 경로 저장 데이터를 저장할 합니다. 코드를 초기화 하는 또한 합니다 `FolderPath` 속성에는 `App` 생성자 및 초기화를 [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) 속성을를 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 호스팅하는는 인스턴스의 `NotesPage`합니다. 탐색에 대 한 자세한 내용은 참조 하세요. [탐색](deepdive.md#navigation) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    **CTRL+S** 키를 눌러 변경 내용을 **App.xaml.cs**에 저장하고 파일을 닫습니다.

14. **솔루션 탐색기**를 **메모** 프로젝트를 마우스 오른쪽 단추로 클릭 **MainPage.xaml**, 선택한 **삭제**합니다. 키를 눌러 나타나는 대화 상자에는 **확인** 하드 디스크에서 파일을 제거 하려면 단추입니다.

    이 더 이상 사용 되는 페이지를 제거 합니다.

15. 빌드 및 각 플랫폼에서 프로젝트를 실행 합니다. 자세한 내용은 [빠른 시작을 작성](single-page.md#building-the-quickstart)합니다.

    에 **NotesPage** 키를 누릅니다를 **+** 단추를를 **NoteEntryPage** 메모를 입력 합니다. 응용 프로그램 메모를 저장 합니다.이 다시로 탐색 한 후 합니다 **NotesPage**합니다.

    다양 한 응용 프로그램 동작을 관찰 하기 위해 다양 한 길이, 메모를 입력 합니다.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Mac용 Visual Studio를 사용하여 앱 업데이트

1. Mac용 Visual Studio. 시작 페이지에서 클릭 **열기...** , 대화 상자에서 정보 프로젝트에 대 한 솔루션 파일을 선택 합니다.

    ![](multi-page-images/vsmac/open-solution.png "솔루션 열기")

2. 에 **Solution Pad**를 선택 합니다 **정보** 프로젝트를 마우스 오른쪽 단추로 **추가 > 새 폴더**:

    ![](multi-page-images/vsmac/add-new-folder.png "새 폴더 추가")

3. 에 **Solution Pad**에 새 폴더 이름을 **모델**:

    ![](multi-page-images/vsmac/name-folder.png "Models 폴더")

4. 에 **Solution Pad**를 선택 합니다 **모델** 폴더를 마우스 오른쪽 단추로 **추가 > 새 파일...** :

    ![](multi-page-images/vsmac/add-new-models-file.png "새 파일 추가")

5. 에 **새 파일** 대화 상자에서 **일반 > 빈 클래스**, 새 파일의 이름을 **참고**, 클릭 합니다 **새로 만들기** 단추:

    ![](multi-page-images/vsmac/add-note-class.png "참고 클래스 추가")

    라는 클래스를 추가 하는이 **참고** 에 **모델** 폴더를 **메모** 프로젝트.

6. **Note.cs**, 모든 템플릿 코드를 제거 하 고 다음 코드로 바꿉니다.

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

    이 클래스 정의 `Note` 응용 프로그램에서 각 메모에 대 한 데이터를 저장 하는 모델입니다.

    변경 내용을 저장 **Note.cs** 를 선택 하 여 **파일 > 저장** (또는 키를 눌러  **&#8984; + S**), 파일을 닫습니다.

7. 에 **Solution Pad**를 선택 합니다 **정보** 프로젝트를 마우스 오른쪽 단추로 **추가 > 새 파일...** . 에 **새 파일** 대화 상자에서 **Forms > Forms ContentPage XAML**, 새 파일의 이름을 **NoteEntryPage**, 클릭 합니다 **새로 만들기** 단추:

    ![](multi-page-images/vsmac/add-note-entry-page.png "Xamarin.Forms ContentPage 추가")

    이 명명 된 새 페이지 추가 **NoteEntryPage** 프로젝트의 루트 폴더에 있습니다. 이 페이지는 응용 프로그램에서 두 번째 페이지를 수 있습니다.

8. **NoteEntryPage.xaml**, 모든 템플릿 코드를 제거 하 고 다음 코드로 바꿉니다.

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

      구성 된 페이지에 대 한 사용자 인터페이스를 선언적으로 정의 하는이 코드는 [ `Editor` ](xref:Xamarin.Forms.Editor) 텍스트 입력 및 2에 대 한 [ `Button` ](xref:Xamarin.Forms.Button) 저장 또는 삭제 하 고 응용 프로그램이 있는 인스턴스를 파일입니다. 두 `Button` 인스턴스는 가로로 배치를 [ `Grid` ](xref:Xamarin.Forms.Grid)를 사용 하 여는 `Editor` 및 `Grid` 에 세로로 배치 되는 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). 또한를 `Editor` 바인딩할 데이터 바인딩을 사용 하는 `Text` 의 속성은 `Note` 모델. 데이터 바인딩에 대 한 자세한 내용은 참조 하세요. [증상](deepdive.md#data-binding) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

      변경 내용을 저장 **NoteEntryPage.xaml** 를 선택 하 여 **파일 > 저장** (또는 키를 눌러  **&#8984; + S**), 파일을 닫습니다.

9. **NoteEntryPage.xaml.cs**, 모든 템플릿 코드를 제거 하 고 다음 코드로 바꿉니다.

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

      이 코드에서는 저장을 `Note` 에서 단일 메모를 나타내는 인스턴스를 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 페이지의 합니다. 경우는 **저장** [ `Button` ](xref:Xamarin.Forms.Button) 눌러져 합니다 `OnSaveButtonClicked` 의 콘텐츠를 저장 하거나 이벤트 처리기가 실행을 `Editor` 임의로 생성 된 파일 이름 사용 하 여 새 파일에 또는 메모를 업데이트 하는 경우 기존 파일입니다. 두 경우 모두 파일은 응용 프로그램에 대 한 로컬 응용 프로그램 데이터 폴더에 저장 됩니다. 다음 메서드는 이전 페이지로 다시 이동합니다. 경우는 **삭제** `Button` 눌러져는 `OnDeleteButtonClicked` 존재 하 고 이전 페이지로 다시 이동 된 파일을 삭제 하는 이벤트 처리기가 실행 합니다. 탐색에 대 한 자세한 내용은 참조 하세요. [탐색](deepdive.md#navigation) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

      변경 내용을 저장 **NoteEntryPage.xaml.cs** 를 선택 하 여 **파일 > 저장** (또는 키를 눌러  **&#8984; + S**), 파일을 닫습니다.

      > [!WARNING]
      > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

10. 에 **Solution Pad**를 선택 합니다 **정보** 프로젝트를 마우스 오른쪽 단추로 **추가 > 새 파일...** . 에 **새 파일** 대화 상자에서 **Forms > Forms ContentPage XAML**, 새 파일의 이름을 **NotesPage**, 클릭 합니다 **새로 만들기** 단추.

      이 라는 페이지를 추가 합니다 **NotesPage** 프로젝트의 루트 폴더에 있습니다. 이 페이지에는 응용 프로그램의 루트 페이지 수 있습니다.

11. **NotesPage.xaml**, 모든 템플릿 코드를 제거 하 고 다음 코드로 바꿉니다.

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

    구성 된 페이지에 대 한 사용자 인터페이스를 선언적으로 정의 하는이 코드는 [ `ListView` ](xref:Xamarin.Forms.ListView) 와 [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem)합니다. 합니다 `ListView` 응용 프로그램에서 검색 되는 모든 메모를 표시 하려면 바인딩 및 메모를 선택 하는 데이터 이동은 `NoteEntryPage` 메모를 수정할 수 있습니다. 또는 키를 눌러 새 메모를 만들 수 있습니다는 `ToolbarItem`합니다. 데이터 바인딩에 대 한 자세한 내용은 참조 하세요. [증상](deepdive.md#data-binding) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    변경 내용을 저장 **NotesPage.xaml** 를 선택 하 여 **파일 > 저장** (또는 키를 눌러  **&#8984; + S**), 파일을 닫습니다.

12. **NotesPage.xaml.cs**, 모든 템플릿 코드를 제거 하 고 다음 코드로 바꿉니다.

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

    이 코드에 대 한 기능을 정의 합니다 `NotesPage`합니다. 페이지가 표시 되는 `OnAppearing` 메서드를 실행 하는 채웁니다 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 로컬 응용 프로그램 데이터 폴더에서 검색 된 모든 정보를 사용 하 여 합니다. 경우는 [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) 눌러져는 `OnNoteAddedClicked` 이벤트 처리기가 실행 됩니다. 이 메서드를 탐색 합니다 `NoteEntryPage`설정, 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 의 `NoteEntryPage` 새 `Note` 인스턴스. 항목이 `ListView` 을 선택는 `OnListViewItemSelected` 이벤트 처리기가 실행 됩니다. 이 메서드를 탐색 합니다 `NoteEntryPage`설정, 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 의 `NoteEntryPage` 하 여 선택한 `Note` 인스턴스. 탐색에 대 한 자세한 내용은 참조 하세요. [탐색](deepdive.md#navigation) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    변경 내용을 저장 **NotesPage.xaml.cs** 를 선택 하 여 **파일 > 저장** (또는 키를 눌러  **&#8984; + S**), 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

13. 에 **Solution Pad**를 두 번 클릭 **App.xaml.cs** 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
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
            ...
        }
    }
    ```

    이 코드에 대 한 네임 스페이스 선언을 추가 합니다 `System.IO` 네임 스페이스 정적에 대 한 선언을 추가 하 고 `FolderPath` 형식의 속성 `string`합니다. `FolderPath` 속성을 사용 하는 장치에서 경로 저장 데이터를 저장할 합니다. 코드를 초기화 하는 또한 합니다 `FolderPath` 속성에는 `App` 생성자 및 초기화를 [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) 속성을를 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 호스팅하는는 인스턴스의 `NotesPage`합니다. 탐색에 대 한 자세한 내용은 참조 하세요. [탐색](deepdive.md#navigation) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **App.xaml.cs**에 저장하고 파일을 닫습니다.

14. 에 **Solution Pad**를 **정보** 프로젝트를 마우스 오른쪽 단추로 클릭 **MainPage.xaml**, 선택한 **제거**합니다. 키를 눌러 나타나는 대화 상자에는 **삭제** 하드 디스크에서 파일을 제거 하려면 단추입니다.

    이 더 이상 사용 되는 페이지를 제거 합니다.

15. 빌드 및 각 플랫폼에서 프로젝트를 실행 합니다. 자세한 내용은 [빠른 시작을 작성](single-page.md#building-the-quickstart)합니다.

    에 **NotesPage** 키를 누릅니다를 **+** 단추를를 **NoteEntryPage** 메모를 입력 합니다. 응용 프로그램 메모를 저장 합니다.이 다시로 탐색 한 후 합니다 **NotesPage**합니다.

    다양 한 응용 프로그램 동작을 관찰 하기 위해 다양 한 길이, 메모를 입력 합니다.

::: zone-end

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서 학습 하는 방법.

- Xamarin.Forms 솔루션에 추가 페이지를 추가 합니다.
- 페이지 간의 탐색을 수행 합니다.
- 데이터 바인딩을 사용 하 여 사용자 인터페이스 요소와 해당 데이터 원본 간에 데이터를 동기화 합니다.

파일을 로컬 SQLite.NET 데이터베이스에서 데이터를 저장 되도록 응용 프로그램을 수정 하려면 다음 빠른 시작을 계속 합니다.

> [!div class="nextstepaction"]
> [다음](database.md)

## <a name="related-links"></a>관련 링크

- [노트(샘플)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/MultiPage/)
- [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)
