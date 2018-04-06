---
title: Xamarin.Forms 소개
description: Xamarin.Forms는 개발자가 Android, iOS, Windows 및 Windows Phone에서 공유할 수 있는 사용자 인터페이스를 쉽게 만들 수 있도록 하는 UI 도구 키트 추상화를 기본적으로 지원하는 크로스 플랫폼입니다. 사용자 인터페이스는 Xamarin.Forms 응용 프로그램이 각 플랫폼에 맞게 적절한 모양 및 느낌을 유지할 수 있도록 하는 대상 플랫폼의 네이티브 컨트롤을 사용하여 렌더링 됩니다. 이 아티클에서는 Xamarin.Forms에 대한 소개 및 그것을 사용해 응용 프로그램을 작성하기 시작하는 방법을 제공합니다.
ms.prod: xamarin
ms.assetid: f619595f-3ee7-439b-a1bc-d13e5106e6e9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 6428f1658245ec5ecf47e474bc5ffd5d49663bf2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="an-introduction-to-xamarinforms"></a>Xamarin.Forms 소개

_Xamarin.Forms는 개발자가 Android, iOS, Windows 및 Windows Phone에서 공유할 수 있는 사용자 인터페이스를 쉽게 만들 수 있도록 하는 UI 도구 키트 추상화를 기본적으로 지원하는 크로스 플랫폼입니다. 사용자 인터페이스는 Xamarin.Forms 응용 프로그램이 각 플랫폼에 맞게 적절한 모양 및 느낌을 유지할 수 있도록 하는 대상 플랫폼의 네이티브 컨트롤을 사용하여 렌더링 됩니다. 이 아티클에서는 Xamarin.Forms에 대한 소개 및 그것을 사용해 응용 프로그램을 작성하기 시작하는 방법을 제공합니다._

<a name="Overview" />

## <a name="overview"></a>개요

Xamarin.Forms는 개발자가 신속하게 플랫폼 간 사용자 인터페이스를 만들 수 있도록 하는 프레임워크입니다. iOS, Android, Windows 또는 Windows Phone에서 네이티브 컨트롤을 사용하여 렌더링 되는 사용자 인터페이스에 대한 고유한 추상화를 제공합니다. 즉, 응용 프로그램은 사용자 인터페이스 코드의 많은 부분을 공유할 수 있으면서도 여전히 대상 플랫폼의 네이티브 모양 및 느낌을 유지합니다.

Xamarin.Forms를 사용하면 시간이 지남에 따라 복잡한 응용 프로그램으로 진화할 수 있는 응용 프로그램의 프로토타입을 빠르게 만들 수 있습니다. Xamarin.Forms 응용 프로그램은 네이티브 응용 프로그램이기 때문에 다른 도구 키트가 보이는 브라우저 샌드박스, 제한된 API 또는 성능 저하와 같은 제한이 없습니다. Xamarin.Forms를 사용하여 작성된 응용 프로그램은 어떠한 API나 iOS에서 CoreMotion, PassKit 및 StoreKit, Android에서 NFC와 Google Play 서비스, Windows에서 Tiles와 같은(그러나 그것들에 국한되지 않음) 기본 플랫폼의 기능을 사용할 수 있습니다. 또한 Xamarin.Forms를 사용하여 만든 사용자 인터페이스 부분이 있는 응용 프로그램을 만드는 것이 가능한 한편 다른 부분은 네이티브 UI 도구 키트를 사용하여 만듭니다.

Xamarin.Forms 응용 프로그램은 기존의 플랫폼 간 응용 프로그램과 같은 방식으로 설계됩니다. 가장 일반적인 방법은 [이식 가능한 라이브러리](~/cross-platform/app-fundamentals/pcl.md) 또는 [공유 프로젝트](~/cross-platform/app-fundamentals/shared-projects.md)를 사용하여 공유 코드를 보관하고 공유 코드를 사용할 플랫폼 특유 응용 프로그램을 만듭니다.

Xamarin.Forms에서 사용자 인터페이스를 만드는 기술에는 두 가지가 있습니다. 첫 번째 기술은 전적으로 C# 소스 코드를 사용하여 UI를 만드는 것입니다. 두 번째 방법은 사용자 인터페이스를 설명하는 데 사용되는 선언적 태그 언어인 *Extensible Application Markup Language*(XAML)을 사용하는 것입니다. XAML에 대한 자세한 내용은 [XAML 기본 사항](~/xamarin-forms/xaml/xaml-basics/index.md)을 참조하세요.

이 아티클은 Xamarin.Forms 프레임워크의 기본 사항을 설명하고 다음 항목을 다룹니다.

-  [Xamarin.Forms 응용 프로그램 검사](#Examining_A_Xamarin.Forms_Application).
-  [Xamarin.Forms 페이지 및 컨트롤 사용법](#Views_and_Layouts).
-  [데이터 목록 디스플레이 사용 방법](#Lists_in_Xamarin.Forms).
-  [데이터 바인딩 설정 방법](#Data_Binding).
-  [페이지 간 이동 방법](#Navigation).
-  [다음 단계](#Next_Steps)

<a name="Examining_A_Xamarin_Forms_Application" />

### <a name="examining-a-xamarinforms-application"></a>Xamarin.Forms 응용 프로그램 검사

Mac용 Visual Studio 및 Visual Studio에서 기본 Xamarin.Forms 앱 템플릿은 사용자에게 텍스트를 표시하는 가능한 가장 간단한 Xamarin.Forms 솔루션을 만듭니다. 응용 프로그램을 실행하는 경우, 다음 스크린샷과 비슷하게 나타납니다.

[![](introduction-to-xamarin-forms-images/image05-sml.png "기본 Xamarin.Forms 응용 프로그램")](introduction-to-xamarin-forms-images/image05.png#lightbox "기본 Xamarin.Forms 응용 프로그램")

스크린샷의 각 화면은 Xamarin.Forms에 있는 *페이지*에 해당합니다. [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)은 Android에서 *활동*, iOS에서 *뷰-컨트롤러* 또는 UWP(유니버설 Windows 플랫폼)에서 *페이지*를 나타냅니다. 위 스크린샷의 샘플은 [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 개체를 인스턴스화하고 [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)을 표시하기 위해 그것을 사용합니다.

시작 코드의 재사용을 최대화하기 위해 Xamarin.Forms 응용 프로그램에는 표시될 첫 번째 [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)의 인스턴스화를 담당하는 `App`라는 단일 클래스가 있습니다. `App` 클래스의 예를 다음 코드에서 볼 수 있습니다.

```csharp
public class App : Application
{
  public App ()
  {
    MainPage = new ContentPage {
      Content =  new Label
      {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
      }
    };
  }
}
```

이 코드는 페이지에 가로 및 세로로 중앙에 놓일 단일 [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)를 표시할 새 [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 개체를 인스턴스화합니다.

<a name="Launching_the_Initial_Xamarin_Forms_Page_on_Each_Platform" />

### <a name="launching-the-initial-xamarinforms-page-on-each-platform"></a>각 플랫폼에서 초기 Xamarin.Forms 페이지를 시작하기

응용 프로그램 내에서 이 [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)을 사용하려면 각 플랫폼 응용 프로그램은 Xamarin.Forms 프레임워크를 초기화하고 그것이 시작될 때 [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)의 인스턴스를 제공해야 합니다. 이 초기화 단계는 플랫폼마다 다르며 다음 섹션에서 설명됩니다.

<a name="Launching_in_iOS" />

#### <a name="ios"></a>iOS

iOS에서 초기 Xamarin.Forms 페이지를 시작하기 위해 플랫폼 프로젝트는 다음 코드 예제에서 설명한 것처럼 `Xamarin.Forms.Platform.iOS.FormsApplicationDelegate` 클래스에서 상속하는 `AppDelegate` 클래스를 포함합니다.

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
      global::Xamarin.Forms.Forms.Init ();
      LoadApplication (new App ());
      return base.FinishedLaunching (app, options);
    }
}
```

`FinishedLoading` 재정의는 `Init` 메서드를 호출하여 Xamarin.Forms 프레임 워크를 초기화합니다. Xamarin.Forms의 iOS 특정 구현이 루트 뷰 컨트롤러가 `LoadApplication` 메서드에 대한 호출로 설정되기 전에 응용 프로그램에 로드됩니다.

<a name="Launching_in_Android" />

#### <a name="android"></a>Android

Android에서 초기 Xamarin.Forms 페이지를 시작하기 위해 플랫폼 프로젝트는 다음 코드 예제에서 설명한 것처럼 `FormsApplicationActivity` 클래스에서 상속한 활동을 사용하여 `MainLauncher` 속성을 가진 `Activity`을 만드는 코드를 포함합니다.

```csharp
namespace HelloXamarinFormsWorld.Android
{
    [Activity(Label = "HelloXamarinFormsWorld", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication (new App ());
        }
    }
}
```

`OnCreate` 재정의는 `Init` 메서드를 호출하여 Xamarin.Forms 프레임 워크를 초기화합니다. 이로 인해 Xamarin.Forms 응용 프로그램이 로드되기 전에 Xamarin.Forms의 Android 특정 구현이 응용 프로그램에 로드됩니다.

<a name="Launching_in_Windows_Phone" />

#### <a name="windows-phone-81-winrt"></a>Windows Phone 8.1(WinRT)

Windows 런타임 응용 프로그램에서 Xamarin.Forms 프레임워크를 초기화하는 `Init` 메서드가 `App` 클래스에서 호출됩니다.

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

이렇게 하면 Xamarin.Forms의 Windows Phone 특정 구현이 응용 프로그램에 로드됩니다. `MainPage` 클래스는 초기 Xamarin.Forms 페이지를 다음 코드 예제에서 설명한 것처럼 시작합니다.

```csharp
public partial class MainPage
{
    public MainPage()
    {
      this.InitializeComponent();
      this.NavigationCacheMode = NavigationCacheMode.Required;
      this.LoadApplication(new HelloXamarinFormsWorld.App());
    }
}
```

Xamarin.Forms 응용 프로그램은 `LoadApplication` 메서드를 사용해 로드합니다.

또한 Xamarin.Forms는 Windows 8.1을 지원합니다. 이 프로젝트 형식을 구성하는 방법에 대한 자세한 내용은 [Windows 프로젝트 설정](~/xamarin-forms/platform/windows/installation/index.md)을 참조하세요.

#### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

UWP(유니버설 Windows 플랫폼) 응용 프로그램에서 Xamarin.Forms 프레임워크를 초기화하는 `Init` 메서드가 `App` 클래스에서 호출됩니다.

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

이렇게 하면 Xamarin.Forms의 UWP 특정 구현이 응용 프로그램에 로드됩니다. `MainPage` 클래스는 초기 Xamarin.Forms 페이지를 다음 코드 예제에서 설명한 것처럼 시작합니다.

```csharp
public partial class MainPage
{
    public MainPage()
    {
      this.InitializeComponent();
      this.LoadApplication(new HelloXamarinFormsWorld.App());
    }
}
```

Xamarin.Forms 응용 프로그램은 `LoadApplication` 메서드를 사용해 로드합니다.

<a name="Views_and_Layouts" />

### <a name="views-and-layouts"></a>보기 및 레이아웃

Xamarin.Forms 응용 프로그램의 사용자 인터페이스를 만드는 데 사용되는 4개의 주된 제어 그룹이 있습니다.

1. **페이지** – Xamarin.Forms 페이지는 플랫폼 간 모바일 응용 프로그램 화면을 나타냅니다. 페이지에 대한 자세한 내용은 [Xamarin.Forms 페이지](~/xamarin-forms/user-interface/controls/pages.md)를 참조하세요.
1. **레이아웃** – Xamarin.Forms 레이아웃은 뷰를 논리 구조로 구성하는 데 사용된 컨테이너입니다. 레이아웃에 대한 자세한 내용은 [Xamarin.Forms 레이아웃](~/xamarin-forms/user-interface/controls/layouts.md)을 참조하세요.
1. **뷰** – Xamarin.Forms 뷰는 레이블, 단추 및 텍스트 입력 상자 등의 사용자 인터페이스에 표시되는 컨트롤입니다. 보기에 대한 자세한 내용은 [Xamarin.Forms 보기](~/xamarin-forms/user-interface/controls/views.md)를 참조하세요.
1. **셀** – Xamarin.Forms 셀은 목록에 있는 항목에 사용되는 특수한 요소이며, 목록의 각 항목이 어떻게 그려져야 하는지를 설명합니다. 셀에 대한 자세한 내용은 [Xamarin.Forms 셀](~/xamarin-forms/user-interface/controls/cells.md)을 참조하세요.

런타임 시 각 컨트롤은 렌더링될 것이기도 한 해당 네이티브에 매핑됩니다.

컨트롤은 레이아웃 내에 호스팅됩니다. 자주 사용되는 레이아웃을 구현하는 [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 클래스를 지금 검사합니다.

<a name="StackLayout" />

#### <a name="stacklayout"></a>StackLayout

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)은 화면 크기와 관계없이 화면에 컨트롤을 자동으로 정렬하여 플랫폼 간 응용 프로그램 개발을 단순화합니다. 각 자식 요소는 가로 또는 세로로 추가된 순서대로 차례로 위치 지정됩니다. 얼마나 큰 공간을 `StackLayout`이 사용할지는 [`HorizontalOptions`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 및 [`VerticalOptions`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) 속성을 어떻게 설정했는지에 달려 있지만, 기본적으로는 `StackLayout`은 전체 화면을 사용하려고 합니다.

다음 XAML 코드는 [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)을 사용하여 3개의 [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 컨트롤을 정렬하는 예를 보여줍니다.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample1" Padding="20">
  <StackLayout Spacing="10">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

해당하는 C# 코드가 다음 코드 예제에 표시됩니다.

```csharp
public class StackLayoutExample : ContentPage
{
    public StackLayoutExample()
    {
        Padding = new Thickness(20);
        var red = new Label
        {
            Text = "Stop", BackgroundColor = Color.Red, FontSize = 20
        };
        var yellow = new Label
        {
            Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20
        };
        var green = new Label
        {
            Text = "Go", BackgroundColor = Color.Green, FontSize = 20
        };

        Content = new StackLayout
        {
            Spacing = 10,
            Children = { red, yellow, green }
        };
    }
}
```

기본적으로 [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)은 다음 스크린샷과 같이 세로 방향이라고 가정합니다.

[![](introduction-to-xamarin-forms-images/image09-sml.png "세로 StackLayout")](introduction-to-xamarin-forms-images/image09.png#lightbox "세로 StackLayout")

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)의 방향은 다음 XAML 코드 예제에서 설명한 것처럼 가로 방향으로 바뀔 수 있습니다.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample2" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

해당하는 C# 코드가 다음 코드 예제에 표시됩니다.

```csharp
public class StackLayoutExample: ContentPage
{
    public StackLayoutExample()
    {
        // Code that creates labels removed for clarity
        Content = new StackLayout
        {
            Spacing = 10,
            VerticalOptions = LayoutOptions.End,
            Orientation = StackOrientation.Horizontal,
            HorizontalOptions = LayoutOptions.Start,
            Children = { red, yellow, green }
        };
    }
}
```

다음 스크린샷은 결과 레이아웃을 보여줍니다.

[![](introduction-to-xamarin-forms-images/image10-sml.png "가로 StackLayout")](introduction-to-xamarin-forms-images/image10.png#lightbox "가로 StackLayout")

컨트롤의 크기는 다음 XAML 코드 예제에서 설명한 것처럼 `HeightRequest`과 `WidthRequest` 속성을 통해 설정할 수 있습니다.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample3" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" WidthRequest="100" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" WidthRequest="100" />
    <Label Text="Go" BackgroundColor="Green" Font="20" WidthRequest="200" />
  </StackLayout>
</ContentPage>
```

해당하는 C# 코드가 다음 코드 예제에 표시됩니다.

```csharp
var red = new Label
{
    Text = "Stop", BackgroundColor = Color.Red, FontSize = 20, WidthRequest = 100
};
var yellow = new Label
{
    Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20, WidthRequest = 100
};
var green = new Label
{
    Text = "Go", BackgroundColor = Color.Green, FontSize = 20, WidthRequest = 200
};

Content = new StackLayout
{
    Spacing = 10,
    VerticalOptions = LayoutOptions.End,
    Orientation = StackOrientation.Horizontal,
    HorizontalOptions = LayoutOptions.Start,
    Children = { red, yellow, green }
};
```

다음 스크린샷은 결과 레이아웃을 보여줍니다.

[![](introduction-to-xamarin-forms-images/image11-sml.png "LayoutOptions을 사용한 가로 StackLayout")](introduction-to-xamarin-forms-images/image11.png#lightbox "LayoutOptions을 사용한 가로 StackLayout")

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) 클래스에 대한 자세한 내용은 [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)을 참조하세요.

<a name="Lists_in_Xamarin_Forms" />

## <a name="lists-in-xamarinforms"></a>Xamarin.Forms의 목록

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 컨트롤은 화면에 항목 컬렉션 표시를 담당합니다 - `ListView`의 각 항목은 단일 셀에 포함됩니다. 기본적으로 `ListView`은 기본 제공된 [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) 템플릿을 사용하여 한 줄의 텍스트를 렌더링합니다.

다음 코드 예제는 간단한 [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 예제를 보여줍니다.

```csharp
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = new string []
{
    "Buy pears", "Buy oranges", "Buy mangos", "Buy apples", "Buy bananas"
};
Content = new StackLayout
{
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = { listView }
};
```

다음 스크린샷은 결과 [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)을 보여줍니다.

 ![](introduction-to-xamarin-forms-images/image13.png "ListView")

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 컨트롤에 대한 자세한 내용은 [ListView](~/xamarin-forms/user-interface/listview/index.md)를 참조하세요.

<a name="Binding_to_a_Custom_Class" />

### <a name="binding-to-a-custom-class"></a>사용자 지정 클래스에 바인딩

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 컨트롤은 기본 [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) 템플릿을 사용하여 사용자 지정 개체를 표시할 수도 있습니다.

다음 코드 예제는 `TodoItem` 클래스를 보여줍니다.

```csharp
public class TodoItem
{
    public string Name { get; set; }
    public bool Done { get; set; }
}
```

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 컨트롤은 다음 코드 예제에서 설명한 것처럼 채워질 수 있습니다.

```csharp
listView.ItemsSource = new TodoItem [] {
    new TodoItem { Name = "Buy pears" },
    new TodoItem { Name = "Buy oranges", Done=true} ,
    new TodoItem { Name = "Buy mangos" },
    new TodoItem { Name = "Buy apples", Done=true },
    new TodoItem { Name = "Buy bananas", Done=true }
};
```

바인딩은 다음 코드 예제에서 설명한 것처럼 [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)가 어떤 `TodoItem` 속성을 표시할지 설정하도록 만들 수 있습니다.

```csharp
listView.ItemTemplate = new DataTemplate(typeof(TextCell));
listView.ItemTemplate.SetBinding(TextCell.TextProperty, "Name");
```

이렇게 하면 `TodoItem.Name` 속성에 경로를 지정하는 바인딩이 만들어지고 이전에 표시된 스크린샷과 같이 됩니다.

사용자 지정 클래스에 바인딩하는 것에 대한 자세한 내용은 [ListView 데이터 원본](~/xamarin-forms/user-interface/listview/data-and-databinding.md)을 참조하세요.

<a name="Selecting_an_Item_in_a_ListView" />

### <a name="selecting-an-item-in-a-listview"></a>ListView에서 항목 선택.

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)에서 셀을 터치하는 사용자에게 응답하려면 [`ItemSelected`](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) 이벤트를 다음 코드 예제에서 설명한 것처럼 처리해야 합니다.

```csharp
listView.ItemSelected += async (sender, e) => {
    await DisplayAlert("Tapped!", e.SelectedItem + " was tapped.", "OK");
};
```

[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 내에 포함된 경우, [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) 메서드를 기본 제공된 뒤로 탐색으로 새 페이지를 여는 데 사용할 수 있습니다. [`ItemSelected`](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) 이벤트는 다음 코드 예제에서 설명한 것처럼 [`e.SelectedItem`](https://developer.xamarin.com/api/property/Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem/) 속성을 통해 셀과 연결된 개체에 액세스하여 새 페이지에 바인딩하고 `PushAsync`을 사용하여 새 페이지를 표시합니다.

```csharp
listView.ItemSelected += async (sender, e) => {
    var todoItem = (TodoItem)e.SelectedItem;
    var todoPage = new TodoItemPage(todoItem); // so the new page shows correct data
    await Navigation.PushAsync(todoPage);
};
```

각 플랫폼은 기본 제공된 뒤로 탐색을 자체적으로 구현합니다. 자세한 내용은 [탐색](#Navigation)를 참조하세요.

[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 선택에 관한 자세한 내용은 [ListView 대화형 작업](~/xamarin-forms/user-interface/listview/interactivity.md)을 참조하세요.

<a name="Customizing_the_appearance_of_a_cell" />

### <a name="customizing-the-appearance-of-a-cell"></a>셀 모양 사용자 지정

셀 모양은 [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 클래스를 서브클래싱하고 이 클래스의 형식을 [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)의 [`ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) 속성에 설정함으로써 사용자 지정할 수 있습니다.

다음 스크린샷에 표시된 셀은 한 개의 [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)와 두 개의 [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 컨트롤로 구성됩니다.

 ![](introduction-to-xamarin-forms-images/image14.png "ListView 사용자 지정 셀 모양")

이 사용자 지정 레이아웃을 만들려면 [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 클래스를 다음 코드 예제에서 설명한 것처럼 서브클래싱해야 합니다.

```csharp
class EmployeeCell : ViewCell
{
    public EmployeeCell()
    {
        var image = new Image
        {
            HorizontalOptions = LayoutOptions.Start
        };
        image.SetBinding(Image.SourceProperty, new Binding("ImageUri"));
        image.WidthRequest = image.HeightRequest = 40;

        var nameLayout = CreateNameLayout();
        var viewLayout = new StackLayout()
        {
           Orientation = StackOrientation.Horizontal,
           Children = { image, nameLayout }
        };
        View = viewLayout;
    }

    static StackLayout CreateNameLayout()
    {
        var nameLabel = new Label
        {
            HorizontalOptions= LayoutOptions.FillAndExpand
        };
        nameLabel.SetBinding(Label.TextProperty, "DisplayName");

        var twitterLabel = new Label
        {
           HorizontalOptions = LayoutOptions.FillAndExpand,
           Font = Fonts.Twitter
        };
        twitterLabel.SetBinding(Label.TextProperty, "Twitter");

        var nameLayout = new StackLayout()
        {
           HorizontalOptions = LayoutOptions.StartAndExpand,
           Orientation = StackOrientation.Vertical,
           Children = { nameLabel, twitterLabel }
        };
        return nameLayout;
    }
}
```

코드는 다음 작업을 수행합니다.

-  [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 컨트롤을 추가하고 `Employee` 개체의 `ImageUri` 속성에 바인딩합니다. 데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩](#Data_Binding)을 참조하세요.
-  두 개의 [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 컨트롤을 보관하도록 세로 방향으로 [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)을 만듭니다. `Label` 컨트롤은 `Employee` 개체의 `DisplayName`와 `Twitter`속성에 바인딩됩니다.
-  기존의 [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)과 `StackLayout`을 호스팅할 [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)을 만듭니다 가로 방향을 사용하여 자식을 정렬합니다.

사용자 지정 셀이 만들어지면 다음 코드 예제에서 설명한 것처럼 [`DataTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)에서 래핑하여 [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 컨트롤에서 사용할 수 있습니다.

```csharp
List<Employee> myListOfEmployeeObjects = GetAListOfAllEmployees();
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = myListOfEmployeeObjects;
listView.ItemTemplate = new DataTemplate(typeof(EmployeeCell));
```

이 코드는 `Employee`의 `List`을 [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)에 제공합니다. 각 셀은 `EmployeeCell` 클래스를 사용하여 렌더링 됩니다. `ListView`은 `Employee` 개체를 `EmployeeCell`에 [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)으로 전달합니다.

셀 모양을 사용자 지정하는 방법에 대한 자세한 내용은 [셀 모양](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)을 참조하세요.

<a name="Using_XAML_to_Create_and_Customize_A_List" />

### <a name="using-xaml-to-create-and-customize-a-list"></a>XAML를 사용하여 목록을 만들고 사용자 지정하기

이전 섹션에서 [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)에 해당하는 XAML이 다음 코드 예제에서 설명됩니다.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:XamarinFormsXamlSample;assembly=XamarinFormsXamlSample"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="listView" IsVisible="false" ItemsSource="{x:Static local:App.Employees}" ItemSelected="EmployeeListOnItemSelected">
    <ListView.ItemTemplate>
      <DataTemplate>
        <ViewCell>
          <ViewCell.View>
            <StackLayout Orientation="Horizontal">
              <Image Source="{Binding ImageUri}" WidthRequest="40" HeightRequest="40" />
              <StackLayout Orientation="Vertical" HorizontalOptions="StartAndExpand">
                <Label Text="{Binding DisplayName}" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Twitter}" Font="{x:Static constants:Fonts.Twitter}"/>
              </StackLayout>
            </StackLayout>
          </ViewCell.View>
        </ViewCell>
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

이 XAML은 [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)을 포함하는 [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)을 정의합니다. `ListView`의 데이터 원본은 [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) 특성를 통해 설정됩니다. `ItemsSource`에서 각 행의 레이아웃은 [`ListView.ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) 요소 내에 정의됩니다.

<a name="Data_Binding" />

## <a name="data-binding"></a>데이터 바인딩

데이터 바인딩은 *소스* 및 *대상*이라는 두 개의 개체를 연결합니다. *소스* 개체는 데이터를 제공합니다. *대상* 개체는 원본 개체의 데이터를 사용(하고 흔히 표시)합니다. 예를 들어 한 [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)(*대상* 개체)는 일반적으로 *소스* 개체에 있는 해당 [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) 속성을 공용 `string` 속성에 바인딩합니다. 다음 다이어그램은 바인딩 관계를 보여 줍니다.

![](introduction-to-xamarin-forms-images/data-binding.png "데이터 바인딩")

데이터 바인딩의 주요 장점은 더 이상 뷰와 데이터 원본 간의 데이터 동기화에 대해 걱정할 필요가 없다는입니다. *소스* 개체의 변경 사항은 바인딩 프레임워크이 배후에서 *대상* 개체에 자동으로 푸시하며, 대상 개체의 변경 내용은 필요한 경우 *소스* 개체를 다시 푸시될 수 있습니다.

데이터 바인딩 설정은 두 단계 프로세스입니다.

- *대상* 개체의 [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 속성은 *소스*에 설정되어야 합니다.
- 바인딩이 *소스*와 *대상* 간에 설정되어야 합니다. XAML의 경우 [`Binding`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) 태그 확장을 사용하여 이렇게 합니다. C#의 경우 [`SetBinding`](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) 메서드로 이렇게 합니다.

데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)을 참조하세요.

### <a name="xaml"></a>XAML

다음 코드는 XAML에서 데이터 바인딩을 수행하는 예를 보여줍니다.

```xaml
<Entry Text="{Binding FirstName}" ... />
```

*소스* 개체의 [`Entry.Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) 속성과 `FirstName` 속성 간의 바인딩이 설정됩니다. `Entry` 컨트롤에서 변경된 내용은 자동으로 `employeeToDisplay` 개체로 전파됩니다. 마찬가지로, `employeeToDisplay.FirstName` 속성을 변경하는 경우, Xamarin.Forms 바인딩 엔진은 `Entry` 컨트롤의 내용도 업데이트합니다. 이것을 *양방향 바인딩*이라고 합니다. 양방향 바인딩이 작동하기 위해서는 모델 클래스가 `INotifyPropertyChanged` 인터페이스를 구현해야 합니다.

`EmployeeDetailPage` 클래스의 [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 속성은 XAML에서 설정할 수 있지만, `Employee` 개체의 인스턴스에 코드 숨김으로 설정됩니다.

```csharp
public EmployeeDetailPage(Employee employee)
{
    InitializeComponent();
    this.BindingContext = employee;
}
```

각 *대상* 개체의 [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 속성은 개별적으로 설정할 수 있으나, 이렇게 할 필요는 없습니다. `BindingContext`은 모든 자식에서 상속한 특별 속성입니다. 따라서, [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)의 `BindingContext`이 `Employee` 인스턴스에 설정될 경우, `ContentPage`의 모든 자식 항목은 동일한 `BindingContext`을 가지며 `Employee`개체의 공용 속성에 바인딩할 수 있습니다.

### <a name="c35"></a>C&#35;

다음 코드는 C#에서 데이터 바인딩을 수행하는 예를 보여줍니다.

```csharp
public EmployeeDetailPage(Employee employeeToDisplay)
{
    this.BindingContext = employeeToDisplay;
    var firstName = new Entry()
    {
        HorizontalOptions = LayoutOptions.FillAndExpand
    };
    firstName.SetBinding(Entry.TextProperty, "FirstName");
    ...
}
```

[`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 생성자는 `Employee` 개체의 인스턴스로 전달되고, [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)을 바인딩할 개체에 설정합니다. [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 컨트롤이 인스턴스화되고, *소스* 개체의 [`Entry.Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) 속성과 `FirstName` 속성 간의 바인딩이 설정됩니다. `Entry` 컨트롤에서 변경된 내용은 자동으로 `employeeToDisplay` 개체로 전파됩니다. 마찬가지로, `employeeToDisplay.FirstName` 속성을 변경하는 경우, Xamarin.Forms 바인딩 엔진은 `Entry` 컨트롤의 내용도 업데이트합니다. 이것을 *양방향 바인딩*이라고 합니다. 양방향 바인딩이 작동하기 위해서는 모델 클래스가 `INotifyPropertyChanged` 인터페이스를 구현해야 합니다.

`SetBinding` 메서드는 두 매개 변수를 사용합니다. 첫 번째 매개 변수는 바인딩 유형에 관한 정보를 지정합니다. 두 번째 매개 변수는 바인딩할 항목 또는 바인딩하는 방법에 대한 정보를 제공하는 데 사용됩니다. 두 번째 매개 변수는 대부분의 경우 [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)의 속성 이름이 담긴 문자열입니다. 다음 구문을 사용하여 이를 `BindingContext`에 직접 바인딩합니다.

```csharp
someLabel.SetBinding(Label.TextProperty, new Binding("."));
```

점 구문은 Xamarin.Forms에게 [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)을 `BindingContext`에 있는 속성이라기 보다는 데이터 원본으로 사용하도록 지시합니다. `BindingContext`이 `string` 또는 `int`과 같은 단순 형식일 때 유용합니다.

<a name="INotifyPropertyChanged" />

### <a name="property-change-notification"></a>속성 변경 알림

기본적으로는 바인딩이 만들어질 때 *대상* 개체는 *소스* 개체의 값만 받습니다. UI와 데이터 원본의 동기화를 유지하려면 *소스* 개체가 변경 되었을 때 *대상* 개체에 알릴 방법이 있어야 합니다. 이 메커니즘은 `INotifyPropertyChanged` 인터페이스에서 제공합니다. 이 인터페이스를 구현하면 기본 속성 값이 변경될 때 데이터 바인딩된 컨트롤에 알립니다.

`INotifyPropertyChanged`을 구현하는 개체는 다음 코드 예제에서 설명한 것처럼 해당 속성 중 하나가 새 값으로 업데이트 될 때 `PropertyChanged` 이벤트를 발생시켜야 합니다.

```csharp
public class MyObject : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    string _firstName;
    public string FirstName
    {
        get { return _firstName; }
        set
        {
            if (value.Equals(_firstName, StringComparison.Ordinal))
            {
                // Nothing to do - the value hasn't changed;
                return;
            }
            _firstName = value;
            OnPropertyChanged();

        }
    }

    void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        var handler = PropertyChanged;
        if (handler != null)
        {
            handler(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

`MyObject.FirstName` 속성이 변경될 경우 `OnPropertyChanged` 메서드가 호출되어 `PropertyChanged` 이벤트를 발생시켜야 합니다. 불필요한 이벤트 발생을 방지하기 위해 속성 값이 바뀌지 않는 경우 `PropertyChanged` 이벤트는 발생되지 않습니다.

`OnPropertyChanged` 메서드에서 `propertyName` 매개 변수는 `CallerMemberName` 특성으로 표시됩니다. 이렇게 하면 `OnPropertyChanged` 메서드가 `null` 값으로 호출되는 경우 `CallerMemberName` 특성이 `OnPropertyChanged`를 호출하는 메서드의 이름을 제공합니다.

<a name="Navigation" />

## <a name="navigation"></a>탐색

Xamarin.Forms는 사용된 [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 형식에 따라 여러 다른 페이지 탐색 환경을 제공합니다. [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 인스턴스의 경우 두 가지 탐색 환경이 있습니다.

- [계층적 탐색](#Hierarchical_Navigation)
- [모달 탐색](#Modal_Navigation)

[`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) 및 [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) 클래스는 대체 탐색 환경을 제공합니다. 자세한 내용은 [탐색](~/xamarin-forms/app-fundamentals/navigation/index.md)를 참조하세요.

<a name="Hierarchical_Navigation" />

### <a name="hierarchical-navigation"></a>계층적 탐색

[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 클래스는 사용자가 필요에 따라 페이지를 앞으로 뒤로 탐색할 수 있는 계층적 탐색 환경을 제공합니다. 이 모델은 탐색을 [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 개체의 LIFO(후입선출) 스택으로 구현합니다.

계층적 탐색에는 [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 클래스가 [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 개체의 스택을 탐색하는 데 사용됩니다. 한 페이지에서 다른 페이지로 이동하려면 응용 프로그램은 새 페이지를 탐색 스택으로 푸시하여 활성 페이지가 되게 합니다. 이전 페이지로 돌아가기 위해 응용 프로그램은 탐색 스택에서 현재 페이지를 팝하고 맨 위에 있는 새 페이지는 활성 페이지가 됩니다.

탐색 스택에 추가된 첫 번째 페이지는 응용 프로그램의 *루트* 페이지라고 하며, 다음 코드 예제는 이것을 수행하는 방법을 보여줍니다.

```csharp
public App ()
{
    MainPage = new NavigationPage(new EmployeeListPage());
}
```

`LoginPage`로 이동하려면 다음 코드 예제에서 설명한 것처럼 현재 페이지의 [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 속성에서 [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) 메서드를 호출해야 합니다.

```csharp
await Navigation.PushAsync(new LoginPage());
```

새 `LoginPage` 개체가 탐색 스택으로 푸시되어 활성 페이지가 됩니다.

활성 페이지는 장치의 *다시* 단추를 눌러 탐색 스택에서 팝할 수 있습니다. 이때 단추는 장치의 물리적 단추이든 화면상 단추이든 상관없습니다. 프로그래밍 방식으로 이전 페이지로 돌아가려면 `LoginPage` 개체가 다음 코드 예제에서 설명한 것처럼 [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) 메서드를 호출해야 합니다.

```csharp
await Navigation.PopAsync();
```

계층적 탐색에 대한 자세한 내용은 [계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)을 참조하세요.

<a name="Modal_Navigation" />

### <a name="modal-navigation"></a>모달 탐색

Xamarin.Forms는 모달 페이지를 지원합니다. 모달 페이지는 사용자가 작업이 완료되거나 취소될 때까지 다른 부분으로 이동할 수 없는 자체 포함된 작업을 완료하도록 권장합니다.

모달 페이지는 Xamarin.Forms에서 지원하는 [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 형식이라면 어떤 것이든 될 수 있습니다. 모달 페이지를 표시하려면 응용 프로그램은 새 페이지를 탐색 스택으로 푸시하여 활성 페이지가 되게 합니다. 이전 페이지로 돌아가기 위해 응용 프로그램은 탐색 스택에서 현재 페이지를 팝하고 맨 위에 있는 새 페이지는 활성 페이지가 됩니다.

모달 탐색 메서드는 모든 [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 파생 형식의 [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 속성에 의해 노출됩니다. [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 속성은 또한 탐색 스택의 모달 페이지를 얻을 수 있는 [`ModalStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) 속성을 노출합니다. 그러나 모달 스택 조작을 수행하거나 모달 탐색에서 루트 페이지에 팝하는 개념은 없습니다. 이러한 작업이 기본 플랫폼에서 보편적으로 지원되지 않기 때문입니다.

> [!NOTE]
> [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 인스턴스는 모달 페이지 탐색을 수행하는 데 필요하지 않습니다.

형태상으로 `LoginPage`로 이동하려면 다음 코드 예제에서 설명한 것처럼 현재 페이지의 [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 속성에서 [`PushModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) 메서드를 호출해야 합니다.

```csharp
await Navigation.PushModalAsync(new LoginPage());
```

`LoginPage` 인스턴스가 탐색 스택으로 푸시되어 활성 페이지가 됩니다.

활성 페이지는 장치의 *다시* 단추를 눌러 탐색 스택에서 팝할 수 있습니다. 이때 단추는 장치의 물리적 단추이든 화면상 단추이든 상관없습니다. 프로그래밍 방식으로 원래 페이지로 돌아가려면 `LoginPage` 개체가 다음 코드 예제에서 설명한 것처럼 [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) 메서드를 호출해야 합니다.

```csharp
await Navigation.PopModalAsync();
```

`LoginPage` 인스턴스가 탐색 스택에서 제거되어 맨 위에 있는 새 페이지가 활성 페이지가 됩니다.

모달 탐색에 대한 자세한 내용은 [모달 탐색](~/xamarin-forms/app-fundamentals/navigation/modal.md)을 참조하세요.

<a name="Next_Steps" />

## <a name="next-steps"></a>다음 단계

이 소개 아티클을 통해 Xamarin.Forms 응용 프로그램 작성을 시작할 수 있을 것입니다. 제안된 다음 단계로 다음 기능에 대한 읽기가 있습니다.

- 컨트롤 템플릿은 런타임에 응용 프로그램 페이지를 주제로 다루고 다시 주제로 다루는 기능을 제공합니다. 자세한 내용은 [컨트롤 템플릿](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)을 참조하세요.
- 데이터 템플릿은 지원되는 컨트롤에 있는 데이터 프레젠테이션을 정의하는 기능을 제공합니다. 자세한 내용은 [데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.
- 공유 코드는 [`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) 클래스를 통해 네이티브 기능에 액세스할 수 있습니다. 자세한 내용은 [DependencyService를 사용한 네이티브 기능 액세스](~/xamarin-forms/app-fundamentals/dependency-service/index.md)를 참조하세요.
- Xamarin.Forms에는 메시지를 보내거나 받을 수 있는 간단한 메시징 서비스가 들어 있어 클래스 간의 결합을 줄일 수 있습니다. 자세한 내용은 [MessagingCenter를 사용한 게시 및 구독](~/xamarin-forms/app-fundamentals/messaging-center.md)을 참조하세요.
- 각 페이지, 레이아웃 및 컨트롤은 차례로 네이티브 컨트롤을 만들고, 화면에 정렬하고, 공유 코드에 지정한 동작을 추가하는 `Renderer` 클래스를 사용하여 각 플랫폼에서 다르게 렌더링됩니다. 개발자는 컨트롤의 모양 및/또는 동작을 사용자 지정하기 위해 자신 만의 사용자 지정 `Renderer` 클래스를 구현할 수 있습니다. 자세한 내용은 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)를 참조하세요.
- 각 플랫폼의 네이티브 컨트롤을 사용자 지정할 수 있는 효과가 있습니다. 효과가 플랫폼별 프로젝트에 [`PlatformEffect`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/) 컨트롤을 서브클래싱하여 만들어지고, 적절한 Xamarin.Forms 컨트롤에 연결하여 사용됩니다. 자세한 내용은 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)를 참조하세요.

또는 Charles Petzold의 책인 Creating Mobile Apps with Xamarin.Forms는 Xamarin.Forms에 대해 더 배울 수 있는 좋은 자료입니다. 자세한 내용은 [Creating Mobile Apps with Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)를 참조하세요.

## <a name="summary"></a>요약

이 아티클에서는 Xamarin.Forms에 대한 소개 및 그것을 사용해 응용 프로그램을 작성하기 시작하는 방법을 제공했습니다. Xamarin.Forms는 개발자가 Android, iOS, Windows 및 Windows Phone에서 공유할 수 있는 사용자 인터페이스를 쉽게 만들 수 있도록 하는 UI 도구 키트 추상화를 기본적으로 지원하는 크로스 플랫폼입니다. 사용자 인터페이스는 Xamarin.Forms 응용 프로그램이 각 플랫폼에 맞게 적절한 모양 및 느낌을 유지할 수 있도록 하는 대상 플랫폼의 네이티브 컨트롤을 사용하여 렌더링 됩니다.


## <a name="related-links"></a>관련 링크

- [XAML 기본 사항](~/xamarin-forms/xaml/xaml-basics/index.md)
- [컨트롤 참조](~/xamarin-forms/user-interface/controls/index.md)
- [사용자 인터페이스](~/xamarin-forms/user-interface/index.md)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [시작 샘플](https://developer.xamarin.com/samples/xamarin-forms/GettingStarted/)
- [Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
- [무료 사용자 진행 방식 학습(비디오)](https://university.xamarin.com/self-guided)
- [Hello, Xamarin.Forms iOS 통합 문서](https://developer.xamarin.com/workbooks/xamarin-forms/getting-started/GettingStartedWithXamarinForms-ios.workbook)
