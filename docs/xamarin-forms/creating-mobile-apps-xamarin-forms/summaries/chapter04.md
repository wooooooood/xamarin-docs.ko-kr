---
title: 4 장 요약 스택 스크롤
description: 'Xamarin.ios를 사용 하 여 Mobile Apps 만들기: 4 장 요약 스택 스크롤'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: bda9d5cb323524981bed9c3bb55998513dd69aab
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032870"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>4 장 요약 스택 스크롤

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)

이 장은 주로 *레이아웃*의 개념을 소개 하는 데 사용 됩니다 .이는 Xamarin에서 페이지에 있는 여러 뷰의 시각적 표시를 구성 하는 데 사용 하는 클래스 및 기술에 대 한 전체 용어입니다.

레이아웃에는 [`Layout`](xref:Xamarin.Forms.Layout) 및 [`Layout<T>`](xref:Xamarin.Forms.Layout`1)에서 파생 되는 여러 클래스가 포함 됩니다. 이 장에서는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)에 대해 중점적으로 설명 합니다.

> [!NOTE]
> Xamarin. Forms 3.0에 도입 된 [`FlexLayout`](~/xamarin-forms/user-interface/layouts/flex-layout.md) 는 `StackLayout`와 비슷하지만 유연성이 뛰어난 방식으로 사용할 수 있습니다.

또한이 장에서 소개 하는 [`ScrollView`](xref:Xamarin.Forms.ScrollView), [`Frame`](xref:Xamarin.Forms.Frame)및 [`BoxView`](xref:Xamarin.Forms.BoxView) 클래스입니다.

## <a name="stacks-of-views"></a>뷰 스택

[`StackLayout`](xref:Xamarin.Forms.StackLayout) `Layout<View>`에서 파생 되며 `IList<View>`형식의 [`Children`](xref:Xamarin.Forms.Layout`1) 속성을 상속 합니다. 이 컬렉션에 여러 개의 보기 항목을 추가 하 고 `StackLayout` 가로 또는 세로 스택으로 표시 합니다.

`StackLayout`의 [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) 속성을 [`StackOrientation`](xref:Xamarin.Forms.StackOrientation) 열거형의 멤버 ( [`Vertical`](xref:Xamarin.Forms.StackOrientation.Vertical) 또는 [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal))로 설정 합니다. 기본값은 `Vertical`입니다.

`StackLayout`의 [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) 속성을 `double` 값으로 설정 하 여 자식 사이의 간격을 지정 합니다. 기본값은 6입니다.

코드에서 [**colorloop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) 샘플에 설명된 대로 `for` 또는 `foreach` 루프에서 `StackLayout`의 `Children` 컬렉션에 항목을 추가하거나 [**ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList)에 설명된 대로 개별 뷰 목록을 사용하여 `Children` 컬렉션을 초기화할 수 있습니다. 자식은 `View`에서 파생 되어야 하지만 다른 `StackLayout` 개체를 포함할 수 있습니다.

## <a name="scrolling-content"></a>콘텐츠 스크롤

`StackLayout`에 너무 많은 자식이 포함 되어 있으면 페이지에 표시할 수 있는 `StackLayout`를 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 에 배치 하 여 스크롤할 수 있습니다.

`ScrollView`의 [`Content`](xref:Xamarin.Forms.ScrollView.Content) 속성을 스크롤할 뷰로 설정 합니다. 이는 `StackLayout`경우가 많지만 모든 보기가 될 수 있습니다.

`ScrollView`의 [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) 속성을 [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) 속성, [`Vertical`](xref:Xamarin.Forms.ScrollOrientation.Vertical), [`Horizontal`](xref:Xamarin.Forms.ScrollOrientation.Horizontal)또는 [`Both`](xref:Xamarin.Forms.ScrollOrientation.Both)의 멤버로 설정 합니다. 기본값은 `Vertical`입니다. `ScrollView` 콘텐츠가 `StackLayout`이면 두 방향이 일치 해야 합니다.

[**ReflectedColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) 샘플에서는 `ScrollView` 및 `StackLayout`를 사용 하 여 사용 가능한 색을 표시 하는 방법을 보여 줍니다. 또한이 샘플에서는 .NET 리플렉션을 사용 하 여 명시적으로 나열할 필요 없이 `Color` 구조체의 모든 공용 정적 속성 및 필드를 가져오는 방법을 보여 줍니다.

## <a name="the-expands-option"></a>확장 옵션

`StackLayout`에서 자식을 스택에 놓으면 각 자식은 자식의 크기 및 `VerticalOptions` 속성 `HorizontalOptions`의 설정에 따라 `StackLayout`의 전체 높이에서 특정 슬롯을 차지 합니다. 이러한 속성에는 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions)형식의 값이 할당 됩니다.

`LayoutOptions` 구조는 다음과 같은 두 가지 속성을 정의 합니다.

- 열거형 형식에 대 한 [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start), [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center), [`End`](xref:Xamarin.Forms.LayoutAlignment.End) [,`Fill`의](xref:Xamarin.Forms.LayoutAlignment.Fill) 네 가지 멤버로 [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment)
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands)(`bool` 형식)

사용자 편의를 위해 `LayoutOptions` 구조는 두 인스턴스 속성의 모든 조합을 포함 하는 `LayoutOptions` 유형의 정적 읽기 전용 필드인 8 개를 정의 합니다.

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

다음 설명에는 기본 세로 방향 `StackLayout` 포함 되어 있습니다. 가로 `StackLayout`는 유사 합니다.

세로 `StackLayout`의 경우 `HorizontalOptions` 설정은 자식이 `StackLayout`의 너비 내에서 가로로 배치 되는 방법을 결정 합니다. `Start`, `Center`또는 `End`의 `Alignment` 설정으로 인해 자식에 게는 행이 제한 되지 않습니다. 자식은 자체 너비를 결정 하 고 `StackLayout`왼쪽, 가운데 또는 오른쪽에 배치 됩니다. `Fill` 옵션은 자식에 가로 제한이 적용 되도록 하 고 `StackLayout`의 너비를 채웁니다.

세로 `StackLayout`의 경우 각 자식은 세로로 제한 되지 않으며 자식의 높이에 따라 세로 슬롯을 가져옵니다 .이 경우 `VerticalOptions` 설정은 관련이 없습니다.

세로 `StackLayout` 자체&mdash;제한 되지 않는 경우 즉, `VerticalOptions` 설정이 `Start`, `Center`또는 `End`이면 `StackLayout`의 높이가 자식의 총 높이입니다.

그러나 세로 `StackLayout`&mdash;수직으로 제한 되는 경우에는 `VerticalOptions` 설정이&mdash;`Fill`경우 `StackLayout` 높이는 해당 컨테이너의 높이가 됩니다 .이는 해당 컨테이너의 전체 높이 보다 클 수 있습니다. 해당 하는 경우와 하나 이상의 자식에 `true`의 `Expands` 플래그를 사용 하는 `VerticalOptions` 설정이 있는 경우 `StackLayout`의 추가 공간은 `Expands` 플래그를 사용 하 여 해당 자식 중에 동일 하 게 할당 됩니다. 그러면 자식의 총 높이가 `StackLayout`의 높이와 동일 하 고, `VerticalOptions` 설정의 `Alignment` 부분은 자식이 해당 슬롯에서 세로 방향으로 배치 되는 방법을 결정 합니다.

이는 [**VerticalOptionsDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) 샘플에 설명 되어 있습니다.

## <a name="frame-and-boxview"></a>프레임 및 BoxView

이러한 두 개의 사각형 보기는 프레젠테이션 용도로 사용 되는 경우가 많습니다.

[`Frame`](xref:Xamarin.Forms.Frame) 보기는 `StackLayout`와 같은 레이아웃 일 수 있는 다른 뷰 주위에 사각형 프레임을 표시 합니다. `Frame`는 `Frame`내에 표시 되도록 설정 하는 [`ContentView`](xref:Xamarin.Forms.ContentView) 에서 [`Content`](xref:Xamarin.Forms.ContentView.Content) 속성을 상속 합니다. `Frame`은 기본적으로 투명 합니다. 다음 세 가지 속성을 설정 하 여 프레임의 모양을 사용자 지정 합니다.

- 표시 하는 [`OutlineColor`](xref:Xamarin.Forms.Frame.OutlineColor) 속성입니다. 기본 색 구성표를 모를 경우에는 `OutlineColor`를 `Color.Accent`으로 설정 하는 것이 일반적입니다.
- [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) 속성을 `true`으로 설정 하 여 iOS 장치에 검정색 그림자를 표시할 수 있습니다.
- [`Padding`](xref:Xamarin.Forms.Layout.Padding) 속성을 `Thickness` 값으로 설정 하 여 프레임과 프레임의 내용 사이에 공백을 둡니다. 기본값은 모든 면에서 20 단위입니다.

`Frame`는 `LayoutOptions.Fill`의 기본 `HorizontalOptions` 및 `VerticalOptions` 값을 가지 며,이는 `Frame`가 해당 컨테이너를 채우도록 합니다. 다른 설정을 사용 하는 경우 `Frame`의 크기는 해당 콘텐츠의 크기를 기준으로 합니다.

`Frame` [**FramedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) 샘플에 나와 있습니다.

[`BoxView`](xref:Xamarin.Forms.BoxView) [`Color`](xref:Xamarin.Forms.BoxView.Color) 속성으로 지정 된 색의 사각형 영역을 표시 합니다.

`BoxView` 제한 되는 경우 (`HorizontalOptions` 및 `VerticalOptions` 속성의 기본 설정이 `LayoutOptions.Fill`) `BoxView`에서 사용 가능한 공간을 채웁니다. `BoxView` 제한 되지 않는 경우 (`Start`, `Center`또는 `End`의 `HorizontalOptions` 및 `LayoutOptions` 설정을 사용 하는 경우) 기본 차원은 40 단위 정사각형입니다. `BoxView`은 한 차원에서 제약을 받을 수 있으며 다른 차원에서는 제한 되지 않습니다.

[`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) `BoxView` 속성을 설정 하 여 특정 크기를 지정 하는 경우가 많습니다. 이는 [**SizedBoxView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) 샘플에 설명 되어 있습니다.

여러 `StackLayout` 인스턴스를 사용 하 여 `Frame`에서 `BoxView`와 여러 `Label` 인스턴스를 결합 하 여 특정 색을 표시 한 다음 이러한 각 뷰를 `StackLayout`에 배치 하 여 멋진 색 목록을 만들 수 있습니다. [**Colorblocks**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) 샘플에 표시 됩니다.

[![색 블록의 삼중 스크린 샷](images/ch04fg11-small.png "색 목록")](images/ch04fg11-large.png#lightbox "색 목록")

## <a name="a-scrollview-in-a-stacklayout"></a>StackLayout의 ScrollView?

`StackLayout`를 `ScrollView`에 배치 하는 것이 일반적 이지만 `StackLayout`에 `ScrollView`를 배치 하는 것도 편리한 방법입니다. 이론적으로는 세로 `StackLayout`의 자식이 세로로 제한 되지 않으므로이를 수행할 수 없습니다. 그러나 `ScrollView`는 수직으로 제한 되어야 합니다. 스크롤할 자식 크기를 결정 하려면 특정 높이를 지정 해야 합니다.

트릭은 `FillAndExpand`의 `VerticalOptions` 설정 `StackLayout`의 `ScrollView` 자식을 제공 하는 것입니다. 이는 [**블랙 cat**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) 샘플에서 보여 줍니다.

또한 **블랙 cat** 샘플은 공유 라이브러리에 포함 된 프로그램 리소스를 정의 하 고 액세스 하는 방법을 보여 줍니다. SAPs (공유 자산 프로젝트)를 사용 하 여이 작업을 수행할 수도 있지만 [**블랙 인 sap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) 샘플에서 보여 주는 것 처럼 프로세스는 조금 더 까다롭습니다.

## <a name="related-links"></a>관련 링크

- [4 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [4 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [4 F# 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
