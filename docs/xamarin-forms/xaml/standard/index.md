---
title: XAML 표준 (미리 보기)
description: 이 문서에서는 xamarin.forms에서 XAML 표준 미리 보기를 탐색 시작 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 4e65f31d76d9540bed6110198d7cafaab9fe78f5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114901"
---
# <a name="xaml-standard-preview"></a>XAML 표준 (미리 보기)

![미리 보기](~/media/shared/preview.png)

Xamarin.forms에서 XAML 표준을 사용 하 여 실험 하려면 다음이 단계를 수행 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 다운로드 합니다 [여기에 NuGet 패키지를 미리 보려면](https://aka.ms/xf-xamlstandard-nuget)합니다.
2. 추가 된 **Xamarin.Forms.Alias** Xamarin.Forms.NET Standard 및 플랫폼 프로젝트에 NuGet 패키지.
3. 사용 하 여 패키지를 초기화 합니다. `Alias.Init()`
4. 추가 `xmlns:a` 참조 `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. XAML의 형식을 사용할-참조를 [제어 참조](controls.md) 자세한 내용은 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 다운로드 합니다 [여기에 NuGet 패키지를 미리 보려면](https://aka.ms/xf-xamlstandard-nuget)합니다.
2. 추가 된 **Xamarin.Forms.Alias** Xamarin.Forms.NET Standard 및 플랫폼 프로젝트에 NuGet 패키지.
3. 사용 하 여 패키지를 초기화 합니다. `Alias.Init()`
4. 추가 `xmlns:a` 참조 `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. XAML의 형식을 사용할-참조를 [제어 참조](controls.md) 자세한 내용은 합니다.

-----

다음 XAML을 Xamarin.Forms에서 사용 되 고 일부 XAML 표준 컨트롤을 보여 줍니다. `ContentPage`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage 
    xmlns="http://xamarin.com/schemas/2014/forms" 
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
    xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"
    x:Class="XAMLStandardSample.ItemsPage" 
    Title="{Binding Title}" x:Name="BrowseItemsPage">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Add" Clicked="AddItem_Clicked" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <a:StackPanel>
            <ListView x:Name="ItemsListView" ItemsSource="{Binding Items}" VerticalOptions="FillAndExpand" HasUnevenRows="true" RefreshCommand="{Binding LoadItemsCommand}" IsPullToRefreshEnabled="true" IsRefreshing="{Binding IsBusy, Mode=OneWay}" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <ViewCell>
                            <StackLayout Padding="10">
                                <a:TextBlock Text="{Binding Text}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemTextStyle}" FontSize="16" />
                                <a:TextBlock Text="{Binding Description}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemDetailTextStyle}" FontSize="13" />
                            </StackLayout>
                        </ViewCell>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </a:StackPanel>
    </ContentPage.Content>
</ContentPage>
```

> [!NOTE]
> Xmlns 요구 `a:` XAML 표준 컨트롤의 접두사는 현재 미리 보기의 제한 사항입니다.


## <a name="related-links"></a>관련 링크

- [NuGet 미리 보기](https://aka.ms/xf-xamlstandard-nuget)
- [컨트롤 참조](controls.md)
