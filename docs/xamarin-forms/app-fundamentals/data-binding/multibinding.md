---
title: Xamarin.Forms 다중 바인딩
description: 이 문서에서는 MultiBinding 클래스를 사용하여 Binding 개체 컬렉션을 단일 바인딩 대상 속성에 연결하는 방법에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: E73AE622-664C-4A90-B5B2-BD47D0E7A1A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2020
ms.openlocfilehash: dfe6da8a76b447bf0c2a6c0a3bea9823e498d5e4
ms.sourcegitcommit: 8a18471b3d96f3f726b66f9bc50a829f1c122f29
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84988186"
---
# <a name="xamarinforms-multi-bindings"></a>Xamarin.Forms 다중 바인딩

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

다중 바인딩으로 [`Binding`](xref:Xamarin.Forms.Binding) 개체 컬렉션을 단일 바인딩 대상 속성에 연결할 수 있습니다. `MultiBinding` 클래스로 생성되며, 이 클래스는 모든 `Binding` 개체를 평가하고, 애플리케이션에서 제공된 `IMultiValueConverter` 인스턴스를 통해 단일 값을 반환합니다. 또한, 바인딩된 데이터 중 변경되는 것이 있을 경우 `MultiBinding`는 모든 `Binding` 개체를 다시 평가합니다.

`MultiBinding` 클래스는 다음과 같은 속성을 정의합니다.

- 형식 `IList<BindingBase>`의 `Bindings`, `MultiBinding` 인스턴스 내부의 [`Binding`](xref:Xamarin.Forms.Binding) 개체 컬렉션을 나타냅니다.
- 형식 `IMultiValueConverter`의 `Converter`, 소스 값을 대상 값으로 변환하거나 그 반대로 변환하는 데 사용할 변환기를 나타냅니다.
- 형식 `object`의 `ConverterParameter`, `Converter`에 전달한 선택적 매개 변수를 나타냅니다.

`Bindings` 속성은 `MultiBinding` 클래스의 콘텐츠 속성이므로 XAML에서 명시적으로 설정할 필요가 없습니다.

또한 `MultiBinding` 클래스는 `BindingBase` 클래스에서 다음 속성을 상속합니다.

- 형식 `object`의 `FallbackValue`는 다중 바인딩이 값을 반환할 수 없을 때 사용하는 값을 나타냅니다.
- 형식 [`BindingMode`](xref:Xamarin.Forms.BindingMode)의 `Mode`는 다중 바인딩 데이터 흐름의 방향을 나타냅니다.
- 형식 `string`의 `StringFormat`은 문자열로 표시될 경우 다중 바인딩 결과의 서식 지정 방식을 지정합니다.
- 형식 `object`의 `TargetNullValue`는 소스 값이 `null`일 경우 대상에서 사용할 값을 나타냅니다.

`MultiBinding`에서는 `Bindings` 컬렉션의 바인딩 값을 기준으로 바인딩 대상의 값을 생성하는 데 `IMultiValueConverter`를 사용해야 합니다. 예를 들어 빨간색, 파란색 및 녹색 값을 기준으로 [`Color`](xref:Xamarin.Forms.Color)를 계산할 수 있고 이러한 값은 같거나 다른 바인딩 소스 개체의 값일 수 있습니다. 값이 대상에서 소스로 이동할 경우, 대상 속성 값은 바인딩으로 다시 공급되는 값 집합으로 변환됩니다.

> [!IMPORTANT]
> `Bindings` 컬렉션의 개별 바인딩에는 고유한 값 변환기가 있을 수 있습니다.

`Mode` 속성 값은 `MultiBinding`의 기능을 결정하며 개별 바인딩이 속성을 재정의하지 않는 한, 컬렉션에 있는 모든 바인딩에 대해 바인딩 모드로 사용됩니다. 예를 들어 `MultiBinding` 개체의 `Mode` 속성이 [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)로 설정되었다면 바인딩 중 하나에 다른 `Mode` 값이 명시적으로 설정되지 않는 한 컬렉션의 모든 바인딩은 `TwoWay`로 간주됩니다.

## <a name="define-a-imultivalueconverter"></a>IMultiValueConverter 정의

`IMultiValueConverter` 인터페이스를 사용하면 사용자 지정 로직을 `MultiBinding`에 적용할 수 있습니다. 변환기를 `MultiBinding`과 연결하려면 `IValueConverter` 인터페이스를 구현하는 클래스를 만든 다음 `Convert` 및 `ConvertBack` 메서드를 구현합니다.

```csharp
public class AllTrueMultiConverter : IMultiValueConverter
{
    public object Convert(object[] values, Type targetType, object parameter, CultureInfo culture)
    {
        if (values == null || !targetType.IsAssignableFrom(typeof(bool)))
        {
            return false;
            // Alternatively, return BindableProperty.UnsetValue to use the binding FallbackValue
        }

        foreach (var value in values)
        {
            if (!(value is bool b))
            {
                return false;
                // Alternatively, return BindableProperty.UnsetValue to use the binding FallbackValue
            }
            else if (!b)
            {
                return false;
            }
        }
        return true;
    }

    public object[] ConvertBack(object value, Type[] targetTypes, object parameter, CultureInfo culture)
    {
        if (!(value is bool b) || targetTypes.Any(t => !t.IsAssignableFrom(typeof(bool))))
        {
            // Return null to indicate conversion back is not possible
            return null;
        }

        if (b)
        {
            return targetTypes.Select(t => (object)true).ToArray();
        }
        else
        {
            // Can't convert back from false because of ambiguity
            return null;
        }
    }
}
```

`Convert` 메서드는 소스 값을 바인딩 대상의 값으로 변환합니다. Xamarin.Forms가 소스 바인딩에서 바인딩 대상으로 값을 전파할 때 이 메서드를 호출합니다. 이 메서드는 네 개의 인수를 허용합니다.

- 형식 `object[]`의 `values`는 `MultiBinding`의 소스 바인딩이 만들어 내는 일련의 값입니다.
- 형식 `Type`의 `targetType`은 바인딩 대상 속성 형식입니다.
- 형식 `object`의 `parameter`는 사용할 변환기 매개 변수입니다.
- 형식 `CultureInfo`의 `culture`는 변환기에서 사용할 문화권입니다.

`Convert` 메서드는 변환된 값을 나타내는 `object`를 반환합니다. 이 메서드는 다음을 반환해야 합니다.

- `BindableProperty.UnsetValue`는 변환기가 값을 생성하지 않았으며, 바인딩에 `FallbackValue`가 사용될 것임을 나타냅니다.
- `Binding.DoNothing`은 Xamarin.Forms에 아무 작업도 수행하지 않도록 지시합니다. 예를 들어, Xamarin.Forms에 값을 바인딩 대상으로 전송하지 않도록 지시하거나 `FallbackValue`를 사용하지 않도록 지시합니다.
- `null`은 변환기가 변환을 수행할 수 없으며, 바인딩에 `TargetNullValue`가 사용될 것임을 나타냅니다.

> [!IMPORTANT]
> `Convert` 메서드에서 `BindableProperty.UnsetValue`를 수신하는 `MultiBinding`은 [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) 속성을 정의해야 합니다. 마찬가지로 `Convert` 메서드에서 `null`을 수신하는 `MultiBinding`도 [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) 속성을 정의해야 합니다.

`ConvertBack` 메서드는 바인딩 대상을 소스 바인딩 값으로 변환합니다. 이 메서드는 네 개의 인수를 허용합니다.

- 형식 `object`의 `value`는 바인딩 대상에서 생성하는 값입니다.
- 형식 `Type[]`의 `targetTypes`는 변환할 일련의 형식입니다. 배열 길이는 메서드에서 반환하도록 제안되는 값의 개수와 형식을 나타냅니다.
- 형식 `object`의 `parameter`는 사용할 변환기 매개 변수입니다.
- 형식 `CultureInfo`의 `culture`는 변환기에서 사용할 문화권입니다.

형식 `object[]`의 `ConvertBack` 메서드는 값의 배열을 반환하며, 이 배열은 대상 값에서 다시 소스 값으로 변환되었습니다. 이 메서드는 다음을 반환해야 합니다.

- `i` 위치의 `BindableProperty.UnsetValue`는 변환기가 인덱스 `i`의 소스 바인딩에 대한 값을 제공할 수 없으며 설정된 값이 없음을 나타냅니다.
- `i` 위치의 `Binding.DoNothing`은 인덱스 `i`의 소스 바인딩에 설정된 값이 없음을 나타냅니다.
- `null`은 변환기가 변환을 수행할 수 없거나 이 방향으로는 변환이 지원되지 않음을 나타냅니다.

## <a name="consume-a-imultivalueconverter"></a>IMultiValueConverter 사용

`IMultiValueConverter`는 리소스 사전에서 인스턴스화한 다음 `MultiBinding.Converter` 속성을 설정하는 `StaticResource` 태그 환장을 사용하여 참조함으로써 사용됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MultiBindingConverterPage"
             Title="MultiBinding Converter demo">

    <ContentPage.Resources>
        <local:AllTrueMultiConverter x:Key="AllTrueConverter" />
        <local:InverterConverter x:Key="InverterConverter" />
    </ContentPage.Resources>

    <CheckBox>
        <CheckBox.IsChecked>
            <MultiBinding Converter="{StaticResource AllTrueConverter}">
                <Binding Path="Employee.IsOver16" />
                <Binding Path="Employee.HasPassedTest" />
                <Binding Path="Employee.IsSuspended"
                         Converter="{StaticResource InverterConverter}" />
            </MultiBinding>
        </CheckBox.IsChecked>
    </CheckBox>
</ContentPage>    
```

이 예제에서 `MultiBinding` 개체는 세 개의 [`Binding`](xref:Xamarin.Forms.Binding) 개체가 `true`로 평가할 경우 [`CheckBox.IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked) 속성을 `true`로 설정하는 데 `AllTrueMultiConverter`를 사용합니다. 그렇지 않으면 `CheckBox.IsChecked` 속성은 `false`로 설정됩니다.

기본적으로 [`CheckBox.IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked) 속성은 [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) 바인딩을 사용합니다. 따라서 `AllTrueMultiConverter` 인스턴스의 `ConvertBack` 메서드는 사용자가 [`CheckBox`](xref:Xamarin.Forms.CheckBox)를 선택하지 않은 경우에 실행되며, 이때 소스 바인딩 값은 `CheckBox.IsChecked` 속성 값으로 설정됩니다.

## <a name="format-strings"></a>형식 문자열

`MultiBinding`은 `StringFormat` 속성을 통해 문자열로 표시되는 다중 바인딩 결과의 서식을 지정할 수 있습니다. 이 속성은 자리 표시자를 포함한 표준 .NET 서식 지정 문자열로 설정될 수 있으며, 이는 다중 바인딩 결과의 서식 지정 방식을 지정합니다.

```xaml
<Label>
    <Label.Text>
        <MultiBinding StringFormat="{}{0} {1} {2}">
            <Binding Path="Employee1.Forename" />
            <Binding Path="Employee1.MiddleName" />
            <Binding Path="Employee1.Surname" />
        </MultiBinding>
    </Label.Text>
</Label>
```

이 예제에서 `StringFormat` 속성은 세 개의 바인딩된 값을 [`Label`](xref:Xamarin.Forms.Label)로 표시되는 하나의 문자열로 결합합니다.

> [!IMPORTANT]
> 복합 문자열 서식의 매개 변수 수는 `MultiBinding`에 있는 하위 `Binding` 개체의 수를 초과할 수 없습니다.

`Converter` 및 `StringFormat` 속성을 설정할 경우, 변환기가 먼저 데이터 값에 적용된 다음 `StringFormat`이 적용됩니다.

Xamarin.Forms의 문자열 서식 지정에 대한 자세한 정보는 [Xamarin.Forms 문자열 서식 지정](string-formatting.md)을 참조하세요.

## <a name="provide-fallback-values"></a>대체 값 제공

바인딩 프로세스가 실패할 경우 사용할 대체 값을 정의하여 데이터 바인딩을 더욱 강력하게 만들 수 있습니다. `MultiBinding` 개체에서 [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) 및 [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) 속성을 선택적으로 정의하여 이 작업을 수행할 수 있습니다.

`IMultiValueConverter` 인스턴스의 `Convert` 메서드가 `BindableProperty.UnsetValue`를 반환하면 `MultiBinding`은 [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue)를 사용하며, 이는 변환기가 값을 생성하지 않았음을 나타냅니다. `IMultiValueConverter` 인스턴스의 `Convert` 메서드가 `null`을 반환하면 `MultiBinding`은 [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue)를 사용하며, 이는 변환기가 변환을 수행할 수 없음을 나타냅니다.

바인딩 대체에 대한 자세한 정보는 [Xamarin.Forms 바인딩 대체](binding-fallbacks.md)를 참조하세요.

## <a name="nest-multibinding-objects"></a>MultiBinding 개체 중첩

`MultiBinding` 개체가 중첩될 수 있으므로 다중 `MultiBinding` 개체는 `IMultiValueConverter` 인스턴스를 통해 값을 반환하도록 평가됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.NestedMultiBindingPage"
             Title="Nested MultiBinding demo">

    <ContentPage.Resources>
        <local:AllTrueMultiConverter x:Key="AllTrueConverter" />
        <local:AnyTrueMultiConverter x:Key="AnyTrueConverter" />
        <local:InverterConverter x:Key="InverterConverter" />
    </ContentPage.Resources>

    <CheckBox>
        <CheckBox.IsChecked>
            <MultiBinding Converter="{StaticResource AnyTrueConverter}">
                <MultiBinding Converter="{StaticResource AllTrueConverter}">
                    <Binding Path="Employee.IsOver16" />
                    <Binding Path="Employee.HasPassedTest" />
                    <Binding Path="Employee.IsSuspended" Converter="{StaticResource InverterConverter}" />                        
                </MultiBinding>
                <Binding Path="Employee.IsMonarch" />
            </MultiBinding>
        </CheckBox.IsChecked>
    </CheckBox>
</ContentPage>
```

이 예에서 내부 `MultiBinding` 개체의 모든 [`Binding`](xref:Xamarin.Forms.Binding) 개체가 `true`로 평가하거나 외부 `MultiBinding` 개체의 `Binding` 개체가 `true`로 평가할 경우 `MultiBinding` 개체는 `AnyTrueMultiConverter` 인스턴스를 사용하여 [`CheckBox.IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked) 속성을 `true`로 설정합니다. 그렇지 않으면 `CheckBox.IsChecked` 속성은 `false`로 설정됩니다.

## <a name="use-a-relativesource-binding-in-a-multibinding"></a>MultiBinding에서 RelativeSource 바인딩 사용

`MultiBinding` 개체는 상대 바인딩을 지원하며, 이 바인딩을 사용하면 바인딩 대상의 위치에 상대적으로 바인딩 소스를 설정할 수 있습니다.

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:DataBindingDemos">
    <ContentPage.Resources>
        <local:AllTrueMultiConverter x:Key="AllTrueConverter" />

        <ControlTemplate x:Key="CardViewExpanderControlTemplate">
            <Expander BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
                      IsExpanded="{Binding IsExpanded, Source={RelativeSource TemplatedParent}}"
                      BackgroundColor="{Binding CardColor}">
                <Expander.IsVisible>
                    <MultiBinding Converter="{StaticResource AllTrueConverter}">
                        <Binding Path="IsExpanded" />
                        <Binding Path="IsEnabled" />
                    </MultiBinding>
                </Expander.IsVisible>
                <Expander.Header>
                    <Grid>
                        <!-- XAML that defines Expander header goes here -->
                    </Grid>
                </Expander.Header>
                <Grid>
                    <!-- XAML that defines Expander content goes here -->
                </Grid>
            </Expander>
        </ControlTemplate>
    </ContentPage.Resources>

    <StackLayout>
        <controls:CardViewExpander BorderColor="DarkGray"
                                   CardTitle="John Doe"
                                   CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                                   IconBackgroundColor="SlateGray"
                                   IconImageSource="user.png"
                                   ControlTemplate="{StaticResource CardViewExpanderControlTemplate}"
                                   IsEnabled="True"
                                   IsExpanded="True" />
    </StackLayout>
</ContentPage>
```

이 예에서 `TemplatedParent` 상대 바인딩 모드는 컨트롤 템플릿 내부에서 해당 템플릿이 적용되는 런타임 개체 인스턴스에 바인딩하는 데 사용됩니다. [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)의 루트 요소인 `Expander`의 `BindingContext`는 해당 템플릿이 적용되는 런타임 개체 인스턴스로 설정됩니다. 따라서, `Expander`와 그 자식은 `CardViewExpander` 개체의 속성에 대해 바인딩 식과 [`Binding`](xref:Xamarin.Forms.Binding) 개체를 확인합니다. 두 개의 [`Binding`](xref:Xamarin.Forms.Binding) 개체가 `true`로 평가할 경우 `MultiBinding`은 `AllTrueMultiConverter` 인스턴스를 사용하여 `Expander.IsVisible` 속성을 `true`로 설정합니다. 그렇지 않으면 `Expander.IsVisible` 속성은 `false`로 설정됩니다.

상대 바인딩에 대한 자세한 내용은 [Xamarin.Forms 상대 바인딩](relative-bindings.md)을 참조하세요. 컨트롤 템플릿에 대한 자세한 내용은 [Xamarin.Forms 컨트롤 템플릿](~/xamarin-forms/app-fundamentals/templates/control-template.md)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Xamarin.Forms 문자열 서식 지정](string-formatting.md)
- [Xamarin.Forms 바인딩 대체](binding-fallbacks.md)
- [Xamarin.Forms 상대 바인딩](relative-bindings.md)
- [Xamarin.Forms 컨트롤 템플릿](~/xamarin-forms/app-fundamentals/templates/control-template.md)
