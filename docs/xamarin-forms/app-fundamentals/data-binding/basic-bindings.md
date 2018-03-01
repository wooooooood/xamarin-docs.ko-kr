---
title: "기본 바인딩"
description: "데이터 바인딩 대상, 원본 및 바인딩 컨텍스트"
ms.topic: article
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 500ad02d79cea79f59b1aca91b0312c9a9d6bac3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="basic-bindings"></a>기본 바인딩

Xamarin.Forms 데이터 바인딩 중에 사용자 인터페이스 개체는 일반적으로 두 개체 간의 속성 쌍을 연결 합니다. 이 두 개체 라고는 *대상* 및 *소스*:

- *대상* 데이터 바인딩이 설정 된 개체 (및 속성)가 있습니다.
- *소스* 데이터 바인딩에서 참조 된 개체 (및 속성).

이러한 구분이 약간 복잡할 수: 가장 간단한 경우 데이터 흐름 원본에서 대상에 대상 속성의 값이 source 속성의 값에서 설정 되어 있는 것을 의미 합니다. 그러나 경우에 따라 데이터 또는 이동 대상에서 원본으로 또는 양방향 합니다. 혼동을 피하기 위해 대상 데이터를 제공 하는 대신 되며 데이터를 받는 경우에 데이터 바인딩이 설정 된 개체는 항상 염두에서에 둬야 합니다.

## <a name="bindings-with-a-binding-context"></a>바인딩 컨텍스트를 사용 하는 바인딩

데이터 바인딩이 XAML을 일반적으로 지정, 이지만 코드에서 데이터 바인딩을 참조 하는 방법도 있습니다. **기본 코드 바인딩** 페이지와 XAML 파일에는 `Label` 및 `Slider`:

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

`Slider` 범위 0-360에 대해 설정 됩니다. 이 프로그램의 의도 회전 하는 것은 `Label` 조작 하 여는 `Slider`합니다.

없이 데이터 바인딩을 설정는 `ValueChanged` 의 이벤트는 `Slider` 에 액세스 하는 이벤트 처리기에는 `Value` 의 속성은 `Slider` 해당 값을 설정 하 고는 `Rotation` 속성의는 `Label`합니다. 작업을 자동화 하는 데이터 바인딩 이벤트 처리기 및 내의 코드는 더 이상 필요 합니다. 

파생 된 클래스의 인스턴스에 대 한 바인딩을 설정할 수 있습니다 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)를 포함 하는 `Element`, `VisualElement`, `View`, 및 `View` 파생 항목입니다.  바인딩 대상 개체에 항상 설정 됩니다. 바인딩 소스 개체를 참조합니다. 데이터 바인딩을 설정 하려면 대상 클래스의 다음 두 명의 멤버를 사용 합니다.

- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 속성 소스 개체를 지정 합니다.
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) 메서드 대상 속성 및 source 속성을 지정 합니다.

이 예제는 `Label` 은 바인딩 대상 및 `Slider` 바인딩 소스인 합니다. 변경 된 `Slider` 의 회전 하는 소스에 영향을 줄는 `Label` 대상입니다. 데이터 원본에서 대상으로 진행 합니다.

`SetBinding` 정의한 메서드 `BindableObject` 형식의 인수에 [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) 입니다는 [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) 클래스 파생 되지만 다른 `SetBinding` 메서드 정의한는 [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) 클래스입니다. 코드 숨김 파일에는 **기본 코드 바인딩** 샘플에서는 사용 하면 더 간단한 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) 이 클래스에서 확장 메서드.

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

`Label` 개체가 바인딩 대상이이 속성이 설정 되어와 메서드가 호출 되는 개체입니다. `BindingContext` 속성 표시 되는 바인딩 소스에서 `Slider`합니다.

`SetBinding` 메서드는 바인딩 대상에서 호출 되지만 대상 속성 및 source 속성을 모두 지정 합니다. 대상 속성으로 지정 되는 `BindableProperty` 개체: `Label.RotationProperty`합니다. Source 속성을 문자열로 지정 된 문서를 나타내고는 `Value` 속성 `Slider`합니다. 

`SetBinding` 규칙 중 하나는 가장 중요 한 데이터 바인딩의 메서드 표시:

*대상 속성 바인딩 가능한 속성으로 지원 되어야 합니다.*

이 규칙은 대상 개체에서 파생 되는 클래스의 인스턴스를 되도록 의미 `BindableObject`합니다. 참조는 [ **바인딩 가능 속성** ](~/xamarin-forms/xaml/bindable-properties.md) 바인딩 가능한 개체 및 바인딩 가능한 속성에 대 한 개요 문서.

그러한는 문자열로 지정 된 원본 속성에 대 한 규칙이 있습니다. 내부적으로 리플렉션은 실제 속성에 액세스 하는 데 사용 됩니다. 그러나 경우에 따라,는 `Value` 속성 바인딩 가능한 속성에 의해도 지원 됩니다.

코드 수 있습니다 간소화 비교적:는 `RotationProperty` 바인딩 가능한 속성으로 정의 된 `VisualElement`, 상속 및 `Label` 및 `ContentPage` 클래스 이름에 필요 하지 않습니다는 하므로도 `SetBinding` 호출:

```csharp
label.SetBinding(RotationProperty, "Value");
```

그러나 클래스 이름을 포함 하 여 대상 개체의 좋은 미리 알림입니다.

조작할 때는 `Slider`, `Label` 적절 하 게 회전 합니다.

[![바인딩 Basice 코드](basic-bindings-images/basiccodebinding-small.png "기본 코드 바인딩")](basic-bindings-images/basiccodebinding-large.png "기본 코드 바인딩")

**기본 Xaml 바인딩** 페이지는 동일 **기본 코드 바인딩** 제외 하 고 XAML에서 전체 데이터 바인딩이 정의:

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

코드에서와 마찬가지로 데이터 바인딩은 대상 개체에 설정 됩니다는 `Label`합니다. 두 XAML 태그 확장 관련 됩니다. 다음은 중괄호로 묶습니다 즉시 인식할 수입니다.

- `x:Reference` 태그 확장이 원본 개체를 참조 하는 데 필요는 `Slider` 라는 `slider`합니다.
- `Binding` 태그 확장 링크는 `Rotation` 속성의는 `Label` 에 `Value` 의 속성은 `Slider`합니다. 

문서 참조 [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md) XAML 태그 확장에 대 한 자세한 내용은 합니다. `x:Reference` 태그 확장으로 사용할 수는 [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) 클래스입니다. `Binding` 으로 사용할 수는 [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) 클래스입니다. Xml 네임 스페이스 접두사 결과, `x:Reference` XAML 2009 사양의 일부가 동안 `Binding` Xamarin.Forms의 일부입니다. 인용 부호 제외 중괄호 안에 표시 되는지 확인 합니다.

쉽게 잊기는 `x:Reference` 태그 확장을 설정할 때의 `BindingContext`합니다. 일반적으로 다음과 같은 바인딩 소스의 이름에 직접 속성을 잘못 설정은:

```xaml
BindingContext="slider"
```

하지만 그렇지 않습니다. 설정 하는 태그는 `BindingContext` 속성을는 `string` 문자로 spell "slider" 개체!

Source 속성으로 지정 된 통지는 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) 속성의 `BindingExtension`, 해당 하는 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) 속성의는 [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) 클래스입니다.

에 표시 된 태그는 **기본 XAML 바인딩** 페이지를 간소화할 수 있습니다:와 같은 XAML 태그 확장 `x:Reference` 및 `Binding` 가질 수 있습니다 *contentproperty* XAML에 대해 특성 정의 태그 확장 속성 이름 표시 필요 하지 않은 것을 의미 합니다. `Name` 속성의 콘텐츠 속성은 `x:Reference`, 및 `Path` 속성의 콘텐츠 속성은 `Binding`, 즉, 식에서 제거할 수:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>바인딩 컨텍스트 없이 바인딩

`BindingContext` 속성은 데이터 바인딩의 중요 한 요소 이지만 항상 필요는 없습니다. 에 소스 개체 대신 지정할 수는 `SetBinding` 호출 또는 `Binding` 태그 확장 합니다.

이 확인할는 **대체 코드 바인딩** 샘플. XAML 파일은 유사는 **기본 코드 바인딩** 점을 제외 하 고 샘플에서 `Slider` 제어에 정의 된는 `Scale` 속성의는 `Label`합니다. 이런 이유로 `Slider` 의 범위에 대 한 설정 &ndash;2 ~ 2:

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

사용 하 여 바인딩을 설정 하는 코드 숨김 파일에서 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) 정의한 메서드 `BindableObject`합니다. 인수는는 [생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) 에 대 한는 [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) 클래스:

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

`Binding` 생성자 6 개이고 하므로 `source` 는 명명 된 인수를 가진 매개 변수를 지정 합니다. 인수는는 `slider` 개체입니다. 

이 프로그램을 실행 하는 것은 놀라운 약간 수 있습니다.

[![대체 코드 바인딩](basic-bindings-images/alternativecodebinding-small.png "대체 코드 바인딩")](basic-bindings-images/alternativecodebinding-large.png "대체 코드 바인딩")

IOS 화면 왼쪽에는 페이지가 처음 나타날 때 화면 표시 되는 모양을 보여 줍니다. 여기서는 `Label`? 

문제는 `Slider` 의 초기 값은 0입니다. 이 인해는 `Scale` 의 속성은 `Label` 0, 1의 기본값을 재정의로 설정 해야 합니다. 이 인해는 `Label` 되 처음에 표시 되 고 있습니다. Android 및 유니버설 Windows 플랫폼 (UWP) 스크린 샷을 보여주듯이 조작할 수 있습니다는 `Slider` 있도록는 `Label` 다시 나타나지만 해당 초기 표시 안 함 게 혼란을 줄 됩니다.

[다음 기사](binding-mode.md) 초기화 하 여이 문제를 방지 하는 방법의 `Slider` 의 기본값에서는 `Scale` 속성입니다.

**대신 XAML 바인딩** 페이지에 해당 하는 바인딩 XAML을 표시 합니다.

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

이제는 `Binding` 태그 확장을 설정 하는 두 가지 속성은 `Source` 및 `Path`을 쉼표로 구분 합니다. 원하는 경우 같은 줄에 나타날 수 있습니다.

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source` 속성에 포함 된 `x:Reference` 그렇지 않은 경우는 구문이 있는 동일한 설정으로 태그 확장의 `BindingContext`합니다. 인용 부호 제외에 오는 중괄호 안에 표시 되는지 두 속성은 쉼표로 구분 된 해야 살펴보세요.

콘텐츠 속성이 `Binding` 태그 확장은 `Path`, 하지만 `Path=` 태그 확장의 일부가 식에서 첫 번째 속성 경우에 제거할 수 있습니다. 제거 하는 `Path=` 두 속성은 교환 해야 하는 파트:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

일반적으로 XAML 태그 확장 중괄호로 구분, 있지만 표시할 수 있습니다 개체 요소로:

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

이제는 `Source` 및 `Path` 속성은 일반 XAML 특성: 값이 따옴표로 표시 및 특성은 쉼표로 분리 되지 않습니다. `x:Reference` 태그 확장 object 요소 메시지가 될 수도 있습니다.

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

이 구문은 일반적이 지 않지만 경우에 따라 필요한 경우 관련 된 복잡 한 개체 합니다.

앞에서 설명한 예제 설정는 `BindingContext` 속성 및 `Source` 속성 `Binding` 에 `x:Reference` 태그 확장을 페이지에서 다른 뷰를 참조 합니다. 이러한 속성은 두 가지 유형의 `Object`를 바인딩 원본에 적합 한 속성을 포함 하는 개체에 설정할 수 있습니다. 

문서에 계속, 배워 설정할 수 있습니다는 `BindingContext` 또는 `Source` 속성을는 `x:Static` 정적 속성 또는 필드의 값을 참조할 되도록 태그 확장 또는 `StaticResource` 태그 확장에 저장 된 개체를 참조 하는 리소스 사전 또는 직접 개체에는 일반적으로 (항상 그렇지는 않지만)는 ViewModel의 인스턴스입니다. 

`BindingContext` 속성으로 설정할 수도 있습니다는 `Binding` 개체 있도록는 `Source` 및 `Path` 의 속성 `Binding` 바인딩 컨텍스트를 정의 합니다.

## <a name="binding-context-inheritance"></a>바인딩 컨텍스트 상속

이 문서에서는 살펴 보았으며 사용 하 여 원본 개체를 지정할 수는 `BindingContext` 속성 또는 `Source` 의 속성은 `Binding` 개체입니다. 모두 설정 하는 경우는 `Source` 의 속성은 `Binding` 우선는 `BindingContext`합니다.

`BindingContext` 속성에는 매우 중요 한 특징:

*설정에서 `BindingContext` 속성이 시각적 트리를 통해 상속 됩니다.*

알 수 있듯이이 매우 유용할 수 단순화 바인딩 식 및 경우에 따라 &mdash; 모델-뷰-MVVM () 시나리오에서 특히 &mdash; 것이 중요 합니다.

**바인딩 컨텍스트 상속** 샘플은 바인딩 컨텍스트에 상속의 간단한 데모:

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

`BindingContext` 속성은 `StackLayout` 로 설정 되는 `slider` 개체입니다. 이 바인딩 컨텍스트가 모두에 의해 상속 되는 `Label` 및 `BoxView`모두 충족 하는의 해당 `Rotation` 속성으로 설정는 `Value` 속성의는 `Slider`: 

[![바인딩 컨텍스트 상속](basic-bindings-images/bindingcontextinheritance-small.png "컨텍스트 상속 바인딩")](basic-bindings-images/bindingcontextinheritance-large.png "컨텍스트 상속 바인딩")

에 [다음 기사](binding-mode.md), 표시 됩니다는 방법을 *바인딩 모드* 개체 대상과 원본 간의 데이터 흐름을 변경할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
