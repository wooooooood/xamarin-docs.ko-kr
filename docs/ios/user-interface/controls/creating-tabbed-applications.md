---
title: 탭 막대와 Xamarin.iOS의 탭 모음 컨트롤러
description: 이 문서에서는 iOS 탭 모음 컨트롤러 및 Xamarin.iOS 그러한 형식을 사용 하는 방법을 설명 합니다. UITabBarController 설정, 이미지와 함께 작동 배지 값, 이벤트와 작업 등을 설정 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: d8b096774e60ec0e0b69e109fa5da53c25e66d25
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789760"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>탭 막대와 Xamarin.iOS의 탭 모음 컨트롤러

탭된 응용 프로그램은 iOS에 임의의 순서로 여러 화면에 액세스할 수 있는 사용자 인터페이스를 지원 하기 위해 사용 됩니다. 통해는 `UITabBarController` 클래스에는 이러한 다중 화면 시나리오에 대 한 지원을 쉽게 포함 될 수 있습니다. `UITabBarController` 각 화면에 대 한 세부 정보에 초점을 응용 프로그램 개발자는 다중 화면 관리를 담당 합니다.

탭된 응용 프로그램을 빌드해야은 일반적으로 `UITabBarController` 되는 `RootViewController` 주 창. 그러나 약간의 추가 코드를 응용 프로그램 탭된 몇 가지 다른 초기 화면에서 응용 프로그램 로그인 화면을 뒤에 탭된 인터페이스를 먼저 제공 시나리오와 같은 연속으로 데도 수 있습니다.

여기에 탭을 사용 하 여 간단한 응용 프로그램의 연습을 통해 작업 하 여 검토 합니다. 그런 다음 살펴보겠습니다 작업 이외에 탭으로 구분 하는 방법을 `RootViewController` 시나리오입니다.

## <a name="introducing-uitabbarcontroller"></a>UITabBarController 소개

`UITabBarController` 지원 다음 응용 프로그램 개발을 탭 합니다.

-  여러 명의 컨트롤러를 추가할 수 있습니다.
-  탭된 사용자 인터페이스를 통해 제공 하는 `UITabBar` 사용자 컨트롤러와 해당 뷰 간에 전환할 수 있도록 클래스입니다. 


컨트롤러에 추가 되는 `UITabBarController` 를 통해 해당 `ViewControllers` 속성, 즉는 `UIViewController` 배열입니다. `UITabBarController` 적절 한 컨트롤러를 로드 하 고 선택 된 탭에 따라 해당 뷰를 제시 자체에서 처리 합니다.

탭의 인스턴스인는 `UITabBarItem` 클래스에 포함 되어 있는 한 `UITabBar` 인스턴스. 각 `UITabBar` 인스턴스는를 통해 액세스할 수는 `TabBarItem` 각 탭에 있는 컨트롤러의 속성입니다.

사용 하는 방법 이해 하는 `UITabBarController`, 하나를 사용 하는 간단한 응용 프로그램을 작성 하는 과정을 살펴보겠습니다.

## <a name="tabbed-application-walkthrough"></a>응용 프로그램 탭된 연습

이 연습에 대 한 다음 응용 프로그램을 만드는 하겠습니다.

[![](creating-tabbed-applications-images/00-app.png "샘플 탭된 응용 프로그램")](creating-tabbed-applications-images/00-app.png#lightbox)

를 없지만 이미 탭된 응용 프로그램 템플릿을이 예에서는, Mac 용 Visual Studio에서 사용할 수 있는 응용 프로그램 생성 방법의 더 잘 이해할 수를 빈 프로젝트에서 작동 하도록 하겠습니다.

 <a name="Creating_the_Application" />


### <a name="creating-the-application"></a>응용 프로그램 작성

새 응용 프로그램을 만들어 보겠습니다.

선택 된 **파일 > 새로 만들기 > 솔루션** Mac 및 선택 용 Visual Studio에서 메뉴 항목은 **iOS > 앱 > 빈 프로젝트** 서식 파일을 프로젝트 이름 `TabbedApplication`아래와 같이,:

[![](creating-tabbed-applications-images/newsolution1.png "빈 프로젝트 템플릿 선택")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "TabbedApplication 프로젝트 이름을")](creating-tabbed-applications-images/newsolution2.png#lightbox)



### <a name="adding-the-uitabbarcontroller"></a>UITabBarController 추가

다음으로 빈 클래스를 선택 하 여 추가 **파일 > 새 파일** 선택 하 고는 **일반: 빈 클래스** 템플릿. 파일 이름을 `TabController` 아래와 같이:

[![](creating-tabbed-applications-images/02-newclass.png "TabController 클래스 추가")](creating-tabbed-applications-images/02-newclass.png#lightbox)

`TabController` 클래스의 구현에 포함 됩니다는 `UITabBarController` 배열의 관리할 `UIViewControllers`합니다. 사용자가 탭을 선택 된 `UITabBarController` 하므로 적절 한 보기 컨트롤러에 대 한 보기를 제공 합니다.

구현 하는 `UITabBarController` 에서 다음을 수행 해야 합니다.

1.  기본 클래스 설정 `TabController` 를 `UITabBarController` 합니다. 
1.  만들 `UIViewController` 에 추가할 인스턴스를는 `TabController` 합니다. 
1.  추가 `UIViewController` 에 할당 된 배열에는 인스턴스는 `ViewControllers` 의 속성은 `TabController` 합니다. 


다음 코드를 추가 하는 `TabController` 다음이 단계를 수행 하는 클래스:

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

각각에 대해 다음에 유의 `UIViewController` 설정 인스턴스를는 `Title` 의 속성은 `UIViewController`합니다. 컨트롤러에 추가 될 때는 `UITabBarController`, `UITabBarController` 읽힙니다는 `Title` 각 컨트롤러에 대해 아래와 같이 연결 된 탭의 레이블에 표시:

[![](creating-tabbed-applications-images/00-app.png "샘플 응용 프로그램 실행")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>TabController는 RootViewController로 설정

에 추가 되는 순서에 해당 하는 컨트롤러가 탭에 배치 된 순서는 `ViewControllers` 배열입니다.

가져오려는 `UITabController` 창의 확인 하도록 설정 해야 첫 번째 화면으로 로드, `RootViewController`에 다음 코드에 나온 것 처럼는 `AppDelegate`:

```csharp
[Register ("AppDelegate")]
        public partial class AppDelegate : UIApplicationDelegate
        {
                UIWindow window;
                TabController tabController;

                public override bool FinishedLaunching (UIApplication app, NSDictionary options)
                {
                        window = new UIWindow (UIScreen.MainScreen.Bounds);

                        var tabController = new TabController ();
                        window.RootViewController = tabController;

                        window.MakeKeyAndVisible ();
            
                        return true;
                }
        }
```

응용 프로그램을 지금 실행 하는 경우는 `UITabBarController` 첫 번째 탭이 기본적으로 선택 된 로드 됩니다. 다른 탭을 선택 하면 연결된 된 컨트롤러에서 결과 보기에서 제공 되는 `UITabBarController,` 최종 사용자가 두 번째 탭을 선택 하는 위치 아래와 같이:

[![](creating-tabbed-applications-images/03-secondtab.png "표시 된 두 번째 탭")](creating-tabbed-applications-images/03-secondtab.png#lightbox)

 <a name="Modifying_TabBarItems" />


### <a name="modifying-tabbaritems"></a>TabBarItems 수정

실행 중인 응용 프로그램 탭에서 수정 해 보겠습니다 았는 `TabBarItem` 표시 되는 텍스트와 이미지를 변경 하려면 뿐만 아니라 배지는 탭 중 하나에 추가할 합니다.

 <a name="Setting_a_System_Item" />


#### <a name="setting-a-system-item"></a>시스템 항목을 설정합니다.

첫째, 시스템 항목을 사용 하는 첫 번째 탭 설정 하겠습니다. 생성자에는 `TabController`, 컨트롤러의 설정 하는 줄을 제거 `Title` 에 대 한는 `tab1` 인스턴스 및 다음 코드를 컨트롤러의 설정으로 대체 `TabBarItem` 속성:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

만들 때의 `UITabBarItem` 를 사용 하는 `UITabBarSystemItem`, 제목 및 이미지는 자동으로 제공 iOS에 의해 표시 된 아래 스크린샷에 표시 된 대로 **즐겨찾기** 아이콘 및 제목을 첫 번째 탭에서:

 ![](creating-tabbed-applications-images/04a-tabimage.png "별표 아이콘을 사용 하 여 첫 번째 탭")

 <a name="Setting_the_Title_and_Image" />


#### <a name="setting-the-title-and-image"></a>제목 및 이미지 설정

시스템 항목을, 제목 및의 이미지를 사용 하 여 한 `UITabBarItem` 사용자 지정 값으로 설정할 수 있습니다. 예를 들어 설정 하는 코드를 변경는 `TabBarItem` 라는 컨트롤러의 속성 `tab2` 다음과 같습니다.

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

위의 코드에서는 명명 된 이미지 `second.png` Mac.에 대 한 Visual Studio에서 프로젝트의 루트에 추가 된 실제로 세 개의 이미지 아래와 같이 장치 해상도 모두 포함 하는 프로젝트에 추가 되었습니다.

 [![](creating-tabbed-applications-images/tabbedimages7new.png "프로젝트에 추가 이미지")](creating-tabbed-applications-images/tabbedimages7new.png#lightbox)

탭 이미지에는 일반적인 확인 고해상도 대 한 60 및 iPhone 6에 대 한 90 x 90 60에 대 한 투명도 있는 30 x 30 png 여야 합니다 Plus 확인 합니다. 이 코드만 하면 명명 된 파일을 로드 `second.png` iOS 레 티 나 디스플레이 사용 하 여 장치에서 높은 해상도 자동으로 로드 됩니다. 자세한 내용은이에 대 한는 [이미지 작업](~/ios/app-fundamentals/images-icons/index.md) 가이드 합니다. 기본적으로 탭 모음 항목이 선택 파란색 tint으로 회색, 됩니다.

**참고:**

위의 이미지를 추가할 수도 있습니다는 **리소스** 디렉터리를 자동으로 해당 내용이 응용 프로그램 번들의 루트에 복사 될 특수 디렉터리:

[![](creating-tabbed-applications-images/tabbedapplication8.png "리소스 그룹으로 이미지")](creating-tabbed-applications-images/tabbedapplication8.png#lightbox)

또한에서는 설정 된 경우는 `Title` 속성을 직접는 `TabBarItem`, 설정에 대 한 모든 값을 재정의 `Title` 컨트롤러 자체에 있습니다.

응용 프로그램을 지금를 실행 하면 두 번째 탭이 표시 사용자 지정 우리의 제목과 이미지 아래와 같이:

[![](creating-tabbed-applications-images/05-customtab.png "사각형 아이콘을 사용 하 여 두 번째 탭")](creating-tabbed-applications-images/05-customtab.png#lightbox)

 <a name="Setting_the_Badge_Value" />


#### <a name="setting-the-badge-value"></a>배지 값 설정

탭 배지를 표시할 수도 있습니다. 예를 들어, 세 번째 탭에서 배지를 설정 하는 코드의 다음 줄을 추가 합니다.

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

아래와 같이 탭의 왼쪽된 위 모퉁이에 있는 문자열 "Hi"를 사용 하 여 빨간색 레이블을에서는이 명령을 실행 합니다.

[![](creating-tabbed-applications-images/06-badge.png "Hi 배지 사용 하 여 두 번째 탭")](creating-tabbed-applications-images/06-badge.png#lightbox)

배지 숫자 읽지 않은 나타내는 값을 표시 하는 대개 새 항목입니다. 배지를 제거 하려면 설정는 `BadgeValue` 아래와 같이 null로:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />


## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>비 RootViewController 시나리오에서 탭

위의 예에서 사용 하는 방법을 살펴보았습니다는 `UITabBarController` 되었을 때의 `RootViewController` 창. 이 예제를 사용 하는 방법을 검토 합니다는 `UITabBarController` 때는 `RootViewController` 및 스토리 보드를 사용 하 여이 만드는 방법을 보여 줍니다.

 <a name="Initial_Screen_Example" />


### <a name="initial-screen-example"></a>초기 화면 예제

이 시나리오에 대 한 초기 화면 없는 컨트롤러에서 로드 한 `UITabBarController`합니다. 단추를 눌러 사용자는 화면과 상호 상호 작용 하는 경우 동일한 보기 컨트롤러에 로드 되는 `UITabBarController`, 사용자에 게 제공 되는 합니다. 다음 스크린 샷은 응용 프로그램 흐름을 보여 줍니다.

[![](creating-tabbed-applications-images/inital-screen-application.png "이 스크린 샷 응용 프로그램 흐름을 보여 줍니다.")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

이 예제에 대 한 새 응용 프로그램을 시작 하겠습니다. 에서는 다시는 **iPhone > 앱 > 빈 프로젝트 (C#)** 서식 파일, 프로젝트 이름 지정이 이번 `InitialScreenDemo`합니다.


이 예에서 컨트롤러는 보기를 저장 하는 스토리 보드가 필요 합니다. 추가 하려면 스토리 보드:

- 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 파일**합니다.

- 로 이동 하는 새 파일 대화 상자가 나타나면 **iOS > iPhone 스토리 보드를 빈**합니다.

이 새 스토리 보드 부르겠습니다 **MainStoryboard** 아래 그림과 같이,: 

[![](creating-tabbed-applications-images/new-file-dialog.png "MainStoryboard 파일을 프로젝트에 추가")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

다루는 이전에 스토리 보드 아닌 파일 스토리 보드를 추가할 때 알아야 할 몇 가지 중요 한 단계는 [스토리 보드에는 소개](~/ios/user-interface/storyboards/index.md) 가이드입니다. 이러한 항목은 다음과 같습니다.

 
1. 에 사용자 스토리 보드의 이름을 추가 **주 인터페이스** 의 섹션은 `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "주 인터페이스 MainStoryboard로 설정")](creating-tabbed-applications-images/project-options.png#lightbox)
1. 사용자 `App Delegate`를 다음 코드로 창 메서드를 재정의:

    ```csharp
    public override UIWindow Window {
      get;
      set;
    }
    ```

이 예제에 대 한 세 명의 보기 컨트롤러 필요를 하겠습니다. 이름이 각각 `ViewController1`, 초기 보기 Controller로 하 고 첫 번째 탭에서 사용 됩니다. 명명 된, 다른 두 `ViewController2` 및 `ViewController3`, 두 번째 및 세 번째 탭에서 각각 사용 될 합니다.

컨트롤러 디자인 화면에 세 명의 보기를 끌어서 두 번 MainStoryboard.storyboard 파일을 클릭 하 여 디자이너를 엽니다. 원하는 각 위의 이름에 해당 하는 자체 클래스 있는 컨트롤러 이러한 보기에서 그러한 **Identity > 클래스**, 아래 스크린샷에 표시 된 것과 같이 이름에 입력:

[![](creating-tabbed-applications-images/class-name.png "클래스 ViewController1로 설정")](creating-tabbed-applications-images/class-name.png#lightbox)

Mac 용 visual Studio는 클래스와 필요한 디자이너 파일을 자동으로 생성 됩니다, 그리고 아래 그림과 같이 솔루션 패드에서 확인할 수 있습니다.

[![](creating-tabbed-applications-images/solution-pad2.png "프로젝트에서 자동으로 생성 된 파일")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

 <a name="Creating_the_UI" />


#### <a name="creating-the-ui"></a>이제 UI 작성

다음으로, 만들겠습니다 간단한 사용자 인터페이스 각 ViewController의 보기에 대 한 Xamarin iOS 디자이너를 사용 하 여 합니다.

끌어 원하는 `Label` 및 `Button` 에서 ViewController1에는 **도구 상자** 오른쪽에 있습니다. 다음 속성 패드 이름과 다음과 컨트롤의 텍스트 편집을 사용 해야 합니다.

-  **레이블** : `Text`  =  **하나**
-  **단추** : `Title`  =  **사용자가 초기 동작을 수행 합니다.**


이 단추의 표시 여부를 제어 우리는 `TouchUpInside` 이벤트 않으며이 뒤에 있는 코드에서 참조 하는 데 필요 합니다. 보겠습니다로 식별 된 **이름** `aButton` 다음 스크린샷에 표시 된 것 처럼 속성 패드에서:

[![](creating-tabbed-applications-images/abutton-properties.png "aButton 속성 패드에서에 이름을 설정합니다")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

이제 디자인 화면 아래 스크린샷과 유사 하 게 표시 됩니다.

[![](creating-tabbed-applications-images/design-surface1.png "디자인 화면 해야 이제 유사 하 게이 스크린 샷")](creating-tabbed-applications-images/design-surface1.png#lightbox)

조금 더 자세히를 추가 하겠습니다. `ViewController2` 및 `ViewController3`을 각각, 레이블을 추가 하거나 '2' 및 '3'에 각각 텍스트를 변경 합니다. 사용자에 게이 하이라이트 살펴보고 탭/보기.

#### <a name="wiring-up-the-button"></a>에 연결 하는 단추

로드 하 여 `ViewController1` 응용 프로그램을 처음으로 시작 합니다. 단추를 숨길 알아보고 로드 하겠습니다 사용자가 단추를 누르면는 `UITabBarController` 와 `ViewController1` 첫 번째 탭에서 인스턴스.

놓으면는 `aButton`, TouchUpInside 이벤트를 트리거하려면 주시기 바랍니다. 이제 단추를 선택 및는 **이벤트 탭** 속성 패드의 이벤트 처리기 – 선언 `InitialActionCompleted` – 되므로 코드에서를 참조할 수 있습니다. 아래 스크린샷에서이 보여 줍니다.

[![](creating-tabbed-applications-images/event-handler.png "사용자는은 aButton 놓으면 TouchUpInside 이벤트 트리거")](creating-tabbed-applications-images/event-handler.png#lightbox)

이제 단추를 숨기려면이 이벤트가 발생 하는 경우 뷰 컨트롤러를 지시 해야 `InitialActionCompleted`합니다. `ViewController1`, 다음 부분 메서드를 추가 합니다.

```csharp
partial void InitialActionCompleted (UIButton sender)
    {
      aButton.Hidden = true;  
    }
```

파일을 저장 하 고 응용 프로그램을 실행 합니다. म 하나가 표시 되는 화면 및 사진에 사라질 단추 표시 됩니다.

#### <a name="adding-the-tab-bar-controller"></a>탭 표시줄 추가 컨트롤러

예상 대로 작동 하는 초기 보기를 했습니다. 에 추가 하고자 하는 다음으로 `UITabBarController`뷰 2와 3 단계와 함께 합니다. 디자이너에서 스토리 보드를 열고 보겠습니다.

**도구 상자**, 검색할는 **탭 모음 컨트롤러** 컨트롤러 & 개체에서이 디자인 화면으로 끕니다. 아래 스크린샷에서 보시 탭 모음 컨트롤러는 ui와 기본적으로 된 두 명의 뷰 컨트롤러의 따라서:

[![](creating-tabbed-applications-images/tabbarcontroller.png "레이아웃 탭 모음 컨트롤러 추가")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

아래쪽의 검은색 표시줄을 선택 하 고 delete 키를 눌러 하 여 새 컨트롤러 이러한 보기를 삭제 합니다.

이 스토리 보드는 TabBarController 및 우리의 보기 컨트롤러 간의 전환을 처리 하도록 Segues를 사용할 수 있습니다. 초기 보기와의 상호 작용, 후 사용자에 게 TabBarController에 로드 하려고 합니다. 설정이 디자이너에서 합니다.

**Ctrl + 클릭** 및 **끌어서** 단추는 TabBarController에서 합니다. 마우스 놓기 상황에 맞는 메뉴가 나타납니다. 모달 segue 사용 하려고 합니다. 
 
이 탭의 각를 설정 하려면 **Ctrl + 클릭** 1 ~ 3 차원 및 관계 선택 순서로 컨트롤러 우리의 보기의 각 TabBarController에서 **탭** 아래 그림과 같이 상황에 맞는 메뉴에서:

[![](creating-tabbed-applications-images/context-menu.png "탭 관계를 선택 합니다.")](creating-tabbed-applications-images/context-menu.png#lightbox)

스토리 보드에는 아래 스크린샷과 유사 합니다.

[![](creating-tabbed-applications-images/segue-layout.png "스토리 보드가 스크린이 샷을 비슷해야 합니다.")](creating-tabbed-applications-images/segue-layout.png#lightbox)

탭 모음 항목 중 하나를 클릭 하 고 탐색 속성 패널을 아래 그림과 같이 다양 한 다른 옵션을 확인할 수 있습니다.

[![](creating-tabbed-applications-images/properties-panel.png "속성 탐색기 탭 옵션 설정")](creating-tabbed-applications-images/properties-panel.png#lightbox)

IOS 배지, 제목 등 특정 속성을 편집 하는 데 사용할 수 있습니다 [식별자](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html), 기타

저장 하 고 응용 프로그램을 지금 실행, 단추 ViewController1 인스턴스는 TabBarController에 로드 되 면 다시 나타나면 있는지을 찾을 수 있습니다. 현재 보기에 부모 뷰-컨트롤러 있는지를 확인 하 여 문제를 해결 해 보겠습니다. 그렇지 않으면 회원님의 단추 숨김을 따라서 및는 TabBarController 내 합니다. ViewController1 클래스에 아래 코드를 추가 해 보겠습니다.

```csharp
public override void ViewDidLoad ()
{
     if (ParentViewController != null){
       aButton.Hidden = true;
     }

}
```

응용 프로그램을 실행 하 고 사용자가 첫 번째 화면의 UITabBarController 단추를 누를 때 로드 되는 아래와 같이 첫 번째 탭에 배치 하는 첫 번째 화면에서 보기:

[![](creating-tabbed-applications-images/first-view.png "예제 응용 프로그램 출력")](creating-tabbed-applications-images/first-view.png#lightbox)

<!--Save the files and run the application:

[![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)-->

## <a name="summary"></a>요약

이 문서에 사용 하는 방법을 검사 한 `UITabBarController` 응용 프로그램에서 합니다. 이러한 제목, 이미지 및 배지 탭에서 속성을 설정 하는 방법 뿐만 아니라 각 탭에 컨트롤러를 로드 하는 방법을 살펴보았습니다. 에서는 다음 검사, 스토리 보드를 사용 하 여 로드 하는 방법을 `UITabBarController` 하지 않을 때 런타임 시는 `RootViewController` 창.


## <a name="related-links"></a>관련 링크

- [탭 응용 프로그램 (샘플) 만들기](https://developer.xamarin.com/samples/monotouch/CreatingTabbedApplications/)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [UITabBarController 클래스 참조](http://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
