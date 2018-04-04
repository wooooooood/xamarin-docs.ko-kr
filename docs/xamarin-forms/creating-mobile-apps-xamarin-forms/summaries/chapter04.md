---
title: 요약 Chapter 4입니다. 스택 스크롤
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 09a9d21b7979e0eb3f12d3b1207f2185e059f65c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>요약 Chapter 4입니다. 스택 스크롤

이 장에서 주로 개념이 도입로 할당 되어 *레이아웃*, 클래스 및 Xamarin.Forms를 사용 하 여 페이지에서 여러 보기의 시각적 표시 구성 방법에 대 한 전반적인 용어입니다.

레이아웃에서 파생 되는 몇 가지 클래스를 포함 [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) 및 [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)합니다. 이 장에서 중점적 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)합니다.

이 장의 도입 했는데이는 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/), [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/), 및 [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) 클래스입니다.

## <a name="stacks-of-views"></a>스택 뷰

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 파생 `Layout<View>` 상속는 [ `Children` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) 형식의 속성이 `IList<View>`합니다. 이 컬렉션에 여러 보기 항목을 추가 하 고 `StackLayout` 가로 또는 세로 스택 목록이 표시 됩니다.

설정의 [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) 속성 `StackLayout` 의 멤버에는 [ `StackOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackOrientation/) 열거형 중 하나가 [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Vertical/) 또는 [ `Horizontal`](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Horizontal/). 기본값은 `Vertical`입니다.

설정의 [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) 속성의 `StackLayout` 에 `double` 자식 사이의 간격을 지정 하는 값입니다. 기본값은 6입니다.

코드에서 항목을 추가할 수는 `Children` 컬렉션 `StackLayout` 에 `for` 또는 `foreach` 에서 같이 루프는 [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) 하거나 샘플을 초기화할 수는 `Children` 에서 데모로 개별 뷰의 목록과 함께 컬렉션 [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList)합니다. 자식을에서 파생 되어야 `View` 다른 포함할 수 있지만 `StackLayout` 개체입니다.

## <a name="scrolling-content"></a>콘텐츠 스크롤

경우는 `StackLayout` 페이지에 표시 하려면 너무 많은 자식이 포함 넣을 수 있습니다는 `StackLayout` 에 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) 스크롤할 수 있도록 합니다.

설정의 [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) 속성 `ScrollView` 이동 하려는 보기를 합니다. 이 문제는 주로 한 `StackLayout`, 이지만 모든 보기 수 있습니다.

설정의 [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) 속성 `ScrollView` 의 멤버에는 [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/) 속성 [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Vertical/), [ `Horizontal` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Horizontal/), 또는 [ `Both` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Both/)합니다. 기본값은 `Vertical`입니다. 하는 경우의 콘텐츠는 `ScrollView` 는 `StackLayout`, 두 방향 일치 해야 합니다.

[ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) 샘플의 사용법을 보여줍니다 `ScrollView` 및 `StackLayout` 사용할 수 있는 색을 표시 합니다. 샘플에는 또한 모든 공용 정적 속성 및 필드를 가져오는.NET 리플렉션을 사용 하는 방법을 보여 줍니다는 `Color` 구조 명시적으로 나열 하지 않아도 됩니다.

## <a name="the-expands-option"></a>확장 옵션

때는 `StackLayout` 스택 자식을 각 자식 특정 슬롯의 전체 높이 내에서 차지 하는 `StackLayout` 자식의 크기 및의 설정에 종속 된 해당 `HorizontalOptions` 및 `VerticalOptions` 속성입니다. 이러한 속성은 형식의 값이 할당 [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/)합니다.

`LayoutOptions` 구조 두 속성을 정의 합니다.

- [`Alignment`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) 열거형 형식의 [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/) 4 개 멤버와 [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), 및 [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/)
- [`Expands`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) 형식의 `bool`

사용자 편의 위해는 `LayoutOptions` 구조는 또한 형식의 8 개의 정적 읽기 전용 필드 정의 `LayoutOptions` 두 인스턴스 속성의 모든 조합을 포함 하는:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

다음 논의에서는 `StackLayout` 기본 세로 방향으로 합니다. 가로 `StackLayout` 유사 합니다.

세로 `StackLayout`, `HorizontalOptions` 설정은 자식 너비에 가로로 배치 되는 방법을 결정는 `StackLayout`합니다. `Alignment` 설정인 `Start`, `Center`, 또는 `End` 하면 자식 가로 방향으로 제한 됩니다. 자식 자체 너비를 결정 하 고 왼쪽, 가운데 또는 오른쪽에 배치 되는 `StackLayout`합니다. `Fill` 옵션 하면 자식 가로 방향으로 제한 되 고의 너비를 채웁니다는 `StackLayout`합니다.

세로 `StackLayout`, 각 자식 세로로 제약을 받지 않는 및 가져옵니다 세로 슬롯 어린이 높이 따라는 쿼리에서 `VerticalOptions` 설정은 적용 됩니다.

경우 세로 `StackLayout` 자체 제약을 받지 않는&mdash;즉 경우 해당 `VerticalOptions` 설정은 `Start`, `Center`, 또는 `End`, 다음의 높이 `StackLayout` 자식 총 높이 같습니다.

그러나 경우 세로 `StackLayout` 제약을 세로로&mdash;경우 해당 `VerticalOptions` 설정은 `Fill` &mdash;다음의 높이 `StackLayout` 합계 보다 클 수 있습니다는 해당 컨테이너의 높이 됩니다 해당 자식 항목의 높이입니다. 고 자식이 하나 이상에 해당 되는 경우는 `VerticalOptions` 사용 하 여 설정는 `Expands` 의 플래그가 `true`에 추가 공간이 다음는 `StackLayout` 와 이러한 모든 자식 항목 간에 균등 하 게 할당 되는 `Expands` 의 플래그가 `true`합니다. 자식 항목의 전체 높이의 높이 용량이 다음 됩니다는 `StackLayout`, 및 `Alignment` 의 일부로 `VerticalOptions` 설정은 슬롯에 자식 세로로 배치 되는 방법을 결정 합니다.

이 확인할는 [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) 샘플.

## <a name="frame-and-boxview"></a>프레임 및 BoxView

이러한 두 개의 사각형 뷰 프레젠테이션을 위해 종종 사용 됩니다.

[ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) 다른 보기와 같은 레이아웃 수 있는 주위 사각형 프레임을 표시 하는 보기 `StackLayout`합니다. `Frame` 상속는 [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) 속성 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) 내에서 표시 되는 보기를 설정 하는 `Frame`합니다. `Frame` 는 기본적으로 투명 합니다. 프레임의 모양을 사용자 지정 하는 다음 세 가지 속성을 설정 합니다.

- [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/) 속성을 설정 하 여 표시 합니다. 일반적으로 설정 `OutlineColor` 를 `Color.Accent` 기본 색 구성표의 알 수 없는 경우.
- [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/) 속성 설정할 수 있습니다 `true` iOS 장치에서 검은색 그림자를 표시 합니다.
- 설정의 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) 속성을는 `Thickness` 프레임과 프레임 간에 공간을 확보 하는 값의 내용을 합니다. 기본값은 모든 면에서 20 단위입니다.

`Frame` 에 기본 `HorizontalOptions` 및 `VerticalOptions` 값의 `LayoutOptions.Fill`, 즉는 `Frame` 컨테이너 채워집니다. 다른 설정의 크기에는 `Frame` 내용의 크기에 따라 합니다.

`Frame` 에 설명 된 [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) 샘플.

[ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) 하 여 지정 된 색의 사각형 영역이 표시 됩니다. 해당 [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) 속성입니다.

경우는 `BoxView` 제한 됩니다 (해당 `HorizontalOptions` 및 `VerticalOptions` 속성의 해당 기본 설정을 `LayoutOptions.Fill`), `BoxView` 것에 대 한 사용 가능한 공간을 채웁니다. 경우는 `BoxView` 제약을 받지 않는 (으로 `HorizontalOptions` 및 `LayoutOptions` 의 설정을 `Start`, `Center`, 또는 `End`), 40 단위 정사각형의 기본 차원이 있습니다. A `BoxView` 에서 하나 이상의 차원을 제한 하 고 다른 제약 받지 않을 수 있습니다.

설정 종종는 [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) 및 [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) 의 속성 `BoxView` 특정 크기를 제공 하기. 이 확인할는 [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) 샘플.

여러 인스턴스를 사용할 수 있습니다 `StackLayout` 결합 하는 `BoxView` 과 몇 가지 `Label` 인스턴스에 `Frame` 특정 색을 표시 하 고 각각에서 이러한 보기를 배치 하는 `StackLayout` 에 `ScrollView` 매력적인는 만들려는 에 표시 된 색 목록을 [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) 샘플:

[![색 블록과 삼중 스크린 샷](images/ch04fg11-small.png "색 목록")](images/ch04fg11-large.png#lightbox "색 목록")

## <a name="a-scrollview-in-a-stacklayout"></a>에 StackLayout ScrollView?

배치는 `StackLayout` 에 `ScrollView` 일반적인 건 배치는 `ScrollView` 에 `StackLayout` 도 때때로 도움이 됩니다. 이론적으로이 서는 안 발생할 수 있으므로 세로의 자식을 `StackLayout` 세로 방향으로 제한 되지 않습니다. 하지만 `ScrollView` 세로로 제약 되어야 합니다. 제공 해야 특정 높이 스크롤할 수 있도록 해당 자식의 크기를 확인한 다음 수 있도록 합니다.

지정 하는 트릭은 `ScrollView` 의 자식은 `StackLayout` 는 `VerticalOptions` 설정인 `FillAndExpand`합니다. 이 확인할는 [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) 샘플.

**BlackCat** 또한 샘플에 정의 하 고 프로그램 포함 된 리소스는 이식 가능한 클래스 라이브러리 (PCL)에 액세스 하는 방법을 보여 줍니다. 또한 공유 자산 프로젝트 (SAPs)와 함께 수행할 수 있습니다 하지만 프로세스는으로 약간 더 복잡는 [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) 샘플을 보여 줍니다.



## <a name="related-links"></a>관련 링크

- [4 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [4 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Chapter 4 F # 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
