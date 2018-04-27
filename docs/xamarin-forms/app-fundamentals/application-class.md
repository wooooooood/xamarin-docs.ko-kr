---
title: App 클래스
description: C# 또는 XAML 수 있는 기본 App 클래스의 기능
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 5c9eed8f48a40bc7feaadd0c644610f691713e9b
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="app-class"></a>App 클래스

`Application` 노출 되는 기본 프로젝트에에서는 다음 기능을 제공 하는 기본 클래스 `App` 서브 클래스:

* A `MainPage` 응용 프로그램에 대 한 초기 페이지를 설정 하는 위치에 해당 하는 속성이 있습니다.
* 영구 [ `Properties` 사전](#Properties_Dictionary) 주기 상태 변경을 통해 간단한 값을 저장할 수 있습니다.
* 정적 `Current` 현재 응용 프로그램 개체에 대 한 참조를 포함 하는 속성입니다.

또한 노출 [수명 주기 메서드](~/xamarin-forms/app-fundamentals/app-lifecycle.md) 같은 `OnStart`, `OnSleep`, 및 `OnResume` 모달 탐색 이벤트 뿐만 아니라 합니다.

템플릿에 따라 선택는 `App` 두 가지 방법 중 하나에서 클래스를 정의할 수 있습니다.

* **C#**, 또는
* **XAML 및 C#**

만들려는 **앱** XAML에서는 기본값을 사용 하 여 클래스 **앱** 클래스는 XAML로 바꾸어야 합니다 **앱** 클래스와 관련 된 코드 숨김, 다음 코드 예제에서와 같이:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

다음 코드 예제에서는 연결 된 코드 숨김을 보여 줍니다.

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

설정 뿐만 아니라는 [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) 속성을 코드 숨김 호출 또한 해야는 `InitializeComponent` 메서드를 로드 및 연결 된 XAML을 구문 분석 합니다.

## <a name="mainpage-property"></a>기본 페이지 속성

`MainPage` 속성에는 `Application` 응용 프로그램의 루트 페이지를 설정 하는 클래스입니다.

예를 들어 논리를 만들 수 있습니다 프로그램 `App` 사용자의 로그인은 여부에 따라 서로 다른 페이지를 표시 하는 클래스입니다.

`MainPage` 속성는 `App` 생성자

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

## <a name="properties-dictionary"></a>속성 사전

`Application` 하위 클래스에 정적 `Properties` 에서 사용 하기 위해 특히 데이터 저장에 사용할 수 있는 사전은 `OnStart`, `OnSleep`, 및 `OnResume` 메서드. 이 사용 하 여 Xamarin.Forms 코드의 모든 위치에서 액세스할 수 `Application.Current.Properties`합니다.

`Properties` 사전을 사용 하 여 한 `string` 키 및 저장소는 `object` 값입니다.

예를 들어 한 영구를 설정할 수 있습니다 `"id"` 코드에서 아무 곳 이나 속성 (항목을 선택 하면 페이지의 `OnDisappearing` 메서드, 또는 `OnSleep` 메서드)를 이렇게:

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

에 `OnStart` 또는 `OnResume` 메서드를 특정 방식으로 사용자의 경험을 다시이 값을 사용할 수 있습니다. `Properties` 사전 저장소 `object`s 사용 하기 전에 해당 값을 캐스팅 해야 합니다.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

예기치 않은 오류를 방지 하기 위해 액세스 하기 전에 항상 키가 있는지 확인 합니다.

> [!NOTE]
> `Properties` 사전 저장소에 대 한 기본 형식만 직렬화 할 수 있습니다. 다른 형식을 저장 하는 (예: `List<string>`) 자동으로 실패할 수 있습니다.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>지속성

`Properties` 사전 장치에 자동으로 저장 됩니다.
사전에 추가 된 데이터는 백그라운드에서 응용 프로그램이 반환 또는 다시 시작 후에 제공 됩니다.

1.4 Xamarin.Forms에는 다른 방법을 추가로 도입는 `Application` 클래스- `SavePropertiesAsync()` -적극적으로 유지 하기 위해 호출할 수 있는 `Properties` 사전입니다. 이 위험 하지 충돌 또는 운영 체제에서 중단 되 고 직렬화 하기에 보다는 중요 한 업데이트 후 속성을 저장할 수 있도록 합니다.

사용 하 여에 대 한 참조를 찾을 수 있습니다는 `Properties` 사전에는 **Xamarin.Forms 사용 하 여 모바일 앱 만들기** 장 예약 [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), 및 [20 ](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf), 및 연결 된 [샘플](https://github.com/xamarin/xamarin-forms-book-preview-2)합니다.



## <a name="the-application-class"></a>Application 클래스

전체 `Application` 참조에 대 한 클래스 구현을 다음과 같습니다.

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

이 클래스는 다음 각 플랫폼 특정 프로젝트에서 인스턴스화된와에 전달 되는 `LoadApplication` where 메서드는 `MainPage` 로드 되 고 사용자에 게 표시 합니다.
각 플랫폼에 대 한 코드는 다음 섹션에 표시 됩니다. 최신 Xamarin.Forms 솔루션 템플릿을는 앱에 대 한 미리 구성 된이 모든 코드가 이미 포함 되어 있습니다.


### <a name="ios-project"></a>iOS 프로젝트

IOS `AppDelegate` 에서 이제 클래스를 상속 `FormsApplicationDelegate`합니다. 수행 해야합니다.

* 호출 `LoadApplication` 의 인스턴스와 `App` 클래스입니다.

* 항상 반환 `base.FinishedLaunching (app, options);`합니다.

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

Android `MainActivity` 에서 이제 상속 `FormsApplicationActivity`합니다. 에 `OnCreate` 재정의 `LoadApplication` 의 인스턴스와 메서드는 `App` 클래스입니다.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

> [!NOTE]
> 최신는 [ `FormsAppCompatActivity` ](~/xamarin-forms/platform/android/appcompat.md) 기본 Android 자료 디자인을 더 잘 지원 하기 위해 사용할 수 있는 클래스입니다.
> 이것은 기본 Android 템플릿을 나중에 되지만 참고할 수 [이러한 지침](~/xamarin-forms/platform/android/appcompat.md) 기존 Android 앱을 업데이트 합니다.

### <a name="universal-windows-project-uwp-for-windows-10"></a>Windows 10 용 유니버설 Windows 프로젝트 (UWP)

참조 [설치 Windows 프로젝트](~/xamarin-forms/platform/windows/installation/index.md) xamarin.forms에서 UWP 지원에 대 한 정보에 대 한 합니다.

UWP 프로젝트에서 기본 페이지에서 상속 해야 `WindowsPage`합니다. 즉, XAML 및 C#에 대 한 `MainPage` 참조는 `FormsApplicationPage` 표시 된 것 처럼 클래스입니다.

XAML 루트 요소 반영 되도록 사용자 지정 네임 스페이스에서 사용 하 여 `FormsApplicationPage` 클래스:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

C# 코드 숨김의 구조를 호출 해야 `LoadApplication` 프로그램 Xamarin.Forms의 인스턴스를 만드는 `App`합니다. 명시적으로 한정 하려면 응용 프로그램 네임 스페이스를 사용 하 여 것이 좋습니다는는 `App` UWP 응용 프로그램에 있으므로 또한 자신의 `App` Xamarin.Forms와 관련 없는 클래스입니다.

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

`Forms.Init()` 호출 해야만 **App.xaml.cs** 줄 63입니다.
