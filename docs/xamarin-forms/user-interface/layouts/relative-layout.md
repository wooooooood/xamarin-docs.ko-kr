---
title: Xamarin.FormsRelativeLayout
description: 이 문서에서는 RelativeLayout 클래스를 사용 하 여 Xamarin.Forms 모든 화면 크기에 맞게 크기가 조정 되는 ui를 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2530BCB8-01B8-4C4F-BF14-CA53659F1B5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f250b109f759bcf6bb7fa4ac0573743ac12c4bc1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127194"
---
# <a name="xamarinforms-relativelayout"></a>Xamarin.FormsRelativeLayout

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

`RelativeLayout`는 레이아웃 또는 형제 뷰의 속성을 기준으로 보기를 배치 하 고 크기를 조정 하는 데 사용 됩니다. 와 달리 `AbsoluteLayout` 에는 `RelativeLayout` 이동 앵커의 개념이 없고 레이아웃의 아래쪽 또는 오른쪽 가장자리를 기준으로 요소의 위치를 지정 하는 기능이 없습니다. `RelativeLayout`는 자체 범위를 벗어나는 요소 위치 지정을 지원 합니다.

[![](relative-layout-images/layouts-sml.png "Xamarin.Forms Layouts")](relative-layout-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>목적

`RelativeLayout`전체 레이아웃 또는 다른 보기를 기준으로 화면에 보기를 배치 하는 데 사용할 수 있습니다.

![](relative-layout-images/flag.png "RelativeLayout Exploration")

## <a name="usage"></a>사용

### <a name="understanding-constraints"></a>제약 조건 이해

내에서 뷰 위치 지정 및 크기 조정은 `RelativeLayout` 제약 조건을 사용 하 여 수행 됩니다. 제약 조건 식에는 다음과 같은 정보가 포함 될 수 있습니다.

- **유형** &ndash; 제약 조건이 부모 또는 다른 뷰에 상대적인 지 여부입니다.
- **속성** &ndash; 제약 조건에 대 한 기준으로 사용할 속성입니다.
- **요소** &ndash; 속성 값에 적용할 인수입니다.
- **상수** &ndash; 값의 오프셋으로 사용할 값입니다.
- **ElementName** &ndash; 제약 조건이 기준으로 하는 뷰의 이름입니다.

XAML에서 제약 조건은 s로 표시 됩니다 `ConstraintExpression` . 다음 예제를 참조하세요.

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

C #에서 제약 조건은 뷰에서 식 대신 함수를 사용 하 여 약간 다르게 표현 됩니다. 제약 조건은 레이아웃의 메서드에 대 한 인수로 지정 됩니다 `Add` .

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

위의 레이아웃에서 다음과 같은 측면을 유의 하십시오.

- `x`및 `y` 제약 조건은 고유한 제약 조건을 사용 하 여 지정 됩니다.
- C #에서 상대 제약 조건은 함수로 정의 됩니다. 과 같은 개념 `Factor` 은 없지만 수동으로 구현할 수 있습니다.
- 상자의 좌표는 `x` 부모의 절반 (-100)으로 정의 됩니다.
- 상자의 좌표는 `y` 부모의 절반 높이 (-100)로 정의 됩니다.

> [!NOTE]
> 제약 조건이 정의 된 방식으로 인해, XAML로 지정 된 것 보다 더 복잡 한 레이아웃을 c #으로 만들 수 있습니다.

위의 두 예제에서는 제약 조건을 정의 합니다 `RelativeToParent` &ndash; . 즉, 해당 값은 부모 요소를 기준으로 합니다. 다른 뷰를 기준으로 제약 조건을 정의 하는 것도 가능 합니다. 이렇게 하면 보다 직관적인 (개발자) 레이아웃을 사용할 수 있으며 레이아웃 코드의 의도를 더 쉽게 알 수 있습니다.

한 요소가 다른 요소 보다 20 픽셀이 되어야 하는 레이아웃을 고려 합니다. 상수 값을 사용 하 여 두 요소를 정의 하는 경우 lower는 `Y` `Y` 더 높은 요소의 제약 조건 보다 20 픽셀 더 큰 상수로 정의 된 제약 조건을 가질 수 있습니다. 상위 요소가 비율을 사용 하 여 배치 되는 경우이 방법은 짧습니다. 따라서 픽셀 크기를 알 수 없습니다. 이 경우 다른 요소의 위치에 따라 요소를 제한 하는 것이 더 강력 합니다.

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

C #에서 동일한 레이아웃을 수행 하려면 다음을 수행 합니다.

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

그러면 빨간색 상자의 위치를 _기준_ 으로 파란색 상자의 위치가 결정 된 다음 출력이 생성 됩니다.

![](relative-layout-images/red-blue-box.png "RelativeLayout with Red and Blue BoxViews")

### <a name="sizing"></a>크기 조정

로 레이아웃 된 뷰는 `RelativeLayout` 크기를 지정 하는 두 가지 옵션이 있습니다.

- `HeightRequest & WidthRequest`
- `RelativeLayout.WidthConstraint` & `RelativeLayout.HeightConstraint`

`HeightRequest``WidthRequest`보기의 원하는 높이와 너비를 지정 하지만 필요에 따라 레이아웃에 의해 재정의 될 수 있습니다. `WidthConstraint`및는 `HeightConstraint` 레이아웃의 또는 다른 보기의 속성을 기준으로 높이 및 너비를 값으로 설정 하거나 상수 값으로 설정 하는 기능을 지원 합니다.

## <a name="exploring-a-complex-layout"></a>복잡 한 레이아웃 탐색
각 레이아웃에는 특정 레이아웃을 만들기 위한 장점 및 단점이 있습니다. 이 일련의 레이아웃 문서에서 샘플 앱은 세 가지 다른 레이아웃을 사용 하 여 구현 된 동일한 페이지 레이아웃을 사용 하 여 만들어졌습니다.

다음 XAML을 참조 하십시오.

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

위의 코드는 다음과 같은 레이아웃을 생성 합니다.

![](relative-layout-images/relative.png "Complex RelativeLayout")

`RelativeLayouts`일부 경우에는 동일한 레이아웃 내에 모든 요소를 표시 하는 것 보다 더 쉽게 중첩 된 레이아웃을 사용할 수 있기 때문에가 중첩 되어 있습니다. 또한 일부 요소는 `RelativeToView` 뷰 간의 관계를 지정 하는 경우 더 쉽고 직관적인 레이아웃을 가능 하 게 하는입니다.

## <a name="related-links"></a>관련 링크

- [레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 예제 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
