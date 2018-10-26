---
title: Xamarin.Forms 스텝 퍼
description: Xamarin.Forms 스텝 퍼 범위의 값을 숫자 값을 선택할 수 있도록 허용 합니다. 레이블이 지정 된 두 개의 단추 이루어져 빼기와 더하기 기호입니다. 선택한 값을 증분 변경 두 개의 단추를 조작 합니다.
ms.prod: xamarin
ms.assetid: 62571B3E-D84B-4F52-9FC7-C105D6733B16
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/17/2018
ms.openlocfilehash: 53485d0d948f31b69f74b6933d05c14f69fa64c0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111579"
---
# <a name="xamarinforms-stepper"></a>Xamarin.Forms 스텝 퍼

_값의 범위에서 숫자 값을 선택 하기 위한 스텝을 사용 합니다._

Xamarin.Forms [ `Stepper` ](xref:Xamarin.Forms.Stepper) 레이블이 지정 된 두 개의 단추 이루어져 빼기와 더하기 기호입니다. 증분 방식으로 선택 하려면 사용자가 이러한 단추를 조작할 수 있습니다는 `double` 값의 범위에서 값입니다.

합니다 [ `Stepper` ](xref:Xamarin.Forms.Stepper) 형식의 네 가지 속성을 정의 `double`:

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) 기본값은 1에서 선택한 값을 변경 하려면 크기가입니다.
- [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) 기본값은 0 사용 하 여 범위의 최소값이입니다.
- [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) 기본값은 100 사용 하 여 범위의 최대값입니다.
- [`Value`](xref:Xamarin.Forms.Stepper.Value) 스텝 퍼의 값 사이의 범위 이므로 `Minimum` 및 `Maximum` 있고 기본값은 0입니다.

이러한 모든 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 개체입니다. [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 속성이의 기본 바인딩 모드 [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), 즉, 사용 하는 응용 프로그램에서 바인딩 소스로 적합 한지를 [ 모델-뷰-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처입니다.

> [!WARNING]
> 내부적으로 [ `Stepper` ](xref:Xamarin.Forms.Stepper) 되도록 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) 는 미만 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum)합니다. 하는 경우 `Minimum` 또는 `Maximum` 적이 설정 되어 있도록 `Minimum` 는 보다 작지 않음 `Maximum`, 예외가 발생 합니다. 설정에 대 한 자세한 내용은 합니다 `Minimum` 하 고 `Maximum` 속성을 참조 하세요 [예방 조치](#precautions) 섹션입니다.

[ `Stepper` ](xref:Xamarin.Forms.Stepper) 강제 변환 합니다 [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 속성 간에 되도록 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) 하 고 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum)(포함). 경우는 `Minimum` 속성 보다 큰 값으로 설정 됩니다는 `Value` 속성을 `Stepper` 설정를 `Value` 속성을 `Minimum`. 마찬가지로, 경우 `Maximum` 값으로 설정 됩니다 보다 작은 `Value`, 다음 `Stepper` 설정의 `Value` 속성을 `Maximum`입니다.

[`Stepper`](xref:Xamarin.Forms.Stepper) 정의 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) 때 발생 하는 이벤트를 [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 의 사용자 조작을 통해 변경 내용을 합니다 `Stepper` 하거나 응용 프로그램 설정 하는 경우는 `Value` 속성에 직접 해당 합니다. A `ValueChanged` 도 이벤트가 실행 될 때를 `Value` 속성에는 이전 단락에 설명 된 대로 강제 변환 됩니다.

[ `ValueChangedEventArgs` ](xref:Xamarin.Forms.ValueChangedEventArgs) 와 함께 제공 되는 개체를 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) 이벤트의 형식은 모두 두 개의 속성에 `double`: [ `OldValue` ](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) 및[ `NewValue`](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue). 시 이벤트 발생의 값 `NewValue` 와 같습니다는 [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 속성을 [ `Stepper` ](xref:Xamarin.Forms.Stepper) 개체.

## <a name="basic-stepper-code-and-markup"></a>기본 스텝 퍼 코드와 태그

합니다 [ **StepperDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/StepperDemos) 샘플 기능적으로 동일 하지만 다른 방법으로 구현 되는 3 개의 페이지가 포함 되어 있습니다. 첫 번째 페이지만 사용 하 여 C# 코드, 두 번째 용도에서 코드 및 세 번째 이벤트 처리기를 사용 하 여 XAML은 XAML 파일에서 데이터 바인딩을 사용 하 여 이벤트 처리기를 피할 수는 없습니다.

### <a name="creating-a-stepper-in-code"></a>스텝 코드에서 만들기

**기본 스텝 퍼 코드** 페이지에 [ **StepperDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/StepperDemos) 샘플에는 만드는 방법을 보여 줍니다는 [ `Stepper` ](xref:Xamarin.Forms.Stepper) 두 개의 [ `Label` ](xref:Xamarin.Forms.Label) 코드의 개체:

```csharp
public class BasicStepperCodePage : ContentPage
{
    public BasicStepperCodePage()
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

        Stepper stepper = new Stepper
        {
            Maximum = 360,
            Increment = 30,
            HorizontalOptions = LayoutOptions.Center
        };
        stepper.ValueChanged += (sender, e) =>
        {
            rotationLabel.Rotation = stepper.Value;
            displayLabel.Text = string.Format("The Stepper value is {0}", e.NewValue);
        };

        Title = "Basic Stepper Code";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = { rotationLabel, stepper, displayLabel }
        };
    }
}
```

합니다 [ `Stepper` ](xref:Xamarin.Forms.Stepper) 하도록 초기화 됩니다는 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) 360의 속성 및 [ `Increment` ](xref:Xamarin.Forms.Stepper.Increment) 30 속성입니다. 조작 합니다 `Stepper` 선택한 값 간의 증분 변경 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) 하 `Maximum` 의 값을 기반으로 `Increment` 속성. [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) 처리기를 `Stepper` 사용 하 여를 [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 의 속성을 `stepper` 설정할 개체입니다를 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) 첫 번째 속성 [ `Label` ](xref:Xamarin.Forms.Label) 사용 하는 `string.Format` 메서드를 `NewValue` 설정에 대 한 이벤트 인수의 속성을 [ `Text` ](xref:Xamarin.Forms.Label.Text) 속성은 두 번째 `Label`입니다. 이러한 두 가지 방법의 현재 값을 가져옵니다는 `Stepper` 바꾸어 사용할 수 있습니다.

다음 스크린샷에서 표시 된 **기본 스텝 퍼 코드** 페이지:

[![기본 스텝 퍼 코드](stepper-images/basic-stepper-code.png "기본 스텝 퍼 코드")](stepper-images/basic-stepper-code-large.png#lightbox)

두 번째 [ `Label` ](xref:Xamarin.Forms.Label) 까지 "(초기화 되지 않음)" 텍스트를 표시 합니다 [ `Stepper` ](xref:Xamarin.Forms.Stepper) 조작 되 첫 번째 발생 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) 이벤트 발생 합니다.

### <a name="creating-a-stepper-in-xaml"></a>스텝 XAML에서 만들기

**기본 스텝 퍼 XAML** 페이지는 기능적으로 동일 **기본 스텝 퍼 코드** 하지만 XAML에서 주로 구현 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StepperDemo.BasicStepperXAMLPage"
             Title="Basic Stepper XAML">
    <StackLayout Margin="20">
        <Label x:Name="_rotatingLabel"
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
        <Stepper Maximum="360"
                 Increment="30"
                 HorizontalOptions="Center"
                 ValueChanged="OnStepperValueChanged" />
        <Label x:Name="_displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />        
    </StackLayout>
</ContentPage>
```

코드 숨김 파일에 대 한 처리기가 포함 된 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) 이벤트:

```csharp
public partial class BasicStepperXAMLPage : ContentPage
{
    public BasicStepperXAMLPage()
    {
        InitializeComponent();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs e)
    {
        double value = e.NewValue;
        _rotatingLabel.Rotation = value;
        _displayLabel.Text = string.Format("The Stepper value is {0}", value);
    }
}
```

이벤트 처리기를 가져올 수 이기도 합니다 [ `Stepper` ](xref:Xamarin.Forms.Stepper) 를 통해 이벤트를 발생 하는 `sender` 인수. 합니다 [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 속성 현재 값을 포함 합니다.

```csharp
double value = ((Stepper)sender).Value;
```

경우는 [ `Stepper` ](xref:Xamarin.Forms.Stepper) 개체에 사용 하 여 XAML 파일에 이름이 지정 된는 `x:Name` 특성 (예: "스텝")을 설정한 다음 이벤트 처리기는 개체를 직접 참조할 수:

```csharp
double value = stepper.Value;
```

### <a name="data-binding-the-stepper"></a>데이터 바인딩 스텝 퍼

합니다 **기본 스텝 퍼 바인딩을** 페이지를 제거 하는 거의 같은 응용 프로그램을 작성 하는 방법을 보여 줍니다 합니다 [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 사용 하 여 이벤트 처리기 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StepperDemo.BasicStepperBindingsPage"
             Title="Basic Stepper Bindings">
    <StackLayout Margin="20">
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference _stepper}, Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
        <Stepper x:Name="_stepper"
                 Maximum="360"
                 Increment="30"
                 HorizontalOptions="Center" />
        <Label Text="{Binding Source={x:Reference _stepper}, Path=Value, StringFormat='The Stepper value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

[ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) 첫 번째 속성 [ `Label` ](xref:Xamarin.Forms.Label) 바인딩되는 [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 속성을 [ `Stepper` ](xref:Xamarin.Forms.Stepper)는 합니다 [ `Text` ](xref:Xamarin.Forms.Label.Text) 의 두 번째 속성 `Label` 사용 하 여를 `StringFormat` 사양입니다. 합니다 **기본 스텝 퍼 바인딩을** 페이지 함수 약간 다르게 두 이전 페이지에서: 페이지가 처음 나타날 경우, 두 번째 `Label` 값을 사용 하 여 텍스트 문자열을 표시 합니다. 데이터 바인딩을 사용 하 여이 유용 합니다. 데이터 바인딩 없이 텍스트를 표시 하려면 특히 초기화 해야는 `Text` 의 속성을 `Label` 의 발생을 시뮬레이션 하거나는 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) 클래스 생성자에서 이벤트 처리기를 호출 하 여 이벤트 .

## <a name="precautions"></a>주의 사항

값을 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) 속성은 항상 작아야 값 보다는 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) 속성. 다음 코드 조각은 원인 합니다 [ `Stepper` ](xref:Xamarin.Forms.Stepper) 예외를 발생 시키려면:

```csharp
// Throws an exception!
Stepper stepper = new Stepper
{
    Minimum = 180,
    Maximum = 360
};
```

C# 컴파일러 순서 대로 이러한 두 속성을 설정 하는 코드를 생성 및 시기를 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) 180 속성을 기본값 보다 큰 것 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) 100의 값입니다. 설정 하 여이 경우 예외를 방지할 수는 `Maximum` 속성 첫 번째:

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

설정 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) 360 아니므로 문제가 기본값 보다 크면 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) 값이 0입니다. 때 `Minimum` 값이 설정 보다 작은 `Maximum` 360의 값입니다.

XAML에 동일한 문제가 있습니다. 되도록 하는 순서에에서 속성을 설정 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) 보다 항상 큽니다 `Minimum`:

```xaml
<Stepper Maximum="360"
         Minimum="180" ... />
```

설정할 수 있습니다 합니다 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) 및 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) 음수 있지만 순서로 값 여기서 `Minimum` 는 보다 항상 작거나 `Maximum`:

```xaml
<Stepper Minimum="-360"
         Maximum="-180" ... />
```

합니다 [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 속성은 항상 보다 크거나 같은 합니다 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) 값 보다 작거나 같음 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum)합니다. 경우 `Value` 설정 된 범위를 벗어난 값으로 값 범위 내에 배치 되도록 강제 됩니다 있지만 예외가 발생 하지 않습니다. 예를 들어이 코드에서는 *되지* 예외를 발생 시킵니다.

```csharp
Stepper stepper = new Stepper
{
    Value = 180
};
```

대신 합니다 [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 속성은 문자열로 변환 합니다 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) 100의 값입니다.

위의 코드 조각은 다음과 같습니다.

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

때 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) 180도로 설정 됩니다 [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 180로 설정 됩니다.

경우는 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) 시점에 이벤트 처리기에 연결 하는 [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 속성은 기본값인 0 이외의 값으로 강제 변환는 `ValueChanged` 이벤트가 발생 합니다. XAML의 코드 조각은 다음과 같습니다.

```xaml
<Stepper ValueChanged="OnStepperValueChanged"
         Maximum="360"
         Minimum="180" />
```

때 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) 180도로 설정 되어 [ `Value` ](xref:Xamarin.Forms.Stepper.Value) 를 180도로 및 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) 이벤트가 발생 합니다. 이 페이지의 나머지를 생성 하 고 처리기가 아직 만들어지지 않은 페이지의 다른 요소를 참조 하려고 하기 전에 발생할 수 있습니다. 일부 코드를 추가 하는 것이 좋습니다 합니다 `ValueChanged` 처리기에 대 한 확인 `null` 페이지의 다른 요소의 값입니다. 하거나 설정할 수 있습니다 합니다 `ValueChanged` 후 이벤트 처리기는 [ `Stepper` ](xref:Xamarin.Forms.Stepper) 값 초기화 되었습니다.

## <a name="related-links"></a>관련 링크

- [스텝 퍼 데모 샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/StepperDemos)
- [스텝 퍼 API](xref:Xamarin.Forms.Stepper)
