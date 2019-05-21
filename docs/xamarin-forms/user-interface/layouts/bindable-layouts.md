---
title: Xamarin.Forms에 바인딩할 수 있는 레이아웃
description: 바인딩 가능한 레이아웃 DataTemplate 사용 하 여 각 항목의 모양을 설정 하는 옵션을 사용 하 여 항목의 컬렉션에 바인딩하여 해당 콘텐츠를 생성 하려면 레이아웃 클래스를 사용 합니다.
ms.prod: xamarin
ms.assetid: 824C3319-20A0-42D0-8632-CDECD98349C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
ms.openlocfilehash: 28846e6e9590d2adf56114fce8bc6056c0112ac1
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970962"
---
# <a name="bindable-layouts-in-xamarinforms"></a>Xamarin.Forms에 바인딩할 수 있는 레이아웃

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)

바인딩 가능한 레이아웃에서 파생 되는 모든 레이아웃 클래스를 사용 하도록 설정 합니다 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) 사용 하 여 각 항목의 모양을 설정 하는 옵션을 사용 하 여 항목의 컬렉션에 바인딩하여 해당 콘텐츠를 생성 하는 클래스를 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). 제공 하는 바인딩 가능한 레이아웃을 `BindableLayout` 다음 연결 된 속성을 노출 하는 클래스:

- `ItemsSource` -컬렉션을 지정 `IEnumerable` 레이아웃으로 표시 될 항목입니다.
- `ItemTemplate` – 지정 된 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 레이아웃으로 표시 되는 항목 컬렉션의 각 항목에 적용할 합니다.
- `ItemTemplateSelector` – 지정 합니다 [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) 선택 하는 데 수는 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 런타임 시 항목에 대 한 합니다.

이러한 속성을 연결할 수는 [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout)를 [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout)를 [ `Grid` ](xref:Xamarin.Forms.Grid)를 [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) 및 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 에서 파생 되는 클래스를 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) 클래스입니다.

> [!NOTE]
> `ItemTemplate` 속성이 우선 때 모두 합니다 `ItemTemplate` 및 `ItemTemplateSelector` 속성이 설정 됩니다.

합니다 `Layout<T>` 노출 클래스를 [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) 레이아웃의 자식 요소에 추가 되는 컬렉션입니다. 경우는 `BinableLayout.ItemsSource` 속성 항목의 컬렉션 설정 되 고 연결을 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)-파생된 클래스는 컬렉션의 각 항목에 추가 됩니다는 `Layout<T>.Children` 레이아웃으로 표시 하기 위해 컬렉션. `Layout<T>`-파생된 클래스는 기본 컬렉션이 변경 될 때에 다음 해당 자식 뷰를 업데이트 합니다. Xamarin.Forms 레이아웃 주기에 대 한 자세한 내용은 참조 하세요. [사용자 지정 레이아웃 만들기](~/xamarin-forms/user-interface/layouts/custom.md)합니다.

표시할 항목의 컬렉션 이며, 스크롤 및 선택을 필요 하지 않습니다 하는 경우에 바인딩할 수 있는 레이아웃을 사용 해야 합니다. 스크롤에 바인딩할 수 있는 레이아웃을 래핑하여 제공 될 수 있습니다 하는 동안를 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView)를 바인딩할 수 있는 레이아웃 UI 가상화 부족으로 권장 되지 않습니다. 스크롤이 필요한 경우, 같은 UI 가상화를 포함 하는 스크롤 가능한 뷰입니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 하거나 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView)를 사용 해야 합니다. 이 권장 사항을 준수 하지 않으면 성능 문제가 발생할 수 있습니다.

> [!IMPORTANT]
>기술적으로 바인딩할 수 있는 레이아웃에서 파생 되는 모든 레이아웃 클래스에 연결할 수 있지만 합니다 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) 클래스인 어렵습니다 항상 이렇게 하는 데에 [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout) 하십시오 [ `Grid` ](xref:Xamarin.Forms.Grid), 및 [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout) 클래스입니다. 예를 들어 있는 데이터의 컬렉션을 표시 하려는 시나리오를 [ `Grid` ](xref:Xamarin.Forms.Grid) 여기서 각 항목 컬렉션에는 개체를 바인딩할 수 있는 레이아웃을 사용 하 여 여러 속성을 포함 합니다. 각 행에는 `Grid` 의 각 열을 사용 하 여 컬렉션에서 개체를 표시 해야 합니다 `Grid` 개체의 속성 중 하나를 표시 합니다. 때문에 합니다 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 특정에서개체의속성중하나가표시각각여러뷰가포함된레이아웃클래스로개체에필요한것만바인딩할수있는레이아웃단일개체를포함할수있습니다에대한`Grid` 열입니다. 이 시나리오는 바인딩할 수 있는 레이아웃을 사용 하 여 realised 수, 하는 동안 발생 하는 부모 `Grid` 자식이 포함 된 `Grid` 바인딩된 컬렉션에 있는 각 항목에 대해는 매우 비효율적 이며 문제가 사용 되는 `Grid` 레이아웃 합니다.

## <a name="populating-a-bindable-layout-with-data"></a>데이터를 사용 하 여 바인딩 가능한 레이아웃 채우기

바인딩할 수 있는 레이아웃을 설정 하 여 데이터로 채워질 때 해당 `ItemsSource` 속성을 구현 하는 컬렉션 `IEnumerable`에 연결 하는 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)-파생 클래스:

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

때를 `BindableLayout.ItemsSource` 레이아웃을 연결 된 속성이 설정 되어 있지만 `BindableLayout.ItemTemplate` 연결된 속성을 설정 하지 않으면 모든 항목는 `IEnumerable` 컬렉션으로 표시 됩니다는 [ `Label` ](xref:Xamarin.Forms.Label) 합니다 에의해만들어집니다`BindableLayout` 클래스입니다.

## <a name="defining-item-appearance"></a>항목 모양을 정의

바인딩 가능한 레이아웃의 각 항목의 모양을 설정 하 여 정의할 수 있습니다 합니다 `BindableLayout.ItemTemplate` 연결 된 속성을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

이 예제에서는 모든 항목에는 `TopFollowers` 컬렉션으로 표시 되는 `CircleImage` 에 정의 된 뷰를 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

![DataTemplate 사용 하 여 바인딩 가능한 레이아웃](bindable-layouts-images/top-followers.png "데이터 템플릿 사용 하 여 바인딩 가능한 레이아웃")

데이터 템플릿에 대한 자세한 내용은 [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.

## <a name="choosing-item-appearance-at-runtime"></a>런타임 시 선택 항목 모양

값을 기반으로 항목을 설정 하 여 런타임에 바인딩할 수 있는 레이아웃의 각 항목의 모양을 선택할 수 있습니다 합니다 `BindableLayout.ItemTemplateSelector` 연결 된 속성을 [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector):

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

합니다 [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector) 사용 되는 샘플에서 응용 프로그램 예제에 표시 됩니다.

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

합니다 `TechItemTemplateSelector` 클래스 정의 `DefaultTemplate` 하 고 `XamarinFormsTemplate` [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 다른 데이터 템플릿에 설정 된 속성을 합니다. 합니다 `OnSelectTemplate` 메서드가 반환 되는 `XamarinFormsTemplate`, 항목 "Xamarin.Forms" 다음과 같을 경우 항목 옆에 핵심을 사용 하 여 진한 빨간색으로 표시 하는 합니다. 항목 "Xamarin.Forms"과 같지 않습니다 경우는 `OnSelectTemplate` 메서드가 반환 되는 합니다 `DefaultTemplate`의 기본 색을 사용 하 여 항목을 표시 하는 [ `Label` ](xref:Xamarin.Forms.Label):

![DataTemplateSelector 사용 하 여 바인딩 가능한 레이아웃](bindable-layouts-images/favorite-tech.png "데이터 템플릿 선택기를 사용 하 여 바인딩 가능한 레이아웃")

데이터 템플릿 선택기에 대 한 자세한 내용은 참조 하세요. [Xamarin.Forms DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)합니다.

## <a name="related-links"></a>관련 링크

- [바인딩 가능한 레이아웃 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)
- [사용자 지정 레이아웃 만들기](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin.Forms 데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin.Forms DataTemplateSelector 만들기](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
