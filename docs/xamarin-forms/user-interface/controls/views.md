---
title: Xamarin.Forms 뷰
description: Xamarin.Forms 뷰는 플랫폼 간 모바일 사용자 인터페이스의 구성 요소입니다. 이 문서에서는 Xamarin.Forms에 포함 된 뷰를 나열 합니다.
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/14/2020
ms.openlocfilehash: 1e8b6f5e1ea090abc8ebd6084095bf6b34663a42
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635870"
---
# <a name="xamarinforms-views"></a>Xamarin.Forms 뷰

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin 양식 보기는 플랫폼 간 모바일 사용자 인터페이스의 빌딩 블록입니다._

뷰는 다른 그래픽 프로그래밍 환경에서 일반적으로 *컨트롤이* 나 *위젯* 으로 알려진 레이블, 단추 및 슬라이더와 같은 사용자 인터페이스 개체입니다. Xamarin.ios에서 지원 되는 뷰는 모두 [`View`](xref:Xamarin.Forms.View) 클래스에서 파생 됩니다. 이러한 여러 범주로 나눌 수 있습니다.

## <a name="views-for-presentation"></a>프레젠테이션 뷰

### <a name="label"></a>레이블

|     |     |
| --- | --- |
| 상수 또는 변수 서식 지정을 사용 하 여 한 줄 텍스트 문자열이 나 여러 줄 텍스트 블록을 표시 [`Label`](xref:Xamarin.Forms.Label) . [`Text`](xref:Xamarin.Forms.Label.Text) 속성을 상수 서식 지정을 위한 문자열로 설정 하거나 변수 서식 지정을 위해 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) 속성을 [`FormattedString`](xref:Xamarin.Forms.FormattedString) 개체로 설정 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Label) / [가이드](~/xamarin-forms/user-interface/text/label.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![레이블 예](views-images/Label.png "레이블 예")](views-images/Label-Large.png#lightbox "레이블 예")<br /> 이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) |
|     |     |

### <a name="image"></a>이미지

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image) 비트맵을 표시 합니다. 비트맵은 웹을 통해 다운로드 하거나, 공용 프로젝트 또는 플랫폼 프로젝트에 리소스로 포함 하거나, .NET `Stream` 개체를 사용 하 여 만들 수 있습니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Image) / [가이드](~/xamarin-forms/user-interface/images.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![이미지 예제](views-images/Image.png "이미지 예제")](views-images/Image-Large.png#lightbox "이미지 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) [`Color`](xref:Xamarin.Forms.BoxView.Color) 속성으로 색이 지정 된 단색 사각형을 표시 합니다. `BoxView`의 기본 크기 요청은 40x40입니다. 다른 크기의 경우 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 및 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성을 할당 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.BoxView) / [가이드](~/xamarin-forms/user-interface/boxview.md) / [Sample 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration), [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/), [4](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife), [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock), [6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![BoxView 예제](views-images/BoxView.png "BoxView 예제")](views-images/BoxView-Large.png#lightbox "BoxView 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) |
|     |     |

### <a name="webview"></a>웹 보기

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) [`Source`](xref:Xamarin.Forms.WebView.Source) 속성이 [`UriWebViewSource`](xref:Xamarin.Forms.UrlWebViewSource) 또는 [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource) 개체로 설정 되었는지 여부에 따라 웹 페이지 또는 HTML 콘텐츠를 표시 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.WebView) / [가이드](~/xamarin-forms/user-interface/webview.md) / [샘플 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview) 및 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![웹 보기 예제](views-images/WebView.png "웹 보기 예제")](views-images/WebView-Large.png#lightbox "웹 보기 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView) 는 IOS 및 Android 프로젝트에서 OpenGL 그래픽을 표시 합니다. 유니버설 Windows 플랫폼에 대 한 지원은 없습니다. IOS 및 Android 프로젝트에는 **OpenTK-1.0** 어셈블리 또는 **OpenTK** 버전 1.0.0.0 어셈블리에 대 한 참조가 필요 합니다. `OpenGLView`는 공유 프로젝트에서 보다 쉽게 사용할 수 있습니다. .NET Standard 라이브러리에서 사용 되는 경우 샘플 코드에 표시 된 것과 같이 종속성 서비스도 필요 합니다.<br /><br />이는 Xamarin.ios에 기본 제공 되는 유일한 그래픽 기능 이지만, Xamarin.ios 응용 프로그램은 [`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)또는 [`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md)를 사용 하 여 그래픽을 렌더링할 수도 있습니다.<br /><br />[API 문서](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![OpenGLView 예제](views-images/OpenGLView.png "OpenGLView 예제")](views-images/OpenGLView-Large.png#lightbox "OpenGLView 예제")<br />[코드 숨김으로](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) / [이 페이지에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) |
|     |     |

### <a name="map"></a>지도

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map) 지도를 표시 합니다. **Xamarin.ios** NuGet 패키지를 설치 해야 합니다. Android 및 유니버설 Windows 플랫폼 맵 권한 부여 키가 필요 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Maps.Map) / [가이드](~/xamarin-forms/user-interface/map/index.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![지도 예제](views-images/Map.png "지도 예제")](views-images/Map-Large.png#lightbox "지도 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) |
|     |     |

### <a name="mediaelement"></a>MediaElement

|     |     |
| --- | --- |
| [`MediaElement`](xref:Xamarin.Forms.MediaElement) 비디오 또는 오디오를 재생 합니다. [`Source`](xref:Xamarin.Forms.MediaElement.Source) 속성이 [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource) 또는 [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)설정 되었는지 여부에 따라 URL 또는 로컬 파일에서 미디어를 재생할 수 있습니다.<br /><br />[API 설명서](xref:Xamarin.Forms.MediaElement) / [가이드](~/xamarin-forms/user-interface/mediaelement.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos) | [![MediaElement 예](views-images/MediaElement.png "MediaElement 예")](views-images/MediaElement-Large.png#lightbox "MediaElement 예")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MediaElementDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MediaElementDemoPage.cs) |
|     |     |

## <a name="views-that-initiate-commands"></a>시작 명령 뷰

### <a name="button"></a>단추

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button) 는 텍스트를 표시 하 고 눌린 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트를 발생 시키는 사각형 개체입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Button) / [가이드](~/xamarin-forms/user-interface/button.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![단추 예제](views-images/Button.png "단추 예제")](views-images/Button-Large.png#lightbox "단추 예제")<br /> [코드 숨김으로](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) / [이 페이지에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton`는 이미지를 표시 하 고 눌린 `Clicked` 이벤트를 발생 시키는 사각형 개체입니다.<br /><br /> [가이드](~/xamarin-forms/user-interface/imagebutton.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![ImageButton 예제](views-images/ImageButton.png "ImageButton 예제")](views-images/ImageButton-Large.png#lightbox "ImageButton 예제")<br /> [코드 숨김으로](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) / [이 페이지에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs) |
|     |     |

### <a name="refreshview"></a>RefreshView

|     |     |
| --- | --- |
| `RefreshView`은 스크롤할 수 있는 콘텐츠에 대 한 끌어오기-새로 고침 기능을 제공 하는 컨테이너 컨트롤입니다. `Command` 속성으로 정의 된 `ICommand`는 새로 고침이 트리거될 때 실행 되 고, `IsRefreshing` 속성은 컨트롤의 현재 상태를 나타냅니다.<br /><br /> [가이드](~/xamarin-forms/user-interface/refreshview.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![RefreshView 예제](views-images/RefreshView.png "RefreshView 예제")](views-images/RefreshView-Large.png#lightbox "RefreshView 예제")<br /> [코드 숨김으로](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml.cs) [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml) / [이 페이지에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RefreshViewDemoPage.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar) 는 사용자가 텍스트 문자열을 입력할 수 있는 영역을 표시 하 고, 응용 프로그램에 검색을 수행 하도록 신호를 표시 하는 단추 (또는 키보드 키)를 표시 합니다. [`Text`](xref:Xamarin.Forms.InputView.Text) 속성은 텍스트에 대 한 액세스를 제공 하 고 [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) 이벤트는 단추를 눌렀음을 나타냅니다.<br /><br />[API 설명서](xref:Xamarin.Forms.SearchBar) / [가이드](~/xamarin-forms/user-interface/searchbar.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/) | [![SearchBar 예제](views-images/SearchBar.png "SearchBar 예제")](views-images/SearchBar-Large.png#lightbox "SearchBar 예제")<br /> [코드 숨김으로](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) / [이 페이지에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) |
|     |     |

### <a name="swipeview"></a>SwipeView

|     |     |
| --- | --- |
| `SwipeView`은 콘텐츠 항목 주위에 래핑하는 컨테이너 컨트롤이 며 살짝 밀기 제스처로 표시 되는 상황에 맞는 메뉴 항목을 제공 합니다. 각 메뉴 항목은 항목을 탭 할 때 `ICommand`를 실행 하는 `Command` 속성이 있는 `SwipeItem`으로 표시 됩니다.<br /><br /> [가이드](~/xamarin-forms/user-interface/swipeview.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![SwipeView 예제](views-images/SwipeView.png "SwipeView 예제")](views-images/SwipeView-Large.png#lightbox "SwipeView 예제")<br /> [코드 숨김으로](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml.cs) [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml) / [이 페이지에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwipeViewDemoPage.cs) |
|     |     |

## <a name="views-for-setting-values"></a>값 설정 뷰

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| `CheckBox`를 사용 하면 사용자가 선택 하거나 비워 둘 수 있는 단추 유형을 사용 하 여 부울 값을 선택할 수 있습니다. `IsChecked` 속성은 `CheckBox`상태 이며 상태가 변경 될 때 `CheckedChanged` 이벤트가 발생 합니다.<br /><br />API 설명서/ [가이드](~/xamarin-forms/user-interface/checkbox.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) | [![CheckBox 예](views-images/CheckBox.png "CheckBox 예")](views-images/CheckBox-Large.png#lightbox "CheckBox 예")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxPage.cs) |
|     |     |

### <a name="slider"></a>슬라이더

|     |     |
| --- | --- |
| 사용자는 [`Slider`](xref:Xamarin.Forms.Slider) 를 사용 하 여 [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) 및 [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) 속성으로 지정 된 연속 범위에서 `double` 값을 선택할 수 있습니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Slider) / [가이드](~/xamarin-forms/user-interface/slider.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![슬라이더 예제](views-images/Slider.png "슬라이더 예제")](views-images/Slider-Large.png#lightbox "슬라이더 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) |
|     |     |

### <a name="stepper"></a>Stepper

|     |     |
| --- | --- |
| 사용자는 [`Stepper`](xref:Xamarin.Forms.Stepper) 를 사용 하 여 [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum), [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum)및 [`Increment`](xref:Xamarin.Forms.Stepper.Increment) 속성으로 지정 된 증분 값 범위에서 `double` 값을 선택할 수 있습니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Stepper)  / [가이드](~/xamarin-forms/user-interface/stepper.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![스텝 퍼 예제](views-images/Stepper.png "스텝 퍼 예제")](views-images/Stepper-Large.png#lightbox "스텝 퍼 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) |
|     |     |

### <a name="switch"></a>스위치

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch) 는 설정/해제 스위치의 형식을 사용 하 여 사용자가 부울 값을 선택할 수 있도록 합니다. [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) 속성은 스위치 상태 이며 상태가 변경 될 때 [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) 이벤트가 발생 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Switch) / [가이드](~/xamarin-forms/user-interface/switch.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/) | [![Switch 예](views-images/Switch.png "Switch 예")](views-images/Switch-Large.png#lightbox "Switch 예")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| 사용자는 [`DatePicker`](xref:Xamarin.Forms.DatePicker) 를 사용 하 여 플랫폼 날짜 선택으로 날짜를 선택할 수 있습니다. [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) 및 [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) 속성을 사용 하 여 허용 되는 날짜 범위를 설정 합니다. [`Date`](xref:Xamarin.Forms.DatePicker.Date) 속성은 선택한 날짜 이며 해당 속성이 변경 될 때 [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) 이벤트가 발생 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.DatePicker) / [가이드](~/xamarin-forms/user-interface/datepicker.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![DatePicker 예제](views-images/DatePicker.png "DatePicker 예제")](views-images/DatePicker-Large.png#lightbox "DatePicker 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| 사용자는 [`TimePicker`](xref:Xamarin.Forms.TimePicker) 를 사용 하 여 플랫폼 시간 선택으로 시간을 선택할 수 있습니다. [`Time`](xref:Xamarin.Forms.TimePicker.Time) 속성은 선택한 시간입니다. 응용 프로그램은 [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) 이벤트에 대 한 처리기를 설치 하 여 `Time` 속성의 변경 내용을 모니터링할 수 있습니다.<br /><br />[API 설명서](xref:Xamarin.Forms.TimePicker) / [가이드](~/xamarin-forms/user-interface/timepicker.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![TimePicker 예제](views-images/TimePicker.png "TimePicker 예제")](views-images/TimePicker-Large.png#lightbox "TimePicker 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) |
|     |     |

## <a name="views-for-editing-text"></a>텍스트 편집 뷰

이러한 두 클래스는 [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) 속성을 정의 하는 [`InputView`](xref:Xamarin.Forms.InputView) 클래스에서 파생 됩니다.

### <a name="entry"></a>항목

|     |     |
| --- | --- |
| 사용자는 [`Entry`](xref:Xamarin.Forms.Entry) 를 사용 하 여 한 줄의 텍스트를 입력 하 고 편집할 수 있습니다. 텍스트는 [`Text`](xref:Xamarin.Forms.InputView.Text) 속성으로 사용할 수 있으며, [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 및 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트는 텍스트를 변경 하거나 사용자가 enter 키를 눌러 완료를 신호의 때 발생 합니다.<br /><br />여러 줄의 텍스트를 입력 하 고 편집 하려면 [`Editor`](#editor) 을 사용 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Entry) / [가이드](~/xamarin-forms/user-interface/text/entry.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![항목 예제](views-images/Entry.png "항목 예제")](views-images/Entry-Large.png#lightbox "항목 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) |
|     |     |

### <a name="editor"></a>편집기

|     |     |
| --- | --- |
| 사용자는 [`Editor`](xref:Xamarin.Forms.Editor) 를 사용 하 여 여러 줄의 텍스트를 입력 하 고 편집할 수 있습니다. 텍스트는 [`Text`](xref:Xamarin.Forms.InputView.Text) 속성으로 사용할 수 있으며, [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 및 [`Completed`](xref:Xamarin.Forms.Editor.Completed) 이벤트는 텍스트를 변경 하거나 사용자가 완료 하면 신호를 발생 시킵니다.<br /><br />[`Entry`](#entry) 뷰를 사용 하 여 한 줄의 텍스트를 입력 하 고 편집 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Editor) / [가이드](~/xamarin-forms/user-interface/text/editor.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![항목 예제](views-images/Editor.png "편집기 예제")](views-images/Editor-Large.png#lightbox "편집기 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) |
|     |     |

## <a name="views-to-indicate-activity"></a>작업 표시 뷰

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) 는 애니메이션을 사용 하 여 응용 프로그램이 진행 상황을 표시 하지 않고 시간이 오래 걸리는 작업을 진행 중임을 나타냅니다. [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) 속성은 애니메이션을 제어 합니다.<br /><br />활동의 진행률이 알려져 있는 경우 [`ProgressBar`](#progressbar) 를 대신 사용 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ActivityIndicator) / [가이드](~/xamarin-forms/user-interface/activityindicator.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/) | [![ActivityIndicator 예](views-images/ActivityIndicator.png "ActivityIndicator 예")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator 예")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) |
|     |     |

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) 는 애니메이션을 사용 하 여 응용 프로그램이 긴 작업을 진행 하 고 있음을 보여 줍니다. [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) 속성을 0에서 1 사이의 값으로 설정 하 여 진행 상황을 표시 합니다.<br /><br />활동의 진행 상황을 알 수 없는 경우 [`ActivityIndicator`](#activityindicator) 를 대신 사용 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ProgressBar) / [가이드](~/xamarin-forms/user-interface/progressbar.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/) | [![ProgressBar 예](views-images/ProgressBar.png "ProgressBar 예")](views-images/ProgressBar-Large.png#lightbox "ProgressBar 예")<br />[코드 숨김으로](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) / [이 페이지에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) |
|     |     |

## <a name="views-that-display-collections"></a>컬렉션 표시 뷰

### <a name="carouselview"></a>CarouselView

|     |     |
| --- | --- |
| [`CarouselView`](xref:Xamarin.Forms.CarouselView) 스크롤 가능한 데이터 항목 목록을 표시 합니다. `ItemsSource` 속성을 개체의 컬렉션으로 설정 하 고 `ItemTemplate` 속성을 항목의 형식을 지정 하는 방법을 설명 하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 개체로 설정 합니다. `CurrentItemChanged` 이벤트는 현재 표시 된 항목이 변경 되었음을 신호로 보냅니다 .이는 `CurrentItem` 속성으로 사용할 수 있습니다.<br /><br />[가이드](~/xamarin-forms/user-interface/carouselview/index.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/) | [![CarouselView 예제](views-images/CarouselView.png "CarouselView 예제")](views-images/CarouselView-Large.png#lightbox "CarouselView 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselViewDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselViewDemoPage.cs) |
|     |     |

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView) 는 다른 레이아웃 사양을 사용 하 여 선택 가능한 데이터 항목의 스크롤 가능한 목록을 표시 합니다. [`ListView`](xref:Xamarin.Forms.ListView)보다 유연 하 고 성능이 뛰어난 대안을 제공 하는 것을 목표로 합니다. `ItemsSource` 속성을 개체의 컬렉션으로 설정 하 고 `ItemTemplate` 속성을 항목의 형식을 지정 하는 방법을 설명 하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 개체로 설정 합니다. `SelectionChanged` 이벤트는 `SelectedItem` 속성으로 사용할 수 있는 선택이 되었음을 신호로 알립니다.<br /><br />[가이드](~/xamarin-forms/user-interface/collectionview/index.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/) | [![CollectionView 예제](views-images/CollectionView.png "CollectionView 예제")](views-images/CollectionView-Large.png#lightbox "CollectionView 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs) |
|     |     |

### <a name="indicatorview"></a>IndicatorView

|     |     |
| --- | --- |
| `IndicatorView` `CarouselView`의 항목 수를 나타내는 표시기를 표시 합니다. 표시기를 표시할 `CarouselView` 개체로 `IndicatorView.ItemsSourceBy` 연결 된 속성을 설정 합니다. <br /><br />[가이드](~/xamarin-forms/user-interface/indicatorview.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/) | [![IndicatorView 예제](views-images/IndicatorView.png "IndicatorView 예제")](views-images/IndicatorView-Large.png#lightbox "IndicatorView 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/IndicatorViewDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/IndicatorViewDemoPage.cs) |
|     |     |

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView) [`ItemsView`](xref:Xamarin.Forms.ItemsView`1) 에서 파생 되 고 선택 가능한 데이터 항목의 스크롤 가능한 목록을 표시 합니다. [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 속성을 개체의 컬렉션으로 설정 하 고 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성을 항목의 형식을 지정 하는 방법을 설명 하는 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 개체로 설정 합니다. [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트는 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) 속성으로 사용할 수 있는 선택이 되었음을 신호로 알립니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ListView) / [가이드](~/xamarin-forms/user-interface/listview/index.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![ListView 예](views-images/ListView.png "ListView 예")](views-images/ListView-Large.png#lightbox "ListView 예")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) |
|     |     |

### <a name="picker"></a>선택기

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker) 텍스트 문자열 목록에서 선택한 항목을 표시 하 고 뷰를 탭 할 때 해당 항목을 선택할 수 있습니다. [`Items`](xref:Xamarin.Forms.Picker.Items) 속성을 문자열 목록으로 설정 하거나 [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) 속성을 개체의 컬렉션으로 설정 합니다. [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) 이벤트는 항목을 선택할 때 발생 합니다.<br /><br />`Picker` 선택 된 경우에만 항목 목록을 표시 합니다. 페이지에 남아 있는 스크롤 가능한 목록에 [`ListView`](#listview) 또는 [`TableView`](#tableview) 를 사용 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Picker) / [가이드](~/xamarin-forms/user-interface/picker/index.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![선택 예제](views-images/Picker.png "선택 예제")](views-images/Picker-Large.png#lightbox "선택 예제")<br />[코드 숨김으로](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) / [이 페이지에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) |
|     |     |

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView) 선택적 헤더와 하위 헤더가 있는 [`Cell`](xref:Xamarin.Forms.Cell) 형식의 행 목록을 표시 합니다. [`Root`](xref:Xamarin.Forms.TableView.Root) 속성을 [`TableRoot`](xref:Xamarin.Forms.TableRoot)형식의 개체로 설정 하 고 해당 `TableRoot`에 [`TableSection`](xref:Xamarin.Forms.TableSection) 개체를 추가 합니다. 각 `TableSection`은 `Cell` 개체의 컬렉션입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.TableView) / [가이드](~/xamarin-forms/user-interface/tableview.md) / [샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![TableView 예제](views-images/TableView.png "TableView 예제")](views-images/TableView-Large.png#lightbox "TableView 예제")<br />이 페이지 / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewFormDemoPage.xaml) [에 대 한 코드 C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewFormDemoPage.cs) |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.ios 양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API 설명서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
