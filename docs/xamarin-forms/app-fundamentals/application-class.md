---
title: Xamarin.Forms App 클래스
description: 이 문서에서는 기본 App 클래스의 기능을 설명합니다. 이 클래스에는 앱의 초기 페이지로 설정할 속성과 수명 주기 상태 변경 전체에 걸친 단순 값을 저장하는 영구 사전이 포함되어 있습니다.
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 9acd1b8f25696267578f5cc269eb1b0c738be571
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675096"
---
# <a name="xamarinforms-app-class"></a>Xamarin.Forms App 클래스

`Application` 기본 클래스에서 제공하여 프로젝트의 기본 `App` 하위 클래스에 공개되는 기능은 다음과 같습니다.

* `MainPage` 속성 - 앱의 초기 페이지를 설정할 위치입니다.
* 영구 [`Properties` 사전](#Properties_Dictionary) - 수명 주기 상태 변경 전반에 걸친 단순 값을 저장합니다.
* 정적 `Current` 속성 - 현재 애플리케이션 개체에 대한 참조를 포함합니다.

또한 [수명 주기 메서드](~/xamarin-forms/app-fundamentals/app-lifecycle.md)(예: `OnStart`, `OnSleep`, `OnResume`)와 모달 탐색 이벤트도 제공합니다.

선택한 템플릿에 따라 `App` 클래스는 다음 두 가지 방법 중 하나로 정의할 수 있습니다.

* **C#** 또는
* **XAML 및 C#**

XAML을 사용하여 **App** 클래스를 만들려면 다음 코드 예제와 같이 기본 **App** 클래스를 XAML **App** 클래스 및 연결된 코드 숨김으로 바꿔야 합니다.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

다음 코드 예제에서는 연결된 코드 숨김을 보여 줍니다.

```csharp
public partial class App : Application
{
    public App ()
    {
        InitializeComponent ();
        MainPage = new HomePage ();
    }
    ...
}
```

[`MainPage`](xref:Xamarin.Forms.Application.MainPage) 속성을 설정할 것뿐만 아니라 코드 숨김에서 `InitializeComponent` 메서드를 호출하여 연결된 XAML을 로드하고 구문 분석해야 합니다.

## <a name="mainpage-property"></a>MainPage 속성

`Application` 클래스의 `MainPage` 속성은 애플리케이션의 루트 페이지를 설정합니다.

예를 들어 사용자가 로그인했는지 여부에 따라 다른 페이지를 표시하는 논리를 `App` 클래스에 만들 수 있습니다.

`MainPage` 속성은 `App` 생성자에 설정해야 합니다.

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }
}
```

<a name="Properties_Dictionary" />

## <a name="properties-dictionary"></a>Properties 사전

`Application` 하위 클래스에는 데이터를 저장하는 데 사용할 수 있는 정적 `Properties` 사전이 있으며, 특히 `OnStart`, `OnSleep` 및 `OnResume` 메서드에서 사용할 수 있습니다. 이 하위 클래스는 `Application.Current.Properties`를 사용하여 Xamarin.Forms 코드의 어느 곳에서나 액세스할 수 있습니다.

`Properties` 사전은 `string` 키를 사용하고 `object` 값을 저장합니다.

예를 들어 다음과 같이 코드의 아무 곳에서(항목이 선택된 경우 페이지의 `OnDisappearing` 메서드 또는 `OnSleep` 메서드에서) 영구 `"id"` 속성을 설정할 수 있습니다.

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

그러면 `OnStart` 또는 `OnResume` 메서드에서 이 값을 사용하여 사용자의 환경을 어떤 방식으로든 다시 만들 수 있습니다. `Properties` 사전에는 `object`가 저장되므로 사용하기 전에 해당 값을 캐스팅해야 합니다.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

예기치 않은 오류를 방지하기 위해 키에 액세스하기 전에 키가 있는지 항상 확인하세요.

> [!NOTE]
> `Properties` 사전은 스토리지에 대한 기본 형식만 직렬화할 수 있습니다. 다른 형식(예: `List<string>`)을 저장하려고 하면 자동으로 실패할 수 있습니다.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>지속성

`Properties` 사전은 자동으로 디바이스에 저장됩니다.
사전에 추가된 데이터는 애플리케이션이 백그라운드에서 반환되거나 다시 시작된 후에도 사용할 수 있습니다.

Xamarin.Forms 1.4는 `Application` 클래스에 `SavePropertiesAsync()` 추가 메서드를 도입했으며, 이 메서드는 `Properties` 사전을 사전에 유지하기 위해 호출할 수 있습니다. 이렇게 하면 중요한 업데이트 후에 충돌로 인해 직렬화되지 않거나 OS에서 종료하는 위험을 감수하지 않으면서 속성을 저장할 수 있습니다.

**Xamarin.Forms를 사용하여 모바일 애플리케이션 만들기** 서적의 [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf) 및 [20](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) 장과 관련 [샘플](https://github.com/xamarin/xamarin-forms-book-preview-2)에서 `Properties` 사전 사용에 대한 참조를 찾을 수 있습니다.



## <a name="the-application-class"></a>Application 클래스

참조에 대한 완전한 `Application` 클래스 구현은 아래와 같습니다.

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }

    protected override void OnStart()
    {
        // Handle when your app starts
        Debug.WriteLine ("OnStart");
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Debug.WriteLine ("OnSleep");
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
        Debug.WriteLine ("OnResume");
    }
}

```

그러면 이 클래스가 각 플랫폼별 프로젝트에서 인스턴스화되고, `MainPage`가 로드되어 사용자에게 표시되는 `LoadApplication` 메서드로 전달됩니다.
각 플랫폼에 대한 코드는 다음 섹션에서 보여 줍니다. 최신 Xamarin.Forms 솔루션 템플릿에는 이 코드가 모두 포함되어 있으며 애플리케이션에 맞게 미리 구성되어 있습니다.


### <a name="ios-project"></a>iOS 프로젝트

iOS `AppDelegate` 클래스는 `FormsApplicationDelegate`에서 상속됩니다. 수행하는 작업은 다음과 같습니다.

* `App` 클래스의 인스턴스를 사용하여 `LoadApplication`을 호출합니다.

* 항상 `base.FinishedLaunching (app, options);`를 반환합니다.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="android-project"></a>Android 프로젝트

Android `MainActivity`는 `FormsAppCompatActivity`에서 상속됩니다. `OnCreate` 재정의에서 `LoadApplication` 메서드가 `App` 클래스의 인스턴스를 사용하여 호출됩니다.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity : FormsAppCompatActivity
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="universal-windows-project-uwp-for-windows-10"></a>Windows 10용 UWP(유니버설 Windows 프로젝트)

Xamarin.Forms의 UWP 지원에 대한 자세한 내용은 [Windows 프로젝트 설정](~/xamarin-forms/platform/windows/installation/index.md)을 참조하세요.

UWP 프로젝트의 기본 페이지는 `WindowsPage`에서 상속됩니다.

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

C# 코드 숨김 생성은 `LoadApplication`을 호출하여 Xamarin.Forms `App`의 인스턴스를 만들어야 합니다. UWP 애플리케이션에도 Xamarin.Forms와 관련이 없는 자체의 고유한 `App` 클래스가 있으므로 애플리케이션 네임스페이스를 사용하여 `App`을 한정하는 것이 좋습니다.

```csharp
public sealed partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();

        LoadApplication(new YOUR_NAMESPACE.App());
    }
 }
```

`Forms.Init()`는 **App.xaml.cs**의 63번 줄 근처에서 호출해야 합니다.
