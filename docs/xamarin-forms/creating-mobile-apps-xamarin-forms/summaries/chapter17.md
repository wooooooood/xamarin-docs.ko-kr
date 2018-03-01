---
title: "17 장의 요약입니다. 표를 마스터합니다."
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c0a184b80b57980c7ae00572517ad52a18b5755c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>17 장의 요약입니다. 표를 마스터합니다.

[ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) 는 셀의 행과 열에 배열 하는 강력한 레이아웃 메커니즘입니다. 유사한 HTML 달리 `table` 요소는 `Grid` 표시가 아니라 레이아웃의 목적 으로만 합니다.

## <a name="the-basic-grid"></a>기본 표

`Grid` 파생 [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/), 정의 하는 [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) 속성은 `Grid` 상속 합니다. XAML 또는 코드에서이 컬렉션을 채울 수 있습니다.

### <a name="the-grid-in-xaml"></a>XAML에서 표

정의 `Grid` XAML에서 일반적으로 채우기로 시작는 [ `RowDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/) 및 [ `ColumnDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/) 의 컬렉션은 `Grid` 와 [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) 및 [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/) 개체입니다. 이 행 개수 및 열을 설정 하는 방법의 `Grid`, 및 해당 속성입니다.

`RowDefinition` 에 [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/) 속성 및 `ColumnDefinition` 에 [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/) 속성 형식의 둘 다 [ `GridLength` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/)는 구조입니다.

Xaml에서는 [ `GridLengthTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/) 간단한 텍스트 문자열을 변환 `GridLength` 값입니다. 내부적으로 [ `GridLength` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/) 만듭니다는 `GridLength` 숫자와 형식의 값에 따라 값 [ `GridUnitType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/), 세 멤버가 포함 된 열거:

- [`Absolute`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Absolute/) & #x 2014; 너비 또는 높이 장치 독립적 단위 (XAML의 번호)에 지정 된
- [`Auto`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Auto/) & #x 2014; height 또는 width가 셀 내용 (XAML에서 "자동")에 따라 창의 자동 크기 조정
- [`Star`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Star/) & #x 2014; 남은 높이 또는 너비 비례적으로 할당 됩니다 (사용 하 여 숫자 "\*" 이라는 *별모양*, XAML에서)

각 자식은 `Grid` 할당 되어야 행 및 열 (명시적 또는 암시적으로). 행에 걸쳐 및 열 범위는 선택 사항입니다. 이러한 모든 지정 된 바인딩 가능한 속성 & #x 2014; 연결 된 사용 하 여 정의한 속성은 `Grid` 의 자식을에서 설정 하지만 `Grid`합니다. `Grid` 4 개의 정적 연결 된 바인딩 가능한 속성을 정의합니다.

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) & #x 2014; 행 0부터 시작 합니다. 기본값은 0
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) & #x 2014; 0부터 시작 열입니다. 기본값은 0
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) & #x 2014; 수의 행 자식 걸쳐; 기본값은 1
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) & #x 2014; 수의 열 자식 걸쳐; 기본값은 1

프로그램 코드에서 설정 하 고 이러한 값을 가져오는 8 개의 정적 메서드를 사용할 수 있습니다.

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) 및 [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) 및 [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) 및 [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) 및 [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

XAML에서 이러한 값을 설정 하기 위한 다음과 같은 특성을 사용 하면:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) 샘플 만들고 초기화 하는 `Grid` XAML에서 합니다.

`Grid` 상속는 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) 속성 `Layout` 행과 열 사이의 간격을 제공 하는 두 개의 추가 속성을 정의 하 고:

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) 기본값은 6에
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) 기본값은 6에

`RowDefinitions` 및 `ColumnDefinitions` 컬렉션 엄격 하 게 필요가 없습니다. 없는 경우는 `Grid` 위한 행과 열을 만듭니다는 `Grid` 자식 항목을 만들고 모든 기본 `GridLength` 의 "\*" (별표).

### <a name="the-grid-in-code"></a>코드에서 표

[ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) 샘플을 만들고 채우는 방법을 보여 줍니다는 `Grid` 코드에서입니다. 한 각 자식에 직접 또는 간접적으로 호출 추가 하 여 연결 된 속성을 설정할 수 있습니다 `Add` 와 같은 메서드 [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) 정의한는 [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) 인터페이스입니다.

### <a name="the-grid-bar-chart"></a>표의 가로 막대형 차트

[ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) 샘플에서는 여러 개 추가 하는 방법을 보여 줍니다. `BoxView` 요소를 사용 하는 `Grid` 대량을 사용 하 여 [ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) 메서드. 기본적으로 이러한 `BoxView` 요소는 동일 너비입니다. 각각의 높이 `BoxView` 가로 막대형 차트를 유사 하 게 제어할 수 있습니다.

`Grid` 에 **GridBarChart** 샘플 공유는 `AbsoluteLayout` 처음 보이지 않는 인 상위 `Frame`합니다. 또한 설정 되므로 `TapGestureRecognizer` 각 `BoxView` 사용 하는 `Frame` 탭된 모음에 대 한 정보를 표시 합니다.

### <a name="alignment-in-the-grid"></a>눈금에 맞춤

[ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) 샘플에 사용 하는 방법을 보여 줍니다는 `VerticalOptions` 및 `HorizontalOptions` 의 자식에 맞게 속성을 `Grid` 셀입니다.

[ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) 샘플 동일 하 게 공백을 `Button` 요소 가운데에 맞춰지고 `Grid` 셀입니다.

### <a name="cell-dividers-and-borders"></a>셀 구분선 및 테두리

`Grid` 셀 분할자 또는 테두리를 그릴 수 있는 새로운 기능이 포함 되어 있지 않습니다. 그러나 가능 직접 합니다.

[ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) 씬 추가 행 및 열에 대해 구체적으로 정의 하는 방법을 보여 줍니다. `BoxView` 요소를 구분 하는 기준은 모방 합니다.

[ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) 프로그램 추가 셀을 만들지 않고 하지만 대신 맞춥니다 `BoxView` 셀 테두리를 모방 하기 위해 각 셀에 있는 요소입니다.

## <a name="almost-real-life-grid-examples"></a>거의 실전 표 예제

[ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) 샘플에서는 `Grid` 키패드를 표시 하려면:

[![키패드 눈금의 삼중 스크린 샷](images/ch17fg12-small.png "키패드 그리드")](images/ch17fg12-large.png "키패드 표")

### <a name="responding-to-orientation-changes"></a>방향 변경 내용에 응답

`Grid` 방향 변경에 응답 하는 프로그램을 구성 하는 데 도움이 됩니다. [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) 샘플은 세로 방향의 휴대폰의 두 번째 행은 가로 방향 휴대폰의 두 번째 열 간에 요소를 이동 하는 기술을 보여 줍니다.

프로그램 초기화 `Slider` 0 ~ 255 및 슬라이더의 값을 16 진수에서 표시를 사용 하 여 데이터 바인딩을의 다양 한 요소입니다. 때문에 `Slider` 값은 부동 소수점, 및 16 진수 정수, 에서만 작동에 대 한 문자열의 서식을 지정 하는.NET은 [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리를 사용 합니다.



## <a name="related-links"></a>관련 링크

- [17 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [17 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [눈금](~/xamarin-forms/user-interface/layouts/grid.md)
