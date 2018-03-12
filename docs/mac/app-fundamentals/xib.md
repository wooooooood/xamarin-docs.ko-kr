---
title: ".xib 파일"
description: "이 문서에서는.xib 만들고 Xamarin.Mac 응용 프로그램에 대 한 사용자 인터페이스를 유지 관리 하는 Xcode의 인터페이스 작성기에서 만든 파일에서 작업을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 675B9405-D9A7-49F0-94AD-417F10A71D11
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 8ca187b86126c9a0f2d9931f63d75e99ac4d2b23
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="xib-files"></a>.xib 파일

_이 문서에서는.xib 만들고 Xamarin.Mac 응용 프로그램에 대 한 사용자 인터페이스를 유지 관리 하는 Xcode의 인터페이스 작성기에서 만든 파일에서 작업을 설명 합니다._

> [!NOTE]
> 스토리 보드가 있는 Xamarin.Mac 앱에 대 한 사용자 인터페이스를 만들 수는 것이 좋습니다. 이 설명서 지금 및 이전 Xamarin.Mac 프로젝트와 작업에 대 한 위치에 남아 있습니다. 자세한 내용은 참조 하십시오 우리의 [스토리 보드에는 소개](~/mac/platform/storyboards/index.md) 설명서입니다.

## <a name="overview"></a>개요

동일한 사용자 인터페이스 요소에 대 한 액세스 권한이 Xamarin.Mac 응용 프로그램에서 C# 및.NET으로 작업을 하는 경우 및 작업 하는 개발자 도구를 *Objective-C* 및 *Xcode* 않습니다. Xamarin.Mac Xcode에 직접 통합을 사용 하면 Xcode의 _인터페이스 작성기_ 만들기 및 사용자 인터페이스를 유지 관리 (또는 C# 코드에서 직접 만들 필요에 따라).

.Xib 파일은 Xcode의 인터페이스 작성기에서 작성 및 관리 되는 응용 프로그램의 사용자 인터페이스 (예: 메뉴, Windows, 뷰, 레이블, 텍스트 필드)의 요소를 그래픽으로 정의 하 여 macOS 사용 됩니다.

[![실행 중인 응용 프로그램의 예로](xib-images/intro01.png "실행 중인 응용 프로그램의 예")](xib-images/intro01-large.png)

이 문서에서는의 기본적인 Xamarin.Mac 응용 프로그램에서.xib 파일을 사용 하겠습니다. 것이 가장 좋습니다를 통해 협력 하 고 [Hello, Mac](~/mac/get-started/hello-mac.md) 이 문서에서 사용할 수 있는 주요 개념 및 기술을 설명 하는 대로 먼저 문서.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 특성 요소 Objective-C 개체 및 UI에 C# 클래스를 연결 하는 데 사용 합니다.


## <a name="introduction-to-xcode-and-interface-builder"></a>Xcode 및 Interface Builder 소개

Xcode의 일환으로, 사과 디자이너에서 사용자 인터페이스를 시각적으로 만들 수 있는 인터페이스 작성기 라는 도구를 만들었습니다. Xamarin.Mac 통합 fluently 작성기 인터페이스를 Objective-C 사용자가 수행 하는 같은 도구로 UI를 만들 수 있습니다.


### <a name="components-of-xcode"></a>Xcode 구성 요소

Mac 용 Visual Studio에서 Xcode에서.xib 파일을 열 때 사용 하 여 여는 **Project Navigator** 왼쪽에는 **인터페이스 계층 구조** 및 **인터페이스 편집기** 중간에 및 **속성 및 유틸리티** 오른쪽의 섹션:

[![Xcode UI의 구성 요소](xib-images/xcode03.png "Xcode UI의 구성 요소")](xib-images/xcode03-large.png)

직접 살펴보겠습니다 이러한 각 기능에서 Xcode 섹션은 며는 사용법을 Xamarin.Mac 응용 프로그램에 대 한 인터페이스를 만들려고 합니다.


#### <a name="project-navigation"></a>프로젝트 탐색

Xcode에서 편집할.xib 파일을 여는 경우 Mac 용 Visual Studio 자체와 Xcode 간의 변경 내용을 통신 하는 백그라운드에서 Xcode 프로젝트 파일을 만듭니다. 나중을 전환 하면 Visual Studio로 다시 Mac 용 Xcode에서이 프로젝트의 변경 된 프로젝트와 동기화 Xamarin.Mac Visual Studio에서 Mac.에 대 한

**프로젝트 탐색** 섹션에서는이 구성 하는 파일의 모든 간을 탐색할 수 있습니다 _shim_ Xcode 프로젝트. 일반적으로 수만 있습니다.xib 파일 등이 목록에 관심이 **MainMenu.xib** 및 **MainWindow.xib**합니다.


#### <a name="interface-hierarchy"></a>인터페이스 계층 구조

**인터페이스 계층 구조** 섹션와 같은 사용자 인터페이스의 몇 가지 주요 속성을 쉽게 액세스할 수 있습니다는 **자리 표시자** 및 주 **창**합니다. 이 섹션을 사용 하 여 끌어 주위 계층 내에서 중첩 되어 하는 방법은 사용자 인터페이스를 조정 하는 개별 요소 (views)에 액세스할 수 있습니다.


#### <a name="interface-editor"></a>인터페이스 편집기

**인터페이스 편집기** 섹션에는 화면을 제공 하면 그래픽으로 레이아웃 회원님의 사용자 인터페이스입니다. 요소를 끌면 됩니다는 **라이브러리** 의 섹션은 **속성 및 유틸리티** 프로그램 디자인을 만드는 섹션. 디자인 화면에 추가 하는 사용자 인터페이스 요소 (views)과 수에 추가지 것입니다는 **인터페이스 계층 구조** 섹션에 나타나는 순서는 **인터페이스 편집기**합니다.


#### <a name="properties--utilities"></a>속성 및 유틸리티

**속성 및 유틸리티** 섹션은 함께 사용할 수 있는 두 개의 주 섹션으로 devided **속성** (검사자 라고도 함) 및 **라이브러리**:

![속성 검사자](xib-images/xcode04.png "속성 관리자")

그러나 처음이 섹션은 거의 비어에서 요소를 선택 하는 경우는 **인터페이스 편집기** 또는 **인터페이스 계층 구조**, **속성** 섹션 채워집니다. 지정 된 요소 및 조정할 수 있는 속성에 대 한 정보입니다.

**속성** 섹션 내에는 다음 그림처럼 8개의 *검사기 탭*이 있습니다.

[![모든 검사기에 대해 간략하게](xib-images/xcode05.png "모든 검사기에 대 한 개요")](xib-images/xcode05-large.png)

왼쪽부터 시작해서 오른쪽으로 가면서 다음과 같은 탭이 있습니다.

-   **파일 검사기** – 파일 검사기는 파일 이름, 편집 중인 Xib 파일의 위치와 같은 파일 정보를 보여줍니다.
-   **빠른 도움말** – 빠른 도움말 탭에서는 현재 Xcode에서 선택한 항목에 따라 상황에 맞는 도움말을 제공합니다.
-   **ID 검사기** – ID 검사기는 선택한 컨트롤/보기에 대한 정보를 제공합니다.
-   **검사기 특성** – The 특성 검사기를 사용 하면 선택된 된 컨트롤/보기의 다양 한 특성을 사용자 지정할 수 있습니다.
-   **크기 조정 검사기** –의 크기 검사기를 사용 하면 크기 및 크기 조정 선택한 컨트롤/보기의 동작을 제어할 수 있습니다.
-   **연결 검사기** – 연결 관리자의 선택 된 컨트롤의 콘센트 및 작업 연결을 표시 합니다. 콘센트 및 작업을 잠시 후에 검토 합니다.
-   **바인딩 검사기** – The 바인딩 관리자를 사용 하면 해당 값 데이터 모델에 자동으로 바인딩된 컨트롤을 구성할 수 있습니다.
-   **효과 검사기 볼** – The 보기 효과 검사기를 사용 하면 애니메이션 등의 컨트롤에 미치는 영향을 지정할 수 있습니다.

에 **라이브러리** 섹션에서 컨트롤을 찾을 수 있습니다 및 그래픽을 디자이너에 배치할 개체를 사용자 인터페이스 빌드:

![라이브러리 관리자의 예로](xib-images/xcode06.png "라이브러리 관리자의 예")

인 경우 Xcode IDE와 인터페이스 작성기에 이해 했으므로 사용자 인터페이스 만들기에서 살펴보겠습니다.


## <a name="creating-and-maintaining-windows-in-xcode"></a>만들기 및 Xcode에서 windows 유지 관리

스토리 보드가 있는 것이 좋습니다 Xamarin.Mac 응용 프로그램의 사용자 인터페이스를 만들기 위한 (참조 하십시오 우리의 [스토리 보드에 소개](~/mac/platform/storyboards/index.md) 자세한 정보에 대 한 설명서)와 사용할 Xamarin.Mac 됩니다 모든 새 프로젝트를 시작 하는 결과적으로, 기본적으로 스토리 보드를 사용 합니다.

기반 UI를는.xib을 사용 하 여 전환 하려면 다음을 수행 합니다.

1. Mac 용 Visual Studio를 열고 새 Xamarin.Mac 프로젝트를 시작 합니다.
2. 에 **솔루션 패드**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...**
3. 선택 **Mac** > **Windows 컨트롤러**:

    ![새 창 컨트롤러 추가](xib-images/setup00.png "새 창 컨트롤러 추가")
4. 입력 `MainWindow` 클릭 이나 이름에는 **새로** 단추:

    ![새 주 창 추가](xib-images/setup01.png "새 주 창 추가")
5. 프로젝트를 다시 클릭 하 고 선택 **추가** > **새 파일...**
6. 선택 **Mac** > **주 메뉴**:

    ![새 주 메뉴 추가](xib-images/setup02.png "새 주 메뉴 추가")
7. 로 이름을 둡니다 `MainMenu` 클릭는 **새로** 단추입니다.
8. 에 **솔루션 패드** 선택 된 **Main.storyboard** 파일을 마우스 오른쪽 단추로 클릭 한 다음 선택 **제거**:

    ![주 스토리 보드를 선택 하면](xib-images/setup03.png "주 스토리 보드를 선택 합니다.")
9. 제거 대화 상자에서 클릭 된 **삭제** 단추:

    ![삭제 확인](xib-images/setup04.png "삭제 확인")
10. 에 **솔루션 패드**, 두 번 클릭은 **Info.plist** 편집을 위해 열 파일입니다.
11. 선택 `MainMenu` 에서 **주 인터페이스** 드롭다운:

    [![주 메뉴 설정](xib-images/setup05.png "주 메뉴를 설정 합니다.")](xib-images/setup05-large.png)
12. 에 **솔루션 패드**, 두 번 클릭은 **MainMenu.xib** Xcode의 인터페이스 작성기에서 편집을 위해 열 파일입니다.
13. 에 **라이브러리 검사기**, 형식 `object` 검색 필드에 다음 끌어 새 **개체** 디자인 화면으로:

    [![주 메뉴 편집](xib-images/setup06.png "주 메뉴 편집")](xib-images/setup06-large.png)
14. 에 **Identity 관리자**, 입력 `AppDelegate` 에 대 한는 **클래스**:

    [![응용 프로그램 대리자를 선택 하면](xib-images/setup07.png "앱 대리자를 선택 합니다.")](xib-images/setup07-large.png)
15. 선택 **파일의 소유자만** 에서 **인터페이스 계층 구조**, 전환할는 **연결 검사기** 대리자에서 선을 끌어 및는 `AppDelegate` **개체** 프로젝트에 추가 되었습니다.

    [![응용 프로그램 대리자를 연결](xib-images/setup08.png "앱 대리자 연결")](xib-images/setup08-large.png)
16. 변경 내용을 저장 하 고 Mac.에 Visual Studio로 반환 합니다.

위치에서 이러한 모든 변화를 통해 편집는 **AppDelegate.cs** 파일을 다음과 같이 표시 하 게 합니다.

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

이제 응용 프로그램의 주 창에는.xib 정의 (OS **X*- **I**인터페이스 **B**uilder) 창에 추가할 때 프로젝트에 자동으로 포함 파일 컨트롤러입니다. windows 디자인에 맞게 편집 하는 **솔루션 패드**, 두 번 클릭 하 고 **MainWindow.xib** 파일:

![MainWindow.xib 파일을 선택 하면](xib-images/edit01.png "MainWindow.xib 파일 선택")

Xcode의 인터페이스 작성기의 창 디자인을 열립니다.

[![MainWindow.xib 편집](xib-images/edit02.png "는 MainWindow.xib 편집")](xib-images/edit02-large.png)


### <a name="standard-window-workflow"></a>표준 창 워크플로

만들고 Xamarin.Mac 응용 프로그램에서 작동 하는 모든 창에 대 한 프로세스는 기본적으로 동일 합니다.

1. 기본 프로젝트에 자동으로 추가 되지 않은 새 창에 대 한 새 창 정의 프로젝트에 추가 합니다.
2. Xcode의 인터페이스 작성기에서 편집을 위해 창 디자인 열려는.xib 파일을 두 번 클릭 합니다.
3. 모든 필요한 창 속성을 설정는 **특성 검사기** 및 **크기 검사기**합니다.
4. 사용자 인터페이스를 만들고 구성에서 해당 하는 데 필요한 컨트롤을 끌어는 **특성 검사기**합니다.
5. 사용 하 여는 **크기 검사기** UI 요소에 대 한 크기 조정을 처리 하도록 합니다.
6. 창의 UI 요소 콘센트 및 동작을 통해 C# 코드를 노출 합니다.
7. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 다시 전환 합니다.


### <a name="designing-a-window-layout"></a>창 레이아웃을 디자인합니다.

인터페이스 작성기에서 사용자 인터페이스를 배치에 대 한 프로세스는 기본적으로 추가 하는 모든 요소에 대해 동일 합니다.

1. 원하는 컨트롤을 찾을 **라이브러리 검사기** 으로 끌어 놓습니다는 **인터페이스 편집기** 에 배치 합니다.
2. 모든 필요한 창 속성을 설정는 **특성 검사기**합니다.
3. 사용 하 여는 **크기 검사기** UI 요소에 대 한 크기 조정을 처리 하도록 합니다.
4. 사용자 지정 클래스를 사용 하는 경우에 설정 된 **Identity 관리자**합니다.
5. UI 요소를 콘센트 및 동작을 통해 C# 코드를 노출 합니다.
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 다시 전환 합니다.

예를 들면 다음과 같습니다.

1. Xcode의 **라이브러리 섹션**에서 **누름 단추**를 끕니다.

    [![단추를 선택 하 고 라이브러리에서](xib-images/xcode07.png "단추를 선택 하 고 라이브러리에서")](xib-images/xcode07-large.png)
2. 드롭 단추에는 **창** 에 **인터페이스 편집기**:

    [![창에 단추 추가](xib-images/xcode08.png "창에 단추 추가")](xib-images/xcode08-large.png)
3. **특성 검사기**에서 **제목** 속성을 클릭하고 단추 제목을 `Click Me`로 변경합니다.

    ![단추 특성 설정](xib-images/xcode09.png "단추 특성 설정")
4. **라이브러리 섹션**에서 **레이블**을 끕니다.

    [![라이브러리의 레이블을 선택](xib-images/xcode10.png "라이브러리의 레이블을 선택")](xib-images/xcode10-large.png)
5. **인터페이스 편집기**에서 단추 옆에 있는 **창**에 레이블을 놓습니다.

    [![창에 레이블을 추가](xib-images/xcode11.png "창에 레이블 추가")](xib-images/xcode11-large.png)
6. 레이블의 오른쪽 핸들을 잡고 창의 가장자리 근처까지 끕니다.

    [![레이블 크기 조정](xib-images/xcode12.png "레이블 크기 조정")](xib-images/xcode12-large.png)
7. 선택한 상태에서 레이블로 **인터페이스 편집기**, 전환할는 **크기 검사기**:

    ![크기 관리자를 선택 하면](xib-images/xcode13.png "크기 관리자를 선택 합니다.")
8. 에 **자동 크기 조정 상자** 클릭는 **Dim 빨간색 대괄호** 오른쪽에 및 **Dim 빨간색 옆쪽 화살표** 센터에서:

    ![크기 자동 조정 속성을 편집](xib-images/xcode14.png "크기 자동 조정 속성 편집")
9. 이렇게 하면 레이블이 따라 확장 하거나 축소할 수는 창의 실행 중인 응용 프로그램 크기 조정 연장 됩니다. **빨간색 대괄호** 위쪽 및 왼쪽에는 **자동 크기 조정 상자** 상자 위치를 지정한 X 및 Y 중단 된 것으로 레이블의 알려야 합니다.
10. 사용자 인터페이스에 변경 내용을 저장합니다

크기 조정 하 고 이동 하는 컨트롤 주위의 것을 알아야 했습니다 인터페이스 작성기 기반으로 하는 것이 도움이 스냅인 힌트는 하면 [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다. 이러한 지침을 Mac 사용자에 대 한 친숙 한 모양 및 느낌에 있는 고품질 응용 프로그램을 만들 수 있습니다.

보면는 **인터페이스 계층 구조** 섹션에서 방법을 레이아웃과 사용자 인터페이스를 구성 하는 요소 들의 계층이 표시 됩니다.

![인터페이스 계층 구조에서 요소를 선택 하면](xib-images/xcode15.png "인터페이스 계층 구조에서 요소를 선택 합니다.")

여기에서 편집 하거나 필요한 경우에 UI 요소를 다시 정렬 하려면 끌어 항목을 선택할 수 있습니다. 예를 들어 UI 요소의 다른 요소에 의해 검사 되 고, 경우 끌어 놓을 수 있습니다 맨 아래 목록에서 창에서 최상위 항목을 확인 합니다.

Xamarin.Mac 응용 프로그램에서 Windows 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Windows](~/mac/user-interface/window.md) 설명서입니다.


## <a name="exposing-ui-elements-to-c-code"></a>UI 요소를 C# 코드 노출

레이아웃 인터페이스 작성기에서 사용자 인터페이스의 모양과 느낌을 완료 했으면, C# 코드에서 액세스할 수 있도록 UI 요소를 노출 해야 합니다. 이 위해 사용할 작업 및 콘센트 합니다.


### <a name="setting-a-custom-main-window-controller"></a>주 창 사용자 지정 컨트롤러 설정

UI 요소를 C# 코드 노출 하기 위해 콘센트 및 동작을 만들 수 있으려면 Xamarin.Mac 앱 사용자 지정 창 컨트롤러를 사용 해야 합니다.

다음을 수행합니다.

1. Xcode의 인터페이스 작성기에서 응용 프로그램의 스토리 보드를 엽니다.
2. 선택 된 `NSWindowController` 디자인 화면에서 합니다.
3. 전환 하는 **Identity 관리자** 보기 및 입력 `WindowController` 로 **클래스 이름**:

    [![클래스 이름을 편집](xib-images/windowcontroller01.png "클래스 이름 편집")](xib-images/windowcontroller01-large.png)
4. 변경 내용을 저장 하 고 동기화 하는 Mac에 대 한 Visual Studio로 돌아갑니다.
5. A **WindowController.cs** 파일을 프로젝트에 추가 됩니다는 **솔루션 패드** Mac 용 Visual Studio에서:

    ![Mac 용 Visual Studio에서 새 클래스 이름을](xib-images/windowcontroller02.png "Mac 용 Visual Studio에서 새 클래스 이름")
6. Xcode의 인터페이스 작성기에서 스토리 보드를 다시 엽니다.
7. **WindowController.h** 파일은 사용 하기 위해 사용할 수 있습니다.

    [![Xcode에서 일치 하는.h 파일](xib-images/windowcontroller03.png "Xcode에서 일치 하는.h 파일")](xib-images/windowcontroller03-large.png)


### <a name="outlets-and-actions"></a>콘센트 및 동작

따라서 콘센트 및 작업은 무엇입니까? 전통적인 .NET 사용자 인터페이스 프로그래밍에서, 사용자 인터페이스의 컨트롤은 추가될 때 자동으로 속성으로 노출됩니다. Mac에서는 동작이 약간 다릅니다. 보기에 컨트롤을 추가하기만 해서는 코드에 액세스할 수 없습니다. 개발자가 UI 요소를 코드에 명시적으로 노출해야 합니다. 순서에서이 작업을 수행, 사과 목록에 두 가지 옵션:

-  **출선** – 출선은 속성과 비슷합니다. 속성을 통해 코드에 노출 되는 컨트롤을 콘센트에 연결 하는 경우, 이벤트 처리기를 연결 하는 등의 작업을 수행할 수 있도록 하 고, 등의 메서드를 호출 합니다.
-  **작업** – 작업은 WPF의 명령 패턴과 비슷합니다. 예를 들어, 컨트롤에서 작업을 수행 하면 예: 단추 클릭, 컨트롤 코드에서 메서드를 자동으로 호출 됩니다. 많은 컨트롤을 연결 하 여 동일한 작업을 하기 때문에 작업은 강력 하 고 편리한입니다.

통해 코드에서 직접 콘센트 및 작업 Xcode에서 추가 *컨트롤 끌기*합니다. 보다 구체적으로, 즉 콘센트 또는 동작을 만들려면 컨트롤 요소를 콘센트 또는 동작 하는 추가 키를 누른 채 원하는 선택는 **제어** 키보드에서 단추 및 코드에 직접 해당 컨트롤을 끌어 옵니다.

Xamarin.Mac 개발자를 위한 콘센트 또는 동작을 만들려면 원하는 C# 파일에 해당 하는 Objective-C 스텁 파일에 끌어을 의미 합니다. Mac 용 visual Studio 라는 파일을 만든 **MainWindow.h** shim Xcode 프로젝트의 일부로 인터페이스 작성기 사용 하 여 생성:

[![Xcode에서.h 파일의 예](xib-images/xcode16.png "Xcode에서.h 파일의 예")](xib-images/xcode16-large.png)

이 스텁.h 파일을 미러링합니다.는 **MainWindow.designer.cs** 새 Xamarin.Mac 프로젝트에 자동으로 추가 된 `NSWindow` 만들어집니다. 이 파일이 인터페이스 작성기 수행한 변경 내용을 동기화 하는 데 사용할 하며 여기서 만듭니다 콘센트 및 동작 UI 요소는 C# 코드에 노출 되도록 합니다.


#### <a name="adding-an-outlet"></a>추가 콘센트에 연결

콘센트 및 작업은 한 기본적인 설명을 살펴보겠습니다 UI 요소가 C# 코드에 노출 되도록 콘센트 만들기.

다음을 수행합니다.

1. 화면의 오른쪽 맨 위 모서리에 있는 Xcode에서 **이중 원** 단추를 클릭하여 **도우미 편집기**를 엽니다.

    [![도우미 편집기를 선택 하면](xib-images/outlet01.png "길잡이 편집기를 선택 합니다.")](xib-images/outlet01-large.png)
2. Xcode가 분할 보기 모드로 전환되어 한 쪽에는 **인터페이스 편집기**가, 다른 쪽에는 **코드 편집기** 표시됩니다.
3. Xcode가 자동으로 선택 하는 **MainWindowController.m** 파일에 **코드 편집기**는 올바르지 않습니다. 콘센트 및 동작 이란 위에 한 토론에서 기억 있어야 해야는 **MainWindow.h** 선택 합니다.
4. 맨 위에 있는 **코드 편집기** 클릭는 **자동 링크** 선택 하 고는 **MainWindow.h** 파일:

    [![올바른.h 파일을 선택 하면](xib-images/outlet02.png "올바른.h 파일 선택")](xib-images/outlet02-large.png)
5. Xcode가 이제 올바른 파일을 선택했습니다.

    [![올바른 파일 선택](xib-images/outlet03.png "올바른 파일 선택")](xib-images/outlet03-large.png)
6. **마지막 단계는 매우 중요합니다!** 올바른 파일 선택를 설정 하지 않은 콘센트 만들 수 없습니다 및 동작 또는 C#에서 잘못 된 클래스에 노출 됩니다!
7. 에 **인터페이스 편집기**, 키를 누른 채는 **제어** 키보드의 키를 클릭 한 후 코드 편집기에 위에서 만든 레이블 드래그 바로 아래에서 `@interface MainWindow : NSWindow { }` 코드:

    [![새 콘센트 만들려는 끌기](xib-images/outlet04.png "새 콘센트 만들려는 끌기")](xib-images/outlet04-large.png)
8. 대화 상자가 표시됩니다. 유지 된 **연결** 콘센트로 설정 하 고 입력 `ClickedLabel` 에 대 한는 **이름**:

    [![콘센트 속성을 설정](xib-images/outlet05.png "콘센트 속성 설정")](xib-images/outlet05-large.png)
9. 클릭는 **연결** 콘센트 만들기 단추:

    ![완료 된 콘센트](xib-images/outlet06.png "완료 콘센트")
10. 파일의 변경 내용을 저장합니다.


#### <a name="adding-an-action"></a>작업 추가

다음으로, C# 코드에 대 한 UI 요소와 사용자 상호 작용을 노출 하는 작업 만들기에 대해 살펴보겠습니다.

다음을 수행합니다.

1. 우리는 여전히 있는지 확인은 **도우미 편집기** 및 **MainWindow.h** 파일을 볼 수는 **코드 편집기**합니다.
2. 에 **인터페이스 편집기**, 키를 누른 채는 **제어** 키보드의 키 및 코드 편집기에 위에서 만든 단추를 클릭 한 후 드래그 바로 아래에서 `@property (assign) IBOutlet NSTextField *ClickedLabel;` 코드:

    [![동작을 만들려면 끌기](xib-images/action01.png "끌기 동작을 만들려면")](xib-images/action01-large.png)
3. 변경 된 **연결** 액션에는 형식:

    [![작업 유형 선택](xib-images/action02.png "동작 종류를 선택 합니다.")](xib-images/action02-large.png)
4. **이름**으로 `ClickedButton`을 입력합니다.

    [![액션 구성](xib-images/action03.png "액션 구성")](xib-images/action03-large.png)
5. 클릭는 **연결** 단추 동작을 만들려면:

    ![완료 된 동작](xib-images/action04.png "완료 된 동작")
6. 파일의 변경 내용을 저장합니다.

유선 접속 및 C# 코드에 노출 하면 사용자 인터페이스와 Mac 용 Visual Studio로 다시 전환 하 고 Xcode 및 인터페이스 작성기의 변경 내용을 동기화 되기를 기다립니다.


### <a name="writing-the-code"></a>코드 작성

만든 사용자 인터페이스와 콘센트 및 동작을 통해 코드에 노출 하는 UI 요소를 준비가 프로그램 수명을 도입 하는 코드를 작성 합니다. 예를 들어 열은 **MainWindow.cs** 에 두 번 클릭 하 여 편집을 위해 파일의 **솔루션 패드**:

[![MainWindow.cs 파일](xib-images/code01.png "The MainWindow.cs 파일")](xib-images/code01-large.png)

다음 코드를 추가 하 고는 `MainWindow` 위에서 만든 샘플 콘센트 작업할 클래스:

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

`NSLabel` 지정 Xcode에서 Xcode에서 출구를 만들 때,이 경우 호출 됩니다는 직접 이름으로 C#에서 액세스 됩니다 `ClickedLabel`합니다. 일반 C# 클래스 같은 방식으로 노출된 된 개체의 속성이 나 메서드는 액세스할 수 있습니다.

> [!IMPORTANT]
> 사용 해야 `AwakeFromNib`와 같은 다른 방법을 대신 `Initialize`때문에, `AwakeFromNib` 라고 _후_ 운영 체제를 로드 하 고.xib 파일에서 사용자 인터페이스를 인스턴스화할 시킨 합니다. 얻을 하 려 한 경우.xib 파일 전에 레이블 컨트롤에 액세스 완전히 로드 되 고 인스턴스화된는 `NullReferenceException` 오류 label 컨트롤 아직 생성 되지 것입니다.

다음으로, 다음 partial 클래스를 추가 `MainWindow` 클래스:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

이 코드는 단추를 클릭할 때마다 라고 하 고 Xcode 및 인터페이스 작성기에서 만든 하는 동작에 연결 합니다.

일부 UI 요소가 자동으로 기본적으로 작업을 예를 들어 기본 메뉴 모음에서 항목와 같은 **열기...**  메뉴 항목 (`openDocument:`). 에 **솔루션 패드**를 두 번 클릭는 `AppDelegate.CS` 편집 열고 아래에 다음 코드를 추가 하는 파일의 `DidFinishLaunching` 메서드:

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

여기 키 줄이 `[Export ("openDocument:")]`, 인지 `NSMenu` 하는 **AppDelegate** 메서드가 `void OpenDialog (NSObject sender)` 에 대해 응답 하는 `openDocument:` 동작 합니다.

작업 메뉴에 대 한 자세한 내용은 참조 하십시오 우리의 [메뉴](~/mac/user-interface/menu.md) 설명서입니다.


## <a name="synchronizing-changes-with-xcode"></a>Xcode 사용 하 여 동기화 중 변경

을 전환 하면 Visual Studio로 다시 mac에서 Xcode Xcode에서 변경한 모든 내용은 Xamarin.Mac 프로젝트와 함께 자동으로 동기화 됩니다.

선택 하는 경우는 **MainWindow.designer.cs** 에 **솔루션 패드** 어떻게 우리의 콘센트 및 작업 되었습니다 유선를 C# 코드에서 볼 수 있습니다.

[![Xcode 변경 내용을 동기화 할](xib-images/sync01.png "변경 Xcode와 동기화")](xib-images/sync01-large.png)

알림 방법에서 두 개의 정의 **MainWindow.designer.cs** 파일:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

정의 일렬로 **MainWindow.h** Xcode에서 파일:

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Mac 용 Visual Studio.h 파일에 변경 내용을 수신 대기 하 고 다음에 해당 변경 사항을 자동으로 동기화 볼 수 있듯이 **. designer.cs** 파일을 응용 프로그램을 노출 합니다. 알 수 있습니다 **MainWindow.designer.cs** Mac 용 Visual Studio를 수정 하지 않아도 되도록 partial 클래스는 **MainWindow.cs** 있는 우리는 클래스에 대 한 변경 내용을 덮어씁니다.

일반적으로 필요 하지 열려는 **MainWindow.designer.cs** , 직접 것가 여기에 제시 된 교육 목적 으로만 합니다.

> [!IMPORTANT]
> 대부분의 경우에서 Mac 용 Visual Studio 자동으로 Xcode에서 변경한 내용을 참조 하 고 Xamarin.Mac 프로젝트에이 동기화 합니다. 동기화가 자동으로 수행되지 않는 경우 Xcode로 돌아가서 다시 Mac용 Visual Studio로 돌아갑니다. 대부분 이렇게 하면 동기화 주기가 시작됩니다.


## <a name="adding-a-new-window-to-a-project"></a>새 창에서 프로젝트에 추가

기본 문서 창 외 Xamarin.Mac 응용 프로그램 기본 설정 또는 검사기 패널 등 사용자에 게 다른 유형의 windows 표시를 해야 합니다. 항상 사용 해야 프로젝트를 새 창에 추가 하는 경우는 **컨트롤러를 사용 하 여 Cocoa 창을** 옵션,이 인해 로드 프로세스를 창 고.xib에서 보다 쉽게 파일입니다.

새 창에 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 패드**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** .
2. 새 파일 대화 상자에서 선택 **Xamarin.Mac** > **컨트롤러를 사용 하 여 Cocoa 창을**:

    ![새 창 컨트롤러는 추가](xib-images/new01.png "는 새 창 컨트롤러 추가")
3. **이름**에 대해 `PreferencesWindow`를 입력하고 **새로 만들기** 단추를 클릭합니다.
4. 두 번 클릭 하 여 **PreferencesWindow.xib** 인터페이스 작성기에서 편집을 위해 열 파일입니다.

    [![편집 Xcode에서 창](xib-images/new02.png "편집 Xcode에서 창")](xib-images/new02-large.png)
5. 사용자 인터페이스를 디자인 합니다.

    [![창 레이아웃을 디자인](xib-images/new03.png "windows 레이아웃 디자인")](xib-images/new03-large.png)
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

다음 코드를 추가 **AppDelegate.cs** 프로그램 새 창을 표시 하려면:

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

`var preferences = new PreferencesWindowController ();` 줄.xib 파일에서 창을 로드 하 고이 확장 하는 창 컨트롤러의 새 인스턴스를 만듭니다. `preferences.Window.MakeKeyAndOrderFront (this);` 줄 사용자에 게 새 창을 표시 합니다.

코드를 실행 하 고 선택 하는 경우는 **기본 설정 중...**  에서 **응용 프로그램 메뉴**, 창이 표시 됩니다.

![샘플 응용 프로그램을 실행](xib-images/new04.png "샘플 응용 프로그램 실행")

Xamarin.Mac 응용 프로그램에서 Windows 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Windows](~/mac/user-interface/window.md) 설명서입니다.


## <a name="adding-a-new-view-to-a-project"></a>프로젝트에 새 뷰 추가

창의 디자인을 여러 개 더 관리 가능한 수치로.xib 파일으로 구분 하는 보다 쉽게 하는 경우가 있습니다. 예를 들어 주 창의 내용에 있는 도구 모음 항목을 선택 하는 경우 교환 같은 **기본 설정 창** 에 대 한 응답의 콘텐츠를 교환 또는 **소스 목록** 선택 합니다.

항상 사용 해야 프로젝트에 새 보기를 추가 하는 경우는 **컨트롤러와 뷰 Cocoa** 옵션,이 인해 로드 프로세스를 뷰는.xib에서 보다 쉽게 파일입니다.

새 보기를 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 패드**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** .
2. 새 파일 대화 상자에서 선택 **Xamarin.Mac** > **컨트롤러와 뷰 Cocoa**:

    ![새 뷰 추가](xib-images/view01.png "새 뷰 추가")
3. **이름**에 대해 `SubviewTable`를 입력하고 **새로 만들기** 단추를 클릭합니다.
4. 두 번 클릭 하 여 **SubviewTable.xib** 인터페이스 작성기에서 편집을 위해 열고 사용자 인터페이스를 디자인 하는 파일:

    [![Xcode에서 새 뷰 디자인](xib-images/view02.png "Xcode에서 새 뷰 디자인")](xib-images/view02-large.png)
5. 모든 필요한 작업 및 콘센트를 연결 합니다.
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

다음 편집는 **SubviewTable.cs** 다음 코드를 추가 하 고는 **AwakeFromNib** 로드 될 때 새 뷰를 채우는 파일:

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

열거형을 추적 하는 보기는 현재 디스플레이 프로젝트에 추가 합니다. 예를 들어 **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

보기에 사용 되 고 표시 되는 창의.xib 파일을 편집 합니다. 추가 **사용자 지정 보기** 역할을 할 컨테이너 보기에 대 한 C# 코드와 이라는 콘센트도 노출 하 여 메모리에 로드 되 면 `ViewContainer`:

[![필요한 콘센트 만들기](xib-images/view03.png "필요한 콘센트 만들기")](xib-images/view03-large.png)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

다음으로 새 보기를 표시 하는 창의.cs 파일을 편집 (예를 들어 **MainWindow.cs**) 하 고 다음 코드를 추가 합니다.

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

창의 컨테이너에.xib 파일에서 로드 한 경우 새 보기를 표시 해야 (의 **사용자 지정 보기** 위에 추가),이 코드 핸들 한 기존 보기를 제거 하 고 새 인증서에 대 한 교환 아웃 하기. 이미 보기를 표시 하는 경우 지금 제거 화면에서 확인 하는 데 찾습니다. 전달 된 보기 걸리는 옆에 (뷰 컨트롤러에서 로드 된) 콘텐츠 영역에 맞게 크기를 조정할 고 디스플레이 대 한 콘텐츠를 추가 합니다.

새 보기를 표시 하려면 다음 코드를 사용 합니다.

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

표시할 새 보기에 대 한 보기 컨트롤러의 새 인스턴스를 만들고, (지정 된 대로 프로젝트에 추가 하는 열거형에서) 해당 유형을 설정 및 사용 하 여이 고 `DisplaySubview` 메서드가 실제로 보기를 표시 하려면 창 클래스에 추가 합니다. 예:

[![샘플 응용 프로그램을 실행](xib-images/view04.png "샘플 응용 프로그램 실행")](xib-images/view04-large.png)

Xamarin.Mac 응용 프로그램에서 Windows 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Windows](~/mac/user-interface/window.md) 및 [대화 상자의](~/mac/user-interface/dialog.md) 설명서입니다.


## <a name="summary"></a>요약

이 문서에 자세히 살펴보고 Xamarin.Mac 응용 프로그램에서.xib 파일 작업을 수행 했습니다. 다양 한 유형 및.xib 파일의 용도를 응용 프로그램의 사용자 인터페이스를 만들고 인터페이스 작성기 및 C# 코드에서.xib 파일을 사용 하는 방법의 Xcode에서 파일 만들고.xib 유지 관리 하는 방법에 대해 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [MacImages (샘플)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [메뉴](~/mac/user-interface/menu.md)
- [대화 상자](~/mac/user-interface/dialog.md)
- [이미지 작업](~/mac/app-fundamentals/image.md)
- [Human Interface Guidelines macOS](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
