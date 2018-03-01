---
title: RelativeLayout
description: "모든 화면 크기에 맞게 확장할 수 있는 Ui를 만들 RelativeLayout를 사용 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2530BCB8-01B8-4C4F-BF14-CA53659F1B5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: ad621fa093379d5ad2dd81c81ce42dcaaa9bd923
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="relativelayout"></a>RelativeLayout

`RelativeLayout` 위치 및 크기 보기 레이아웃 또는 형제 보기의 속성을 기준으로 하는 데 사용 됩니다. 와 달리 `AbsoluteLayout`, `RelativeLayout` 이동 앵커의 개념 없고 아래쪽 또는 레이아웃의 오른쪽 가장자리를 기준으로 요소를 배치 하기 위한 기능에는 없습니다. `RelativeLayout` 요소 자체의 범위를 벗어난 위치 지정 지원 하지 않습니다.

[ ![](relative-layout-images/layouts-sml.png "Xamarin.Forms Layouts")](relative-layout-images/layouts.png "Xamarin.Forms Layouts")

## <a name="purpose"></a>용도

`RelativeLayout` 데 사용할 수 전체 레이아웃을 기준으로 화면에는 뷰 또는 두 개의 다른 뷰 위치를 지정 합니다.

![](relative-layout-images/flag.png "RelativeLayout 탐색")

## <a name="usage"></a>사용법

### <a name="understanding-constraints"></a>제약 조건 이해

배치 및 보기 내에서 크기 조정 된 `RelativeLayout` 제약 조건으로 수행 됩니다. 제약 조건 식에서 다음 정보를 포함할 수 있습니다.

- **형식** &ndash; 제약 조건 인지 여부는 부모에 상대적인 다른 보기로 합니다.
- **속성** &ndash; 제약 조건에 대 한 기준으로 사용할 속성을 합니다.
- **비율** &ndash; 속성 값에 적용할 비율입니다.
- **상수** &ndash; 값의 오프셋으로 사용할 값입니다.
- **ElementName** &ndash; 기준으로 제약 조건이 설정 된 보기의 이름입니다.

Xaml에서는 제약 조건으로 표현 됩니다 `ConstraintExpression`s입니다. 다음 예제를 참조하세요.

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

C#에서 제약 조건은 표현 됩니다 약간 다르게 식 대신 함수를 사용 하 여 보기에. 제약 조건은 레이아웃의 인수로 지정 `Add` 메서드:

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

- `x` 및 `y` 제약 조건은 고유 제약 조건으로 지정 합니다.
- C#에서 상대 제약 조건은 함수로 정의 됩니다. 개념 같은 `Factor` , 아니지만 수동으로 구현할 수 있습니다.
- 상자의 `x` 좌표 부모-100의 너비 절반으로 정의 됩니다.
- 상자의 `y` 좌표-100 부모의 높이 절반으로 정의 됩니다.

> [!NOTE]
> **참고**: 제약 조건을 정의 하는 방식 때문에 C# XAML로 지정 된 수의 더 복잡 한 레이아웃을 만들 수는 있습니다.

제약 조건으로 정의 위의 예에서의 두 `RelativeToParent` &ndash; 부모 요소를 기준으로 요소의 값은 즉, 합니다. 다른 보기를 기준으로 제약 조건을 정의 하는 것도 가능 합니다. 이 통해 (개발자)에 게 보다 직관적인 레이아웃에 있으며 레이아웃 코드의 의도 분명 더 쉽게 만들 수 있습니다.

하나의 요소가 다른 보다 낮은 20 픽셀이 고 해야 하는 레이아웃을 고려 합니다. 더 작은 값이 있을 수 요소는 모두 상수 값을 정의한 경우 속성이 해당 `Y` 은 20 픽셀 보다 큰 상수로 정의 된 제약 조건은 `Y` 상위 요소의 제약 조건을 합니다. 이 방법에 더 높은 요소가 위치는 비율을 사용 하 여 픽셀 크기를 알 수 없는 있도록 짧은 포함 됩니다. 이 경우 다른 요소의 위치에 따라 요소를 제한가 더 강력 합니다.

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

에 C#에서 동일한 레이아웃을 수행 합니다.

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

다음과 같이 결정 파란색 상자 위치와 출력 생성이 _상대_ 빨간색 상자의 위치:

![](relative-layout-images/red-blue-box.png "빨강 및 파랑 BoxViews와 RelativeLayout")

### <a name="sizing"></a>크기 조정

에 의해 배치 된 뷰 `RelativeLayout` 해당 크기를 지정 하기 위한 두 가지 옵션이 있습니다.

- `HeightRequest & WidthRequest`
- `RelativeLayout.WidthConstraint` & `RelativeLayout.HeightConstraint`

`HeightRequest` 및 `WidthRequest` 의도 된 높이 보기의 너비를 지정 하지만 필요에 따라 레이아웃 재정의 될 수 있습니다. `WidthConstraint` 및 `HeightConstraint` 레이아웃의 또는 다른 보기의 속성을 기준으로 값 또는 상수 값으로 높이 너비를 설정할 수 있습니다.

## <a name="exploring-a-complex-layout"></a>복잡 한 레이아웃을 탐색합니다.
사용할 수 있는 레이아웃의 각 특정 레이아웃을 만들 장단점을 포함. 이러한 일련의 레이아웃 문서 전체에서 샘플 응용 프로그램을 세 개의 다른 레이아웃을 사용 하 여 구현 동일한 페이지 레이아웃 작성 되었습니다.

다음 XAML을 고려 합니다.

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
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5, Constant=}" />
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

위의 코드는 다음과 같은 레이아웃 발생합니다.

![](relative-layout-images/relative.png "복잡 한 RelativeLayout")

Windows Phone 단추 렌더링 방법 차이로 인해를 원 중 일부는 Windows Phone 스크린 샷에 boxviews로 대체 되었습니다.

다음에 유의 `RelativeLayouts`경우에 따라 레이아웃에 중첩 될 수 있으므로 동일한 레이아웃 내에서 모든 요소를 제공 하는 보다 쉽게 s 중첩 됩니다. 또한 구성 요소 개발자는 일부 요소가 `RelativeToView`수 있게 해 주는 더욱 쉽고 레이아웃 위치 뷰 간의 관계를 안내 하는 경우 때문에 있습니다.


## <a name="related-links"></a>관련 링크

- [레이아웃 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 예제 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
