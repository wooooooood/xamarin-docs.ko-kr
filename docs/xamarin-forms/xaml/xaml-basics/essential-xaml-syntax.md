---
title: 2부. 필수 XAML 구문
description: 이 문서에서는 속성 요소와 연결 된 속성의 필수 XAML 구문 기능에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c400bb342568a0399e2a582496f85ead273b6994
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84572184"
---
# <a name="part-2-essential-xaml-syntax"></a>2부. 필수 XAML 구문

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML은 일반적으로 개체를 인스턴스화하고 초기화 하기 위해 디자인 되었습니다. 그러나 속성은 XML 문자열로 표현할 수 없는 복합 개체로 설정 해야 하며, 경우에 따라 한 클래스에서 정의 되는 속성을 자식 클래스에서 설정 해야 합니다. 이러한 두 가지 요구 사항에는 속성 요소 및 연결 된 속성의 필수 XAML 구문 기능이 필요 합니다._

## <a name="property-elements"></a>속성 요소

XAML에서 클래스의 속성은 일반적으로 XML 특성으로 설정 됩니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

그러나 XAML에서 속성을 설정 하는 다른 방법이 있습니다. 에서이 대안을 시도 하려면 `TextColor` 먼저 기존 설정을 삭제 합니다 `TextColor` .

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

이러한 태그 내에서 클래스 이름 및 속성 이름으로 구성 된 시작 및 끝 태그를 마침표로 구분 하 여 추가 합니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

다음과 같이 이러한 새 태그의 콘텐츠로 속성 값을 설정 합니다.

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

속성을 지정 하는 두 가지 방법은 기능적으로 동일 `TextColor` 하지만 속성을 실제로 두 번 설정 하는 것이 모호 해질 수 있으므로 동일한 속성에 두 가지 방법을 사용 하지 마십시오.

이 새로운 구문을 사용 하 여 몇 가지 유용한 용어를 도입할 수 있습니다.

- `Label`는 *개체 요소*입니다. Xamarin.FormsXML 요소로 표현 되는 개체입니다.
- `Text`, `VerticalOptions` `FontAttributes` 및 `FontSize` 은 *속성 특성*입니다. 이러한 Xamarin.Forms 속성은 XML 특성으로 표현 되는 속성입니다.
- 이 마지막 코드 조각에서는 `TextColor` 가 *속성 요소가*되었습니다. Xamarin.Forms속성 이지만 이제는 XML 요소입니다.

속성 요소의 정의는 처음에 XML 구문을 위반 하는 것 처럼 보일 수 있지만이는 그렇지 않습니다. XML에서는이 기간에 특별 한 의미가 없습니다. XML 디코더에 `Label.TextColor` 는 단순히 일반 자식 요소입니다.

그러나 XAML에서이 구문은 매우 특수 합니다. 속성 요소에 대 한 규칙 중 하나는 태그에 아무 것도 표시 될 수 없다는 것입니다 `Label.TextColor` . 속성의 값은 항상 속성 요소 시작 태그와 끝 태그 사이에 콘텐츠로 정의 됩니다.

두 개 이상의 속성에 대 한 속성-요소 구문을 사용할 수 있습니다.

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

또는 모든 속성에 대해 속성-요소 구문을 사용할 수 있습니다.

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

처음에는 속성 요소 구문이 비교적 매우 간단한 항목에 대 한 불필요 한 긴 winded 대체 처럼 보일 수 있으며,이 예제에서는이 경우에 해당 합니다.

그러나 속성의 값이 너무 복잡 하 여 간단한 문자열로 표현할 수 없는 경우에는 속성 요소 구문이 필수적입니다. 속성-요소 태그 내에서 다른 개체를 인스턴스화하고 해당 속성을 설정할 수 있습니다. 예를 들어 `VerticalOptions` 속성 설정을 사용 하 여 속성을 값으로 명시적으로 설정할 수 있습니다 `LayoutOptions` .

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

또 다른 예:에는 `Grid` 및 라는 두 개의 속성이 있습니다. `RowDefinitions` `ColumnDefinitions` 이러한 두 속성은 및 `RowDefinitionCollection` `ColumnDefinitionCollection` 개체의 컬렉션인 및 형식입니다 `RowDefinition` `ColumnDefinition` . 이러한 컬렉션을 설정 하려면 속성 요소 구문을 사용 해야 합니다.

다음은 클래스에 대 한 XAML 파일의 시작 `GridDemoPage` 이며 및 컬렉션의 속성 요소 태그를 보여 `RowDefinitions` 줍니다 `ColumnDefinitions` .

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

자동 크기 셀, 픽셀 너비와 높이의 셀 및 별 설정을 정의 하기 위한 축약 된 구문을 확인 합니다.

## <a name="attached-properties"></a>연결된 속성

에서 `Grid` 및 컬렉션에 대 한 속성 요소를 사용 하 `RowDefinitions` 여 `ColumnDefinitions` 행과 열을 정의 하는 것을 확인 했습니다. 그러나 프로그래머가의 각 자식이 있는 행과 열을 표시 하는 방법도 있습니다 `Grid` .

의 각 자식에 대 한 태그 내에서 `Grid` 다음 특성을 사용 하 여 해당 자식의 행과 열을 지정 합니다.

- `Grid.Row`
- `Grid.Column`

이러한 특성의 기본값은 0입니다. 또한 다음과 같은 특성을 가진 자식이 둘 이상의 행 또는 열에 걸쳐 있는지 여부를 나타낼 수 있습니다.

- `Grid.RowSpan`
- `Grid.ColumnSpan`

이러한 두 특성의 기본값은 1입니다.

전체 GridDemoPage .xaml 파일은 다음과 같습니다.

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

`Grid.Row`및 `Grid.Column` 설정 0은 필요 하지 않지만 일반적으로 명확 하 게 나타내기 위해 포함 됩니다.

모양은 다음과 같습니다.

[![그리드 레이아웃](essential-xaml-syntax-images/griddemo.png)](essential-xaml-syntax-images/griddemo-large.png#lightbox)

구문 에서만 심사,,, `Grid.Row` `Grid.Column` `Grid.RowSpan` 및 `Grid.ColumnSpan` 특성은의 정적 필드 또는 속성으로 보이지만,,, `Grid` `Grid` 또는 이라는 이름을 정의 하지 않습니다 `Row` `Column` `RowSpan` `ColumnSpan` .

대신는,, `Grid` 및 라는 네 개의 바인딩 가능한 속성을 정의 `RowProperty` `ColumnProperty` `RowSpanProperty` `ColumnSpanProperty` 합니다. 이러한 속성은 *연결 된 속성*이라고 하는 바인딩 가능한 속성의 특수 한 형식입니다. 클래스에 의해 정의 `Grid` 되지만의 자식에 대해 설정 됩니다 `Grid` .

이러한 연결 된 속성을 코드에서 사용 하려는 경우 클래스는 `Grid` , 등 이라는 정적 메서드를 제공 `SetRow` `GetColumn` 합니다. 그러나 XAML에서 이러한 연결 된 속성은 `Grid` 간단한 속성 이름을 사용 하 여의 자식에서 특성으로 설정 됩니다.

연결 된 속성은 항상 XAML 파일에서 마침표로 구분 된 클래스 및 속성 이름을 포함 하는 특성으로 인식 됩니다. 이러한 *속성* 은 하나의 클래스 (이 경우)에 의해 정의 되 `Grid` 고 다른 개체 (이 경우의 자식)에 연결 되기 때문에 연결 된 속성 이라고 `Grid` 합니다. 레이아웃 중에는 `Grid` 이러한 연결 된 속성의 값을 검사 하 여 각 자식의 위치를 알 수 있습니다.

`AbsoluteLayout`클래스는 및 이라는 두 개의 연결 된 속성을 정의 합니다 `LayoutBounds` `LayoutFlags` . 다음은의 비례 위치 지정 및 크기 조정 기능을 사용 하 여 실현 되는 바둑판 패턴입니다 `AbsoluteLayout` .

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

여기서는 다음과 같습니다.

[![절대 레이아웃](essential-xaml-syntax-images/absolutedemo-large.png)](essential-xaml-syntax-images/absolutedemo-large.png#lightbox)

이와 같은 경우 XAML을 사용 하는 지혜가를 확인할 수 있습니다. 즉, 사각형의 반복 및 질서는 `LayoutBounds` 코드에서 더 잘 인식 될 수 있다는 것을 의미 합니다.

이것은 타당 한 문제 이며 사용자 인터페이스를 정의할 때 코드 및 태그의 사용을 분산 하는 데 문제가 없습니다. XAML에서 시각적 개체 중 일부를 정의 하 고 코드를 사용 하는 파일의 생성자를 사용 하 여 루프에서 더 잘 생성 될 수 있는 시각적 개체를 추가 하는 것이 쉽습니다.

## <a name="content-properties"></a>콘텐츠 속성

이전 예제에서 `StackLayout` , `Grid` 및 `AbsoluteLayout` 개체는의 속성으로 설정 되 고, `Content` `ContentPage` 이러한 레이아웃의 자식은 실제로 컬렉션의 항목 `Children` 입니다. 그러나 이러한 `Content` 및 `Children` 속성은 XAML 파일에 없습니다.

`Content` `Children` **XamlPlusCode** 샘플과 같이 및 속성을 속성 요소로 포함할 수 있습니다.

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

Xamarin.FormsXAML에서 사용 하기 위해에 정의 된 요소에는 클래스의 특성에 플래그가 지정 된 속성이 하나만 있을 수 있습니다 `ContentProperty` . `ContentPage`온라인 설명서에서 클래스를 조회 하면 Xamarin.Forms 다음 특성이 표시 됩니다.

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

즉, `Content` 속성 요소 태그가 필요 하지 않습니다. 시작 태그와 끝 태그 사이에 나타나는 모든 XML 콘텐츠 `ContentPage` 는 속성에 할당 된 것으로 간주 됩니다 `Content` .

 `StackLayout`, `Grid` , `AbsoluteLayout` 및 `RelativeLayout` 는 모두에서 파생 `Layout<View>` 되며, 설명서를 살펴보면 `Layout<T>` Xamarin.Forms 다른 특성을 볼 수 있습니다 `ContentProperty` .

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

`Children`명시적 속성 요소 태그가 없으면 레이아웃의 콘텐츠를 컬렉션에 자동으로 추가할 수 있습니다 `Children` .

다른 클래스에도 `ContentProperty` 특성 정의가 있습니다. 예를 들어의 content 속성 `Label` 은입니다 `Text` . 다른 사용자에 대 한 API 설명서를 확인 하세요.

## <a name="platform-differences-with-onplatform"></a>OnPlatform의 플랫폼 차이점

단일 페이지 응용 프로그램에서는 `Padding` iOS 상태 표시줄을 덮어쓰지 않도록 페이지의 속성을 설정 하는 것이 일반적입니다. 코드에서는이를 위해 속성을 사용할 수 있습니다 `Device.RuntimePlatform` .

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

및 클래스를 사용 하 여 XAML에서 유사한 작업을 수행할 수도 있습니다 [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) [`On`](xref:Xamarin.Forms.On) . 먼저 페이지 맨 위 근처의 속성에 대 한 속성 요소를 포함 합니다 `Padding` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

이러한 태그 내에 태그를 포함 `OnPlatform` 합니다. `OnPlatform`는 제네릭 클래스입니다. 이 경우 속성의 형식인 제네릭 형식 인수를 지정 해야 합니다 `Thickness` `Padding` . 다행히 라는 제네릭 인수를 정의 하는 XAML 특성이 있습니다 `x:TypeArguments` . 설정 하는 속성의 형식과 일치 해야 합니다.

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

`OnPlatform`에는 개체의 인 라는 속성이 있습니다 `Platforms` `IList` `On` . 해당 속성에 대해 속성 요소 태그를 사용 합니다.

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

이제 `On` 요소를 추가 합니다. 각 항목에 대해 속성 `Platform` 및 속성을 `Value` 속성의 태그로 설정 합니다 `Thickness` .

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

이 태그는 단순화할 수 있습니다. 의 content 속성 `OnPlatform` 은 이므로 `Platforms` 해당 속성 요소 태그를 제거할 수 있습니다.

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

`Platform`의 속성 `On` 은 형식이 `IList<string>` 이므로 값이 동일한 경우 여러 플랫폼을 포함할 수 있습니다.

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

Android 및 UWP는의 기본값으로 설정 되므로 `Padding` 해당 태그를 제거할 수 있습니다.

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

이는 XAML에서 플랫폼 종속 속성을 설정 하는 표준 방법입니다 `Padding` . `Value`설정을 단일 문자열로 나타낼 수 없는 경우 해당 설정을 위한 속성 요소를 정의할 수 있습니다.

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
> `OnPlatform`태그 확장을 XAML에서 사용 하 여 플랫폼 별로 UI 모양을 사용자 지정할 수도 있습니다. 이 클래스는 및 클래스와 같은 기능을 제공 `OnPlatform` `On` 하지만 좀 더 간결 하 게 표현 합니다. 자세한 내용은 [Onplatform 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)을 참조 하세요.

## <a name="summary"></a>요약

속성 요소와 연결 된 속성을 사용 하면 대부분의 기본 XAML 구문이 설정 되었습니다. 그러나 경우에 따라 리소스 사전에서와 같이 간접적으로 개체에 속성을 설정 해야 합니다. 이 방법은 다음 부분인 3 부에서 다룹니다 [. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)입니다.

## <a name="related-links"></a>관련 링크

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [1 부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [3 부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [4 부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [5 부. MVVM에 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
