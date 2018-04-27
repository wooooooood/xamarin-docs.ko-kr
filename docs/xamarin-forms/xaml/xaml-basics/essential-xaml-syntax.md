---
title: 2 부 합니다. 필수 XAML 구문
description: XAML은 주로 위한 것 인스턴스화 및 개체를 초기화 합니다. 하지만 XML 문자열로 쉽게 표현할 수 없는 복잡 한 개체에 속성을 설정 해야 하 고 하나의 클래스에 의해 정의 된 속성을 자식 클래스에 설정 해야 하는 경우에 따라 경우가 많습니다. 이러한 두 보안 요구 사항 속성 요소와 연결 된 속성의는 중요 한 XAML 구문 기능이 필요합니다.
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: d0129ec9872d8e5270ed8f0072cff0035d4f5255
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="part-2-essential-xaml-syntax"></a>2 부 합니다. 필수 XAML 구문

_XAML은 주로 위한 것 인스턴스화 및 개체를 초기화 합니다. 하지만 XML 문자열로 쉽게 표현할 수 없는 복잡 한 개체에 속성을 설정 해야 하 고 하나의 클래스에 의해 정의 된 속성을 자식 클래스에 설정 해야 하는 경우에 따라 경우가 많습니다. 이러한 두 보안 요구 사항 속성 요소와 연결 된 속성의는 중요 한 XAML 구문 기능이 필요합니다._

## <a name="property-elements"></a>속성 요소

Xaml에서는 클래스의 속성 일반적으로 XML 특성으로 설정 됩니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

그러나 XAML에서 속성을 설정 하는 대체 방법이 있습니다. 와이 대안을 시도해보십시오 `TextColor`, 먼저 기존 삭제 `TextColor` 설정:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

빈 요소를 열기 `Label` 시작 및 끝 태그를 분리 하 여 태그:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

이 태그 내 클래스 이름과 마침표로 구분 하는 속성 이름으로 구성 된 시작 및 끝 태그를 추가 합니다.

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

다음과 같이 이러한 새 태그 내용으로 속성 값을 설정 합니다.

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

이 두 가지 방법을 지정 하는 `TextColor` 속성 기능적으로 동일 하지만 효과적으로 설정 속성을 두 번 하 고 모호할 수 때문에 동일한 속성에 대 한 두 가지 방법으로 사용 하지 마십시오.

이 새 구문을 사용 하 여 몇 가지 유용한 용어를 제공할 수 있습니다.

-  `Label` 이 *object 요소*합니다. Xamarin.Forms 개체인 XML 요소로 표현 됩니다.
-  `Text``VerticalOptions`, `FontAttributes` 및 `FontSize` 는 *속성 특성*합니다. 이들은 XML 특성으로 표현 하는 Xamarin.Forms 속성입니다.
-  해당 최종 코드 조각에서 `TextColor` 바뀌었기는 *속성 요소*합니다. Xamarin.Forms 속성 이지만 이제는 XML 요소입니다.


것만 요소 수에 속성의 정의 먼저 XML 구문 위반 될 것 같습니다. 마침표는 XML에서 특별 한 의미가 없습니다. XML 디코더에 `Label.TextColor` 단순히 일반 자식 요소입니다.

그러나 xaml에서는이 구문이 매우 특별 한 되었습니다. 속성 요소에 대 한 규칙 중 하나는 방법 이외에 나타날 수는 `Label.TextColor` 태그입니다. 속성의 값은 항상 속성 요소의 시작 및 끝 태그 사이 내용을으로 정의 됩니다.

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

처음에 속성 요소 구문을 한 불필요 한 너무 길었다 면 죄송 대안 처럼 비교적 매우 간단한 문제에 대 한 것 처럼 보일 수 및 이러한 예에서 않은 확실히 경우.

그러나 속성의 값이 너무 복잡해 서 수를 문자열로 표현 될 때 속성 요소 구문을 필수 됩니다. 속성 요소 태그 내 다른 개체를 인스턴스화할 수 있으며 해당 속성을 설정할 키를 누릅니다. 예를 들어 하면 명시적으로 설정할 수 속성을 같은 `VerticalOptions` 에 `LayoutOptions` 속성이 설정 된 값:

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

또 다른 예:는 `Grid` 라는 두 속성이 `RowDefinitions` 및 `ColumnDefinitions`합니다. 이러한 속성은 두 가지 유형의 `RowDefinitionCollection` 및 `ColumnDefinitionCollection`, 컬렉션인 `RowDefinition` 및 `ColumnDefinition` 개체입니다. 이러한 컬렉션을 설정 하려면 속성 요소 구문을 사용 해야 합니다.

여기에 대 한 XAML 파일의 시작 부분을가 `GridDemoPage` 에 대 한 속성 요소 태그를 보여 주는 클래스는 `RowDefinitions` 및 `ColumnDefinitions` 컬렉션:

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

자동 크기 셀, 픽셀 너비와 높이 및 스타 설정의 셀 정의 대 한 약식된 구문을 확인 합니다.

## <a name="attached-properties"></a>연결된 속성

프로필이 있는 `Grid` 속성 요소에 대 한 요구는 `RowDefinitions` 및 `ColumnDefinitions` 행과 열을 정의 하는 컬렉션입니다. 그러나 또한 몇 가지 방법이 있어야 행과 열을 표시 하는 프로그래머에 대 한 위치의 각 자식은 `Grid` 상주 합니다.

태그의 각 자식에는 `Grid` 다음과 같은 특성을 사용 하 여 해당 자식 열과 행을 지정 합니다.

-  `Grid.Row`
-  `Grid.Column`

이러한 특성의 기본값은 0입니다. 자식 하나 이상의 행 또는 이러한 특성을 가진 열에 걸쳐 있는 경우를 나타낼 수도 있습니다.

-  `Grid.RowSpan`
-  `Grid.ColumnSpan`

이 두 특성에는 1의 기본값은입니다.

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

`Grid.Row` 및 `Grid.Column` 0의 설정이 필요 하지 않지만 명확히 전달 하기 위해 일반적으로 포함 됩니다.

모양 세 플랫폼 모두에서 같습니다.

[![](essential-xaml-syntax-images/griddemo.png "격자 레이아웃")](essential-xaml-syntax-images/griddemo-large.png#lightbox "모눈 레이아웃")

구문에서 전적으로 판단 이러한 `Grid.Row`, `Grid.Column`, `Grid.RowSpan`, 및 `Grid.ColumnSpan` 정적 필드 또는 속성의 특성 표시 `Grid`, 하지만 흥미롭게도 `Grid` 명명 된 모든 항목을 정의 하지 않습니다 `Row`, `Column`, `RowSpan`, 또는 `ColumnSpan`합니다.

대신, `Grid` 라는 4 개의 바인딩 가능한 속성 정의 `RowProperty`, `ColumnProperty`, `RowSpanProperty`, 및 `ColumnSpanProperty`합니다. 특수 한 유형의 바인딩 가능한 속성으로 이들은 *연결 된 속성*합니다. 로 정의 되어는 `Grid` 클래스 하지만의 자식에 설정 된 `Grid`합니다.

연결 된 코드에서 속성을 원하는 경우이 방법을 사용 하는 `Grid` 라는 정적 메서드를 제공 하는 클래스 `SetRow`, `GetColumn`, 등입니다. XAML에서 이러한 연결 된 속성의 자식에서 특성으로 설정 되어 있지만 `Grid` 간단한 속성 이름을 사용 합니다.

연결 된 속성은 항상 인식할 수 있는 XAML 파일에서 클래스와 마침표로 구분 하는 속성 이름 모두를 포함 하는 특성으로 합니다. 호출할 *연결 된 속성* 하나의 클래스에 의해 정의 되므로 (이 경우 `Grid`) 되었지만 다른 개체에 연결 (이 경우의 자식은 `Grid`). 레이아웃 중에서 `Grid` 어디에 배치할지 각 자식에 알아야 이러한 연결 된 속성의 값을 확인 수 있습니다.

`AbsoluteLayout` 클래스 라는 두 개의 연결 된 속성을 정의 `LayoutBounds` 및 `LayoutFlags`합니다. 다음은 비례 위치를 사용 하 여 실현 체크 무늬 패턴와의 크기 조정 기능 `AbsoluteLayout`:

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

다음과 같은 항목에 대 한 XAML을 사용 하 여의 지식을 질문 수입니다. 물론, 반복 및를 `LayoutBounds` 사각형을 것 수 더 잘 실현 될 코드에서 제안 합니다.

합법적인 문제는 분명 하 고 사용자 인터페이스를 정의 하는 경우 코드와 태그를 사용한 분산 문제가 없습니다. XAML에서 시각적 개체 중 일부를 정의 하 고 다음 코드 숨김 파일의 생성자를 사용 하 여 더 잘 루프에서 발생할 수 있는 몇 가지 더 많은 시각적 개체를 추가 하는 것이 쉽습니다.

## <a name="content-properties"></a>콘텐츠 속성

앞의 예에서 `StackLayout`, `Grid`, 및 `AbsoluteLayout` 개체가으로 설정 됩니다는 `Content` 의 속성은 `ContentPage`, 고 이러한 레이아웃의 자식 항목의 항목에 실제로 `Children` 컬렉션입니다. 아직 이러한 `Content` 및 `Children` 반환할 속성은 XAML 파일에 있습니다.

확실히 포함 될 수 있습니다는 `Content` 및 `Children` 속성 등에서 같이 속성 요소로 **XamlPlusCode** 샘플:

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

문제는: 속성 요소에 이러한 이유는 *하지* XAML 파일에서 필요한?

XAML에서 사용 하기 위해 Xamarin.Forms에 정의 된 요소를 하나의 속성이 플래그가 지정 된 수의 `ContentProperty` 클래스의 특성입니다. 조회 하는 경우는 `ContentPage` 클래스는 온라인 Xamarin.Forms 설명서에이 특성이 표시 됩니다.

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

즉는 `Content` 속성 요소 태그 필요 하지 않습니다. 시작 및 끝 사이 나타나는 모든 XML 콘텐츠 `ContentPage` 태그에 할당 될 가정은 `Content` 속성입니다.

 `StackLayout``Grid`, `AbsoluteLayout`, 및 `RelativeLayout` 에서 파생 되는 모든 `Layout<View>`를 조회 하는 경우 및 `Layout<T>` Xamarin.Forms 설명서에 표시 됩니다 다른 `ContentProperty` 특성:

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

레이아웃에 자동으로 추가할의 콘텐츠를 허용 하는 `Children` 없는 컬렉션 명시적 `Children` 속성 요소 태그입니다.

다른 클래스에 `ContentProperty` 특성 정의 합니다. 예를 들어의 content 속성 `Label` 은 `Text`합니다. 다른 사용자에 대 한 API 설명서를 확인 합니다.

## <a name="platform-differences-with-onplatform"></a>OnPlatform와 플랫폼의 차이점

단일 페이지 응용 프로그램에서 설정에 공통적으로 적용 되는 `Padding` 덮어쓰지 않으려면 iOS 상태 표시줄 페이지 속성을 사용 합니다. 코드에서 사용할 수는 `Device.RuntimePlatform` 이 목적을 위해 속성:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

사용 하 여 XAML에서 유사한 에서도 할 수 있습니다는 `OnPlatform` 및 `On` 클래스입니다. 처음에 대 한 속성 요소에 포함 된 `Padding` 속성 페이지의 맨 위 근처에:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

이 태그 내를 포함 한 `OnPlatform` 태그입니다. `OnPlatform` 제네릭 클래스가입니다. 이 경우에 제네릭 형식 인수를 지정 하면 `Thickness`의 종류 `Padding` 속성입니다. 다행히은 호출 하는 제네릭 인수를 정의 하도록 특별히 XAML 특성 `x:TypeArguments`합니다. 이 설정 하 고 속성의 형식을 일치 해야 합니다.

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

`OnPlatform` 명명 된 속성이 `Platforms` 즉는 `IList` 의 `On` 개체입니다. 해당 속성에 속성 요소 태그를 사용 합니다.

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

이제 추가 `On` 요소입니다. 각 onem 설정는 `Platform` 속성 및 `Value` 속성에 대 한 태그는 `Thickness` 속성:

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

이 태그를 단순화할 수 있습니다. 콘텐츠 속성이 `OnPlatform` 은 `Platforms`이므로 이러한 속성 요소 태그를 제거할 수 있습니다.

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

`Platform` 속성의 `On` 형식이 `IList<string>`이므로 값이 동일한 경우에 여러 플랫폼을 포함할 수 있습니다.

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

Android 및 UWP의 기본 값으로 설정 되어 있으므로 `Padding`, 태그를 제거할 수 있습니다.

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

이 플랫폼 마다를 설정 하는 표준 방법 `Padding` XAML에서 속성입니다. 경우는 `Value` 단일 문자열에 의해 설정을 나타낼 수 없는에 대 한 속성 요소를 정의할 수 있습니다.

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

속성 요소 및 연결 된 속성을 사용 하 여 대부분의 기본 XAML 구문 설정 되었습니다. 그러나 때로는 해야 리소스 사전에서 예를 들어 간접는 방식으로 개체에 속성을 설정 하려면. 이 방법은 다음 부분, 부분에에서 대해서는 [3입니다. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)합니다.



## <a name="related-links"></a>관련 링크

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [1부. XAML 시작](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [3부. XAML 태그 확장](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [4부. 데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [5부. MVVM에 데이터 바인딩](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
