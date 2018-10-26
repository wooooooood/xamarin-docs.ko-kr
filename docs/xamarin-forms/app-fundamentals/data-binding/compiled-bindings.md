---
title: Xamarin.Forms 컴파일된 바인딩
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 데이터 바인딩 성능 향상을 위해 컴파일된 바인딩을 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: ABE6B7F7-875E-4402-A1D2-845CE374402B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2018
ms.openlocfilehash: 0b350082c834076a1d69427644259087d64bf26a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111606"
---
# <a name="xamarinforms-compiled-bindings"></a>Xamarin.Forms 컴파일된 바인딩

_컴파일된 바인딩은 Xamarin.Forms 응용 프로그램에서 데이터 바인딩 성능을 향상 시키기 위해 클래식 바인딩 보다 더 빠르게 해결 됩니다._

데이터 바인딩에 두 가지 주요 문제

1. 바인딩 식의 컴파일 타임 검사 하지 않습니다. 대신, 바인딩 런타임에 확인 됩니다. 따라서 모든 잘못 된 바인딩이 응용 프로그램이 예상 대로 작동 하지 않습니다 또는 오류 메시지가 나타날 때 런타임 시까지 검색 되지 않습니다.
1. 비용 효율적인 되지 않습니다. 범용 개체 검사 (리플렉션)를 사용 하 여 런타임에 바인딩 계산은 및이 작업을 수행 하는 오버 헤드는 플랫폼 마다 다릅니다.

런타임 대신 컴파일 타임에 바인딩 식을 확인 하 여 Xamarin.Forms 응용 프로그램에서 데이터 바인딩 성능을 향상 하는 컴파일된 바인딩. 또한이 컴파일 시간 유효성 검사의 바인딩 식에 잘못 된 바인딩이 빌드 오류로 보고 됩니다 때문에 문제 해결 환경을 더 나은 개발자 수 있습니다.

컴파일된 바인딩을 사용 하 여 프로세스 다음과 같습니다.

1. XAML 컴파일을 사용 하도록 설정 합니다. XAML 컴파일에 대 한 자세한 내용은 참조 하세요. [XAML 컴파일](~/xamarin-forms/xaml/xamlc.md)합니다.
1. 설정를 `x:DataType` 특성을 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 개체의 형식에는 `VisualElement` 자식에 바인딩합니다. 이 특성 계층 구조 보기에서에서 모든 위치에서 다시 정의할 수 있습니다 note 합니다.

> [!NOTE]
> 설정할 것이 좋습니다는 `x:DataType` 뷰 계층 구조에 동일한 수준에 있는 특성을 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 설정 됩니다.

XAML 컴파일 타임에 잘못 된 바인딩 식은 빌드 오류로 보고 됩니다. 그러나 XAML 컴파일러는 발견 되는 첫 번째 잘못 된 바인딩 식에 대 한 빌드 오류를 보고만 합니다. 에 정의 된 모든 유효한 바인딩 식의 `VisualElement` 컴파일된, 여부에 관계 없이 자식 또는 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) XAML 또는 코드에서 설정 됩니다. 속성에서 값을 가져오는 컴파일된 코드를 생성 하는 바인딩 식을 컴파일할 합니다 *원본*에 속성에 설정 합니다 *대상* 태그에 지정 된. 또한 바인딩 식에 따라 생성 된 코드를 관찰할 수 있습니다의 값이 변경 합니다 *원본* 속성 및 새로 고침 합니다 *대상* 속성인 합니다 에서변경내용을푸시할수있습니다및*대상* 다시는 *원본*합니다.

> [!IMPORTANT]
> 컴파일된 바인딩을 정의 하는 모든 바인딩 식에 대 한 현재 해제 되어는 [ `Source` ](xref:Xamarin.Forms.Binding.Source) 속성입니다. 왜냐하면를 `Source` 속성은 항상 설정 하 여 사용 하 여는 `x:Reference` 태그 확장은 컴파일 타임에 확인할 수 없습니다.

## <a name="using-compiled-bindings"></a>컴파일된 바인딩을 사용 하 여

합니다 **컴파일된 색 선택기** 페이지 Xamarin.Forms 뷰 및 ViewModel 속성 간의 컴파일된 바인딩을 사용 하는 방법을 보여 줍니다.

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

루트 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 인스턴스화합니다를 `HslColorViewModel` 초기화를 `Color` 내의 속성 요소 태그에 대 한 속성을 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 속성. 이 루트 `StackLayout` 도 정의 합니다 `x:DataType` 루트의 모든 바인딩 식을 나타내는 ViewModel 형식으로 특성 `StackLayout` 뷰 계층 구조 컴파일됩니다. 빌드 오류가 발생 하는 존재 하지 않는 ViewModel 속성을 바인딩할 바인딩 식 중 하나를 변경 하 여이 확인할 수 있습니다.

> [!IMPORTANT]
> `x:DataType` 특성 뷰 계층 구조에 언제 든 지 다시 정의 될 수 있습니다.

합니다 [ `BoxView` ](xref:Xamarin.Forms.BoxView)를 [ `Label` ](xref:Xamarin.Forms.Label) 요소 및 [ `Slider` ](xref:Xamarin.Forms.Slider) 보기에서 바인딩 컨텍스트를 상속 받습니다. 합니다 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). 이러한 뷰는 ViewModel에 원본 속성을 참조 하는 모든 바인딩 대상입니다. 에 대 한 합니다 [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) 속성 및 [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) 속성을 데이터 바인딩에 `OneWay` – 뷰에서 속성을 ViewModel의 속성에서 설정 됩니다. 그러나 합니다 [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) 속성에서 사용 하는 `TwoWay` 바인딩. 이렇게 하면 각 `Slider` ViewModel에서 그리고 각 설정할 ViewModel 설정할 `Slider`합니다.

응용 프로그램을 처음 실행할 때 합니다 [ `BoxView` ](xref:Xamarin.Forms.BoxView)를 [ `Label` ](xref:Xamarin.Forms.Label) 요소 및 [ `Slider` ](xref:Xamarin.Forms.Slider) 요소가 모두 설정에 따라 viewmodel 초기 `Color` ViewModel 인스턴스화된 경우 속성을 설정 합니다. 다음 스크린샷과에서 같습니다.

[![색 선택기 컴파일된](compiled-bindings-images/compiledcolorselector-small.png "색 선택기 컴파일된")](compiled-bindings-images/compiledcolorselector-large.png#lightbox "컴파일된 색 선택기")

슬라이더를 조작 하는 대로 합니다 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 하 고 [ `Label` ](xref:Xamarin.Forms.Label) 요소 적절히 업데이트 됩니다.

이 색 선택기에 대 한 자세한 내용은 참조 하세요. [Viewmodel 및 속성 변경 알림을](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#viewmodels-and-property-change-notifications)합니다.

## <a name="using-compiled-bindings-in-a-datatemplate"></a>DataTemplate에서 컴파일된 바인딩 사용

바인딩은 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 템플릿 개체의 컨텍스트에서 해석 됩니다. 따라서 사용 하 여 컴파일할 때의 바인딩을 `DataTemplate`, `DataTemplate` 사용 하 여 해당 데이터 개체의 형식을 선언 해야 하는 경우는 `x:DataType` 특성입니다.

합니다 **색 목록 컴파일된** 페이지에서 컴파일된 바인딩을 사용 하는 방법을 보여 줍니다는 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

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

합니다 [ `ListView.ItemsSource` ](xref:Xamarin.Forms.ListView) 정적 속성이 `NamedColor.All` 속성입니다. `NamedColor` 클래스.NET 리플렉션을 사용 하 여 모든 정적 공용 필드를 열거 합니다 [ `Color` ](xref:Xamarin.Forms.Color) 구조 및 정적에서 액세스할 수 있는 컬렉션의 이름으로 저장 하기 `All` 속성. 따라서 합니다 `ListView` 일부만 채워진는 `NamedColor` 인스턴스. 각 항목에 대 한 합니다 `ListView`, 항목에 대 한 바인딩 컨텍스트를로 `NamedColor` 개체입니다. 합니다 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 및 [ `Label` ](xref:Xamarin.Forms.Label) 요소에는 [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) 바인딩되는 `NamedColor` 속성입니다.

[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 정의 `x:DataType` 특성을는 `NamedColor` 를 나타내는 형식 바인딩 식에서는 `DataTemplate` 뷰 계층 구조 컴파일됩니다. 존재 하지 않는 바인딩할 바인딩 식 중 하나를 변경 하 여 확인할 수 있습니다 `NamedColor` 속성 빌드 오류가 발생 합니다.

응용 프로그램을 처음 실행할 때 합니다 [ `ListView` ](xref:Xamarin.Forms.ListView) 채워집니다 `NamedColor` 인스턴스. 항목을 `ListView` 을 선택 하면를 [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) 에서 선택한 항목의 색 속성는 `ListView`:

[![색 목록 컴파일된](compiled-bindings-images/compiledcolorlist-small.png "색 목록 컴파일된]")](compiled-bindings-images/compiledcolorlist-large.png#lightbox "Compiled Color List")

다른 항목을 선택 하는 [ `ListView` ](xref:Xamarin.Forms.BoxView) 의 색을 업데이트 합니다 [ `BoxView` ](xref:Xamarin.Forms.BoxView)합니다.

## <a name="combining-compiled-bindings-with-classic-bindings"></a>클래식 바인딩 사용 하 여 컴파일된 결합 바인딩

뷰 계층 구조에 대 한 바인딩 식만 컴파일됩니다는 `x:DataType` 특성에서 정의 됩니다. 반대로 모든 뷰 계층의 기반이 된 `x:DataType` 특성이 정의 되지 않았습니다 클래식 바인딩을 사용 합니다. 되므로 컴파일된 바인딩 및 클래식 바인딩 페이지에서 결합할 수 있습니다. 예를 들어, 이전 섹션 내의 보기를 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 컴파일된 바인딩을 사용 하는 동안 합니다 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 에서 선택한 색으로 설정 된는 [ `ListView` ](xref:Xamarin.Forms.ListView) 하지 않습니다.

신중 하 게 구조화 `x:DataType` 특성 컴파일되고 클래식 바인딩을 사용 하 여 페이지에 따라서 발생할 수 있습니다. 또는 합니다 `x:DataType` 특성은 언제 든 지 보기 계층에서 다시 정의 될 수 `null` 사용 하 여는 `x:Null` 태그 확장 합니다. 뷰 계층 구조 내에서 모든 바인딩 식이 클래식 바인딩을 사용 함을 나타내며이 작업을 수행 합니다. 합니다 *혼합 바인딩* 페이지에는이 방법을 보여 줍니다.

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

루트 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 설정의 `x:DataType` 특성을는 `HslColorViewModel` 를 나타내는 형식 바인딩 식 루트에 `StackLayout` 뷰 계층 구조 컴파일됩니다. 그러나 내부 `StackLayout` 다시 정의 하는 `x:DataType` 특성을 `null` 사용 하 여는 `x:Null` 태그 식입니다. 따라서 내부 내에서 바인딩 식을 `StackLayout` 클래식 바인딩을 사용 합니다. 만 [ `BoxView` ](xref:Xamarin.Forms.BoxView), 루트 `StackLayout` 계층 구조를 사용 하 여 컴파일된 바인딩을 보기.

에 대 한 자세한 내용은 합니다 `x:Null` 태그 식 참조 [X:null 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#null)합니다.

## <a name="performance"></a>성능

컴파일된 바인딩 데이터 바인딩 성능 혜택 다양 한 성능이 향상 됩니다. 단위 테스트 임을 보여 줍니다.

- 속성 변경 알림을 사용 하는 컴파일된 바인딩에서 (예:는 `OneWay`, `OneWayToSource`, 또는 `TwoWay` 바인딩) 약 8 번 클래식 바인딩 보다 더 빠르게 해결 됩니다.
- 컴파일된 바인딩에서 속성 변경 알림을 사용 하지 않는 (즉, 한 `OneTime` 바인딩) 약 20 배 클래식 바인딩 보다 더 빠르게 해결 됩니다.
- 설정 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 속성을 사용 하는 컴파일된 바인딩에서 변경 알림 (즉,는 `OneWay`, `OneWayToSource`, 또는 `TwoWay` 바인딩) 설정 합니다 보다5배정도빠릅니다`BindingContext`클래식 바인딩에서.
- 설정 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 컴파일된 바인딩에서 속성을 사용 하지 않는 변경 알림 (즉,는 `OneTime` 바인딩)는 약 7 회 설정 보다는 `BindingContext` 클래식 바인딩에서.

이러한 성능 차이 응용 프로그램이 실행 되는 장치와 사용 중인 운영 체제의 버전에서 모바일 장치를 사용 중인 플랫폼에 따라 달라 집니다 늘어날 수 있습니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
