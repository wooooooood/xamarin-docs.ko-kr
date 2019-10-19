---
title: Xamarin 네이티브 프로젝트의 xamarin.ios
description: 이 문서에서는 Xamarin 네이티브 프로젝트에 직접 추가 된 ContentPage 파생 페이지를 사용 하는 방법과 이러한 페이지 간에 이동 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/19/2019
ms.openlocfilehash: 0c84b844455b8a792b8cbe2f4dac97097e5ebd97
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2019
ms.locfileid: "69621054"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin 네이티브 프로젝트의 xamarin.ios

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)

일반적으로 Xamarin.ios 응용 프로그램은 [`ContentPage`](xref:Xamarin.Forms.ContentPage)에서 파생 되는 하나 이상의 페이지를 포함 하 고 이러한 페이지는 .NET Standard 라이브러리 프로젝트 또는 공유 프로젝트의 모든 플랫폼에서 공유 됩니다. 그러나 네이티브 폼을 사용 하면 `ContentPage` 파생 된 페이지를 네이티브 Xamarin.ios, Xamarin Android 및 UWP 응용 프로그램에 직접 추가할 수 있습니다. 네이티브 프로젝트가 .NET Standard 라이브러리 프로젝트 또는 공유 프로젝트에서 `ContentPage` 파생 페이지를 사용 하도록 하는 것과 비교 하 여 네이티브 프로젝트에 페이지를 직접 추가 하는 장점은 네이티브 뷰로 페이지를 확장할 수 있다는 것입니다. 그런 다음 네이티브 뷰를 `x:Name`를 사용 하 여 XAML로 명명 하 고 코드 숨김으로 참조할 수 있습니다. 네이티브 뷰에 대 한 자세한 내용은 [네이티브 뷰](~/xamarin-forms/platform/native-views/index.md)를 참조 하세요.

네이티브 프로젝트에서 Xamarin.ios [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지를 사용 하는 프로세스는 다음과 같습니다.

1. 네이티브 프로젝트에 Xamarin.ios NuGet 패키지를 추가 합니다.
1. [@No__t_1](xref:Xamarin.Forms.ContentPage)파생 페이지 및 모든 종속성을 네이티브 프로젝트에 추가 합니다.
1. `Forms.Init` 메서드를 호출합니다.
1. [@No__t_1](xref:Xamarin.Forms.ContentPage)파생 페이지의 인스턴스를 생성 하 고 다음 확장 메서드 중 하나를 사용 하 여 적절 한 네이티브 형식으로 변환 합니다. `CreateViewController` iOS의 경우, Android의 경우 `CreateSupportFragment`, UWP의 경우 `CreateFrameworkElement`입니다.
1. 네이티브 탐색 API를 사용 하 여 [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지의 네이티브 형식 표현으로 이동 합니다.

네이티브 프로젝트가 [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지를 생성 하려면 먼저 `Forms.Init` 메서드를 호출 하 여 xamarin.ios를 초기화 해야 합니다. 이 작업을 수행 하는 경우는 주로 응용 프로그램 흐름에서 가장 편리한 시기에 따라 달라 집니다. 응용 프로그램 시작 시 또는 `ContentPage` 파생 페이지가 생성 되기 직전에 수행할 수 있습니다. 이 문서와 함께 제공 되는 샘플 응용 프로그램에서는 응용 프로그램을 시작할 때 `Forms.Init` 메서드가 호출 됩니다.

> [!NOTE]
> **NativeForms** 샘플 응용 프로그램 솔루션에는 xamarin.ios 프로젝트가 포함 되어 있지 않습니다. 대신, xamarin.ios 프로젝트, Xamarin.ios 프로젝트 및 UWP 프로젝트로 구성 됩니다. 각 프로젝트는 네이티브 폼을 사용 하 여 [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지를 사용 하는 네이티브 프로젝트입니다. 그러나 네이티브 프로젝트가 .NET Standard 라이브러리 프로젝트 또는 공유 프로젝트에서 `ContentPage` 파생 페이지를 사용할 수 없는 이유는 없습니다.

네이티브 폼을 사용 하는 경우 [`DependencyService`](xref:Xamarin.Forms.DependencyService), [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)및 데이터 바인딩 엔진과 같은 Xamarin forms 기능을 계속 사용할 수 있습니다. 그러나 페이지 탐색은 네이티브 탐색 API를 사용 하 여 수행 해야 합니다.

## <a name="ios"></a>iOS

IOS에서 `AppDelegate` 클래스의 `FinishedLaunching` 재정의는 일반적으로 응용 프로그램 시작 관련 작업을 수행 하는 장소입니다. 응용 프로그램이 시작 된 후에 호출 되며 일반적으로 주 창과 뷰 컨트롤러를 구성 하기 위해 재정의 됩니다. 다음 코드 예제에서는 예제 응용 프로그램의 `AppDelegate` 클래스를 보여 줍니다.

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static AppDelegate Instance;
    UIWindow _window;
    AppNavigationController _navigation;

    public static string FolderPath { get; private set; }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        Forms.Init();

        Instance = this;
        _window = new UIWindow(UIScreen.MainScreen.Bounds);

        UINavigationBar.Appearance.SetTitleTextAttributes(new UITextAttributes
        {
            TextColor = UIColor.Black
        });

        FolderPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData));
        UIViewController mainPage = new NotesPage().CreateViewController();
        mainPage.Title = "Notes";

        _navigation = new AppNavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    // ...
}
```

@No__t_0 메서드는 다음 작업을 수행 합니다.

- Xamarin.ios는 `Forms.Init` 메서드를 호출 하 여 초기화 됩니다.
- @No__t_0 클래스에 대 한 참조는 `static` `Instance` 필드에 저장 됩니다. 이는 `AppDelegate` 클래스에 정의 된 메서드를 호출 하는 다른 클래스에 대 한 메커니즘을 제공 하기 위한 것입니다.
- 네이티브 iOS 응용 프로그램의 보기에 대 한 기본 컨테이너인 `UIWindow` 만들어집니다.
- @No__t_0 속성은 메모 데이터가 저장 되는 장치의 경로로 초기화 됩니다.
- XAML로 정의 된 Xamarin.ios [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지인 `NotesPage` 클래스는 `CreateViewController` 확장 메서드를 사용 하 여 생성 되어 `UIViewController`으로 변환 됩니다.
- @No__t_1의 `Title` 속성이 설정 되며 `UINavigationBar`에 표시 됩니다.
- 계층적 탐색을 관리 하기 위한 `AppNavigationController` 만들어집니다. 이 클래스는 `UINavigationController`에서 파생 되는 사용자 지정 탐색 컨트롤러 클래스입니다. @No__t_0 개체는 뷰 컨트롤러의 스택을 관리 하 고 생성자에 전달 된 `UIViewController`는 처음에 `AppNavigationController` 로드 될 때 표시 됩니다.
- @No__t_0 개체는 `UIWindow`에 대 한 최상위 `UIViewController`로 설정 되 고 `UIWindow`는 응용 프로그램에 대 한 키 창으로 설정 되며 표시 됩니다.

@No__t_0 메서드를 실행 하면 다음 스크린샷에 표시 된 것 처럼 Xamarin.ios `NotesPage` 클래스에 정의 된 UI가 표시 됩니다.

[![XAML에 정의 된 UI를 사용 하는 Xamarin.ios 응용 프로그램의 스크린샷](native-forms-images/ios-notespage.png "XAML UI를 사용 하는 xamarin.ios 앱")](native-forms-images/ios-notespage-large.png#lightbox "XAML UI를 사용 하는 xamarin.ios 앱")

예를 들어 **+** [`Button`](xref:Xamarin.Forms.Button)를 탭 하 여 UI와 상호 작용 하면 코드를 실행 하는 `NotesPage` 코드에 다음과 같은 이벤트 처리기가 생성 됩니다.

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToNoteEntryPage(new Note());
}
```

@No__t_0 `AppDelegate.Instance` 필드를 사용 하 여 다음 코드 예제와 같이 `AppDelegate.NavigateToNoteEntryPage` 메서드를 호출할 수 있습니다.

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    var noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateViewController();
    noteEntryPage.Title = "Note Entry";
    _navigation.PushViewController(noteEntryPage, true);
}
```

@No__t_0 메서드는 `CreateViewController` 확장 메서드를 사용 하 여 Xamarin.ios [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지를 `UIViewController` 변환 하 고 `Title`의 `UIViewController` 속성을 설정 합니다. 그런 다음 `PushViewController` 메서드에서 `UIViewController` `AppNavigationController`에 푸시됩니다. 따라서 다음 스크린샷에 표시 된 것 처럼 Xamarin.ios `NoteEntryPage` 클래스에 정의 된 UI가 표시 됩니다.

[![XAML에 정의 된 UI를 사용 하는 Xamarin.ios 응용 프로그램의 스크린샷](native-forms-images/ios-noteentrypage.png "XAML UI를 사용 하는 xamarin.ios 앱")](native-forms-images/ios-noteentrypage-large.png#lightbox "XAML UI를 사용 하는 xamarin.ios 앱")

@No__t_0 표시 되 면 뒤로 탐색은 `AppNavigationController`에서 `NoteEntryPage` 클래스에 대 한 `UIViewController`를 팝 하 고 사용자를 `UIViewController` 클래스의 `NotesPage`로 반환 합니다. 그러나 iOS 네이티브 탐색 스택에서 `UIViewController`를 팝 하면 `UIViewController` 및 연결 된 `Page` 개체가 자동으로 삭제 되지 않습니다. 따라서 `AppNavigationController` 클래스는 `PopViewController` 메서드를 재정의 하 여 역방향 탐색 시 뷰 컨트롤러를 삭제 합니다.

```csharp
public class AppNavigationController : UINavigationController
{
    //...
    public override UIViewController PopViewController(bool animated)
    {
        UIViewController topView = TopViewController;
        if (topView != null)
        {
            // Dispose of ViewController on back navigation.
            topView.Dispose();
        }
        return base.PopViewController(animated);
    }
}
```

@No__t_0 재정의는 iOS 네이티브 탐색 스택에서 팝 된 `UIViewController` 개체의 `Dispose` 메서드를 호출 합니다. 이 작업을 수행 하지 못하면 `UIViewController` 및 연결 된 `Page` 개체가 분리 됩니다.

> [!IMPORTANT]
> 분리 된 개체는 가비지 수집 될 수 없으므로 메모리 누수가 발생 합니다.

## <a name="android"></a>Android

Android에서 `MainActivity` 클래스의 `OnCreate` 재정의는 일반적으로 응용 프로그램 시작 관련 작업을 수행할 수 있습니다. 다음 코드 예제에서는 예제 응용 프로그램의 `MainActivity` 클래스를 보여 줍니다.

```csharp
public class MainActivity : AppCompatActivity
{
    public static string FolderPath { get; private set; }

    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Notes";

        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        Android.Support.V4.App.Fragment mainPage = new NotesPage().CreateSupportFragment(this);
        SupportFragmentManager
            .BeginTransaction()
            .Replace(Resource.Id.fragment_frame_layout, mainPage)
            .Commit();
        ...
    }
    ...
}
```

@No__t_0 메서드는 다음 작업을 수행 합니다.

- Xamarin.ios는 `Forms.Init` 메서드를 호출 하 여 초기화 됩니다.
- @No__t_0 클래스에 대 한 참조는 `static` `Instance` 필드에 저장 됩니다. 이는 `MainActivity` 클래스에 정의 된 메서드를 호출 하는 다른 클래스에 대 한 메커니즘을 제공 하기 위한 것입니다.
- @No__t_0 콘텐츠는 레이아웃 리소스에서 설정 됩니다. 샘플 응용 프로그램에서 레이아웃은 `Toolbar` 포함 된 `LinearLayout`와 조각 컨테이너 역할을 수행할 `FrameLayout` 구성 됩니다.
- @No__t_0 검색 되어 `Activity`의 작업 모음으로 설정 되 고 작업 모음 제목이 설정 됩니다.
- @No__t_0 속성은 메모 데이터가 저장 되는 장치의 경로로 초기화 됩니다.
- XAML로 정의 된 Xamarin.ios [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지인 `NotesPage` 클래스는 `CreateSupportFragment` 확장 메서드를 사용 하 여 생성 되어 `Fragment`으로 변환 됩니다.
- @No__t_0 클래스는 `FrameLayout` 인스턴스를 `NotesPage` 클래스에 대 한 `Fragment`로 바꾸는 트랜잭션을 만들고 커밋합니다.

조각에 대 한 자세한 내용은 [조각](~/android/platform/fragments/index.md)을 참조 하세요.

@No__t_0 메서드를 실행 하면 다음 스크린샷에 표시 된 것 처럼 Xamarin.ios `NotesPage` 클래스에 정의 된 UI가 표시 됩니다.

[![XAML에 정의 된 UI를 사용 하는 Xamarin Android 응용 프로그램의 스크린샷](native-forms-images/android-notespage.png "XAML UI를 사용 하는 Xamarin Android 앱")](native-forms-images/android-notespage-large.png#lightbox "XAML UI를 사용 하는 Xamarin Android 앱")

예를 들어 **+** [`Button`](xref:Xamarin.Forms.Button)를 탭 하 여 UI와 상호 작용 하면 코드를 실행 하는 `NotesPage` 코드에 다음과 같은 이벤트 처리기가 생성 됩니다.

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToNoteEntryPage(new Note());
}
```

@No__t_0 `MainActivity.Instance` 필드를 사용 하 여 다음 코드 예제와 같이 `MainActivity.NavigateToNoteEntryPage` 메서드를 호출할 수 있습니다.

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    Android.Support.V4.App.Fragment noteEntryPage = new NoteEntryPage
    {
        BindingContext = note
    }.CreateSupportFragment(this);
    SupportFragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, noteEntryPage)
        .Commit();
}
```

@No__t_0 메서드는 `CreateSupportFragment` 확장 메서드를 사용 하 여 Xamarin.ios [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지를 `Fragment` 변환 하 고 조각 조각 스택에 `Fragment`를 추가 합니다. 따라서 다음 스크린샷에 표시 된 것 처럼 Xamarin.ios `NoteEntryPage`에 정의 된 UI가 표시 됩니다.

[![XAML에 정의 된 UI를 사용 하는 Xamarin Android 응용 프로그램의 스크린샷](native-forms-images/android-noteentrypage.png "XAML UI를 사용 하는 Xamarin Android 앱")](native-forms-images/android-noteentrypage-large.png#lightbox "XAML UI를 사용 하는 Xamarin Android 앱")

@No__t_0 표시 되 면 뒤로 화살표를 눌러 조각 뒤로 스택에서 `NoteEntryPage`에 대 한 `Fragment`를 팝 하 고 사용자를 `NotesPage` 클래스의 `Fragment` 반환 합니다.

### <a name="enable-back-navigation-support"></a>뒤로 탐색 지원 사용

@No__t_0 클래스에는 조각 다시 스택 내용이 변경 될 때마다 발생 하는 `BackStackChanged` 이벤트가 있습니다. @No__t_1 클래스의 `OnCreate` 메서드는이 이벤트에 대 한 익명 이벤트 처리기를 포함 합니다.

```csharp
SupportFragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = SupportFragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Note Entry" : "Notes";
};
```

이 이벤트 처리기는 조각 모음에 하나 이상의 `Fragment` 인스턴스가 있는 경우 작업 모음에 뒤로 단추를 표시 합니다. 뒤로 단추를 누르기 위한 응답은 `OnOptionsItemSelected` 재정의에 의해 처리 됩니다.

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && SupportFragmentManager.BackStackEntryCount > 0)
    {
        SupportFragmentManager.PopBackStack();
        return true;
    }
    return base.OnOptionsItemSelected(item);
}
```

@No__t_0 재정의는 옵션 메뉴의 항목이 선택 될 때마다 호출 됩니다. 이 구현에서는 뒤로 단추가 선택 되었고 조각 뒤로 스택에 `Fragment` 인스턴스가 하나 이상 있는 경우 조각 뒤로 스택에서 현재 조각을 팝 합니다.

### <a name="multiple-activities"></a>여러 작업

응용 프로그램이 여러 작업으로 구성 된 경우에는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지를 각 작업에 포함할 수 있습니다. 이 시나리오에서 `Forms.Init` 메서드는 Xamarin.ios `ContentPage`를 포함 하는 첫 번째 `Activity`의 `OnCreate` 재정의 에서만 호출 해야 합니다. 그러나 다음과 같은 영향을 미칩니다.

- @No__t_0 값은 `Forms.Init` 메서드를 호출한 `Activity`에서 가져옵니다.
- @No__t_0의 값은 `Forms.Init` 메서드를 호출한 `Activity`와 연결 됩니다.

### <a name="choose-a-file"></a>파일 선택

HTML "파일 선택" 단추를 지원 해야 하는 [`WebView`](xref:Xamarin.Forms.WebView) 를 사용 하는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지를 포함 하는 경우 `Activity` `OnActivityResult` 메서드를 재정의 해야 합니다.

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

UWP에서 네이티브 `App` 클래스는 일반적으로 응용 프로그램 시작 관련 작업을 수행 하는 장소입니다. 일반적으로 xamarin.ios는 네이티브 `App` 클래스의 `OnLaunched` 재정의에서 `LaunchActivatedEventArgs` 인수를 `Forms.Init` 메서드에 전달 하기 위해 Xamarin. Forms UWP 응용 프로그램에서 초기화 됩니다. 이러한 이유로 Xamarin.ios [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지를 사용 하는 네이티브 UWP 응용 프로그램은 `App.OnLaunched` 메서드에서 `Forms.Init` 메서드를 가장 쉽게 호출할 수 있습니다.

기본적으로 네이티브 `App` 클래스는 응용 프로그램의 첫 번째 페이지로 `MainPage` 클래스를 시작 합니다. 다음 코드 예제에서는 예제 응용 프로그램의 `MainPage` 클래스를 보여 줍니다.

```csharp
public sealed partial class MainPage : Page
{
    public static MainPage Instance;

    public static string FolderPath { get; private set; }

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        this.Content = new Notes.UWP.Views.NotesPage().CreateFrameworkElement();
    }
    ...
}
```

@No__t_0 생성자는 다음 작업을 수행 합니다.

- 페이지에 캐싱을 사용할 수 있으므로 사용자가 페이지를 다시 탐색할 때 새 `MainPage` 생성 되지 않습니다.
- @No__t_0 클래스에 대 한 참조는 `static` `Instance` 필드에 저장 됩니다. 이는 `MainPage` 클래스에 정의 된 메서드를 호출 하는 다른 클래스에 대 한 메커니즘을 제공 하기 위한 것입니다.
- @No__t_0 속성은 메모 데이터가 저장 되는 장치의 경로로 초기화 됩니다.
- XAML로 정의 된 Xamarin.ios [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지인 `NotesPage` 클래스는 `CreateFrameworkElement` 확장 메서드를 사용 하 여 생성 되 고 `FrameworkElement` 변환 된 다음 `MainPage` 클래스의 콘텐츠로 설정 됩니다.

@No__t_0 생성자를 실행 한 후에는 다음 스크린샷에 표시 된 것 처럼 Xamarin.ios `NotesPage` 클래스에 정의 된 UI가 표시 됩니다.

[![Xamarin. Forms XAML로 정의 된 UI를 사용 하는 UWP 응용 프로그램의 스크린샷](native-forms-images/uwp-notespage.png "Xamarin Forms XAML UI를 사용 하는 UWP 앱")](native-forms-images/uwp-notespage-large.png#lightbox "Xamarin Forms XAML UI를 사용 하는 UWP 앱")

예를 들어 **+** [`Button`](xref:Xamarin.Forms.Button)를 탭 하 여 UI와 상호 작용 하면 코드를 실행 하는 `NotesPage` 코드에 다음과 같은 이벤트 처리기가 생성 됩니다.

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainPage.Instance.NavigateToNoteEntryPage(new Note());
}
```

@No__t_0 `MainPage.Instance` 필드를 사용 하 여 다음 코드 예제와 같이 `MainPage.NavigateToNoteEntryPage` 메서드를 호출할 수 있습니다.

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    this.Frame.Navigate(new NoteEntryPage
    {
        BindingContext = note
    });
}
```

UWP의 탐색은 일반적으로 `Page` 인수를 사용 하는 `Frame.Navigate` 메서드를 사용 하 여 수행 됩니다. Xamarin.ios는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지 인스턴스를 사용 하는 `Frame.Navigate` 확장 메서드를 정의 합니다. 따라서 `NavigateToNoteEntryPage` 메서드가 실행 될 때 다음 스크린샷에 표시 된 것 처럼 Xamarin.ios `NoteEntryPage`에 정의 된 UI가 표시 됩니다.

[![Xamarin. Forms XAML로 정의 된 UI를 사용 하는 UWP 응용 프로그램의 스크린샷](native-forms-images/uwp-noteentrypage.png "Xamarin Forms XAML UI를 사용 하는 UWP 앱")](native-forms-images/uwp-noteentrypage-large.png#lightbox "Xamarin Forms XAML UI를 사용 하는 UWP 앱")

@No__t_0 표시 되 면 뒤로 화살표를 탭 하 여 앱 내 콜백에서 `NoteEntryPage`에 대 한 `FrameworkElement`를 팝 하 고 사용자를 `NotesPage` 클래스의 `FrameworkElement` 반환 합니다.

### <a name="enable-back-navigation-support"></a>뒤로 탐색 지원 사용

UWP에서 응용 프로그램은 여러 장치 폼 팩터에서 모든 하드웨어 및 소프트웨어 뒤로 단추에 대해 다시 탐색을 사용 하도록 설정 해야 합니다. 이렇게 하려면 네이티브 `App` 클래스의 `OnLaunched` 메서드에서 수행할 수 있는 `BackRequested` 이벤트에 대 한 이벤트 처리기를 등록 합니다.

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

응용 프로그램이 시작 되 면 `GetForCurrentView` 메서드는 현재 뷰와 연결 된 `SystemNavigationManager` 개체를 검색 한 다음 `BackRequested` 이벤트에 대 한 이벤트 처리기를 등록 합니다. 응용 프로그램은 포그라운드 응용 프로그램 인 경우에만이 이벤트를 수신 하 고, 응답에서 `OnBackRequested` 이벤트 처리기를 호출 합니다.

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

@No__t_0 이벤트 처리기는 응용 프로그램의 루트 프레임에서 `GoBack` 메서드를 호출 하 고 `BackRequestedEventArgs.Handled` 속성을 `true`로 설정 하 여 이벤트를 처리 된 것으로 표시 합니다. 이벤트를 처리 된 것으로 표시 하지 못하면 이벤트를 무시 하 게 됩니다.

응용 프로그램은 제목 표시줄에 뒤로 단추를 표시할지 여부를 선택 합니다. 이렇게 하려면 `AppViewBackButtonVisibility` 속성을 `AppViewBackButtonVisibility` 열거형 값 중 하나로 설정 합니다.

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

@No__t_1 이벤트 발생에 대 한 응답으로 실행 되는 `OnNavigated` 이벤트 처리기는 페이지 탐색이 발생할 때 제목 표시줄 뒤로 단추의 표시 여부를 업데이트 합니다. 이렇게 하면 앱 내 백오프 스택이 비어 있지 않거나 앱 내 백오프 스택이 비어 있는 경우 제목 표시줄에서 제거 되 면 제목 표시줄 뒤로 단추가 표시 됩니다.

UWP의 후방 탐색 지원에 대 한 자세한 내용은 [탐색 기록 및 uwp 앱에 대 한 역방향 탐색](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [NativeForms (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)
- [네이티브 뷰](~/xamarin-forms/platform/native-views/index.md)
