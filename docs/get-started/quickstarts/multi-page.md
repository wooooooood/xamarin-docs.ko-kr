---
title: 다중 페이지 Xamarin.Forms 애플리케이션에서 탐색 수행
description: 이 문서에서는 단일 페이지 응용 프로그램을 여러 메모를 저장할 수 있는 다중 페이지 응용 프로그램에 저장할 수 있는 단일 페이지 응용 프로그램을 설정 하는 방법을 설명 합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 9DC3B3D6-6CBC-4705-BE80-3D86A9E65F92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2019
ms.openlocfilehash: 9ce02b4c6412eab1f4b1003b262573c59940286c
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "68653792"
---
# <a name="perform-navigation-in-a-multi-page-xamarinforms-application"></a>다중 페이지 Xamarin Forms 응용 프로그램에서 탐색 수행

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)

이 빠른 시작에서는 다음 방법에 대해 알아봅니다.

- Xamarin Forms 솔루션에 페이지를 추가 합니다.
- 페이지 간 탐색을 수행 합니다.
- 데이터 바인딩을 사용 하 여 사용자 인터페이스 요소와 데이터 원본 간에 데이터를 동기화 합니다.

이 빠른 시작에서는 단일 페이지를 여러 개의 노트를 저장할 수 있는 다중 페이지 응용 프로그램에 저장할 수 있는 단일 페이지 플랫폼 간 Xamarin.ios 응용 프로그램을 설정 하는 방법을 안내 합니다. 최종 애플리케이션은 다음과 같습니다.

[![](multi-page-images/screenshots1-sml.png "메모")](multi-page-images/screenshots1.png#lightbox "메모 페이지")페이지
[(multi-page-images/screenshots2-sml.png "메모 입력") 페이지![]](multi-page-images/screenshots2.png#lightbox "메모 입력 페이지")

### <a name="prerequisites"></a>전제 조건

이 빠른 시작을 시도 하기 전에 [이전 퀵 스타트](single-page.md) 를 성공적으로 완료 해야 합니다. 또는 [이전 빠른 시작 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-singlepage/) 을 다운로드 하 고이 빠른 시작의 시작 지점으로 사용 합니다.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio를 사용하여 앱 업데이트

1. Visual Studio를 실행합니다. 시작 창의 최근 프로젝트/솔루션 목록에서 **참고** 솔루션을 클릭 하거나, 프로젝트 **또는**솔루션 열기를 클릭 하 고, **프로젝트/솔루션 열기** 대화 상자에서 notes 프로젝트에 대 한 솔루션 파일을 선택 합니다.

    ![](multi-page-images/vs/open-solution.png "프로젝트 열기")

2. **솔루션 탐색기**에서 **Notes** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 폴더**를 선택 합니다.

    ![](multi-page-images/vs/add-new-item.png "새 항목 추가")

3. **솔루션 탐색기**에서 새 폴더의 이름을 **모델**으로 합니다.

    ![](multi-page-images/vs/name-folder.png "모델 폴더")

4. **솔루션 탐색기**에서 **모델** 폴더를 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **> 새 항목 추가**...를 선택 합니다.

    ![](multi-page-images/vs/add-new-models-file.png "새 파일 추가")

5. **새 항목 추가** 대화 상자에서 **시각적 항목 > C# 클래스**를 선택 하 고 **새 파일의**이름을로 표시 한 다음 **추가** 단추를 클릭 합니다.

    ![](multi-page-images/vs/add-note-class.png "메모 클래스 추가")

    그러면 **Notes** 프로젝트의 **모델** 폴더에 **Note** 라는 클래스가 추가 됩니다.

6. **Note.cs**에서 템플릿 코드를 모두 제거 하 고 다음 코드로 바꿉니다.

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

    이 클래스는 응용 `Note` 프로그램의 각 메모에 대 한 데이터를 저장 하는 모델을 정의 합니다.    

    **Ctrl + S**를 눌러 변경 내용을 **Note.cs** 에 저장 하 고 파일을 닫습니다.

7. **솔루션 탐색기**에서 **Notes** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **> 새 항목 추가**...를 선택 합니다. **새 항목 추가** 대화 상자에서 **NoteEntryPage** **> 콘텐츠 C# 페이지 > 시각적 항목**을 선택 하 고, 새 파일의 이름을로, **추가** 단추를 클릭 합니다.

    ![](multi-page-images/vs/add-note-entry-page.png "Xamarin Forms ContentPage 추가")

    그러면 **NoteEntryPage** 이라는 새 페이지가 프로젝트의 루트 폴더에 추가 됩니다. 이 페이지는 응용 프로그램의 두 번째 페이지가 됩니다.

8. **NoteEntryPage**에서 템플릿 코드를 모두 제거 하 고 다음 코드로 바꿉니다.

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

      이 코드는 텍스트 입력 [`Editor`](xref:Xamarin.Forms.Editor) 에 대 한로 구성 되는 페이지의 사용자 인터페이스와 파일을 저장 하거나 삭제 하도록 응용 프로그램에 지시 하는 두 개의 [`Button`](xref:Xamarin.Forms.Button) 인스턴스를 선언적으로 정의 합니다. 두 `Button` 인스턴스는에 가로로 배치 [`Grid`](xref:Xamarin.Forms.Grid)되며를 사용 `Editor` `Grid` 하 여에 수직으로 배치 [`StackLayout`](xref:Xamarin.Forms.StackLayout)됩니다. 또한는 `Editor` 데이터 바인딩을 사용 하 여 `Note` 모델의 `Text` 속성에 바인딩합니다. 데이터 바인딩에 대 한 자세한 내용은 Xamarin의 [데이터 바인딩](deepdive.md#data-binding) 빠른 시작 [심층](deepdive.md)이해를 참조 하세요.

      **Ctrl + S**를 눌러 변경 내용을 **NoteEntryPage** 에 저장 하 고 파일을 닫습니다.

9. **NoteEntryPage.xaml.cs**에서 템플릿 코드를 모두 제거 하 고 다음 코드로 바꿉니다.

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

      이 코드는 페이지 `Note` [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 의에서 단일 메모를 나타내는 인스턴스를 저장 합니다. `Editor` [저장`Button`](xref:Xamarin.Forms.Button) 을 누르면`OnSaveButtonClicked` 이벤트 처리기가 실행 되 고,이 처리기는의 콘텐츠를 임의로 생성 된 파일 이름이 있는 새 파일에 저장 하거나, 메모가 업데이트 되는 경우 기존 파일에 저장 합니다. 두 경우 모두 파일은 응용 프로그램의 로컬 응용 프로그램 데이터 폴더에 저장 됩니다. 그런 다음 메서드는 이전 페이지로 다시 이동 합니다. 삭제`Button` 를 누르면`OnDeleteButtonClicked` 이벤트 처리기가 실행 되 고,이 처리기가 있으면 파일이 삭제 되 고 이전 페이지로 다시 이동 합니다. 탐색에 대 한 자세한 내용은 [Xamarin](deepdive.md)의 [탐색](deepdive.md#navigation) 을 참조 하세요.

      **Ctrl + S**를 눌러 변경 내용을 **NoteEntryPage.xaml.cs** 에 저장 하 고 파일을 닫습니다.

      > [!WARNING]
      > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

10. **솔루션 탐색기**에서 **Notes** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **> 새 항목 추가**...를 선택 합니다. **새 항목 추가** 대화 상자에서 **NotesPage** **> 콘텐츠 C# 페이지 > 시각적 항목**을 선택 하 고, 새 파일의 이름을로, **추가** 단추를 클릭 합니다.

      그러면 **NotesPage** 이라는 페이지가 프로젝트의 루트 폴더에 추가 됩니다. 이 페이지는 응용 프로그램의 루트 페이지가 됩니다.

11. **NotesPage**에서 템플릿 코드를 모두 제거 하 고 다음 코드로 바꿉니다.

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

    이 코드는 [`ListView`](xref:Xamarin.Forms.ListView) [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)및으로 구성 된 페이지에 대 한 사용자 인터페이스를 선언적으로 정의 합니다. 는 `ListView` 데이터 바인딩을 사용 하 여 응용 프로그램에서 검색 한 모든 메모를 표시 하 고, 메모를 선택 하면 메모 `NoteEntryPage` 를 수정할 수 있는으로 이동 합니다. 또는를 눌러 `ToolbarItem`새 노트를 만들 수 있습니다. 데이터 바인딩에 대 한 자세한 내용은 Xamarin의 [데이터 바인딩](deepdive.md#data-binding) 빠른 시작 [심층](deepdive.md)이해를 참조 하세요.

    **Ctrl + S**를 눌러 변경 내용을 **NotesPage** 에 저장 하 고 파일을 닫습니다.

12. **NotesPage.xaml.cs**에서 템플릿 코드를 모두 제거 하 고 다음 코드로 바꿉니다.

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

    이 코드는의 기능 `NotesPage`을 정의 합니다. 페이지가 표시 `OnAppearing` 되 면 메서드를 실행 합니다 .이 메서드는 로컬 [`ListView`](xref:Xamarin.Forms.ListView) 응용 프로그램 데이터 폴더에서 검색 된 메모를 사용 하 여를 채웁니다. [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) 를`OnNoteAddedClicked` 누르면 이벤트 처리기가 실행 됩니다. 이 메서드는를 탐색 `NoteEntryPage`하 고 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 의 `NoteEntryPage` 를 새 `Note` 인스턴스로 설정 합니다. 에서 `ListView` 항목을 `OnListViewItemSelected` 선택 하면 이벤트 처리기가 실행 됩니다. 이 메서드는를 탐색 `NoteEntryPage`하 고 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 의 `NoteEntryPage` 를 선택한 `Note` 인스턴스로 설정 합니다. 탐색에 대 한 자세한 내용은 [Xamarin](deepdive.md)의 [탐색](deepdive.md#navigation) 을 참조 하세요.

    **Ctrl + S**를 눌러 변경 내용을 **NotesPage.xaml.cs** 에 저장 하 고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

13. **솔루션 탐색기**에서 **App.xaml.cs** 를 두 번 클릭 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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

    이 코드는 `System.IO` 네임 스페이스에 대 한 네임 스페이스 선언을 추가 하 고 형식의 `string`정적 `FolderPath` 속성에 대 한 선언을 추가 합니다. `FolderPath` 속성은 메모 데이터가 저장 되는 장치에 경로를 저장 하는 데 사용 됩니다. 또한이 `FolderPath` 코드는 `App` [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) `NotesPage`생성자에서 속성을 초기화 하 고 [속성을의인스턴스를호스팅하는로초기화합니다.`MainPage`](xref:Xamarin.Forms.Application.MainPage) 탐색에 대 한 자세한 내용은 [Xamarin](deepdive.md)의 [탐색](deepdive.md#navigation) 을 참조 하세요.

    **CTRL+S** 키를 눌러 변경 내용을 **App.xaml.cs**에 저장하고 파일을 닫습니다.

14. **솔루션 탐색기**의 **Notes** 프로젝트에서 **mainpage**을 마우스 오른쪽 단추로 클릭 하 고 **삭제**를 선택 합니다. 표시 되는 대화 상자에서 **확인** 단추를 눌러 하드 디스크에서 파일을 제거 합니다.

    이렇게 하면 더 이상 사용 되지 않는 페이지가 제거 됩니다.

15. 각 플랫폼에서 프로젝트를 빌드하고 실행 합니다. 자세한 내용은 [빠른 시작 빌드](single-page.md#building-the-quickstart)를 참조 하세요.

    **NotesPage** 에서 **+** 단추를 눌러 **NoteEntryPage** 로 이동 하 고 메모를 입력 합니다. 저장 한 후에 응용 프로그램은 **NotesPage**로 다시 이동 합니다.

    다양 한 길이의 메모를 입력 하 여 응용 프로그램 동작을 관찰할 수 있습니다.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Mac용 Visual Studio를 사용하여 앱 업데이트

1. Mac용 Visual Studio. 시작 창에서 **열기**를 클릭 하 고 대화 상자에서 Notes 프로젝트에 대 한 솔루션 파일을 선택 합니다.

    ![](multi-page-images/vsmac/open-solution.png "솔루션 열기")

2. **Solution Pad**에서 **Notes** 프로젝트를 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **추가 > 새 폴더**를 선택 합니다.

    ![](multi-page-images/vsmac/add-new-folder.png "새 폴더 추가")

3. **Solution Pad**에서 새 폴더의 이름을 **모델**으로 합니다.

    ![](multi-page-images/vsmac/name-folder.png "모델 폴더")

4. **Solution Pad**에서 **모델** 폴더를 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **새 파일 > 추가**...를 선택 합니다.

    ![](multi-page-images/vsmac/add-new-models-file.png "새 파일 추가")

5. **새 파일** 대화 상자에서 **일반 > 빈 클래스**를 선택 하 고 **새 파일의**이름을로 입력 한 다음 **새로 만들기** 단추를 클릭 합니다.

    ![](multi-page-images/vsmac/add-note-class.png "메모 클래스 추가")

    그러면 **Notes** 프로젝트의 **모델** 폴더에 **Note** 라는 클래스가 추가 됩니다.

6. **Note.cs**에서 템플릿 코드를 모두 제거 하 고 다음 코드로 바꿉니다.

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

    이 클래스는 응용 `Note` 프로그램의 각 메모에 대 한 데이터를 저장 하는 모델을 정의 합니다.

    **파일 > 저장** 을 선택 하거나  **&#8984; + S**를 눌러 변경 내용을 **Note.cs** 에 저장 하 고 파일을 닫습니다.

7. **Solution Pad**에서 **Notes** 프로젝트를 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **새 파일 > 추가**...를 선택 합니다. **새 파일** 대화 상자에서 **양식 > 양식 contentpage XAML**을 선택 하 고 새 파일의 이름을 **NoteEntryPage**으로 선택한 다음 **새로 만들기** 단추를 클릭 합니다.

    ![](multi-page-images/vsmac/add-note-entry-page.png "Xamarin Forms ContentPage 추가")

    그러면 **NoteEntryPage** 이라는 새 페이지가 프로젝트의 루트 폴더에 추가 됩니다. 이 페이지는 응용 프로그램의 두 번째 페이지가 됩니다.

8. **NoteEntryPage**에서 템플릿 코드를 모두 제거 하 고 다음 코드로 바꿉니다.

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

      이 코드는 텍스트 입력 [`Editor`](xref:Xamarin.Forms.Editor) 에 대 한로 구성 되는 페이지의 사용자 인터페이스와 파일을 저장 하거나 삭제 하도록 응용 프로그램에 지시 하는 두 개의 [`Button`](xref:Xamarin.Forms.Button) 인스턴스를 선언적으로 정의 합니다. 두 `Button` 인스턴스는에 가로로 배치 [`Grid`](xref:Xamarin.Forms.Grid)되며를 사용 `Editor` `Grid` 하 여에 수직으로 배치 [`StackLayout`](xref:Xamarin.Forms.StackLayout)됩니다. 또한는 `Editor` 데이터 바인딩을 사용 하 여 `Note` 모델의 `Text` 속성에 바인딩합니다. 데이터 바인딩에 대 한 자세한 내용은 Xamarin의 [데이터 바인딩](deepdive.md#data-binding) 빠른 시작 [심층](deepdive.md)이해를 참조 하세요.

      **파일 > 저장** 을 선택 하거나  **&#8984; + S**를 눌러 변경 내용을 **NoteEntryPage** 에 저장 하 고 파일을 닫습니다.

9. **NoteEntryPage.xaml.cs**에서 템플릿 코드를 모두 제거 하 고 다음 코드로 바꿉니다.

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

      이 코드는 페이지 `Note` [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 의에서 단일 메모를 나타내는 인스턴스를 저장 합니다. `Editor` [저장`Button`](xref:Xamarin.Forms.Button) 을 누르면`OnSaveButtonClicked` 이벤트 처리기가 실행 되 고,이 처리기는의 콘텐츠를 임의로 생성 된 파일 이름이 있는 새 파일에 저장 하거나, 메모가 업데이트 되는 경우 기존 파일에 저장 합니다. 두 경우 모두 파일은 응용 프로그램의 로컬 응용 프로그램 데이터 폴더에 저장 됩니다. 그런 다음 메서드는 이전 페이지로 다시 이동 합니다. 삭제`Button` 를 누르면`OnDeleteButtonClicked` 이벤트 처리기가 실행 되 고,이 처리기가 있으면 파일이 삭제 되 고 이전 페이지로 다시 이동 합니다. 탐색에 대 한 자세한 내용은 [Xamarin](deepdive.md)의 [탐색](deepdive.md#navigation) 을 참조 하세요.

      **파일 > 저장** 을 선택 하거나  **&#8984; + S**를 눌러 변경 내용을 **NoteEntryPage.xaml.cs** 에 저장 하 고 파일을 닫습니다.

      > [!WARNING]
      > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

10. **Solution Pad**에서 **Notes** 프로젝트를 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **새 파일 > 추가**...를 선택 합니다. **새 파일** 대화 상자에서 양식 **> 양식 contentpage XAML**을 선택 하 고, 새 파일의 이름을 **NotesPage**로, **새로 만들기** 단추를 클릭 합니다.

      그러면 **NotesPage** 이라는 페이지가 프로젝트의 루트 폴더에 추가 됩니다. 이 페이지는 응용 프로그램의 루트 페이지가 됩니다.

11. **NotesPage**에서 템플릿 코드를 모두 제거 하 고 다음 코드로 바꿉니다.

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

    이 코드는 [`ListView`](xref:Xamarin.Forms.ListView) [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)및으로 구성 된 페이지에 대 한 사용자 인터페이스를 선언적으로 정의 합니다. 는 `ListView` 데이터 바인딩을 사용 하 여 응용 프로그램에서 검색 한 모든 메모를 표시 하 고, 메모를 선택 하면 메모 `NoteEntryPage` 를 수정할 수 있는으로 이동 합니다. 또는를 눌러 `ToolbarItem`새 노트를 만들 수 있습니다. 데이터 바인딩에 대 한 자세한 내용은 Xamarin의 [데이터 바인딩](deepdive.md#data-binding) 빠른 시작 [심층](deepdive.md)이해를 참조 하세요.

    **파일 > 저장** 을 선택 하거나  **&#8984; + S**를 눌러 변경 내용을 **NotesPage** 에 저장 하 고 파일을 닫습니다.

12. **NotesPage.xaml.cs**에서 템플릿 코드를 모두 제거 하 고 다음 코드로 바꿉니다.

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

    이 코드는의 기능 `NotesPage`을 정의 합니다. 페이지가 표시 `OnAppearing` 되 면 메서드를 실행 합니다 .이 메서드는 로컬 [`ListView`](xref:Xamarin.Forms.ListView) 응용 프로그램 데이터 폴더에서 검색 된 메모를 사용 하 여를 채웁니다. [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) 를`OnNoteAddedClicked` 누르면 이벤트 처리기가 실행 됩니다. 이 메서드는를 탐색 `NoteEntryPage`하 고 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 의 `NoteEntryPage` 를 새 `Note` 인스턴스로 설정 합니다. 에서 `ListView` 항목을 `OnListViewItemSelected` 선택 하면 이벤트 처리기가 실행 됩니다. 이 메서드는를 탐색 `NoteEntryPage`하 고 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 의 `NoteEntryPage` 를 선택한 `Note` 인스턴스로 설정 합니다. 탐색에 대 한 자세한 내용은 [Xamarin](deepdive.md)의 [탐색](deepdive.md#navigation) 을 참조 하세요.

    **파일 > 저장** 을 선택 하거나  **&#8984; + S**를 눌러 변경 내용을 **NotesPage.xaml.cs** 에 저장 하 고 파일을 닫습니다.

    > [!WARNING]
    > 이 시점에서 응용 프로그램을 빌드하려고 하면 이후 단계에서 수정 될 오류가 발생 합니다.

13. **Solution Pad**에서 **App.xaml.cs** 를 두 번 클릭 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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

    이 코드는 `System.IO` 네임 스페이스에 대 한 네임 스페이스 선언을 추가 하 고 형식의 `string`정적 `FolderPath` 속성에 대 한 선언을 추가 합니다. `FolderPath` 속성은 메모 데이터가 저장 되는 장치에 경로를 저장 하는 데 사용 됩니다. 또한이 `FolderPath` 코드는 `App` [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) `NotesPage`생성자에서 속성을 초기화 하 고 [속성을의인스턴스를호스팅하는로초기화합니다.`MainPage`](xref:Xamarin.Forms.Application.MainPage) 탐색에 대 한 자세한 내용은 [Xamarin](deepdive.md)의 [탐색](deepdive.md#navigation) 을 참조 하세요.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **App.xaml.cs**에 저장하고 파일을 닫습니다.

14. **Solution Pad**의 **Notes** 프로젝트에서 **mainpage**을 마우스 오른쪽 단추로 클릭 하 고 **제거**를 선택 합니다. 표시 되는 대화 상자에서 **삭제** 단추를 눌러 하드 디스크에서 파일을 제거 합니다.

    이렇게 하면 더 이상 사용 되지 않는 페이지가 제거 됩니다.

15. 각 플랫폼에서 프로젝트를 빌드하고 실행 합니다. 자세한 내용은 [빠른 시작 빌드](single-page.md#building-the-quickstart)를 참조 하세요.

    **NotesPage** 에서 **+** 단추를 눌러 **NoteEntryPage** 로 이동 하 고 메모를 입력 합니다. 저장 한 후에 응용 프로그램은 **NotesPage**로 다시 이동 합니다.

    다양 한 길이의 메모를 입력 하 여 응용 프로그램 동작을 관찰할 수 있습니다.

::: zone-end

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 다음 방법에 대해 알아보았습니다.

- Xamarin Forms 솔루션에 페이지를 추가 합니다.
- 페이지 간 탐색을 수행 합니다.
- 데이터 바인딩을 사용 하 여 사용자 인터페이스 요소와 데이터 원본 간에 데이터를 동기화 합니다.

로컬 SQLite.NET 데이터베이스에 데이터를 저장 하도록 응용 프로그램을 수정 하려면 다음 빠른 시작을 계속 진행 합니다.

> [!div class="nextstepaction"]
> [다음](database.md)

## <a name="related-links"></a>관련 링크

- [노트(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-multipage/)
- [Xamarin.ios 빠른 시작 심층 살펴보기](deepdive.md)
