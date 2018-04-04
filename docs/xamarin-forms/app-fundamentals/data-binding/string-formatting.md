---
title: 문자열 서식 지정
description: 데이터 바인딩을 사용 서식을 지정 하 고 개체를 문자열로 표시 하 여
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 4e143f650c3cde7577def1a95e53b207608a088a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="string-formatting"></a>문자열 서식 지정

경우에 따라 개체 또는 값의 문자열 표현을 표시 하려면 데이터 바인딩을 사용 하면 편리 합니다. 예를 들어 데 사용할 수는 `Label` 의 현재 값을 표시 하는 `Slider`합니다. 이 데이터 바인딩에서는 `Slider` 소스가 고 대상이 `Text` 의 속성은 `Label`합니다.

가장 강력한 도구는 정적 코드에서 문자열을 표시할 때 [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) 메서드. 서식을 지정할 값과 함께 다른 텍스트를 포함할 수 있습니다 및 서식 지정 문자열 형식 지정 코드에 다양 한 형식의 개체에 포함 됩니다. 참조는 [.net에서 형식 지정](/dotnet/standard/base-types/formatting-types/) 문자열 서식 지정에 대 한 자세한 내용은 문서입니다.

## <a name="the-stringformat-property"></a>StringFormat 속성

이 기능은 데이터 바인딩 전달 됩니다: 설정한는 [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) 속성 `Binding` (또는 [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.StringFormat/) 속성의는 `Binding` 태그 확장)에 표준.NET 하나의 자리 표시자와 문자열의 서식을 지정 합니다.

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

서식 문자열이 XAML 파서에 중괄호 다른 XAML 태그 확장으로 처리를 방지 하려면 작은따옴표 (아포스트로피) 문자로 구분 됩니다 확인 합니다. 작은따옴표 문자 동일한 문자열로 사용 하는 것에 대 한 호출에 부동 소수점 값을 표시 하지 않고 하는 문자열, `String.Format`합니다. 서식 사양 `F2` 하면 값이 소수점 두 자리까지 표시 합니다.

`StringFormat` 속성만 적합 한 유형의 대상 속성은 `string`, 바인딩 모드 이며 `OneWay` 또는 `TwoWay`합니다. 양방향 바인딩에 `StringFormat` 소스에서 대상에 전달 하는 값에만 적용 됩니다.

에 다음 문서에서 확인할 수는 [바인딩 경로](binding-path.md), 데이터 바인딩 매우 복잡 하 고 꼬인 될 수 있습니다. 추가할 수 있습니다 이러한 데이터 바인딩은 디버깅 하는 `Label` 와 XAML 파일에는 `StringFormat` 일부 중간 결과를 표시 합니다. 개체의 유형을 표시에 사용 하는 경우에 도움이 될 수 있는 합니다.

**문자열 서식 지정** 페이지에는의 몇 가지 사용 방법을 보여 줍니다.는 `StringFormat` 속성:

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

바인딩에 `Slider` 및 `TimePicker` 를 특정 형식 사양의 사용을 보여 `double` 및 `TimeSpan` 데이터 형식입니다. `StringFormat` 에서 텍스트를 표시 하는 `Entry` 보기를 사용 하 여 서식 문자열에 큰따옴표를 지정 하는 방법을 보여 줍니다는 `&quot;` HTML 엔터티.

XAML 파일의 다음 섹션은는 `StackLayout` 와 `BindingContext` 로 설정 된 `x:Static` 정적 참조 하는 태그 확장 `DateTime.Now` 속성입니다. 첫 번째 바인딩에 속성이 없습니다.

```xaml
<Label Text="{Binding}" />
```

단순히 표시 됩니다는 `DateTime` 의 값은 `BindingContext` 기본 서식 지정 합니다. 두 번째 바인딩을 표시는 `Ticks` 속성 `DateTime`, 다른 두 개의 바인딩 표시 하는 반면는 `DateTime` 특정 서식을 사용 하 여 자체입니다. 이런 `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

서식 문자열에서 왼쪽 또는 오른쪽 둥근 중괄호를 표시 해야 하는 경우 그 중 한 쌍을 사용 하면 됩니다.

마지막 섹션 집합의 `BindingContext` 의 값에 `Math.PI` 기본 서식 및 두 가지 종류의 숫자 형식 지정 함께 표시 됩니다.

다음은 세 플랫폼 모두에서 실행 중인 프로그램입니다.

[![서식 지정 문자열](string-formatting-images/stringformatting-small.png "서식 지정 문자열")](string-formatting-images/stringformatting-large.png#lightbox "서식 지정 문자열")

## <a name="viewmodels-and-string-formatting"></a>Viewmodel 및 문자열 서식 지정

사용 하는 `Label` 및 `StringFormat` 이기도 한 ViewModel 대상 보기의 값을 표시 하려면 정의할 수 있으 뷰에서 바인딩은 `Label` 또는에 ViewModel는 `Label`합니다. 일반적으로 보기 / ViewModel 간에 바인딩을 작동 하 고 있는지 확인 하기 때문에 두 번째 방법은 가장 좋습니다.

이 방법은에 나와 **더 나은 색 선택기** 샘플으로 동일한 ViewModel를 사용 하는 **간단한 색 선택기** 에 표시 된 프로그램의 [ **바인딩 모드** ](binding-mode.md) 문서:

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

이제의 세 쌍 있습니다 `Slider` 및 `Label` 동일 하 게 바인딩된 요소에 원본 속성에는 `HslColorViewModel` 개체입니다. 유일한 차이점은 `Label` 에 `StringFormat` 속성을 표시 하는 각 `Slider` 값입니다.

[![선택기를 더 잘 색](string-formatting-images/bettercolorselector-small.png "선택기를 더 잘 색")](string-formatting-images/bettercolorselector-large.png#lightbox "선택기를 더 잘 색")

기존의 두 자리 16 진수 형식의 RGB (빨강, 녹색, 파랑) 값을 표시할 수는 어떻게 궁금하실 수도 있습니다. 이러한 정수 값에서 직접 사용할 수 없습니다.는 `Color` 구조입니다. 한 가지 해결 ViewModel 내 색상 구성의 정수 값을 계산 하 고 속성으로 노출 하는 것입니다. 다음 서식을 수 있습니다를 사용 하는 `X2` 사양 서식 지정 합니다.

또 다른 방법은 보다 일반적인: 작성할 수 있습니다는 *바인딩 값 변환기* 이후 문서에 설명 된 대로 [ **바인딩 값 변환기**](converters.md)합니다.

그러나 탐색 하는 다음 문서는 [ **바인딩 경로** ](binding-path.md) 더 많은,에 대해 자세히 설명 하 고 사용 하 여 하위 속성 및 컬렉션의 항목을 참조 하는 방법을 보여 줍니다.


## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
