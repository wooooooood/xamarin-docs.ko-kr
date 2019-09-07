---
title: Hello, tvOS 빠른 시작 가이드
description: 이 가이드에서는 첫 번째 tvOS 앱 및 해당 개발 도구 체인를 만드는 과정을 안내 합니다. 또한 코드에 UI 컨트롤을 노출 하는 Xamarin 디자이너를 소개 하 고 tvOS 응용 프로그램을 빌드, 실행 및 테스트 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 6E0AFE58-A13B-492F-861E-D5D73EB1C4A3
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: 9ad1c63dae312546315406d40858ce24802c6a58
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769306"
---
# <a name="hello-tvos-quick-start-guide"></a>Hello, tvOS 빠른 시작 가이드

_이 가이드에서는 첫 번째 tvOS 앱 및 해당 개발 도구 체인를 만드는 과정을 안내 합니다. 또한 코드에 UI 컨트롤을 노출 하는 Xamarin 디자이너를 소개 하 고 tvOS 응용 프로그램을 빌드, 실행 및 테스트 하는 방법을 보여 줍니다._

Apple은 tvOS 11을 실행 하는 apple tv 4K 인 apple TV의 5 세대를 출시 했습니다.

Apple TV 플랫폼은 개발자에 게 열려 있어 풍부한 몰입 형 앱을 만들고 Apple TV의 기본 제공 앱 스토어를 통해 릴리스 합니다.

Xamarin.ios 개발에 익숙한 경우 tvOS 매우 간단한 전환을 찾아야 합니다. 대부분의 Api와 기능은 동일 하지만 많은 공통 Api (예: WebKit)를 사용할 수 없습니다. 또한 Siri 원격으로을 사용 하 여 작업 하는 경우 터치 스크린 기반 iOS 장치에는 없는 몇 가지 디자인 과제가 있습니다.

이 가이드에서는 Xamarin 앱에서 tvOS를 사용 하는 방법을 소개 합니다. TvOS에 대 한 자세한 내용은 apple [TV 4k 준비](https://developer.apple.com/tvos/) 설명서를 참조 하세요.

## <a name="overview"></a>개요

TvOS를 사용 하면 *Swift* (또는 *객관적인 C*) 및 *Xcode*에서 C# 개발할 때 사용 되는 것과 동일한 OS X 라이브러리 및 인터페이스 컨트롤을 사용 하 여 및 .net에서 완전 한 네이티브 Apple TV 앱을 개발할 수 있습니다.

또한 tvOS 앱은 C# 및 .net으로 작성 되므로 일반적으로 백 엔드 코드를 Xamarin.ios, xamarin Android 및 xamarin.ios 앱과 공유할 수 있습니다. 모든 플랫폼에서 기본 환경을 제공 하는 동시에

이 문서에서는 단추 클릭 횟수를 계산 하는 기본 **Hello, tvOS** 앱을 빌드하는 프로세스를 안내 하 여 TvOS 및 Visual Studio를 사용 하 여 Apple TV 앱을 만드는 데 필요한 주요 개념을 소개 합니다.

[![](hello-tvos-images/run05.png "예제 앱 실행")](hello-tvos-images/run05.png#lightbox)

다음 개념을 다룹니다.

- **Mac용 Visual Studio** – Mac용 Visual Studio를 소개 하 고이를 사용 하 여 tvOS 응용 프로그램을 만드는 방법을 소개 합니다.
- **TvOS 앱의 분석** – tvOS 앱이 구성 되는 항목입니다.
- **사용자 인터페이스 만들기** –를 사용 하 여 사용자 인터페이스를 만드는 방법 Xamarin Designer for iOS 합니다.
- **배포 및 테스트** – tvOS 시뮬레이터 및 실제 tvOS 하드웨어에서 앱을 실행 하 고 테스트 하는 방법을 설명 합니다.

## <a name="starting-a-new-xamarintvos-app-in-visual-studio-for-mac"></a>Mac용 Visual Studio에서 새 tvOS 앱을 시작 하는 중

위에서 설명한 것 처럼 기본 화면에 단일 단추 및 레이블을 추가 `Hello-tvOS` 하는 Apple TV 앱을 만듭니다. 단추를 클릭하면 레이블에 단추 클릭 횟수가 표시됩니다.

시작 하려면 다음을 수행 합니다.

1. Mac용 Visual Studio 시작:

    [![](hello-tvos-images/setup01.png "Visual Studio for Mac")](hello-tvos-images/setup01.png#lightbox)
2. 화면의 왼쪽 위 모서리에 있는 **새 솔루션 ...** 링크를 클릭 하 여 **새 프로젝트** 대화 상자를 엽니다.
3. **TvOS** > **app** 단일 뷰 앱을 선택 하 고 다음 단추를 클릭 합니다. > 

    [![](hello-tvos-images/setup02.png "단일 뷰 앱 선택")](hello-tvos-images/setup02.png#lightbox)
4. `Hello, tvOS` **앱 이름**으로를 입력 하 고 **조직 식별자** 를 입력 한 후 **다음** 단추를 클릭 합니다.

    [![](hello-tvos-images/setup04.png "Hello, tvOS를 입력 합니다.")](hello-tvos-images/setup04.png#lightbox)
5. `Hello_tvOS` **프로젝트 이름** 으로를 입력 하 고 **만들기** 단추를 클릭 합니다.

    [![](hello-tvos-images/setup03.png "HellotvOS 입력")](hello-tvos-images/setup03.png#lightbox)

Mac용 Visual Studio는 새 tvOS 앱을 만들고 응용 프로그램의 솔루션에 추가 되는 기본 파일을 표시 합니다.

 [![](hello-tvos-images/project01.png "기본 파일 보기")](hello-tvos-images/project01.png#lightbox)

Mac용 Visual Studio는 Visual Studio와 정확히 동일한 방식으로 **솔루션과** **프로젝트**를 사용 합니다. 솔루션은 하나 이상의 프로젝트를 보관할 수 있는 컨테이너이고, 프로젝트는 애플리케이션, 지원 라이브러리, 테스트 애플리케이션 등을 포함할 수 있습니다. 이 경우 Mac용 Visual Studio는 솔루션 및 응용 프로그램 프로젝트를 모두 만들었습니다.

원하는 경우 공통 된 공유 코드를 포함 하는 하나 이상의 코드 라이브러리 프로젝트를 만들 수 있습니다. 이러한 라이브러리 프로젝트는 표준 .NET 응용 프로그램을 빌드하는 경우와 마찬가지로 응용 프로그램 프로젝트에서 사용 하거나 다른 tvOS 앱 프로젝트 (또는 코드 형식에 따라 xamarin.ios, Xamarin.ios 및 Xamarin.ios)와 공유할 수 있습니다.

## <a name="anatomy-of-a-xamarintvos-app"></a>TvOS 앱의 분석

IOS 프로그래밍에 익숙한 경우 여기에서 많은 유사성을 확인할 수 있습니다. 실제로 tvOS 9는 iOS 9의 하위 집합 이므로, 여기에서는 많은 개념이 함께 진행 됩니다.

프로젝트의 파일을 살펴보겠습니다.

- `Main.cs` - 앱의 진입점을 포함합니다. 앱이 실행되면 첫 번째 클래스와 실행되는 메서드를 포함합니다.
- `AppDelegate.cs`–이 파일에는 운영 체제의 이벤트를 수신 하는 기본 응용 프로그램 클래스가 포함 됩니다.
- `Info.plist`–이 파일에는 응용 프로그램 이름, 아이콘 등의 응용 프로그램 속성이 포함 되어 있습니다.
- `ViewController.cs`– 주 창을 나타내며이 클래스의 수명 주기를 제어 하는 클래스입니다.
- `ViewController.designer.cs`–이 파일은 주 화면의 사용자 인터페이스와 통합 하는 데 도움이 되는 배관 코드를 포함 합니다.
- `Main.storyboard`– 주 창에 대 한 UI입니다. 이 파일은 Xamarin Designer for iOS에서 만들고 유지 관리할 수 있습니다.

다음 섹션에서는 이러한 파일 중 일부를 간략히 살펴보겠습니다. 나중에 더 자세히 살펴볼 것 이지만, 이제 기본 사항을 이해 하는 것이 좋습니다.

### <a name="maincs"></a>Main.cs

파일 `Main.cs` 에는 새 tvOS `Main` 앱 인스턴스를 만들고 OS 이벤트를 처리 하는 클래스의 이름을 전달 하는 정적 메서드가 포함 되어 있습니다 .이 경우 `AppDelegate` 에는 클래스입니다.

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

이 파일에는 `AppDelegate` 창을 만들고 OS 이벤트를 수신 하는 클래스가 포함 되어 있습니다. `AppDelegate.cs`

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

이 코드는 이전에 iOS 응용 프로그램을 빌드하지 않은 경우에 익숙하지 않을 수 있지만 매우 간단 합니다. 중요 한 줄을 살펴보겠습니다.

먼저, 클래스 수준 변수 선언을 살펴보겠습니다.

```csharp
public override UIWindow Window {
            get;
            set;
        }

```

속성 `Window` 은 주 창에 대 한 액세스를 제공 합니다. tvOS는 MVC ( *모델 뷰 컨트롤러* ) 패턴 이라고 하는 기능을 사용 합니다. 일반적으로 사용자가 만드는 모든 창 (그리고 windows 내의 다른 많은 항목)에 대해 창 수명 주기를 담당 하는 컨트롤러가 있습니다. 예를 들어, 창에 새 보기 (컨트롤)를 추가 하 여이를 표시 합니다.

다음에는 `FinishedLaunching` 메서드가 있습니다. 이 메서드는 응용 프로그램을 인스턴스화한 후에 실행 되며 실제로 응용 프로그램 창을 만들고 여기에 뷰를 표시 하는 프로세스를 시작 합니다. 앱에서 Storyboard를 사용 하 여 UI를 정의 하므로 여기에는 추가 코드가 필요 하지 않습니다.

템플릿에는 `DidEnterBackground` 및와 같은 다양 한 `WillEnterForeground`다른 메서드가 제공 됩니다. 응용 프로그램 이벤트를 앱에서 사용 하지 않는 경우 안전 하 게 제거할 수 있습니다.

### <a name="viewcontrollercs"></a>ViewController.cs

클래스 `ViewController` 는 기본 창의 컨트롤러입니다. 즉, 주 창의 수명 주기를 담당 합니다. 이에 대해서는 나중에 자세히 살펴보겠습니다. 이제 간단히 살펴보겠습니다.

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

주 창 클래스의 디자이너 파일은 현재 비어 있지만 iOS 디자이너로 사용자 인터페이스를 만들 때 Mac용 Visual Studio에 의해 자동으로 채워집니다.

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

일반적으로 디자이너 파일은 Mac용 Visual Studio에 의해 자동으로 관리 되므로 응용 프로그램에서 창이 나 보기에 추가 하는 컨트롤에 대 한 액세스를 허용 하는 필수 배관 코드를 제공 하는 것이 일반적입니다.

TvOS 앱을 만들고 해당 구성 요소에 대 한 기본적인 이해를 했으므로 UI를 만드는 방법을 살펴보겠습니다.

<a name="Creating-the-User-Interface" />

## <a name="creating-the-user-interface"></a>사용자 인터페이스 만들기

Xamarin Designer for iOS를 사용 하 여 tvOS 앱에 대 한 사용자 인터페이스를 만들 필요가 없습니다. 코드에서 C# 직접 UI를 만들 수 있지만이 문서에서는 다루지 않습니다. 간단히 하기 위해이 자습서의 나머지 부분 전체에서 iOS Designer를 사용 하 여 UI를 만듭니다.

UI 만들기를 시작 하려면 `Main.storyboard` **솔루션 탐색기** 파일을 두 번 클릭 하 여 iOS 디자이너에서 편집할 수 있도록 엽니다.

[![](hello-tvos-images/designer01.png "솔루션 탐색기의 Main.storyboard 파일")](hello-tvos-images/designer01.png#lightbox)

그러면 디자이너가 시작 되 고 다음과 같이 표시 됩니다.

[![](hello-tvos-images/designer02.png "디자이너")](hello-tvos-images/designer02.png#lightbox)

IOS Designer 및 작동 방식에 대 한 자세한 내용은 [Xamarin Designer for iOS 가이드 소개](~/ios/user-interface/designer/introduction.md) 를 참조 하세요.

이제 tvOS 앱의 디자인 화면에 컨트롤을 추가 하기 시작할 수 있습니다.

다음을 수행합니다.

1. 디자인 화면 오른쪽에 있는 **도구 상자**를 찾습니다.

    [![](hello-tvos-images/designer03.png "도구 상자")](hello-tvos-images/designer03.png#lightbox)

    여기에서 찾을 수 없는 경우 보기 **> 패드 > 도구 상자** 로 이동 하 여 봅니다.
2. **도구 상자** 에서 디자인 화면으로 **레이블을** 끌어 옵니다.

    [![](hello-tvos-images/designer04.png "도구 상자에서 레이블 끌기")](hello-tvos-images/designer04.png#lightbox)
3. **속성 패드** `Hello, tvOS` 에서 **title** 속성을 클릭 하 고, 단추의 제목을로 변경 하 고, **글꼴 크기** 를 128으로 설정 합니다.

    [![](hello-tvos-images/designer05.png "제목을 Hello, tvOS로 설정 하 고 글꼴 크기를 128으로 설정 합니다.")](hello-tvos-images/designer05.png#lightbox)
4. 모든 단어가 표시 되도록 레이블의 크기를 조정 하 고 창 위쪽의 가운데에 배치 합니다.

    [![](hello-tvos-images/designer06.png "레이블 크기 조정 및 가운데 맞춤")](hello-tvos-images/designer06.png#lightbox)
5. 이제 레이블이 의도 한 대로 표시 되도록 해당 위치에 대 한 제약을 받아야 합니다. 화면 크기에 관계 없이 이렇게 하려면 *T 모양 핸들이* 표시 될 때까지 레이블을 클릭 합니다.

    [![](hello-tvos-images/designer07.png "T 모양 핸들입니다.")](hello-tvos-images/designer07.png#lightbox)
6. 레이블을 가로로 제한 하려면 가운데 사각형을 선택 하 고 세로 파선으로 끕니다.

    [![](hello-tvos-images/designer08.png "중심 사각형 선택")](hello-tvos-images/designer08zoom.png#lightbox)

     레이블에 주황색을 설정 해야 합니다.
7. 레이블 위쪽에서 T 핸들을 선택 하 고 창의 위쪽 가장자리로 끕니다.

    [![](hello-tvos-images/designer09.png "핸들을 창의 위쪽 가장자리로 끌어 옵니다.")](hello-tvos-images/designer09.png#lightbox)
8. 그런 다음 아래 그림과 같이 너비와 높이 *뼈 핸들* 을 차례로 클릭 합니다.

    [![](hello-tvos-images/designer10.png "너비 및 높이 뼈 핸들")](hello-tvos-images/designer10.png#lightbox)

     각 *뼈 핸들* 을 클릭 하면 너비와 높이를 각각 선택 하 여 고정 된 차원을 설정 합니다.
9. 완료 되 면 제약 조건은 속성 패드의 레이아웃 탭에 있는 것과 유사 하 게 표시 됩니다.

    [![](hello-tvos-images/designer11.png "예제 제약 조건")](hello-tvos-images/designer11.png#lightbox)
10. **도구 상자** 에서 **단추** 를 끌어 레이블 아래에 놓습니다.
11. **속성 패드** `Click Me`에서 **title** 속성을 클릭 하 고 단추의 제목을 다음과 같이 변경 합니다.

    [![](hello-tvos-images/designer12.png "단추 제목을 변경 하 여 클릭 합니다.")](hello-tvos-images/designer12.png#lightbox)
12. 위의 5 ~ 8 단계를 반복 하 여 tvOS 창에서 단추를 제한 합니다. 그러나 #7 단계에서와 같이 T 핸들을 창의 맨 위로 끄는 대신 레이블 맨 아래로 끌어 옵니다.

    [![](hello-tvos-images/designer14.png "단추 제한")](hello-tvos-images/designer14.png#lightbox)
13. 단추 아래에 있는 다른 레이블을 끌어서 첫 번째 레이블과 같은 너비로 크기를 조정 하 고 **가운데** **맞춤** 을 설정 합니다.

    [![](hello-tvos-images/designer15.png "단추 아래에 있는 다른 레이블을 끌어서 첫 번째 레이블과 같은 너비로 크기를 조정 하 고 가운데 맞춤을 설정 합니다.")](hello-tvos-images/designer15.png#lightbox)
14. 첫 번째 레이블 및 단추와 마찬가지로이 레이블을 가운데로 설정 하 고 위치 및 크기에 고정 합니다.

    [![](hello-tvos-images/designer16.png "위치 및 크기에 레이블 고정")](hello-tvos-images/designer16.png#lightbox)
15. 사용자 인터페이스에 대 한 변경 내용을 저장 합니다.

컨트롤의 크기를 조정 하 고 이동 하는 동안에는 디자이너가 [APPLE TV 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)을 기반으로 하는 유용한 맞춤 힌트를 제공 하는 것을 알 수 있습니다. 이러한 지침은 Apple TV 사용자에 게 친숙 한 모양과 느낌을 주는 고품질 응용 프로그램을 만드는 데 도움이 됩니다.

**문서 개요** 섹션에 표시 되는 경우 사용자 인터페이스를 구성 하는 요소의 레이아웃과 계층이 표시 되는 방식을 확인 합니다.

[![](hello-tvos-images/designer17.png "문서 개요 섹션")](hello-tvos-images/designer17.png#lightbox)

여기에서 필요에 따라 편집 하거나 끌 항목을 선택 하 여 UI 요소를 다시 정렬할 수 있습니다. 예를 들어 다른 요소에서 UI 요소를 검사 하는 경우이 요소를 목록의 맨 아래로 끌어 창에서 맨 위 항목으로 만들 수 있습니다.

이제 사용자 인터페이스를 만들었으므로 tvOS에서 코드를 C# 사용 하 여 액세스 하 고 상호 작용할 수 있도록 UI 항목을 노출 해야 합니다.

### <a name="accessing-the-controls-in-the-code-behind"></a>코드 숨김으로 컨트롤에 액세스

IOS 디자이너에서 코드를 통해 추가 된 컨트롤에 액세스 하는 두 가지 주요 방법이 있습니다.

- 컨트롤에 이벤트 처리기를 만듭니다.
- 나중에 참조할 수 있도록 컨트롤의 이름을 지정 합니다.

둘 중 하나가 추가 되 면 내의 `ViewController.designer.cs` partial 클래스가 변경 내용을 반영 하도록 업데이트 됩니다. 이를 통해 뷰 컨트롤러의 컨트롤에 액세스할 수 있습니다.

### <a name="creating-an-event-handler"></a>이벤트 처리기 만들기

이 샘플 응용 프로그램에서는 단추를 클릭할 때 발생 하는 것을 원하지 않으므로 단추의 특정 이벤트에 이벤트 처리기를 추가 _해야 합니다._ 이를 설정 하려면 다음을 수행 합니다.

1. Xamarin iOS 디자이너에서 보기 컨트롤러의 단추를 선택 합니다.
2. 속성 패드에서 **이벤트** 탭을 선택 합니다.

    [![](hello-tvos-images/event1.png "이벤트 탭")](hello-tvos-images/event1.png#lightbox)
3. TouchUpInside 이벤트를 찾아 이라는 `Clicked`이벤트 처리기를 제공 합니다.

    [![](hello-tvos-images/event2.png "TouchUpInside 이벤트입니다.")](hello-tvos-images/event2.png#lightbox)
4. **Enter**키를 누르면 코드에서 이벤트 처리기에 대 한 위치를 제안 하는 **viewcontroller**.cs 파일이 열립니다. 키보드의 화살표 키를 사용 하 여 위치를 설정 합니다.

    [![](hello-tvos-images/event3.png "위치 설정")](hello-tvos-images/event3.png#lightbox)
5. 이렇게 하면 아래와 같이 부분 메서드를 만듭니다.

    [![](hello-tvos-images/event4.png "부분 메서드")](hello-tvos-images/event4.png#lightbox)

이제 단추를 사용할 수 있는 일부 코드를 추가 하기 시작할 준비가 되었습니다.

### <a name="naming-a-control"></a>컨트롤 이름 지정

단추를 클릭 하면 클릭 횟수에 따라 레이블이 업데이트 됩니다. 이렇게 하려면 코드에서 레이블에 액세스 해야 합니다. 이름을 지정 하 여이 작업을 수행 합니다. 다음을 수행합니다.

1. 스토리 보드를 열고 보기 컨트롤러의 맨 아래에 있는 레이블을 선택 합니다.
2. 속성 패드에서 **위젯** 탭을 선택 합니다.

    [![](hello-tvos-images/name1.png "위젯 탭 선택")](hello-tvos-images/name1.png#lightbox)
3. **Id > 이름**아래에서 다음 `ClickedLabel`을 추가 합니다.

    [![](hello-tvos-images/name2.png "ClickedLabel 설정")](hello-tvos-images/name2.png#lightbox)

이제 레이블 업데이트를 시작할 준비가 되었습니다.

### <a name="how-controls-are-accessed"></a>컨트롤에 액세스 하는 방법

`ViewController.designer.cs` **솔루션 탐색기** 에서를 선택 하면 `ClickedLabel` `Clicked` 레이블과 이벤트 처리기가의 C# **콘센트** 및 **동작** 에 매핑되는 방법을 확인할 수 있습니다.

[![](hello-tvos-images/accesscontrol.png "출 선 및 작업")](hello-tvos-images/accesscontrol.png#lightbox)

는 partial 클래스 이므로 Mac용 Visual Studio는 클래스에 대 한 변경 내용을 덮어쓸 수 있는 수정 `ViewController.cs` 이필요하지않습니다.`ViewController.designer.cs`

이러한 방식으로 UI 요소를 노출 하면 뷰 컨트롤러에서 UI 요소에 액세스할 수 있습니다.

일반적으로 사용자가 직접 열 필요가 없습니다 `ViewController.designer.cs` . 여기에는 교육용 으로만 제공 됩니다.

<a name="Writing-the-Code" />

## <a name="writing-the-code"></a>코드 작성

사용자 인터페이스를 만들고, 해당 UI 요소가 **콘센트** 및 **작업**을 통해 코드에 노출 되 면 프로그램 기능을 제공 하는 코드를 작성할 준비가 된 것입니다.

응용 프로그램에서 첫 번째 단추를 클릭할 때마다 단추를 클릭 한 횟수를 표시 하는 레이블을 업데이트할 예정입니다. 이렇게 하려면 `ViewController.cs` **Solution Pad**에서 파일을 두 번 클릭 하 여 편집을 위해 파일을 열어야 합니다.

[![](hello-tvos-images/code01.png "Solution Pad")](hello-tvos-images/code01.png#lightbox)

첫째, 발생 한 클릭 수를 추적 하기 위해 `ViewController` 클래스에서 클래스 수준 변수를 만들어야 합니다. 클래스 정의를 편집하고 다음과 비슷하게 만듭니다.

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

그런 다음 동일한 클래스 (`ViewController`)에서 **ViewDidLoad** 메서드를 재정의 하 고 레이블에 대 한 초기 메시지를 설정 하는 코드를 추가 해야 합니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Set the initial value for the label
    ClickedLabel.Text = "Button has not been clicked yet.";
}
```

는 `ViewDidLoad` `Initialize` `ViewDidLoad` OS가 파일에서사용자인터페이스를로드하고인스턴스화한후에호출되기때문에와같은다른메서드대신를사용해야`.storyboard` 합니다. `.storyboard` 파일이 완전히 로드 되어 인스턴스화되기 `NullReferenceException` 전에 레이블 컨트롤에 액세스 하려고 하면 레이블 컨트롤이 아직 만들어지지 않았기 때문에 오류가 발생 합니다.

다음으로 단추를 클릭 하 여 사용자에 게 응답 하는 코드를 추가 해야 합니다. 만든 partial 클래스에 다음을 추가 합니다.

```csharp
partial void Clicked (UIButton sender)
{
    ClickedLabel.Text = string.Format("The button has been clicked {0} time{1}.", ++numberOfTimesClicked, (numberOfTimesClicked
```

이 코드는 사용자가 단추를 클릭할 때마다 호출 됩니다.

모든 것이 준비 되었으므로 이제 tvOS 응용 프로그램을 빌드하고 테스트할 준비가 되었습니다.

## <a name="testing-the-application"></a>애플리케이션 테스트

응용 프로그램을 빌드하고 실행 하 여 예상 대로 실행 되는지 확인 해야 합니다. 모든 단계를 한 번에 빌드하고 실행 하거나 실행 하지 않고 빌드할 수 있습니다.

응용 프로그램을 빌드할 때마다 원하는 빌드 종류를 선택할 수 있습니다.

- **디버그** – 디버그 빌드는 응용 프로그램이 실행 되는 동안 발생 하는 상황을 디버그할 수 있도록 하는 추가 메타 데이터를 포함 하는 ' ' (응용 프로그램) 파일로 컴파일됩니다.
- **릴리스** – 릴리스 빌드는 ' ' 파일도 만들지만 디버그 정보를 포함 하지 않으므로 더 작고 더 빠르게 실행 됩니다.  

Mac용 Visual Studio 화면의 왼쪽 위 모서리에 있는 **구성 선택기에서** 빌드 유형을 선택할 수 있습니다.

[![](hello-tvos-images/run01.png "빌드 유형 선택")](hello-tvos-images/run01.png#lightbox)

### <a name="building-the-application"></a>응용 프로그램 빌드

이 경우 디버그 빌드를 원하는 것 이므로 **디버그** 가 선택 되어 있는지 확인 합니다. 먼저 **⌘ B**를 누르거나 **빌드** 메뉴에서 **모두 빌드**를 선택 하 여 응용 프로그램을 빌드 해 보겠습니다.

오류가 발생 하지 않으면 Mac용 Visual Studio의 상태 표시줄에 **빌드 성공** 메시지가 표시 됩니다. 오류가 있는 경우 프로젝트를 검토 하 고 단계를 올바르게 수행 했는지 확인 합니다. 먼저 코드 (Xcode 및 Mac용 Visual Studio)가 자습서의 코드와 일치 하는지 확인 합니다.

### <a name="running-the-application"></a>응용 프로그램 실행

응용 프로그램을 실행 하려면 다음 세 가지 옵션을 사용할 수 있습니다.

- **⌘ + Enter**를 누릅니다.
- **실행** 메뉴에서 **디버그**를 선택합니다.
- Mac용 Visual Studio 도구 모음에서 **재생** 단추를 클릭합니다(**솔루션 탐색기** 바로 위에 있음).

응용 프로그램이 빌드되고 (아직 빌드되지 않은 경우), 디버그 모드에서 시작 되 면 tvOS 시뮬레이터가 시작 되 고 앱이 시작 되 고 기본 인터페이스 창이 표시 됩니다.

[![샘플 앱 홈 화면](hello-tvos-images/run03.png)](hello-tvos-images/run03.png#lightbox)

**하드웨어** 메뉴에서 시뮬레이터를 제어할 수 있도록 **Apple TV 원격 표시** 를 선택 합니다.

[![](hello-tvos-images/run04.png "Apple TV 원격 표시를 선택 합니다.")](hello-tvos-images/run04.png#lightbox)

시뮬레이터의 원격을 사용 하 여 단추를 몇 번 클릭 하면 레이블이 다음 개수로 업데이트 됩니다.

[![](hello-tvos-images/run05.png "업데이트 된 수의 레이블")](hello-tvos-images/run05.png#lightbox)

지금까지 여기에서는 많은 사항을 설명 했지만이 자습서를 처음부터 끝까지 수행한 경우 tvOS 앱의 구성 요소와 이러한 구성 요소를 만드는 데 사용 되는 도구에 대해 확실 하 게 이해 해야 합니다.

## <a name="where-to-next"></a>다음 위치

Apple TV 앱을 개발 하는 경우 사용자와 인터페이스 간의 연결 끊기 (사용자의 손을 사용 하지 않는 공간에 있음)와 앱 크기 및 저장소에 대 한 제한 tvOS 때문에 몇 가지 문제를 해결할 수 있습니다.

결과적으로 tvOS 앱의 디자인으로 이동 하기 전에 다음 문서를 읽을 것을 적극 권장 합니다.

- [TvOS 9 소개](~/ios/tvos/platform/tvos9.md) -이 문서에서는 tvOS 9의 tvOS 개발자에 게 제공 되는 새로운 api 및 수정 된 api 및 기능을 소개 합니다.
- [탐색 및 포커스 작업](~/ios/tvos/app-fundamentals/navigation-focus.md) -tvOS 앱의 사용자는 장치 화면에서 이미지를 탭 하지만 Siri 원격을 사용 하 여 대화방에서 간접적으로 사용 하는 iOS와 마찬가지로이 인터페이스와 직접 상호 작용 하지 않습니다. 이 문서에서는 tvOS 앱의 사용자 인터페이스에서 탐색을 처리 하는 데 중점의 개념과이를 사용 하는 방법을 설명 합니다.
- [Siri 원격 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) – 사용자가 Apple TV와 상호 작용 하는 기본 방법인 tvOS 앱은 포함 된 Siri 원격을 통해 진행 됩니다. 앱이 게임 인 경우에는 앱에서 iOS (MFI) Bluetooth 게임 컨트롤러에 대해 만든 타사에 대 한 지원을 선택적으로 빌드할 수 있습니다. 이 문서에서는 tvOS 앱에서 새 Siri 원격 및 Bluetooth 게임 컨트롤러를 지 원하는 방법을 설명 합니다.
- [리소스 및 데이터 저장소](~/ios/tvos/app-fundamentals/resources-data-storage.md) – iOS 장치와 달리 새로운 Apple TV는 tvOS apps에 대 한 영구 로컬 저장소를 제공 하지 않습니다. 결과적으로 tvOS 앱이 정보 (예: 사용자 기본 설정)를 유지 해야 하는 경우 iCloud에서 해당 데이터를 저장 하 고 검색 해야 합니다. 이 문서에서는 tvOS 앱에서 리소스 및 영구 데이터 저장소를 사용 하는 방법을 설명 합니다.
- [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) -captivating 아이콘 및 이미지 만들기는 Apple TV 앱에 대 한 몰입 형 사용자 환경을 개발 하는 데 중요 한 부분입니다. 이 가이드는 tvOS 앱에 필요한 그래픽 자산을 만들고 포함 하는 데 필요한 단계를 다룹니다.
- [사용자 인터페이스](~/ios/tvos/user-interface/index.md) – tvOS로 작업할 때 UI (사용자 인터페이스) 컨트롤을 포함 하 여 Xcode의 INTERFACE BUILDER 및 ux 디자인 원칙을 포함 하 여 일반 Ux (사용자 환경) 범위입니다.
- [배포 및 테스트](~/ios/tvos/deploy-test/index.md) -이 섹션에서는 앱을 배포 하는 방법 뿐만 아니라 앱을 테스트 하는 데 사용 되는 항목을 설명 합니다. 이러한 항목에는 디버깅에 사용 되는 도구, 테스터에 게 배포 및 Apple TV 앱 스토어에 응용 프로그램을 게시 하는 방법이 포함 됩니다.

TvOS와 관련 된 문제가 발생 하는 경우 문제 및 해결 방법에 대 한 목록은 [문제 해결](~/ios/tvos/troubleshooting.md) 설명서를 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 간단한 Hello, tvOS 앱을 만들어 Mac용 Visual Studio으로 tvOS 앱을 개발 하는 빠른 시작을 제공 했습니다. TvOS 장치 프로 비전, 인터페이스 생성, tvOS 코딩, tvOS 시뮬레이터에서의 테스트에 대 한 기본 사항에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin을 사용 하 여 tvOS 용 앱 빌드 (비디오)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
