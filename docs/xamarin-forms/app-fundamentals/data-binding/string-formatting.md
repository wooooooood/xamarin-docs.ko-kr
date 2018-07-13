---
title: Xamarin.Forms 문자열 서식 지정
description: 이 문서에서는 데이터 바인딩을 Xamarin.FOrms를 사용 하 여 서식을 지정 하 고 개체를 문자열로 표시 하는 방법에 설명 합니다. 이 바인딩의 StringFormat 자리 표시자를 사용 하 여.NET 표준 서식 문자열을 설정 하 여 이루어집니다.
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: a876e81c67b6ec61a2cb29143cb001a7d6160032
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998149"
---
# <a name="xamarinforms-string-formatting"></a>Xamarin.Forms 문자열 서식 지정

경우에 따라 개체 또는 값의 문자열 표현에 표시할 데이터 바인딩을 사용 하는 것이 유용 합니다. 사용 하려는 하는 예를 들어, 한 `Label` 의 현재 값을 표시 하는 `Slider`. 데이터 바인딩에서이 `Slider` 소스가 고 대상이 `Text` 의 속성을 `Label`입니다.

가장 강력한 도구는 정적 코드에서 문자열을 표시 하는 경우 [ `String.Format` ](xref:System.String.Format(System.String,System.Object)) 메서드. 서식 문자열에 다양 한 유형의 개체를 특정 코드 서식 및 다른 텍스트 서식을 지정할 값과 함께 포함할 수 있습니다. 참조 된 [.NET의 서식 지정 형식](/dotnet/standard/base-types/formatting-types/) 문자열 서식 지정에 대 한 자세한 내용은 문서입니다.

## <a name="the-stringformat-property"></a>StringFormat 속성

이 기능은 데이터 바인딩 전달 됩니다: 설정한를 [ `StringFormat` ](xref:Xamarin.Forms.BindingBase.StringFormat) 속성을 `Binding` (또는 [ `StringFormat` ](xref:Xamarin.Forms.Xaml.BindingExtension.StringFormat) 속성을 `Binding` 태그 확장)에 한 자리 표시자를 사용 하 여 문자열 형식을 지정 하는 표준.NET:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

서식 문자열이 XAML 파서는 다른 XAML 태그 확장으로 중괄호를 처리 하지 않도록 하는 데 작은따옴표 (아포스트로피) 문자로 구분 되는 알 수 있습니다. 작은따옴표 문자에 대 한 호출에 부동 소수점 값을 표시 하는 데는 같은 문자열이 없는 해당 문자열이 고, 그렇지 `String.Format`합니다. 서식 사양 `F2` 하면 값이 두 개의 소수 자릿수로 표시 됩니다.

합니다 `StringFormat` 속성에만 의미가 대상 속성 유형의 경우 `string`, 바인딩 모드 이며 `OneWay` 또는 `TwoWay`합니다. 양방향 바인딩에 `StringFormat` 원본에서 대상에 전달 하는 값에만 적용 됩니다.

살펴보겠지만 다음 문서에서에 [바인딩 경로](binding-path.md), 매우 복잡 하 고 복잡 한 데이터 바인딩이 될 수 있습니다. 이러한 데이터 바인딩, 디버깅 하는 경우 추가할 수 있습니다는 `Label` 사용 하 여 XAML 파일에는 `StringFormat` 일부 중간 결과 표시 하려면. 개체의 형식을 표시에 사용 하는 경우에 유용할 수 있습니다.

합니다 **문자열 서식 지정** 페이지에는 몇 가지 사용법을 보여 줍니다는 `StringFormat` 속성:

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

에 대 한 바인딩 합니다 `Slider` 하 고 `TimePicker` 형식 사양 사용 하려면 특정 표시 `double` 및 `TimeSpan` 데이터 형식. `StringFormat` 의 텍스트를 표시 하는 합니다 `Entry` 보기에 사용 하 여 서식 지정 문자열에 큰따옴표를 지정 하는 방법을 보여 줍니다는 `&quot;` HTML 엔터티.

XAML 파일에 다음 섹션은는 `StackLayout` 사용 하 여는 `BindingContext` 로 `x:Static` 정적 참조 하는 태그 확장 `DateTime.Now` 속성입니다. 첫 번째 바인딩 속성이 없습니다.

```xaml
<Label Text="{Binding}" />
```

단순히 표시를 `DateTime` 의 값을 `BindingContext` 기본 서식을 사용 하 여 합니다. 두 번째 바인딩이 표시 합니다 `Ticks` 속성의 `DateTime`반면 다른 두 개의 바인딩 표시는 `DateTime` 특정 서식을 사용 하 여 자체. 이런 문제가 `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

서식 문자열에 중괄호를 왼쪽 또는 오른쪽 중괄호를 표시 해야 할 경우 그 중 한 쌍을 사용 하면 됩니다.

마지막 섹션 집합의 `BindingContext` 의 값으로 `Math.PI` 기본 서식 지정 및 두 가지 유형의 숫자 서식 지정을 사용 하 여 표시 합니다.

다음은 세 플랫폼 모두에서 실행 중인 프로그램이입니다.

[![문자열 서식 지정](string-formatting-images/stringformatting-small.png "문자열 서식 지정")](string-formatting-images/stringformatting-large.png#lightbox "문자열 서식 지정")

## <a name="viewmodels-and-string-formatting"></a>문자열 서식 지정 및 ViewModels

사용 하는 경우 `Label` 및 `StringFormat` 뷰를에서 바인딩을 정의 하거나 ViewModel의 대상이 되는 뷰의 값을 표시 하려면 합니다 `Label` 또는 ViewModel에는 `Label`합니다. 일반적으로 뷰와 ViewModel 간의 바인딩은 작동 하는지 확인 하기 때문에 두 번째 방법은 적합 합니다.

이 방법을 보여 줍니다는 **더 나은 색 선택기** 하는 경우 동일한 ViewModel을 사용 하는 샘플을 **간단한 색 선택기** 에 표시 된 프로그램을 [ **바인딩 모드** ](binding-mode.md) 문서:

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

세 쌍의는 이제 `Slider` 하 고 `Label` 동일 하 게 바인딩된 요소에 소스 속성에는 `HslColorViewModel` 개체입니다. 유일한 차이점은 `Label` 에 `StringFormat` 속성을 표시 하는 각 `Slider` 값입니다.

[![선택기를 더 잘 색](string-formatting-images/bettercolorselector-small.png "선택기를 더 잘 색")](string-formatting-images/bettercolorselector-large.png#lightbox "선택기를 더 잘 색")

궁금할 수 있습니다 어떻게 기존의 두 자리 16 진수 형식의 RGB (빨강, 녹색, 파랑) 값을 표시할 수 있습니다. 해당 정수 값에서 직접 사용할 수 없습니다는 `Color` 구조입니다. 하나의 솔루션 ViewModel 내 색 구성 요소의 정수 값을 계산 하 고 속성으로 노출 하는 것입니다. 다음 형식 수를 사용 하 여는 `X2` 사양 서식 지정 합니다.

또 다른 방법은 보다 일반적인: 작성할 수 있습니다는 *바인딩 값 변환기* 뒷부분에 나오는 문서에서 설명 했 듯이 [ **바인딩 값 변환기**](converters.md)합니다.

그러나 다음 문서를 탐색 합니다 [ **바인딩 경로** ](binding-path.md) 더 detail 및 사용 하 여 컬렉션의 하위 속성 및 항목을 참조 하는 방법을 보여 줍니다.


## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
