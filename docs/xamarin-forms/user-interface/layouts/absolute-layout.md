---
title: Xamarin.Forms AbsoluteLayout
description: 이 문서에는 픽셀을 완벽 하 게 Ui를 만드는 Xamarin.Forms AbsoluteLayout 클래스를 사용 하는 방법을 설명 합니다. 이 클래스는 자체 크기 및 위치 또는 절대 값으로 비례 하는 자식 요소의 크기를 배치 하 고
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: e2abb37a69fc059cea499814eb48453f3bbbed72
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61078573"
---
# <a name="xamarinforms-absolutelayout"></a>Xamarin.Forms AbsoluteLayout

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 배치 하 고 자체 크기와 위치 또는 절대 값으로 비례 하는 자식 요소의 크기를 지정 합니다. 배치 및 크기에 비례 하는 값 또는 정적 값을 사용 하 여 비례 자식 뷰 수 있으며 정적 값을 함께 사용할 수 있습니다.

[![](absolute-layout-images/layouts-sml.png "Xamarin.Forms 레이아웃")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms 레이아웃")

이 문서에서는 설명 합니다.

- **[목적은](#Purpose)**  &ndash; 의 일반적인 용도 `AbsoluteLayout`합니다.
- **[사용 현황](#Usage)**  &ndash; 사용 하는 방법을 `AbsoluteLayout` 원하는 디자인을 달성 하기 위해.
  - **[가변 폭 레이아웃](#Proportional_Layouts)**  &ndash; 이해 하는 방법을 비례 값 작동는 `AbsoluteLayout`합니다.
  - **[값을 지정](#Specifying_Values)**  &ndash; 비례과 절대 값을 지정 하는 방법을 이해 합니다.
  - **[비례 값](#Proportional_Values)**  &ndash; 이해 하는 방법을 비례 값 작동 합니다.
    - **[절대값](#Absolute_Values)**  &ndash; 절대값의 작동 방식을 이해 합니다.

<a name="Purpose" />

## <a name="purpose"></a>용도

위치 지정 모델로 인해 `AbsoluteLayout`, 레이아웃을 사용 하면 비교적 간단 하 게 요소의 위치를 레이아웃에서의 모든 측면을 사용 하 여 플러시 또는 가운데 맞춤 되도록 합니다. 가변 크기, 위치에서 요소를 사용 하 여는 `AbsoluteLayout` 모든 뷰 크기를 자동으로 확장할 수 있습니다. 위치만 있지만 크기가 아니라 확장 해야 하는 항목에 대 한 절대 및 비례 값을 혼합할 수 있습니다.

`AbsoluteLayout` 사용할 수 있습니다 원격 요소 뷰 내에서 배치 해야 할 및 요소 가장자리를 정렬 하는 경우 특히 유용 합니다.

<a name="Usage" />

## <a name="usage"></a>사용법

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>가변 폭 레이아웃

`AbsoluteLayout` 에 고유한 앵커 모델이 가능해 집니다 앵커 요소의 위치는 해당 요소를 기준으로 비례 위치 지정을 사용할 때 요소 레이아웃을 기준으로 배치 됩니다. 절대 위치를 사용 하면 뷰 내에 있는 앵커 (0, 0) 됩니다. 여기에 두 가지 중요 한 문제가 있습니다.

- 비례 값을 사용 하 여 화면 밖으로 요소를 배치할 수 없습니다.
- 요소는 레이아웃의 중간 이나, 장치 또는 레이아웃의 크기에 관계 없이 모든 측면을 따라 안정적으로 배치할 수 있습니다.

`AbsoluteLayout`같은 `RelativeLayout`, 겹치도록 요소를 배치할 수 있습니다.

다음 스크린 샷에서 상자의 앵커 참고는 흰색 점입니다. 레이아웃을 거치면 서 앵커 및 상자 간의 관계를 확인 합니다.

![](absolute-layout-images/anchor-start.png "시작 시 앵커")
![](absolute-layout-images/anchor-center.png "센터에서 앵커")
![](absolute-layout-images/anchor-end.png "끝 앵커")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>값 지정

내에서 뷰는 `AbsoluteLayout` 4 개의 값을 사용 하 여 배치 됩니다.

- **X** &ndash; 뷰의 앵커의 x (수평) 위치
- **Y** &ndash; 뷰의 앵커 위치의 y (세로)
- **너비** &ndash; 뷰의 너비
- **높이** &ndash; 뷰의 높이

로 설정할 수 있습니다 각 값을 [비례](#Proportional_Values) 값 또는 [절대](#Absolute_Values) 값입니다.

값 범위 및 플래그의 조합으로 지정 됩니다. `LayoutBounds` [ `Rectangle` ](xref:Xamarin.Forms.Rectangle) 네 가지 값으로 이루어진: `x`, `y`, `width`, `height`합니다.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) 값은 해석 하는 방법을 지정 하 고 미리 정의 된 다음 옵션이 있습니다.

- **None** &ndash; 절대값으로 모든 값을 해석 합니다. 레이아웃 플래그가 없으므로 지정 하는 경우 이것이 기본값입니다.
- **모든** &ndash; 비례적으로 모든 값을 해석 합니다.
- **WidthProportional** &ndash; 해석 된 `Width` 비례 및 기타 모든 값의 절대 값입니다.
- **HeightProportional** &ndash; 만 높이 값을 비례 절대 다른 모든 값을 해석 합니다.
- **XProportional** &ndash; 해석 된 `X` 비례을 절대값으로 다른 모든 값을 처리 하는 동안 값입니다.
- **YProportional** &ndash; 해석 된 `Y` 비례을 절대값으로 다른 모든 값을 처리 하는 동안 값입니다.
- **PositionProportional** &ndash; 해석 합니다 `X` 및 `Y` 값에 비례 크기 값을 절대값으로 해석 됩니다.
- **SizeProportional** &ndash; 해석 합니다 `Width` 및 `Height` 위치 값은 절대 비례으로 값입니다.

XAML, 범위 및 플래그는 설정 레이아웃 내에서 뷰 정의의 일부로 사용 하 여는 `AbsoluteLayout.LayoutBounds` 속성입니다. 범위 값을 쉼표로 구분 된 목록으로 설정 됩니다 `X`, `Y`를 `Width`, 및 `Height`를 주문 합니다. 플래그 옵션도 사용 하 여 레이아웃 보기의 선언에 지정 된 된 `AbsoluteLayout.LayoutFlags` 속성입니다. 참고 쉼표로 구분 된 목록을 사용 하 여 XAML에서 플래그를 조합할 수 있습니다. 다음 예제를 참조하세요.

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

![](absolute-layout-images/exploration.png "AbsoluteLayout 예제")

다음 사항에 유의하십시오.

- 가운데 레이블 절대 크기와 위치 값을 사용 하 여 배치 됩니다. 따라서 iPhone 4 초에 집중 하 고 아래쪽에 있지만 큰 장치에 집중 하지 나타납니다.
- 텍스트 레이아웃의 맨 아래에 비례하여 크기 및 위치 값을 사용 하 여 배치 됩니다. 레이아웃의 아래쪽 가운데에 항상 표시 됩니다 있지만 더 큰 레이아웃 크기를 사용 하 여 해당 크기는 증가 합니다.
- 세 가지 컬러 `BoxView`s 비례 위치 및 절대 크기를 사용 하 여 화면의 위쪽, 왼쪽 및 오른쪽 가장자리에 배치 됩니다.

다음 C#에서 동일한 레이아웃을 얻을 수 있습니다:

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
<a name="Proportional_Values" />

### <a name="proportional-values"></a>비례 값

비례 값 레이아웃 및 뷰 간의 관계를 정의합니다. 이 관계를 부모 레이아웃의 해당 값의 비율로 자식 뷰 위치 또는 크기 조정 값을 정의합니다. 이러한 값으로 표현 됩니다 `double`0과 1 사이의 값을 가진 s입니다.

비례 값 위치 및 레이아웃 내에서 크기 보기에 사용 됩니다. 따라서 보기의 너비를 비율로 설정 되 면 결과 너비 값은 곱한 비율을 `AbsoluteLayout`의 너비입니다. 예를 들어를 `AbsoluteLayout` 너비의 `500` 보기의 렌더링된 된 너비,.5 비례 너비가 설정 보기는 250 (500 x.5. 됩니다

비례 값을 사용 하려면 설정 `LayoutBounds` (x, y)를 사용 하 여 비율 및 비례하여 크기를 설정한 `LayoutFlags` 에 `All`입니다.

XAML:

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

<a name="Absolute_Values" />

### <a name="absolute-values"></a>절대값

절대값 레이아웃 내에서 뷰를 배치 하도록 명시적으로 정의 합니다. 비례 값과는 달리 절대 값은 위치 및 레이아웃의 범위에 맞지 않는 뷰 크기를 조정할 수 있습니다.

절대 값을 사용 하 여 위치 지정에 대 한 레이아웃의 크기를 알 수 없는 경우 위험할 수 있습니다. 절대 위치를 사용 하는 경우 다른 크기로 오프셋 한 크기로 화면의 가운데에서 요소 수 없습니다. 지원 되는 장치의 다양 한 화면 크기에서 앱을 테스트 하는 것이 반드시 합니다.

절대 레이아웃 값을 사용 하려면 설정 `LayoutBounds` (x, y)를 사용 하 여 좌표 및 명시적 크기 설정한 `LayoutFlags` 에 `None`입니다.

XAML:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

C#:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>복잡 한 레이아웃을 탐색합니다.

각 레이아웃의 강점 및 약점 특정 레이아웃을 만들에 있습니다. 이 문서 시리즈에서는 레이아웃을 전체 세 가지 다른 레이아웃을 사용 하 여 구현 동일한 페이지 레이아웃을 사용 하 여 샘플 앱을 만들었습니다.

다음 XAML을 고려해 야 합니다.

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

위 코드 다음의 레이아웃 발생합니다.

![](absolute-layout-images/abs.png "복잡 한 AbsoluteLayout")

`AbsoluteLayout`s 중첩 된 경우에 따라 레이아웃 중첩 수 있으므로 동일한 레이아웃 내에서 모든 요소를 제공 하는 것 보다 쉽습니다.

## <a name="related-links"></a>관련 링크

- [14 장 Xamarin.Forms를 사용 하 여 모바일 앱 만들기](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](xref:Xamarin.Forms.AbsoluteLayout)
- [레이아웃 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 예제 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
