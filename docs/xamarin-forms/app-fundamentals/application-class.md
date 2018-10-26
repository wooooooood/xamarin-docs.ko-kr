---
title: Xamarin.Forms 앱 클래스
description: 이 문서에서는 앱의 초기 페이지를 설정 하는 속성을 포함 하는 기본 앱 클래스의 기능을 설명 하 고 영구 사전에 대 한 수명 주기 상태 변경에서 단순 값을 저장 합니다.
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: cebdd6fafafae7d1cd6258e6200808731e3c4f29
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118814"
---
# <a name="xamarinforms-app-class"></a>Xamarin.Forms 앱 클래스

합니다 `Application` 기본 클래스는 프로젝트 기본에서 노출 되는 다음 기능을 제공 `App` 하위 클래스입니다.

* `MainPage` 속성은 앱에 대 한 초기 페이지를 설정 하는 위치입니다.
* 영구 [ `Properties` 사전](#Properties_Dictionary) 수명 주기 상태 변경에서 간단한 값을 저장할 수 있습니다.
* 정적 `Current` 현재 응용 프로그램 개체에 대 한 참조를 포함 하는 속성입니다.

도 노출 [수명 주기 메서드](~/xamarin-forms/app-fundamentals/app-lifecycle.md) 와 같은 `OnStart`합니다 `OnSleep`, 및 `OnResume` 모달 탐색 이벤트 뿐만 아니라 합니다.

템플릿에 따라 했다면는 `App` 두 가지 방법 중 하나에서 클래스를 정의할 수 있습니다.

* **C#**, 또는
* **XAML 및 C#**

만들려는 **앱** XAML에서 기본값을 사용 하 여 클래스 **앱** 클래스는 XAML을 사용 하 여 바꾸어야 합니다 **앱** 클래스와 관련 된 코드 숨김에 다음 코드 예제 에서처럼:

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

설정 뿐만 아니라 합니다 [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) 속성을 코드 숨김도 호출 해야 합니다는 `InitializeComponent` 로드 및 연결 된 XAML을 구문 분석 방법입니다.

## <a name="mainpage-property"></a>MainPage 속성

합니다 `MainPage` 속성에는 `Application` 응용 프로그램의 루트 페이지를 설정 하는 클래스입니다.

예를 들어 논리를 만들 수 있습니다 프로그램 `App` 여부는 사용자의 로그인 여부에 따라 다른 페이지를 표시 하는 클래스입니다.

합니다 `MainPage` 속성을 설정 해야 합니다 `App` 생성자

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

`Application` 서브 클래스에 정적 `Properties` 에서 사용 하기 위해 특히 데이터를 저장할 수 있는 사전 합니다 `OnStart`, `OnSleep`, 및 `OnResume` 메서드. 사용 하 여 Xamarin.Forms 코드에서 액세스할 수 있습니다 `Application.Current.Properties`합니다.

합니다 `Properties` 사전을 사용 하는 `string` 키 및 저장소는 `object` 값입니다.

예를 들어, 설정할 수 있습니다 영구적 `"id"` 코드에서 아무 곳 이나 속성 (항목을 선택할 경우 페이지의 `OnDisappearing` 메서드를 또는 `OnSleep` 메서드) 같이:

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

에 `OnStart` 또는 `OnResume` 메서드 어떤 방식으로든에서 사용자 환경을 다시이 값을 사용할 수 있습니다. 합니다 `Properties` 사전 저장 `object`s를 사용 하기 전에 해당 값을 캐스팅 해야 합니다.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

예기치 않은 오류를 방지 하려면 액세스 키가 있는지 항상 확인 합니다.

> [!NOTE]
> `Properties` 사전만이 저장소에 대 한 형식 정보를 serialize 할 수 있습니다. 다른 형식을 저장 하는 동안 (같은 `List<string>`) 자동으로 실패할 수 있습니다.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>지속성

`Properties` 사전 장치에 자동으로 저장 됩니다.
사전에 추가 하는 데이터는 응용 프로그램이 백그라운드에서 반환 하는 경우 또는 다시 시작 후에 제공 됩니다.

Xamarin.Forms 1.4에서 추가 메서드를 도입 합니다 `Application` 클래스- `SavePropertiesAsync()` -적극적으로 유지 하기 위해 호출할 수는 `Properties` 사전입니다. 이 위험 하지 때문에 충돌이 나 OS에서 중단 되 고 serialize 시작 하는 것이 아니라 중요 한 업데이트 후 속성을 저장할 수 있도록 합니다.

사용 하 여에 대 한 참조를 찾을 수 있습니다는 `Properties` 사전에는 **Creating Mobile Apps with Xamarin.Forms** 장에서 책 [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf)를 [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), 및 [20 ](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf), 및 연관 된 [샘플](https://github.com/xamarin/xamarin-forms-book-preview-2)합니다.



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

이 클래스는 다음 각 플랫폼별 프로젝트에서 인스턴스화된 이며에 전달 합니다 `LoadApplication` where 메서드는 `MainPage` 로드 되 고 사용자에 게 표시 합니다.
각 플랫폼에 대 한 코드는 다음 섹션에 표시 됩니다. 최신 Xamarin.Forms 솔루션 템플릿을 이미 앱에 대 한 미리 구성 된이 모든 코드를 포함 합니다.


### <a name="ios-project"></a>iOS 프로젝트

IOS `AppDelegate` 클래스에서 상속 `FormsApplicationDelegate`합니다. 수행 해야합니다.

* 호출 `LoadApplication` 의 인스턴스를 사용 하 여는 `App` 클래스입니다.

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

Android `MainActivity` 에서 상속 `FormsAppCompatActivity`합니다. 에 `OnCreate` 재정의 `LoadApplication` 인스턴스의 메서드를 호출 합니다 `App` 클래스.

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

### <a name="universal-windows-project-uwp-for-windows-10"></a>Windows 10 용 유니버설 Windows 프로젝트 (UWP)

참조 [Windows 프로젝트 설정](~/xamarin-forms/platform/windows/installation/index.md) xamarin.forms에서 UWP 지원에 대 한 정보에 대 한 합니다.

UWP 프로젝트에서 기본 페이지에서 상속 해야 `WindowsPage`:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

C# 코드 숨김의 생성 호출 해야 합니다 `LoadApplication` 하면 Xamarin.Forms의 인스턴스를 만드는 `App`합니다. 좋습니다 되려면 응용 프로그램 네임 스페이스를 명시적으로 사용 하는 것은 `App` UWP 응용 프로그램에 있으므로 또한 자신의 `App` Xamarin.Forms 관련이 클래스.

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

사실은 `Forms.Init()` 에서 호출 해야 **App.xaml.cs** 줄 63입니다.
