---
title: Xamarin.Forms 기본 바인딩
description: 이 문서에서는 Xamarin.Forms 데이터 바인딩을 사용하는 방법을 설명합니다. 이 바인딩은 두 개체 사이의 속성 쌍을 연결하며, 이러한 개체 중 적어도 하나는 일반적으로 사용자 인터페이스 개체입니다. 이러한 두 개체는 대상과 원본이라고 합니다.
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2019
ms.custom: video
ms.openlocfilehash: 2227e2bd47a5b4960d28be67bac7947a4fb57a93
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303782"
---
# <a name="xamarinforms-basic-bindings"></a>Xamarin.Forms 기본 바인딩

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

Xamarin.Forms 데이터 바인딩은 두 개의 개체 사이의 속성 쌍을 연결하며, 이러한 개체 중 적어도 하나는 일반적으로 사용자 인터페이스 개체입니다. 이러한 두 개체는 *대상* 및 *원본*이라고 합니다.

- *대상*은 데이터 바인딩이 설정된 개체(및 속성)입니다.
- *원본*은 데이터 바인딩에서 참조하는 개체(및 속성)입니다.

이 구분은 경우에 따라 다소 혼란스러울 수 있습니다. 가장 간단한 경우 데이터는 소스에서 대상으로 이동합니다. 즉 대상 속성의 값이 소스 속성의 값에서 설정됩니다. 하지만 어떤 경우에는 데이터가 대상에서 원본으로 또는 양방향으로 이동할 수 있습니다. 혼란을 방지하기 위해 데이터를 받는 것이 아니라 데이터를 제공하는 경우에도 대상이 항상 데이터 바인딩이 설정된 개체라는 점을 명심하세요.

## <a name="bindings-with-a-binding-context"></a>바인딩 컨텍스트가 있는 바인딩

데이터 바인딩은 일반적으로 XAML에 완전히 지정되지만 코드에서 데이터 바인딩을 확인하는 것이 좋습니다. **기본 코드 바인딩** 페이지에는 `Label` 및 `Slider`가 있는 XAML 파일이 포함됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicCodeBindingPage"
             Title="Basic Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="48"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Slider`는 0 - 360의 범위로 설정됩니다. 이 프로그램의 목적은 `Slider`를 조작하여 `Label`을 회전시키는 것입니다.

데이터 바인딩이 없으면 `Slider`의 `ValueChanged` 이벤트를 `Slider`의 `Value` 속성에 액세스하고 해당 값을 `Label`의 `Rotation` 속성으로 설정하는 이벤트 처리기로 설정합니다. 데이터 바인딩은 이 작업을 자동화하므로 이벤트 처리기와 그 안에 있는 코드가 더 이상 필요하지 않습니다.

`Element`, `VisualElement`, `View` 및 `View` 파생문이 포함된 [`BindableObject`](xref:Xamarin.Forms.BindableObject)에서 파생되는 모든 클래스의 인스턴스에 바인딩을 설정할 수 있습니다.  바인딩은 항상 대상 개체에 설정됩니다. 바인딩은 원본 개체를 참조합니다. 데이터 바인딩을 설정하려면 대상 클래스의 다음 두 멤버를 사용합니다.

- [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 속성은 원본 개체를 지정합니다.
- [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 메서드는 대상 속성 및 원본 속성을 지정합니다.

이 예제에서 `Label`은 바인딩 대상이고 `Slider`는 바인딩 원본입니다. `Slider` 원본의 변경은 `Label` 대상의 회전에 영향을 줍니다. 데이터는 원본에서 대상으로 이동합니다.

`BindableObject`에서 정의된 `SetBinding` 메서드에 [`Binding`](xref:Xamarin.Forms.Binding) 클래스에서 파생되는 [`BindingBase`](xref:Xamarin.Forms.BindingBase) 형식의 인수가 있지만, [`BindableObjectExtensions`](xref:Xamarin.Forms.BindableObjectExtensions) 클래스에서 정의된 다른 `SetBinding` 메서드가 있습니다. **기본 코드 바인딩** 샘플의 코드 숨김 파일은 이 클래스의 더 간단한 [`SetBinding`](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) 확장 메서드를 사용합니다.

```csharp
public partial class BasicCodeBindingPage : ContentPage
{
    public BasicCodeBindingPage()
    {
        InitializeComponent();

        label.BindingContext = slider;
        label.SetBinding(Label.RotationProperty, "Value");
    }
}
```

`Label` 개체는 바인딩 대상이므로 이 속성이 설정되고 메서드가 호출되는 개체입니다. `BindingContext` 속성은 바인딩 원본, 즉 `Slider`를 나타냅니다.

`SetBinding` 메서드는 바인딩 대상에서 호출되지만 대상 속성과 원본 속성을 모두 지정합니다. 대상 속성은 `BindableProperty` 개체, 즉 `Label.RotationProperty`로 지정됩니다. 원본 속성은 문자열로 지정되고 `Slider`의 `Value` 속성을 나타냅니다.

`SetBinding` 메서드는 데이터 바인딩의 가장 중요한 규칙 중 하나를 표시합니다.

*대상 속성은 바인딩 가능한 속성으로 지원되어야 합니다.*

이 규칙은 대상 개체가 `BindableObject`에서 파생되는 클래스의 인스턴스여야 함을 암시합니다. 바인딩 가능한 개체 및 속성에 대한 개요는 [**바인딩 가능한 속성**](~/xamarin-forms/xaml/bindable-properties.md) 문서를 참조하세요.

문자열로 지정된 원본 속성에 대해 이러한 규칙은 없습니다. 내부적으로 리플렉션은 실제 속성에 액세스하는 데 사용됩니다. 그러나 이 특별한 경우에는 `Value` 속성도 바인딩 가능한 속성으로 지원됩니다.

코드는 다소 간소화될 수 있습니다. 즉, `RotationProperty` 바인딩 가능한 속성은 `VisualElement`에서 정의되고 `Label` 및 `ContentPage`에도 상속되므로 `SetBinding` 호출에서 클래스 이름이 필요하지 않습니다.

```csharp
label.SetBinding(RotationProperty, "Value");
```

그러나 클래스 이름이 포함되면 대상 개체를 알리는 데 도움이 됩니다.

`Slider`를 조작하면 이에 따라 `Label`이 회전합니다.

[![기본 코드 바인딩](basic-bindings-images/basiccodebinding-small.png "기본 코드 바인딩")](basic-bindings-images/basiccodebinding-large.png#lightbox "기본 코드 바인딩")

**기본 Xaml 바인딩** 페이지는 XAML에서 전체 데이터 바인딩을 정의한다는 점을 제외하고는 **기본 코드 바인딩**과 동일합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicXamlBindingPage"
             Title="Basic XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

코드에서와 마찬가지로 데이터 바인딩은 대상 개체, 즉 `Label`에 설정됩니다. 두 개의 XAML 태그 확장이 포함됩니다. 이러한 확장은 중괄호 구분 기호로 즉시 인식할 수 있습니다.

- `x:Reference` 태그 확장은 원본 개체, 즉 `slider`라는 `Slider`를 참조하는 데 필요합니다.
- `Binding` 태그 확장에서 `Label`의 `Rotation` 속성을 `Slider`의 `Value` 속성에 연결합니다.

XAML 태그 확장에 대한 자세한 내용은 [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md) 문서를 참조하세요. `x:Reference` 태그 확장은 [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) 클래스에서 지원되고, `Binding`은 [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) 클래스에서 지원됩니다. XML 네임스페이스 접두사가 표시되면 `x:Reference`는 XAML 2009 사양의 일부이지만 `Binding`은 Xamin.Forms의 일부입니다. 따옴표는 중괄호 안에 표시되지 않습니다.

`BindingContext`를 설정할 때 `x:Reference` 태그 확장을 잊어버리기 쉽습니다. 다음과 같이 실수로 속성을 바인딩 원본 이름으로 직접 설정하는 것이 일반적입니다.

```xaml
BindingContext="slider"
```

하지만 이는 올바르지 않습니다. 해당 태그는 `BindingContext` 속성을 "slider" 문자로 철자되는 `string` 개체로 설정합니다!

원본 속성은 [`Binding`](xref:Xamarin.Forms.Binding) 클래스의 [`Path`](xref:Xamarin.Forms.Binding.Path) 속성에 해당하는 `BindingExtension`의 [`Path`](xref:Xamarin.Forms.Xaml.BindingExtension.Path) 속성으로 지정됩니다.

**기본 XAML 바인딩** 페이지에 표시된 태그는 간소화할 수 있습니다. 즉, `x:Reference` 및 `Binding`과 같은 XAML 태그 확장에는 ‘콘텐츠 속성’ 특성이 정의될 수 있으며, 이는 XAML 태그 확장의 경우 속성 이름을 표시할 필요가 없음을 의미합니다.  `Name` 속성은 `x:Reference`의 콘텐츠 속성이고, `Path` 속성은 `Binding`의 콘텐츠 속성입니다. 즉 다음 식에서 해당 속성을 제외할 수 있습니다.

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>바인딩 컨텍스트가 없는 바인딩

`BindingContext` 속성은 데이터 바인딩의 중요한 구성 요소이지만 항상 필요한 것은 아닙니다. 대신 원본 개체를 `SetBinding` 호출 또는 `Binding` 태그 확장에 지정할 수 있습니다.

이는 **대체 코드 바인딩** 샘플에 나와 있습니다. XAML 파일은 `Slider`가 `Label`의 `Scale` 속성을 제어하도록 정의된다는 점을 제외하고는 **기본 코드 바인딩** 샘플과 비슷합니다. 이러한 이유로 `Slider`는 &ndash;2 - 2의 범위로 설정됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeCodeBindingPage"
             Title="Alternative Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

코드 숨김 파일은 `BindableObject`에서 정의된 [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 메서드를 사용하여 바인딩을 설정합니다. 인수는 [`Binding`](xref:Xamarin.Forms.Binding) 클래스에 대한 [생성자](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object))입니다.

```csharp
public partial class AlternativeCodeBindingPage : ContentPage
{
    public AlternativeCodeBindingPage()
    {
        InitializeComponent();

        label.SetBinding(Label.ScaleProperty, new Binding("Value", source: slider));
    }
}
```

`Binding` 생성자에는 6개의 매개 변수가 있으므로 `source` 매개 변수는 명명된 인수로 지정됩니다. 인수는 `slider` 개체입니다.

이 프로그램을 실행하면 약간 놀랄 수도 있습니다.

[![대체 코드 바인딩](basic-bindings-images/alternativecodebinding-small.png "대체 코드 바인딩")](basic-bindings-images/alternativecodebinding-large.png#lightbox "대체 코드 바인딩")

왼쪽의 iOS 화면에서는 페이지가 처음 나타날 때 화면이 표시되는 모양을 보여 줍니다. `Label`은 어디에 있을까요?

문제는 `Slider`의 초기 값이 0이라는 것입니다. 이로 인해 `Label`의 `Scale` 속성도 0으로 설정되어 기본값인 1을 재정의합니다. 따라서 초기에는 `Label`이 표시되지 않습니다. Android 스크린샷에서 보여 주듯이 `Slider`을(를) 조작하여 `Label`이(가) 다시 나타나도록 할 수 있지만, 초기에 표시되지 않으면 당황스러울 수 있습니다.

[다음 문서](binding-mode.md)에서 `Scale` 속성의 기본값에서 `Slider`를 초기화하여 이 문제를 방지하는 방법을 알아봅니다.

> [!NOTE]
> 또한 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 클래스는 `VisualElement`의 크기를 가로 및 세로 방향으로 다르게 확장할 수 있는 [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) 및 [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) 속성을 정의합니다.

**대체 XAML 바인딩** 페이지에는 XAML에서 완전히 동일한 바인딩이 표시됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeXamlBindingPage"
             Title="Alternative XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="{Binding Source={x:Reference slider},
                               Path=Value}" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

이제 `Binding` 태그 확장에는 쉼표로 구분된 두 개의 속성 세트인 `Source`와 `Path`가 있습니다. 원하는 경우 동일한 줄에 나타날 수 있습니다.

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source` 속성은 포함된 `x:Reference` 태그 확장으로 설정되며, 그렇지 않으면 `BindingContext`를 설정하는 것과 동일한 구문이 포함됩니다. 중괄호 안에 따옴표가 표시되지 않으며, 두 속성은 쉼표로 구분해야 합니다.

`Binding` 태그 확장의 콘텐츠 속성은 `Path`이지만, 태그 확장의 `Path=` 부분은 식의 첫 번째 속성인 경우에만 제거할 수 있습니다. `Path=` 부분을 제거하려면 다음 두 속성을 교환해야 합니다.

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

XAML 태그 확장은 일반적으로 중괄호로 구분되지만, 개체 요소로도 표현할 수 있습니다.

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Source="{x:Reference slider}"
                 Path="Value" />
    </Label.Scale>
</Label>
```

이제 `Source` 및 `Path` 속성은 일반 XAML 특성입니다. 값이 따옴표 안에 표시되고, 특성은 쉼표로 구분되지 않습니다. 또한 `x:Reference` 태그 확장은 개체 요소가 될 수 있습니다.

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Path="Value">
            <Binding.Source>
                <x:Reference Name="slider" />
            </Binding.Source>
        </Binding>
    </Label.Scale>
</Label>
```

이 구문은 일반적이지 않지만, 때때로 복잡한 개체가 관련되는 경우에는 필요합니다.

지금까지 표시된 예제에서는 `BindingContext` 속성과 `Binding`의 `Source` 속성을 `x:Reference` 태그 확장으로 설정하여 페이지의 다른 보기를 참조했습니다. 이 두 속성은 `Object` 형식이며, 바인딩 원본에 적합한 속성이 포함된 개체로 설정할 수 있습니다.

앞서의 문서에서 `BindingContext` 또는 `Source` 속성은 정적 속성 또는 필드의 값을 참조하는 `x:Static` 태그 확장, 리소스 사전에 저장된 개체를 참조하는 `StaticResource` 태그 확장으로 설정하거나, 일반적으로(항상은 아님) ViewModel의 인스턴스인 개체로 직접 설정할 수 있음을 알 수 있습니다.

또한 `BindingContext` 속성을 `Binding` 개체로 설정하여 `Binding`의 `Source` 및 `Path` 속성에서 바인딩 컨텍스트를 정의할 수도 있습니다.

## <a name="binding-context-inheritance"></a>바인딩 컨텍스트 상속

이 문서에서는 `BindingContext` 속성 또는 `Binding` 개체의 `Source` 속성을 사용하여 원본 개체를 지정할 수 있음을 살펴보았습니다. 둘 다 설정되는 경우 `Binding`의 `Source` 속성이 `BindingContext`보다 우선합니다.

`BindingContext` 속성에는 다음과 같은 매우 중요한 특징이 있습니다.

*`BindingContext` 속성의 설정은 시각적 트리를 통해 상속됩니다.*

보다시피, 이 방법은 바인딩 식을 간소화하는 데 매우 유용할 수 있으며, 경우에 따라, 특히 MVVM(Model-View-ViewModel) 시나리오에서는 필수적입니다.

**바인딩 컨텍스트 상속** 샘플은 바인딩 컨텍스트의 상속에 대한 간단한 데모입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BindingContextInheritancePage"
             Title="BindingContext Inheritance">
    <StackLayout Padding="10">

        <StackLayout VerticalOptions="FillAndExpand"
                     BindingContext="{x:Reference slider}">

            <Label Text="TEXT"
                   FontSize="80"
                   HorizontalOptions="Center"
                   VerticalOptions="EndAndExpand"
                   Rotation="{Binding Value}" />

            <BoxView Color="#800000FF"
                     WidthRequest="180"
                     HeightRequest="40"
                     HorizontalOptions="Center"
                     VerticalOptions="StartAndExpand"
                     Rotation="{Binding Value}" />
        </StackLayout>

        <Slider x:Name="slider"
                Maximum="360" />

    </StackLayout>
</ContentPage>
```

`StackLayout`의 `BindingContext` 속성은 `slider` 개체로 설정됩니다. 이 바인딩 컨텍스트는 `Label`과 `BoxView` 모두에서 상속되며, 둘 모두의 `Rotation` 속성이 `Slider`의 `Value` 속성으로 설정됩니다.

[![바인딩 컨텍스트 상속](basic-bindings-images/bindingcontextinheritance-small.png "바인딩 컨텍스트 상속")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "바인딩 컨텍스트 상속")

[다음 문서](binding-mode.md)에서는 *바인딩 모드*에서 대상 개체와 원본 개체 간의 데이터 흐름을 변경하는 방법을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Xamarin.Forms 서적의 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Data-Binding/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
