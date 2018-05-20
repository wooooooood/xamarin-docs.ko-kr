---
title: 기본 형식
description: 기본 형식 Xamarin.Forms ContentPage 파생 페이지 네이티브 Xamarin.iOS, Xamarin.Android, 및 유니버설 Windows 플랫폼 (UWP) 프로젝트에서 사용할 수 있도록 허용 합니다. 네이티브 프로젝트는.NET 표준 라이브러리, 표준.NET 라이브러리 또는 공유 프로젝트 또는 프로젝트에 직접 추가 된 ContentPage 파생 된 페이지를 사용할 수 있습니다. 이 문서는 네이티브 프로젝트에 직접 추가 된 ContentPage 파생 된 페이지를 사용 하는 방법 및 간에 탐색 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: a103d360221650ee4f679ee285dbedd65e62f947
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2018
---
# <a name="native-forms"></a>기본 형식

_기본 형식 Xamarin.Forms ContentPage 파생 페이지 네이티브 Xamarin.iOS, Xamarin.Android, 및 유니버설 Windows 플랫폼 (UWP) 프로젝트에서 사용할 수 있도록 허용 합니다. 네이티브 프로젝트는.NET 표준 라이브러리, 표준.NET 라이브러리 또는 공유 프로젝트 또는 프로젝트에 직접 추가 된 ContentPage 파생 된 페이지를 사용할 수 있습니다. 이 문서는 네이티브 프로젝트에 직접 추가 된 ContentPage 파생 된 페이지를 사용 하는 방법 및 간에 탐색 하는 방법을 설명 합니다._

Xamarin.Forms 응용 프로그램에서 파생 되는 하나 이상의 페이지를 포함 하는 일반적으로 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), 이러한 페이지는 표준.NET 라이브러리 프로젝트 또는 공유 프로젝트에서 모든 플랫폼에서 공유 되 고 있습니다. 그러나 기본 형식에서는 `ContentPage`-네이티브 Xamarin.iOS, Xamarin.Android, 및 UWP 응용 프로그램에 직접 추가 될 페이지를 파생 합니다. 지정 하는 네이티브 프로젝트 사용에 비해 `ContentPage`-.NET 표준 라이브러리 프로젝트 또는 공유 프로젝트를 네이티브 프로젝트에 직접 페이지를 추가할 경우의 장점은에서 파생 된 페이지는 페이지 기본 보기와 확장 될 수 있습니다. 기본 뷰로 XAML 다음 이름을 지정할 수 `x:Name` 및 코드 숨김 파일에서 참조 합니다. 기본 보기에 대 한 자세한 내용은 참조 [네이티브 뷰](~/xamarin-forms/platform/native-views/index.md)합니다.

Xamarin.Forms를 사용 하기 위한 프로세스 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-기본 프로젝트에서 파생 된 페이지는 다음과 같습니다.

1. 네이티브 프로젝트에 Xamarin.Forms NuGet 패키지를 추가 합니다.
1. 추가 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-파생 된 페이지 및 네이티브 프로젝트에 모든 종속 파일을 합니다.
1. `Forms.Init` 메서드를 호출합니다.
1. 인스턴스를 생성 된 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-페이지를 파생 하 고 다음 확장 메서드 중 하나를 사용 하 여 해당 네이티브 형식으로 변환: `CreateViewController` ios의 경우 `CreateFragment` 또는 `CreateSupportFragment` android의 경우 또는 `CreateFrameworkElement` UWP에 대 한 합니다.
1. 네이티브 형식 표현에 이동 된 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-네이티브 탐색 API를 사용 하 여 페이지를 파생 합니다.

Xamarin.Forms를 호출 하 여 초기화 해야 합니다는 `Forms.Init` 네이티브 프로젝트를 생성 하려면 먼저 메서드는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-파생 페이지입니다. 응용 프로그램 흐름에 가장 편리한 시간에 따라 달라 집니다이 작업을 주로 수행 하는 시기 선택 – 바로 앞 이나 응용 프로그램 시작 시 수행 될 수는 `ContentPage`-파생된 페이지 생성 됩니다. 이 문서와 함께 제공 된 예제 응용 프로그램에는 `Forms.Init` 메서드는 응용 프로그램 시작 시 호출 됩니다.

> [!NOTE]
> **NativeForms** 샘플 응용 프로그램 솔루션 모든 Xamarin.Forms 프로젝트가 포함 되어 있지 않습니다. 대신, 프로젝트 Xamarin.iOS, Xamarin.Android 프로젝트 및 UWP 프로젝트의 구성 됩니다. 각 프로젝트를 사용할 기본 폼을 사용 하는 네이티브 프로젝트는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-페이지를 파생 합니다. 그러나은 아니지만 네이티브 프로젝트를 사용할 수 없습니다. 이유 `ContentPage`-.NET 표준 라이브러리 프로젝트 또는 공유 프로젝트에서 페이지를 파생 합니다.

기본 폼을 사용할 때와 같은 Xamarin.Forms 기능 [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/), [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/), 및 데이터 바인딩 엔진 모두 작동 합니다.

## <a name="ios"></a>iOS

Ios,는 `FinishedLaunching` 의 재정의 `AppDelegate` 클래스는 일반적으로 응용 프로그램을 수행할 수 있는 곳 관련 작업을 시작 합니다. 응용 프로그램에 시작 되 고 주 창을 구성 하 고 컨트롤러를 확인 하기 위해 일반적으로 재정의 됩니다 후 호출 됩니다. 다음 코드 예제는 `AppDelegate` 샘플 응용 프로그램에서 클래스:

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
- 에 대 한 참조는 `AppDelegate` 클래스에 저장 되는 `static` `Instance` 필드입니다. 다른 클래스에 정의 된 메서드를 호출 하는 메커니즘을 제공 하는 것이 고 `AppDelegate` 클래스입니다.
- `UIWindow`, 네이티브 iOS 응용 프로그램의 뷰에 대 한 기본 컨테이너는 만들어집니다.
- `PhonewordPage` Xamarin.Forms는 클래스를 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-파생 XAML에서 정의 된 페이지, 구현 되 고 변환 된 `UIViewController` 를 사용 하 여는 `CreateViewController` 확장 메서드.
- `Title` 의 속성은 `UIViewController` 표시 되는 개체에 설정 되어는 `UINavigationBar`합니다.
- A `UINavigationController` 관리 계층 탐색에 대해 생성 됩니다. `UINavigationController` 클래스 관리 컨트롤러 보기의 스택 및 `UIViewController` 로 전달 된 생성자 나타납니다 처음 때는 `UINavigationController` 로드 됩니다.
- `UINavigationController` 인스턴스가 최상위 수준으로 설정 되어 `UIViewController` 에 대 한는 `UIWindow`, 및 `UIWindow` 응용 프로그램에 대 한 키 창으로 설정 되 고 표시 되도록 설정 됩니다.

한 번는 `FinishedLaunching` 메서드가 실행, UI는 Xamarin.Forms에 정의 된 `PhonewordPage` 클래스 다음 스크린샷에 표시 된 것 처럼 표시 됩니다.

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png#lightbox "PhonewordPage iOS")

탭 하 여 예를 들어 UI와 상호 작용 한 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), 이벤트 처리기에 발생 합니다는 `PhonewordPage` 관련 코드를 실행 합니다. 사용자가 누를 때을 예를 들어는 **호출 기록** 단추를 다음 이벤트 처리기 실행:

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

`NavigateToCallHistoryPage` 메서드 변환 하는 Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-파생 하는 페이지는 `UIViewController` 와 `CreateViewController` 확장 메서드와 설정은 `Title` 속성은 `UIViewController`합니다. `UIViewController` 다음 푸시되 `UINavigationController` 여는 `PushViewController` 메서드. UI는 Xamarin.Forms에 따라서, 정의 `CallHistoryPage` 클래스 다음 스크린샷에 표시 된 것 처럼 표시 됩니다.

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png#lightbox "CallHistoryPage iOS")

때는 `CallHistoryPage` 뒤로 탭 표시 됩니다 팝업 화살표는 `UIViewController` 에 대 한는 `CallHistoryPage` 에서 클래스는 `UINavigationController`, 사용자를 반환는 `UIViewController` 에 대 한는 `PhonewordPage` 클래스입니다.

## <a name="android"></a>Android

Android에서는 `OnCreate` 의 재정의 `MainActivity` 클래스는 일반적으로 응용 프로그램을 수행할 수 있는 곳 관련 작업을 시작 합니다. 다음 코드 예제는 `MainActivity` 샘플 응용 프로그램에서 클래스:

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
- 에 대 한 참조는 `MainActivity` 클래스에 저장 되는 `static` `Instance` 필드입니다. 다른 클래스에 정의 된 메서드를 호출 하는 메커니즘을 제공 하는 것이 고 `MainActivity` 클래스입니다.
- `Activity` 내용이 레이아웃 리소스에서 설정 합니다. 레이아웃 이루어져 샘플 응용 프로그램에는 `LinearLayout` 를 포함 하는 `Toolbar`, 및 `FrameLayout` 에 조각의 컨테이너 역할을 합니다.
- `Toolbar` 검색 및에 대 한 작업 모음으로 설정 된 `Activity`, 작업 모음 제목 설정 됩니다.
- `PhonewordPage` Xamarin.Forms는 클래스를 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-파생 XAML에서 정의 된 페이지, 구현 되 고 변환 된 `Fragment` 를 사용 하 여는 `CreateFragment` 확장 메서드.
- `FragmentManager` 클래스를 만들고 대체 하는 트랜잭션을 커밋하는 `FrameLayout` 인스턴스를 함께 `Fragment` 에 대 한는 `PhonewordPage` 클래스입니다.

조각에 대 한 자세한 내용은 참조 [조각](~/android/platform/fragments/index.md)합니다.

> [!NOTE]
> 이외에 `CreateFragment` 확장 메서드를 Xamarin.Forms도 포함 되어는 `CreateSupportFragment` 메서드. `CreateFragment` 메서드 만듭니다는 `Android.App.Fragment` API 11을 대상으로 하는 응용 프로그램에서 사용 하는 큰 수입니다. `CreateSupportFragment` 메서드 만듭니다는 `Android.Support.V4.App.Fragment` 11 이전 API 버전을 대상으로 하는 응용 프로그램에서 사용할 수 있는 합니다.

한 번는 `OnCreate` 메서드가 실행, UI는 Xamarin.Forms에 정의 된 `PhonewordPage` 클래스 다음 스크린샷에 표시 된 것 처럼 표시 됩니다.

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png#lightbox "Android PhonewordPage")

탭 하 여 예를 들어 UI와 상호 작용 한 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), 이벤트 처리기에 발생 합니다는 `PhonewordPage` 관련 코드를 실행 합니다. 사용자가 누를 때을 예를 들어는 **호출 기록** 단추를 다음 이벤트 처리기 실행:

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

`NavigateToCallHistoryPage` 메서드 변환 하는 Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-파생 하는 페이지는 `Fragment` 와 `CreateFragment` 확장 메서드를 추가 하 고는 `Fragment` 백 스택은 조각에 합니다. UI는 Xamarin.Forms에 따라서, 정의 `CallHistoryPage` 다음 스크린샷에 표시 된 것 처럼 표시 될 됩니다.

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png#lightbox "Android CallHistoryPage")

때는 `CallHistoryPage` 뒤로 탭 표시 됩니다 팝업 화살표는 `Fragment` 에 대 한는 `CallHistoryPage` 조각 백 스택에에서 사용자를 반환는 `Fragment` 에 대 한는 `PhonewordPage` 클래스입니다.

### <a name="enabling-back-navigation-support"></a>뒤로 탐색 지원을 사용 하도록 설정

`FragmentManager` 클래스에는 `BackStackChanged` 조각 백 스택에의 콘텐츠가 변경 될 때마다 발생 하는 이벤트입니다. `OnCreate` 에서 메서드는 `MainActivity` 클래스가이 이벤트에 대 한 익명 이벤트 처리기가 포함 되어 있습니다.

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

이 이벤트 처리기 하나 이상의 있을 작업 모음에 뒤로 단추 표시 `Fragment` 조각에서 인스턴스 백 스택 합니다. 뒤로 단추를 눌러에 대 한 응답에서 처리 되는 `OnOptionsItemSelected` 재정의:

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

`OnOptionsItemSelected` 재정의 옵션 메뉴에서 항목을 선택할 때마다 호출 됩니다. 뒤로 단추를 선택 하 고 있는 하나 이상의 현재 조각 조각 뒤로 스택에서 팝 하는이 구현 `Fragment` 조각에서 인스턴스 백 스택 합니다.

### <a name="multiple-activities"></a>여러 활동

응용 프로그램은 여러 작업으로 구성 된 경우 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-파생된 페이지는 각 활동에 포함 될 수 있습니다. 이 시나리오는 `Forms.Init` 에서만 메서드를 호출 해야는 `OnCreate` 첫 번째 재정의 `Activity` 는 Xamarin.Forms를 생성 하는 `ContentPage`합니다. 그러나이 다음과 같은 영향이:

- 값 `Xamarin.Forms.Color.Accent` 에서 가져올 수는 `Activity` 를 호출한는 `Forms.Init` 메서드.
- 값 `Xamarin.Forms.Application.Current` 연관 될는 `Activity` 를 호출한는 `Forms.Init` 메서드.

### <a name="choosing-a-file"></a>파일 선택

포함 하는 경우는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-파생 페이지를 사용 하는 [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) 는 필요한 HTML "파일 선택" 지원 단추, `Activity` 재정의 해야 합니다는 `OnActivityResult` 방법:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

UWP, 네이티브에서 `App` 클래스는 일반적으로 응용 프로그램을 수행할 수 있는 곳 관련 작업을 시작 합니다. Xamarin.Forms는 일반적으로 초기화 Xamarin.Forms UWP 응용 프로그램에서의 `OnLaunched` 네이티브에서 재정의할 `App` 클래스를 전달 하는 `LaunchActivatedEventArgs` 인수에는 `Forms.Init` 메서드. 이러한 이유로 Xamarin.Forms를 사용 하는 네이티브 UWP 응용 프로그램 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-파생된 페이지 가장 쉽게 호출할 수는 `Forms.Init` 에서 메서드는 `App.OnLaunched` 메서드.

기본적으로 네이티브 `App` 를 실행 하는 클래스는 `MainPage` 첫 번째 페이지와 응용 프로그램의 클래스입니다. 다음 코드 예제는 `MainPage` 샘플 응용 프로그램에서 클래스:

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

- 페이지에 대해 캐싱을 사용할 수 있도록 새 `MainPage` 는 사용자가을 페이지에 다시 생성 되지 않습니다.
- 에 대 한 참조는 `MainPage` 클래스에 저장 되는 `static` `Instance` 필드입니다. 다른 클래스에 정의 된 메서드를 호출 하는 메커니즘을 제공 하는 것이 고 `MainPage` 클래스입니다.
- `PhonewordPage` Xamarin.Forms는 클래스를 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-파생 XAML에서 정의 된 페이지, 구현 되 고 변환는 `FrameworkElement` 를 사용 하는 `CreateFrameworkElement` 확장 메서드를 제거한 다음 설정의 내용으로는 `MainPage` 클래스입니다.

한 번는 `MainPage` 생성자가 실행, UI는 Xamarin.Forms에 정의 된 `PhonewordPage` 클래스 다음 스크린샷에 표시 된 것 처럼 표시 됩니다.

[![](native-forms-images/uwp-phonewordpage.png "UWP PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png#lightbox "UWP PhonewordPage")

탭 하 여 예를 들어 UI와 상호 작용 한 [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), 이벤트 처리기에 발생 합니다는 `PhonewordPage` 관련 코드를 실행 합니다. 사용자가 누를 때을 예를 들어는 **호출 기록** 단추를 다음 이벤트 처리기 실행:

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

UWP에서 탐색 하 여 일반적으로 수행 됩니다는 `Frame.Navigate` 사용 메서드는 `Page` 인수입니다. Xamarin.Forms 정의 `Frame.Navigate` 확장 메서드를 사용 하는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-파생 페이지 인스턴스입니다. 따라서,는 `NavigateToCallHistoryPage` 메서드가 실행 되 고 Xamarin.Forms에 정의 된 UI `CallHistoryPage` 다음 스크린샷에 표시 된 것 처럼 표시 될지 것입니다:

[![](native-forms-images/uwp-callhistorypage.png "UWP CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png#lightbox "UWP CallHistoryPage")

때는 `CallHistoryPage` 뒤로 탭 표시 됩니다 팝업 화살표는 `FrameworkElement` 에 대 한는 `CallHistoryPage` 앱에서 바로 뒤로 스택에서 사용자를 반환는 `FrameworkElement` 에 대 한는 `PhonewordPage` 클래스입니다.

### <a name="enabling-back-navigation-support"></a>뒤로 탐색 지원을 사용 하도록 설정

UWP에서 응용 프로그램 폼 팩터에 모두 다른 장치에서 모든 하드웨어 및 소프트웨어 뒤로 단추에 대 한 탐색을 활성화 해야 합니다. 이 대 한 이벤트 처리기를 등록 하 여 수행할 수 있습니다는 `BackRequested` 에서 수행할 수 있는 이벤트는 `OnLaunched` 메서드는 네이티브에서 `App` 클래스:

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

응용 프로그램을 시작할 때는 `GetForCurrentView` 메서드에서 검색 된 `SystemNavigationManager` 개체가 현재 보기와 관련 된 다음에 대 한 이벤트 처리기를 등록는 `BackRequested` 이벤트입니다. 포그라운드 응용 프로그램이 되 고 응답에서 호출 되는 경우만이 이벤트를 받는 응용 프로그램의 `OnBackRequested` 이벤트 처리기.

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

`OnBackRequested` 이벤트 처리기 호출은 `GoBack` 메서드는 응용 프로그램 및 집합의 루트 프레임에는 `BackRequestedEventArgs.Handled` 속성을 `true` 처리으로 이벤트를 표시 합니다. 처리 된 것으로 이벤트를 표시 하지 않으면 시스템 (모바일 장치 패밀리)에서 응용 프로그램에서 다른 페이지로 이동 또는 데스크톱 장치 제품군) (에서 이벤트를 무시 될 수 있습니다.

응용 프로그램이 의존 하는 휴대폰에 제공 하는 시스템 뒤로 단추 하지만 데스크톱 장치에서 제목 표시줄의 뒤로 단추 표시 여부를 선택 합니다. 설정 하 여 이렇게는 `AppViewBackButtonVisibility` 속성 중 하나에 `AppViewBackButtonVisibility` 열거형 값:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated` 대 한 응답으로 실행 되는 이벤트 처리기는 `Navigated` 페이지 탐색이 발생할 때 이벤트 발생의 제목 표시줄 뒤로 단추 표시 여부를 업데이트 합니다. 이렇게 하면 제목 표시줄의 뒤로 단추 앱 백 스택에 비어 있지 않을 경우에 표시 되는지 또는 앱에서 바로 뒤로 스택이 비어 있으면 제목 표시줄에서 제거 합니다.

UWP에서 뒤로 탐색 지원에 대 한 자세한 내용은 참조 [탐색 기록 및 뒤로 탐색 UWP 앱 용](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/)합니다.

## <a name="summary"></a>요약

Xamarin.Forms를 허용 하는 기본 형식 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-네이티브 Xamarin.iOS, Xamarin.Android, 및 유니버설 Windows 플랫폼 (UWP) 프로젝트에서 사용할 수 있도록 페이지를 파생 합니다. 네이티브 프로젝트에 사용할 수 있는 `ContentPage`-.NET 표준 라이브러리 프로젝트 또는 공유 프로젝트 또는 프로젝트에 직접 추가 된 페이지를 파생 합니다. 이 문서에서는 사용 하는 방법을 `ContentPage`-기본 프로젝트 및 탐색 하는 방법에 직접 추가 된 페이지를 파생 합니다.


## <a name="related-links"></a>관련 링크

- [NativeForms (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [네이티브 뷰](~/xamarin-forms/platform/native-views/index.md)
