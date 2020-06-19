---
title: Xamarin.FormsStackLayout
description: StackLayout은 1 차원 스택의 자식 뷰를 가로 또는 세로로 구성 합니다.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/11/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f624674cc6d4ba1bdc34a42fb52fb63ff8a7135a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137971"
---
# <a name="xamarinforms-stacklayout"></a>Xamarin.FormsStackLayout

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stacklayoutdemos)

[![Xamarin.FormsStackLayout](stacklayout-images/layouts.png "[! OP. NO-LOC (Xamarin.ios)] StackLayout")](stacklayout-images/layouts-large.png#lightbox "[! OP. NO-LOC (Xamarin.ios)] StackLayout")

는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 1 차원 스택의 자식 뷰를 가로 또는 세로로 구성 합니다. 기본적으로는 `StackLayout` 세로 방향입니다. 또한 `StackLayout` 다른 자식 레이아웃을 포함 하는 부모 레이아웃으로를 사용할 수 있습니다.

[`StackLayout`](xref:Xamarin.Forms.StackLayout)클래스는 다음 속성을 정의 합니다.

- [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)형식의는 [`StackOrientation`](xref:Xamarin.Forms.StackOrientation) 자식 뷰가 배치 되는 방향을 나타냅니다. 이 속성의 기본값은 `Vertical`입니다.
- [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)형식의는 `double` 각 자식 뷰 사이의 공간 크기를 나타냅니다. 이 속성의 기본값은 6 개의 장치 독립적 단위입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 속성이 데이터 바인딩 및 스타일의 대상이 될 수 있습니다.

[`StackLayout`](xref:Xamarin.Forms.StackLayout)클래스는 `Layout<T>` 형식의 속성을 정의 하는 클래스에서 파생 `Children` `IList<T>` 됩니다. `Children`속성은 `ContentProperty` 클래스의입니다 `Layout<T>` . 따라서 XAML에서 명시적으로 설정할 필요는 없습니다.

> [!TIP]
> 가능한 최상의 레이아웃 성능을 얻으려면 [레이아웃 성능 최적화](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)의 지침을 따르세요.

## <a name="vertical-orientation"></a>세로 방향

다음 XAML은 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 다른 자식 뷰를 포함 하는 세로 지향적을 만드는 방법을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.VerticalStackLayoutPage"
             Title="Vertical StackLayout demo">
    <StackLayout Margin="20">
        <Label Text="Primary colors" />
        <BoxView Color="Red" />
        <BoxView Color="Yellow" />
        <BoxView Color="Blue" />
        <Label Text="Secondary colors" />
        <BoxView Color="Green" />
        <BoxView Color="Orange" />
        <BoxView Color="Purple" />
    </StackLayout>
</ContentPage>
```

이 예제에서는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 및 개체가 포함 된 세로를 만듭니다 [`Label`](xref:Xamarin.Forms.Label) [`BoxView`](xref:Xamarin.Forms.BoxView) . 기본적으로 자식 보기 사이에는 6 개의 장치 독립적 공간 단위가 있습니다.

[![세로 방향 StackLayout의 스크린샷](stacklayout-images/vertical.png "세로 방향 StackLayout")](stacklayout-images/vertical-large.png#lightbox "세로 방향 StackLayout")

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class VerticalStackLayoutPageCS : ContentPage
{
    public VerticalStackLayoutPageCS()
    {
        Title = "Vertical StackLayout demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children =
            {
                new Label { Text = "Primary colors" },
                new BoxView { Color = Color.Red },
                new BoxView { Color = Color.Yellow },
                new BoxView { Color = Color.Blue },
                new Label { Text = "Secondary colors" },
                new BoxView { Color = Color.Green },
                new BoxView { Color = Color.Orange },
                new BoxView { Color = Color.Purple }
            }
        };
    }
}
```

> [!NOTE]
> 속성의 값은 [`Margin`](xref:Xamarin.Forms.View.Margin) 요소와 인접 요소 사이의 거리를 나타냅니다. 자세한 내용은 [여백 및 패딩](margin-and-padding.md)을 참조하세요.

## <a name="horizontal-orientation"></a>가로 방향

다음 XAML은 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 속성을로 설정 하 여 가로 방향으로을 만드는 방법을 보여 줍니다 [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) `Horizontal` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.HorizontalStackLayoutPage"
             Title="Horizontal StackLayout demo">
    <StackLayout Margin="20"
                 Orientation="Horizontal"
                 HorizontalOptions="Center">
        <BoxView Color="Red" />
        <BoxView Color="Yellow" />
        <BoxView Color="Blue" />
        <BoxView Color="Green" />
        <BoxView Color="Orange" />
        <BoxView Color="Purple" />
    </StackLayout>
</ContentPage>
```

이 예제에서는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`BoxView`](xref:Xamarin.Forms.BoxView) 자식 뷰 사이에 6 개의 장치 독립적 공간 단위가 있는 가로 포함 개체를 만듭니다.

[![가로 방향 StackLayout의 스크린샷](stacklayout-images/horizontal.png "가로 방향 StackLayout")](stacklayout-images/horizontal-large.png#lightbox "가로 방향 StackLayout")

해당하는 C# 코드는 다음과 같습니다.

```csharp
public HorizontalStackLayoutPageCS()
{
    Title = "Horizontal StackLayout demo";
    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Orientation = StackOrientation.Horizontal,
        HorizontalOptions = LayoutOptions.Center,
        Children =
        {
            new BoxView { Color = Color.Red },
            new BoxView { Color = Color.Yellow },
            new BoxView { Color = Color.Blue },
            new BoxView { Color = Color.Green },
            new BoxView { Color = Color.Orange },
            new BoxView { Color = Color.Purple }
        }
    };
}
```

## <a name="space-between-child-views"></a>자식 뷰 간 간격

[`StackLayout`](xref:Xamarin.Forms.StackLayout)속성을 값으로 설정 하 여의 자식 뷰 간 간격을 변경할 수 있습니다 [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) `double` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.StackLayoutSpacingPage"
             Title="StackLayout Spacing demo">
    <StackLayout Margin="20"
                 Spacing="0">
        <Label Text="Primary colors" />
        <BoxView Color="Red" />
        <BoxView Color="Yellow" />
        <BoxView Color="Blue" />
        <Label Text="Secondary colors" />
        <BoxView Color="Green" />
        <BoxView Color="Orange" />
        <BoxView Color="Purple" />
    </StackLayout>
</ContentPage>
```

이 예제에서는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 및 개체가 포함 된 세로를 만듭니다 [`Label`](xref:Xamarin.Forms.Label) [`BoxView`](xref:Xamarin.Forms.BoxView) .

[![간격이 없는 StackLayout의 스크린샷](stacklayout-images/spacing.png "공백이 없는 StackLayout")](stacklayout-images/spacing-large.png#lightbox "공백이 없는 StackLayout")

> [!TIP]
> [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)속성을 음수 값으로 설정 하 여 자식 보기가 겹칠 수 있습니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class StackLayoutSpacingPageCS : ContentPage
{
    public StackLayoutSpacingPageCS()
    {
        Title = "StackLayout Spacing demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Spacing = 0,
            Children =
            {
                new Label { Text = "Primary colors" },
                new BoxView { Color = Color.Red },
                new BoxView { Color = Color.Yellow },
                new BoxView { Color = Color.Blue },
                new Label { Text = "Secondary colors" },
                new BoxView { Color = Color.Green },
                new BoxView { Color = Color.Orange },
                new BoxView { Color = Color.Purple }
            }
        };
    }
}
```

## <a name="position-and-size-of-child-views"></a>자식 뷰의 위치 및 크기

내에서 자식 뷰의 크기와 위치는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 자식 뷰의 값과 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성 및 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 해당 및 속성의 값에 따라 달라 집니다 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) . 세로 방향의 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 자식 보기는 크기가 명시적으로 설정 되지 않은 경우 사용 가능한 너비를 채우도록 확장 됩니다. 마찬가지로, 가로에서 `StackLayout` 자식 뷰는 크기가 명시적으로 설정 되지 않은 경우 사용 가능한 높이를 채우도록 확장 됩니다.

및 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 자식 뷰의 및 속성은 [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 두 가지 레이아웃 기본 설정을 캡슐화 하는 구조체의 필드로 설정할 수 있습니다.

- *맞춤* 부모 레이아웃 내에서 자식 뷰의 위치와 크기를 결정 합니다.
- *확장* 은 자식 뷰에서 사용할 수 있는 경우 추가 공간을 사용 해야 하는지 여부를 나타냅니다.

> [!TIP]
> [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 필요 하지 않은 경우의 및 속성을 설정 하지 마세요 [`StackLayout`](xref:Xamarin.Forms.StackLayout) . `LayoutOptions.Fill` 및 `LayoutOptions.FillAndExpand`의 기본값은 최고의 레이아웃 최적화를 지원합니다. 이러한 속성을 변경 하면 해당 속성을 기본값으로 다시 설정 하는 경우에도 비용이 발생 하며 메모리가 소비 됩니다.

### <a name="alignment"></a>맞춤

다음 XAML 예제에서는의 각 자식 뷰에서 맞춤 기본 설정을 지정 합니다 [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.AlignmentPage"
             Title="Alignment demo">
    <StackLayout Margin="20">
        <Label Text="Start"
               BackgroundColor="Gray"
               HorizontalOptions="Start" />
        <Label Text="Center"
               BackgroundColor="Gray"
               HorizontalOptions="Center" />
        <Label Text="End"
               BackgroundColor="Gray"
               HorizontalOptions="End" />
        <Label Text="Fill"
               BackgroundColor="Gray"
               HorizontalOptions="Fill" />
    </StackLayout>
</ContentPage>
```

이 예제에서 맞춤 기본 설정은 개체에 대해 설정 되어 [`Label`](xref:Xamarin.Forms.Label) 내에서의 위치를 제어 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 합니다. [`Start`](xref:Xamarin.Forms.LayoutOptions.Start),, [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) [`End`](xref:Xamarin.Forms.LayoutOptions.End) 및 필드는 [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) [`Label`](xref:Xamarin.Forms.Label) 부모 내에서 개체의 맞춤을 정의 하는 데 사용 됩니다 `StackLayout` .

[![맞춤 옵션이 설정 된 StackLayout의 스크린샷](stacklayout-images/alignment.png "맞춤 옵션이 있는 StackLayout")](stacklayout-images/alignment-large.png#lightbox "맞춤 옵션이 있는 StackLayout")

[`StackLayout`](xref:Xamarin.Forms.StackLayout)은 `StackLayout` 방향과 반대 방향에 있는 자식 뷰의 맞춤 기본 설정을 따릅니다. 따라서 세로 방향 `StackLayout` 내의 [`Label`](xref:Xamarin.Forms.Label) 자식 뷰는 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 속성을 맞춤 필드 중 하나로 설정합니다.

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)는의 왼쪽에를 배치 합니다 [`Label`](xref:Xamarin.Forms.Label) [`StackLayout`](xref:Xamarin.Forms.StackLayout) .
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 [`Label`](xref:Xamarin.Forms.Label)을 집중시킵니다.
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)는의 오른쪽에를 배치 합니다 [`Label`](xref:Xamarin.Forms.Label) [`StackLayout`](xref:Xamarin.Forms.StackLayout) .
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)은 [`Label`](xref:Xamarin.Forms.Label)이 [`StackLayout`](xref:Xamarin.Forms.StackLayout)의 너비를 채웁니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class AlignmentPageCS : ContentPage
{
    public AlignmentPageCS()
    {
        Title = "Alignment demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children =
            {
                new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
                new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
                new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
                new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
            }
        };
    }
}
```

### <a name="expansion"></a>확장

다음 XAML 예제에서는의 각에 대 한 확장 기본 설정을 지정 합니다 [`Label`](xref:Xamarin.Forms.Label) [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.ExpansionPage"
             Title="Expansion demo">
    <StackLayout Margin="20">
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="Start"
               BackgroundColor="Gray"
               VerticalOptions="StartAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="Center"
               BackgroundColor="Gray"
               VerticalOptions="CenterAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="End"
               BackgroundColor="Gray"
               VerticalOptions="EndAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="Fill"
               BackgroundColor="Gray"
               VerticalOptions="FillAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
    </StackLayout>
</ContentPage>
```

이 예제에서 확장 기본 설정은 개체에 대해 설정 되어 [`Label`](xref:Xamarin.Forms.Label) 내에서 크기를 제어 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 합니다. [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand), [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand) , [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand) 및 필드는 [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) 맞춤 기본 설정을 정의 하는 데 사용 되 고, `Label` 부모 내에서 사용할 수 있는 경우가 더 많은 공간을 차지 하는지 여부를 정의 하는 데 사용 됩니다 `StackLayout` .

[![확장 옵션이 설정 된 StackLayout의 스크린샷](stacklayout-images/expansion.png "확장 옵션이 있는 StackLayout")](stacklayout-images/expansion-large.png#lightbox "확장 옵션이 있는 StackLayout")

[`StackLayout`](xref:Xamarin.Forms.StackLayout)은 해당 방향으로만 자식 뷰를 확장할 수 있습니다. 따라서 세로 방향 `StackLayout`은 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 확장 필드 중 하나로 설정하는 [`Label`](xref:Xamarin.Forms.Label) 자식 뷰를 확장할 수 있습니다. 즉, 세로 맞춤의 경우 각 `Label`은 `StackLayout` 내의 동일한 공간을 차지합니다. 그러나 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)로 설정하는 최종 `Label`만 크기가 다릅니다.

> [!TIP]
> 을 사용 하는 경우 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 하나의 자식 보기만로 설정 되었는지 확인 [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) 합니다. 이 속성은 지정된 자식 요소가 `StackLayout`에서 허용하는 최대의 공간을 차지하도록 하며, 이는 이러한 계산이 두 번 이상 수행되도록 하여 불필요합니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
public ExpansionPageCS()
{
    Title = "Expansion demo";
    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children =
        {
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
        }
    };
}
```

> [!IMPORTANT]
> [`StackLayout`](xref:Xamarin.Forms.StackLayout)의 모든 공간을 사용할 때 확장 기본 설정은 아무런 영향을 미치지 않습니다.

맞춤 및 확장에 대 한 자세한 내용은 [ Xamarin.Forms 의 레이아웃 옵션 ](layout-options.md)을 참조 하십시오.

## <a name="nested-stacklayout-objects"></a>중첩 된 StackLayout 개체

는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 중첩 된 자식 `StackLayout` 개체 또는 다른 자식 레이아웃을 포함 하는 부모 레이아웃으로 사용할 수 있습니다.

다음 XAML에서는 중첩 개체의 예를 보여 줍니다 [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.CombinedStackLayoutPage"
             Title="Combined StackLayouts demo">
    <StackLayout Margin="20">
        ...
        <Frame BorderColor="Black"
               Padding="5">
            <StackLayout Orientation="Horizontal"
                         Spacing="15">
                <BoxView Color="Red" />
                <Label Text="Red"
                       FontSize="Large"
                       VerticalOptions="Center" />
            </StackLayout>
        </Frame>
        <Frame BorderColor="Black"
               Padding="5">
            <StackLayout Orientation="Horizontal"
                         Spacing="15">
                <BoxView Color="Yellow" />
                <Label Text="Yellow"
                       FontSize="Large"
                       VerticalOptions="Center" />
            </StackLayout>
        </Frame>
        <Frame BorderColor="Black"
               Padding="5">
            <StackLayout Orientation="Horizontal"
                         Spacing="15">
                <BoxView Color="Blue" />
                <Label Text="Blue"
                       FontSize="Large"
                       VerticalOptions="Center" />
            </StackLayout>
        </Frame>
        ...
    </StackLayout>
</ContentPage>
```

이 예제에서 부모는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 개체 내에 중첩 된 개체를 포함 `StackLayout` [`Frame`](xref:Xamarin.Forms.Frame) 합니다. 부모는 `StackLayout` 세로 방향 이지만 자식 개체는 가로 방향입니다 `StackLayout` .

[![중첩 된 StackLayout 개체의 스크린샷](stacklayout-images/combined.png "중첩 된 StackLayouts")](stacklayout-images/combined-large.png#lightbox "중첩 된 StackLayouts")

> [!IMPORTANT]
> [`StackLayout`](xref:Xamarin.Forms.StackLayout)개체와 기타 레이아웃을 더 깊게 중첩할 수록 중첩 된 레이아웃은 성능에 영향을 줍니다. 자세한 내용은 [올바른 레이아웃 선택](~/xamarin-forms/deploy-test/performance.md#choose-the-correct-layout)을 참조 하세요.

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class CombinedStackLayoutPageCS : ContentPage
{
    public CombinedStackLayoutPageCS()
    {
        Title = "Combined StackLayouts demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children =
            {
                new Label { Text = "Primary colors" },
                new Frame
                {
                    BorderColor = Color.Black,
                    Padding = new Thickness(5),
                    Content = new StackLayout
                    {
                        Orientation = StackOrientation.Horizontal,
                        Spacing = 15,
                        Children =
                        {
                            new BoxView { Color = Color.Red },
                            new Label { Text = "Red", FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), VerticalOptions = LayoutOptions.Center }
                        }
                    }
                },
                new Frame
                {
                    BorderColor = Color.Black,
                    Padding = new Thickness(5),
                    Content = new StackLayout
                    {
                        Orientation = StackOrientation.Horizontal,
                        Spacing = 15,
                        Children =
                        {
                            new BoxView { Color = Color.Yellow },
                            new Label { Text = "Yellow", FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), VerticalOptions = LayoutOptions.Center }
                        }
                    }
                },
                new Frame
                {
                    BorderColor = Color.Black,
                    Padding = new Thickness(5),
                    Content = new StackLayout
                    {
                        Orientation = StackOrientation.Horizontal,
                        Spacing = 15,
                        Children =
                        {
                            new BoxView { Color = Color.Blue },
                            new Label { Text = "Blue", FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), VerticalOptions = LayoutOptions.Center }
                        }
                    }
                },
                // ...
            }
        };
    }
}
```

## <a name="related-links"></a>관련 링크

- [StackLayout 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stacklayoutdemos)
- [의 레이아웃 옵션Xamarin.Forms](layout-options.md)
- [레이아웃 선택 Xamarin.Forms](choose-layout.md)
- [Xamarin.Forms앱 성능 향상](~/xamarin-forms/deploy-test/performance.md)
