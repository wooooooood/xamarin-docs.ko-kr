---
title: Xamarin.Forms 바인딩 값 변환기
description: 이 문서에는 캐스팅 되거나 (임 라고도 바인딩 변환기 또는 바인딩 값 변환기)는 값 변환기를 구현 하 여 Xamarin.Forms 데이터 바인딩에 대 한 값을 변환 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 28892692133020de1fa5a6eb007bb3f9bcf2612b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997486"
---
# <a name="xamarinforms-binding-value-converters"></a>Xamarin.Forms 바인딩 값 변환기

데이터 바인딩을 일반적으로 송수신 데이터 원본 속성의 대상 속성을 소스 속성에 대상 속성에서 일부 경우에 합니다. 이 전송을 간단 하 게 원본 및 대상 속성을 동일한 형식의 경우 형식 변환 하는 암시적 변환을 통해 다른 유형으로 변환할 수 있습니다. 대/소문자는 없는 경우 형식 변환을 수행 해야 합니다.

에 [ **문자열 서식 지정** ](string-formatting.md) 문서를 사용 하는 방법을 살펴보았습니다는 `StringFormat` 모든 형식을 문자열로 변환에 대 한 데이터 바인딩의 속성입니다. 다른 유형의 변환 구현 하는 클래스에서 일부 특수 한 코드를 작성 해야 합니다 [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) 인터페이스입니다. (유사한 클래스를 포함 하는 유니버설 Windows 플랫폼 [ `IValueConverter` ](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) 에 `Windows.UI.Xaml.Data` 이 네임 스페이스 `IValueConverter` 에 `Xamarin.Forms` 네임 스페이스입니다.) 구현 하는 클래스 `IValueConverter` 이라고 *값 변환기*에 종종 라고 하지만 *바인딩 변환기* 또는 *바인딩 값 변환기*합니다.

## <a name="the-ivalueconverter-interface"></a>IValueConverter 인터페이스

형식의 원본 속성은 데이터 바인딩을 정의 하기를 원한다고 가정 `int` 는 대상 속성을 `bool`입니다. 이 데이터 바인딩이 생성 하려는 `false` 정수 원본이 0 인 경우 값 및 `true` 그렇지 않은 경우.  

구현 하는 클래스를 사용 하 여이 수행할 수는 `IValueConverter` 인터페이스:

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

이 클래스의 인스턴스를 설정 합니다 [ `Converter` ](xref:Xamarin.Forms.Binding.Converter) 의 속성을 `Binding` 클래스 또는 [ `Converter` ](xref:Xamarin.Forms.Xaml.BindingExtension.Converter) 속성은 `Binding` 태그 확장. 이 클래스에는 데이터 바인딩의 일부가 됩니다.

합니다 `Convert` 메서드는 원본에서 대상의 데이터 이동 `OneWay` 또는 `TwoWay` 바인딩. `value` 개체 또는 데이터 바인딩 원본의 값 매개 변수입니다. 메서드는 데이터 바인딩된 대상 형식의 값을 반환 해야 합니다. 캐스트 여기 표시 된 메서드를 `value` 매개 변수를는 `int` 과 0을 사용 하 여 비교를 `bool` 값을 반환 합니다.

합니다 `ConvertBack` 메서드는 소스에 대상에서 데이터 이동 `TwoWay` 또는 `OneWayToSource` 바인딩. `ConvertBack` 변환 수행: 것으로 가정 합니다 `value` 매개 변수는를 `bool` 대상에서으로 변환 하 고는 `int` 원본에 대 한 값을 반환 합니다.

데이터 바인딩에 포함 된 경우는 `StringFormat` 설정, 값 변환기는 전에 호출 결과 문자열로 서식이 지정 된 합니다.

합니다 **단추를 사용 하도록 설정** 페이지에 [ **데이터 바인딩 데모** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) 샘플 데이터 바인딩에이 값 변환기를 사용 하는 방법을 보여 줍니다. `IntToBoolConverter` 페이지의 리소스 사전에 인스턴스화됩니다. 사용 하 여 다음 참조를 `StaticResource` 태그 확장을 설정 하는 `Converter` 두 데이터 바인딩에서 속성. 것은 매우 일반적인 페이지의 여러 데이터 바인딩 간에 데이터 변환기를 공유 하려면:

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

값 변환기를 응용 프로그램의 여러 페이지에서 사용 하는 경우의 리소스 사전에 인스턴스화할 수 있습니다 합니다 **App.xaml** 파일입니다.

합니다 **단추를 사용 하도록 설정** 페이지 일반적인 경우 해야 하는 방법을 보여 줍니다는 `Button` 에 사용자가 텍스트에 따라 작업을 수행을 `Entry` 보기. 에 입력 된 아무 작업도 수행 하는 경우는 `Entry`, `Button` 수 없게 됩니다. 각 `Button` 에 데이터 바인딩이 포함 해당 `IsEnabled` 속성입니다. 데이터 바인딩 원본이 합니다 `Length` 의 속성을 `Text` 해당 속성 `Entry`. 있는지 `Length` 속성은 0, 값 변환기를 반환 합니다 `true` 하며 `Button` 사용 가능:

[![단추를 사용 하도록 설정](converters-images/enablebuttons-small.png "단추를 사용 하도록 설정")](converters-images/enablebuttons-large.png#lightbox "단추를 사용 하도록 설정")

다음에 유의 합니다 `Text` 각 속성 `Entry` 빈 문자열로 초기화 됩니다. 합니다 `Text` 속성은 `null` 기본적으로 데이터 바인딩은 작동 하지 것입니다 경우.

일부 값 변환기는 다른 일반화 하는 동안 특정 응용 프로그램만 위한 기록 됩니다. 값 변환기에만 사용할 수는 알고 있는 경우 `OneWay` 바인딩, 해당 `ConvertBack` 메서드를 반환할 수 있습니다 `null`합니다.

`Convert` 가정 암시적으로 위에 나와 있는 메서드에 `value` 형식의 인수가 `int` 반환 값 형식 이어야 하 고 `bool`. 마찬가지로,는 `ConvertBack` 가정 메서드는 `value` 형식의 인수가 `bool` 및 반환 값은 `int`. 하지 않은 경우 런타임 예외가 발생 합니다.

값 변환기 보다 일반화 및 여러 다른 유형의 데이터를 허용 하도록 작성할 수 있습니다. `Convert` 및 `ConvertBack` 메서드를 사용할 수는 `as` 또는 `is` 연산자를 합니다 `value` 매개 변수를 하거나 호출할 수 `GetType` 해당 유형 및 다음 작업 항목 확인 하려면 해당 매개 변수에 적절 한 합니다. 각 메서드의 반환 값의 예상된 형식을 지정 하 여는 `targetType` 매개 변수입니다. 값 변환기 다양 한 대상 유형에;의 데이터 바인딩과 함께 사용 되는 경우에 따라 값 변환기를 사용할 수는 `targetType` 올바른 형식에 대 한 변환을 수행 하는 인수입니다.

사용 하 여 변환이 수행 되 고 다른 문화권에 대 한 다른 경우는 `culture` 이 목적을 위해 매개 변수입니다. 합니다 `parameter` 인수를 `Convert` 및 `ConvertBack` 이 문서의 뒷부분에서 설명 됩니다.

## <a name="binding-converter-properties"></a>바인딩 변환기 속성

속성 및 일반 매개 변수 값 변환기 클래스가 있을 수 있습니다. 이 특정 값 변환기에서 변환 된 `bool` 형식의 개체를 원본에서 `T` 대상:

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

합니다 **스위치 표시기** 페이지의 값을 표시 하 고 있습니다 사용 하는 방법을 보여 줍니다는 `Switch` 보기. 이 페이지는 대체 방법을 보여 줍니다 일반적인 리소스 사전에 리소스로 값 변환기를 인스턴스화하는 아니지만: 각 값 변환기 간에 인스턴스화됩니다 `Binding.Converter` 속성 요소 태그입니다. 합니다 `x:TypeArguments` 제네릭 인수를 나타냅니다 및 `TrueObject` 및 `FalseObject` 해당 형식의 개체에 설정 됩니다.

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

마지막에서 세 가지 `Switch` 및 `Label` 쌍 제네릭 인수 설정할지 `Style`, 전체 `Style` 개체의 값에 대해 제공 됩니다 `TrueObject` 및 `FalseObject`합니다. 이러한 재정의 대 한 암시적 스타일 `Label` 리소스 사전에 설정 되므로 해당 스타일의 속성을 명시적으로 할당 된는 `Label`합니다. 설정/해제 합니다 `Switch` 해당 하면 `Label` 변경 내용을 반영 하려면:

[![표시기 전환](converters-images/switchindicators-small.png "표시기 전환")](converters-images/switchindicators-large.png#lightbox "지표를 전환 합니다.")

사용할 수 이기도 [ `Triggers` ](~/xamarin-forms/app-fundamentals/triggers.md) 다른 뷰를 기반으로 사용자 인터페이스에 비슷한 변경을 구현 합니다.

## <a name="binding-converter-parameters"></a>변환기 매개 변수 바인딩

`Binding` 클래스 정의 [ `ConverterParameter` ](xref:Xamarin.Forms.Binding.ConverterParameter) 속성 및 `Binding` 태그 확장도 정의 [ `ConverterParameter` ](xref:Xamarin.Forms.Xaml.BindingExtension.ConverterParameter) 속성입니다. 이 속성이 설정 되어 있으면 값이 전달 되는 `Convert` 하 고 `ConvertBack` 메서드를 `parameter` 인수. 여러 데이터 바인딩 값 변환기 인스턴스의 공유 하는 경우에는 `ConverterParameter` 약간 다른 변환을 수행 하는 다를 수 있습니다.

사용 `ConverterParameter` 색 선택 영역 프로그램을 사용 하 여 보여 줍니다. 이 경우에 `RgbColorViewModel` 형식의 세 가지 속성이 있습니다 `double` 라는 `Red`를 `Green`, 및 `Blue` 생성 하는 데 사용 하는 `Color` 값:

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

합니다 `Red`, `Green`, 및 `Blue` 0과 1 사이의 속성 범위입니다. 그러나 두 자리 16 진수 값으로 구성 요소를 표시 하도록 하는 것이 좋습니다.

16 진수 값으로이 XAML에 표시 하려면 이러한 여야 255를 곱하고, 정수로 변환 하며 다음에서 "X2"의 사양으로 형식이 지정 된 `StringFormat` 속성입니다. 값 변환기에서 처음 두 작업 (255를 곱하여 및 정수 변환)를 처리할 수 있습니다. 최대한 일반화 된 것으로 값 변환기가 하도록 곱하기 비율을 지정할 수 있습니다는 `ConverterParameter` 가 해당 의미 하는 속성을 `Convert` 및 `ConvertBack` 메서드를 `parameter` 인수:

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

`Convert` 에서 변환를 `double` 를 `int` 곱하여 하는 동안를 `parameter` ; 값를 `ConvertBack` 정수를 나누고 `value` 인수를 `parameter` 반환을 `double` 결과. (아래에 표시 된 프로그램에서 값 변환기는만 관련 하 여 문자열 서식 지정, 따라서 `ConvertBack` 사용 되지 않습니다.)

형식의 `parameter` 인수는 코드 또는 XAML 데이터 바인딩을 정의 여부에 따라 다를 수 있습니다. 경우는 `ConverterParameter` 속성의 `Binding` 설정 된 코드에서 숫자 값으로 설정할 수는:

```csharp
binding.ConverterParameter = 255;
```

합니다 `ConverterParameter` 형식의 속성이 `Object`이므로 C# 컴파일러는 정수 리터럴 255를 해석 하 고 해당 값으로 속성을 설정 합니다.

그러나 XAML에는 `ConverterParameter` 다음과 같이 설정할 수:

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

255 같습니다 number, 하지만 때문에 `ConverterParameter` 유형의 `Object`, XAML 파서를 문자열로 255를 처리 합니다.

따라서 위의 값 변환기는 별도 포함 `GetParameter` 에 대 한 사례를 처리 하는 메서드 `parameter` 형식의 `double`를 `int`, 또는 `string`합니다.  

합니다 **RGB 색 선택기** 페이지를 인스턴스화하고 `DoubleToIntConverter` 두 암시적 스타일의 정의 다음 해당 리소스 사전에:

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

값을 `Red` 및 `Green` 속성으로 표시 됩니다는 `Binding` 태그 확장 합니다. 그러나 `Blue` 속성인 인스턴스화하는 `Binding` 명시적 방법을 보여 주기 위해 클래스 `double` 값 설정할 수 있습니다 `ConverterParameter` 속성입니다.

결과 다음과 같습니다.

[![RGB 색 선택기](converters-images/rgbcolorselector-small.png "RGB 색 선택기")](converters-images/rgbcolorselector-large.png#lightbox "RGB 색 선택기")


## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
