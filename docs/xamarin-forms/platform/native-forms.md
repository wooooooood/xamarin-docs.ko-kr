---
title: Xamarin.FormsXamarin 네이티브 프로젝트에서
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9fb741a03d1c8dd2a8754120d0b46567d8889a0b
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84132277"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin.FormsXamarin 네이티브 프로젝트에서

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)

일반적으로 Xamarin.Forms 응용 프로그램에는에서 파생 되는 하나 이상의 페이지가 포함 되어 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 있으며, 이러한 페이지는 .NET Standard 라이브러리 프로젝트 또는 공유 프로젝트의 모든 플랫폼에서 공유 됩니다. 그러나 네이티브 폼을 사용 하면 `ContentPage` 파생 된 페이지를 네이티브 xamarin.ios, Xamarin Android 및 UWP 응용 프로그램에 직접 추가할 수 있습니다. 네이티브 프로젝트를 사용 하 여 네이티브 프로젝트 .NET Standard에서 파생 페이지를 사용 하는 것과 비교 하 여 네이티브 프로젝트에 페이지를 직접 추가 하는 경우 `ContentPage` 기본 뷰로 페이지를 확장할 수 있다는 이점이 있습니다. 그런 다음 네이티브 뷰를 XAML로 명명 하 `x:Name` 고 코드 숨김으로 참조할 수 있습니다. 네이티브 뷰에 대 한 자세한 내용은 [네이티브 뷰](~/xamarin-forms/platform/native-views/index.md)를 참조 하세요.

Xamarin.Forms네이티브 프로젝트에서 파생 된 페이지를 사용 하는 프로세스는 다음과 같습니다 [`ContentPage`](xref:Xamarin.Forms.ContentPage) .

1. Xamarin.Forms네이티브 프로젝트에 NuGet 패키지를 추가 합니다.
1. [`ContentPage`](xref:Xamarin.Forms.ContentPage)파생 페이지 및 모든 종속성을 네이티브 프로젝트에 추가 합니다.
1. `Forms.Init` 메서드를 호출합니다.
1. 파생 페이지의 인스턴스를 생성 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 하 여 `CreateViewController` IOS, `CreateSupportFragment` Android 또는 UWP 용 확장 메서드 중 하나를 사용 하 여 적절 한 네이티브 형식으로 변환 합니다 `CreateFrameworkElement` .
1. [`ContentPage`](xref:Xamarin.Forms.ContentPage)네이티브 탐색 API를 사용 하 여 파생 페이지의 네이티브 형식 표현으로 이동 합니다.

Xamarin.Forms`Forms.Init`네이티브 프로젝트가 파생 페이지를 생성 하기 전에 메서드를 호출 하 여을 초기화 해야 합니다 [`ContentPage`](xref:Xamarin.Forms.ContentPage) . 이 작업을 수행 하는 경우는 주로 응용 프로그램 흐름에서 가장 편리한 시기에 따라 달라 집니다. 응용 프로그램 시작 시 또는 파생 페이지가 생성 되기 직전에 수행할 수 있습니다 `ContentPage` . 이 문서와 함께 제공 되는 샘플 응용 프로그램에서 `Forms.Init` 메서드는 응용 프로그램 시작 시 호출 됩니다.

> [!NOTE]
> **NativeForms** 샘플 응용 프로그램 솔루션에는 프로젝트가 포함 되어 있지 않습니다 Xamarin.Forms . 대신, xamarin.ios 프로젝트, Xamarin.ios 프로젝트 및 UWP 프로젝트로 구성 됩니다. 각 프로젝트는 파생 페이지를 사용 하기 위해 네이티브 폼을 사용 하는 네이티브 프로젝트입니다 [`ContentPage`](xref:Xamarin.Forms.ContentPage) . 그러나 네이티브 프로젝트가 `ContentPage` .NET Standard 라이브러리 프로젝트 또는 공유 프로젝트에서 파생 된 페이지를 사용할 수 없는 이유는 없습니다.

네이티브 폼을 사용 하는 경우 Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) , [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 및 데이터 바인딩 엔진과 같은 기능을 모두 계속 사용할 수 있습니다. 그러나 페이지 탐색은 네이티브 탐색 API를 사용 하 여 수행 해야 합니다.

## <a name="ios"></a>iOS

IOS에서 `FinishedLaunching` 클래스의 재정의는 `AppDelegate` 일반적으로 응용 프로그램 시작 관련 작업을 수행 하는 장소입니다. 응용 프로그램이 시작 된 후에 호출 되며 일반적으로 주 창과 뷰 컨트롤러를 구성 하기 위해 재정의 됩니다. 다음 코드 예제에서는 `AppDelegate` 예제 응용 프로그램의 클래스를 보여 줍니다.

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

`FinishedLaunching` 메서드에서 수행하는 작업은 다음과 같습니다.

- Xamarin.Forms는 메서드를 호출 하 여 초기화 됩니다 `Forms.Init` .
- 클래스에 대 한 참조는 `AppDelegate` 필드에 저장 됩니다 `static` `Instance` . 이는 클래스에 정의 된 메서드를 호출 하는 다른 클래스에 대 한 메커니즘을 제공 하기 위한 것입니다 `AppDelegate` .
- `UIWindow`네이티브 iOS 응용 프로그램의 보기에 대 한 기본 컨테이너인가 만들어집니다.
- `FolderPath`속성은 메모 데이터가 저장 되는 장치의 경로로 초기화 됩니다.
- `NotesPage` Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) XAML에 정의 된 파생 페이지인 클래스는 `UIViewController` 확장 메서드를 사용 하 여 생성 되 고로 변환 됩니다 `CreateViewController` .
- 에 `Title` 표시 되는의 속성 `UIViewController` 을 설정 합니다 `UINavigationBar` .
- `AppNavigationController`계층적 탐색을 관리 하기 위해가 만들어집니다. 이 클래스는에서 파생 되는 사용자 지정 탐색 컨트롤러 클래스입니다 `UINavigationController` . `AppNavigationController`개체는 뷰 컨트롤러의 스택을 관리 하 고 생성자에 `UIViewController` 전달 된는가 로드 될 때 처음에 표시 됩니다 `AppNavigationController` .
- `AppNavigationController`개체가의 최상위 수준으로 설정 되 `UIViewController` `UIWindow` 고 `UIWindow` 이 응용 프로그램에 대 한 키 창으로 설정 되 고 표시 됩니다.

메서드가 실행 된 후에는 `FinishedLaunching` Xamarin.Forms `NotesPage` 다음 스크린샷에 표시 된 것 처럼 클래스에 정의 된 UI가 표시 됩니다.

[![XAML에 정의 된 UI를 사용 하는 Xamarin.ios 응용 프로그램의 스크린샷](native-forms-images/ios-notespage.png "XAML UI를 사용 하는 xamarin.ios 앱")](native-forms-images/ios-notespage-large.png#lightbox "XAML UI를 사용 하는 xamarin.ios 앱")

UI와 상호 작용 하는 예를 들어를 탭 하면 **+** [`Button`](xref:Xamarin.Forms.Button) 코드를 실행 하는 동안 다음 이벤트 처리기가 발생 합니다 `NotesPage` .

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `AppDelegate.Instance` 필드를 사용 `AppDelegate.NavigateToNoteEntryPage` 하 여 다음 코드 예제와 같이 메서드를 호출할 수 있습니다.

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

`NavigateToNoteEntryPage`메서드는 Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) 확장명 메서드를 사용 하 여 파생 페이지를로 변환 하 `UIViewController` `CreateViewController` 고 `Title` 의 속성을 설정 합니다 `UIViewController` . `UIViewController`그런 다음 `AppNavigationController` 메서드가로 푸시됩니다 `PushViewController` . 따라서 Xamarin.Forms `NoteEntryPage` 다음 스크린샷에 표시 된 것 처럼 클래스에 정의 된 UI가 표시 됩니다.

[![XAML에 정의 된 UI를 사용 하는 Xamarin.ios 응용 프로그램의 스크린샷](native-forms-images/ios-noteentrypage.png "XAML UI를 사용 하는 xamarin.ios 앱")](native-forms-images/ios-noteentrypage-large.png#lightbox "XAML UI를 사용 하는 xamarin.ios 앱")

`NoteEntryPage`이 표시 되 면 후방 탐색이의 `UIViewController` 클래스에 대 한를 팝 하 여 `NoteEntryPage` 클래스에 `AppNavigationController` 대해 사용자를로 반환 합니다 `UIViewController` `NotesPage` . 그러나 `UIViewController` iOS 네이티브 탐색 스택에서를 팝 해도 및 연결 된 개체는 자동으로 삭제 되지 않습니다 `UIViewController` `Page` . 따라서 클래스는 `AppNavigationController` `PopViewController` 메서드를 재정의 하 여 역방향 탐색 시 뷰 컨트롤러를 삭제 합니다.

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

`PopViewController`재정의는 `Dispose` `UIViewController` iOS 네이티브 탐색 스택에서 팝 된 개체에 대해 메서드를 호출 합니다. 이 작업을 수행 하지 못하면 `UIViewController` 및 연결 된 `Page` 개체가 분리 됩니다.

> [!IMPORTANT]
> 분리 된 개체는 가비지 수집 될 수 없으므로 메모리 누수가 발생 합니다.

## <a name="android"></a>Android

Android에서 `OnCreate` 클래스의 재정의는 `MainActivity` 일반적으로 응용 프로그램 시작 관련 작업을 수행 하는 장소입니다. 다음 코드 예제에서는 `MainActivity` 예제 응용 프로그램의 클래스를 보여 줍니다.

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

`OnCreate` 메서드에서 수행하는 작업은 다음과 같습니다.

- Xamarin.Forms는 메서드를 호출 하 여 초기화 됩니다 `Forms.Init` .
- 클래스에 대 한 참조는 `MainActivity` 필드에 저장 됩니다 `static` `Instance` . 이는 클래스에 정의 된 메서드를 호출 하는 다른 클래스에 대 한 메커니즘을 제공 하기 위한 것입니다 `MainActivity` .
- `Activity`콘텐츠는 레이아웃 리소스에서 설정 됩니다. 샘플 응용 프로그램에서 레이아웃은를 `LinearLayout` 포함 하는 `Toolbar` 와 `FrameLayout` 조각 컨테이너 역할을 하는로 구성 됩니다.
- `Toolbar`가 검색 되어에 대 한 작업 모음으로 설정 되 `Activity` 고 작업 모음 제목이 설정 됩니다.
- `FolderPath`속성은 메모 데이터가 저장 되는 장치의 경로로 초기화 됩니다.
- `NotesPage` Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) XAML에 정의 된 파생 페이지인 클래스는 `Fragment` 확장 메서드를 사용 하 여 생성 되 고로 변환 됩니다 `CreateSupportFragment` .
- `SupportFragmentManager`클래스는 `FrameLayout` 인스턴스를 `Fragment` 클래스에 대 한로 바꾸는 트랜잭션을 만들고 커밋합니다 `NotesPage` .

조각에 대 한 자세한 내용은 [조각](~/android/platform/fragments/index.md)을 참조 하세요.

메서드가 실행 된 후에는 `OnCreate` Xamarin.Forms `NotesPage` 다음 스크린샷에 표시 된 것 처럼 클래스에 정의 된 UI가 표시 됩니다.

[![XAML에 정의 된 UI를 사용 하는 Xamarin Android 응용 프로그램의 스크린샷](native-forms-images/android-notespage.png "XAML UI를 사용 하는 Xamarin Android 앱")](native-forms-images/android-notespage-large.png#lightbox "XAML UI를 사용 하는 Xamarin Android 앱")

UI와 상호 작용 하는 예를 들어를 탭 하면 **+** [`Button`](xref:Xamarin.Forms.Button) 코드를 실행 하는 동안 다음 이벤트 처리기가 발생 합니다 `NotesPage` .

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `MainActivity.Instance` 필드를 사용 `MainActivity.NavigateToNoteEntryPage` 하 여 다음 코드 예제와 같이 메서드를 호출할 수 있습니다.

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

`NavigateToNoteEntryPage`메서드는 확장명 메서드를 사용 하 여 파생 페이지를로 변환 하 고를 Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) `Fragment` `CreateSupportFragment` `Fragment` 조각 백 스택에 추가 합니다. 따라서 Xamarin.Forms `NoteEntryPage` 다음 스크린샷에 표시 된 것 처럼에 정의 된 UI가 표시 됩니다.

[![XAML에 정의 된 UI를 사용 하는 Xamarin Android 응용 프로그램의 스크린샷](native-forms-images/android-noteentrypage.png "XAML UI를 사용 하는 Xamarin Android 앱")](native-forms-images/android-noteentrypage-large.png#lightbox "XAML UI를 사용 하는 Xamarin Android 앱")

`NoteEntryPage`이 표시 되 면 뒤로 화살표를 누르면 `Fragment` `NoteEntryPage` 조각 뒤로 스택의가 표시 되 고 사용자가 `Fragment` 클래스에 대 한로 반환 됩니다 `NotesPage` .

### <a name="enable-back-navigation-support"></a>뒤로 탐색 지원 사용

클래스에는 `SupportFragmentManager` `BackStackChanged` 조각 다시 스택 내용이 변경 될 때마다 발생 하는 이벤트가 있습니다. `OnCreate`클래스의 메서드는 `MainActivity` 이 이벤트에 대 한 익명 이벤트 처리기를 포함 합니다.

```csharp
SupportFragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = SupportFragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Note Entry" : "Notes";
};
```

이 이벤트 처리기는 조각 뒤로 스택에 하나 이상의 인스턴스가 있는 경우 작업 모음에 뒤로 단추를 표시 합니다 `Fragment` . 뒤로 단추를 누르기 위한 응답은 재정의에 의해 처리 됩니다 `OnOptionsItemSelected` .

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

`OnOptionsItemSelected`재정의는 옵션 메뉴의 항목이 선택 될 때마다 호출 됩니다. 이 구현은 뒤로 단추가 선택 되었고 조각 뒤로 스택에 하나 이상의 인스턴스가 있는 경우 조각 뒤로 스택에서 현재 조각을 팝 합니다 `Fragment` .

### <a name="multiple-activities"></a>여러 작업

응용 프로그램이 여러 작업으로 구성 된 경우에는 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 각 작업에 파생 페이지를 포함할 수 있습니다. 이 시나리오에서는 `Forms.Init` `OnCreate` `Activity` 를 포함 하는 첫 번째의 재정의 에서만 메서드를 호출 해야 합니다 Xamarin.Forms `ContentPage` . 그러나 다음과 같은 영향을 미칩니다.

- 의 값은 `Xamarin.Forms.Color.Accent` `Activity` 메서드를 호출한에서 가져옵니다 `Forms.Init` .
- 의 값은 `Xamarin.Forms.Application.Current` `Activity` 메서드를 호출한와 연결 됩니다 `Forms.Init` .

### <a name="choose-a-file"></a>파일 선택

[`ContentPage`](xref:Xamarin.Forms.ContentPage) [`WebView`](xref:Xamarin.Forms.WebView) HTML "파일 선택" 단추를 지원 해야 하는를 사용 하는 파생 페이지를 포함 하는 경우는 `Activity` 메서드를 재정의 해야 합니다 `OnActivityResult` .

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

UWP에서 네이티브 클래스는 `App` 일반적으로 응용 프로그램 시작 관련 작업을 수행 하는 장소입니다. Xamarin.Forms는 일반적으로 Xamarin.Forms UWP 응용 프로그램에서 `OnLaunched` 네이티브 클래스의 재정의에서 초기화 되어 `App` `LaunchActivatedEventArgs` 메서드에 인수를 전달 `Forms.Init` 합니다. 이러한 이유로 파생 페이지를 사용 하는 네이티브 UWP 응용 프로그램은 Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) 메서드에서 메서드를 가장 쉽게 호출할 수 있습니다 `Forms.Init` `App.OnLaunched` .

기본적으로 네이티브 클래스는 `App` `MainPage` 응용 프로그램의 첫 번째 페이지로 클래스를 시작 합니다. 다음 코드 예제에서는 `MainPage` 예제 응용 프로그램의 클래스를 보여 줍니다.

```csharp
public sealed partial class MainPage : Page
{
    NotesPage notesPage;
    NoteEntryPage noteEntryPage;
    
    public static MainPage Instance;
    public static string FolderPath { get; private set; }

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        FolderPath = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.LocalApplicationData));
        notesPage = new Notes.UWP.Views.NotesPage();
        this.Content = notesPage.CreateFrameworkElement();
        // ...        
    } 
    // ...
}
```

`MainPage`생성자는 다음 작업을 수행 합니다.

- 페이지에 캐싱을 사용할 수 있으므로 `MainPage` 사용자가 페이지를 다시 탐색할 때 새가 생성 되지 않습니다.
- 클래스에 대 한 참조는 `MainPage` 필드에 저장 됩니다 `static` `Instance` . 이는 클래스에 정의 된 메서드를 호출 하는 다른 클래스에 대 한 메커니즘을 제공 하기 위한 것입니다 `MainPage` .
- `FolderPath`속성은 메모 데이터가 저장 되는 장치의 경로로 초기화 됩니다.
- `NotesPage` Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) XAML에 정의 된 파생 페이지인 클래스는 확장 메서드를 사용 하 여 생성 되 고로 변환 된 `FrameworkElement` `CreateFrameworkElement` 다음 클래스의 콘텐츠로 설정 됩니다 `MainPage` .

생성자가 실행 된 후에는 `MainPage` Xamarin.Forms `NotesPage` 다음 스크린샷에 표시 된 것 처럼 클래스에 정의 된 UI가 표시 됩니다.

[![XAML로 정의 된 UI를 사용 하는 UWP 응용 프로그램의 스크린샷 Xamarin.Forms](native-forms-images/uwp-notespage.png "UWP 앱에서 [! OP. NO-LOC (Xamarin.ios)] XAML UI")](native-forms-images/uwp-notespage-large.png#lightbox "UWP 앱에서 [! OP. NO-LOC (Xamarin.ios)] XAML UI")

UI와 상호 작용 하는 예를 들어를 탭 하면 **+** [`Button`](xref:Xamarin.Forms.Button) 코드를 실행 하는 동안 다음 이벤트 처리기가 발생 합니다 `NotesPage` .

```csharp
void OnNoteAddedClicked(object sender, EventArgs e)
{
    MainPage.Instance.NavigateToNoteEntryPage(new Note());
}
```

`static` `MainPage.Instance` 필드를 사용 `MainPage.NavigateToNoteEntryPage` 하 여 다음 코드 예제와 같이 메서드를 호출할 수 있습니다.

```csharp
public void NavigateToNoteEntryPage(Note note)
{
    noteEntryPage = new Notes.UWP.Views.NoteEntryPage
    {
        BindingContext = note
    };
    this.Frame.Navigate(noteEntryPage);
}
```

UWP의 탐색은 일반적으로 인수를 사용 하는 메서드를 사용 하 여 수행 됩니다 `Frame.Navigate` `Page` . Xamarin.Forms`Frame.Navigate`파생 페이지 인스턴스를 사용 하는 확장 메서드를 정의 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 합니다. 따라서 `NavigateToNoteEntryPage` 메서드가 실행 될 때 Xamarin.Forms `NoteEntryPage` 다음 스크린샷에 표시 된 것 처럼에 정의 된 UI가 표시 됩니다.

[![XAML로 정의 된 UI를 사용 하는 UWP 응용 프로그램의 스크린샷 Xamarin.Forms](native-forms-images/uwp-noteentrypage.png "UWP 앱에서 [! OP. NO-LOC (Xamarin.ios)] XAML UI")](native-forms-images/uwp-noteentrypage-large.png#lightbox "UWP 앱에서 [! OP. NO-LOC (Xamarin.ios)] XAML UI")

`NoteEntryPage`이 표시 되 면 뒤로 화살표를 눌러 `FrameworkElement` `NoteEntryPage` 앱 내 백오프 스택의를 팝 하 고 사용자를 `FrameworkElement` 클래스에 대 한로 반환 합니다 `NotesPage` .

### <a name="enable-page-resizing-support"></a>페이지 크기 조정 지원 사용

UWP 응용 프로그램 창의 크기를 조정 하는 경우에 Xamarin.Forms 도 콘텐츠의 크기를 조정 해야 합니다. 이 작업을 수행 하려면 생성자에서 이벤트에 대 한 이벤트 처리기 `Loaded` 를 등록 합니다 `MainPage` .

```csharp
public MainPage()
{
    // ...
    this.Loaded += OnMainPageLoaded;
    // ...
}
```

`Loaded`이 이벤트는 페이지가 레이아웃 되 고 렌더링 되 고 상호 작용할 준비가 되 면 발생 하 고 응답 하 여 메서드를 실행 합니다 `OnMainPageLoaded` .

```csharp
void OnMainPageLoaded(object sender, RoutedEventArgs e)
{
    this.Frame.SizeChanged += (o, args) =>
    {
        if (noteEntryPage != null)
            noteEntryPage.Layout(new Xamarin.Forms.Rectangle(0, 0, args.NewSize.Width, args.NewSize.Height));
        else
            notesPage.Layout(new Xamarin.Forms.Rectangle(0, 0, args.NewSize.Width, args.NewSize.Height));
    };
}
```

메서드는에 `OnMainPageLoaded` 대해 `Frame.SizeChanged` `ActualHeight` 또는 속성이 변경 될 때 발생 하는 이벤트에 대 한 익명 이벤트 처리기를 등록 합니다 `ActualWidth` `Frame` . 응답으로 Xamarin.Forms 메서드를 호출 하 여 활성 페이지의 콘텐츠 크기를 조정 합니다 `Layout` .

### <a name="enable-back-navigation-support"></a>뒤로 탐색 지원 사용

UWP에서 응용 프로그램은 여러 장치 폼 팩터에서 모든 하드웨어 및 소프트웨어 뒤로 단추에 대해 다시 탐색을 사용 하도록 설정 해야 합니다. 이 `BackRequested` 작업은 생성자에서 수행할 수 있는 이벤트에 대 한 이벤트 처리기를 등록 하 여 수행할 수 있습니다 `MainPage` .

```csharp
public MainPage()
{
    // ...
    SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
}
```

응용 프로그램이 시작 되 면 메서드는 `GetForCurrentView` `SystemNavigationManager` 현재 뷰와 연결 된 개체를 검색 한 다음 이벤트에 대 한 이벤트 처리기를 등록 `BackRequested` 합니다. 응용 프로그램은 포그라운드 응용 프로그램이 고 응답으로 이벤트 처리기를 호출 하는 경우에만이 이벤트를 수신 합니다 `OnBackRequested` .

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
        noteEntryPage = null;
    }
}
```

`OnBackRequested`이벤트 처리기는 `GoBack` 응용 프로그램의 루트 프레임에서 메서드를 호출 하 고 속성을로 설정 하 여 `BackRequestedEventArgs.Handled` 이벤트를 처리 된 `true` 것으로 표시 합니다. 이벤트를 처리 된 것으로 표시 하지 못하면 이벤트를 무시 하 게 됩니다.

응용 프로그램은 제목 표시줄에 뒤로 단추를 표시할지 여부를 선택 합니다. 이렇게 `AppViewBackButtonVisibility` 하려면 클래스에서 속성을 열거형 값 중 하나로 설정 합니다 `AppViewBackButtonVisibility` `App` .

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

이벤트 `OnNavigated` 발생에 대 한 응답으로 실행 되는 이벤트 처리기는 `Navigated` 페이지 탐색이 발생 하는 경우 제목 표시줄 뒤로 단추의 표시 여부를 업데이트 합니다. 이렇게 하면 앱 내 백오프 스택이 비어 있지 않거나 앱 내 백오프 스택이 비어 있는 경우 제목 표시줄에서 제거 되 면 제목 표시줄 뒤로 단추가 표시 됩니다.

UWP의 후방 탐색 지원에 대 한 자세한 내용은 [탐색 기록 및 uwp 앱에 대 한 역방향 탐색](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [NativeForms (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/native2forms)
- [네이티브 뷰](~/xamarin-forms/platform/native-views/index.md)
