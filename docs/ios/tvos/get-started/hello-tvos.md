---
title: Tvos 빠른 시작 가이드
description: 이 가이드 첫 번째 Xamarin.tvOS 앱 및 해당 개발 도구 체인을 만드는 방법을 안내 합니다. 또한 코드에 UI 컨트롤을 노출 하 고 빌드, 실행 및 Xamarin.tvOS 응용 프로그램을 테스트 하는 방법을 보여 줍니다. Xamarin 디자이너를 소개 합니다.
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 02/02/2018
ms.openlocfilehash: 8c8bf3f86091f49633913b37ef5108ddbae6d276
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60951777"
---
# <a name="hello-tvos-quick-start-guide"></a>Tvos 빠른 시작 가이드

_이 가이드 첫 번째 Xamarin.tvOS 앱 및 해당 개발 도구 체인을 만드는 방법을 안내 합니다. 또한 코드에 UI 컨트롤을 노출 하 고 빌드, 실행 및 Xamarin.tvOS 응용 프로그램을 테스트 하는 방법을 보여 줍니다. Xamarin 디자이너를 소개 합니다._

Apple의 Apple TV에서 Apple TV (4k, tvOS 11을 실행 하는 5 세대를 출시 했습니다.

Apple TV 플랫폼의 개발자에 게 다양 한 몰입 형 앱을 만들고 Apple TV 기본 제공 앱 스토어를 통해 해제할 수 있도록 열려 있습니다.

Xamarin.iOS 개발에 익숙한 경우에 tvOS 전환을 매우 간단한 찾아야 합니다. 대부분의 Api 및 기능 동일, 많은 일반적인 Api (예: WebKit) 사용할 수 있지만. 또한 작업을 Siri 원격을 사용 하 여 터치 기반 iOS 장치에 존재 하지 않는 몇 가지 디자인 문제를 제기 합니다.

이 가이드에서는 Xamarin 앱에서 tvOS를 사용 하 여 작업에 대 한 소개를 제공 합니다. TvOS에 대 한 자세한 내용은 Apple의를 참조 하세요 [4k Apple TV에 대 한 준비](https://developer.apple.com/tvos/) 설명서.

## <a name="overview"></a>개요

Xamarin.tvOS을 사용 하면에서 완전 한 네이티브 Apple TV 앱을 개발할 수 있습니다 C# 및.NET에서 개발할 때 사용 되는 인터페이스 컨트롤과 동일한 OS X 라이브러리를 사용 하 여 *Swift* (또는 *Objective-c*) 및  *Xcode*합니다.

또한 Xamarin.tvOS에서 앱이 작성 된 이후 C# Xamarin.iOS, Xamarin.Android 및 Xamarin.Mac 앱을 사용 하 여.NET에서 일반적인 백 엔드 코드를 공유할 수 있습니다 각 플랫폼에 기본 경험을 제공 하면서 합니다.

이 문서에서는 소개 Xamarin.tvOS 및 Visual Studio를 사용 하 여 기본 빌드 프로세스를 통해 Apple TV 앱을 만드는 데 필요한 핵심 개념 **Tvos** 횟수를 계산 하는 앱에 단추에는 클릭 했습니다.

[![](hello-tvos-images/run05.png "예제 앱 실행")](hello-tvos-images/run05.png#lightbox)

다음 개념 설명 하겠습니다.

-  **Mac 용 visual Studio** – Mac 및이 사용 하 여 Xamarin.tvOS 응용 프로그램을 만드는 방법에 대 한 Visual Studio를 소개 합니다.
-  **Xamarin.tvOS 앱 분석** – 무엇을 Xamarin.tvOS 앱으로 구성 됩니다.
-  **사용자 인터페이스 만들기** – 사용자 인터페이스를 만들려면 Xamarin iOS 디자이너를 사용 하는 방법입니다.
-  **배포 및 테스트** – 실행 하 고 실제 tvOS 하드웨어 및 tvOS 시뮬레이터에서에서 앱을 테스트 하는 방법입니다.

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Mac 용 Visual Studio에서 새 Xamarin.tvOS 앱 시작

라는 Apple TV 앱을 만들어 위에서 설명한 대로 `Hello-tvOS` 주 화면에 단일 단추 및 레이블을 추가 합니다. 단추를 클릭하면 레이블에 단추 클릭 횟수가 표시됩니다.

시작 하려면 다음을 수행 하겠습니다.

1. Mac용 Visual Studio 시작:

    [![](hello-tvos-images/setup01.png "Visual Studio for Mac")](hello-tvos-images/setup01.png#lightbox)
2. 클릭 된 **새 솔루션...**  열려면 화면의 왼쪽 위 모서리에 있는 링크를 **새 프로젝트** 대화 상자.
3. 선택 **tvOS** > **앱** > **단일 뷰 앱** 을 클릭 합니다 **다음** 단추:

    [![](hello-tvos-images/setup02.png "단일 뷰 앱 선택")](hello-tvos-images/setup02.png#lightbox)
4. 입력 `Hello, tvOS` 에 대 한는 **앱 이름**, 입력에 **조직 식별자** 을 클릭 합니다 **다음** 단추:

    [![](hello-tvos-images/setup04.png "Enter Hello, tvOS")](hello-tvos-images/setup04.png#lightbox)
5. 입력 `Hello_tvOS` 에 대 한 합니다 **프로젝트 이름** 을 클릭 합니다 **만들기** 단추:

    [![](hello-tvos-images/setup03.png "Enter HellotvOS")](hello-tvos-images/setup03.png#lightbox)

Mac 용 visual Studio를 새 Xamarin.tvOS 앱을 만들고 응용 프로그램의 솔루션에 추가 하는 기본 파일을 표시 합니다.

 [![](hello-tvos-images/project01.png "기본 파일 보기")](hello-tvos-images/project01.png#lightbox)

사용 하 여 Mac 용 visual Studio **솔루션** 하 고 **프로젝트**, Visual Studio는 정확히 동일한 방식으로 합니다. 솔루션은 하나 이상의 프로젝트를 보관할 수 있는 컨테이너이고, 프로젝트는 애플리케이션, 지원 라이브러리, 테스트 애플리케이션 등을 포함할 수 있습니다. 이 경우 Mac 용 Visual Studio를 모두 솔루션 및 응용 프로그램 프로젝트를 마련 했습니다.

를 하려는 경우 일반적인 공유 코드를 포함 하는 라이브러리 프로젝트 하나 이상의 코드를 만들 수 있습니다. 이러한 라이브러리 프로젝트를 응용 프로그램 프로젝트에서 사용 하거나 다른 Xamarin.tvOS 앱 프로젝트와 공유할 수 있습니다 (또는 Xamarin.iOS, Xamarin.Android 및 Xamarin.Mac 코드의 형식에 따라) 동일한 방식으로 표준.NET 응용 프로그램을 빌드하는 경우.

## <a name="anatomy-of-a-xamarintvos-app"></a>Xamarin.tvOS 앱 분석

IOS 프로그래밍에 익숙한 경우 수많은 유사성을 발견할 여기서 확인할 수 있습니다. 사실, tvOS 9 여기 교차 하므로 수많은 개념이 하므로 iOS 9의 일부입니다.

프로젝트의 파일에 살펴보겠습니다.

-   `Main.cs` - 앱의 진입점을 포함합니다. 앱이 실행되면 첫 번째 클래스와 실행되는 메서드를 포함합니다.
-   `AppDelegate.cs` –이 파일은 운영 체제에서 이벤트를 수신 하는 일을 담당 하는 주 응용 프로그램 클래스를 포함 합니다.
-   `Info.plist` –이 파일에는 응용 프로그램 이름, 아이콘 등과 같은 응용 프로그램 속성이 포함 됩니다.
-   `ViewController.cs` – 주 창을 나타내며의 수명 주기를 제어 하는 클래스입니다.
-   `ViewController.designer.cs` –이 파일은 기본 화면의 사용자 인터페이스와 통합 하도록 도와주는 배관 코드를 포함 합니다.
-  `Main.storyboard` – 주 창에 대 한 UI입니다. 이 파일을 생성 하 고 iOS 용 Xamarin 디자이너에서 유지 관리 될 수 있습니다.

다음 섹션에서는 이러한 파일 중 일부를 통해 신속 하 게 확인에 알아보겠습니다. 탐색을 자세히 나중에 있지만 이제는 기본 사항을 이해 하는 것이 좋습니다.

### <a name="maincs"></a>Main.cs

`Main.cs` 파일에는 정적 `Main` 새 Xamarin.tvOS 앱 인스턴스를 만들고 해당 클래스의 이름을 전달 하는 메서드 OS 이벤트를 처리는 여기서는 `AppDelegate` 클래스:

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

`AppDelegate.cs` 파일에는 `AppDelegate` 우리의 창을 만들고 OS 이벤트를 수신 하는 일을 담당 하는 클래스:

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

이전에 iOS 응용 프로그램을 구축 했으므로 이지만 매우 간단 하지 않는 한이 코드를 익숙할 아닙니다. 중요 한 줄을 살펴보겠습니다.

먼저 클래스 수준 변수 선언에 살펴보겠습니다.

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

`Window` 속성에는 주 창에 대 한 액세스를 제공 합니다. tvOS 라고를 사용 합니다 *모델 뷰 컨트롤러* (MVC) 패턴. 일반적으로 만든 모든 창에 대 한 (그리고 창 내부의 여러 들) 표시 하 고, 새 보기 (컨트롤) 등를 추가 하는 등 창 수명 주기를 담당 하는 컨트롤러가 됩니다.

다음에 `FinishedLaunching` 메서드. 이 메서드는 응용 프로그램이 시작 된 후 실제로 응용 프로그램 창을 만들고 그 안에 보기를 표시 하는 프로세스를 시작 하기 위한 담당 하 고 실행 합니다. 스토리 보드를 사용 하 여 UI를 정의 하는 앱, 때문에 추가 코드가 필요 하지 않습니다 여기 있습니다.

다른 메서드가 같은 템플릿에서 제공 되는 많이 `DidEnterBackground` 고 `WillEnterForeground`입니다. 앱에서 응용 프로그램 이벤트를 사용 하는 경우를 안전 하 게 제거할 수 있습니다 이러한 합니다.

### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController` 클래스는 주 창의 컨트롤러입니다. 즉, 주 창의 수명 주기를 담당 합니다. 여기서 나중에 검사이 세부 정보에서 이제 보겠습니다 빠른 살펴보겠습니다.

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

주 창 클래스에 대 한 디자이너 파일이 지금은 비어, 하지만 자동으로 채워지고 Visual Studio에서 Mac 용 iOS 디자이너를 사용 하 여 사용자 인터페이스를 만들 것:

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

Mac 용 Visual Studio에서 자동으로 관리 하는 디자이너 파일을 사용 하 여 일반적으로 관련 되지 않습니다 하 고 모든 창에 추가 했습니다 또는 응용 프로그램의 컨트롤에 대 한 액세스를 허용 하는 필수 배관 코드를 제공 하면 됩니다.

Xamarin.tvOS 앱을 만들었습니다. 해당 구성 요소에 대 한 기본적인 지식이 있다고를 UI 만들기에서 살펴보겠습니다.

<a name="Creating-the-User-Interface" />

## <a name="creating-the-user-interface"></a>사용자 인터페이스 만들기

IOS 용 Xamarin 디자이너를 사용 하 여 Xamarin.tvOS 앱에 대 한 사용자 인터페이스를 만들 필요가 없습니다, UI에서 직접 만들 수 있습니다 C# 코드 있지만이 문서의 범위를 벗어납니다. 편의상이 자습서의 나머지 UI를 만들려면 iOS 디자이너를 사용 하겠습니다.

UI 만들기를 시작 하려면 보겠습니다 두 번 클릭 합니다 `Main.storyboard` 파일을 **솔루션 탐색기** iOS 디자이너에서에서 편집 하기 위해 열려는:

[![](hello-tvos-images/designer01.png "솔루션 탐색기의 Main.storyboard 파일")](hello-tvos-images/designer01.png#lightbox)

이 디자이너를 시작 해야 하며 다음과 같습니다.

[![](hello-tvos-images/designer02.png "디자이너")](hello-tvos-images/designer02.png#lightbox)

IOS 디자이너에 대 한 자세한 정보 및 작동 방식에 대 한 참조를 [iOS 용 Xamarin 디자이너 소개](~/ios/user-interface/designer/introduction.md) 가이드입니다.

Xamarin.tvOS 앱의 디자인 화면에 컨트롤을 추가 하는 것은 이제 수 있습니다.

다음을 수행합니다.

1. 찾을 **도구 상자**, 디자인 화면의 오른쪽 이어야 합니다.

    [![](hello-tvos-images/designer03.png "도구 상자")](hello-tvos-images/designer03.png#lightbox)

    여기서 찾을 수 없으면, 이동할 **보기 > 패드 > 도구 상자** 볼 수 있습니다.
2. 끌어서를 **레이블을** 에서 합니다 **도구 상자** 디자인 화면으로:

    [![](hello-tvos-images/designer04.png "레이블을 도구 상자에서 끌어 옵니다.")](hello-tvos-images/designer04.png#lightbox)
3. 클릭 합니다 **제목** 속성에는 **속성 패드** 단추의 제목을 변경 하 고 `Hello, tvOS` 설정를 **글꼴 크기** 128:

    [![](hello-tvos-images/designer05.png "Tvos 제목을 설정합니다 하 고 128 글꼴 크기를 설정")](hello-tvos-images/designer05.png#lightbox)
4. 단어를 모두 표시 되며 창의 위쪽 가운데에 배치 되도록 레이블의 크기를 조정 합니다.

    [![](hello-tvos-images/designer06.png "크기를 조정 하 고 레이블을 가운데로합니다")](hello-tvos-images/designer06.png#lightbox)
5. 의도 한 대로 표시 되도록 제한 되도록 하는 해당 위치에 레이블을 이제 해야 합니다. 화면 크기에 관계 없이 이 위해 될 때까지 레이블을 클릭 합니다 *T 모양의 핸들* 나타납니다.

    [![](hello-tvos-images/designer07.png "T 모양의 핸들")](hello-tvos-images/designer07.png#lightbox)
6. 가로 레이블 제한할 가운데 사각형을 선택 하 고 세로 파선 놓습니다.

    [![](hello-tvos-images/designer08.png "가운데 사각형을 선택 합니다.")](hello-tvos-images/designer08zoom.png#lightbox)

     레이블을은 주황색으로 설정 해야 합니다.
7. 레이블의 맨 위에 있는 T 핸들을 선택 하 고 창의 위쪽 가장자리에 놓습니다.

    [![](hello-tvos-images/designer09.png "창의 위쪽 가장자리에 핸들을 끌어 옵니다.")](hello-tvos-images/designer09.png#lightbox)
8. 다음으로, 너비 및 높이 클릭 *뼈 핸들* 아래 그림과 같이:

    [![](hello-tvos-images/designer10.png "너비 및 높이 뼈 핸들")](hello-tvos-images/designer10.png#lightbox)

     때 각 *뼈 핸들* 는 너비와 높이 모두 각각 고정 된 크기를 설정 하려면 선택를 클릭 합니다.
9. 완료 되 면 제약 조건 속성 패드의 레이아웃 탭에서 유사 합니다.

    [![](hello-tvos-images/designer11.png "예제에서는 제약 조건")](hello-tvos-images/designer11.png#lightbox)
8. 끌어서를 **단추** 에서 합니다 **도구 상자** 레이블 아래에 놓습니다.
9. 클릭 합니다 **제목** 속성에는 **속성 패드** 단추의 제목을 변경 하 고 `Click Me`:

    [![](hello-tvos-images/designer12.png "Click Me 단추 제목을 변경합니다")](hello-tvos-images/designer12.png#lightbox)
10. TvOS 창의 단추를 제한 하려면 위의 5 ~ 8 단계를 반복 합니다. 그러나 끌어오는 방법 대신 T 핸들 (예: #7 단계) 창의 맨 위에, 놓습니다 레이블 아래:

    [![](hello-tvos-images/designer14.png "단추를 제한 합니다.")](hello-tvos-images/designer14.png#lightbox)
11. 단추 아래에서 다른 레이블을 끌어, 크기 집합 및 첫 번째 레이블이와 동일한 너비가 되도록 해당 **맞춤** 하 **Center**:

    [![](hello-tvos-images/designer15.png "단추 아래에서 다른 레이블을 끌어, 동일한 첫 번째 레이블의 너비 및 맞춤 Center로 크기 조정")](hello-tvos-images/designer15.png#lightbox)
12. 첫 번째 레이블 및 단추와 같은 중심 및 위치와 크기에 고정이 레이블을 설정 합니다.

    [![](hello-tvos-images/designer16.png "레이블 위치와 크기에 고정")](hello-tvos-images/designer16.png#lightbox)
13. 사용자 인터페이스에 변경 내용을 저장 합니다.

크기를 조정 하 고 관련 컨트롤을 이동 된을 알아야 했습니다는 디자이너를 사용 하면 유용한 스냅 힌트에 기반한 [Apple TV Human Interface Guidelines](https://developer.apple.com/tvos/human-interface-guidelines/)합니다. 이러한 지침을 Apple TV 사용자에 대 한 친숙 한 모양과 느낌을 갖게 되는 고품질 응용 프로그램을 만들 수 있습니다.

보면 합니다 **문서 개요** 섹션, 레이아웃 및 사용자 인터페이스를 구성 하는 요소는 계층 표시 하는 방법을 확인할 수 있습니다.

[![](hello-tvos-images/designer17.png "문서 개요 섹션")](hello-tvos-images/designer17.png#lightbox)

여기에서 편집 하거나 필요한 경우에 UI 요소를 다시 정렬 하려면 끌 항목을 선택할 수 있습니다. 예를 들어, UI 요소를 다른 요소가 가리는, 하는 경우 창에서 최상위 항목을 확인 하려면 목록 아래쪽으로 끌 수 있습니다.

Xamarin.tvOS에서 액세스 하 고에서 상호 작용할 수 있도록 UI 항목을 노출 해야 만든 사용자 인터페이스를 만들었으므로 이제 해당 C# 코드입니다.

### <a name="accessing-the-controls-in-the-code-behind"></a>코드 숨김에서 컨트롤 액세스

두 가지 주요 코드에서 iOS 디자이너에 추가한 컨트롤에 액세스할 수 있습니다.

* 컨트롤에 이벤트 처리기를 만드는 중입니다.
* 에서는 나중에 코드를 참조할 수 있도록 컨트롤 이름을 제공 합니다.

이러한 추가 되는 경우, 내 partial 클래스는 `ViewController.designer.cs` 변경 내용을 반영 하도록 업데이트 됩니다. 이렇게 하면 다음 뷰 컨트롤러에서 컨트롤에 액세스할 수 있습니다.

### <a name="creating-an-event-handler"></a>이벤트 처리기 만들기

이 샘플 응용 프로그램에서 단추를 클릭할 때 원하는 _무언가_ 수행 되므로 이벤트 처리기 단추는 특정 이벤트에 추가 해야 합니다. 이 작업을 설정 하려면 다음을 수행 합니다.

1. Xamarin iOS 디자이너에서에서 뷰 컨트롤러에 있는 단추를 선택 합니다.
2. Properties pad에서 선택 합니다 **이벤트** 탭:

    [![](hello-tvos-images/event1.png "이벤트 탭")](hello-tvos-images/event1.png#lightbox)
3. TouchUpInside 이벤트를 찾아 라는 이벤트 처리기를 제공 `Clicked`:

    [![](hello-tvos-images/event2.png "TouchUpInside 이벤트")](hello-tvos-images/event2.png#lightbox)
4. 누르면 **Enter**의 **ViewController**.cs 파일이 열리고 코드에서 이벤트 처리기에 대 한 위치를 제안 합니다. 키보드의 화살표 키를 사용 하 여 위치를 설정 하려면:

    [![](hello-tvos-images/event3.png "위치 설정")](hello-tvos-images/event3.png#lightbox)
5. 이 아래와 같이 부분 메서드를 만듭니다.

    [![](hello-tvos-images/event4.png "부분 메서드")](hello-tvos-images/event4.png#lightbox)

이제 단추 함수를 허용 하는 일부 코드를 추가할 준비가 되었습니다.

### <a name="naming-a-control"></a>컨트롤 이름 지정

레이블을 업데이트 해야 단추를 클릭 하면 클릭 수를 기반으로 합니다. 이 위해 레이블을 코드에서 액세스 해야 합니다. 이렇게 이름을 지정 합니다. 다음을 수행합니다.

1. 스토리 보드를 열고 뷰 컨트롤러의 맨 아래에 레이블을 선택 합니다.
2. Properties pad에서 선택 합니다 **위젯** 탭:

    [![](hello-tvos-images/name1.png "위젯을 탭 선택")](hello-tvos-images/name1.png#lightbox)
3. 아래 **Identity > 이름**에 추가 `ClickedLabel`:

    [![](hello-tvos-images/name2.png "ClickedLabel 설정")](hello-tvos-images/name2.png#lightbox)

이제 레이블을 업데이트를 시작할 준비가 되었습니다!

### <a name="how-controls-are-accessed"></a>컨트롤 액세스 하는 방법

선택 하는 경우는 `ViewController.designer.cs` 에 **솔루션 탐색기** 볼 수 하는 방법을 `ClickedLabel` 레이블 및 `Clicked` 이벤트 처리기에 매핑된를 **콘센트** 및 **동작** 에서 C#:

[![](hello-tvos-images/accesscontrol.png "출 선 및 작업")](hello-tvos-images/accesscontrol.png#lightbox)

또한 알 수 있습니다 `ViewController.designer.cs` Mac 용 Visual Studio를 수정 하지 않아도 되도록 부분 클래스는 `ViewController.cs` 에서는 클래스에 대 한 모든 변경 내용을 덮어쓰는 것입니다.

이러한 방식으로 UI 요소에 노출 뷰 컨트롤러에서 액세스할 수 있습니다.

일반적으로 필요가 엽니다는 `ViewController.designer.cs` 직접 보여드린 것 여기 교육 목적에 대 한 합니다.

<a name="Writing-the-Code" />

## <a name="writing-the-code"></a>코드 작성

만든 사용자 인터페이스와 해당 UI 요소를 통해 코드에 노출 **출 선** 하 고 **작업**, 마지막으로 프로그램 기능을 제공 하는 코드를 작성할 준비가 완료 됩니다.

응용 프로그램에서는 첫 번째 단추를 클릭할 때마다 하겠습니다 단추 클릭 된 횟수를 표시 하는 레이블을 업데이트 합니다. 이를 위해 열기 해야 합니다 `ViewController.cs` 에서 두 번 클릭 하 여 편집을 위해 파일을 **Solution Pad**:

[![](hello-tvos-images/code01.png "Solution Pad")](hello-tvos-images/code01.png#lightbox)

먼저 클래스 수준 변수를 만들려면이 `ViewController` 발생 한 클릭 횟수를 추적 하는 클래스입니다. 클래스 정의를 편집하고 다음과 비슷하게 만듭니다.

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

같은 클래스에 다음 (`ViewController`)를 재정의 해야 합니다 **ViewDidLoad** 메서드는 레이블에 대 한 초기 메시지를 설정 하는 일부 코드를 추가 하 고:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

사용 해야 `ViewDidLoad `와 같은 다른 메서드 대신 `Initialize`이므로 `ViewDidLoad ` 라고 *후* OS가 로드 하 고에서 사용자 인터페이스를 인스턴스화를 `.storyboard` 파일입니다. 전에 레이블 컨트롤에 액세스 하려고 할 경우는 `.storyboard` 얻게, 파일이 완전히 로드 되어 인스턴스화된는 `NullReferenceException` 오류 하면 레이블 컨트롤이 아직 만들 수는 없으므로 합니다.

그런 다음 단추를 클릭 하면 사용자에 게 응답 하는 코드를 추가 해야 합니다. 다음 부분을 추가 클래스를 만들었습니다.

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

이 코드는 단추를 클릭할 때마다 호출 됩니다.

에서는 곳에서 모든 항목을 사용 하 여 빌드하고 Xamarin.tvOS 응용 프로그램을 테스트할 준비가 이제 됩니다.

## <a name="testing-the-application"></a>애플리케이션 테스트

빌드 및 예상 대로 실행 되도록 응용 프로그램을 실행 하는 차례입니다. 빌드 수 고 한 단계에서 모두 실행 하거나 실행 하지 않고 작성할 수 있습니다.

응용 프로그램을 구축 하는 때마다 원하는 빌드 종류를 선택할 수 있습니다.

-   **디버그** -디버그 빌드를 컴파일될는 ' 응용 프로그램이 실행 되는 동안 발생 하는 상황을 디버깅할 수 있도록 하는 추가 메타 데이터를 사용 하 여 (응용 프로그램) 파일입니다.
-   **릴리스** – 릴리스 빌드도 만듭니다.는 ' 파일을 만들지만 작은 되 고 더 빠르게 실행 되므로 디버그 정보를 포함 하지 않습니다.  

빌드 유형을 선택할 수 있습니다 합니다 **구성 선택기** 화면 Mac 용 Visual Studio의 왼쪽 위 모퉁이에서.

[![](hello-tvos-images/run01.png "빌드 형식을 선택 합니다.")](hello-tvos-images/run01.png#lightbox)

### <a name="building-the-application"></a>응용 프로그램 빌드

경우에 우리가 원하는 디버그 빌드를 보겠습니다 있는지 확인 합니다 **디버그** 을 선택 합니다. 누르거나 보겠습니다 응용 프로그램을 빌드 **⌘ B**, 또는 합니다 **빌드** 메뉴에서 선택 **모두 빌드**합니다.

오류가 없으면 표시를 **빌드에 성공** Visual Studio에서 Mac의 상태 표시줄에 대 한 메시지입니다. 오류가 발생 하는 경우 프로젝트를 검토 하 고 단계를 올바르게 수행 했으면 있는지 확인 합니다. 자습서에서 코드 (및 Xcode에서 Mac 용 Visual Studio에서) 코드에 일치 하는지 확인 하 여 시작 합니다.

### <a name="running-the-application"></a>응용 프로그램 실행

응용 프로그램을 실행 하는 세 가지 옵션이 있습니다.

-  **⌘ + Enter**를 누릅니다.
-  **실행** 메뉴에서 **디버그**를 선택합니다.
-  Mac용 Visual Studio 도구 모음에서 **재생** 단추를 클릭합니다(**솔루션 탐색기** 바로 위에 있음).

응용 프로그램 (아직 빌드되지 않은 이미) 하는 경우, 시작 시뮬레이터 tvOS 디버그 모드로 시작 됩니다 빌드하고 앱을 시작 하 고 주 인터페이스 창을 표시 됩니다.

[![샘플 앱 홈 화면](hello-tvos-images/run03.png)](hello-tvos-images/run03.png#lightbox)

**하드웨어** 메뉴 선택 **Apple TV 원격 표시** 시뮬레이터를 제어할 수 있도록 합니다.

[![](hello-tvos-images/run04.png "Apple TV 원격 표시를 선택 합니다.")](hello-tvos-images/run04.png#lightbox)

단추를 몇 번 레이블을 클릭 하면 시뮬레이터의 원격을 사용 하 여 수를 사용 하 여 업데이트 해야 합니다.

[![](hello-tvos-images/run05.png "업데이트 된 개수를 사용 하 여 레이블")](hello-tvos-images/run05.png#lightbox)

지금까지 매우 방대 여기에서 다룬 있지만 확실 하 게 만드는 데 사용 하는 도구 뿐만 아니라 Xamarin.tvOS 앱의 구성 요소 이해 이제 시작부터 끝까지이 자습서를 따른 경우.

## <a name="where-to-next"></a>다음 위치?

Apple TV 앱에서는 사용자 인터페이스 (것은 사용자의 직접에 없는 대화방에서) 및 제한 사항 tvOS 간의 분리로 인해 몇 가지 문제를 개발 앱 크기 및 저장소에 배치 합니다.

결과적으로 높은 것이 좋습니다는 사용자의 읽기 다음 문서를 Xamarin.tvOS 앱을 디자인 하기 전에:

- [TvOS 9 소개](~/ios/tvos/platform/tvos9.md) -이 문서에서는 Xamarin.tvOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 tvOS 9에서에서 사용할 수 있는 기능을 소개 합니다.
- [탐색 및 포커스 작업](~/ios/tvos/app-fundamentals/navigation-focus.md) – Xamarin.tvOS 앱의 사용자가 해당 인터페이스로 직접 ios를 탭 할 장치 화면의 있지만 걸쳐에서 간접적으로 이미지 Siri 원격을 사용 하 여 대화방 상호 작용 하지 않습니다. 이 문서에서는 포커스 및 Xamarin.tvOS 앱의 사용자 인터페이스에서 탐색을 처리 하는 방법의 개념에 설명 합니다.
- [Siri 원격 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) – 상호 사용자 Apple TV 및 Xamarin.tvOS 앱 상호 작용, Siri 원격 포함된 하는 것입니다. 게임 앱을 사용 하는 경우 빌드할 수 있습니다 필요에 따라 다른 이름에 대 한 제 3 자에 대 한 지원 (MFI) iOS Bluetooth 게임 컨트롤러도 앱에서. 이 문서에서는 Xamarin.tvOS 앱에서 새 Siri 원격 및 Bluetooth 게임 컨트롤러 지원.
- [리소스 및 데이터 저장소](~/ios/tvos/app-fundamentals/resources-data-storage.md) – iOS 장치와 달리 새 Apple TV에서는 로컬 저장소 tvOS 앱에 대 한 합니다. 결과적으로, Xamarin.tvOS 앱 정보 (예: 사용자 기본 설정)를 유지 해야 하는 경우 저장 하 고 iCloud에서 해당 데이터를 검색 합니다. 이 문서에서는 리소스 및 Xamarin.tvOS 앱에서 영구 데이터 저장소를 사용 하 여 작업을 설명 합니다.
- [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 만들기 – 영상을 아이콘 및 이미지는 Apple TV 앱에 대 한 생생한 사용자 환경을 개발 하는 중요 한 부분입니다. 이 가이드에서는 만들고 Xamarin.tvOS 앱에 대 한 필요한 그래픽 자산을 포함 하는 데 필요한 단계를 설명 합니다.
- [사용자 인터페이스](~/ios/tvos/user-interface/index.md) – Xamarin.tvOS를 작업할 때 UI (사용자 인터페이스) 컨트롤을 포함 하는 일반 사용자 환경 (UX) 검사 Xcode의 Interface Builder 및 UX 디자인 원칙 사용 합니다.
- [배포 및 테스트](~/ios/tvos/deploy-test/index.md) -이 섹션에서는 배포 하는 방법과 앱을 테스트 하는 데 사용 되는 항목을 설명 합니다. 항목 디버깅, 테스터 및 Apple TV 앱 스토어에 응용 프로그램을 게시 하는 방법에 대 한 배포에 사용 되는 도구 등을 포함 합니다.

Xamarin.tvOS 작업 문제를 실행 하는 경우를 참조 하십시오 우리의 [문제 해결](~/ios/tvos/troubleshooting.md) 의 목록에 대 한 설명서 알려진 문제 및 해결 방법입니다.

## <a name="summary"></a>요약

이 문서에서는 간단한 Hello, tvOS 앱을 만들어 Mac 용 Visual Studio를 사용 하 여 tvOS에 대 한 앱 개발 빠른 시작 정보를 제공 합니다. TvOS 장치 프로 비전을 인터페이스 만들기, tvOS에 대 한 코딩 및 tvOS 시뮬레이터에서 테스트의 기본을 다루었습니다.


## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin (비디오)를 사용 하 여 tvOS에 대 한 앱 빌드](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
