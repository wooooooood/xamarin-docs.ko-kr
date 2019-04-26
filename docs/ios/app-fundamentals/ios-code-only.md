---
title: Xamarin.iOS에서 코드로 iOS 사용자 인터페이스 만들기
description: 이 문서에서는 코드를 사용 하 여 Xamarin.iOS 앱에 대 한 사용자 인터페이스를 구축 하는 방법을 설명 합니다. 뷰 컨트롤러, 빌드 등 회전을 처리 하는 뷰 계층 구조에 설명 합니다.
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/03/2018
ms.openlocfilehash: 28475df4baa225cc9a608607be6ed673ad0e6e8a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61251382"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Xamarin.iOS에서 코드로 iOS 사용자 인터페이스 만들기

IOS 앱의 사용자 인터페이스는 storefront – 응용 프로그램은 일반적으로 하나의 창을 가져옵니다 비슷하지만 채울 수 창 사용 하 여 많은 개체가 필요한 개체 및 배열은 어떤 앱에 따라 변경할 수 있습니다 및 표시 하려고 하는 대로 합니다. 사용자에게 표시되는 항목인 이 시나리오의 개체를 뷰라고 합니다. 응용 프로그램에서 단일 화면을 빌드하려면 뷰는 서로 위에 쌓인 콘텐츠 뷰 계층 구조에서 및 단일 뷰 컨트롤러에서 관리 하는 계층입니다. 여러 화면이 있는 애플리케이션은 각각 고유한 뷰 컨트롤러가 있는 여러 콘텐츠 뷰 계층 구조가 있으며, 애플리케이션은 사용자가 보는 화면에 따라 다른 콘텐츠 뷰 계층 구조를 만들도록 창에 뷰를 배치합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

다음 다이어그램에는 디바이스 화면에 사용자 인터페이스를 가져오는 창, 뷰, 하위 뷰 및 뷰 컨트롤러 간의 관계가 나와 있습니다.

[![](ios-code-only-images/image9.png "이 다이어그램 창, 뷰, 하위 뷰 및 뷰 컨트롤러 간의 관계를 보여 줍니다.")](ios-code-only-images/image9.png#lightbox)

그러나 사용 하 여 이러한 뷰 계층 구조를 생성할 수 있습니다 합니다 [iOS 용 Xamarin 디자이너](~/ios/user-interface/designer/index.md) Visual Studio에서는 것이 좋습니다 코드에서 완전히 작동 하는 방법에 대 한 기본적인 이해 해야 합니다. 이 문서에서는 몇 가지 기본 사항을 시작 및 코드 전용 사용자 인터페이스 개발을 사용 하 여 실행 안내 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

다음 다이어그램에는 디바이스 화면에 사용자 인터페이스를 가져오는 창, 뷰, 하위 뷰 및 뷰 컨트롤러 간의 관계가 나와 있습니다.

[![](ios-code-only-images/image9.png "이 다이어그램 창, 뷰, 하위 뷰 및 뷰 컨트롤러 간의 관계를 보여 줍니다.")](ios-code-only-images/image9.png#lightbox)

그러나 사용 하 여 이러한 뷰 계층 구조를 생성할 수 있습니다 합니다 [iOS 용 Xamarin 디자이너](~/ios/user-interface/designer/index.md) Mac 용 Visual Studio에서는 것이 좋습니다 코드에서 완전히 작동 하는 방법에 대 한 기본적인 이해 해야 합니다. 이 문서에서는 몇 가지 기본 사항을 시작 및 코드 전용 사용자 인터페이스 개발을 사용 하 여 실행 안내 합니다.

-----

## <a name="creating-a-code-only-project"></a>코드 전용 프로젝트 만들기

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="ios-blank-project-template"></a>iOS 빈 프로젝트 템플릿

먼저 사용 하 여 Visual Studio에서 iOS 프로젝트를 만듭니다는 **파일 > 새 프로젝트 > Visual C# > iPhone & iPad > iOS 앱 (Xamarin)** 프로젝트, 아래 참조:

[![새 프로젝트 대화 상자](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

다음을 선택 합니다 **비어 있는 앱** 프로젝트 템플릿:

[![템플릿 대화 상자를 선택 합니다.](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

빈 프로젝트 템플릿을 프로젝트에 4 개 파일을 추가합니다.

[![프로젝트 파일](ios-code-only-images/empty-project.w157-sml.png "프로젝트 파일")](ios-code-only-images/empty-project.w157.png#lightbox)

1. **AppDelegate.cs** -포함 된 `UIApplicationDelegate` 서브 클래스에서 `AppDelegate` 는 iOS에서 응용 프로그램 이벤트를 처리 하는 데 사용 됩니다. 응용 프로그램 창은 합니다 `AppDelegate`의 `FinishedLaunching` 메서드.
1. **Main.cs** -클래스를 지정 하는 응용 프로그램에 대 한 진입점을 포함 한를 `AppDelegate` 입니다.
1. **Info.plist** -응용 프로그램 구성 정보를 포함 하는 속성 목록 파일.
1. **Entitlements.plist** – 응용 프로그램의 권한과 기능에 대 한 정보를 포함 하는 속성 목록 파일입니다.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="ios-templates"></a>iOS 템플릿


Mac 용 visual Studio에는 빈 템플릿을 제공 하지 않습니다. 모든 템플릿에 UI를 만들기 위한 기본 방법으로 Apple이 권장 하는 스토리 보드가 지원 제공 됩니다. 그러나 코드에서 완전히 UI를 만들 수 것입니다.

아래 단계를 안내 응용 프로그램에서 스토리 보드를 제거 합니다.


1. 단일 뷰 앱 템플릿을 사용 하 여 새 iOS 프로젝트를 만듭니다.

    [![](ios-code-only-images/single-view-app.png "단일 뷰 앱 템플릿 사용")](ios-code-only-images/single-view-app.png#lightbox)

1. 삭제 된 `Main.Storyboard` 고 `ViewController.cs` 파일입니다. 수행 **되지** 삭제 된 `LaunchScreen.Storyboard`합니다. 뷰 컨트롤러 스토리 보드에서 만든 뷰 컨트롤러에 대 한 코드 숨김 이므로 삭제 해야 합니다.
1. 선택 되어 있는지 확인 **삭제** 팝업 대화 상자에서:

    [![](ios-code-only-images/delete.png "팝업 대화 상자에서 삭제를 선택 합니다.")](ios-code-only-images/delete.png#lightbox)

1. Info.plist의 내부의 정보를 삭제 합니다 **배포 정보 > 주 인터페이스** 옵션:

    [![](ios-code-only-images/main-interface.png "주 인터페이스 옵션 내에서 정보를 삭제 합니다.")](ios-code-only-images/main-interface.png#lightbox)

1. 마지막으로 다음 코드를 추가 하면 `FinishedLaunching` AppDelegate 클래스의 메서드:

    ```csharp
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        window.MakeKeyAndVisible();

        return true;
    }
    ```

에 추가 된 코드는 `FinishedLaunching` 메서드 위의 5 단계에서 iOS 응용 프로그램에 대 한 창을 만드는 데 필요한 코드의 최소 양입니다.

-----

사용 하 여 iOS 응용 프로그램은 빌드된 합니다 [MVC 패턴](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#model-view-controller-mvc)합니다. 응용 프로그램에 표시 되는 첫 번째 화면은 창의 루트 뷰 컨트롤러에서 생성 됩니다. 참조 된 [Hello, iOS 멀티 스크린](~/ios/get-started/hello-ios-multiscreen/index.md) MVC에 대 한 자세한 내용은 패턴 자체에 대 한 가이드입니다.

에 대 한 구현을 `AppDelegate` 추가 하 여 템플릿 창을 만듭니다. 응용 프로그램에서의 며 모든 iOS 응용 프로그램에 대 한 하나만 하 고 다음 코드를 사용 하 여 표시 되도록 설정:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    public override UIWindow Window
            {
                get;
                set;
            }

    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        window.MakeKeyAndVisible();

        return true;
    }
}
```

가능성이 없다는 throw 된 예외 지금이 응용 프로그램을 실행 하는 경우 얻게 `Application windows are expected to have a root view controller at the end of application launch`합니다. 컨트롤러를 추가 하 고 앱의 루트 뷰 컨트롤러를 확인 하겠습니다.

## <a name="adding-a-controller"></a>컨트롤러 추가

앱 많은 뷰 컨트롤러를 포함할 수 있지만 모든 뷰 컨트롤러를 제어 하려면 루트 뷰 컨트롤러를 하나 있어야 합니다.  만들어 창에 컨트롤러 추가 `UIViewController` 인스턴스 및로 설정 된 `window.RootViewController` 속성:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;

        Window.RootViewController = controller;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

모든 컨트롤러에서 액세스할 수 있는 관련된 보기에는 `View` 속성입니다. 위의 코드 변경 뷰의 `BackgroundColor` 속성을 `UIColor.LightGray` 를 아래와 같이 표시 됩니다.

 [![](ios-code-only-images/image1.png "보기의 백그라운드는 밝은 회색 표시")](ios-code-only-images/image1.png#lightbox)

모든 설정 수 `UIViewController` 으로 하위 클래스 입니다는 `RootViewController` 이 방식으로 UIKit 뿐 직접 작성에서 컨트롤러도 포함 합니다. 다음 코드를 추가 하는 예를 들어, 한 `UINavigationController` 으로 `RootViewController`:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;
        controller.Title = "My Controller";

        var navController = new UINavigationController(controller);

        Window.RootViewController = navController;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

이 아래와 같이 탐색 컨트롤러 내에 중첩 된 컨트롤러를 생성 합니다.

 [![](ios-code-only-images/image2.png "탐색 컨트롤러 내에 중첩 된 컨트롤러")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>뷰 컨트롤러를 만들기

이제 살펴보았으므로 다음으로 컨트롤러를 추가 하는 방법의 `RootViewController` 창의 코드에서 사용자 지정 뷰 컨트롤러를 만드는 방법을 확인해 보겠습니다.

라는 새 클래스 추가 `CustomViewController` 아래와 같이:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](ios-code-only-images/customviewcontroller.w157-sml.png "CustomViewController 라는 새 클래스 추가")](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](ios-code-only-images/new-file.png "CustomViewController 라는 새 클래스 추가")](ios-code-only-images/new-file.png#lightbox)

-----

클래스에서 상속 해야 `UIViewController`에 `UIKit` 같이 네임 스페이스:

```csharp
using System;
using UIKit;

namespace CodeOnlyDemo
{
    class CustomViewController : UIViewController
    {
    }
}
```

## <a name="initializing-the-view"></a>뷰를 초기화합니다.

`UIViewController` 라는 메서드를 포함 `ViewDidLoad` 뷰 컨트롤러는 메모리로 처음 로드 될 때 호출 되는 합니다. 보기의 속성을 설정 하는 등의 초기화 작업을 수행 하는 적절 한 위치입니다.

예를 들어, 단추 및 단추를 누를 때 새 뷰 컨트롤러를 탐색 스택으로 푸시에 이벤트 처리기를 다음 코드를 추가 합니다.

```csharp
using System;
using CoreGraphics;
using UIKit;

namespace CodyOnlyDemo
{
    public class CustomViewController : UIViewController
    {
        public CustomViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            View.BackgroundColor = UIColor.White;
            Title = "My Custom View Controller";

            var btn = UIButton.FromType (UIButtonType.System);
            btn.Frame = new CGRect (20, 200, 280, 44);
            btn.SetTitle ("Click Me", UIControlState.Normal);

            var user = new UIViewController ();
            user.View.BackgroundColor = UIColor.Magenta;

            btn.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (user, true);
            };

            View.AddSubview (btn);

        }
    }
}
```

응용 프로그램에서이 컨트롤러를 로드 하 고 간단한 탐색을 보여 줍니다 하려면의 새 인스턴스를 만듭니다 `CustomViewController`합니다. 새 탐색 컨트롤러를 만들고 보기 컨트롤러 인스턴스에 전달 창의 새 탐색 컨트롤러로 `RootViewController` 에 `AppDelegate` 이전과:

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

이제 로드 되 면 응용 프로그램을 `CustomViewController` 탐색 컨트롤러 내에서 로드 됩니다.

 [![](ios-code-only-images/customvc.png "탐색 컨트롤러 내에서 로드 되는 CustomViewController")](ios-code-only-images/customvc.png#lightbox)

단추를 클릭 _푸시_ 탐색 스택에 새 뷰 컨트롤러:

[![](ios-code-only-images/customvca.png "새 뷰 컨트롤러를 탐색 스택으로 푸시")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>뷰 계층 구조를 작성합니다.

위의 예제에서 인터페이스를 만드는 사용자 코드에서 뷰 컨트롤러에 단추를 추가 하 여 시작 합니다.

iOS 사용자 인터페이스 계층 구조 보기 이루어져 있습니다. 일부 부모 뷰의 하위 레이블, 단추, 슬라이더 등의 추가 보기, 추가 됩니다.

예를 들어, 편집할는 `CustomViewController` 사용자 이름과 암호를 입력할 수 있는 로그인 화면을 만듭니다. 두 텍스트 필드 및 단추는 화면으로 구성 됩니다.

### <a name="adding-the-text-fields"></a>텍스트 필드를 추가합니다.

첫째,에 추가 된 단추와 이벤트 처리기를 제거 합니다 [뷰를 초기화 하](#initializing-the-view) 섹션입니다. 

만들고 초기화 하 여 사용자 이름에 대 한 컨트롤을 추가 `UITextField` 아래와 같이 뷰 계층 구조에 추가 합니다.

```csharp
class CustomViewController : UIViewController
{
    UITextField usernameField;

    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        View.BackgroundColor = UIColor.Gray;

        nfloat h = 31.0f;
        nfloat w = View.Bounds.Width;

        usernameField = new UITextField
        {
            Placeholder = "Enter your username",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 82, w - 20, h)
        };

        View.AddSubview(usernameField);
    }
}
```

만들 때 합니다 `UITextField`, 설정는 `Frame` 해당 위치와 크기를 정의 하는 속성입니다. IOS에 대 한 좌표 0, 0의 왼쪽 위에 있는 x +로 오른쪽 및 + y 다운 합니다. 설정한 후 합니다 `Frame` 이라고는 몇 가지 다른 속성과 함께 `View.AddSubview` 추가할를 `UITextField` 뷰 계층 구조에 합니다. 따라서 합니다 `usernameField` 하위 뷰를를 `UIView` 인스턴스를 `View` 속성 참조. 하위 뷰는 화면에는 부모 뷰의 앞에 나타나는 이므로 해당 부모 보기 보다 더 높은 z 순서를 사용 하 여 추가 됩니다.

응용 프로그램에는 `UITextField` 포함 아래에 표시 됩니다.

 [![](ios-code-only-images/image4.png "포함 된 UITextField 사용 하 여 응용 프로그램")](ios-code-only-images/image4.png#lightbox)

추가할 수 있습니다는 `UITextField` 비슷한 방식으로 암호를이 이번에만 설정 된 `SecureTextEntry` 속성을 true로 아래와 같이:

```csharp
public class CustomViewController : UIViewController
{
    UITextField usernameField, passwordField;
    public override void ViewDidLoad()
    {
       // keep the code the username UITextField
        passwordField = new UITextField
        {
            Placeholder = "Enter your password",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 114, w - 20, h),
            SecureTextEntry = true
        };

      View.AddSubview(usernameField);
      View.AddSubview(passwordField);
   }
}

```

설정 `SecureTextEntry = true` 에 입력 된 텍스트를 숨기는 `UITextField` 아래와 같이 사용자:

 [![](ios-code-only-images/image4a.png "True는 사용자가 입력 한 텍스트를 숨기려면 SecureTextEntry 설정")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>단추 추가

다음으로, 사용자 사용자 이름 및 암호를 전송할 수 있도록 단추를 추가 합니다. 단추는 부모 보기에 인수로 전달 하 여 다른 컨트롤과 마찬가지로 뷰 계층 구조에 추가 됩니다 `AddSubview` 메서드를 다시 합니다.

다음 코드는 단추를 추가 하 고에 대 한 이벤트 처리기를 등록 합니다 `TouchUpInside` 이벤트:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

로그인 화면 위치에이 사용 하 여 아래와 같이 이제 나타납니다.

 [![](ios-code-only-images/image5.png "로그인 화면")](ios-code-only-images/image5.png#lightbox)

이전 버전의 iOS와 달리 기본 단추 배경을 투명 합니다. 단추의 변경 `BackgroundColor` 속성이이 변경 합니다.

```csharp
submitButton.BackgroundColor = UIColor.White;
```

그러면 정사각 단추에서 일반적인 둥근 가장자리 있는 모양된 단추 대신 합니다. 둥근된 가장자리를 가져오려면 다음 코드 조각을 사용 합니다.

```csharp
submitButton.Layer.CornerRadius = 5f;
```

이러한 변경으로 뷰는 다음과 같이 표시 됩니다.

[![](ios-code-only-images/image6.png "뷰는 예제 실행")](ios-code-only-images/image6.png#lightbox)

## <a name="adding-multiple-views-to-the-view-hierarchy"></a>뷰 계층 구조에 뷰 여러 개 추가

여러 뷰를 사용 하 여 계층 구조 보기를 추가 하는 기능을 제공 하는 iOS `AddSubviews`합니다.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton });
```

## <a name="adding-button-functionality"></a>단추 기능 추가

단추를 클릭할 때 사용자에 게 어떤 항목을 예상 됩니다. 예를 들어, 경고가 표시 됩니다 또는 다른 화면으로 탐색이 수행 됩니다.

두 번째 뷰 컨트롤러를 탐색 스택으로 푸시 하려면 일부 코드를 추가 하겠습니다.

먼저 두 번째 뷰 컨트롤러를 만듭니다.

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

그런 다음 기능을 추가 합니다 `TouchUpInside` 이벤트:

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

탐색 아래에서 설명 됩니다.

[![](ios-code-only-images/navigation.png "이 차트에서는 탐색을 보여 줍니다.")](ios-code-only-images/navigation.png#lightbox)

기본적으로 탐색 컨트롤러를 사용 하는 경우 iOS 응용 프로그램에 제공 탐색 모음 및 스택을 통해 다시 이동할 수 있도록 뒤로 단추를 확인 합니다.

## <a name="iterating-through-the-view-hierarchy"></a>뷰 계층 구조 반복

하위 뷰 계층 구조를 통해 반복 하 고 특정 보기를 선택 하는 것이 가능 합니다. 예를 들어 각각 찾으려고 `UIButton` 단추를 다른 지정 `BackgroundColor`, 다음 코드 조각을 사용할 수 있습니다

```csharp
foreach(var subview in View.Subviews)
{
    if (subview is UIButton)
    {
         var btn = subview as UIButton;
         btn.BackgroundColor = UIColor.Green;
    }
}
```

이 있지만 작동 하지 것입니다 보기에 대 한 반복 되는 경우는 `UIView` 대로 모든 보기가 돌아올 것으로 `UIView` 상속 자체는 부모 보기에 추가 된 개체와 `UIView`합니다.

## <a name="handling-rotation"></a>회전 처리

가로 방향으로 장치를 회전할 경우 컨트롤 크기가 조정 되지 않습니다 적절 하 게 다음 스크린 샷에서 같이:

[![](ios-code-only-images/image7.png "가로 방향으로 장치를 회전할 경우 컨트롤 크기가 조정 되지 않습니다 적절 하 게")](ios-code-only-images/image7.png#lightbox)

이 문제를 해결 하는 한 가지 방법은 설정 된 경우는 `AutoresizingMask` 각 보기에는 속성입니다. 이 경우 하려고 컨트롤을 수평으로 확장 하므로 각 설정할 것 `AutoresizingMask`입니다. 다음 예제는 `usernameField`, 있지만 뷰 계층 구조에서 각 가젯은 적용할 수는 동일 합니다.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

이제 장치 또는 시뮬레이터를 회전 하는 것 모두 확장을 채우는 추가 공간을 아래와 같이:

[![](ios-code-only-images/image8.png "모든 컨트롤을 추가 공간을 채우도록 확장")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>사용자 지정 뷰 만들기

UIKit의 일부인 컨트롤을 사용 하는 것 외에도 사용자 지정 보기도 사용할 수 있습니다. 상속 하 여 사용자 지정 보기를 만들 수 있습니다 `UIView` 재정의 `Draw`합니다. 사용자 지정 보기를 만들고 보여 주기 위해 뷰 계층 구조에 추가 해 보겠습니다.

### <a name="inheriting-from-uiview"></a>UIView에서 상속

가장 먼저 해야 할 경우 사용자 지정 보기에 대 한 클래스 만들기 위해이 사용 하는 **클래스** 이라는 빈 클래스로 추가할 Visual Studio에서 템플릿 `CircleView`합니다. 기본 클래스에 설정할 `UIView`, 회수에서는 보기로 제공 되는 `UIKit` 네임 스페이스입니다. 도 필요 합니다 `System.Drawing` 네임 스페이스에도 합니다. 다른 다양 한 `System.*` 네임 스페이스 수 없습니다이 예제에서 사용 되므로 자유롭게 제거할 수 있습니다.

클래스는 다음과 같습니다.

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>UIView에서 그리기

모든 `UIView` 에 `Draw` 그려야 할 때 시스템에 의해 호출 되는 메서드. `Draw` 호출 되 면 안 직접. 실행된 루프 처리 하는 동안 시스템에 의해 호출 됩니다. 뷰는 뷰 계층 구조에 추가 된 후 실행된 루프를 통해 처음으로 해당 `Draw` 메서드가 호출 됩니다. 에 대 한 후속 호출 `Draw` 뷰를 호출 하 여 그려야 하는 것으로 표시 되어 표시 될 `SetNeedsDisplay` 또는 `SetNeedsDisplayInRect` 뷰.

재정의 된 내부에서 이러한 코드를 추가 하 여 보기에 그리기 코드를 추가할 수 있습니다 `Draw` 아래와 같이 메서드:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    //get graphics context
    using (var g = UIGraphics.GetCurrentContext())
    {
        // set up drawing attributes
        g.SetLineWidth(10.0f);
        UIColor.Green.SetFill();
        UIColor.Blue.SetStroke();

        // create geometry
        var path = new CGPath();
        path.AddArc(Bounds.GetMidX(), Bounds.GetMidY(), 50f, 0, 2.0f * (float)Math.PI, true);

        // add geometry to graphics context and draw
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);
    }
}
```

이후 `CircleView` 는 `UIView`를 설정할 수도 있습니다 `UIView` 속성도 합니다. 예를 들어, 설정할 수 있습니다는 `BackgroundColor` 생성자에서:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

사용 하는 `CircleView` 방금 만든, 추가할 수 있습니다 하거나 것 하위 뷰를 기존 컨트롤러, 보기 계층으로와 마찬가지로 합니다 `UILabels` 및 `UIButton` 앞 하거나 새 컨트롤러 뷰로 것을 로드할 수 있습니다. 후자를 보겠습니다.

### <a name="loading-a-view"></a>뷰를 로드합니다.

 `UIViewController` 라는 메서드가 `LoadView` 해당 보기를 만들려면 컨트롤러에서 호출 되는 합니다. 이 뷰를 만들고 컨트롤러의 할당을 적절 한 위치 `View` 속성입니다.

먼저 컨트롤러 해야 하므로 라는 새 빈 클래스를 만드는 `CircleController`합니다.

`CircleController` 설정 하려면 다음 코드를 추가 합니다 `View` 에 `CircleView` (호출 하지 않아야는 `base` 재정의에서 구현):

```csharp
using UIKit;

namespace CodeOnlyDemo
{
    class CircleController : UIViewController
    {
        CircleView view;

        public override void LoadView()
        {
            view = new CircleView();
            View = view;
        }
    }
}
```

마지막으로, 런타임 시 컨트롤러를 제공 해야 합니다. 이전에 다음과 같은 추가 전송 단추에 이벤트 처리기를 추가 하 여이 실행 해 보겠습니다.

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

이제 응용 프로그램을 실행 하 고 전송 단추 탭을 원을 사용 하 여 새 뷰에 표시 됩니다.

[![](ios-code-only-images/circles.png "원을 사용 하 여 새 뷰가 표시 됩니다.")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>시작 화면 만들기

A [시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md) 앱 응답성이 뛰어난 임을 사용자에 게 표시 하는 방법으로 시작 하는 경우에 표시 됩니다. 앱이 로드 될 때에 시작 화면이 표시 되 면 되므로 응용 프로그램은 여전히 메모리로 로드 되 면 코드에서 다시 만들 수 없습니다 것입니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 iOS 프로젝트를 만들면 시작 화면으로 제공 됩니다.xib 파일에서 찾을 수 있는 형태로 합니다 **리소스** 프로젝트 폴더입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac 용 Visual Studio에서 iOS 프로젝트를 만들면 시작 화면에서 스토리 보드 파일 형식으로 제공 됩니다.

-----

이 두 번 클릭 하 고 iOS 디자이너에서에서 열어에서 편집할 수 있습니다.

Apple에.xib 또는 스토리 보드 파일은 iOS 8을 대상으로 하는 응용 프로그램에 대 한 사용은 또는 나중에 iOS 디자이너에 있는 파일 중 하나를 시작할 때 사용 크기 클래스 및 자동 레이아웃을 정상적으로 진행 하 고 모든 장치에 대 한 올바르게 표시 됩니다 있도록 레이아웃을 조정 하는 것이 좋습니다. 크기는 합니다. 이전 버전을 대상으로 하는 응용 프로그램에 대 한 지원을 허용 하려면.xib 또는 스토리 보드를 실행 하는 것 외에도 정적 시작 이미지를 사용할 수 있습니다.

시작 화면을 만드는 방법에 대 한 자세한 내용은 아래 문서를 참조 하세요.

- [.xib을 사용 하 여 시작 화면 만들기](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)
- [시작 화면 스토리 보드를 사용 하 여 관리](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> Apple는 iOS 9 기준으로 시작 화면을 만드는 기본 방법으로 스토리 보드를 사용 해야 하는 권장 합니다.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>시작 이미지를 사전 iOS 8 응용 프로그램 만들기

이전의 iOS 8 버전의 응용 프로그램을 대상으로 하는 경우 정적 이미지를.xib 또는 시작 화면 스토리 보드를 실행 하는 것 외에도 사용할 수 있습니다.

응용 프로그램에서 (iOS 7)에 대 한 자산 카탈로그 또는 Info.plist 파일에서이 정적 이미지를 설정할 수 있습니다. 응용 프로그램에서 실행 될 수 있는 각 장치 크기 (320 × 480, 640 x 960 640 x 1136)에 대 한 별도 이미지를 제공 해야 합니다. 시작 화면 크기에 대 한 자세한 내용은 다음을 확인 합니다 [시작 화면 이미지](~/ios/app-fundamentals/images-icons/launch-screens.md) 가이드입니다.

> [!IMPORTANT]
> 앱에 시작 화면이 없는 경우 화면을 맞지 않는 완벽 하 게 확인할 수 있습니다. 이 경우 해야 적어도 라는 640 x 1136 이미지를 포함 하려면 `Default-568@2x.png` info.plist 합니다.

## <a name="summary"></a>요약

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

이 문서에서는 Visual Studio에서 프로그래밍 방식으로 iOS 응용 프로그램을 개발 하는 방법을 설명 합니다. 살펴보았습니다, 빈 프로젝트 템플릿에서 프로젝트를 빌드하는 방법을 만들고 창에는 루트 보기 컨트롤러를 추가 하는 방법을 설명 합니다. 그런 다음 응용 프로그램 화면을 개발 하려면 컨트롤러 내 보기 계층 구조를 만들 UIKit의 컨트롤을 사용 하는 방법을 살펴보았습니다. 보기 수 있도록 레이아웃 적절 하 게 다른 방향으로 방법과 서브클래싱하 여 사용자 지정 보기를 만드는 방법에 살펴보았습니다 검사한 다음 `UIView`방법, 컨트롤러 내 보기를 로드 합니다. 마지막으로 응용 프로그램에 시작 화면을 추가 하는 방법을 살펴보았습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이 문서에서는 mac 용 Visual Studio에서 프로그래밍 방식으로 iOS 응용 프로그램을 개발 하는 방법 설명 살펴보았습니다 단일 보기 템플릿에서 프로젝트를 빌드하는 방법을 만들고 창에는 루트 보기 컨트롤러를 추가 하는 방법을 설명 합니다. 그런 다음 응용 프로그램 화면을 개발 하려면 컨트롤러 내 보기 계층 구조를 만들 UIKit의 컨트롤을 사용 하는 방법을 살펴보았습니다. 보기 수 있도록 레이아웃 적절 하 게 다른 방향으로 방법과 서브클래싱하 여 사용자 지정 보기를 만드는 방법에 살펴보았습니다 검사한 다음 `UIView`방법, 컨트롤러 내 보기를 로드 합니다. 마지막으로 응용 프로그램에 시작 화면을 추가 하는 방법을 살펴보았습니다.

-----

## <a name="related-links"></a>관련 링크

- [SimpleLogin (샘플)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
