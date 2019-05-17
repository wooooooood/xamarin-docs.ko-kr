---
title: Xamarin.Forms 뷰
description: Xamarin.Forms 뷰는 플랫폼 간 모바일 사용자 인터페이스의 구성 요소입니다. 이 문서에서는 Xamarin.Forms에 포함 된 뷰를 나열 합니다.
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/21/2019
ms.openlocfilehash: 5b2e58901d4a850863f68b26ce41e1aa4e8daee4
ms.sourcegitcommit: a9c60f50b40203dd784e3e790b0d83e2bfc86129
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/16/2019
ms.locfileid: "61358952"
---
# <a name="xamarinforms-views"></a>Xamarin.Forms 뷰

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/FormsGallery/)

_Xamarin.Forms 뷰는 플랫폼 간 모바일 사용자 인터페이스의 구성 요소입니다._

뷰는 레이블, 단추 및 일반적으로 이라고 하는 슬라이더와 같은 사용자 인터페이스 개체 *컨트롤* 또는 *위젯* 다른 그래픽 프로그래밍 환경에서입니다. 파생 되는 모든 Xamarin.Forms에서 지 원하는 뷰를 [ `View` ](xref:Xamarin.Forms.View) 클래스입니다. 이러한 여러 범주로 나눌 수 있습니다.

## <a name="views-for-presentation"></a>프레젠테이션 보기

### <a name="label"></a>레이블

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label) 한 줄 텍스트 문자열 또는 여러 줄 텍스트 블록을 사용 하 여 상수 또는 변수 서식을 표시합니다. 설정 합니다 [ `Text` ](xref:Xamarin.Forms.Label.Text) 속성을 상수 형식 지정 또는 집합에 대 한 문자열로 합니다 [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) 속성을을 [ `FormattedString` ](xref:Xamarin.Forms.FormattedString) 변수에 대 한 개체 서식 지정 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Label) / [가이드](~/xamarin-forms/user-interface/text/label.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![예제 레이블을](views-images/Label.png "예제에서는 레이블을")](views-images/Label-Large.png#lightbox "레이블 예제")<br /> [이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>이미지

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image) 비트맵을 표시합니다. 일반적인 프로젝트 또는 플랫폼 프로젝트에 리소스로 포함 또는.NET을 사용 하 여 만든 웹을 통해 비트맵을 다운로드할 수 있습니다 `Stream` 개체입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Image) / [가이드](~/xamarin-forms/user-interface/images.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![예제 이미지](views-images/Image.png "예제에서는 이미지")](views-images/Image-Large.png#lightbox "예제 이미지")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) 색이 지정 solid 사각형을 표시 합니다 [ `Color` ](xref:Xamarin.Forms.BoxView.Color) 속성입니다. `BoxView` 40 x 40의 기본 크기 요청이 있습니다. 다른 크기에 대 한 할당 합니다 [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) 하 고 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.BoxView) / [가이드](~/xamarin-forms/user-interface/boxview.md) / [1 샘플](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)하십시오 [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)를 [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)하십시오 [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), 및 [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![BoxView 예제](views-images/BoxView.png "BoxView 예제")](views-images/BoxView-Large.png#lightbox "BoxView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) 하는지에 따라 웹 페이지 또는 HTML 콘텐츠를 표시 합니다 [ `Source` ](xref:Xamarin.Forms.WebView.Source) 속성을 [ `UriWebViewSource` ](xref:Xamarin.Forms.UrlWebViewSource) 또는 [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource) 개체입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.WebView) / [가이드](~/xamarin-forms/user-interface/webview.md) / [1 샘플](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) 고 [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![WebView 예제](views-images/WebView.png "WebView 예제")](views-images/WebView-Large.png#lightbox "WebView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView) iOS 및 Android 프로젝트의 OpenGL 그래픽을 표시합니다. 유니버설 Windows 플랫폼에 대 한 지원은 없습니다. IOS 및 Android 프로젝트에 대 한 참조가 필요 합니다 **OpenTK 1.0** 어셈블리 또는 **OpenTK** 버전 1.0.0.0 어셈블리입니다. `OpenGLView` 공유 프로젝트에 사용 하기 쉬우며 .NET Standard 라이브러리를 사용 하는 경우 다음 종속성 서비스도 해야 (에서처럼 샘플 코드) 합니다.<br /><br />Xamarin.Forms에 기본 제공 되는 유일한 그래픽 기능 이지만 Xamarin.Forms 응용 프로그램을 사용 하 여 그래픽을 렌더링할 수도 있습니다 [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md)하십시오 [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), 또는 [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[API 문서](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![OpenGLView 예제](views-images/OpenGLView.png "OpenGLView 예제")](views-images/OpenGLView-Large.png#lightbox "OpenGLView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) 사용 하 여 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>맵

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map) 지도 표시합니다. 합니다 **Xamarin.Forms.Maps** Nuget 패키지를 설치 해야 합니다. Android 및 유니버설 Windows 플랫폼 맵 권한 부여 키가 필요 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Maps.Map) / [가이드](~/xamarin-forms/user-interface/map.md) / [샘플](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![예제 매핑](views-images/Map.png "예제를 매핑하려면")](views-images/Map-Large.png#lightbox "매핑 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>명령을 시작 하는 뷰

### <a name="button"></a>단추

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button) 텍스트를 표시 하는 사각형 개체 및 발생 하는 한 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) 를 누르면 이벤트.<br /><br />[API 설명서](xref:Xamarin.Forms.Button) / [가이드](~/xamarin-forms/user-interface/button.md) / [샘플](https://developer.xamarin.com/samples/UserInterface/ButtonDemos/) | [![예제에서는 단추](views-images/Button.png "예제에서는 단추")](views-images/Button-Large.png#lightbox "단추 예제")<br /> [이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) 사용 하 여 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton` 해당 이미지를 표시 하 고 발생 하는 개체는 사각형을 `Clicked` 를 누르면 이벤트입니다.<br /><br /> [가이드](~/xamarin-forms/user-interface/imagebutton.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/) | [![ImageButton 예제](views-images/ImageButton.png "ImageButton 예제")](views-images/ImageButton-Large.png#lightbox "ImageButton 예제")<br /> [이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) 사용 하 여 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar) 검색을 수행 하려면 응용 프로그램을 알리는 텍스트 문자열을 입력 하 고 단추 (또는 키보드 키)를 사용자에 대 한 영역을 표시 합니다. 합니다 [ `Text` ](xref:Xamarin.Forms.SearchBar.Text) 속성은 텍스트에 대 한 액세스를 제공 하며 [ `SearchButtonPressed` ](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) 이벤트 단추를 눌렀음을 나타냅니다.<br /><br />[API 문서](xref:Xamarin.Forms.SearchBar) | [![SearchBar 예제](views-images/SearchBar.png "SearchBar 예제")](views-images/SearchBar-Large.png#lightbox "SearchBar 예제")<br /> [이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) 사용 하 여 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>값을 설정 하는 것에 대 한 보기

### <a name="slider"></a>슬라이더

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider) 사용자가 선택할 수 있습니다는 `double` 지정 된 연속 범위에서 값을 [ `Minimum` ](xref:Xamarin.Forms.Slider.Minimum) 및 [ `Maximum` ](xref:Xamarin.Forms.Slider.Maximum) 속성.<br /><br />[API 설명서](xref:Xamarin.Forms.Slider) / [가이드](~/xamarin-forms/user-interface/slider.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) | [![슬라이더 예제](views-images/Slider.png "슬라이더 예제")](views-images/Slider-Large.png#lightbox "슬라이더 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>스텝 퍼

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper) 사용자가 선택할 수 있습니다는 `double` 증분 값을 사용 하 여 지정 된 범위에서 값을 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum)를 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum), 및 [ `Increment` ](xref:Xamarin.Forms.Stepper.Increment) 속성입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Stepper)  / [가이드](~/xamarin-forms/user-interface/stepper.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/StepperDemos) | [![스텝 퍼 예제](views-images/Stepper.png "스텝 퍼 예제")](views-images/Stepper-Large.png#lightbox "스텝 퍼 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>전환

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch) 부울 값을 선택할 수 있도록 설정/해제 스위치의 형식을 사용 합니다. 합니다 [ `IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled) 속성은 스위치의 상태 및 [ `Toggled` ](xref:Xamarin.Forms.Switch.Toggled) 상태가 변경 될 때 이벤트가 발생 합니다.<br /><br />[API 문서](xref:Xamarin.Forms.Switch) | [![전환 예제](views-images/Switch.png "예제에서는 전환")](views-images/Switch-Large.png#lightbox "전환 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker) 플랫폼 날짜 선택기를 사용 하 여 날짜를 선택할 수 있습니다. 사용 하 여 허용 되는 날짜 범위를 설정 합니다 [ `MinimumDate` ](xref:Xamarin.Forms.DatePicker.MinimumDate) 하 고 [ `MaximumDate` ](xref:Xamarin.Forms.DatePicker.MaximumDate) 속성입니다. 합니다 [ `Date` ](xref:Xamarin.Forms.DatePicker.Date) 속성이 선택된 된 날짜와 [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) 해당 속성이 변경 될 때 이벤트가 발생 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.DatePicker) / [가이드](~/xamarin-forms/user-interface/datepicker.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![DatePicker 예제](views-images/DatePicker.png "DatePicker 예제")](views-images/DatePicker-Large.png#lightbox "DatePicker 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker) 플랫폼 시간 선택기를 사용 하 여 시간을 선택할 수 있습니다. 합니다 [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) 속성은 선택한 시간입니다. 응용 프로그램의 변경 내용을 모니터링할 수 있습니다 합니다 `Time` 에 대 한 처리기를 설치 하 여 속성을 [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) 이벤트입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.TimePicker) / [가이드](~/xamarin-forms/user-interface/timepicker.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TimePicker) | [![TimePicker 예제](views-images/TimePicker.png "TimePicker 예제")](views-images/TimePicker-Large.png#lightbox "TimePicker 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>텍스트 편집에 대 한 보기

이 두 클래스에서 파생 된 [ `InputView` ](xref:Xamarin.Forms.InputView) 클래스를 정의 하는 합니다 [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) 속성.

<a name="entry" />

### <a name="entry"></a>입력

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry) 사용자를 입력 하 고 한 줄 텍스트를 편집할 수 있습니다. 텍스트를 사용할 수 있습니다는 [ `Text` ](xref:Xamarin.Forms.Entry.Text) 속성 및 [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) 및 [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) 때 이벤트가 발생 된 텍스트를 변경 하거나 사용자 enter 키를 눌러 완료 신호를 보냅니다.<br /><br />사용 하 여는 [ `Editor` ](#editor) 를 입력 하 고 여러 줄의 텍스트를 편집 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Entry) / [가이드](~/xamarin-forms/user-interface/text/entry.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![항목 예제](views-images/Entry.png "항목 예제")](views-images/Entry-Large.png#lightbox "항목 예")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>편집기

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor) 입력 하 고 여러 줄의 텍스트를 편집할 수 있습니다. 텍스트를 사용할 수 있습니다는 [ `Text` ](xref:Xamarin.Forms.Editor.Text) 속성 및 [ `TextChanged` ](xref:Xamarin.Forms.Editor.TextChanged) 및 [ `Completed` ](xref:Xamarin.Forms.Editor.Completed) 때 이벤트가 발생 된 텍스트를 변경 하거나 사용자 완료 신호를 보냅니다.<br /><br />사용 하 여는 [ `Entry` ](#entry) 보기를 입력 하 고 텍스트 한 줄을 편집 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Editor) / [가이드](~/xamarin-forms/user-interface/text/editor.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![항목 예제](views-images/Editor.png "편집기 예제")](views-images/Editor-Large.png#lightbox "편집기 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>작업을 나타내기 위해 뷰

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) 응용 프로그램에에서 참여 하는 시간이 오래 걸리는 작업 진행률의 표시가 전혀 제공 하지 않고 표시할 애니메이션을 사용 합니다. 합니다 [ `IsRunning` ](xref:Xamarin.Forms.ActivityIndicator.IsRunning) 속성 애니메이션을 제어 합니다.<br /><br />사용 하 여 작업의 진행률을 아는 경우는 [ `ProgressBar` ](#progressbar) 대신 합니다.<br /><br />[API 문서](xref:Xamarin.Forms.ActivityIndicator) | [![ActivityIndicator 예제](views-images/ActivityIndicator.png "ActivityIndicator 예제")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) 애니메이션을 사용 하 여 응용 프로그램 시간이 오래 걸리는 작업을 통해 진행 되 고 있는지 보여 줍니다. 설정 된 [ `Progress` ](xref:Xamarin.Forms.ProgressBar.Progress) 속성을 0에서 진행률을 나타내는 1 사이의 값입니다.<br /><br />사용 하 여 작업의 진행 상태를 알 수 없는 경우는 [ `ActivityIndicator` ](#activityindicator) 대신 합니다.<br /><br />[API 문서](xref:Xamarin.Forms.ProgressBar) | [![ProgressBar 예제](views-images/ProgressBar.png "ProgressBar 예제")](views-images/ProgressBar-Large.png#lightbox "ProgressBar 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) 사용 하 여 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>컬렉션을 표시 하는 보기

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| `CollectionView` 다른 레이아웃 사양을 사용 하 여 선택할 수 있는 데이터 항목의 스크롤 가능한 목록이 표시 됩니다. 보다 유연한 제공 하려고 하 고에 효율적인 대안 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 설정 합니다 `ItemsSource` 속성을 설정 하 고 개체의 컬렉션을 `ItemTemplate` 속성을를 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 항목 형식을 지정 하는 하는 방법을 설명 하는 개체. 합니다 `SelectionChanged` 이벤트는 선택 된 내용이으로 사용할 수 있는 신호를 `SelectedItem` 속성입니다.<br /><br />[가이드](~/xamarin-forms/user-interface/collectionview/index.md) / [샘플](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/) | [![CollectionView 예제](views-images/CollectionView.png "CollectionView 예제")](views-images/CollectionView-Large.png#lightbox "CollectionView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/forms40/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/forms40/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView) 파생 [ `ItemsView` ](xref:Xamarin.Forms.ItemsView`1) 선택할 수 있는 데이터 항목의 스크롤 가능한 목록이 표시 됩니다. 설정 합니다 [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 속성을 설정 하 고 개체의 컬렉션을 [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 속성을을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 항목 하는 방법을 설명 하는 개체 형식을 지정 해야 합니다. 합니다 [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) 이벤트 신호는 선택 된 내용이으로 제공 되는 [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) 속성입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ListView) / [가이드](~/xamarin-forms/user-interface/listview/index.md) / [샘플](https://developer.xamarin.com/samples/WorkingWithListview) | [![ListView 예제](views-images/ListView.png "ListView 예제")](views-images/ListView-Large.png#lightbox "ListView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

### <a name="picker"></a>선택

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker) 텍스트 문자열의 목록에서 선택한 항목을 표시 하 고 보기를 탭 할 때 해당 항목을 선택할 수 있습니다. 설정 된 [ `Items` ](xref:Xamarin.Forms.Picker.Items) 속성을 문자열의 목록 또는 [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) 속성 개체의 컬렉션을 합니다. 합니다 [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) 이벤트 항목이 선택 될 때 발생 합니다.<br /><br />`Picker` 을 선택 하는 경우에 항목 목록을 표시 합니다. 사용 된 [ `ListView` ](#listView) 또는 [ `TableView` ](#tableView) 페이지에 남아 있는 스크롤 가능한 목록이 대 한 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.Picker) / [가이드](~/xamarin-forms/user-interface/picker/index.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![선택기 예제](views-images/Picker.png "선택기 예제")](views-images/Picker-Large.png#lightbox "선택기 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) 사용 하 여 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView) 행 형식의 목록이 표시 됩니다 [ `Cell` ](xref:Xamarin.Forms.Cell) 선택적 헤더와 없앴습니다. 설정 합니다 [ `Root` ](xref:Xamarin.Forms.TableView.Root) 형식의 개체에 속성 [ `TableRoot` ](xref:Xamarin.Forms.TableRoot), 추가한 [ `TableSection` ](xref:Xamarin.Forms.TableSection) 개체를 `TableRoot`입니다. 각 `TableSection` 컬렉션인 `Cell` 개체입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.TableView) / [가이드](~/xamarin-forms/user-interface/tableview.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![TableView 예제](views-images/TableView.png "TableView 예제")](views-images/TableView-Large.png#lightbox "TableView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms FormsGallery 샘플](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API 설명서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
