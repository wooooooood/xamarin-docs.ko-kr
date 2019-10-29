---
title: Xamarin.ios의 코드에서 iOS 사용자 인터페이스 만들기
description: 이 문서에서는 코드를 사용 하 여 Xamarin.ios 앱에 대 한 사용자 인터페이스를 빌드하는 방법을 설명 합니다. 뷰 컨트롤러, 뷰 계층 구조 빌드, 회전 처리 등을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/03/2018
ms.openlocfilehash: 42a2694239fdd55efa91b7fa30be8a10cafb4cc5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73010319"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Xamarin.ios의 코드에서 iOS 사용자 인터페이스 만들기

IOS 앱의 사용자 인터페이스는 storefront와 같습니다. 응용 프로그램은 일반적으로 하나의 창을 가져오지만 필요한 만큼의 개체를 사용 하 여 창을 채울 수 있으며, 응용 프로그램에서 표시 하려는 내용에 따라 개체와 정렬을 변경할 수 있습니다. 사용자에게 표시되는 항목인 이 시나리오의 개체를 뷰라고 합니다. 응용 프로그램에서 단일 화면을 작성 하기 위해 뷰는 콘텐츠 뷰 계층 구조에서 서로 위에 누적 되며 계층은 단일 뷰 컨트롤러에 의해 관리 됩니다. 여러 화면이 있는 애플리케이션은 각각 고유한 뷰 컨트롤러가 있는 여러 콘텐츠 뷰 계층 구조가 있으며, 애플리케이션은 사용자가 보는 화면에 따라 다른 콘텐츠 뷰 계층 구조를 만들도록 창에 뷰를 배치합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

다음 다이어그램에는 디바이스 화면에 사용자 인터페이스를 가져오는 창, 뷰, 하위 뷰 및 뷰 컨트롤러 간의 관계가 나와 있습니다.

[![](ios-code-only-images/image9.png "This diagram illustrates the relationships between the Window, Views, Subviews, and View Controller")](ios-code-only-images/image9.png#lightbox)

이러한 뷰 계층 구조는 Visual Studio의 [Xamarin Designer for iOS](~/ios/user-interface/designer/index.md) 를 사용 하 여 생성할 수 있지만 완전히 코드에서 작업 하는 방법에 대 한 기본적인 이해를 주는 것이 좋습니다. 이 문서에서는 코드 전용 사용자 인터페이스 개발을 시작 및 실행 하기 위한 몇 가지 기본 사항을 안내 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

다음 다이어그램에는 디바이스 화면에 사용자 인터페이스를 가져오는 창, 뷰, 하위 뷰 및 뷰 컨트롤러 간의 관계가 나와 있습니다.

[![](ios-code-only-images/image9.png "This diagram illustrates the relationships between the Window, Views, Subviews, and View Controller")](ios-code-only-images/image9.png#lightbox)

이러한 뷰 계층 구조는 Mac용 Visual Studio [Xamarin Designer for iOS](~/ios/user-interface/designer/index.md) 를 사용 하 여 생성할 수 있지만 완전히 코드에서 작업 하는 방법에 대 한 기본적인 이해를 주는 것이 좋습니다. 이 문서에서는 코드 전용 사용자 인터페이스 개발을 시작 및 실행 하기 위한 몇 가지 기본 사항을 안내 합니다.

-----

## <a name="creating-a-code-only-project"></a>코드 전용 프로젝트 만들기

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="ios-blank-project-template"></a>iOS 빈 프로젝트 템플릿

먼저 아래와 같이 Visual Studio에서 **visual C# > iPhone & IPad > Ios 앱 (Xamarin) 프로젝트 > > 파일** 을 사용 하 여 ios 프로젝트를 만듭니다.

[![새 프로젝트 대화 상자](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

그런 다음 **빈 앱** 프로젝트 템플릿을 선택 합니다.

[![템플릿 대화 상자 선택](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

빈 프로젝트 템플릿은 프로젝트에 4 개의 파일을 추가 합니다.

[![프로젝트 파일](ios-code-only-images/empty-project.w157-sml.png "프로젝트 파일")](ios-code-only-images/empty-project.w157.png#lightbox)

1. **AppDelegate.cs** -iOS의 응용 프로그램 이벤트를 처리 하는 데 사용 되는 `UIApplicationDelegate` 하위 클래스 `AppDelegate`를 포함 합니다. `AppDelegate`의 `FinishedLaunching` 메서드에서 응용 프로그램 창이 만들어집니다.
1. **Main.cs** -`AppDelegate`에 대 한 클래스를 지정 하는 응용 프로그램에 대 한 진입점을 포함 합니다.
1. **Info.plist** -응용 프로그램 구성 정보를 포함 하는 속성 목록 파일입니다.
1. **Info.plist** – 응용 프로그램의 기능 및 사용 권한에 대 한 정보를 포함 하는 속성 목록 파일입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="ios-templates"></a>iOS 템플릿

Mac용 Visual Studio는 빈 템플릿을 제공 하지 않습니다. 모든 템플릿은 Apple에서 UI를 만드는 기본 방법으로 권장 되는 스토리 보드 지원과 함께 제공 됩니다. 그러나 코드에서 UI를 완전히 만들 수 있습니다.

아래 단계는 응용 프로그램에서 스토리 보드를 제거 하는 과정을 안내 합니다.

1. 단일 뷰 응용 프로그램 템플릿을 사용 하 여 새 iOS 프로젝트를 만듭니다.

    [![](ios-code-only-images/single-view-app.png "Use the Single View App template")](ios-code-only-images/single-view-app.png#lightbox)

1. `Main.Storyboard` 및 `ViewController.cs` 파일을 삭제 합니다. `LaunchScreen.Storyboard`를 삭제 **하지** 마십시오. 뷰 컨트롤러는 스토리 보드에 생성 된 뷰 컨트롤러에 대 한 코드 숨김이 되기 때문에 삭제 해야 합니다.
1. 팝업 대화 상자에서 **삭제** 를 선택 해야 합니다.

    [![](ios-code-only-images/delete.png "Select Delete from the pop-up dialog")](ios-code-only-images/delete.png#lightbox)

1. Info.plist에서 **배포 정보 > 주 인터페이스** 옵션 내의 정보를 삭제 합니다.

    [![](ios-code-only-images/main-interface.png "Delete the information inside the Main Interface option")](ios-code-only-images/main-interface.png#lightbox)

1. 마지막으로 AppDelegate 클래스의 `FinishedLaunching` 메서드에 다음 코드를 추가 합니다.

    ```csharp
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
    ```

위의 5 단계에서 `FinishedLaunching` 메서드에 추가 된 코드는 iOS 응용 프로그램에 대 한 창을 만드는 데 필요한 최소한의 코드 양입니다.

-----

iOS 응용 프로그램은 [MVC 패턴](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#model-view-controller-mvc)을 사용 하 여 빌드됩니다. 응용 프로그램이 표시 하는 첫 번째 화면은 창의 루트 뷰 컨트롤러에서 만들어집니다. MVC 패턴 자체에 대 한 자세한 내용은 [Hello, IOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) guide를 참조 하세요.

템플릿에 의해 추가 된 `AppDelegate`에 대 한 구현은 응용 프로그램 창을 만들며,이 창에는 모든 iOS 응용 프로그램에 대 한 응용 프로그램 창이 하나 뿐 이며 다음 코드와 함께 표시 됩니다.

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
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

지금이 응용 프로그램을 실행 하는 경우 `Application windows are expected to have a root view controller at the end of application launch`를 나타내는 예외가 발생 했을 수 있습니다. 컨트롤러를 추가 하 고 앱의 루트 뷰 컨트롤러로 만듭니다.

## <a name="adding-a-controller"></a>컨트롤러 추가

앱은 많은 뷰 컨트롤러를 포함할 수 있지만 모든 뷰 컨트롤러를 제어 하려면 하나의 루트 뷰 컨트롤러가 있어야 합니다.  `UIViewController` 인스턴스를 만들고 `Window.RootViewController` 속성으로 설정 하 여 컨트롤러를 창에 추가 합니다.

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

모든 컨트롤러에는 `View` 속성에서 액세스할 수 있는 연결 된 뷰가 있습니다. 위의 코드는 뷰의 `BackgroundColor` 속성을 `UIColor.LightGray`으로 변경 하 여 아래와 같이 표시 됩니다.

 [![](ios-code-only-images/image1.png "The View's background is a visible light gray")](ios-code-only-images/image1.png#lightbox)

UIKit의 컨트롤러와 우리가 직접 작성 하는 컨트롤러를 포함 하 여 `UIViewController` 하위 클래스를 이러한 방식으로 `RootViewController`로 설정할 수 있습니다. 예를 들어 다음 코드는 `RootViewController``UINavigationController`를 추가 합니다.

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

이렇게 하면 아래와 같이 탐색 컨트롤러 내에 중첩 된 컨트롤러가 생성 됩니다.

 [![](ios-code-only-images/image2.png "The controller nested within the navigation controller")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>뷰 컨트롤러 만들기

이제 창의 `RootViewController`으로 컨트롤러를 추가 하는 방법을 알아보았습니다. 코드에서 사용자 지정 뷰 컨트롤러를 만드는 방법을 알아보겠습니다.

아래와 같이 `CustomViewController` 라는 새 클래스를 추가 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](ios-code-only-images/customviewcontroller.w157-sml.png "Add a new class named CustomViewController")](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](ios-code-only-images/new-file.png "Add a new class named CustomViewController")](ios-code-only-images/new-file.png#lightbox)

-----

클래스는 다음과 같이 `UIKit` 네임 스페이스에 있는 `UIViewController`에서 상속 해야 합니다.

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

## <a name="initializing-the-view"></a>뷰 초기화

`UIViewController`에는 뷰 컨트롤러를 메모리에 처음 로드할 때 호출 되는 `ViewDidLoad` 라는 메서드가 포함 되어 있습니다. 속성을 설정 하는 것과 같이 뷰를 초기화 하는 적절 한 장소입니다.

예를 들어 다음 코드는 단추를 누르면 단추와 이벤트 처리기를 추가 하 여 새 뷰 컨트롤러를 탐색 스택으로 푸시합니다.

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

응용 프로그램에서이 컨트롤러를 로드 하 고 간단한 탐색을 보여 주기 위해 `CustomViewController`의 새 인스턴스를 만듭니다. 새 탐색 컨트롤러를 만들고, 뷰 컨트롤러 인스턴스를 전달 하 고, 이전 처럼 `AppDelegate`의 창 `RootViewController` 새 탐색 컨트롤러를 설정 합니다.

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

이제 응용 프로그램이 로드 되 면 탐색 컨트롤러 내에 `CustomViewController` 로드 됩니다.

 [![](ios-code-only-images/customvc.png "The CustomViewController is loaded inside a navigation controller")](ios-code-only-images/customvc.png#lightbox)

단추를 _클릭 하면 새_ 뷰 컨트롤러가 탐색 스택으로 푸시됩니다.

[![](ios-code-only-images/customvca.png "A new View Controller pushed onto the navigation stack")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>뷰 계층 구조 빌드

위의 예제에서는 뷰 컨트롤러에 단추를 추가 하 여 코드에서 사용자 인터페이스 만들기를 시작 했습니다.

iOS 사용자 인터페이스는 뷰 계층 구조로 구성 됩니다. 레이블, 단추, 슬라이더 등의 추가 보기가 일부 부모 뷰의 하위 뷰로 추가 됩니다.

예를 들어 사용자가 사용자 이름과 암호를 입력할 수 있는 로그인 화면을 만드는 `CustomViewController`를 편집 하겠습니다. 화면은 두 개의 텍스트 필드와 단추로 구성 됩니다.

### <a name="adding-the-text-fields"></a>텍스트 필드 추가

먼저 [뷰 초기화](#initializing-the-view) 섹션에 추가 된 단추 및 이벤트 처리기를 제거 합니다. 

`UITextField`를 만들고 초기화 한 후 아래와 같이 뷰 계층 구조에 추가 하 여 사용자 이름에 대 한 컨트롤을 추가 합니다.

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

`UITextField`를 만들 때 `Frame` 속성을 설정 하 여 해당 위치와 크기를 정의 합니다. IOS에서 0은 왼쪽 위에서 + x가 오른쪽에 있고 + y 아래쪽에는 0 좌표가 있습니다. `Frame`을 다른 몇 가지 속성과 함께 설정한 후에는 `View.AddSubview`를 호출 하 여 뷰 계층 구조에 `UITextField`를 추가 합니다. 이렇게 하면 `usernameField` `View` 속성이 참조 하는 `UIView` 인스턴스의 하위 뷰가 됩니다. 하위 뷰가 부모 뷰 보다 높은 z 순서를 사용 하 여 추가 되었으므로 화면에서 부모 뷰의 앞에 표시 됩니다.

`UITextField` 포함 된 응용 프로그램은 다음과 같습니다.

 [![](ios-code-only-images/image4.png "The application with the UITextField included")](ios-code-only-images/image4.png#lightbox)

비슷한 방식으로 암호에 대 한 `UITextField`를 추가할 수 있습니다 .이 경우에만 아래와 같이 `SecureTextEntry` 속성을 true로 설정 합니다.

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

`SecureTextEntry = true` 설정 하면 아래와 같이 사용자가 `UITextField` 입력 한 텍스트를 숨깁니다.

 [![](ios-code-only-images/image4a.png "Setting SecureTextEntry true hides the text entered by the user")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>단추 추가

그런 다음 사용자가 사용자 이름과 암호를 전송할 수 있도록 단추를 추가 합니다. 이 단추는 부모 뷰의 `AddSubview` 메서드에 인수로 전달 하 여 다른 컨트롤과 마찬가지로 뷰 계층 구조에 추가 됩니다.

다음 코드는 단추를 추가 하 고 `TouchUpInside` 이벤트에 대 한 이벤트 처리기를 등록 합니다.

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

이제 다음과 같이 로그인 화면이 표시 됩니다.

 [![](ios-code-only-images/image5.png "The login screen")](ios-code-only-images/image5.png#lightbox)

이전 버전의 iOS와 달리 기본 단추 배경은 투명 합니다. 단추의 `BackgroundColor` 속성을 변경 하면 다음과 같이 변경 됩니다.

```csharp
submitButton.BackgroundColor = UIColor.White;
```

이렇게 하면 일반적으로 모퉁이가 둥근 가장자리 단추가 아니라 정사각형 단추가 생성 됩니다. 모퉁이가 둥근 가장자리를 가져오려면 다음 코드 조각을 사용 합니다.

```csharp
submitButton.Layer.CornerRadius = 5f;
```

이러한 변경 내용으로 보기는 다음과 같습니다.

[![](ios-code-only-images/image6.png "An example run of the view")](ios-code-only-images/image6.png#lightbox)

## <a name="adding-multiple-views-to-the-view-hierarchy"></a>뷰 계층 구조에 여러 뷰 추가

iOS는 `AddSubviews`을 사용 하 여 뷰 계층 구조에 여러 뷰를 추가 하는 기능을 제공 합니다.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton });
```

## <a name="adding-button-functionality"></a>단추 기능 추가

단추를 클릭 하면 사용자가 발생 하는 것으로 간주 됩니다. 예를 들어 경고가 표시 되거나 탐색이 다른 화면에 수행 됩니다.

두 번째 뷰 컨트롤러를 탐색 스택으로 푸시하는 코드를 추가 해 보겠습니다.

먼저 두 번째 뷰 컨트롤러를 만듭니다.

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

그런 다음 `TouchUpInside` 이벤트에 기능을 추가 합니다.

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

탐색은 다음과 같이 표시 됩니다.

[![](ios-code-only-images/navigation.png "The navigation is illustrated in this chart")](ios-code-only-images/navigation.png#lightbox)

기본적으로 탐색 컨트롤러를 사용 하는 경우 iOS는 응용 프로그램에 탐색 모음을 제공 하 고 뒤로 단추를 사용 하 여 스택 간을 다시 이동할 수 있도록 합니다.

## <a name="iterating-through-the-view-hierarchy"></a>뷰 계층 구조 반복

하위 뷰 계층 구조를 반복 하 고 특정 뷰를 선택할 수 있습니다. 예를 들어 각 `UIButton`를 찾고 해당 단추에 다른 `BackgroundColor`을 지정 하려면 다음 코드 조각을 사용할 수 있습니다.

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

그러나이 경우에 대해 반복 되는 뷰는 부모 뷰에 추가 된 개체가 `UIView`상속 하는 것 처럼 `UIView` 되는 것 처럼 반환 되는 경우 `UIView`에는 작동 하지 않습니다.

## <a name="handling-rotation"></a>회전 처리

사용자가 장치를 가로 방향으로 회전 하는 경우 다음 스크린샷에 나와 있는 것 처럼 컨트롤의 크기가 적절 하 게 조정 되지 않습니다.

[![](ios-code-only-images/image7.png "If the user rotates the device to landscape, the controls do not resize appropriately")](ios-code-only-images/image7.png#lightbox)

이 문제를 해결 하는 한 가지 방법은 각 뷰에서 `AutoresizingMask` 속성을 설정 하는 것입니다. 이 경우 컨트롤을 가로로 스트레치 하 여 각 `AutoresizingMask`를 설정 합니다. 다음 예는 `usernameField`에 대 한 것 이지만 뷰 계층의 각 가젯에 동일한를 적용 해야 합니다.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

이제 장치 또는 시뮬레이터를 회전할 때 아래와 같이 모든 항목을 확장 하 여 추가 공간을 채웁니다.

[![](ios-code-only-images/image8.png "All the controls stretch to fill the additional space")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>사용자 지정 보기 만들기

UIKit에 포함 된 컨트롤을 사용 하는 것 외에도 사용자 지정 뷰를 사용할 수 있습니다. `UIView`에서 상속 하 고 `Draw`를 재정의 하 여 사용자 지정 뷰를 만들 수 있습니다. 사용자 지정 보기를 만들어 보여줄 뷰 계층 구조에 추가 해 보겠습니다.

### <a name="inheriting-from-uiview"></a>UIView에서 상속

가장 먼저 해야 할 일은 사용자 지정 보기에 대 한 클래스를 만드는 것입니다. 이 작업을 수행 하려면 Visual Studio의 **클래스** 템플릿을 사용 하 여 `CircleView`라는 빈 클래스를 추가 합니다. 기본 클래스는 `UIKit` 네임 스페이스에 있는 `UIView`로 설정 되어야 합니다. `System.Drawing` 네임 스페이스도 필요 합니다. 다른 다양 한 `System.*` 네임 스페이스는이 예제에서 사용 되지 않으므로 자유롭게 제거할 수 있습니다.

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

### <a name="drawing-in-a-uiview"></a>UIView 그리기

모든 `UIView`에는 시스템을 그려야 할 때 시스템에서 호출 하는 `Draw` 메서드가 있습니다. `Draw` 직접 호출 하면 안 됩니다. 루프 처리를 실행 하는 동안 시스템에서 호출 됩니다. 뷰가 뷰 계층 구조에 추가 된 후 실행 루프를 처음으로 실행 하면 해당 `Draw` 메서드가 호출 됩니다. 뷰에 대해 `SetNeedsDisplay` 또는 `SetNeedsDisplayInRect`를 호출 하 여 뷰가 그려야 하는 것으로 표시 되는 경우에 대 한 후속 호출 `Draw` 발생 합니다.

아래와 같이 재정의 된 `Draw` 메서드 내에 이러한 코드를 추가 하 여 뷰에 그리기 코드를 추가할 수 있습니다.

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

`CircleView`은 `UIView`이므로 `UIView` 속성도 설정할 수 있습니다. 예를 들어 생성자에 `BackgroundColor`를 설정할 수 있습니다.

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

방금 만든 `CircleView`를 사용 하려면 이전에 `UILabels` 및 `UIButton` 했던 것 처럼 기존 컨트롤러의 뷰 계층 구조에 하위 뷰로 추가 하거나 새 컨트롤러의 뷰로 로드할 수 있습니다. 후자를 살펴보겠습니다.

### <a name="loading-a-view"></a>뷰 로드

 `UIViewController`에는 컨트롤러에서 뷰를 만들기 위해 호출 하는 `LoadView` 라는 메서드가 있습니다. 뷰를 만들어 컨트롤러의 `View` 속성에 할당 하는 것이 적절 한 장소입니다.

먼저 컨트롤러가 필요 하므로 `CircleController`라는 새 빈 클래스를 만들어야 합니다.

`CircleController`에서 `View`를 `CircleView` 설정 하는 다음 코드를 추가 합니다. 재정의에서 `base` 구현을 호출 해서는 안 됩니다.

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

마지막으로 런타임에 컨트롤러를 제공 해야 합니다. 앞에서 추가한 전송 단추에 다음과 같이 이벤트 처리기를 추가 하 여이 작업을 수행해 보겠습니다.

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

이제 응용 프로그램을 실행 하 고 제출 단추를 누르면 원이 포함 된 새 보기가 표시 됩니다.

[![](ios-code-only-images/circles.png "The new view with a circle is displayed")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>시작 화면 만들기

앱이 시작 될 때 응답 하는 사용자에 게 표시 하는 방법으로 시작 [화면이](~/ios/app-fundamentals/images-icons/launch-screens.md) 표시 됩니다. 응용 프로그램이 로드 될 때 시작 화면이 표시 되기 때문에 응용 프로그램이 아직 메모리로 로드 되 고 있으므로 코드에서 만들 수 없습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 iOS 프로젝트를 만드는 경우 xib 파일 형식으로 실행 화면이 제공 됩니다 .이 파일은 프로젝트 내의 **Resources** 폴더에서 찾을 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac용 Visual Studio에서 iOS 프로젝트를 만드는 경우에는 스토리 보드 파일 형식으로 실행 화면이 제공 됩니다.

-----

이를 두 번 클릭 하 고 iOS 디자이너에서 열어 편집할 수 있습니다.

Apple은 iOS 8 이상을 대상으로 하는 응용 프로그램에 xib 또는 스토리 보드 파일을 사용 하는 것을 권장 합니다. iOS 디자이너에서 두 파일을 시작할 때 크기 클래스와 자동 레이아웃을 사용 하 여 모든 장치에 대해 제대로 표시 되 고 올바르게 표시 되도록 레이아웃을 조정 합니다. 조정. 이전 버전을 대상으로 하는 응용 프로그램을 지원할 수 있도록 xib 또는 Storyboard 외에 정적 시작 이미지를 사용할 수 있습니다.

시작 화면을 만드는 방법에 대 한 자세한 내용은 아래 문서를 참조 하세요.

- [. Xib를 사용 하 여 시작 화면 만들기](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)
- [Storyboard를 사용 하 여 시작 화면 관리](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> IOS 9부터 Apple에서는 Storyboard를 시작 화면을 만드는 기본 방법으로 사용 하는 것이 좋습니다.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>IOS 전 8 개 응용 프로그램에 대 한 시작 이미지 만들기

응용 프로그램이 iOS 8 이전 버전을 대상으로 하는 경우 xib 또는 Storyboard 시작 화면 외에 정적 이미지를 사용할 수 있습니다.

이 정적 이미지는 info.plist 파일에서 설정 하거나 응용 프로그램에서 자산 카탈로그 (iOS 7의 경우)로 설정할 수 있습니다. 응용 프로그램이 실행 될 수 있는 각 장치 크기 (320x480, 640x960, 640x1136)에 대 한 별도의 이미지를 제공 해야 합니다. 시작 화면 크기에 대 한 자세한 내용은 [화면 이미지 시작](~/ios/app-fundamentals/images-icons/launch-screens.md) 가이드를 참조 하세요.

> [!IMPORTANT]
> 앱에 시작 화면이 없으면 화면이 완전히 일치 하지 않는 것을 알 수 있습니다. 이 경우 info.plist에 `Default-568@2x.png` 이라는 이름의 640x1136 이미지를 최소한 포함 해야 합니다.

## <a name="summary"></a>요약

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

이 문서에서는 Visual Studio에서 프로그래밍 방식으로 iOS 응용 프로그램을 개발 하는 방법을 설명 했습니다. 빈 프로젝트 템플릿에서 프로젝트를 빌드하는 방법에 대해 설명 하 고 루트 뷰 컨트롤러를 만들어 창에 추가 하는 방법을 설명 합니다. 그런 다음 UIKit의 컨트롤을 사용 하 여 응용 프로그램 화면을 개발 하는 컨트롤러 내에서 뷰 계층 구조를 만드는 방법을 살펴보았습니다. 다음으로 뷰를 다른 방향으로 적절 하 게 배치 하는 방법을 검토 하 고 컨트롤러 내에서 뷰를 로드 하는 방법 뿐만 아니라 `UIView`를 서브클래싱 하 여 사용자 지정 보기를 만드는 방법을 살펴보았습니다. 마지막으로 응용 프로그램에 시작 화면을 추가 하는 방법을 살펴보았습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이 문서에서는 Mac용 Visual Studio에서 프로그래밍 방식으로 iOS 응용 프로그램을 개발 하는 방법을 설명 했습니다. 단일 뷰 템플릿에서 프로젝트를 빌드하는 방법에 대해 살펴보고 루트 뷰 컨트롤러를 만들어 창에 추가 하는 방법을 설명 합니다. 그런 다음 UIKit의 컨트롤을 사용 하 여 응용 프로그램 화면을 개발 하는 컨트롤러 내에서 뷰 계층 구조를 만드는 방법을 살펴보았습니다. 다음으로 뷰를 다른 방향으로 적절 하 게 배치 하는 방법을 검토 하 고 컨트롤러 내에서 뷰를 로드 하는 방법 뿐만 아니라 `UIView`를 서브클래싱 하 여 사용자 지정 보기를 만드는 방법을 살펴보았습니다. 마지막으로 응용 프로그램에 시작 화면을 추가 하는 방법을 살펴보았습니다.

-----

## <a name="related-links"></a>관련 링크

- [SimpleLogin (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplelogin)
