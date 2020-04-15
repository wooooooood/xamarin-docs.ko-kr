---
title: 플랫폼 간 Xamarin.Forms 애플리케이션 스타일 지정
description: 이 문서에서는 XAML 스타일로 플랫폼 간 Xamarin.Forms 애플리케이션의 스타일을 지정하는 방법을 설명합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: CCCF8E57-D021-4542-8709-5808570FC26A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/07/2020
ms.openlocfilehash: fbd957c68d7a9aa2f8e44c91fab6174d8ed72014
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "77068751"
---
# <a name="style-a-cross-platform-xamarinforms-application"></a>플랫폼 간 Xamarin.Forms 애플리케이션 스타일 지정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)

이 빠른 시작에서 다음과 같은 작업을 수행하는 방법을 알아봅니다.

- XAML 스타일을 사용하여 Xamarin.Forms 애플리케이션 스타일을 만듭니다.

빠른 시작은 XAML 스타일을 사용하여 플랫폼 간 Xamarin.Forms 애플리케이션의 스타일을 만드는 방법을 안내합니다. 최종 애플리케이션은 다음과 같습니다.

[![](styling-images/screenshots1-sml.png "Notes Page")](styling-images/screenshots1.png#lightbox "Notes Page")
[![](styling-images/screenshots2-sml.png "Note Entry Page")](styling-images/screenshots2.png#lightbox "Note Entry Page")

### <a name="prerequisites"></a>사전 요구 사항

이 빠른 시작을 시도하기 전에 [이전 빠른 시작](database.md)을 성공적으로 완료해야 합니다. 또는 [이전 빠른 시작 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/)을 다운로드하고 이 빠른 시작의 시작점으로 사용하세요.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio를 사용하여 앱 업데이트

1. Visual Studio를 시작하고 Notes 솔루션을 엽니다.

2. **솔루션 탐색기**의 **Notes** 프로젝트에서 **App.xaml**을 두 번 클릭하여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="NavigationBarColor">#1976D2</Color>
            <Color x:Key="NavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{StaticResource NavigationBarColor}" />
                 <Setter Property="BarTextColor"
                        Value="{StaticResource NavigationBarTextColor}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    이 코드는 [`Thickness`](xref:Xamarin.Forms.Thickness) 값, 일련의 [`Color`](xref:Xamarin.Forms.Color) 값 및 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)의 암시적 스타일과 [`ContentPage`](xref:Xamarin.Forms.ContentPage)를 정의합니다. 애플리케이션 수준 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 있는 해당 스타일은 애플리케이션 전체에서 사용할 수 있습니다. XAML 스타일 지정에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [스타일 지정](deepdive.md#styling)을 참조하세요.

    **CTRL+S**를 눌러 변경 내용을 **App.xaml**에 저장하고 파일을 닫습니다.

3. **솔루션 탐색기**의 **Notes** 프로젝트에서 **NotesPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type ListView}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>
        </ContentPage.Resources>

        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>

        <ListView x:Name="listView"
                  Margin="{StaticResource PageMargin}"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              TextColor="Black"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    이 코드는 [`ListView`](xref:Xamarin.Forms.ListView)에 대한 암시적 스타일을 페이지 수준 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 추가하고 `ListView.Margin` 속성을 애플리케이션 수준 `ResourceDictionary`에 정의된 값으로 설정합니다. `ListView` 암시적 스타일은 `NotesPage`에서만 사용되므로 페이지 수준 `ResourceDictionary`에 추가되었습니다. XAML 스타일 지정에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [스타일 지정](deepdive.md#styling)을 참조하세요.

    **CTRL+S**를 눌러 변경 내용을 **NotesPage.xaml**에 저장하고 파일을 닫습니다.

4. **솔루션 탐색기**의 **Notes** 프로젝트에서 **NoteEntryPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button"
                   ApplyToDerivedTypes="True"
                   CanCascade="True">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
                <Setter Property="CornerRadius" Value="5" />
            </Style>
        </ContentPage.Resources>

        <StackLayout Margin="{StaticResource PageMargin}">
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
                        Clicked="OnDeleteButtonClicked" />
            </Grid>
        </StackLayout>

    </ContentPage>
    ```

    이 코드는 [`Editor`](xref:Xamarin.Forms.Editor) 및 [`Button`](xref:Xamarin.Forms.Button) 보기에 대한 암시적 스타일을 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 페이지 수준에 추가하고 `StackLayout.Margin` 속성을 애플리케이션 수준 `ResourceDictionary`에 정의된 값으로 설정합니다. `Editor` 및 `Button` 암시적 스타일은 `NoteEntryPage`에서만 사용되므로 페이지 수준 `ResourceDictionary`에 추가되었습니다. XAML 스타일 지정에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [스타일 지정](deepdive.md#styling)을 참조하세요.

    **CTRL+S**를 눌러 변경 내용을 **NoteEntryPage.xaml**에 저장하고 파일을 닫습니다.

5. 각 플랫폼에서 프로젝트를 빌드하고 실행합니다. 자세한 내용은 [빠른 시작 빌드](single-page.md#building-the-quickstart)를 참조하세요.

    **NotesPage**에서 **+** 단추를 눌러 **NoteEntryPage**로 이동하고 메모를 입력합니다. 각 페이지에서 스타일이 이전 빠른 시작과 어떻게 달라졌는지 살펴봅니다.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Mac용 Visual Studio를 사용하여 앱 업데이트

1. Mac용 Visual Studio를 시작하고 Notes 프로젝트를 엽니다.

2. **Solution Pad**의 **Notes** 프로젝트에서 **App.xaml**을 두 번 클릭하여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="NavigationBarColor">#1976D2</Color>
            <Color x:Key="NavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{StaticResource NavigationBarColor}" />
                 <Setter Property="BarTextColor"
                        Value="{StaticResource NavigationBarTextColor}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    이 코드는 [`Thickness`](xref:Xamarin.Forms.Thickness) 값, 일련의 [`Color`](xref:Xamarin.Forms.Color) 값 및 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)의 암시적 스타일과 [`ContentPage`](xref:Xamarin.Forms.ContentPage)를 정의합니다. 애플리케이션 수준 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 있는 해당 스타일은 애플리케이션 전체에서 사용할 수 있습니다. XAML 스타일 지정에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [스타일 지정](deepdive.md#styling)을 참조하세요.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **App.xaml**에 저장하고 파일을 닫습니다.

3. **Solution Pad**의 **Notes** 프로젝트에서 **NotesPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NotesPage"
                 Title="Notes">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type ListView}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>
        </ContentPage.Resources>

        <ContentPage.ToolbarItems>
            <ToolbarItem Text="+"
                         Clicked="OnNoteAddedClicked" />
        </ContentPage.ToolbarItems>

        <ListView x:Name="listView"
                  Margin="{StaticResource PageMargin}"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Text}"
                              TextColor="Black"
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    이 코드는 [`ListView`](xref:Xamarin.Forms.ListView)에 대한 암시적 스타일을 페이지 수준 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 추가하고 `ListView.Margin` 속성을 애플리케이션 수준 `ResourceDictionary`에 정의된 값으로 설정합니다. `ListView` 암시적 스타일은 `NotesPage`에서만 사용되므로 페이지 수준 `ResourceDictionary`에 추가되었습니다. XAML 스타일 지정에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [스타일 지정](deepdive.md#styling)을 참조하세요.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **NotesPage.xaml**에 저장하고 파일을 닫습니다.

4. **Solution Pad**의 **Notes** 프로젝트에서 **NoteEntryPage.xaml**을 두 번 클릭하여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button"
                   ApplyToDerivedTypes="True"
                   CanCascade="True">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
                <Setter Property="CornerRadius" Value="5" />
            </Style>
        </ContentPage.Resources>

        <StackLayout Margin="{StaticResource PageMargin}">
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
                        Clicked="OnDeleteButtonClicked" />
            </Grid>
        </StackLayout>

    </ContentPage>
    ```

    이 코드는 [`Editor`](xref:Xamarin.Forms.Editor) 및 [`Button`](xref:Xamarin.Forms.Button) 보기에 대한 암시적 스타일을 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 페이지 수준에 추가하고 `StackLayout.Margin` 속성을 애플리케이션 수준 `ResourceDictionary`에 정의된 값으로 설정합니다. `Editor` 및 `Button` 암시적 스타일은 `NoteEntryPage`에서만 사용되므로 페이지 수준 `ResourceDictionary`에 추가되었습니다. XAML 스타일 지정에 대한 자세한 내용은 [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)에서 [스타일 지정](deepdive.md#styling)을 참조하세요.

    **파일 > 저장**을 선택(또는 **&#8984; + S**를 누름)하여 변경 내용을 **NoteEntryPage.xaml**에 저장하고 파일을 닫습니다.

5. 각 플랫폼에서 프로젝트를 빌드하고 실행합니다. 자세한 내용은 [빠른 시작 빌드](single-page.md#building-the-quickstart)를 참조하세요.

    **NotesPage**에서 **+** 단추를 눌러 **NoteEntryPage**로 이동하고 메모를 입력합니다. 각 페이지에서 스타일이 이전 빠른 시작과 어떻게 달라졌는지 살펴봅니다.

::: zone-end

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서 다음과 같은 방법을 배웠습니다.

- XAML 스타일을 사용하여 Xamarin.Forms 애플리케이션 스타일을 만듭니다.

Xamarin.Forms를 사용하여 애플리케이션을 개발하는 기본 사항에 대해 자세히 알아보려면 빠른 시작 심층 분석을 계속하세요.

> [!div class="nextstepaction"]
> [다음](deepdive.md)

## <a name="related-links"></a>관련 링크

- [노트(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)
- [Xamarin.Forms 빠른 시작 심층 분석](deepdive.md)
