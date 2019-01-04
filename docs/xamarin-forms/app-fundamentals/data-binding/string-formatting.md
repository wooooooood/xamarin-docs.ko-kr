---
title: Xamarin.Forms 문자열 형식 지정
description: 이 문서에서는 Xamarin.FOrms 데이터 바인딩을 사용하여 개체에 문자열 형식을 지정하고 나타내는 방법을 설명합니다. 이것은 자리 표시자를 사용하여 바인딩의 StringFormat을 표준 .NET 형식 지정 문자열로 설정하여 구현할 수 있습니다.
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 2dd7efb9f295143775961afb97e70b5f241d1337
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056125"
---
# <a name="xamarinforms-string-formatting"></a>Xamarin.Forms 문자열 형식 지정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)

데이터 바인딩을 사용하여 개체나 값의 문자열 표현을 나타내는 것이 편리한 경우도 있습니다. 예를 들어, `Label`을 사용하여 `Slider`의 현재 값을 표시할 수 있습니다. 이 데이터 바인딩에서 `Slider`는 원본이고 대상은 `Label`의 `Text` 속성입니다.

코드로 문자열을 나타낼 때 가장 강력한 도구는 정적 [`String.Format`](xref:System.String.Format(System.String,System.Object)) 메서드입니다. 문자열 형식 지정에는 다양한 유형의 개체에 대한 형식 지정 코드가 포함되며 형식이 지정되는 값과 함께 다른 텍스트를 포함할 수 있습니다. 문자열 형식 지정에 대한 자세한 내용은 [.NET의 형식 지정 유형](/dotnet/standard/base-types/formatting-types/) 문서를 참조하세요.

## <a name="the-stringformat-property"></a>StringFormat 속성

이 기능은 데이터 바인딩으로 전달됩니다. 하나의 자리 표시자를 사용하여 `Binding`의 [`StringFormat`](xref:Xamarin.Forms.BindingBase.StringFormat) 속성(또는 `Binding` 태그 확장의 [`StringFormat`](xref:Xamarin.Forms.Xaml.BindingExtension.StringFormat) 속성)을 표준 .NET 문자열 형식 지정으로 설정합니다.

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

XAML 파서가 중괄호를 다른 XAML 태그 확장으로 처리하는 것을 방지하기 위해서 형식 지정 문자열을 작은따옴표(아포스트로피) 문자로 구분합니다. 이렇게 하지 않으면 작은따옴표 문자가 없는 문자열이 `String.Format` 호출에서 부동 소수점 값을 표시하는 데 사용하는 문자열과 같아집니다. `F2` 형식 지정 사양으로 인해 값이 소수점 이하 두 자리로 표시됩니다.

`StringFormat` 속성은 대상 속성이 `string` 유형이고 바인딩 모드가 `OneWay` 또는 `TwoWay`인 경우에만 의미가 있습니다. 양방향 바인딩의 경우 `StringFormat`은 원본에서 대상으로 전달되는 값에만 적용할 수 있습니다.

[바인딩 경로](binding-path.md)에 대한 다음 문서에서 살펴보겠지만 데이터 바인딩은 매우 복잡하고 난해할 수 있습니다. 이러한 데이터 바인딩을 디버깅할 때 `StringFormat`과 함께 XAML 파일에 `Label`을 추가하여 중간 결과를 표시할 수 있습니다. 개체 형식을 나타내는 데에만 사용하더라도 도움이 될 수 있습니다.

**String Formatting**(문자열 형식 지정) 페이지에서는 `StringFormat` 속성의 여러 용도를 보여줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="DataBindingDemos.StringFormattingPage"
             Title="String Formatting">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Blue" />
                <Setter Property="HeightRequest" Value="2" />
                <Setter Property="Margin" Value="0, 5" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <Slider x:Name="slider" />
        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='The slider value is {0:F2}'}" />

        <BoxView />

        <TimePicker x:Name="timePicker" />
        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time,
                              StringFormat='The TimeSpan is {0:c}'}" />

        <BoxView />

        <Entry x:Name="entry" />
        <Label Text="{Binding Source={x:Reference entry},
                              Path=Text,
                              StringFormat='The Entry text is &quot;{0}&quot;'}" />

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:DateTime.Now}">
            <Label Text="{Binding}" />
            <Label Text="{Binding Path=Ticks,
                                  StringFormat='{0:N0} ticks since 1/1/1'}" />
            <Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
            <Label Text="{Binding StringFormat='The long date is {0:D}'}" />
        </StackLayout>

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:Math.PI}">
            <Label Text="{Binding}" />
            <Label Text="{Binding StringFormat='PI to 4 decimal points = {0:F4}'}" />
            <Label Text="{Binding StringFormat='PI in scientific notation = {0:E7}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

`Slider` 및 `TimePicker`의 바인딩은 `double` 및 `TimeSpan` 데이터 형식에 해당하는 형식 사양을 사용하는 방법을 보여줍니다. `Entry` 뷰의 텍스트를 표시하는 `StringFormat`은 `&quot;` HTML 엔터티를 사용하여 형식 지정 문자열에 큰따옴표를 지정하는 방법을 보여줍니다.

XAML 파일의 다음 섹션은 정적 `DateTime.Now` 속성을 참조하는 `x:Static` 태그 확장으로 `BindingContext`가 설정된 `StackLayout`입니다. 첫 번째 바인딩에는 속성이 없습니다.

```xaml
<Label Text="{Binding}" />
```

`BindingContext`의 `DateTime` 값을 기본 형식으로 표시합니다. 두 번째 바인딩은 `DateTime`의 `Ticks` 속성을 표시하지만 다른 두 바인딩은 특정 형식의 `DateTime` 자체를 표시합니다. 아래 `StringFormat`을 확인하세요.

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

형식 지정 문자열에 왼쪽 또는 오른쪽 중괄호를 표시해야 하는 경우에는 한 쌍을 사용하면 됩니다.

마지막 섹션에서는 `BindingContext`를 `Math.PI` 값으로 설정하고 기본 형식과 두 가지 다른 유형의 숫자 형식을 사용하여 표시합니다.

실행 중인 프로그램은 다음과 같습니다.

[![문자열 형식 지정](string-formatting-images/stringformatting-small.png "문자열 형식 지정")](string-formatting-images/stringformatting-large.png#lightbox "문자열 형식 지정")

## <a name="viewmodels-and-string-formatting"></a>ViewModels 및 문자열 형식 지정

`Label` 및 `StringFormat`을 사용하여 ViewModel의 대상이기도 한 보기의 값을 표시하는 경우에는 뷰에서 `Label`로 바인딩을 정의하거나 ViewModel에서 `Label`로 바인딩을 정의할 수 있습니다. 일반적으로 두 번째 방법이 가장 좋습니다. View와 ViewModel 사이의 바인딩이 작동하는지 확인하기 때문입니다.

이 방법은 **Better Color Selector**(더 나은 색 선택기) 샘플에 나와 있으며 [**바인딩 모드**](binding-mode.md) 문서에 표시된 **Simple Color Selector**(간단한 색 선택기) 프로그램과 동일한 ViewModel이 사용되었습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.BetterColorSelectorPage"
             Title="Better Color Selector">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Hue}" />
            <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

            <Slider Value="{Binding Saturation}" />
            <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

            <Slider Value="{Binding Luminosity}" />
            <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

이제 `HslColorViewModel` 개체의 동일한 원본 속성에 바인딩된 `Slider` 및 `Label` 요소 쌍이 3개 있습니다. 유일한 차이는 `Label`에 각 `Slider` 값을 표시하는 `StringFormat` 속성이 있는 것입니다.

[![더 나은 색 선택기](string-formatting-images/bettercolorselector-small.png "더 나은 색 선택기")](string-formatting-images/bettercolorselector-large.png#lightbox "더 나은 색 선택기")

RGB(빨강, 녹색, 파랑) 값을 기존의 2자리 16진수 형식으로 표현하는 방법이 궁금할 수도 있습니다. 이러한 정수 값은 `Color` 구조체에서 직접 사용할 수 없습니다. 한 가지 솔루션은 ViewModel에서 색 구성 요소의 정수 값을 계산하여 속성으로 나타내는 것입니다. 그런 다음, `X2` 형식 지정 사양을 사용하여 형식을 지정할 수 있습니다.

다른 방식은 더 일반적입니다. [**바인딩 값 변환기**](converters.md) 문서 뒷부분의 설명대로 ‘바인딩 값 변환기’를 작성할 수 있습니다.

하지만 다음 문서에서는 [**바인딩 경로**](binding-path.md)에 대해 더 자세히 살펴보고 이것을 사용하여 컬렉션의 하위 속성과 항목을 참조하는 방법을 보여줍니다.


## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모(샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 서적의 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
