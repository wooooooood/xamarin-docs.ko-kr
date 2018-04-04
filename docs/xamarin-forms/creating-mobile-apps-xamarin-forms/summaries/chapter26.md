---
title: 요약 장 26입니다. 사용자 지정 레이아웃
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 5d2dc3e809026a36d186c9a582fcd047f6be24fe
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-26-custom-layouts"></a>요약 장 26입니다. 사용자 지정 레이아웃

파생 된 여러 클래스를 포함 하는 Xamarin.Forms [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout`및
* `RelativeLayout`.

이 장에서 설명에서 파생 되는 사용자 고유의 클래스를 만드는 방법 `Layout<View>`합니다.

## <a name="an-overview-of-layout"></a>레이아웃의 개요

Xamarin.Forms 레이아웃을 처리 하는 중앙 집중식된 시스템이 없습니다 있습니다. 각 요소는 자체 크기 해야 하는지, 및 특정 영역 내에서 자체를 렌더링 하는 방법을 결정 합니다.

### <a name="parents-and-children"></a>부모 및 자식

자식이 있는 모든 요소는 이러한 자식도 자체 내 위치를 지정 하는 일을 담당 합니다. 가 자식의 크기를 결정 하는 부모의 기준이 되는 크기에 사용할 수 있는 기능이 있으며 크기 자식 수 있도록 하려고 합니다.

### <a name="sizing-and-positioning"></a>크기 조정 및 위치 지정

레이아웃은 페이지와 시각적 트리 맨 위에 있는 시작 되 고 모든 분기 진행 됩니다. 레이아웃에서 가장 중요 한 공용 메서드는 [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) 정의한 `VisualElement`합니다. 다른 요소 호출이 부모가 있는 모든 요소 `Layout` 각 크기와의 형태로 자체에 상대적인 위치를 자식에 게 자식에 대 한는 [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) 값입니다. 이러한 `Layout` 호출의 표시 트리를 통해 전파 합니다.

에 대 한 호출 `Layout` 화면에 표시할 요소에 대 한 필요 하 고 하면 다음 읽기 전용 속성을 설정 해야 합니다. 일치는 `Rectangle` 메서드에 전달 합니다.

- [`Bounds`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) 형식의 `Rectangle`
- [`X`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.X/) 형식의 `double`
- [`Y`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Y/) 형식의 `double`
- [`Width`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) 형식의 `double`
- [`Height`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) 형식의 `double`

이전에 `Layout` 호출, `Height` 및 `Width` 모의 값이 있는 &ndash;1입니다.

에 대 한 호출 `Layout` 다음 보호 된 메서드 호출을 트리거합니다.

- [`SizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.SizeAllocated/p/System.Double/System.Double/)되는 호출
- [`OnSizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeAllocated/p/System.Double/System.Double/)를 재정의할 수 있습니다.

마지막으로, 다음 이벤트가 발생 합니다.

- [`SizeChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/)

`OnSizeAllocated` 메서드는 `Page` 및 `Layout`, 두 개의 클래스인 xamarin.forms 자식을 포함할 수 있습니다. 재정의 된 메서드 호출

- [`UpdateChildrenLayout`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.UpdateChildrenLayout()/) 에 대 한 `Page` 파생 항목 및 [ `UpdateChildrenLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.UpdateChildrenLayout()/) 에 대 한 `Layout` 호출 하는 파생 항목
- [`LayoutChildren`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) 에 대 한 `Page` 파생 항목 및 [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) 에 대 한 `Layout` 파생 항목입니다.

`LayoutChildren` 그런 다음 호출 `Layout` 각 자식 요소에 대 한 합니다. 하나 이상의 자식에 있는 새 경우 `Bounds` 다음 이벤트가 설정한 다음 설정:

- [`LayoutChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.LayoutChanged/) 에 대 한 `Page` 파생 항목 및 [ `LayoutChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Layout.LayoutChanged/) 에 대 한 `Layout` 파생 항목

### <a name="constraints-and-size-requests"></a>제약 조건 및 크기 요청

에 대 한 `LayoutChildren` 를 지능적으로 호출 `Layout` 모든 자식을에서 알고 있어야는 *기본* 또는 *원하는* 의 자식에 대 한 크기입니다. 따라서에 대 한 호출 `Layout` 각 자식에 대 한 일반적으로 다음에 오는를 호출 하 여

- [`GetSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/)

책 게시 된 후에 `GetSizeRequest` 메서드를 더 이상 사용 되지 않으며로 대체

- [`Measure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)

`Measure` 메서드 수용는 [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) 속성 형식의 인수를 포함 하 고 [ `MeasureFlag` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MeasureFlags/), 멤버가 두 개:

- [`IncludeMargins`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.IncludeMargins/)
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.None/) 여백을 포함 하지 않도록

많은 요소에 대 한 `GetSizeRequest` 또는 `Measure` 해당 렌더러에에서 요소의 기본 크기를 가져옵니다. 두 방법 모두 너비와 높이 대 한 매개 변수 *제약 조건을*합니다. 예를 들어 한 `Label` 너비 제약 조건을 여러 줄의 텍스트를 래핑하는 방법을 결정 하기 위해 사용 됩니다.

둘 다 `GetSizeRequest`및 `Measure` 형식의 값을 반환 [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/), 두 속성이 포함:

- [`Request`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Request/) 형식의 `Size`
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Minimum/) 형식의 `Size`

자주이 두 값은 동일 및 `Minimum` 값을 일반적으로 무시할 수 있습니다.

`VisualElement` 도 유사한 보호 된 메서드를 정의 `GetSizeRequest` 에서 호출 되는 `GetSizeRequest`:

- [`OnSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeRequest/p/System.Double/System.Double/) 반환 된 `SizeRequest` 값

해당 메서드는 이제 사용 되지 않으며 아래 템플릿으로 바뀝니다.

- [`OnMeasure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)

파생 되는 모든 클래스는 `Layout` 또는 `Layout<T>` 재정의 해야 `OnSizeRequest` 또는 `OnMeasure`합니다. 이 레이아웃 클래스 일반적으로 호출 하 여 얻을 수 있는 자식의 크기를 기반으로 하는 자체 크기를 결정 하는 위치는 `GetSizeRequest` 또는 `Measure` 자식에 대해 합니다. 호출 전후 `OnSizeRequest` 또는 `OnMeasure`, `GetSizeRequest` 또는 `Measure` 다음 속성에 따라 조정 합니다.

- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)형식의 `double`, 영향을 줍니다는 `Request` 속성 `SizeRequest`
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) 형식의 `double`, 영향을 줍니다는 `Request` 속성 `SizeRequest`
- [`MinimumWidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumWidthRequest/) 형식의 `double`, 영향을 줍니다는 `Minimum` 속성 `SizeRequest`
- [`MinimumHeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumHeightRequest/) 형식의 `double`, 영향을 줍니다는 `Minimum` 속성 `SizeRequest`

### <a name="infinite-constraints"></a>무한 제약 조건

제약 조건 인수에 전달 된 `GetSizeRequest` (또는 `Measure`) 및 `OnSizeRequest` (또는 `OnMeasure`) 무한 할 수 (즉, 값의 `Double.PositiveInfinity`). 그러나는 `SizeRequest` 이 항목에서 반환 된 메서드는 무한 차원을 포함할 수 없습니다.

무한 제약 조건에 요청 된 크기의 효과 자연 크기를 반영 해야 나타냅니다. 세로 `StackLayout` 호출 `GetSizeRequest` (또는 `Measure`) 무한 높이 제약 조건으로 자식 요소에 있습니다. 호출 하는 가로 스택 레이아웃 `GetSizeRequest` (또는 `Measure`) 무한대의 너비 제약 조건으로 자식 요소에 있습니다. `AbsoluteLayout` 호출 `GetSizeRequest` (또는 `Measure`) 무한 너비와 높이 제약 조건 가진 자식 요소에 있습니다.

### <a name="peeking-inside-the-process"></a>프로세스 내 보기

[ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) 표시 제약 조건 및 크기 단순한 레이아웃에 대 한 정보를 요청 합니다.

## <a name="deriving-from-layoutview"></a>레이아웃에서 파생<View>

사용자 지정 레이아웃 클래스에서 파생 `Layout<View>`합니다. 여기에 두 가지가 있습니다.

- 재정의 `OnMeasure` 호출할 `Measure` 레이아웃의 모든 자식에서 해당 합니다. 자체 레이아웃에 대 한 요청 된 크기를 반환 합니다.
- 재정의 `LayoutChildren` 호출할 `Layout` 레이아웃의 모든 자식에서

`for` 또는 `foreach` 루프 이러한 재정의의 모든 자식을 건너뛰어야 하면 해당 `IsVisible` 속성이 `false`합니다.

에 대 한 호출 `OnMeasure` 보장 되지 않습니다. `OnMeasure` 호출 되지 않습니다 부모 스 레이아웃의은 레이아웃의 크기 (예를 들어 페이지를 채우는 레이아웃)를 제어 하는 경우. 이러한 이유로 `LayoutChildren` 하는 동안 가져온 자식 크기를 사용할 수 없게 된 `OnMeasure` 호출 합니다. 대개 `LayoutChildren` 자체를 호출 해야 `Measure` 레이아웃의 자식에서 캐싱 (나중에 논의 될)을 논리 크기의 몇 가지 종류를 구현할 수 있습니다.

### <a name="an-easy-example"></a>쉬운 예

[ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) 샘플에는 간단해 진 [ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) 클래스 및 해당 사용 데모입니다.

### <a name="vertical-and-horizontal-positioning-simplified"></a>간소화 된 세로 및 가로 위치

작업 중 하나가 하는 `VerticalStack` 수행 해야 하는 동안 발생는 `LayoutChildren` 재정의 합니다. 메서드에서 사용 자식 `HorizontalOptions` 자식에서 슬롯 내 위치를 지정 하는 방법을 결정 하는 속성의 `VerticalStack`합니다. 정적 메서드를 대신 호출할 수 있습니다 [ `Layout.LayoutChildIntoBoundingRect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/)합니다. 이 메서드를 호출 `Measure` 의 자식에 사용 하 여 해당 `HorizontalOptions` 및 `VerticalOptions` 속성을 지정된 된 사각형 내에서 자식 위치를 지정 합니다.

### <a name="invalidation"></a>무효화

종종 요소 속성의 변경을 해당 요소가 레이아웃에 표시 되는 방식을 영향을 줍니다. 레이아웃을 트리거하는 새로운 레이아웃 유효성이 검사 된 해제 해야 합니다.

`VisualElement` 보호 된 메서드를 정의 [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/)는 일반적으로 호출 되는 바인딩 가능한 속성의 속성 변경 처리기에서 해당 변경 변경 효과 크기에 영향을 줍니다. `InvalidateMeasure` 메서드 발생 한 [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) 이벤트입니다.

`Layout` 라는 비슷한 보호 된 메서드를 정의 하는 클래스 [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/)되는 `Layout` 파생 클래스를 배치 하는 방법을 자식의 크기에 영향을 주는 변경에 대 한 호출 해야 합니다.

### <a name="some-rules-for-coding-layouts"></a>레이아웃 코딩 하기 위한 몇 가지 규칙

1. 정의한 속성 `Layout<T>` 바인딩 가능한 속성에 의해 파생 항목을 백업 해야 하며 속성 변경 처리기를 호출 해야 `InvalidateLayout`합니다.

2. A `Layout<T>` 연결 된 바인딩 가능한 속성을 정의 하는 파생 클래스에서 재정의 해야 [ `OnAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnAdded/p/T/) 자식에 속성 변경 처리기를 추가 하 고 [ `OnRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnRemoved/p/T/) 를 제거 하려면 처리기입니다. 처리기에서 이러한 변경 내용을 연결 된 바인딩 가능한 속성에 대 한 확인 되 고 호출 하 여 응답 해야 `InvalidateLayout`합니다.

3. A `Layout<T>` 자식 크기의 캐시를 구현 하는 파생 클래스에서 재정의 해야 `InvalidateLayout` 및 [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) 이러한 메서드가 호출 되 면 캐시의 선택을 취소 합니다.

### <a name="a-layout-with-properties"></a>속성을 가진 레이아웃

[ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 모든 자식의 크기를 동일 하 고 한 행 (또는 열)에서 자식이 바꿈됩니다 된다고 가정 next입니다. 정의 `Orientation` 처럼 속성 `StackLayout`, 및 `ColumnSpacing` 및 `RowSpacing` 같은 속성 `Grid`, 자식 크기를 캐시 합니다.

[ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) 넣습니다 예제는 `WrapLayout` 에 `ScrollView` 주식 사진 표시 합니다.

### <a name="no-unconstrained-dimensions-allowed"></a>허용 제한 되지 않은 차원이 없습니다!

[ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) 에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리 자체 내 모든 자식 표시 됩니다. 따라서 제한 되지 않은 차원을 처리할 수 없습니다 및 발생 한 경우 예외가 발생 합니다.

[ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) 샘플 `UniformGridLayout`:

[![사진 눈금의 삼중 스크린 샷](images/ch26fg08-small.png "균일 한 모눈 레이아웃")](images/ch26fg08-large.png#lightbox "균일 한 모눈 레이아웃")

### <a name="overlapping-children"></a>겹치는 하위

A `Layout<T>` 파생 클래스에는 자식 겹칠 수 있습니다. 그러나 자식을에서 순서 대로 렌더링 됩니다는 `Children` 컬렉션과는 순서가 아니라 해당 `Layout` 메서드가 호출 됩니다.

`Layout` 자식 컬렉션 내에서 이동할 수 있는 두 개의 메서드를 정의 하는 클래스:

- [`LowerChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LowerChild/p/Xamarin.Forms.View/) 자식 컬렉션의 시작 부분을 이동 하려면
- [`RaiseChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.RaiseChild/p/Xamarin.Forms.View/) 자식 컬렉션의 끝으로 이동 하려면

겹치는 자식에 대 한 자식 컬렉션의 끝에 시각적으로 컬렉션의 시작 부분에 자식 항목 위에 나타납니다.

[ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 렌더링 순서일 따라서 중 하나를 허용 하는 연결된 속성을 정의 하는 라이브러리의 다른 맨 위에 표시 될 자식입니다. [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) 이 샘플을 보여 줍니다.

[![학생 카드 파일 눈금의 삼중 스크린 샷](images/ch26fg10-small.png "레이아웃 자식 겹치는")](images/ch26fg10-large.png#lightbox "레이아웃 자식 겹치는")

### <a name="more-attached-bindable-properties"></a>추가 연결 된 바인딩 가능한 속성

[ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) 클래스에 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리 두 개를 지정 하는 연결 된 바인딩 가능한 속성 정의 `Point` 값 및 두께 값을 조작 하 고 `BoxView` 요소 줄 유사 하 게 합니다.

[ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) 샘플 3D 큐브를 그리는 데 사용 합니다.

### <a name="layout-and-layoutto"></a>레이아웃 및 LayoutTo

A `Layout<T>` 파생 클래스를 호출할 수 `LayoutTo` 대신 `Layout` 레이아웃 애니메이션 효과를 주는 합니다. [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) 클래스는이 작업을 수행 하며 [ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) 샘플 것을 보여 줍니다.



## <a name="related-links"></a>관련 링크

- [장 26 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [장 26 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [사용자 지정 레이아웃 만들기](~/xamarin-forms/user-interface/layouts/custom.md)
