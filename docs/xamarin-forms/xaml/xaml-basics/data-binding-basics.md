---
title: "4 부입니다. 데이터 바인딩 기본 사항"
description: "데이터 바인딩을 변경 하나에 다른 변경으로 인해 되도록 연결 된 두 개체의 속성을 허용 합니다. 매우 중요 한 도구 이며 코드에서 완전히 데이터 바인딩을 정의할 수 있지만 XAML에서는 바로 가기 키, 편리 함을 제공 합니다. 따라서 Xamarin.Forms의 가장 중요 한 태그 확장 중 하나는 바인딩 됩니다."
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 46e0c1f9b2aff52c1d31774a15e818c78a70056a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="part-4-data-binding-basics"></a>4 부입니다. 데이터 바인딩 기본 사항

_데이터 바인딩을 변경 하나에 다른 변경으로 인해 되도록 연결 된 두 개체의 속성을 허용 합니다. 매우 중요 한 도구 이며 코드에서 완전히 데이터 바인딩을 정의할 수 있지만 XAML에서는 바로 가기 키, 편리 함을 제공 합니다. 따라서 Xamarin.Forms의 가장 중요 한 태그 확장 중 하나는 바인딩 됩니다._

## <a name="data-bindings"></a>데이터 바인딩

데이터 바인딩 라고 하는 두 개체의 속성을 연결 된 *소스* 및 *대상*합니다. 코드에서 두 단계를 수행 해야:는 `BindingContext` 원본 개체에는 대상 개체의 속성을 설정 해야 및 `SetBinding` 메서드 (함께에서 자주 사용 되는 `Binding` 클래스)에 바인딩하는 속성의 대상 개체에서 호출 되어야 합니다 개체는 원본 개체의 속성입니다.

대상 속성에는 대상 개체에서 파생 되어야 합니다 즉 바인딩 가능한 속성 이어야 합니다 `BindableObject`합니다. 온라인 Xamarin.Forms 문서 속성은 바인딩 가능한 속성을 나타냅니다. 속성 `Label` 같은 `Text` 바인딩할 수 있는 속성과 연결 된 `TextProperty`합니다.

태그에서도 코드에서 필요한 동일한 두 단계를 수행 해야 점을 제외 하 고는 `Binding` 태그 확장의 일어나는 `SetBinding` 호출 및 `Binding` 클래스입니다.

그러나 XAML에서 데이터 바인딩을 정의할 때 있습니다 여러 가지 방법으로 설정 하 여 `BindingContext` 대상 개체의 합니다. 경우에 따라 때로는 사용 하 여 코드 숨김 파일에서 설정 됩니다는 `StaticResource` 또는 `x:Static` 태그 확장의 내용으로 또는 `BindingContext` 속성 요소 태그입니다.

바인딩 가장 자주 사용 되는 MVVM (모델-뷰-ViewModel) 응용 프로그램 아키텍처의 구현에 일반적으로 기본 데이터 모델을 프로그램의 시각적 개체를 연결 하는 데에 설명 된 대로 [5 부 합니다. MVVM에 대 한 데이터 바인딩을에서](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), 하지만 다른 시나리오가 가능 합니다.

## <a name="view-to-view-bindings"></a>보기-바인딩

연결 속성 같은 페이지에는 두 보기 중에 대 한 데이터 바인딩을 정의할 수 있습니다. 설정 하는 경우에 `BindingContext` 사용 하 여 대상 개체의는 `x:Reference` 태그 확장 합니다.

다음은 포함 된 XAML 파일은 `Slider` 와 두 개의 `Label` 뷰 중 하나는 방향으로 회전는 `Slider` 값을 표시 하는 `Slider` 값:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderBindingsPage"
             Title="Slider Bindings Page">

    <StackLayout>
        <Label Text="ROTATION"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Slider` 포함 한 `x:Name` 두에서 참조 되는 특성 `Label` 를 사용 하 여 뷰는 `x:Reference` 태그 확장 합니다.

`x:Reference` 라는 속성을 정의 바인딩 확장 `Name` 이 경우 참조 된 요소의 이름으로 설정 `slider`합니다. 그러나는 `ReferenceExtension` 정의 하는 클래스는 `x:Reference` 태그 확장은 정의 `ContentProperty` 특성에 대 한 `Name`, 명시적으로 필요 없음을 의미 합니다. 다양 한에 대 한 첫 번째 `x:Reference` 포함 "Name =" 하지만 두 번째 하지 않습니다:

```csharp
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

`Binding` 태그 확장 자체와 동일 하 게 몇 가지 속성을 가질 수 있습니다는 `BindingBase` 및 `Binding` 클래스입니다. `ContentProperty` 에 대 한 `Binding` 은 `Path`, 하지만 "경로 =" 경로에서 첫 번째 항목은 경우에 태그 확장의 일부를 생략할 수 있습니다는 `Binding` 태그 확장 합니다. 첫 번째 예제는 "Path =" 두 번째 예에서는 생략 되지만:

```csharp
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

한 줄에 있을 수 이나 여러 줄으로 구분 된 속성:

```csharp
Text="{Binding Value, 
               StringFormat='The angle is {0:F0} degrees'}"
```

무엇이 편리 하 게 수행 합니다.

공지는 `StringFormat` 두 번째에서 속성 `Binding` 태그 확장 합니다. Xamarin.forms에 바인딩 모든 암시적 형식 변환을 수행 하지 않습니다 및 형식 변환기를 제공 하거나 사용 해야 문자열이 아닌 개체를 문자열로 표시 하기 위해 필요한 경우 `StringFormat`합니다. 내부적으로 정적 `String.Format` 메서드를 구현 하는 `StringFormat`합니다. 즉 잠재적으로 문제를.NET 서식 사양을 태그 확장을 구분 하는 데 사용 되는 중괄호를 포함 하기 때문입니다. 이렇게 하면 XAML 파서에 혼란 스 러의 위험이 만들어집니다. 이 문제를 작은따옴표로 전체 서식 문자열을 넣으십시오.

```csharp
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

다음은 실행 중인 프로그램이입니다.

[ ![](data-binding-basics-images/sliderbinding.png "보기-바인딩")](data-binding-basics-images/sliderbinding-large.png "보기-바인딩 ")

## <a name="the-binding-mode"></a>바인딩 모드 

단일 보기에는 일부의 속성에 데이터 바인딩을 가질 수 있습니다. 그러나 각 보기 하나만 가질 수 있습니다 `BindingContext`이므로 해당 보기에 여러 개의 데이터 바인딩이 해야 모두 같은 개체의 속성을 참조 합니다.

이 및 다른 문제에 솔루션에서는 `Mode` 의 구성원으로 설정 된 속성은 `BindingMode` 열거형:

- `Default` 
- `OneWay` -값이 소스에서 대상으로 전송 됩니다
- `OneWayToSource` -값은 적용할 대상에서 소스
- `TwoWay` -값 원본과 대상 간의 양방향 전송 됩니다

다음 프로그램에서는 일반적으로 사용 된 `OneWayToSource` 및 `TwoWay` 바인딩 모드입니다. 4 개의 `Slider` 뷰 제어 하기 위한 것은 `Scale`, `Rotate`, `RotateX`, 및 `RotateY` 의 속성은 `Label`합니다. 처음에 보이는 처럼 이러한 네 가지 속성의는 `Label` 에 의해 설정 되는 각 하기 때문에 데이터 바인딩 대상으로 해야는 `Slider`합니다. 그러나는 `BindingContext` 의 `Label` 하나의 개체가 될 수 있고 4 개의 다른 슬라이더 합니다.

이러한 이유 때문에 대 한 모든 바인딩을에서 설정 됩니다 겉보기 이전 버전과 같은 방법으로:는 `BindingContext` 로 설정 된 각각의 4 개 슬라이더는 `Label`에 바인딩을 설정 하 고는 `Value` 슬라이더의 속성입니다. 사용 하 여는 `OneWayToSource` 및 `TwoWay` 모드에서는 이러한 `Value` 속성에는 원본 속성을 설정할 수는 `Scale`, `Rotate`, `RotateX`, 및 `RotateY` 의 속성은 `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderTransformsPage"
             Padding="5"
             Title="Slider Transforms Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <!-- Scaled and rotated Label -->
        <Label x:Name="label"
               Text="TEXT"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <!-- Slider and identifying Label for Scale -->
        <Slider x:Name="scaleSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="1" Grid.Column="0"
                Maximum="10"
                Value="{Binding Scale, Mode=TwoWay}" />

        <Label BindingContext="{x:Reference scaleSlider}"
               Text="{Binding Value, StringFormat='Scale = {0:F1}'}"
               Grid.Row="1" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for Rotation -->
        <Slider x:Name="rotationSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="2" Grid.Column="0"
                Maximum="360"
                Value="{Binding Rotation, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationSlider}"
               Text="{Binding Value, StringFormat='Rotation = {0:F0}'}"
               Grid.Row="2" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationX -->
        <Slider x:Name="rotationXSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="3" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationX, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationXSlider}"
               Text="{Binding Value, StringFormat='RotationX = {0:F0}'}"
               Grid.Row="3" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationY -->
        <Slider x:Name="rotationYSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="4" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationY, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationYSlider}"
               Text="{Binding Value, StringFormat='RotationY = {0:F0}'}"
               Grid.Row="4" Grid.Column="1"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

바인딩을 중 3 개는 `Slider` 뷰는 `OneWayToSource`즉, 하는 `Slider` 값의 속성에서 변경으로 인해 해당 `BindingContext`, 변수인는 `Label` 라는 `label`합니다. 이 세 가지 `Slider` 으로 인해 뷰는 `Rotate`, `RotateX`, 및 `RotateY` 의 속성은 `Label`합니다.

그러나에 대 한 바인딩을 `Scale` 속성은 `TwoWay`합니다. ¿¡´는 `Scale` 속성에는 기본값은 1 일을 사용 하 여 한 `TwoWay` 원인을 바인딩는 `Slider` 초기 값을 0이 아닌 1에서 설정할 수 있습니다. 해당 바인딩이 되었으면 `OneWayToSource`, `Scale` 속성은 처음에서 0으로 설정 됩니다는 `Slider` 기본값입니다. `Label` 사용자에 게 혼란을 일으킬 수 있는 및 표시 됩니다.

 [ ![](data-binding-basics-images/slidertransforms.png "이전 버전과 바인딩")](data-binding-basics-images/slidertransforms-large.png "뒤로 바인딩")

## <a name="bindings-and-collections"></a>바인딩 및 컬렉션

템플릿 기반 보다 더 나은 XAML 및 데이터 바인딩 기능을 보여 줍니다 nothing `ListView`합니다.

`ListView` 정의 `ItemsSource` 형식의 속성이 `IEnumerable`, 해당 컬렉션의 항목을 표시 하 고 있습니다. 이러한 항목에는 특정 유형의 개체 수 있습니다. 기본적으로 `ListView` 사용 하 여는 `ToString` 메서드 해당 항목을 표시 하려면 각 항목의 합니다. 방금 원하는 하는 경우가 있지만 대부분의 경우에서 `ToString` 만 개체의 정규화 된 클래스 이름을 반환 합니다.

그러나 항목에는 `ListView` 컬렉션을 사용 하 여 원하는 방식으로 표시할 수는 *템플릿*에서 파생 된 클래스와 관련이 있는 `Cell`합니다. 서식 파일에 있는 모든 항목에 대 한 복제 되는 `ListView`, 서식 파일에 설정 된 데이터 바인딩 개별 복제본에 전송 됩니다.

자주 사용 하 여 이러한 항목에 대 한 사용자 지정 셀을 만들어서 합니다는 `ViewCell` 클래스입니다. 이 프로세스는 코드에서 다소 복잡 한 아니지만 XAML에서 매우 간단 하 게 됩니다.

XamlSamples에 포함 된 프로젝트는 라는 클래스 `NamedColor`합니다. 각 `NamedColor` 개체에 `Name` 및 `FriendlyName` 형식의 속성 `string`, 및 `Color` 형식의 속성이 `Color`합니다. 또한 `NamedColor` 형식의 141 정적 읽기 전용 필드에 `Color` 에 해당 하는 Xamarin.Forms에 정의 된 색 `Color` 클래스입니다. 정적 생성자를 만듭니다는 `IEnumerable<NamedColor>` 포함 된 컬렉션 `NamedColor` 이러한 정적 필드에 해당 하는 개체를 만들어 클래스의 공용 정적 할당 `All` 속성입니다.

정적 설정 `NamedColor.All` 속성을는 `ItemsSource` 의 `ListView` 쉽게 사용 하는 `x:Static` 태그 확장:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

결과 디스플레이 설정 항목 유형의 진정한은 `XamlSamples.NamedColor`:

[ ![](data-binding-basics-images/listview1.png "컬렉션에 바인딩")](data-binding-basics-images/listview1-large.png "컬렉션에 바인딩")

많은 정보 않습니다 되지만 `ListView` 스크롤 가능 하 고 선택할 수는 있습니다.

항목에 대 한 서식 파일을 정의 하려면 분해 해야는 `ItemTemplate` 속성 요소로 속성으로 설정 하는 `DataTemplate`, 다음 참조 하는 `ViewCell`합니다. 에 `View` 의 속성은 `ViewCell` 각 항목을 표시 하려면 하나 이상의 보기의 레이아웃을 정의할 수 있습니다. 간단한 예는 다음과 같습니다.

```xaml
<ListView ItemsSource="{x:Static local:NamedColor.All}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.View>
                    <Label Text="{Binding FriendlyName}" />
                </ViewCell.View>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

`Label` 요소가로 설정 되는 `View` 의 속성은 `ViewCell`합니다. (의 `ViewCell.View` 태그 되므로 세 키워드가 필요 하지는 `View` 속성의 콘텐츠 속성은 `ViewCell`.) 이 태그 표시는 `FriendlyName` 각 속성 `NamedColor` 개체:

[ ![](data-binding-basics-images/listview2.png "DataTemplate 사용 하 여 컬렉션에 바인딩")](data-binding-basics-images/listview2-large.png "DataTemplate 사용 하 여 컬렉션에 바인딩")

훨씬 나은. 이제 필요한 모든 되려고 가문비나무 자세한 정보 및 실제 색 항목 템플릿을 구성 합니다. 이 서식 파일을 지원 하려면 일부 값과 개체에에서 정의 된 페이지의 리소스 사전:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <OnPlatform x:Key="boxSize"
                        x:TypeArguments="x:Double">
                <On Platform="iOS, Android, UWP" Value="50" />
            </OnPlatform>

            <OnPlatform x:Key="rowHeight"
                        x:TypeArguments="x:Int32">
                <On Platform="iOS, Android, UWP" Value="60" />
            </OnPlatform>

            <local:DoubleToIntConverter x:Key="intConverter" />

        </ResourceDictionary>
    </ContentPage.Resources>

    <ListView ItemsSource="{x:Static local:NamedColor.All}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <StackLayout Padding="5, 5, 0, 5"
                                 Orientation="Horizontal"
                                 Spacing="15">

                        <BoxView WidthRequest="{StaticResource boxSize}"
                                 HeightRequest="{StaticResource boxSize}"
                                 Color="{Binding Color}" />

                        <StackLayout Padding="5, 0, 0, 0"
                                     VerticalOptions="Center">

                            <Label Text="{Binding FriendlyName}"
                                   FontAttributes="Bold"
                                   FontSize="Medium" />

                            <StackLayout Orientation="Horizontal"
                                         Spacing="0">
                                <Label Text="{Binding Color.R,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat='R={0:X2}'}" />

                                <Label Text="{Binding Color.G,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', G={0:X2}'}" />

                                <Label Text="{Binding Color.B,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', B={0:X2}'}" />
                            </StackLayout>
                        </StackLayout>
                    </StackLayout>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

사용 `OnPlatform` 의 크기를 정의 하는 `BoxView` 및의 높이 `ListView` 행. 세 플랫폼 모두에 대 한 값이 동일 하지만 태그를 쉽게 디스플레이 세밀 하 게 조정 다른 값에 적용할 수 있습니다. 

## <a name="binding-value-converters"></a>바인딩 값 변환기

이전 **ListView 데모** XAML 파일의 개별 표시 `R`, `G`, 및 `B` 는 Xamarin.Forms의 속성 `Color` 구조입니다. 이러한 속성은 형식의 `double` 및 범위는 0에서 1입니다. 16 진수 값을 표시 하려는 경우 사용할 수 `StringFormat` 는 "X2" 사양 서식을 사용 합니다. 정수에 대 한 및 외에도, 에서만 작동 하는 `double` 255 곱해집니다 값이 필요 합니다.

사용 하 여 이러한 문제를 해결 한 *값 변환기*라고도 하는 *바인딩 변환기*합니다. 이것은 구현 하는 클래스는 `IValueConverter` 있기 라는 두 가지 방법 즉 인터페이스 `Convert` 및 `ConvertBack`합니다. `Convert` 메서드 값이 소스에서 대상;으로 전송 될 때 호출 됩니다는 `ConvertBack` 메서드는 전송에 대 한 대상에서 원본에 `OneWayToSource` 또는 `TwoWay` 바인딩:

```csharp
using System;
using System.Globalization;
using Xamarin.Forms;

namespace XamlSamples
{
    class DoubleToIntConverter : IValueConverter
    {
        public object Convert(object value, Type targetType,
                              object parameter, CultureInfo culture)
        {
            double multiplier;

            if (!Double.TryParse(parameter as string, out multiplier))
                multiplier = 1;

            return (int)Math.Round(multiplier * (double)value);
        }

        public object ConvertBack(object value, Type targetType,
                                  object parameter, CultureInfo culture)
        {
            double divider;

            if (!Double.TryParse(parameter as string, out divider))
                divider = 1;

            return ((double)(int)value) / divider;
        }
    }
}
```

`ConvertBack` 메서드에서에서 재생 되지 않으면 역할이이 프로그램 바인딩에 있기 때문에 한 가지 방법은 소스에서 대상으로 합니다. 

바인딩 바인딩 변환기를 참조 하는 `Converter` 속성입니다. 바인딩 변환기와 지정 된 매개 변수도 받아들일 수 있습니다는 `ConverterParameter` 속성입니다. 몇 가지 다양 한 기능에 대 한 승수를 지정 하는 방법을입니다. 올바른 변환기 매개 변수를 확인 하는 바인딩 변환기 `double` 값입니다.

여러 바인딩 간에 공유할 수 있도록 리소스 사전에서 변환기가 인스턴스화됩니다.

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

이 단일 인스턴스를 참조 하는 세 가지 데이터 바인딩. 에 `Binding` 포함 된 태그 확장 포함 `StaticResource` 태그 확장:

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

다음은 결과가입니다.

[ ![](data-binding-basics-images/listview3.png "DataTemplate 및 변환기를 사용 하 여 컬렉션에 바인딩")](data-binding-basics-images/listview3-large.png "DataTemplate 및 변환기를 사용 하 여 컬렉션에 바인딩")

`ListView` 은 매우 정교 내부에서 동적으로 발생할 수 있는 변경 내용 처리의 경우에만 아니라 데이터가 특정 단계를 수행 합니다. 항목의 컬렉션에 할당 된 경우는 `ItemsSource` 속성은 `ListView` 런타임 중에 변경-하는 항목에 추가할 수 있는 경우 또는 컬렉션에서 제거-사용 하 여는 `ObservableCollection` 이러한 항목에 대 한 클래스입니다. `ObservableCollection` 구현 하는 `INotifyCollectionChanged` 인터페이스 및 `ListView` 에 대 한 처리기를 설치 합니다는 `CollectionChanged` 이벤트입니다.

런타임 중 자체의 속성을 변경한 후 컬렉션의 항목을 구현 해야 하는 경우는 `INotifyPropertyChanged` 인터페이스 및 신호 변경 내용을 사용 하 여 속성 값은 `PropertyChanged` 이벤트입니다. 이 시리즈의 다음 부분에서이 확인할 [5 부 합니다. MVVM에 데이터 바인딩에서](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)합니다.

## <a name="summary"></a>요약

데이터 바인딩 속성 페이지 내에서 두 개체 간의 또는 시각적 개체 간의 연결 및 내부 데이터에 대 한 강력한 메커니즘을 제공 합니다. 하지만 유용한 패러다임으로 대입 인기 있는 응용 프로그램 아키텍처 패턴 시작 되는 응용 프로그램 데이터 원본 사용을 시작 하는 경우. 이에 대해서는 [5 부 합니다. MVVM에 대 한 데이터 바인딩을에서](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)합니다.



## <a name="related-links"></a>관련 링크

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [1 부입니다. XAML (샘플)를 시작 하기](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2 부 합니다. 필수 XAML 구문 (샘플)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [3 부 합니다. XAML 태그 확장 (샘플)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [5 단계입니다. MVVM (샘플)에 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
