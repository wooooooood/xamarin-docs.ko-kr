---
title: Xamarin.Forms슬라이드
description: Xamarin.Forms슬라이더는 연속 범위에서 double 값을 선택 하기 위해 사용자가 조작할 수 있는 가로 막대입니다. 이 문서에서는 Slider 클래스를 사용 하 여 연속 값 범위에서 값을 선택 하는 방법을 설명 합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1cde999e6781f019b6abceee82caf259e1e5a710
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140155"
---
# <a name="xamarinforms-slider"></a>Xamarin.Forms슬라이드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)

_연속 값 범위에서 선택 하려면 슬라이더를 사용 합니다._

는 Xamarin.Forms [`Slider`](xref:Xamarin.Forms.Slider) 사용자가 `double` 연속 범위에서 값을 선택 하기 위해 조작할 수 있는 가로 막대입니다.

는 `Slider` 다음과 같은 세 가지 유형의 속성을 정의 합니다 `double` .

- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum)는 범위의 최소값 이며 기본값은 0입니다.
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum)범위의 최대값입니다. 기본값은 1입니다.
- [`Value`](xref:Xamarin.Forms.Slider.Value)는 슬라이더의 값으로,와의 범위에 `Minimum` 있으며 `Maximum` 기본값은 0입니다.

세 속성은 모두 개체에 의해 지원 됩니다 `BindableProperty` . `Value`속성의 기본 바인딩 모드를 사용 하는 `BindingMode.TwoWay` 경우이는 [MVVM (모델-뷰-ViewModel)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처를 사용 하는 응용 프로그램에서 바인딩 소스로 적합 함을 의미 합니다.

> [!WARNING]
> 내부적으로는 `Slider` `Minimum` 가 보다 작음을 확인 합니다 `Maximum` . `Minimum` `Maximum` 가 보다 작지 않도록 또는를 설정 하는 경우 `Minimum` `Maximum` 예외가 발생 합니다. 및 속성 설정에 대 한 자세한 내용은 아래의 [**예방 조치**](#precautions) 섹션을 참조 하세요 `Minimum` `Maximum` .

는 `Slider` `Value` `Minimum` 와 (포함) 사이에 있도록 속성을 강제 변환 합니다 `Maximum` . 속성이 `Minimum` 속성 보다 큰 값으로 설정 된 경우 `Value` 는 `Slider` 속성을로 설정 합니다 `Value` `Minimum` . 마찬가지로 `Maximum` ,가 보다 작은 값으로 설정 된 경우에는 `Value` 속성을 `Slider` 로 설정 합니다 `Value` `Maximum` .

`Slider`의 [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) `Value` 사용자 조작을 통해 `Slider` 또는 프로그램에서 속성을 직접 설정 하는 경우가 변경 될 때 발생 하는 이벤트를 정의 합니다 `Value` . `ValueChanged`이벤트는 `Value` 이전 단락에서 설명한 대로 속성을 강제 변환할 때에도 발생 합니다.

[`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs)이벤트와 함께 제공 되는 개체에는 `ValueChanged` 두 가지 속성인 `double` 및 형식이 [`OldValue`](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) [`NewValue`](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue) 있습니다. 이벤트가 발생 한 시점에의 값은 `NewValue` `Value` 개체의 속성과 같습니다 `Slider` .

`Slider`또한는 `DragStarted` `DragCompleted` 끌기 작업의 시작과 끝에서 발생 하는 및 이벤트를 정의 합니다. 이벤트와 달리 [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) `DragStarted` 및 이벤트는 `DragCompleted` 의 사용자 조작을 통해서만 발생 합니다 `Slider` . `DragStarted`이벤트가 발생 하면 `DragStartedCommand` 형식의이 `ICommand` 실행 됩니다. 마찬가지로 `DragCompleted` 이벤트가 발생 하면 `DragCompletedCommand` 형식의이 `ICommand` 실행 됩니다.

> [!WARNING]
> , 또는에서 제한 없는 가로 레이아웃 옵션을 사용 하지 마십시오 `Center` `Start` `End` `Slider` . Android 및 UWP 둘 다에서는 `Slider` 0 길이 막대로 축소 되 고, iOS에서는 막대가 매우 짧습니다. `HorizontalOptions`의 기본 설정을 유지 `Fill` 하 고 `Auto` 레이아웃에 넣을 때 너비를 사용 하지 않습니다 `Slider` `Grid` .

는 `Slider` 또한 모양에 영향을 주는 몇 가지 속성을 정의 합니다.

- [`MinimumTrackColor`](xref:Xamarin.Forms.Slider.MinimumTrackColorProperty)엄지 단추의 왼쪽에 있는 막대 색입니다.
- [`MaximumTrackColor`](xref:Xamarin.Forms.Slider.MaximumTrackColorProperty)thumb의 오른쪽에 있는 막대 색입니다.
- [`ThumbColor`](xref:Xamarin.Forms.Slider.ThumbColorProperty)thumb 색입니다.
- [`ThumbImageSource`](xref:Xamarin.Forms.Slider.ThumbImageSourceProperty)형식의 thumb에 사용할 이미지입니다 [`ImageSource`](xref:Xamarin.Forms.ImageSource) .

> [!NOTE]
> `ThumbColor`및 `ThumbImageSource` 속성은 함께 사용할 수 없습니다. 두 속성이 모두 설정 되 면 `ThumbImageSource` 속성이 우선적으로 적용 됩니다.

## <a name="basic-slider-code-and-markup"></a>기본 슬라이더 코드 및 태그

[**SliderDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) 샘플은 기능적으로 동일 하지만 다른 방법으로 구현 되는 세 페이지로 시작 합니다. 첫 번째 페이지는 c # 코드만 사용 하 고, 두 번째는 코드에서 이벤트 처리기를 사용 하 여 XAML을 사용 하 고, 세 번째 페이지는 XAML 파일에서 데이터 바인딩을 사용 하 여 이벤트 처리기를 방지할 수 있습니다.

### <a name="creating-a-slider-in-code"></a>코드에서 슬라이더 만들기

[**SliderDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) 샘플의 **기본 슬라이더 코드** 페이지는 `Slider` 코드에서 및 두 개의 개체를 만드는 방법을 보여 줍니다 `Label` .

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

는 `Slider` 360의 속성을 포함 하도록 초기화 됩니다 `Maximum` . `ValueChanged`의 처리기는 `Slider` 개체의 속성을 사용 하 여 `Value` `slider` `Rotation` 첫 번째의 속성을 설정 하 `Label` 고 메서드를 `String.Format` 이벤트 인수의 속성과 함께 사용 하 여 `NewValue` 두 번째의 속성을 설정 합니다 `Text` `Label` . 의 현재 값을 가져오는 두 가지 방법은 `Slider` 서로 바꿔 사용할 수 있습니다.

IOS 및 Android 장치에서 실행 되는 프로그램은 다음과 같습니다.

[![기본 슬라이더 코드](slider-images/BasicSliderCode.png "기본 슬라이더 코드")](slider-images/BasicSliderCode-Large.png#lightbox)

두 번째는 `Label` 가 조작 될 때까지 "(초기화 되지 않음)" 텍스트를 표시 하 여 `Slider` 첫 번째 `ValueChanged` 이벤트를 발생 시킵니다. 표시 되는 소수 자릿수는 각 플랫폼 마다 다릅니다. 이러한 차이점은의 플랫폼 구현과 관련 되며 `Slider` , [플랫폼 구현 차이점](#implementations)섹션에서이 문서의 뒷부분에서 설명 합니다.

### <a name="creating-a-slider-in-xaml"></a>XAML에서 슬라이더 만들기

**기본 슬라이더 XAML** 페이지는 **기본 슬라이더 코드** 와 동일 하지만 대부분 XAML에서 구현 됩니다.

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

코드를 포함 하는 파일에는 이벤트에 대 한 처리기가 포함 됩니다 `ValueChanged` .

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

이벤트 처리기가 `Slider` 인수를 통해 이벤트를 발생 시키는를 가져올 수도 있습니다 `sender` . 속성에는 `Value` 현재 값이 포함 되어 있습니다.

```csharp
double value = ((Slider)sender).Value;
```

개체에 `Slider` 특성 (예: "slider")을 사용 하 여 XAML 파일에 이름이 지정 된 경우 `x:Name` 이벤트 처리기는 해당 개체를 직접 참조할 수 있습니다.

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>슬라이더의 데이터 바인딩

**기본 슬라이더 바인딩** 페이지에서는 `Value` [데이터 바인딩을](~/xamarin-forms/app-fundamentals/data-binding/index.md)사용 하 여 이벤트 처리기를 제거 하는 거의 동일한 프로그램을 작성 하는 방법을 보여 줍니다.

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

첫 번째의 속성은 지정 된 사양의 속성에 `Rotation` `Label` `Value` 따라의 속성에 바인딩됩니다 `Slider` `Text` `Label` `StringFormat` . **기본 슬라이더 바인딩** 페이지는 이전 페이지 두 개와 약간 다르게 작동 합니다. 페이지가 처음 나타날 때 두 번째는 값이 `Label` 있는 텍스트 문자열을 표시 합니다. 이는 데이터 바인딩을 사용 하는 경우의 장점입니다. 데이터 바인딩을 사용 하지 않고 텍스트를 표시 하려면 `Text` 의 속성을 초기화 `Label` 하거나 `ValueChanged` 클래스 생성자에서 이벤트 처리기를 호출 하 여 이벤트 발생을 시뮬레이션 해야 합니다.

<a name="precautions" />

## <a name="precautions"></a>조치가

속성의 값은 `Minimum` 항상 속성의 값 보다 작아야 합니다 `Maximum` . 다음 코드 조각에서는에서 `Slider` 예외를 발생 시킵니다.

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

C # 컴파일러는 이러한 두 속성을 순서 대로 설정 하는 코드를 생성 하 고 `Minimum` , 속성을 10으로 설정 하면 기본값 1 보다 큽니다 `Maximum` . 이 경우에는 먼저 속성을 설정 하 여 예외를 방지할 수 있습니다 `Maximum` .

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

를 20으로 설정 하는 것 `Maximum` 은 기본값 0 보다 크므로 문제가 되지 않습니다 `Minimum` . `Minimum`가 설정 되 면 값이 20 보다 작은 값입니다 `Maximum` .

XAML에 동일한 문제가 있습니다. 이 항상 보다 큰지 확인 하는 순서 대로 속성을 설정 합니다 `Maximum` `Minimum` .

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

`Minimum`및 값을 음수로 설정할 수 `Maximum` 있지만 `Minimum` 가 항상 보다 작은 순서에 불과합니다 `Maximum` .

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

`Value`속성은 항상 값 보다 크거나 같고 `Minimum` 보다 작거나 같습니다 `Maximum` . `Value`이 해당 범위 밖의 값으로 설정 된 경우 값은 범위 내에 있는 것으로 강제 변환 되지만 예외가 발생 하지 않습니다. 예를 들어 다음 코드는 예외를 발생 시 키 *지 않습니다* .

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

대신 속성은 `Value` 값 1로 강제 변환 됩니다 `Maximum` .

위에 표시 된 코드 조각은 다음과 같습니다.

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

`Minimum`가 10으로 설정 되 면 `Value` 도 10으로 설정 됩니다.

`ValueChanged` `Value` 속성이 기본값 0 이외의 값으로 강제 변환 될 때 이벤트 처리기가 연결 된 경우 `ValueChanged` 이벤트가 발생 합니다. 다음은 XAML의 코드 조각입니다.

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

`Minimum`가 10으로 설정 되 면 `Value` 도 10으로 설정 되 고 `ValueChanged` 이벤트가 발생 합니다. 이는 페이지의 나머지 부분을 생성 하기 전에 발생할 수 있으며, 처리기가 아직 만들지 않은 페이지의 다른 요소를 참조 하려고 할 수 있습니다. `ValueChanged` `null` 페이지에서 다른 요소의 값을 확인 하는 일부 코드를 처리기에 추가 하려고 할 수 있습니다. 또는 `ValueChanged` 값이 초기화 된 후에 이벤트 처리기를 설정할 수 있습니다 `Slider` .

<a name="implementations" />

## <a name="platform-implementation-differences"></a>플랫폼 구현 차이점

앞에 표시 된 스크린샷은 소수점이 하 자릿수의 값을 표시 합니다 `Slider` . 이는이 `Slider` Android 및 UWP 플랫폼에서 구현 되는 방법과 관련이 있습니다.

### <a name="the-android-implementation"></a>Android 구현

의 Android 구현은 `Slider` android를 기반으로 [`SeekBar`](xref:Android.Widget.SeekBar) 하며 항상 [`Max`](xref:Android.Widget.ProgressBar.Max) 속성을 1000로 설정 합니다. 즉, `Slider` Android의에는 1001 불연속 값만 있습니다. 가 `Slider` `Minimum` 0과 5000의를 갖도록 설정 하면 `Maximum` `Slider` 가 조작 될 때 `Value` 속성의 값은 0, 5, 10, 15 등이 됩니다.

### <a name="the-uwp-implementation"></a>UWP 구현

의 UWP 구현은 `Slider` uwp 컨트롤을 기반으로 합니다 [`Slider`](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.slider) . `StepFrequency`UWP의 속성 `Slider` 은 및 속성의 차이로 설정 되며 `Maximum` `Minimum` 1 보다 크지는 않습니다.

예를 들어 기본 범위인 0에서 1로 설정 된 경우 `StepFrequency` 속성은 0.1로 설정 됩니다. `Slider`가 조작 될 때 속성은 `Value` 0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9 및 1.0로 제한 됩니다. 이는 [**SliderDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) 샘플의 마지막 페이지에 있습니다. `Maximum`및 속성의 차이가 `Minimum` 10 이상일 경우 `StepFrequency` 은 1로 설정 되 고 속성에는 `Value` 정수 값이 있습니다.

### <a name="the-stepslider-solution"></a>Stslider 솔루션

더 다양 한 `StepSlider` 기능은 [27 장에서 설명 합니다. ](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) *을 사용 하 여 Mobile Apps를 Xamarin.Forms 만든 *책의 사용자 지정 렌더러 는 `StepSlider` `Slider` 와 비슷하지만 속성을 추가 하 여 `Steps` 와 사이의 값 수를 지정 합니다 `Minimum` `Maximum` .

## <a name="sliders-for-color-selection"></a>색 선택 슬라이더

[**SliderDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) 샘플의 마지막 두 페이지는 색 선택에 세 개의 인스턴스를 사용 합니다 `Slider` . 첫 번째 페이지는 코드 사용 파일의 모든 상호 작용을 처리 하는 반면 두 번째 페이지는 ViewModel에서 데이터 바인딩을 사용 하는 방법을 보여 줍니다.

### <a name="handling-sliders-in-the-code-behind-file"></a>코드 숨겨진 파일의 처리 슬라이더

**RGB 색 슬라이더** 페이지는를 인스턴스화하여 색 `BoxView` `Slider` 의 빨간색, 녹색 및 파랑 구성 요소를 선택할 수 있는 세 개의 인스턴스와 색 값을 표시 하는 세 가지 요소가 표시 됩니다 `Label` .

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

는 `Style` 세 개의 `Slider` 요소에 0에서 255 사이의 범위를 제공 합니다. `Slider`요소는 `ValueChanged` 코드 숨김으로 구현 되는 동일한 처리기를 공유 합니다.

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

첫 번째 섹션은 `Text` 인스턴스 중 하나의 속성을 `Label` 16 진수의 값을 나타내는 짧은 텍스트 문자열로 설정 합니다 `Slider` . 그런 다음, 세 개의 인스턴스 모두에 `Slider` 액세스 하 여 `Color` RGB 구성 요소에서 값을 만듭니다.

[![RGB 색 슬라이더](slider-images/RgbColorSliders.png "RGB 색 슬라이더")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>ViewModel에 슬라이더 바인딩

**HSL 색 슬라이더** 페이지에서는 ViewModel을 사용 하 여 `Color` 색상, 채도 및 명도 값에서 값을 만드는 데 사용 되는 계산을 수행 하는 방법을 보여 줍니다. 모든 ViewModels 마찬가지로 클래스는 `HSLColorViewModel` 인터페이스를 구현 `INotifyPropertyChanged` 하 고 `PropertyChanged` 속성 중 하나가 변경 될 때마다 이벤트를 발생 시킵니다.

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

ViewModels 인터페이스에 `INotifyPropertyChanged` 대해서는 문서 [데이터 바인딩에](~/xamarin-forms/app-fundamentals/data-binding/index.md)설명 되어 있습니다.

**HslColorSlidersPage** 파일은를 인스턴스화하고 `HslColorViewModel` 페이지의 속성으로 설정 `BindingContext` 합니다. 이를 통해 XAML 파일의 모든 요소를 ViewModel의 속성에 바인딩할 수 있습니다.

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

`Slider`요소가 조작 될 때 `BoxView` 및 `Label` 요소는 ViewModel에서 업데이트 됩니다.

[![HSL 색 슬라이더](slider-images/HslColorSliders.png "HSL 색 슬라이더")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat`태그 확장의 구성 요소는 `Binding` 두 소수 자릿수를 표시 하기 위해 "F2" 형식에 대해 설정 됩니다. 데이터 바인딩의 문자열 서식 지정에 대해서는 문서 [문자열 서식 지정](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md)에서 설명 합니다. 그러나 UWP 버전의 프로그램은 0, 0.1, 0.2, ...의 값으로 제한 됩니다. 0.9 및 1.0입니다. 이는 `Slider` [플랫폼 구현 차이점](#implementations)섹션에서 위에서 설명한 대로 UWP의 구현에 대 한 직접적인 결과입니다.

## <a name="related-links"></a>관련 링크

- [슬라이더 데모 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos)
- [슬라이더 API](xref:Xamarin.Forms.Slider)
