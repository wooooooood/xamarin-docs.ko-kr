---
title: Xamarin.Forms 바인딩 값 변환기
description: 이 문서에서는 캐스팅 하거나 (이 라고도 바인딩 변환기 또는 바인딩 값 변환기)는 값 변환기를 구현 하 여 Xamarin.Forms 데이터 바인딩 내에서 값을 변환 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: a5bd52d43ef93013537f30c7d5e0c31cbf336d07
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241831"
---
# <a name="xamarinforms-binding-value-converters"></a>Xamarin.Forms 바인딩 값 변환기

데이터 바인딩은 일반적으로 데이터 원본 속성의 대상 속성에 및 전송 소스 속성에 대상 속성에서 일부 경우에 합니다. 이 이전은 한 형식 변환 하는 암시적 변환을 통해 다른 형식으로 변환할 수 있을 경우 또는 동일한 유형의 원본 및 대상 속성 때 간단 합니다. 하지 않은 경우 형식 변환을 수행 해야 합니다.

에 [ **문자열 서식 지정** ](string-formatting.md) 문서를 사용 하는 방법을 살펴보았습니다는 `StringFormat` 모든 형식을 문자열로 변환 하는 데이터 바인딩 속성입니다. 다른 유형의 변환 구현 하는 클래스의 특수화 된 코드를 작성 해야는 [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) 인터페이스입니다. (비슷한 클래스를 포함 하는 유니버설 Windows 플랫폼 [ `IValueConverter` ](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) 에 `Windows.UI.Xaml.Data` 있지만이 네임 스페이스 `IValueConverter` 에 `Xamarin.Forms` 네임 스페이스입니다.) 구현 하는 클래스 `IValueConverter` 라고 *값 변환기*, 것 역시 라고 하지만 *변환기 바인딩* 또는 *값 변환기 바인딩*합니다.

## <a name="the-ivalueconverter-interface"></a>IValueConverter 인터페이스

형식의 경우 소스가 속성은 데이터 바인딩을 정의 하는 경우 `int` target 속성이 있지만 `bool`합니다. 생성 하기 위해이 데이터 바인딩 원하는 `false` 정수 원본 0과 같은 경우 값 및 `true` 그렇지 않은 경우.  

구현 하는 클래스와 함께 수행할 수 있습니다는 `IValueConverter` 인터페이스:

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

이 클래스의 인스턴스를 설정는 [ `Converter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Converter/) 속성의는 `Binding` 클래스 또는 [ `Converter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Converter/) 의 속성은 `Binding` 태그 확장 합니다. 이 클래스에는 데이터 바인딩의 일부가 됩니다.

`Convert` 대상이 소스에서 데이터 이동 하는 경우 메서드는 `OneWay` 또는 `TwoWay` 바인딩. `value` 매개 변수는 개체 또는 데이터 바인딩 원본의 값입니다. 메서드는 데이터 바인딩 대상의 형식의 값을 반환 해야 합니다. 캐스트 여기 표시 된 메서드는 `value` 매개 변수를 프로그램 `int` 다음에 0에 대 한 비교는 `bool` 값을 반환 합니다.

`ConvertBack` 에서 소스를 대상에서 데이터를 이동 하면 메서드는 `TwoWay` 또는 `OneWayToSource` 바인딩. `ConvertBack` 반대 변환을 수행: 가정는 `value` 매개 변수는 한 `bool` 에서 대상으로 변환 하 고는 `int` 소스에 대 한 값을 반환 합니다.

데이터 바인딩도 포함 된 경우는 `StringFormat` 설정, 값 변환기 전에 호출 됩니다는 결과 문자열 서식이 합니다.

**사용 단추** 페이지에 [ **데이터 바인딩 데모** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) 샘플에는 데이터 바인딩의이 값 변환기를 사용 하는 방법을 보여 줍니다. `IntToBoolConverter` 페이지의 리소스 사전에 인스턴스화됩니다. 으로 참조 된 다음는 `StaticResource` 설정 되도록 태그 확장의 `Converter` 두 데이터 바인딩에 대 한 속성. 매우 일반적 데이터 변환기 페이지에서 여러 데이터 바인딩 간에 공유할 수 있습니다:

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

응용 프로그램의 여러 페이지에서 값 변환기가 사용 하는 경우에 리소스 사전에 인스턴스화할 수 있습니다는 **App.xaml** 파일입니다.

**사용 단추** 페이지는 공통 필요한 경우 보여 줍니다.는 `Button` 사용자에 입력 텍스트에 기반 하는 작업을 수행는 `Entry` 보기. Nothing에 입력 된 경우는 `Entry`, `Button` 비활성화 해야 합니다. 각 `Button` 에 데이터 바인딩이 포함 되어 해당 `IsEnabled` 속성입니다. 데이터 바인딩 소스는는 `Length` 의 속성은 `Text` 해당 속성 `Entry`합니다. 경우 해당 `Length` 속성은 0, 값 변환기 반환 `true` 및 `Button` 사용 됨:

[![단추를 사용 하도록 설정](converters-images/enablebuttons-small.png "단추를 사용 하도록 설정")](converters-images/enablebuttons-large.png#lightbox "단추를 사용 하도록 설정")

에 `Text` 각 속성 `Entry` 빈 문자열로 초기화 합니다. `Text` 속성은 `null` 기본적으로, 및 데이터 바인딩 작동 하지 것입니다이 경우.

일부 값 변환기는 다른 일반화 하는 동안 특정 응용 프로그램만 위한 기록 됩니다. 값 변환기에만 사용할 수는 알고 있는 경우 `OneWay` 바인딩으로 설정 하면 `ConvertBack` 메서드 반환할 수 `null`합니다.

`Convert` 메서드를 암시적으로 위에 표시 된 것으로 가정은 `value` 형식의 인수는 `int` 반환 값 형식 이어야 하 고 `bool`합니다. 마찬가지로,는 `ConvertBack` 메서드 가정 하는 `value` 형식의 인수는 `bool` 값 반환 `int`합니다. 해당 되는 경우 런타임 예외가 발생 합니다.

값 변환기 더 일반화 하 고 몇 가지 유형의 데이터를 허용 하도록 작성할 수 있습니다. `Convert` 및 `ConvertBack` 메서드 צ ְ ײ는 `as` 또는 `is` 연산자는 `value` 매개 변수를 하거나 호출할 수 `GetType` 해당 매개 변수에 해당 유형 및 다음 작업 지정 된 것에 해당 합니다. 각 메서드의 반환 값의 예상된 형식을 지정 하 여는 `targetType` 매개 변수입니다. 값 변환기는 다양 한 대상 유형에;의 데이터 바인딩을 사용 하는 경우에 따라 사용 됩니다. 값 변환기에서 사용할 수는 `targetType` 올바른 형식에 대 한 변환을 수행 하는 인수입니다.

사용 하 여 변환이 수행 되 고 서로 다른 문화권에 대 한 다른 경우는 `culture` 이 용도 대 한 매개 변수입니다. `parameter` 인수를 `Convert` 및 `ConvertBack` 이 문서의 뒷부분에서 설명 합니다.

## <a name="binding-converter-properties"></a>바인딩 변환기 속성

값 변환기 클래스 속성 및 제네릭 매개 변수를 가질 수 있습니다. 이 특정 값 변환기에서 변환 된 `bool` 형식의 개체에 소스의 `T` 대상에 대해:

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

**스위치 표시기** 페이지의 값을 표시 하 고 수 사용 하는 방법을 보여 줍니다.는 `Switch` 보기. 이 페이지에는 대신 보여 줍니다 리소스 사전에 리소스 그룹으로 값 변환기를 인스턴스화할 수 일반적 하지만: 사이의 각 값 변환기를 인스턴스화할 `Binding.Converter` 속성 요소 태그입니다. `x:TypeArguments` 제네릭 인수를 나타내는 및 `TrueObject` 및 `FalseObject` 해당 유형의 개체를 모두 설정:

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

세 가지 지난에서 `Switch` 및 `Label` 쌍의 제네릭 인수가로 설정 된 `Style`, 전체 및 `Style` 개체의 값에 대해 제공 되 `TrueObject` 및 `FalseObject`합니다. 이러한 재정의 대 한 암시적 스타일 `Label` 리소스 사전에서 설정 되므로 해당 스타일의 속성을 명시적으로 할당 된는 `Label`합니다. 설정/해제는 `Switch` 해당 하면 `Label` 변경 사항을 반영 합니다.

[![표시기 전환](converters-images/switchindicators-small.png "표시기 전환")](converters-images/switchindicators-large.png#lightbox "표시기를 전환 합니다.")

사용 하 여 이기도 [ `Triggers` ](~/xamarin-forms/app-fundamentals/triggers.md) 유사한 변경을 다른 보기에 따라 사용자 인터페이스에서 구현 합니다.

## <a name="binding-converter-parameters"></a>변환기 매개 변수 바인딩

`Binding` 클래스 정의 [ `ConverterParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.ConverterParameter/) 속성 및 `Binding` 태그 확장은 정의 [ `ConverterParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.ConverterParameter/) 속성입니다. 이 속성이 설정 되어 있으면 값이 전달 되는 `Convert` 및 `ConvertBack` 와 메서드는 `parameter` 인수입니다. 값 변환기의 인스턴스는 여러 가지 데이터 바인딩 간에 공유 하는 경우에는 `ConverterParameter` 다소 다른 변환을 수행 하기 위해 다를 수 있습니다.

사용 하는 `ConverterParameter` 색 선택을 프로그램으로 표시 됩니다. 이 경우에 `RgbColorViewModel` 유형의 세 가지 속성에 `double` 라는 `Red`, `Green`, 및 `Blue` 생성에 사용 되는 `Color` 값:

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

`Red`, `Green`, 및 `Blue` 0과 1 사이의 속성 범위입니다. 그러나 구성 두 자리 16 진수 값으로 표시 될 수도 있습니다.

표시 하려면 16 진수 값으로 이러한 XAML에서은 해야 수 255 곱한는 정수로 변환할 마우스 오른쪽에서 "X2"에 대 한 사양으로 포맷 된 후의 `StringFormat` 속성입니다. 값 변환기에서 처음 두 작업 (255로 곱한 및 정수 변환)를 처리할 수 있습니다. 가능한 범용으로 설정 하는 대로 값 변환기를 하려면 배수 함께 지정할 수는 `ConverterParameter` 속성을 의미 하는 `Convert` 및 `ConvertBack` 와 메서드는 `parameter` 인수:

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

`Convert` 에서 변환 하는 `double` 를 `int` 곱한 하는 동안는 `parameter` 값입니다;는 `ConvertBack` 정수 나눕니다 `value` 으로 인수 증가 `parameter` 반환는 `double` 결과입니다. (아래에 표시 된 프로그램에는 값 변환기가 사용 됩니다 문자열 서식 지정과 관련 되므로 `ConvertBack` 사용 되지 않습니다.)

종류는 `parameter` 인수는 데이터 바인딩 코드 또는 XAML에서 정의 되었는지 여부에 따라 다를 수 있습니다. 경우는 `ConverterParameter` 속성 `Binding` 설정 되어 코드에서 될 숫자 값으로 설정:

```csharp
binding.ConverterParameter = 255;
```

`ConverterParameter` 속성은 형식이 `Object`이므로 C# 컴파일러를 나타내는 정수 리터럴 255를 해석 하 고 해당 값으로 속성을 설정 합니다.

그러나 XAML에서는 `ConverterParameter` 다음과 같이 설정할 수 있습니다.

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

때문에 숫자, 서로과 같은 255 형식 `ConverterParameter` 유형의 `Object`, XAML 구문 분석기를 문자열로 255를 처리 합니다.

이러한 이유로, 위에 표시 된 값 변환기는 별도 포함 `GetParameter` 에 대 한 사례를 처리 하는 메서드 `parameter` 형식의 `double`, `int`, 또는 `string`합니다.  

**RGB 색 선택기** 페이지 인스턴스화합니다 `DoubleToIntConverter` 리소스 사전의 다음 두 가지 암시적 스타일의 정의:

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

값은 `Red` 및 `Green` 속성으로 표시 됩니다는 `Binding` 태그 확장 합니다. 그러나 `Blue` 속성, 인스턴스화하는 `Binding` 명시적 방법을 보여 주기 위해 클래스 `double` 값 설정할 수 있습니다 `ConverterParameter` 속성.

다음은 결과가입니다.

[![RGB 색 선택기](converters-images/rgbcolorselector-small.png "RGB 색 선택기")](converters-images/rgbcolorselector-large.png#lightbox "RGB 색 선택기")


## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
