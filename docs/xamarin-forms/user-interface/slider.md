---
title: 슬라이더를 사용 하 여
description: 연속 값의 범위에서를 선택 하는 슬라이더를 사용 합니다.
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/16/2018
ms.openlocfilehash: 99109f6377037ffb9f622b7ddb237b42d241e505
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="using-slider"></a>슬라이더를 사용 하 여

_연속 값의 범위에서를 선택 하는 슬라이더를 사용 합니다._

Xamarin.Forms는 [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) 은 선택 하는 사용자가 조작할 수 있는 가로 막대는 `double` 연속 된 범위에서 값입니다. 

`Slider` 유형의 세 가지 속성을 정의 `double`:

- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) 기본값인 0으로, 범위의 최소가입니다.
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) 기본값은 1에서 범위의 최대값이입니다.
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) 슬라이더의 값을 지정할 수 있습니다 이므로 `Minimum` 및 `Maximum` 이며 0의 기본값은입니다.

모든 세 가지 속성에 의해 지원 됩니다 `BindableProperty` 개체입니다. `Value` 속성의 기본 바인딩 모드에 `BindingMode.TwoWay`를 사용 하는 응용 프로그램에서 바인딩 소스로는 [모델-뷰-MVVM ()](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처. 

> [!WARNING]
> 내부적으로 `Slider` 되도록 `Minimum` 는 보다 작은 `Maximum`합니다. 경우 `Minimum` 또는 `Maximum` 적이 설정 되어 있도록 `Minimum` 은 보다 작지 않음 `Maximum`, 예외가 발생 합니다. 참조는 [ **예방 조치** ](#precautions) 설정에 대 한 자세한 내용은 아래 섹션에서 `Minimum` 및 `Maximum` 속성입니다.

`Slider` 강제 변환의 `Value` 한다는 범위에 속함 속성 `Minimum` 및 `Maximum`(포함). 경우는 `Minimum` 속성이 보다 큰 값으로 설정 된는 `Value` 속성에는 `Slider` 설정는 `Value` 속성을 `Minimum`합니다. 마찬가지로, 경우 `Maximum` 값으로 설정 보다 작은 `Value`, 다음 `Slider` 설정는 `Value` 속성을 `Maximum`합니다. 

`Slider` 정의 [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) 는 발생 이벤트는 `Value` 사용자 조작을 통해 변경 내용을 `Slider` 설정 되므로 또는 `Value` 속성을 직접 합니다. A `ValueChanged` 도 이벤트가 실행 될 때는 `Value` 속성에 이전 단락에 설명 된 대로 강제 변환 됩니다. 

[ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) 함께 제공 되는 개체는 `ValueChanged` 이벤트에 두 가지 속성 형식의 둘 다 `double`: [ `OldValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.OldValue/) 및 [ `NewValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.NewValue/). 시간에는 이벤트가 발생의 값 `NewValue` 동일는 `Value` 속성의는 `Slider` 개체입니다.

> [!WARNING]
> 제약을 받지 않는 가로 레이아웃 옵션을 사용 하지 마십시오 `Center`, `Start`, 또는 `End` 와 `Slider`합니다. Android와 UWP에 `Slider` 축소 막대의 길이가 0 및 ios에서 막대에 정도로 매우 짧습니다. 기본값을 그대로 유지 `HorizontalOptions` 설정인 `Fill`의 너비를 사용 하지 않습니다 `Auto` 설정할 때 `Slider` 에 `Grid` 레이아웃 합니다.

## <a name="basic-slider-code-and-markup"></a>기본 슬라이더 코드 및 태그

[ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) 기능적으로 동일 하지만 다른 방법으로 구현 하는 세 개의 페이지와 샘플을 시작 합니다. C# 코드만 사용 하 여 첫 번째 페이지, 두 번째 XAML를 이벤트 처리기에 코드를 사용 하 여 및 세 번째는 XAML 파일에서 데이터 바인딩을 사용 하 여 이벤트 처리기를 필요가 없습니다.

### <a name="creating-a-slider-in-code"></a>코드에서 슬라이더를 만드는 방법

**기본 슬라이더 코드** 페이지에 [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) 예제를 만들려면 표시를 보여 줍니다.는 `Slider` 와 두 개의 `Label` 코드에서 개체:

```csharp
public class BasicSliderCodePage : ContentPage
{
    public BasicSliderCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Slider slider = new Slider
        {
            Maximum = 360
        };
        slider.ValueChanged += (sender, args) =>
        {
            rotationLabel.Rotation = slider.Value;
            displayLabel.Text = String.Format("The Slider value is {0}", args.NewValue);
        };

        Title = "Basic Slider Code";
        Padding = new Thickness(10, 0);
        Content = new StackLayout
        {
            Children =
            {
                rotationLabel,
                slider,
                displayLabel
            }
        };
    }
}
```

`Slider` 있어야 초기화는 `Maximum` 360의 속성입니다. `ValueChanged` 의 처리기는 `Slider` 사용 하 여는 `Value` 속성의는 `slider` 설정할 개체는 `Rotation` 첫 번째 속성 `Label` 사용 하 여는 `String.Format` 사용 하 여 메서드는 `NewValue` 속성의는 설정 하는 이벤트 인수는 `Text` 두 번째 속성 `Label`합니다. 이 두 가지 방법의 현재 값을 가져옵니다는 `Slider` 은 서로 전환이 가능 합니다. 

다음은 장치, iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 실행 되는 프로그램입니다.

[![기본 슬라이더 코드](slider-images/BasicSliderCode.png "기본 슬라이더 코드")](slider-images/BasicSliderCode-Large.png#lightbox)

두 번째 `Label` 될 때까지 "(초기화 되지 않음)" 텍스트를 표시는 `Slider` 조작 되는 경우 첫 번째 `ValueChanged` 이벤트를 발생 합니다. 표시 되는 소수 자릿수는 세 플랫폼에 대 한 다른 표시 됩니다. 이러한 차이점의 플랫폼 구현 관련 되어는 `Slider` 섹션에서이 문서의 뒷부분에서 설명 하 고 [플랫폼 구현 차이](#implementations)합니다.

### <a name="creating-a-slider-in-xaml"></a>XAML에서 슬라이더를 만들기

**기본 슬라이더 XAML** 페이지는 기능적으로 동일 **기본 슬라이더 코드** XAML에서 주로 구현 하지만:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderXamlPage"
             Title="Basic Slider XAML"
             Padding="10, 0">
    <StackLayout>
        <Label x:Name="rotatingLabel" 
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider Maximum="360"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

코드 숨김 파일에 대 한 처리기가 포함 된 `ValueChanged` 이벤트:

```csharp
public partial class BasicSliderXamlPage : ContentPage
{
    public BasicSliderXamlPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double value = args.NewValue;
        rotatingLabel.Rotation = value;
        displayLabel.Text = String.Format("The Slider value is {0}", value);
    }
}
```

이벤트 처리기를 가져올 수 이기도 `Slider` 를 통해 이벤트를 발생 하는 `sender` 인수입니다. `Value` 속성 현재 값을 포함 합니다.

```csharp
double value = ((Slider)sender).Value;
```

경우는 `Slider` 개체에 사용 하 여 XAML 파일에 이름이 지정 된는 `x:Name` 특성 (예: "slider")을 설정한 다음 이벤트 처리기 해당 개체를 직접 참조할 수 있습니다.

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>데이터 바인딩 슬라이더

**기본 슬라이더 바인딩** 페이지를 제거 하는 거의 같은 프로그램을 작성 하는 방법을 보여 줍니다는 `Value` 이벤트 처리기를 사용 하 여 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderBindingsPage"
             Title="Basic Slider Bindings"
             Padding="10, 0">
    <StackLayout>
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference slider}, 
                                  Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360" />

        <Label x:Name="displayLabel"
               Text="{Binding Source={x:Reference slider}, 
                              Path=Value, 
                              StringFormat='The Slider value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Rotation` 첫 번째 속성 `Label` 바인딩되는 `Value` 의 속성은 `Slider`을 있는 그대로 `Text` 두 번째 속성 `Label` 와 `StringFormat` 사양입니다. **기본 슬라이더 바인딩** 페이지 함수 약간 다르게 두 이전 페이지에서: 페이지가 처음 나타날 때, 두 번째 `Label` 값과 텍스트 문자열을 표시 합니다. 데이터 바인딩을 사용 하 여의 장점입니다. 텍스트 데이터를 표시 하려면 구체적으로 초기화 해야는 `Text` 속성의는 `Label` 하거나의 발생을 시뮬레이트하는 `ValueChanged` 클래스 생성자에서 이벤트 처리기를 호출 하 여 이벤트입니다.

<a name="precautions" />

## <a name="precautions"></a>예방 조치

값은 `Minimum` 속성의 값 보다 작을 수 항상 해야는 `Maximum` 속성입니다. 다음 코드 조각 원인을 `Slider` 에서 예외가 발생 하 합니다.

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

시퀀스에서 이러한 두 속성을 설정 하는 코드를 생성 하는 C# 컴파일러 및 시기는 `Minimum` 속성은 10으로 설정, 값이 기본값 보다 큰 `Maximum` 값이 1입니다. 설정 하 여이 경우는 예외를 방지할 수 있습니다는 `Maximum` 속성 첫 번째:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

설정 `Maximum` 20으로 문제가 되지 않습니다는 기본값 보다 크기 때문에 `Minimum` 0을 설정 합니다. 때 `Minimum` 이 값은 설정 보다 작은 `Maximum` 값이 20입니다.

XAML에 동일한 문제가 있습니다. 속성을 확인 하는 순서로 설정 `Maximum` 보다 항상 큽니다 `Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

설정할 수 있습니다는 `Minimum` 및 `Maximum` 음수 값에 있지만 주문에만 값 여기서 `Minimum` 은 항상 미만 `Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

`Value` 속성은 항상 보다 크거나 같은 경우는 `Minimum` 값과 같거나 작은 `Maximum`합니다. 경우 `Value` 설정 된 범위를 벗어난 값으로 값은으로 강제 변환할 수는 범위 내에 포함 되지만 예외가 발생 합니다. 예를 들어이 코드에서는 *하지* 예외를 발생 시킵니다.

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

대신,는 `Value` 속성으로 강제 변환 되는 `Maximum` 값이 1입니다.

위에 표시 된 코드 조각은 다음과 같습니다. 

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

때 `Minimum` 를 10으로 설정한 후 `Value` 10로 설정 됩니다. 

경우는 `ValueChanged` 때 이벤트 처리기가 연결 하는 `Value` 속성은 기본값인 0 이외의 값으로 강제 변환 되는 `ValueChanged` 이벤트가 발생 합니다. XAML의 코드 조각은 다음과 같습니다. 

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

때 `Minimum` 를 10으로 설정 되어 `Value` 10로 설정 됩니다 및 `ValueChanged` 이벤트가 발생 합니다. 이 페이지의 나머지를 생성 하 고 처리기가 아직 만들어지지 않은 페이지에서 다른 요소를 참조 하려고 하기 전에 발생할 수 있습니다. 일부 코드를 추가할 수는 `ValueChanged` 이벤트 처리기에 대 한 검사를 `null` 페이지에 있는 다른 요소의 값입니다. 또는 설정할 수는 `ValueChanged` 후 이벤트 처리기는 `Slider` 초기화 된 값입니다.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>플랫폼 구현 차이

값을 표시 하는 앞에 표시 된 스크린샷을 보려면은 `Slider` 다른 수가 소수점이 하입니다. 이 방법을 관련이 `Slider` UWP 및 Android 플랫폼에 구현 됩니다.

### <a name="the-android-implementation"></a>Android 구현 

Android 구현의 `Slider` Android 기반 [ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/) 항상 설정 하 고는 [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) 1000 속성입니다. 즉는 `Slider` android에 1,001 불연속 값입니다. 설정 하는 경우는 `Slider` 있어야는 `Minimum` 0 및 `Maximum` 5000을 다음으로 `Slider` 조작 되는 `Value` 속성에 0, 5, 10, 15, 등의 값입니다. 

### <a name="the-uwp-implementation"></a>UWP 구현

UWP 구현의 `Slider` UWP 기반 [ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider) 제어 합니다. `StepFrequency` UWP의 속성 `Slider` 의 차이로 설정 되는 `Maximum` 및 `Minimum` 속성 10, 하지만 1 보다 크지 않음로 나눈 값입니다. 

0-1의 기본 범위에 대 한 예를 들어는 `StepFrequency` 0.1 속성이 설정 되어 있습니다. 로 `Slider` 조작 되는 `Value` 속성은 0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9 및 1.0으로 제한 합니다. (이것은의 마지막 페이지에서 분명 하 게는 [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) 샘플.) 때 간의 차이 `Maximum` 및 `Minimum` 다음 속성은 10 이상 `StepFrequency` 1로 설정 및 `Value` 속성에는 정수 계열 값입니다. 

### <a name="the-stepslider-solution"></a>StepSlider 솔루션

더 다양 한 `StepSlider` 에 대해서는 설명 [장 27입니다. 사용자 지정 렌더러](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) 책의 *Xamarin.Forms 사용 하 여 모바일 앱 만들기*합니다. `StepSlider` 비슷합니다 `Slider` 하지만 추가 `Steps` 사이 값의 수를 지정 하려면 속성 `Minimum` 및 `Maximum`합니다.

## <a name="sliders-for-color-selection"></a>색 선택에 대 한 슬라이더

마지막 두 페이지에 [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) 둘 다 사용 하 여 3 개 샘플 `Slider` 색 선택에 대 한 인스턴스. 첫 번째 페이지의 두 번째 페이지는 ViewModel와 데이터 바인딩을 사용 하는 방법을 표시 하는 동안 코드 숨김 파일의 모든 상호 작용을 처리 합니다. 

### <a name="handling-sliders-in-the-code-behind-file"></a>코드 숨김 파일에서 슬라이더를 처리합니다. 

**RGB 색 슬라이더** 페이지를 인스턴스화하는 `BoxView` 색을 표시 하려면 세 개의 `Slider` 인스턴스 세 개와 표시 되는 색의 빨강, 녹색 및 파랑 구성 요소를 선택할 `Label` 해당 색을 표시 하기 위한 요소 값:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.RgbColorSlidersPage"
             Title="RGB Color Sliders">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="Maximum" Value="255" />
            </Style>
            
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView x:Name="boxView"
                 Color="Black"
                 VerticalOptions="FillAndExpand" />

        <Slider x:Name="redSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="redLabel" />

        <Slider x:Name="greenSlider" 
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="greenLabel" />

        <Slider x:Name="blueSlider" 
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="blueLabel" />
    </StackLayout>
</ContentPage>
```

A `Style` 3을 모두 제공 `Slider` 0 ~ 255 범위의 한 요소입니다. `Slider` 요소가 동일한 공유 `ValueChanged` 코드 숨김 파일에서 구현 되는 처리기.

```csharp
public partial class RgbColorSlidersPage : ContentPage
{
    public RgbColorSlidersPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (sender == redSlider)
        {
            redLabel.Text = String.Format("Red = {0:X2}", (int)args.NewValue);
        }
        else if (sender == greenSlider)
        {
            greenLabel.Text = String.Format("Green = {0:X2}", (int)args.NewValue);
        }
        else if (sender == blueSlider)
        {
            blueLabel.Text = String.Format("Blue = {0:X2}", (int)args.NewValue);
        }

        boxView.Color = Color.FromRgb((int)redSlider.Value,
                                      (int)greenSlider.Value,
                                      (int)blueSlider.Value);
    }
}
```

첫 번째 섹션 집합은 `Text` 속성 중 하나는 `Label` 값을 나타내는 짧은 텍스트 문자열에 인스턴스는 `Slider` 16 진수에서입니다. 그런 다음 세 가지 모두 `Slider` 인스턴스에 만들려면 액세스할는 `Color` RGB 구성 요소에서 값:

[![RGB 색 슬라이더](slider-images/RgbColorSliders.png "RGB 색 슬라이더")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>슬라이더는 ViewModel에 바인딩

**HSL 색 슬라이더** 페이지를 만드는 데 사용 하는 계산을 수행 하는 ViewModel를 사용 하는 방법을 표시는 `Color` 색상, 채도 및 명도 값에서 값입니다. 마찬가지로 모든 Viewmodel는 `HSLColorViewModel` 클래스 구현은 `INotifyPropertyChanged` 인터페이스 및 발생 한 `PropertyChanged` 속성 중 하나가 변경 될 때마다 이벤트:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get 
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));
            }
        }
        get
        {
            return color;
        }
    }
}
```

Viewmodel 및 `INotifyPropertyChanged` 인터페이스는 문서에서 설명한 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)합니다.

**HslColorSlidersPage.xaml** 파일 인스턴스화하는 `HslColorViewModel` 하 고 페이지의 설정 `BindingContext` 속성입니다. 그러면 ViewModel에서 속성에 바인딩할 XAML 파일의 모든 요소가 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SliderDemos"
             x:Class="SliderDemos.HslColorSlidersPage"
             Title="HSL Color Sliders">

    <ContentPage.BindingContext>
        <local:HslColorViewModel Color="Chocolate" />
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</ContentPage> 
```

로 `Slider` 요소를 조작할는 `BoxView` 및 `Label` ViewModel에서 요소가 업데이트 됩니다.

[![HSL 색 슬라이더](slider-images/HslColorSliders.png "HSL 색 슬라이더")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat` 의 구성 요소는 `Binding` 태그 확장 f "2"의 형식에 대 한 두 소수 자릿수를 표시 하도록 설정 합니다. (문자열 데이터 바인딩이 서식 지정의 문서에 설명 된 [문자열 서식 지정](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) 그러나 UWP 버전의 프로그램 0, 0.1, 0.2, 요소의 값으로 제한 됩니다... 0.9 및 1.0입니다. UWP의 구현에 대 한 직접 결과로 `Slider` 위의 섹션에서 설명한 대로 [플랫폼 구현 차이](#implementations)합니다.

## <a name="related-links"></a>관련 링크

- [슬라이더 데모 샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [슬라이더 API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)
