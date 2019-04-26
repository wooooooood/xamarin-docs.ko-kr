---
title: 요약 2 장입니다. 앱 분석
description: Xamarin.Forms를 사용 하 여 모바일 앱을 만듭니다. 요약 2 장입니다. 앱 분석
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: 95defd11a9e568d1089cb2f262cb323045b6c247
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334395"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>요약 2 장입니다. 앱 분석

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)

> [!NOTE]
> 이 페이지에 대 한 참고 사항 Xamarin.Forms 책의 내용을에서 달라졌는지를 위치 하는 영역을 나타냅니다.

Xamarin.Forms 응용 프로그램에서 화면에서 공간을 차지 하는 개체 라고 *시각적 요소*하 여 캡슐화 된, 합니다 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 클래스입니다. 시각적 요소는 이러한 클래스에 해당 하는 세 가지 범주로 분할 될 수 있습니다.

- [페이지](xref:Xamarin.Forms.Page)
- [레이아웃](xref:Xamarin.Forms.Layout)
- [보기](xref:Xamarin.Forms.View)

`Page` 파생 클래스는 전체 화면 또는 거의 전체 화면을 차지 합니다. 대개는 페이지의 자식은 `Layout` 자식 시각적 요소를 구성 하는 파생 합니다. 자식의 합니다 `Layout` 다른 될 수 있습니다 `Layout` 클래스 또는 `View` 파생형 (라고도 *요소*), 텍스트, 비트맵, 슬라이더, 단추, 목록 상자 등과 같은 친숙 한 개체입니다.

이 장의에 집중 하 여 응용 프로그램을 만드는 방법을 보여 줍니다 합니다 [ `Label` ](xref:Xamarin.Forms.Label)는 `View` 텍스트를 표시 하는 파생 합니다.

## <a name="say-hello"></a>살펴보기

설치 하는 Xamarin 플랫폼을 사용 하 여 만들 수 있습니다 새 Xamarin.Forms 솔루션을 Visual Studio 또는 Visual Studio for mac 합니다 [ **Hello** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) 솔루션은 공용 코드에 대 한 이식 가능한 클래스 라이브러리를 사용 합니다.

> [!NOTE]
> Portable Class Library는 .NET Standard 라이브러리로 변경되었습니다. 이 책에서 모든 샘플 코드는 .NET 표준 라이브러리를 사용하도록 변경되었습니다.

이 샘플에는 수정 하지 않고도 Visual Studio에서 만들어진 Xamarin.Forms 솔루션을 보여 줍니다. 솔루션은 6 개 프로젝트:

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), 다른 프로젝트에서 공유 하는 이식 가능한 클래스 라이브러리 (PCL)
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), Android 용 응용 프로그램 프로젝트
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), iOS 용 응용 프로그램 프로젝트
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), 유니버설 Windows 플랫폼 (Windows 10 및 Windows 10 Mobile)에 대 한 응용 프로그램 프로젝트
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), Windows 8.1 대 한 응용 프로그램 프로젝트
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), Windows Phone 8.1에 대 한 응용 프로그램 프로젝트

> [!NOTE]
> Xamarin.Forms는 더 이상 Windows 8.1, Windows Phone 8.1 또는 Windows 10 Mobile을 지원하지 않지만 Windows 10 데스크톱에서 실행은 지원합니다.

수의 응용 프로그램 프로젝트 시작 프로젝트 및 다음 빌드를 장치 또는 시뮬레이터에서 프로그램을 실행 합니다.

Xamarin.Forms 프로그램의 대부분에서는 응용 프로그램 프로젝트를 수정 하지 않습니다. 이러한 프로그램을 시작 하려면 방금 작은 스텁 종종 남아 있습니다. 대부분의 집중 라이브러리는 모든 응용 프로그램에 공통적으로 적용 됩니다.

## <a name="inside-the-files"></a>파일 내에서

표시 하 여 시각적 개체를 **Hello** 프로그램의 생성자에 정의 된 합니다 [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) 클래스입니다. `App` Xamarin.Forms 클래스에서 파생 [ `Application` ](xref:Xamarin.Forms.Application)합니다.

> [!NOTE]
> Xamarin.Forms에 대 한 Visual Studio 솔루션 템플릿 XAML 파일을 사용 하 여 페이지를 만듭니다. XAML 될 때까지이 가이드에서 다루지 [7 장](chapter07.md)합니다.

합니다 **참조** 섹션을 **Hello** PCL 프로젝트에 다음 Xamarin.Forms 어셈블리에 포함:

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

합니다 **참조** 섹션 5 응용 프로그램 프로젝트의 개별 플랫폼에 적용 되는 추가 어셈블리를 포함 합니다.

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

> [!NOTE]
> 합니다 **참조가** 섹션에서는 이러한 프로젝트의 어셈블리를 더 이상 나열 합니다. 대신 프로젝트 파일에는 **PackageReference** Xamarin.Forms NuGet 패키지를 참조 하는 태그입니다. 합니다 **참조** Visual Studio 목록의 섹션의 **Xamarin.Forms** Xamarin.Forms 어셈블리 대신 패키지 합니다.

정적 호출이 포함 되어 응용 프로그램 프로젝트의 각 `Forms.Init` 의 메서드는 `Xamarin.Forms` 네임 스페이스입니다. 이 Xamarin.Forms 라이브러리를 초기화합니다. 다른 버전의 `Forms.Init` 각 플랫폼에 대해 정의 됩니다. 다음 클래스에이 메서드의 호출을 찾을 수 있습니다.

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- : UWP [ `App` 클래스인 `OnLaunched` 메서드](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)

또한 각 플랫폼을 인스턴스화해야 합니다 `App` 공유 라이브러리의 위치를 클래스. 이에 대 한 호출에서 발생 `LoadApplication` 다음 클래스에:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)

그렇지 않으면 이러한 응용 프로그램 프로젝트는 일반 "아무 작업도 수행" 프로그램입니다.

## <a name="pcl-or-sap"></a>PCL 또는 SAP?

이식 가능한 클래스 라이브러리 (PCL) 또는 공유 자산 프로젝트 (SAP)에서 공통 코드를 사용 하 여 Xamarin.Forms 솔루션을 만들 수는 것입니다. SAP 솔루션을 만들려면 Visual Studio에서 공유 옵션을 선택 합니다. 합니다 [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) 솔루션은 수정 하지 않고도 SAP 템플릿을 보여 줍니다.

> [!NOTE]
> 이식 가능한 클래스 라이브러리는.NET Standard 라이브러리로 바뀌었습니다. 이 책에서 모든 샘플 코드는 .NET 표준 라이브러리를 사용하도록 변경되었습니다. 그렇지 않은 경우 PCL 및.NET Standard 라이브러리는 개념적으로 유사 합니다.

모든 일반적인 플랫폼 응용 프로그램 프로젝트에서 참조 하는 라이브러리 프로젝트에서 코드 라이브러리 접근 방식 번들입니다. SAP 방식의 경우 공통 코드는 효과적으로 모든 플랫폼 응용 프로그램 프로젝트에 존재 하 고 그 중 공유 됩니다.

대부분의 개발자 들은 Xamarin.Forms 라이브러리 접근 방식을 선호합니다. 이 책에이 나온 대부분의 솔루션이 라이브러리를 사용 합니다. SAP를 사용 하는 포함 된 **Sap** 프로젝트 이름에서 접미사.

SAP 접근 방식으로 공유 프로젝트에서 코드를 실행할 수 있습니다는 다양 한 플랫폼에 대해 서로 다른 코드를 사용 하 여 C# 전처리기 지시문 (`#if`, #`elif`, 및 `#endif`) 이러한 미리 정의 된 식별자:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`

공유 라이브러리의이 장 뒷부분에서 살펴보겠지만 런타임 시 실행 중인 플랫폼을 확인할 수 있습니다.

## <a name="labels-for-text"></a>텍스트에 대 한 레이블

[ **Greetings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) 솔루션에 추가 하는 방법을 보여 줍니다 C# 파일을 **Greetings** 프로젝트. 이 파일은 라는 클래스를 정의 `GreetingsPage` 에서 파생 되는 `ContentPage`합니다. 이 책에이 나온 대부분의 프로젝트는 단일 포함 `ContentPage` 접미사를 사용 하 여 프로젝트의 이름인 이름이 파생 `Page` 추가 합니다.

`GreetingsPage` 생성자를 인스턴스화하는 [ `Label` ](xref:Xamarin.Forms.Label) 뷰를 텍스트를 표시 하는 Xamarin.Forms 보기. [ `Text` ](xref:Xamarin.Forms.Label.Text) 으로 표시 되는 텍스트 속성을 `Label`입니다. 이 프로그램을 설정 합니다 `Label` 에 `Content` 의 속성 `ContentPage`합니다. 생성자는 `App` 클래스를 인스턴스화하고 `GreetingsPage` 로 설정 하 고 해당 `MainPage` 속성입니다.

텍스트는 페이지의 왼쪽 위 모퉁이에 표시 됩니다. Ios의 경우 페이지의 상태 표시줄을 겹치는 것을 의미 합니다. 이 문제에 대 한 솔루션 몇 가지가 있습니다.

### <a name="solution-1-include-padding-on-the-page"></a>솔루션 1입니다. 페이지의 안쪽 여백을 포함

설정 된 [ `Padding` ](xref:Xamarin.Forms.Page.Padding) 페이지의 속성입니다. `Padding` 유형의 [ `Thickness` ](xref:Xamarin.Forms.Thickness), 네 가지 속성을 사용 하 여 구조:

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding` 콘텐츠는 제외 하는 페이지 내의 영역을 정의 합니다. 따라서는 `Label` iOS 상태 표시줄을 덮어쓰지 않도록 합니다.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>솔루션 2입니다. IOS (SAP에만 해당)에 대 한 안쪽 여백을 포함

'안쪽 여백' 속성을 사용 하 여 SAP를 사용 하 여 iOS에만 설정 된 C# 전처리기 지시문입니다. 에 설명 되어이 [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) 솔루션입니다.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>3 솔루션입니다. IOS (PCL 또는 SAP)에 대 한 안쪽 여백을 포함

Xamarin.forms 책에 사용 되는 버전에는 `Padding` 를 사용 하는 PCL 또는 SAP에서 iOS에 대 한 속성을 선택할 수 있습니다 합니다 [ `Device.OnPlatform` ](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) 또는 [ `Device.OnPlatform<T>` ](xref:Xamarin.Forms.Device.OnPlatform*) 정적 메서드. 이러한 메서드는 이제 사용 되지 않습니다.

`Device.OnPlatform` 메서드 플랫폼별 코드를 실행 하거나 플랫폼 특정 값을 선택 하는 데 사용 합니다. 할 내부적으로 사용 합니다 [ `Device.OS` ](xref:Xamarin.Forms.Device.OS) 정적 읽기 전용 속성의 멤버를 반환 합니다 [ `TargetPlatform` ](xref:Xamarin.Forms.TargetPlatform) 열거형:

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) UWP 장치용입니다.

합니다 `Device.OnPlatform` 메서드를 `Device.OS` 속성인 및 `TargetPlatform` 열거형은 이제 사용 되지 않는 모든 합니다. 대신 사용 하 여는 [ `Device.RuntimePlatform` ](xref:Xamarin.Forms.Device.RuntimePlatform) 속성과 비교는 `string` 다음 정적 필드를 사용 하 여 값을 반환 합니다.

- [`iOS`](xref:Xamarin.Forms.Device.iOS)"iOS" 문자열
- [`Android`](xref:Xamarin.Forms.Device.Android)"Android" 문자열
- [`UWP`](xref:Xamarin.Forms.Device.UWP)"UWP", 유니버설 Windows 플랫폼에 참조 문자열

합니다 [ `Device.Idiom` ](xref:Xamarin.Forms.Device.Idiom) 정적 읽기 전용 속성 관련 됩니다. 멤버를 반환 하는이 [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom), 이러한 멤버에는:

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) 사용 되지 않습니다.

IOS 및 Android 간에 구분 `Tablet` 및 `Phone` 600 단위는 세로 너비입니다. Windows 플랫폼용 `Desktop` Windows 10에서 실행 되는 UWP 응용 프로그램을 나타내는 및 `Phone` Windows 10 응용 프로그램에서 실행 되는 UWP 응용 프로그램을 나타냅니다.

## <a name="solution-3a-set-margin-on-the-label"></a>솔루션 3a 합니다. 레이블에 여백 설정

합니다 [ `Margin` ](xref:Xamarin.Forms.View.Margin) 속성이 너무 늦게 책에 포함 될 도입 하지만 형식 이기도 `Thickness` 에서 설정할 수 있습니다는 `Label` 계산에 포함 된 보기의 외부 영역을 정의 하는 뷰의 레이아웃입니다.

`Padding` 속성은 에서만 사용할 수 [ `Layout` ](xref:Xamarin.Forms.Layout) 하 고 [ `Page` ](xref:Xamarin.Forms.Page) 파생형입니다. 합니다 `Margin` 속성은 모두에서 사용할 수 있습니다 [ `View` ](xref:Xamarin.Forms.View) 파생 합니다.

## <a name="solution-4-center-the-label-within-the-page"></a>4 솔루션입니다. 페이지 내에서 레이블을 가운데합니다

Center 수를 `Label` 내에서 `Page` (또는 다른 위치를 8 중 하나에 배치) 설정 하 여를 [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) 및 [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) 속성을는 `Label` 형식의 값으로 [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions)합니다. `LayoutOptions` 구조 두 속성을 정의 합니다.

- [ `Alignment` ](xref:Xamarin.Forms.LayoutOptions.Alignment) 형식의 속성 [ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment), 4 개 멤버가 포함 된 열거형: [ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start), 왼쪽 또는 위쪽에 따라 의미는 방향 [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center)합니다 [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End), 즉, 오른쪽 또는 아래쪽 방향에 따라 및 [ `Fill` ](xref:Xamarin.Forms.LayoutAlignment.Fill)합니다.

- [ `Expands` ](xref:Xamarin.Forms.LayoutOptions.Expands) 형식의 속성 `bool`합니다.

일반적으로 이러한 속성을 직접 사용 되지 않습니다. 이러한 두 속성의 조합도 형식의 8 개의 정적 읽기 전용 속성에서 제공 하는 대신 `LayoutOptions`:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`HorizontalOptions` 및 `VerticalOptions` Xamarin.Forms 레이아웃에서 가장 중요 한 속성은 및에서 자세히 설명 [ **4 장입니다. 스택 스크롤**](chapter04.md)합니다.

사용 하 여 결과 `HorizontalOptions` 하 고 `VerticalOptions` 의 속성 `Label` 둘 다로 설정 `LayoutOptions.Center`:

[![Greetings 프로그램의 3 배가 스크린 샷](images/ch02fg05-small.png "가운데에 레이블을 가로 및 세로로")](images/ch02fg05-large.png#lightbox "가로 및 세로로 가운데 레이블")

## <a name="solution-5-center-the-text-within-the-label"></a>5 솔루션입니다. 레이블 내에서 텍스트를 가운데

가운데 텍스트 (하거나 수도 페이지의 다른 8 개 위치에 배치) 설정 하 여는 [ `HorizontalTextAlignment` ](xref:Xamarin.Forms.Label.HorizontalTextAlignment) 하 고 [ `VerticalTextAlignment` ](xref:Xamarin.Forms.Label.VerticalTextAlignment) 의 속성 `Label` 합니다 의멤버에[ `TextAlignment` ](xref:Xamarin.Forms.TextAlignment) 열거형:

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start)를 의미 왼쪽 또는 위쪽 (방향)에 따라 다름
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End)즉 오른쪽 또는 아래쪽 (방향)에 따라 다름

이러한 두 속성 에서만 정의 됩니다 `Label`반면 합니다 `HorizontalAlignment` 및 `VerticalAlignment` 속성으로 정의 된 `View` 모든 상속 `View` 파생형입니다. Visual 결과 수 비슷해 보이지만 다음 장에서 보여 주듯이 매우 다릅니다.

## <a name="related-links"></a>관련 링크

- [2 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [2 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [2 장 F# 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Xamarin.Forms를 사용 하 여 시작](~/get-started/index.yml)
