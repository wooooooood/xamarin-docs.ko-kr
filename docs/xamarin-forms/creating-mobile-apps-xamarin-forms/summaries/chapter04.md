---
title: 챕터 4 요약. 스택 스크롤
description: 'Xamarin.Forms로 모바일 앱 만들기: 챕터 4 요약. 스택 스크롤'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 5313dd34839d6a5d21432161b9fd3a0ffce6e816
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83149948"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>챕터 4 요약. 스택 스크롤

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)

이 챕터에서는 Xamarin.Forms가 페이지에 있는 여러 가지 보기의 시각적 표시를 구성하는 데 사용하는 클래스와 기법을 통칭하는 용어인 *레이아웃*의 개념을 소개합니다.

레이아웃에는 [`Layout`](xref:Xamarin.Forms.Layout)과 [`Layout<T>`](xref:Xamarin.Forms.Layout`1)에서 파생되는 몇 가지 클래스가 있습니다. 이 챕터에서는 [`StackLayout`](xref:Xamarin.Forms.StackLayout)을 주로 다룹니다.

> [!NOTE]
> Xamarin.Forms 3.0에서 도입된 [`FlexLayout`](~/xamarin-forms/user-interface/layouts/flex-layout.md)은 `StackLayout`과 비슷한 방법으로 사용할 수 있으며 그보다 더 많은 유연성을 제공합니다.

이 챕터에서는 [`ScrollView`](xref:Xamarin.Forms.ScrollView), [`Frame`](xref:Xamarin.Forms.Frame) 및 [`BoxView`](xref:Xamarin.Forms.BoxView) 클래스도 소개합니다.

## <a name="stacks-of-views"></a>보기 스택

[`StackLayout`](xref:Xamarin.Forms.StackLayout)은 `Layout<View>`에서 파생되며 `IList<View>` 형식의 [`Children`](xref:Xamarin.Forms.Layout`1) 속성을 상속합니다. 이 컬렉션에는 여러 개의 보기 항목을 추가할 수 있으며, `StackLayout`은 여러 개의 항목을 가로 또는 세로 스택으로 표시합니다.

`StackLayout`의 [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) 속성을 [`StackOrientation`](xref:Xamarin.Forms.StackOrientation) 열거형의 멤버인 [`Vertical`](xref:Xamarin.Forms.StackOrientation.Vertical) 또는 [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal)로 설정합니다. 기본값은 `Vertical`입니다.

`StackLayout`의 [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) 속성을 `double` 값으로 지정하여 자식 요소 사이의 간격을 지정합니다. 기본값은 6입니다.

코드에서는 [**ColorLoop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) 샘플에 나와 있는 것처럼 `for` 또는 `foreach` 루프에서 `StackLayout`의 `Children` 컬렉션에 여러 항목을 추가하거나 [**ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList)에 나와 있는 것처럼 `Children` 컬렉션을 개별 보기의 목록으로 초기화할 수 있습니다. 자식 요소는 `View`에서 파생되어야 하지만 다른 `StackLayout` 개체를 포함할 수 있습니다.

## <a name="scrolling-content"></a>콘텐츠 스크롤

`StackLayout`에 하나의 페이지에 표시하기에 너무 많은 자식 요소가 있는 경우 [`ScrollView`](xref:Xamarin.Forms.ScrollView)에 `StackLayout`을 배치하여 스크롤을 사용할 수 있습니다.

`ScrollView`의 [`Content`](xref:Xamarin.Forms.ScrollView.Content) 속성을 스크롤하려는 보기로 설정합니다. 이 보기는 `StackLayout`인 경우가 많지만 임의의 보기일 수 있습니다.

`ScrollView`의 [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) 속성을 [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) 속성의 멤버인 [`Vertical`](xref:Xamarin.Forms.ScrollOrientation.Vertical), [`Horizontal`](xref:Xamarin.Forms.ScrollOrientation.Horizontal) 또는 [`Both`](xref:Xamarin.Forms.ScrollOrientation.Both)로 설정합니다. 기본값은 `Vertical`입니다. `ScrollView`의 콘텐츠가 `StackLayout`이면 두 방향이 동일해야 합니다.

[**ReflectedColors**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) 샘플에서는 `ScrollView`와 `StackLayout`을 사용하여 사용 가능한 색을 표시하는 방법을 보여 줍니다. 샘플에서는 .NET 리플렉션을 사용하여 명시적으로 나열하지 않고도 `Color` 구조체의 모든 고정적인 퍼블릭 속성과 필드를 가져오는 방법도 보여 줍니다.

## <a name="the-expands-option"></a>Expands 옵션

`StackLayout`이 자식 요소를 스택하면 각 자식 요소는 자식 요소의 크기와 `HorizontalOptions` 및 `VerticalOptions` 속성의 설정에 따라 달라지는 `StackLayout`의 총 높이 내에서 특정 슬롯을 차지합니다. 해당 속성에는 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 형식의 값이 할당됩니다.

`LayoutOptions` 구조체는 다음과 같은 두 가지 속성을 정의합니다.

- 열거형 형식 [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment)의 [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment)를 4개의 멤버 [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start), [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center), [`End`](xref:Xamarin.Forms.LayoutAlignment.End), [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)로 정의합니다.
- [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands)(`bool` 형식)

편의를 위해, `LayoutOptions` 구조체는 두 가지 인스턴스 속성의 모든 조합을 아우르는 `LayoutOptions` 형식의 8가지 고정적인 읽기 전용 필드도 정의합니다.

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

아래에서는 기본 세로 방향을 갖는 `StackLayout`에 대해 설명하며, 가로 `StackLayout`도 이와 동일합니다.

세로 `StackLayout`에 대해, `HorizontalOptions` 설정은 자식 요소가 `StackLayout`의 너비 내에서 어떻게 배치되는지를 결정합니다. `Alignment`가 `Start`, `Center` 또는 `End`로 설정되면 자식 요소가 가로 방향으로 제한되지 않습니다. 자식 요소는 자체적으로 너비를 결정하며 `StackLayout`의 왼쪽, 가운데 또는 오른쪽에 배치됩니다. `Fill` 옵션을 사용하면 자식 요소가 가로 방향으로 제한되어 `StackLayout`의 너비를 채웁니다.

세로 `StackLayout`에 대해, 각 자식 요소는 세로 방향으로 제한되지 않으며 자식 요소의 높이에 따라 세로 슬롯을 갖습니다. 이때 `VerticalOptions` 설정은 무시됩니다.

세로 `StackLayout` 자체도 제한되지 않는 경우&mdash;즉, `VerticalOptions` 설정이 `Start`, `Center` 또는 `End`인 경우 `StackLayout`의 높이는 자식 요소들의 높이 합계입니다.

그러나 세로 `StackLayout`이 세로 방향으로 제한되는 경우&mdash;즉, `VerticalOptions` 설정이 `Fill`인 경우&mdash;`StackLayout`의 높이는 컨테이너의 높이가 되며, 이는 자식 요소들의 높이 합계보다 클 수 있습니다. 이때 하나 이상의 자식 요소의 `VerticalOptions` 설정에서 `Expands` 플래그가 `true`로 설정된 경우, `StackLayout`의 여분의 공간은 `Expands` 플래그가 `true`인 자식 요소들에 동일하게 할당됩니다. 자식 요소들의 높이 합계는 `StackLayout`의 높이와 같아지며, `VerticalOptions` 설정의 `Alignment` 부분이 슬롯에서 자식 요소가 세로 방향으로 배치되는 방식을 결정합니다.

이는 [**VerticalOptionsDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) 샘플에 나와 있습니다.

## <a name="frame-and-boxview"></a>Frame 및 BoxView

이 두 개의 사각형 보기는 프레젠테이션 용도로 자주 사용됩니다.

[`Frame`](xref:Xamarin.Forms.Frame) 보기는 다른 보기를 둘러싸는 사각형 프레임을 표시합니다. 이 다른 보기는 `StackLayout`과 같은 레이아웃일 수 있습니다. `Frame`은 `Frame` 내부에 표시되도록 설정된 [`ContentView`](xref:Xamarin.Forms.ContentView)에서 [`Content`](xref:Xamarin.Forms.ContentView.Content) 속성을 상속합니다. `Frame`은 기본적으로 투명합니다. 다음과 같은 세 가지 속성을 설정하여 프레임의 모양을 사용자 지정할 수 있습니다.

- [`OutlineColor`](xref:Xamarin.Forms.Frame.OutlineColor) 속성을 설정하여 표시합니다. 기본 색 구성표를 모를 때에는 일반적으로 `OutlineColor`를 `Color.Accent`로 설정합니다.
- [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) 속성을 `true`로 설정하여 iOS 디바이스에서 검은색 그림자를 표시할 수 있습니다.
- [`Padding`](xref:Xamarin.Forms.Layout.Padding) 속성을 `Thickness` 값으로 설정하여 프레임과 프레임의 콘텐츠 사이에 공백을 둘 수 있습니다. 기본값은 모든 쪽에서 20단위입니다.

`Frame`의 `HorizontalOptions` 및 `VerticalOptions` 기본값은 `LayoutOptions.Fill`입니다. 즉, `Frame`이 컨테이너를 채웁니다. 다른 설정에서는 `Frame`의 크기가 콘텐츠의 크기에 따라 정해집니다.

`Frame`의 예는 [**FramedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) 샘플에 나와 있습니다.

[`BoxView`](xref:Xamarin.Forms.BoxView)는 [`Color`](xref:Xamarin.Forms.BoxView.Color) 속성으로 지정된 색으로 사각형 영역을 표시합니다.

`BoxView`가 제한되는 경우(`HorizontalOptions` 및 `VerticalOptions` 속성이 기본값 `LayoutOptions.Fill`로 설정된 경우) `BoxView`는 사용 가능한 공간을 채웁니다. `BoxView`가 제한되지 않은 경우(`HorizontalOptions` 및 `LayoutOptions` 설정이 `Start`, `Center` 또는 `End`인 경우) 크기가 40단위인 정사각형의 기본 크기를 갖습니다. `BoxView`는 하나의 차원에서는 제한되고 다른 차원에서는 제한되지 않을 수 있습니다.

`BoxView`의 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성을 설정하여 특정 크기를 지정할 수 있습니다. 이는 [**SizedBoxView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) 샘플에 나와 있습니다.

`StackLayout`의 여러 인스턴스를 사용하여 하나의 `Frame`에서 하나의 `BoxView`와 여러 개의 `Label` 인스턴스를 결합하여 특정 색을 표시하도록 한 다음 각 보기를 하나의 `ScrollView` 안에 있는 `StackLayout`에 배치하여 [**ColorBlocks**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) 샘플에 나와 있는 화려한 색 목록을 만들 수 있습니다.

[![색 블록의 삼중 스크린샷](images/ch04fg11-small.png "색 목록")](images/ch04fg11-large.png#lightbox "색 목록")

## <a name="a-scrollview-in-a-stacklayout"></a>StackLayout 안의 ScrollView?

`ScrollView` 안에 `StackLayout`을 배치하는 것은 흔한 일이지만 때때로 `StackLayout` 안에 `ScrollView`를 배치하는 것이 유용할 수 있습니다. 세로 `StackLayout`의 자식 요소는 세로 방향으로 제한되지 않으므로 이론상으로는 이것이 불가능합니다. 그러나 `ScrollView`는 세로 방향으로 제한되어야 합니다. 스크롤을 위해 자식 요소의 크기를 확인할 수 있어야 하므로 특정 높이가 지정되어야 합니다.

이를 위한 요령은 `StackLayout`의 `ScrollView` 자식 요소의 `VerticalOptions`를 `FillAndExpand`로 설정하는 것입니다. 이는 [**BlackCat**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) 샘플에 나와 있습니다.

**BlackCat** 샘플에서는 공유 라이브러리에 포함된 프로그램 리소스를 정의하고 라이브러리에 액세스하는 방법도 보여 줍니다. 이는 SAP(공유 자산 프로젝트)를 사용하여 달성할 수 있지만, [**BlackCatSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) 샘플에 나와 있는 것처럼 그보다 조금 더 까다롭습니다.

## <a name="related-links"></a>관련 링크

- [챕터 4 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [챕터 4 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [챕터 4 F# 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stacklayout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
