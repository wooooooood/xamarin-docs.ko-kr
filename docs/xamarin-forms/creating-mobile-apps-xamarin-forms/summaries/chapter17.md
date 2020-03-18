---
title: 요약 - 17장. Grid 마스터
description: 'Xamarin.Forms로 모바일 앱 만들기: 요약 - 17장. Grid 마스터'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 37b5e2bbafa816de27390771ae6daa33c74f7651
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "70760636"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>요약 - 17장. Grid 마스터

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)

[`Grid`](xref:Xamarin.Forms.Grid)는 자식 요소를 셀의 행과 열로 배열하는 강력한 레이아웃 메커니즘입니다. 유사한 HTML `table` 요소와 달리 `Grid`는 프레젠테이션 대신 레이아웃의 목적으로만 사용됩니다.

## <a name="the-basic-grid"></a>기본 Grid

`Grid`는 `Grid`가 상속하는 [`Children`](xref:Xamarin.Forms.Layout`1.Children) 속성을 정의하는 [`Layout<View>`](xref:Xamarin.Forms.Layout`1)에서 파생됩니다. XAML 또는 코드에서 이 컬렉션을 채울 수 있습니다.

### <a name="the-grid-in-xaml"></a>XAML의 Grid

XAML의 `Grid` 정의는 일반적으로 [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) 및 [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 개체로 `Grid`의 [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) 및 [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) 컬렉션을 채우는 것으로 시작됩니다. 이는 `Grid`의 행 및 열 수와 해당 속성을 설정하는 방법입니다.

`RowDefinition`에는 [`Height`](xref:Xamarin.Forms.RowDefinition.Height) 속성이 있고 `ColumnDefinition`에는 [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width) 속성이 있으며 둘 다 [`GridLength`](xref:Xamarin.Forms.GridLength) 형식의 구조체입니다.

XAML에서 [`GridLengthTypeConverter`](xref:Xamarin.Forms.GridLengthTypeConverter)는 단순 텍스트 문자열을 `GridLength` 값으로 변환합니다. 내부적으로 [`GridLength` 생성자](xref:Xamarin.Forms.GridLength.%23ctor(System.Double,Xamarin.Forms.GridUnitType))는 3개의 멤버를 포함하는 열거형인 [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) 형식의 값과 숫자를 기반으로 `GridLength` 값을 만듭니다.

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; 너비 또는 높이가 디바이스 독립적 단위(XAML의 숫자)로 지정됩니다.
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; 높이 또는 너비가 셀 내용에 따라 자동으로 크기가 조정됩니다(XAML의 "Auto").
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; 나머지 높이 또는 너비가 비례적으로 할당됩니다(XAML에서 *star*라고 하는 "\*"이 있는 숫자).

`Grid`의 각 자식 요소에도 명시적으로 또는 암시적으로 행과 열을 할당해야 합니다. 행 범위 및 열 범위는 선택 사항입니다. 이는 모두 연결된 바인딩 가능한 속성을 사용하여 지정되었습니다. 이는 `Grid`로 정의되었지만 `Grid`의 자식 요소에 설정된 &mdash; 속성입니다. `Grid`는 연결된 바인딩 가능한 정적 속성 4개를 정의합니다.

- [`RowProperty`](xref:Xamarin.Forms.Grid.RowProperty) &mdash; 0부터 시작하는 행. 기본값은 0입니다.
- [`ColumnProperty`](xref:Xamarin.Forms.Grid.ColumnProperty) &mdash; 0부터 시작하는 열. 기본값은 0입니다.
- [`RowSpanProperty`](xref:Xamarin.Forms.Grid.RowSpanProperty) &mdash; 자식 요소가 걸쳐 있는 행 수. 기본값은 1입니다.
- [`ColumnSpanProperty`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) &mdash; 자식 요소가 걸쳐 있는 열 수. 기본값은 1입니다.

코드에서 프로그램은 8개의 정적 메서드를 사용하여 이러한 값을 설정하고 가져올 수 있습니다.

- [`Grid.SetRow`](xref:Xamarin.Forms.Grid.SetRow(Xamarin.Forms.BindableObject,System.Int32)) 및 [`Grid.GetRow`](xref:Xamarin.Forms.Grid.GetRow(Xamarin.Forms.BindableObject))
- [`Grid.SetColumn`](xref:Xamarin.Forms.Grid.SetColumn(Xamarin.Forms.BindableObject,System.Int32)) 및 [`Grid.GetColumn`](xref:Xamarin.Forms.Grid.GetColumn(Xamarin.Forms.BindableObject))
- [`Grid.SetRowSpan`](xref:Xamarin.Forms.Grid.SetRowSpan(Xamarin.Forms.BindableObject,System.Int32)) 및 [`Grid.GetRowSpan`](xref:Xamarin.Forms.Grid.GetRowSpan(Xamarin.Forms.BindableObject))
- [`Grid.SetColumnSpan`](xref:Xamarin.Forms.Grid.SetColumnSpan(Xamarin.Forms.BindableObject,System.Int32)) 및 [`Grid.GetColumnSpan`](xref:Xamarin.Forms.Grid.GetColumnSpan(Xamarin.Forms.BindableObject))

XAML에서는 다음 특성을 사용하여 이러한 값을 설정합니다.

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[**SimpleGridDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) 샘플은 XAML에서 `Grid`를 만들고 초기화하는 방법을 보여줍니다.

`Grid`는 `Layout`에서 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 속성을 상속하고 행과 열 사이의 간격을 지정하는 두 개의 추가 속성을 정의합니다.

- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing)의 기본값은 6입니다.
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing)의 기본값은 6입니다.

`RowDefinitions` 및 `ColumnDefinitions` 컬렉션이 반드시 필요한 것은 아닙니다. 이 컬렉션이 없으면 `Grid`가 `Grid` 자식 요소의 행과 열을 만들고 여기에 "\*"(별표)라는 기본 `GridLength`를 지정합니다.

### <a name="the-grid-in-code"></a>코드의 Grid

[**GridCodeDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) 샘플은 코드에서 `Grid`를 만들고 채우는 방법을 보여줍니다. [Grid.IGridList<T>](xref:Xamarin.Forms.Grid.IGridList`1) 인터페이스가 정의한 [`Add`](xref:Xamarin.Forms.Grid.IGridList`1.Add*) 같은 추가 `Add` 메서드를 호출하여 각 자식 요소의 연결된 속성을 직간접적으로 설정할 수 있습니다.

### <a name="the-grid-bar-chart"></a>Grid 가로 막대형 차트

[**GridBarChart**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) 샘플에서는 대량 [`AddHorizontal`](xref:Xamarin.Forms.Grid.IGridList`1.AddHorizontal*) 메서드를 사용하여 `Grid`에 여러 개의 `BoxView` 요소를 추가하는 방법을 보여줍니다. 기본적으로 이러한 `BoxView` 요소는 너비가 동일합니다. 그러면 각 `BoxView`의 높이를 가로 막대형 차트와 비슷하게 제어할 수 있습니다.

**GridBarChart** 샘플의 `Grid`는 처음에 표시되지 않는 `Frame`을 사용하여 `AbsoluteLayout` 부모를 공유합니다. 또한 이 프로그램은 `Frame`을 사용하여 탭 모음에 대한 정보를 표시하도록 각 `BoxView`에 대한 `TapGestureRecognizer`를 설정합니다.

### <a name="alignment-in-the-grid"></a>Grid의 맞춤

[**GridAlignment**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) 샘플은 `VerticalOptions` 및 `HorizontalOptions` 속성을 사용하여 `Grid` 셀의 자식 요소를 맞추는 방법을 보여줍니다.

[**SpacingButtons**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) 샘플은 `Grid` 셀의 가운데에 있는 `Button` 요소에 동일한 간격을 지정합니다.

### <a name="cell-dividers-and-borders"></a>셀 구분선 및 테두리

`Grid`에는 셀 구분선 또는 테두리를 그리는 기능이 포함되어 있지 않습니다. 그러나 직접 만들 수 있습니다.

[**GridCellDividers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers)에서는 얇은 `BoxView` 요소에 구체적으로 추가 행과 열을 정의하여 구분선처럼 보이게 하는 방법을 보여줍니다.

[**GridCellBorders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) 프로그램은 추가 셀을 만드는 대신 각 셀의 `BoxView` 요소를 맞춰서 셀 테두리처럼 보이게 합니다.

## <a name="almost-real-life-grid-examples"></a>실제와 가까운 Grid 예

[**KeypadGrid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) 샘플에서는 `Grid`를 사용하여 키패드를 표시합니다.

[![KeypadGrid의 삼중 스크린샷](images/ch17fg12-small.png "키패드 그리드")](images/ch17fg12-large.png#lightbox "키패드 그리드")

### <a name="responding-to-orientation-changes"></a>방향 변경에 응답

`Grid`는 방향 변경에 응답하도록 프로그램을 구성하는 데 도움이 될 수 있습니다. [**GridRgbSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) 샘플에서는 세로 방향 전화의 두 번째 행과 가로 방향 전화의 두 번째 열 간에 요소를 이동하는 기법을 보여줍니다.

이 프로그램은 `Slider` 요소를 0~255 범위로 초기화하고 데이터 바인딩을 사용하여 슬라이더의 값을 16진수로 표시합니다. `Slider` 값은 부동 소수점이고 16진수에 대한 .NET 서식 지정 문자열은 정수에만 유효하므로 [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`DoubleToIntConvert`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) 클래스가 도움이 됩니다.

## <a name="related-links"></a>관련 링크

- [17장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [17장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [눈금](~/xamarin-forms/user-interface/layouts/grid.md)
