---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 26. Custom layouts''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: deb46d1a70e7c707c998be8669b4af3b8e8d7ead
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136606"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>요약 - 26장. 사용자 지정 레이아웃

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)

Xamarin.Forms는 [`Layout<View>`](xref:Xamarin.Forms.Layout`1)에서 파생된 여러 클래스를 포함합니다.

- `StackLayout`,
- `Grid`,
- `AbsoluteLayout` 및
- `RelativeLayout`.

이 장에서는 `Layout<View>`에서 파생되는 고유한 클래스를 만드는 방법을 설명합니다.

## <a name="an-overview-of-layout"></a>레이아웃 개요

Xamarin.Forms 레이아웃을 처리하는 중앙 집중식 시스템은 없습니다. 각 요소는 자체 크기와 특정 영역에서 렌더링되는 방법을 결정해야 합니다.

### <a name="parents-and-children"></a>부모 및 자식

자식이 있는 모든 요소는 해당 자식을 내부에 배치해야 합니다. 부모는 사용 가능한 크기와 원하는 자식의 크기를 기준으로 최종적으로 자식 크기를 결정합니다.

### <a name="sizing-and-positioning"></a>크기 및 위치 지정

레이아웃은 페이지를 사용하여 시각적 트리 위쪽에서 시작하여 모든 분기를 진행합니다. 레이아웃에서 가장 중요한 공용 메서드는 `VisualElement`로 정의되는 [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle))입니다. 다른 요소의 부모인 모든 요소는 각 자식에 대해 `Layout`을 호출하여 [`Rectangle`](xref:Xamarin.Forms.Rectangle) 값의 형태로 부모에 상대적인 크기 및 위치를 자식에 지정합니다. 이러한 `Layout` 호출은 시각적 트리를 통해 전파됩니다.

`Layout` 호출은 요소를 화면에 표시하는 데 필요하며, 다음과 같은 읽기 전용 속성을 설정합니다. 이들은 메서드에 전달된 `Rectangle`과 일관됩니다.

- [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds)(`Rectangle` 형식)
- [`X`](xref:Xamarin.Forms.VisualElement.X)(`double` 형식)
- [`Y`](xref:Xamarin.Forms.VisualElement.Y)(`double` 형식)
- [`Width`](xref:Xamarin.Forms.VisualElement.Width)(`double` 형식)
- [`Height`](xref:Xamarin.Forms.VisualElement.Height)(`double` 형식)

`Layout` 호출 전에 `Height`와 `Width`에는 모의 값 &ndash;1이 있습니다.

또한 `Layout` 호출은 다음과 같은 보호된 메서드에 대한 호출을 트리거합니다.

- [`SizeAllocated`](xref:Xamarin.Forms.VisualElement.SizeAllocated(System.Double,System.Double)). 이 메서드는
- 재정의될 수 있는 [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated(System.Double,System.Double))를 호출합니다.

마지막으로 다음 이벤트가 발생합니다.

- [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

`OnSizeAllocated` 메서드는 `Page` 및 `Layout`에 의해 재정의됩니다. 이들은 Xamarin.Forms에서 유일하게 자식을 포함할 수 있는 두 클래스입니다. 재정의된 메서드는

- `Page` 파생 항목에 대해 [`UpdateChildrenLayout`](xref:Xamarin.Forms.Page.UpdateChildrenLayout)를 호출하고 `Layout` 파생 항목에 대해 [`UpdateChildrenLayout`](xref:Xamarin.Forms.Layout.UpdateChildrenLayout)를 호출합니다. 이는
- `Page` 파생 항목에 대해 [`LayoutChildren`](xref:Xamarin.Forms.Page.LayoutChildren(System.Double,System.Double,System.Double,System.Double))을 호출하고, `Layout` 파생 항목에 대해 [`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double))을 호출합니다.

그런 다음 `LayoutChildren`은 각 요소의 자식에 대해 `Layout`을 호출합니다. 하나 이상의 자식에 새로운 `Bounds` 설정이 있으면 다음 이벤트가 발생합니다.

- `Page` 파생 항목에 대해 [`LayoutChanged`](xref:Xamarin.Forms.Page.LayoutChanged), `Layout` 파생 항목에 대해 [`LayoutChanged`](xref:Xamarin.Forms.Layout.LayoutChanged)

### <a name="constraints-and-size-requests"></a>제약 조건 및 크기 요청

`LayoutChildren`이 모든 자식에 대해 `Layout`을 지능적으로 호출하려면 자식에 대한 *기본 크기* 또는 *원하는* 크기를 알고 있어야 합니다. 따라서 각 자식에 대한 `Layout` 호출은 일반적으로 다음에 대한 호출 뒤에 옵니다.

- [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))

이 책이 출판된 후 `GetSizeRequest` 메서드는 더 이상 사용되지 않으며 다음으로 대체되었습니다.

- [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))

`Measure` 메서드는 [`Margin`](xref:Xamarin.Forms.View.Margin) 속성을 수용하고 두 개의 멤버가 있는 [`MeasureFlag`](xref:Xamarin.Forms.MeasureFlags) 형식의 인수를 포함합니다.

- [`IncludeMargins`](xref:Xamarin.Forms.MeasureFlags.IncludeMargins)
- [`None`](xref:Xamarin.Forms.MeasureFlags.None) - 여백을 포함하지 않음

많은 요소에 대해 `GetSizeRequest` 또는 `Measure`는 렌더러에서 요소의 기본 크기를 가져옵니다. 두 메서드에는 너비 및 높이 *제약 조건*에 대한 매개 변수가 있습니다. 예를 들어 `Label`은 너비 제약 조건을 사용하여 여러 줄의 텍스트를 래핑하는 방법을 결정합니다.

`GetSizeRequest`와 `Measure`는 모두 두 개의 속성을 포함하는 [`SizeRequest`](xref:Xamarin.Forms.SizeRequest) 형식의 값을 반환합니다.

- [`Request`](xref:Xamarin.Forms.SizeRequest.Request)(`Size` 형식)
- [`Minimum`](xref:Xamarin.Forms.SizeRequest.Minimum)(`Size` 형식)

이 두 값이 동일하고 `Minimum` 값이 일반적으로 무시될 수 있는 경우가 매우 많습니다.

또한 `VisualElement`는 `GetSizeRequest`에서 호출되는 `GetSizeRequest`와 유사한 보호된 메서드도 정의합니다.

- [`OnSizeRequest`](xref:Xamarin.Forms.VisualElement.OnSizeRequest(System.Double,System.Double))는 `SizeRequest` 값을 반환합니다.

이 메서드는 더 이상 사용되지 않으며 다음으로 대체되었습니다.

- [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))

`Layout` 또는 `Layout<T>`에서 파생되는 모든 클래스는 `OnSizeRequest` 또는 `OnMeasure`를 재정의해야 합니다. 이는 레이아웃 클래스가 자체 크기를 결정하는 메서드로, 이 크기는 일반적으로 자식에 대해 `GetSizeRequest` 또는 `Measure`를 호출하여 가져오는 자식의 크기를 기준으로 합니다. `OnSizeRequest` 또는 `OnMeasure`를 호출하기 전과 후에 `GetSizeRequest` 또는 `Measure`는 다음 속성에 따라 조정을 수행합니다.

- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)(`double` 형식)는 `SizeRequest`의 `Request` 속성에 영향을 줍니다.
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)(`double` 형식)는 `SizeRequest`의 `Request` 속성에 영향을 줍니다.
- [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest)(`double` 형식)는 `SizeRequest`의 `Minimum` 속성에 영향을 줍니다.
- [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest)(`double` 형식)는 `SizeRequest`의 `Minimum` 속성에 영향을 줍니다.

### <a name="infinite-constraints"></a>무한 제약 조건

`GetSizeRequest`(또는 `Measure`) 및 `OnSizeRequest`(또는 `OnMeasure`)에 전달되는 제약 조건 인수는 무한(즉, `Double.PositiveInfinity` 값)일 수 있습니다. 그러나 이러한 메서드에서 반환되는 `SizeRequest`는 무한 차원을 포함할 수 없습니다.

무한 제약 조건은 요청된 크기가 요소의 기본 크기를 반영해야 함을 나타냅니다. 세로 `StackLayout`는 무한 높이 제약 조건을 사용하여 자식에 대해 `GetSizeRequest`(또는 `Measure`)를 호출합니다. 가로 스택 레이아웃은 무한 너비 제약 조건을 사용하여 해당 자식에 대해 `GetSizeRequest`(또는 `Measure`)를 호출합니다. `AbsoluteLayout`는 무한 너비 및 높이 제약 조건을 사용하여 자식에 대해 `GetSizeRequest`(또는 `Measure`)를 호출합니다.

### <a name="peeking-inside-the-process"></a>프로세스 내부 보기

[**ExploreChildSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes)는 간단한 레이아웃에 대한 제약 조건 및 크기 요청 정보를 표시합니다.

## <a name="deriving-from-layoutview"></a>Layout\<View>에서 파생

사용자 지정 레이아웃 클래스는 `Layout<View>`에서 파생됩니다. 이 클래스에는 두 가지 책임이 있습니다.

- 모든 레이아웃의 자식에 대해 `Measure`를 호출하도록 `OnMeasure`를 재정의합니다. 레이아웃 자체에 대해 요청된 크기를 반환합니다.
- 모든 레이아웃의 자식에 대해 `Layout`을 호출하도록 `LayoutChildren`을 재정의합니다.

이러한 재정의에서 `for` 또는 `foreach` 루프는 `IsVisible` 속성이 `false`로 설정된 모든 자식을 건너뛰어야 합니다.

`OnMeasure` 호출은 보장되지 않습니다. 레이아웃의 부모가 레이아웃의 크기를 관리하는 경우(예: 페이지를 채우는 레이아웃) `OnMeasure`는 호출되지 않습니다. 따라서 `LayoutChildren`은 `OnMeasure` 호출 중에 가져온 자식 크기를 사용할 수 없습니다. 경우에 따라 `LayoutChildren` 자체가 레이아웃의 자식에 대해 `Measure`를 호출해야 하거나, 사용자가 일부 종류의 크기 캐싱 논리(나중에 설명)를 구현할 수 있습니다.

### <a name="an-easy-example"></a>간단한 예제

[**VerticalStackDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) 샘플에는 단순화된 [`VerticalStack`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) 클래스 및 사용법 데모가 포함되어 있습니다.

### <a name="vertical-and-horizontal-positioning-simplified"></a>수직 및 수평 배치 단순화

`VerticalStack`이 수행해야 하는 작업 중 하나는 `LayoutChildren`을 재정의하는 동안 발생합니다. 이 메서드는 자식의 `HorizontalOptions` 속성을 사용하여 `VerticalStack`의 슬롯에 자식을 배치하는 방법을 결정합니다. 대신 정적 메서드 [`Layout.LayoutChildIntoBoundingRect`](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle))을 호출할 수 있습니다. 이 메서드는 자식에 대해 `Measure`를 호출하고 `HorizontalOptions` 및 `VerticalOptions` 속성을 사용하여 지정된 사각형 안에 자식을 배치합니다.

### <a name="invalidation"></a>무효화

일반적으로 요소의 속성을 변경하면 해당 요소가 레이아웃에 표시되는 방식에 영향을 줍니다. 새 레이아웃을 트리거하기 위해 기존 레이아웃을 무효화해야 합니다.

`VisualElement`는 보호된 메서드 [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure)를 정의합니다. 이 메서드는 해당 변경 내용이 요소의 크기에 영향을 주는 바인딩 가능한 속성의 속성 변경 처리기에 의해 일반적으로 호출됩니다. `InvalidateMeasure` 메서드는 [`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) 이벤트를 발생시킵니다.

`Layout` 클래스는 [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout)이라는 유사한 보호된 메서드를 정의합니다. 이 메서드는 `Layout` 파생 항목이 자식의 위치 및 크기에 영향을 주는 모든 변경 내용에 대해 호출해야 합니다.

### <a name="some-rules-for-coding-layouts"></a>레이아웃 코딩에 대한 몇 가지 규칙

1. `Layout<T>` 파생 항목이 정의하는 속성은 바인딩 가능한 속성에 의해 지원되고, 속성 변경 처리기는 `InvalidateLayout`을 호출해야 합니다.

2. 연결된 바인딩 가능한 속성을 정의하는 `Layout<T>` 파생 항목은 [`OnAdded`](xref:Xamarin.Forms.Layout`1.OnAdded*)를 재정의하여 속성 변경 처리기를 해당 자식과 [`OnRemoved`](xref:Xamarin.Forms.Layout`1.OnRemoved*)에 추가하여 해당 처리기를 제거해야 합니다. 처리기는 이러한 연결된 바인딩 가능한 속성의 변경 내용을 확인하고 `InvalidateLayout`을 호출하여 응답해야 합니다.

3. 자식 크기의 캐시를 구현하는 `Layout<T>` 파생 항목은 `InvalidateLayout` 및 [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated)를 재정의하고 이러한 메서드가 호출될 때 캐시를 지워야 합니다.

### <a name="a-layout-with-properties"></a>속성이 있는 레이아웃

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)에서 [`WrapLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) 클래스는 모든 자식을 동일한 크기로 가정하고 자식을 한 행(또는 열)에서 다음 행(또는 열)으로 래핑합니다. 이 클래스는 `StackLayout` 같은 `Orientation` 속성을 정의하고 `Grid` 같은 `ColumnSpacing` 및 `RowSpacing` 속성을 정의하며, 자식 크기를 캐시합니다.

[**PhotoWrap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) 샘플에서는 재고 사진을 표시하기 위해 `WrapLayout`에 `ScrollView`을 배치합니다.

### <a name="no-unconstrained-dimensions-allowed"></a>비제한 차원은 허용되지 않습니다.

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`UniformGridLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs)은 내부의 모든 자식을 표시하기 위한 것입니다. 따라서 비제한 차원을 처리할 수 없으며 비제한 차원이 있을 경우 예외가 발생합니다.

[**PhotoGrid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) 샘플에서는 `UniformGridLayout`을 보여 줍니다.

[![사진 그리드의 삼중 스크린샷](images/ch26fg08-small.png "균일 눈금 레이아웃")](images/ch26fg08-large.png#lightbox "균일 눈금 레이아웃")

### <a name="overlapping-children"></a>겹치는 자식

`Layout<T>` 파생 항목은 자식을 겹칠 수 있습니다. 그러나 자식은 `Layout` 메서드가 호출되는 순서가 아니라 `Children` 컬렉션 내 순서대로 렌더링됩니다.

`Layout` 클래스는 컬렉션 내에서 자식을 이동할 수 있는 두 가지 메서드를 정의합니다.

- [`LowerChild`](xref:Xamarin.Forms.Layout.LowerChild(Xamarin.Forms.View)) - 자식을 컬렉션의 시작 부분으로 이동
- [`RaiseChild`](xref:Xamarin.Forms.Layout.RaiseChild(Xamarin.Forms.View)) - 자식을 컬렉션의 끝부분으로 이동

겹치는 자식의 경우 컬렉션의 끝부분에 있는 자식은 시각적으로 컬렉션의 시작 부분에 표시됩니다.

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`OverlapLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) 클래스는 렌더링 순서를 나타내고 따라서 해당 자식 중 하나를 다른 항목 위에 표시하도록 허용하는 연결된 속성을 정의합니다. [**StudentCardFile**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) 샘플에서는 이를 보여 줍니다.

[![학생 카드 파일 그리드의 삼중 스크린샷](images/ch26fg10-small.png "겹치는 레이아웃 자식")](images/ch26fg10-large.png#lightbox "겹치는 레이아웃 자식")

### <a name="more-attached-bindable-properties"></a>더 많은 연결된 바인딩 가능한 속성

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리의 [`CartesianLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) 클래스는 연결된 바인딩 가능한 속성을 정의하여 두 개의 `Point` 값과 두께 값을 지정하고 `BoxView` 요소를 조작하여 줄을 유사하게 변경합니다.

[**UnitCube**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) 샘플에서는 이 속성을 사용하여 3D 큐브를 그립니다.

### <a name="layout-and-layoutto"></a>Layout 및 LayoutTo

`Layout<T>` 파생 항목은 `Layout`이 아닌 `LayoutTo`를 호출하여 레이아웃을 애니메이션화할 수 있습니다. [`AnimatedCartesianLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) 클래스가 이 작업을 수행하고 [**AnimatedUnitCube**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) 샘플에서는 이를 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [26장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [26장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [사용자 지정 레이아웃 만들기](~/xamarin-forms/user-interface/layouts/custom.md)
