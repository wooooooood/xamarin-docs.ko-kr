---
title: 4 부입니다. 데이터 바인딩 기본 사항
description: 데이터 바인딩 변경 하나에 다른 변경으로 인해 있도록 연결할 두 개체의 속성을 허용 합니다.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 342288C3-BB4C-4924-B178-72E112D777BA
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: e0ad67db0671996e594f9c5d48b329a5d676fc1d
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563435"
---
# <a name="part-4-data-binding-basics"></a>4 부입니다. 데이터 바인딩 기본 사항

_데이터 바인딩 변경 하나에 다른 변경으로 인해 있도록 연결할 두 개체의 속성을 허용 합니다. 이 매우 유용한 도구 및 XAML 바로 가기 및 편의 제공 코드에서 완전히 데이터 바인딩을 정의할 수 있지만. 따라서 Xamarin.Forms의 가장 중요 한 태그 확장 중 하나는 바인딩 됩니다._

## <a name="data-bindings"></a>데이터 바인딩

데이터 바인딩 이라는 두 개체, 속성 연결 합니다 *소스* 및 *대상*합니다. 코드에서 두 단계가 필요: 합니다 `BindingContext` 원본 개체를 대상 개체의 속성을 설정 해야 및 `SetBinding` 메서드 (종종 함께 사용 합니다 `Binding` 클래스)의 속성을 바인딩하려면 대상 개체에서 호출 해야 원본 개체의 속성에 대 한 개체입니다.

대상 속성에는 대상 개체에서 파생 되어야 하 의미 하는 바인딩 가능한 속성을 해야 `BindableObject`합니다. 온라인 Xamarin.Forms 설명서는 속성은 바인딩 가능한 속성을 나타냅니다. 속성 `Label` 와 같은 `Text` 바인딩할 수 있는 속성과 연결 된 `TextProperty`합니다.

태그에도 코드에 필요 합니다 동일한 두 단계를 수행 해야 점을 제외 하 고는 `Binding` 태그 확장의 수행 합니다 `SetBinding` 호출 및 `Binding` 클래스입니다.

그러나 XAML에서 데이터 바인딩을 정의한 경우 여러 가지 설정 하는 `BindingContext` 대상 개체의 합니다. 경우에 따라 사용 하 여 코드 숨김 파일에서 설정 되어 경우에 따라를 `StaticResource` 또는 `x:Static` 태그 확장의 콘텐츠로 때로는 `BindingContext` 속성 요소 태그입니다.

바인딩 가장 자주 사용 되는 MVVM (Model View ViewModel) 응용 프로그램 아키텍처의 인식에서 일반적으로 기본 데이터 모델을 사용 하 여 프로그램의 시각적 개체를 연결할에 설명 된 대로 [5 부입니다. 데이터 바인딩부터 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), 하지만 다른 시나리오가 가능 합니다.

## <a name="view-to-view-bindings"></a>뷰를 바인딩

같은 페이지에 두 가지 보기의 속성을 연결할 데이터 바인딩을 정의할 수 있습니다. 설정 하는 경우에 `BindingContext` 사용 하 여 대상 개체는 `x:Reference` 태그 확장 합니다.

포함 된 XAML 파일을 다음과 같습니다를 `Slider` 두 개의 `Label` 뷰 중 하나는 방향으로 회전 합니다 `Slider` 값을 표시 하는 `Slider` 값:

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

`Slider` 포함를 `x:Name` 두 참조 되는 특성 `Label` 를 사용 하 여 뷰를 `x:Reference` 태그 확장 합니다.

합니다 `x:Reference` 라는 속성을 정의 바인딩 확장 `Name` 이 경우 참조 된 요소의 이름으로 설정 하려면 `slider`합니다. 그러나 합니다 `ReferenceExtension` 정의 하는 클래스를 `x:Reference` 태그 확장도 정의 합니다.를 `ContentProperty` 특성에 대 한 `Name`, 즉, 명시적으로 필요 하지는. 다양성에 대 한 첫 번째 `x:Reference` 포함 "이름 =" 하지만 두 번째 그렇지 않습니다.

```csharp
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

합니다 `Binding` 자체 태그 확장 처럼 몇 가지 속성을 가질 수 있습니다 합니다 `BindingBase` 및 `Binding` 클래스입니다. `ContentProperty` 에 대 한 `Binding` 됩니다 `Path`, 하지만 "경로 =" 경로에서 첫 번째 항목인 경우 태그 확장의 부분을 생략할 수 있습니다는 `Binding` 태그 확장 합니다. 첫 번째 예제는 "경로 =" 생략 하는 두 번째 예제:

```csharp
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

한 줄에 있을 수 또는 여러 줄으로 구분 된 속성:

```csharp
Text="{Binding Value,
               StringFormat='The angle is {0:F0} degrees'}"
```

무엇이 든 편리 하 게 수행 합니다.

표시 된 `StringFormat` 두 번째에서 속성 `Binding` 태그 확장 합니다. Xamarin.forms에 바인딩 모든 암시적 형식 변환을 수행 하지 않습니다 및 형식 변환기를 제공 하거나 사용 해야는 문자열이 아닌 개체를 문자열로 표시 하는 경우 `StringFormat`합니다. 내부적으로 정적 `String.Format` 메서드를 구현 하는 `StringFormat`합니다. 잠재적으로 문제가 있는 경우, 중괄호도 태그 확장 구분 하는 데 사용 되는.NET의 서식 사양을 사용 되므로 이렇게 하면 XAML 파서를 혼동 하는 위험이 만들어집니다. 이 문제를 방지 하려면 작은따옴표로 전체 서식 문자열을 넣습니다.

```csharp
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

실행 중인 프로그램은 다음과 같습니다.

[![](data-binding-basics-images/sliderbinding.png "뷰를 바인딩")](data-binding-basics-images/sliderbinding-large.png#lightbox "뷰를 바인딩 ")

## <a name="the-binding-mode"></a>바인딩 모드

단일 뷰를 일부의 속성에 데이터 바인딩을 가질 수 있습니다. 그러나 각 뷰 하나만 있을 수 있습니다 `BindingContext`이므로 해당 보기에서 여러 데이터 바인딩 해야 모든 동일한 개체의 속성을 참조 합니다.

이 및 다른 문제에 솔루션에서는 합니다 `Mode` 의 멤버에 설정 된 속성을 `BindingMode` 열거형:

- `Default`
- `OneWay` -값 소스에서 대상으로 전송 됩니다
- `OneWayToSource` -소스 대상에서 전송할지 값
- `TwoWay` -원본과 대상 간의 값 양방향 전송

다음 프로그램에서는 일반적으로 사용 합니다 `OneWayToSource` 및 `TwoWay` 바인딩 모드입니다. 네 `Slider` 뷰 컨트롤에는 `Scale`, `Rotate`, `RotateX`, 및 `RotateY` 의 속성을 `Label`입니다. 어렵지 처음에 처럼의 이러한 네 가지 속성을 `Label` 에 의해 설정 되는 각 때문에 데이터 바인딩 대상이 되어야를 `Slider`. 그러나 합니다 `BindingContext` 의 `Label` 하나만 개체 수 있으며 다른 슬라이더를 네 가지입니다.

따라서 모든 바인딩을 설정 보이는 이전 버전과 방법으로:는 `BindingContext` 로 설정 되어 각 네 개의 슬라이더를 `Label`에 바인딩을 설정 하 고는 `Value` 슬라이더의 속성입니다. 사용 하 여는 `OneWayToSource` 및 `TwoWay` 모드 이러한 `Value` 속성에는 원본 속성을 설정할 수는 `Scale`, `Rotate`, `RotateX`, 및 `RotateY` 의 속성을 `Label`:

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

중 3 개에 대 한 바인딩을 `Slider` 뷰는 `OneWayToSource`즉는 `Slider` 값의 속성에서을 변경 하면 해당 `BindingContext`, 되는 `Label` 라는 `label`합니다. 이 세 가지 `Slider` 뷰는 변경 사항이 발생할를 `Rotate`, `RotateX`, 및 `RotateY` 의 속성을 `Label`입니다.

그러나에 대 한 바인딩을 합니다 `Scale` 속성은 `TwoWay`합니다. 때문에 이것이 합니다 `Scale` 속성이 기본값은 1이 고 사용 하 여는 `TwoWay` 원인 바인딩는 `Slider` 초기 값을 0이 아닌 1에서 설정할 수. 해당 바인딩 되었으면 `OneWayToSource`서 `Scale` 속성은 처음에에서 0으로 설정 됩니다는 `Slider` 기본값. `Label` 표시 되지 않습니다 및 사용자에 게 약간의 혼동이 발생할 수 있습니다.

 [![](data-binding-basics-images/slidertransforms.png "이전 버전과 바인딩을")](data-binding-basics-images/slidertransforms-large.png#lightbox "이전 버전과 바인딩")

 > [!NOTE]
 > 합니다 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 클래스에 [ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX) 및 [ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY) 크기를 조정 하는 속성을 `VisualElement` x 축 및 y 축 각각.

## <a name="bindings-and-collections"></a>바인딩 및 컬렉션

XAML 및 데이터 바인딩 템플릿 기반 보다 더 나은 성능을 설명 nothing `ListView`합니다.

`ListView` 정의 `ItemsSource` 형식의 속성 `IEnumerable`, 해당 컬렉션의 항목을 표시 합니다. 이러한 항목에는 모든 형식의 개체 수 있습니다. 기본적으로 `ListView` 사용을 `ToString` 메서드는 항목을 표시 하려면 각 항목의 합니다. 경우에 따라 이것이 바로 원하는 하지만 대부분의 경우에서 `ToString` 개체의 정규화 된 클래스 이름만 반환 합니다.

그러나의 항목을 `ListView` 컬렉션에 사용 하 여 원하는 방식으로 표시할 수 있습니다를 *템플릿*에서 파생 된 클래스를 포함 하는 `Cell`합니다. 서식 파일에 있는 모든 항목에 대 한 복제 되는 `ListView`, 고 서식 파일에서 설정 된 데이터 바인딩을 개별 복제본으로 전송 됩니다.

사용 하 여 이러한 항목에 대 한 사용자 지정 셀을 만들어서 하려는 매우 빈번 하 고는 `ViewCell` 클래스입니다. 이 프로세스는 코드에서 다소 복잡 하지만 XAML에서 매우 간단 하 게 됩니다.

XamlSamples에 포함 된 프로젝트는 라는 클래스 `NamedColor`합니다. 각 `NamedColor` 개체에 `Name` 하 고 `FriendlyName` 형식의 속성 `string`, 및 `Color` 형식의 속성 `Color`합니다. 또한 `NamedColor` 형식의 141 정적 읽기 전용 필드에 `Color` 에 해당 하는 Xamarin.Forms에 정의 된 색 `Color` 클래스입니다. 정적 생성자를 만듭니다는 `IEnumerable<NamedColor>` 포함 된 컬렉션 `NamedColor` 이러한 정적 필드에 해당 하는 개체 및 해당 공용 정적 할당 `All` 속성입니다.

정적 설정 `NamedColor.All` 속성을 합니다 `ItemsSource` 의 `ListView` 쉽습니다를 사용 하는 `x:Static` 태그 확장:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

결과 표시 설정 항목 유형의 진정한은 `XamlSamples.NamedColor`:

[![](data-binding-basics-images/listview1.png "컬렉션에 바인딩")](data-binding-basics-images/listview1-large.png#lightbox "컬렉션에 바인딩")

많은 정보 없는 하지만 `ListView` 스크롤 및 선택할 수 있는입니다.

항목에 대 한 서식 파일을 정의 하려면 중단 하려는 `ItemTemplate` 속성 요소로 속성으로 설정 하 고는 `DataTemplate`, 다음 참조 하는 `ViewCell`합니다. 에 `View` 의 속성을 `ViewCell` 각 항목을 표시 하는 하나 이상의 보기 레이아웃을 정의할 수 있습니다. 간단한 예는 다음과 같습니다.

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

`Label` 로 설정 된를 `View` 의 속성을 `ViewCell`입니다. (합니다 `ViewCell.View` 태그 때문에 필요 하지 않은 합니다 `View` 속성의 콘텐츠 속성은 `ViewCell`.) 이 태그를 표시 합니다 `FriendlyName` 의 각 속성 `NamedColor` 개체:

[![](data-binding-basics-images/listview2.png "DataTemplate 사용 하 여 컬렉션에 바인딩")](data-binding-basics-images/listview2-large.png#lightbox "DataTemplate 사용 하 여 컬렉션에 바인딩")

훨씬 나은. 이제 필요한 모든 내용이 자세한 내용 및 실제 색을 사용 하 여 항목 템플릿을 무척 반가 웠 하는 것입니다. 이 서식 파일을 지원 하려면 일부 값과 개체에에서 정의 된 페이지의 리소스 사전:

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

사용 하 여 `OnPlatform` 의 크기를 정의 하는 `BoxView` 와의 높이 `ListView` 행. 세 플랫폼 모두에 대 한 값이 동일 하지만 태그를 쉽게 디스플레이 미세 조정 하기 위해 다른 값에 적용할 수 있습니다.

## <a name="binding-value-converters"></a>바인딩 값 변환기

이전 **ListView 데모** XAML 파일 표시 개별 `R`를 `G`, 및 `B` Xamarin.Forms의 속성 `Color` 구조입니다. 이러한 속성은 형식의 `double` 및 0에서 1 사이입니다. 16 진수 값을 표시 하려는 경우 사용할 수 없습니다 단순히 `StringFormat` 사양 서식 지정을 "X2"를 사용 하 여 합니다. 외에도 정수에만 작동 합니다 `double` 값 255를 곱한 수 해야 합니다.

사용 하 여 이러한 문제 해결 되었습니다를 *값 변환기*라고도 하는 *바인딩 변환기*합니다. 이 클래스는 구현 하는 클래스를 `IValueConverter` 인터페이스를 통해 라는 두 개의 메서드를가지고 `Convert` 및 `ConvertBack`합니다. `Convert` 메서드는 원본에서 대상; 값이 전송 합니다 `ConvertBack` 메서드는 전송에 대 한 대상에서 원본에 `OneWayToSource` 또는 `TwoWay` 바인딩:

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

`ConvertBack` 메서드 재생 되지 않는 역할을이 프로그램에 바인딩을 있기 때문에 한 가지 방법은 원본에서 대상으로 합니다.

바인딩 바인딩 변환기를 사용 하 여 참조를 `Converter` 속성입니다. 바인딩 변환기를 사용 하 여 지정 된 매개 변수도 받아들일 수 있습니다는 `ConverterParameter` 속성입니다. 몇 가지 다양 한 기능에 대 한 승수를 지정 하는 방법을 하는 것이 이것이입니다. 올바른 변환기 매개 변수를 확인 하는 바인딩 변환기 `double` 값입니다.

여러 바인딩 간에 공유할 수 있도록 리소스 사전에 변환기를 인스턴스화합니다.

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

이 단일 인스턴스를 참조 하는 세 가지 데이터 바인딩. 다음에 유의 합니다 `Binding` 태그 확장에 포함 된 포함 `StaticResource` 태그 확장:

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

결과 다음과 같습니다.

[![](data-binding-basics-images/listview3.png "DataTemplate 및 변환기를 사용 하 여 컬렉션에 바인딩")](data-binding-basics-images/listview3-large.png#lightbox "DataTemplate 및 변환기를 사용 하 여 컬렉션에 바인딩")

`ListView` 는 특정 단계를 수행 하는 경우에 기본 데이터를 동적으로 발생할 수 있는 변경 내용 처리에서 매우 복잡 합니다. 항목의 컬렉션에 할당 하는 경우를 `ItemsSource` 의 속성을 `ListView` 런타임 중에 변경-에 항목을 추가할 수 있습니다 하는 경우 또는 컬렉션에서 제거 하는-사용 하 여는 `ObservableCollection` 이러한 항목에 대 한 클래스. `ObservableCollection` 구현 합니다 `INotifyCollectionChanged` 인터페이스 및 `ListView` 에 대 한 처리기가 설치를 `CollectionChanged` 이벤트.

런타임에 항목 자체의 속성 변경 경우 컬렉션의 항목이 구현 해야 합니다 `INotifyPropertyChanged` 인터페이스 및 신호를 사용 하 여 속성 값 변경의 `PropertyChanged` 이벤트. 이 시리즈의 다음 부분에서는이 확인할 [5 부입니다. MVVM에 데이터 바인딩에서](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)합니다.

## <a name="summary"></a>요약

데이터 바인딩 속성 페이지 내에서 두 개체 간의 또는 시각적 개체 간의 연결 및 기본 데이터에 대 한 강력한 메커니즘을 제공 합니다. 하지만 데이터 소스를 사용 하 여 작업을 시작 하는 응용 프로그램, 인기 있는 응용 프로그램 아키텍처 패턴 유용한 패러다임으로 등장 하기 시작 합니다. 이에 대해서는 [5 부입니다. 데이터 바인딩부터 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)합니다.



## <a name="related-links"></a>관련 링크

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [1부. XAML (샘플)를 사용 하 여 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2부. 필수 XAML 구문 (샘플)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [3부. XAML 태그 확장 (샘플)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [5부. MVVM (샘플)에 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
