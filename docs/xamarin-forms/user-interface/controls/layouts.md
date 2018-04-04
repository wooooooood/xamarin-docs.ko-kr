---
title: Xamarin.Forms Layouts
description: Xamarin.Forms 레이아웃은 visual 구조로 사용자 인터페이스 컨트롤을 작성 하는 데 사용 됩니다.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 0a3164dc0da7534e6bffc011ad2fdde894d6c74a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms Layouts

_Xamarin.Forms 레이아웃은 visual 구조로 사용자 인터페이스 컨트롤을 작성 하는 데 사용 됩니다._

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) 및 [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) Xamarin.Forms의 클래스 뷰 및 기타 레이아웃에 대 한 컨테이너 역할을 하는 뷰의 특수 하위 유형이 됩니다. `Layout` 클래스 자체에서 파생 [ `View` ](views.md)합니다. A `Layout` 파생 클래스에는 일반적으로 Xamarin.Forms 응용 프로그램에서 자식 요소의 크기와 위치를 설정 하기 위한 논리가 포함 됩니다.

 [ ![](layouts-images/layouts-sml.png "Xamarin.Forms Layout Types")](layouts-images/layouts.png#lightbox "Xamarin.Forms Layout Types")

파생 된 클래스 `Layout` 두 가지 범주로 나눌 수 있습니다.

## <a name="layouts-with-single-content"></a>단일 콘텐츠로 레이아웃

이러한 클래스에서 파생 [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)를 정의 하는 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) 및 [ `IsClippedToBounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.IsClippedToBounds/) 속성입니다.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) 으로 설정 된 단일 자식이 포함 되어는 [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) 속성입니다. `Content` 모든 속성을 설정할 수 있습니다 `View` 파생 개체를 비롯 한 다른 `Layout` 파생 항목입니다. `ContentView` 구조 요소도 주로 사용 되며를 기본 클래스 역할을 [ `Frame` ](#frame)합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) | [![ContentView 예제](layouts-images/ContentView.png "ContentView 예제")](layouts-images/ContentView-Large.png#lightbox "ContentView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>프레임

|     |     |
| --- | --- |
| [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) 클래스에서 파생 [ `ContentView` ](#contentView) 자식 주위의 사각형 프레임을 표시 합니다. `Frame` 에 기본 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) 값이 20, 정의 [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/), [ `CornerRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.CornerRadius/), 및 [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/)속성입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) | [![예제 프레임](layouts-images/Frame.png "예제 프레임")](layouts-images/Frame-Large.png#lightbox "예제를 작성 합니다.")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) 해당 내용을 스크롤할 수 있습니다. 설정의 [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) 속성 보기 또는 레이아웃 너무 커서 화면에 맞게 합니다. (의 콘텐츠는 `ScrollView` 방식은 매우는 [ `StackLayout` ](#stackLayout).) 설정는 [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) 속성을 지정 하는 경우 스크롤 수직, 수평, 또는 둘 다 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) / [가이드](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![ScrollView 예제](layouts-images/ScrollView.png "ScrollView 예제")](layouts-images/ScrollView-Large.png#lightbox "ScrollView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) 컨트롤 템플릿 사용 하 여 콘텐츠를 표시 하 고는 기본 클래스에 대 한 [ `ContentView` ](#contentView)합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) / [가이드](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![TemplatedView 예제](layouts-images/TemplatedView.png "TemplatedView 예제")](layouts-images/TemplatedView.png#lightbox "TemplatedView 예제") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) 내에서 사용 하는 템플릿 기반 뷰에 대 한 레이아웃 관리자는 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 를 표시 해야 하는 내용을 표시할 위치를 표시 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) / [가이드](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![ContentPresenter 예제](layouts-images/ContentPresenter.png "ContentPresenter 예제")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter 예제") |
|     |     |

## <a name="layouts-with-multiple-children"></a>여러 자식으로 레이아웃

이러한 클래스에서 파생 [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)합니다.

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 가로 또는 세로로에 따라 스택에서 자식 요소를 배치는 [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) 속성입니다. [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) 속성은 자식 간의 간격을 제어 하며 6의 기본값입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) / [가이드](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)| [![StackLayout 예제](layouts-images/StackLayout.png "StackLayout 예제")](layouts-images/StackLayout-Large.png#lightbox "StackLayout 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [XAML 페이지]((https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml)) |
|     |     |

<a name="grid" />

### <a name="grid"></a>표

|     |     |
| --- | --- |
| [`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) 행과 열의 표의 자식 요소를 배치합니다. 사용 하 여 자녀의 위치가 표시 됩니다는 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/), [ `Column` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/), [ `RowSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/), 및 [ `ColumnSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/)합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) / [가이드](~/xamarin-forms/user-interface/layouts/grid.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![표 예제](layouts-images/Grid.png "표 예제")](layouts-images/Grid-Large.png#lightbox "표 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [XAML 페이지]((https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml)) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) 자식 요소가 부모를 기준으로 특정 위치에 배치 합니다. 사용 하 여 자녀의 위치가 표시 됩니다는 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/) 및 [ `LayoutFlags` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/)합니다. `AbsoluteLayout` 뷰 위치에 애니메이션을 적용 하는 데 유용 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) / [가이드](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![AbsoluteLayout 예제](layouts-images/AbsoluteLayout.png "AbsoluteLayout 예제")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayout.xaml) 와 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayout.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) 기준으로 자식 요소는 `RelativeLayout` 자체 또는 해당 형제입니다. 사용 하 여 자녀의 위치가 표시 됩니다는 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md) 형식의 개체에 설정 됩니다는 [ `Constraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/) 및 [ `BoundsConstraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/)합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)/ [가이드](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![RelativeLayout 예제](layouts-images/RelativeLayout.png "RelativeLayout 예제")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [XAML 페이지]((https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml)) |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery 샘플](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API 설명서](https://developer.xamarin.com/api/root/Xamarin.Forms/)
