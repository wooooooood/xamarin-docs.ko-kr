---
title: Xamarin.Forms모양의
description: Xamarin.Forms레이아웃은 사용자 인터페이스 컨트롤을 시각적 구조체로 구성 하는 데 사용 됩니다. 이 문서에서는에 포함 된 레이아웃을 나열 합니다 Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c39bf29feceaf598ac8fd38e6af3d227b6deddc0
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137308"
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms모양의

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin.ios 레이아웃은 사용자 인터페이스 컨트롤을 시각적 구조체로 구성 하는 데 사용 됩니다._

[`Layout`](xref:Xamarin.Forms.Layout)의 및 [`Layout<T>`](xref:Xamarin.Forms.Layout`1) 클래스는 Xamarin.Forms 보기 및 기타 레이아웃의 컨테이너 역할을 하는 뷰의 특수 한 하위 형식입니다. `Layout`클래스 자체는에서 파생 [`View`](views.md) 됩니다. `Layout`파생물은 일반적으로 응용 프로그램에서 자식 요소의 위치와 크기를 설정 하는 논리를 포함 합니다 Xamarin.Forms .

[![Xamarin.Forms레이아웃 유형](layouts-images/layouts-sml.png "[! OP. 비 LOC (Xamarin.ios)] 레이아웃 형식")](layouts-images/layouts.png#lightbox "[! OP. 비 LOC (Xamarin.ios)] 레이아웃 형식")

에서 파생 되는 클래스는 `Layout` 두 가지 범주로 나눌 수 있습니다.

## <a name="layouts-with-single-content"></a>단일 콘텐츠가 있는 레이아웃

이러한 클래스 [`Layout`](xref:Xamarin.Forms.Layout) 는 및 속성을 정의 하는에서 파생 [`Padding`](xref:Xamarin.Forms.Layout.Padding) [`IsClippedToBounds`](xref:Xamarin.Forms.Layout.IsClippedToBounds) 됩니다.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView)속성을 사용 하 여 설정 된 단일 자식을 포함 [`Content`](xref:Xamarin.Forms.ContentView.Content) 합니다. `Content`속성은 다른 파생물을 포함 하 여 모든 파생으로 설정할 수 있습니다 `View` `Layout` . `ContentView`는 주로 구조 요소로 사용 되며에 대 한 기본 클래스 역할을 [`Frame`](#frame) 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ContentView)  /  [가이드](~/xamarin-forms/user-interface/layouts/contentview.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/) | [![ContentView 예제](layouts-images/ContentView.png "ContentView 예제")](layouts-images/ContentView-Large.png#lightbox "ContentView 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>프레임

|     |     |
| --- | --- |
| [`Frame`](xref:Xamarin.Forms.Frame)클래스는에서 파생 [`ContentView`](#contentView) 되 고 자식 주위에 테두리 또는 프레임을 표시 합니다. `Frame`클래스의 기본값은 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 20이 고, [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor) [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius) 및 속성도 정의 [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Frame)  /  [가이드](~/xamarin-forms/user-interface/layouts/frame.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/) | [![프레임 예제](layouts-images/Frame.png "프레임 예제")](layouts-images/Frame-Large.png#lightbox "프레임 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView)는 내용을 스크롤할 수 있습니다. 속성을 [`Content`](xref:Xamarin.Forms.ScrollView.Content) 보기 또는 레이아웃으로 너무 커서 화면에 맞게 설정 합니다. 의 내용은 `ScrollView` 매우 일반적입니다 [`StackLayout`](#stackLayout) . 스크롤을 [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) 세로, 수평 또는 둘 다로 지정 하려면 속성을 설정 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ScrollView)  /  [가이드](~/xamarin-forms/user-interface/layouts/scroll-view.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![ScrollView 예제](layouts-images/ScrollView.png "ScrollView 예제")](layouts-images/ScrollView-Large.png#lightbox "ScrollView 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)컨트롤 템플릿을 사용 하 여 콘텐츠를 표시 하 고의 기본 클래스입니다 [`ContentView`](#contentView) .<br /><br />[API 설명서](xref:Xamarin.Forms.TemplatedView)  /  [가이드](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![TemplatedView 예제](layouts-images/TemplatedView.png "TemplatedView 예제")](layouts-images/TemplatedView.png#lightbox "TemplatedView 예제") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)는에서 제공 되는 콘텐츠가 표시 되는 위치를 표시 하는 데 사용 되는 템플릿 기반 뷰의 레이아웃 관리자입니다 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) .<br /><br />[API 설명서](xref:Xamarin.Forms.ContentPresenter)  /  [가이드](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![ContentPresenter 예제](layouts-images/ContentPresenter.png "ContentPresenter 예제")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter 예제") |
|     |     |

## <a name="layouts-with-multiple-children"></a>여러 자식이 있는 레이아웃

이러한 클래스는에서 파생 [`Layout<View>`](xref:Xamarin.Forms.Layout`1) 됩니다.

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout)속성에 따라 가로 또는 세로로 스택에 자식 요소를 배치 [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) 합니다. [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)속성은 자식 사이의 간격을 제어 하며 기본값은 6입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.StackLayout)  /  [가이드](~/xamarin-forms/user-interface/layouts/stacklayout.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)| [![StackLayout 예제](layouts-images/StackLayout.png "StackLayout 예제")](layouts-images/StackLayout-Large.png#lightbox "StackLayout 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>그리드

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid)자식 요소를 행과 열의 표로 배치 합니다. 자식의 위치는 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md) ,, 및를 사용 하 여 표시 됩니다 [`Row`](xref:Xamarin.Forms.Grid.RowProperty) [`Column`](xref:Xamarin.Forms.Grid.ColumnProperty) [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) .<br /><br />[API 설명서](xref:Xamarin.Forms.Grid)  /  [가이드](~/xamarin-forms/user-interface/layouts/grid.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![표 예](layouts-images/Grid.png "표 예")](layouts-images/Grid-Large.png#lightbox "표 예")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)부모를 기준으로 특정 위치에 자식 요소를 배치 합니다. 자식 위치는 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md) 및를 사용 하 여 표시 됩니다 [`LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) [`LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) . 는 `AbsoluteLayout` 보기의 위치에 애니메이션 효과를 주는 데 유용 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.AbsoluteLayout)  /  [가이드](~/xamarin-forms/user-interface/layouts/absolute-layout.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![AbsoluteLayout 예제](layouts-images/AbsoluteLayout.png "AbsoluteLayout 예제")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutDemoPage.cs)  /  에 대 한 c # 코드 [코드 숨김이](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) 포함 된 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)자체 또는 해당 형제를 기준으로 자식 요소를 배치 `RelativeLayout` 합니다. 자식의 위치는 및 형식의 개체로 설정 된 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md) 을 사용 하 여 표시 됩니다 [`Constraint`](xref:Xamarin.Forms.Constraint) [`BoundsConstraint`](xref:Xamarin.Forms.Constraint) .<br /><br />[API 설명서](xref:Xamarin.Forms.RelativeLayout)  /  [가이드](~/xamarin-forms/user-interface/layouts/relative-layout.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![RelativeLayout 예제](layouts-images/RelativeLayout.png "RelativeLayout 예제")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout)는 일반적으로 _flex 레이아웃_ 또는 _flex 상자_라고 하는 CSS [유연한 상자 레이아웃 모듈](https://www.w3.org/TR/css-flexbox-1/)을 기반으로 합니다. `FlexLayout`여러 맞춤 및 방향 옵션을 사용 하 여 자식을 누적 하거나 래핑할 수 있도록 하는 바인딩 가능 속성 6 개와 연결 된 바인딩 가능 속성 5 개를 정의 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.FlexLayout)  /  [가이드](~/xamarin-forms/user-interface/layouts/flex-layout.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos) | [![안 면 레이아웃 예제](layouts-images/FlexLayout.png "안 면 레이아웃 예제")](layouts-images/FlexLayout-Large.png#lightbox "안 면 레이아웃 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms표본의](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI 설명서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
