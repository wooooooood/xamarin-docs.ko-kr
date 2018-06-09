---
title: XAML 표준 (미리 보기)
description: 이 문서에서는 xamarin.forms에서는 XAML 표준 미리 보기를 탐색을 시작 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 61e0fa2587ce9a8794dbd32ff9de1f13da857342
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245796"
---
# <a name="xaml-standard-preview"></a>XAML 표준 (미리 보기)

![미리 보기](~/media/shared/preview.png)

XAML Standard Xamarin.Forms를 테스트 하려면 다음이 단계를 수행:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 다운로드는 [여기에 NuGet 패키지를 미리 볼](https://aka.ms/xf-xamlstandard-nuget)합니다.
2. 추가 **Xamarin.Forms.Alias** Xamarin.Forms.NET 표준 및 플랫폼 프로젝트에 NuGet 패키지 합니다.
3. 사용 하 여 패키지를 초기화 합니다. `Alias.Init()`
4. 추가 `xmlns:a` 참조 `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. XAML의 형식을 사용-참조는 [참조 제어](controls.md) 자세한 정보에 대 한 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 다운로드는 [여기에 NuGet 패키지를 미리 볼](https://aka.ms/xf-xamlstandard-nuget)합니다.
2. 추가 **Xamarin.Forms.Alias** Xamarin.Forms.NET 표준 및 플랫폼 프로젝트에 NuGet 패키지 합니다.
3. 사용 하 여 패키지를 초기화 합니다. `Alias.Init()`
4. 추가 `xmlns:a` 참조 `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. XAML의 형식을 사용-참조는 [참조 제어](controls.md) 자세한 정보에 대 한 합니다.

-----

다음 XAML에서는 Xamarin.Forms는에서 사용 되 고 일부 표준 XAML 컨트롤 `ContentPage`:

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
> Xmlns 요구 `a:` 표준 XAML 컨트롤에는 접두사는 현재 미리 보기의 제한 사항입니다.


## <a name="related-links"></a>관련 링크

- [미리 보기 NuGet](https://aka.ms/xf-xamlstandard-nuget)
- [컨트롤 참조](controls.md)
