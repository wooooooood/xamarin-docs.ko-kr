---
제목: "4 부. 데이터 바인딩 기본 사항 "설명:" 데이터 바인딩을 사용 하면 두 개체의 속성을 연결 하 여 한 개체의 변경으로 인해 다른 개체가 변경 될 수 있습니다. "
ms. prod: xamarin ms. 기술: xamarin assetid: 342288C3-BB4C-4924-B178-72E112D777BA author: davidbritch ms. author: dabritch: 10/25/2017: no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="part-4-data-binding-basics"></a>4부. 데이터 바인딩 기본 사항

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_데이터 바인딩을 사용 하면 두 개체의 속성을 연결 하 여 한 개체의 변경으로 인해 다른 개체의 변경 내용을 적용할 수 있습니다. 이는 매우 중요 한 도구 이며, 데이터 바인딩을 코드로 완전히 정의할 수 있는 반면 XAML은 바로 가기 키와 편리 함을 제공 합니다. 따라서에서 가장 중요 한 태그 확장 중 하나는 Xamarin.Forms 바인딩입니다._

## <a name="data-bindings"></a>데이터 바인딩

데이터 바인딩은 *원본* 및 *대상*이라는 두 개체의 속성을 연결 합니다. 코드에서는 두 단계가 필요 합니다. `BindingContext` 대상 개체의 속성은 원본 개체로 설정 해야 하며, `SetBinding` 클래스와 함께 사용 되는 메서드는 `Binding` 대상 개체에서 호출 되어야 해당 개체의 속성을 소스 개체의 속성에 바인딩할 수 있습니다.

대상 속성은 바인딩 가능한 속성 이어야 합니다. 즉, 대상 개체는에서 파생 되어야 합니다 `BindableObject` . 온라인 Xamarin.Forms 설명서에서는 바인딩 가능한 속성인 속성을 표시 합니다. `Label`과 같은의 속성 `Text` 은 바인딩 가능한 속성과 연결 됩니다 `TextProperty` .

태그 `Binding` 확장은 `SetBinding` 호출 및 클래스를 대신 하는 것을 제외 하 고 코드에 필요한 것과 동일한 두 단계를 수행 해야 합니다 `Binding` .

그러나 XAML에서 데이터 바인딩을 정의 하는 경우 대상 개체의를 설정 하는 방법에는 여러 가지가 있습니다 `BindingContext` . 경우에 따라 코드 숨김이 파일에서 설정 `StaticResource` `x:Static` 됩니다. 또는 태그 확장을 사용 하는 경우가 있으며 때로는 `BindingContext` 속성-요소 태그의 콘텐츠로도 사용 됩니다.

바인딩은 일반적으로 5 부에 설명 된 대로 일반적으로 MVVM (모델-뷰-ViewModel) 응용 프로그램 아키텍처를 실현 하 여 프로그램의 시각적 개체를 기본 데이터 모델과 연결 하는 데 사용 됩니다 [. 데이터 바인딩에서 MVVM로](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), 다른 시나리오가 가능 합니다.

## <a name="view-to-view-bindings"></a>뷰 뷰 바인딩

동일한 페이지에서 두 뷰의 속성을 연결 하는 데이터 바인딩을 정의할 수 있습니다. 이 경우 `BindingContext` 태그 확장을 사용 하 여 대상 개체의을 설정 합니다 `x:Reference` .

다음은 두 개의 보기를 포함 하는 XAML 파일 `Slider` `Label` 입니다. 하나는 값으로 회전 하 `Slider` 고 다른 하나는 값을 표시 합니다 `Slider` .

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

에는 `Slider` `x:Name` `Label` 태그 확장을 사용 하 여 두 개의 뷰에서 참조 하는 특성이 포함 되어 있습니다 `x:Reference` .

`x:Reference`바인딩 확장은로 설정 된 속성을 정의 합니다 `Name` .이 경우에는 참조 되는 요소의 이름으로 설정 `slider` 됩니다. 그러나 `ReferenceExtension` 태그 확장을 정의 하는 클래스는 `x:Reference` `ContentProperty` 에 대 한 특성도 정의 합니다 `Name` . 즉, 명시적으로 필요 하지 않습니다. 무엇 보다도, 첫 번째는 `x:Reference` "Name ="를 포함 하지만 두 번째는이를 포함 하지 않습니다.

```xaml
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

`Binding`태그 확장 자체는 및 클래스와 마찬가지로 여러 속성을 가질 수 있습니다 `BindingBase` `Binding` . `ContentProperty`의는 `Binding` 이지만 `Path` , 경로가 태그 확장의 첫 번째 항목인 경우 태그 확장의 "Path =" 부분을 생략할 수 있습니다 `Binding` . 첫 번째 예제에는 "Path ="가 있지만 두 번째 예에서는이를 생략 합니다.

```xaml
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

속성은 모두 한 줄에 있거나 여러 줄로 구분 될 수 있습니다.

```xaml
Text="{Binding Value,
               StringFormat='The angle is {0:F0} degrees'}"
```

모든 작업을 편리 하 게 수행할 수 있습니다.

`StringFormat`두 번째 태그 확장의 속성을 확인 `Binding` 합니다. 에서 Xamarin.Forms 바인딩은 암시적 형식 변환을 수행 하지 않으며 문자열이 아닌 개체를 문자열로 표시 해야 하는 경우 형식 변환기를 제공 하거나를 사용 해야 합니다 `StringFormat` . 내부적으로 정적 메서드는을 `String.Format` 구현 하는 데 사용 됩니다 `StringFormat` . .NET 형식 지정 사양에는 태그 확장을 구분 하는 데에도 사용 되는 중괄호가 포함 되어 있기 때문에 문제가 될 수 있습니다. 이렇게 하면 XAML 파서가 혼동 될 위험이 있습니다. 이를 방지 하려면 전체 형식 지정 문자열을 작은따옴표로 묶어 입력 합니다.

```xaml
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

실행 중인 프로그램은 다음과 같습니다.

[![뷰 뷰 바인딩](data-binding-basics-images/sliderbinding.png)](data-binding-basics-images/sliderbinding-large.png#lightbox)

## <a name="the-binding-mode"></a>바인딩 모드

단일 뷰에는 여러 가지 속성에 대 한 데이터 바인딩이 있을 수 있습니다. 그러나 각 보기에는 하나만 있을 수 `BindingContext` 있으므로 해당 뷰의 여러 데이터 바인딩이 동일한 개체의 모든 참조 속성을 가져야 합니다.

이 문제와 기타 문제에 대 한 해결 방법은 `Mode` 열거형의 멤버로 설정 된 속성을 포함 합니다 `BindingMode` .

- `Default`
- `OneWay`-값이 원본에서 대상으로 전송 됩니다.
- `OneWayToSource`-값이 대상에서 원본으로 전송 됩니다.
- `TwoWay`-값이 소스와 대상 간에 두 가지 방식으로 전송 됩니다.
- `OneTime`-데이터가 원본에서 대상으로 이동 하지만 변경 되는 경우에만 `BindingContext`

다음 프로그램에서는 `OneWayToSource` 및 바인딩 모드를 일반적으로 사용 하는 방법을 보여 줍니다 `TwoWay` . 네 가지 `Slider` 뷰는 `Scale` 의,, 및 속성을 제어 하기 위한 것 `Rotate` `RotateX` `RotateY` `Label` 입니다. 처음에는에 의해 설정 되 고 있으므로의 이러한 4 가지 속성이 `Label` 데이터 바인딩 대상 이어야 하는 것 처럼 보입니다 `Slider` . 그러나의는 `BindingContext` `Label` 하나의 개체 일 수 있으며 네 가지 슬라이더가 있습니다.

이러한 이유로 모든 바인딩은 역방향으로 설정 됩니다. `BindingContext` 네 개의 슬라이더 각각의는로 설정 되 `Label` 고 바인딩은 `Value` 슬라이더의 속성에 설정 됩니다. `OneWayToSource`이러한 속성은 및 `TwoWay` 모드를 사용 하 여 `Value` `Scale` 의,, `Rotate` `RotateX` 및 `RotateY` 속성인 원본 속성을 설정할 수 있습니다 `Label` .

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

세 가지 보기의 바인딩은입니다 `Slider` . 즉, `OneWayToSource` `Slider` 값이로 명명 된의 속성을 변경 합니다 `BindingContext` `Label` `label` . 이 세 가지 `Slider` 뷰는 `Rotate` `RotateX` 의, 및 속성을 변경 합니다 `RotateY` `Label` .

그러나 속성의 바인딩은 `Scale` `TwoWay` 입니다. 이는 속성의 `Scale` 기본값은 1이 고, `TwoWay` 바인딩을 사용 하면 초기 값이 0이 아닌 1로 설정 되기 때문입니다 `Slider` . 해당 바인딩이 인 경우 `OneWayToSource` 속성은 `Scale` 처음에 기본값에서 0으로 설정 됩니다 `Slider` . 는 `Label` 표시 되지 않으며 사용자에 게 혼동을 일으킬 수 있습니다.

 [![역방향 바인딩](data-binding-basics-images/slidertransforms.png)](data-binding-basics-images/slidertransforms-large.png#lightbox)

 > [!NOTE]
 > [`VisualElement`](xref:Xamarin.Forms.VisualElement)또한 클래스에 [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) 는 `VisualElement` x 축과 y 축에서 각각의 크기를 조정 하는 및 속성이 있습니다.

## <a name="bindings-and-collections"></a>바인딩 및 컬렉션

XAML 및 데이터 바인딩의 강력한 기능을 템플릿 보다 더 잘 보여 주는 것은 아닙니다 `ListView` .

`ListView``ItemsSource`형식의 속성을 정의 하 `IEnumerable` 고 해당 컬렉션의 항목을 표시 합니다. 이러한 항목은 모든 형식의 개체 일 수 있습니다. 기본적으로에서는 `ListView` `ToString` 각 항목의 메서드를 사용 하 여 해당 항목을 표시 합니다. 경우에 따라 원하는 것만이 아니라 대부분의 경우 `ToString` 개체의 정규화 된 클래스 이름만 반환 합니다.

그러나 컬렉션의 항목은 `ListView` 에서 파생 되는 클래스를 포함 하는 *템플릿을*사용 하 여 원하는 방식으로 표시 될 수 있습니다 `Cell` . 템플릿은의 모든 항목에 대해 복제 되 `ListView` 고, 템플릿에 설정 된 데이터 바인딩은 개별 복제본으로 전송 됩니다.

일반적으로 클래스를 사용 하 여 이러한 항목에 대 한 사용자 지정 셀을 만들 수 있습니다 `ViewCell` . 이 프로세스는 코드에서 다소 복잡 하지만 XAML에서 매우 간단 합니다.

XamlSamples 프로젝트에는 라는 클래스가 포함 되어 `NamedColor` 있습니다. 각 `NamedColor` 개체에 `Name` 는 `FriendlyName` 형식의 및 속성과 `string` `Color` 형식의 속성이 `Color` 있습니다. 또한에는 `NamedColor` `Color` 클래스에 정의 된 색에 해당 하는 형식의 정적 읽기 전용 필드 141가 있습니다 Xamarin.Forms `Color` . 정적 생성자는 `IEnumerable<NamedColor>` 이러한 정적 필드에 해당 하는 개체를 포함 하는 컬렉션을 만들고이를 `NamedColor` 공용 정적 `All` 속성에 할당 합니다.

`NamedColor.All`태그 확장을 사용 하 여 정적 속성을의로 설정 하 `ItemsSource` `ListView` 는 것이 쉽습니다 `x:Static` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

결과 표시는 항목이 진정한 유형 임을 나타냅니다 `XamlSamples.NamedColor` .

[![컬렉션에 바인딩](data-binding-basics-images/listview1.png)](data-binding-basics-images/listview1-large.png#lightbox)

정보는 많지 않지만는 `ListView` 스크롤 가능 하 고 선택할 수 있습니다.

항목에 대 한 템플릿을 정의 하려면 속성을 속성 요소로 분할 하 고로 설정 하 여를 참조 하는 것 `ItemTemplate` 이 좋습니다 `DataTemplate` `ViewCell` . `View`의 속성을 통해 `ViewCell` 각 항목을 표시 하는 하나 이상의 뷰 레이아웃을 정의할 수 있습니다. 다음은 간단한 예제입니다.

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

> [!NOTE]
> 셀의 바인딩 소스와 셀의 자식은 `ListView.ItemsSource` 컬렉션입니다.

`Label`요소는의 속성으로 설정 됩니다 `View` `ViewCell` . ( `ViewCell.View` 속성이의 content 속성 이므로 태그가 필요 하지 않습니다 `View` `ViewCell` .) 이 태그는 `FriendlyName` 각 개체의 속성을 표시 합니다 `NamedColor` .

[![DataTemplate를 사용 하 여 컬렉션에 바인딩](data-binding-basics-images/listview2.png)](data-binding-basics-images/listview2-large.png#lightbox)

훨씬 나은. 이제 추가 정보 및 실제 색을 사용 하 여 항목 템플릿을 등록 하는 데 필요한 모든 작업을 수행 해야 합니다. 이 템플릿을 지원 하기 위해 일부 값과 개체는 페이지의 리소스 사전에 정의 되어 있습니다.

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

를 사용 하 여의 `OnPlatform` 크기 `BoxView` 와 행 높이를 정의 합니다 `ListView` . 모든 플랫폼의 값이 동일 하지만 표시를 미세 조정 하기 위해 다른 값에 대 한 태그를 쉽게 적용할 수 있습니다.

## <a name="binding-value-converters"></a>바인딩 값 변환기

이전 **ListView Demo** XAML 파일은 `R` 구조체의 개별, `G` 및 `B` 속성 Xamarin.Forms `Color` 을 표시 합니다. 이러한 속성의 형식은이 `double` 고 범위는 0에서 1 사이입니다. 16 진수 값을 표시 하려면 `StringFormat` "X2" 서식 지정 사양과 함께를 사용 하면 됩니다. 정수 및 이외의 경우에만 사용할 수 있는 `double` 값에는 255을 곱합니다.

이러한 사소한 문제는 *값 변환기*( *바인딩 변환기*라고도 함)로 해결 되었습니다. 인터페이스를 구현 하는 클래스로,이 클래스에는 `IValueConverter` 및 라는 두 개의 메서드가 `Convert` 있습니다 `ConvertBack` . `Convert`메서드는 값이 소스에서 대상으로 전송 될 때 호출 됩니다 .이 메서드는 `ConvertBack` 또는 바인딩에서 대상에서 소스로 전송 하기 위해 호출 됩니다 `OneWayToSource` `TwoWay` .

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

`ConvertBack`바인딩은 소스에서 대상으로의 단방향 이므로이 프로그램에서 역할을 재생 하지 않습니다.

바인딩은 속성을 사용 하 여 바인딩 변환기를 참조 `Converter` 합니다. 바인딩 변환기는 속성을 사용 하 여 지정 된 매개 변수도 사용할 수 있습니다 `ConverterParameter` . 일부 다양성의 경우 승수를 지정 하는 방법입니다. 바인딩 변환기는 변환기 매개 변수에서 유효한 값을 확인 합니다 `double` .

변환기는 여러 바인딩 간에 공유 될 수 있도록 리소스 사전에서 인스턴스화됩니다.

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

이 단일 인스턴스를 참조 하는 세 개의 데이터 바인딩입니다. `Binding`태그 확장에는 포함 된 태그 확장이 포함 되어 있습니다 `StaticResource` .

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

결과는 다음과 같습니다.

[![DataTemplate 및 변환기를 사용 하 여 컬렉션에 바인딩](data-binding-basics-images/listview3.png)](data-binding-basics-images/listview3-large.png#lightbox)

는 `ListView` 기본 데이터에서 동적으로 발생할 수 있는 변경 사항을 처리 하는 데 매우 정교 하지만 특정 단계를 수행 하는 경우에만 해당 됩니다. `ItemsSource` `ListView` 항목을 컬렉션에 추가 하거나 컬렉션에서 제거할 수 있는 경우의 속성에 할당 된 항목의 컬렉션이 런타임 중에 변경 되 면 `ObservableCollection` 이러한 항목에 대해 클래스를 사용 합니다. `ObservableCollection`인터페이스를 구현 `INotifyCollectionChanged` 하 고 `ListView` 이벤트에 대 한 처리기를 설치 `CollectionChanged` 합니다.

런타임 중에 항목 자체의 속성이 변경 되 면 컬렉션의 항목이 인터페이스를 구현 하 `INotifyPropertyChanged` 고 이벤트를 사용 하 여 속성 값의 신호를 변경 해야 합니다 `PropertyChanged` . 이는이 시리즈의 다음 부분인 5 부에서 설명 [합니다. 데이터 바인딩에서 MVVM로](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

## <a name="summary"></a>요약

데이터 바인딩은 페이지 내의 두 개체 간 또는 시각적 개체와 기본 데이터 간에 속성을 연결 하는 강력한 메커니즘을 제공 합니다. 그러나 응용 프로그램이 데이터 원본 작업을 시작 하면 인기 있는 응용 프로그램 아키텍처 패턴이 유용한 패러다임으로 나타나기 시작 합니다. 5 부에서 다룹니다 [. 데이터 바인딩에서 MVVM로](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

## <a name="related-links"></a>관련 링크

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [1 부. XAML 시작 (샘플)](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [2 부. 필수 XAML 구문 (샘플)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [3 부. XAML 태그 확장 (샘플)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [5 부. 데이터 바인딩에서 MVVM (샘플)로](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
