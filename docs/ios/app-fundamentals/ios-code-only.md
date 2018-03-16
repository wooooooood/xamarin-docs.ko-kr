---
title: "코드에서 iOS 사용자 인터페이스 만들기"
description: "Xamarin.iOS 앱에 대 한 – 또는 코드를 iOS 용 Xamarin 디자이너를 사용 하 여 사용자 인터페이스를 만드는 두 가지 방법을 제공 합니다. 이 문서 전체를 코드로 iOS 사용자 인터페이스를 만드는 방법을 검사 합니다. 컨트롤러에는 응용 프로그램 화면 UIKit에서 보기의 계층 구조를 만들어 만들려는 프로젝트 템플릿으로 시작 하는 방법을 보여 줍니다. 그런 다음는 컨트롤러에서 로드할 수 있는 사용자 지정 보기를 만드는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: d53dea1a46c6b42f901beb217eb00b3a3fa0fd92
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/16/2018
---
# <a name="creating-ios-user-interfaces-in-code"></a>코드에서 iOS 사용자 인터페이스 만들기

_Xamarin.iOS 앱에 대 한 – 또는 코드를 iOS 용 Xamarin 디자이너를 사용 하 여 사용자 인터페이스를 만드는 두 가지 방법을 제공 합니다. 이 문서 전체를 코드로 iOS 사용자 인터페이스를 만드는 방법을 검사 합니다. 컨트롤러에는 응용 프로그램 화면 UIKit에서 보기의 계층 구조를 만들어 만들려는 프로젝트 템플릿으로 시작 하는 방법을 보여 줍니다. 그런 다음는 컨트롤러에서 로드할 수 있는 사용자 지정 보기를 만드는 방법을 설명 합니다._

IOS 앱의 사용자 인터페이스는는 storefront와 – 응용 프로그램은 일반적으로 한 창의 가져옵니다 하지만 것으로 채울 수 창에 많은 개체가 필요 하 고 개체와 배열을 표시 하려고 하는 응용 프로그램에 따라 변경할 수 있습니다. 사용자에게 표시되는 항목인 이 시나리오의 개체를 뷰라고 합니다. 응용 프로그램에서 단일 화면을 빌드합니다 뷰는 서로 위에 쌓입니다 콘텐츠 뷰 계층 구조에서 및 단일 보기 컨트롤러에서 관리 하는 계층입니다. 여러 화면이 있는 응용 프로그램은 각각 고유한 뷰 컨트롤러가 있는 여러 콘텐츠 뷰 계층 구조가 있으며, 응용 프로그램은 사용자가 보는 화면에 따라 다른 콘텐츠 뷰 계층 구조를 만들도록 창에 뷰를 배치합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

다음 다이어그램에는 장치 화면에 사용자 인터페이스를 가져오는 창, 뷰, 하위 뷰 및 뷰 컨트롤러 간의 관계가 나와 있습니다. 

[![](ios-code-only-images/image9.png "이 다이어그램 창, 뷰, 하위 뷰가, 및 뷰-컨트롤러 간의 관계를 보여 줍니다.")](ios-code-only-images/image9.png#lightbox)

그러나 사용 하 여 이러한 보기 계층을 생성할 수는 [iOS에 대 한 Xamarin 디자이너](~/ios/user-interface/designer/index.md) Visual Studio에서 좋네요 코드에서 완전히 작동 하는 방법에 대 한 기본적인 이해 합니다. 이 문서 코드 전용 사용자 인터페이스 개발 실행 되 고 작동 하는 기본 가리키는 몇 가지 안내 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

다음 다이어그램에는 장치 화면에 사용자 인터페이스를 가져오는 창, 뷰, 하위 뷰 및 뷰 컨트롤러 간의 관계가 나와 있습니다. 

[![](ios-code-only-images/image9.png "이 다이어그램 창, 뷰, 하위 뷰가, 및 뷰-컨트롤러 간의 관계를 보여 줍니다.")](ios-code-only-images/image9.png#lightbox)


그러나 사용 하 여 이러한 보기 계층을 생성할 수는 [iOS에 대 한 Xamarin 디자이너](~/ios/user-interface/designer/index.md) Mac 용 Visual Studio에서 좋네요 코드에서 완전히 작동 하는 방법에 대 한 기본적인 이해 합니다. 이 문서 코드 전용 사용자 인터페이스 개발 실행 되 고 작동 하는 기본 가리키는 몇 가지 안내 합니다.


-----

## <a name="creating-a-code-only-project"></a>코드 전용 프로젝트 만들기

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="ios-blank-project-template"></a>iOS 빈 프로젝트 템플릿

첫째, iPhone을 사용 하 여 Visual Studio에서 iOS 프로젝트를 만들 **빈 프로젝트** 서식 파일을 아래 표시 된는 컨트롤러와 뷰를 추가 하려면 확장 합니다.


[![](ios-code-only-images/blankapp-vs.png "새 프로젝트 대화 상자")](ios-code-only-images/blankapp-vs.png#lightbox)


빈 프로젝트 템플릿을 프로젝트에 4 개 파일을 추가합니다.


[![](ios-code-only-images/empty-project.png "프로젝트 파일")](ios-code-only-images/empty-project.png#lightbox)


1. **AppDelegate.cs** -를 포함 한 `UIApplicationDelegate` 하위 클래스 `AppDelegate` , iOS에서 응용 프로그램 이벤트를 처리 하는 데 사용 되는 합니다. 응용 프로그램 창에 만들어집니다는 `AppDelegate`의 `FinishedLaunching` 메서드.
1. **Main.cs** -클래스를 지정 하는 응용 프로그램에 대 한 진입점을 포함에 대 한는 `AppDelegate` 합니다.
1. **Info.plist** -응용 프로그램 구성 정보를 포함 하는 속성 목록 파일입니다.
1. **Entitlements.plist** – 기능 및 응용 프로그램의 권한에 대 한 정보를 포함 하는 속성 목록 파일입니다.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="ios-templates"></a>iOS 템플릿


Mac 용 visual Studio에는 빈 템플릿을 제공 하지 않습니다. 모든 서식 파일에는 UI를 만드는 기본 방법으로 Apple 권장 하는 스토리 보드 지원을 제공 합니다. 그러나, 코드에서 완전히 UI를 만들 수는 있습니다. 

다음 단계 응용 프로그램에서 스토리 보드를 제거 하는 과정을 안내 합니다. 


1. 단일 보기 응용 프로그램 템플릿을 사용 하 여 새 iOS 프로젝트를 만들려면:
    
    [![](ios-code-only-images/single-view-app.png "단일 보기 응용 프로그램 템플릿을 사용 하 여")](ios-code-only-images/single-view-app.png#lightbox)

1. 삭제 된 `Main.Storyboard` 및 `ViewController.cs` 파일입니다. 수행 **하지** 삭제는 `LaunchScreen.Storyboard`합니다. 스토리 보드에서 만들어진 보기 컨트롤러에 대 한 코드 숨김 그대로 뷰 컨트롤러를 삭제 해야 합니다.
1. 선택 되어 있는지 확인 **삭제** 팝업 대화 상자에서:
    
    [![](ios-code-only-images/delete.png "팝업 대화 상자에서 삭제를 선택 합니다.")](ios-code-only-images/delete.png#lightbox)

1. Info.plist에서 내부 정보를 삭제는 **배포 정보 > 주 인터페이스** 옵션:
    
    [![](ios-code-only-images/main-interface.png "주 인터페이스 옵션에 포함 된 정보를 삭제 합니다.")](ios-code-only-images/main-interface.png#lightbox)

1. 마지막으로, 다음 코드를 추가 하면 `FinishedLaunching` AppDelegate 클래스의 메서드:
        
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            // create a new window instance based on the screen size
            window = new UIWindow(UIScreen.MainScreen.Bounds);

            // make the window visible
            window.MakeKeyAndVisible();

            return true;
        }


-----

## <a name="creating-a-window"></a>창 만들기

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

에 추가 된 코드는 `FinishedLaunching` 위의 3 단계에는 iOS 응용 프로그램에 대 한 창을 만드는 데 필요한 코드의 최소 크기입니다.  

-----

iOS 응용 프로그램을 사용 하 여 빌드됩니다는 [MVC 패턴](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#Model_View_Controller)합니다. 응용 프로그램에 표시 되는 첫 번째 화면 창의 루트 뷰-컨트롤러에서 만들어집니다. 참조는 [Hello, iOS 멀티 스크린](~/ios/get-started/hello-ios-multiscreen/index.md) MVC에 대 한 자세한 내용은 패턴 자체에 대 한 안내 합니다.

에 대 한 구현을 `AppDelegate`에 의해 추가 서식 파일을 만듭니다는 응용 프로그램 창의 며는 모든 iOS 응용 프로그램에 대 한 하나의 하 고 다음 코드로 표시 되도록 합니다.

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

이 응용 프로그램을 지금 실행 한다면 가능성이 얻을 수 내용을 입력 하는 throw 된 예외 `Application windows are expected to have a root view controller at the end of application launch`합니다. 컨트롤러를 추가 하 고 응용 프로그램의 루트 뷰-컨트롤러 확인 살펴보겠습니다.

## <a name="adding-a-controller"></a>컨트롤러 추가

앱에는 여러 컨트롤러 보기 포함 될 수 있습니다 하지만 모든 보기 컨트롤러를 제어 하려면 루트 뷰 컨트롤러가 포함 해야 합니다.  컨트롤러를 만들어 창에 추가 `UIViewController` 인스턴스 속성을 설정 하 고는 `window.RootViewController` 속성:

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

모든 컨트롤러에서 액세스할 수 있는 관련된 보기에는 `View` 속성입니다. 위의 코드에서는 변경 보기의 `BackgroundColor` 속성을 `UIColor.LightGray` 를 아래와 같이 표시 됩니다.

 [![](ios-code-only-images/image1.png "보기의 배경을 표시 밝은 회색은")](ios-code-only-images/image1.png#lightbox)

모든 설정 수 `UIViewController` 으로 하위 클래스는 `RootViewController` 이러한 방식으로 포함 하 여 컨트롤러에서 도메인 UIKit 직접 작성 하는 것입니다. 예를 들어 다음 코드는 추가 `UINavigationController` 로 `RootViewController`:

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

아래와 같이 탐색 컨트롤러 내에 중첩 된 컨트롤러를 생성 합니다.

 [![](ios-code-only-images/image2.png "탐색 컨트롤러 내에 중첩 된 컨트롤러")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>보기 컨트롤러 만들기

으로 컨트롤러를 추가 하는 방법을 살펴보았습니다 했으므로 `RootViewController` 창의 코드에서 사용자 지정 보기 컨트롤러를 만드는 방법을 확인해 보겠습니다.

라는 새 클래스를 추가 `CustomViewController` 아래와 같이:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](ios-code-only-images/customviewcontroller.png "CustomViewController 라는 새 클래스 추가")](ios-code-only-images/customviewcontroller.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](ios-code-only-images/new-file.png "CustomViewController 라는 새 클래스 추가")](ios-code-only-images/new-file.png#lightbox)

-----

클래스에서 상속 해야 `UIViewController`에 `UIKit` 표시 된 대로 네임 스페이스:

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

<a name="Initializing_the_View"/>

## <a name="initializing-the-view"></a>뷰 초기화

`UIViewController` 라는 메서드를 포함 `ViewDidLoad` 뷰 컨트롤러 메모리에 처음 로드할 때 호출 되 합니다. 보기의 속성을 설정 하는 등의 초기화 작업을 수행 하는 데는 적합 한 곳입니다.

예를 들어 다음 코드는 단추와 이벤트 처리기는 단추를 누를 때 탐색 스택에 새 뷰 컨트롤러 푸시를 추가 합니다.

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

응용 프로그램에서이 컨트롤러를 로드 하 고 간단한 탐색 시연, 하려면의 새 인스턴스를 만듭니다 `CustomViewController`합니다. 새 탐색 컨트롤러 만들기, 보기 컨트롤러 인스턴스를 전달 및 창에 새 탐색 컨트롤러를 설정 `RootViewController` 에 `AppDelegate` 이전과:

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

이제 로드 될 때 응용 프로그램의 `CustomViewController` 탐색 컨트롤러 내 로드 됩니다.

 [![](ios-code-only-images/customvc.png "탐색 컨트롤러 내에서 CustomViewController 로드")](ios-code-only-images/customvc.png#lightbox)
 
단추를 클릭 합니다 _푸시_ 탐색 스택에 새 뷰 컨트롤러:

[![](ios-code-only-images/customvca.png "탐색 스택에 밀어 넣은 새 뷰 컨트롤러")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>뷰 계층 구조를 작성합니다.

위의 예에서 인터페이스를 만드는 사용자 코드에서 단추 뷰 컨트롤러를 추가 하 여 시작 합니다.

iOS 사용자 인터페이스 계층 구조 보기 이루어져 있습니다. 추가 보기, 레이블, 단추, 슬라이더, 등과 같은 일부 부모 보기의 하위로 추가 됩니다.

예를 들어 편집는 `CustomViewController` 사용자 사용자 이름 및 암호를 입력할 수 있는 로그인 화면을 만듭니다. 화면에 두 개의 텍스트 필드와 단추로 단계로 구성 됩니다.

### <a name="adding-the-text-fields"></a>텍스트 필드 추가

첫째,에 추가 된 단추와 이벤트 처리기를 제거는 [보기 초기화](#Initializing_the_View) 섹션. 

만들고 초기화 하 여 사용자 이름에 대 한 컨트롤을 추가할는 `UITextField` 사용한 후 다음과 같이 뷰 계층 구조를 추가 합니다.

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

만들 때는 `UITextField`를 설정 하는 이유는 `Frame` 위치 및 크기를 정의 하는 속성입니다. IOS 0, 0 좌표는 왼쪽 위에 있는 + x 오른쪽 및 + y 다운 합니다. 설정한 후는 `Frame` 이라고 속성과 다른 몇 가지 속성을 `View.AddSubview` 추가 하는 `UITextField` 보기 계층에 있습니다. 이렇게 하면는 `usernameField` 의 하위 뷰는 `UIView` 인스턴스가 `View` 속성 참조 합니다. 이 화면에는 부모 뷰의 앞에 나타나도록 해당 부모 뷰 보다 높은 z 순서를 하위 뷰 추가 됩니다.

응용 프로그램을는 `UITextField` 포함 아래에 표시 됩니다.

 [![](ios-code-only-images/image4.png "포함 된 UITextField로 응용 프로그램")](ios-code-only-images/image4.png#lightbox)

추가할 수 있습니다는 `UITextField` 유사한 방식으로 암호를이 경우에만 설정는 `SecureTextEntry` 속성을 아래와 같이 true로:

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

설정 `SecureTextEntry = true` 에 입력 한 텍스트를 숨기는 `UITextField` 아래와 같이 사용자가:

 [![](ios-code-only-images/image4a.png "True는 사용자가 입력 한 텍스트를 숨기려면 SecureTextEntry 설정")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>단추 추가

다음으로, 사용자 사용자 이름 및 암호를 전송할 수 있도록 단추를 추가 합니다. 단추는 부모 뷰에 대 한 인수로 전달 하 여 다른 컨트롤 처럼 뷰 계층 구조에 추가 됩니다 `AddSubview` 메서드를 다시 합니다.

다음 코드는 단추를 추가 하 고에 대 한 이벤트 처리기를 등록 된 `TouchUpInside` 이벤트:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

로그인 화면 위치에이 아래와 같이 이제 나타납니다.

 [![](ios-code-only-images/image5.png "로그인 화면")](ios-code-only-images/image5.png#lightbox)

와 달리 이전 버전 iOS의 기본 단추 배경은 투명 합니다. 단추의 변경 `BackgroundColor` 이 속성이 변경:

```csharp
submitButton.BackgroundColor = UIColor.White;
```

이 인해 보다 일반적인 둥근 가장자리 있는 모양된 단추는 정사각형 단추 발생 합니다. 둥근된 가장자리를 가져오려면 다음 코드 조각을 사용 합니다.

```csharp
submitButton.Layer.CornerRadius = 5f;
```

이러한 변경 내용으로 뷰는 다음과 같이 표시 됩니다.

[![](ios-code-only-images/image6.png "보기의 한 예 실행")](ios-code-only-images/image6.png#lightbox)
 
## <a name="adding-multiple-views-to-the-view-hierarchy"></a>보기 계층에 뷰 여러 개 추가

여러 뷰를 사용 하 여 계층 구조 보기를 추가 하는 기능을 제공 하는 iOS `AddSubviews`합니다.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>단추 기능 추가

단추를 클릭할 때 사용자에 게 필요 특정 작업이 실행 합니다. 예를 들어 경고를 표시할지 탐색 다른 화면으로 수행 됩니다. 

두 번째 보기 컨트롤러 탐색 스택의에 적용할 몇 가지 코드를 추가 해 보겠습니다.

첫 번째, 두 번째 보기 컨트롤러를 만듭니다.

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

그런 다음에 기능을 추가 `TouchUpInside` 이벤트:

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

탐색 아래 그림에 나와 있습니다.

[![](ios-code-only-images/navigation.png "이 차트 탐색 보여 줍니다.")](ios-code-only-images/navigation.png#lightbox)

기본적으로 탐색 컨트롤러를 사용 하는 경우 iOS 응용 프로그램에 제공 탐색 모음 및 사용자 스택을 통해 다시 이동할 수 있도록 뒤로 단추를 확인 합니다.

## <a name="iterating-through-the-view-hierarchy"></a>뷰 계층 구조 반복

하위 뷰 계층 구조를 통해 반복 하 고 특정 보기 중 하나를 선택 하는 것이 불가능 합니다. 예를 들어 각각을 찾을 수 `UIButton` 주며 해당 단추의 다른 `BackgroundColor`, 다음 코드 조각 수 있습니다.

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

그러나이 작동 하지 것입니다 보기에 대해 반복 되는 경우는 `UIView` 모든 보기 다시 행사로 나올지 대로 `UIView` 상속 자체는 부모 뷰에 추가 된 개체와 `UIView`합니다.

## <a name="handling-rotation"></a>처리 회전

가로 방향으로 장치를 회전할 하는 경우 컨트롤 크기가 조정 되지 않습니다 적절 하 게 다음 스크린 샷에서 같이:

 [![](ios-code-only-images/image7.png "가로 방향으로 장치를 회전할 하는 경우 컨트롤 크기가 조정 되지 않습니다 적절 하 게")](ios-code-only-images/image7.png#lightbox)

이 문제를 해결 하는 방법을 설정 하는 것은 `AutoresizingMask` 각 보기에는 속성입니다. 이 경우에 있게 하려면이 컨트롤을 수평으로 확장 하도록 각 설정할 것 `AutoresizingMask`합니다. 다음 예제에 대 한는 `usernameField`, 있으 나 동일한 들은 뷰 계층 구조에서 각 가젯에 적용 될 수 있습니다.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

이제 장치 또는 시뮬레이터를 회전에서는 모든 확장을 채우는 추가 공간을 아래와 같이:

 [![](ios-code-only-images/image8.png "모든 컨트롤을 추가 공간을 채우도록 확장")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>사용자 지정 보기 만들기

UIKit의 일부인 컨트롤을 사용 하는 것 외에도 사용자 지정 보기 사용할 수도 있습니다. 상속 하 여 사용자 지정 보기를 만들 수 있습니다 `UIView` 재정의 `Draw`합니다. 사용자 지정 보기를 만들고 보여 주기 위해 계층 구조 보기에 추가 보겠습니다.

### <a name="inheriting-from-uiview"></a>UIView에서 상속

가장 먼저 해야 할 경우 사용자 지정 보기에 대 한 클래스 만들기 사용 하 여 수행 합니다 우리는 **클래스** 이라는 빈 클래스로 추가 하려면 Visual Studio에서 템플릿을 `CircleView`합니다. 기본 클래스 설정 해야 `UIView`, 우리 회수 중인는 `UIKit` 네임 스페이스입니다. 필요 합니다는 `System.Drawing` 네임 스페이스에도 합니다. 다른 다양 한 `System.*` 네임 스페이스 됩니다이 예제에서 사용 되므로 자유롭게를 제거 합니다.

클래스는 다음과 같이 표시 됩니다.

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>UIView에 그리기

모든 `UIView` 에 `Draw` 을 그릴 수 있도록 해야 하는 경우 시스템에서 호출 되어 메서드. `Draw` 호출 되 면 안 직접 합니다. 실행된 루프 처리 하는 동안 시스템에 의해 호출 됩니다. 뷰 계층 구조 보기에 추가 된 후 실행된 루프를 통해 처음으로 해당 `Draw` 메서드를 호출 합니다. 에 대 한 후속 호출 `Draw` 뷰 중 하나를 호출 하 여 그려야 하는 것으로 표시 하면 발생 `SetNeedsDisplay` 또는 `SetNeedsDisplayInRect` 보기.

재정의 된 내부에서 이러한 코드를 추가 하 여이 보기에 그리기 코드를 추가할 수 있습니다 `Draw` 아래와 같이 메서드:

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

이후 `CircleView` 은 `UIView`도 설정할 수 있습니다 `UIView` 속성도 있습니다. 예를 들어, 설정할 수 있습니다는 `BackgroundColor` 생성자에서:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

사용 하는 `CircleView` 방금 만든, 우리 하거나에 추가할 수 하위 뷰도는 기존 컨트롤러의 보기 계층 구조와 동일한 방식으로 `UILabels` 및 `UIButton` 앞에서 새 컨트롤러의 보기를 로드할 수 있습니다. 후자 보겠습니다.

### <a name="loading-a-view"></a>뷰를 로드합니다.

 `UIViewController` 라는 메서드가 `LoadView` 해당 보기를 만드는 컨트롤러에서 라고 합니다. 이 보기를 만들 하는 컨트롤러에 할당 된 적절 한 위치 `View` 속성입니다.

먼저 컨트롤러를 따라서 라는 새 빈 클래스를 만들 `CircleController`합니다.

`CircleController` 설정 하려면 다음 코드를 추가 `View` 에 `CircleView` (호출 하지 않아야는 `base` 재정의에서 구현):

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

마지막으로 컨트롤러를 런타임에 제공 해야 합니다. 이전에 다음과 같이 추가 하 고 전송 단추에 이벤트 처리기를 추가 하 여이 수행 하겠습니다.

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

이제 응용 프로그램을 실행 하 고 전송 단추를 탭, 원이 있는 새 보기가 표시 됩니다.

 [![](ios-code-only-images/circles.png "새 보기가 원으로 표시 됩니다.")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>시작 화면 만들기

A [실행 화면](~/ios/app-fundamentals/images-icons/launch-screens.md) 앱 장치가 응답 하는지를 사용자에 게 표시 하는 방법으로 시작 될 때 표시 됩니다. 앱을 로드 하는 경우는 시작 화면이 표시 되 면 때문에 응용 프로그램은 아직 메모리에 로드 되 고 대로 코드에서 다시 만들 수 없습니다 것입니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

경우에 Visual Studio에서는 시작 화면에서에서 프로젝트를 자동으로 제공 됩니다.xib 파일에서 찾을 수 있는 형태로 iOS 만들기는 **리소스** 프로젝트 내부에서 폴더입니다. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

경우에 Mac, 시작 화면에서 스토리 보드 파일 형식으로 제공 됩니다에 대 한 Visual Studio에서 iOS 프로젝트를 만듭니다. 

-----

이 두 번 클릭 하 여에 iOS 디자이너에서에서 열어 편집할 수 있습니다.

Apple는.xib 또는 스토리 보드 파일 iOS 8을 대상으로 하는 응용 프로그램에 사용 되는 또는 이상 버전에서는 iOS 디자이너에서에서 파일 중 하나를 시작할 때 사용 하면 크기 클래스와 자동 레이아웃은 하 고 모든 장치에 대 한 올바르게 표시 됩니다. 있도록 레이아웃을 조정 하는 것이 좋습니다. 크기입니다. 정적 시작 이미지 이전 버전을 대상으로 하는 응용 프로그램에 사용할 수 있도록.xib 또는 스토리 보드 외에도 사용할 수 있습니다.

시작 화면을 만드는 방법에 대 한 자세한 내용은 아래 문서를 참조 합니다.

- [.xib을 사용 하 여 시작 화면 만들기](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)
- [스토리 보드가 있는 관리 시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> **참고:** Apple iOS 9 기준으로 스토리 보드의 시작 화면을 만들기 위한 기본 방법으로 사용 해야 함을 권장 합니다.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>사전 iOS에 대 한 시작 이미지 8 응용 프로그램 만들기

응용 프로그램 이전 버전과 달라진 iOS 8 대상으로 하는 경우 정적 이미지.xib 또는 스토리 보드 실행 화면 외에도 사용할 수 있습니다. 

이 정적 이미지 Info.plist 파일 또는 응용 프로그램에서 (iOS 7)에 대 한 자산 카탈로그도 설정할 수 있습니다. 응용 프로그램에서 실행 될 수 있는 각 장치 크기 (예: 320 x 480, 640 x 960, 640 x 1136)에 대 한 별도 이미지를 제공 해야 합니다. 시작 화면 크기에 자세한 내용은 참조는 [시작 화면 이미지 등](~/ios/app-fundamentals/images-icons/launch-screens.md) 가이드입니다.

> [!IMPORTANT]
> **참고:** 응용 프로그램에 없는 시작 화면, 화면을 맞지 않으면 완벽 하 게 표시 될 수도 있습니다. 이 경우 라는 640 x 1136 이미지에 포함 되었는지 확인 해야 `Default-568@2x.png` 프로그램 Info.plist에 있습니다. 



## <a name="summary"></a>요약

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

이 문서에 프로그래밍 방식으로 Visual Studio에서 iOS 응용 프로그램을 개발 하는 방법을 설명 합니다. 살펴보았습니다 빈 프로젝트 템플릿에서 프로젝트를 빌드하는 방법을 만들고 창에는 루트 보기 컨트롤러를 추가 하는 방법에 논의 합니다. 응용 프로그램 화면을 개발 하는 컨트롤러 내에서 뷰 계층을 만들려면 UIKit에서 컨트롤을 사용 하는 방법을 살펴보았습니다. 어떻게 뷰를 적절 하 게 다른 방향으로의 레이아웃과 서브클래싱 하 여 사용자 지정 보기를 만드는 방법에 살펴보았습니다 다음 검사 했습니다 `UIView`방법, 컨트롤러 내에서 뷰를 로드 합니다. 마지막으로 응용 프로그램에 실행 화면을 추가 하는 방법에서 살펴본 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

이 문서 설명 Mac.에 대 한 프로그래밍 방식으로 Visual Studio에서 iOS 응용 프로그램을 개발 하는 방법 보이는 것 처럼 단일 보기 템플릿에서 프로젝트를 빌드하는 방법을 만들고 창에는 루트 보기 컨트롤러를 추가 하는 방법에 논의 합니다. 응용 프로그램 화면을 개발 하는 컨트롤러 내에서 뷰 계층을 만들려면 UIKit에서 컨트롤을 사용 하는 방법을 살펴보았습니다. 어떻게 뷰를 적절 하 게 다른 방향으로의 레이아웃과 서브클래싱 하 여 사용자 지정 보기를 만드는 방법에 살펴보았습니다 다음 검사 했습니다 `UIView`방법, 컨트롤러 내에서 뷰를 로드 합니다. 마지막으로 응용 프로그램에 실행 화면을 추가 하는 방법에서 살펴본 합니다.

-----




## <a name="related-links"></a>관련 링크

- [SimpleLogin (샘플)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
