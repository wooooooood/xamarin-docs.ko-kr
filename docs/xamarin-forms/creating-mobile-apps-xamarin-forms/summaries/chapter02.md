---
title: 2장의 요약 정보입니다. 앱 분석
description: 'Xamarin.Forms로 모바일 앱 만들기: 2장의 요약 정보입니다. 앱 분석'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: f900cb1532ba4415127c95b07e777881e1d74994
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "76724996"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>2장의 요약 정보입니다. 앱 분석

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)

> [!NOTE]
> 이 페이지의 정보는 Xamarin.Forms가 책에 제공된 자료와 다른 영역을 표시합니다.

Xamarin.Forms 애플리케이션에서 화면의 공간을 차지하는 개체는 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 클래스로 캡슐화된 *시각적 요소*라고 합니다. 시각적 요소는 다음 클래스에 해당하는 세 가지 범주로 나눌 수 있습니다.

- [페이지](xref:Xamarin.Forms.Page)
- [레이아웃](xref:Xamarin.Forms.Layout)
- [보기](xref:Xamarin.Forms.View)

`Page` 파생 항목이 전체 화면 또는 거의 전체 화면을 차지합니다. 일반적으로 페이지의 자식은 자식 시각적 요소를 구성하기 위한 `Layout` 파생 항목입니다. `Layout`의 자식은 다른 `Layout` 클래스나 `View` 파생 항목(일반적으로 *요소*라고 함)이 될 수 있으며, 이는 텍스트, 비트맵, 슬라이더, 단추, 목록 상자 등의 친숙한 개체입니다.

이 장에서는 [`Label`](xref:Xamarin.Forms.Label)(텍스트를 표시하는 `View` 파생 항목)에 초점을 맞추고 애플리케이션을 만드는 방법을 보여 줍니다.

## <a name="say-hello"></a>인사말 Hello

Xamarin 플랫폼이 설치되어 있으면 Visual Studio 또는 Mac용 Visual Studio에서 새 Xamarin.Forms 솔루션을 만들 수 있습니다. [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) 솔루션은 공통 코드에 이식 가능한 클래스 라이브러리를 사용합니다.

> [!NOTE]
> 이식 가능한 클래스 라이브러리는 .NET Standard 라이브러리로 대체되었습니다. 이 책의 모든 샘플 코드는 .NET Standard 라이브러리를 사용하도록 변환되었습니다.

이 샘플에서는 Visual Studio에서 만든 Xamarin.Forms 솔루션을 수정 없이 보여 줍니다. 이 솔루션은 네 개의 프로젝트로 구성되어 있습니다.

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), 다른 프로젝트에서 공유하는 PCL(이식 가능한 클래스 라이브러리)
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), Android용 애플리케이션 프로젝트
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), iOS용 애플리케이션 프로젝트
- [**Hello. UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), 유니버설 Windows 플랫폼(Windows 10 및 Windows 10 Mobile)용 애플리케이션 프로젝트

> [!NOTE]
> Xamarin.Forms는 Windows 8.1, Windows Phone 8.1 또는 Windows 10 Mobile을 더 이상 지원하지 않지만 Xamarin.Forms 애플리케이션은 Windows 10 데스크톱에서 실행됩니다.

이러한 애플리케이션 프로젝트를 시작 프로젝트로 만든 다음, 디바이스 또는 시뮬레이터에서 프로그램을 빌드하고 실행할 수 있습니다.

많은 Xamarin.Forms 프로그램에서 애플리케이션 프로젝트를 수정하지 않습니다. 이는 종종 프로그램을 시작하기 위한 작은 부분으로 남아 있습니다. 대부분은 모든 애플리케이션에서 공통으로 사용되는 라이브러리에 초점을 맞춥니다.

## <a name="inside-the-files"></a>파일 기본 사항

**Hello** 프로그램에 의해 표시되는 시각적 개체는 [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) 클래스의 생성자에서 정의됩니다. `App`은 Xamarin.Forms 클래스 [`Application`](xref:Xamarin.Forms.Application)에서 파생됩니다.

> [!NOTE]
> Xamarin.Forms용 Visual Studio 솔루션 템플릿은 XAML 파일을 사용하여 페이지를 만듭니다. [7장](chapter07.md)까지는 이 책에서 XAML을 다루지 않습니다.

**Hello** PCL 프로젝트의 **참조** 섹션에는 다음과 같은 Xamarin.Forms 어셈블리가 포함됩니다.

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

5개 애플리케이션 프로젝트의 **참조** 섹션에는 개별 플랫폼에 적용되는 추가 어셈블리가 포함되어 있습니다.

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

> [!NOTE]
> 이러한 프로젝트의 **참조** 섹션에는 더 이상 어셈블리가 나열되지 않습니다. 대신, 프로젝트 파일에는 Xamarin.Forms NuGet 패키지를 참조하는 **PackageReference** 태그가 포함됩니다. Visual Studio의 **참조** 섹션에는 Xamarin.Forms 어셈블리가 아닌 **Xamarin.Forms** 패키지가 나열됩니다.

각 애플리케이션 프로젝트에는 `Xamarin.Forms` 네임스페이스의 정적 `Forms.Init` 메서드에 대한 호출이 포함됩니다. 이는 Xamarin.Forms 라이브러리를 초기화합니다. 각 플랫폼에 대해 다른 버전의 `Forms.Init`가 정의됩니다. 이 메서드에 대한 호출은 다음 클래스에서 찾을 수 있습니다.

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`App` 클래스, `OnLaunched` 메서드](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)

또한 각 플랫폼은 공유 라이브러리의 `App` 클래스 위치를 인스턴스화해야 합니다. 이는 다음 클래스의 `LoadApplication`에 대한 호출에서 발생합니다.

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)

그렇지 않으면 이러한 애플리케이션 프로젝트는 정상적인 "아무 것도 안 함" 프로그램입니다.

## <a name="pcl-or-sap"></a>PCL 또는 SAP?

PCL(이식 가능한 클래스 라이브러리) 또는 SAP(공유 자산 프로젝트)의 공통 코드를 사용하여 Xamarin.Forms 솔루션을 만들 수 있습니다. SAP 솔루션을 만들려면 Visual Studio에서 공유 옵션을 선택합니다. [**HelloSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) 솔루션에서는 SAP 템플릿을 수정 없이 보여 줍니다.

> [!NOTE]
> 이식 가능한 클래스 라이브러리는 .NET Standard 라이브러리로 대체되었습니다. 이 책의 모든 샘플 코드는 .NET Standard 라이브러리를 사용하도록 변환되었습니다. 그렇지 않으면 PCL 및 .NET Standard 라이브러리는 개념적으로 매우 유사합니다.

라이브러리 방식은 플랫폼 애플리케이션 프로젝트에서 참조하는 라이브러리 프로젝트에 모든 공통 코드를 번들로 처리합니다. SAP 방식에서는 공통 코드가 모든 플랫폼 애플리케이션 프로젝트에 효과적으로 존재하고 서로 공유됩니다.

대부분의 Xamarin.Forms 개발자는 라이브러리 방식을 선호합니다. 이 책에서 대부분의 솔루션은 라이브러리를 사용합니다. SAP를 사용하는 경우 프로젝트 이름에는 **Sap** 접미사가 포함됩니다.

SAP 방식에서 공유 프로젝트의 코드는 다음과 같이 미리 정의된 식별자가 있는 C# 전처리기 지시문(`#if`, #`elif` 및 `#endif`)을 사용하여 다양한 플랫폼에 대해 서로 다른 코드를 실행할 수 있습니다.

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`

공유 라이브러리에서는 이 장 뒷부분에 나오는 것처럼 런타임에 실행 중인 플랫폼을 확인할 수 있습니다.

## <a name="labels-for-text"></a>텍스트 레이블

[**Greetings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) 솔루션에서는 **Greetings** 프로젝트에 새 C# 파일을 추가하는 방법을 보여 줍니다. 이 파일은 `ContentPage`에서 파생되는 `GreetingsPage`라는 이름의 클래스를 정의합니다. 이 책에서 대부분의 프로젝트에는 프로젝트 이름에 `Page` 접미사를 추가하여 이름을 지정하는 단일 `ContentPage` 파생 항목이 포함되어 있습니다.

`GreetingsPage` 생성자는 [`Label`](xref:Xamarin.Forms.Label) 뷰(텍스트를 표시하는 Xamarin.Forms 뷰)를 인스턴스화합니다. [`Text`](xref:Xamarin.Forms.Label.Text) 속성은 `Label`에 표시되는 텍스트로 설정됩니다. 이 프로그램은 `Label`을 `ContentPage`의 `Content` 속성으로 설정합니다. 그러면 `App` 클래스의 생성자가 `GreetingsPage`를 인스턴스화한 후 `MainPage` 속성으로 설정합니다.

해당 페이지의 왼쪽 위에 텍스트가 표시됩니다. 이로 인해 iOS에서는 페이지의 상태 표시줄과 겹치게 됩니다. 이 문제에 대한 여러 솔루션이 있습니다.

### <a name="solution-1-include-padding-on-the-page"></a>솔루션 1. 페이지에 안쪽 여백 포함

페이지에서 [`Padding`](xref:Xamarin.Forms.Page.Padding) 속성을 설정합니다. `Padding`은 [`Thickness`](xref:Xamarin.Forms.Thickness) 형식(네 개의 속성이 포함된 구조)입니다.

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding`은 페이지 내에서 콘텐츠가 제외되는 영역을 정의합니다. 이렇게 하면 `Label`이 iOS 상태 표시줄을 덮어쓰는 것을 방지할 수 있습니다.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>솔루션 2. iOS에 대해서만 안쪽 여백 포함(SAP만 해당)

C# 전처리기 지시문이 포함된 SAP를 사용하여 iOS에서만 '안쪽 여백' 속성을 설정합니다. 여기에 대해서는 [**GreetingsSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) 솔루션에 설명되어 있습니다.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>솔루션 3. iOS에 대해서만 안쪽 여백 포함(PCL 또는 SAP)

이 책에 사용되는 Xamarin.Forms 버전에서는 PCL 또는 SAP의 iOS에만 해당하는 `Padding` 속성을 [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) 또는 [`Device.OnPlatform<T>`](xref:Xamarin.Forms.Device.OnPlatform*) 정적 메서드를 사용하여 선택할 수 있습니다. 이러한 메서드는 이제 사용되지 않습니다.

`Device.OnPlatform` 메서드는 플랫폼별 코드를 실행하거나 플랫폼별 값을 선택하는 데 사용됩니다. 내부적으로는 [`Device.OS`](xref:Xamarin.Forms.Device.OS) 정적 읽기 전용 속성을 사용하며 이 속성은 [`TargetPlatform`](xref:Xamarin.Forms.TargetPlatform) 열거형 멤버를 반환합니다.

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- UWP 디바이스에 대한 [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows).

`Device.OnPlatform` 메서드, `Device.OS` 속성 및 `TargetPlatform` 열거형은 이제 모두 사용되지 않습니다. 대신 [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) 속성을 사용하며 `string` 반환 값을 다음 정적 필드와 비교합니다.

- [`iOS`](xref:Xamarin.Forms.Device.iOS), 문자열 "iOS"
- [`Android`](xref:Xamarin.Forms.Device.Android), 문자열 "Android"
- [`UWP`](xref:Xamarin.Forms.Device.UWP), 문자열 "UWP", 유니버설 Windows 플랫폼 참조

[`Device.Idiom`](xref:Xamarin.Forms.Device.Idiom) 정적 읽기 전용 속성이 연결되어 있습니다. 다음 멤버가 있는 [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom)의 멤버를 반환합니다.

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported)는 사용되지 않습니다.

iOS 및 Android의 경우 `Tablet` 및 `Phone` 사이의 경계는 600 단위의 세로 너비입니다. Windows 플랫폼의 경우 `Desktop`은 Windows 10에서 실행되는 UWP 애플리케이션을 나타내고 `Phone`은 Windows 10 애플리케이션에서 실행되는 UWP 애플리케이션을 나타냅니다.

## <a name="solution-3a-set-margin-on-the-label"></a>솔루션 3a. 레이블의 여백 설정

[`Margin`](xref:Xamarin.Forms.View.Margin) 속성은 너무 늦게 도입되어 책에 포함되지 못했습니다. 하지만 `Thickness` 형식이며 `Label`에서 이 속성을 설정하여 뷰의 레이아웃 계산에 포함된 뷰 외부 영역을 정의할 수 있습니다.

`Padding` 속성은 [`Layout`](xref:Xamarin.Forms.Layout) 및 [`Page`](xref:Xamarin.Forms.Page) 파생 항목에서만 사용할 수 있습니다. `Margin` 속성은 모든 [`View`](xref:Xamarin.Forms.View) 파생 항목에서만 사용할 수 있습니다.

## <a name="solution-4-center-the-label-within-the-page"></a>솔루션 4. 페이지 내에서 레이블 가운데 맞춤

`Label`의 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 및 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 속성을 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 형식의 값으로 설정하여 `Page` 내에서 `Label`을 가운데에 배치(또는 8개의 다른 위치 중 하나에 배치)할 수 있습니다. `LayoutOptions` 구조체는 다음 두 가지 속성을 정의합니다.

- [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment) 형식(네 개의 멤버가 있는 열거형)의 [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) 속성: 방향에 따라 왼쪽 또는 위쪽을 의미하는 [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start), [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center), 방향에 따라 오른쪽 또는 아래쪽을 의미하는 [`End`](xref:Xamarin.Forms.LayoutAlignment.End) 및 [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill).

- `bool` 형식의 [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) 속성.

일반적으로 이러한 속성은 직접 사용되지 않습니다. 대신 이 두 속성의 조합은 `LayoutOptions` 형식의 8개 정적 읽기 전용 속성을 통해 제공됩니다.

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`HorizontalOptions` 및 `VerticalOptions`는 Xamarin.Forms 레이아웃에서 가장 중요한 속성으로, [**4장. 스택 스크롤**](chapter04.md)에서 더 자세히 설명합니다.

`Label`의 `HorizontalOptions` 및 `VerticalOptions` 속성을 모두 `LayoutOptions.Center`로 설정한 결과는 다음과 같습니다.

[![인사말 프로그램의 삼중 스크린샷](images/ch02fg05-small.png "레이블을 가로 및 세로로 가운데 맞춤")](images/ch02fg05-large.png#lightbox "레이블을 가로 및 세로로 가운데 맞춤")

## <a name="solution-5-center-the-text-within-the-label"></a>솔루션 5. 레이블 내에서 텍스트 가운데 맞춤

`Label`의 [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) 및 [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) 속성을 [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) 열거형의 멤버로 설정하여 텍스트를 가운데에 배치(또는 페이지의 8개 다른 위치에 배치)할 수도 있습니다.

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start)(방향에 따라 왼쪽 또는 위쪽)
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End)(방향에 따라 오른쪽 또는 아래쪽)

이 두 속성은 `Label`에 의해서만 정의되지만 `HorizontalAlignment` 및 `VerticalAlignment` 속성은 `View`에 의해 정의되고 모든 `View` 파생 항목에서 상속됩니다. 시각적 결과는 유사하게 보일 수 있지만 다음 장에서 설명하는 대로 서로 매우 다릅니다.

## <a name="related-links"></a>관련 링크

- [2장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [2장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [2장 F# 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Xamarin.Forms 시작](~/get-started/index.yml)
