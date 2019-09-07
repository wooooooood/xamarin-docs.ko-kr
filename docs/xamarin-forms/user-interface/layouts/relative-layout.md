---
title: Xamarin.Forms RelativeLayout
description: 이 문서는 Xamarin.Forms RelativeLayout 클래스를 사용 하 여 어떤 화면 크기에 맞게 규모가 조정 되는 Ui를 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2530BCB8-01B8-4C4F-BF14-CA53659F1B5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: d8c2cc4f31b148ee3181629e5b3b5faf01016617
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772532"
---
# <a name="xamarinforms-relativelayout"></a>Xamarin.Forms RelativeLayout

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

`RelativeLayout` 위치 및 레이아웃 또는 형제 뷰의 속성을 기준으로 크기 보기에 사용 됩니다. 와 달리 `AbsoluteLayout`, `RelativeLayout` 이동 앵커 개념이 없고 아래쪽 또는 레이아웃의 오른쪽 가장자리를 기준으로 요소를 배치 하는 기능에는 없습니다. `RelativeLayout` 자체의 범위를 벗어난 요소 위치 지정 지원 하지 않습니다.

[![](relative-layout-images/layouts-sml.png "Xamarin.Forms 레이아웃")](relative-layout-images/layouts.png#lightbox "Xamarin.Forms 레이아웃")

## <a name="purpose"></a>용도

`RelativeLayout` 전체 레이아웃을 기준으로 또는 다른 뷰 화면에서 보기를 배치에 사용할 수 있습니다.

![](relative-layout-images/flag.png "RelativeLayout 탐색")

## <a name="usage"></a>사용법

### <a name="understanding-constraints"></a>제약 조건 이해

위치 및 크기 내에서 뷰를 `RelativeLayout` 제약 조건으로 수행 됩니다. 제약 조건 식에는 다음 정보를 포함할 수 있습니다.

- **형식** &ndash; 제약 조건에서 부모에 상대적인 또는 다른 보기로 인지 합니다.
- **속성** &ndash; 제약 조건에 대 한 기초로 사용 하는 속성입니다.
- **계수** &ndash; 속성 값을 적용할 요소입니다.
- **상수** &ndash; 값의 오프셋으로 사용할 값입니다.
- **ElementName** &ndash; 제약 조건을 기준으로 하는 뷰의 이름입니다.

XAML을에서 제약 조건으로 표현 됩니다 `ConstraintExpression`s입니다. 다음 예제를 참조하세요.

```xaml
<BoxView Color="Green" WidthRequest="50" HeightRequest="50"
    RelativeLayout.XConstraint =
      "{ConstraintExpression Type=RelativeToParent,
                             Property=Width,
                             Factor=0.5,
                             Constant=-100}"
    RelativeLayout.YConstraint =
      "{ConstraintExpression Type=RelativeToParent,
                             Property=Height,
                             Factor=0.5,
                             Constant=-100}" />
```

C#, 뷰의 식 대신 함수를 사용 하 여 제약 조건을 조금씩 다르게 표현 됩니다. 제약 조건은 레이아웃의 인수로 지정 `Add` 메서드:

```csharp
layout.Children.Add(box, Constraint.RelativeToParent((parent) =>
    {
      return (.5 * parent.Width) - 100;
    }),
    Constraint.RelativeToParent((parent) =>
    {
        return (.5 * parent.Height) - 100;
    }),
    Constraint.Constant(50), Constraint.Constant(50));
```

위의 레이아웃의 다음 측면을 note:

- 합니다 `x` 고 `y` 제약 조건은 고유한 제약 조건으로 지정 됩니다.
- C#를 상대 제약 조건은 함수로 정의 됩니다. 개념 같은 `Factor` , 아니지만 수동으로 구현할 수 있습니다.
- 상자의 `x` 좌표-100 부모의 너비 절반으로 정의 됩니다.
- 상자의 `y` 좌표-100 부모의 높이 반으로 정의 됩니다.

> [!NOTE]
> 제약 조건이 정의 된 방식으로 인해에서 더 복잡 한 레이아웃을 만들 수는 C# 보다 XAML을 사용 하 여 지정할 수 있습니다.

제약 조건으로 정의 위의 예에서의 두 `RelativeToParent` &ndash; 부모 요소를 기준으로 해당 값은 즉, 합니다. 다른 보기를 기준으로 제약 조건을 정의할 수 이기도 합니다. 이 통해 (개발자)에 게 보다 직관적인 레이아웃에 대 한 레이아웃 코드의 의도 분명 더 쉽게 만들 수 있습니다.

하나의 요소가 다른 보다 낮은 20 픽셀 수 해야 하는 레이아웃을 고려해 야 합니다. 두 요소는 상수 값으로 정의 하는 경우 아래 가질 수 해당 `Y` 제약 조건이 있는 20 픽셀 보다 큰 상수로 정의 `Y` 상위 요소의 제약 조건. 상위 요소 위치는 비율을 사용 하 여 픽셀 크기를 알 수 없습니다 있도록 짧은 방법은 대체 됩니다. 이 경우 다른 요소의 위치를 기준으로 요소를 제한 보다 강력한는:

```xaml
<RelativeLayout>
    <BoxView Color="Red" x:Name="redBox"
        RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent,
            Property=Height,Factor=.15,Constant=0}"
        RelativeLayout.WidthConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Width,Factor=1,Constant=0}"
        RelativeLayout.HeightConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Height,Factor=.8,Constant=0}" />
    <BoxView Color="Blue"
        RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView,
            ElementName=redBox,Property=Y,Factor=1,Constant=20}"
        RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView,
            ElementName=redBox,Property=X,Factor=1,Constant=20}"
        RelativeLayout.WidthConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Width,Factor=.5,Constant=0}"
        RelativeLayout.HeightConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Height,Factor=.5,Constant=0}" />
</RelativeLayout>
```

동일한 레이아웃을 위해 C#:

```csharp
layout.Children.Add (redBox, Constraint.RelativeToParent ((parent) => {
        return parent.X;
    }), Constraint.RelativeToParent ((parent) => {
        return parent.Y * .15;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Width;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Height * .8;
    }));
layout.Children.Add (blueBox, Constraint.RelativeToView (redBox, (Parent, sibling) => {
        return sibling.X + 20;
    }), Constraint.RelativeToView (blueBox, (parent, sibling) => {
        return sibling.Y + 20;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Width * .5;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Height * .5;
    }));
```

다음과 같은 결정 하는 파란색 상자의 위치를 사용 하 여 출력을 생성 _상대_ 빨간색 상자 위치:

![](relative-layout-images/red-blue-box.png "빨강 및 파랑 BoxViews 사용 하 여 RelativeLayout")

### <a name="sizing"></a>크기 조정

뷰에 의해 배치 `RelativeLayout` 크기를 지정 하기 위한 두 가지 옵션이 있습니다.

- `HeightRequest & WidthRequest`
- `RelativeLayout.WidthConstraint` & `RelativeLayout.HeightConstraint`

`HeightRequest` 및 `WidthRequest` 의도 한 높이 및 보기의 너비를 지정 하지만 필요에 따라 레이아웃에 의해 재정의 될 수 있습니다. `WidthConstraint` 및 `HeightConstraint` 너비와 높이가 레이아웃의 또는 다른 보기의 속성을 기준으로 값 또는 상수 값 설정을 지원 합니다.

## <a name="exploring-a-complex-layout"></a>복잡 한 레이아웃을 탐색합니다.
각 레이아웃의 강점 및 약점 특정 레이아웃을 만들에 있습니다. 이 문서 시리즈에서는 레이아웃을 전체 세 가지 다른 레이아웃을 사용 하 여 구현 동일한 페이지 레이아웃을 사용 하 여 샘플 앱을 만들었습니다.

다음 XAML을 고려해 야 합니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.RelativeLayoutPage"
BackgroundColor="Maroon"
Title="RelativeLayout">
    <ContentPage.Content>
    <ScrollView>
      <RelativeLayout>
        <BoxView Color="Gray" HeightRequest="100"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <Button BorderRadius="35" x:Name="imageCircleBack"
            BackgroundColor="Maroon" HeightRequest="70" WidthRequest="70" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.5, Constant = -35}" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Factor=0, Property=Y, Constant=70}" />
        <Button BorderRadius="30" BackgroundColor="Red" HeightRequest="60"
            WidthRequest="60" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=imageCircleBack, Property=X, Factor=1,Constant=5}" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Factor=0, Property=Y, Constant=75}" />
        <Label Text="User Name" FontAttributes="Bold" FontSize="26"
            HorizontalTextAlignment="Center" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=140}" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <Entry Text="Bio + Hashtags" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=180}" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <RelativeLayout BackgroundColor="White" RelativeLayout.YConstraint="
            {ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=220}" HeightRequest="60" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" >
            <BoxView BackgroundColor="Black" WidthRequest="50"
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=X, Factor=0, Constant=5}" />
            <BoxView BackgroundColor="Maroon" WidthRequest="50"
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5}" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color"
                RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=20}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=X, Factor=0, Constant=60}" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color"
                RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=20}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5, Constant=55}" />
        </RelativeLayout>
        <RelativeLayout Padding="5,0,0,0">
          <Label FontSize="14" Text="Age:" TextColor="White"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=305}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=280}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
        <RelativeLayout  Padding="5,0,0,0">
          <Label FontSize="14" Text="Interests:" TextColor="White"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=345}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=320}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
        <RelativeLayout  Padding="5,0,0,0">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
            LineBreakMode="WordWrap"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=395}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
            BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=370}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
      </RelativeLayout>
    </ScrollView>
  </ContentPage.Content>
</ContentPage>
```

위 코드 다음의 레이아웃 발생합니다.

![](relative-layout-images/relative.png "복잡 한 RelativeLayout")

`RelativeLayouts`s 중첩 된 경우에 따라 레이아웃 중첩 수 있으므로 동일한 레이아웃 내에서 모든 요소를 제공 하는 것 보다 쉽습니다. 일부 요소가 되었다는 `RelativeToView`뷰 간의 관계 가이드 위치를 지정 하는 경우 더욱 쉽고 레이아웃에 허용 되는 때문입니다.

## <a name="related-links"></a>관련 링크

- [레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 예제 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
