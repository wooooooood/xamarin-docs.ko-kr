---
title: 4부. 데이터 바인딩 기본 사항
description: 데이터 바인딩은 한 개체의 변경으로 인해 다른 개체의 변경이 일어나도록 두 개체의 속성을 연결하도록 해 줍니다.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 342288C3-BB4C-4924-B178-72E112D777BA
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: bd13163b513ea1f6b0381e99e65d0bd727f97735
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055730"
---
# <a name="part-4-data-binding-basics"></a>4부. 데이터 바인딩 기본 사항

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)

_데이터 바인딩은 한 개체의 변경으로 인해 다른 개체의 변경이 일어나도록 두 개체의 속성을 연결하도록 해줍니다. 매우 유용한 도구이며 데이터 바인딩은 코드로만 정의할 수 있는 반면 XAML은 바로 가기 및 편의성을 제공합니다. 따라서 Xamarin.Forms에서 태그 확장 중 가장 중요한 하나는 바인딩(Binding)입니다._

## <a name="data-bindings"></a>데이터 바인딩

데이터 바인딩은 두 개체의 속성을 연결하고, *source* 및 *target*을 호출합니다. 코드에서 두 가지 단계가 필요합니다. 대상(target) 개체의 `BindingContext`는 원본(source) 개체로 설정되어야 하며, `SetBinding` 메서드(종종 `Binding` 클래스와 함께 사용됨)는 해당 개체의 속성을 원본 개체의 속성으로 바인딩하는 대상 개체에서 호출해야 합니다.

대상 속성은 대상 개체가 `BindableObject`에서 파생되어야 한다는 것을 의미하는 바인딩 가능한 속성이어야 합니다. 온라인 Xamarin.Forms 문서는 어떤 속성이 바인딩 가능한 속성인지를 나타냅니다. `Text`와 같은 `Label`의 속성은 바인딩 가능한 속성 `TextProperty`와 연관됩니다.

태그에서도 `Binding` 태그 확장이 `SetBinding` 호출과 `Binding` 클래스를 대신한다는 점을 제외하면 코드에서 필요한 것과 동일한 두 단계를 수행해야 합니다.

그러나 XAML에서 데이터 바인딩을 정의할 때 대상 개체의 `BindingContext`를 설정하는 여러 가지 방법이 있습니다. 때로는 `StaticResource`나 `x:Static` 태그 확장을 사용하여, 때로는 `BindingContext` 속성 요소 태그의 내용으로 코드 비하인드에서 설정됩니다.

바인딩은 [5부. 데이터 바인딩부터 MVVM까지](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)에서 설명한 대로 일반적으로 MVVM(Model View ViewModel) 응용 프로그램 아키텍처의 구현에서 프로그램의 비주얼을 기본 데이터 모델과 연결하는 데 가장 많이 사용되지만, 다른 시나리오도 가능합니다.

## <a name="view-to-view-bindings"></a>뷰를 뷰에 바인딩

같은 페이지에서 두 가지 뷰 속성을 연결하기 위한 데이터 바인딩을 정의할 수 있습니다. 이 경우 `x:Reference` 태그 확장을 사용하여 대상 개체의 `BindingContext`를 설정합니다.

다음은 `Slider`와 두 개의 `Label` 뷰를 포함하는 XAML 파일입니다. 다음과 같이 레이블 중 하나는 `Slider` 값에 의해 회전되고, 다른 하나는 `Slider` 값을 표시합니다.

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

`Slider`는 `x:Reference` 태그 확장을 사용하여 두 개의 `Label` 뷰에 의해 참조되는 `x:Name` 특성을 포함하고 있습니다.

`x:Reference` 바인딩 확장은 참조된 요소의 이름으로 설정하기 위해 `Name`이라는 속성을 정의하는데, 이 경우에는 `slider`입니다. 그러나 `x:Reference` 태그 확장을 정의하는 `ReferenceExtension` 클래스는 또한 명시적으로 필수가 아닌 `Name`을 위한 `ContentProperty` 특성을 정의합니다. 다양성을 위해서, 다음과 같이 `x:Reference`는 "Name="을 포함하지만 두 번째는 포함하지 않습니다.

```csharp
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

`Binding` 태그 확장 자체는 `BindingBase` 및 `Binding` 클래스처럼 여러 속성을 가질 수 있습니다. `Binding`에 대한 `ContentProperty`는 `Path`이지만, 태그 확장의 일부인 "Path="은 경로가 `Binding` 태그 확장에서 첫 번째 항목인 경우 생략할 수 있습니다. 다음과 같이 첫 번째 예제는 "Path="이 있지만 두 번째 예제에서는 생략합니다.

```csharp
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

다음과 같이 속성은 모두 한줄에 표현하거나 여러 줄로 구분할 수 있습니다.

```csharp
Text="{Binding Value,
               StringFormat='The angle is {0:F0} degrees'}"
```

무엇이든 편리한 것을 수행하면 됩니다.

`Binding` 태그 확장에서 두 번째 `StringFormat` 속성을 확인하십시오. Xamarin.Forms에 바인딩은 모든 암시적 유형 변환을 수행하지 않으므로 문자열이 아닌 개체를 문자열로 표시해야 하는 경우 유형 변환기를 제공하거나 `StringFormat`을 사용해야 합니다. 배후에서 정적(static) `String.Format` 메서드가 `StringFormat`을 구현하는 데 사용됩니다. .NET 서식 지정 사양에는 중괄호가 포함되어 있기 때문에 잠제적으로 문제가 될 수 있습니다. 이로 인해 XAML 파서가 혼동될 위험이 생깁니다. 해당 문제를 방지하려면 작은따옴표로 전체 서식 문자열을 묶습니다.

```csharp
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

실행 프로그램은 다음과 같습니다.

[![](data-binding-basics-images/sliderbinding.png "뷰를 바인딩")](data-binding-basics-images/sliderbinding-large.png#lightbox "뷰를 바인딩 ")

## <a name="the-binding-mode"></a>바인딩 모드

단일 뷰는 여러 속성에 데이터 바인딩을 가질 수 있습니다. 그러나 각 뷰는 단 하나의 `BindingContext`만 가질 수 있기 때문에 해당 뷰에 다수의 데이터를 바인딩하려면 동일한 개체의 모든 속성을 참조해야 합니다.

해당 문제와 기타 문제에 대한 해결책은 다음과 같이 `BindingMode` 열거형의 멤버로 설정된 `Mode`의 속성과 관련됩니다.

- `Default`
- `OneWay` -값이 원본(source)에서 대상(target)으로 전송됩니다.
- `OneWayToSource` -값이 대상에서 원본으로 전송됩니다.
- `TwoWay` -값이 원본과 대상 간의 값 양방향으로 전송됩니다.

다음 프로그램은 `OneWayToSource`와 `TwoWay` 바인딩 모드의 일반적인 사용법을 보여줍니다. 네 개의 `Slider` 뷰는 `Label`의 `Scale`, `Rotate`, `RotateX` 및 `RotateY` 속성을 제어하기 위한 것입니다. 처음에는 `Label`의 네 가지 속성이 각각 `Slider`에 의해 설정되기 때문에 데이터 바인딩 대상이어야 하는 것처럼 보입니다. 그러나, `Label`의 `BindingContext`는 하나의 개체일 수 있으며, 네 개의 다른 슬라이더가 있습니다.

이러한 이유 때문에 모든 바인딩은 겉으로 보기에 역으로 설정된 것 같습니다. 네 개의 슬라이더 각각의 `BindingContext`는 `Label`에 설정되고 바인딩은 슬라이더의 `Value` 속성에 설정됩니다. `OneWayToSource` 및 `TwoWay` 모드를 사용하면, 다음과 같이 `Value` 속성은 `Label`의 `Scale`, `Rotate`, `RotateX` 및 `RotateY` 속성인 원본 속성을 설정할 수 있습니다.

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

`Slider` 뷰 중 3개의 바인딩은 `OneWayToSource`이며, `Slider` 값은 `label`이라는 `Label`의 해당 `BindingContext` 속성에 변화를 일으킨다는 것을 의미합니다. 해당 세 가지 `Slider` 뷰는 `Label`의 `Rotate`, `RotateX` 및 `RotateY` 속성을 변경시킵니다.

그러나 `Scale` 속성에 대한 바인딩은 `TwoWay`입니다. 이것은 `Scale` 속성이 기본값 1을 가지며 `TwoWay` 바인딩을 사용하면 `Slider` 초기 값이 0이 아닌 1로 설정됩니다. 해당 바인딩이 `OneWayToSource`라면 `Scale` 속성은 초기에 `Slider` 기본값 0으로 설정됩니다. `Label`이 표시되지 않으며, 사용자에게 약간의 혼동이 발생할 수 있습니다.

 [![](data-binding-basics-images/slidertransforms.png "역 바인딩")](data-binding-basics-images/slidertransforms-large.png#lightbox "역 바인딩")

 > [!NOTE]
 > [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 클래스는 또한 x축 및 y축 각각 `VisualElement`로 크기를 조정하는 [ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX) 및 [ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY) 속성을 가지고 있습니다.

## <a name="bindings-and-collections"></a>바인딩 및 컬렉션

템플릿 기반 `ListView`보다 XAML 및 데이터 바인딩의 기능을 잘 보여주는 것은 없습니다.

`ListView`는 `IEnumerable` 유형의 `ItemsSource` 속성을 정의하고, 해당 컬렉션의 항목을 표시합니다. 해당 항목은 모든 유형의 개체가 될 수 있습니다. 기본적으로 `ListView`는 각 항목의 `ToString` 메서드를 사용하여 해당 항목을 표시합니다. 경우에 따라 이것이 바로 원하는 것일 수 있지만 대부분의 경우에 `ToString`은 개체의 완전한 클래스 이름만을 반환합니다.

그러나 `ListView` 컬렉션의 항목은 `Cell`에서 파생된 클래스를 포함하는 *template*을 사용하여 원하는 방식으로 표시할 수 있습니다. 템플릿은 `ListView`의 모든 항목에 대해 복제되고, 템플릿에서 설정된 데이터 바인딩은 개별 복제본으로 전송됩니다.

`ViewCell` 클래스를 사용하여 해당 항목에 대한 사용자 지정 셀을 생성하는 경우가 많습니다. 이 프로세스는 코드에서 다소 복잡하지만 XAML에서는 매우 간단합니다.

XamlSamples 프로젝트에는 `NamedColor`라는 클래스가 포함되어 있습니다. 각각의 `NamedColor` 개체는 `Name`과 `FriendlyName` 속성이 `string` 유형이고 `Color` 속성이 `Color` 유형입니다. 또한 `NamedColor`는 Xamarin.Forms `Color` 클래스에 정의된 색상에 해당하는 `Color` 유형의 141개의 정적 읽기 전용 필드를 가지고 있습니다. 정적 생성자는 해당 정적 필드에 대응하는 `NamedColor` 개체를 포함하는 `IEnumerable<NamedColor>` 컬렉션을 생성하고 그것을 자신의 공용(public) 정적(static) `All` 속성에 할당합니다.

정적 `NamedColor.All` 속성을 `ListView`의 `ItemsSource`에 설정하는 것은 다음과 같이 `x:Static` 태그 확장을 사용하면 간단합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

다음과 같이 결과 표시는 항목이 실제로 `XamlSamples.NamedColor` 유형임을 입증하고 있습니다.

[![](data-binding-basics-images/listview1.png "컬렉션에 바인딩")](data-binding-basics-images/listview1-large.png#lightbox "컬렉션에 바인딩")

정보가 많지는 않지만 `ListView`는 스크롤이 가능하고 선택이 가능합니다.

항목에 대한 템플릿을 정의하려면 `ItemTemplate` 속성을 속성 요소로 분리하여 `ViewCell`을 참조하는 `DataTemplate`으로 설정합니다. `ViewCell`의 `View` 속성에 각 항목을 표시하기 위한 하나 이상의 뷰 레이아웃을 정의할 수 있습니다. 다음은 간단한 예 입니다.

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

`Label` 요소는 `ViewCell`의 `View` 속성으로 설정됩니다. (`ViewCell.View` 태그는 `View` 속성이 `ViewCell`의 콘텐츠 속성이므로 필요하지 않습니다.) 해당 태그는 다음과 같이 각 `NamedColor` 개체의 `FriendlyName` 속성을 표시합니다.

[![](data-binding-basics-images/listview2.png "DataTemplate을 사용하여 컬렉션에 바인딩")](data-binding-basics-images/listview2-large.png#lightbox "DataTemplate을 사용하여 컬렉션에 바인딩")

훨씬 낫습니다. 이제 더 많은 정보와 실제 색상으로 항목 템플릿을 멋지게 꾸미기만 하면 됩니다. 해당 템플릿을 지원하기 위해 일부 값과 개체가 다음과 같이 페이지의 리소스 사전에 정의되어 있습니다.

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

    <ListView ItemsSource="{x:Static local:NamedColor.All}"
              RowHeight="{StaticResource rowHeight}">
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

`BoxView`의 크기와 `ListView` 행의 높이를 정의하기 위해 `OnPlatform`을 사용합니다. 모든 플랫폼의 값은 동일하지만 태그를 다른 값으로 쉽게 조정하여 표시되는 것을 미세 조정할 수 있습니다.

## <a name="binding-value-converters"></a>바인딩 값 변환기

이전 **ListView 데모** XAML 파일은 Xamarin.Forms `Color` 구조체의 개별 `R`, `G`, `B` 속성을 표시합니다. 해당 속성은 `double` 유형이고 범위는 0에서 1까지입니다. 16 진수 값을 표시하려는 경우 "X2" 형식 지정과 함께 단순히 `StringFormat`을 사용할 수 없습니다. 그것은 정수에만 작동하고, 게다가 `double` 값에는 255를 곱해야 합니다.

이 사소한 문제는 *바인딩 변환기*라고도 하는 *값 변환기(value converter)*로 해결되었습니다. 이 클래스는 `IValueConverter` 인터페이스를 구현하는 클래스입니다. 즉, `Convert` 및 `ConvertBack`이라는 두 가지 메서드를 가지고 있습니다. `Convert` 메서드는 값이 원본(source)에서 대상(target)으로 전달될 때 호출됩니다. `ConvertBack` 메서드는 `OneWayToSource` 또는 `TwoWay` 바인딩 내에서 대상에서 원본으로 전송을 위해 호출됩니다.

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

바인딩이 원본에서 대상으로 오직 단 방향이므로, `ConvertBack` 메서드는 이 프로그램에서 역할을 하지 않습니다.

바인딩은 바인딩 변환기를 `Converter` 속성으로 참조합니다. 바인딩 변환기는 `ConverterParameter` 속성으로 지정된 매개 변수를 받아들일 수도 있습니다. 몇 가지 다양한 기능에 대한 승수를 지정하는 방법입니다. 바인딩 변환기는 변환 매개 변수에서 유효한 `double` 값을 확인합니다.

다음과 같이 변환기는 리소스 사전에서 인스턴스화되므로 여러 바인딩 간에 공유될 수 있습니다.

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

세 개의 데이터 바인딩이 단일 인스턴스를 참조합니다. 다음과 같이 `Binding` 태그 확장은 내부에 `StaticResource` 태그 확장을 포함합니다.

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

결과 다음과 같습니다.

[![](data-binding-basics-images/listview3.png "DataTemplate 및 변환기를 사용하여 컬렉션에 바인딩")](data-binding-basics-images/listview3-large.png#lightbox "DataTemplate 및 변환기를 사용하여 컬렉션에 바인딩")

`ListView`는 기본 데이터에서 동적으로 발생할 수 있는 변경 사항을 처리하는 데 상당히 정교하지만 특정 단계를 수행할 때만 가능합니다. `ListView`의 `ItemsSource` 속성에 할당된 항목의 컬렉션이 런타임 중에 변경되면(즉, 항목이 컬렉션에서 추가되거나 제거될 수 있는 경우), 해당 항목에 대해 `ObservableCollection` 클래스를 사용하십시오. `ObservableCollection`은 `INotifyCollectionChanged` 인터페이스를 구현하고, `ListView`는 `CollectionChanged` 이벤트를 위한 처리기를 설치합니다.

항목 자체의 속성이 런타임 중에 변경되면, 컬렉션의 항목은 `INotifyPropertyChanged` 인터페이스를 구현하고, `PropertyChanged` 이벤트를 사용하여 속성 값 변경 사항에 대해 신호합니다. 해당 내용은 이 시리즈의 다음 부분인 [5부. 데이터 바인딩부터 MVVM까지](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)에서 설명합니다.

## <a name="summary"></a>요약

데이터 바인딩은 페이지 내의 두 개체 간, 또는 시각적 개체와 기본 데이터 사이의 속성을 연결하는 강력한 메커니즘을 제공합니다. 그러나 응응 프로그램이 데이터 원본(sources)으로 작업하기 시작하면, 인기 있는 응용 프로그램 아키텍처 패턴이 유용한 패러다임으로 부상하기 시작합니다. 이에 대해서는 [5부. 데이터 바인딩부터 MVVM까지](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)에서 다룹니다.



## <a name="related-links"></a>관련 링크

- [Xaml 샘플](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [1부. XAML 시작(샘플)](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2부. 필수 XAML 구문 (샘플)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [3부. XAML 태그 확장 (샘플)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [5부. MVVM에 데이터 바인딩(샘플)](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
