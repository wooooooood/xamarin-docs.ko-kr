---
title: Xamarin.ios의 바인딩 가능한 레이아웃
description: 바인딩 가능한 레이아웃을 사용 하면 레이아웃 클래스가 항목 컬렉션에 바인딩하여 콘텐츠를 생성할 수 있으며,이 옵션을 사용 하 여 각 항목의 모양을 DataTemplate로 설정할 수 있습니다.
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/09/2020
ms.openlocfilehash: d084d910786c24a4b0333ecbc3e893cfe144d404
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517178"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Xamarin.ios의 바인딩 가능한 레이아웃

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

바인딩 가능한 레이아웃을 사용 하면 [`Layout<T>`](xref:Xamarin.Forms.Layout`1) 클래스에서 파생 되는 모든 레이아웃 클래스가 항목 컬렉션에 바인딩하여 항목의 모양을로 설정 하는 옵션을 사용 하 여 콘텐츠를 생성할 수 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)있습니다. 바인딩 가능한 레이아웃은 다음 연결 `BindableLayout` 된 속성을 노출 하는 클래스에서 제공 됩니다.

- `ItemsSource`– 레이아웃에 표시 되 `IEnumerable` 는 항목의 컬렉션을 지정 합니다.
- `ItemTemplate`– 레이아웃에 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 의해 표시 되는 항목 컬렉션의 각 항목에 적용할를 지정 합니다.
- `ItemTemplateSelector`– 런타임에 항목 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 에 대해를 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 선택 하는 데 사용 되는를 지정 합니다.

> [!NOTE]
> 및 속성이 모두 설정 된 경우 속성이 우선적으로 `ItemTemplate` 적용 됩니다. `ItemTemplateSelector` `ItemTemplate`

또한 클래스는 `BindableLayout` 다음과 같은 바인딩 가능한 속성을 노출 합니다.

- `EmptyView``string` – `ItemsSource` 속성이 `null`이거나 `ItemsSource` 속성으로 지정 된 컬렉션이 `null` 이거나 비어 있을 때 표시 되는 또는 뷰를 지정 합니다. 기본값은 `null`입니다.
- `EmptyViewTemplate`[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) – `ItemsSource` 속성이 `null`이거나 `ItemsSource` 속성으로 지정 된 컬렉션이 `null` 이거나 비어 있을 때 표시 되는를 지정 합니다. 기본값은 `null`입니다.

> [!NOTE]
> 및 속성이 모두 설정 된 경우 속성이 우선적으로 `EmptyViewTemplate` 적용 됩니다. `EmptyViewTemplate` `EmptyView`

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)이러한 속성은 모두 [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) [`Grid`](xref:Xamarin.Forms.Grid) [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) [`StackLayout`](xref:Xamarin.Forms.StackLayout) 클래스에서 파생 되는,,, 및 클래스에 연결할 수 있습니다. [`Layout<T>`](xref:Xamarin.Forms.Layout`1)

클래스 `Layout<T>` 는 레이아웃의 [`Children`](xref:Xamarin.Forms.Layout`1.Children) 자식 요소가 추가 되는 컬렉션을 노출 합니다. `BinableLayout.ItemsSource` 속성이 항목의 컬렉션으로 설정 되 고 파생 클래스에 [`Layout<T>`](xref:Xamarin.Forms.Layout`1)연결 되 면 컬렉션의 각 항목이 레이아웃에 의해 표시 되기 위해 `Layout<T>.Children` 컬렉션에 추가 됩니다. 그러면 `Layout<T>`파생 클래스는 기본 컬렉션이 변경 될 때 자식 뷰를 업데이트 합니다. Xamarin.ios 레이아웃 주기에 대 한 자세한 내용은 [사용자 지정 레이아웃 만들기](~/xamarin-forms/user-interface/layouts/custom.md)를 참조 하세요.

바인딩 가능한 레이아웃은 표시할 항목 컬렉션이 작고 스크롤 및 선택이 필요 하지 않은 경우에만 사용 해야 합니다. 에서 바인딩할 수 있는 레이아웃을 래핑하여 스크롤 [`ScrollView`](xref:Xamarin.Forms.ScrollView)을 제공할 수 있지만,이는 바인딩 가능한 레이아웃에 UI 가상화가 없어도 권장 되지 않습니다. 스크롤이 필요한 경우 또는 [`ListView`](xref:Xamarin.Forms.ListView) [`CollectionView`](xref:Xamarin.Forms.CollectionView)와 같은 UI 가상화를 포함 하는 스크롤 가능한 뷰를 사용 해야 합니다. 이 권장 사항을 관찰 하지 못하면 성능 문제가 발생할 수 있습니다.

> [!IMPORTANT]
> 기술적 [`Layout<T>`](xref:Xamarin.Forms.Layout`1) 으로는 클래스에서 파생 되는 레이아웃 클래스에 바인딩할 수 있는 레이아웃을 연결할 수 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)있지만, [`Grid`](xref:Xamarin.Forms.Grid), 및 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 클래스의 경우 특히 유용 하지 않습니다. 예를 들어, 바인딩 가능한 레이아웃을 [`Grid`](xref:Xamarin.Forms.Grid) 사용 하 여에 데이터 컬렉션을 표시 하려는 경우를 예로 들어 보겠습니다. 컬렉션의 각 항목은 여러 속성을 포함 하는 개체입니다. 의 각 행은 `Grid` 개체의 속성 중 하나를 표시 하는 `Grid` 의 각 열을 포함 하 여 컬렉션의 개체를 표시 합니다. 바인딩 가능한 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 레이아웃의는 단일 개체만 포함할 수 있으므로 해당 개체는 각각 특정 `Grid` 열에서 개체의 속성 중 하나를 표시 하는 여러 뷰를 포함 하는 레이아웃 클래스 여야 합니다. 이 시나리오는 바인딩 가능한 레이아웃을 사용 하 여 realised 수 있지만,이 `Grid` 로 인해 부모 `Grid` 는 바인딩 컬렉션의 각 항목에 대 한 자식을 포함 하 고 있으며이는 `Grid` 레이아웃의 매우 비효율적 이며 문제가 있는 사용입니다.

## <a name="populate-a-bindable-layout-with-data"></a>데이터를 사용 하 여 바인딩 가능한 레이아웃 채우기

바인딩 가능한 레이아웃은 해당 `ItemsSource` 속성을를 구현 `IEnumerable`하는 컬렉션으로 설정 하 고 파생 클래스에 [`Layout<T>`](xref:Xamarin.Forms.Layout`1)연결 하 여 데이터로 채워집니다.

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

`BindableLayout.ItemsSource` 연결 된 속성이 레이아웃에 대해 설정 되어 `BindableLayout.ItemTemplate` 있지만 연결 된 속성이 설정 되지 않은 경우에는 해당 `IEnumerable` 컬렉션의 모든 항목이 [`Label`](xref:Xamarin.Forms.Label) `BindableLayout` 클래스에서 만든에 의해 표시 됩니다.

## <a name="define-item-appearance"></a>항목 모양 정의

바인딩 가능한 레이아웃에서 각 항목의 모양은 `BindableLayout.ItemTemplate` 연결 된 속성을로 설정 하 여 정의할 수 있습니다 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate).

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding User.TopFollowers}"
             Orientation="Horizontal"
             ...>
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <controls:CircleImage Source="{Binding}"
                                  Aspect="AspectFill"
                                  WidthRequest="44"
                                  HeightRequest="44"
                                  ... />
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</StackLayout>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

이 예제에서 `TopFollowers` 컬렉션의 모든 항목은에 정의 된 `CircleImage` 뷰로 표시 됩니다. [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)

![DataTemplate를 사용 하 여 바인딩 가능한 레이아웃](bindable-layouts-images/top-followers.png "데이터 템플릿을 사용 하 여 바인딩 가능한 레이아웃")

데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

## <a name="choose-item-appearance-at-runtime"></a>런타임에 항목 모양 선택

바인딩 가능한 레이아웃에서 각 항목의 모양은 `BindableLayout.ItemTemplateSelector` 연결 된 속성을로 설정 하 여 항목 값에 따라 런타임에 선택할 수 있습니다. [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
DataTemplateSelector dataTemplateSelector = new TechItemTemplateSelector { ... };
var flexLayout = new FlexLayout();
BindableLayout.SetItemsSource(flexLayout, viewModel.User.FavoriteTech);
BindableLayout.SetItemTemplateSelector(flexLayout, dataTemplateSelector);
```

예제 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 응용 프로그램에서 사용 되는은 다음 예제에 나와 있습니다.

```csharp
public class TechItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinFormsTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return (string)item == "Xamarin.Forms" ? XamarinFormsTemplate : DefaultTemplate;
    }
}
```

클래스 `TechItemTemplateSelector` 는 다른 `DefaultTemplate` 데이터 `XamarinFormsTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 템플릿으로 설정 된 및 속성을 정의 합니다. 메서드 `OnSelectTemplate` 는 항목을 `XamarinFormsTemplate`"xamarin.ios"와 같은 항목 옆에 있는 진한 빨간색으로 표시 하는을 반환 합니다. 항목이 "Xamarin.ios"와 같지 않은 경우 메서드는 `OnSelectTemplate` 의 기본 색을 사용 하 `DefaultTemplate`여 항목을 표시 하는를 반환 합니다 [`Label`](xref:Xamarin.Forms.Label).

![DataTemplateSelector를 사용 하 여 바인딩 가능한 레이아웃](bindable-layouts-images/favorite-tech.png "데이터 템플릿 선택기를 사용 하 여 바인딩 가능한 레이아웃")

데이터 템플릿 선택기에 대 한 자세한 내용은 [DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)를 참조 하세요.

## <a name="display-a-string-when-data-is-unavailable"></a>데이터를 사용할 수 없을 때 문자열 표시

`EmptyView` [`Label`](xref:Xamarin.Forms.Label) 속성은 `ItemsSource` `null`속성이 이거나 `ItemsSource` 속성으로 지정 된 컬렉션이 `null` 이거나 비어 있는 경우에 의해 표시 되는 문자열로 설정 될 수 있습니다. 다음 XAML은이 시나리오의 예를 보여 줍니다.

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}"
             BindableLayout.EmptyView="No achievements">
    ...
</StackLayout>
```

따라서 데이터 바인딩된 컬렉션이 이면 `null` `EmptyView` 속성 값으로 설정 된 문자열이 표시 됩니다.

[![IOS 및 Android에서 바인딩 가능한 레이아웃 문자열 빈 보기의 스크린샷](bindable-layouts-images/emptyview-string.png "바인딩 가능한 레이아웃 문자열 빈 뷰")](bindable-layouts-images/emptyview-string-large.png#lightbox "바인딩 가능한 레이아웃 문자열 빈 뷰")

## <a name="display-views-when-data-is-unavailable"></a>데이터를 사용할 수 없을 때 보기 표시

`EmptyView` 속성은 `ItemsSource` 속성이 `null`이거나 `ItemsSource` 속성으로 지정 된 컬렉션이 `null` 이거나 비어 있을 때 표시 되는 뷰로 설정할 수 있습니다. 단일 보기 이거나 여러 자식 뷰를 포함 하는 뷰입니다. 다음 XAML 예제에서는 여러 자식 `EmptyView` 뷰를 포함 하는 뷰로 설정 된 속성을 보여 줍니다.

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
    <BindableLayout.EmptyView>
        <StackLayout>
            <Label Text="None."
                   FontAttributes="Italic"
                   FontSize="{StaticResource smallTextSize}" />
            <Label Text="Try harder and return later?"
                   FontAttributes="Italic"
                   FontSize="{StaticResource smallTextSize}" />
        </StackLayout>
    </BindableLayout.EmptyView>
    ...
</StackLayout>
```

그 결과 데이터 바인딩된 컬렉션이 이면 `null` [`StackLayout`](xref:Xamarin.Forms.StackLayout) 와 해당 자식 뷰가 표시 됩니다.

[![IOS 및 Android의 여러 뷰가 포함 된 바인딩 가능한 레이아웃 빈 보기의 스크린샷](bindable-layouts-images/emptyview-views.png "바인딩 가능한 레이아웃 빈 뷰")](bindable-layouts-images/emptyview-views-large.png#lightbox "바인딩 가능한 레이아웃 빈 뷰")

마찬가지로,는 `EmptyViewTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `ItemsSource` 속성이 `null`이거나 `ItemsSource` 속성으로 지정 된 컬렉션이 `null` 이거나 비어 있을 때 표시 되는로 설정할 수 있습니다. 에 `DataTemplate` 는 단일 뷰나 여러 자식 뷰가 포함 된 뷰가 포함 될 수 있습니다. 또한 `BindingContext` `EmptyViewTemplate` 의는의에서 상속 `BindingContext` 됩니다. `BindableLayout` 다음 XAML 예제에서는 단일 뷰 `EmptyViewTemplate` 를 포함 `DataTemplate` 하는로 설정 된 속성을 보여 줍니다.

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
    <BindableLayout.EmptyViewTemplate>
        <DataTemplate>
            <Label Text="{Binding Source={x:Reference usernameLabel}, Path=Text, StringFormat='{0} has no achievements.'}" />
        </DataTemplate>
    </BindableLayout.EmptyViewTemplate>
    ...
</StackLayout>
```

따라서 데이터 바인딩된 컬렉션이 이면 `null` [`Label`](xref:Xamarin.Forms.Label) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 의가 표시 됩니다.

[![IOS 및 Android의 바인딩 가능한 레이아웃 빈 보기 템플릿 스크린샷](bindable-layouts-images/emptyviewtemplate.png "바인딩 가능한 레이아웃 빈 뷰 템플릿")](bindable-layouts-images/emptyviewtemplate-large.png#lightbox "바인딩 가능한 레이아웃 빈 뷰 템플릿")

> [!NOTE]
> 속성 `EmptyViewTemplate` 은를 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)통해 설정할 수 없습니다.

## <a name="choose-an-emptyview-at-runtime"></a>런타임에 EmptyView 선택

데이터를 사용할 수 없을 `EmptyView` 때로 표시 되는 뷰는의 [`ContentView`](xref:Xamarin.Forms.ContentView) 개체로 정의 될 수 있습니다. [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 그런 `EmptyView` 다음 런타임에 특정 비즈니스 논리에 따라 속성 `ContentView`을 특정으로 설정할 수 있습니다. 다음 XAML은이 시나리오의 예를 보여 줍니다.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...    
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No achievements."
                       FontSize="14" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="None."
                       FontAttributes="Italic"
                       FontSize="14" />
                <Label Text="Try harder and return later?"
                       FontAttributes="Italic"
                       FontSize="14" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout>
        ...
        <Switch Toggled="OnEmptyViewSwitchToggled" />

        <StackLayout x:Name="stackLayout"
                     BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
            ...
        </StackLayout>
    </StackLayout>
</ContentPage>
```

XAML은 `EmptyView` 속성 값 [`ContentView`](xref:Xamarin.Forms.ContentView) 으로 설정 될 `ContentView` 개체를 제어 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)하는 [`Switch`](xref:Xamarin.Forms.Switch) 개체를 사용 하 여 페이지 수준에서 두 개체를 정의 합니다. `Switch` 가 전환 되 면 `OnEmptyViewSwitchToggled` 이벤트 처리기가 `ToggleEmptyView` 메서드를 실행 합니다.

```csharp
void ToggleEmptyView(bool isToggled)
{
    object view = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
    BindableLayout.SetEmptyView(stackLayout, view);
}
```

메서드 `ToggleEmptyView` `EmptyView` 는 [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) 속성 값에 따라 `stackLayout` 개체의 속성을에 [`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)저장 된 두 개체 중 하나로 설정 합니다. 그런 다음 데이터 바인딩된 컬렉션이 이면 `null` `ContentView` `EmptyView` 속성으로 설정 된 개체가 표시 됩니다.

[![IOS 및 Android에서 런타임에의 빈 뷰 선택 스크린샷](bindable-layouts-images/emptyview-runtime.png "바인딩 가능한 레이아웃 빈 뷰 런타임 선택")](bindable-layouts-images/emptyview-runtime-large.png#lightbox "바인딩 가능한 레이아웃 빈 뷰 런타임 선택")

## <a name="related-links"></a>관련 링크

- [바인딩 가능한 레이아웃 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [사용자 지정 레이아웃 만들기](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
