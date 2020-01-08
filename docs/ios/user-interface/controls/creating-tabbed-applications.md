---
title: 탭 표시줄 및 Xamarin.iOS에 탭 표시줄 컨트롤러
description: 이 문서에서는 iOS 탭 표시줄 컨트롤러 및 Xamarin.iOS를 사용 하는 방법을 설명 합니다. UITabBarController 설정, 이미지를 사용 하 여 작동 배지 값과 이벤트를 사용 하 여 작업을 설정 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: ad4682e9a3d4de2565bee54ffa159fd739572e24
ms.sourcegitcommit: d8af612b6b3218fea396d2f180e92071c4d4bf92
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75663342"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Xamarin.ios의 탭 모음 및 탭 모음 컨트롤러

탭된 응용 프로그램은 특정 순서 없이 여러 화면에 액세스할 수 있는 사용자 인터페이스를 지원 하기 위해 iOS에서 사용 됩니다. 통해를 `UITabBarController` 클래스를 응용 프로그램에는 이러한 다중 화면 시나리오에 대 한 지원을 쉽게 포함할 수 있습니다. `UITabBarController` 각 화면의 세부 정보를 중점적으로 응용 프로그램 개발자는 다중 화면 관리를 담당 합니다.

일반적으로 응용 프로그램 탭된으로 작성 합니다 `UITabBarController` 되는 `RootViewController` 주 창. 그러나 약간의 추가 코드를 사용 하 여 탭된 응용 프로그램 몇 가지 다른 초기 화면에 응용 프로그램 로그인 화면을 뒤에 탭된 인터페이스를 먼저 제공 되는 시나리오와 같은 연속 해 서에서 데도 수 있습니다.

이 페이지에서는 탭이 응용 프로그램 뷰 계층 구조의 루트에 있고`RootViewController` 되지 않은 시나리오에 대 한 두 가지 시나리오를 모두 설명 합니다.

## <a name="introducing-uitabbarcontroller"></a>UITabBarController 소개

`UITabBarController` 지 원하는 다음과 같은 응용 프로그램 개발을 탭 합니다.

- 여러 컨트롤러를를 추가할 수 있습니다.
- 탭된 사용자 인터페이스를 통해 제공 된 `UITabBar` 사용자가 뷰와 컨트롤러 사이 전환할 수 있도록 클래스입니다. 

컨트롤러에 추가 됩니다는 `UITabBarController` 를 통해 해당 `ViewControllers` 속성을 `UIViewController` 배열입니다. `UITabBarController` 적절 한 컨트롤러를 로드 하 고 선택한 탭에 따라 해당 뷰를 제공 합니다. 자체 처리 합니다.

탭은의 인스턴스를 `UITabBarItem` 에 포함 된 클래스는 `UITabBar` 인스턴스. 각 `UITabBar` 인스턴스를 통해 액세스할 수는 `TabBarItem` 각 탭에 있는 컨트롤러의 속성입니다.

사용 하는 방법을 이해 하는 `UITabBarController`, 하나를 사용 하는 간단한 응용 프로그램을 구축 하는 과정을 살펴보겠습니다.

## <a name="tabbed-application-walkthrough"></a>탭 응용 프로그램 연습

이 연습에 대 한 다음 응용 프로그램을 만들려면 하겠습니다.

[![샘플 앱 탭](creating-tabbed-applications-images/00-app.png)](creating-tabbed-applications-images/00-app.png#lightbox)

Mac용 Visual Studio에서 사용할 수 있는 탭 응용 프로그램 템플릿이 이미 있지만이 예에서는 빈 프로젝트에서 이러한 지침을 사용 하 여 응용 프로그램의 구성 방식을 보다 잘 이해할 수 있습니다.

### <a name="creating-the-application"></a>응용 프로그램 만들기

새 응용 프로그램을 만들어 시작 합니다.

선택 된 **파일 > 새로 만들기 > 솔루션** 선택한 Mac 용 Visual Studio에서 메뉴 항목을 **iOS > 앱 > 빈 프로젝트** 서식 파일을 프로젝트의 이름 `TabbedApplication`아래 표시 된 것 처럼:

[![](creating-tabbed-applications-images/newsolution1.png "Select the Empty Project template")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "Name the project TabbedApplication")](creating-tabbed-applications-images/newsolution2.png#lightbox)

### <a name="adding-the-uitabbarcontroller"></a>UITabBarController 추가

다음을 선택 하 여 빈 클래스를 추가 **파일 > 새 파일** 선택 합니다 **일반: 빈 클래스** 템플릿. 파일 이름을 `TabController` 아래와 같이:

[![](creating-tabbed-applications-images/02-newclass.png "Add the TabController class")](creating-tabbed-applications-images/02-newclass.png#lightbox)

합니다 `TabController` 클래스의 구현이 포함 된 `UITabBarController` 배열을 관리 하는 `UIViewControllers`합니다. 사용자가 탭을 선택 합니다 `UITabBarController` 적절 한 뷰 컨트롤러에 대 한 보기를 제공 해 줄 것입니다.

구현 하는 `UITabBarController` 다음을 수행 해야 합니다.

1. 기본 클래스를 설정 `TabController` 에 `UITabBarController` 입니다. 
1. 만들 `UIViewController` 에 추가할 인스턴스는 `TabController` 합니다. 
1. 추가 `UIViewController` 에 할당 된 배열에는 인스턴스를 `ViewControllers` 의 속성을 `TabController` 입니다. 

다음 코드를 추가 합니다 `TabController` 다음이 단계를 달성 하기 위해 클래스:

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

경우 각 `UIViewController` 설정 인스턴스를 `Title` 의 속성을 `UIViewController`합니다. 컨트롤러에 추가 될 때를 `UITabBarController`, `UITabBarController` 읽을지를 `Title` 각 컨트롤러에 대 한 아래와 같이 연결된 탭의 레이블에 표시:

[![](creating-tabbed-applications-images/00-app.png "The sample app run")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>RootViewController는 TabController 설정

에 추가 된 순서에 해당 하는 컨트롤러 탭에 배치 되도록 하는 순서는 `ViewControllers` 배열입니다.

가져오려는 합니다 `UITabController` 첫 번째 화면으로 로드 하려면 창의 있도록 해야 `RootViewController`에 다음 코드 에서처럼는 `AppDelegate`:

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

응용 프로그램을 지금 실행 하는 경우는 `UITabBarController` 기본적으로 선택 하 고 첫 번째 탭을 사용 하 여 로드 됩니다. 다른 탭을 선택 하면 연결된 된 컨트롤러에서 결과 보기에서 제공 되는 `UITabBarController,` 최종 사용자가 두 번째 탭을 선택 하는 위치 아래와 같이:

![표시 되는 두 번째 탭](creating-tabbed-applications-images/03-secondtab-sml.png)

### <a name="modifying-tabbaritems"></a>TabBarItems 수정

실행 중인 응용 프로그램 탭에서 수정 해 보겠습니다 만들었으므로 `TabBarItem` 표시 되는 텍스트와 이미지를 변경 하려면 뿐만 아니라 탭 중 하나에 배지를 추가 하려면.

#### <a name="setting-a-system-item"></a>시스템 항목 설정

첫째, 시스템 항목을 사용 하 여 첫 번째 탭을 설정 해 보겠습니다. 생성자에는 `TabController`, 컨트롤러를 설정 하는 줄을 제거 `Title` 에 대 한 합니다 `tab1` 인스턴스 및 컨트롤러를 설정 하려면 다음 코드로 바꿉니다 `TabBarItem` 속성:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

만들 때를 `UITabBarItem` 를 사용 하 여는 `UITabBarSystemItem`, 제목 및 이미지는 자동으로 제공 iOS에 의해 보여 주는 아래 스크린샷에 표시 된 것 처럼 합니다 **즐겨찾기** 아이콘 및 제목을 첫 번째 탭에서:

 ![별표 아이콘을 사용 하는 첫 번째 탭](creating-tabbed-applications-images/04a-tabimage-sml.png)

#### <a name="setting-the-image"></a>이미지 설정

시스템 항목, 제목 및 이미지를 사용 하는 것 외에도 `UITabBarItem` 사용자 지정 값으로 설정할 수 있습니다. 예를 들어, 설정 하는 코드를 변경 합니다 `TabBarItem` 라는 컨트롤러의 속성 `tab2` 다음과 같습니다.

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

위의 코드에서는 `second.png` 라는 이미지가 프로젝트의 루트 또는 **Resources** 디렉터리에 추가 된 것으로 가정 합니다. 모든 화면 밀도를 지원 하려면 아래와 같이 세 개의 이미지가 필요 합니다.

![프로젝트에 추가 된 이미지](creating-tabbed-applications-images/tabbedimages7new.png)

올바른 차원에 대 한 지침은 [Apple 사용자 지정 아이콘 페이지](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons/) 의 **탭 모음 아이콘 크기** 섹션을 참조 하세요. 권장 되는 크기는 이미지의 스타일 (원형, 사각형, 와이드 또는 높이)에 따라 달라 집니다.

`Image` 속성은 **두 번째 .png** 파일 이름 으로만 설정 해야 하며, 필요할 때 iOS에서 더 높은 해상도 파일을 자동으로 로드 합니다. 알아볼 수 있습니다이에 대 한 합니다 [이미지를 사용 하 여 작업](~/ios/app-fundamentals/images-icons/index.md) 가이드입니다. 기본적으로 탭 표시줄 항목에는 회색으로 선택한 경우 파랑 색조를 사용 하 여 됩니다.

#### <a name="overriding-the-title"></a>제목 재정의

`Title` 속성이 `TabBarItem`에서 직접 설정 되 면 컨트롤러 자체에서 `Title`에 대해 설정 된 모든 값이 재정의 됩니다.

이 스크린샷에서 두 번째 (가운데) 탭에는 사용자 지정 제목 및 이미지가 표시 됩니다.

![사각형 아이콘이 있는 두 번째 탭](creating-tabbed-applications-images/05-customtab-sml.png)

#### <a name="setting-the-badge-value"></a>배지 값 설정

탭에 배지를 표시할 수도 있습니다. 예를 들어, 세 번째 탭에 배지를 설정 하는 코드의 다음 줄을 추가 합니다.

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

아래와 같이 탭의 왼쪽된 위 모퉁이에 있는 문자열 "Hi"를 사용 하 여 빨간색 레이블을 실행이 발생 합니다.

![Hi 배지를 포함 하는 두 번째 탭](creating-tabbed-applications-images/06-badge-sml.png)

배지, 읽지 않은 번호 표시를 표시 하는 대개 새 항목입니다. 배지를 제거 하려면 설정의 `BadgeValue` 아래와 같이 null로:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>비 RootViewController 시나리오의 탭

위의 예제에서 사용 하는 방법을 살펴보았습니다를 `UITabBarController` 되었을 때를 `RootViewController` 창입니다. 이 예제를 사용 하는 방법을 살펴보겠습니다를 `UITabBarController` 있지 않은 경우는 `RootViewController` 스토리 보드를 사용 하 여이 만드는 방법을 보여 줍니다.

### <a name="initial-screen-example"></a>초기 화면 예제

이 시나리오에 대 한 초기 화면 없는 컨트롤러에서 로드 한 `UITabBarController`합니다. 동일한 뷰 컨트롤러도 로드 될 때 사용자가 단추를 탭 하 여 화면에는 `UITabBarController`는 사용자에 게 제공 됩니다. 다음 스크린샷은 응용 프로그램 흐름을 보여 줍니다.

[![](creating-tabbed-applications-images/inital-screen-application.png "This screenshot shows the application flow")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

이 예제에 대 한 새 응용 프로그램을 시작 해 보겠습니다. 다시 사용 하 여 합니다 **iPhone > 앱 > 빈 프로젝트 (C#)** 이 이번에는 프로젝트를 명명 템플릿에서 `InitialScreenDemo`합니다.

이 예에서는 storyboard를 사용 하 여 뷰 컨트롤러를 레이아웃 합니다. 스토리 보드를 추가 하려면:

- 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 파일**합니다.

- 새 파일 대화 상자에 표시 되 면 이동할 **iOS > 빈 iPhone 스토리 보드**합니다.

이 새 스토리 보드를 호출 해 보겠습니다 **MainStoryboard** 아래 그림과 같이: 

[![](creating-tabbed-applications-images/new-file-dialog.png "Add a MainStoryboard file to the project")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

몇 가지 중요 한 단계에 나와 있는 이전에 비-스토리 보드 파일에 스토리 보드를 추가 하는 경우에 [스토리 보드 소개](~/ios/user-interface/storyboards/index.md) 가이드입니다. 이러한 설정은 다음과 같습니다.

1. 스토리 보드 이름을 추가 합니다 **주 인터페이스** 섹션을 `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "Set the Main Interface to MainStoryboard")](creating-tabbed-applications-images/project-options.png#lightbox)
1. 사용자 `App Delegate`, 창 메서드를 다음 코드로 재정의 합니다.

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

이 예제에 대 한 세 가지 뷰 컨트롤러 필요 예정입니다. 이름이 `ViewController1`인 하나는 초기 뷰 컨트롤러로 사용 되 고 첫 번째 탭에서는 사용 됩니다. 두 번째 및 세 번째 탭에서 각각 사용 되는 다른 두 개의 명명 된 `ViewController2` 및 `ViewController3`입니다.

두 번 MainStoryboard.storyboard 파일을 클릭 하 여 디자이너를 열고 디자인 화면에 세 가지 뷰 컨트롤러를 끕니다. 원하는 위의 이름에 해당 하는 자체 클래스가 이러한 뷰 컨트롤러의 각 따라서 아래 **Identity > 클래스**, 아래 스크린샷에 표시 된 것과 같이 해당 이름을 입력 합니다.

[![](creating-tabbed-applications-images/class-name.png "Set the Class to ViewController1")](creating-tabbed-applications-images/class-name.png#lightbox)

Mac 용 visual Studio는 클래스 및 필요한 디자이너 파일에 자동으로 생성 됩니다, 그리고 아래 그림과 같이 Solution Pad에서 볼 수이 있습니다.

[![](creating-tabbed-applications-images/solution-pad2.png "Auto-generated files in the project")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

#### <a name="creating-the-ui"></a>UI 만들기

다음으로 만들겠습니다 간단한 사용자 인터페이스를 각 ViewController의 보기에 대 한 Xamarin iOS 디자이너를 사용 하 여 합니다.

끌어 하고자를 `Label` 와 `Button` 에서 ViewController1에는 **도구 상자** 오른쪽에서. 다음 사용 하 여 Properties Pad 이름과 다음 컨트롤의 텍스트를 편집 합니다.

- **레이블을** : `Text`  =  **하나**
- **단추** : `Title`  =  **일부 초기 작업을 수행 하는 사용자**

이 단추의 표시 여부를 제어할 수는 것을 `TouchUpInside` 이벤트를 코드 숨김에 참조 하는 데 필요 합니다. 보겠습니다 식별 하는 **이름을** `aButton` 다음 스크린샷에 표시 된 것 처럼 Properties Pad에서:

[![](creating-tabbed-applications-images/abutton-properties.png "Set the Name to aButton in the Properties Pad")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

이제 아래 스크린샷과 비슷한 디자인 화면에 표시 됩니다.

[![](creating-tabbed-applications-images/design-surface1.png "Your Design Surface should now look similar to this screenshot")](creating-tabbed-applications-images/design-surface1.png#lightbox)

좀 더 자세히 추가 해 보겠습니다 `ViewController2` 고 `ViewController3`, 각각에 레이블 추가, '2' 및 '3'에 각각 텍스트를 변경 합니다. 사용자에 게이 강조 했습니다 보면 탭/보기.

#### <a name="wiring-up-the-button"></a>단추를 연결 합니다.

로드 하겠습니다 `ViewController1` 응용 프로그램을 처음으로 시작 합니다. 사용자가 단추를 누를 때 단추 숨기기 하 고 로드 드리겠습니다를 `UITabBarController` 사용 하 여는 `ViewController1` 첫 번째 탭에서 인스턴스.

사용자를 놓을 때의 `aButton`, 트리거될 TouchUpInside 이벤트를 원하는 합니다. 단추를 선택 및 합니다 **이벤트 탭** Properties pad의 이벤트 처리기 – 선언 `InitialActionCompleted` – 코드에서를 참조할 수 있도록 합니다. 이 옵션은 아래 스크린샷에 나와 있습니다.

[![](creating-tabbed-applications-images/event-handler.png "When the user releases the aButton, trigger a TouchUpInside event")](creating-tabbed-applications-images/event-handler.png#lightbox)

이제는 단추를 숨깁니다 이벤트가 발생 하면 뷰 컨트롤러를 지시할 필요가 `InitialActionCompleted`합니다. `ViewController1`, 다음 부분 메서드를 추가 합니다.

```csharp
partial void InitialActionCompleted (UIButton sender)
{
    aButton.Hidden = true;  
}
```

파일을 저장 하 고 응용 프로그램을 실행 합니다. 에서는 화면 표시 하나 및 사진에서 사라집니다 단추 표시 됩니다.

#### <a name="adding-the-tab-bar-controller"></a>탭 표시줄 컨트롤러 추가

이제 예상 대로 작동 하는 초기 보기. 에 추가 하고자 하는 다음으로 `UITabBarController`뷰 2 및 3와 함께 합니다. 스토리 보드 디자이너에서 열어 보겠습니다.

에 **도구 상자**, 검색를 **탭 표시줄 컨트롤러** 컨트롤러 개체에 따라이 디자인 화면으로 끕니다. 탭 표시줄 컨트롤러는 아래 스크린샷을 보시 ui 따라서 기본적으로이 사용 하 여 두 뷰 컨트롤러를 가져옵니다.

[![](creating-tabbed-applications-images/tabbarcontroller.png "Adding a Tab Bar Controller to the layout")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

맨 아래에 있는 검은색 표시줄을 선택 하 고 delete 키를 눌러 하 여 이러한 새 뷰 컨트롤러를 삭제 합니다.

이 스토리 보드를 TabBarController 및 우리의 보기 컨트롤러 간의 전환을 처리 하 고 Segues를 사용할 수 있습니다. 초기 보기와 상호 작용을 후 사용자에 게 TabBarController에 로드 하려고 합니다. 설정 해 보겠습니다이 디자이너에서.

**Ctrl + 클릭** 하 고 **끌어서** 는 TabBarController 단추에서. 마우스 놓기에서 상황에 맞는 메뉴가 표시 됩니다. 모달 segue를 사용 하려고 합니다. 

이 탭의 각 설정 하려면 **Ctrl-클릭** 에서 1 ~ 3 차원 및 선택 관계 순서는 뷰 컨트롤러의 각 TabBarController **탭** 아래 그림과 같이 상황에 맞는 메뉴에서:

[![](creating-tabbed-applications-images/context-menu.png "Select the Tab Relationship")](creating-tabbed-applications-images/context-menu.png#lightbox)

스토리 보드에는 아래 스크린샷과 유사 해야 합니다.

[![](creating-tabbed-applications-images/segue-layout.png "The Storyboard should resemble this screenshot")](creating-tabbed-applications-images/segue-layout.png#lightbox)

탭 표시줄 항목 중 하나를 클릭 하 고 [속성] 패널을 탐색, 아래와 같이 다양 한 다른 옵션을 확인할 수 있습니다.

[![](creating-tabbed-applications-images/properties-panel.png "Setting the tab options in the Properties Explorer")](creating-tabbed-applications-images/properties-panel.png#lightbox)

배지, 제목 및 iOS와 같은 특정 특성을 편집 하는 데 사용할 수 있습니다 [식별자](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html), 특히

저장 하 고 응용 프로그램을 지금 실행을 단추 ViewController1 인스턴스는 TabBarController에 로드 되 면 다시 나타납니다는 소요 됩니다. 현재 보기에 부모 뷰 컨트롤러에 있는지를 확인 하 여이 수정 해 보겠습니다. 이 경우 알게 TabBarController를 내 고 단추를 숨겨야 하므로 합니다. 아래 코드를 ViewController1 클래스를 추가 하겠습니다.

```csharp
public override void ViewDidLoad ()
{
    if (ParentViewController != null){
        aButton.Hidden = true;
    }
}
```

응용 프로그램을 실행 하 고 사용자가 첫 번째 화면에서 UITabBarController 단추를 누를 때 로드 아래와 같이 첫 번째 탭에 배치 하는 첫 번째 화면에서 보기를 사용 하 여:

[샘플 앱 출력 ![](creating-tabbed-applications-images/first-view-sml.png)](creating-tabbed-applications-images/first-view.png#lightbox)

## <a name="summary"></a>요약

이 문서에서는 사용 하는 방법을 설명를 `UITabBarController` 응용 프로그램에서 합니다. 이러한 제목, 이미지 및 배지 탭에서 속성을 설정 하는 방법 뿐만 아니라 각 탭에 컨트롤러를 로드 하는 방법을 살펴보았습니다. 에서는 다음 검사를 스토리 보드를 사용 하 여 로드 하는 방법을 `UITabBarController` 없는 경우 런타임에 `RootViewController` 창의 합니다.

## <a name="related-links"></a>관련 링크

- [탭 응용 프로그램 (샘플) 만들기](https://docs.microsoft.com/samples/xamarin/ios-samples/creatingtabbedapplications)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [UITabBarController 클래스 참조](https://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
