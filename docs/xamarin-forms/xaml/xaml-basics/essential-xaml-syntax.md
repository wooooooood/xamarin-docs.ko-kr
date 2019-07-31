---
title: 2부. 필수 XAML 구문
description: 이 문서에서는 속성 요소 및 연결 속성의 필수 XAML 구문 기능을 설명합니다.
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: d8052e00809b15f0858583ee2919c47cfd8af00b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646567"
---
# <a name="part-2-essential-xaml-syntax"></a>2부. 필수 XAML 구문

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML은 주로 인스턴스화 및 초기화 개체에 대 한 설계 되었습니다. 하지만 종종 속성을 XML 문자열로 쉽게 표현할 수 없는 복잡한 개체 속성으로 설정해야 하고 하나의 클래스에 의해 정의된 속성을 자식 클래스에 설정해야 하는 경우가 있습니다. 따라서 속성 요소 및 연결 속성이라는 필수 XAML 구문 기능이 필요합니다._

## <a name="property-elements"></a>속성 요소

XAML에서 클래스의 속성은 일반적으로 다음과 같이 XML 특성으로 설정됩니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

그러나 XAML에서 속성을 설정하는 대체 방법이 있습니다. `TextColor`를 사용하여 해당 대안을 시도해 보려면 먼저 다음과 같이 기존 `TextColor` 설정을 삭제하십시오.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

다음과 같이 시작 및 끝 태그로 분리하여 `Label` 태그 빈 요소를 엽니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

이 태그 안에 다음과 같이 점으로 구분된 클래스 이름과 속성 이름으로 구성된 시작 및 끝 태그를 추가합니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

다음과 같이 해당 새 태그의 내용으로 속성 값을 설정합니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

`TextColor` 속성을 지정하는 해당 두 방법은 기능적으로 동일하지만, 속성을 실제로 두 번 설정하게 되고 모호할 수 있으므로 동일한 속성에 두 가지 방법을 사용하지 마십시오.

해당 새 구문으로 다음과 같은 몇 가지 편리한 용어를 사용할 수 있습니다.

-  `Label`은 *개체 요소*입니다. XML 요소로 표현된 Xamarin.Forms 개체입니다.
-  `Text`, `VerticalOptions`, `FontAttributes` 및 `FontSize`는 *속성 특성* 입니다. 이들은 XML 특성으로 표현된 Xamarin.Forms 속성입니다.
-  해당 마지막 조각에서 `TextColor`는 *속성 요소*가 되었습니다. 이는 Xamarin.Forms 속성이지만 이제 XML 요소입니다.


속성 요소의 정의는 처음에는 XML 구문을 위반하는 것처럼 보일 수 있지만 그렇지 않습니다. 마침표는 XML에서 특별한 의미가 없습니다. XML 디코더에서 `Label.TextColor`는 단순히 일반적인 자식 요소입니다.

하지만 XAML에서는 해당 구문이 매우 특별합니다. 속성 요소에 대한 규칙 중 하나는 `Label.TextColor` 태그에 다른 것을 표시할 수 없다는 것입니다. 속성의 값은 항상 속성 요소 시작 태그와 종료 태그 사이의 내용으로 정의됩니다.

다음과 같이 둘 이상의 속성에서 속성 요소 구문을 사용할 수 있습니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center">
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

또는 다음과 같이 모든 속성에 대한 속성 요소 구문을 사용할 수 있습니다.

```xaml
<Label>
    <Label.Text>
        Hello, XAML!
    </Label.Text>
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
    <Label.VerticalOptions>
        Center
    </Label.VerticalOptions>
</Label>
```

처음에 속성 요소 구문이 비교적 꽤 단순한 것에 대한 불필요하게 장황한 대체 수단으로 여겨질 수 있고, 해당 예제에서는 확실히 그렇습니다.

그러나 속성의 값이 너무 복잡해서 간단한 문자열로 표현할 수 없으면 속성 요소 구문이 필수적입니다. 속성 요소 태그 내에서 다른 개체를 인스턴스화하고 해당 속성을 설정할 수 있습니다. 예를 들어 다음과 같이 속성 설정을 사용하여 `VerticalOptions`와 같은 속성을 `LayoutOptions` 값으로 명시적으로 설정할 수 있습니다.

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

또 다른 예는 다음과 같습니다. 에 `Grid` 는 `RowDefinitions` 및`ColumnDefinitions`라는 두 개의 속성이 있습니다. 해당 두 속성은 `RowDefinitionCollection`및 `ColumnDefinitionCollection` 형식이며, 이들은 `RowDefinition` 및 `ColumnDefinition` 개체의 컬렉션입니다. 이러한 컬렉션을 설정하려면 속성 요소 구문을 사용해야 합니다.

다음은 `GridDemoPage` 클래스에 대한 XAML 파일의 시작 부분으로, `RowDefinitions` 및 `ColumnDefinitions` 컬렉션에 대한 속성 요소 태그를 보여줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

자동 크기 조정 셀, 픽셀 너비 및 높이 셀 및 별 설정을 정의하기 위한 약식 구문을 확인하십시오.

## <a name="attached-properties"></a>연결 속성

방금 살펴 보았듯이 행과 열을 정의하기 위해 `Grid`는 `RowDefinitions` 및 `ColumnDefinitions` 컬렉션에 대한 속성 요소가 필요합니다. 그러나 프로그래머가 `Grid`의 각 자식이 위치하는 행과 열을 나타낼 방법이 있어야 합니다.

다음 특성을 사용하여 `Grid`의 각 자식에 대한 태그 내에서의 해당 자식의 행과 열을 지정합니다.

-  `Grid.Row`
-  `Grid.Column`

해당 특성의 기본값은 0입니다. 다음의 특성을 사용하여 해당 속성을 가진 자식이 둘 이상의 행 또는 열에 걸쳐 있는지 여부를 나타낼 수도 있습니다.

-  `Grid.RowSpan`
-  `Grid.ColumnSpan`

해당 두 특성의 기본값은 1입니다.

다음은 전체 GridDemoPage.xaml 파일입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>

        <Label Text="Autosized cell"
               Grid.Row="0" Grid.Column="0"
               TextColor="White"
               BackgroundColor="Blue" />

        <BoxView Color="Silver"
                 HeightRequest="0"
                 Grid.Row="0" Grid.Column="1" />

        <BoxView Color="Teal"
                 Grid.Row="1" Grid.Column="0" />

        <Label Text="Leftover space"
               Grid.Row="1" Grid.Column="1"
               TextColor="Purple"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two rows (or more if you want)"
               Grid.Row="0" Grid.Column="2" Grid.RowSpan="2"
               TextColor="Yellow"
               BackgroundColor="Blue"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two columns"
               Grid.Row="2" Grid.Column="0" Grid.ColumnSpan="2"
               TextColor="Blue"
               BackgroundColor="Yellow"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Fixed 100x100"
               Grid.Row="2" Grid.Column="2"
               TextColor="Aqua"
               BackgroundColor="Red"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

    </Grid>
</ContentPage>
```

`Grid.Row`와 `Grid.Column`을 0으로 설정하는 것은 필요하지 않지만, 명확히 하기 위해 일반적으로 포함합니다.

해당 모양은 다음과 같습니다.

[![](essential-xaml-syntax-images/griddemo.png "모눈 레이아웃")](essential-xaml-syntax-images/griddemo-large.png#lightbox "모눈 레이아웃")

구문으로만 미루어 보면, 해당 `Grid.Row`, `Grid.Column`, `Grid.RowSpan` 및 `Grid.ColumnSpan` 속성은 정적(static) 필드 또는 `Grid`의 특성으로 보이지만, 흥미롭게도 `Grid`는 `Row`, `Column`, `RowSpan` 또는 `ColumnSpan` 이름을 가진 어떤 것도 정의하지 않습니다.

대신 `Grid`는 `RowProperty`, `ColumnProperty`, `RowSpanProperty` 및 `ColumnSpanProperty`라는 4개의 바인딩 가능한 속성을 정의합니다. 해당 속성은 *연결 속성*이라는 특별한 유형의 바인딩 가능한 속성입니다. 이는 `Grid` 클래스에 의해 정의되지만 `Grid`의 자식에 설정됩니다.

코드에서 해당 연결 속성을 사용하려는 경우, `Grid` 클래스는 `SetRow`, `GetColumn`등의 정적(static) 메서드를 제공합니다. 그러나 XAML에서 해당 연결 속성은 간단한 속성 이름을 사용하여 `Grid`의 자식에서 특성으로 설정됩니다.

연결 속성은 항상 XAML 파일에서 클래스와 속성 이름을 마침표로 구분하여 포함하는 특성으로 인식할 수 있습니다. 하나의 클래스(이 예제의 경우 `Grid`)에서 정의되었지만, 다른 개체(여기서는 `Grid`의 자식)에 연결되었기 때문에 *연결 속성*이라고 합니다. 레이아웃하는 동안, `Grid`는 각 자식을 배치할 위치를 알기 위해 해당 연결 속성의 값을 조사할 수 있습니다.

`AbsoluteLayout` 클래스는 `LayoutBounds`와 `LayoutFlags`라는 두 개의 연결 속성을 정의합니다. 다음은 `AbsoluteLayout`의 비례 위치 지정 및 크기 조정 기능을 사용하여 구현된 체스판 패턴입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.AbsoluteDemoPage"
             Title="Absolute Demo Page">

    <AbsoluteLayout BackgroundColor="#FF8080">
        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

  </AbsoluteLayout>
</ContentPage>
```

화면은 다음과 같습니다.

[![](essential-xaml-syntax-images/absolutedemo-large.png "절대 레이아웃")](essential-xaml-syntax-images/absolutedemo-large.png#lightbox "절대 레이아웃")

이런 경우 XAML 사용의 지혜로움에 의문을 제기할 수 있습니다. `LayoutBounds` 사각형의 반복 및 규칙성으로 보아 코드에서 구현되는 편이 낫지 않았나 생각할 수 있습니다.

이는 합당한 우려이며, 사용자 인터페이스를 정의하는 경우 코드와 태그의 균형을 잘 맞추면 문제가 없습니다. XAML에서 일부 시각적 개체를 정의하고, 그런 다음 코드 숨김 파일의 생성자를 사용하여 루프에서 더 잘 생성할 수 있는 몇 가지 시각적 개체를 추가하는 것이 쉽습니다.

## <a name="content-properties"></a>콘텐츠 속성

이전 예제에서 `StackLayout`, `Grid` 및 `AbsoluteLayout` 개체는 `ContentPage`의 `Content` 속성으로 설정되었고, 해당 레이아웃의 자식들은 실제로 `Children` 컬렉션의 항목들입니다. 아직 해당 `Content` 및 `Children` 속성은 XAML 파일에 없습니다.

다음의 **XamlPlusCode** 예제와 같이 `Content` 및 `Children` 속성을 속성 요소로 포함할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout.Children>
                <Slider VerticalOptions="CenterAndExpand"
                        ValueChanged="OnSliderValueChanged" />

                <Label x:Name="valueLabel"
                       Text="A simple Label"
                       FontSize="Large"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand" />

                <Button Text="Click Me!"
                      HorizontalOptions="Center"
                      VerticalOptions="CenterAndExpand"
                      Clicked="OnButtonClicked" />
            </StackLayout.Children>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

실제 질문은 다음과 같습니다. 이러한 속성 요소가 XAML 파일에 필요 *하지* 않은 이유는 무엇 인가요?

XAML에서 사용하기 위해 Xamarin.Forms에 정의된 요소는 클래스의 `ContentProperty` 특성에 플래그가 지정된 하나의 속성을 가질 수 있습니다. 온라인 Xamarin.Forms 문서에서 `ContentPage` 클래스를 검색하면 해당 특성을 볼 수 있습니다.

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

즉, `Content` 속성 요소 태그가 필요하지 않다는 의미입니다. `ContentPage` 태그의 시작과 끝 사이에 나타나는 모든 XML 내용은 `Content` 속성에 할당되는 것으로 간주됩니다.

 `StackLayout`, `Grid`, `AbsoluteLayout` 및 `RelativeLayout`은 모두 `Layout<View>`에서 파생되었으며, Xamarin.Forms 문서에서 `Layout<T>`를 검색하면 다음과 같이 또 다른 `ContentProperty` 특성을 찾을 수 있습니다.

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

이렇게 하면 레이아웃의 컨텐츠를 명시적으로 `Children` 속성 요소 태그 없이 `Children` 컬렉션에 자동으로 추가할 수 있습니다.

다른 클래스들은 `ContentProperty` 특성 정의가 있습니다. 예를 들어, `Label`의 콘텐츠 속성은 `Text` 입니다. 다른 것들은 API 설명서를 확인하십시오.

## <a name="platform-differences-with-onplatform"></a>OnPlatform 사용 시의 플랫폼별 차이

단일 페이지 응용 프로그램에서는 iOS 상태 표시줄을 덮어쓰지 않도록 페이지에서 `Padding` 속성을 설정하는 것이 일반적입니다. 이를 위해 다음과 같이 코드에서 `Device.RuntimePlatform` 속성을 사용할 수 있습니다.

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) [및`On`](xref:Xamarin.Forms.On) 클래스를 사용 하 여 XAML에서 유사한 작업을 수행할 수도 있습니다. 먼저 페이지의 상단 근처의 `Padding` 속성을 위해 다음과 같이 속성 요소를 추가합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

해당 태그 안에 `OnPlatform` 태그를 포함하십시오. `OnPlatform`은 제네릭 클래스입니다. 제네릭(generic) 형식 인수를 지정해야 하고, 이 경우에는 `Padding` 속성 형식인 `Thickness`입니다. 다행스럽게도 `x:TypeArguments`라는 제네릭 인수를 정의하는 XAML 특성이 있습니다. 속성 유형을 매칭하는 설정은 다음과 같습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">

        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`OnPlatform`에는 `On` 개체의 `IList`인 `Platforms`라는 속성이 있습니다. 해당 속성에 대한 속성 요소 태그는 다음과 같습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>

            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

이제 `On` 요소를 추가합니다. 각각에 대해 다음과 같이 `Platform` 속성 및 `Value` 속성을 설정하여 `Thickness` 속성을 다음과 같이 설정합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>
                <On Platform="iOS" Value="0, 20, 0, 0" />
                <On Platform="Android" Value="0, 0, 0, 0" />
                <On Platform="UWP" Value="0, 0, 0, 0" />
            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

해당 태그는 단순화할 수 있습니다. `OnPlatform`의 콘텐츠 속성은 `Platforms`이므로 해당 속성 요소 태그는 다음과 같이 제거할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android" Value="0, 0, 0, 0" />
            <On Platform="UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`On`의 `Platform` 속성은 `IList<string>` 유형이므로 다음과 같이 값이 동일한 경우 여러 플랫폼을 포함할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android, UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

Android 및 UWP의 `Padding`은 기본값으로 설정되어 있으므로, 다음과 같이 해당 태그는 제거할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

이는 XAML에서 플랫폼 종속적인 `Padding` 속성을 설정하는 표준 방법입니다. `Value` 설정을 단일 문자열로 표현할 수 없는 경우, 해당 속성 요소를 다음과 같이 정의할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS">
                <On.Value>
                    0, 20, 0, 0
                </On.Value>
            </On>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

> [!NOTE]
> `OnPlatform` 태그 확장을 XAML에서 사용 하 여 플랫폼 별로 UI 모양을 사용자 지정할 수도 있습니다. 이 클래스는 `OnPlatform` 및 `On` 클래스와 같은 기능을 제공 하지만 좀 더 간결 하 게 표현 합니다. 자세한 내용은 [OnPlatform 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform)합니다.

## <a name="summary"></a>요약

속성 요소 및 연결 속성을 사용하여 상당 부분의 기본 XAML 구문을 설정하였습니다. 그러나 때로는 리소스 사전과 같은 간접 방식으로 개체 속성을 설정해야 합니다. 해당 접근 방식은 다음 부분인 [3부.  XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)에서 다룹니다.

## <a name="related-links"></a>관련 링크

- [Xaml 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [1부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [3부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [4부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [5장. MVVM 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
