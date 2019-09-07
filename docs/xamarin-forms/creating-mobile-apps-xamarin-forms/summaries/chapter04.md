---
title: 요약 4 장입니다. 스택 스크롤
description: 'Xamarin.ios를 사용 하 여 Mobile Apps 만들기: 요약 4 장입니다. 스택 스크롤'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 66e4f52e87a4398dd2e09d2d128f43de9a71a665
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760835"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>요약 4 장입니다. 스택 스크롤

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)

이 장에서 주로 전용으로 지정 개념을 도입 *레이아웃*, 있으며 클래스 및 Xamarin.Forms 페이지의 여러 보기의 시각적 표시를 구성 하는 데 사용 하는 방법에 대 한 전체 용어입니다.

레이아웃에서 파생 되는 여러 클래스를 포함 [ `Layout` ](xref:Xamarin.Forms.Layout) 하 고 [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1)합니다. 이 장에서 중점적 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)합니다.

> [!NOTE]
> 합니다 [ `FlexLayout` ](~/xamarin-forms/user-interface/layouts/flex-layout.md) 에 도입 된 Xamarin.Forms 3.0과 비슷한 방법으로 사용할 수 있습니다 `StackLayout` 하지만 더 유연 하 게 합니다.

이 챕터에 도입 되었습니다은 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView)를 [ `Frame` ](xref:Xamarin.Forms.Frame), 및 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 클래스입니다.

## <a name="stacks-of-views"></a>스택 뷰

[`StackLayout`](xref:Xamarin.Forms.StackLayout) 파생 `Layout<View>` 상속 된 [ `Children` ](xref:Xamarin.Forms.Layout`1) 형식의 속성 `IList<View>`합니다. 이 컬렉션에 여러 보기 항목을 추가 하 고 `StackLayout` 가로 또는 세로 스택을 표시 합니다.

설정 합니다 [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) 속성을 `StackLayout` 의 멤버에는 [ `StackOrientation` ](xref:Xamarin.Forms.StackOrientation) 열거형 중 하나 [ `Vertical` ](xref:Xamarin.Forms.StackOrientation.Vertical) 또는 [ `Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal). 기본값은 `Vertical`입니다.

설정 합니다 [ `Spacing` ](xref:Xamarin.Forms.StackLayout.Spacing) 의 속성 `StackLayout` 에 `double` 자식 간의 간격을 지정 하는 값. 기본값은 6입니다.

코드에서 항목을 추가할 수 있습니다는 `Children` 의 컬렉션 `StackLayout` 에 `for` 또는 `foreach` 에 설명 된 대로 루프를 [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) 샘플에서는 하거나 초기화할 수 있습니다는 `Children` 목록 사용 하 여 컬렉션에서 설명한 것 처럼 개별 뷰 [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList)합니다. 자식에서 파생 되어야 합니다 `View` 기타를 포함할 수 있지만 `StackLayout` 개체입니다.

## <a name="scrolling-content"></a>콘텐츠 스크롤

경우는 `StackLayout` 페이지에 표시할 너무 많은 자식이 포함 넣을 수 있습니다를 `StackLayout` 에 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) 스크롤을 허용 하도록 합니다.

설정 된 [ `Content` ](xref:Xamarin.Forms.ScrollView.Content) 속성의 `ScrollView` 스크롤하려면 원하는 보기를 합니다. 이 방식은 비용를 `StackLayout`, 이지만 모든 보기 수 있습니다.

설정 합니다 [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation) 의 속성 `ScrollView` 의 멤버에는 [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) 속성인 [ `Vertical` ](xref:Xamarin.Forms.ScrollOrientation.Vertical), [ `Horizontal` ](xref:Xamarin.Forms.ScrollOrientation.Horizontal), 또는 [ `Both` ](xref:Xamarin.Forms.ScrollOrientation.Both)합니다. 기본값은 `Vertical`입니다. 경우 내용의 `ScrollView` 는 `StackLayout`, 두 방향을 일치 해야 합니다.

[ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) 의 사용법을 보여 줍니다 `ScrollView` 및 `StackLayout` 색을 표시 합니다. 샘플.NET 리플렉션을 사용 하 여 모든 공용 정적 속성 및 필드를 가져오는 방법도 보여 줍니다는 `Color` 구조를 명시적으로 나열 하지 않아도 됩니다.

## <a name="the-expands-option"></a>확장 옵션

경우는 `StackLayout` 스택 자식 각 자식 차지의 전체 높이 내 특정 슬롯을 `StackLayout` 자식의 크기 및의 설정에 종속 된 해당 `HorizontalOptions` 및 `VerticalOptions` 속성입니다. 이러한 속성 형식의 값이 할당 됩니다 [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/)합니다.

`LayoutOptions` 구조 두 속성을 정의 합니다.

- [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) 열거형 형식의 [ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment) 네 가지 멤버를 사용 하 여 [ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start)하십시오 [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center)를 [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End), 및 [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) 형식의 `bool`

사용자 편의 위해 합니다 `LayoutOptions` 구조 형식의 8 개의 정적 읽기 전용 필드가 정의 `LayoutOptions` 두 인스턴스 속성의 모든 조합을 포함 하는:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

다음 논의에서는 `StackLayout` 기본 세로 방향으로 합니다. 가로 `StackLayout` 것과 유사 합니다.

세로 `StackLayout`는 `HorizontalOptions` 설정에 따라 자식 너비에 가로로 배치 되는 방법을 결정 합니다 `StackLayout`합니다. `Alignment` 설정 `Start`, `Center`, 또는 `End` 가로 방향으로 제한 되지 자식 발생 합니다. 자식 자체 너비를 결정 하 고 왼쪽, 가운데 또는 오른쪽에 배치 되는 `StackLayout`합니다. 합니다 `Fill` 가로 방향으로 제한 될 자식 하면 옵션과의 너비를 채웁니다는 `StackLayout`합니다.

세로 `StackLayout`, 각 자식 세로로 제약을 받지 않는 및 가져옵니다 세로 슬롯 자식의 높이 따라이 경우는 `VerticalOptions` 설정을 관련이 없습니다.

경우 세로 `StackLayout` 자체는 제한 되지&mdash;즉 경우 해당 `VerticalOptions` 설정은 `Start`를 `Center`, 또는 `End`, 다음의 높이 `StackLayout` 자식의 전체 높이 합니다.

그러나 경우 세로 `StackLayout` 세로 방향으로 제한 됩니다&mdash;하는 경우 해당 `VerticalOptions` 설정은 `Fill` &mdash;다음의 높이 `StackLayout` 합계 보다 클 수 있습니다는 해당 컨테이너의 높이 됩니다 해당 자식 항목의 높이입니다. 하는 경우 및 자식이 하나 이상 있으면를 `VerticalOptions` 사용 하 여 설정는 `Expands` 의 플래그 `true`, 다음 추가 공간을 `StackLayout` 사용 하 여 이러한 모든 자식 항목 간에 균등 하 게 할당 되는 `Expands` 의 플래그 `true`합니다. 자식의 전체 높이의 높이 같은 다음은 `StackLayout`, 및 `Alignment` 부분을 `VerticalOptions` 설정은 자식 해당 슬롯에 세로로 배치 되는 방법을 결정 합니다.

에 설명 되어이 [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) 샘플입니다.

## <a name="frame-and-boxview"></a>프레임 및 BoxView

이러한 두 사각형 뷰 프레젠테이션을 위해 자주 사용 됩니다.

합니다 [ `Frame` ](xref:Xamarin.Forms.Frame) 표시와 같은 레이아웃을 수 있는 다른 뷰 주위에 사각형 프레임 `StackLayout`합니다. `Frame` 상속을 [ `Content` ](xref:Xamarin.Forms.ContentView.Content) 속성을 [ `ContentView` ](xref:Xamarin.Forms.ContentView) 사용자가 설정한 뷰에 내에 표시할 수는 `Frame`합니다. `Frame` 기본적으로 이루어집니다. 프레임의 모양을 사용자 지정 하는 다음 세 가지 속성을 설정 합니다.

- 합니다 [ `OutlineColor` ](xref:Xamarin.Forms.Frame.OutlineColor) 속성을 볼 수 있도록 합니다. 일반적으로 설정할 `OutlineColor` 에 `Color.Accent` 기본 색 구성표를 알 수 없는 경우.
- [ `HasShadow` ](xref:Xamarin.Forms.Frame.HasShadow) 속성 설정할 수 있습니다 `true` iOS 장치에서 검은색 그림자를 표시 합니다.
- 설정 된 [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) 속성을는 `Thickness` 공백을 프레임과 프레임 사이 남길 값의 콘텐츠입니다. 기본값은 모든 면에서 20 명의 단위입니다.

`Frame` 기본값이 `HorizontalOptions` 및 `VerticalOptions` 의 값 `LayoutOptions.Fill`, 즉는 `Frame` 해당 컨테이너를 입력 합니다. 다른 설정의 크기에는 `Frame` 내용의 크기를 기반으로 하는 합니다.

합니다 `Frame` 에 설명 되어 합니다 [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) 샘플입니다.

합니다 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 지정 되는 색의 사각형 영역을 표시 합니다. 해당 [ `Color` ](xref:Xamarin.Forms.BoxView.Color) 속성입니다.

경우는 `BoxView` 제한 됩니다 (해당 `HorizontalOptions` 하 고 `VerticalOptions` 속성의 해당 기본 설정을 `LayoutOptions.Fill`), `BoxView` 에 대 한 사용 가능한 공간을 채웁니다. 경우는 `BoxView` 제약을 받지 않는 (사용 하 여 `HorizontalOptions` 하 고 `LayoutOptions` 의 설정을 `Start`, `Center`, 또는 `End`), 40 단위 정사각형의 기본 차원이 합니다. `BoxView` 하나 이상의 차원을 제한 되며 다른 비제한 수 있습니다.

설정 종종 합니다 [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) 의 속성 `BoxView` 특정 크기를 제공 합니다. 자세히 설명 합니다 [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) 샘플입니다.

여러 인스턴스를 사용할 수 있습니다 `StackLayout` 결합 하는 `BoxView` 및 여러 `Label` 인스턴스를 `Frame` 특정 색을 표시 한 다음 각각에서 이러한 보기를 배치 하는 `StackLayout` 에 `ScrollView` 매력적인를 만들려면 에 표시 된 색 목록을 합니다 [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) 샘플:

[![색 블록의 삼중 스크린 샷](images/ch04fg11-small.png "색의 목록")](images/ch04fg11-large.png#lightbox "색의 목록")

## <a name="a-scrollview-in-a-stacklayout"></a>에 StackLayout는 ScrollView?

배치를 `StackLayout` 에 `ScrollView` 이 일반적 이지만 배치를 `ScrollView` 에서 `StackLayout` 편리한는 경우도 있습니다. 이론적으로이 아니어야 수 때문에 세로 자식의 `StackLayout` 세로 방향으로 제한 되지 않습니다. 하지만 `ScrollView` 세로 방향으로 제한 해야 합니다. 이 스크롤에 대 한 다음 자식의 크기를 결정할 수 있도록 특정 높이가 지정 되어야 합니다.

트릭을 제공 하는 것을 `ScrollView` 자식의 `StackLayout` 는 `VerticalOptions` 설정 `FillAndExpand`합니다. 에 설명 되어이 [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) 샘플입니다.

합니다 **BlackCat** 샘플 정의 공유 라이브러리에 포함 된 프로그램 리소스에 액세스 하는 방법을 보여 줍니다. 또한 공유 자산 프로젝트 SAPs ()를 사용 하 여 수행할 수 있습니다 하지만 프로세스는으로 조금 더 까다롭습니다 합니다 [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) 샘플을 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [4 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [4 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [4 장 F# 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
