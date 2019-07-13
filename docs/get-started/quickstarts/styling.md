---
title: 플랫폼 간 Xamarin.Forms 애플리케이션 스타일 지정
description: 이 문서에는 XAML 스타일을 사용 하 여 플랫폼 간 Xamarin.Forms 응용 프로그램 스타일을 지정 하는 방법을 설명 합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: CCCF8E57-D021-4542-8709-5808570FC26A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/02/2019
ms.openlocfilehash: 1b68afc1f3d3c57a5c336e9d30c97ce2375acb9f
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864342"
---
# <a name="style-a-cross-platform-xamarinforms-application"></a>플랫폼 간 Xamarin.Forms 응용 프로그램 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Styled/)

이 빠른 시작에서는 배웁니다 방법:

- XAML 스타일을 사용 하 여 Xamarin.Forms 응용 프로그램 스타일을 지정 합니다.

빠른 시작에서는 XAML 스타일을 사용 하 여 플랫폼 간 Xamarin.Forms 응용 프로그램 스타일을 지정 하는 방법을 설명 합니다. 최종 애플리케이션은 다음과 같습니다.

[![](styling-images/screenshots1-sml.png "페이지 정보")](styling-images/screenshots1.png#lightbox "노트가")
[![](styling-images/screenshots2-sml.png "항목 페이지를 참고")](styling-images/screenshots2.png#lightbox "참고 항목 페이지")

### <a name="prerequisites"></a>필수 구성 요소

성공적으로 완료 해야 합니다 [이전 빠른 시작](database.md) 이 빠른 시작을 시도 하기 전에 합니다. 또는 다운로드 합니다 [이전 빠른 시작 샘플](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Database/) 이 빠른 시작에 대 한 시작 점으로 사용 합니다.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio를 사용하여 앱 업데이트

1. Visual Studio를 시작 하 고 정보 솔루션을 엽니다.

2. **솔루션 탐색기**의 **노트** 프로젝트를 두 번 클릭 **App.xaml** 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">WhiteSmoke</Color>
            <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
            <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
            <Color x:Key="iOSNavigationBarTextColor">Black</Color>
            <Color x:Key="AndroidNavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                           Android={StaticResource AndroidNavigationBarColor}}" />
                 <Setter Property="BarTextColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                           Android={StaticResource AndroidNavigationBarTextColor}}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    이 코드는 정의 [ `Thickness` ](xref:Xamarin.Forms.Thickness) 값, 일련의 [ `Color` ](xref:Xamarin.Forms.Color) 값 및에 대 한 암시적 스타일 합니다 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 및 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). 응용 프로그램 수준에 있는 참고는 이러한 스타일 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 응용 프로그램 전체에서 사용할 수 있습니다. XAML 스타일 지정에 대 한 자세한 내용은 참조 하세요. [스타일](deepdive.md#styling) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    변경 내용을 저장 **App.xaml** 키를 눌러 **CTRL + S**, 파일을 닫습니다.

3. **솔루션 탐색기**의 **노트** 프로젝트를 두 번 클릭 **NotesPage.xaml** 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    이 코드에 대 한 암시적 스타일을 추가 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 페이지 수준 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 가져오거나 설정 합니다 `ListView.Margin` 속성 응용 프로그램 수준에서 정의 된 값을 `ResourceDictionary` . 합니다 `ListView` 암시적 스타일 페이지 수준에 추가 되었습니다 `ResourceDictionary`이므로 사용 되는 `NotesPage`합니다. XAML 스타일 지정에 대 한 자세한 내용은 참조 하세요. [스타일](deepdive.md#styling) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    변경 내용을 저장 **NotesPage.xaml** 키를 눌러 **CTRL + S**, 파일을 닫습니다.

4. **솔루션 탐색기**의 **노트** 프로젝트를 두 번 클릭 **NoteEntryPage.xaml** 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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
                <Setter Property="BackgroundColor" Value="LightGray" />
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="5" />
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

    이 코드에 대 한 암시적 스타일을 추가 합니다 [ `Editor` ](xref:Xamarin.Forms.Editor) 및 [ `Button` ](xref:Xamarin.Forms.Button) 페이지 수준 보기 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)를 설정 하 고는 `StackLayout.Margin` 응용 프로그램 수준에 정의 된 값으로 속성 `ResourceDictionary`합니다. `Editor` 하 고 `Button` 암시적 스타일 페이지 수준에 추가 되었습니다 `ResourceDictionary`이므로에서 사용 됩니다는 `NoteEntryPage`. XAML 스타일 지정에 대 한 자세한 내용은 참조 하세요. [스타일](deepdive.md#styling) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    변경 내용을 저장 **NoteEntryPage.xaml** 키를 눌러 **CTRL + S**, 파일을 닫습니다.

5. 빌드 및 각 플랫폼에서 프로젝트를 실행 합니다. 자세한 내용은 [빠른 시작을 작성](single-page.md#building-the-quickstart)합니다.

    에 **NotesPage** 키를 누릅니다를 **+** 단추를를 **NoteEntryPage** 메모를 입력 합니다. 각 페이지에서 이전 빠른 시작에서 스타일 어떻게 변경 되었는지 확인 합니다.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Mac용 Visual Studio를 사용하여 앱 업데이트

1. Mac 용 Visual Studio를 시작 하 고 정보 프로젝트를 엽니다.

2. 에 **Solution Pad**의 **정보** 프로젝트를 두 번 클릭 **App.xaml** 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppBackgroundColor">WhiteSmoke</Color>
            <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
            <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
            <Color x:Key="iOSNavigationBarTextColor">Black</Color>
            <Color x:Key="AndroidNavigationBarTextColor">White</Color>

            <!-- Implicit styles -->
            <Style TargetType="{x:Type NavigationPage}">
                <Setter Property="BarBackgroundColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                           Android={StaticResource AndroidNavigationBarColor}}" />
                 <Setter Property="BarTextColor"
                        Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                           Android={StaticResource AndroidNavigationBarTextColor}}" />           
            </Style>

            <Style TargetType="{x:Type ContentPage}"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    이 코드는 정의 [ `Thickness` ](xref:Xamarin.Forms.Thickness) 값, 일련의 [ `Color` ](xref:Xamarin.Forms.Color) 값 및에 대 한 암시적 스타일 합니다 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 및 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). 응용 프로그램 수준에 있는 참고는 이러한 스타일 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 응용 프로그램 전체에서 사용할 수 있습니다. XAML 스타일 지정에 대 한 자세한 내용은 참조 하세요. [스타일](deepdive.md#styling) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    변경 내용을 저장 **App.xaml** 를 선택 하 여 **파일 > 저장** (또는 키를 눌러  **&#8984; + S**), 파일을 닫습니다.

3. 에 **Solution Pad**의 **정보** 프로젝트를 두 번 클릭 **NotesPage.xaml** 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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
                              Detail="{Binding Date}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>

    </ContentPage>
    ```

    이 코드에 대 한 암시적 스타일을 추가 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 페이지 수준 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), 가져오거나 설정 합니다 `ListView.Margin` 속성 응용 프로그램 수준에서 정의 된 값을 `ResourceDictionary` . 합니다 `ListView` 암시적 스타일 페이지 수준에 추가 되었습니다 `ResourceDictionary`이므로 사용 되는 `NotesPage`합니다. XAML 스타일 지정에 대 한 자세한 내용은 참조 하세요. [스타일](deepdive.md#styling) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    변경 내용을 저장 **NotesPage.xaml** 를 선택 하 여 **파일 > 저장** (또는 키를 눌러  **&#8984; + S**), 파일을 닫습니다.

4. 에 **Solution Pad**의 **정보** 프로젝트를 두 번 클릭 **NoteEntryPage.xaml** 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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
                <Setter Property="BackgroundColor" Value="LightGray" />
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="5" />
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

    이 코드에 대 한 암시적 스타일을 추가 합니다 [ `Editor` ](xref:Xamarin.Forms.Editor) 및 [ `Button` ](xref:Xamarin.Forms.Button) 페이지 수준 보기 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)를 설정 하 고는 `StackLayout.Margin` 응용 프로그램 수준에 정의 된 값으로 속성 `ResourceDictionary`합니다. `Editor` 하 고 `Button` 암시적 스타일 페이지 수준에 추가 되었습니다 `ResourceDictionary`이므로에서 사용 됩니다는 `NoteEntryPage`. XAML 스타일 지정에 대 한 자세한 내용은 참조 하세요. [스타일](deepdive.md#styling) 에 [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)합니다.

    변경 내용을 저장 **NoteEntryPage.xaml** 를 선택 하 여 **파일 > 저장** (또는 키를 눌러  **&#8984; + S**), 파일을 닫습니다.

5. 빌드 및 각 플랫폼에서 프로젝트를 실행 합니다. 자세한 내용은 [빠른 시작을 작성](single-page.md#building-the-quickstart)합니다.

    에 **NotesPage** 키를 누릅니다를 **+** 단추를를 **NoteEntryPage** 메모를 입력 합니다. 각 페이지에서 이전 빠른 시작에서 스타일 어떻게 변경 되었는지 확인 합니다.

::: zone-end


## <a name="next-steps"></a>다음 단계

이 빠른 시작에서 학습 하는 방법.

- XAML 스타일을 사용 하 여 Xamarin.Forms 응용 프로그램 스타일을 지정 합니다.

Xamarin.Forms를 사용 하 여 응용 프로그램 개발의 기본 사항에 대 한 자세한 내용은 빠른 시작 심층 분석을 계속 진행 하세요.

> [!div class="nextstepaction"]
> [다음](deepdive.md)

## <a name="related-links"></a>관련 링크

- [노트(샘플)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/Styled/)
- [Xamarin.Forms 빠른 시작에 대 한 심층 정보](deepdive.md)
