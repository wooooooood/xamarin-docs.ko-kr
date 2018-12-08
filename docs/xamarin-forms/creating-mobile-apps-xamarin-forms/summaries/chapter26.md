---
title: 요약 26 장입니다. 사용자 지정 레이아웃
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 26 장 요약 합니다. 사용자 지정 레이아웃'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 9fa9802f94e10612c4b0fe02c84ddcabc89820a8
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53053132"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>요약 26 장입니다. 사용자 지정 레이아웃

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)

파생 된 여러 클래스를 포함 하는 Xamarin.Forms [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout`및
* `RelativeLayout`.

이 챕터에서 파생 되는 고유한 클래스를 만드는 방법에 설명 합니다 `Layout<View>`합니다.

## <a name="an-overview-of-layout"></a>레이아웃의 개요

Xamarin.Forms 레이아웃을 처리 하는 중앙 집중식된 체제가 없는 경우 각 요소는 새로운 자체 크기 해야 및 특정 영역 내에서 자체를 렌더링 하는 방법을 결정 하는 일을 담당 합니다.

### <a name="parents-and-children"></a>부모 및 자식

자식이 있는 모든 요소는 자체 내에서 해당 자식을 배치 하는 일을 담당 합니다. 궁극적으로 자식의 크기를 결정 하는 부모 기반으로 하는 것이 크기에 사용할 수 있는 및 자식 크기 하고자 합니다.

### <a name="sizing-and-positioning"></a>크기 조정 및 위치 지정

레이아웃은 페이지를 사용 하 여 시각적 트리의 맨 위에 있는 시작 하 고 모든 분기가 진행 됩니다. 레이아웃에서 가장 중요 한 공용 메서드는 [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 정의한 `VisualElement`합니다. 모든 요소가 다른 요소 호출에 부모인 `Layout` 각 크기 및 형식의 자체를 기준으로 위치를 자식 있도록 자식에 대 한는 [ `Rectangle` ](xref:Xamarin.Forms.Rectangle) 값. 이러한 `Layout` 호출 시각적 트리를 통해 전파 합니다.

에 대 한 호출 `Layout` 화면에 표시할 요소에 대 한 필수 항목이 며 하면 다음 읽기 전용 속성을 설정 해야 합니다. 일치는 `Rectangle` 메서드에 전달 합니다.

- [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds) 형식의 `Rectangle`
- [`X`](xref:Xamarin.Forms.VisualElement.X) 형식의 `double`
- [`Y`](xref:Xamarin.Forms.VisualElement.Y) 형식의 `double`
- [`Width`](xref:Xamarin.Forms.VisualElement.Width) 형식의 `double`
- [`Height`](xref:Xamarin.Forms.VisualElement.Height) 형식의 `double`

이전에 `Layout` 를 호출 `Height` 하 고 `Width` 모의 값이 있는 &ndash;1입니다.

에 대 한 호출 `Layout` 도 다음 보호 된 메서드에 대 한 호출을 트리거합니다.

- [`SizeAllocated`](xref:Xamarin.Forms.VisualElement.SizeAllocated(System.Double,System.Double))를 호출 하는
- [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated(System.Double,System.Double))를 재정의할 수 있습니다.

마지막으로 다음 이벤트가 발생 합니다.

- [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

합니다 `OnSizeAllocated` 메서드를 재정의 하 여 `Page` 및 `Layout`, 자식을 가질 수 있는 Xamarin.Forms의 두 클래스는 합니다. 재정의 된 메서드 호출

- [`UpdateChildrenLayout`](xref:Xamarin.Forms.Page.UpdateChildrenLayout) 에 대 한 `Page` 파생물와 [ `UpdateChildrenLayout` ](xref:Xamarin.Forms.Layout.UpdateChildrenLayout) 에 대 한 `Layout` 파생 항목을 호출 하는
- [`LayoutChildren`](xref:Xamarin.Forms.Page.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 에 대 한 `Page` 파생물와 [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) 에 대 한 `Layout` 파생형입니다.

`LayoutChildren` 그런 다음 호출 `Layout` 각 자식 요소에 대 한 합니다. 새 자식이 하나 이상 있으면 `Bounds` 다음 이벤트가 발생 하는 다음을 설정 합니다.

- [`LayoutChanged`](xref:Xamarin.Forms.Page.LayoutChanged) 에 대 한 `Page` 파생물와 [ `LayoutChanged` ](xref:Xamarin.Forms.Layout.LayoutChanged) 에 대 한 `Layout` 파생형

### <a name="constraints-and-size-requests"></a>제약 조건 및 요청 크기

에 대 한 `LayoutChildren` 지능적으로 호출 하 `Layout` 모든 자식에서 알고 있어야 합니다를 *선호* 또는 *원하는* 자식에 대해 크기입니다. 따라서 호출 `Layout` 각 자식에 대 한 일반적으로 앞에 호출 하 여

- [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))

책 게시 된 후에 `GetSizeRequest` 메서드를 사용 하 여 대체 않으며

- [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))

`Measure` 메서드를 수용 합니다 [ `Margin` ](xref:Xamarin.Forms.View.Margin) 속성 형식의 인수를 포함 하 고 [ `MeasureFlag` ](xref:Xamarin.Forms.MeasureFlags), 멤버가 두:

- [`IncludeMargins`](xref:Xamarin.Forms.MeasureFlags.IncludeMargins)
- [`None`](xref:Xamarin.Forms.MeasureFlags.None) 여백을 포함 하지 않으려면

여러 요소에 대 한 `GetSizeRequest` 또는 `Measure` 해당 렌더러에에서 요소의 기본 크기를 가져옵니다. 두 방법 모두 너비와 높이 대 한 매개 변수 *제약 조건*합니다. 예를 들어, 한 `Label` 를 사용 하 여 너비 제약 조건을 여러 줄의 텍스트를 줄 바꿈 하는 방법을 결정 합니다.

둘 다 `GetSizeRequest`하 고 `Measure` 형식의 값을 반환 [ `SizeRequest` ](xref:Xamarin.Forms.SizeRequest), 두 개의 속성이 있는:

- [`Request`](xref:Xamarin.Forms.SizeRequest.Request) 형식의 `Size`
- [`Minimum`](xref:Xamarin.Forms.SizeRequest.Minimum) 형식의 `Size`

자주이 두 값은 동일 하며 `Minimum` 값을 일반적으로 무시할 수 있습니다.

`VisualElement` 또한 처럼 보호 된 메서드를 정의 `GetSizeRequest` 에서 호출 되는 `GetSizeRequest`:

- [`OnSizeRequest`](xref:Xamarin.Forms.VisualElement.OnSizeRequest(System.Double,System.Double)) 반환 된 `SizeRequest` 값

해당 메서드는 이제 사용 되지 않으며 바뀝니다.

- [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))

파생 되는 모든 클래스 `Layout` 나 `Layout<T>` 재정의 해야 `OnSizeRequest` 또는 `OnMeasure`합니다. 이 레이아웃 클래스는 일반적으로 크기를 기준으로 호출 하 여 가져온 자식에 자체 크기를 결정 하는 위치 `GetSizeRequest` 또는 `Measure` 자식에 대해 합니다. 호출 전후 `OnSizeRequest` 나 `OnMeasure`, `GetSizeRequest` 또는 `Measure` 다음 속성을 기반으로 조정 합니다.

- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)형식의 `double`에 영향을 줍니다는 `Request` 속성 `SizeRequest`
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 형식의 `double`에 영향을 줍니다는 `Request` 속성 `SizeRequest`
- [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest) 형식의 `double`에 영향을 줍니다는 `Minimum` 속성 `SizeRequest`
- [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest) 형식의 `double`에 영향을 줍니다는 `Minimum` 속성 `SizeRequest`

### <a name="infinite-constraints"></a>무한 제약 조건

제약 조건 인수에 전달 `GetSizeRequest` (또는 `Measure`) 및 `OnSizeRequest` (또는 `OnMeasure`) 무한 할 수 있습니다 (즉, 값 `Double.PositiveInfinity`). 그러나는 `SizeRequest` 이러한에서 반환 된 메서드는 무한 차원을 포함할 수 없습니다.

무한 제약 조건은 요청 된 크기를 자연스럽 게 요소의 크기를 반영 해야 나타냅니다. 세로 `StackLayout` 호출 `GetSizeRequest` (또는 `Measure`)는 무한 높이 제약 조건 사용 하 여 자식 요소에 있습니다. 호출 하는 가로 스택 레이아웃 `GetSizeRequest` (또는 `Measure`)는 무한 너비 제약 조건 사용 하 여 자식 요소에 있습니다. `AbsoluteLayout` 호출 `GetSizeRequest` (또는 `Measure`) 무한 너비 및 높이 제약 조건 사용 하 여 자식 요소에 있습니다.

### <a name="peeking-inside-the-process"></a>프로세스 내에서 보기

합니다 [ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) 표시 제약 조건 및 크기 단순형 레이아웃에 대 한 정보를 요청 합니다.

## <a name="deriving-from-layoutview"></a>레이아웃에서 파생<View>

사용자 지정 레이아웃 클래스에서 파생 되며 `Layout<View>`합니다. 두 가지 책임이 있습니다.

- 재정의 `OnMeasure` 호출할 `Measure` 레이아웃의 모든 자식에서. 자체 레이아웃에 대해 요청 된 크기를 반환 합니다.
- 재정의 `LayoutChildren` 호출할 `Layout` 레이아웃의 모든 자식에서

`for` 또는 `foreach` 루프 이러한 재정의의 모든 자식 건너뛰어야입니다 `IsVisible` 속성이 `false`합니다.

에 대 한 호출 `OnMeasure` 보장 되지 않습니다. `OnMeasure` 레이아웃의 부모는 레이아웃의 크기 (예를 들어, 페이지를 채우는 레이아웃)를 제어 하는 경우 없습니다 호출 됩니다. 이러한 이유로 `LayoutChildren` 중에 가져온 자식 크기를 사용할 수 없게 된 `OnMeasure` 호출 합니다. 많습니다 `LayoutChildren` 자체를 호출 해야 `Measure` 레이아웃의 자식에서 일종의 캐싱 논리 (나중에 설명) 크기를 구현할 수 있습니다.

### <a name="an-easy-example"></a>쉬운 예

합니다 [ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) 샘플에는 간단해 [ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) 클래스 및 해당 사용을 보여 줍니다.

### <a name="vertical-and-horizontal-positioning-simplified"></a>간소화 된 세로 및 가로 위치

작업 중 하나는 `VerticalStack` 수행 해야 하는 동안 발생는 `LayoutChildren` 재정의 합니다. 메서드는 자식의 사용 `HorizontalOptions` 자식에서 슬롯 내에 배치 하는 방법을 결정 하는 속성을 `VerticalStack`입니다. 대신 정적 메서드를 호출할 수 있습니다 [ `Layout.LayoutChildIntoBoundingRect` ](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle))합니다. 이 메서드를 호출 `Measure` 자식에 사용 하 여 해당 `HorizontalOptions` 및 `VerticalOptions` 속성을 지정된 된 사각형 내의 자식 위치 합니다.

### <a name="invalidation"></a>무효화

종종 요소의 속성 변경 요소 레이아웃에 표시 되는 방식을 영향을 줍니다. 레이아웃을 트리거하는 새 레이아웃 유효성을 검사 하지 않아야 합니다.

`VisualElement` 보호 된 메서드를 정의 [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure), 요소의 크기에 영향을 줍니다가 일반적으로 호출 하는 바인딩 가능한 속성의 속성 변경 처리기를 변경 합니다. 합니다 `InvalidateMeasure` 메서드 실행을 [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) 이벤트입니다.

`Layout` 라는 비슷한 보호 된 메서드를 정의 하는 클래스 [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout)는 `Layout` 파생 배치 및 자식의 크기를 조정 하는 방법에 영향을 주는 모든 변경에 대 한 호출 해야 합니다.

### <a name="some-rules-for-coding-layouts"></a>레이아웃 코딩에 대 한 몇 가지 규칙

1. 속성에서 정의한 `Layout<T>` 파생형 바인딩 가능한 속성에 의해 백업 해야 하 고 속성 변경 처리기를 호출 해야 `InvalidateLayout`합니다.

2. `Layout<T>` 연결 된 바인딩 가능한 속성을 정의 하는 파생 클래스에서 재정의 해야 [ `OnAdded` ](xref:Xamarin.Forms.Layout`1.OnAdded*) 자식에 속성 변경 처리기를 추가 하 고 [ `OnRemoved` ](xref:Xamarin.Forms.Layout`1.OnRemoved*) 는 제거할 처리기입니다. 이러한 변경 내용을 연결 된 바인딩 가능한 속성에 대 한 확인 처리기를 호출 하 여 응답 `InvalidateLayout`합니다.

3. A `Layout<T>` 자식 크기의 캐시를 구현 하는 파생 클래스에서 재정의 해야 `InvalidateLayout` 하 고 [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) 이러한 메서드가 호출 되 면 캐시의 선택을 취소 하 고 있습니다.

### <a name="a-layout-with-properties"></a>속성을 사용 하 여 레이아웃

합니다 [ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 모든 자식의 크기 집합과에 하나의 행 (또는 열의)에서 자식을 래핑하는 것으로 가정 다음입니다. 정의 `Orientation` 처럼 속성 `StackLayout`, 및 `ColumnSpacing` 하 고 `RowSpacing` 등의 속성 `Grid`, 자식 크기 캐시 및 합니다.

[ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) put 샘플를 `WrapLayout` 에 `ScrollView` 주식 사진 표시에 대 한 합니다.

### <a name="no-unconstrained-dimensions-allowed"></a>허용 제한 차원이 없습니다.

합니다 [ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) 에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리 자체 내에서 모든 자식을 표시 됩니다. 따라서 비제한 차원을 처리할 수 없습니다 하 고 발생 한 경우 예외가 발생 합니다.

합니다 [ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) 샘플과 `UniformGridLayout`:

[![사진 그리드의 삼중 스크린 샷](images/ch26fg08-small.png "Uniform 격자 레이아웃")](images/ch26fg08-large.png#lightbox "균일 한 모눈 레이아웃")

### <a name="overlapping-children"></a>겹치는 자식

`Layout<T>` 파생 자식 겹칠 수 있습니다. 그러나 자식에서 순서 대로 렌더링 됩니다 합니다 `Children` 컬렉션과 나타나는 순서가 아니라 해당 `Layout` 메서드를 호출 합니다.

`Layout` 클래스의 자식 컬렉션 내에서 이동할 수 있도록 두 개의 메서드를 정의 합니다.

- [`LowerChild`](xref:Xamarin.Forms.Layout.LowerChild(Xamarin.Forms.View)) 자식 컬렉션의 시작 부분으로 이동 하려면
- [`RaiseChild`](xref:Xamarin.Forms.Layout.RaiseChild(Xamarin.Forms.View)) 자식 컬렉션의 끝에 이동 하려면

겹치는 자식에 대 한 자식 컬렉션의 끝에 시각적으로 자식 컬렉션의 시작 부분 맨 위에 표시 합니다.

합니다 [ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 렌더링 순서를 나타내며 따라서 중 하나를 허용 하는 연결된 속성을 정의 하는 라이브러리의 다른 위에 표시할 자식입니다. 합니다 [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) 샘플에서는이 방법을 보여 줍니다.

[![학생 카드 파일 그리드의 삼중 스크린 샷](images/ch26fg10-small.png "레이아웃 자식 겹치는")](images/ch26fg10-large.png#lightbox "겹치는 레이아웃 자식")

### <a name="more-attached-bindable-properties"></a>자세한 내용은 연결 된 바인딩 가능한 속성

합니다 [ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) 클래스를 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리는 두 개를 지정 하는 연결 된 바인딩 가능한 속성 정의 `Point` 값 및 두께 값 및 조작 `BoxView` 요소 줄을 유사 하 게 합니다.

합니다 [ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) 샘플 그리려면 3D 큐브는 메시지를 표시 합니다.

### <a name="layout-and-layoutto"></a>레이아웃 및 LayoutTo

A `Layout<T>` 파생 클래스를 호출할 수 있습니다 `LayoutTo` 대신 `Layout` 레이아웃에 애니메이션 효과를 합니다. 합니다 [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) 클래스는이 작업을 수행 하며 [ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) 샘플 이것을 보여 줍니다.



## <a name="related-links"></a>관련 링크

- [26 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [26 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [사용자 지정 레이아웃 만들기](~/xamarin-forms/user-interface/layouts/custom.md)
