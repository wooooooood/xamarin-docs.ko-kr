---
title: 2 부입니다. 필수 XAML 구문
description: 이 문서에서는 속성 요소의 필수 XAML 구문 기능 설명 및 연결 된 속성입니다.
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: 985ca3b34b4b85ef234f12fe3f25edd1d1556e23
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563240"
---
# <a name="part-2-essential-xaml-syntax"></a>2 부입니다. 필수 XAML 구문

_XAML은 주로 인스턴스화 및 초기화 개체에 대 한 설계 되었습니다. 하지만 XML 문자열로 쉽게 표현할 수 없는 복잡 한 개체에 속성을 설정 해야 하 고 하나의 클래스에 의해 정의 된 속성을 자식 클래스에 설정 해야 하는 경우에 따라 경우가 많습니다. 이러한 두 요구 속성 요소 및 연결 된 속성의 필수 XAML 구문 기능 필요합니다._

## <a name="property-elements"></a>속성 요소

XAML을에서 클래스의 속성은 일반적으로 XML 특성으로 설정 됩니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

그러나 XAML에서 속성을 설정 하는 대체 방법이 있습니다. 사용 하 여이 대안을 시도해보십시오 `TextColor`, 먼저 기존 삭제 `TextColor` 설정:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

빈 요소 열기 `Label` 시작 및 끝 태그를 구분 하 여 태그:

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

이 두 가지 방법을 지정 하는 `TextColor` 속성 기능적으로 동일 하지만 효과적으로 설정할 속성을 두 번을 모호할 수 있으므로 동일한 속성에 대 한 두 가지 방법으로 사용 하지 마세요.

이 새 구문을 사용 하 여 몇 가지 유용한 용어를 도입할 수 있습니다.

-  `Label` *object 요소*합니다. XML 요소로 표현 된 Xamarin.Forms 개체입니다.
-  `Text`를 `VerticalOptions`, `FontAttributes` 하 고 `FontSize` 됩니다 *속성 특성*합니다. Xamarin.Forms 속성 XML 특성으로 표현 됩니다.
-  해당 최종 조각과에서 `TextColor` 바뀌었기를 *property 요소*. 이 Xamarin.Forms 속성 이지만 이제 XML 요소입니다.


속성에서 요소 수의 정의 먼저 XML 구문의 위반 같지만 아닙니다. 마침표는 XML에서 특별 한 의미가 없습니다. XML 디코더의 `Label.TextColor` 단순히 일반 자식 요소입니다.

하지만 XAML을이 구문이 매우 특별 한 합니다. 속성 요소에 대 한 규칙 중 하나는 아무에 나타날 수 있음을 `Label.TextColor` 태그입니다. 속성의 값 속성 요소 시작 태그와 끝 태그 사이 콘텐츠로 항상 정의 됩니다.

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

그러나 속성의 값이 너무 복잡해 서 간단한 문자열로 표현 될 때 속성 요소 구문을 필수적입니다. 속성 요소 태그 내의 다른 개체 인스턴스화하고 해당 속성을 설정 합니다. 예를 들어 하면 명시적으로 설정할 수 속성을 같은 `VerticalOptions` 에 `LayoutOptions` 속성이 설정 된 값:

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

또 다른 예: 합니다 `Grid` 라는 두 속성이 `RowDefinitions` 및 `ColumnDefinitions`합니다. 이러한 속성은 두 가지 유형의 `RowDefinitionCollection` 하 고 `ColumnDefinitionCollection`, 컬렉션인 `RowDefinition` 및 `ColumnDefinition` 개체입니다. 이러한 컬렉션을 설정 하려면 속성 요소 구문을 사용 해야 합니다.

여기는 XAML 파일의 시작 부분을 `GridDemoPage` 클래스에 대 한 속성 요소 태그를 표시 합니다 `RowDefinitions` 및 `ColumnDefinitions` 컬렉션:

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

방금 살펴봤습니다는 `Grid` 속성 요소에 대 한 필요 합니다 `RowDefinitions` 및 `ColumnDefinitions` 행 및 열을 정의 하는 컬렉션입니다. 그러나도 몇 가지 방법이 있어야 행 및 열을 나타내는 때문에 프로그래머가 있는의 각 자식은 `Grid` 상주 합니다.

태그의 각 자식에는 `Grid` 다음 특성을 사용 하 여 해당 자식 열과 행을 지정 합니다.

-  `Grid.Row`
-  `Grid.Column`

이러한 특성의 기본값은 0입니다. 둘 이상의 행 또는 열의 이러한 특성을 사용 하 여 자식 걸쳐 있는 경우를 나타낼 수도 있습니다.

-  `Grid.RowSpan`
-  `Grid.ColumnSpan`

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

합니다 `Grid.Row` 고 `Grid.Column` 0의 설정이 필요 하지 않지만 명확히 전달 하기 위해 일반적으로 포함 됩니다.

모양을 세 플랫폼 모두에서 다음과 같습니다.

[![](essential-xaml-syntax-images/griddemo.png "모눈 레이아웃")](essential-xaml-syntax-images/griddemo-large.png#lightbox "모눈 레이아웃")

구문에서 전적으로 미루어 보면 이러한 `Grid.Row`, `Grid.Column`, `Grid.RowSpan`, 및 `Grid.ColumnSpan` 정적 필드 또는 속성의 특성이 나타납니다 `Grid`, 하지만 흥미롭게도 `Grid` 이라는 정의 하지 않습니다 `Row`, `Column`하십시오 `RowSpan`, 또는 `ColumnSpan`합니다.

대신 `Grid` 라는 4 개의 바인딩 가능한 속성 정의 `RowProperty`를 `ColumnProperty`를 `RowSpanProperty`, 및 `ColumnSpanProperty`합니다. 바인딩 가능한 속성으로 알려진 특수 한 유형의 이들은 *연결 된 속성*합니다. 정의한를 `Grid` 하지만 자식 클래스에 설정 된 `Grid`합니다.

연결 된 코드에서 속성을 사용 하려는 경우는 `Grid` 라는 정적 메서드를 제공 하는 클래스 `SetRow`, `GetColumn`등입니다. XAML에서 이러한 연결 된 속성의 자식에 있는 특성으로 설정 됩니다 있지만 `Grid` 단순 속성 이름을 사용 하 여 합니다.

연결 된 속성은 항상 인식할 수 있는 XAML 파일에서 클래스와 속성 이름에서 마침표를 포함 하는 특성으로 합니다. 라고 *연결 된 속성* 하나의 클래스에서 정의 되기 때문 (이 예제의 경우 `Grid`) 하지만, 다른 개체에 연결 된 (여기서는 자식의 `Grid`). 레이아웃 중는 `Grid` 각 자식을 배치 하는 위치를 알아야 이러한 연결 된 속성의 값을 검색할 수 있습니다.

합니다 `AbsoluteLayout` 라는 두 가지 연결 된 속성을 정의 하는 클래스 `LayoutBounds` 고 `LayoutFlags`입니다. 다음은 비례 위치 지정을 사용 하 여 실현을 체크 무늬 패턴 및 크기 조정 기능의 `AbsoluteLayout`:

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

[![](essential-xaml-syntax-images/absolutedemo-large.png "절대 레이아웃")](essential-xaml-syntax-images/absolutedemo-large.png#lightbox "절대 레이아웃")

이와 같은 무엇 인가 XAML을 사용 하 여 지식을 질문 수 있습니다. 물론 반복 및의 질서를 `LayoutBounds` 사각형 제안 하 고 실현 될 수 있습니다 더 잘 코드에서입니다.

합법적인 문제가 분명 하 고 사용자 인터페이스를 정의 하는 경우 코드와 태그를 사용 하 여 분산 문제가 없습니다. XAML에 시각적 개체 중 일부를 정의 하 고 다음 코드 숨김 파일의 생성자를 사용 하 여 루프에서 더 잘 발생할 수 있는 몇 가지 자세한 시각적 개체를 추가 하는 것이 쉽습니다.

## <a name="content-properties"></a>콘텐츠 속성

이전 예제에서를 `StackLayout`, `Grid`, 및 `AbsoluteLayout` 개체가으로 설정 됩니다는 `Content` 의 속성을 `ContentPage`, 이러한 레이아웃 중 자식 항목이 실제로에서 및를 `Children` 컬렉션. 아직 이러한 `Content` 및 `Children` 장소가 없으며 속성은 XAML 파일에 있습니다.

확실히 포함할 수 있습니다는 `Content` 하 고 `Children` 같은 속성 요소로 속성을 **XamlPlusCode** 샘플:

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

현실적인 질문은: 속성 요소가 이러한 이유 *되지* 는 XAML 파일에 필요한?

Xamarin.Forms XAML에서 사용 하기 위해 정의 된 요소에 플래그가 지정 된 하나의 속성을 포함할 수 있습니다는 `ContentProperty` 클래스의 특성입니다. 조회 하는 경우는 `ContentPage` 클래스 Xamarin.Forms 설명서의 온라인에이 특성이 표시 됩니다.

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

즉는 `Content` 속성 요소 태그는 필요 하지 않습니다. 시작과 끝 사이 나타나는 모든 XML 콘텐츠 `ContentPage` 태그에 할당할 가정는 `Content` 속성입니다.

 `StackLayout`를 `Grid`, `AbsoluteLayout`, 및 `RelativeLayout` 에서 파생 되는 모든 `Layout<View>`를 조회 하는 경우 `Layout<T>` Xamarin.Forms 설명서에서 보면 다른 `ContentProperty` 특성:

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

내용의 레이아웃을 자동으로 추가할 수 있도록 합니다 `Children` 없이 컬렉션 명시적 `Children` 속성 요소 태그입니다.

다른 클래스에 `ContentProperty` 특성 정의 합니다. 콘텐츠 속성의 예를 들어 `Label` 는 `Text`합니다. 다른 사용자에 대 한 API 설명서를 확인 합니다.

## <a name="platform-differences-with-onplatform"></a>OnPlatform 사용 하 여 플랫폼의 차이점

단일 페이지 응용 프로그램의 설정에 공통적으로 적용 되는 `Padding` iOS 상태 표시줄을 덮어쓰지 않도록 하려면 페이지의 속성입니다. 코드에서 사용할 수는 `Device.RuntimePlatform` 이 목적을 위해 속성:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

XAML 사용 하 여 비슷한 수행할 수도 있습니다는 `OnPlatform` 고 `On` 클래스입니다. 첫 번째 속성 요소는 포함 된 `Padding` 속성 페이지의 위쪽에:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

이 태그 내 포함을 `OnPlatform` 태그입니다. `OnPlatform` 제네릭 클래스가입니다. 이 경우 제네릭 형식 인수를 지정 해야 `Thickness`의 형식인 `Padding` 속성입니다. 다행 스럽게도 호출 하는 제네릭 인수 정의 맞게 XAML 특성을 `x:TypeArguments`입니다. 설정 속성의 형식을 일치 해야 합니다.

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

`OnPlatform` 이라는 속성을 가진 `Platforms` 즉를 `IList` 의 `On` 개체입니다. 해당 속성에 대 한 속성 요소 태그를 사용 합니다.

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

이제 추가 `On` 요소입니다. 각각에 대해 설정 합니다 `Platform` 속성 및 `Value` 속성에 대 한 태그는 `Thickness` 속성:

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

이 태그를 단순화할 수 있습니다. 콘텐츠 속성을 `OnPlatform` 는 `Platforms`이므로 이러한 속성 요소 태그를 제거할 수 있습니다.

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

`Platform` 속성을 `On` 형식의 `IList<string>`이므로 값은 동일한 경우에 여러 플랫폼을 포함할 수 있습니다.

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

Android 및 UWP의 기본값으로 설정 되어 있으므로 `Padding`, 태그를 제거할 수 있습니다.

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

이 플랫폼에 종속 된을 설정 하는 표준 방법은 `Padding` XAML의 속성입니다. 경우는 `Value` 설정은 단일 문자열로 표현할 수 없는, 해당 속성 요소를 정의할 수 있습니다.

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

## <a name="summary"></a>요약

속성 요소 및 연결 된 속성을 사용 하 여 기본 XAML 구문의 상당 부분이 설정 되었습니다. 그러나 때로는 해야 리소스 사전에서 간접 방식으로, 예를 들어, 개체 속성을 설정 합니다. 이 방법은 다음 부분을 파트에 대해서는 [3입니다. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)합니다.



## <a name="related-links"></a>관련 링크

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [1부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [3부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [4부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [5부. MVVM에 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
