---
title: Xamarin.Forms 레이아웃
description: Xamarin.Forms 레이아웃은 visual 구조로 사용자 인터페이스 컨트롤을 작성 하는 데 사용 됩니다. 이 문서에서는 Xamarin.Forms에 포함 된 레이아웃을 나열 합니다.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 4747ce6555a6440c687dc3d239d75307f68683ca
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724484"
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms 레이아웃

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin.ios 레이아웃은 사용자 인터페이스 컨트롤을 시각적 구조체로 구성 하는 데 사용 됩니다._

Xamarin.ios의 [`Layout`](xref:Xamarin.Forms.Layout) 및 [`Layout<T>`](xref:Xamarin.Forms.Layout`1) 클래스는 보기 및 기타 레이아웃의 컨테이너 역할을 하는 뷰의 특수 한 하위 형식입니다. `Layout` 클래스 자체는 [`View`](views.md)에서 파생 됩니다. 일반적으로 `Layout` 파생물은 Xamarin.ios 응용 프로그램에서 자식 요소의 위치와 크기를 설정 하는 논리를 포함 합니다.

[![Xamarin.ios 레이아웃 형식](layouts-images/layouts-sml.png "Xamarin.ios 레이아웃 형식")](layouts-images/layouts.png#lightbox "Xamarin.ios 레이아웃 형식")

`Layout`에서 파생 되는 클래스는 두 가지 범주로 나눌 수 있습니다.

## <a name="layouts-with-single-content"></a>단일 콘텐츠와 레이아웃

이러한 클래스는 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 및 [`IsClippedToBounds`](xref:Xamarin.Forms.Layout.IsClippedToBounds) 속성을 정의 하는 [`Layout`](xref:Xamarin.Forms.Layout)에서 파생 됩니다.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView) 는 [`Content`](xref:Xamarin.Forms.ContentView.Content) 속성으로 설정 된 단일 자식을 포함 합니다. `Content` 속성은 다른 `Layout` 파생물을 포함 하 여 모든 `View` 파생 된 것으로 설정할 수 있습니다. `ContentView`는 주로 구조적 요소로 사용 되며 [`Frame`](#frame)기본 클래스 역할을 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ContentView) / [가이드](~/xamarin-forms/user-interface/layouts/contentview.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/) | [![ContentView 예제](layouts-images/ContentView.png "ContentView 예제")](layouts-images/ContentView-Large.png#lightbox "ContentView 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) |
|     |     |

<a named="frame" />

### <a name="frame"></a>프레임

|     |     |
| --- | --- |
| [`Frame`](xref:Xamarin.Forms.Frame) 클래스는 [`ContentView`](#contentView) 에서 파생 되 고 해당 자식 주위에 테두리 또는 프레임을 표시 합니다. `Frame` 클래스의 기본 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 값은 20 이며 [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor), [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)및 [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) 속성도 정의 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Frame) / [가이드](~/xamarin-forms/user-interface/layouts/frame.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/) | [![프레임 예제](layouts-images/Frame.png "프레임 예제")](layouts-images/Frame-Large.png#lightbox "프레임 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView) 내용을 스크롤할 수 있습니다. [`Content`](xref:Xamarin.Forms.ScrollView.Content) 속성을 보기 또는 레이아웃으로 너무 커서 화면에 맞게 설정 합니다. `ScrollView`의 콘텐츠는 [`StackLayout`](#stackLayout)매우 많습니다. [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) 속성을 설정 하 여 스크롤이 세로, 가로 또는 둘 다 인지 여부를 표시 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ScrollView) / [가이드](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![ScrollView 예제](layouts-images/ScrollView.png "ScrollView 예제")](layouts-images/ScrollView-Large.png#lightbox "ScrollView 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) 는 컨트롤 템플릿을 사용 하 여 콘텐츠를 표시 하 고 [`ContentView`](#contentView)의 기본 클래스입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.TemplatedView) / [가이드](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![TemplatedView 예제](layouts-images/TemplatedView.png "TemplatedView 예제")](layouts-images/TemplatedView.png#lightbox "TemplatedView 예제") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) 은 제공 될 콘텐츠가 표시 되는 위치를 표시 하기 위해 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 내에서 사용 되는 템플릿 기반 뷰의 레이아웃 관리자입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ContentPresenter) / [가이드](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![ContentPresenter 예제](layouts-images/ContentPresenter.png "ContentPresenter 예제")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter 예제") |
|     |     |

## <a name="layouts-with-multiple-children"></a>여러 자식 요소를 사용 하 여 레이아웃

이러한 클래스는 [`Layout<View>`](xref:Xamarin.Forms.Layout`1)에서 파생 됩니다.

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) 속성에 따라 가로 또는 세로로 스택에 자식 요소를 배치 합니다. [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) 속성은 자식 사이의 간격을 제어 하며 기본값은 6입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.StackLayout) / [가이드](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)| [![StackLayout 예제](layouts-images/StackLayout.png "StackLayout 예제")](layouts-images/StackLayout-Large.png#lightbox "StackLayout 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) |
|     |     |

<a name="grid" />

### <a name="grid"></a>모눈

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid) 은 자식 요소를 행과 열의 표로 배치 합니다. 자식 위치는 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md) [`Row`](xref:Xamarin.Forms.Grid.RowProperty), [`Column`](xref:Xamarin.Forms.Grid.ColumnProperty), [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty)및 [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty)를 사용 하 여 표시 됩니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Grid) / [가이드](~/xamarin-forms/user-interface/layouts/grid.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![표 예](layouts-images/Grid.png "표 예")](layouts-images/Grid-Large.png#lightbox "표 예")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| 부모를 기준으로 특정 위치에 자식 요소를 배치 [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 합니다. 자식 위치는 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md) [`LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) 및 [`LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)를 사용 하 여 표시 됩니다. `AbsoluteLayout`는 보기의 위치에 애니메이션 효과를 주는 데 유용 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.AbsoluteLayout) / [가이드](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![AbsoluteLayout 예제](layouts-images/AbsoluteLayout.png "AbsoluteLayout 예제")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout 예제")<br />[코드 숨김으로](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) / [이 페이지에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutDemoPage.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) `RelativeLayout` 자체 또는 해당 형제에 상대적인 자식 요소를 배치 합니다. 자식 위치는 [`Constraint`](xref:Xamarin.Forms.Constraint) 형식의 개체로 설정 된 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md) 및 [`BoundsConstraint`](xref:Xamarin.Forms.Constraint)를 사용 하 여 표시 됩니다.<br /><br />[API 설명서](xref:Xamarin.Forms.RelativeLayout) / [가이드](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![RelativeLayout 예제](layouts-images/RelativeLayout.png "RelativeLayout 예제")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) 은 일반적으로 _flex 레이아웃_ 또는 _FLEX 상자_라고 하는 CSS [유연한 상자 레이아웃 모듈](https://www.w3.org/TR/css-flexbox-1/)을 기반으로 합니다. `FlexLayout`는 여러 맞춤 및 방향 옵션을 사용 하 여 자식을 누적 하거나 래핑할 수 있도록 하는 바인딩 가능한 속성 6 개와 연결 된 바인딩 가능 속성 5 개를 정의 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.FlexLayout) / [가이드](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos) | [![안 면 레이아웃 예제](layouts-images/FlexLayout.png "안 면 레이아웃 예제")](layouts-images/FlexLayout-Large.png#lightbox "안 면 레이아웃 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.ios 양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API 설명서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
