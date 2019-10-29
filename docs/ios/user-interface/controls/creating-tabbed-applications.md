---
title: Xamarin.ios의 탭 모음 및 탭 모음 컨트롤러
description: 이 문서에서는 iOS 탭 모음 컨트롤러 및 Xamarin.ios와 함께 사용 하는 방법을 설명 합니다. Uitab 컨트롤러를 설정 하 고, 이미지 작업을 수행 하 고, 배지 값을 설정 하 고, 이벤트에 대 한 작업을 수행 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 9f8a5e568946e1aea8541211ec3adc45a25f1897
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022144"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Xamarin.ios의 탭 모음 및 탭 모음 컨트롤러

탭 응용 프로그램은 iOS에서 여러 화면에 특정 순서로 액세스할 수 있는 사용자 인터페이스를 지 원하는 데 사용 됩니다. 응용 프로그램은 `UITabBarController` 클래스를 통해 이러한 다중 화면 시나리오에 대 한 지원을 쉽게 포함할 수 있습니다. `UITabBarController`는 다중 화면 관리를 처리 하므로 응용 프로그램 개발자가 각 화면에 대 한 세부 정보에 집중할 수 있습니다.

일반적으로 탭 응용 프로그램은 주 창의 `RootViewController` 되는 `UITabBarController`를 사용 하 여 빌드됩니다. 그러나 약간의 추가 코드를 사용 하면 응용 프로그램에서 먼저 로그인 화면을 표시 한 다음 탭 인터페이스를 사용 하는 시나리오와 같은 다른 초기 화면에도 탭 응용 프로그램을 연속 해 서 사용할 수 있습니다.

간단한 응용 프로그램의 연습을 수행 하 여 여기에서 탭 사용을 살펴보겠습니다. 그런 다음 `RootViewController` 되지 않은 시나리오에서 탭으로 작업 하는 방법을 살펴보겠습니다.

## <a name="introducing-uitabbarcontroller"></a>UITabBarController 소개

`UITabBarController`는 다음과 같은 방법으로 탭 응용 프로그램 개발을 지원 합니다.

- 여러 컨트롤러를 추가할 수 있습니다.
- 사용자가 컨트롤러와 뷰 간을 전환할 수 있도록 `UITabBar` 클래스를 통해 탭 사용자 인터페이스 제공 

컨트롤러는 `UIViewController` 배열인 `ViewControllers` 속성을 통해 `UITabBarController`에 추가 됩니다. `UITabBarController` 자체는 적절 한 컨트롤러를 로드 하 고 선택 된 탭을 기준으로 보기를 표시 합니다.

탭은 `UITabBar` 인스턴스에 포함 된 `UITabBarItem` 클래스의 인스턴스입니다. 각 `UITabBar` 인스턴스는 각 탭에 있는 컨트롤러의 `TabBarItem` 속성을 통해 액세스할 수 있습니다.

`UITabBarController`를 사용 하는 방법을 이해 하려면 하나를 사용 하는 간단한 응용 프로그램을 빌드하는 과정을 살펴보겠습니다.

## <a name="tabbed-application-walkthrough"></a>탭 응용 프로그램 연습

이 연습에서는 다음 응용 프로그램을 만듭니다.

[![](creating-tabbed-applications-images/00-app.png "Sample tabbed app")](creating-tabbed-applications-images/00-app.png#lightbox)

Mac용 Visual Studio에서 사용할 수 있는 탭 응용 프로그램 템플릿이 이미 있지만이 예에서는 빈 프로젝트에서 작업 하 여 응용 프로그램의 구성 방식을 보다 잘 이해할 수 있습니다.

 <a name="Creating_the_Application" />

### <a name="creating-the-application"></a>애플리케이션 작성

새 응용 프로그램을 만들어 시작 해 보겠습니다.

Mac용 Visual Studio에서 **파일 > 새 > 솔루션** 메뉴 항목을 선택 하 고 **iOS > 앱 > 빈 프로젝트** 템플릿을 선택 하 고 아래와 같이 프로젝트 이름을 `TabbedApplication`로 선택 합니다.

[![](creating-tabbed-applications-images/newsolution1.png "Select the Empty Project template")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "Name the project TabbedApplication")](creating-tabbed-applications-images/newsolution2.png#lightbox)

### <a name="adding-the-uitabbarcontroller"></a>UITabBarController 추가

그런 다음 **파일 > 새 파일** 을 선택 하 고 **일반: 빈 클래스** 템플릿을 선택 하 여 빈 클래스를 추가 합니다. 아래와 같이 파일 이름을 `TabController`로 합니다.

[![](creating-tabbed-applications-images/02-newclass.png "Add the TabController class")](creating-tabbed-applications-images/02-newclass.png#lightbox)

`TabController` 클래스에는 `UIViewControllers`배열을 관리할 `UITabBarController` 구현이 포함 됩니다. 사용자가 탭을 선택 하면 해당 보기 컨트롤러에 대 한 보기를 제공 하는 `UITabBarController` 됩니다.

`UITabBarController` 구현 하려면 다음을 수행 해야 합니다.

1. `TabController`의 기본 클래스를 `UITabBarController`로 설정 합니다. 
1. `TabController`에 추가할 `UIViewController` 인스턴스를 만듭니다. 
1. `TabController`의 `ViewControllers` 속성에 할당 된 배열에 `UIViewController` 인스턴스를 추가 합니다. 

`TabController` 클래스에 다음 코드를 추가 하 여 이러한 단계를 수행 합니다.

```csharp
using System;
using UIKit;

namespace TabbedApplication {
    public class TabController : UITabBarController {

        UIViewController tab1, tab2, tab3;

        public TabController ()
        {
            tab1 = new UIViewController();
            tab1.Title = "Green";
            tab1.View.BackgroundColor = UIColor.Green;

            tab2 = new UIViewController();
            tab2.Title = "Orange";
            tab2.View.BackgroundColor = UIColor.Orange;

            tab3 = new UIViewController();
            tab3.Title = "Red";
            tab3.View.BackgroundColor = UIColor.Red;

            var tabs = new UIViewController[] {
                tab1, tab2, tab3
            };

            ViewControllers = tabs;
        }
    }
}
```

각 `UIViewController` 인스턴스에 대해 `UIViewController`의 `Title` 속성을 설정 합니다. 컨트롤러가 `UITabBarController`에 추가 되 면 `UITabBarController`는 각 컨트롤러에 대 한 `Title`를 읽고 아래와 같이 관련 탭의 레이블에 표시 합니다.

[![](creating-tabbed-applications-images/00-app.png "The sample app run")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>TabController를 RootViewController로 설정

컨트롤러가 탭에 배치 되는 순서는 `ViewControllers` 배열에 추가 되는 순서에 해당 합니다.

`UITabController`을 첫 번째 화면으로 로드 하려면 `AppDelegate`에 대 한 다음 코드에 나와 있는 것 처럼 창의 `RootViewController`으로 설정 해야 합니다.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    UIWindow window;
    TabController tabController;
    
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        window = new UIWindow (UIScreen.MainScreen.Bounds);
        
        tabController = new TabController ();
        window.RootViewController = tabController;
        
        window.MakeKeyAndVisible ();
        
        return true;
    }
}
```

지금 응용 프로그램을 실행 하는 경우 `UITabBarController`는 기본적으로 선택 된 첫 번째 탭과 함께 로드 됩니다. 다른 탭 중 하나를 선택 하면 아래와 같이 최종 사용자가 두 번째 탭을 선택한 경우 관련 컨트롤러의 뷰가 `UITabBarController,` 표시 됩니다.

[![](creating-tabbed-applications-images/03-secondtab.png "The second tab shown")](creating-tabbed-applications-images/03-secondtab.png#lightbox)

 <a name="Modifying_TabBarItems" />

### <a name="modifying-tabbaritems"></a>Tab바 항목 수정

이제 실행 중인 탭 응용 프로그램이 있으므로 표시 되는 이미지와 텍스트를 변경 하 고 탭 중 하나에 배지를 추가 하도록 `TabBarItem`를 수정 하겠습니다.

 <a name="Setting_a_System_Item" />

#### <a name="setting-a-system-item"></a>시스템 항목 설정

먼저 시스템 항목을 사용 하도록 첫 번째 탭을 설정 해 보겠습니다. `TabController`생성자에서 `tab1` 인스턴스의 컨트롤러 `Title`를 설정 하는 줄을 제거 하 고 다음 코드로 바꾸어 컨트롤러의 `TabBarItem` 속성을 설정 합니다.

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

`UITabBarSystemItem`를 사용 하 여 `UITabBarItem`를 만들 때 제목 및 이미지는 다음 스크린샷에 표시 된 것 처럼 iOS에서 자동으로 제공 되며, 첫 번째 탭에서 **즐겨찾기** 아이콘 및 제목을 표시 합니다.

 ![](creating-tabbed-applications-images/04a-tabimage.png "The first tab with a star icon")

 <a name="Setting_the_Title_and_Image" />

#### <a name="setting-the-title-and-image"></a>제목 및 이미지 설정

시스템 항목을 사용 하는 것 외에도 `UITabBarItem`의 제목과 이미지를 사용자 지정 값으로 설정할 수 있습니다. 예를 들어 다음과 같이 `tab2` 라는 컨트롤러의 `TabBarItem` 속성을 설정 하는 코드를 변경 합니다.

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

위의 코드에서는 `second.png` 라는 이미지가 Mac용 Visual Studio의 프로젝트 루트에 추가 된 것으로 가정 합니다. 다음과 같이 모든 장치 해상도를 포함 하기 위해 세 개의 이미지를 프로젝트에 실제로 추가 했습니다.

 [![](creating-tabbed-applications-images/tabbedimages7new.png "The images added to the project")](creating-tabbed-applications-images/tabbedimages7new.png#lightbox)

탭 이미지는 일반적인 해상도에 대 한 투명도가 있는 30x30 png, 높은 해상도의 경우 합니다, iPhone 6의 경우 90 x 90 및 해상도입니다. 이 코드에서는 `second.png` 파일을 로드 하기만 하면 됩니다. 그러면 iOS에서 레 티 나 디스플레이를 사용 하는 장치에 고해상도 1을 자동으로 로드 합니다. [이미지 작업](~/ios/app-fundamentals/images-icons/index.md) 가이드에서이에 대해 자세히 알아볼 수 있습니다. 기본적으로 탭 모음 항목은 회색 이며 선택 시 파란색 색조가 표시 됩니다.

**참고:**

위의 이미지는 응용 프로그램 번들의 루트에 콘텐츠가 자동으로 복사 되는 특수 디렉터리인 **resources** 디렉터리에 추가 될 수도 있습니다.

[![](creating-tabbed-applications-images/tabbedapplication8.png "The images as Resources")](creating-tabbed-applications-images/tabbedapplication8.png#lightbox)

또한 `TabBarItem`에 `Title` 속성을 직접 설정 하는 경우 컨트롤러 자체에서 `Title`에 대해 설정 된 모든 값을 재정의 합니다.

이제 응용 프로그램을 실행할 때 두 번째 탭에는 아래와 같이 사용자 지정 제목 및 이미지가 표시 됩니다.

[![](creating-tabbed-applications-images/05-customtab.png "The second tab with a square icon")](creating-tabbed-applications-images/05-customtab.png#lightbox)

 <a name="Setting_the_Badge_Value" />

#### <a name="setting-the-badge-value"></a>배지 값 설정

탭에 배지를 표시할 수도 있습니다. 예를 들어 세 번째 탭에 배지를 설정 하는 다음 코드 줄을 추가 합니다.

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

이를 실행 하면 아래와 같이 탭의 왼쪽 위 모퉁이에 문자열 "Hi"가 표시 된 빨간색 레이블이 표시 됩니다.

[![](creating-tabbed-applications-images/06-badge.png "The second tab with a Hi badge")](creating-tabbed-applications-images/06-badge.png#lightbox)

배지는 읽지 않은 항목, 새 항목을 표시 하는 데 종종 사용 됩니다. 배지를 제거 하려면 아래와 같이 `BadgeValue`를 null로 설정 합니다.

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />

## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>비 RootViewController 시나리오의 탭

위의 예에서는 창의 `RootViewController` 일 때 `UITabBarController`를 사용 하는 방법을 살펴보았습니다. 이 예제에서는 `RootViewController` 아닌 `UITabBarController` 사용 하는 방법 및 Storyboard를 사용 하 여이를 만드는 방법을 보여 줍니다.

 <a name="Initial_Screen_Example" />

### <a name="initial-screen-example"></a>초기 화면 예제

이 시나리오의 경우 초기 화면은 `UITabBarController`아닌 컨트롤러에서 로드 됩니다. 사용자가 단추를 탭 하 여 화면을 조작할 때 동일한 뷰 컨트롤러가 `UITabBarController`에 로드 되 고 사용자에 게 표시 됩니다. 다음 스크린샷은 응용 프로그램 흐름을 보여 줍니다.

[![](creating-tabbed-applications-images/inital-screen-application.png "This screenshot shows the application flow")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

이 예에서는 새 응용 프로그램을 시작 해 보겠습니다. 다시, **iPhone > 앱 > 빈 프로젝트C#()** 템플릿을 사용 합니다. 이번에는 프로젝트`InitialScreenDemo`이름을 지정 합니다.

이 예제에서는 뷰 컨트롤러를 보관할 Storyboard가 필요 합니다. 스토리 보드를 추가 하려면:

- 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 파일**을 선택 합니다.

- 새 파일 대화 상자가 나타나면 **iOS > 빈 IPhone Storyboard**로 이동 합니다.

아래 그림과 같이 새 Storyboard **mainstoryboard.storyboard** 를 호출 해 보겠습니다. 

[![](creating-tabbed-applications-images/new-file-dialog.png "Add a MainStoryboard file to the project")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

스토리 보드 [소개](~/ios/user-interface/storyboards/index.md) 가이드에 설명 되어 있는 이전 비 storyboard 파일에 스토리 보드를 추가할 때 유의 해야 하는 몇 가지 중요 한 단계가 있습니다. 이러한 방법은 다음과 같습니다.

1. 스토리 보드 이름을 `Info.plist`의 **주 인터페이스** 섹션에 추가 합니다.

    [![](creating-tabbed-applications-images/project-options.png "Set the Main Interface to MainStoryboard")](creating-tabbed-applications-images/project-options.png#lightbox)
1. `App Delegate`에서 Window 메서드를 다음 코드로 재정의 합니다.

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

이 예에서는 3 개의 보기 컨트롤러가 필요 합니다. 이름이 `ViewController1`인 하나는 초기 뷰 컨트롤러로 사용 되 고 첫 번째 탭에서는 사용 됩니다. 두 번째 및 세 번째 탭에서 각각 사용 되는 다른 두 개의 명명 된 `ViewController2` 및 `ViewController3`입니다.

Mainstoryboard.storyboard 파일을 두 번 클릭 하 여 디자이너를 열고 세 개의 뷰 컨트롤러를 디자인 화면으로 끌어 옵니다. 이러한 각 뷰 컨트롤러에는 위의 이름에 해당 하는 고유한 클래스가 있어야 합니다. 따라서 **id > 클래스**아래 스크린샷에 나와 있는 것 처럼 이름을 입력 합니다.

[![](creating-tabbed-applications-images/class-name.png "Set the Class to ViewController1")](creating-tabbed-applications-images/class-name.png#lightbox)

필요한 클래스 및 디자이너 파일을 자동으로 생성 Mac용 Visual Studio 아래 그림과 같이 Solution Pad에서 볼 수 있습니다.

[![](creating-tabbed-applications-images/solution-pad2.png "Auto-generated files in the project")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

 <a name="Creating_the_UI" />

#### <a name="creating-the-ui"></a>UI 만들기

다음으로, Xamarin iOS Designer를 사용 하 여 각 ViewController의 보기에 대 한 간단한 사용자 인터페이스를 만듭니다.

오른쪽의 **도구 상자** 에서 `Label` 및 `Button`를 ViewController1로 끌어 놓을 수 있습니다. 다음으로 Properties Pad를 사용 하 여 컨트롤의 이름과 텍스트를 다음과 같이 편집 합니다.

- **레이블** : **`Text` = **
- **단추** `Title` = **사용자가 초기 작업을 수행** 합니다.

`TouchUpInside` 이벤트에서 단추의 표시 여부를 제어 하 고 있으며 코드 숨김으로이를 참조 해야 합니다. 다음 스크린샷에 표시 된 것 처럼 Properties Pad에서 `aButton` **이름** 으로 식별 해 보겠습니다.

[![](creating-tabbed-applications-images/abutton-properties.png "Set the Name to aButton in the Properties Pad")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

이제 Design Surface는 아래 스크린샷에서와 같이 표시 됩니다.

[![](creating-tabbed-applications-images/design-surface1.png "Your Design Surface should now look similar to this screenshot")](creating-tabbed-applications-images/design-surface1.png#lightbox)

각각에 레이블을 추가 하 고 텍스트를 각각 ' 2 '과 ' 3 '으로 변경 하 여 `ViewController2` 및 `ViewController3`에 대 한 자세한 정보를 추가 해 보겠습니다. 이는 사용자가 원하는 탭/보기를 표시 합니다.

#### <a name="wiring-up-the-button"></a>단추를 연결 합니다.

응용 프로그램을 처음 시작할 때 `ViewController1`을 로드 합니다. 사용자가 단추를 탭 하면 단추를 숨기고 첫 번째 탭에서 `ViewController1` 인스턴스와 함께 `UITabBarController`를 로드 합니다.

사용자가 `aButton`을 해제 하면 TouchUpInside 이벤트가 트리거됩니다. 단추를 선택 하 고, 속성 패드의 **이벤트 탭** 에서 이벤트 처리기를 선언 합니다. `InitialActionCompleted` – 코드에서 참조할 수 있습니다. 이는 아래 스크린샷에 설명 되어 있습니다.

[![](creating-tabbed-applications-images/event-handler.png "When the user releases the aButton, trigger a TouchUpInside event")](creating-tabbed-applications-images/event-handler.png#lightbox)

이제 이벤트 `InitialActionCompleted`발생 하는 경우 단추를 숨기도록 뷰 컨트롤러에 지시 해야 합니다. `ViewController1`에서 다음 부분 메서드를 추가 합니다.

```csharp
partial void InitialActionCompleted (UIButton sender)
{
    aButton.Hidden = true;  
}
```

파일을 저장 하 고 응용 프로그램을 실행 합니다. 화면이 표시 되 고 터치 시 단추가 사라집니다.

#### <a name="adding-the-tab-bar-controller"></a>탭 표시줄 컨트롤러 추가

이제 초기 보기가 정상적으로 작동 합니다. 다음으로, 보기 2 및 3과 함께 `UITabBarController`에 추가 하려고 합니다. 디자이너에서 Storyboard를 열어 보겠습니다.

**도구 상자**의 컨트롤러 & 개체에서 **탭 모음 컨트롤러** 를 검색 하 고이를 Design Surface으로 끌어 옵니다. 아래 스크린샷에서 볼 수 있듯이 탭 모음 컨트롤러는 UI를 포함 하지 않으므로 기본적으로 두 개의 뷰 컨트롤러를 제공 합니다.

[![](creating-tabbed-applications-images/tabbarcontroller.png "Adding a Tab Bar Controller to the layout")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

아래쪽에 있는 검은색 표시줄을 선택 하 고 delete 키를 눌러 새 뷰 컨트롤러를 삭제 합니다.

이 스토리 보드에서 Segue를 사용 하 여 Tab바 컨트롤러와 뷰 컨트롤러 간의 전환을 처리할 수 있습니다. 초기 보기와 상호 작용 한 후 사용자에 게 제공 되는 TabBarController로 로드 하려고 합니다. 디자이너에서 설정 해 보겠습니다.

**Ctrl 키를 누른 채** 단추를 TabBarController로 **끕니다** . 마우스를 가져가면 상황에 맞는 메뉴가 표시 됩니다. 모달 segue를 사용 하려고 합니다. 

각 탭을 설정 하려면 아래 그림과 같이 각 보기 컨트롤러에 대 한 Tab 컨트롤러에서 각 보기 컨트롤러에 대 한 **Ctrl + 클릭** 을 차례로 클릭 하 고 상황에 맞는 메뉴에서 관계 **탭** 을 선택 합니다.

[![](creating-tabbed-applications-images/context-menu.png "Select the Tab Relationship")](creating-tabbed-applications-images/context-menu.png#lightbox)

스토리 보드는 아래 스크린샷에서와 비슷해야 합니다.

[![](creating-tabbed-applications-images/segue-layout.png "The Storyboard should resemble this screenshot")](creating-tabbed-applications-images/segue-layout.png#lightbox)

탭 모음 항목 중 하나를 클릭 하 고 속성 패널을 탐색 하는 경우 아래 그림과 같이 다양 한 옵션을 볼 수 있습니다.

[![](creating-tabbed-applications-images/properties-panel.png "Setting the tab options in the Properties Explorer")](creating-tabbed-applications-images/properties-panel.png#lightbox)

이를 사용 하 여 배지, 제목 및 iOS [식별자](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html)와 같은 특정 특성을 편집할 수 있습니다.

지금 응용 프로그램을 저장 하 고 실행 하면 ViewController1 인스턴스가 TabBarController로 로드 될 때 단추가 다시 나타납니다. 현재 뷰에 부모 뷰 컨트롤러가 있는지 확인 하 여이 문제를 해결 해 보겠습니다. 이 경우에는 Tab 컨트롤러 내에 있다는 것을 알 수 있으므로 단추를 숨겨야 합니다. ViewController1 클래스에 아래 코드를 추가 해 보겠습니다.

```csharp
public override void ViewDidLoad ()
{
    if (ParentViewController != null){
        aButton.Hidden = true;
    }
}
```

응용 프로그램이 실행 되 고 사용자가 첫 번째 화면에서 단추를 누르면 아래와 같이 첫 번째 탭에 있는 첫 번째 화면의 뷰가 포함 된 Uitab바 컨트롤러가 로드 됩니다.

[![](creating-tabbed-applications-images/first-view.png "The sample app output")](creating-tabbed-applications-images/first-view.png#lightbox)

<!--Save the files and run the application:

[![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)-->

## <a name="summary"></a>요약

이 문서에서는 응용 프로그램에서 `UITabBarController`를 사용 하는 방법을 설명 했습니다. 각 탭에 컨트롤러를 로드 하는 방법 뿐만 아니라 제목, 이미지, 배지 등의 탭에 대 한 속성을 설정 하는 방법을 살펴보았습니다. 그런 다음 storyboard를 사용 하 여 창 `RootViewController`가 아닌 런타임에 `UITabBarController`를 로드 하는 방법을 검사 했습니다.

## <a name="related-links"></a>관련 링크

- [탭 응용 프로그램 만들기 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/creatingtabbedapplications)
- [이미지 .zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [UITabBarController 클래스 참조](https://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
