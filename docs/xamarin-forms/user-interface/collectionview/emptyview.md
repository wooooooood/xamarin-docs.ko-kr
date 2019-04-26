---
title: 데이터를 사용할 수 없는 경우는 EmptyView 표시
description: CollectionView, 빈 뷰를 표시할 수 있는 데이터가 없는 경우 사용자에 게 피드백을 제공 하는 지정할 수 있습니다. 빈 뷰는 문자열로, 뷰 또는 여러 뷰 수 있습니다.
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/19/2019
ms.openlocfilehash: a430387bba83887045e5687c99d9295d4be373e4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61019468"
---
# <a name="display-an-emptyview-when-data-is-unavailable"></a>데이터를 사용할 수 없는 경우는 EmptyView 표시

![미리 보기](~/media/shared/preview.png)

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView` 는 현재 미리 보기, 및 일부 계획된 기능 부족 합니다. 또한 API 구현을 완료 되 면 변경할 수 있습니다.

`CollectionView` 표시할 데이터가 없는 경우 사용자에 게 피드백을 제공 하는 다음 속성을 정의 합니다.

- `EmptyView`형식의 `object`, 문자열, 바인딩 또는 보기를 선택할 때 표시 된를 `ItemsSource` 속성은 `null`, 컬렉션에서 지정 하는 경우 또는 `ItemsSource` 속성이 `null` 또는 빈 합니다. 기본값은 `null`입니다.
- `EmptyViewTemplate`형식의 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)에 지정 된 형식을 지정 하는 데 템플릿을 `EmptyView`합니다. 기본값은 `null`입니다.

이러한 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체 속성을 데이터 바인딩의 대상 수 있음을 의미 합니다.

설정에 대 한 주요 사용 시나리오는 `EmptyView` 속성에서 작업을 필터링 할 때 사용자 의견 표시는 `CollectionView` 웹 서비스에서 데이터를 검색 하는 동안 없는 데이터 및 표시 사용자 피드백을 생성 합니다.

> [!NOTE]
> `EmptyView` 필요한 경우 대화형 콘텐츠를 포함 하는 보기 속성을 설정할 수 있습니다.

데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

## <a name="display-a-string-when-data-is-unavailable"></a>데이터를 사용할 수 없는 경우 문자열을 표시 합니다.

`EmptyView` 될 문자열로 속성을 설정할 수 있습니다 때 표시 되는 `ItemsSource` 속성은 `null`, 컬렉션에서 지정 하는 경우 또는 `ItemsSource` 속성이 `null` 이거나 비어 합니다. 다음 XAML에서는이 시나리오의 예를 보여 줍니다.

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

해당 하는 C# 코드가입니다.

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

결과, 데이터 바인딩 컬렉션 이므로 `null`, 문자열으로는 `EmptyView` 속성 값이 표시 됩니다.

[![IOS 및 Android에서 빈 텍스트 뷰를 사용 하 여 CollectionView 세로 목록 스크린샷](emptyview-images/null-itemssource.png "CollectionView 텍스트 빈 뷰를 사용 하 여 세로 목록")](emptyview-images/null-itemssource-large.png#lightbox "빈 텍스트로 CollectionView 세로 목록 보기")

## <a name="display-views-when-data-is-unavailable"></a>데이터를 사용할 수 없는 경우 보기를 표시 합니다.

`EmptyView` 될 보기로 속성을 설정할 수 있습니다 때 표시 되는 `ItemsSource` 속성은 `null`, 컬렉션에서 지정 하는 경우 또는 `ItemsSource` 속성은 `null` 이거나 비어 합니다. 단일 뷰 또는 여러 자식 뷰를 포함 하는 뷰를 수 있습니다. 다음 XAML 예제에서는 `EmptyView` 속성이 여러 자식 뷰를 포함 하는 뷰입니다로 설정 합니다.

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </CollectionView.EmptyView>
    </CollectionView>
</StackLayout>
```

해당 하는 C# 코드가입니다.

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new StackLayout
    {
        Children =
        {
            new Label { Text = "No results matched your filter.", ... },
            new Label { Text = "Try a broader filter?", ... }
        }
    }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

경우는 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 실행를 `FilterCommand`, 컬렉션으로 표시를 `CollectionView` 에 저장 된 검색 용어에 대 한 필터링 된를 [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) 속성입니다. 필터링 작업에 없는 데이터를 생성 하는 경우는 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 로 `EmptyView` 속성 값이 표시 됩니다.

[![IOS 및 Android에서 사용자 지정 빈 뷰를 사용 하 여 CollectionView 세로 목록 스크린샷](emptyview-images/filter-multiple-views.png "빈 사용자 지정 보기를 사용 하 여 세로 목록 CollectionView")](emptyview-images/filter-multiple-views-large.png#lightbox "CollectionView 세로 목록 사용자 지정 빈 뷰")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>데이터를 사용할 수 없을 때 템플릿 기반 사용자 지정 형식 표시

`EmptyView` 해당 템플릿은 사용자 지정 형식에 속성을 설정할 수 있습니다 때 표시 되는 `ItemsSource` 속성은 `null`, 컬렉션에서 지정 하는 경우 또는 `ItemsSource` 속성이 `null` 이거나 비어 합니다. 합니다 `EmptyViewTemplate` 속성 설정할 수 있습니다를 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 의 모양을 정의 하는 `EmptyView`합니다. 다음 XAML에서는이 시나리오의 예를 보여 줍니다.

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <views:FilterData Filter="{Binding Source={x:Reference searchBar}, Path=Text}" />
        </CollectionView.EmptyView>
        <CollectionView.EmptyViewTemplate>
            <DataTemplate>
                <Label Text="{Binding Filter, StringFormat='Your filter term of {0} did not match any records.'}"
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </DataTemplate>
        </CollectionView.EmptyViewTemplate>
    </CollectionView>
</StackLayout>
```

해당 하는 C# 코드가입니다.

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new FilterData { Filter = searchBar.Text },
    EmptyViewTemplate = new DataTemplate(() =>
    {
        return new Label { ... };
    })
};
```

합니다 `FilterData` 형식 정의 `Filter` 속성 및 해당 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty):

```csharp
public class FilterData : BindableObject
{
    public static readonly BindableProperty FilterProperty = BindableProperty.Create(nameof(Filter), typeof(string), typeof(FilterData), null);

    public string Filter
    {
        get { return (string)GetValue(FilterProperty); }
        set { SetValue(FilterProperty, value); }
    }
}
```

`EmptyView` 속성을 `FilterData` 개체 및 `Filter` 속성 데이터를 바인딩합니다 합니다 [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) 속성. 경우는 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 실행를 `FilterCommand`, 컬렉션으로 표시를 `CollectionView` 에 저장 된 검색 용어에 대 한 필터링 된는 `Filter` 속성입니다. 필터링 작업에 없는 데이터를 생성 하는 경우는 [ `Label` ](xref:Xamarin.Forms.Label) 에 정의 된 합니다 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)로 설정 되어 있는 `EmptyViewTemplate` 속성 값이 표시 됩니다:

[![IOS 및 Android에서 빈 뷰 템플릿으로 CollectionView 세로 목록 스크린샷](emptyview-images/emptyviewtemplate.png "빈 뷰 템플릿 사용 하 여 세로 목록 CollectionView")](emptyview-images/emptyviewtemplate-large.png#lightbox "CollectionView 세로 목록 빈 뷰 템플릿")

> [!NOTE]
> 데이터를 사용할 수 없을 때 템플릿 기반 사용자 지정 형식을 표시할 때의 `EmptyViewTemplate` 여러 자식 뷰를 포함 하는 보기 속성을 설정할 수 있습니다.

## <a name="choose-an-emptyview-at-runtime"></a>런타임에 EmptyView 선택

으로 표시 되는 뷰를 `EmptyView` 데이터를 사용할 수 없는 경우으로 정의할 수 있습니다 [ `ContentView` ](xref:Xamarin.Forms.ContentView) 개체를 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)합니다. 합니다 `EmptyView` 한 다음 속성을 특정 `ContentView`런타임 시 일부 비즈니스 논리에 따라 합니다. 다음 XAML 예제에서는이 시나리오의 예를 보여 줍니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.EmptyViewSwapPage"
             Title="EmptyView (swap)">
    <ContentPage.Resources>
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No items to display."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Toggle EmptyViews" />
            <Switch Toggled="OnEmptyViewSwitchToggled" />
        </StackLayout>
        <CollectionView x:Name="collectionView"
                        ItemsSource="{Binding Monkeys}">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    ...
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

이 XAML 정의 두 [ `ContentView` ](xref:Xamarin.Forms.ContentView) 페이지 수준에서 개체 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)를 사용 하 여는 [ `Switch` ](xref:Xamarin.Forms.Switch) 개체는 제어 `ContentView` 으로 설정 될 개체를 `EmptyView` 속성 값입니다. 경우는 [ `Switch` ](xref:Xamarin.Forms.Switch) 설정/해제는 `OnEmptyViewSwitchToggled` 이벤트 처리기가 실행 되는 `ToggleEmptyView` 메서드:

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

`ToggleEmptyView` 메서드 집합을 `EmptyView` 의 속성을 `collectionView` 개체 중 하나를 [ `ContentView` ](xref:Xamarin.Forms.ContentView) 에 저장 된 개체를 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)값에 따라는 [ `Switch.IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled) 속성입니다. 경우는 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 실행를 `FilterCommand`, 컬렉션으로 표시를 `CollectionView` 에 저장 된 검색 용어에 대 한 필터링 된를 [ `SearchBar.Text` ](xref:Xamarin.Forms.SearchBar.Text) 속성입니다. 필터링 작업에 없는 데이터를 생성 하는 경우는 `ContentView` 개체로는 `EmptyView` 속성이 표시 됩니다.

[![IOS 및 Android에서 교체 된 빈 뷰를 사용 하 여 CollectionView 세로 목록 스크린샷](emptyview-images/swap.png "바뀌었으면된 빈 뷰를 사용 하 여 세로 목록 CollectionView")](emptyview-images/swap-large.png#lightbox "CollectionView 세로 목록 빈 뷰를 교환합니다.")

리소스 사전에 대 한 자세한 내용은 참조 하세요. [Xamarin.Forms 리소스가](~/xamarin-forms/xaml/resource-dictionaries.md)합니다.

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms 리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
