---
title: Xamarin.Forms그리드에
description: Xamarin.Forms표는 자식 항목을 셀의 행과 열로 구성 하는 레이아웃입니다.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9d2e697a07e033fd7c3c8d3efffa1d67f6c097c3
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84946341"
---
# <a name="xamarinforms-grid"></a>Xamarin.Forms그리드에

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-griddemos)

[![Xamarin.Forms그리드에](grid-images/layouts.png "[! OP. 비 LOC (Xamarin.ios)] 그리드")](grid-images/layouts-large.png#lightbox "[! OP. 비 LOC (Xamarin.ios)] 그리드")

는 [`Grid`](xref:Xamarin.Forms.Grid) 하위 항목을 행과 열로 구성 하는 레이아웃으로, 비례 또는 절대 크기를 가질 수 있습니다. 기본적으로에는 `Grid` 하나의 행과 하나의 열이 포함 됩니다. 또한 `Grid` 다른 자식 레이아웃을 포함 하는 부모 레이아웃으로를 사용할 수 있습니다.

[`Grid`](xref:Xamarin.Forms.Grid)레이아웃은 테이블과 혼동 해서는 안 되며 표 형식 데이터를 제공 하기 위한 것이 아닙니다. HTML 테이블과 달리는 `Grid` 콘텐츠를 레이아웃 하기 위한 것입니다. 표 형식 데이터를 표시 하려면 [ListView](~/xamarin-forms/user-interface/listview/index.md), [CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)또는 [TableView](~/xamarin-forms/user-interface/tableview.md)를 사용 하는 것이 좋습니다.

[`Grid`](xref:Xamarin.Forms.Grid)클래스는 다음 속성을 정의 합니다.

- [`Column`](xref:Xamarin.Forms.Grid.ColumnProperty)`int`부모에 있는 뷰의 열 맞춤을 나타내는 연결 된 속성인 형식의 `Grid` 이 속성의 기본값은 0입니다. 유효성 검사 콜백은 속성이 설정 될 때 해당 값이 0 보다 크거나 같은 경우를 확인 합니다.
- [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions)형식의는 [`ColumnDefinitionCollection`](xref:Xamarin.Forms.ColumnDefinitionCollection) [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 그리드 열의 너비를 정의 하는 개체의 목록입니다.
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing)형식의는 `double` 그리드 열 간의 거리를 나타냅니다. 이 속성의 기본값은 6 개의 장치 독립적 단위입니다.
- [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty). 형식의 이며, `int` 부모 내에서 뷰가 걸쳐 있는 열의 총 수를 나타내는 연결 된 속성입니다 `Grid` . 이 속성의 기본값은 1입니다. 유효성 검사 콜백은 속성이 설정 될 때 해당 값이 1 보다 크거나 같으면이를 확인 합니다.
- [`Row`](xref:Xamarin.Forms.Grid.RowProperty)`int`부모에 있는 뷰의 행 맞춤을 나타내는 연결 된 속성인 형식의입니다 `Grid` . 이 속성의 기본값은 0입니다. 유효성 검사 콜백은 속성이 설정 될 때 해당 값이 0 보다 크거나 같은 경우를 확인 합니다.
- [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions)형식의는 [`RowDefinitionCollection`](xref:Xamarin.Forms.RowDefinitionCollection) [`RowDefintion`](xref:Xamarin.Forms.RowDefinition) 표 형태 창의 행 높이를 정의 하는 개체의 목록입니다.
- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing)형식의는 `double` 표 형태 창 행 간의 거리를 나타냅니다. 이 속성의 기본값은 6 개의 장치 독립적 단위입니다.
- [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty). 형식의 이며, `int` 부모 내에서 뷰가 걸쳐 있는 행의 총 수를 나타내는 연결 된 속성입니다 `Grid` . 이 속성의 기본값은 1입니다. 유효성 검사 콜백은 속성이 설정 될 때 해당 값이 1 보다 크거나 같으면이를 확인 합니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 속성이 데이터 바인딩 및 스타일의 대상이 될 수 있습니다.

[`Grid`](xref:Xamarin.Forms.Grid)클래스는 `Layout<T>` 형식의 속성을 정의 하는 클래스에서 파생 `Children` `IList<T>` 됩니다. `Children`속성은 `ContentProperty` 클래스의입니다 `Layout<T>` . 따라서 XAML에서 명시적으로 설정할 필요는 없습니다.

> [!TIP]
> 가능한 최상의 레이아웃 성능을 얻으려면 [레이아웃 성능 최적화](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)의 지침을 따르세요.

## <a name="rows-and-columns"></a>행 및 열

기본적으로에는 [`Grid`](xref:Xamarin.Forms.Grid) 하나의 행과 하나의 열이 포함 됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridTutorial.MainPage">
    <Grid Margin="20,35,20,20">
        <Label Text="By default, a Grid contains one row and one column." />
    </Grid>
</ContentPage>
```

이 예제에서는 [`Grid`](xref:Xamarin.Forms.Grid) [`Label`](xref:Xamarin.Forms.Label) 단일 위치에 자동으로 배치 되는 단일 자식을 포함 합니다.

[![기본 모눈 레이아웃의 스크린샷](grid-images/default.png "기본 모눈 레이아웃")](grid-images/default-large.png#lightbox "기본 모눈 레이아웃")

의 레이아웃 동작은 [`Grid`](xref:Xamarin.Forms.Grid) [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) 각각 및 개체의 컬렉션인 및 속성을 사용 하 여 정의할 수 있습니다 [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) . 이러한 컬렉션은의 행 및 열 특성을 정의 `Grid` 하며,의 [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) 각 행에 대해 하나의 개체를 포함 하 `Grid` 고의 [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 각 열에 대해 하나의 개체를 포함 해야 합니다 `Grid` .

[`RowDefinition`](xref:Xamarin.Forms.RowDefinition)클래스는 [`Height`](xref:Xamarin.Forms.RowDefinition.Height) 형식의 속성을 정의 [`GridLength`](xref:Xamarin.Forms.GridLength) 하 고 [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 클래스는 형식의 속성을 정의 합니다 [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width) [`GridLength`](xref:Xamarin.Forms.GridLength) . [`GridLength`](xref:Xamarin.Forms.GridLength)구조체는 [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) 세 개의 멤버를 포함 하는 열거형을 기준으로 행 높이나 열 너비를 지정 합니다.

- `Absolute`– 행 높이 또는 열 너비는 장치 독립적 단위의 값 (XAML의 숫자)입니다.
- `Auto`– 행 높이 또는 열 너비는 셀 내용 (XAML)에 따라 자동으로 크기가 조정 됩니다 `Auto` .
- `Star`– 남겨진 행 높이 또는 열 너비는 비례적으로 할당 됩니다 (숫자 뒤에 XAML로 표시 됨 `*` ).

[`Grid`](xref:Xamarin.Forms.Grid)속성이 인 행은 `Height` `Auto` 세로와 동일한 방식으로 해당 행의 뷰 높이를 제한 합니다 [`StackLayout`](xref:Xamarin.Forms.StackLayout) . 마찬가지로 속성을 가진 열은 `Width` `Auto` 가로와 매우 유사 하 게 작동 `StackLayout` 합니다.

> [!CAUTION]
> 가능한 한 적은 수의 행과 열이 size로 설정 되어 있는지 확인 하십시오 [`Auto`](xref:Xamarin.Forms.GridLength.Auto) . 자동으로 크기가 조정된 행이나 열은 레이아웃 엔진이 추가적인 레이아웃 계산을 수행하도록 합니다. 가능한 경우 고정된 크기의 행과 열을 대신 사용하세요. 또는 열거형 값을 사용 하 여 행과 열을 비례 하는 공간을 차지 하도록 설정 [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) 합니다.

다음 XAML은 [`Grid`](xref:Xamarin.Forms.Grid) 3 개의 행과 두 개의 열이 있는을 만드는 방법을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.BasicGridPage"
             Title="Basic Grid demo">
   <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

이 예제에서의 [`Grid`](xref:Xamarin.Forms.Grid) 전체 높이는 페이지 높이입니다. 는 `Grid` 세 번째 행의 높이가 100 장치 독립적 단위 임을 인식 합니다. 이는 해당 높이를 자체 높이로 빼고 별 앞의 숫자를 기준으로 첫 번째 및 두 번째 행 사이에 비례적으로 나머지 높이를 할당 합니다. 이 예에서는 첫 번째 행의 높이가 두 번째 행의 두 배가 됩니다.

두 [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 개체는 모두를로 설정 합니다 [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width) . 즉 `*` `1*` , 화면의 너비가 두 열 아래에 동일 하 게 분할 됩니다.

> [!IMPORTANT]
> 속성의 기본값은 [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height) `*` 입니다. 마찬가지로 속성의 기본값은 [`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width) `*` 입니다. 따라서 이러한 기본값을 사용할 수 있는 경우에는 이러한 속성을 설정할 필요가 없습니다.

[`Grid`](xref:Xamarin.Forms.Grid)및 연결 된 속성을 사용 하 여 자식 뷰를 특정 셀에 배치할 수 있습니다 [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) . 또한 자식 뷰가 여러 행과 열에 걸쳐 있도록 하려면 [`Grid.RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) 및 연결 된 속성을 사용 [`Grid.ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) 합니다.

다음 XAML에서는 동일한 [`Grid`](xref:Xamarin.Forms.Grid) 정의를 보여 주고 특정 셀에 자식 뷰를 배치 합니다 `Grid` .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.BasicGridPage"
             Title="Basic Grid demo">
   <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <BoxView Color="Green" />
        <Label Text="Row 0, Column 0"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Column="1"
                 Color="Blue" />
        <Label Grid.Column="1"
               Text="Row 0, Column 1"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Color="Teal" />
        <Label Grid.Row="1"
               Text="Row 1, Column 0"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Grid.Column="1"
                 Color="Purple" />
        <Label Grid.Row="1"
               Grid.Column="1"
               Text="Row1, Column 1"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="2"
                 Grid.ColumnSpan="2"
                 Color="Red" />
        <Label Grid.Row="2"
               Grid.ColumnSpan="2"
               Text="Row 2, Columns 0 and 1"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

> [!NOTE]
> `Grid.Row`및 `Grid.Column` 속성은 모두 0에서 인덱싱되는 `Grid.Row="2"` 반면는 `Grid.Column="1"` 두 번째 열을 참조 하는 동안 세 번째 행을 참조 합니다. 또한 두 속성의 기본값은 0 이므로의 첫 번째 행 또는 첫 번째 열을 차지 하는 자식 뷰에 대해 설정할 필요가 없습니다 [`Grid`](xref:Xamarin.Forms.Grid) .

이 예제에서는 세 개의 행이 모두 [`Grid`](xref:Xamarin.Forms.Grid) [`BoxView`](xref:Xamarin.Forms.BoxView) 및 뷰로 점유 됩니다 [`Label`](xref:Xamarin.Forms.Label) . 세 번째 행은 100 장치 독립적 단위이 고 처음 두 행은 나머지 공간을 차지 합니다. 첫 번째 행은 두 번째 행 보다 두 배 큽니다. 두 열의 너비는 같으며 반으로 나눕니다 `Grid` . `BoxView`세 번째 행의는 두 열에 모두 걸쳐 있습니다.

[![기본 모눈 레이아웃의 스크린샷](grid-images/basic.png "기본 모눈 레이아웃")](grid-images/basic-large.png#lightbox "기본 모눈 레이아웃")

또한의 자식 뷰는 셀을 [`Grid`](xref:Xamarin.Forms.Grid) 공유할 수 있습니다. 자식이 XAML에 표시 되는 순서는 자식 항목이에 배치 되는 순서입니다 `Grid` . 이전 예제에서 [`Label`](xref:Xamarin.Forms.Label) 개체는 개체 위에 렌더링 되기 때문에 표시 됩니다 [`BoxView`](xref:Xamarin.Forms.BoxView) . 개체가 `Label` 위에 렌더링 된 경우 개체는 표시 되지 않습니다 `BoxView` .

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class BasicGridPageCS : ContentPage
{
    public BasicGridPageCS()
    {
        Grid grid = new Grid
        {
            RowDefinitions =
            {
                new RowDefinition { Height = new GridLength(2, GridUnitType.Star) },
                new RowDefinition(),
                new RowDefinition { Height = new GridLength(100) }
            },
            ColumnDefinitions =
            {
                new ColumnDefinition(),
                new ColumnDefinition()
            }
        };

        // Row 0
        // The BoxView and Label are in row 0 and column 0, and so only needs to be added to the
        // Grid.Children collection to get default row and column settings.
        grid.Children.Add(new BoxView
        {
            Color = Color.Green
        });
        grid.Children.Add(new Label
        {
            Text = "Row 0, Column 0",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        });

        // This BoxView and Label are in row 0 and column 1, which are specified as arguments
        // to the Add method.
        grid.Children.Add(new BoxView
        {
            Color = Color.Blue
        }, 1, 0);
        grid.Children.Add(new Label
        {
            Text = "Row 0, Column 1",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 1, 0);

        // Row 1
        // This BoxView and Label are in row 1 and column 0, which are specified as arguments
        // to the Add method overload.
        grid.Children.Add(new BoxView
        {
            Color = Color.Teal
        }, 0, 1, 1, 2);
        grid.Children.Add(new Label
        {
            Text = "Row 1, Column 0",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 0, 1, 1, 2); // These arguments indicate that that the child element goes in the column starting at 0 but ending before 1.
                        // They also indicate that the child element goes in the row starting at 1 but ending before 2.

        grid.Children.Add(new BoxView
        {
            Color = Color.Purple
        }, 1, 2, 1, 2);
        grid.Children.Add(new Label
        {
            Text = "Row1, Column 1",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 1, 2, 1, 2);

        // Row 2
        // Alternatively, the BoxView and Label can be positioned in cells with the Grid.SetRow
        // and Grid.SetColumn methods.
        BoxView boxView = new BoxView { Color = Color.Red };
        Grid.SetRow(boxView, 2);
        Grid.SetColumnSpan(boxView, 2);
        Label label = new Label
        {
            Text = "Row 2, Column 0 and 1",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        };
        Grid.SetRow(label, 2);
        Grid.SetColumnSpan(label, 2);

        grid.Children.Add(boxView);
        grid.Children.Add(label);

        Title = "Basic Grid demo";
        Content = grid;
    }
}
```

코드에서 개체의 높이 [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) 와 개체의 너비를 지정 하려면 [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 일반적으로 [`GridLength`](xref:Xamarin.Forms.GridLength) 열거형과 함께 구조체의 값을 사용 합니다 [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) .

위의 예제 코드는에 자식을 추가 하 고 해당 셀이 상주 하는 셀을 지정 하는 여러 가지 방법을 보여 줍니다 [`Grid`](xref:Xamarin.Forms.Grid) . 왼쪽 `Add` , *오른쪽*, *위쪽*및 *left* *아래쪽* 인수를 지정 하는 오버 로드를 사용 하는 경우 *left* 및 *top* 인수는 항상 내의 셀을 참조 하 고 `Grid` , *오른쪽* 및 *아래쪽* 인수가 외부에 있는 셀을 참조 하는 것으로 나타납니다 `Grid` . 이는 *오른쪽* 인수가 항상 *왼쪽* 인수 보다 커야 하 고 *맨 아래* 인수가 항상 *top* 인수 보다 커야 하기 때문입니다. 다음 예에서는 2x2를 가정 하 고 `Grid` 두 오버 로드를 모두 사용 하는 동일한 코드를 보여 줍니다 `Add` .

```csharp
// left, top
grid.Children.Add(topLeft, 0, 0);           // first column, first row
grid.Children.Add(topRight, 1, 0);          // second column, first tow
grid.Children.Add(bottomLeft, 0, 1);        // first column, second row
grid.Children.Add(bottomRight, 1, 1);       // second column, second row

// left, right, top, bottom
grid.Children.Add(topLeft, 0, 1, 0, 1);     // first column, first row
grid.Children.Add(topRight, 1, 2, 0, 1);    // second column, first tow
grid.Children.Add(bottomLeft, 0, 1, 1, 2);  // first column, second row
grid.Children.Add(bottomRight, 1, 2, 1, 2); // second column, second row
```

> [!NOTE]
> 또한 및 메서드를 사용 하 여 자식 뷰를에 추가할 수 있습니다 [`Grid`](xref:Xamarin.Forms.Grid) [`AddHorizontal`](xref:Xamarin.Forms.Grid.IGridList`1.AddHorizontal*) .이 메서드는 [`AddVertical`](xref:Xamarin.Forms.Grid.IGridList`1.AddVertical*) 단일 행 이나 단일 열에 자식을 추가 `Grid` 합니다. `Grid`그러면는 이러한 호출이 수행 될 때 행 또는 열을 확장 하 고 올바른 셀에 자식을 자동으로 배치할 수 있습니다.

### <a name="simplify-row-and-column-definitions"></a>행 및 열 정의 간소화

XAML에서의 행 및 열 특성은 [`Grid`](xref:Xamarin.Forms.Grid) [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 각 행과 열에 대해 및 개체를 정의 하지 않아도 되는 단순화 된 구문을 사용 하 여 지정할 수 있습니다. 대신 [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) 및 속성은 [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) 쉼표로 구분 된 값을 포함 하는 문자열로 설정 될 수 있습니다 [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) . 여기서 형식 변환기는 Xamarin.Forms create 및 object로 기본 설정 됩니다 `RowDefinition` `ColumnDefinition` .

```xaml
<Grid RowDefinitions="1*, Auto, 25, 14, 20"
      ColumnDefinitions="*, 2*, Auto, 300">
    ...
</Grid>
```

이 예제에서에는 [`Grid`](xref:Xamarin.Forms.Grid) 5 개의 행과 4 개의 열이 있습니다. 세 번째, 네 번째 및 다섯 번째 행은 절대값으로 설정 되 고 두 번째 행은 내용에 자동으로 크기가 조정 됩니다. 그런 다음 나머지 높이가 첫 번째 행에 할당 됩니다.

위의 열은 절대 너비로 설정 되며 세 번째 열은 해당 내용에 대 한 자동 크기 조정입니다. 나머지 너비는 별 앞의 숫자를 기준으로 첫 번째 및 두 번째 열 사이에 비례 하 게 할당 됩니다. 이 예에서는가와 같기 때문에 두 번째 열의 너비가 첫 번째 열의 두 배가 됩니다 `*` `1*` .

## <a name="space-between-rows-and-columns"></a>행과 열 사이의 간격

기본적으로 [`Grid`](xref:Xamarin.Forms.Grid) 행은 6 개의 장치 독립적 공간 단위로 구분 됩니다. 마찬가지로, `Grid` 열은 장치 독립적인 6 개의 공간 단위로 구분 됩니다. 이러한 기본값은 [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) 각각 및 속성을 설정 하 여 변경할 수 있습니다 [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.GridSpacingPage"
             Title="Grid spacing demo">
    <Grid RowSpacing="0"
          ColumnSpacing="0">
        ..
    </Grid>
</ContentPage>
```

이 예제에서는 [`Grid`](xref:Xamarin.Forms.Grid) 행과 열 사이에 공백이 없는을 만듭니다.

[![셀 사이에 간격이 없는 모눈의 스크린샷](grid-images/spacing.png "셀 사이에 공백이 없는 그리드")](grid-images/spacing-large.png#lightbox "셀 사이에 공백이 없는 그리드")

> [!TIP]
> [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing)및 [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) 속성을 음수 값으로 설정 하 여 셀 내용이 겹칠 수 있습니다.

해당하는 C# 코드는 다음과 같습니다.

```csharp
public GridSpacingPageCS()
{
    Grid grid = new Grid
    {
        RowSpacing = 0,
        ColumnSpacing = 0,
        // ...
    };
    // ...

    Content = grid;
}
```

## <a name="alignment"></a>맞춤

의 자식 뷰는 [`Grid`](xref:Xamarin.Forms.Grid) 및 속성을 통해 셀 내에 배치 될 수 있습니다 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) . 이러한 속성은 구조체에서 다음 필드로 설정할 수 있습니다 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) .

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)

> [!IMPORTANT]
> `AndExpands`구조체의 필드는 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 개체에만 적용 됩니다 [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

다음 XAML에서는 [`Grid`](xref:Xamarin.Forms.Grid) 크기가 같은 9 개의 셀을 사용 하 여를 만들고 [`Label`](xref:Xamarin.Forms.Label) 각 셀에 다른 맞춤을 추가 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.GridAlignmentPage"
             Title="Grid alignment demo">
    <Grid RowSpacing="0"
          ColumnSpacing="0">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>

        <BoxView Color="AliceBlue" />
        <Label Text="Upper left"
               HorizontalOptions="Start"
               VerticalOptions="Start" />
        <BoxView Grid.Column="1"
                 Color="LightSkyBlue" />
        <Label Grid.Column="1"
               Text="Upper center"
               HorizontalOptions="Center"
               VerticalOptions="Start"/>
        <BoxView Grid.Column="2"
                 Color="CadetBlue" />
        <Label Grid.Column="2"
               Text="Upper right"
               HorizontalOptions="End"
               VerticalOptions="Start" />
        <BoxView Grid.Row="1"
                 Color="CornflowerBlue" />
        <Label Grid.Row="1"
               Text="Center left"
               HorizontalOptions="Start"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Grid.Column="1"
                 Color="DodgerBlue" />
        <Label Grid.Row="1"
               Grid.Column="1"
               Text="Center center"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Grid.Column="2"
                 Color="DarkSlateBlue" />
        <Label Grid.Row="1"
               Grid.Column="2"
               Text="Center right"
               HorizontalOptions="End"
               VerticalOptions="Center" />
        <BoxView Grid.Row="2"
                 Color="SteelBlue" />
        <Label Grid.Row="2"
               Text="Lower left"
               HorizontalOptions="Start"
               VerticalOptions="End" />
        <BoxView Grid.Row="2"
                 Grid.Column="1"
                 Color="LightBlue" />
        <Label Grid.Row="2"
               Grid.Column="1"
               Text="Lower center"
               HorizontalOptions="Center"
               VerticalOptions="End" />
        <BoxView Grid.Row="2"
                 Grid.Column="2"
                 Color="BlueViolet" />
        <Label Grid.Row="2"
               Grid.Column="2"
               Text="Lower right"
               HorizontalOptions="End"
               VerticalOptions="End" />
    </Grid>
</ContentPage>
```

이 예제에서 [`Label`](xref:Xamarin.Forms.Label) 각 행의 개체는 모두 동일 하 게 세로로 정렬 되지만 다른 가로 맞춤을 사용 합니다. 또는 `Label` 서로 다른 세로 맞춤을 사용 하 여 각 열의 개체가 동일 하 게 가로로 정렬 되는 것으로 생각할 수 있습니다.

[![표 형태의 셀 맞춤 스크린샷](grid-images/alignment.png "표의 셀 맞춤")](grid-images/alignment-large.png#lightbox "표의 셀 맞춤")

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class GridAlignmentPageCS : ContentPage
{
    public GridAlignmentPageCS()
    {
        Grid grid = new Grid
        {
            RowSpacing = 0,
            ColumnSpacing = 0,
            RowDefinitions =
            {
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition()
            },
            ColumnDefinitions =
            {
                new ColumnDefinition(),
                new ColumnDefinition(),
                new ColumnDefinition()
            }
        };

        // Row 0
        grid.Children.Add(new BoxView
        {
            Color = Color.AliceBlue
        });
        grid.Children.Add(new Label
        {
            Text = "Upper left",
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.Start
        });

        grid.Children.Add(new BoxView
        {
            Color = Color.LightSkyBlue
        }, 1, 0);
        grid.Children.Add(new Label
        {
            Text = "Upper center",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Start
        }, 1, 0);

        grid.Children.Add(new BoxView
        {
            Color = Color.CadetBlue
        }, 2, 0);
        grid.Children.Add(new Label
        {
            Text = "Upper right",
            HorizontalOptions = LayoutOptions.End,
            VerticalOptions = LayoutOptions.Start
        }, 2, 0);

        // Row 1
        grid.Children.Add(new BoxView
        {
            Color = Color.CornflowerBlue
        }, 0, 1);
        grid.Children.Add(new Label
        {
            Text = "Center left",
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.Center
        }, 0, 1);

        grid.Children.Add(new BoxView
        {
            Color = Color.DodgerBlue
        }, 1, 1);
        grid.Children.Add(new Label
        {
            Text = "Center center",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 1, 1);

        grid.Children.Add(new BoxView
        {
            Color = Color.DarkSlateBlue
        }, 2, 1);
        grid.Children.Add(new Label
        {
            Text = "Center right",
            HorizontalOptions = LayoutOptions.End,
            VerticalOptions = LayoutOptions.Center
        }, 2, 1);

        // Row 2
        grid.Children.Add(new BoxView
        {
            Color = Color.SteelBlue
        }, 0, 2);
        grid.Children.Add(new Label
        {
            Text = "Lower left",
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.End
        }, 0, 2);

        grid.Children.Add(new BoxView
        {
            Color = Color.LightBlue
        }, 1, 2);
        grid.Children.Add(new Label
        {
            Text = "Lower center",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.End
        }, 1, 2);

        grid.Children.Add(new BoxView
        {
            Color = Color.BlueViolet
        }, 2, 2);
        grid.Children.Add(new Label
        {
            Text = "Lower right",
            HorizontalOptions = LayoutOptions.End,
            VerticalOptions = LayoutOptions.End
        }, 2, 2);

        Title = "Grid alignment demo";
        Content = grid;
    }
}
```

## <a name="nested-grid-objects"></a>중첩 된 표 개체

는 [`Grid`](xref:Xamarin.Forms.Grid) 중첩 된 자식 `Grid` 개체 또는 다른 자식 레이아웃을 포함 하는 부모 레이아웃으로 사용할 수 있습니다. `Grid`개체를 중첩 하는 경우 `Grid.Row` ,, `Grid.Column` `Grid.RowSpan` 및 `Grid.ColumnSpan` 연결 된 속성은 항상 부모 내에서 뷰의 위치를 참조 합니다 `Grid` .

다음 XAML에서는 중첩 개체의 예를 보여 줍니다 [`Grid`](xref:Xamarin.Forms.Grid) .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:converters="clr-namespace:GridDemos.Converters"
             x:Class="GridDemos.Views.ColorSlidersGridPage"
             Title="Nested Grids demo">

    <ContentPage.Resources>
        <converters:DoubleToIntConverter x:Key="doubleToInt" />

        <Style TargetType="Label">
            <Setter Property="HorizontalTextAlignment"
                    Value="Center" />
        </Style>
    </ContentPage.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <BoxView x:Name="boxView"
                 Color="Black" />
        <Grid Grid.Row="1"
              Margin="20">
            <Grid.RowDefinitions>
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
            </Grid.RowDefinitions>
            <Slider x:Name="redSlider"
                    ValueChanged="OnSliderValueChanged" />
            <Label Grid.Row="1"
                   Text="{Binding Source={x:Reference redSlider},
                                  Path=Value,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Red = {0}'}" />
            <Slider x:Name="greenSlider"
                    Grid.Row="2"
                    ValueChanged="OnSliderValueChanged" />
            <Label Grid.Row="3"
                   Text="{Binding Source={x:Reference greenSlider},
                                  Path=Value,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Green = {0}'}" />
            <Slider x:Name="blueSlider"
                    Grid.Row="4"
                    ValueChanged="OnSliderValueChanged" />
            <Label Grid.Row="5"
                   Text="{Binding Source={x:Reference blueSlider},
                                  Path=Value,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Blue = {0}'}" />
        </Grid>
    </Grid>
</ContentPage>
```

이 예제에서 루트 레이아웃에 [`Grid`](xref:Xamarin.Forms.Grid) 는 [`BoxView`](xref:Xamarin.Forms.BoxView) 첫 번째 행에가 포함 되 고 `Grid` 두 번째 행에는 자식이 포함 됩니다. 자식은에 `Grid` 표시 되는 [`Slider`](xref:Xamarin.Forms.Slider) 색을 조작 하는 개체 `BoxView` 와 [`Label`](xref:Xamarin.Forms.Label) 각각의 값을 표시 하는 개체 `Slider` 를 포함 합니다.

[![중첩 된 표 스크린샷](grid-images/nesting.png "중첩 된 표 개체")](grid-images/nesting-large.png#lightbox "중첩 된 표 개체")

> [!IMPORTANT]
> [`Grid`](xref:Xamarin.Forms.Grid)개체와 기타 레이아웃을 더 깊게 중첩할 수록 중첩 된 레이아웃은 성능에 영향을 줍니다. 자세한 내용은 [올바른 레이아웃 선택](~/xamarin-forms/deploy-test/performance.md#choose-the-correct-layout)을 참조 하세요.

해당하는 C# 코드는 다음과 같습니다.

```csharp
public class ColorSlidersGridPageCS : ContentPage
{
    BoxView boxView;
    Slider redSlider;
    Slider greenSlider;
    Slider blueSlider;

    public ColorSlidersGridPageCS()
    {
        // Create an implicit style for the Labels
        Style labelStyle = new Style(typeof(Label))
        {
            Setters =
            {
                new Setter { Property = Label.HorizontalTextAlignmentProperty, Value = TextAlignment.Center }
            }
        };
        Resources.Add(labelStyle);

        // Root page layout
        Grid rootGrid = new Grid
        {
            RowDefinitions =
            {
                new RowDefinition(),
                new RowDefinition()
            }
        };

        boxView = new BoxView { Color = Color.Black };
        rootGrid.Children.Add(boxView);

        // Child page layout
        Grid childGrid = new Grid
        {
            Margin = new Thickness(20),
            RowDefinitions =
            {
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition()
            }
        };

        DoubleToIntConverter doubleToInt = new DoubleToIntConverter();

        redSlider = new Slider();
        redSlider.ValueChanged += OnSliderValueChanged;
        childGrid.Children.Add(redSlider);

        Label redLabel = new Label();
        redLabel.SetBinding(Label.TextProperty, new Binding("Value", converter: doubleToInt, converterParameter: "255", stringFormat: "Red = {0}", source: redSlider));
        Grid.SetRow(redLabel, 1);
        childGrid.Children.Add(redLabel);

        greenSlider = new Slider();
        greenSlider.ValueChanged += OnSliderValueChanged;
        Grid.SetRow(greenSlider, 2);
        childGrid.Children.Add(greenSlider);

        Label greenLabel = new Label();
        greenLabel.SetBinding(Label.TextProperty, new Binding("Value", converter: doubleToInt, converterParameter: "255", stringFormat: "Green = {0}", source: greenSlider));
        Grid.SetRow(greenLabel, 3);
        childGrid.Children.Add(greenLabel);

        blueSlider = new Slider();
        blueSlider.ValueChanged += OnSliderValueChanged;
        Grid.SetRow(blueSlider, 4);
        childGrid.Children.Add(blueSlider);

        Label blueLabel = new Label();
        blueLabel.SetBinding(Label.TextProperty, new Binding("Value", converter: doubleToInt, converterParameter: "255", stringFormat: "Blue = {0}", source: blueSlider));
        Grid.SetRow(blueLabel, 5);
        childGrid.Children.Add(blueLabel);

        // Place the child Grid in the root Grid
        rootGrid.Children.Add(childGrid, 0, 1);

        Title = "Nested Grids demo";
        Content = rootGrid;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs e)
    {
        boxView.Color = new Color(redSlider.Value, greenSlider.Value, blueSlider.Value);
    }
}
```

## <a name="related-links"></a>관련 링크

- [그리드 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-griddemos)
- [의 레이아웃 옵션Xamarin.Forms](layout-options.md)
- [레이아웃 선택 Xamarin.Forms](choose-layout.md)
- [Xamarin.Forms앱 성능 향상](~/xamarin-forms/deploy-test/performance.md)
