---
title: 요약 2 장입니다. 앱 분석
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 2 장 요약 합니다. 앱 분석'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: f900cb1532ba4415127c95b07e777881e1d74994
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724996"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>요약 2 장입니다. 앱 분석

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)

> [!NOTE]
> 이 페이지에 대 한 참고 사항 Xamarin.Forms 책의 내용을에서 달라졌는지를 위치 하는 영역을 나타냅니다.

Xamarin.ios 응용 프로그램에서 화면에 공간을 차지 하는 개체는 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 클래스에 의해 캡슐화 된 *시각적 요소*라고 합니다. 시각적 요소는 이러한 클래스에 해당 하는 세 가지 범주로 분할 될 수 있습니다.

- [호출](xref:Xamarin.Forms.Page)
- [레이아웃](xref:Xamarin.Forms.Layout)
- [보기](xref:Xamarin.Forms.View)

`Page` 파생물은 전체 화면 또는 거의 전체 화면을 차지 합니다. 일반적으로 페이지의 자식은 자식 시각적 요소를 구성 하기 위한 파생 `Layout`입니다. `Layout` 자식은 텍스트, 비트맵, 슬라이더, 단추, 목록 상자 등의 친숙 한 개체인 다른 `Layout` 클래스 또는 `View` 파생 된 ( *요소*라고도 함) 일 수 있습니다.

이 장에서는 텍스트를 표시 하는 `View` 파생물 인 [`Label`](xref:Xamarin.Forms.Label)에 초점을 맞춘 응용 프로그램을 만드는 방법을 보여 줍니다.

## <a name="say-hello"></a>살펴보기

설치 하는 Xamarin 플랫폼을 사용 하 여 만들 수 있습니다 새 Xamarin.Forms 솔루션을 Visual Studio 또는 Visual Studio for mac [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) 솔루션은 공용 코드에 이식 가능한 클래스 라이브러리를 사용 합니다.

> [!NOTE]
> Portable Class Library는 .NET Standard 라이브러리로 변경되었습니다. 이 책에서 모든 샘플 코드는 .NET 표준 라이브러리를 사용하도록 변경되었습니다.

이 샘플에는 수정 하지 않고도 Visual Studio에서 만들어진 Xamarin.Forms 솔루션을 보여 줍니다. 솔루션은 다음 네 가지 프로젝트로 구성 됩니다.

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), 다른 프로젝트에서 공유 하는 PCL (이식 가능한 클래스 라이브러리)
- [**Hello. Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), Android 용 응용 프로그램 프로젝트
- [**Hello.** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS)iOS, ios 용 응용 프로그램 프로젝트
- [**Hello. UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), 유니버설 Windows 플랫폼의 응용 프로그램 프로젝트 (windows 10 및 Windows 10 Mobile)

> [!NOTE]
> Xamarin.Forms는 더 이상 Windows 8.1, Windows Phone 8.1 또는 Windows 10 Mobile을 지원하지 않지만 Windows 10 데스크톱에서 실행은 지원합니다.

수의 응용 프로그램 프로젝트 시작 프로젝트 및 다음 빌드를 장치 또는 시뮬레이터에서 프로그램을 실행 합니다.

Xamarin.Forms 프로그램의 대부분에서는 응용 프로그램 프로젝트를 수정 하지 않습니다. 이러한 프로그램을 시작 하려면 방금 작은 스텁 종종 남아 있습니다. 대부분의 집중 라이브러리는 모든 응용 프로그램에 공통적으로 적용 됩니다.

## <a name="inside-the-files"></a>파일 내에서

**Hello** 프로그램에 의해 표시 되는 시각적 개체는 [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) 클래스의 생성자에 정의 됩니다. `App`는 Xamarin.ios 클래스 [`Application`](xref:Xamarin.Forms.Application)에서 파생 됩니다.

> [!NOTE]
> Xamarin.Forms에 대 한 Visual Studio 솔루션 템플릿 XAML 파일을 사용 하 여 페이지를 만듭니다. XAML은 [7 장](chapter07.md)까지이 서적에서 다루지 않습니다.

**Hello** PCL 프로젝트의 **참조** 섹션에는 다음과 같은 Xamarin. Forms 어셈블리가 포함 되어 있습니다.

- **Xamarin.ios. 핵심**
- **Xamarin.ios**
- **Xamarin.ios. 플랫폼**

5 개 응용 프로그램 프로젝트의 **참조** 섹션은 개별 플랫폼에 적용 되는 추가 어셈블리를 포함 합니다.

- **Xamarin.ios. Android**
- **Xamarin.ios.**
- **Xamarin.ios. Platform.object**
- **Xamarin.ios. Platform.object**
- **Xamarin.ios (xamarin.ios)**
- **Xamarin.ios..**

> [!NOTE]
> 이러한 프로젝트의 **참조** 섹션은 더 이상 어셈블리를 나열 하지 않습니다. 대신, 프로젝트 파일에는 Xamarin.ios NuGet 패키지를 참조 하는 **PackageReference** 태그가 포함 됩니다. Visual Studio의 **참조** 섹션에는 xamarin.ios 어셈블리가 아니라 **xamarin.ios** 패키지가 나열 됩니다.

각 응용 프로그램 프로젝트에는 `Xamarin.Forms` 네임 스페이스의 정적 `Forms.Init` 메서드에 대 한 호출이 포함 되어 있습니다. 이 Xamarin.Forms 라이브러리를 초기화합니다. 각 플랫폼에 대해 다른 버전의 `Forms.Init` 정의 됩니다. 다음 클래스에이 메서드의 호출을 찾을 수 있습니다.

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`App` 클래스, `OnLaunched` 메서드](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)

또한 각 플랫폼은 공유 라이브러리의 `App` 클래스 위치를 인스턴스화해야 합니다. 이는 다음 클래스의 `LoadApplication`에 대 한 호출에서 발생 합니다.

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)

그렇지 않으면 이러한 응용 프로그램 프로젝트는 일반 "아무 작업도 수행" 프로그램입니다.

## <a name="pcl-or-sap"></a>PCL 또는 SAP?

이식 가능한 클래스 라이브러리 (PCL) 또는 공유 자산 프로젝트 (SAP)에서 공통 코드를 사용 하 여 Xamarin.Forms 솔루션을 만들 수는 것입니다. SAP 솔루션을 만들려면 Visual Studio에서 공유 옵션을 선택 합니다. [**HelloSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) 솔루션은 수정 없이 SAP 템플릿을 보여 줍니다.

> [!NOTE]
> 이식 가능한 클래스 라이브러리는.NET Standard 라이브러리로 바뀌었습니다. 이 책에서 모든 샘플 코드는 .NET 표준 라이브러리를 사용하도록 변경되었습니다. 그렇지 않은 경우 PCL 및.NET Standard 라이브러리는 개념적으로 유사 합니다.

모든 일반적인 플랫폼 응용 프로그램 프로젝트에서 참조 하는 라이브러리 프로젝트에서 코드 라이브러리 접근 방식 번들입니다. SAP 방식의 경우 공통 코드는 효과적으로 모든 플랫폼 응용 프로그램 프로젝트에 존재 하 고 그 중 공유 됩니다.

대부분의 개발자 들은 Xamarin.Forms 라이브러리 접근 방식을 선호합니다. 이 책에이 나온 대부분의 솔루션이 라이브러리를 사용 합니다. SAP를 사용 하는 것은 프로젝트 이름에 **sap** 접미사를 포함 합니다.

SAP 접근 방식에서 공유 프로젝트의 코드는 다음과 같은 미리 정의 된 식별자로 전처리기 지시문 (`#if`C# , #`elif`및 `#endif`)을 사용 하 여 다양 한 플랫폼에 대해 다른 코드를 실행할 수 있습니다.

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`

공유 라이브러리의이 장 뒷부분에서 살펴보겠지만 런타임 시 실행 중인 플랫폼을 확인할 수 있습니다.

## <a name="labels-for-text"></a>텍스트에 대 한 레이블

[**인사말**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) 솔루션은 C# **인사말** 프로젝트에 새 파일을 추가 하는 방법을 보여 줍니다. 이 파일은 `ContentPage`에서 파생 되는 `GreetingsPage` 라는 클래스를 정의 합니다. 이 책에서 대부분의 프로젝트에는 이름이 추가 `Page` 접미사가 있는 프로젝트의 이름인 단일 `ContentPage` 파생물이 포함 되어 있습니다.

`GreetingsPage` 생성자는 텍스트를 표시 하는 Xamarin.ios 뷰로 된 [`Label`](xref:Xamarin.Forms.Label) 뷰를 인스턴스화합니다. [`Text`](xref:Xamarin.Forms.Label.Text) 속성은 `Label`표시 되는 텍스트로 설정 됩니다. 이 프로그램은 `Label`를 `ContentPage`의 `Content` 속성으로 설정 합니다. 그러면 `App` 클래스의 생성자가 `GreetingsPage`을 인스턴스화하고 `MainPage` 속성으로 설정 합니다.

텍스트는 페이지의 왼쪽 위 모퉁이에 표시 됩니다. Ios의 경우 페이지의 상태 표시줄을 겹치는 것을 의미 합니다. 이 문제에 대 한 솔루션 몇 가지가 있습니다.

### <a name="solution-1-include-padding-on-the-page"></a>솔루션 1입니다. 페이지의 안쪽 여백을 포함

페이지에서 [`Padding`](xref:Xamarin.Forms.Page.Padding) 속성을 설정 합니다. `Padding`은 형식 [`Thickness`](xref:Xamarin.Forms.Thickness)이며, 4 개의 속성이 있는 구조체입니다.

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding` 콘텐츠가 제외 되는 페이지 내의 영역을 정의 합니다. 이렇게 하면 `Label` iOS 상태 표시줄을 덮어쓰는 것을 방지할 수 있습니다.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>솔루션 2입니다. IOS (SAP에만 해당)에 대 한 안쪽 여백을 포함

'안쪽 여백' 속성을 사용 하 여 SAP를 사용 하 여 iOS에만 설정 된 C# 전처리기 지시문입니다. 이는 [**GreetingsSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) 솔루션에서 보여 줍니다.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>3 솔루션입니다. IOS (PCL 또는 SAP)에 대 한 안쪽 여백을 포함

책에 사용 되는 Xamarin.ios 버전에서 PCL 또는 SAP의 iOS와 관련 된 `Padding` 속성은 [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) 또는 [`Device.OnPlatform<T>`](xref:Xamarin.Forms.Device.OnPlatform*) 정적 메서드를 사용 하 여 선택할 수 있습니다. 이러한 메서드는 이제 사용 되지 않습니다.

`Device.OnPlatform` 메서드는 플랫폼별 코드를 실행 하거나 플랫폼별 값을 선택 하는 데 사용 됩니다. 내부적으로는 [`Device.OS`](xref:Xamarin.Forms.Device.OS) 정적 읽기 전용 속성을 사용 하며,이 속성은 [`TargetPlatform`](xref:Xamarin.Forms.TargetPlatform) 열거형의 멤버를 반환 합니다.

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- UWP 장치에 대 한 [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) 입니다.

`Device.OnPlatform` 메서드, `Device.OS` 속성 및 `TargetPlatform` 열거형은 모두 이제 사용 되지 않습니다. 대신 [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) 속성을 사용 하 여 `string` 반환 값을 다음 정적 필드와 비교 합니다.

- [`iOS`](xref:Xamarin.Forms.Device.iOS)"iOS" 문자열
- [`Android`](xref:Xamarin.Forms.Device.Android)"Android" 문자열
- [`UWP`](xref:Xamarin.Forms.Device.UWP)유니버설 Windows 플랫폼를 참조 하는 문자열 "UWP"

[`Device.Idiom`](xref:Xamarin.Forms.Device.Idiom) 정적 읽기 전용 속성은 서로 관련 되어 있습니다. 이러한 멤버를 포함 하는 [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom)의 멤버를 반환 합니다.

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) 사용 되지 않음

IOS 및 Android의 경우 `Tablet`와 `Phone` 간의 구분은 세로 너비 600 단위입니다. Windows 플랫폼의 경우 `Desktop`은 Windows 10에서 실행 되는 UWP 응용 프로그램을 나타내고 `Phone`는 Windows 10 응용 프로그램에서 실행 되는 UWP 응용 프로그램을 나타냅니다.

## <a name="solution-3a-set-margin-on-the-label"></a>솔루션 3a 합니다. 레이블에 여백 설정

[`Margin`](xref:Xamarin.Forms.View.Margin) 속성은 너무 늦게 도입 되어 책에 포함 될 수 있지만, `Thickness` 형식 이므로, `Label`에서이 속성을 설정 하 여 보기의 레이아웃 계산에 포함 되는 보기 외부 영역을 정의할 수 있습니다.

`Padding` 속성은 [`Layout`](xref:Xamarin.Forms.Layout) 및 [`Page`](xref:Xamarin.Forms.Page) 파생 된 경우에만 사용할 수 있습니다. `Margin` 속성은 모든 [`View`](xref:Xamarin.Forms.View) 파생물에서 사용할 수 있습니다.

## <a name="solution-4-center-the-label-within-the-page"></a>4 솔루션입니다. 페이지 내에서 레이블을 가운데합니다

`Label`의 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions)형식의 값으로 설정 하 여 `Page` 내에 `Label`을 가운데에 배치 하거나 8 개의 다른 위치 중 하나에 배치할 수 있습니다. `LayoutOptions` 구조는 다음과 같은 두 가지 속성을 정의 합니다.

- [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment)형식의 [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) 속성입니다. 네 개의 멤버가 포함 된 열거형입니다. [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start)는 방향에 따라 오른쪽 또는 아래쪽을 의미 하는 [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center) [`End`](xref:Xamarin.Forms.LayoutAlignment.End)방향에 따라 왼쪽 또는 위쪽을 의미 [합니다. ](xref:Xamarin.Forms.LayoutAlignment.Fill)`Fill`

- `bool`형식의 [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) 속성입니다.

일반적으로 이러한 속성을 직접 사용 되지 않습니다. 대신 이러한 두 속성의 조합은 `LayoutOptions`형식의 8 개의 정적 읽기 전용 속성을 통해 제공 됩니다.

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`HorizontalOptions` 및 `VerticalOptions`는 Xamarin.ios 레이아웃에서 가장 중요 한 속성 이며, 4 장에서 더 자세히 설명 합니다 [ **. 스택을 스크롤합니다**](chapter04.md).

`Label`의 `HorizontalOptions` 및 `VerticalOptions` 속성이 `LayoutOptions.Center`로 설정 된 결과는 다음과 같습니다.

[![인사말 프로그램의 삼중 스크린 샷](images/ch02fg05-small.png "가로 및 세로 가운데 맞춤 레이블")](images/ch02fg05-large.png#lightbox "가로 및 세로 가운데 맞춤 레이블")

## <a name="solution-5-center-the-text-within-the-label"></a>5 솔루션입니다. 레이블 내에서 텍스트를 가운데

`Label`의 [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) 및 [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) 속성을 [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) 열거형의 멤버로 설정 하 여 텍스트를 가운데에 배치 하거나 페이지의 8 가지 다른 위치에 배치 하 여 사용할 수도 있습니다.

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start)(방향에 따라 왼쪽 또는 위쪽)
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End)(방향에 따라 오른쪽 또는 아래쪽)

이러한 두 속성은 `Label`에 의해서만 정의 되지만 `HorizontalAlignment` 및 `VerticalAlignment` 속성은 `View`에 의해 정의 되 고 모든 `View` 파생 항목에서 상속 됩니다. Visual 결과 수 비슷해 보이지만 다음 장에서 보여 주듯이 매우 다릅니다.

## <a name="related-links"></a>관련 링크

- [2 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [2 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [2 F# 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Xamarin.ios 시작](~/get-started/index.yml)
