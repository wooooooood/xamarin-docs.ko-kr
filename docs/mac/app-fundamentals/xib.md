---
title: xamarin.mac에서.xib 파일
description: 이 문서를 만들어 Xamarin.Mac 응용 프로그램에 대 한 사용자 인터페이스를 유지 관리 하는 Xcode의 Interface Builder에서 만든.xib 파일을 사용 하 여 작업을 다룹니다.
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 45eeee745b133646aef0f775bc879fa6a5d867c7
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109434"
---
# <a name="xib-files-in-xamarinmac"></a>xamarin.mac에서.xib 파일

_이 문서를 만들어 Xamarin.Mac 응용 프로그램에 대 한 사용자 인터페이스를 유지 관리 하는 Xcode의 Interface Builder에서 만든.xib 파일을 사용 하 여 작업을 다룹니다._

> [!NOTE]
> Xamarin.Mac 앱에 대 한 사용자 인터페이스를 만드는 것이 더 좋습니다 스토리 보드를 사용 하 여 합니다. 이 설명서 역사적인 이유 및 이전 Xamarin.Mac 프로젝트와 작업에 대 한 위치에 남아 있습니다. 자세한 내용은 참조 하십시오 우리의 [스토리 보드 소개](~/mac/platform/storyboards/index.md) 설명서.

## <a name="overview"></a>개요

동일한 사용자 인터페이스 요소에 대 한 액세스 권한이 Xamarin.Mac 응용 프로그램에서 C# 및.NET으로 작업을 하는 경우 및 작업 하는 개발자 도구를 *Objective-C* 및 *Xcode* 않습니다. Xcode의 Xamarin.Mac이 Xcode와 직접 통합 되므로 사용할 수 있습니다 _Interface Builder_ 만들기 및 사용자 인터페이스를 유지 관리 (또는 필요에 따라 C# 코드에서 바로 작성).

.Xib 파일은 macOS에서 Xcode의 Interface Builder에서 생성 및 유지 관리 되는 응용 프로그램의 사용자 인터페이스 (예: 메뉴, Windows, 뷰, 레이블, 텍스트 필드)의 요소를 그래픽으로 정의할 수 사용 됩니다.

[![실행 중인 앱의 예가](xib-images/intro01.png "실행 중인 앱의 예")](xib-images/intro01-large.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램에서.xib 파일의 기본 사항을 설명 합니다. 것이 가장 좋습니다 진행 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 주요 개념 및이 문서를 사용 하는 기술을 설명 하는 대로 먼저 문서.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 특성 요소 Objective-C 개체 및 UI에 C# 클래스를 연결 하는 데 사용 합니다.


## <a name="introduction-to-xcode-and-interface-builder"></a>Xcode 및 Interface Builder 소개

Xcode의 일부로, Apple에서는 디자이너에서 사용자 인터페이스를 시각적으로 만들 수 있는 Interface Builder 라는 도구를 만들었습니다. Xamarin.Mac 통합 fluently 작성기 인터페이스를 Objective-C 사용자가 수행 하는 같은 도구로 UI를 만들 수 있습니다.


### <a name="components-of-xcode"></a>Xcode 구성 요소

Mac 용 Visual Studio에서 xcode에서.xib 파일을 열을 사용 하 여 열립니다는 **Project Navigator** 왼쪽에 **인터페이스 계층 구조** 하 고 **인터페이스 편집기** 중간에 및 **속성 및 유틸리티** 오른쪽의 섹션:

[![Xcode UI의 구성 요소](xib-images/xcode03.png "Xcode UI의 구성 요소")](xib-images/xcode03-large.png#lightbox)

살펴보겠습니다 이러한 각 기능에서 Xcode 섹션지 않습니다 및 사용법에 이러한 인터페이스를 만드는 Xamarin.Mac 응용 프로그램에 대 한 합니다.


#### <a name="project-navigation"></a>프로젝트 탐색

Xcode에서 편집을 위해.xib 파일을 열면 Mac 용 Visual Studio는 Xcode와 변경 내용을 전달 하기 위해 백그라운드에서 Xcode 프로젝트 파일을 만듭니다. 나중에 전환할 때는 Visual Studio로 다시 Mac 용 Xcode에서 변경한 내용은이 프로젝트에는 동기화 Xamarin.Mac 프로젝트와 Visual Studio에서 mac 용

합니다 **프로젝트 탐색** 섹션을 사용 하면 모든이 구성 하는 파일 사이 이동할 수 있습니다 _shim_ Xcode 프로젝트입니다. 일반적으로 수와 같은이 목록의.xib 파일 관심이 **MainMenu.xib** 하 고 **MainWindow.xib**합니다.


#### <a name="interface-hierarchy"></a>인터페이스 계층 구조

합니다 **인터페이스 계층 구조** 섹션을 사용 하면 같은 사용자 인터페이스의 몇 가지 주요 속성에 쉽게 액세스할 수 있습니다 해당 **자리 표시자** 및 주 **창**합니다. 또한를 중첩 하는 계층 내에서 끌어서 서 사용자 인터페이스 및 조정 하는 개별 요소 (뷰)에 액세스 하려면이 섹션에서는 사용할 수 있습니다.


#### <a name="interface-editor"></a>인터페이스 편집기

합니다 **인터페이스 편집기** 섹션에서는는 화면을 제공 하면 그래픽 레이아웃 사용자 인터페이스입니다. 요소를 끌면 됩니다는 **라이브러리** 섹션을 **속성 및 유틸리티** 디자인을 만들려면 섹션입니다. 디자인 화면에 사용자 인터페이스 요소 (보기)를 추가 하면 이러한에 추가 됩니다는 **인터페이스 계층 구조** 섹션에 표시 되는 순서는 **인터페이스 편집기**합니다.


#### <a name="properties--utilities"></a>속성 및 유틸리티

합니다 **속성 및 유틸리티** 섹션을 사용 하 여 사용할 두 개의 주요 섹션으로 나뉩니다 **속성** (검사기 라고도 함) 및 **라이브러리**:

![속성 검사자](xib-images/xcode04.png "속성 관리자")

하지만 처음에이 섹션은 거의 비어의 요소를 선택 하는 경우는 **인터페이스 편집기** 또는 **인터페이스 계층 구조**의 **속성** 단원 채워집니다. 특정된 요소 및 조정할 수 있는 속성에 대 한 정보입니다.

**속성** 섹션 내에는 다음 그림처럼 8개의 *검사기 탭*이 있습니다.

[![모든 검사기 개요](xib-images/xcode05.png "모든 검사기 개요")](xib-images/xcode05-large.png#lightbox)

왼쪽부터 시작해서 오른쪽으로 가면서 다음과 같은 탭이 있습니다.

-   **파일 검사기** – 파일 검사기는 파일 이름, 편집 중인 Xib 파일의 위치와 같은 파일 정보를 보여줍니다.
-   **빠른 도움말** – 빠른 도움말 탭에서는 현재 Xcode에서 선택한 항목에 따라 상황에 맞는 도움말을 제공합니다.
-   **ID 검사기** – ID 검사기는 선택한 컨트롤/보기에 대한 정보를 제공합니다.
-   **특성 검사기** – 특성 검사기를 사용 하면 선택한 컨트롤/보기의 다양 한 특성을 사용자 지정할 수 있습니다.
-   **크기 검사기** – 크기 검사기를 사용 하면 크기 및 크기 조정 선택한 컨트롤/보기의 동작을 제어할 수 있습니다.
-   **연결 검사기** – 연결 검사기는 선택한 컨트롤의 출 선 및 작업 연결을 보여 줍니다. 출 선 및 작업을 잠시 후에 검사 합니다.
-   **바인딩 검사기** -바인딩 검사기를 사용 하면 해당 값 데이터 모델에 자동으로 바인딩된 컨트롤을 구성할 수 있습니다.
-   **보기 효과 검사기** – 보기 효과 검사기를 사용 하면 애니메이션 등의 컨트롤에 미치는 영향을 지정할 수 있습니다.

에 **라이브러리** 섹션에서는 컨트롤을 찾을 수 있습니다 및 그래픽 디자이너를 배치 하는 개체에 사용자 인터페이스를 작성 합니다.

![라이브러리 검사기 예가](xib-images/xcode06.png "라이브러리 검사기의 예")

이제 Xcode IDE 및 Interface Builder를 사용 하 여 익숙한 살펴보겠습니다 사용자 인터페이스를 만드는 데 사용 합니다.


## <a name="creating-and-maintaining-windows-in-xcode"></a>만들기 및 Xcode에서 windows 유지 관리

스토리 보드를 사용 하 여 Xamarin.Mac 앱의 사용자 인터페이스를 만들기 위한 기본 메서드는 (하세요 우리의 [스토리 보드 소개](~/mac/platform/storyboards/index.md) 자세한 설명서) 고, 결과적으로 모든 새 프로젝트에서에서 시작 된 Xamarin.Mac은 기본적으로 스토리 보드를 사용 합니다.

기반 UI를.xib을 사용 하 여 전환 하려면 다음을 수행 하십시오.

1. Mac 용 Visual Studio를 열고 새 Xamarin.Mac 프로젝트를 시작 합니다.
2. 에 **Solution Pad**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...**
3. 선택 **Mac** > **Windows 컨트롤러**:

    ![새 창 컨트롤러 추가](xib-images/setup00.png "새 창 컨트롤러 추가")
4. 입력 `MainWindow` 이름과 클릭 합니다 **새로 만들기** 단추:

    ![새 주 창을 추가](xib-images/setup01.png "새 주 창 추가")
5. 프로젝트에서 다시 마우스 오른쪽 단추로 누르고 **추가** > **새 파일...**
6. 선택 **Mac** > **주 메뉴**:

    ![새 주 메뉴 추가](xib-images/setup02.png "Main에 새 메뉴 추가")
7. 이름으로 그대로 둡니다 `MainMenu` 을 클릭 합니다 **새로 만들기** 단추입니다.
8. 에 **Solution Pad** 를 선택 합니다 **Main.storyboard** 파일, 마우스 오른쪽 단추로 **제거**:

    ![주 스토리 보드를 선택 하면](xib-images/setup03.png "주 스토리 보드를 선택 합니다.")
9. 제거 대화 상자를 클릭 합니다 **삭제** 단추:

    ![삭제 확인](xib-images/setup04.png "삭제 확인")
10. 에 **Solution Pad**를 두 번 클릭 합니다 **Info.plist** 파일을 편집용으로 엽니다.
11. 선택 `MainMenu` 에서 합니다 **주 인터페이스** 드롭다운:

    [![주 메뉴를 설정](xib-images/setup05.png "주 메뉴를 설정 합니다.")](xib-images/setup05-large.png#lightbox)
12. 에 **Solution Pad**, 두 번 클릭 합니다 **MainMenu.xib** Xcode의 Interface Builder에서 편집 하기 위해 열려는 파일.
13. 에 **라이브러리 검사기**, 형식 `object` 검색 필드에 다음 끌어 새 **개체** 디자인 화면으로:

    [![주 메뉴 편집](xib-images/setup06.png "주 메뉴 편집")](xib-images/setup06-large.png#lightbox)
14. 에 **검사기**를 입력 `AppDelegate` 에 대 한 합니다 **클래스**:

    [![앱 대리자를 선택 하면](xib-images/setup07.png "앱 대리자를 선택 합니다.")](xib-images/setup07-large.png#lightbox)
15. 선택 **파일의 소유자** 에서 합니다 **인터페이스 계층 구조**, 전환할 합니다 **연결 검사기** 대리자에서 줄을 끌어서를 `AppDelegate` **개체** 프로젝트에 추가 합니다.

    [![연결 앱 대리자](xib-images/setup08.png "앱 대리자 연결")](xib-images/setup08-large.png#lightbox)
16. 변경 내용을 저장 하 고 mac 용 Visual Studio로 돌아가서

현재 위치에서 이러한 모든 변화를 통해 편집 합니다 **AppDelegate.cs** 파일과 다음과 비슷하게 표시:

```csharp
using AppKit;
using Foundation;

namespace MacXib
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public MainWindowController mainWindowController { get; set; }

        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
            mainWindowController = new MainWindowController ();
            mainWindowController.Window.MakeKeyAndOrderFront (this);
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

앱의 주 창에 정의 되어 이제는 **.xib** 창 컨트롤러를 추가 하는 경우 프로젝트에 자동으로 포함 파일입니다. windows 디자인에 맞게 편집 하는 **Solution Pad**를 두 번 클릭 합니다 **MainWindow.xib** 파일:

![MainWindow.xib 파일 선택](xib-images/edit01.png "MainWindow.xib 파일 선택")

Xcode의 Interface Builder에서 창 디자인을 엽니다.

[![편집 된 MainWindow.xib](xib-images/edit02.png "는 MainWindow.xib 편집")](xib-images/edit02-large.png#lightbox)


### <a name="standard-window-workflow"></a>표준 창 워크플로

만들고 Xamarin.Mac 응용 프로그램에서 사용 하는 모든 창에 대 한 프로세스는 기본적으로 동일 합니다.

1. 새 windows의 기본 프로젝트에 자동으로 추가 되지 않은 경우 새 창 정의 프로젝트에 추가 합니다.
2. Xcode의 Interface Builder에서 편집용으로 창 디자인을 열려는.xib 파일을 두 번 클릭 합니다.
3. 필수 창의 속성을 설정할 합니다 **특성 검사기** 하며 **크기 검사기**합니다.
4. 인터페이스를 만들고 구성 하는 데 필요한 컨트롤에 끌어 합니다 **특성 검사기**합니다.
5. 사용 하 여 합니다 **크기 검사기** 처리할 UI 요소에 대 한 크기를 조정 합니다.
6. 창의 UI 요소를 노출 C# 출 선 및 작업을 통해 코드입니다.
7. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 다시 전환 합니다.


### <a name="designing-a-window-layout"></a>창 레이아웃을 디자인합니다.

Interface builder에서 사용자 인터페이스 레이아웃에 대 한 프로세스는 기본적으로 추가 하는 모든 요소에 대해 동일 합니다.

1. 원하는 컨트롤을 찾을 합니다 **라이브러리 검사기** 으로 끌어 놓습니다 합니다 **인터페이스 편집기** 에 놓습니다.
2. 모든 필수 창의 속성을 설정 합니다 **특성 검사기**합니다.
3. 사용 하 여 합니다 **크기 검사기** 처리할 UI 요소에 대 한 크기를 조정 합니다.
4. 사용자 지정 클래스를 사용 하는 경우 설정 된 **검사기**합니다.
5. UI 요소를 노출 C# 출 선 및 작업을 통해 코드입니다.
6. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 다시 전환 합니다.

예를 들면 다음과 같습니다.

1. Xcode의 **라이브러리 섹션**에서 **누름 단추**를 끕니다.

    [![라이브러리에서 단추를 선택 하](xib-images/xcode07.png "라이브러리에서 단추를 선택")](xib-images/xcode07-large.png#lightbox)
2. 단추를 놓습니다 합니다 **창을** 에 **인터페이스 편집기**:

    [![창에 단추 추가](xib-images/xcode08.png "창에 단추 추가")](xib-images/xcode08-large.png#lightbox)
3. **특성 검사기**에서 **제목** 속성을 클릭하고 단추 제목을 `Click Me`로 변경합니다.

    ![단추 속성 설정](xib-images/xcode09.png "단추 특성 설정")
4. **라이브러리 섹션**에서 **레이블**을 끕니다.

    [![라이브러리에서 레이블 선택](xib-images/xcode10.png "라이브러리에서 레이블 선택")](xib-images/xcode10-large.png#lightbox)
5. **인터페이스 편집기**에서 단추 옆에 있는 **창**에 레이블을 놓습니다.

    [![창에 레이블 추가](xib-images/xcode11.png "창에 레이블 추가")](xib-images/xcode11-large.png#lightbox)
6. 레이블의 오른쪽 핸들을 잡고 창의 가장자리 근처까지 끕니다.

    [![레이블 크기 조정](xib-images/xcode12.png "레이블 크기 조정")](xib-images/xcode12-large.png#lightbox)
7. 선택한 상태에서 레이블로 합니다 **인터페이스 편집기**, 전환할 합니다 **크기 검사기**:

    ![크기 검사기 선택](xib-images/xcode13.png "크기 검사기를 선택 합니다.")
8. 에 **자동 크기 조정 상자** 클릭 합니다 **Dim Red 대괄호** 오른쪽에 있는 및 **Dim Red 옆쪽 화살표** 센터에서:

    ![자동 크기 조정 속성을 편집](xib-images/xcode14.png "자동 크기 조정 속성 편집")
9. 이렇게 하면 레이블이 확장 되 고 실행 중인 응용 프로그램에서 사용 하는 창에서 크기를 조정 하는 대로 축소 스트레치 될 합니다. **빨간색 대괄호** 위쪽 및 왼쪽을 **자동 크기 조정 상자** 상자를 지정 된 X 및 Y 위치 레이블 지시 합니다.
10. 사용자 인터페이스에 변경 내용을 저장합니다

크기를 조정 하 고 관련 컨트롤을 이동 된을 알아야 했습니다 Interface Builder 기반으로 하는 유용한 스냅 힌트는 하면 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다. 이러한 지침은 Mac 사용자에 대 한 친숙 한 모양과 느낌을 갖게 되는 고품질 응용 프로그램을 만드는 데 도움이 됩니다.

보면 합니다 **인터페이스 계층 구조** 섹션, 레이아웃 및 사용자 인터페이스를 구성 하는 요소는 계층 표시 하는 방법을 확인할 수 있습니다.

![인터페이스 계층 구조에서 요소 선택](xib-images/xcode15.png "인터페이스 계층 구조에서 요소 선택")

여기에서 편집 하거나 필요한 경우에 UI 요소를 다시 정렬 하려면 끌 항목을 선택할 수 있습니다. 예를 들어, UI 요소를 다른 요소가 가리는, 하는 경우 창에서 최상위 항목을 확인 하려면 목록 아래쪽으로 끌 수 있습니다.

Xamarin.Mac 응용 프로그램에서 Windows 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Windows](~/mac/user-interface/window.md) 설명서.


## <a name="exposing-ui-elements-to-c-code"></a>UI 요소를 노출 C# 코드

UI의 요소에서 액세스할 수 있도록 노출 해야 Interface Builder에서 사용자 인터페이스의 모양과 느낌을 레이아웃할 마친 후 C# 코드입니다. 이 위해 사용할 작업 및 출 선을 합니다.


### <a name="setting-a-custom-main-window-controller"></a>사용자 지정 주 창의 컨트롤러 설정

C# 코드에 UI 요소를 노출 하는 출 선 및 작업을 만들 수, Xamarin.Mac 앱 사용자 지정 창 컨트롤러를 사용 해야 합니다.

다음을 수행합니다.

1. Xcode의 Interface Builder에서 앱의 스토리 보드를 엽니다.
2. 선택 된 `NSWindowController` 디자인 화면에서 합니다.
3. 전환할 합니다 **검사기** 살펴보고 입력 `WindowController` 으로 **클래스 이름**:

    [![클래스 이름을 편집](xib-images/windowcontroller01.png "클래스 이름 편집")](xib-images/windowcontroller01-large.png#lightbox)
4. 변경 내용을 저장 하 고 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.
5. A **WindowController.cs** 에서 프로젝트에 추가할 파일을 **Solution Pad** Mac 용 Visual Studio에서:

    ![Mac 용 Visual Studio에서 새 클래스 이름을](xib-images/windowcontroller02.png "Mac 용 Visual Studio에서 새 클래스 이름")
6. Xcode의 Interface Builder 스토리 보드를 다시 엽니다.
7. 합니다 **WindowController.h** 파일 용도로 제공 됩니다.

    [![Xcode에서 일치 하는.h 파일](xib-images/windowcontroller03.png "Xcode에서 일치 하는.h 파일")](xib-images/windowcontroller03-large.png#lightbox)


### <a name="outlets-and-actions"></a>출 선 및 작업

따라서 출 선 및 작업은 무엇입니까? 전통적인 .NET 사용자 인터페이스 프로그래밍에서, 사용자 인터페이스의 컨트롤은 추가될 때 자동으로 속성으로 노출됩니다. Mac에서는 동작이 약간 다릅니다. 보기에 컨트롤을 추가하기만 해서는 코드에 액세스할 수 없습니다. 개발자가 UI 요소를 코드에 명시적으로 노출해야 합니다. 순서에서이 수행, Apple 두 옵션 제공:

-  **출선** – 출선은 속성과 비슷합니다. 속성을 통해 코드에 표시 되는 컨트롤을 출 선에 연결 하는 경우, 이벤트 처리기를 연결 하는 등을 수행할 수 있도록 등의 메서드를 호출 합니다.
-  **작업** – 작업은 WPF의 명령 패턴과 비슷합니다. 예를 들어 컨트롤에서 작업을 수행 하면 단추 클릭, 컨트롤 코드에서 메서드를 자동으로 호출 됩니다. 작업 되므로 강력 하 고 편리 하 게 동일한 작업에 여러 컨트롤을 연결할 수 있습니다.

Xcode에서 출 선 및 작업은 추가 통해 코드에서 직접 *컨트롤을 끌어*합니다. 더 구체적으로 말해서, 출 선 또는 작업을 만들려면를 누른 상태에서 출 선 또는 작업을 추가 하려는 컨트롤 요소 선택 합니다 **제어** 키보드에서 단추 및 제어 하는 코드로 직접 끕니다.

Xamarin.Mac 개발자를 위한 콘센트 또는 동작을 만들려면 원하는 C# 파일에 해당 하는 Objective-C 스텁 파일에 끌어을 의미 합니다. Mac 용 visual Studio 라는 파일을 만들었습니다 **MainWindow.h** Interface Builder를 사용 하기 위해 생성 한 shim Xcode 프로젝트의 일환으로:

[![Xcode에서.h 파일의 예로](xib-images/xcode16.png "Xcode에서.h 파일의 예")](xib-images/xcode16-large.png#lightbox)

이 스텁.h 파일을 미러링 합니다 **MainWindow.designer.cs** 새 Xamarin.Mac 프로젝트에 자동으로 추가 됩니다 `NSWindow` 만들어집니다. 이 파일 Interface Builder에서 변경한 내용을 동기화 하는 데 사용할 이며 여기서 만들겠습니다 출 선 및 작업에 UI 요소에 노출 되는 C# 코드입니다.


#### <a name="adding-an-outlet"></a>출 선 추가

출 선 및 작업은 기본적으로 이해, 살펴보겠습니다 UI 요소를 노출 하려면 아웃렛 만들기에 C# 코드입니다.

다음을 수행합니다.

1. 화면의 오른쪽 맨 위 모서리에 있는 Xcode에서 **이중 원** 단추를 클릭하여 **도우미 편집기**를 엽니다.

    [![단말기 편집기 선택](xib-images/outlet01.png "단말기 편집기를 선택 합니다.")](xib-images/outlet01-large.png#lightbox)
2. Xcode가 분할 보기 모드로 전환되어 한 쪽에는 **인터페이스 편집기**가, 다른 쪽에는 **코드 편집기** 표시됩니다.
3. Xcode에 자동으로 선택 하는 **MainWindowController.m** 파일을 **코드 편집기**, 잘못 된 합니다. 출 선 및 작업 이란 위에 대 한 토론에서 기억할 것 해야 합니다 **MainWindow.h** 선택한 합니다.
4. 맨 위에 있는 **코드 편집기** 클릭 합니다 **자동 링크** 선택 하 고는 **MainWindow.h** 파일:

    [![올바른.h 파일을 선택 하면](xib-images/outlet02.png "올바른.h 파일 선택")](xib-images/outlet02-large.png#lightbox)
5. Xcode가 이제 올바른 파일을 선택했습니다.

    [![올바른 파일을 선택](xib-images/outlet03.png "올바른 파일 선택")](xib-images/outlet03-large.png#lightbox)
6. **마지막 단계는 매우 중요합니다!** 올바른 파일을 선택 목록에 없으면 콘센트를 만들 수 없습니다 및 작업 하거나 잘못 된 클래스에 노출 될 C#!
7. 에 **인터페이스 편집기**를 누른를 **컨트롤** 키보드의 키 및 코드 편집기 위에서 만든 레이블 클릭 끌기 바로 아래는 `@interface MainWindow : NSWindow { }` 코드:

    [![새 콘센트를 만들려면 끌기](xib-images/outlet04.png "끌기 새 콘센트를 만들려면")](xib-images/outlet04-large.png#lightbox)
8. 대화 상자가 표시됩니다. 유지 합니다 **연결** 콘센트를 설정 하 고 입력 `ClickedLabel` 에 대 한 합니다 **이름**:

    [![출 선 속성을 설정](xib-images/outlet05.png "출 선 속성 설정")](xib-images/outlet05-large.png#lightbox)
9. 클릭 합니다 **Connect** 단추 콘센트를 만들려면:

    ![완료 된 출 선](xib-images/outlet06.png "완료 출 선")
10. 파일의 변경 내용을 저장합니다.


#### <a name="adding-an-action"></a>작업 추가

다음으로, UI 요소를 사용 하 여 사용자 상호 작용을 노출 하는 작업 만들기에 대해 살펴보겠습니다에 C# 코드입니다.

다음을 수행합니다.

1. 여전히 있는지는 **도우미 편집기** 하며 **MainWindow.h** 파일을 볼 수는 **코드 편집기**합니다.
2. 에 **인터페이스 편집기**를 누른를 **컨트롤** 키보드의 키 및 코드 편집기 위에서 만든 단추 클릭 끌기 바로 아래는 `@property (assign) IBOutlet NSTextField *ClickedLabel;` 코드:

    [![끌기 작업을 만들려면](xib-images/action01.png "끌기 동작을 만들려면")](xib-images/action01-large.png#lightbox)
3. 변경 된 **연결** 작업 형식:

    [![작업 유형 선택](xib-images/action02.png "작업 유형 선택")](xib-images/action02-large.png#lightbox)
4. **이름**으로 `ClickedButton`을 입력합니다.

    [![동작 구성](xib-images/action03.png "작업 구성")](xib-images/action03-large.png#lightbox)
5. 클릭 합니다 **Connect** 단추 동작을 만들려면:

    ![완료 된 동작](xib-images/action04.png "완료 된 작업")
6. 파일의 변경 내용을 저장합니다.

사용자 인터페이스를 사용 하 여 유선 및에 노출 C# 코드, Mac 용 Visual Studio로 다시 전환 하 고, Xcode 및 Interface Builder에서 변경 내용을 동기화 합니다.


### <a name="writing-the-code"></a>코드 작성

출 선 및 작업을 통해 코드에 노출 하는 UI 요소를 만든 사용자 인터페이스를 사용 하 여 프로그램에 활기를 코드를 쓸 준비가 됩니다. 예를 들어 열을 **MainWindow.cs** 에서 두 번 클릭 하 여 편집을 위해 파일을 **Solution Pad**:

[![MainWindow.cs 파일](xib-images/code01.png "The MainWindow.cs 파일")](xib-images/code01-large.png#lightbox)

다음 코드를 추가 하 고는 `MainWindow` 위에서 만든 샘플 콘센트를 사용 하는 클래스:

```csharp
private int numberOfTimesClicked = 0;
...

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

`NSLabel` 액세스 C# Xcode에서 해당 콘센트를 만든 경우 Xcode에서 할당 하는 직접 이름으로이 예제의 경우 라고 `ClickedLabel`합니다. 액세스할 수 있습니다 메서드 또는 속성 노출된 된 개체의 모든 일반 하는 동일한 방식으로 C# 클래스입니다.

> [!IMPORTANT]
> 사용 해야 `AwakeFromNib`와 같은 다른 메서드 대신 `Initialize`이므로 `AwakeFromNib` 라고 _후_ OS가 로드 및.xib 파일에서 사용자 인터페이스를 인스턴스화입니다. 얻게 하 려 한 경우.xib 파일 전에 레이블 컨트롤에 액세스를 완전히 로드 되어를 `NullReferenceException` 오류 하면 레이블 컨트롤이 아직 만들 수는 있습니다.

다음으로, 다음 partial 클래스에 추가 된 `MainWindow` 클래스:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

이 코드는 Xcode 및 Interface Builder에서 생성 하는 단추를 클릭할 때마다 호출 됩니다 하는 작업에 연결 합니다.

일부 UI 요소가 자동으로 구축한 동작에서 예를 들어, 기본 메뉴 모음에서 항목 등을 **열기...**  메뉴 항목 (`openDocument:`). 에 **Solution Pad**, 두 번 클릭 합니다 **AppDelegate.cs** 편집용으로 엽니다 아래 다음 코드를 추가 하는 파일은 `DidFinishLaunching` 메서드:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

여기 키 줄이 `[Export ("openDocument:")]`, 알려줍니다 `NSMenu` 는 합니다 **AppDelegate** 메서드가 `void OpenDialog (NSObject sender)` 응답 하는 `openDocument:` 작업 합니다.

작업 메뉴에 대 한 자세한 내용은 참조 하십시오 우리의 [메뉴](~/mac/user-interface/menu.md) 설명서.


## <a name="synchronizing-changes-with-xcode"></a>Xcode와 변경 내용 동기화

에서 전환 하면 Visual Studio로 다시 Mac 용 Xcode, Xcode에서 변경한 내용은 자동으로 Xamarin.Mac 프로젝트와 동기화 됩니다.

선택 하는 경우는 **MainWindow.designer.cs** 에 **Solution Pad** 어떻게는 출 선 및 작업 연결 구성 보려는 수는 C# 코드:

[![Xcode와 변경 내용 동기화](xib-images/sync01.png "Xcode와 변경 내용 동기화")](xib-images/sync01-large.png#lightbox)

알림 방법의 두 정의 **MainWindow.designer.cs** 파일:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

정의 사용 하 여 줄 위로 합니다 **MainWindow.h** Xcode에서 파일:

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Mac 용 Visual Studio.h 파일에 변경 내용을 수신 대기 하 고 각각에 해당 변경 내용을 자동으로 동기화 알 수 있듯이 **. designer.cs** 파일을 응용 프로그램에 노출 합니다. 또한 알 수 있습니다 **MainWindow.designer.cs** Mac 용 Visual Studio를 수정할 필요가 없습니다 있도록 partial 클래스에는 **MainWindow.cs** 에서는 클래스에 대 한 모든 변경 내용을 덮어쓰는 것입니다.

일반적으로 필요가 엽니다는 **MainWindow.designer.cs** 직접 보여드린 것 여기 교육 목적에 대 한 합니다.

> [!IMPORTANT]
> 대부분의 경우 Mac 용 Visual Studio 자동으로 Xcode에서 변경 된 내용을 참조 하 고 Xamarin.Mac 프로젝트에 동기화 합니다. 동기화가 자동으로 수행되지 않는 경우 Xcode로 돌아가서 다시 Mac용 Visual Studio로 돌아갑니다. 대부분 이렇게 하면 동기화 주기가 시작됩니다.


## <a name="adding-a-new-window-to-a-project"></a>프로젝트에 새 창을 추가

기본 문서 창 외에도 Xamarin.Mac 응용 프로그램을 다른 유형의 windows 기본 설정이 나 검사기 패널 등 사용자에 게 표시 해야 합니다. 프로젝트에 새 창을 추가 하는 경우는 항상 사용 해야 합니다 **Cocoa 창 컨트롤러를 사용 하 여** 옵션을이 사용 하면 쉽게 파일을.xib에서 창을 로드 프로세스입니다.

새 창에 추가 하려면 다음을 수행 합니다.

1. 에 **Solution Pad**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** .
2. 새 파일 대화 상자에서 선택 **Xamarin.Mac** > **컨트롤러를 사용 하 여 Cocoa 창을**:

    ![새 창 컨트롤러를 추가](xib-images/new01.png "새 창 컨트롤러를 추가 합니다.")
3. **이름**에 대해 `PreferencesWindow`를 입력하고 **새로 만들기** 단추를 클릭합니다.
4. 두 번 클릭 합니다 **PreferencesWindow.xib** Interface Builder에서 편집 하기 위해 열려는 파일:

    [![Xcode에서 창의 편집](xib-images/new02.png "창 Xcode에서 편집")](xib-images/new02-large.png#lightbox)
5. 사용자 인터페이스를 디자인 합니다.

    [![Windows 레이아웃 디자인](xib-images/new03.png "windows 레이아웃 디자인")](xib-images/new03-large.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

다음 코드를 추가 **AppDelegate.cs** 에 새 창을 표시 하려면:

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

`var preferences = new PreferencesWindowController ();` 줄.xib 파일에서 창을 로드 하 고이 확장 하는 창 컨트롤러의 새 인스턴스를 만듭니다. `preferences.Window.MakeKeyAndOrderFront (this);` 줄 사용자에 게 새 창을 표시 합니다.

코드를 실행 하 고 선택 하는 경우는 **기본 설정...**  에서 합니다 **응용 프로그램 메뉴**는 창이 표시 됩니다.

![샘플 앱 실행](xib-images/new04.png "샘플 앱 실행")

Xamarin.Mac 응용 프로그램에서 Windows 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Windows](~/mac/user-interface/window.md) 설명서.


## <a name="adding-a-new-view-to-a-project"></a>프로젝트에 새 뷰 추가

쉽게 관리가 더 용이한.xib 파일을 여러 개에 창의 디자인을 세분화할 수 하는 경우가 있습니다. 도구 모음 항목을 선택 하는 경우 주 창의 내용을 전환 같은 예를 들어를 **기본 설정 창** 대 한 응답으로 콘텐츠를 교환 또는 **원본 목록** 선택 합니다.

프로젝트에 새 뷰를 추가 하는 경우는 항상 사용 해야 합니다 **Cocoa 뷰 컨트롤러를 사용 하 여** 옵션을이 인해 더 쉽게 파일을.xib에서 뷰를 로드 하는 프로세스입니다.

새 보기를 추가 하려면 다음을 수행 합니다.

1. 에 **Solution Pad**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** .
2. 새 파일 대화 상자에서 선택 **Xamarin.Mac** > **Cocoa 뷰 컨트롤러를 사용 하 여**:

    ![새 뷰 추가](xib-images/view01.png "새 뷰 추가")
3. **이름**에 대해 `SubviewTable`를 입력하고 **새로 만들기** 단추를 클릭합니다.
4. 두 번 클릭 합니다 **SubviewTable.xib** Interface Builder에서 편집용으로 엽니다 및 사용자 인터페이스를 디자인 하는 파일:

    [![Xcode에서 새 보기를 디자인할](xib-images/view02.png "Xcode에서 새 뷰를 디자인 합니다.")](xib-images/view02-large.png#lightbox)
5. 모든 필요한 작업 및 출 선을 연결 합니다.
6. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

다음 편집 합니다 **SubviewTable.cs** 하 고 다음 코드를 추가 합니다 **AwakeFromNib** 파일 로드 될 때 새 뷰를 채우는:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));
    DataSource.Sort ("Title", true);

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);

    // Auto select the first row
    ProductTable.SelectRow (0, false);
}
```

보기는 현재 표시 되는 프로젝트를 추적 하는 열거형을 추가 합니다. 예를 들어 **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

뷰에 사용 되 고 표시 하는 창의.xib 파일을 편집 합니다. 추가 된 **사용자 지정 보기** 역할을 할 컨테이너 보기에 대 한 메모리에 로드 되 면 C# 코드 및 호출 하는 콘센트에 게 노출 `ViewContainer`:

[![필요한 콘센트를 만드는](xib-images/view03.png "필요한 출 선 만들기")](xib-images/view03-large.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

다음으로, 새 보기를 표시 하는 창의.cs 파일을 편집 (예를 들어 **MainWindow.cs**) 다음 코드를 추가 합니다.

```csharp
private SubviewType ViewType = SubviewType.None;
private NSViewController SubviewController = null;
private NSView Subview = null;
...

private void DisplaySubview(NSViewController controller, SubviewType type) {

    // Is this view already displayed?
    if (ViewType == type) return;

    // Is there a view already being displayed?
    if (Subview != null) {
        // Yes, remove it from the view
        Subview.RemoveFromSuperview ();

        // Release memory
        Subview = null;
        SubviewController = null;
    }

    // Save values
    ViewType = type;
    SubviewController = controller;
    Subview = controller.View;

    // Define frame and display
    Subview.Frame = new CGRect (0, 0, ViewContainer.Frame.Width, ViewContainer.Frame.Height);
    ViewContainer.AddSubview (Subview);
}
```

창의 컨테이너.xib 파일에서 로드 된 새 보기를 표시 하려고 하는 경우 (합니다 **사용자 지정 보기** 위에서 추가한), 기존 보기를 제거 하 고 새 가설에 대해 교환 하는 것이 코드 처리 합니다. 이미는 보기를 표시 하므로 해당 제거 화면에서 하는 경우를 찾습니다. 전달 된 뷰는 다음의 (뷰 컨트롤러에서 로드할) 콘텐츠 영역에 맞게 크기가 조정 되므로으로 표시 한 콘텐츠를 추가 합니다.

새 보기를 표시 하려면 다음 코드를 사용 합니다.

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

표시할 새 보기에 대 한 뷰 컨트롤러의 새 인스턴스를 만듭니다이, 해당 형식 (프로젝트에 추가 하는 열거형으로 지정 된)으로 설정 하 고 사용 하 여는 `DisplaySubview` 메서드가 실제로 뷰를 표시 하는 창의 클래스에 추가 합니다. 예를 들어:

[![샘플 앱 실행](xib-images/view04.png "샘플 앱 실행")](xib-images/view04-large.png#lightbox)

Xamarin.Mac 응용 프로그램에서 Windows 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Windows](~/mac/user-interface/window.md) 하 고 [대화 상자](~/mac/user-interface/dialog.md) 설명서.


## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 응용 프로그램에서.xib 파일 작업을 자세히 살펴보고를 걸렸습니다. 응용 프로그램의 사용자 인터페이스를 만들고, xcode의 파일을 만들고.xib을 유지 관리 하는 방법에.xib 파일을 사용 하는 방법과 작성기 인터페이스를 다양 한 유형 및.xib 파일의 사용에 살펴보았습니다 C# 코드입니다.


## <a name="related-links"></a>관련 링크

- [MacImages(샘플)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [메뉴](~/mac/user-interface/menu.md)
- [대화 상자](~/mac/user-interface/dialog.md)
- [이미지 작업](~/mac/app-fundamentals/image.md)
- [macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
