---
title: Xamarin.Forms 컴파일된 바인딩
description: 이 문서에서는 컴파일된 바인딩을 사용하여 Xamarin.Forms 애플리케이션에서 데이터 바인딩 성능을 향상시키는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: ABE6B7F7-875E-4402-A1D2-845CE374402B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2019
ms.openlocfilehash: 531d9719eb4bf5c23001ebe4260254e13f9989eb
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697152"
---
# <a name="xamarinforms-compiled-bindings"></a>Xamarin.Forms 컴파일된 바인딩

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

_컴파일된 바인딩을 클래식 바인딩보다 더 빨리 확인할 수 있으므로 Xamarin.Forms 애플리케이션에서 데이터 바인딩 성능이 향상됩니다._

데이터 바인딩에는 두 개의 주요 문제가 있습니다.

1. 바인딩 식의 컴파일 시간 유효성 검사가 없습니다. 대신 바인딩을 런타임 시 확인합니다. 따라서 애플리케이션이 예상대로 동작하지 않거나 오류 메시지가 나타나는 경우 모든 잘못된 바인딩은 런타임 때까지 검색되지 않습니다.
1. 데이터 바인딩은 비용 효율적이지 않습니다. 범용 개체 검사(리플렉션)를 사용하여 런타임 시 바인딩을 확인하므로 이 작업을 수행하는 오버헤드는 플랫폼마다 다릅니다.

컴파일된 바인딩은 런타임 대신 컴파일 시간에 바인딩 식을 확인하여 Xamarin.Forms 애플리케이션에서 데이터 바인딩 성능을 향상시킵니다. 또한 이 바인딩 식의 컴파일 시간 유효성 검사를 사용하면 잘못된 바인딩이 빌드 오류로 보고되기 때문에 개발자의 문제 해결 환경을 개선할 수 있습니다.

컴파일된 바인딩을 사용하는 프로세스는 다음과 같습니다.

1. XAML 컴파일 사용 XAML 컴파일에 대한 자세한 내용은 [XAML 컴파일](~/xamarin-forms/xaml/xamlc.md)을 참조하세요.
1. [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 `x:DataType` 특성을 `VisualElement` 및 자식이 바인딩되는 개체의 형식으로 설정합니다.

> [!NOTE]
> [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)로 설정되므로 뷰 계층 구조의 동일한 수준으로 `x:DataType` 특성을 설정하는 것이 좋습니다. 그러나 이 특성은 뷰 계층 구조의 모든 위치에서 다시 정의할 수 있습니다.

컴파일된 바인딩을 사용하려면 `x:DataType` 특성을 문자열 리터럴로 설정하거나 `x:Type` 태그 확장을 사용하여 형식으로 설정해야 합니다. XAML 컴파일 시간에 잘못된 바인딩 식은 빌드 오류로 보고됩니다. 그러나 XAML 컴파일러는 발견되는 첫 번째 잘못된 바인딩 식에 대한 빌드 오류만 보고합니다. `VisualElement` 또는 자식에서 정의된 모든 유효한 바인딩 식은 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)가 XAML 또는 코드로 설정됐는지 여부에 관계 없이 컴파일됩니다. 바인딩 식을 컴파일하면 *원본*의 속성에서 값을 가져오고 태그에서 지정되는 *대상*의 속성에서 그 값을 설정하는 컴파일된 코드를 생성합니다. 또한 바인딩 식에 따라 생성된 코드는 *원본* 속성의 값의 변경 내용을 관찰하고 *대상* 속성을 새로 고침하여 거꾸로 *대상*부터 *원본*까지 변경 내용을 푸시할 수 있습니다.

> [!IMPORTANT]
> 컴파일된 바인딩은 [`Source`](xref:Xamarin.Forms.Binding.Source) 속성을 정의하는 모든 바인딩 식에 대해 현재 사용할 수 없습니다. 이는 컴파일 시간에 확인할 수 없는 `x:Reference` 태그 확장을 사용하여 항상 `Source` 속성이 설정되기 때문입니다.

## <a name="use-compiled-bindings"></a>컴파일된 바인딩 사용

**컴파일된 색상 선택기** 페이지에서는 Xamarin.Forms 뷰 및 viewmodel 속성 간에 컴파일된 바인딩을 사용하는 방법을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.CompiledColorSelectorPage"
             Title="Compiled Color Selector">
    ...
    <StackLayout x:DataType="local:HslColorViewModel">
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>
        <BoxView Color="{Binding Color}"
                 ... />
        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />
            <Slider Value="{Binding Hue}" />
            <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />
            <Slider Value="{Binding Saturation}" />
            <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />
            <Slider Value="{Binding Luminosity}" />
            <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
        </StackLayout>
    </StackLayout>    
</ContentPage>
```

루트 [`StackLayout`](xref:Xamarin.Forms.StackLayout)은 `HslColorViewModel`을 인스턴스화하고 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 속성에 대한 속성 요소 태그 내의 `Color` 속성을 초기화합니다. 이 루트 `StackLayout`도 `x:DataType` 특성을 viewmodel 형식으로 정의하고 루트 `StackLayout` 뷰 계층 구조의 모든 바인딩 식을 컴파일한다는 것을 나타냅니다. 결국 빌드 오류로 나타나는 존재하지 않는 viewmodel 속성에 바인딩하려면 바인딩 식 중 하나를 변경하여 확인할 수 있습니다. 이 예제에서는 `x:DataType` 특성을 문자열 리터럴로 설정하고 있지만, `x:Type` 태그 확장을 사용하여 형식으로 설정할 수도 있습니다. `x:Type` 태그 확장에 대한 자세한 내용은 [x:Type 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#type)을 참조하세요.

> [!IMPORTANT]
> 이 `x:DataType` 특성은 뷰 계층 구조의 모든 지점에서 다시 정의할 수 있습니다.

[`BoxView`](xref:Xamarin.Forms.BoxView), [`Label`](xref:Xamarin.Forms.Label) 요소 및 [`Slider`](xref:Xamarin.Forms.Slider) 뷰는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에서 바인딩 컨텍스트를 상속받습니다. 이러한 뷰는 모두 viewmodel의 원본 속성을 참조하는 바인딩 대상입니다. [`BoxView.Color`](xref:Xamarin.Forms.BoxView.Color) 속성 및 [`Label.Text`](xref:Xamarin.Forms.Label.Text) 속성의 경우 데이터 바인딩은 `OneWay`이며, 뷰의 속성은 viewmodel의 속성에서 설정됩니다. 그러나 [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) 속성은 `TwoWay` 바인딩을 사용합니다. 이렇게 하면 각 `Slider`를 viewmodel에서 설정하고 각 `Slider`에서 viewmodel을 설정할 수 있습니다.

애플리케이션을 처음 실행하는 경우 [`BoxView`](xref:Xamarin.Forms.BoxView), [`Label`](xref:Xamarin.Forms.Label) 요소 및 [`Slider`](xref:Xamarin.Forms.Slider) 요소는 viewmodel을 인스턴스화했을 때 설정된 초기 `Color` 속성에 따라 모두 viewmodel에서 설정됩니다. 이 과정은 다음 스크린샷에 나와 있습니다.

[![컴파일된 색 선택기](compiled-bindings-images/compiledcolorselector-small.png "컴파일된 색 선택기")](compiled-bindings-images/compiledcolorselector-large.png#lightbox "컴파일된 색 선택기")

슬라이더를 조작하는 대로 적절히 [`BoxView`](xref:Xamarin.Forms.BoxView) 및 [`Label`](xref:Xamarin.Forms.Label) 요소를 업데이트합니다.

이 색상 선택기에 대한 자세한 내용은 [Viewmodel 및 속성 변경 알림](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#viewmodels-and-property-change-notifications)을 참조하세요.

## <a name="use-compiled-bindings-in-a-datatemplate"></a>DataTemplate에서 컴파일된 바인딩 사용

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)의 바인딩은 템플릿이 적용된 개체의 컨텍스트로 해석됩니다. 따라서 `DataTemplate`에서 컴파일된 바인딩을 사용하는 경우 `DataTemplate`은 `x:DataType` 특성을 사용하여 해당 데이터 개체의 형식을 선언해야 합니다.

**컴파일된 색상 목록** 페이지에서는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)에서 컴파일된 바인딩을 사용하는 방법을 보여줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.CompiledColorListPage"
             Title="Compiled Color List">
    <Grid>
        ...
        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  ... >
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:NamedColor">
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     ... />
                            <Label Text="{Binding FriendlyName}"
                                   ... />
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
        <!-- The BoxView doesn't use compiled bindings -->
        <BoxView Color="{Binding Source={x:Reference colorListView}, Path=SelectedItem.Color}"
                 ... />
    </Grid>
</ContentPage>
```

[`ListView.ItemsSource`](xref:Xamarin.Forms.ListView) 속성은 `NamedColor.All` 정적 속성으로 설정됩니다. `NamedColor` 클래스는 .NET 리플렉션을 사용하여 [`Color`](xref:Xamarin.Forms.Color) 구조에서 모든 정적 공용 필드를 열거하고 `All` 정적 속성에서 액세스할 수 있는 컬렉션의 이름으로 저장합니다. 따라서 `ListView`는 모든 `NamedColor` 인스턴스로 채워집니다. `ListView`의 각 항목의 경우 해당 항목에 대한 바인딩 컨텍스트는 `NamedColor` 개체로 설정됩니다. [`ViewCell`](xref:Xamarin.Forms.ViewCell)의 [`BoxView`](xref:Xamarin.Forms.BoxView) 및 [`Label`](xref:Xamarin.Forms.Label) 요소는 `NamedColor` 속성에 바인딩됩니다.

[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)은 `x:DataType` 특성을 `NamedColor` 형식으로 정의하고 `DataTemplate` 뷰 계층 구조의 모든 바인딩 식을 컴파일한다는 것을 나타냅니다. 결국 빌드 오류로 나타나는 존재하지 않는 `NamedColor` 속성에 바인딩하려면 바인딩 식 중 하나를 변경하여 확인할 수 있습니다.  이 예제에서는 `x:DataType` 특성을 문자열 리터럴로 설정하고 있지만, `x:Type` 태그 확장을 사용하여 형식으로 설정할 수도 있습니다. `x:Type` 태그 확장에 대한 자세한 내용은 [x:Type 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#type)을 참조하세요.

애플리케이션을 처음 실행하는 경우 [`ListView`](xref:Xamarin.Forms.ListView)는 `NamedColor` 인스턴스로 채워집니다. `ListView`의 항목을 선택하면 [`BoxView.Color`](xref:Xamarin.Forms.BoxView.Color) 속성이 `ListView`에서 선택한 항목의 색상으로 설정됩니다.

[![컴파일된 색 목록](compiled-bindings-images/compiledcolorlist-small.png "컴파일된 색 목록]")](compiled-bindings-images/compiledcolorlist-large.png#lightbox "Compiled Color List")

[`ListView`](xref:Xamarin.Forms.BoxView)에서 다른 항목을 선택하면 [`BoxView`](xref:Xamarin.Forms.BoxView)의 색상을 업데이트합니다.

## <a name="combine-compiled-bindings-with-classic-bindings"></a>클래식 바인딩과 컴파일된 바인딩 결합

바인딩 식은 `x:DataType` 특성을 정의하는 뷰 계층 구조에 대해서만 컴파일됩니다. 반대로 `x:DataType` 특성을 정의하지 않는 계층 구조의 모든 뷰는 클래식 바인딩을 사용합니다. 따라서 페이지에서 컴파일된 바인딩 및 클래식 바인딩을 결합할 수 있습니다. 예를 들어 이전 섹션에서 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 내의 뷰는 컴파일된 바인딩을 사용하는 반면 [`ListView`](xref:Xamarin.Forms.ListView)에서 선택된 색상으로 설정된 [`BoxView`](xref:Xamarin.Forms.BoxView)은 사용하지 않습니다.

따라서 `x:DataType` 특성을 신중하게 구성하면 컴파일된 바인딩 및 클래식 바인딩을 사용하는 페이지가 표시될 수 있습니다. 또는 `x:DataType` 특성은 언제든지 뷰 계층 구조에서 `x:Null` 태그 확장을 사용하는 `null`로 다시 정의될 수 있습니다. 이 작업을 수행하면 뷰 계층 구조 내의 모든 바인딩 식이 클래식 바인딩을 사용한다는 것이 표시됩니다. *혼합 바인딩* 페이지에서 이 방법을 보여줍니다.

```xaml
<StackLayout x:DataType="local:HslColorViewModel">
    <StackLayout.BindingContext>
        <local:HslColorViewModel Color="Sienna" />
    </StackLayout.BindingContext>
    <BoxView Color="{Binding Color}"
             VerticalOptions="FillAndExpand" />
    <StackLayout x:DataType="{x:Null}"
                 Margin="10, 0">
        <Label Text="{Binding Name}" />
        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />
        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />
        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</StackLayout>   
```

루트 [`StackLayout`](xref:Xamarin.Forms.StackLayout)은 `x:DataType` 특성을 `HslColorViewModel` 형식으로 설정하고 루트 `StackLayout` 뷰 계층 구조의 모든 바인딩 식을 컴파일한다는 것을 나타냅니다. 그러나 내부 `StackLayout`은 `x:Null` 태그 식을 사용하여 `x:DataType` 특성을 `null`로 다시 정의합니다. 따라서 내부 `StackLayout` 내의 바인딩 식은 클래식 바인딩을 사용합니다. 루트 `StackLayout` 계층 구조 내의 [`BoxView`](xref:Xamarin.Forms.BoxView)만 컴파일된 바인딩을 사용합니다.

`x:Null` 태그 식에 대한 자세한 내용은 [x:Null Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#null)을 참조하세요.

## <a name="performance"></a>성능

컴파일된 바인딩은 다양한 성능 이점으로 데이터 바인딩 성능을 향상시킵니다. 단위 테스트는 다음을 보여줍니다.

- 속성 변경 알림을 사용하는 컴파일된 바인딩(예: `OneWay`, `OneWayToSource` 또는 `TwoWay` 바인딩)은 클래식 바인딩보다 약 8배 빠르게 확인합니다.
- 속성 변경 알림을 사용하지 않는 컴파일된 바인딩(예: `OneTime` 바인딩)은 클래식 바인딩보다 약 20배 빠르게 확인합니다.
- 속성 변경 알림을 사용하는 컴파일된 바인딩(예: `OneWay`, `OneWayToSource` 또는 `TwoWay` 바인딩)에서 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 설정하면 클래식 바인딩에서 `BindingContext`를 설정하는 것보다 약 5배 빠릅니다.
- 속성 변경 알림을 사용하지 않는 컴파일된 바인딩(예: `OneTime` 바인딩)에서 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 설정하면 클래식 바인딩에서 `BindingContext`를 설정하는 것보다 약 7배 빠릅니다.

이러한 성능 차이는 모바일 디바이스에서 수정되며, 사용되는 플랫폼, 사용되는 운영 체제 버전 및 애플리케이션이 실행되는 디바이스에 따라 달라질 수 있습니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
