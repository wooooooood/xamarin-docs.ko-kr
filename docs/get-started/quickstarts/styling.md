---
title: 플랫폼 간 Xamarin.Forms 애플리케이션 스타일 지정
description: 이 문서에서는 XAML 스타일을 사용 하 여 플랫폼 간 Xamarin Forms 응용 프로그램의 스타일을 만드는 방법을 설명 합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: CCCF8E57-D021-4542-8709-5808570FC26A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/02/2019
ms.openlocfilehash: e26a71ad72b557a27841bfee1d26001126e2a2a2
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68654659"
---
# <a name="style-a-cross-platform-xamarinforms-application"></a>플랫폼 간 Xamarin Forms 응용 프로그램 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)

이 빠른 시작에서는 다음 방법에 대해 알아봅니다.

- XAML 스타일을 사용 하 여 Xamarin.ios 응용 프로그램 스타일을 만듭니다.

퀵 스타트는 XAML 스타일을 사용 하 여 플랫폼 간 Xamarin.ios 응용 프로그램의 스타일을 만드는 방법을 안내 합니다. 최종 애플리케이션은 다음과 같습니다.

[ ![(styling-images/screenshots1-sml.png " ")]메모 페이지] (styling-images/screenshots1.png#lightbox "메모 페이지") 참고 항목 페이지(styling-images/screenshots2.png#lightbox "메모 입력 페이지") [ ![(styling-images/screenshots2-sml.png " ")]] 


### <a name="prerequisites"></a>필수 구성 요소

이 빠른 시작을 시도 하기 전에 [이전 퀵 스타트](database.md) 를 성공적으로 완료 해야 합니다. 또는 [이전 빠른 시작 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/) 을 다운로드 하 고이 빠른 시작의 시작 지점으로 사용 합니다.

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio를 사용하여 앱 업데이트

1. Visual Studio를 시작 하 고 Notes 솔루션을 엽니다.

2. **솔루션 탐색기**의 **Notes** 프로젝트에서 **app.xaml** 을 두 번 클릭 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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

    이 코드는 [`Thickness`](xref:Xamarin.Forms.Thickness) [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 및 [`Color`](xref:Xamarin.Forms.Color) [에`ContentPage`](xref:Xamarin.Forms.ContentPage)대 한 값, 일련의 값 및 암시적 스타일을 정의 합니다. 응용 프로그램 수준 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 있는 이러한 스타일은 응용 프로그램 전체에서 사용 될 수 있습니다. XAML 스타일 지정에 대 한 자세한 내용은 [xamarin.ios](deepdive.md)의 [스타일](deepdive.md#styling) 지정을 참조 하세요.

    **Ctrl + S**를 눌러 변경 내용을 **app.xaml** 에 저장 하 고 파일을 닫습니다.

3. **솔루션 탐색기**의 **Notes** 프로젝트에서 **NotesPage** 를 두 번 클릭 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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

    이 [`ListView`](xref:Xamarin.Forms.ListView) 코드는의 암시적 스타일을 페이지 수준 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 추가 하 고 `ListView.Margin` 속성을 응용 프로그램 수준 `ResourceDictionary`에 정의 된 값으로 설정 합니다. 암시적 스타일은에서 `ResourceDictionary` `NotesPage`사용 되기 때문에 페이지 수준에 추가 되었습니다. `ListView` XAML 스타일 지정에 대 한 자세한 내용은 [xamarin.ios](deepdive.md)의 [스타일](deepdive.md#styling) 지정을 참조 하세요.

    **Ctrl + S**를 눌러 변경 내용을 **NotesPage** 에 저장 하 고 파일을 닫습니다.

4. **솔루션 탐색기**의 **Notes** 프로젝트에서 **NoteEntryPage** 를 두 번 클릭 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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

    이 코드는 페이지 [`Editor`](xref:Xamarin.Forms.Editor) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Button`](xref:Xamarin.Forms.Button) 수준에`ResourceDictionary`및 뷰에 대 한 암시적 스타일을 추가 하 고 속성을응용프로그램수준에정의된값으로설정합니다.`StackLayout.Margin` 및 암시적 스타일 `ResourceDictionary`은 에서만 사용 `NoteEntryPage`되므로 페이지 수준에 추가 되었습니다. `Button` `Editor` XAML 스타일 지정에 대 한 자세한 내용은 [xamarin.ios](deepdive.md)의 [스타일](deepdive.md#styling) 지정을 참조 하세요.

    **Ctrl + S**를 눌러 변경 내용을 **NoteEntryPage** 에 저장 하 고 파일을 닫습니다.

5. 각 플랫폼에서 프로젝트를 빌드하고 실행 합니다. 자세한 내용은 [빠른 시작 빌드](single-page.md#building-the-quickstart)를 참조 하세요.

    **NotesPage** 에서 **+** 단추를 눌러 **NoteEntryPage** 로 이동 하 고 메모를 입력 합니다. 각 페이지에서 이전 퀵 스타트에서 스타일을 변경 하는 방법을 살펴봅니다.

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Mac용 Visual Studio를 사용하여 앱 업데이트

1. Mac용 Visual Studio를 시작 하 고 Notes 프로젝트를 엽니다.

2. **Solution Pad**의 **Notes** 프로젝트에서 **app.xaml** 을 두 번 클릭 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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

    이 코드는 [`Thickness`](xref:Xamarin.Forms.Thickness) [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 및 [`Color`](xref:Xamarin.Forms.Color) [에`ContentPage`](xref:Xamarin.Forms.ContentPage)대 한 값, 일련의 값 및 암시적 스타일을 정의 합니다. 응용 프로그램 수준 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 있는 이러한 스타일은 응용 프로그램 전체에서 사용 될 수 있습니다. XAML 스타일 지정에 대 한 자세한 내용은 [xamarin.ios](deepdive.md)의 [스타일](deepdive.md#styling) 지정을 참조 하세요.

    **파일 > 저장** (또는  **&#8984; + S**를 눌러)을 선택 하 여 변경 내용을 **app.xaml** 에 저장 하 고 파일을 닫습니다.

3. **Solution Pad**의 **Notes** 프로젝트에서 **NotesPage** 를 두 번 클릭 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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

    이 [`ListView`](xref:Xamarin.Forms.ListView) 코드는의 암시적 스타일을 페이지 수준 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에 추가 하 고 `ListView.Margin` 속성을 응용 프로그램 수준 `ResourceDictionary`에 정의 된 값으로 설정 합니다. 암시적 스타일은에서 `ResourceDictionary` `NotesPage`사용 되기 때문에 페이지 수준에 추가 되었습니다. `ListView` XAML 스타일 지정에 대 한 자세한 내용은 [xamarin.ios](deepdive.md)의 [스타일](deepdive.md#styling) 지정을 참조 하세요.

    **파일 > 저장** 을 선택 하거나  **&#8984; + S**를 눌러 변경 내용을 **NotesPage** 에 저장 하 고 파일을 닫습니다.

4. **Solution Pad**의 **Notes** 프로젝트에서 **NoteEntryPage** 를 두 번 클릭 하 여 엽니다. 그런 다음 기존 코드를 다음 코드로 바꿉니다.

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

    이 코드는 페이지 [`Editor`](xref:Xamarin.Forms.Editor) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Button`](xref:Xamarin.Forms.Button) 수준에`ResourceDictionary`및 뷰에 대 한 암시적 스타일을 추가 하 고 속성을응용프로그램수준에정의된값으로설정합니다.`StackLayout.Margin` 및 암시적 스타일 `ResourceDictionary`은 에서만 사용 `NoteEntryPage`되므로 페이지 수준에 추가 되었습니다. `Button` `Editor` XAML 스타일 지정에 대 한 자세한 내용은 [xamarin.ios](deepdive.md)의 [스타일](deepdive.md#styling) 지정을 참조 하세요.

    **파일 > 저장** 을 선택 하거나  **&#8984; + S**를 눌러 변경 내용을 **NoteEntryPage** 에 저장 하 고 파일을 닫습니다.

5. 각 플랫폼에서 프로젝트를 빌드하고 실행 합니다. 자세한 내용은 [빠른 시작 빌드](single-page.md#building-the-quickstart)를 참조 하세요.

    **NotesPage** 에서 **+** 단추를 눌러 **NoteEntryPage** 로 이동 하 고 메모를 입력 합니다. 각 페이지에서 이전 퀵 스타트에서 스타일을 변경 하는 방법을 살펴봅니다.

::: zone-end


## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 다음 방법에 대해 알아보았습니다.

- XAML 스타일을 사용 하 여 Xamarin.ios 응용 프로그램 스타일을 만듭니다.

Xamarin.ios를 사용 하 여 응용 프로그램을 개발 하는 기본 사항에 대해 자세히 알아보려면 빠른 시작 심층 정보를 계속 확인 하세요.

> [!div class="nextstepaction"]
> [다음](deepdive.md)

## <a name="related-links"></a>관련 링크

- [노트(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)
- [Xamarin.ios 빠른 시작 심층 살펴보기](deepdive.md)
