---
title: Xamarin 양식 레이아웃 선택
description: Xamarin Forms 레이아웃 클래스를 사용 하면 응용 프로그램에서 UI 컨트롤을 정렬 하 고 그룹화 할 수 있습니다.
ms.prod: xamarin
ms.assetid: 05A39752-A174-447E-A30D-3CC9EF98CB96
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/21/2018
ms.openlocfilehash: 14e48d04696bb758a2010bd1d56ecaa125bbd30a
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83150006"
---
# <a name="choose-a-xamarinforms-layout"></a>Xamarin 양식 레이아웃 선택

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

Xamarin Forms 레이아웃 클래스를 사용 하면 응용 프로그램에서 UI 컨트롤을 정렬 하 고 그룹화 할 수 있습니다. 레이아웃 클래스를 선택 하려면 레이아웃이 자식 요소를 배치 하는 방법과 레이아웃의 자식 요소 크기를 조정 하는 방법을 알고 있어야 합니다. 또한 원하는 레이아웃을 만드는 레이아웃을 중첩 해야 할 수도 있습니다.

다음 이미지는 기본 Xamarin.ios 레이아웃 클래스를 사용 하 여 얻을 수 있는 일반적인 레이아웃을 보여 줍니다.

[![Xamarin.ios의 주 레이아웃 클래스](images/layouts.png "Xamarin Forms 레이아웃 클래스")](images/layouts-large.png#lightbox "Xamarin Forms 레이아웃 클래스")

## <a name="stacklayout"></a>StackLayout

는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 1 차원 스택의 요소를 가로 또는 세로로 구성 합니다. [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)속성은 요소의 방향을 지정 하 고 기본 방향은 [`Vertical`](xref:Xamarin.Forms.StackOrientation) 입니다. `StackLayout`는 일반적으로 페이지에서 UI의 하위 섹션을 정렬 하는 데 사용 됩니다.

다음 XAML은 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 세 가지 개체를 포함 하는 세로를 만드는 방법을 보여 줍니다 [`Label`](xref:Xamarin.Forms.Label) .

```xaml
<StackLayout Margin="20,35,20,25">
    <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
    <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
    <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
</StackLayout>
```

에서 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 요소의 크기를 명시적으로 설정 하지 않으면 확장 하 여 사용 가능한 너비 또는 높이 (속성이로 설정 된 경우)를 채웁니다 [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) [`Horizontal`](xref:Xamarin.Forms.StackOrientation) .

는 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 다른 자식 레이아웃을 포함 하는 부모 레이아웃으로 사용 되는 경우가 많습니다. 그러나 `StackLayout` [`Grid`](xref:Xamarin.Forms.Grid) 개체의 조합을 사용 하 여 레이아웃을 재현 하는 데는를 사용 하면 안 됩니다 `StackLayout` . 다음 코드에서는이 잘못 된 방법의 예를 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Name:" />
            <Entry Placeholder="Enter your name" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Age:" />
            <Entry Placeholder="Enter your age" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Occupation:" />
            <Entry Placeholder="Enter your occupation" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Address:" />
            <Entry Placeholder="Enter your address" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

불필요한 레이아웃 계산이 수행되기 때문에 이는 불필요합니다. 대신를 사용 하 여 원하는 레이아웃을 보다 효율적으로 구현할 수 있습니다 [`Grid`](xref:Xamarin.Forms.Grid) .

> [!TIP]
> 을 사용 하는 경우 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 하나의 자식 요소만로 설정 되었는지 확인 [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) 합니다. 이 속성은 지정된 자식 요소가 `StackLayout`에서 허용하는 최대의 공간을 차지하도록 하며, 이는 이러한 계산이 두 번 이상 수행되도록 하여 불필요합니다.

자세한 내용은 [Xamarin.ios StackLayout](stacklayout.md)을 참조 하세요.

## <a name="grid"></a>그리드

는 [`Grid`](xref:Xamarin.Forms.Grid) 비례 또는 절대 크기를 가질 수 있는 행 및 열에 요소를 표시 하는 데 사용 됩니다. 표의 행과 열은 및 속성을 사용 하 여 지정 됩니다 [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) .

특정 셀에 요소를 배치 하려면 [`Grid`](xref:Xamarin.Forms.Grid) [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) 및 연결 된 속성을 사용 [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) 합니다. 요소가 여러 행과 열에 걸쳐 있도록 하려면 [`Grid.RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) 및 연결 된 속성을 사용 [`Grid.ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) 합니다.

> [!NOTE]
> [`Grid`](xref:Xamarin.Forms.Grid)레이아웃은 테이블과 혼동 해서는 안 되며 표 형식 데이터를 제공 하기 위한 것이 아닙니다. HTML 테이블과 달리는 `Grid` 콘텐츠를 레이아웃 하기 위한 것입니다. 표 형식 데이터를 표시 하려면 [ListView](~/xamarin-forms/user-interface/listview/index.md), [CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)또는 [TableView](~/xamarin-forms/user-interface/tableview.md)를 사용 하는 것이 좋습니다.

다음 XAML에서는 [`Grid`](xref:Xamarin.Forms.Grid) 두 개의 행과 두 개의 열이 있는을 만드는 방법을 보여 줍니다.

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50" />
        <RowDefinition Height="50" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>    
    <Label Text="Column 0, Row 0"
           WidthRequest="200" />
    <Label Grid.Column="1"
           Text="Column 1, Row 0" />
    <Label Grid.Row="1"
           Text="Column 0, Row 1" />
    <Label Grid.Column="1"
           Grid.Row="1"
           Text="Column 1, Row 1" />
</Grid>
```

이 예에서 크기 조정은 다음과 같이 작동 합니다.

- 각 행의 명시적 높이는 50 장치 독립적 단위입니다.
- 첫 번째 열의 너비는로 설정 되므로 [`Auto`](xref:Xamarin.Forms.GridLength.Auto) 자식 항목에 필요한 너비의 너비입니다. 이 경우 첫 번째 너비를 수용할 수 있도록 장치 독립적 단위 200입니다 [`Label`](xref:Xamarin.Forms.Label) .

자동 크기 조정을 사용 하 여 열 이나 행 내에 공간을 분산 시킬 수 있습니다. 그러면 열과 행 크기가 내용에 맞게 조정 됩니다. 이렇게 하려면의 높이나 [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) 너비를 [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 로 설정 [`Auto`](xref:Xamarin.Forms.GridLength.Auto) 합니다. 비례 크기 조정을 사용 하 여 표의 행과 열 사이에 사용 가능한 공간을 가중치가 적용 된 비율로 분산할 수도 있습니다. 이는의 높이나의 너비를 연산자를 `RowDefinition` 사용 하는 값으로 설정 하 여 달성할 수 `ColumnDefinition` `*` 있습니다.

> [!CAUTION]
> 가능한 한 적은 수의 행과 열이 size로 설정 되어 있는지 확인 하십시오 [`Auto`](xref:Xamarin.Forms.GridLength.Auto) . 자동으로 크기가 조정된 행이나 열은 레이아웃 엔진이 추가적인 레이아웃 계산을 수행하도록 합니다. 가능한 경우 고정된 크기의 행과 열을 대신 사용하세요. 또는 열거형 값을 사용 하 여 행과 열을 비례 하는 공간을 차지 하도록 설정 [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) 합니다.

자세한 내용은 [Xamarin.ios 표](grid.md)를 참조 하세요.

## <a name="flexlayout"></a>FlexLayout

는 [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) [`StackLayout`](xref:Xamarin.Forms.StackLayout) 스택에서 가로 또는 세로 방향으로 자식 요소를 표시 한다는 점에서와 비슷합니다. 그러나는 `FlexLayout` 단일 행 이나 열에 맞지 않는 경우 자식을 래핑하고 자식 요소의 크기, 방향 및 맞춤을 보다 세부적으로 제어할 수도 있습니다.

다음 XAML은 [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) 단일 열에 뷰를 표시 하는를 만드는 방법을 보여 줍니다.

```xaml
<FlexLayout Direction="Column"
            AlignItems="Center"
            JustifyContent="SpaceEvenly">
    <Label Text="FlexLayout in Action" />
    <Button Text="Button" />
    <Label Text="Another Label" />
</FlexLayout>
```

이 예제에서 레이아웃은 다음과 같이 작동 합니다.

- [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)속성이로 설정 되어의 `Column` 자식이 `FlexLayout` 단일 항목 열에 정렬 되도록 합니다.
- [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)속성이로 설정 되어 `Center` 각 항목의 가로 가운데 맞춤을 설정 합니다.
- [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)속성은로 설정 됩니다 `SpaceEvenly` . 그러면 모든 항목, 첫 번째 항목 위 및 마지막 항목 아래에 있는 모든 남아 있는 세로 공간이 동일 하 게 할당 됩니다.

자세한 내용은 [Xamarin.ios 레이아웃](flex-layout.md)을 참조 하세요.

## <a name="relativelayout"></a>RelativeLayout

는 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 레이아웃이 나 형제 요소의 속성을 기준으로 요소를 배치 하 고 크기를 조정 하는 데 사용 됩니다. 기본적으로 요소는 레이아웃의 왼쪽 위 모퉁이에 배치 됩니다. 는 `RelativeLayout` 장치 크기에 비례하여 크기를 조정 하는 ui를 만드는 데 사용할 수 있습니다.

내에서 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 위치와 크기는 제약 조건으로 지정 됩니다. 제약 조건에는 [`Factor`](xref:Xamarin.Forms.ConstraintExpression.Factor) 및 [`Constant`](xref:Xamarin.Forms.ConstraintExpression.Constant) 속성이 있습니다 .이 속성을 사용 하 여 위치와 크기를 다른 개체의 여러 속성 (또는 분수)으로 정의 하 고 상수를 지정할 수 있습니다. 또한 상수는 음수일 수 있습니다.

> [!NOTE]
> 는 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 자체 범위 외부에 위치 지정 요소를 지원 합니다.

다음 XAML에서는에서 요소를 정렬 하는 방법을 보여 줍니다 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) .

```xaml
<RelativeLayout>
    <BoxView Color="Blue"
             HeightRequest="50"
             WidthRequest="50"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0}" />
    <BoxView Color="Red"
             HeightRequest="50"
             WidthRequest="50"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.85}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0}" />
    <BoxView x:Name="pole"
             Color="Gray"
             WidthRequest="15"
             RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.45}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.25}" />
    <BoxView Color="Green"
             RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
             RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.2, Constant=20}"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

이 예제에서 레이아웃은 다음과 같이 작동 합니다.

- 파랑은 [`BoxView`](xref:Xamarin.Forms.BoxView) 5 0x50 장치 독립적 단위의 명시적 크기를 제공 합니다. 기본 위치인 레이아웃의 왼쪽 위 모퉁이에 배치 됩니다.
- 빨강에는 [`BoxView`](xref:Xamarin.Forms.BoxView) 50x50 장치 독립적 단위의 명시적 크기가 지정 됩니다. 레이아웃의 오른쪽 위 모퉁이에 배치 됩니다.
- 회색에는 [`BoxView`](xref:Xamarin.Forms.BoxView) 장치 독립적 단위의 명시적인 너비가 지정 되 고, 높이는 부모 높이의 75%로 설정 됩니다.
- 녹색에는 [`BoxView`](xref:Xamarin.Forms.BoxView) 명시적 크기가 제공 되지 않습니다. 해당 위치는 명명 된를 기준으로 설정 됩니다 `BoxView` `pole` .

> [!WARNING]
> 가능한 경우 `RelativeLayout`을 사용하지 마세요. 그러면 CPU가 훨씬 더 많은 작업을 수행해야 합니다.

자세한 내용은 [RelativeLayout](relative-layout.md)를 참조 하세요.

## <a name="absolutelayout"></a>AbsoluteLayout

는 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 명시적 값 또는 레이아웃 크기를 기준으로 하는 값을 사용 하 여 요소를 배치 하 고 크기를 조정 하는 데 사용 됩니다. 위치는의 왼쪽 위 모퉁이를 기준으로 자식의 왼쪽 위 모퉁이에 의해 지정 됩니다 `AbsoluteLayout` .

는 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 자식에 크기를 적용할 수 있거나 요소의 크기가 다른 자식의 위치 지정에 영향을 주지 않는 경우에만 사용 되는 특수 한 용도의 레이아웃으로 간주 되어야 합니다. 이 레이아웃의 표준 사용은 다른 컨트롤과 페이지를 포함 하는 오버레이를 만들어 사용자가 페이지의 일반 컨트롤과 상호 작용 하지 못하도록 하는 것입니다.

> [!IMPORTANT]
> `HorizontalOptions`및 `VerticalOptions` 속성은의 자식에는 영향을 주지 않습니다 `AbsoluteLayout` .

내에서 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 연결 된 [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) 속성은 요소의 가로 위치, 세로 위치, 너비 및 높이를 지정 하는 데 사용 됩니다. 또한 [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) 연결 된 속성은 레이아웃 경계가 해석 되는 방법을 지정 합니다.

다음 XAML에서는에서 요소를 정렬 하는 방법을 보여 줍니다 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) .

```xaml
<AbsoluteLayout Margin="40">
    <BoxView Color="Red"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
             Rotation="30" />
    <BoxView Color="Green"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
             Rotation="60" />
    <BoxView Color="Blue"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" />
</AbsoluteLayout>
```

이 예제에서 레이아웃은 다음과 같이 작동 합니다.

- 각에 [`BoxView`](xref:Xamarin.Forms.BoxView) 는 100x100의 명시적 크기가 지정 되 고, 가로로 가운데 맞춤 되는 동일한 위치에 표시 됩니다.
- 빨강은 [`BoxView`](xref:Xamarin.Forms.BoxView) 30도 회전 되 고 녹색은 `BoxView` 60도 회전 됩니다.
- 각에서 [`BoxView`](xref:Xamarin.Forms.BoxView) 연결 된 [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) 속성은로 설정 되며 `PositionProportional` ,이는 너비와 높이가 고려 된 후의 위치가 나머지 공간에 비례 함을 나타냅니다.

> [!CAUTION]
> [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize)레이아웃 엔진이 추가 레이아웃 계산을 수행 하 게 하므로 가능 하면 항상 속성을 사용 하지 않도록 합니다.

자세한 내용은 [AbsoluteLayout](absolute-layout.md)를 참조 하세요.

## <a name="input-transparency"></a>입력 투명도

각 시각적 요소에는 [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent) 요소가 입력을 받을지 여부를 정의 하는 데 사용 되는 속성이 있습니다. 기본값은 이며 `false` 요소가 입력을 받도록 합니다.

레이아웃 클래스에서이 속성을 설정 하면 해당 값이 자식 요소로 전송 됩니다. 따라서 [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent) 레이아웃 클래스에서 속성을로 설정 하면 `true` 레이아웃 내의 모든 요소가 입력을 받지 않게 됩니다.

## <a name="layout-performance"></a>레이아웃 성능

가능한 최상의 레이아웃 성능을 얻으려면 [레이아웃 성능 최적화](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)의 지침을 따르세요.

또한 시각적 트리에서 지정 된 레이아웃을 제거 하는 레이아웃 압축을 사용 하 여 페이지 렌더링 성능도 향상 시킬 수 있습니다. 자세한 내용은 [레이아웃 압축](layout-compression.md)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Xamarin.ios 레이아웃 (비디오)](https://youtu.be/4HlLjTZQzjM)
- [Xamarin.ios StackLayout](stacklayout.md)
- [Xamarin.ios 표](grid.md)
- [Xamarin.ios의 레이아웃](flex-layout.md)
- [Xamarin.ios AbsoluteLayout](absolute-layout.md)
- [Xamarin.ios RelativeLayout](relative-layout.md)
- [레이아웃 성능 최적화](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)
- [레이아웃 압축](layout-compression.md)
