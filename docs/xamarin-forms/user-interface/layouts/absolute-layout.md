---
title: Xamarin.FormsAbsoluteLayout
description: 이 문서에서는 AbsoluteLayout 클래스를 사용 하 여 픽셀에 맞는 Ui를 만드는 방법을 설명 합니다 Xamarin.Forms . 이 클래스는 자식 요소를 고유한 크기와 위치에 비례 하거나 절대 값에 비례하여 배치 및 크기를 지정 합니다.
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 110b01d6482fbe3e23a772c90194b6bf40d54877
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84570533"
---
# <a name="xamarinforms-absolutelayout"></a>Xamarin.Forms AbsoluteLayout

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)위치 및 크기 자식 요소는 자체 크기와 위치 또는 절대 값에 비례 합니다. 비례 값 이나 정적 값을 사용하여 자식 뷰 위치를 지정하고 크기를 조정할 수 있으며, 비례 및 정적 값은 혼합할 수 있습니다.

[![](absolute-layout-images/layouts-sml.png "Xamarin.Forms Layouts")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms Layouts")

이 문서에서는 다음을 설명합니다.

- **[목적](#purpose)** `AbsoluteLayout`의 일반적인 용도.
- **[사용](#usage)** `AbsoluteLayout`을 사용하여 원하는 디자인을 구현하는 방법.
  - **[비례 레이아웃](#proportional-layouts)** `AbsoluteLayout`에서 비례 값이 작동 하는 방식을 이해.
  - **[값 지정](#specifying-values)** &ndash; 비례 및 절대값을 지정 하는 방법을 이해.
  - **[비례값](#proportional-values)** &ndash; 비례 값의 작동 방식을 이해.
    - **[절대값](#absolute-values)** &ndash; 절대값이 어떻게 작동 하는지 이해.

## <a name="purpose"></a>목적

`AbsoluteLayout`의 위치 지정 모델 때문에 레이아웃을 사용하면 레이아웃의 어느 쪽이나 가운데 맞춤을 사용하여 요소를 쉽게 배치할 수 있습니다. 비례 크기와 위치를 사용하여 `AbsoluteLayout`의 요소는 보기 크기에 맞게 자동으로 확장 될 수 있습니다. 크기를 제외하고 위치만 을 조정해야 하는 항목의 경우에는 절대값 및 비례 값을 혼합할 수 있습니다.

`AbsoluteLayout`은 뷰 내에서 요소를 배치 해야 하는 모든 위치에서 사용할 수 있으며, 요소를 가장자리에 맞출 때 특히 유용합니다.

## <a name="usage"></a>사용

### <a name="proportional-layouts"></a>비례 레이아웃

`AbsoluteLayout`에는 비례 배치를 사용할 때 요소를 레이아웃에 상대적으로 배치할 때 요소의 앵커를 요소에 상대적으로 배치하는 고유한 앵커 모델이 있습니다. 절대 위치 지정을 사용하는 경우 앵커는 뷰 내의 (0, 0)에 있습니다. 여기에는 두 가지 중요한 결과가 있습니다.

- 가변 값을 사용하여 요소를 화면에서 놓을 수 없습니다.
- 레이아웃 또는 장치의 크기에 관계 없이 요소를 레이아웃 또는 가운데에 따라 안정적으로 배치할 수 있습니다.

`AbsoluteLayout`와 마찬가지로 `RelativeLayout` 는 요소가 겹칠 수 있도록 요소를 배치할 수 있습니다.

다음 스크린샷에서 상자의 앵커는 흰색 점입니다. 레이아웃을 이동할 때 앵커와 상자 간의 관계를 확인하세요.

![](absolute-layout-images/anchor-start.png "Anchor at Start")
![](absolute-layout-images/anchor-center.png "Anchor at Center")
![](absolute-layout-images/anchor-end.png "Anchor at End")

### <a name="specifying-values"></a>값 지정

`AbsoluteLayout` 내의 뷰는 다음 네 가지 값을 사용하여 배치 됩니다.

- **X** &ndash; 뷰 앵커의 x (가로) 위치
- **Y** &ndash; 뷰 앵커의 y (세로) 위치
- **너비** &ndash; 뷰의 너비입니다.
- **높이** &ndash; 뷰의 높이입니다.

이러한 각 값은 [비례값](#proportional-values) 또는 [절대값](#absolute-values) 으로 설정할 수 있습니다.

값은 범위와 플래그의 조합으로 지정 됩니다. `LayoutBounds`는 네 가지 값: `x`, `y`, `width`, `height` 으로 구성된 [`Rectangle`](xref:Xamarin.Forms.Rectangle) 입니다.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags)은 값을 해석하는 방법을 지정하고 다음과 같은 미리 정의된 옵션을 포함합니다.

- **None** &ndash; 모든 값을 절대 값으로 해석합니다. 레이아웃 플래그가 지정되지 않은 경우이 값이 기본값입니다.
- **All** &ndash; 모든 값을 비례적으로 해석합니다.
- **WidthProportional** &ndash; `Width` 값을 비례로 해석하고 다른 모든 값은 절대값으로 해석합니다.
- **HeightProportional** &ndash; 높이 값만 다른 모든 값의 절대값으로 해석합니다.
- **Xproportional** &ndash; `X`다른 모든 값을 절대값으로 처리하는 동안 값을 비례로 해석합니다.
- **Yproportional** &ndash; `Y`다른 모든 값을 절대값으로 처리하는 동안 값을 비례로 해석합니다.
- **Positionproportional** &ndash; 는 `X` 및 `Y` 값을 비례적으로 해석하는 반면 크기 값은 절대값으로 해석됩니다.
- **Sizeproportional** &ndash; 는 `Width` 및 `Height` 값을 비례 하는 반면, 위치 값은 절대값으로 해석합니다.

XAML에서 범위 및 플래그는 `AbsoluteLayout.LayoutBounds` 속성을 사용하여 레이아웃 내의 뷰 정의의 일부로 설정됩니다. 범위는 쉼표로 구분된 `X`, `Y`, `Width`, `Height`의 순서 목록으로 설정됩니다. 플래그는 `AbsoluteLayout.LayoutFlags` 속성을 사용하여 레이아웃의 뷰 선언에도 지정됩니다. 플래그는 쉼표로 구분된 목록을 사용하여 XAML에서 결합할 수 있습니다. 다음 예제를 참조하세요.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.AbsoluteLayoutExploration"
Title="Absolute Layout Exploration">
    <ContentPage.Content>
        <AbsoluteLayout>
            <Label Text="I'm centered on iPhone 4 but no other device"
                AbsoluteLayout.LayoutBounds="115,150,100,100" LineBreakMode="WordWrap"  />
            <Label Text="I'm bottom center on every device."
                AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"
                LineBreakMode="WordWrap"  />
            <BoxView Color="Olive"  AbsoluteLayout.LayoutBounds="1,.5, 25, 100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Red" AbsoluteLayout.LayoutBounds="0,.5,25,100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Blue" AbsoluteLayout.LayoutBounds=".5,0,100,25"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

![](absolute-layout-images/exploration.png "AbsoluteLayout Examples")

다음 사항에 유의하세요.

- 가운데의 레이블은 절대 크기 및 위치 값을 사용하여 배치됩니다. 이로 인해 iPhone 4S 및 하위에 중앙에 표시 되지만 큰 장치에는 중앙에 표시 되지 않습니다.
- 레이아웃의 아래쪽에 있는 텍스트는 비례 크기 및 위치 값을 사용하여 배치됩니다. 이는 레이아웃의 아래쪽 가운데에 항상 표시 되지만 크기가 클수록 레이아웃 크기가 증가합니다.
- 3 가지 색의 `BoxView`가 비례 위치 및 절대 크기를 사용하여 화면의 위쪽, 왼쪽 및 오른쪽 가장자리에 배치됩니다.

다음은 C#에서 동일한 레이아웃을 나타냅니다.

```csharp
public class AbsoluteLayoutExplorationCode : ContentPage
{
    public AbsoluteLayoutExplorationCode ()
    {
        Title = "Absolute Layout Exploration - Code";
        var layout = new AbsoluteLayout();

        var centerLabel = new Label {
        Text = "I'm centered on iPhone 4 but no other device.",
        LineBreakMode = LineBreakMode.WordWrap};

        AbsoluteLayout.SetLayoutBounds (centerLabel, new Rectangle (115, 159, 100, 100));
        // No need to set layout flags, absolute positioning is the default

        var bottomLabel = new Label { Text = "I'm bottom center on every device.", LineBreakMode = LineBreakMode.WordWrap };
        AbsoluteLayout.SetLayoutBounds (bottomLabel, new Rectangle (.5, 1, .5, .1));
        AbsoluteLayout.SetLayoutFlags (bottomLabel, AbsoluteLayoutFlags.All);

        var rightBox = new BoxView{ Color = Color.Olive };
        AbsoluteLayout.SetLayoutBounds (rightBox, new Rectangle (1, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (rightBox, AbsoluteLayoutFlags.PositionProportional);

        var leftBox = new BoxView{ Color = Color.Red };
        AbsoluteLayout.SetLayoutBounds (leftBox, new Rectangle (0, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (leftBox, AbsoluteLayoutFlags.PositionProportional);

        var topBox = new BoxView{ Color = Color.Blue };
        AbsoluteLayout.SetLayoutBounds (topBox, new Rectangle (.5, 0, 100, 25));
        AbsoluteLayout.SetLayoutFlags (topBox, AbsoluteLayoutFlags.PositionProportional);

        layout.Children.Add (bottomLabel);
        layout.Children.Add (centerLabel);
        layout.Children.Add (rightBox);
        layout.Children.Add (leftBox);
        layout.Children.Add (topBox);

        Content = layout;
    }
}
```

### <a name="proportional-values"></a>비례 값

비례 값은 레이아웃과 뷰 간의 관계를 정의 합니다. 이 관계는 자식 뷰의 위치 또는 배율 값을 부모 레이아웃의 해당 값에 대한 비율로 정의 합니다. 이러한 값은 `double` 0에서 1 사이의 값으로 표현됩니다.

비례 값은 레이아웃 내에서 보기를 배치하고 크기를 조정하는 데 사용 됩니다. 따라서 보기의 너비가 비율로 설정된 경우 결과 너비 값은 비율에 `AbsoluteLayout`의 너비를 곱한 값입니다. 예를 들어 너비 `500`의 `AbsoluteLayout` 너비와 비례 너비가 0.5 인 뷰 집합을 사용하는 경우 렌더링 된 뷰의 너비는 250 (500 x. 5)가 됩니다.

가변 값을 사용하려면 `LayoutBounds` (x, y) 비율 및 비례 크기를 사용하여 설정하고 `LayoutFlags` 를 `All`로 설정합니다.

XAML에서:

```xaml
<Label Text="I'm bottom center on every device."
  AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"  />
```

C#:

```csharp
var label = new Label {Text = "I'm bottom center on every device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(.5,1,.5,.1));
AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.All);
```

### <a name="absolute-values"></a>절대값

절대값은 레이아웃 내에서 뷰 위치를 지정하는 위치를 명시적으로 정의합니다. 가변 값과 달리 절대값은 레이아웃의 범위 내에 포함되지 않는 뷰의 위치를 지정하고 크기를 조정할 수 있습니다.

레이아웃의 크기를 알 수 없는 경우 위치 지정에 절대값을 사용하면 위험할 수 있습니다. 절대 위치를 사용하는 경우 화면 가운데에서 한 크기의 요소가 다른 크기에서 오프셋 될 수 있습니다. 지원되는 장치의 다양한 화면 크기에서 앱을 테스트 하는 것이 중요합니다.

절대 레이아웃 값을 사용하려면 `LayoutBounds` (x, y) 좌표 및 명시적 크기를 사용하여 설정하고 `LayoutFlags` 를 `None`로 설정합니다.

XAML에서:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

C#:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>복잡 한 레이아웃 탐색

각 레이아웃에는 특정 레이아웃을 만들기 위한 장점 및 단점이 있습니다. 이 일련의 레이아웃 문서에서 샘플 앱은 세 가지 다른 레이아웃을 사용 하 여 구현된 동일한 페이지 레이아웃을 사용하여 만들어졌습니다.

다음 XAML을 참조 하세요.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.AbsoluteLayoutPage"
Title="AbsoluteLayout">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Save" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <ScrollView>
            <AbsoluteLayout BackgroundColor="Maroon">
                <BoxView BackgroundColor="Gray" AbsoluteLayout.LayoutBounds="0
                    0,1,100" AbsoluteLayout.LayoutFlags="XProportional,YProportional,WidthProportional" />
                <Button BackgroundColor="Maroon"
                    AbsoluteLayout.LayoutBounds=".5,55,70,70" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="35" />
                <Button BackgroundColor="Red" AbsoluteLayout.LayoutBounds=".5
                    60,60,60" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="30" />
                <Label Text="User Name" FontAttributes="Bold" FontSize="26"
                    TextColor="Black" HorizontalTextAlignment="Center" AbsoluteLayout.LayoutBounds=".5,140,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <Entry Text="Bio + Hashtags" TextColor="White"
                    BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds=".5,180,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <AbsoluteLayout BackgroundColor="White"
                        AbsoluteLayout.LayoutBounds="0, 220, 1, 50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional">
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional">
                        <Button BackgroundColor="Black" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Accent Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="1,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional,XProportional">
                        <Button BackgroundColor="Maroon" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Primary Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,270,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Age:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
                        AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,320,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Interests:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin.Forms" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,370,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Ask me about:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
            </AbsoluteLayout>
        </ScrollView>
    </ContentPage.Content>
</ContentPage>
```

위의 코드는 다음과 같은 레이아웃을 생성합니다.

![](absolute-layout-images/abs.png "Complex AbsoluteLayout")

일부 경우에는 동일한 레이아웃 내에 모든 요소를 표시하는 것 보다 중첩 된 레이아웃을 사용하는 것이 쉽기 때문에 `AbsoluteLayout`이 중첩 되어 있습니다.

## <a name="related-links"></a>관련 링크

- [을 사용 하 여 Mobile Apps 만들기 Xamarin.Forms , 14 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](xref:Xamarin.Forms.AbsoluteLayout)
- [레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 예제 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
