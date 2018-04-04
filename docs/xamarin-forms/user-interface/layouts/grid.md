---
title: 표
description: 표에서 뷰를 제공 합니다.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 084fde62687bd9bdd84779e4486dd8a971d1acdd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="grid"></a>표

[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) 행과 열으로 뷰를 정렬 하는 지원 합니다. 행과 열을 비례 크기 또는 절대 크기를 설정할 수 있습니다. `Grid` 레이아웃 기존 테이블와 혼동 해서는 안 하며 테이블 형식 데이터를 표시할 수 없습니다. `Grid` 행, 열 또는 셀 서식의 개념을 없습니다. HTML 테이블과 달리 `Grid` 콘텐츠도 배치 하기 위한 순수 하 게 됩니다.

[![](grid-images/layouts-sml.png "Xamarin.Forms Layouts")](grid-images/layouts.png#lightbox "Xamarin.Forms Layouts")

이 문서에서는 설명 합니다.

- **[용도](#Purpose)**  &ndash; 일반적인 용도 `Grid`합니다.
- **[사용 현황](#Usage)**  &ndash; 사용 하는 방법을 `Grid` 원하는 설계를 달성 하기 위해.
  - **[행과 열](#Rows_and_Columns)**  &ndash; 행 및 열에 대 한 지정 된 `Grid`합니다.
  - **[보기를 배치](#Placing_Views)**  &ndash; 뷰 특정 행과 열에 있는 표에서를 추가 합니다.
  - **[간격](#Spacing)**  &ndash; 행과 열 사이의 간격을 구성 합니다.
  - **[범위](#Spans)**  &ndash; 여러 행 이나 열에 걸쳐 요소를 구성 합니다.

![](grid-images/grid.png "표 탐색")

## <a name="purpose"></a>용도

`Grid` 데 사용할 수는 표 형태에 보기를 정렬 합니다. 경우에 유용합니다.

- 계산기 응용 프로그램의 단추 정렬
- 예: iOS 또는 Android 홈 화면 그리드로 정렬 단추/선택
- 1 차원 (일부 도구 모음에서 같이)에 동일한 크기의 되도록 보기를 정렬

## <a name="usage"></a>사용법

전통적인 테이블과 달리 `Grid` 수 및 내용에서 행 및 열 크기를 유추 하지 않습니다. 대신, `Grid` 가 `RowDefinitions` 및 `ColumnDefinitions` 컬렉션입니다. 이 행과 열 레이아웃 되는 정의 보유 합니다. 보기에 추가 됩니다 `Grid` 지정한 행 및 열 인덱스를 식별 하는 행 및 열에는 보기를 배치 해야 합니다.

<a name="Rows_and_Columns" />

### <a name="rows-and-columns"></a>행 및 열

행 및 열 정보에 저장 됩니다 `Grid`의 `RowDefinitions`  &  `ColumnDefinitions` 속성은 각 컬렉션의 [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) 및 [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/)각각 개체입니다. `RowDefinition` 단일 속성을 가진 `Height`, 및 `ColumnDefinition` 단일 속성을 가진 `Width`합니다. 높이 너비에 대 한 옵션은 다음과 같습니다.

- **자동** &ndash; 행 이나 열에 대 한 내용에 맞게 자동으로 크기입니다. 로 지정 된 [ `GridUnitType.Auto` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/) 으로 또는 C#에서 `Auto` XAML에서 합니다.
- **Proportional(*)** &ndash; 나머지 공간의 비율로 열과 행의 크기를 조정 합니다. 값으로 지정 하 고 `GridUnitType.Star` C#에서 `#*` XAML에서와 `#` 되 사용자가 원하는 값입니다. 한 개의 행/열과 지정 `*` 하면 사용 가능한 공간을 채웁니다.
- **절대** &ndash; 열 및 특정 고정된 높이 너비 값이 있는 행의 크기를 조정 합니다. 값으로 지정 하 고 `GridUnitType.Absolute` C#에서 `#` XAML에서와 `#` 되 사용자가 원하는 값입니다.

> [!NOTE]
> 열에 대 한 너비 값으로 설정 된 ' * ' Xamarin.Forms에는 기본적으로를 통해 열으로 가득 찰 사용 가능한 공간입니다.

세 개의 행과 두 개의 열을 필요로 하는 앱을 살펴보겠습니다. 맨 아래 행 높이가 200px 정확 하 게 해야 하며 맨 위 행 높이가 가운데 행의 두 배가 됩니다. 왼쪽된 열 수 있을 만큼의 너비만를 콘텐츠에 맞게 하 고 오른쪽 열 나머지 공간을 채우기 위해 필요 합니다.

Xaml의 경우:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="2*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="200" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

C#:

```csharp
var grid = new Grid();
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(2, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(200)});
grid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (200) });
```

<a name="Placing_Views" />

### <a name="placing-views-in-a-grid"></a>뷰는 표에 배치

보기를 배치 하는 `Grid` 눈금에 자식으로 추가 하 고에서 속해 있는 행 및 열을 지정 해야 합니다.

XAML에서 사용 하 여 `Grid.Row` 및 `Grid.Column` 배치를 지정 하려면 각 개별 보기에 있습니다. `Grid.Row` 및 `Grid.Column` 행과 열의 0부터 시작 하는 목록을 기반으로 하는 위치를 지정 합니다. 즉, 했는지 4x4 모눈의 왼쪽된 위 셀은 (0, 0)를 오른쪽 아래 셀 (3.3).

`Grid` 표시 된 다음 네 개의 셀을 포함 합니다.

![](grid-images/label-grid.png "4 개의 보기가 있는 그리드")

Xaml의 경우:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Label Text="Top Left" Grid.Row="0" Grid.Column="0" />
  <Label Text="Top Right" Grid.Row="0" Grid.Column="1" />
  <Label Text="Bottom Left" Grid.Row="1" Grid.Column="0" />
  <Label Text="Bottom Right" Grid.Row="1" Grid.Column="1" />
</Grid>
```

C#:

```csharp
var grid = new Grid();

grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});

var topLeft = new Label { Text = "Top Left" };
var topRight = new Label { Text = "Top Right" };
var bottomLeft = new Label { Text = "Bottom Left" };
var bottomRight = new Label { Text = "Bottom Right" };

grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 0, 1);
grid.Children.Add(bottomLeft, 1, 0);
grid.Children.Add(bottomRight, 1, 1);
```

위의 코드는 레이블 4 개, 두 개의 열과 두 개의 행 그리드를 만듭니다. 참고 각 레이블에 동일한 크기를 포함 하 고 행 사용 가능한 모든 공간을 사용 하도록 확장 됩니다.

위의 예에서 보기에 추가 됩니다는 [ `Grid.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.Children/) 사용 하 여 컬렉션의 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/) 왼쪽 및 위쪽 인수를 지정 하는 오버 로드 합니다. 사용 하는 경우는 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) 왼쪽 지정 하는 오버 로드, 오른쪽, 위쪽 및 아래쪽 인수 동안 왼쪽 및 위쪽 인수는 항상 내에 있는 셀을 나타냅니다는 [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), 오른쪽 및 아래쪽 인수 외부에 있는 셀을 참조 하는 것 처럼는 `Grid`합니다. 오른쪽 인수 항상 왼쪽된 인수 보다 크고 아래쪽 인수 항상 보다 커야 상위 인수 때문입니다. 다음 예제에서는 모두 사용 하 여 해당 하는 코드 `Add` 오버 로드 합니다.

```csharp
// left, top
grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);

// left, right, top, bottom
grid.Children.Add(topLeft, 0, 1, 0, 1);
grid.Children.Add(topRight, 1, 2, 0, 1);
grid.Children.Add(bottomLeft, 0, 1, 1, 2);
grid.Children.Add(bottomRight, 1, 2, 1, 2);
```

### <a name="spacing"></a>간격

`Grid` 행과 열 사이의 간격을 제어 하는 속성에 있습니다.  다음 속성을 사용자 지정할 수는 `Grid`:

- **ColumnSpacing** &ndash; 열 사이의 간격 크기입니다.
- **RowSpacing** &ndash; 행 사이의 간격 크기입니다.

다음 XAML 지정는 `Grid` 두 개의 열, 하나의 행과 5 px 열 사이의 간격입니다.

```xaml
<Grid ColumnSpacing="5">
  <Grid.ColumnDefinitions>
    <ColumnDefinitions Width="*" />
    <ColumnDefinitions Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

C#:

```csharp
var grid = new Grid { ColumnSpacing = 5 };
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
```

### <a name="spans"></a>범위

눈금을 사용할 경우에 종종 둘 이상의 행 또는 열 차지 하는 요소가 않습니다. 간단한 계산기 응용 프로그램을 고려 합니다.

![](grid-images/calculator.png "Calulator 응용 프로그램")

0 단추 걸쳐와 동일 하 게 두 개의 열에서 각 플랫폼에 대 한 기본 제공 계산기 있는 확인 합니다. 사용 하 여 이렇게는 `ColumnSpan` 속성 요소는 열 개수를 차지 합니다. 해당 단추에 대 한 XAML:

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

및 C#:

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

해당에 코드의 정적 메서드는 `Grid` 클래스는 변경 내용을 포함 한 위치 변경을 수행 하는 데 사용 됩니다 `ColumnSpan` 및 `RowSpan`합니다. 또한, 언제 든 지 설정할 수 있는 다른 속성과 달리 정적 메서드를 사용 하 여 설정할 속성 해야 이미 표에 수 변경.

위의 계산기 응용 프로그램에 대 한 전체 XAML은 다음과 같습니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.CalculatorGridXAML"
Title = "Calculator - XAML"
BackgroundColor="#404040">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="plainButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#eee"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="darkerButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#ddd"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="orangeButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#E8AD00"/>
                <Setter Property="TextColor" Value="White" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <Grid x:Name="controlGrid" RowSpacing="1" ColumnSpacing="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="150" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Label Text="0" Grid.Row="0" HorizontalTextAlignment="End" VerticalTextAlignment="End" TextColor="White"
        FontSize="60" Grid.ColumnSpan="4" />
            <Button Text = "C" Grid.Row="1" Grid.Column="0"
        Style="{StaticResource darkerButton}" />
            <Button Text = "+/-" Grid.Row="1" Grid.Column="1"
        Style="{StaticResource darkerButton}" />
            <Button Text = "%" Grid.Row="1" Grid.Column="2"
        Style="{StaticResource darkerButton}" />
            <Button Text = "div" Grid.Row="1" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "7" Grid.Row="2" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "8" Grid.Row="2" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "9" Grid.Row="2" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "X" Grid.Row="2" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "4" Grid.Row="3" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "5" Grid.Row="3" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "6" Grid.Row="3" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "-" Grid.Row="3" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "1" Grid.Row="4" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "2" Grid.Row="4" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "3" Grid.Row="4" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "+" Grid.Row="4" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "0" Grid.ColumnSpan="2"
        Grid.Row="5" Grid.Column="0" Style="{StaticResource plainButton}" />
            <Button Text = "." Grid.Row="5" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "=" Grid.Row="5" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

모두 표의 맨 위에 있는 레이블 및 0 단추는 열이 하나 이상 occuping를 확인 합니다. 중첩 된 표를 사용 하 여 유사한 레이아웃을 얻을 수 있지만 `ColumnSpan`  &  `RowSpan` 방법은 더 간단 합니다.

C# 구현:

```csharp
public CalculatorGridCode ()
{
  Title = "Calculator - C#";
  BackgroundColor = Color.FromHex ("#404040");

  var plainButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#eee") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var darkerButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#ddd") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var orangeButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#E8AD00") },
      new Setter { Property = Button.TextColorProperty, Value = Color.White },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };

  var controlGrid = new Grid { RowSpacing = 1, ColumnSpacing = 1 };
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (150) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });

  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });

  var label = new Label {
    Text = "0",
    HorizontalTextAlignment = TextAlignment.End,
    VerticalTextAlignment = TextAlignment.End,
    TextColor = Color.White,
    FontSize = 60
  };
  controlGrid.Children.Add (label, 0, 0);

  Grid.SetColumnSpan (label, 4);

  controlGrid.Children.Add (new Button { Text = "C", Style = darkerButton }, 0, 1);
  controlGrid.Children.Add (new Button { Text = "+/-", Style = darkerButton }, 1, 1);
  controlGrid.Children.Add (new Button { Text = "%", Style = darkerButton }, 2, 1);
  controlGrid.Children.Add (new Button { Text = "div", Style = orangeButton }, 3, 1);
  controlGrid.Children.Add (new Button { Text = "7", Style = plainButton }, 0, 2);
  controlGrid.Children.Add (new Button { Text = "8", Style = plainButton }, 1, 2);
  controlGrid.Children.Add (new Button { Text = "9", Style = plainButton }, 2, 2);
  controlGrid.Children.Add (new Button { Text = "X", Style = orangeButton }, 3, 2);
  controlGrid.Children.Add (new Button { Text = "4", Style = plainButton }, 0, 3);
  controlGrid.Children.Add (new Button { Text = "5", Style = plainButton }, 1, 3);
  controlGrid.Children.Add (new Button { Text = "6", Style = plainButton }, 2, 3);
  controlGrid.Children.Add (new Button { Text = "-", Style = orangeButton }, 3, 3);
  controlGrid.Children.Add (new Button { Text = "1", Style = plainButton }, 0, 4);
  controlGrid.Children.Add (new Button { Text = "2", Style = plainButton }, 1, 4);
  controlGrid.Children.Add (new Button { Text = "3", Style = plainButton }, 2, 4);
  controlGrid.Children.Add (new Button { Text = "+", Style = orangeButton }, 3, 4);
  controlGrid.Children.Add (new Button { Text = ".", Style = plainButton }, 2, 5);
  controlGrid.Children.Add (new Button { Text = "=", Style = orangeButton }, 3, 5);

  var zeroButton = new Button { Text = "0", Style = plainButton };
  controlGrid.Children.Add (zeroButton, 0, 5);
  Grid.SetColumnSpan (zeroButton, 2);

  Content = controlGrid;
}
```


## <a name="related-links"></a>관련 링크

- [17 장 워크인 Xamarin.Forms를 사용 하 여 모바일 앱 만들기](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [눈금](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)
- [레이아웃 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 예제 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
