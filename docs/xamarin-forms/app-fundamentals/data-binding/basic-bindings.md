---
title: Xamarin.Forms 기본 바인딩
description: 이 문서에서는 Xamarin.Forms 데이터 바인딩, 적어도 한 쌍의 두 개체 간의 속성 링크는 그 중 하나는 일반적으로 사용자 인터페이스 개체를 사용 하는 방법을 설명 합니다. 이러한 두 개체는 원본과 대상 이라고 합니다.
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: e31cba5c61624b0bca03443262b95d7497564750
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675200"
---
# <a name="xamarinforms-basic-bindings"></a>Xamarin.Forms 기본 바인딩

데이터 바인딩을 Xamarin.Forms는 하나 이상의 사용자 인터페이스 개체는 일반적으로 두 개체 사이의 속성 쌍을 연결 합니다. 이 두 개체 라고 합니다 *대상* 하며 *원본*:

- 합니다 *대상* 데이터 바인딩이 설정 된 개체 (및 속성) 됩니다.
- 합니다 *원본* 데이터 바인딩에서 참조 된 개체 (및 속성).

이러한 차이점으로이 인해 혼동이 발생할 수: 가장 간단한 경우, 데이터 흐름 원본에서 대상에 대상 속성의 값은 소스 속성의 값에서 설정 되어 있는지를 의미 합니다. 그러나 일부 경우에 데이터 전달 될 수 있습니다 또는 대상에서 원본 또는 양방향. 혼동을 피하기 위해 대상 된 개체 데이터를 제공 하는 대신 되며 데이터를 수신 하는 경우에 데이터 바인딩이 설정 됩니다는 항상 염두에서에 둡니다.

## <a name="bindings-with-a-binding-context"></a>바인딩 컨텍스트를 사용 하 여 바인딩

데이터 바인딩에 일반적으로 완전히 XAML에 지정 이지만 보이는지 코드의 데이터 바인딩에 있습니다. 합니다 **기본 코드 바인딩** 사용 하 여 XAML 파일을 포함 하는 페이지는 `Label` 및 `Slider`:

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

`Slider` 0-360 범위에 대해 설정 됩니다. 이 프로그램의 의도를 회전 시키는 것은 `Label` 조작 하 여는 `Slider`합니다.

데이터 바인딩 없이 설정할 수 있습니다는 `ValueChanged` 의 이벤트를 `Slider` 에 액세스 하는 이벤트 처리기에는 `Value` 의 속성을 `Slider` 고 해당 값을 설정를 `Rotation` 속성을 `Label`. 해당 작업을 자동화 하는 데이터 바인딩 이벤트 처리기 내 코드와 더 이상 필요 합니다.

파생 된 클래스의 인스턴스에서 바인딩을 설정할 수 있습니다 [ `BindableObject` ](xref:Xamarin.Forms.BindableObject)를 포함 하는 `Element`, `VisualElement`를 `View`, 및 `View` 파생형입니다.  바인딩 대상 개체에 항상 설정 됩니다. 바인딩 소스 개체를 참조합니다. 데이터 바인딩을 설정 하려면 대상 클래스의 다음 두 명의 멤버를 사용 합니다.

- 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 속성 원본 개체를 지정 합니다.
- 합니다 [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 메서드 대상 속성과 소스 속성을 지정 합니다.

이 예제는 `Label` 바인딩 대상 및 `Slider` 은 바인딩 소스입니다. 변경 합니다 `Slider` 회전 하는 원본에 영향을 줄를 `Label` 대상입니다. 데이터 원본에서 대상으로 이동합니다.

합니다 `SetBinding` 정의한 메서드 `BindableObject` 형식의 인수를 사용 [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) 올 합니다 [ `Binding` ](xref:Xamarin.Forms.Binding) 클래스 파생 하지만 다른 `SetBinding` 메서드 정의한 합니다 [ `BindableObjectExtensions` ](xref:Xamarin.Forms.BindableObjectExtensions) 클래스입니다. 코드 숨김 파일에는 **기본 코드 바인딩** 샘플에서는 더욱 간단 [ `SetBinding` ](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) 이 클래스에서 확장 메서드.

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

`Label` 개체 이므로 바인딩 대상에서이 속성을 설정 및 메서드를 호출 하는 개체입니다. 합니다 `BindingContext` 속성에는 바인딩 소스를 나타냅니다는 `Slider`합니다.

`SetBinding` 메서드는 바인딩 대상에서 호출 되지만 대상 속성과 소스 속성이 모두 지정 합니다. 대상 속성으로 지정 되는 `BindableProperty` 개체: `Label.RotationProperty`합니다. Source 속성을 문자열로 지정 된 문서를 나타내고 합니다 `Value` 속성의 `Slider`합니다.

`SetBinding` 메서드 데이터 바인딩의 가장 중요 한 규칙 중 하나를 보여 줍니다.

*대상 속성 바인딩 가능한 속성으로 백업 해야 합니다.*

이 규칙은 대상 개체에서 파생 된 클래스의 인스턴스를 되도록 의미 `BindableObject`합니다. 참조 된 [ **바인딩 가능한 속성** ](~/xamarin-forms/xaml/bindable-properties.md) 바인딩할 수 있는 개체 및 바인딩 가능한 속성에 대 한 개요 문서입니다.

문자열로 지정 된 원본 속성에 대 한 이러한 규칙이 없는 경우 내부적으로 리플렉션은 실제 속성에 액세스 하는 데 사용 됩니다. 그러나이 경우,는 `Value` 속성은 또한 바인딩 가능한 속성으로 지원 됩니다.

코드 수 약간 간소화: 합니다 `RotationProperty` 바인딩 가능한 속성으로 정의 됩니다 `VisualElement`, 상속 `Label` 및 `ContentPage` 클래스 이름에 필요 하지 않습니다 하므로 에서도 `SetBinding` 호출:

```csharp
label.SetBinding(RotationProperty, "Value");
```

그러나 클래스 이름을 포함 하 여 대상 개체의 좋은 미리 알림입니다.

조작 하면 합니다 `Slider`, `Label` 그에 따라 회전 합니다.

[![기본 코드 바인딩](basic-bindings-images/basiccodebinding-small.png "기본 코드 바인딩")](basic-bindings-images/basiccodebinding-large.png#lightbox "기본 코드 바인딩")

**기본 Xaml 바인딩** 페이지는 동일 **기본 코드 바인딩** 제외 하 고 전체 데이터 바인딩은 XAML에서 정의 합니다.

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

데이터 바인딩 상태인 대상 개체에 설정 되어 코드에서와 마찬가지로 `Label`합니다. 두 XAML 태그 확장에 포함 됩니다. 다음은 바로 알아볼 중괄호로 묶습니다.

- `x:Reference` 태그 확장은 원본 개체를 참조 해야 합니다 `Slider` 라는 `slider`합니다.
- `Binding` 태그 확장 링크는 `Rotation` 의 속성을 `Label` 에 `Value` 속성은 `Slider`.

문서를 참조 하세요 [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md) XAML 태그 확장에 대 한 자세한 내용은 합니다. 합니다 `x:Reference` 태그 확장에서 지원 되는 [ `ReferenceExtension` ](xref:Xamarin.Forms.Xaml.ReferenceExtension) 클래스 `Binding` 되는 [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension) 클래스입니다. Xml 네임 스페이스 접두사 나타낼 `x:Reference` XAML 2009 사양의 일부가 동안 `Binding` Xamarin.Forms의 일부인 합니다. 중괄호 안의 하지 인용 부호 표시 되는지 확인 합니다.

잊기 쉽습니다 합니다 `x:Reference` 태그 확장 설정 하는 경우는 `BindingContext`합니다. 일반적인 실수로 같이 바인딩 원본의 이름에 직접 속성을 설정 하는 것:

```xaml
BindingContext="slider"
```

하지만 그렇지 않습니다. 해당 태그를 설정 합니다 `BindingContext` 속성을는 `string` 문자가 맞춤법 "slider" 개체!

원본 속성에 지정 되는 [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path) 의 속성 `BindingExtension`, 해당 하는 [ `Path` ](xref:Xamarin.Forms.Binding.Path) 속성을 [ `Binding` ](xref:Xamarin.Forms.Binding) 클래스입니다.

에 표시 된 태그를 **기본 XAML 바인딩** 페이지를 단순화할 수 있습니다.:와 같은 XAML 태그 확장 `x:Reference` 하 고 `Binding` 있을 수 있습니다 *콘텐츠 속성* XAML에 대해 특성 정의 태그 확장 속성 이름을 표시할 필요가 있는지를 의미 합니다. `Name` 속성의 콘텐츠 속성인 `x:Reference`, 및 `Path` 속성의 콘텐츠 속성은 `Binding`, 즉, 식에서 제거할 수:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>바인딩 컨텍스트 없이 바인딩

`BindingContext` 속성은 데이터 바인딩의 중요 한 요소가 이지만 항상 필요는 없습니다. 원본 개체에서 대신 지정할 수 있습니다 합니다 `SetBinding` 호출 또는 `Binding` 태그 확장 합니다.

에 설명 되어이 **대체 코드 바인딩** 샘플입니다. XAML 파일은 유사 합니다 **기본 코드 바인딩** 는 제외 하 고 샘플를 `Slider` 컨트롤에 정의 된를 `Scale` 의 속성은 `Label`. 이런 이유로 합니다 `Slider` 범위에 대해 설정 된 &ndash;2에서 2:

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

코드 숨김 파일을 사용 하 여 바인딩을 설정 합니다 [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 메서드를 정의한 `BindableObject`합니다. 인수가 [생성자](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) 에 대 한 합니다 [ `Binding` ](xref:Xamarin.Forms.Binding) 클래스:

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

합니다 `Binding` 생성자에 매개 변수가 6 하므로 `source` 명명된 된 인수를 사용 하 여 매개 변수를 지정 합니다. 인수가 `slider` 개체입니다.

이 프로그램을 실행 하는 것은 다소 놀라울 수 있습니다.

[![대체 코드 바인딩](basic-bindings-images/alternativecodebinding-small.png "대체 코드 바인딩")](basic-bindings-images/alternativecodebinding-large.png#lightbox "대체 코드 바인딩")

IOS 화면 왼쪽에는 페이지가 처음 나타날 때 화면 표시 되는 모양을 보여 줍니다. 여기서는 `Label`?

문제는는 `Slider` 의 초기 값은 0입니다. 이 인해를 `Scale` 의 속성은 `Label` 0, 1의 기본값으로 재정의로 설정 해야 합니다. 이 인해는 `Label` 가 처음에 표시 합니다. 스크린샷에서 Android 및 유니버설 Windows 플랫폼 (UWP)에서 알 수 있듯이 조작할 수 있습니다 합니다 `Slider` 있도록는 `Label` 다시 표시 되지만 해당 초기 사라지는 혼란을 줄.

수를 [다음 문서](binding-mode.md) 초기화 하 여이 문제를 방지 하는 방법의 `Slider` 의 기본 값에서를 `Scale` 속성.

> [!NOTE]
> [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 클래스도 정의 [ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX) 및 [ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY) 확장 될 수 있는 속성을 `VisualElement` 에서 다르게는 가로 및 세로 방향입니다.

합니다 **대신 XAML 바인딩** XAML의 완전히 동일한 바인딩을 보여 줍니다.

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

이제는 `Binding` 태그 확장에는 두 가지 속성을 설정 `Source` 고 `Path`쉼표로 구분 합니다. 원하는 경우 동일한 줄에 나타날 수 있습니다.

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source` 속성은 포함 된 `x:Reference` 설정으로 동일한 구문을 포함 하는 태그 확장의 `BindingContext`. 따옴표, 중괄호 안의 나타나고 두 속성은 쉼표로 구분 해야 합니다는 알 수 있습니다.

콘텐츠 속성을 `Binding` 태그 확장은 `Path`, 하지만 `Path=` 첫 번째 속성 식의 경우에 태그 확장의 파트를 제거할 수 있습니다. 제거 하는 `Path=` 두 속성은 교환 해야 하는 부분:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

하지만 XAML 태그 확장은 중괄호로 묶어 구분 일반적으로 표시할 수 있습니다 개체 요소로:

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

이제는 `Source` 고 `Path` 속성은 일반 XAML 특성: 값이 따옴표로 표시 하 고 특성에서 쉼표 구분 되지 않은 합니다. `x:Reference` 태그 확장 개체 요소 될 수도 있습니다.

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

이 구문은 일반적인 없지만 경우가 필요한 복잡 한 개체와 관련 된 경우.

지금 나와 있는 예제 설정 합니다 `BindingContext` 속성 및 `Source` 의 속성 `Binding` 에 `x:Reference` 참조 페이지의 다른 보기에 태그 확장 합니다. 이러한 속성은 두 가지 형식의 `Object`를 바인딩 원본에 대 한 적합 한 속성을 포함 하는 모든 개체에 설정할 수 있습니다.

계속 해 서 문서에서는 알게 설정할 수 있습니다 합니다 `BindingContext` 또는 `Source` 속성을는 `x:Static` 태그 확장 참조는 정적 속성 또는 필드의 값을 또는 `StaticResource` 태그 확장에 저장 된 개체를 참조 하는 리소스 사전을 직접 개체에 일반적으로 (항상 그렇지는 않지만) 또는 ViewModel의 인스턴스.

`BindingContext` 속성 설정할 수도 있습니다는 `Binding` 개체 있도록를 `Source` 및 `Path` 속성의 `Binding` 바인딩 컨텍스트를 정의 합니다.

## <a name="binding-context-inheritance"></a>바인딩 컨텍스트 상속

이 문서에서는 지금까지 살펴본 사용 하 여 원본 개체를 지정할 수는 `BindingContext` 속성 또는 `Source` 의 속성을 `Binding` 개체입니다. 모두 설정 된 경우는 `Source` 의 속성을 `Binding` 우선를 `BindingContext`.

`BindingContext` 속성이 매우 중요 한 특징:

*설정 된 `BindingContext` 속성은 시각적 트리를 통해 상속 됩니다.*

알 수 있듯이이 매우 유용할 수 바인딩 식 단순화 한 경우도 &mdash; -Model-view-viewmodel (MVVM) 시나리오에서 특히 &mdash; 것이 중요 합니다.

합니다 **바인딩 컨텍스트 상속** 샘플은 바인딩 컨텍스트의 상속의 간단한 데모:

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

`BindingContext` 의 속성을 `StackLayout` 로 설정 되어를 `slider` 개체입니다. 이 바인딩 컨텍스트에 둘 다에서 상속 됩니다는 `Label` 및 `BoxView`모두 있는의 해당 `Rotation` 속성으로 설정 합니다 `Value` 속성의는 `Slider`:

[![상황에 맞는 상속 바인딩](basic-bindings-images/bindingcontextinheritance-small.png "상황에 맞는 상속 바인딩")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "상황에 맞는 상속 바인딩")

에 [다음 문서](binding-mode.md), 표시 하는 방법을 *바인딩 모드* 대상 및 소스 개체 간의 데이터 흐름을 변경할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
