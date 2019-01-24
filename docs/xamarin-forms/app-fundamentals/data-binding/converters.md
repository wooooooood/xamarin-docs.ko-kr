---
title: Xamarin.Forms 바인딩 값 변환기
description: 이 문서에서는 값 변환기를 구현하여(바인딩 변환기 또는 바인딩 값 변환기라고도 함) Xamarin.Forms 데이터 바인딩 내의 값을 캐스팅하거나 변환하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 4594da09d48a0888a88cbce9ab135a007eb6f4cd
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53054333"
---
# <a name="xamarinforms-binding-value-converters"></a>Xamarin.Forms 바인딩 값 변환기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)

데이터 바인딩은 일반적으로 원본 속성에서 대상 속성으로, 일부 경우에는 대상 속성에서 원본 속성으로 데이터를 전송합니다. 이 전송은 원본 및 대상 속성이 동일한 형식인 경우 또는 하나의 형식을 암시적 변환을 통해 다른 형식으로 변환할 수 있는 경우에 간단합니다. 그렇지 않은 경우 형식 변환을 수행해야 합니다.

[**문자열 서식 지정**](string-formatting.md) 문서에서 데이터 바인딩의 `StringFormat` 속성을 사용하여 형식을 문자열로 변환하는 방법을 살펴보았습니다. 다른 형식의 변환의 경우 [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) 인터페이스를 구현하는 클래스에 일부 특수한 코드를 작성해야 합니다. (유니버설 Windows 플랫폼은 `Windows.UI.Xaml.Data` 네임스페이스에 [`IValueConverter`](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/)라는 유사한 클래스를 포함하지만 이 `IValueConverter`는 `Xamarin.Forms` 네임스페이스에 있습니다.) `IValueConverter`를 구현하는 클래스는 *값 변환기*라고 하지만 종종 *바인딩 변환기* 또는 *바인딩 값 변환기*라고도 합니다.

## <a name="the-ivalueconverter-interface"></a>IValueConverter 인터페이스

원본 속성의 형식이 `int`이지만 대상 속성이 `bool`인 데이터 바인딩을 정의하려 한다고 가정합니다. 이 데이터 바인딩에서 정수 원본이 0인 경우 `false` 값을, 그렇지 않은 경우 `true` 값을 생성하기를 원합니다.  

`IValueConverter` 인터페이스를 구현하는 클래스를 사용하여 이를 수행할 수 있습니다.

```csharp
public class IntToBoolConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value != 0;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? 1 : 0;
    }
}
```

이 클래스의 인스턴스를 `Binding` 클래스의 [`Converter`](xref:Xamarin.Forms.Binding.Converter) 속성 또는 `Binding` 태그 확장의 [`Converter`](xref:Xamarin.Forms.Xaml.BindingExtension.Converter) 속성으로 설정합니다. 이 클래스는 데이터 바인딩의 일부가 됩니다.

데이터가 원본에서 `OneWay` 또는 `TwoWay` 바인딩의 대상으로 이동하는 경우 `Convert` 메서드가 호출됩니다. `value` 매개 변수는 데이터 바인딩 원본의 개체 또는 값입니다. 메서드는 데이터 바인딩 대상 형식의 값을 반환해야 합니다. 여기에 표시된 메서드는 `value` 매개 변수를 `int`에 캐스팅한 다음, `bool` 반환 값에 대해 0과 비교합니다.

데이터가 대상에서 `TwoWay` 또는 `OneWayToSource` 바인딩의 원본으로 이동하는 경우 `ConvertBack` 메서드가 호출됩니다. `ConvertBack`은 반대 변환을 수행합니다. `value` 매개 변수가 대상의 `bool`이라고 가정하고, 소스에 대한 `int` 반환 값으로 변환합니다.

데이터 바인딩에 `StringFormat` 설정이 포함되는 경우 값 변환기는 결과가 문자열로 서식 지정되기 전에 호출됩니다.

[**데이터 바인딩 데모**](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) 샘플의 **단추 사용** 페이지는 데이터 바인딩에서 이 값 변환기를 사용하는 방법을 보여줍니다. `IntToBoolConverter`는 페이지의 리소스 사전에서 인스턴스화됩니다. 그런 다음, `StaticResource` 태그 확장을 참조하여 두 개의 데이터 바인딩에서 `Converter` 속성을 설정합니다. 페이지의 여러 데이터 바인딩 간에 데이터 변환기를 공유하는 것은 매우 일반적입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.EnableButtonsPage"
             Title="Enable Buttons">
    <ContentPage.Resources>
        <ResourceDictionary>
            <local:IntToBoolConverter x:Key="intToBool" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <Entry x:Name="entry1"
               Text=""
               Placeholder="enter search term"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Search"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry1},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />

        <Entry x:Name="entry2"
               Text=""
               Placeholder="enter destination"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Submit"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry2},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />
    </StackLayout>
</ContentPage>
```

애플리케이션의 여러 페이지에서 값 변환기가 사용되는 경우 **App.xaml** 파일의 리소스 사전에서 인스턴스화할 수 있습니다.

**단추 사용** 페이지는 `Button`에서 사용자가 `Entry` 보기로 입력하는 텍스트에 따라 작업을 수행하는 경우 일반적인 필요를 보여줍니다. `Entry`에 아무 것도 입력되지 않은 경우 `Button`은 비활성화되어야 합니다. 각 `Button`은 해당 `IsEnabled` 속성에 데이터 바인딩을 포함합니다. 데이터 바인딩 원본은 해당 `Entry`의 `Text` 속성의 `Length` 속성입니다. 해당 `Length` 속성이 0이 아닌 경우 값 변환기는 `true`를 반환하며 `Button`이 활성화됩니다.

[![단추 사용](converters-images/enablebuttons-small.png "단추 사용")](converters-images/enablebuttons-large.png#lightbox "단추 사용")

각 `Entry`의 `Text` 속성은 빈 문자열로 초기화됩니다. `Text` 속성은 기본적으로 `null`이며, 이 경우 데이터 바인딩은 작동하지 않습니다.

일부 값 변환기는 특정 애플리케이션에 대해 구체적으로 작성되는 반면 다른 값 변환기는 일반화됩니다. 값 변환기가 `OneWay` 바인딩에서만 사용된다는 것을 아는 경우 `ConvertBack` 메서드는 간단하게 `null`을 반환할 수 있습니다.

위에 표시된 `Convert` 메서드는 `value` 인수가 `int` 형식이며 반환 값은 `bool` 형식이어야 한다는 것을 암시적으로 가정합니다. 마찬가지로 `ConvertBack` 메서드는 `value` 인수가 `bool` 형식이며 반환 값은 `int`라는 것을 가정합니다. 그렇지 않은 경우 런타임 예외가 발생합니다.

값 변환기를 보다 일반화되고 여러 다른 유형의 데이터를 허용하도록 작성할 수 있습니다. `Convert` 및 `ConvertBack` 메서드는 `value` 매개 변수와 함께 `as` 또는 `is` 연산자를 사용하거나 해당 매개 변수에서 `GetType`을 호출하여 해당 형식을 결정한 다음, 적절한 작업을 수행할 수 있습니다. 각 메서드의 반환 값의 예상 형식은 `targetType` 매개 변수에서 지정됩니다. 경우에 따라 값 변환기는 다양한 대상 유형의 데이터 바인딩과 함께 사용됩니다. 값 변환기는 `targetType` 인수를 사용하여 올바른 형식에 대한 변환을 수행할 수 있습니다.

수행되는 변환이 다른 문화권과 다른 경우 이 목적을 위해 `culture` 매개 변수를 사용합니다. `Convert` 및 `ConvertBack`에 대한 `parameter` 인수는 이 문서의 뒷부분에서 설명됩니다.

## <a name="binding-converter-properties"></a>바인딩 변환기 속성

값 변환기 클래스에는 속성 및 일반 매개 변수가 있을 수 있습니다. 이 특정 값 변환기는 원본에서 대상에 대한 `T` 형식의 개체로 `bool`을 변환합니다.

```csharp
public class BoolToObjectConverter<T> : IValueConverter
{
    public T TrueObject { set; get; }

    public T FalseObject { set; get; }

    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? TrueObject : FalseObject;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return ((T)value).Equals(TrueObject);
    }
}
```

**스위치 표시기** 페이지는 `Switch` 보기의 값을 표시하기 위해 사용하는 방법을 보여줍니다. 값 변환기를 리소스 사전의 리소스로 인스턴스화하는 것은 일반적이지만 이 페이지는 대체 방법을 보여 줍니다. 각 값 변환기는 `Binding.Converter` 속성 요소 태그 간에 인스턴스화됩니다. `x:TypeArguments`는 제네릭 인수를 나타내며, `TrueObject` 및 `FalseObject`는 해당 형식의 개체로 설정됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SwitchIndicatorsPage"
             Title="Switch Indicators">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>

            <Style TargetType="Switch">
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Subscribe?" />
            <Switch x:Name="switch1" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch1}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Of course!"
                                                         FalseObject="No way!" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Allow popups?" />
            <Switch x:Name="switch2" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Yes"
                                                         FalseObject="No" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
                <Label.TextColor>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Color"
                                                         TrueObject="Green"
                                                         FalseObject="Red" />
                        </Binding.Converter>
                    </Binding>
                </Label.TextColor>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Learn more?" />
            <Switch x:Name="switch3" />
            <Label FontSize="18"
                   VerticalOptions="Center">
                <Label.Style>
                    <Binding Source="{x:Reference switch3}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Style">
                                <local:BoolToObjectConverter.TrueObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Indubitably!" />
                                        <Setter Property="FontAttributes" Value="Italic, Bold" />
                                        <Setter Property="TextColor" Value="Green" />
                                    </Style>                                    
                                </local:BoolToObjectConverter.TrueObject>

                                <local:BoolToObjectConverter.FalseObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Maybe later" />
                                        <Setter Property="FontAttributes" Value="None" />
                                        <Setter Property="TextColor" Value="Red" />
                                    </Style>
                                </local:BoolToObjectConverter.FalseObject>
                            </local:BoolToObjectConverter>
                        </Binding.Converter>
                    </Binding>
                </Label.Style>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

마지막 세 개의 `Switch` 및 `Label` 쌍에서 제네릭 인수는 `Style`로 설정되며, 전체 `Style` 개체는 `TrueObject` 및 `FalseObject`의 값에 대해 제공됩니다. 이는 리소스 사전에 설정된 `Label`에 대한 암시적 스타일을 재정의하므로 해당 스타일의 속성은 `Label`에 명시적으로 할당됩니다. `Switch`를 설정/해제하면 해당 `Label`에 변경 내용이 반영됩니다.

[![스위치 표시기](converters-images/switchindicators-small.png "스위치 표시기")](converters-images/switchindicators-large.png#lightbox "스위치 표시기")

[`Triggers`](~/xamarin-forms/app-fundamentals/triggers.md)를 사용하여 다른 보기를 기반으로 하는 사용자 인터페이스에서 비슷한 변경을 구현할 수도 있습니다.

## <a name="binding-converter-parameters"></a>바인딩 변환기 매개 변수

`Binding` 클래스는 [`ConverterParameter`](xref:Xamarin.Forms.Binding.ConverterParameter) 속성을 정의하며, `Binding` 태그 확장도 [`ConverterParameter`](xref:Xamarin.Forms.Xaml.BindingExtension.ConverterParameter) 속성을 정의합니다. 이 속성이 설정되어 있는 경우 값은 `parameter` 인수로 `Convert` 및 `ConvertBack` 메서드에 전달됩니다. 값 변환기의 인스턴스가 여러 데이터 바인딩 간에 공유되는 경우에도 `ConverterParameter`는 약간 다른 변환을 수행하는 데 다를 수 있습니다.

`ConverterParameter`의 사용은 색 선택 영역 프로그램으로 설명됩니다. 이 경우 `RgbColorViewModel`에는 `Color` 값을 생성하는 데 사용하는 `Red`, `Green` 및 `Blue`라는 `double` 형식의 세 가지 속성이 있습니다.

```csharp
public class RgbColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Red
    {
        set
        {
            if (color.R != value)
            {
                Color = new Color(value, color.G, color.B);
            }
        }
        get
        {
            return color.R;
        }
    }

    public double Green
    {
        set
        {
            if (color.G != value)
            {
                Color = new Color(color.R, value, color.B);
            }
        }
        get
        {
            return color.G;
        }
    }

    public double Blue
    {
        set
        {
            if (color.B != value)
            {
                Color = new Color(color.R, color.G, value);
            }
        }
        get
        {
            return color.B;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Red"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Green"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Blue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

`Red`, `Green` 및 `Blue` 속성은 0과 1 사이의 범위입니다. 그러나 두 자리 16진수 값으로 구성 요소를 표시하는 것을 선호할 수 있습니다.

XAML에서 16진수 값으로 표시하려면 255를 곱하고, 정수로 변환한 다음, `StringFormat` 속성에서 "X2"의 사양으로 형식을 지정해야 합니다. 값 변환기에서 처음 두 작업(255 곱하기 및 정수로 변환)을 처리할 수 있습니다. 값 변환기를 최대한 일반화하기 위해 `ConverterParameter` 속성으로 곱하기 비율을 지정할 수 있습니다. 즉, `parameter` 인수로 `Convert` 및 `ConvertBack` 메서드를 입력합니다.

```csharp
public class DoubleToIntConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)Math.Round((double)value * GetParameter(parameter));
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value / GetParameter(parameter);
    }

    double GetParameter(object parameter)
    {
        if (parameter is double)
            return (double)parameter;

        else if (parameter is int)
            return (int)parameter;

        else if (parameter is string)
            return double.Parse((string)parameter);

        return 1;
    }
}
```

`Convert`는 `parameter` 값을 곱하는 동안 `double`에서 `int`로 변환하고, `ConvertBack`은 정수 `value` 인수를 `parameter`로 나누고 `double` 결과를 반환합니다. (아래에 표시된 프로그램에서 값 변환기는 문자열 서식 지정과 관련된 경우에만 사용되므로 `ConvertBack`은 사용되지 않습니다.)

`parameter` 인수의 형식은 데이터 바인딩이 코드 또는 XAML에서 정의되는지 여부에 따라 다를 수 있습니다. `Binding`의 `ConverterParameter` 속성이 코드에서 설정된 경우 숫자 값으로 설정될 수 있습니다.

```csharp
binding.ConverterParameter = 255;
```

`ConverterParameter` 속성은 `Object` 형식이므로 C# 컴파일러는 정수로 리터럴 255를 해석하고, 해당 값으로 속성을 설정합니다.

그러나 XAML에서 `ConverterParameter`는 다음과 같이 설정될 수 있습니다.

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

255는 숫자처럼 보이지만 `ConverterParameter`는 `Object` 형식이므로 XAML 파서는 255를 문자열로 처리합니다.

이러한 이유로 위에 표시된 값 변환기는 `double`, `int` 또는 `string` 형식이 되는 `parameter`에 대한 사례를 처리하는 별도의 `GetParameter` 메서드를 포함합니다.  

**RGB 색 선택기** 페이지는 두 암시적 스타일의 정의에 따라 해당 리소스 사전에서 `DoubleToIntConverter`를 인스턴스화합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.RgbColorSelectorPage"
             Title="RGB Color Selector">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <local:DoubleToIntConverter x:Key="doubleToInt" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:RgbColorViewModel Color="Gray" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Red}" />
            <Label Text="{Binding Red,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Red = {0:X2}'}" />

            <Slider Value="{Binding Green}" />
            <Label Text="{Binding Green,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Green = {0:X2}'}" />

            <Slider Value="{Binding Blue}" />
            <Label>
                <Label.Text>
                    <Binding Path="Blue"
                             StringFormat="Blue = {0:X2}"
                             Converter="{StaticResource doubleToInt}">
                        <Binding.ConverterParameter>
                            <x:Double>255</x:Double>
                        </Binding.ConverterParameter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

`Red` 및 `Green` 속성의 값은 `Binding` 태그 확장으로 표시됩니다. 그러나 `Blue` 속성은 `Binding` 클래스를 인스턴스화하여 명시적 `double` 값을 `ConverterParameter` 속성으로 설정할 수 있는 방법을 보여줍니다.

결과는 다음과 같습니다.

[![RGB 색 선택기](converters-images/rgbcolorselector-small.png "RGB 색 선택기")](converters-images/rgbcolorselector-large.png#lightbox "RGB 색 선택기")


## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모(샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 서적의 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
