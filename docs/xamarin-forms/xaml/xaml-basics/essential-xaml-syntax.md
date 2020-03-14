---
title: 2부. 필수 XAML 구문
description: 이 문서에서는 속성 요소의 필수 XAML 구문 기능 설명 및 연결 된 속성입니다.
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: f79a07a04eddeea1441f7938fdef210a37fb920a
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306574"
---
# <a name="part-2-essential-xaml-syntax"></a>2부. 필수 XAML 구문

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML은 일반적으로 개체를 인스턴스화하고 초기화 하기 위해 디자인 되었습니다. 그러나 속성은 XML 문자열로 표현할 수 없는 복합 개체로 설정 해야 하며, 경우에 따라 한 클래스에서 정의 되는 속성을 자식 클래스에서 설정 해야 합니다. 이러한 두 가지 요구 사항에는 속성 요소 및 연결 된 속성의 필수 XAML 구문 기능이 필요 합니다._

## <a name="property-elements"></a>속성 요소

XAML을에서 클래스의 속성은 일반적으로 XML 특성으로 설정 됩니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

그러나 XAML에서 속성을 설정 하는 대체 방법이 있습니다. `TextColor`를 사용 하 여이 대안을 시도 하려면 먼저 기존 `TextColor` 설정을 삭제 합니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

빈 요소 `Label` 태그를 시작 및 끝 태그로 구분 하 여 엽니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

이 태그 내 클래스 이름과 마침표로 구분 된 속성 이름으로 구성 된 시작 및 끝 태그를 추가 합니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

이러한 새 태그를 다음과 같은 내용으로 속성 값을 설정 합니다.

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

`TextColor` 속성을 지정 하는 두 가지 방법은 기능적으로 동일 하지만 속성을 두 번 설정 하는 것이 모호 해질 수 있으므로 동일한 속성에 두 가지 방법을 사용 하지 마십시오.

이 새 구문을 사용 하 여 몇 가지 유용한 용어를 도입할 수 있습니다.

- `Label`은 *개체 요소*입니다. XML 요소로 표현 된 Xamarin.Forms 개체입니다.
- `Text`, `VerticalOptions`, `FontAttributes` 및 `FontSize`은 *속성 특성*입니다. Xamarin.Forms 속성 XML 특성으로 표현 됩니다.
- 이 마지막 코드 조각에서는 `TextColor`가 *속성 요소가*되었습니다. 이 Xamarin.Forms 속성 이지만 이제 XML 요소입니다.

속성에서 요소 수의 정의 먼저 XML 구문의 위반 같지만 아닙니다. 마침표는 XML에서 특별 한 의미가 없습니다. XML 디코더에 `Label.TextColor`은 단순히 일반 자식 요소입니다.

하지만 XAML을이 구문이 매우 특별 한 합니다. 속성 요소에 대 한 규칙 중 하나는 `Label.TextColor` 태그에 다른 항목이 표시 되지 않는 것입니다. 속성의 값 속성 요소 시작 태그와 끝 태그 사이 콘텐츠로 항상 정의 됩니다.

둘 이상의 속성에 속성 요소 구문을 사용할 수 있습니다.

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

또는 모든 속성에 대 한 속성 요소 구문을 사용할 수 있습니다.

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

처음에 속성 요소 구문을 여겨질 수는 불필요 한 너무 길었다 면 죄송 대체 무엇 인가 비교적 간단 하 있으며 이러한 예제에서이 분명 합니다.

그러나 속성의 값이 너무 복잡해 서 간단한 문자열로 표현 될 때 속성 요소 구문을 필수적입니다. 속성 요소 태그 내의 다른 개체 인스턴스화하고 해당 속성을 설정 합니다. 예를 들어 속성 설정을 사용 하 여 `VerticalOptions`와 같은 속성을 `LayoutOptions` 값으로 명시적으로 설정할 수 있습니다.

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

또 다른 예: `Grid`에는 `RowDefinitions` 및 `ColumnDefinitions`라는 두 개의 속성이 있습니다. 이러한 두 속성은 `RowDefinition` 및 `ColumnDefinition` 개체의 컬렉션인 `RowDefinitionCollection` 및 `ColumnDefinitionCollection`형식입니다. 이러한 컬렉션을 설정 하려면 속성 요소 구문을 사용 해야 합니다.

다음은 `GridDemoPage` 클래스에 대 한 XAML 파일의 시작 이며 `RowDefinitions` 및 `ColumnDefinitions` 컬렉션에 대 한 속성 요소 태그를 보여 줍니다.

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

자동 크기 셀을 셀의 픽셀 너비 및 높이 별 설정을 정의 하는 것에 대 한 약식된 구문을 확인 합니다.

## <a name="attached-properties"></a>연결된 속성

`Grid`에는 `RowDefinitions` 및 `ColumnDefinitions` 컬렉션에 대 한 속성 요소가 필요 하 고 행과 열을 정의 하는 것이 나타났습니다. 그러나 프로그래머가 `Grid`의 각 자식이 있는 행과 열을 나타낼 수 있는 방법이 있어야 합니다.

`Grid`의 각 자식에 대 한 태그 내에서 다음 특성을 사용 하 여 해당 자식의 행과 열을 지정 합니다.

- `Grid.Row`
- `Grid.Column`

이러한 특성의 기본값은 0입니다. 둘 이상의 행 또는 열의 이러한 특성을 사용 하 여 자식 걸쳐 있는 경우를 나타낼 수도 있습니다.

- `Grid.RowSpan`
- `Grid.ColumnSpan`

이러한 두 특성 기본값이 1입니다.

다음은 전체 GridDemoPage.xaml 파일이입니다.

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

`Grid.Row` 및 `Grid.Column` 설정 0은 필요 하지 않지만 명확 하 게 하기 위해 일반적으로 포함 됩니다.

해당 모양은 다음과 같습니다.

[![모눈 레이아웃](essential-xaml-syntax-images/griddemo.png)](essential-xaml-syntax-images/griddemo-large.png#lightbox)

이러한 `Grid.Row`, `Grid.Column`, `Grid.RowSpan`및 `Grid.ColumnSpan` 특성은 구문 에서만 심사 `Grid`의 정적 필드 또는 속성으로 보이지만 `Grid`, `Row`, `Column`또는 `RowSpan`이라는 이름을 정의 하지는 않습니다.`ColumnSpan`

대신 `Grid` `RowProperty`, `ColumnProperty`, `RowSpanProperty`및 `ColumnSpanProperty`이라는 네 개의 바인딩 가능한 속성을 정의 합니다. 이러한 속성은 *연결 된 속성*이라고 하는 바인딩 가능한 속성의 특수 한 형식입니다. 이러한 클래스는 `Grid` 클래스에 의해 정의 되지만 `Grid`의 자식에 대해 설정 됩니다.

이러한 연결 된 속성을 코드에서 사용 하려는 경우 `Grid` 클래스는 `SetRow`, `GetColumn`등의 정적 메서드를 제공 합니다. 그러나 XAML에서 이러한 연결 된 속성은 간단한 속성 이름을 사용 하 여 `Grid`의 자식에서 특성으로 설정 됩니다.

연결 된 속성은 항상 인식할 수 있는 XAML 파일에서 클래스와 속성 이름에서 마침표를 포함 하는 특성으로 합니다. 이러한 속성은 하나의 클래스 (이 경우에는 `Grid`)에 의해 정의 되 고 다른 개체 (이 경우에는 `Grid`의 자식)에 연결 되기 때문에 *연결 된 속성* 이라고 합니다. 레이아웃 하는 동안 `Grid`는 이러한 연결 된 속성의 값을 확인 하 여 각 자식의 위치를 알 수 있습니다.

`AbsoluteLayout` 클래스는 `LayoutBounds` 및 `LayoutFlags`라는 두 개의 연결 된 속성을 정의 합니다. `AbsoluteLayout`의 비례 배치 및 크기 조정 기능을 사용 하 여 인식 되는 바둑판 패턴은 다음과 같습니다.

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

와 같습니다.

[![절대 레이아웃](essential-xaml-syntax-images/absolutedemo-large.png)](essential-xaml-syntax-images/absolutedemo-large.png#lightbox)

이와 같은 무엇 인가 XAML을 사용 하 여 지식을 질문 수 있습니다. `LayoutBounds` 사각형의 반복 및 질서는 코드에서 더 잘 인식 될 수 있다는 것을 의미 합니다.

합법적인 문제가 분명 하 고 사용자 인터페이스를 정의 하는 경우 코드와 태그를 사용 하 여 분산 문제가 없습니다. XAML에 시각적 개체 중 일부를 정의 하 고 다음 코드 숨김 파일의 생성자를 사용 하 여 루프에서 더 잘 발생할 수 있는 몇 가지 자세한 시각적 개체를 추가 하는 것이 쉽습니다.

## <a name="content-properties"></a>콘텐츠 속성

이전 예에서는 `StackLayout`, `Grid`및 `AbsoluteLayout` 개체가 `ContentPage`의 `Content` 속성으로 설정 되 고 이러한 레이아웃의 자식은 실제로 `Children` 컬렉션의 항목입니다. 그러나 이러한 `Content` 및 `Children` 속성은 XAML 파일에 없습니다.

`Content` 및 `Children` 속성을 **XamlPlusCode** 샘플과 같은 속성 요소로 포함할 수 있습니다.

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

실제 질문: 이러한 속성 요소가 XAML 파일에 필요 *하지* 않은 이유는 무엇 인가요?

XAML에서 사용 하기 위해 Xamarin.ios에 정의 된 요소는 클래스의 `ContentProperty` 특성에 플래그 지정 된 속성을 하나 포함할 수 있습니다. 온라인 Xamarin.ios 설명서에서 `ContentPage` 클래스를 조회 하는 경우이 특성을 확인할 수 있습니다.

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

이는 `Content` 속성 요소 태그가 필요 하지 않음을 의미 합니다. Start 및 end `ContentPage` 태그 사이에 표시 되는 모든 XML 콘텐츠는 `Content` 속성에 할당 된 것으로 간주 됩니다.

 `StackLayout`, `Grid`, `AbsoluteLayout`및 `RelativeLayout` 모두 `Layout<View>`에서 파생 되며, Xamarin.ios 설명서에서 `Layout<T>`을 조회 하는 경우 다른 `ContentProperty` 특성을 볼 수 있습니다.

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

이렇게 하면 명시적 `Children` 속성 요소 태그 없이 레이아웃의 콘텐츠를 `Children` 컬렉션에 자동으로 추가할 수 있습니다.

다른 클래스에도 `ContentProperty` 특성 정의가 있습니다. 예를 들어 `Label`의 content 속성은 `Text`입니다. 다른 사용자에 대 한 API 설명서를 확인 합니다.

## <a name="platform-differences-with-onplatform"></a>OnPlatform 사용 하 여 플랫폼의 차이점

단일 페이지 응용 프로그램에서는 iOS 상태 표시줄을 덮어쓰지 않도록 페이지의 `Padding` 속성을 설정 하는 것이 일반적입니다. 코드에서는이를 위해 `Device.RuntimePlatform` 속성을 사용할 수 있습니다.

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) 및 [`On`](xref:Xamarin.Forms.On) 클래스를 사용 하 여 XAML에서 유사한 작업을 수행할 수도 있습니다. 먼저 페이지 맨 위 근처의 `Padding` 속성에 대 한 속성 요소를 포함 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

이러한 태그 내에 `OnPlatform` 태그를 포함 합니다. `OnPlatform`는 제네릭 클래스입니다. 제네릭 형식 인수 (이 경우에는 `Padding` 속성의 형식인 `Thickness`)를 지정 해야 합니다. 다행히 `x:TypeArguments`라는 제네릭 인수를 정의 하는 XAML 특성이 있습니다. 설정 속성의 형식을 일치 해야 합니다.

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

`OnPlatform`에는 `On` 개체의 `IList` `Platforms` 라는 속성이 있습니다. 해당 속성에 대 한 속성 요소 태그를 사용 합니다.

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

이제 `On` 요소를 추가 합니다. 각 항목에 대해 `Platform` 속성을 설정 하 고 `Value` 속성을 `Thickness` 속성의 태그로 설정 합니다.

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

이 태그를 단순화할 수 있습니다. `OnPlatform`의 content 속성은 `Platforms`이므로 해당 속성 요소 태그를 제거할 수 있습니다.

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

`On`의 `Platform` 속성은 `IList<string>`형식 이므로 값이 동일한 경우 여러 플랫폼을 포함할 수 있습니다.

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

Android 및 UWP는 `Padding`기본값으로 설정 되어 있으므로 해당 태그를 제거할 수 있습니다.

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

이는 XAML에서 플랫폼 종속 `Padding` 속성을 설정 하는 표준 방법입니다. `Value` 설정을 단일 문자열로 나타낼 수 없는 경우에는 속성 요소를 정의할 수 있습니다.

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
> XAML에서 `OnPlatform` 태그 확장을 사용 하 여 플랫폼 별로 UI 모양을 사용자 지정할 수도 있습니다. `OnPlatform` 및 `On` 클래스와 동일한 기능을 제공 하지만 보다 간결한 표현을 제공 합니다. 자세한 내용은 [Onplatform 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform)을 참조 하세요.

## <a name="summary"></a>요약

속성 요소 및 연결 속성을 사용하여 상당 부분의 기본 XAML 구문을 설정하였습니다. 그러나 때로는 리소스 사전과 같은 간접 방식으로 개체 속성을 설정해야 합니다. 이 방법은 다음 부분인 3 부에서 다룹니다 [. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)입니다.

## <a name="related-links"></a>관련 링크

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [1 부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [3 부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [4 부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [5 부. MVVM에 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
