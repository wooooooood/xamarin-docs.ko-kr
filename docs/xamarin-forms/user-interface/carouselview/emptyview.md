---
title: Xamarin.FormsCarouselView EmptyView
description: CarouselView에서 표시할 수 있는 데이터가 없는 경우 사용자에 게 피드백을 제공 하는 빈 뷰를 지정할 수 있습니다. 빈 뷰는 문자열, 뷰 또는 여러 뷰가 될 수 있습니다.
ms.prod: xamarin
ms.assetid: C6DEE1A9-63FC-4889-BC77-F401D5D7DF32
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/03/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a9f952da75e68e9ad39e0a15f57fbd0379233d7e
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137399"
---
# <a name="xamarinforms-carouselview-emptyview"></a>Xamarin.FormsCarouselView EmptyView

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)표시할 데이터가 없을 때 사용자 의견을 제공 하는 데 사용할 수 있는 다음 속성을 정의 합니다.

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)`object` [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성이 `null` 이거나 속성에서 지정 된 컬렉션이 `ItemsSource` `null` 이거나 비어 있을 때 표시 되는 문자열, 바인딩 또는 뷰의 형식입니다. 기본값은 `null`입니다.
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)지정 된의 형식을 지정 하는 데 사용할 템플릿인 형식의입니다 `EmptyView` . 기본값은 `null`입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 속성은 데이터 바인딩의 대상이 될 수 있습니다.

속성을 설정 하는 주요 사용 시나리오는에 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 대 한 필터링 작업에서 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 데이터가 생성 되지 않을 때 사용자 의견을 표시 하 고 웹 서비스에서 데이터를 검색 하는 동안 사용자 의견을 표시 하는 것입니다.

> [!NOTE]
> [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)필요한 경우 대화형 콘텐츠를 포함 하는 뷰로 속성을 설정할 수 있습니다.

데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

## <a name="display-a-string-when-data-is-unavailable"></a>데이터를 사용할 수 없을 때 문자열 표시

속성은 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성이 이거나 `null` 속성으로 지정 된 컬렉션이 `ItemsSource` `null` 이거나 비어 있을 때 표시 되는 문자열로 설정 될 수 있습니다. 다음 XAML은이 시나리오의 예를 보여 줍니다.

```xaml
<CarouselView ItemsSource="{Binding EmptyMonkeys}"
              EmptyView="No items to display." />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
CarouselView carouselView = new CarouselView
{
    EmptyView = "No items to display."
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

그 결과, 데이터 바인딩된 컬렉션이 이기 때문에 `null` 속성 값으로 설정 된 문자열이 표시 되기 때문입니다 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) .

## <a name="display-views-when-data-is-unavailable"></a>데이터를 사용할 수 없을 때 보기 표시

속성은 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성이 이거나 `null` 속성으로 지정 된 컬렉션이 `ItemsSource` `null` 이거나 비어 있을 때 표시 되는 뷰로 설정할 수 있습니다. 단일 보기 이거나 여러 자식 뷰를 포함 하는 뷰입니다. 다음 XAML 예제에서는 `EmptyView` 여러 자식 뷰를 포함 하는 뷰로 설정 된 속성을 보여 줍니다.

```xaml
<StackLayout Margin="20">
    <SearchBar SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
               Placeholder="Filter" />
    <CarouselView ItemsSource="{Binding Monkeys}">
        <CarouselView.EmptyView>
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
        </CarouselView.EmptyView>
        <CarouselView.ItemTemplate>
            ...
        </CarouselView.ItemTemplate>
    </CarouselView>
</StackLayout>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
SearchBar searchBar = new SearchBar { ... };
CarouselView carouselView = new CarouselView
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
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

가를 실행 하면에서 표시 하는 [`SearchBar`](xref:Xamarin.Forms.SearchBar) `FilterCommand` 컬렉션이 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 속성에 저장 된 검색 용어에 대해 필터링 됩니다 [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) . 필터링 작업에서 데이터를 생성 하지 않으면 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 속성 값으로 설정 된 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 이 표시 됩니다.

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>데이터를 사용할 수 없을 때 템플릿 사용자 지정 형식 표시

속성은 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 속성이 `null` 이거나 속성으로 지정 된 컬렉션이 `ItemsSource` `null` 이거나 비어 있는 경우 템플릿이 표시 되는 사용자 지정 형식으로 설정할 수 있습니다. [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)속성은 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 의 모양을 정의 하는로 설정할 수 있습니다 `EmptyView` . 다음 XAML은이 시나리오의 예를 보여 줍니다.

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
               Placeholder="Filter" />
    <CarouselView ItemsSource="{Binding Monkeys}">
        <CarouselView.EmptyView>
            <controls:FilterData Filter="{Binding Source={x:Reference searchBar}, Path=Text}" />
        </CarouselView.EmptyView>
        <CarouselView.EmptyViewTemplate>
            <DataTemplate>
                <Label Text="{Binding Filter, StringFormat='Your filter term of {0} did not match any records.'}"
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </DataTemplate>
        </CarouselView.EmptyViewTemplate>
        <CarouselView.ItemTemplate>
            ...
        </CarouselView.ItemTemplate>
    </CarouselView>
</StackLayout>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
SearchBar searchBar = new SearchBar { ... };
CarouselView carouselView = new CarouselView
{
    EmptyView = new FilterData { Filter = searchBar.Text },
    EmptyViewTemplate = new DataTemplate(() =>
    {
        return new Label { ... };
    })
};
```

`FilterData`형식은 속성을 정의 `Filter` 하 고 해당 하는를 정의 합니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) .

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

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)속성이 개체로 설정 되 고 속성 `FilterData` `Filter` 데이터가 속성에 바인딩됩니다 [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) . 가를 실행 하면에서 표시 하는 [`SearchBar`](xref:Xamarin.Forms.SearchBar) `FilterCommand` 컬렉션이 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 속성에 저장 된 검색 용어에 대해 필터링 됩니다 `Filter` . 필터링 작업에서 데이터를 생성 하지 않으면 [`Label`](xref:Xamarin.Forms.Label) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 속성 값으로 설정 된에 정의 된 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 이 표시 됩니다.

> [!NOTE]
> 데이터를 사용할 수 없을 때 템플릿 사용자 지정 형식을 표시 하는 경우 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 여러 자식 뷰가 포함 된 뷰로 속성을 설정할 수 있습니다.

## <a name="choose-an-emptyview-at-runtime"></a>런타임에 EmptyView 선택

데이터를 사용할 수 없을 때로 표시 되는 뷰는 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 의 개체로 정의 될 수 있습니다 [`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . `EmptyView`그런 다음 `ContentView` 런타임에 특정 비즈니스 논리에 따라 속성을 특정으로 설정할 수 있습니다. 다음 XAML 예제에서는이 시나리오의 예를 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:viewmodels="clr-namespace:CarouselViewDemos.ViewModels"
             x:Class="CarouselViewDemos.Views.EmptyViewSwapPage"
             Title="EmptyView (swap)">
    <ContentPage.BindingContext>
        <viewmodels:MonkeysViewModel />
    </ContentPage.BindingContext>
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
        <SearchBar SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
                   Placeholder="Filter" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Toggle EmptyViews" />
            <Switch Toggled="OnEmptyViewSwitchToggled" />
        </StackLayout>
        <CarouselView x:Name="carouselView"
                      ItemsSource="{Binding Monkeys}">
            <CarouselView.ItemTemplate>
                ...
            </CarouselView.ItemTemplate>
        </CarouselView>
    </StackLayout>
</ContentPage>
```

이 XAML은 [`ContentView`](xref:Xamarin.Forms.ContentView) 페이지 수준에서 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Switch`](xref:Xamarin.Forms.Switch) `ContentView` 속성 값으로 설정 될 개체를 제어 하는 개체를 사용 하 여 두 개체를 정의 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 합니다. [`Switch`](xref:Xamarin.Forms.Switch)가 전환 되 면 `OnEmptyViewSwitchToggled` 이벤트 처리기가 메서드를 실행 합니다 `ToggleEmptyView` .

```csharp
void ToggleEmptyView(bool isToggled)
{
    carouselView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

`ToggleEmptyView`메서드는 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) `carouselView` [`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 속성 값에 따라 개체의 속성을에 저장 된 두 개체 중 하나로 [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) 설정 합니다. 가를 실행 하면에서 표시 하는 [`SearchBar`](xref:Xamarin.Forms.SearchBar) `FilterCommand` 컬렉션이 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 속성에 저장 된 검색 용어에 대해 필터링 됩니다 [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) . 필터링 작업에서 데이터가 생성 되지 않으면 `ContentView` 속성으로 설정 된 개체가 `EmptyView` 표시 됩니다.

리소스 사전에 대 한 자세한 내용은 [ Xamarin.Forms 리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)을 참조 하세요.

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>런타임에 EmptyViewTemplate 선택

의 모양은 속성을 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 개체로 설정 하 여 런타임에 해당 값을 기준으로 선택할 수 있습니다 [`CarouselView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) .

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CarouselViewDemos.Controls">
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
                   SearchCommandParameter="{Binding Source={RelativeSource Self}, Path=Text}"
                   Placeholder="Filter" />
        <CarouselView ItemsSource="{Binding Monkeys}"
                      EmptyView="{Binding Source={x:Reference searchBar}, Path=Text}"
                      EmptyViewTemplate="{StaticResource SearchSelector}">
            <CarouselView.ItemTemplate>
                ...
            </CarouselView.ItemTemplate>
        </CarouselView>
    </StackLayout>
</ContentPage>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
SearchBar searchBar = new SearchBar { ... };
CarouselView carouselView = new CarouselView()
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

속성은 속성 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 으로 설정 되 [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) 고 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 속성은 개체로 설정 됩니다 `SearchTermDataTemplateSelector` .

가를 실행 하면에서 표시 하는 [`SearchBar`](xref:Xamarin.Forms.SearchBar) `FilterCommand` 컬렉션이 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 속성에 저장 된 검색 용어에 대해 필터링 됩니다 [`SearchBar.Text`](xref:Xamarin.Forms.InputView.Text) . 필터링 작업에서 데이터를 생성 하지 않으면 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 개체에서 선택한가 `SearchTermDataTemplateSelector` 속성으로 설정 되 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 고 표시 됩니다.

다음 예제에서는 클래스를 보여 줍니다 `SearchTermDataTemplateSelector` .

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

`SearchTermTemplateSelector`클래스는 `DefaultTemplate` `OtherTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 다른 데이터 템플릿으로 설정 된 및 속성을 정의 합니다. `OnSelectTemplate`재정의는 `DefaultTemplate` 검색 쿼리가 "xamarin"과 같지 않은 경우 사용자에 게 메시지를 표시 하는을 반환 합니다. 검색 쿼리가 "xamarin"과 같을 때 `OnSelectTemplate` 재정의는 `OtherTemplate` 사용자에 게 기본 메시지를 표시 하는를 반환 합니다.

데이터 템플릿 선택기에 대 한 자세한 내용은 [Create a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [CarouselView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Xamarin.Forms데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms 리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [DataTemplateSelector 만들기 Xamarin.Forms](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
