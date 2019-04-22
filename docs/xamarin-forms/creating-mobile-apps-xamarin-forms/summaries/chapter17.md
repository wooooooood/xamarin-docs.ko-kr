---
title: 요약 17 장입니다. 눈금 마스터
description: Xamarin.Forms를 사용 하 여 모바일 앱을 만듭니다. 요약 17 장입니다. 눈금 마스터
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 3aaf8e9d1eb8e0d98ad32a6b5a1286f14c7bb906
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58869990"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>요약 17 장입니다. 눈금 마스터

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)

합니다 [ `Grid` ](xref:Xamarin.Forms.Grid) 자식 셀의 행과 열으로 정렬 하는 강력한 레이아웃 메커니즘입니다. 비슷한 HTML과는 달리 `table` 요소는 `Grid` 표시가 아니라 레이아웃의 용도로 됩니다.

## <a name="the-basic-grid"></a>기본 표

`Grid` 파생 [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1)를 정의 하는 [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) 속성은 `Grid` 상속 합니다. XAML 또는 코드에서이 컬렉션을 채울 수 있습니다.

### <a name="the-grid-in-xaml"></a>XAML에서 표

정의 `Grid` XAML에서 일반적으로 채우기를 사용 하 여 시작 합니다 [ `RowDefinitions` ](xref:Xamarin.Forms.Grid.RowDefinitions) 및 [ `ColumnDefinitions` ](xref:Xamarin.Forms.Grid.ColumnDefinitions) 컬렉션을 `Grid` 사용 하 여 [ `RowDefinition` ](xref:Xamarin.Forms.RowDefinition) 하 고 [ `ColumnDefinition` ](xref:Xamarin.Forms.ColumnDefinition) 개체입니다. 이 행 개수 및 열을 설정 하는 방법의 `Grid`, 및 해당 속성입니다.

`RowDefinition` 에 [ `Height` ](xref:Xamarin.Forms.RowDefinition.Height) 속성 및 `ColumnDefinition` 에 [ `Width` ](xref:Xamarin.Forms.ColumnDefinition.Width) 형식의 두 속성인 [ `GridLength` ](xref:Xamarin.Forms.GridLength), 구조체입니다.

XAML에 [ `GridLengthTypeConverter` ](xref:Xamarin.Forms.GridLengthTypeConverter) 간단한 텍스트 문자열을 변환 `GridLength` 값입니다. 내부적으로 [ `GridLength` 생성자](xref:Xamarin.Forms.GridLength.%23ctor(System.Double,Xamarin.Forms.GridUnitType)) 만듭니다 합니다 `GridLength` 숫자와 형식의 값에 따라 값 [ `GridUnitType` ](xref:Xamarin.Forms.GridUnitType), 세 가지 멤버로 구성 된 열거형:

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; 너비 또는 높이가 장치 독립적 단위 (XAML의 숫자)에 지정 된
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; 높이 또는 너비는 셀 내용 (XAML에서 "Auto")에 따라 자동 크기 조정
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; 남은 높이 또는 너비 크기를 비례적으로 할당 됩니다 (숫자 "\*" 라는 *스타*, XAML에서)

각 자식은 `Grid` 할당 되어야 행 및 열 (명시적 또는 암시적으로). 행에 걸쳐 및 열 범위는 선택 사항입니다. 이러한 모든 지정 된 연결 된 바인딩 가능한 속성을 사용 하 여 &mdash; 에 정의 된 속성을 `Grid` 자식의 설정 하지만 `Grid`합니다. `Grid` 4 개의 정적 연결 된 바인딩 가능한 속성을 정의합니다.

- [`RowProperty`](xref:Xamarin.Forms.Grid.RowProperty) &mdash; 0부터 시작 행입니다. 기본값은 0
- [`ColumnProperty`](xref:Xamarin.Forms.Grid.ColumnProperty) &mdash; 0부터 시작 열입니다. 기본값은 0
- [`RowSpanProperty`](xref:Xamarin.Forms.Grid.RowSpanProperty) &mdash; 수의 행 자식 걸쳐; 기본값은 1
- [`ColumnSpanProperty`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) &mdash; 수의 열 자식 걸쳐; 기본값은 1

코드에서 프로그램을 설정 하 고 이러한 값을 가져오는 8 개의 정적 메서드를 따르면 됩니다.

- [`Grid.SetRow`](xref:Xamarin.Forms.Grid.SetRow(Xamarin.Forms.BindableObject,System.Int32)) 및 [`Grid.GetRow`](xref:Xamarin.Forms.Grid.GetRow(Xamarin.Forms.BindableObject))
- [`Grid.SetColumn`](xref:Xamarin.Forms.Grid.SetColumn(Xamarin.Forms.BindableObject,System.Int32)) 및 [`Grid.GetColumn`](xref:Xamarin.Forms.Grid.GetColumn(Xamarin.Forms.BindableObject))
- [`Grid.SetRowSpan`](xref:Xamarin.Forms.Grid.SetRowSpan(Xamarin.Forms.BindableObject,System.Int32)) 및 [`Grid.GetRowSpan`](xref:Xamarin.Forms.Grid.GetRowSpan(Xamarin.Forms.BindableObject))
- [`Grid.SetColumnSpan`](xref:Xamarin.Forms.Grid.SetColumnSpan(Xamarin.Forms.BindableObject,System.Int32)) 및 [`Grid.GetColumnSpan`](xref:Xamarin.Forms.Grid.GetColumnSpan(Xamarin.Forms.BindableObject))

XAML에서 이러한 값을 설정 하는 것에 대 한 다음과 같은 특성을 사용 합니다.

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

합니다 [ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) 샘플과 만들기 및 초기화를 `Grid` XAML에서.

`Grid` 상속 된 [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) 속성을 `Layout` 행 및 열 사이의 간격을 제공 하는 두 개의 추가 속성을 정의 하 고:

- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) 기본값은 6
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) 기본값은 6

합니다 `RowDefinitions` 고 `ColumnDefinitions` 컬렉션을 엄격 하 게 필요 하지 않습니다. 없는 경우는 `Grid` 행 및 열을 만듭니다 합니다 `Grid` 자식 모두 기본값을 제공 하 고 `GridLength` 의 "\*" (별표).

### <a name="the-grid-in-code"></a>코드에서 표

합니다 [ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) 샘플을 만들고 채우는 방법을 보여 줍니다는 `Grid` 코드에서입니다. 각 자식에 직접 또는 간접적으로 추가 호출 하 여 연결 된 속성을 설정할 수 있습니다 `Add` 와 같은 메서드와 [ `Add` ](xref:Xamarin.Forms.Grid.IGridList`1.Add*) 정의한 합니다 [Grid.IGridList<T> ](xref:Xamarin.Forms.Grid.IGridList`1) 인터페이스입니다.

### <a name="the-grid-bar-chart"></a>표 가로 막대형 차트

합니다 [ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) 샘플에서는 여러 개를 추가 하는 방법을 보여 줍니다 `BoxView` 요소를 사용 하는 `Grid` 대량을 사용 하 여 [ `AddHorizontal` ](xref:Xamarin.Forms.Grid.IGridList`1.AddHorizontal*) 메서드. 기본적으로 이러한 `BoxView` 요소는 동일 너비입니다. 각각의 높이 `BoxView` 가로 막대형 차트와 유사 하 게 제어할 수 있습니다.

`Grid` 에 **GridBarChart** 샘플 공유를 `AbsoluteLayout` 처음에 보이지 않는 사용 하 여 부모 `Frame`. 프로그램 설정를 `TapGestureRecognizer` 각 `BoxView` 사용 하는 `Frame` 탭된 막대에 대 한 정보를 표시 합니다.

### <a name="alignment-in-the-grid"></a>눈금에 맞춤

[ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) 샘플을 사용 하는 방법에 설명 합니다 `VerticalOptions` 및 `HorizontalOptions` 속성에서 자식에 맞게를 `Grid` 셀.

합니다 [ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) 공간을 균등 하 게 샘플 `Button` 요소 가운데에 `Grid` 셀입니다.

### <a name="cell-dividers-and-borders"></a>셀 구분선 및 테두리

`Grid` 셀 분할자 또는 테두리를 그릴 수 있는 기능을 다루지 않습니다. 그러나 가능 고유한 합니다.

합니다 [ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) 씬 추가 행과 열에 구체적으로 정의 하는 방법을 보여 줍니다 `BoxView` 기준은 모방 하는 요소입니다.

합니다 [ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) 프로그램 추가 셀 만들지는 않지만 대신 맞춥니다 `BoxView` 셀 테두리를 모방 하기 위해 각 셀에는 요소입니다.

## <a name="almost-real-life-grid-examples"></a>거의 실제 Grid 예제

합니다 [ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) 사용 하 여 샘플을 `Grid` 키패드를 표시 하려면:

[![키패드 그리드의 삼중 스크린 샷](images/ch17fg12-small.png "키패드 표")](images/ch17fg12-large.png#lightbox "키패드 표")

### <a name="responding-to-orientation-changes"></a>방향 변경에 응답

`Grid` 방향 변경에 응답 하는 프로그램을 구성 하는 데 도움이 됩니다. 합니다 [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) 샘플 세로 방향의 휴대폰의 두 번째 행에서 가로 방향 휴대폰의 두 번째 열 사이의 요소를 이동 하는 방법을 보여 줍니다.

프로그램 초기화 `Slider` 다양 한 0 ~ 255 및 슬라이더의 값을 16 진수에서 표시를 사용 하 여 데이터 바인딩 요소입니다. 때문에 합니다 `Slider` 값은 부동 소수점, 정수, 16 진수 에서만 작동에 대 한 문자열의 형식을 지정 하는.NET을 [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) out 라이브러리를 사용 하면 됩니다.



## <a name="related-links"></a>관련 링크

- [17 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [17 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [눈금](~/xamarin-forms/user-interface/layouts/grid.md)
