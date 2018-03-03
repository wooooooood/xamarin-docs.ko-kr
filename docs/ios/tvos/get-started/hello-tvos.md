---
title: "Hello, tvOS 빠른 시작 가이드"
description: "이 가이드는 첫 번째 Xamarin.tvOS 앱 및 해당 개발 도구 체인을 만들어 안내 합니다. 또한 UI 컨트롤, 코드에 표시 빌드, 실행 및 Xamarin.tvOS 응용 프로그램을 테스트 하는 방법을 설명 하는 Xamarin 디자이너를 제공 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/02/2018
ms.openlocfilehash: fe58aa8ffb74a9b6e937be5a7f1dde0432794405
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="hello-tvos-quick-start-guide"></a>Hello, tvOS 빠른 시작 가이드

_이 가이드는 첫 번째 Xamarin.tvOS 앱 및 해당 개발 도구 체인을 만들어 안내 합니다. 또한 UI 컨트롤, 코드에 표시 빌드, 실행 및 Xamarin.tvOS 응용 프로그램을 테스트 하는 방법을 설명 하는 Xamarin 디자이너를 제공 합니다._

Apple의 Apple TV, Apple TV 4k, tvOS 11 실행 되는 5 번째 세대를 출시 했습니다.

Apple TV 플랫폼은 다양 한 앱을 만들고 Apple TV 기본 제공 앱 스토어를 통해 해제할 수 있도록 개발자에 게 열기.

Xamarin.iOS 개발에 익숙한 경우 찾아야 tvOS로의 전환을 매우 간단 합니다. 그러나 대부분의 Api 및 기능을 동일 합니다. 대부분의 일반적인 Api는 (예: WebKit) 사용할 수 없습니다. 또한 작업에서 Siri 원격와 되지만 터치 스크린을 기반으로 iOS 장치에 존재 하지 않는 몇 가지 디자인 문제가 발생 합니다.

이 가이드는 tvOS Xamarin 앱에서 작업에 대 한 소개를 제공 합니다. TvOS에 대 한 자세한 내용은 Apple의를 참조 하십시오 [4k Apple TV에 대 한 준비](https://developer.apple.com/tvos/) 설명서입니다.

## <a name="overview"></a>개요

Xamarin.tvOS C# 및.NET에서 개발할 때 사용 되는 인터페이스 컨트롤과 동일한 OS X 라이브러리를 사용 하 여 완전 한 네이티브 Apple TV 앱을 개발할 수 *Swift* (또는 *Objective-c*) 및 *Xcode*합니다.

또한 Xamarin.tvOS 앱이 C# 및.NET으로 작성 된, 이후 일반적인 백 엔드 코드와 공유할 수 Xamarin.iOS, Xamarin.Android 및 Xamarin.Mac 앱; 기본 제공 하는 동시에 모든 각 플랫폼에서 발생 합니다.

이 문서 Xamarin.tvOS 및 Visual Studio를 사용 하 여 기본 구축 프로세스를 통해 Apple TV 응용 프로그램을 만드는 데 필요한 핵심 개념을 소개 합니다 **Hello, tvOS** 횟수를 계산 하는 응용 프로그램 단추에 클릭 합니다.

[ ![](hello-tvos-images/run05.png "예제 앱 실행")](hello-tvos-images/run05.png)

다음과 같은 개념을 설명 합니다.

-  **Mac 용 visual Studio** – Mac 및 Xamarin.tvOS 응용 프로그램을 만드는 방법에 대 한 Visual Studio에 소개 합니다.
-  **Xamarin.tvOS 앱 분석** – 어떤 정도 Xamarin.tvOS 앱으로 이루어져 있습니다.
-  **사용자 인터페이스 만들기** -사용자 인터페이스를 만드는 iOS에 대 한 Xamarin 디자이너를 사용 하는 방법입니다.
-  **배포 및 테스트** – 실행 하 고 실제 tvOS 하드웨어와 tvOS 시뮬레이터에서에서 앱을 테스트 하는 방법입니다.

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Mac 용 Visual Studio에서 새 Xamarin.tvOS 앱 시작

Apple TV 응용 프로그램이 호출한 만들겠습니다 위에서 설명한 대로 `Hello-tvOS` 단일 단추 및 레이블을 주 화면에 추가 하 합니다. 단추를 클릭하면 레이블에 단추 클릭 횟수가 표시됩니다.

시작 하려면 다음을 수행 하겠습니다.

1. Mac용 Visual Studio 시작:

    [ ![](hello-tvos-images/setup01.png "Visual Studio for Mac")](hello-tvos-images/setup01.png)
2. 클릭는 **새 솔루션 중...**  링크를 열려면 화면의 왼쪽 위 모서리에는 **새 프로젝트** 대화 상자.
3. 선택 **tvOS** > **앱** > **단일 앱 보기** 클릭는 **다음** 단추:

    [ ![](hello-tvos-images/setup02.png "단일 보기는 앱 선택")](hello-tvos-images/setup02.png)
4. 입력 `Hello, tvOS` 에 대 한는 **응용 프로그램 이름**를 입력 하면 **조직 식별자** 클릭는 **다음** 단추:

    [ ![](hello-tvos-images/setup04.png "Enter Hello, tvOS")](hello-tvos-images/setup04.png)
5. 입력 `Hello_tvOS` 에 대 한는 **프로젝트 이름** 클릭는 **만들기** 단추:

    [ ![](hello-tvos-images/setup03.png "HellotvOS 입력")](hello-tvos-images/setup03.png)

Mac 용 visual Studio 새 Xamarin.tvOS 앱 만들고 응용 프로그램의 솔루션에 추가 되는 기본 파일 표시 됩니다.

 [ ![](hello-tvos-images/project01.png "기본 파일 보기")](hello-tvos-images/project01.png)

사용 하 여 Mac에 대 한 visual Studio **솔루션** 및 **프로젝트**와 정확히 같은 방식으로 Visual Studio에서 합니다. 솔루션은 하나 이상의 프로젝트를 보관할 수 있는 컨테이너이고, 프로젝트는 응용 프로그램, 지원 라이브러리, 테스트 응용 프로그램 등을 포함할 수 있습니다. 이 경우 Mac 용 Visual Studio가 생성 한 솔루션 및 응용 프로그램 프로젝트 모두.

를 하려는 경우 일반적으로 공유 코드를 포함 하는 라이브러리 프로젝트 하나 이상의 코드를 만들 수 있습니다. 이러한 라이브러리 프로젝트를 응용 프로그램 프로젝트에서 사용 하거나 다른 Xamarin.tvOS 앱 프로젝트와 공유할 수 (또는 Xamarin.iOS, Xamarin.Android 및 Xamarin.Mac 코드의 형식에 따라) 표준.NET 응용 프로그램을 빌드하는 경우와 동일한 방식으로 합니다.

## <a name="anatomy-of-a-xamarintvos-app"></a>Xamarin.tvOS 응용 프로그램 분석

IOS 프로그래밍에 익숙한 경우 많은 유사성 여기 알 수 있습니다. 사실, tvOS 9 개념 많은 여기 교차 하므로 iOS 9의 일부입니다.

프로젝트의 파일에 살펴보겠습니다.

-   `Main.cs` - 앱의 진입점을 포함합니다. 앱이 실행되면 첫 번째 클래스와 실행되는 메서드를 포함합니다.
-   `AppDelegate.cs` –이 파일 운영 체제에서 이벤트를 수신 하는 일을 담당 하는 기본 응용 프로그램 클래스를 포함 합니다.
-   `Info.plist` –이 파일이 응용 프로그램 이름, 아이콘 등과 같은 응용 프로그램 속성을 포함합니다.
-   `ViewController.cs` -주 창을 나타내는 것의 수명 주기를 제어 하는 클래스입니다.
-   `ViewController.designer.cs` –이 파일에는 기본 화면 사용자 인터페이스와 통합 하는 데 도움이 되는 내부 작업 코드가 포함 됩니다.
-  `Main.storyboard` -주 창에 대 한 UI입니다. 이 파일 작성 및 iOS에 대 한 Xamarin 디자이너에서 유지 관리 합니다.

다음 섹션에서 이러한 파일 중 일부를 통해 신속 하 게 확인할을 이동 합니다. 살펴볼 것을 보다 자세히 나중 하지만 이제 자신의 기본 사항을 이해 하는 것이 좋습니다.

### <a name="maincs"></a>Main.cs

`Main.cs` 파일에는 고정 `Main` 메서드 새 Xamarin.tvOS 응용 프로그램 인스턴스를 만들고 해당 클래스의 이름을 전달 하는 운영 체제 이벤트를 처리 하는 경우에는 `AppDelegate` 클래스:

```csharp
using UIKit;

namespace Hello_tvOS
{
    public class Application
    {
        // This is the main entry point of the application.
        static void Main (string[] args)
        {
            // if you want to use a different Application Delegate class from "AppDelegate"
            // you can specify it here.
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="appdelegatecs"></a>AppDelegate.cs

`AppDelegate.cs` 파일에 포함 되어 우리의 `AppDelegate` 우리의 창을 만들고 운영 체제 이벤트 수신을 담당 하는 클래스:

```csharp
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    // The UIApplicationDelegate for the application. This class is responsible for launching the
    // User Interface of the application, as well as listening (and optionally responding) to application events from iOS.
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        // class-level declarations

        public override UIWindow Window {
            get;
            set;
        }

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Override point for customization after application launch.
            // If not required for your application you can safely delete this method

            return true;
        }

        public override void OnResignActivation (UIApplication application)
        {
            // Invoked when the application is about to move from active to inactive state.
            // This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message)
            // or when the user quits the application and it begins the transition to the background state.
            // Games should use this method to pause the game.
        }

        public override void DidEnterBackground (UIApplication application)
        {
            // Use this method to release shared resources, save user data, invalidate timers and store the application state.
            // If your application supports background execution this method is called instead of WillTerminate when the user quits.
        }

        public override void WillEnterForeground (UIApplication application)
        {
            // Called as part of the transition from background to active state.
            // Here you can undo many of the changes made on entering the background.
        }

        public override void OnActivated (UIApplication application)
        {
            // Restart any tasks that were paused (or not yet started) while the application was inactive.
            // If the application was previously in the background, optionally refresh the user interface.
        }

        public override void WillTerminate (UIApplication application)
        {
            // Called when the application is about to terminate. Save data, if needed. See also DidEnterBackground.
        }
    }
}
```

이 코드 하지 않는 이미 알고 있을 것은 매우 간단 하지만 이전에 iOS 응용 프로그램을 구축 했습니다. 중요 한 줄을 살펴보겠습니다.

첫째, 클래스 수준 변수 선언에 살펴보겠습니다.

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

`Window` 속성은 주 창에 대 한 액세스를 제공 합니다. tvOS로 사용 하 여 *모델 뷰 컨트롤러* (MVC) 패턴. 일반적으로 만들면 모든 창 (및 창 내에서 다른 여러 가지 원인에 대 한), 컨트롤러를 보여주는 것 등 새 보기 (제어)를 추가 하는 등의 창 수명 주기를 담당 하는입니다.

다음으로 `FinishedLaunching` 메서드. 이 메서드는 응용 프로그램이 시작 된 후를 실제로 만들고 응용 프로그램 창에 보기를 표시 합니다. 프로세스 시작이 실행 됩니다. 스토리 보드를 사용 하 여 UI를 정의 하는 앱, 추가 코드 없이 이므로 필요한 여기 합니다.

와 같은 서식 파일에서 제공 되는 다른 여러 방법이 `DidEnterBackground` 및 `WillEnterForeground`합니다. 응용 프로그램에서 응용 프로그램 이벤트를 사용 하는 경우를 안전 하 게 제거할 수 있습니다 이러한.

### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController` 클래스는 컨트롤러의 주 창. 하는 주 창의 수명 주기를 의미 합니다. 여기 나중에 검사이 자세히, 지금은 보겠습니다 빠르게 살펴보는:

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
    }
}
```

#### <a name="viewcontrollerdesignercs"></a>ViewController.Designer.cs

주 창 클래스에 대 한 디자이너 파일은 지금 바로 비어 있지만 자동으로 입력 됩니다 Visual Studio가 Mac 용 ios 디자이너 사용자 인터페이스를 만드는 것 만큼:

```csharp
using Foundation;

namespace HellotvOS
{
    [Register ("ViewController")]
    partial class ViewController
    {
        void ReleaseDesignerOutlets ()
        {
        }
    }
}
```

Mac 용 Visual Studio에서 자동으로 관리 하는 대로 디자이너 파일와 일반적으로 관련 되지 하 고 방금 म 모든 창에 추가 하거나 응용 프로그램의 컨트롤에 대 한 액세스를 허용 하는 필수 연결 코드를 제공 합니다.

Xamarin.tvOS 앱에서 만든 하 고 해당 구성 요소에 대 한 기본적인 이해 있다고 했으므로 이제 UI 작성에서 살펴보겠습니다.

<a name="Creating-the-User-Interface" />

## <a name="creating-the-user-interface"></a>사용자 인터페이스 만들기

IOS 용 Xamarin 디자이너를 사용 하 여 Xamarin.tvOS 앱에 대 한 사용자 인터페이스를 만들 필요가 없습니다 하 고, UI C# 코드에서 직접 만들 수 있지만이 문서의 범위를 벗어납니다. 간단한 설명을 위해가이 자습서의 나머지 부분에서 우리 UI를 만들기 위해 iOS 디자이너를 사용 합니다.

UI 만들기를 시작 하려면 보겠습니다 두 번 클릭은 `Main.storyboard` 파일에 **솔루션 탐색기** iOS 디자이너에서에서 편집 하기 위해 열려는:

[ ![](hello-tvos-images/designer01.png "솔루션 탐색기의 Main.storyboard 파일")](hello-tvos-images/designer01.png)

이 디자이너를 시작 하 고 다음과 같은 해야 합니다.

[ ![](hello-tvos-images/designer02.png "디자이너")](hello-tvos-images/designer02.png)

자세한 내용은 iOS 디자이너에서 및 작동 방식에 대 한 참조는 [iOS 용 Xamarin 디자이너 소개](~/ios/user-interface/designer/introduction.md) 가이드입니다.

이제 Xamarin.tvOS 앱의 디자인 화면에 컨트롤 추가 시작할 수 있습니다.

다음을 수행합니다.

1. 찾습니다는 **도구 상자**, 디자인 화면 오른쪽에 여야 합니다.

    [![](hello-tvos-images/designer03.png "도구 상자")](hello-tvos-images/designer03.png)

    여기 찾을 수 없으면로 이동 **보기 > 패드 > 도구 상자** 볼 수 있습니다.
2. 끌어서는 **레이블** 에서 **도구 상자** 디자인 화면에:

    [ ![](hello-tvos-images/designer04.png "도구 상자에서 레이블을 끌어 옵니다.")](hello-tvos-images/designer04.png)
3. 클릭는 **제목** 속성에는 **속성 패드** 단추의 제목을 변경 하 고 `Hello, tvOS` 설정 하 고는 **글꼴 크기** 128로:

    [ ![](hello-tvos-images/designer05.png "Hello, tvOS 제목을 설정합니다 하 고 글꼴 크기를 128로 설정")](hello-tvos-images/designer05.png)
4. 모든 단어 표시 되며 창의 위쪽 가운데에 배치 되도록 레이블의 크기를 조정 합니다.

    [ ![](hello-tvos-images/designer06.png "크기를 조정 하 고 레이블을 가운데로 정렬")](hello-tvos-images/designer06.png)
5. 의도 한 대로 표시 되도록의 위치를 제한 되는 레이블 이제 할 수 있습니다. 화면 크기에 관계 없이 이 위해까지 레이블을 클릭는 *T 모양의 핸들* 나타납니다.

    [ ![](hello-tvos-images/designer07.png "T 모양 핸들")](hello-tvos-images/designer07.png)
6. 레이블을 가로로 제한 하려면 가운데 사각형을 선택 하 고 세로로 파선으로 끕니다.

    [ ![](hello-tvos-images/designer08.png "가운데 사각형을 선택 합니다.")](hello-tvos-images/designer08zoom.png)

     레이블을 주황색을 설정 해야 합니다.
7. 레이블의 상단에서 T 핸들을 선택 하 고 창의 위쪽 가장자리를 끌어:

    [ ![](hello-tvos-images/designer09.png "창의 위쪽 가장자리에 핸들을 끌어 옵니다.")](hello-tvos-images/designer09.png)
8. 너비와 높이 다음으로 클릭 *본 핸들* 아래 그림과 같이:

    [ ![](hello-tvos-images/designer10.png "너비 및 높이 본 핸들")](hello-tvos-images/designer10.png)

     때 각 *본 핸들* 은 너비와 높이가 모두 각각 고정 된 크기를 설정 하려면 선택를 클릭 합니다.
9. 완료 되 면 제약 조건이 배포 속성 패드의 레이아웃 탭에서와 비슷한 같아야 합니다.

    [ ![](hello-tvos-images/designer11.png "예제에서는 제약 조건")](hello-tvos-images/designer11.png)
8. 끌어서는 **단추** 에서 **도구 상자** 레이블 아래에 놓습니다.
9. 클릭는 **제목** 속성에는 **속성 패드** 단추의 제목을 변경 하 고 `Click Me`:

    [ ![](hello-tvos-images/designer12.png "Click Me 단추 제목을 변경합니다")](hello-tvos-images/designer12.png)
10. TvOS 창에서 단추를 제한 하려면 위의 5 ~ 8 단계를 반복 합니다. 그러나, 끌어오는 방법 대신 T 핸들 (단계 7)에서 같이 창의 맨 위로 이동, 끌어 레이블 맨 아래에:

    [ ![](hello-tvos-images/designer14.png "단추를 제한 합니다.")](hello-tvos-images/designer14.png)
11. 단추 아래에서 다른 레이블을 끌어 크기를 조정 동일한 너비로 첫 번째 레이블과 집합 수, 해당 **맞춤** 를 **센터**:

    [ ![](hello-tvos-images/designer15.png "단추 아래에서 다른 레이블을 끌어, 센터로 맞춤을 설정 하 고 동일한 너비로 첫 번째 레이블의 됩니다 size")](hello-tvos-images/designer15.png)
12. 첫 번째 레이블 및 단추와 같은이 레이블을 가운데 맞춤 및 위치와 크기에 pin을 설정 합니다.

    [ ![](hello-tvos-images/designer16.png "위치와 크기를 레이블이 고정합니다")](hello-tvos-images/designer16.png)
13. 사용자 인터페이스에 변경 내용을 저장 합니다.

크기 조정 하 고 이동 하는 컨트롤 주위의 것을 알아야 했습니다 디자이너 기반으로 하는 것이 도움이 스냅인 힌트는 하면 [Apple TV Human Interface Guidelines](https://developer.apple.com/tvos/human-interface-guidelines/)합니다. 이러한 지침을 Apple TV 사용자에 대 한 친숙 한 모양 및 느낌에 있는 고품질 응용 프로그램을 만들 수 있습니다.

보면는 **문서 개요** 섹션에서 방법을 레이아웃과 사용자 인터페이스를 구성 하는 요소 들의 계층이 표시 됩니다.

[ ![](hello-tvos-images/designer17.png "문서 개요 섹션")](hello-tvos-images/designer17.png)

여기에서 편집 하거나 필요한 경우에 UI 요소를 다시 정렬 하려면 끌어 항목을 선택할 수 있습니다. 예를 들어 UI 요소의 다른 요소에 의해 검사 되 고, 경우 끌어 놓을 수 있습니다 맨 아래 목록에서 창에서 최상위 항목을 확인 합니다.

만든이 사용자 인터페이스를 만들었으므로 이제 해당 Xamarin.tvOS 액세스 하 고 C# 코드에서 상호 작용할 수 있도록 UI 항목을 노출 해야 합니다.

### <a name="accessing-the-controls-in-the-code-behind"></a>코드 숨김에 컨트롤 액세스

코드에서 iOS 디자이너에 추가 된 컨트롤에 액세스할 수 있는 다음 두 가지가 있습니다.

* 컨트롤에 대 한 이벤트 처리기를 만듭니다.
* 있도록 제공 하는 컨트롤 이름, 나중에 참조할 수 있습니다.

이러한 추가 되는 경우, 내에서 partial 클래스는 `ViewController.designer.cs` 변경 내용을 반영 하도록 업데이트 됩니다. 이렇게 하면 다음 뷰 컨트롤러의 컨트롤에 액세스할 수 있습니다.

### <a name="creating-an-event-handler"></a>이벤트 처리기 만들기

이 예제 응용 프로그램 단추를 클릭할 때 원하는 _문제가_ 발생 하므로 이벤트 처리기 단추에 특정 이벤트에 추가 해야 합니다. 이렇게 설정 하려면 다음을 수행 합니다.

1. Xamarin iOS 디자이너 뷰 컨트롤러에 있는 단추를 선택 합니다.
2. 속성 패드에서 선택 된 **이벤트** 탭:

    [![](hello-tvos-images/event1.png "이벤트 탭")](hello-tvos-images/event1.png)
3. TouchUpInside 이벤트를 찾아 이라는 이벤트 처리기를 지정 `Clicked`:

    [![](hello-tvos-images/event2.png "TouchUpInside 이벤트")](hello-tvos-images/event2.png)
4. 누를 때 **Enter**, **ViewController**.cs 파일은 열립니다 코드에서 이벤트 처리기에 대 한 위치를 제안 하는 것입니다. 키보드의 화살표 키를 사용 하 여 위치를 설정 하려면:

    [![](hello-tvos-images/event3.png "위치 설정")](hello-tvos-images/event3.png)
5. 아래와 같이 부분 메서드를 만들어집니다.

    [![](hello-tvos-images/event4.png "부분 메서드")](hello-tvos-images/event4.png)

이제 단추 함수를 허용 하는 코드 추가 시작할 준비가 되었습니다.

### <a name="naming-a-control"></a>컨트롤 이름 지정

레이블을 업데이트 해야 단추를 클릭할 경우의 클릭 수에 따라 합니다. 이 수행 하려면 코드에서 레이블을 액세스 해야 합니다. 이 이름을 지정 하 여 수행 됩니다. 다음을 수행합니다.

1. 스토리 보드를 열고 뷰 컨트롤러의 아래쪽에 레이블을 선택 합니다.
2. 속성 패드에서 선택 된 **위젯** 탭:

    [![](hello-tvos-images/name1.png "위젯을 탭 선택")](hello-tvos-images/name1.png)
3. 아래 **Identity > 이름을**, 추가 `ClickedLabel`:

    [![](hello-tvos-images/name2.png "ClickedLabel 설정")](hello-tvos-images/name2.png)

이제는 레이블을 업데이트를 시작할 준비가 되었습니다!

### <a name="how-controls-are-accessed"></a>컨트롤 액세스 하는 방법은

선택 하는 경우는 `ViewController.designer.cs` 에 **솔루션 탐색기** 볼 수는 어떻게는 `ClickedLabel` 레이블 및 `Clicked` 이벤트 처리기에 매핑된는 **콘센트** 및  **동작** C#:

[ ![](hello-tvos-images/accesscontrol.png "콘센트 및 동작")](hello-tvos-images/accesscontrol.png)

알 수 있습니다 `ViewController.designer.cs` Mac 용 Visual Studio를 수정 하지 않아도 되도록 partial 클래스는 `ViewController.cs` 있는 우리는 클래스에 대 한 변경 내용을 덮어씁니다.

이러한 방식으로 UI 요소를 노출 뷰 컨트롤러에 관계 없이 액세스할 수 있습니다.

일반적으로 필요 하지 열려는 `ViewController.designer.cs` , 직접 것가 여기에 제시 된 교육 목적 으로만 합니다.

<a name="Writing-the-Code" />

## <a name="writing-the-code"></a>코드 작성

만든이 사용자 인터페이스와 UI 요소를 통해 코드에 노출 된 **콘센트** 및 **동작**, 마지막으로 프로그램 기능을 제공 하는 코드를 작성 준비가 됩니다.

응용 프로그램에서는 첫 번째 단추를 클릭할 때마다을 맞추려고는 단추를 클릭 하는 횟수를 표시 하는 레이블을 업데이트 합니다. 열려는 해야이를 위해는 `ViewController.cs` 에 두 번 클릭 하 여 편집을 위해 파일의 **솔루션 패드**:

[ ![](hello-tvos-images/code01.png "솔루션 채움")](hello-tvos-images/code01.png)

먼저에서 클래스 수준 변수를 만들려면 우리의 `ViewController` 발생 한 번의 클릭 수를 추적 하는 클래스입니다. 클래스 정의를 편집하고 다음과 비슷하게 만듭니다.

```csharp
using System;
using Foundation;
using UIKit;

namespace Hello_tvOS
{
    public partial class ViewController : UIViewController
    {
        private int numberOfTimesClicked = 0;
        ...
```

동일한 클래스에 다음 (`ViewController`)를 재정의 해야는 **ViewDidLoad** 메서드 우리의 레이블에 대 한 초기 메시지를 설정 하는 일부 코드를 추가 하 고:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

사용 해야 `ViewDidLoad `와 같은 다른 방법을 대신 `Initialize`때문에, `ViewDidLoad ` 라고 *후* 운영 체제를 로드 하 고에서 사용자 인터페이스를 인스턴스화할 시킨는 `.storyboard` 파일입니다. Label 컨트롤 앞에 액세스 하려고 할 경우는 `.storyboard` 얻을 수 있는 것, 파일은 완전히 로드과 인스턴스화할는 `NullReferenceException` 오류 label 컨트롤은 아직 생성 되지 합니다.

다음으로 단추를 클릭 하면 사용자에 게 응답 하는 코드를 추가 해야 합니다. 다음 부분을 추가 클래스를 만들었다고:

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

이 코드는이 단추를 클릭할 때마다 호출 됩니다.

에서는 있는 원위치에서 모든 빌드하고 Xamarin.tvOS 응용 프로그램을 테스트할 준비가 이제 됩니다.

## <a name="testing-the-application"></a>응용 프로그램 테스트

작성 하 고 예상 대로 실행 되는지 확인 하려면 응용 프로그램을 실행 차례입니다. 빌드하고 모두 한 번에에서 실행할 수 있습니다 또는 실행 하지 않고 작성할 수 있습니다.

응용 프로그램을 빌드할 म 때마다 원하는 빌드 종류를 선택할 수 있습니다.

-   **디버그** -디버그 빌드 컴파일될는 ' 응용 프로그램이 실행 되는 동안 발생 하는 작업을 디버깅할 수 있도록 추가 메타 데이터 파일 (응용 프로그램).
-   **릴리스** – 릴리스 빌드도 만듭니다.는 ' 파일을 하지만 더 빠르게 실행 되 고 작은 있도록 디버그 정보를 포함 하지 않습니다.  

빌드 형식을 선택할 수 있습니다는 **구성 선택기** Mac 화면에 대 한 Visual Studio의 왼쪽 위 모서리에:

[ ![](hello-tvos-images/run01.png "빌드 종류 선택")](hello-tvos-images/run01.png)

### <a name="building-the-application"></a>응용 프로그램 빌드

경우에에서는 디버그 빌드를 원하는 있으므로 되도록 **디버그** 을 선택 합니다. 중 하나를 눌러 응용 프로그램을 먼저 제작 해 보겠습니다 **⌘B**, 또는 **빌드** 메뉴 선택 **모두 빌드**합니다.

오류가 있는 weren't, 볼 수 있습니다는 **빌드에 성공** Visual Studio에서 Mac의 상태 표시줄에 대 한 메시지입니다. 오류 인 경우 프로젝트를 검토 하 고 단계를 올바르게 수행 했으면 있는지 확인 합니다. (Xcode 내부와 모두 Mac 용 Visual Studio에서) 코드를 위한 자습서의 코드와 일치 하는지 확인 하 여 시작 합니다.

### <a name="running-the-application"></a>응용 프로그램 실행

응용 프로그램을 실행 하는 세 가지 옵션이 있습니다.

-  **⌘ + Enter**를 누릅니다.
-  **실행** 메뉴에서 **디버그**를 선택합니다.
-  Mac용 Visual Studio 도구 모음에서 **재생** 단추를 클릭합니다(**솔루션 탐색기** 바로 위에 있음).

(이미 빌드 되었는지 되지 않은) 하는 경우 응용 프로그램을 작성 합니다, 시작 시뮬레이터 tvOS 디버그 모드로 시작 됩니다 및 응용 프로그램 시작 되며 주 인터페이스 창을 표시 합니다.

[ ![샘플 앱 홈 화면](hello-tvos-images/run03.png)](hello-tvos-images/run03.png)

**하드웨어** 메뉴 선택 **Apple TV 원격 표시** 시뮬레이터를 제어할 수 있게 합니다.

[ ![](hello-tvos-images/run04.png "Apple TV 원격 표시를 선택 합니다.")](hello-tvos-images/run04.png)

단추를 몇 번 레이블을 클릭 하면 시뮬레이터의 원격을 사용 하 여 개수와 업데이트 해야 합니다.

[ ![](hello-tvos-images/run05.png "업데이트 된 개수를 사용 하 여 레이블")](hello-tvos-images/run05.png)

지금까지 명확 하 게 여기에서 많은 다룬 하지만 이제 만드는 데 사용 되는 도구 뿐만 아니라 Xamarin.tvOS 응용 프로그램의 구성 요소를 확실 하 게 이해 한 경우이 자습서에서는 처음부터 끝까지, 있어야 합니다.

## <a name="where-to-next"></a>다음 위치

Apple TV 앱 표시 합니다. 사용자와 (사용자의 손 모양에 없는 간에) 인터페이스 및 제한 사항 tvOS 간의 분리로 인해 몇 가지 과제 개발 응용 프로그램 크기 및 저장소에 배치 합니다.

결과적으로, 항상 제안 하 여 읽기 다음에 설명 Xamarin.tvOS 응용 프로그램의 디자인에 시작 하기 전에:

- [9 tvOS 소개](~/ios/tvos/platform/tvos9.md) -이 문서 Xamarin.tvOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 tvOS 9에서에서 사용할 수 있는 기능을 소개 합니다.
- [탐색 및 포커스 작업](~/ios/tvos/app-fundamentals/navigation-focus.md) – Xamarin.tvOS 응용 프로그램의 사용자의 인터페이스를 직접으로 ios 여기서은 탭 장치 화면에 있지만 간접적으로 옆에서 이미지 Siri 원격을 사용 하 여 공간 상호 작용 하지 것입니다. 이 문서에서는 포커스 및 Xamarin.tvOS 응용 프로그램의 사용자 인터페이스에 대 한 탐색을 처리 하는 방법의 개념을 설명 합니다.
- [Siri 원격 인스턴스 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) – 사용자가 Apple TV 및 Xamarin.tvOS 앱 상호 작용 합니다, 포함된 Siri 원격 방식은 하는 기본 방식입니다. 게임 응용 프로그램을 사용 하는 경우 필요에 따라 빌드할 수에 만든 제 3 자에 대 한 지원이 iOS (MFI) Bluetooth 게임 컨트롤러도 응용 프로그램에서 합니다. 이 문서에서는 Xamarin.tvOS 응용 프로그램에서 새 Siri 원격 인스턴스 및 Bluetooth 게임 컨트롤러를 지 원하는 설명 합니다.
- [리소스 및 데이터 저장소](~/ios/tvos/app-fundamentals/resources-data-storage.md) – iOS 장치와 달리 새 Apple TV의 제공 하지 않으면 영구적이 고 로컬 저장소 tvOS 앱. 결과적으로, Xamarin.tvOS 앱 (예: 사용자 기본 설정) 정보를 유지 하는 경우 저장 하 고 iCloud에서 해당 데이터를 검색 해야 것입니다. 이 문서에서는 리소스와 Xamarin.tvOS 응용 프로그램에서 영구 데이터 저장소 작업을 수행 합니다.
- [아이콘과 이미지 작업을](~/ios/tvos/app-fundamentals/icons-images.md) 만드는 – 영상을 아이콘 및 이미지는 Apple TV 앱에 대 한 풍부한 사용자 환경을 개발의 중요 한 부분입니다. 이 가이드에서는 만들고 Xamarin.tvOS 앱에 대 한 필요한 그래픽 자산을 포함 하는 데 필요한 단계를 설명 합니다.
- [사용자 인터페이스](~/ios/tvos/user-interface/index.md) – Xamarin.tvOS를 작업할 때 UI (사용자 인터페이스) 컨트롤을 포함 하는 일반 사용자 환경 (UX) 검사 Xcode의 작성기 인터페이스 및 UX 디자인 원칙 사용 합니다.
- [배포 및 테스트](~/ios/tvos/deploy-test/index.md) -이 섹션에서는 배포 하는 방법과 앱을 테스트 하는 데 사용 되는 항목을 다룹니다. 이 항목에는 디버깅, 테스터 및 Apple TV 앱 스토어에 응용 프로그램을 게시 하는 방법에 대 한 배포에 사용 되는 도구 등 포함 됩니다.

Xamarin.tvOS 작업 중 모든 문제를 실행 하면 참조 하십시오 우리의 [문제 해결](~/ios/tvos/troubleshooting.md) 설명서에 있는 목록에 대 한 알려진 문제 및 솔루션입니다.

## <a name="summary"></a>요약

이 문서에 간단한 Hello, tvOS 응용 프로그램을 만들어 Mac 용 Visual Studio를 사용 하 여 tvOS에 대 한 앱을 개발 하는 빠른 시작 정보를 제공 합니다. 그 tvOS 장치 프로 비전 인터페이스 만들기, tvOS에 대 한 코딩 및 tvOS 시뮬레이터에 대 한 테스트의 기본 사항을 다룹니다.


## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin (비디오)는 tvOS에 대 한 응용 프로그램 구축](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
