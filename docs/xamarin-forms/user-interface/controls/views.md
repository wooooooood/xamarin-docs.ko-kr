---
title: Xamarin.Forms레이아웃
description: Xamarin.Forms뷰는 플랫폼 간 모바일 사용자 인터페이스의 빌딩 블록입니다. 이 문서에서는에 포함 된 보기를 나열 합니다 Xamarin.Forms .
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/09/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bd503ce9fd04d80fc0f791131f67f6f1a86ae84a
ms.sourcegitcommit: cd0c0999b53e825b60471bfbfd4144cfcd783587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86225690"
---
# <a name="xamarinforms-views"></a>Xamarin.Forms레이아웃

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin.Forms뷰는 플랫폼 간 모바일 사용자 인터페이스의 빌딩 블록입니다._

뷰는 다른 그래픽 프로그래밍 환경에서 일반적으로 *컨트롤이* 나 *위젯* 으로 알려진 레이블, 단추 및 슬라이더와 같은 사용자 인터페이스 개체입니다. 모두에서 지원 되는 뷰는 Xamarin.Forms 클래스에서 파생 [`View`](xref:Xamarin.Forms.View) 됩니다. 여러 범주로 나눌 수 있습니다.

## <a name="views-for-presentation"></a>프레젠테이션 뷰

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView)속성으로 색이 지정 된 실선 사각형을 표시 합니다 [`Color`](xref:Xamarin.Forms.BoxView.Color) . `BoxView`에는 40x40의 기본 크기 요청이 있습니다. 다른 크기의 경우 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 속성을 할당 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.BoxView)  /  [가이드](~/xamarin-forms/user-interface/boxview.md)  /  [Sample 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration), [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/), [4](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife), [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock), [6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![BoxView 예제](views-images/BoxView.png "BoxView 예제")](views-images/BoxView-Large.png#lightbox "BoxView 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="ellipse"></a>타원

|     |    |
| --- | ---|
| [`Ellipse`](xref:Xamarin.Forms.Shapes.Ellipse)x 크기의 타원이 나 원을 표시 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 합니다. 타원 안쪽에를 그리려면 해당 속성을로 설정 [`Fill`](xref:Xamarin.Forms.Shapes.Shape.Fill) [`Color`](xref:Xamarin.Forms.Color) 합니다. 타원에 윤곽선을 지정 하려면 해당 속성을로 설정 [`Stroke`](xref:Xamarin.Forms.Shapes.Shape.Stroke) `Color` 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Shapes.Ellipse)  /  [가이드](~/xamarin-forms/user-interface/shapes/ellipse.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos) | [![타원 예](views-images/Ellipse.png "타원 예")](views-images/Ellipse-Large.png#lightbox "타원 예")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EllipseDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EllipseDemoPage.xaml) |
|     |     |

### <a name="expander"></a>Expander

|     |     |
| --- | --- |
| [`Expander`](xref:Xamarin.Forms.Expander)모든 콘텐츠를 호스트 하는 확장 가능한 컨테이너를 제공 하며 헤더와 콘텐츠로 구성 됩니다. 속성을 헤더로 표시 되는로 설정 하 `Header` [`View`](xref:Xamarin.Forms.View) 고, 탭을 `Content` [`View`](xref:Xamarin.Forms.View) 통해 머리글을 확장할 때 표시 되는 속성을로 설정 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Expander)  /  [가이드](~/xamarin-forms/user-interface/expander.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos) | [![확장기 예제](views-images/Expander.png "확장기 예제")](views-images/Expander-Large.png#lightbox "확장기 예제")<br /> [이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ExpanderDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ExpanderDemoPage.xaml) |
|     |     |

### <a name="label"></a>레이블

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label)상수 또는 변수 서식 지정을 사용 하 여 한 줄 텍스트 문자열 또는 텍스트의 여러 줄 블록을 표시 합니다. [`Text`](xref:Xamarin.Forms.Label.Text)상수 형식에 대 한 속성을 문자열로 설정 하거나 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) [`FormattedString`](xref:Xamarin.Forms.FormattedString) 변수 서식 지정을 위해 속성을 개체로 설정 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Label)  /  [가이드](~/xamarin-forms/user-interface/text/label.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![레이블 예](views-images/Label.png "레이블 예")](views-images/Label-Large.png#lightbox "레이블 예")<br /> [이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="line"></a>꺾은선형

|     |     |
| --- | --- |
| [`Line`](xref:Xamarin.Forms.Shapes.Line)시작점에서 끝점 까지의 선을 표시 합니다. 시작점은 및 속성으로 표시 되 [`X1`](xref:Xamarin.Forms.Shapes.Line.X1) 는 [`Y1`](xref:Xamarin.Forms.Shapes.Line.Y1) 반면 끝점은 및 속성으로 표시 됩니다 [`X2`](xref:Xamarin.Forms.Shapes.Line.X2) [`Y2`](xref:Xamarin.Forms.Shapes.Line.Y2) . 줄의 색을 설정 하려면 해당 속성을로 설정 [`Stroke`](xref:Xamarin.Forms.Shapes.Shape.Stroke) [`Color`](xref:Xamarin.Forms.Color) 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Shapes.Line)  /  [가이드](~/xamarin-forms/user-interface/shapes/line.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos) | [![줄 예제](views-images/Line.png "줄 예제")](views-images/Line-Large.png#lightbox "줄 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LineDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LineDemoPage.xaml) |
|     |     |

### <a name="image"></a>이미지

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image)비트맵을 표시 합니다. 비트맵은 웹을 통해 다운로드 하거나, 공용 프로젝트 또는 플랫폼 프로젝트에 리소스로 포함 하거나, .NET 개체를 사용 하 여 만들 수 있습니다 `Stream` .<br /><br />[API 설명서](xref:Xamarin.Forms.Image)  /  [가이드](~/xamarin-forms/user-interface/images.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![이미지 예제](views-images/Image.png "이미지 예제")](views-images/Image-Large.png#lightbox "이미지 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="map"></a>맵

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map)지도를 표시 합니다. ** Xamarin.Forms 입니다. Maps** NuGet 패키지를 설치 해야 합니다. Android 및 유니버설 Windows 플랫폼에는 맵 인증 키가 필요 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Maps.Map)  /  [가이드](~/xamarin-forms/user-interface/map/index.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![지도 예제](views-images/Map.png "지도 예제")](views-images/Map-Large.png#lightbox "지도 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

### <a name="mediaelement"></a>MediaElement

|     |     |
| --- | --- |
| [`MediaElement`](xref:Xamarin.Forms.MediaElement)비디오 또는 오디오를 재생 합니다. [`Source`](xref:Xamarin.Forms.MediaElement.Source)속성이 또는로 설정 되었는지 여부에 따라 URL 또는 로컬 파일에서 미디어를 재생할 수 있습니다 [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource) [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) .<br /><br />[API 설명서](xref:Xamarin.Forms.MediaElement)  /  [가이드](~/xamarin-forms/user-interface/mediaelement.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos) | [![MediaElement 예](views-images/MediaElement.png "MediaElement 예")](views-images/MediaElement-Large.png#lightbox "MediaElement 예")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MediaElementDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MediaElementDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView)iOS 및 Android 프로젝트에서 OpenGL 그래픽을 표시 합니다. 유니버설 Windows 플랫폼는 지원 되지 않습니다. IOS 및 Android 프로젝트에는 **OpenTK-1.0** 어셈블리 또는 **OpenTK** 버전 1.0.0.0 어셈블리에 대 한 참조가 필요 합니다. `OpenGLView`공유 프로젝트에서를 사용 하는 것이 더 쉽습니다. .NET Standard 라이브러리에서 사용 되는 경우 샘플 코드에 표시 된 것과 같이 종속성 서비스도 필요 합니다.<br /><br />이 기능은에 기본 제공 되는 유일한 그래픽 기능 Xamarin.Forms 이지만 Xamarin.Forms 응용 프로그램은, 또는를 사용 하 여 그래픽을 렌더링할 수도 있습니다 [`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) [`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md) .<br /><br />[API 문서](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![OpenGLView 예제](views-images/OpenGLView.png "OpenGLView 예제")](views-images/OpenGLView-Large.png#lightbox "OpenGLView 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs)  /  에 대 한 c # 코드 [코드 숨김이](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) 포함 된 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) |
|     |     |

### <a name="path"></a>경로

|     |     |
| --- | --- |
| [`Path`](xref:Xamarin.Forms.Shapes.Path)곡선 및 복잡 한 도형을 표시 합니다. [`Data`](xref:Xamarin.Forms.Shapes.Path.Data)속성은 그릴 모양을 지정 합니다. 모양의 색을 설정 하려면 해당 속성을로 설정 [`Stroke`](xref:Xamarin.Forms.Shapes.Shape.Stroke) [`Color`](xref:Xamarin.Forms.Color) 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Shapes.Path)  /  [가이드](~/xamarin-forms/user-interface/shapes/path.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos) | [![경로 예](views-images/Path.png "경로 예")](views-images/Path-Large.png#lightbox "경로 예")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PathDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PathDemoPage.xaml) |
|     |     |

### <a name="polygon"></a>다각형

|     |     |
| --- | --- |
| [`Polygon`](xref:Xamarin.Forms.Shapes.Polygon)다각형을 표시 합니다. 속성은 다각형의 [`Points`](xref:Xamarin.Forms.Shapes.Polygon.Points) 꼭 짓 점을 지정 하는 반면 속성은 [`FillRule`](xref:Xamarin.Forms.Shapes.Polygon.FillRule) 다각형의 내부 채우기가 결정 되는 방법을 지정 합니다. 다각형 내부를 그리려면 해당 속성을로 설정 [`Fill`](xref:Xamarin.Forms.Shapes.Shape.Fill) [`Color`](xref:Xamarin.Forms.Color) 합니다. 다각형에 윤곽선을 지정 하려면 해당 속성을로 설정 [`Stroke`](xref:Xamarin.Forms.Shapes.Shape.Stroke) `Color` 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Shapes.Polygon)  /  [가이드](~/xamarin-forms/user-interface/shapes/polygon.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos) | [![Polygon 예](views-images/Polygon.png "Polygon 예")](views-images/Polygon-Large.png#lightbox "Polygon 예")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PolygonDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PolygonDemoPage.xaml) |
|     |     |

### <a name="polyline"></a>폴리라인

|     |     |
| --- | --- |
| [`Polyline`](xref:Xamarin.Forms.Shapes.Polyline)일련의 연결 된 직선을 표시 합니다. 속성은 다중선의 [`Points`](xref:Xamarin.Forms.Shapes.Polygon.Points) 꼭 짓 점을 지정 하는 반면 속성은 [`FillRule`](xref:Xamarin.Forms.Shapes.Polygon.FillRule) 다중선의 내부 채우기가 결정 되는 방법을 지정 합니다. 다중선 내부에를 그리려면 해당 속성을로 설정 [`Fill`](xref:Xamarin.Forms.Shapes.Shape.Fill) [`Color`](xref:Xamarin.Forms.Color) 합니다. 다중선에 윤곽선을 지정 하려면 해당 속성을로 설정 [`Stroke`](xref:Xamarin.Forms.Shapes.Shape.Stroke) `Color` 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Shapes.Polyline)  /  [가이드](~/xamarin-forms/user-interface/shapes/polyline.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos) | [![다중선 예](views-images/Polyline.png "다중선 예")](views-images/Polyline-Large.png#lightbox "다중선 예")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PolylineDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PolylineDemoPage.xaml) |
|     |     |

### <a name="rectangle"></a>사각형

|     |     |
| --- | --- |
| [`Rectangle`](xref:Xamarin.Forms.Shapes.Rectangle)사각형 또는 사각형을 표시 합니다. 사각형 안에를 그리려면 해당 속성을로 설정 [`Fill`](xref:Xamarin.Forms.Shapes.Shape.Fill) [`Color`](xref:Xamarin.Forms.Color) 합니다. 사각형에 윤곽선을 지정 하려면 해당 속성을로 설정 [`Stroke`](xref:Xamarin.Forms.Shapes.Shape.Stroke) `Color` 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Shapes.Rectangle)  /  [가이드](~/xamarin-forms/user-interface/shapes/rectangle.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos) | [![사각형 예제](views-images/Rectangle.png "사각형 예제")](views-images/Rectangle-Large.png#lightbox "사각형 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RectangleDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RectangleDemoPage.xaml) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView)속성이 또는 개체로 설정 되었는지 여부에 따라 웹 페이지 또는 HTML 콘텐츠를 표시 [`Source`](xref:Xamarin.Forms.WebView.Source) [`UriWebViewSource`](xref:Xamarin.Forms.UrlWebViewSource) [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource) 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.WebView)  /  [가이드](~/xamarin-forms/user-interface/webview.md)  /  [샘플 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview) 및 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![웹 보기 예제](views-images/WebView.png "웹 보기 예제")](views-images/WebView-Large.png#lightbox "웹 보기 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>명령 시작 뷰

### <a name="button"></a>단추

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button)텍스트를 표시 하 고 눌린 이벤트를 발생 시키는 사각형 개체입니다 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) .<br /><br />[API 설명서](xref:Xamarin.Forms.Button)  /  [가이드](~/xamarin-forms/user-interface/button.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![단추 예제](views-images/Button.png "단추 예제")](views-images/Button-Large.png#lightbox "단추 예제")<br /> [이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs)  /  에 대 한 c # 코드 [코드 숨김이](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) 포함 된 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| [`ImageButton`](xref:Xamarin.Forms.ImageButton)는 이미지를 표시 하 고 눌린 이벤트를 발생 시키는 사각형 개체입니다 `Clicked` .<br /><br />[API 설명서](xref:Xamarin.Forms.ImageButton)  /  [가이드](~/xamarin-forms/user-interface/imagebutton.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![ImageButton 예제](views-images/ImageButton.png "ImageButton 예제")](views-images/ImageButton-Large.png#lightbox "ImageButton 예제")<br /> [이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs)  /  에 대 한 c # 코드 [코드 숨김이](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) 포함 된 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) |
|     |     |

### <a name="radiobutton"></a>RadioButton

|     |     |
| --- | --- |
| [`RadioButton`](xref:Xamarin.Forms.RadioButton)집합에서 하나의 옵션을 선택할 수 있도록 허용 하 고 선택이 `CheckedChanged` 발생할 때 이벤트를 발생 시킵니다.<br /><br />[API 설명서](xref:Xamarin.Forms.RadioButton)  /  [가이드](~/xamarin-forms/user-interface/radiobutton.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/) | [![RadioButton 예](views-images/RadioButton.png "RadioButton 예")](views-images/RadioButton-Large.png#lightbox "RadioButton 예")<br /> [이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RadioButtonDemoPage.cs)  /  에 대 한 c # 코드 [코드 숨김이](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RadioButtonDemoPage.xaml.cs) 포함 된 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RadioButtonDemoPage.xaml) |
|     |     |

### <a name="refreshview"></a>RefreshView

|     |     |
| --- | --- |
| [`RefreshView`](xref:Xamarin.Forms.RefreshView)는 스크롤 가능한 콘텐츠를 위한 끌어오기-새로 고침 기능을 제공 하는 컨테이너 컨트롤입니다. `ICommand`속성으로 정의 된는 `Command` 새로 고침이 트리거될 때 실행 되 고, 속성은 `IsRefreshing` 컨트롤의 현재 상태를 나타냅니다.<br /><br />[API 설명서](xref:Xamarin.Forms.RefreshView)  /  [가이드](~/xamarin-forms/user-interface/refreshview.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![RefreshView 예제](views-images/RefreshView.png "RefreshView 예제")](views-images/RefreshView-Large.png#lightbox "RefreshView 예제")<br /> [이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RefreshViewDemoPage.cs)  /  에 대 한 c # 코드 [코드 숨김이](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml.cs) 포함 된 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar)사용자가 텍스트 문자열을 입력할 영역을 표시 하 고, 응용 프로그램에 검색을 수행 하도록 신호를 표시 하는 단추 (또는 키보드 키)를 표시 합니다. [`Text`](xref:Xamarin.Forms.InputView.Text)속성은 텍스트에 대 한 액세스를 제공 하 고 [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) 이벤트는 단추를 눌렀음을 나타냅니다.<br /><br />[API 설명서](xref:Xamarin.Forms.SearchBar)  /  [가이드](~/xamarin-forms/user-interface/searchbar.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/) | [![SearchBar 예제](views-images/SearchBar.png "SearchBar 예제")](views-images/SearchBar-Large.png#lightbox "SearchBar 예제")<br /> [이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs)  /  에 대 한 c # 코드 [코드 숨김이](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) 포함 된 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) |
|     |     |

### <a name="swipeview"></a>SwipeView

|     |     |
| --- | --- |
| [`SwipeView`](xref:Xamarin.Forms.SwipeView)는 콘텐츠 항목 주위에 래핑하는 컨테이너 컨트롤이 며 살짝 밀기 제스처로 표시 되는 상황에 맞는 메뉴 항목을 제공 합니다. 각 메뉴 항목은 `SwipeItem` `Command` 항목을 탭 할 때를 실행 하는 속성이 있는로 표시 됩니다 `ICommand` .<br /><br />[API 설명서](xref:Xamarin.Forms.SwipeView)  /  [가이드](~/xamarin-forms/user-interface/swipeview.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![SwipeView 예제](views-images/SwipeView.png "SwipeView 예제")](views-images/SwipeView-Large.png#lightbox "SwipeView 예제")<br /> [이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwipeViewDemoPage.cs)  /  에 대 한 c # 코드 [코드 숨김이](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml.cs) 포함 된 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml) |
|     |     |

## <a name="views-for-setting-values"></a>값 설정 뷰

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| [`CheckBox`](xref:Xamarin.Forms.CheckBox)사용자가 선택 하거나 비워 둘 수 있는 단추 유형을 사용 하 여 부울 값을 선택할 수 있습니다. `IsChecked`속성은의 상태 이며 `CheckBox` `CheckedChanged` 상태가 변경 될 때 이벤트가 발생 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.CheckBox)  /  [가이드](~/xamarin-forms/user-interface/checkbox.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) | [![CheckBox 예](views-images/CheckBox.png "CheckBox 예")](views-images/CheckBox-Large.png#lightbox "CheckBox 예")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxPage.xaml) |
|     |     |

### <a name="slider"></a>슬라이더

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider)사용자가 `double` 및 속성으로 지정 된 연속 범위에서 값을 선택할 수 [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) 있습니다 [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) .<br /><br />[API 설명서](xref:Xamarin.Forms.Slider)  /  [가이드](~/xamarin-forms/user-interface/slider.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![슬라이더 예제](views-images/Slider.png "슬라이더 예제")](views-images/Slider-Large.png#lightbox "슬라이더 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Stepper

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper)사용자가 `double` [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) , 및 속성으로 지정 된 증분 값 범위에서 값을 선택할 수 있습니다 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) [`Increment`](xref:Xamarin.Forms.Stepper.Increment) .<br /><br />[API 설명서](xref:Xamarin.Forms.Stepper)   /  [가이드](~/xamarin-forms/user-interface/stepper.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![스텝 퍼 예제](views-images/Stepper.png "스텝 퍼 예제")](views-images/Stepper-Large.png#lightbox "스텝 퍼 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>스위치

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch)사용자가 부울 값을 선택할 수 있도록 설정/해제 스위치의 형식을 사용 합니다. [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)속성은 스위치의 상태 이며, [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) 상태가 변경 될 때 이벤트가 발생 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Switch)  /  [가이드](~/xamarin-forms/user-interface/switch.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/) | [![Switch 예](views-images/Switch.png "Switch 예")](views-images/Switch-Large.png#lightbox "Switch 예")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker)사용자가 플랫폼 날짜 선택으로 날짜를 선택할 수 있습니다. 및 속성을 사용 하 여 허용 되는 날짜 범위를 설정 [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) 합니다. [`Date`](xref:Xamarin.Forms.DatePicker.Date)속성은 선택한 날짜 이며 [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) 해당 속성이 변경 되 면 이벤트가 발생 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.DatePicker)  /  [가이드](~/xamarin-forms/user-interface/datepicker.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![DatePicker 예제](views-images/DatePicker.png "DatePicker 예제")](views-images/DatePicker-Large.png#lightbox "DatePicker 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker)사용자가 플랫폼 시간 선택으로 시간을 선택할 수 있습니다. [`Time`](xref:Xamarin.Forms.TimePicker.Time)속성은 선택한 시간입니다. 응용 프로그램에서는 `Time` 이벤트에 대 한 처리기를 설치 하 여 속성의 변경 내용을 모니터링할 수 있습니다 [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) .<br /><br />[API 설명서](xref:Xamarin.Forms.TimePicker)  /  [가이드](~/xamarin-forms/user-interface/timepicker.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![TimePicker 예제](views-images/TimePicker.png "TimePicker 예제")](views-images/TimePicker-Large.png#lightbox "TimePicker 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>텍스트 편집 뷰

이러한 두 클래스는 속성을 정의 하는 클래스에서 파생 [`InputView`](xref:Xamarin.Forms.InputView) [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) 됩니다.

### <a name="entry"></a>입력

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry)사용자가 한 줄의 텍스트를 입력 하 고 편집할 수 있습니다. 텍스트는 속성으로 사용할 수 [`Text`](xref:Xamarin.Forms.InputView.Text) 있으며, [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 및 이벤트는 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 텍스트를 변경 하거나 사용자가 enter 키를 눌러 완료 신호를 발생 시킬 때 발생 합니다.<br /><br />[`Editor`](#editor)여러 줄의 텍스트를 입력 하 고 편집 하는 데를 사용 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Entry)  /  [가이드](~/xamarin-forms/user-interface/text/entry.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![항목 예제](views-images/Entry.png "항목 예제")](views-images/Entry-Large.png#lightbox "항목 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

### <a name="editor"></a>편집기

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor)사용자가 여러 줄의 텍스트를 입력 하 고 편집할 수 있습니다. 텍스트는 속성으로 사용할 수 [`Text`](xref:Xamarin.Forms.InputView.Text) 있으며, [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 및 이벤트는 [`Completed`](xref:Xamarin.Forms.Editor.Completed) 텍스트를 변경 하거나 사용자가 신호를 전달 하는 경우에 발생 합니다.<br /><br />뷰를 사용 하 여 한 [`Entry`](#entry) 줄의 텍스트를 입력 하 고 편집 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Editor)  /  [가이드](~/xamarin-forms/user-interface/text/editor.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![항목 예제](views-images/Editor.png "편집기 예제")](views-images/Editor-Large.png#lightbox "편집기 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>작업 표시 뷰

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)애니메이션을 사용 하 여 응용 프로그램이 진행 상황을 표시 하지 않고 시간이 오래 걸리는 작업을 진행 중임을 표시 합니다. [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)속성은 애니메이션을 제어 합니다.<br /><br />활동의 진행률이 알려져 있는 경우를 [`ProgressBar`](#progressbar) 대신 사용 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ActivityIndicator)  /  [가이드](~/xamarin-forms/user-interface/activityindicator.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/) | [![ActivityIndicator 예](views-images/ActivityIndicator.png "ActivityIndicator 예")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator 예")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)애니메이션을 사용 하 여 응용 프로그램이 긴 작업을 진행 하 고 있음을 보여 줍니다. 속성을 [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) 0에서 1 사이의 값으로 설정 하 여 진행 상황을 표시 합니다.<br /><br />활동의 진행 상황을 알 수 없는 경우를 [`ActivityIndicator`](#activityindicator) 대신 사용 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ProgressBar)  /  [가이드](~/xamarin-forms/user-interface/progressbar.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/) | [![ProgressBar 예](views-images/ProgressBar.png "ProgressBar 예")](views-images/ProgressBar-Large.png#lightbox "ProgressBar 예")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs)  /  에 대 한 c # 코드 [코드 숨김이](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) 포함 된 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) |
|     |     |

## <a name="views-that-display-collections"></a>컬렉션 표시 뷰

### <a name="carouselview"></a>CarouselView

|     |     |
| --- | --- |
| [`CarouselView`](xref:Xamarin.Forms.CarouselView)스크롤 가능한 데이터 항목 목록을 표시 합니다. 속성을 `ItemsSource` 개체의 컬렉션으로 설정 하 고 `ItemTemplate` 속성을 서식을 지정 하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 방법을 설명 하는 개체로 설정 합니다. `CurrentItemChanged`이벤트는 현재 표시 된 항목이 변경 되었음을 신호로 보냅니다 .이는 속성으로 사용할 수 있습니다 `CurrentItem` .<br /><br />[가이드](~/xamarin-forms/user-interface/carouselview/index.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/) | [![CarouselView 예제](views-images/CarouselView.png "CarouselView 예제")](views-images/CarouselView-Large.png#lightbox "CarouselView 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselViewDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselViewDemoPage.xaml) |
|     |     |

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView)다른 레이아웃 사양을 사용 하 여 선택 가능한 데이터 항목의 스크롤 가능한 목록을 표시 합니다. 보다 유연 하 고 성능이 뛰어난 대안을 제공 하는 것을 목표로 [`ListView`](xref:Xamarin.Forms.ListView) 합니다. 속성을 `ItemsSource` 개체의 컬렉션으로 설정 하 고 `ItemTemplate` 속성을 서식을 지정 하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 방법을 설명 하는 개체로 설정 합니다. `SelectionChanged`이벤트는 속성으로 사용할 수 있는 선택이 되었음을 신호로 알립니다 `SelectedItem` .<br /><br />[가이드](~/xamarin-forms/user-interface/collectionview/index.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/) | [![CollectionView 예제](views-images/CollectionView.png "CollectionView 예제")](views-images/CollectionView-Large.png#lightbox "CollectionView 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) |
|     |     |

### <a name="indicatorview"></a>IndicatorView

|     |     |
| --- | --- |
| [`IndicatorView`](xref:Xamarin.Forms.IndicatorView)의 항목 수를 나타내는 표시기를 표시 `CarouselView` 합니다. 속성을 `CarouselView.IndicatorView` 개체에 설정 `IndicatorView` 하 여에 대 한 표시기를 표시 `CarouselView` 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.IndicatorView)  /  [가이드](~/xamarin-forms/user-interface/indicatorview.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/) | [![IndicatorView 예제](views-images/IndicatorView.png "IndicatorView 예제")](views-images/IndicatorView-Large.png#lightbox "IndicatorView 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/IndicatorViewDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/IndicatorViewDemoPage.xaml) |
|     |     |

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView)에서 파생 되 [`ItemsView`](xref:Xamarin.Forms.ItemsView`1) 고 선택 가능한 데이터 항목의 스크롤 가능한 목록을 표시 합니다. 속성을 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 개체의 컬렉션으로 설정 하 고 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성을 서식을 지정 하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 방법을 설명 하는 개체로 설정 합니다. [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)이벤트는 속성으로 사용할 수 있는 선택이 되었음을 신호로 알립니다 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) .<br /><br />[API 설명서](xref:Xamarin.Forms.ListView)  /  [가이드](~/xamarin-forms/user-interface/listview/index.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![ListView 예](views-images/ListView.png "ListView 예")](views-images/ListView-Large.png#lightbox "ListView 예")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

### <a name="picker"></a>선택기

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker)텍스트 문자열 목록에서 선택한 항목을 표시 하 고 뷰가 탭 될 때 해당 항목을 선택할 수 있도록 합니다. 속성을 [`Items`](xref:Xamarin.Forms.Picker.Items) 문자열 목록이 나 [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) 속성을 개체의 컬렉션으로 설정 합니다. [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged)항목이 선택 되 면 이벤트가 발생 합니다.<br /><br />는 `Picker` 선택 된 경우에만 항목 목록을 표시 합니다. [`ListView`](#listview) [`TableView`](#tableview) 페이지에 남아 있는 스크롤 가능한 목록에 또는를 사용 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Picker)  /  [가이드](~/xamarin-forms/user-interface/picker/index.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![선택 예제](views-images/Picker.png "선택 예제")](views-images/Picker-Large.png#lightbox "선택 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs)  /  에 대 한 c # 코드 [코드 숨김이](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) 포함 된 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) |
|     |     |

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView)[`Cell`](xref:Xamarin.Forms.Cell)선택적 헤더와 하위 헤더가 있는 형식의 행 목록을 표시 합니다. 속성을 [`Root`](xref:Xamarin.Forms.TableView.Root) 형식의 개체로 설정 하 [`TableRoot`](xref:Xamarin.Forms.TableRoot) 고 [`TableSection`](xref:Xamarin.Forms.TableSection) 해당에 개체를 추가 `TableRoot` 합니다. 각 `TableSection` 은 개체의 컬렉션입니다 `Cell` .<br /><br />[API 설명서](xref:Xamarin.Forms.TableView)  /  [가이드](~/xamarin-forms/user-interface/tableview.md)  /  [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![TableView 예제](views-images/TableView.png "TableView 예제")](views-images/TableView-Large.png#lightbox "TableView 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewFormDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewFormDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI 설명서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
