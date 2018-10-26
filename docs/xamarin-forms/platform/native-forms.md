---
title: Xamarin Native 프로젝트에서 Xamarin.Forms
description: 이 문서에서는 직접 Xamarin 네이티브 프로젝트에 추가 된 ContentPage에서 파생 된 페이지를 사용 하는 방법 및 이들 사이 탐색 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: 04d435b29f6f2f577df5025995fcc074ba5d9d9d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122753"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin Native 프로젝트에서 Xamarin.Forms

_네이티브 양식 Xamarin.Forms ContentPage 파생 페이지 Xamarin.iOS, Xamarin.Android 및 유니버설 Windows 플랫폼 (UWP)에 대 한 네이티브 프로젝트에서 사용할 수 있도록 허용 합니다. 네이티브 프로젝트는.NET Standard 라이브러리,.NET Standard 라이브러리 또는 공유 프로젝트 또는 프로젝트에 직접 추가 된 ContentPage에서 파생 된 페이지를 사용할 수 있습니다. 이 문서에서는 직접 네이티브 프로젝트에 추가 된 ContentPage에서 파생 된 페이지를 사용 하는 방법 및 이들 사이 탐색 하는 방법을 설명 합니다._

Xamarin.Forms 응용 프로그램에서 파생 되는 하나 이상의 페이지를 포함 하는 일반적으로 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), 이러한 페이지는.NET Standard 라이브러리 프로젝트 또는 공유 프로젝트에서 모든 플랫폼에서 공유 되 고 있습니다. 그러나 네이티브 Forms 허용 `ContentPage`-네이티브 Xamarin.iOS, Xamarin.Android 및 UWP 응용 프로그램에 직접 추가 페이지를 파생 합니다. 사용 하는 네이티브 프로젝트에 비해 `ContentPage`-.NET Standard 라이브러리 프로젝트 또는 공유 프로젝트를 네이티브 프로젝트에 직접 페이지를 추가 하는 장점은에서 파생 된 페이지는 기본 뷰를 사용 하 여 페이지를 확장할 수 있습니다. 네이티브 뷰 사용 하 여 XAML에서 이름을 지정할 수 있습니다 `x:Name` 및 코드 숨김 파일에서 참조 합니다. 기본 보기에 대 한 자세한 내용은 참조 하세요. [네이티브 뷰](~/xamarin-forms/platform/native-views/index.md)합니다.

Xamarin.Forms를 사용 하기 위한 프로세스 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-네이티브 프로젝트에서 파생 된 페이지는 다음과 같습니다.

1. 네이티브 프로젝트에 Xamarin.Forms NuGet 패키지를 추가 합니다.
1. 추가 된 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-페이지 및 네이티브 프로젝트에 모든 종속성을 파생 합니다.
1. `Forms.Init` 메서드를 호출합니다.
1. 인스턴스를 생성 합니다 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-페이지를 파생 하 고 다음 확장 메서드 중 하나를 사용 하 여 적절 한 네이티브 형식으로 변환: `CreateViewController` ios의 경우 `CreateFragment` 또는 `CreateSupportFragment` android의 경우 또는 `CreateFrameworkElement` UWP에 대 한 합니다.
1. 네이티브 형식 표현으로 이동 합니다 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-기본 탐색 API를 사용 하 여 페이지를 파생 합니다.

Xamarin.Forms를 호출 하 여 초기화 해야 합니다는 `Forms.Init` 메서드가 네이티브 프로젝트를 만들 수는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-페이지를 파생 합니다. 응용 프로그램 흐름에서 가장 편리한 시간에 따라이 작업을 주로 수행 하는 경우 선택 – 바로 앞 이나 응용 프로그램 시작 시 수행할 수는 `ContentPage`-파생된 페이지 생성 됩니다. 이 문서 및 함께 제공 되는 샘플 응용 프로그램에는 `Forms.Init` 응용 프로그램 시작 시 호출 됩니다.

> [!NOTE]
> 합니다 **NativeForms** Xamarin.Forms 프로젝트 샘플 응용 프로그램 솔루션에 없습니다. 대신, Xamarin.iOS 프로젝트, Xamarin.Android 프로젝트 및 UWP 프로젝트의 구성 됩니다. 각 프로젝트에 사용할 기본 폼을 사용 하는 네이티브 프로젝트는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-페이지를 파생 합니다. 그러나 네이티브 프로젝트에서 사용할 수 없습니다 이유는 없는 이유는 `ContentPage`-.NET Standard 라이브러리 프로젝트 또는 공유 프로젝트에서 페이지를 파생 합니다.

Xamarin.Forms와 같은 기능 기본 폼을 사용할 때 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService)를 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter), 및 데이터 바인딩 엔진에서 모두 계속 작동 합니다. 그러나 페이지 탐색 네이티브 탐색 API를 사용 하 여 수행 되어야 합니다.

## <a name="ios"></a>iOS

Ios의 경우는 `FinishedLaunching` 의 재정의 `AppDelegate` 클래스는 일반적으로 응용 프로그램을 수행 하는 위치 관련 작업을 시작 합니다. 응용 프로그램에 시작 되며 주 창을 구성 하 고 컨트롤러를 보려면 일반적으로 재정의 되 면 호출 됩니다. 다음 코드 예제는 `AppDelegate` 샘플 응용 프로그램에서 클래스:

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static AppDelegate Instance;

    UIWindow _window;
    UINavigationController _navigation;

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        Forms.Init();

        Instance = this;
        _window = new UIWindow(UIScreen.MainScreen.Bounds);

        UINavigationBar.Appearance.SetTitleTextAttributes(new UITextAttributes
        {
            TextColor = UIColor.Black
        });

        var mainPage = new PhonewordPage().CreateViewController();
        mainPage.Title = "Phoneword";

        _navigation = new UINavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    ...
}
```

`FinishedLaunching` 메서드는 다음 작업을 수행 합니다.

- Xamarin.Forms를 호출 하 여 초기화 되는 `Forms.Init` 메서드.
- 에 대 한 참조를 `AppDelegate` 클래스에 저장 되는 `static` `Instance` 필드입니다. 다른 클래스에 정의 된 메서드를 호출 하는 메커니즘을 제공 하는 것이 `AppDelegate` 클래스입니다.
- `UIWindow`, 네이티브 iOS 응용 프로그램에서 보기에 대 한 기본 컨테이너는 만들어집니다.
- 합니다 `PhonewordPage` 는 Xamarin.Forms 클래스 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-파생 XAML에 정의 된 페이지, 생성 되 고 변환를 `UIViewController` 사용 하 여는 `CreateViewController` 확장 메서드.
- `Title` 의 속성을 `UIViewController` 에 표시 되는 설정 되어를 `UINavigationBar`.
- `UINavigationController` 계층적 탐색 관리에 대해 생성 됩니다. `UINavigationController` 클래스 뷰 컨트롤러의 스택을 관리 및 `UIViewController` 에 전달 된 생성자 나타납니다 처음 경우는 `UINavigationController` 로드 됩니다.
- 합니다 `UINavigationController` 인스턴스는 최상위 수준으로 설정 됩니다 `UIViewController` 에 대 한 합니다 `UIWindow`, 및 `UIWindow` 응용 프로그램에 대 한 키 창으로 설정 되 고 표시 됩니다.

한 번 합니다 `FinishedLaunching` 메서드가 실행 되었는지, Xamarin.Forms UI 정의 `PhonewordPage` 클래스 다음 스크린샷에 표시 된 것 처럼 표시 될:

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png#lightbox "PhonewordPage iOS")

탭 하 여 예를 들어 UI와 상호 작용을 [ `Button` ](xref:Xamarin.Forms.Button), 이벤트 처리기에서 발생 합니다 `PhonewordPage` 코드 숨김 실행. 사용자가 경우 예를 들어 합니다 **통화 기록** 단추, 다음 이벤트 처리기가 실행:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToCallHistoryPage();
}
```

`static` `AppDelegate.Instance` 필드를 사용 하 여 `AppDelegate.NavigateToCallHistoryPage` 다음 코드 예제에 나와 있는 메서드를 호출 하려면:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateViewController();
    callHistoryPage.Title = "Call History";
    _navigation.PushViewController(callHistoryPage, true);
}
```

`NavigateToCallHistoryPage` 메서드 변환 Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-페이지를 파생을 `UIViewController` 사용 하 여를 `CreateViewController` 집합과 확장 메서드를를 `Title` 속성은 `UIViewController`합니다. 합니다 `UIViewController` 다음 푸시됩니다 `UINavigationController` 여는 `PushViewController` 메서드. Xamarin.Forms에 UI를 정의 하는 따라서 `CallHistoryPage` 클래스 다음 스크린샷에 표시 된 것 처럼 표시 됩니다.

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png#lightbox "CallHistoryPage iOS")

경우는 `CallHistoryPage` 뒤로 탭 표시 됩니다 화살표 표시 됩니다는 `UIViewController` 에 대 한는 `CallHistoryPage` 에서 클래스를 `UINavigationController`, 사용자 수를 반환를 `UIViewController` 에 대 한는 `PhonewordPage` 클래스.

## <a name="android"></a>Android

Android에는 `OnCreate` 에서 재정의 `MainActivity` 클래스는 일반적으로 응용 프로그램을 수행 하는 위치 관련 작업을 시작 합니다. 다음 코드 예제는 `MainActivity` 샘플 응용 프로그램에서 클래스:

```csharp
public class MainActivity : AppCompatActivity
{
    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Phoneword";

        var mainPage = new PhonewordPage().CreateFragment(this);
        FragmentManager
            .BeginTransaction()
            .Replace(Resource.Id.fragment_frame_layout, mainPage)
            .Commit();
        ...
    }
    ...
}
```

`OnCreate` 메서드는 다음 작업을 수행 합니다.

- Xamarin.Forms를 호출 하 여 초기화 되는 `Forms.Init` 메서드.
- 에 대 한 참조를 `MainActivity` 클래스에 저장 되는 `static` `Instance` 필드입니다. 다른 클래스에 정의 된 메서드를 호출 하는 메커니즘을 제공 하는 것이 `MainActivity` 클래스입니다.
- `Activity` 콘텐츠 레이아웃 리소스에서 설정 됩니다. 샘플 응용 프로그램을 레이아웃으로 구성 됩니다는 `LinearLayout` 포함 하는 `Toolbar`, 및 `FrameLayout` 조각 컨테이너 역할을 합니다.
- 합니다 `Toolbar` 를 검색 하 고 작업 표시줄에 대 한 설정의 `Activity`, 작업 표시줄 제목 설정 됩니다.
- 합니다 `PhonewordPage` 는 Xamarin.Forms 클래스 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-파생 XAML에 정의 된 페이지, 생성 되 고 변환를 `Fragment` 사용 하 여는 `CreateFragment` 확장 메서드.
- `FragmentManager` 클래스를 만들고를 대체 하는 트랜잭션을 커밋합니다 합니다 `FrameLayout` 인스턴스를 `Fragment` 에 대 한는 `PhonewordPage` 클래스입니다.

조각에 대 한 자세한 내용은 참조 하세요. [조각](~/android/platform/fragments/index.md)합니다.

> [!NOTE]
> 외에 `CreateFragment` 확장 메서드를 Xamarin.Forms 또한을 `CreateSupportFragment` 메서드. 합니다 `CreateFragment` 메서드를 만듭니다를 `Android.App.Fragment` 이상 API 11을 대상으로 하는 응용 프로그램에서 사용 될 수 있습니다. 합니다 `CreateSupportFragment` 메서드를 만듭니다를 `Android.Support.V4.App.Fragment` 11 이전 API 버전을 대상으로 하는 응용 프로그램에서 사용할 수 있습니다.

한 번 합니다 `OnCreate` 메서드가 실행 되었는지, Xamarin.Forms UI 정의 `PhonewordPage` 클래스 다음 스크린샷에 표시 된 것 처럼 표시 될:

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png#lightbox "Android PhonewordPage")

탭 하 여 예를 들어 UI와 상호 작용을 [ `Button` ](xref:Xamarin.Forms.Button), 이벤트 처리기에서 발생 합니다 `PhonewordPage` 코드 숨김 실행. 사용자가 경우 예를 들어 합니다 **통화 기록** 단추, 다음 이벤트 처리기가 실행:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainActivity.Instance` 필드를 사용 하 여 `MainActivity.NavigateToCallHistoryPage` 다음 코드 예제에 나와 있는 메서드를 호출 하려면:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateFragment(this);
    FragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, callHistoryPage)
        .Commit();
}
```

`NavigateToCallHistoryPage` 메서드 변환 Xamarin.Forms [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-페이지를 파생를 `Fragment` 사용 하 여를 `CreateFragment` 확장 메서드를 추가 합니다 `Fragment` 조각으로 백 스택. Xamarin.Forms에 UI를 정의 하는 따라서 `CallHistoryPage` 표시할 다음 스크린샷에 표시 된 대로:

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png#lightbox "Android CallHistoryPage")

때를 `CallHistoryPage` 뒤로 탭 표시 됩니다 화살표 표시 됩니다는 `Fragment` 에 대 한를 `CallHistoryPage` 조각 백 스택 으로부터 사용자를 반환 합니다 `Fragment` 에 대 한는 `PhonewordPage` 클래스.

### <a name="enabling-back-navigation-support"></a>후방 탐색 지원을 사용 하도록 설정

합니다 `FragmentManager` 클래스에는 `BackStackChanged` 조각 백 스택의 콘텐츠가 변경 될 때마다 발생 하는 이벤트입니다. `OnCreate` 에서 메서드를 `MainActivity` 클래스는이 이벤트에 대 한 익명 이벤트 처리기를 포함 합니다.

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

이 이벤트 처리기는 있는 하나 이상의 작업 모음에서 뒤로 단추 표시 `Fragment` 조각 인스턴스에서 백 스택. 뒤로 단추를 탭에 대 한 응답으로 처리 되는 `OnOptionsItemSelected` 재정의:

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && FragmentManager.BackStackEntryCount > 0)
    {
        FragmentManager.PopBackStack();
        return true;
    }
    return base.OnOptionsItemSelected(item);
}
```

`OnOptionsItemSelected` 재정의 옵션 메뉴에서 항목을 선택할 때마다 호출 됩니다. 이 구현은 뒤로 단추는 선택 되어 있고 하나 이상 가지 조각 백 스택에서 현재 조각 팝 `Fragment` 조각 인스턴스에서 백 스택.

### <a name="multiple-activities"></a>여러 활동

응용 프로그램은 여러 활동을 구성 하는 경우 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-각 작업에 파생 된 페이지를 포함할 수 있습니다. 이 시나리오에서는 합니다 `Forms.Init` 에서만 메서드를 호출 해야 합니다 `OnCreate` 첫 번째 재정의 `Activity` 는 Xamarin.Forms를 포함 하는 `ContentPage`합니다. 그러나,이 작업은 같은 영향을 줍니다.

- 값 `Xamarin.Forms.Color.Accent` 에서 수행 될 합니다 `Activity` 호출한는 `Forms.Init` 메서드.
- 값 `Xamarin.Forms.Application.Current` 연관 될 합니다 `Activity` 호출한는 `Forms.Init` 메서드.

### <a name="choosing-a-file"></a>파일 선택

포함 하는 경우를 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-페이지를 사용 하는 파생을 [ `WebView` ](xref:Xamarin.Forms.WebView) 는 해야 지원 HTML "파일 선택" 단추는 `Activity` 재정의 해야 합니다는 `OnActivityResult` 방법:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

네이티브 UWP에서 `App` 클래스는 일반적으로 응용 프로그램을 수행 하면 관련 작업을 시작 합니다. Xamarin.Forms는 일반적으로, Xamarin.Forms UWP 응용 프로그램에서에서 초기화를 `OnLaunched` 네이티브에서 재정의할 `App` 클래스를 전달 하는 `LaunchActivatedEventArgs` 인수를는 `Forms.Init` 메서드. 이러한 이유로 Xamarin.Forms를 사용 하는 네이티브 UWP 응용 프로그램 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-파생된 페이지를 가장 쉽게 호출할 수는 `Forms.Init` 메서드에서 `App.OnLaunched` 메서드.

기본적으로 네이티브 `App` 시작 클래스는 `MainPage` 응용 프로그램의 첫 번째 페이지 클래스입니다. 다음 코드 예제는 `MainPage` 샘플 응용 프로그램에서 클래스:

```csharp
public sealed partial class MainPage : Page
{
    public static MainPage Instance;

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        this.Content = new Phoneword.UWP.Views.PhonewordPage().CreateFrameworkElement();
    }
    ...
}
```

`MainPage` 생성자는 다음 작업을 수행 합니다.

- 페이지 캐싱을 사용할 수 있도록 새 `MainPage` 사용자 페이지로 이동 하면 생성 되지 않습니다.
- 에 대 한 참조를 `MainPage` 클래스에 저장 되는 `static` `Instance` 필드입니다. 다른 클래스에 정의 된 메서드를 호출 하는 메커니즘을 제공 하는 것이 `MainPage` 클래스입니다.
- `PhonewordPage` 는 Xamarin.Forms 클래스 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-파생 XAML에 정의 된 페이지, 생성 되 고 변환를 `FrameworkElement` 사용 하 여를 `CreateFrameworkElement` 확장 메서드와의 콘텐츠로 설정 합니다 `MainPage` 클래스입니다.

한 번 합니다 `MainPage` 생성자가 실행, Xamarin.Forms UI 정의 `PhonewordPage` 클래스 다음 스크린샷에 표시 된 것 처럼 표시 될:

[![](native-forms-images/uwp-phonewordpage.png "UWP PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png#lightbox "UWP PhonewordPage")

탭 하 여 예를 들어 UI와 상호 작용을 [ `Button` ](xref:Xamarin.Forms.Button), 이벤트 처리기에서 발생 합니다 `PhonewordPage` 코드 숨김 실행. 사용자가 경우 예를 들어 합니다 **통화 기록** 단추, 다음 이벤트 처리기가 실행:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    Phoneword.UWP.MainPage.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainPage.Instance` 필드를 사용 하 여 `MainPage.NavigateToCallHistoryPage` 다음 코드 예제에 나와 있는 메서드를 호출 하려면:

```csharp
public void NavigateToCallHistoryPage()
{
    this.Frame.Navigate(new CallHistoryPage());
}
```

UWP의 탐색은 일반적으로 수행 합니다 `Frame.Navigate` 메서드를 사용 하는 `Page` 인수. Xamarin.Forms 정의 `Frame.Navigate` 사용 하는 확장 메서드를 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)-페이지 인스턴스를 파생 합니다. 따라서 경우 합니다 `NavigateToCallHistoryPage` 메서드가 실행 Xamarin.Forms에 정의 된 UI `CallHistoryPage` 표시할 다음 스크린샷에 표시 된 대로:

[![](native-forms-images/uwp-callhistorypage.png "UWP CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png#lightbox "UWP CallHistoryPage")

때를 `CallHistoryPage` 뒤로 탭 표시 됩니다 화살표 표시 됩니다는 `FrameworkElement` 에 대 한를 `CallHistoryPage` 는 앱의 백 스택 으로부터 사용자를 반환 합니다 `FrameworkElement` 에 대 한는 `PhonewordPage` 클래스.

### <a name="enabling-back-navigation-support"></a>후방 탐색 지원을 사용 하도록 설정

UWP에서 응용 프로그램에서 다른 장치 형식 요인과 모든 하드웨어 및 소프트웨어 뒤로 단추에 대 한 후방 탐색을 활성화 해야 합니다. 에 대 한 이벤트 처리기를 등록 하 여이 작업을 수행할 수 있습니다 합니다 `BackRequested` 에서 수행할 수 있는 이벤트를 `OnLaunched` 메서드는 네이티브에서 `App` 클래스:

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    if (rootFrame == null)
    {
        ...      
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
    }
    ...
}
```

응용 프로그램이 시작 되 면를 `GetForCurrentView` 메서드 검색 합니다 `SystemNavigationManager` 개체가 현재 보기와 관련 된 다음에 대 한 이벤트 처리기를 등록 합니다 `BackRequested` 이벤트. 포그라운드 응용 프로그램 이므로 응답으로 호출 하는 경우만이 이벤트를 받는 응용 프로그램을 `OnBackRequested` 이벤트 처리기:

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
    }
}
```

`OnBackRequested` 이벤트 처리기 호출을 `GoBack` 메서드 집합과 응용 프로그램의 루트 프레임에는 `BackRequestedEventArgs.Handled` 속성을 `true` 처리 이벤트를 표시 하 합니다. 이벤트를 처리 됨으로 표시 하지 못했습니다 (모바일 장치 패밀리)에서 응용 프로그램에서 벗어날 또는 데스크톱 장치 제품군) (에서 이벤트를 무시 하 고 시스템에서 발생할 수 있습니다.

응용 프로그램을 휴대폰에서 제공 하는 시스템 뒤로 단추 사용 하지만 데스크톱 장치에서 제목 표시줄의 뒤로 단추를 표시할지 여부를 선택 합니다. 설정 하 여 이렇게 합니다 `AppViewBackButtonVisibility` 속성 중 하나는 `AppViewBackButtonVisibility` 열거형 값:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

합니다 `OnNavigated` 대 한 응답으로 실행 되는 이벤트 처리기를 `Navigated` 페이지 탐색이 발생할 때 이벤트 발생 제목 뒤로 단추 표시줄의 표시 여부를 업데이트 합니다. 이렇게 하면 제목 표시줄의 뒤로 단추를 앱에 백 스택이 비어 있지 않은 경우에 표시 되었거나 앱에 백 스택이 비어 있으면 제목 표시줄에서 제거 합니다.

UWP의 후방 탐색 지원에 대 한 자세한 내용은 참조 [탐색 기록 및 이전 버전과 UWP 앱에 대 한 탐색](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/)합니다.

## <a name="summary"></a>요약

Native Forms는 Xamarin.Forms의 파생된 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 페이지들을 네이티브 Xamarin.iOS, Xamarin.Android 및 유니버설 Windows 플랫폼(UWP) 프로젝트에서 사용할 수 있게 파생합니다. 네이티브 프로젝트에서 사용할 수 있습니다 `ContentPage`-.NET Standard 라이브러리 프로젝트 또는 공유 프로젝트 또는 프로젝트에 직접 추가 되는 페이지를 파생 합니다. 이 문서를 사용 하는 방법을 설명 `ContentPage`-네이티브 프로젝트 및 이들 사이 탐색 하는 방법에 직접 추가 되는 페이지를 파생 합니다.


## <a name="related-links"></a>관련 링크

- [NativeForms (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [네이티브 뷰](~/xamarin-forms/platform/native-views/index.md)
