---
title: Xamarin.ios CollectionView EmptyView
description: CollectionView에서 표시할 수 있는 데이터가 없는 경우 사용자에 게 피드백을 제공 하는 빈 뷰를 지정할 수 있습니다. 빈 뷰는 문자열, 뷰 또는 여러 뷰가 될 수 있습니다.
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: c6a2a53f267a7f6764ec441944193e8c5ecd9189
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "68739186"
---
# <a name="xamarinforms-collectionview-emptyview"></a>Xamarin.ios CollectionView EmptyView

![](~/media/shared/preview.png "이 API는 현재 시험판임")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)표시할 데이터가 없을 때 사용자 의견을 제공 하는 데 사용할 수 있는 다음 속성을 정의 합니다.

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)[`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성이 `object` 이거나속성에서지정된`null` 컬렉션이 이거나 비어 있을 때 표시 되는 문자열, 바인딩 또는 뷰의 형식입니다. `ItemsSource` `null` 기본값은 `null`입니다.
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)지정 된의 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)형식을 지정 `EmptyView`하는 데 사용할 템플릿인 형식의입니다. 기본값은 `null`입니다.

이러한 속성은 개체에 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 의해 지원 됩니다. 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 속성을 설정 하는 주요 사용 시나리오는에 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 대 한 필터링 작업에서 데이터가 생성 되지 않을 때 사용자 의견을 표시 하 고 웹 서비스에서 데이터를 검색 하는 동안 사용자 의견을 표시 하는 것입니다.

> [!NOTE]
> 필요한 경우 대화형 콘텐츠를 포함 하는 뷰로 [속성을설정할수있습니다.`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)

데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

## <a name="display-a-string-when-data-is-unavailable"></a>데이터를 사용할 수 없을 때 문자열 표시

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 속성은 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성이 이거나`null`속성으로 지정 된 컬렉션이 이거나비어있을때표시되는문자열로설정될수있습니다.`null` `ItemsSource` 다음 XAML은이 시나리오의 예를 보여 줍니다.

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

데이터 바인딩된 컬렉션이 `null`이기 때문에 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 속성 값으로 설정 된 문자열이 표시 되기 때문에 결과는 다음과 같습니다.

[![IOS 및 Android에서 텍스트가 비어 있는 CollectionView 세로 목록의 스크린샷](emptyview-images/null-itemssource.png "텍스트가 빈 CollectionView 세로 목록 표시")](emptyview-images/null-itemssource-large.png#lightbox "텍스트가 빈 CollectionView 세로 목록 표시")

## <a name="display-views-when-data-is-unavailable"></a>데이터를 사용할 수 없을 때 보기 표시

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 속성은 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성이 이거나`null`속성으로 지정 된 컬렉션이 이거나비어있을때표시되는뷰로설정할수있습니다.`null` `ItemsSource` 단일 보기 이거나 여러 자식 뷰를 포함 하는 뷰입니다. 다음 XAML 예제에서는 여러 자식 `EmptyView` 뷰를 포함 하는 뷰로 설정 된 속성을 보여 줍니다.

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

해당하는 C# 코드는 다음과 같습니다.

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

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 가를 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 실행 하면에서 표시 하는 컬렉션이 [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) 속성에 저장 된 검색 용어에 대해 필터링 됩니다. `FilterCommand` 필터링 작업에서 데이터를 생성 하지 않으면 [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 속성 값으로 설정 된이 표시 됩니다.

[![IOS 및 Android에서 사용자 지정 빈 보기가 있는 CollectionView 세로 목록의 스크린샷](emptyview-images/filter-multiple-views.png "사용자 지정 빈 뷰가 있는 CollectionView 세로 목록")](emptyview-images/filter-multiple-views-large.png#lightbox "사용자 지정 빈 뷰가 있는 CollectionView 세로 목록")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>데이터를 사용할 수 없을 때 템플릿 사용자 지정 형식 표시

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 속성은 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성이 `null`이거나 속성`ItemsSource` 으로 지정 된 컬렉션이 이거나 비어 있는 경우 템플릿이 표시 되는 사용자 지정 형식으로 설정할 수 있습니다. `null` 속성 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 은의 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 모양을`EmptyView`정의 하는로 설정할 수 있습니다. 다음 XAML은이 시나리오의 예를 보여 줍니다.

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

해당하는 C# 코드는 다음과 같습니다.

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

형식은 속성을 정의 하 고 해당 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)하는를 정의 합니다. `Filter` `FilterData`

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

속성이 개체로 설정 `Filter` 되 고 속성 데이터가 [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) 속성에 바인딩됩니다. `FilterData` [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`CollectionView`](xref:Xamarin.Forms.CollectionView) 가를 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 실행 하면에서 표시 하는 컬렉션이 `Filter` 속성에 저장 된 검색 용어에 대해 필터링 됩니다. `FilterCommand` 필터링 작업에서 데이터를 생성 하지 않으면 [`Label`](xref:Xamarin.Forms.Label) [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 속성 값으로 설정 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)되는에 정의 된이 표시 됩니다.

[![IOS 및 Android에서 빈 보기 템플릿을 포함 하는 CollectionView 세로 목록의 스크린샷](emptyview-images/emptyviewtemplate.png "빈 뷰 템플릿이 있는 CollectionView 세로 목록")](emptyview-images/emptyviewtemplate-large.png#lightbox "빈 뷰 템플릿이 있는 CollectionView 세로 목록")

> [!NOTE]
> 데이터를 사용할 수 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 없을 때 템플릿 사용자 지정 형식을 표시 하는 경우 여러 자식 뷰가 포함 된 뷰로 속성을 설정할 수 있습니다.

## <a name="choose-an-emptyview-at-runtime"></a>런타임에 EmptyView 선택

데이터를 사용할 수 없을 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 때로 표시 되는 뷰는의 [`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)개체로 정의 될 수 있습니다. 그런 다음 런타임에 특정 비즈니스 논리에 따라 `ContentView` 속성을특정으로설정할수있습니다.`EmptyView` 다음 XAML 예제에서는이 시나리오의 예를 보여 줍니다.

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

이 XAML은 페이지 [`ContentView`](xref:Xamarin.Forms.ContentView) 수준 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에서 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 속성 값으로 설정 될 `ContentView` 개체를 [`Switch`](xref:Xamarin.Forms.Switch) 제어 하는 개체를 사용 하 여 두 개체를 정의 합니다. 가 전환 되 면 이벤트 처리기가 `ToggleEmptyView` 메서드를 실행 합니다. `OnEmptyViewSwitchToggled` [`Switch`](xref:Xamarin.Forms.Switch)

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

메서드 `ToggleEmptyView` [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) 는 속성 값 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 에 따라 `collectionView` 개체의 속성을에 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)저장 된 [`ContentView`](xref:Xamarin.Forms.ContentView) 두 개체 중 하나로 설정 합니다. [`CollectionView`](xref:Xamarin.Forms.CollectionView) 가를 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 실행 하면에서 표시 하는 컬렉션이 [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) 속성에 저장 된 검색 용어에 대해 필터링 됩니다. `FilterCommand` 필터링 작업에서 데이터가 `ContentView` 생성 되지 않으면 `EmptyView` 속성으로 설정 된 개체가 표시 됩니다.

[![IOS 및 Android에서 교체 된 빈 보기가 있는 CollectionView 세로 목록의 스크린샷](emptyview-images/swap.png "비어 있는 뷰가 있는 CollectionView 세로 목록")](emptyview-images/swap-large.png#lightbox "비어 있는 뷰가 있는 CollectionView 세로 목록")

리소스 사전에 대 한 자세한 내용은 [Xamarin.ios 리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)을 참조 하세요.

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>런타임에 EmptyViewTemplate 선택

의 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 모양은 [`CollectionView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 속성 [을개체로설정하여런타임에해당값을기준으로선택할수있습니다.`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
    <ContentPage.Resources>
        <DataTemplate x:Key="AdvancedTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="BasicTemplate">
            ...
        </DataTemplate>

        <controls:SearchTermDataTemplateSelector x:Key="SearchSelector"
                                                 DefaultTemplate="{StaticResource AdvancedTemplate}"
                                                 OtherTemplate="{StaticResource BasicTemplate}" />
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <CollectionView ItemsSource="{Binding Monkeys}"
                        EmptyView="{Binding Source={x:Reference searchBar}, Path=Text}"
                        EmptyViewTemplate="{StaticResource SearchSelector}" />
    </StackLayout>
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

속성은 속성 [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) 으로 설정 되 고 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 속성은 `SearchTermDataTemplateSelector` 개체로 설정 됩니다. [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 가를 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 실행 하면에서 표시 하는 컬렉션이 [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text) 속성에 저장 된 검색 용어에 대해 필터링 됩니다. `FilterCommand` 필터링 작업에서 데이터를 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 생성 하지 않으면 `SearchTermDataTemplateSelector` [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 개체에서 선택한가 속성으로 설정 되 고 표시 됩니다.

다음 예제에서는 클래스를 `SearchTermDataTemplateSelector` 보여 줍니다.

```csharp
public class SearchTermDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate OtherTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        string query = (string)item;
        return query.ToLower().Equals("xamarin") ? OtherTemplate : DefaultTemplate;
    }
}
```

클래스 `SearchTermTemplateSelector` 는 다른 `DefaultTemplate` 데이터 `OtherTemplate` 템플릿으로 설정 된 및 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 속성을 정의 합니다. 재정의 `OnSelectTemplate` 는 검색 `DefaultTemplate`쿼리가 "xamarin"과 같지 않은 경우 사용자에 게 메시지를 표시 하는을 반환 합니다. 검색 쿼리가 "xamarin"과 같을 때 재정의는 사용자에 `OnSelectTemplate` 게 기본 `OtherTemplate`메시지를 표시 하는를 반환 합니다.

[![IOS 및 Android에서 CollectionView runtime 빈 뷰 템플릿 선택의 스크린샷](emptyview-images/datatemplateselector.png "CollectionView의 런타임 빈 뷰 템플릿 선택")](emptyview-images/datatemplateselector-large.png#lightbox "CollectionView의 런타임 빈 뷰 템플릿 선택")

데이터 템플릿 선택기에 대 한 자세한 내용은 [DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin Forms 리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Xamarin. Forms DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
