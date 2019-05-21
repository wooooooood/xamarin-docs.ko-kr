---
title: Xamarin.Forms 슬라이더
description: Xamarin.Forms 슬라이더는 연속 된 범위에서 double 값을 선택 하는 사용자가 조작할 수 있는 가로 막대입니다. 이 문서에서는 슬라이더 클래스를 사용 하 여 연속 값의 범위에서 값을 선택 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2019
ms.openlocfilehash: 6e65124df4b20a50091ad93e18621f8e6707ebbe
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970546"
---
# <a name="xamarinforms-slider"></a>Xamarin.Forms 슬라이더

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)

_연속 값의 범위에서 선택 하는 슬라이더를 사용 합니다._

Xamarin.Forms [ `Slider` ](xref:Xamarin.Forms.Slider) 은 선택 하는 사용자가 조작할 수 있는 가로 막대를 `double` 연속 범위 값입니다.

합니다 `Slider` 유형의 세 가지 속성을 정의 `double`:

- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) 기본값은 0 사용 하 여 범위의 최소값이입니다.
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) 1의 기본값을 사용 하 여 범위의 최대값입니다.
- [`Value`](xref:Xamarin.Forms.Slider.Value) 범위 슬라이더의 값 이므로 `Minimum` 및 `Maximum` 있고 기본값은 0입니다.

세 가지 속성 모두를 지 원하는 `BindableProperty` 개체입니다. `Value` 속성이의 기본 바인딩 모드를 `BindingMode.TwoWay`, 즉, 사용 하는 응용 프로그램에서 바인딩 소스로 적합 한지를 [-Model-view-viewmodel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처입니다.

> [!WARNING]
> 내부적으로 `Slider` 되도록 `Minimum` 는 보다 작은 `Maximum`합니다. 하는 경우 `Minimum` 또는 `Maximum` 적이 설정 되어 있도록 `Minimum` 는 보다 작지 않음 `Maximum`, 예외가 발생 합니다. 참조를 [ **예방 조치** ](#precautions) 설정에 대 한 자세한 내용은 아래 섹션의 `Minimum` 및 `Maximum` 속성입니다.

합니다 `Slider` 강제 변환 합니다 `Value` 속성 간에 되도록 `Minimum` 및 `Maximum`(포함). 경우는 `Minimum` 속성 보다 큰 값으로 설정 됩니다는 `Value` 속성을 `Slider` 설정를 `Value` 속성을 `Minimum`. 마찬가지로, 경우 `Maximum` 값으로 설정 됩니다 보다 작은 `Value`, 다음 `Slider` 설정의 `Value` 속성을 `Maximum`입니다.

`Slider` 정의 [ `ValueChanged` ](xref:Xamarin.Forms.Slider.ValueChanged) 때 발생 하는 이벤트를 `Value` 의 사용자 조작을 통해 변경 내용을 합니다 `Slider` 하거나 프로그램을 설정 하는 경우는 `Value` 속성을 직접. A `ValueChanged` 도 이벤트가 실행 될 때를 `Value` 속성에는 이전 단락에 설명 된 대로 강제 변환 됩니다.

[ `ValueChangedEventArgs` ](xref:Xamarin.Forms.ValueChangedEventArgs) 와 함께 제공 되는 개체를 `ValueChanged` 이벤트 라는 두 가지 속성이 형식 둘 다 `double`: [ `OldValue` ](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) 및 [ `NewValue` ](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue). 시 이벤트 발생의 값 `NewValue` 동일 합니다 `Value` 의 속성은 `Slider` 개체.

`Slider` 또한 정의 `DragStarted` 고 `DragCompleted` 끌기 작업의 시작과 끝에서 발생 하는 이벤트입니다. 달리 합니다 [ `ValueChanged` ](xref:Xamarin.Forms.Slider.ValueChanged) 이벤트를 `DragStarted` 및 `DragCompleted` 만 이벤트가 사용자 조작을 통해는 `Slider`합니다. 경우는 `DragStarted` 이벤트가 발생 합니다 `DragStartedCommand`, 형식의 `ICommand`, 실행 됩니다. 마찬가지로,는 `DragCompleted` 이벤트가 발생 합니다 `DragCompletedCommand`, 형식의 `ICommand`, 실행 됩니다.

> [!WARNING]
> 제약을 받지 않는 가로 레이아웃 옵션을 사용 하지 마십시오 `Center`, `Start`, 또는 `End` 사용 하 여 `Slider`입니다. Android 및 UWP, 모두는 `Slider` 길이가 0 인 및 iOS의 경우 막대에 막대에는 축소 정도로 매우 짧습니다. 기본값을 유지 `HorizontalOptions` 설정 `Fill`, 고의 너비를 사용 하지 않는 `Auto` 전환할 때 `Slider` 에 `Grid` 레이아웃 합니다.

`Slider` 도 해당 모양에 영향을 주는 몇 가지 속성을 정의 합니다.

- [`MinimumTrackColor`](xref:Xamarin.Forms.Slider.MinimumTrackColorProperty) 막대는 왼쪽 엄지 단추의 색입니다.
- [`MaximumTrackColor`](xref:Xamarin.Forms.Slider.MaximumTrackColorProperty) 막대는 오른쪽에 있는 엄지 단추의 색입니다.
- [`ThumbColor`](xref:Xamarin.Forms.Slider.ThumbColorProperty) thumb 색이 됩니다.
- [`ThumbImageSource`](xref:Xamarin.Forms.Slider.ThumbImageSourceProperty) 형식의 thumb을 사용 하는 이미지인 [ `ImageSource` ](xref:Xamarin.Forms.ImageSource)합니다.

> [!NOTE]
> 합니다 `ThumbColor` 고 `ThumbImageSource` 속성은 함께 사용할 수 없습니다. 두 속성을 설정 하는 경우는 `ThumbImageSource` 속성 우선 적용 됩니다.

## <a name="basic-slider-code-and-markup"></a>기본 슬라이더 코드와 태그

합니다 [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) 기능적으로 동일 하지만 다른 방법으로 구현 되는 세 개의 페이지를 사용 하 여 샘플을 시작 합니다. C# 코드만 사용 하 여 첫 번째 페이지, 코드에서 이벤트 처리기를 사용 하 여 XAML을 사용 하는 두 번째 및 세 번째는 XAML 파일에서 데이터 바인딩을 사용 하 여 이벤트 처리기를 피할 수는 없습니다.

### <a name="creating-a-slider-in-code"></a>코드에서 슬라이더를 만드는 방법

합니다 **기본 슬라이더 코드** 페이지에 [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) 샘플 만들려면 표시를 보여 줍니다.를 `Slider` 두 개의 `Label` 코드의 개체:

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

합니다 `Slider` 하도록 초기화 됩니다는 `Maximum` 360의 속성입니다. `ValueChanged` 처리기를 `Slider` 사용 하 여를 `Value` 의 속성을 `slider` 설정할 개체입니다를 `Rotation` 첫 번째 속성 `Label` 사용 하는 `String.Format` 메서드를 `NewValue` 의 속성을 설정에 대 한 이벤트 인수를 `Text` 두 번째 속성 `Label`합니다. 이러한 두 가지 방법의 현재 값을 가져옵니다는 `Slider` 바꾸어 사용할 수 있습니다.

장치 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 실행 중인 프로그램이 다음과 같습니다.

[![기본 슬라이더 코드](slider-images/BasicSliderCode.png "기본 슬라이더 코드")](slider-images/BasicSliderCode-Large.png#lightbox)

두 번째 `Label` 될 때까지 "(초기화 되지 않음)" 텍스트를 표시 합니다 `Slider` 조작 되 하면 첫 번째 `ValueChanged` 이벤트를 발생 합니다. 각 플랫폼에 대해 표시 되는 소수 자릿수는 알 수 있습니다. 이러한 차이점의 플랫폼 구현에 관련 된 합니다 `Slider` 섹션에서이 문서의 뒷부분에서 설명 하 고 [플랫폼 구현 차이로](#implementations)합니다.

### <a name="creating-a-slider-in-xaml"></a>XAML에서 슬라이더를 만들기

합니다 **기본 슬라이더 XAML** 페이지는 기능적으로 동일 **기본 슬라이더 코드** 하지만 XAML에서 주로 구현 합니다.

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

이벤트 처리기를 가져올 수 이기도 합니다 `Slider` 를 통해 이벤트를 발생 하는 `sender` 인수. `Value` 속성 현재 값을 포함 합니다.

```csharp
double value = ((Slider)sender).Value;
```

경우는 `Slider` 개체에 사용 하 여 XAML 파일에 이름이 지정 된는 `x:Name` 특성 (예: "slider")을 설정한 다음 이벤트 처리기는 개체를 직접 참조할 수:

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>데이터 바인딩 슬라이더

합니다 **기본 슬라이더 바인딩을** 페이지를 제거 하는 거의 같은 프로그램을 작성 하는 방법을 보여 줍니다 합니다 `Value` 사용 하 여 이벤트 처리기 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md):

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

`Rotation` 첫 번째 속성 `Label` 바인딩되는 `Value` 의 속성을 `Slider`는 `Text` 두 번째 속성 `Label` 사용 하 여를 `StringFormat` 사양입니다. 합니다 **기본 슬라이더 바인딩을** 페이지 함수 약간 다르게 두 이전 페이지에서: 페이지가 처음 나타날 경우, 두 번째 `Label` 값을 사용 하 여 텍스트 문자열을 표시 합니다. 데이터 바인딩을 사용 하 여이 유용 합니다. 데이터 바인딩 없이 텍스트를 표시 하려면 특히 초기화 해야는 `Text` 의 속성을 `Label` 의 발생을 시뮬레이션 하거나는 `ValueChanged` 클래스 생성자에서 이벤트 처리기를 호출 하 여 이벤트.

<a name="precautions" />

## <a name="precautions"></a>주의 사항

값을 `Minimum` 속성은 항상 작아야 값 보다는 `Maximum` 속성. 다음 코드 조각은 원인의 `Slider` 예외를 발생 시키려면:

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

시퀀스에서 이러한 두 속성을 설정 하는 코드를 생성 하는 C# 컴파일러 및 시기를 `Minimum` 10 속성을 기본값 보다 큰 경우 `Maximum` 값이 1입니다. 설정 하 여이 경우 예외를 방지할 수는 `Maximum` 속성 첫 번째:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

설정 `Maximum` 20으로 문제가 되지 않습니다 기본값 보다 큰 이므로 `Minimum` 값이 0입니다. 때 `Minimum` 값이 설정 보다 작은 `Maximum` 값은 20입니다.

XAML에 동일한 문제가 있습니다. 되도록 하는 순서에에서 속성을 설정 `Maximum` 보다 항상 큽니다 `Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

설정할 수 있습니다를 `Minimum` 하 고 `Maximum` 순서로 음수를 값 위치 `Minimum` 는 보다 항상 작거나 `Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

합니다 `Value` 속성은 항상 보다 크거나 같은 경우는 `Minimum` 값 보다 작거나 같음 `Maximum`합니다. 경우 `Value` 설정 된 범위를 벗어난 값으로 값 범위 내에 배치 되도록 강제 됩니다 있지만 예외가 발생 하지 않습니다. 예를 들어이 코드에서는 *되지* 예외를 발생 시킵니다.

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

대신 합니다 `Value` 속성은 문자열로 변환 합니다 `Maximum` 값이 1입니다.

위의 코드 조각은 다음과 같습니다.

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

때 `Minimum` 10으로 설정 됩니다 `Value` 10도 설정 됩니다.

경우는 `ValueChanged` 시점에 이벤트 처리기에 연결 하는 `Value` 속성은 기본값인 0 이외의 값으로 강제 변환는 `ValueChanged` 이벤트가 발생 합니다. XAML의 코드 조각은 다음과 같습니다.

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

때 `Minimum` 10으로 설정 됩니다 `Value` 10으로 설정 됩니다 및 `ValueChanged` 이벤트가 발생 합니다. 이 페이지의 나머지를 생성 하 고 처리기가 아직 만들어지지 않은 페이지의 다른 요소를 참조 하려고 하기 전에 발생할 수 있습니다. 일부 코드를 추가 하는 것이 좋습니다 합니다 `ValueChanged` 처리기에 대 한 확인 `null` 페이지의 다른 요소의 값입니다. 또는 설정할 수는 `ValueChanged` 후 이벤트 처리기를 `Slider` 값 초기화 되었습니다.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>플랫폼 구현의 차이점

값을 표시 하는 이전에 표시 된 스크린샷에서 `Slider` 소수점 수가 서로 다른 합니다. 이 방법을 관련이 `Slider` Android 및 UWP 플랫폼에서 구현 됩니다.

### <a name="the-android-implementation"></a>Android 구현

Android 구현의 `Slider` Android 기반 [ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/) 항상 설정 합니다 [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) 1000 속성입니다. 즉는 `Slider` Android에만 1,001 불연속 값입니다. 설정 하는 경우를 `Slider` 할를 `Minimum` 0 및 `Maximum` 로 서 then 5000를 `Slider` 조작 되는 `Value` 속성이 0, 5, 10, 15 및 등의 값입니다.

### <a name="the-uwp-implementation"></a>UWP 구현

UWP 구현의 `Slider` UWP 기반 [ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider) 제어 합니다. `StepFrequency` UWP의 속성 `Slider` 의 차이로 `Maximum` 및 `Minimum` 10, 하지만 1 보다 크지 않음로 나눈 값 속성입니다.

예를 들어, 기본 범위인 0 ~ 1에 대 한는 `StepFrequency` 속성은 0.1로 설정 합니다. 로 `Slider` 조작 되는 `Value` 속성은 0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9 및 1.0으로 제한 합니다. (이것은의 마지막 페이지에서 명백 합니다 [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) 샘플.) 때 간의 차이 `Maximum` 및 `Minimum` 한 다음 속성은 10 이상이 `StepFrequency` 1로 설정 됩니다 및 `Value` 속성이 정수 계열 값입니다.

### <a name="the-stepslider-solution"></a>StepSlider 솔루션

더욱 유용한 `StepSlider` 에 설명 되어 [27 장입니다. 사용자 지정 렌더러](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) 책 *Creating Mobile Apps with Xamarin.Forms*합니다. 합니다 `StepSlider` 비슷합니다 `Slider` 있지만 추가 `Steps` 사이의 값의 수를 지정 하려면 속성 `Minimum` 및 `Maximum`합니다.

## <a name="sliders-for-color-selection"></a>색 선택 영역에 대 한 슬라이더

마지막 두 페이지에 [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) 둘 다 사용 하 여 세 가지 샘플 `Slider` 색 선택 영역에 대 한 인스턴스. 두 번째 페이지는 ViewModel을 사용 하 여 데이터 바인딩을 사용 하는 방법을 표시 하는 동안 첫 번째 페이지 코드 숨김 파일에서 모든 상호 작용을 처리 합니다.

### <a name="handling-sliders-in-the-code-behind-file"></a>코드 숨김 파일에서 슬라이더를 처리합니다.

**RGB 색 슬라이더** 페이지를 인스턴스화하는 `BoxView` 색을 표시할 세 `Slider` 개와 색의 빨강, 녹색 및 파랑 구성 요소를 선택 하는 인스턴스 `Label` 해당 색을 표시 하기 위한 요소 값:

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

A `Style` 세 가지를 모두 제공 `Slider` 0에서 255 까지의 범위에는 요소입니다. 합니다 `Slider` 요소에 동일한 공유 `ValueChanged` 코드 숨김 파일에서 구현 되는 처리기:

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

첫 번째 섹션 집합 합니다 `Text` 속성 중 하나는 `Label` 짧은 텍스트 문자열의 값을 나타내는 인스턴스는 `Slider` 16 진수에서. 그런 다음 모든 세 `Slider` 만들려면 인스턴스에 액세스할 수는 `Color` RGB 구성 요소에서 값:

[![RGB 색 슬라이더](slider-images/RgbColorSliders.png "RGB 색 슬라이더")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>슬라이더를 ViewModel에 바인딩

합니다 **HSL 색 슬라이더** 페이지를 만드는 데 사용 하는 계산을 수행 하는 ViewModel을 사용 하는 방법을 보여 줍니다는 `Color` 색상, 채도 및 명도 값에서 값입니다. 모든 Viewmodel에 같은 합니다 `HSLColorViewModel` 구현 클래스를 `INotifyPropertyChanged` 인터페이스 및 발생을 `PropertyChanged` 속성 중 하나가 변경 될 때마다 이벤트:

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

Viewmodel 하며 `INotifyPropertyChanged` 인터페이스는 문서에서 설명한 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)합니다.

합니다 **HslColorSlidersPage.xaml** 파일은 합니다 `HslColorViewModel` 페이지의 설정 하는 `BindingContext` 속성입니다. 이렇게 하면 ViewModel의 속성에 바인딩하는 XAML 파일의 모든 요소.

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

으로 `Slider` 요소를 조작할 합니다 `BoxView` 및 `Label` 요소 ViewModel에서 업데이트 됩니다.

[![HSL 색 슬라이더](slider-images/HslColorSliders.png "HSL 색 슬라이더")](slider-images/HslColorSliders-Large.png#lightbox)

합니다 `StringFormat` 구성 요소는 `Binding` 두 소수 자릿수를 표시 하려면 "F2"의 형식에 대 한 태그 확장 설정 됩니다. (데이터 바인딩에서 서식 지정 문자열은 문서에서 설명한 [문자열 서식 지정](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) 그러나 프로그램의 UWP 버전은 0, 0.1, 0.2의 값으로 제한 하는 중... 0.9 및 1.0입니다. UWP의 구현 직접적인 결과로 이것이 `Slider` 섹션에서 설명한 것 처럼 [플랫폼 구현 차이로](#implementations)합니다.

## <a name="related-links"></a>관련 링크

- [슬라이더 데모 샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [슬라이더 API](xref:Xamarin.Forms.Slider)
