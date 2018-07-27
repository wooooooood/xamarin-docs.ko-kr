---
title: Xamarin.Forms 심층 분석
description: 이 문서에서는 Xamarin.Forms를 사용하여 응용 프로그램 개발의 기본적인 사항을 검사합니다. Xamarin.Forms 응용 프로그램 분석, 아키텍처 및 응용 프로그램 기본 사항, 사용자 인터페이스에 대해 다루었습니다.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: d97aa580-1eb9-48b3-b15b-0d7421ea7ae
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: 7eff7f4413b533caadcf2aa8b5eed8c4ab65449d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242227"
---
# <a name="xamarinforms-deep-dive"></a>Xamarin.Forms 심층 분석

[Xamarin.Forms 빠른 시작](~/xamarin-forms/get-started/hello-xamarin-forms/quickstart.md)에서는 Phoneword 응용 프로그램을 빌드했습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램의 핵심 작동 원리를 이해하기 위해 무엇이 빌드되었는지 검토합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="introduction-to-visual-studio"></a>Visual Studio 소개

Visual Studio는 Microsoft의 강력한 IDE입니다. 완전히 통합된 비주얼 디자이너, 리팩터리 도구가 함께 포함된 텍스트 편집기, 어셈블리 브라우저, 소스 코드 통합 등을 제공합니다. 이 문서에서는 Visual Studio의 일부 기본 기능을 Xamarin 플러그 인과 함께 사용하는 방법을 집중적으로 다룹니다.

Visual Studio는 코드를 *솔루션* 및 *프로젝트*로 구성합니다. 솔루션은 하나 이상의 프로젝트를 포함할 수 있는 컨테이너입니다. 프로젝트는 응용 프로그램, 지원 라이브러리, 테스트 응용 프로그램 등이 될 수 있습니다. Phoneword 응용 프로그램은 다음 스크린샷처럼 4개의 프로젝트를 포함하는 솔루션 하나로 구성됩니다.

![](deepdive-images/vs/solution.png "Visual Studio 솔루션 탐색기")

프로젝트는 다음과 같습니다.

- Phoneword - 이 프로젝트는 모든 공유 코드와 공유 UI를 보관하는 .NET 표준 라이브러리 프로젝트입니다.
- Phoneword.Android - 이 프로젝트는 Android 관련 코드를 보관하며 Android 응용 프로그램의 진입점입니다.
- Phoneword.iOS - 이 프로젝트는 iOS 관련 코드를 보관하며 iOS 응용 프로그램의 진입점입니다.
- Phoneword.UWP - 이 프로젝트는 UWP(유니버설 Windows 플랫폼) 관련 코드를 보관하며 UWP(유니버설 Windows 플랫폼) 응용 프로그램의 진입점입니다.

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms 응용 프로그램 분석

다음 스크린샷은 Visual Studio의 Phoneword .NET Standard 라이브러리 프로젝트 콘텐츠를 보여줍니다.

![](deepdive-images/vs/net-standard-project.png "Phoneword .NET Standard 프로젝트 콘텐츠")

프로젝트에는 **NuGet** 및 **SDK** 노드를 포함하는 **종속성** 노드가 있습니다. **NuGet** 노드는 프로젝트에 추가된 Xamarin.Forms NuGet 패키지를 포함하고, **SDK** 노드는 .NET Standard를 정의하는 전체 NuGet 패키지 집합을 참조하는 `NETStandard.Library` 메타패키지를 포함합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="introduction-to-visual-studio-for-mac"></a>Mac용 Visual Studio 소개

Mac용 Visual Studio는 Visual Studio와 유사한 무료 오픈 소스 IDE입니다. 완전히 통합된 비주얼 디자이너, 리팩터리 도구가 함께 포함된 텍스트 편집기, 어셈블리 브라우저, 소스 코드 통합 등을 제공합니다. Mac용 Visual Studio에 대한 자세한 내용은 [Mac용 Visual Studio 소개](/visualstudio/mac/)를 참조하세요.

Mac용 Visual Studio는 코드를 *솔루션* 및 *프로젝트*로 구성하는 Visual Studio 연습을 따릅니다. 솔루션은 하나 이상의 프로젝트를 포함할 수 있는 컨테이너입니다. 프로젝트는 응용 프로그램, 지원 라이브러리, 테스트 응용 프로그램 등이 될 수 있습니다. Phoneword 응용 프로그램은 다음 스크린샷처럼 3개의 프로젝트를 포함하는 솔루션 하나로 구성됩니다.

![](deepdive-images/xs/solution.png "Mac용 Visual Studio 솔루션 창")

프로젝트는 다음과 같습니다.

- Phoneword - 이 프로젝트는 모든 공유 코드와 공유 UI를 보관하는 .NET 표준 라이브러리 프로젝트입니다.
- Phoneword.Droid - 이 프로젝트는 Android 관련 코드를 보관하며 Android 응용 프로그램의 진입점입니다.
- Phoneword.iOS - 이 프로젝트는 iOS 관련 코드를 보관하며 iOS 응용 프로그램의 진입점입니다.

## <a name="anatomy-of-a-xamarinforms-application"></a>Xamarin.Forms 응용 프로그램 분석

다음 스크린샷은 Mac용 Visual Studio의 Phoneword .NET Standard 라이브러리 프로젝트 콘텐츠를 보여줍니다.

![](deepdive-images/xs/library-project.png "Phoneword .NET Standard 라이브러리 프로젝트 콘텐츠")

프로젝트에는 **NuGet** 및 **SDK** 노드를 포함하는 **종속성** 노드가 있습니다. **NuGet** 노드는 프로젝트에 추가된 Xamarin.Forms NuGet 패키지를 포함하고, **SDK** 노드는 .NET Standard를 정의하는 전체 NuGet 패키지 집합을 참조하는 `NETStandard.Library` 메타패키지를 포함합니다.

-----

프로젝트는 여러 파일로도 구성됩니다.

- **App.xaml** - `App` 클래스에 대한 XAML 태그로, 응용 프로그램의 리소스 사전을 정의합니다.
- **App.xaml.cs** – `App` 클래스의 코드 숨김으로, 각 플랫폼에서 응용 프로그램이 표시할 첫 번째 페이지를 인스턴스화하고 응용 프로그램 수명 주기 이벤트를 처리하는 역할을 담당합니다.
- **IDialer.cs** – 구현 클래스를 통해 `Dial` 메서드를 제공해야 한다고 지정하는 `IDialer` 인터페이스입니다.
- **MainPage.xaml** - `MainPage` 클래스에 대한 XAML 태그로, 응용 프로그램이 시작될 때 표시되는 페이지의 UI를 정의합니다.
- **MainPage.xaml.cs** – `MainPage` 클래스의 코드 숨김으로, 사용자가 페이지와 상호 작용할 때 실행되는 비즈니스 논리를 포함하고 있습니다.
- **PhoneTranslator.cs** – 전화 단어를 전화 번호로 변환하는 역할을 담당하는 비즈니스 논리로, **MainPage.xaml.cs**에서 호출됩니다.

Xamarin.iOS 응용 프로그램에 대한 자세한 내용은 [Xamarin.iOS 응용 프로그램 분석](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application)을 참조하세요. Xamarin.Android 응용 프로그램에 대한 자세한 내용은 [Xamarin.Android 응용 프로그램 분석](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy)을 참조하세요.

## <a name="architecture-and-application-fundamentals"></a>아키텍처 및 응용 프로그램 기본 사항

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.Forms 응용 프로그램은 기존의 플랫폼 간 응용 프로그램과 같은 방식으로 설계됩니다. 공유 코드는 일반적으로 .NET 표준 라이브러리에 배치되고 플랫폼 관련 응용 프로그램은 공유 코드를 사용합니다. 다음 다이어그램은 Phoneword 응용 프로그램에 대한 이 관계의 개요를 보여줍니다.

![](deepdive-images/vs/architecture.png "Phoneword 아키텍처")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Xamarin.Forms 응용 프로그램은 기존의 플랫폼 간 응용 프로그램과 같은 방식으로 설계됩니다. 공유 코드는 일반적으로 .NET 표준 라이브러리에 배치되고 플랫폼 관련 응용 프로그램은 공유 코드를 사용합니다. 다음 다이어그램은 Phoneword 응용 프로그램에 대한 이 관계의 개요를 보여줍니다.

![](deepdive-images/xs/architecture.png "Phoneword 아키텍처")

-----

다음 코드 예제와 같이, 시작 코드의 재사용을 최대화하기 위해 Xamarin.Forms 응용 프로그램에는 각 플랫폼에서 응용 프로그램이 표시할 첫 번째 페이지를 인스턴스화하는 역할을 담당하는 `App`이라는 단일 클래스가 있습니다.

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
namespace Phoneword
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new MainPage();
        }
        ...
    }
}
```

이 코드는 `App` 클래스의 `MainPage` 속성을 [`MainPage`](xref:Xamarin.Forms.Application.MainPage) 클래스의 새 인스턴스로 설정합니다. 또한 [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 특성은 XAML이 중간 언어로 직접 컴파일되도록 컴파일러를 켭니다. 자세한 내용은 [XAML 컴파일](~/xamarin-forms/xaml/xamlc.md)을 참조하세요.

## <a name="launching-the-application-on-each-platform"></a>각 플랫폼에서 응용 프로그램 시작

### <a name="ios"></a>iOS

iOS에서 초기 Xamarin.Forms 페이지를 시작하기 위해 Phoneword.iOS 프로젝트는 다음 코드 예제와 같이 `FormsApplicationDelegate` 클래스에서 상속하는 `AppDelegate` 클래스를 포함하고 있습니다.

```csharp
namespace Phoneword.iOS
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init ();
            LoadApplication (new App ());
            return base.FinishedLaunching (app, options);
        }
    }
}
```

`FinishedLaunching` 재정의는 `Init` 메서드를 호출하여 Xamarin.Forms 프레임워크를 초기화합니다. Xamarin.Forms의 iOS 특정 구현이 루트 뷰 컨트롤러가 `LoadApplication` 메서드에 대한 호출로 설정되기 전에 응용 프로그램에 로드됩니다.

### <a name="android"></a>Android

Android에서 초기 Xamarin.Forms 페이지를 시작하기 위해 Phoneword.Droid 프로젝트는 다음 코드 예제와 같이 `FormsAppCompatActivity` 클래스에서 상속한 작업을 사용하여 `MainLauncher` 특성을 가진 `Activity`를 만드는 코드를 포함하고 있습니다.

```csharp
namespace Phoneword.Droid
{
    [Activity(Label = "Phoneword", 
              Icon = "@mipmap/icon", 
              Theme = "@style/MainTheme", 
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        internal static MainActivity Instance { get; private set; }

        protected override void OnCreate(Bundle bundle)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(bundle);
            Instance = this;
            global::Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication(new App());
        }
    }
}
```

`OnCreate` 재정의는 `Init` 메서드를 호출하여 Xamarin.Forms 프레임워크를 초기화합니다. 이로 인해 Xamarin.Forms 응용 프로그램이 로드되기 전에 Xamarin.Forms의 Android 특정 구현이 응용 프로그램에 로드됩니다. 또한 `MainActivity` 클래스는 그 자체에 대한 참조를 `Instance` 속성에 저장합니다. `Instance` 속성을 로컬 컨텍스트라고도 하며, `PhoneDialer` 클래스에서 참조됩니다.

## <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

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
namespace Phoneword.UWP
{
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.LoadApplication(new Phoneword.App());
        }
    }
}
```

Xamarin.Forms 응용 프로그램은 `LoadApplication` 메서드를 사용해 로드합니다.

> [!NOTE]
> UWP(유니버설 Windows 플랫폼) 앱은 Xamarin.Forms로 빌드할 수 있지만 Windows에서 Visual Studio만 사용합니다.

## <a name="user-interface"></a>사용자 인터페이스

Xamarin.Forms 응용 프로그램의 사용자 인터페이스를 만드는 데 사용되는 4개의 주 제어 그룹이 있습니다.

1. **페이지** – Xamarin.Forms 페이지는 플랫폼 간 모바일 응용 프로그램 화면을 나타냅니다. Phoneword 응용 프로그램은 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 클래스를 사용하여 단일 화면을 표시합니다. 페이지에 대한 자세한 내용은 [Xamarin.Forms 페이지](~/xamarin-forms/user-interface/controls/pages.md)를 참조하세요.
1. **레이아웃** – Xamarin.Forms 레이아웃은 뷰를 논리 구조로 구성하는 데 사용된 컨테이너입니다. Phoneword 응용 프로그램은 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 클래스를 사용하여 컨트롤을 가로 스택에 정렬합니다. 레이아웃에 대한 자세한 내용은 [Xamarin.Forms 레이아웃](~/xamarin-forms/user-interface/controls/layouts.md)을 참조하세요.
1. **뷰** – Xamarin.Forms 뷰는 레이블, 단추 및 텍스트 입력 상자 등의 사용자 인터페이스에 표시되는 컨트롤입니다. Phoneword 응용 프로그램은 [`Label`](xref:Xamarin.Forms.Label), [`Entry`](xref:Xamarin.Forms.Entry) 및 [`Button`](xref:Xamarin.Forms.Button) 컨트롤을 사용합니다. 보기에 대한 자세한 내용은 [Xamarin.Forms 보기](~/xamarin-forms/user-interface/controls/views.md)를 참조하세요.
1. **셀** – Xamarin.Forms 셀은 목록에 있는 항목에 사용되는 특수한 요소이며, 목록의 각 항목이 어떻게 그려져야 하는지를 설명합니다. Phoneword 응용 프로그램은 어떤 셀도 사용하지 않습니다. 셀에 대한 자세한 내용은 [Xamarin.Forms 셀](~/xamarin-forms/user-interface/controls/cells.md)을 참조하세요.

런타임 시 각 컨트롤은 렌더링될 것이기도 한 해당 네이티브에 매핑됩니다.

아무 플랫폼에서 Phoneword 응용 프로그램이 실행되면 Xamarin.Forms의 [`Page`](xref:Xamarin.Forms.Page)에 해당하는 단일 화면이 표시됩니다. `Page`는 Android에서 *ViewGroup*, iOS에서 *보기 컨트롤러* 또는 유니버설 Windows 플랫폼에서 *페이지*를 나타냅니다. 또한 Phoneword 응용 프로그램은 `MainPage` 클래스를 나타내는 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 개체를 인스턴스화하며, 다음 코드 예제에 XAML 태그가 나와 있습니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                         xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                         x:Class="Phoneword.MainPage">
        ...
        <StackLayout>
            <Label Text="Enter a Phoneword:" />
            <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
            <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
            <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
</ContentPage>
```

`MainPage` 클래스는 화면 크기에 관계없이 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 컨트롤을 사용하여 자동으로 화면의 컨트롤을 정렬합니다. 각 자식 요소는 추가된 순서대로 세로 방향으로 차례로 배치됩니다. `StackLayout` 컨트롤은 페이지에 텍스트를 표시하는 [`Label`](xref:Xamarin.Forms.Label) 컨트롤, 텍스트 사용자 입력을 수락하는 [`Entry`](xref:Xamarin.Forms.Entry) 컨트롤, 터치 이벤트에 응답하여 코드를 실행하는 데 사용되는 [`Button`](xref:Xamarin.Forms.Button) 컨트롤 2개를 포함합니다.

Xamarin.Forms의 XAML에 대한 자세한 내용은 [Xamarin.Forms XAML 기본 사항](~/xamarin-forms/xaml/xaml-basics/index.md)을 참조하세요.

### <a name="responding-to-user-interaction"></a>사용자 상호 작용에 응답

XAML에 정의된 개체는 코드 숨김 파일에서 처리되는 이벤트를 발생시킬 수 있습니다. 다음 코드 예제는 *변환* 단추에서 발생하는 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트에 응답하여 실행되는 `MainPage` 클래스에 대한 코드 숨김의 `OnTranslate` 메서드를 보여줍니다.

```csharp
void OnTranslate(object sender, EventArgs e)
{
    translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
    if (!string.IsNullOrWhiteSpace (translatedNumber)) {
        callButton.IsEnabled = true;
        callButton.Text = "Call " + translatedNumber;
    } else {
        callButton.IsEnabled = false;
        callButton.Text = "Call";
    }
}
```

`OnTranslate` 메서드는 전화 단어를 해당하는 전화 번호로 변환하고, 그에 대한 응답으로 통화 단추의 속성을 설정합니다. XAML 클래스의 코드 숨김 파일은 `x:Name` 특성으로 할당된 이름을 사용하여 XAML에서 정의된 개체에 액세스할 수 있습니다. 이 특성에 할당된 값은 C# 변수와 동일한 규칙을 가지며, 따라서 문자 또는 밑줄로 시작해야 하고 공백을 포함하면 안 됩니다.

변환 단추를 `OnTranslate` 메서드에 연결하는 동작은 `MainPage` 클래스에 대한 XAML 태그에서 발생합니다.

```xaml
<Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword에 도입된 추가 개념

Xamarin.Forms에 대한 Phoneword 응용 프로그램에는 이 문서에서 다루지 않은 몇 가지 개념이 도입되었습니다. 이러한 개념은 다음과 같습니다.

- 단추 사용 및 사용 안 함. [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) 속성을 변경하여 [`Button`](xref:Xamarin.Forms.Button)을 켜거나 끌 수 있습니다. 예를 들어 다음 코드 예제에서는 `callButton`을 사용하지 않습니다.

    ```csharp
    callButton.IsEnabled = false;
    ```

- 경고 대화 상자 표시. 사용자가 통화 **단추**를 누르면 Phoneword 응용 프로그램은 *경고 대화 상자*를 표시하고 전화를 걸거나 취소하는 옵션을 제공합니다. [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) 메서드는 다음 코드 예제와 같이 대화 상자를 만드는 데 사용됩니다.

    ```csharp
    await this.DisplayAlert (
            "Dial a Number",
            "Would you like to call " + translatedNumber + "?",
            "Yes",
            "No");
    ```

- [`DependencyService`](xref:Xamarin.Forms.DependencyService) 클래스를 통해 네이티브 기능에 액세스. Phoneword 응용 프로그램은 Phoneword 프로젝트의 다음 코드 예제와 같이 `DependencyService` 클래스를 사용하여 `IDialer` 인터페이스를 플랫폼 관련 전화 걸기 구현으로 해결합니다.

    ```csharp
    async void OnCall (object sender, EventArgs e)
    {
        ...
        var dialer = DependencyService.Get<IDialer> ();
        ...
    }
    ```

  [`DependencyService`](xref:Xamarin.Forms.DependencyService) 클래스에 대한 자세한 내용은 [DependencyService를 통한 네이티브 기능에 액세스](~/xamarin-forms/app-fundamentals/dependency-service/index.md)를 참조하세요.

- URL로 전화 걸기. Phoneword 응용 프로그램은 `OpenURL`을 사용하여 시스템 전화 앱을 시작합니다. iOS 프로젝트의 다음 코드 예제와 같이, URL은 `tel:` 접두사 뒤에 전화를 걸 전화 번호가 붙습니다.

    ```csharp
    return UIApplication.SharedApplication.OpenUrl (new NSUrl ("tel:" + number));
    ```

- 플랫폼 레이아웃 조정. 여러 플랫폼에서 여러 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 값을 사용하여 각 페이지를 올바르게 표시하는 다음 코드 예제처럼, 개발자는 [`Device`](xref:Xamarin.Forms.Device) 클래스를 사용하여 플랫폼별로 응용 프로그램 레이아웃 및 기능을 사용자 지정할 수 있습니다.

    ```xaml
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms" ... >
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        ...
    </ContentPage>
    ```

  플랫폼 조정에 대한 자세한 내용은 [장치 클래스](~/xamarin-forms/platform/device.md)를 참조하세요.

## <a name="testing-and-deployment"></a>테스트 및 배포

Mac용 Visual Studio와 Visual Studio는 응용 프로그램을 테스트하고 배포하기 위한 다양한 옵션을 제공합니다. 응용 프로그램 디버그는 응용 프로그램 개발 주기에서 일반적인 과정이며 코드 문제를 진단하는 데 도움이 됩니다. 자세한 내용은 [중단점 설정](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [단계별 코드 실행](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code) 및 [로그 창에 정보 출력](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)을 참조하세요.

시뮬레이터는 응용 프로그램 배포 및 테스트를 시작하기에 좋은 위치이며, 응용 프로그램을 테스트하는 유용한 기능을 제공합니다. 그러나 사용자가 최종 응용 프로그램을 시뮬레이터에서 사용하지는 않으므로 초기에 자주 실제 장치에서 응용 프로그램을 테스트해야 합니다. iOS 장치 프로비전에 대한 자세한 내용은 [장치 프로비전](~/ios/get-started/installation/device-provisioning/index.md)을 참조하세요. Android 장치 프로비전에 대한 자세한 내용은 [개발용 장치 설정](~/android/get-started/installation/set-up-device-for-development.md)을 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms를 사용한 응용 프로그램 개발의 기본적인 사항을 살펴보았습니다. Xamarin.Forms 응용 프로그램 분석, 아키텍처 및 응용 프로그램 기본 사항, 사용자 인터페이스에 대해 다루었습니다.

이 가이드의 다음 섹션에서는 여러 화면을 포함하고 좀 더 고급 Xamarin.Forms 아키텍처 및 개념을 살펴볼 수 있도록 응용 프로그램을 확장하겠습니다.
