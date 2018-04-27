---
title: AbsoluteLayout
description: 픽셀 완벽 하 게 Ui를 만들 AbsoluteLayout를 사용 합니다.
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 110c608558059ba0f207b4cff343b428125e1784
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="absolutelayout"></a>AbsoluteLayout

[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) 배치 및 크기와 위치 또는 절대 값으로 비례 자식 요소의 크기를 지정 합니다. 위치 지정 및 크기의 비례 값 또는 정적 값을 사용 하 고 비례 자식 뷰 수 있으며 정적 값을 함께 사용할 수 있습니다.

[![](absolute-layout-images/layouts-sml.png "Xamarin.Forms 레이아웃")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms 레이아웃")

이 문서에서는 설명 합니다.

- **[용도](#Purpose)**  &ndash; 일반적인 용도 `AbsoluteLayout`합니다.
- **[사용 현황](#Usage)**  &ndash; 사용 하는 방법을 `AbsoluteLayout` 원하는 설계를 달성 하기 위해.
  - **[가변 폭 레이아웃](#Proportional_Layouts)**  &ndash; 어떻게 비례 값 이해할에서 회사는 `AbsoluteLayout`합니다.
  - **[값 지정](#Specifying_Values)**  &ndash; 비례과 절대 값을 지정 하는 방법을 이해 합니다.
  - **[비례 값](#Proportional_Values)**  &ndash; 어떻게 비례 값 이해할 작동 합니다.
    - **[절대값](#Absolute_Values)**  &ndash; 절대 값의 작동 방식을 이해 합니다.

<a name="Purpose" />

## <a name="purpose"></a>용도

위치 지정 모델로 인해 `AbsoluteLayout`, 레이아웃을 사용 하면 비교적 간단 하 게 스 레이아웃의 모든 면 아래에 맞추려면 맞춤 또는 가운데 맞춤 되도록 요소 위치를 지정 합니다. 비례 크기 및 위치,의 요소는 `AbsoluteLayout` 모든 보기 크기를 자동으로 확장할 수 있습니다. 위치만 있지만 크기가 아니라 확장 해야 하는 항목에 대 한 절대 곡선과 비례 값을 혼합할 수 있습니다.

`AbsoluteLayout` 사용할 수 있습니다 원하는 위치에 요소는 뷰 내에서 배치 해야 할 및 요소 가장자리를 정렬 하는 경우 특히 유용 합니다.

<a name="Usage" />

## <a name="usage"></a>사용법

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>가변 폭 레이아웃

`AbsoluteLayout` 에 고유한 앵커 모델이 표현의 앵커 요소의 위치은 요소를 기준으로 비례 위치를 사용 하는 요소 레이아웃을 기준으로 배치 됩니다. 절대 위치를 사용 하면 보기 내에서 앵커 (0, 0)는입니다. 여기에 두 가지 중요 한 문제가 있습니다.

- 요소 화면 비례 값을 사용 하 여 배치할 수 없습니다.
- 요소는 레이아웃의 중간 이나, 장치나 레이아웃의 크기에 관계 없이 모든 측면을 따라 안정적으로 배치 될 수 있습니다.

`AbsoluteLayout`같은 `RelativeLayout`, 겹치도록 요소 위치를 지정할 수 있습니다.

다음 스크린샷에 상자의 앵커 참고는 흰색 점입니다. 레이아웃을 통해 이동할 때 있는 앵커 부분과 상자 간의 관계를 확인 합니다.

![](absolute-layout-images/anchor-start.png "시작 시 앵커")
![](absolute-layout-images/anchor-center.png "센터 앵커")
![](absolute-layout-images/anchor-end.png "끝 앵커")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>값 지정

내에서 뷰는 `AbsoluteLayout` 4 개의 값을 사용 하 여 배치 됩니다.

- **X** &ndash; 보기의 앵커의 x (수평) 위치
- **Y** &ndash; 보기의 앵커 위치의 y (수직)
- **너비** &ndash; 보기의 너비
- **높이** &ndash; 보기의 높이

각 값으로 설정할 수는 [비례](#Proportional_Values) 값 또는 [절대](#Absolute_Values) 값입니다.

값 범위 및 플래그의 조합으로 지정 됩니다. `LayoutBounds` 이 [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) 4 개의 값으로 이루어진: `x`, `y`, `width`, `height`합니다.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/) 값은 해석 하는 방법을 지정 하 고 미리 정의 된 옵션은 다음과:

- **None** &ndash; 절대도 모든 값을 해석 합니다. 레이아웃 플래그가 지정 된 경우 이것이 기본값입니다.
- **모든** &ndash; 비례으로 모든 값을 해석 합니다.
- **WidthProportional** &ndash; 해석 하는 `Width` 절대도 값을 비례 및 다른 모든 값으로.
- **HeightProportional** &ndash; 높이 값만 비례으로 절대 다른 모든 값을 해석 합니다.
- **XProportional** &ndash; 해석 하는 `X` 절대 대로 다른 모든 값을 처리 하는 동안 값을 비례 합니다.
- **YProportional** &ndash; 해석 하는 `Y` 절대 대로 다른 모든 값을 처리 하는 동안 값을 비례 합니다.
- **PositionProportional** &ndash; 해석 하는 `X` 및 `Y` 크기 값은 절대로 해석 하는 동안 값을 비례으로 합니다.
- **SizeProportional** &ndash; 해석 하는 `Width` 및 `Height` 위치 값은 절대 값으로 비례 합니다.

레이아웃 내에서 뷰 정의의 일부로 범위 및 플래그 XAML에서 설정 사용 하는 `AbsoluteLayout.LayoutBounds` 속성입니다. 경계 값의 쉼표로 구분 된 목록으로 설정 되어 `X`, `Y`, `Width`, 및 `Height`한다는 점에서 순서입니다. 플래그 옵션도 사용 하 여 레이아웃 보기의 선언에 지정 된 된 `AbsoluteLayout.LayoutFlags` 속성입니다. 참고 쉼표로 구분 된 목록을 사용 하 여 XAML에서 플래그를 결합할 수 있습니다. 다음 예제를 참조하세요.

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

- 절대 크기와 위치 값을 사용 하 여 가운데에 레이블을 배치 됩니다. 따라서, iPhone 4S에 집중 하 고, 더 낮은 있지만 더 큰 장치에 집중 하지 나타납니다.
- 레이아웃의 맨 아래에 있는 텍스트에 비례하여 크기와 위치 값을 사용 하 여 배치 됩니다. 레이아웃의 아래쪽 가운데에 항상 표시 됩니다 있지만 레이아웃 크기가 더 큰 크기로 크기가 계속 커집니다.
- 세 명의 색이 지정 된 `BoxView`s 비례 위치와 크기를 사용 하 여 화면의 위쪽, 왼쪽 및 오른쪽 가장자리에 배치 됩니다.

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

비례 값 레이아웃과 뷰 간의 관계를 정의합니다. 이 관계의 자식 뷰 위치 또는 배율 값이 부모 레이아웃의 해당 값의 비율로 정의합니다. 이러한 값으로 표현 됩니다 `double`0과 1 사이의 값을 가진 s입니다.

비례 값 위치 및 레이아웃 내에서 크기 보기에 사용 됩니다. 따라서 보기의 너비는 한 비율로 설정 되 면 결과 너비 값은 곱한 비율은 `AbsoluteLayout`의 너비입니다. 예를 들어, 한 `AbsoluteLayout` 너비의 `500` .5, 보기의 렌더링된 된 너비의 가변 폭 사용 하도록 설정 하는 보기는 250 (500 x.5. 됩니다

비례 값을 사용 하려면 설정 `LayoutBounds` (x, y)를 사용 하 여 비율 및 비례 크기는 다음 설정 `LayoutFlags` 를 `All`합니다.

Xaml의 경우:

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

### <a name="absolute-values"></a>절대 값

절대 값 레이아웃 내에서 뷰 해야 위치 명시적으로 정의 합니다. 비례 값과는 달리 절대 값은 배치 및 레이아웃의 경계에 맞지 않는 뷰 크기 조정 수 있습니다.

절대 값을 사용 하 여 위치 지정에 대 한 레이아웃의 크기를 알 수 없는 경우 위험할 수 있습니다. 절대 위치를 사용할 때 다른 크기로 오프셋의 화면 크기에 가운데에 요소 수 없습니다. 지원 되는 장치의 다양 한 화면 크기에서 응용 프로그램을 테스트 하는 것이 유용 합니다.

레이아웃 절대 값을 사용 하려면 설정 `LayoutBounds` (x, y)를 사용 하 여 다음 설정 좌표 및 명시적 크기 `LayoutFlags` 를 `None`합니다.

Xaml의 경우:

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

사용할 수 있는 레이아웃의 각 특정 레이아웃을 만들 장단점을 포함. 이러한 일련의 레이아웃 문서 전체에서 샘플 응용 프로그램을 세 개의 다른 레이아웃을 사용 하 여 구현 동일한 페이지 레이아웃 작성 되었습니다.

다음 XAML을 고려 합니다.

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

위의 코드는 다음과 같은 레이아웃 발생합니다.

![](absolute-layout-images/abs.png "복잡 한 AbsoluteLayout")

다음에 유의 `AbsoluteLayout`경우에 따라 레이아웃에 중첩 될 수 있으므로 동일한 레이아웃 내에서 모든 요소를 제공 하는 보다 쉽게 s 중첩 됩니다.

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms에 장 14를 사용 하 여 모바일 앱 만들기](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)
- [레이아웃 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 예제 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
