---
title: "Xamarin.Forms 뷰"
description: "Xamarin.Forms 뷰는 플랫폼 간 모바일 사용자 인터페이스의 구성 요소입니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: ef4de2d544f3bcfb661b29dd90de738ae0442373
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/16/2018
---
# <a name="xamarinforms-views"></a>Xamarin.Forms 뷰

_Xamarin.Forms 뷰는 플랫폼 간 모바일 사용자 인터페이스의 구성 요소입니다._

뷰는 레이블, 단추 및 이라고 슬라이더와 같은 사용자 인터페이스 개체 *컨트롤* 또는 *위젯* 다른 그래픽 프로그래밍 환경에서입니다. 파생 되는 모든 Xamarin.Forms에서 지 원하는 뷰에 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 클래스입니다. 여러 범주로 나눌 수 있습니다.

## <a name="views-for-presentation"></a>프레젠테이션에 대 한 보기

### <a name="label"></a>레이블

|     |     |
| --- | --- |
| [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 한 줄 텍스트 문자열이 나 여러 줄 텍스트 블록을 상수 또는 변수일 서식을 사용 하 여 하나를 표시합니다. 설정의 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) 속성을 상수 서식 또는 집합에 대 한 문자열로 [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) 속성을 한 [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/) 변수에 대 한 개체 서식 지정 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) / [가이드](~/xamarin-forms/user-interface/text/label.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![예제 레이블을](views-images/Label.png "예제 레이블을")](views-images/Label-Large.png#lightbox "레이블 예제")<br /> [이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>이미지

|     |     |
| --- | --- |
| [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 비트맵을 표시합니다. 비트맵 공통 프로젝트 또는 플랫폼 프로젝트에 리소스로 포함 또는.NET을 사용 하 여 만든 웹을 통해 다운로드할 수 있습니다 `Stream` 개체입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) / [가이드](~/xamarin-forms/user-interface/images.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![예제 이미지](views-images/Image.png "예제 이미지")](views-images/Image-Large.png#lightbox "예제 이미지")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) 색이 지정 단색 사각형 표시는 [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) 속성입니다. `BoxView` 40 x 40의 기본 크기 요청을 있습니다. 다른 크기에 대 한 할당은 [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) 및 [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) 속성입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) / [가이드](~/xamarin-forms/user-interface/boxview.md) / [예제 1](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView), [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration), [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife), [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), 및 [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![BoxView 예제](views-images/BoxView.png "BoxView 예제")](views-images/BoxView-Large.png#lightbox "BoxView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>웹 보기

|     |     |
| --- | --- |
| [`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) 지 여부에 따라 웹 페이지 또는 HTML 콘텐츠를 표시는 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) 속성이로 설정 되는 [ `UriWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UrlWebViewSource/) 또는 [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/) 개체입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) / [가이드](~/xamarin-forms/user-interface/webview.md) / [예제 1](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) 및 [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![WebView 예제](views-images/WebView.png "WebView 예제")](views-images/WebView-Large.png#lightbox "WebView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/) iOS 및 Android 프로젝트에 OpenGL 그래픽을 표시합니다. 유니버설 Windows 플랫폼에 대 한 지원은 없습니다. IOS 및 Android 프로젝트에 대 한 참조가 필요는 **OpenTK 1.0** 어셈블리 또는 **OpenTK** 버전 1.0.0.0 어셈블리입니다. `OpenGLView` 더 쉽게 공유 프로젝트;에서 사용할 수 PCL 또는 표준.NET 라이브러리에 사용 후 종속 서비스가 (으로 샘플 코드와 같이)에 필요한 수도 있습니다.<br /><br />Xamarin.Forms에 기본 제공 되는 유일한 그래픽 기능 이지만 Xamarin.Forms 응용 프로그램을 사용 하 여 그래픽을 렌더링할 수도 있습니다 [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md), [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), 또는 [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/)<br /><br /> | [![OpenGLView 예제](views-images/OpenGLView.png "OpenGLView 예제")](views-images/OpenGLView-Large.png#lightbox "OpenGLView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) 와 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>맵

|     |     |
| --- | --- |
| [`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) 지도 표시합니다. **Xamarin.Forms.Maps** Nuget 패키지를 설치 해야 합니다. Android 및 유니버설 Windows 플랫폼 지도 인증 키가 필요 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) / [가이드](~/xamarin-forms/user-interface/map.md) / [샘플](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![예제 매핑](views-images/Map.png "예제 매핑")](views-images/Map-Large.png#lightbox "예제 매핑")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>명령을 시작 하는 뷰

### <a name="button"></a>단추

|     |     |
| --- | --- |
| [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 텍스트를 표시 하는 사각형 개체 및 발생 하는 [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) 를 누를 때 이벤트입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) | [![예제에서는 단추](views-images/Button.png "예제 단추")](views-images/Button-Large.png#lightbox "단추 예제")<br /> [이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) 와 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) 검색을 수행 하려면 응용 프로그램에 신호를 보내는 텍스트 문자열을 형식 및 단추 (또는 키보드 키)를 사용자에 대 한 영역을 표시 합니다. [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.Text/) 속성의 텍스트에 대 한 액세스를 제공 및 [ `SearchButtonPressed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/) 이벤트 단추를 눌렀음을 나타냅니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) | [![SearchBar 예제](views-images/SearchBar.png "SearchBar 예제")](views-images/SearchBar-Large.png#lightbox "SearchBar 예제")<br /> [이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) 와 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>값을 설정 하는 것에 대 한 보기 

### <a name="slider"></a>슬라이더

|     |     |
| --- | --- |
| [`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) 사용자 선택 하도록는 `double` 지정 된 연속 된 범위에서 값의 [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) 및 [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) 속성입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) | [![슬라이더 예제](views-images/Slider.png "슬라이더 예제")](views-images/Slider-Large.png#lightbox "슬라이더 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>스텝 퍼

|     |     |
| --- | --- |
| [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) 사용자 선택 하도록는 `double` 증분 값으로 지정 된 범위에서 값의 [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Minimum/), [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Maximum/), 및 [ `Increment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) 속성입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) | [![스텝 퍼 예제](views-images/Stepper.png "스텝 퍼 예제")](views-images/Stepper-Large.png#lightbox "스텝 퍼 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>전환 

|     |     |
| --- | --- |
| [`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) 부울 값을 선택할 수 있도록 하려면 켜기/끄기 스위치의 형식을 사용 합니다. [ `IsToggled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) 속성은 스위치의 상태 및 [ `Toggled` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) 상태가 변경 되 면 이벤트가 발생 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) | [![예제 전환](views-images/Switch.png "예제 전환")](views-images/Stepper-Large.png#lightbox "전환 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) 사용자를 플랫폼 날짜 선택 된 날짜를 선택할 수 있습니다. 포함 하는 허용 가능한 날짜 범위를 설정의 [ `MinimumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) 및 [ `MaximumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) 속성입니다. [ `Date` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) 속성은 선택된 된 날짜 및 [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) 해당 속성이 변경 되 면 이벤트가 발생 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) / [가이드](~/xamarin-forms/user-interface/datepicker.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![DatePicker 예제](views-images/DatePicker.png "DatePicker 예제")](views-images/DatePicker-Large.png#lightbox "DatePicker 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) 사용자를 플랫폼 시간 선택 된 시간을 선택할 수 있습니다. [ `Time` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) 속성은 선택한 시간입니다. 응용 프로그램의 변경 내용을 모니터링할 수는 `Time` 속성에 대 한 처리기를 설치 하 여는 [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) 이벤트입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) | [![TimePicker 예제](views-images/TimePicker.png "TimePicker 예제")](views-images/TimePicker-Large.png#lightbox "TimePicker 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>텍스트 편집 뷰

이 두 클래스에서 파생 되는 [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) 를 정의 하는 클래스는 [ `Keyboard` ](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) 속성입니다.

<a name="entry" />

### <a name="entry"></a>입력

|     |     |
| --- | --- |
| [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 입력 하 고 한 줄 텍스트를 편집할 수 있습니다. 텍스트를 사용할 수는 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) 속성 및 [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) 및 [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) 이 이벤트는 발생 하는 경우 텍스트를 변경 하거나 사용자 enter 키를 눌러 완료 신호를 보냅니다.<br /><br />사용 하 여 프로그램 [ `Editor` ](#editor) 를 입력 하 고 여러 줄의 텍스트를 편집 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) / [가이드](~/xamarin-forms/user-interface/text/entry.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![항목 예](views-images/Entry.png "항목 예")](views-images/Entry-Large.png#lightbox "항목 예")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>편집기

|     |     |
| --- | --- |
| [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) 입력 하 고 여러 줄의 텍스트를 편집할 수 있습니다. 텍스트를 사용할 수는 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Editor.Text/) 속성 및 [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) 및 [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) 이 이벤트는 발생 하는 경우 텍스트를 변경 하거나 사용자 완료 신호를 보냅니다.<br /><br />사용 하 여 프로그램 [ `Entry` ](#entry) 보기를 입력 하 고 한 줄 텍스트를 편집 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) / [가이드](~/xamarin-forms/user-interface/text/editor.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![항목 예](views-images/Editor.png "편집기 예제")](views-images/Editor-Large.png#lightbox "편집기 예")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>뷰를 작업의 징후 일

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) 애니메이션을 사용 하 여는 응용 프로그램에에서 참여 하는 오래 걸리는 작업 진행 상태를 알리는 표시가 제공 하지 않고도 보여 줍니다. [ `IsRunning` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) 속성 애니메이션을 제어 합니다.<br /><br />사용 하 여 작업의 진행 상태를 알 경우는 [ `ProgressBar` ](#progressbar) 대신 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) | [![ActivityIndicator 예제](views-images/ActivityIndicator.png "ActivityIndicator 예제")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) 애니메이션을 사용 하 여 오래 걸리는 작업을 통해 응용 프로그램이 진행 되 고 표시 하도록 합니다. 설정의 [ `Progress` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ProgressBar.Progress/) 속성을 0과 진행률을 나타내는 1 사이의 값입니다.<br /><br />작업의 진행 상태를 알 수 없는 경우 사용 하 여 프로그램 [ `ActivityIndicator` ](#activityindicator) 대신 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) | [![ProgressBar 예제](views-images/ProgressBar.png "ProgressBar 예제")](views-images/ProgressBar-Large.png#lightbox "ProgressBar 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) 와 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>컬렉션을 표시 하는 뷰

### <a name="picker"></a>선택

|     |     |
| --- | --- |
| [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 텍스트 문자열의 목록에서 선택한 항목을 표시 하 고 보기 탭이 수행 되는 경우 해당 항목을 선택할 수 있습니다. 설정의 [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) 속성을 문자열의 목록 또는 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) 속성을 개체의 컬렉션입니다. [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) 이벤트 항목이 선택 될 때 발생 합니다.<br /><br />`Picker` 을 선택 하는 경우에 항목 목록이 표시 됩니다. 사용 하 여 한 [ `ListView` ](#listView) 또는 [ `TableView` ](#tableView) 페이지에 남아 있는 스크롤 가능한 목록에 대 한 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) / [가이드](~/xamarin-forms/user-interface/picker/index.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![선택기 예제](views-images/Picker.png "선택기 예제")](views-images/Picker-Large.png#lightbox "선택기 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) 와 [코드 숨김](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 파생 [ `ItemsView[Cell]` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) 스크롤할 수 있는 선택 가능한 데이터 항목 목록이 표시 됩니다. 설정의 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) 속성 개체 및 설정의 컬렉션을는 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) 속성을 한 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 항목이 방법을 설명 하는 개체 형식이 지정 됩니다. [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) 이벤트 신호는 선택 된 내용이으로 제공 되는 [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) 속성입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) / [가이드](~/xamarin-forms/user-interface/listview/index.md) / [샘플](https://developer.xamarin.com/samples/WorkingWithListview) | [![ListView 예제](views-images/ListView.png "ListView 예제")](views-images/ListView-Large.png#lightbox "ListView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) 형식의 행의 목록을 표시 [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) subheaders 및 선택적 헤더입니다. 설정의 [ `Root` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) 속성 형식의 개체로 [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/), 추가 [ `TableSection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) 개체를 `TableRoot`합니다. 각 `TableSection` 의 컬렉션인 `Cell` 개체입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) / [가이드](~/xamarin-forms/user-interface/tableview.md) / [샘플](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![TableView 예제](views-images/TableView.png "TableView 예제")](views-images/TableView-Large.png#lightbox "TableView 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery 샘플](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API 설명서](https://developer.xamarin.com/api/root/Xamarin.Forms/)
