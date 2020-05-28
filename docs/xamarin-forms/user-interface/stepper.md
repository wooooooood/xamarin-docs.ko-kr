---
title: Xamarin.Forms퍼
description: Xamarin.Forms스텝 퍼를 사용 하면 사용자가 값 범위에서 숫자 값을 선택할 수 있습니다. 이는 빼기 및 더하기 기호를 사용 하 여 레이블이 지정 된 두 개의 단추로 구성 됩니다. 두 단추를 조작 하면 선택한 값이 증분 방식으로 변경 됩니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4f071530fb17de44d8ede786ca1b42f5e11f4f7c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130548"
---
# <a name="xamarinforms-stepper"></a>Xamarin.Forms퍼

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos)

_값 범위에서 숫자 값을 선택 하는 경우 스텝 퍼를 사용 합니다._

는 Xamarin.Forms [`Stepper`](xref:Xamarin.Forms.Stepper) 빼기 및 더하기 기호를 사용 하 여 레이블이 지정 된 두 개의 단추로 구성 됩니다. 사용자가 이러한 단추를 조작 하 여 `double` 값 범위에서 값을 증분 방식으로 선택할 수 있습니다.

는 [`Stepper`](xref:Xamarin.Forms.Stepper) 다음과 같은 네 가지 형식의 속성을 정의 합니다 `double` .

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment)선택한 값을 변경할 크기 이며 기본값은 1입니다.
- [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum)는 범위의 최소값 이며 기본값은 0입니다.
- [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum)는 범위의 최대값 이며 기본값은 100입니다.
- [`Value`](xref:Xamarin.Forms.Stepper.Value)는 스텝 퍼의 값으로, 및의 범위를 지정할 수 있으며 `Minimum` `Maximum` 기본값은 0입니다.

이러한 모든 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . [`Value`](xref:Xamarin.Forms.Stepper.Value)속성의 기본 바인딩 모드를 사용 하는 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) 경우이는 [MVVM (모델-뷰-ViewModel)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처를 사용 하는 응용 프로그램에서 바인딩 소스로 적합 함을 의미 합니다.

> [!WARNING]
> 내부적으로는 [`Stepper`](xref:Xamarin.Forms.Stepper) [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) 가 보다 작음을 확인 합니다 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) . `Minimum` `Maximum` 가 보다 작지 않도록 또는를 설정 하는 경우 `Minimum` `Maximum` 예외가 발생 합니다. 및 속성을 설정 하는 방법에 대 한 자세한 내용은 `Minimum` `Maximum` [주의](#precautions) 사항 섹션을 참조 하세요.

는 [`Stepper`](xref:Xamarin.Forms.Stepper) [`Value`](xref:Xamarin.Forms.Stepper.Value) [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) 와 (포함) 사이에 있도록 속성을 강제 변환 합니다 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) . 속성이 `Minimum` 속성 보다 큰 값으로 설정 된 경우 `Value` 는 `Stepper` 속성을로 설정 합니다 `Value` `Minimum` . 마찬가지로 `Maximum` ,가 보다 작은 값으로 설정 된 경우에는 `Value` 속성을 `Stepper` 로 설정 합니다 `Value` `Maximum` .

[`Stepper`](xref:Xamarin.Forms.Stepper)[`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) [`Value`](xref:Xamarin.Forms.Stepper.Value) 가 변경 될 때 `Stepper` 또는 응용 프로그램에서 속성을 직접 설정 하는 경우 발생 하는 이벤트를 정의 합니다 `Value` . `ValueChanged`이벤트는 `Value` 이전 단락에서 설명한 대로 속성을 강제 변환할 때에도 발생 합니다.

[`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs)이벤트와 함께 제공 되는 개체에는 [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) 두 가지 속성인 `double` 및 형식이 [`OldValue`](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) [`NewValue`](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue) 있습니다. 이벤트가 발생 한 시점에의 값은 `NewValue` [`Value`](xref:Xamarin.Forms.Stepper.Value) 개체의 속성과 같습니다 [`Stepper`](xref:Xamarin.Forms.Stepper) .

## <a name="basic-stepper-code-and-markup"></a>기본 스텝 퍼 코드 및 태그

[**Stepperdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) 샘플에는 기능적으로 동일 하지만 다른 방법으로 구현 되는 세 개의 페이지가 포함 되어 있습니다. 첫 번째 페이지는 c # 코드만 사용 하 고, 두 번째는 코드에서 이벤트 처리기를 사용 하 여 XAML을 사용 하 고, 세 번째 페이지는 XAML 파일에서 데이터 바인딩을 사용 하 여 이벤트 처리기를 방지할 수 있습니다.

### <a name="creating-a-stepper-in-code"></a>코드에서 스텝 퍼 만들기

[**Stepperdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) 샘플의 **기본 스텝 퍼 코드** 페이지는 [`Stepper`](xref:Xamarin.Forms.Stepper) 코드에서 및 두 개의 개체를 만드는 방법을 보여 줍니다 [`Label`](xref:Xamarin.Forms.Label) .

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

는 [`Stepper`](xref:Xamarin.Forms.Stepper) [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) 360 속성 및 30의 속성을 포함 하도록 초기화 됩니다 [`Increment`](xref:Xamarin.Forms.Stepper.Increment) . 를 조작 하면 `Stepper` 선택 된 값이 [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) `Maximum` 속성 값에 따라로 증분 변경 `Increment` 됩니다. [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged)의 처리기는 `Stepper` 개체의 속성을 사용 하 여 [`Value`](xref:Xamarin.Forms.Stepper.Value) `stepper` [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 첫 번째의 속성을 설정 하 [`Label`](xref:Xamarin.Forms.Label) 고 메서드를 `string.Format` 이벤트 인수의 속성과 함께 사용 하 여 `NewValue` 두 번째의 속성을 설정 합니다 [`Text`](xref:Xamarin.Forms.Label.Text) `Label` . 의 현재 값을 가져오는 두 가지 방법은 `Stepper` 서로 바꿔 사용할 수 있습니다.

다음 스크린샷은 **기본 스텝 퍼 코드** 페이지를 보여 줍니다.

[![기본 스텝 퍼 코드](stepper-images/basic-stepper-code.png "기본 스텝 퍼 코드")](stepper-images/basic-stepper-code-large.png#lightbox)

두 번째는 [`Label`](xref:Xamarin.Forms.Label) 가 조작 될 때까지 "(초기화 되지 않음)" 텍스트를 표시 하 여 [`Stepper`](xref:Xamarin.Forms.Stepper) 첫 번째 [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) 이벤트를 발생 시킵니다.

### <a name="creating-a-stepper-in-xaml"></a>XAML로 스텝 퍼 만들기

**기본 스텝 퍼 xaml** 페이지는 **기본 스텝 퍼 코드** 와 동일 하지만 대부분 XAML에서 구현 됩니다.

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

코드를 포함 하는 파일에는 이벤트에 대 한 처리기가 포함 됩니다 [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) .

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

이벤트 처리기가 [`Stepper`](xref:Xamarin.Forms.Stepper) 인수를 통해 이벤트를 발생 시키는를 가져올 수도 있습니다 `sender` . 속성에는 [`Value`](xref:Xamarin.Forms.Stepper.Value) 현재 값이 포함 되어 있습니다.

```csharp
double value = ((Stepper)sender).Value;
```

개체에 [`Stepper`](xref:Xamarin.Forms.Stepper) 특성을 사용 하 여 XAML 파일에 이름이 지정 된 경우 `x:Name` (예: "스텝 퍼") 이벤트 처리기는 해당 개체를 직접 참조할 수 있습니다.

```csharp
double value = stepper.Value;
```

### <a name="data-binding-the-stepper"></a>스텝 퍼의 데이터 바인딩

**기본 스텝 퍼 바인딩** 페이지에서는 [`Value`](xref:Xamarin.Forms.Stepper.Value) [데이터 바인딩을](~/xamarin-forms/app-fundamentals/data-binding/index.md)사용 하 여 이벤트 처리기를 제거 하는 거의 동일한 응용 프로그램을 작성 하는 방법을 보여 줍니다.

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

첫 번째의 속성은 지정 된 사양의 속성에 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`Label`](xref:Xamarin.Forms.Label) [`Value`](xref:Xamarin.Forms.Stepper.Value) 따라의 속성에 바인딩됩니다 [`Stepper`](xref:Xamarin.Forms.Stepper) [`Text`](xref:Xamarin.Forms.Label.Text) `Label` `StringFormat` . **기본 스텝 퍼 바인딩** 페이지는 이전 페이지 두 개와 약간 다르게 작동 합니다. 페이지가 처음 나타날 때 두 번째는 `Label` 값이 포함 된 텍스트 문자열을 표시 합니다. 이는 데이터 바인딩을 사용 하는 경우의 장점입니다. 데이터 바인딩을 사용 하지 않고 텍스트를 표시 하려면 `Text` 의 속성을 초기화 `Label` 하거나 [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) 클래스 생성자에서 이벤트 처리기를 호출 하 여 이벤트 발생을 시뮬레이션 해야 합니다.

## <a name="precautions"></a>조치가

속성의 값은 [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) 항상 속성의 값 보다 작아야 합니다 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) . 다음 코드 조각에서는에서 [`Stepper`](xref:Xamarin.Forms.Stepper) 예외를 발생 시킵니다.

```csharp
// Throws an exception!
Stepper stepper = new Stepper
{
    Minimum = 180,
    Maximum = 360
};
```

C # 컴파일러는 이러한 두 속성을 순서 대로 설정 하는 코드를 생성 하 고 [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) , 속성이 180로 설정 된 경우 기본값 100 보다 큽니다 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) . 이 경우에는 먼저 속성을 설정 하 여 예외를 방지할 수 있습니다 `Maximum` .

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

를 360로 설정 하는 것 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) 은 기본값 0 보다 크므로 문제가 되지 않습니다 [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) . `Minimum`가 설정 된 경우 값은 360 값 보다 낮습니다 `Maximum` .

XAML에 동일한 문제가 있습니다. 이 항상 보다 큰지 확인 하는 순서 대로 속성을 설정 합니다 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) `Minimum` .

```xaml
<Stepper Maximum="360"
         Minimum="180" ... />
```

[`Minimum`](xref:Xamarin.Forms.Stepper.Minimum)및 값을 음수로 설정할 수 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) 있지만 `Minimum` 가 항상 보다 작은 순서에 불과합니다 `Maximum` .

```xaml
<Stepper Minimum="-360"
         Maximum="-180" ... />
```

[`Value`](xref:Xamarin.Forms.Stepper.Value)속성은 항상 값 보다 크거나 같고 [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) 보다 작거나 같습니다 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) . `Value`이 해당 범위 밖의 값으로 설정 된 경우 값은 범위 내에 있는 것으로 강제 변환 되지만 예외가 발생 하지 않습니다. 예를 들어 다음 코드는 예외를 발생 시 키 *지 않습니다* .

```csharp
Stepper stepper = new Stepper
{
    Value = 180
};
```

대신 [`Value`](xref:Xamarin.Forms.Stepper.Value) 속성이 100 값으로 강제 변환 됩니다 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) .

위에 표시 된 코드 조각은 다음과 같습니다.

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

[`Minimum`](xref:Xamarin.Forms.Stepper.Minimum)가 180로 설정 [`Value`](xref:Xamarin.Forms.Stepper.Value) 된 경우도 180로 설정 됩니다.

[`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) [`Value`](xref:Xamarin.Forms.Stepper.Value) 속성이 기본값 0 이외의 값으로 강제 변환 될 때 이벤트 처리기가 연결 된 경우 `ValueChanged` 이벤트가 발생 합니다. 다음은 XAML의 코드 조각입니다.

```xaml
<Stepper ValueChanged="OnStepperValueChanged"
         Maximum="360"
         Minimum="180" />
```

[`Minimum`](xref:Xamarin.Forms.Stepper.Minimum)가 180로 설정 된 경우 [`Value`](xref:Xamarin.Forms.Stepper.Value) 도 180로 설정 되 고 [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) 이벤트가 발생 합니다. 이는 페이지의 나머지 부분을 생성 하기 전에 발생할 수 있으며, 처리기가 아직 만들지 않은 페이지의 다른 요소를 참조 하려고 할 수 있습니다. `ValueChanged` `null` 페이지에서 다른 요소의 값을 확인 하는 일부 코드를 처리기에 추가 하려고 할 수 있습니다. 또는 `ValueChanged` 값이 초기화 된 후에 이벤트 처리기를 설정할 수 있습니다 [`Stepper`](xref:Xamarin.Forms.Stepper) .

## <a name="related-links"></a>관련 링크

- [스텝 퍼 데모 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos)
- [스텝 퍼 API](xref:Xamarin.Forms.Stepper)
