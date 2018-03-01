---
title: "요약 Chapter 2입니다. 응용 프로그램 분석"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 893030170175403c7f7d6885e924e425b4f73c05
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>요약 Chapter 2입니다. 응용 프로그램 분석

Xamarin.Forms 응용 프로그램에서 화면에서 공간을 차지 하는 개체 라고 *시각적 요소*여 캡슐화 된는 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) 클래스입니다. 시각적 요소는 이러한 클래스에 해당 하는 세 개의 범주별으로 분할 될 수 있습니다.

- [페이지](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)
- [레이아웃](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [보기](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)

A `Page` 파생 클래스는 전체 화면 또는 거의 전체 화면을 차지 합니다. 대개는 페이지의 자식은 `Layout` 시각적 자식 요소를 구성 하는 파생 클래스입니다. 자식은 `Layout` 다른 수 `Layout` 클래스 또는 `View` 파생 항목 (라고도 *요소*), 하는 텍스트, 비트맵, 슬라이더, 단추, 목록 상자 등과 같은 친숙 한 개체입니다.

이 장에서에 집중 하 여 응용 프로그램을 만드는 방법을 보여 줍니다.는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), 즉는 `View` 텍스트를 표시 하는 파생 클래스입니다.

## <a name="say-hello"></a>Hello를 표시 합니다.

Xamarin 플랫폼에 설치 하는 만들 수 있습니다 새 Xamarin.Forms 솔루션을 Visual Studio 또는 Visual Studio에서 Mac.에 대 한 [ **Hello** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) 솔루션 이식 가능한 클래스 라이브러리를 사용 하 여 공통 코드에 대 한 합니다. 수정 없이 Visual Studio에서 생성 하는 Xamarin.Forms 솔루션을 보여 줍니다. 솔루션 (2 개 만들어집니다 현재 Xamarin.Forms 솔루션 템플릿을 사용 하 여 마지막) 나와 프로젝트 6 이루어져 있습니다.

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), 다른 프로젝트에서 공유 하는 PCL 이식 가능한 클래스 라이브러리 ()
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), Android 용 응용 프로그램 프로젝트
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), iOS 용 응용 프로그램 프로젝트
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), 유니버설 Windows 플랫폼 (Windows 10 및 Windows 10 Mobile)에 대 한 응용 프로그램 프로젝트
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), Windows 8.1 대 한 응용 프로그램 프로젝트
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), Windows Phone 8.1에 대 한 응용 프로그램 프로젝트

수 시작 프로젝트의 응용 프로그램 프로젝트를 확인 하 고 빌드하고 장치 또는 시뮬레이터에서 프로그램을 실행 합니다.

Xamarin.Forms 프로그램의 여러 응용 프로그램 프로젝트를 수정 하지 않습니다. 이 프로그램을 시작 하려면 방금 아주 작은 스텁을 종종 남아 있습니다. 대부분의 포커스 이식 가능한 클래스 라이브러리의 모든 응용 프로그램에 공통적으로 적용 됩니다.

## <a name="inside-the-files"></a>파일 내부의

표시 하 여 시각적 개체는 **Hello** 프로그램의 생성자에 정의 된는 [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) 클래스입니다. `App` Xamarin.Forms 클래스에서 파생 [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)합니다.

**참조** 의 섹션은 **Hello** PCL 프로젝트에 다음 Xamarin.Forms 어셈블리에 포함 됩니다.

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

**참조** 각 플랫폼에 적용 되는 추가 어셈블리를 포함 하는 5 개의 응용 프로그램 프로젝트의 섹션:

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

정적 호출이 포함 되어 각 응용 프로그램 프로젝트 `Forms.Init` 에서 메서드는 `Xamarin.Forms` 네임 스페이스입니다. 이 Xamarin.Forms 라이브러리를 초기화합니다. 다른 버전의 `Forms.Init` 각 플랫폼에 대해 정의 됩니다. 다음 클래스에이 메서드의 호출을 확인할 수 있습니다.

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [ `App` 클래스 `OnLaunched` 메서드](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1: [ `App` 클래스 `OnLaunched` 메서드](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1: [ `App` 클래스 `OnLaunched` 메서드](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

각 플랫폼 인스턴스화해야 또한는 `App` 클래스 PCL에 위치 합니다. 이 오류에 대 한 호출에서 발생 `LoadApplication` 다음 클래스에:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

그렇지 않으면 이러한 응용 프로그램 프로젝트에는 일반 "아무 작업도 하지 않음" 프로그램입니다.

## <a name="pcl-or-sap"></a>PCL 또는 SAP?

이식 가능한 클래스 라이브러리 (PCL) 또는 공유 자산 프로젝트 (SAP)에서 공통 코드를 사용 하 여 Xamarin.Forms 솔루션을 만들려면 것 같습니다. SAP 솔루션을 만들려면 Visual Studio에서 공유 옵션을 선택 합니다. [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) 솔루션 수정 없이 SAP 템플릿을 보여 줍니다.

PCL 접근 방식 번들 플랫폼 응용 프로그램 프로젝트에서 참조 하는 라이브러리 프로젝트의 모든 공통 코드. 효과적으로 SAP 접근 방식으로 공용 코드는 모든 플랫폼 응용 프로그램 프로젝트에 존재 하 고 이들 사이에서 공유 됩니다.

대부분의 개발자 Xamarin.Forms PCL 접근 방식을 선호합니다. 이 가이드의 PCL는 솔루션의 대부분입니다. SAP를 사용 하는 포함 된 **Sap** 프로젝트 이름에 접미사입니다.

모든 Xamarin.Forms 플랫폼을 지원 하려면 PCL에서 사용 하는.NET 버전을 다음 플랫폼을 허용 해야 합니다.

- .NET Framework 4.5
- Windows 8
- Windows Phone 8.1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (클래식)

이 PC 프로필 111 라고 합니다.

SAP 접근 방식으로 공유 프로젝트에 코드 코드 실행할 수 있으며 서로 다른 다양 한 플랫폼에 대 한 C# 전처리기 지시문을 사용 하 여 (`#if`, #`elif`, 및 `#endif`) 이러한 미리 정의 된 식별자:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`
- Windows 8.1: `WINDOWS_APP`
- Windows Phone 8.1: `WINDOWS_PHONE_APP`

이 장의 하겠지만 PCL에 어떤 플랫폼에서 런타임 시 실행 중인를 확인할 수 있습니다.

## <a name="labels-for-text"></a>텍스트에 대 한 레이블

[ **Greetings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) 솔루션에 새 C# 파일을 추가 하는 방법을 보여 줍니다는 **Greetings** 프로젝트. 클래스를 정의 하는이 파일 `GreetingsPage` 에서 파생 된 `ContentPage`합니다. 이 가이드의 대부분의 프로젝트는 단일 들어 `ContentPage` 파생 클래스 이름의 접미사를 사용 하 여 프로젝트의 이름인 `Page` 추가 합니다.

`GreetingsPage` 생성자를 인스턴스화하는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 보기 텍스트를 표시 하는 Xamarin.Forms 뷰입니다. [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) 속성으로 표시 되는 텍스트는 `Label`합니다. 설정 하는이 프로그램의 `Label` 에 `Content` 속성 `ContentPage`합니다. 생성자는 `App` 클래스를 인스턴스화하고 `GreetingsPage` 로 설정 하 고 해당 `MainPage` 속성입니다.

텍스트는 페이지의 왼쪽 위 모서리에 표시 됩니다. Ios에서 겹치는 페이지의 상태 표시줄을 의미 합니다. 이 문제에 대 한 여러 가지 솔루션 가지가 있습니다.

### <a name="solution-1-include-padding-on-the-page"></a>해결 방법 1입니다. 페이지에서 안쪽 여백을 포함

설정 된 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) 속성 페이지. `Padding` 형식의 [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/), 속성 4 개를 사용 하는 구조체.

- [`Left`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Left/)
- [`Top`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Top/)
- [`Right`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Right/)
- [`Bottom`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Bottom/)

`Padding` 콘텐츠 제외 된 페이지 내의 영역을 정의 합니다. 이 통해는 `Label` iOS 상태 표시줄을 덮어쓰지 않도록 합니다.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>솔루션 2입니다. IOS (SAP에만 해당)에 대 한 안쪽 여백을 포함

C# 전처리기 지시문에서는 SAP를 사용 하 여 iOS에만 '패딩' 속성을 설정 합니다. 이 확인할는 [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) 솔루션입니다.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>솔루션 3입니다. IOS (PCL 또는 SAP)에 대 한 안쪽 여백을 포함

Xamarin.Forms 책에 사용 되는 버전에는 `Padding` 를 사용 하 여 iOS PCL 또는 SAP와 관련 속성을 선택할 수 있습니다는 [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) 또는 [ `Device.OnPlatform<T>` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform%7BT%7D/p/T/T/T/) 정적 메서드입니다. 이러한 메서드는 이제 사용 되지 않습니다.

`Device.OnPlatform` 메서드 플랫폼별 코드를 실행 하거나 플랫폼 특정 값을 선택 하는 데 사용 합니다. 확인 내부적으로의 사용은 [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) 정적 읽기 전용 속성의 멤버를 반환 하는 [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) 열거형:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/)
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/)
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) Windows 8.1, Windows Phone 8.1, 및 모든 UWP 장치입니다.
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/)을 이전에 Windows Phone 8.0을 식별 하는 데 사용 되지만 이제 사용 되지 않는
- [`Other`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Other/) 사용 되지 않습니다.

`Device.OnPlatform` 메서드는 `Device.OS` 속성 및 `TargetPlatform` 열거형은 이제 사용 되지 않는 모든 합니다. 대신를 사용 하 여는 [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) 속성과 비교는 `string` 다음 정적 필드 값을 반환 합니다.

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/)"iOS" 문자열 
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/)"Android" 문자열
- [`UWP`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/)을 참조 하는 Windows 런타임 플랫폼 "UWP" 문자열
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Windows/)Windows 런타임 (Windows 8.1 및 Windows Phone 8.1)에 대 한 "Windows" 문자열
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/)Windows Phone 8.0 용 "WinPhone" 문자열 

[ `Device.Idiom` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.Idiom/) 정적 읽기 전용 속성 관련 되어 있습니다. 멤버를 반환 합니다.는 [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/)는 다음과 같은이 멤버가 포함:

- [`Desktop`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Desktop/)
- [`Tablet`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Tablet/)
- [`Phone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Phone/)
- [`Unsupported`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Unsupported/) 사용 되지 않습니다.

IOS 및 Android에서 사이의 구분에 대 한 `Tablet` 및 `Phone` 600 단위의 세로 너비입니다. Windows 플랫폼에 대 한 `Desktop` Windows 10에서 실행 중인 UWP 응용 프로그램을 나타내는 `Tablet` 는 Windows 8.1 프로그램 및 `Phone` Windows 10 또는 Windows Phone 8.1 응용 프로그램에서 실행 중인 UWP 응용 프로그램을 나타냅니다.

## <a name="solution-3a-set-margin-on-the-label"></a>솔루션 3a 합니다. 레이블에 여백 설정

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) 속성이 책에 포함할 너무 늦 정의 된 것 뿐만 아니라 형식의 `Thickness` 에 설정할 수 있습니다는 `Label` 계산에 포함 된 보기의 외부 영역을 정의 하는 보기의 레이아웃입니다.

`Padding` 속성은 사용할 수만 [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) 및 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 파생 항목입니다. `Margin` 속성은 모두에서 사용할 수 있는 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 파생 항목입니다.

## <a name="solution-4-center-the-label-within-the-page"></a>솔루션 4입니다. 페이지 내에서 레이블을 가운데로 정렬

가운데 정렬할 수 있습니다는 `Label` 내에서 `Page` (또는 다른 위치를 8 개 중 하나에 저장)을 설정 하 여는 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 및 [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) 의 속성은 `Label` 형식의 값으로 [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)합니다. `LayoutOptions` 구조 두 속성을 정의 합니다.

- [ `Alignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) 형식의 속성이 [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/), 4 개 멤버가 포함 된 열거: [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), 왼쪽 또는 위쪽에 따라 의미는 용지 방향 [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), 즉, 오른쪽 또는 아래쪽의 방향에 따라 및 [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/)합니다.

- [ `Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) 형식의 속성이 `bool`합니다.

일반적으로 이러한 속성 직접 사용 되지 않습니다. 대신, 이러한 두 속성의 조합 제공한 형식의 8 개의 정적 읽기 전용 속성 `LayoutOptions`:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`HorizontalOptions` 및 `VerticalOptions` Xamarin.Forms 레이아웃에서 가장 중요 한 속성은 및에서 더 자세하게 설명 [ **Chapter 4입니다. 스택 스크롤**](chapter04.md)합니다.

다음은 결과를 `HorizontalOptions` 및 `VerticalOptions` 의 속성 `Label` 모두으로 설정 된 `LayoutOptions.Center`:

[![Greetings 프로그램의 삼중 스크린 샷](images/ch02fg05-small.png "가운데에 레이블을 가로 및 세로로")](images/ch02fg05-large.png "가로 및 세로로 가운데 맞춤 레이블")

## <a name="solution-5-center-the-text-within-the-label"></a>솔루션 5입니다. 레이블 내에서 텍스트를 가운데에

텍스트를 가운데 (하거나 수도 페이지에서 다른 8 개 위치에 저장)을 설정 하 여는 [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) 및 [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) 의 속성 `Label` 는 의멤버에[ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) 열거형:

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/)의미 왼쪽 또는 위쪽 (방향)에 따라 다름
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.End/)오른쪽 또는 아래쪽 (방향)에 따라 의미

이러한 두 속성에만 정의 된 `Label`반면는 `HorizontalAlignment` 및 `VerticalAlignment` 속성을 정의한 `View` 되었고 모든 상속 `View` 파생 항목입니다. 시각적 결과 유사할 수 있지만 다음 장에서 설명 된 것 처럼 매우 달라 집니다.



## <a name="related-links"></a>관련 링크

- [전체 텍스트 2 장 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [2 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [2 장 F # 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Xamarin.Forms를 시작 하기](~/xamarin-forms/get-started/index.md)
