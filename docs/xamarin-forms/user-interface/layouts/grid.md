---
title: Xamarin.Forms 표
description: 이 문서에서는 Xamarin.Forms 표 클래스를 사용 하 여 표에서 행과 열을 소유 하는 뷰를 제공 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 0d5df986caa01bba69b03d6502682889e78ecbc7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61300824"
---
# <a name="xamarinforms-grid"></a>Xamarin.Forms 표

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)

[`Grid`](xref:Xamarin.Forms.Grid) 지원 행과 열으로 뷰를 정렬 합니다. 가변 크기 또는 절대 크기는 행과 열을 설정할 수 있습니다. `Grid` 레이아웃 기존 테이블을 사용 하 여 혼동 해서는 안 됩니다 및 테이블 형식 데이터를 표시할 수 없습니다. `Grid` 행, 열 또는 셀 서식 개념이 없습니다. HTML 테이블과 달리 `Grid` 순수 하 게 콘텐츠를 배치 하기 위한 것입니다.

[![](grid-images/layouts-sml.png "Xamarin.Forms 레이아웃")](grid-images/layouts.png#lightbox "Xamarin.Forms 레이아웃")

이 문서에서는 설명 합니다.

- **[목적은](#purpose)**  &ndash; 의 일반적인 용도 `Grid`합니다.
- **[사용 현황](#usage)**  &ndash; 사용 하는 방법을 `Grid` 원하는 디자인을 달성 하기 위해.
  - **[행 및 열](#rows-and-columns)**  &ndash; 행 및 열에 대 한 지정 된 `Grid`합니다.
  - **[뷰를 배치](#placing-views-in-a-grid)**  &ndash; 표에 특정 행과 열에서 뷰를 추가 합니다.
  - **[간격](#spacing)**  &ndash; 행 및 열 사이의 간격을 구성 합니다.
  - **[범위](#spans)**  &ndash; 여러 행 또는 열에 걸쳐에 요소를 구성 합니다.

![](grid-images/grid.png "표 탐색")

## <a name="purpose"></a>용도

`Grid` 표 형태에 뷰를 정렬 하려면 사용할 수 있습니다. 많은 경우에 유용합니다.

- 계산기 앱에서 단추 정렬
- IOS 또는 Android 홈 화면 같은 모눈에 정렬 단추/선택
- 한 차원 (일부 도구 모음에서와 같이)에서 동일한 크기의 수 있도록 뷰 정렬

## <a name="usage"></a>사용법

기존 테이블과 달리 `Grid` 에서는 행 및 열 콘텐츠에서 크기와 수를 유추 하지 않습니다. 대신 `Grid` 했습니다 `RowDefinitions` 고 `ColumnDefinitions` 컬렉션입니다. 이러한 행과 열 개수는 배치의 정의 보유 합니다. 보기에 추가 됩니다 `Grid` 는 지정 된 행 및 열 인덱스를 사용 하 여 식별 행 및 열 뷰를 배치 해야 합니다.

### <a name="rows-and-columns"></a>행 및 열

행 및 열 정보에 저장 됩니다 `Grid`의 `RowDefinitions`  &  `ColumnDefinitions` 속성은 각 컬렉션의 [ `RowDefinition` ](xref:Xamarin.Forms.RowDefinition) 하 고 [ `ColumnDefinition` ](xref:Xamarin.Forms.ColumnDefinition)개체를 각각. `RowDefinition` 단일 속성을 가진 `Height`, 및 `ColumnDefinition` 단일 속성을 가진 `Width`합니다. 높이 너비에 대 한 옵션을 아래와 같습니다.

- **자동** &ndash; 행 이나 열의 내용에 맞게 자동으로 크기입니다. 로 지정 된 [ `GridUnitType.Auto` ](xref:Xamarin.Forms.GridUnitType) C# 또는 `Auto` XAML에서.
- **Proportional(*)** &ndash; 나머지 공간의 비율로 열과 행의 크기를 조정 합니다. 값으로 지정 하 고 `GridUnitType.Star` C#에서 `#*` 에서 XAML을 사용 하 여 `#` 원하는 값 중. 지정 된 행/열 `*` 하면 사용 가능한 공간을 채웁니다.
- **절대** &ndash; 열과 높이 및 너비 값이 특정 한 고정 된 행의 크기를 조정 합니다. 값으로 지정 하 고 `GridUnitType.Absolute` C#에서 `#` 에서 XAML을 사용 하 여 `#` 원하는 값 중.

> [!NOTE]
> 열에 대 한 너비 값으로 설정 되어 `*` Xamarin.Forms에는 기본적으로 확인 하는 열이 사용 가능한 공간을 입력 합니다. 행의 높이 값으로 설정 됩니다 `*` 기본적으로 합니다.

세 개의 행과 두 개의 열을 해야 하는 앱을 고려해 야 합니다. 맨 아래 행 높이가 200px 정확 하 게 해야 하며 맨 위 행은 가운데 행 높이가 두 배가 됩니다. 왼쪽된 열 콘텐츠에 맞게 하기에 충분 해야 하며 오른쪽 열 나머지 공간을 채우도록 합니다.

XAML:

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

### <a name="placing-views-in-a-grid"></a>뷰는 표에 배치

보기를 배치 하는 `Grid` 을 그리드로 자식으로 추가 하 고에 속한 행 및 열을 지정 해야 합니다.

XAML을 사용 하 여 `Grid.Row` 고 `Grid.Column` 배치를 지정 하려면 각 개별 뷰. 사실은 `Grid.Row` 고 `Grid.Column` 행과 열의 0부터 시작 목록을 기반으로 하는 위치를 지정 합니다. 이 4x4 모눈의 왼쪽된 위 셀 (0, 0) 이며 오른쪽 아래 셀 (3, 3)을 의미 합니다.

`Grid` 표시 된 다음 4 개의 셀을 포함 합니다.

![](grid-images/label-grid.png "네 가지 보기를 사용 하 여 눈금")

XAML:

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
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);
```

위의 코드는 네 개의 레이블, 두 개의 열 및 두 개의 행을 사용 하 여 그리드를 만듭니다. 각 레이블에 들이 있는 동일한 크기 및 행 확장 되어 사용 가능한 모든 공간 사용을 참고 합니다.

위의 예제에서는 보기에 추가 됩니다는 [ `Grid.Children` ](xref:Xamarin.Forms.Grid.Children) 사용 하 여 컬렉션의 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/) 왼쪽 및 위쪽 인수를 지정 하는 오버 로드 합니다. 사용 하는 경우는 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) 왼쪽 지정 하는 오버 로드, 오른쪽, 위쪽 및 아래쪽 인수 동안 왼쪽 및 위쪽 인수는 항상 내의 셀으로 참조 합니다 [ `Grid` ](xref:Xamarin.Forms.Grid), 오른쪽 및 아래쪽 인수 외부에 있는 셀을 참조 하는 것 처럼는 `Grid`합니다. 오른쪽 인수 왼쪽된 인수 보다 큰지 여야 하며 아래쪽 인수 항상 커야 상위 인수 보다 때문입니다. 다음 예제에서는 둘 다를 사용 하 여 해당 코드 `Add` 오버 로드 합니다.

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

`Grid` 속성이 키를 눌러 행 및 열 사이의 간격을 제어할 수 있습니다. 다음 속성을 사용자 지정할 수는 `Grid`:

- **ColumnSpacing** &ndash; 열 사이의 간격 크기입니다. 이 속성의 기본값은 6입니다.
- **RowSpacing** &ndash; 행 사이의 공간 크기입니다. 이 속성의 기본값은 6입니다.

다음 XAML 지정를 `Grid` 두 개의 열, 행이 하나 및 5를 사용 하 여 px 열 사이의 간격입니다.

```xaml
<Grid ColumnSpacing="5">
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

C#:

```csharp
var grid = new Grid { ColumnSpacing = 5 };
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
```

### <a name="spans"></a>범위

표를 사용 하는 경우에 종종 둘 이상의 행 또는 열 차지 하는 요소가 있습니다. 간단한 계산기 응용 프로그램을 고려해 야 합니다.

![](grid-images/calculator.png "Calulator 응용 프로그램")

0 단추에 걸쳐 두 개의 열 처럼 각 플랫폼에 대 한 기본 제공 계산기에서 확인할 수 있습니다. 이 위해서는 사용을 `ColumnSpan` 요소는 열 개수 차지 지정 하는 속성입니다. 해당 단추에 대 한 XAML:

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

및 C#:

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

에 해당 코드의 정적 메서드를 확인 합니다 `Grid` 클래스는 변경을 비롯 한 위치 변경을 수행 하는 데 사용 됩니다 `ColumnSpan` 및 `RowSpan`합니다. 참고는 언제 든 지 설정할 수 있는 다른 속성과 달리 정적 메서드를 사용 하 여 설정 하는 속성 이어야 수도 표의 변경 합니다.

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

모두 그리드의 맨 위에 있는 레이블 및 0 단추는 열이 하나 이상 occuping 알 수 있습니다. 중첩 된 표를 사용 하 여 유사한 레이아웃을 얻을 수 있지만 합니다 `ColumnSpan`  &  `RowSpan` 방법은 간단 합니다.

C# 구현 합니다.

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

- [17 장 Xamarin.Forms를 사용 하 여 모바일 앱 만들기](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [눈금](xref:Xamarin.Forms.Grid)
- [레이아웃 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 예제 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
