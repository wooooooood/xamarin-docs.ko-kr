---
title: Windows
description: 이 문서에서는 windows와 패널 Xamarin.Mac 응용 프로그램에서 작업을 수행 합니다. 만드는 창과 Xcode 및 스토리 보드와.xib 파일에서 로드 하기 및 이러한 작업을 프로그래밍 방식으로 인터페이스 작성기의 패널을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: f45bc69b74d98c7b9130f2caeaee91b184c38d87
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="windows"></a>Windows

_이 문서에서는 windows와 패널 Xamarin.Mac 응용 프로그램에서 작업을 수행 합니다. 만드는 창과 Xcode 및 스토리 보드와.xib 파일에서 로드 하기 및 이러한 작업을 프로그래밍 방식으로 인터페이스 작성기의 패널을 설명 합니다._

동일한 Windows에 액세스할 수 있는 Xamarin.Mac 응용 프로그램에서 C# 및.NET으로 작업을 하는 경우 및 개발자의 작업을 하는 패널 *Objective-c* 및 *Xcode* 않습니다. Xamarin.Mac Xcode에 직접 통합을 사용 하면 Xcode의 _인터페이스 작성기_ 만들기 및 유지 관리 창 및 패널 (또는 하려면 필요에 따라 C# 코드에서 직접 만들).

용도에 따라, Xamarin.Mac 응용 프로그램 관리 작업 및 정보를 표시 하 고 협력 하에 화면에 하나 이상의 창을 제공할 수 있습니다. 창 주 기능은 다음과 같습니다.

1. 어떤 보기 및 컨트롤의 영역을 제공 배치 하 고 관리할 수 있습니다.
2. 받아들이고 키보드 및 마우스 사용자 상호 작용에 대 한 응답으로 이벤트에 응답 합니다.

Windows는 모덜리스 상태 (예: 여러 문서를 한 번에 열을 가질 수 있는 텍스트 편집기)에 사용 하거나 (예: 응용 프로그램을 계속 하려면 먼저 해제 해야 하는 내보내기 대화 상자) 모달 수 있습니다.

패널은 특수 한 유형의 창 (기본의 서브 클래스 `NSWindow` 클래스), 보조 함수 형식 검사자 텍스트 및 색상 선택기 시스템 같은 유틸리티 창 같은 응용 프로그램에서 일반적으로 제공 하는 합니다.

[![](window-images/intro01.png "편집 Xcode에서 창")](window-images/intro01.png#lightbox)

이 문서에서는 Windows 및 패널 Xamarin.Mac 응용 프로그램에서 작업의 기본 사항을 하겠습니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 섹션으로이 문서에서 사용할 수 있는 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 명령 와이어 접속 C# 클래스 Objective-c 개체 및 UI 요소에 하는 데 사용 합니다.

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Windows 소개

위에서 설명 했 듯이 창 있는 보기 및 컨트롤 배치 하 고 관리할 수 있는 영역을 제공 및 사용자 상호 작용 (중 키보드 또는 마우스를 통해)에 따라 이벤트에 응답 합니다.

Apple에 따라 5 가지 있습니다 주 창의 macOS 응용 프로그램에서:

- **문서 창** -문서 창 스프레드시트 또는 텍스트 문서와 같은 사용자 파일 기반 데이터를 포함 합니다.
- **응용 프로그램 창** -응용 프로그램 창은 문서 기반 (예: Mac에서 일정 앱) 하지 않은 응용 프로그램의 주 창입니다.
- **패널** -다른 창 위에 배치 하 고 도구를 제공 하는 패널 또는 문서가 있는 동안 사용자가 작업할 수 있는 컨트롤을 엽니다. 일부 경우에는 패널이 반투명 될 수 있습니다 (예를 들어 큰 그래픽 작업).
- **대화** -대화 상자가 사용자 작업에 대 한 응답에 표시 하며 일반적으로 같은 방법으로 사용자가 작업을 완료할 수를 제공 합니다. 대화 상자 컴퓨터가 있어야 사용자의 응답을 닫을 수 있습니다. (참조 [대화 상자 사용](~/mac/user-interface/dialog.md))
- **경고** -경고는 특수 한 유형의 (예: 오류)는 심각한 문제가 발생 하면 나타나는 대화 상자 또는 (예: 파일을 삭제 하려면 준비) 경고로 표시 합니다. 경고 대화 상자 이기 때문에 문제가 닫을 수 전에 사용자 응답도 필요 합니다. (참조 [알림 작업](~/mac/user-interface/alert.md))

자세한 내용은 참조는 [에 대 한 Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Apple의 섹션 [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>주, 키 및 비활성 창

Xamarin.Mac 응용 프로그램의 창 모양과 어떻게는 사용자가 현재 이러한 상호 작용에 따라 다르게 동작할 수 있습니다. 주목할 만한 문서 또는 응용 프로그램 창에 포커스가 있는 현재 사용자의 주의 라고는 _주 창_합니다. 대부분의 경우에서이 창은 또한 수는 _키 창_ (사용자 입력을 현재 적용 되는 창). 그러나이 항상 이렇지는 않습니다, 그리고 예를 들어 색 선택 수 열려 있을 수 있는 문서 창을 (주 창 수)에 있는 항목의 상태를 변경 하는 사용자가 상호 작용 하는 키 창.

주 및 키 창 (별도 경우)는 항상 활성화, _비활성_ 열린 창이 전경에 없는 합니다. 예를 들어, 텍스트 편집기 응용 프로그램 둘 이상의 문서를 가지기 열려 한 번에 활성화 되어 주 창에만 다른 모든 사용자는 비활성화 됩니다. 

자세한 내용은 참조는 [에 대 한 Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Apple의 섹션 [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Windows 이름 지정

창 제목 표시줄 표시 하 고 제목 표시 되 면이 일반적으로 응용 프로그램의 이름, 창 (예: 관리자) 함수 또는 작업 중인 문서의 이름입니다. 플레이어에서 인식할 수 있는 한 문서와 함께 작동 하지 않는 하므로 일부 응용 프로그램 제목 표시줄을 표시 하지 않습니다.

Apple 다음 지침을 제안 합니다.

- 비 문서 주 창 제목에 대 한 응용 프로그램 이름을 사용 합니다. 
- 새 문서 창 이름을 `untitled`합니다. 제목에 숫자가 추가 하지 않는 첫 번째 새 문서에 대 한 (예: `untitled 1`). 사용자를 저장 하 고 첫 제목 하기 전에 다른 새 문서를 만드는 경우 해당 창 호출 `untitled 2`, `untitled 3`등입니다.

자세한 내용은 참조는 [명명 Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) Apple의 섹션 [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>전체 화면 Windows

MacOS 등의 응용 프로그램의 창이 넘어가면 전체 화면 숨기기 (있음 화면 위쪽에 커서를 이동 하 여 표시할 수 있습니다) 응용 프로그램 메뉴 모음을 포함 한 모든 작업을 제공 하려면 무료 상호가 콘텐츠입니다.

다음 지침을 제안 하는 Apple:

- 전체 화면을 창에 대 한 적합 한지 확인 합니다. 간략 한 상호 작용 (예: 계산기)을 제공 하는 응용 프로그램 전체 화면 모드를 제공 하지 않아야 합니다.
- 전체 화면 작업이 필요한 것 도구 모음을 표시 합니다. 일반적으로 전체 화면 모드에 있는 동안 도구 모음이 숨겨져 있습니다.
- 전체 화면 창 작업을 완료 해야 할 모든 기능 사용자가 있어야 합니다.
- 가능 하면 사용자가 전체 화면 창에 있는 동안 Finder 상호 작용을 하지 마십시오.
- 증가 화면 공간 없이 활용 주 작업에서 포커스를 이동 합니다.

자세한 내용은 참조는 [전체 화면 Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) Apple의 섹션 [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>패널

패널은 컨트롤 및 활성 문서 또는 선택 항목 (예: 시스템 색 선택)에 영향을 주는 옵션을 포함 하는 보조 창입니다.

[![](window-images/panel01.png "색 패널")](window-images/panel01.png#lightbox)

패널 수 _응용 프로그램별_ 또는 _시스템 전반에 걸쳐_합니다. 응용 프로그램별 패널 응용 프로그램의 문서 창 맨 위에 위치 및 응용 프로그램은 백그라운드에서 때 사라집니다. 시스템 수준 패널 (같은 **글꼴** 패널), 응용 프로그램에 관계 없이 모든 열린 창 위에 float입니다. 

다음 지침을 제안 하는 Apple:

- 일반적으로 표준 패널을 사용 하 여, 제한적으로 사용 하 고 그래픽을 많이 사용 작업에 대 한 투명 한 패널만 사용 해야 합니다.
- 사용자에 게 쉽게 액세스할 수 있도록 중요 한 컨트롤이 또는 직접 자신의 작업에 영향을 주는 정보 패널을 사용 하는 것이 좋습니다.
- 필요에 따라 패널 표시 / 숨기기.
- 패널에는 제목 표시줄 항상 포함 해야 합니다.
- 패널 활성 최소화 단추를 포함 해야 합니다.

#### <a name="inspectors"></a>검사기

대부분의 요즘 macOS 응용 프로그램이 있는 보조 컨트롤 및 활성 문서 또는 선택 영역에 영향을 주는 옵션 _검사자_ 주 창에 속하는 (같은 **페이지** 아래에 표시 된 응용 프로그램), 대신 패널 창을 사용 하 여:

[![](window-images/panel02.png "예제 검사기")](window-images/panel02.png#lightbox)

자세한 내용은 참조는 [패널](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) Apple의 섹션 [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) 및 [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) 는 의전체구현에대한샘플응용프로그램**검사기 인터페이스** Xamarin.Mac 응용 프로그램에서입니다.

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>만들기 및 Xcode에서 Windows 유지 관리

새 Xamarin.Mac Cocoa 응용 프로그램을 만들 때 기본적으로 표준, 빈 창이 얻을 수 있습니다. 에 정의 되어 있는이 windows는 `.storyboard` 파일은 프로젝트에 자동으로 포함 합니다. windows 디자인에 맞게 편집 하는 **솔루션 탐색기**, 두 번 클릭 하 고 `Main.storyboard` 파일:

[![](window-images/edit01.png "주 스토리 보드를 선택합니다.")](window-images/edit01.png#lightbox)

Xcode의 인터페이스 작성기의 창 디자인을 열립니다.

[![](window-images/edit02.png "Xcode에서 UI를 편집합니다.")](window-images/edit02.png#lightbox)

에 **특성 검사기**를 정의 하 고 사용자가 창을 제어 하는 데 사용할 수 있는 속성이 여러:

- **제목** -창의 제목 표시줄에 표시 되는 텍스트입니다.
- **자동 저장** -이것이 _키_ 의 위치 및 설정은 자동으로 저장 하는 경우 창 ID에 사용할 합니다.
- **제목 표시줄** -창 제목 표시줄을 표시 합니다.
- **제목 및 도구 모음 통합** -창에는 도구 모음, 제목 표시줄의 일부가 될 해야 하는 경우.
- **콘텐츠 보기 크기의 전체** -콘텐츠 영역 창 제목 표시줄 아래에 표시 될 수 있습니다.
- **그림자** -창을 그림자가 있습니다.
- **질감** -질감된 windows (예: 표현) 효과 사용할 수 있으며, 해당 본문의 아무 곳 이나 끌어서 이동할 수 있습니다.
- **닫기** -창을 닫기 단추가 있어야 합니다.
- **최소화** -창을 최소화 단추 있어야 합니다.
- **크기 조정** -창 크기 조정 컨트롤 있어야 합니다.
- **도구 모음 단추** -창 도구 모음 표시/숨기기 단추 있어야 합니다.
- **복원 가능한** -창의 위치 및 설정은 자동으로 저장 되 고 복원 됩니다.
- **표시에서 실행** -창이 자동으로 때 표시 되는 `.xib` 파일을 로드 합니다.
- **비활성화 하면 숨기기** -응용 프로그램이 백그라운드 들어가면 창이 숨겨진 됩니다.
- **닫힙니다 릴리스** -닫을 때 메모리에서 제거 된 창입니다.
- **항상 표시 도구 설명** -도구 설명을 지속적으로 표시 됩니다.
- **보기 루프를 다시 계산** -창이 그리기 전에 다시 계산 하는 보기 순서입니다.
- **공간**, **Exposé** 및 **Cycling** -창 이러한 macOS 환경에서 작동 하는 방식을 모두 정의 합니다. 
- **전체 화면을** -경우이 창을 전체 화면 모드를 입력할 수를 결정 합니다. 
- **애니메이션** -창에 대해 사용할 수 있는 애니메이션의 종류를 제어 합니다.
- **모양** -창의 모양을 제어 합니다. 지금은 바다색 모양 하나만 있습니다.

Apple의 참조 [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) 및 [NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) 자세한 세부 정보에 대 한 설명서입니다.

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>기본 크기 및 위치를 설정합니다.

로 전환 하는 사용자가 창의 초기 위치를 설정 하 고의 크기를 제어 하는 **크기 검사기**:

[![](window-images/edit07.png "기본 크기 및 위치")](window-images/edit07.png#lightbox)

여기에서 창의 초기 크기, 최소 및 최대 크기를 지정, 화면에서 처음 위치를 설정를 창 주변의 테두리를 제어 합니다.

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>주 창 사용자 지정 컨트롤러 설정

UI 요소를 C# 코드 노출 하기 위해 콘센트 및 동작을 만들 수 있으려면 Xamarin.Mac 앱 사용자 지정 창 컨트롤러를 사용 해야 합니다.

다음을 수행합니다.

1. Xcode의 인터페이스 작성기에서 응용 프로그램의 스토리 보드를 엽니다.
2. 선택 된 `NSWindowController` 디자인 화면에서 합니다.
3. 전환 하는 **Identity 관리자** 보기 및 입력 `WindowController` 로 **클래스 이름**: 

    [![](window-images/windowcontroller01.png "설정 클래스 이름")](window-images/windowcontroller01.png#lightbox)
4. 변경 내용을 저장 하 고 동기화 하는 Mac에 대 한 Visual Studio로 돌아갑니다.
5. A `WindowController.cs` 파일을 프로젝트에 추가 됩니다는 **솔루션 탐색기** Mac 용 Visual Studio에서: 

    [![](window-images/windowcontroller02.png "Windows 컨트롤러를 선택 하면")](window-images/windowcontroller02.png#lightbox)
6. Xcode의 인터페이스 작성기에서 스토리 보드를 다시 엽니다.
7. `WindowController.h` 파일은 사용 하기 위해 사용할 수 있습니다. 

    [![](window-images/windowcontroller03.png "WindowController.h 파일 편집")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>UI 요소를 추가합니다.

창의 콘텐츠를 정의 하려면에서 컨트롤을 끌어는 **라이브러리 검사기** 에 **인터페이스 편집기**합니다. 참조 하십시오 우리의 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 인터페이스 작성기를 사용 하 여 만들고 컨트롤을 사용 하는 방법에 대 한 자세한 정보에 대 한 설명서입니다.

예를 들어 보겠습니다에서 도구 모음을 끌어는 **라이브러리 검사기** 의 창에는 **인터페이스 편집기**:

[![](window-images/edit03.png "라이브러리에서 도구 모음을 선택 하면")](window-images/edit03.png#lightbox)

다음으로 끌어는 **텍스트 보기** 의 도구 모음 아래 영역을 채우는 크기:

[![](window-images/edit04.png "텍스트 뷰 추가")](window-images/edit04.png#lightbox)

할 것 이므로 **텍스트 보기** 창의 크기 변화 함에 따라 확장 해 서 축소으로 전환 해 보겠습니다는 **제약 조건 편집기** 다음과 같은 제약 조건이 추가:

[![](window-images/edit05.png "제약 조건 편집")](window-images/edit05.png#lightbox)

클릭 하 여는 대 한 **빨간색 I-빔** 편집기의 맨 위 및 클릭 하면 **4 제약 조건 추가**, 좌표는 주어진된 X, Y에 집중 하 고 확대 또는 가로 및 세로로 축소 하려면 텍스트 보기 한다는 것으로 창 크기가 조정 됩니다.

마지막으로, 보겠습니다 노출는 **텍스트 보기** 를 사용 하는 코드는 **콘센트** (선택는 `ViewController.h` 파일):

[![](window-images/edit06.png "콘센트에 연결 구성")](window-images/edit06.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 다시 전환 합니다.

작업에 대 한 자세한 내용은 **콘센트** 및 **작업**를 참조 하십시오 우리의 [콘센트 및 작업](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 설명서.

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>표준 창 워크플로

만들고 Xamarin.Mac 응용 프로그램에서 작동 하는 모든 창에 대 한 프로세스는 기본적으로 어떤 방금 했던 위에 동일.

1. 기본 프로젝트에 자동으로 추가 되지 않은 새 창에 대 한 새 창 정의 프로젝트에 추가 합니다. 아래에서 자세히 설명 합니다.
2. 두 번 클릭 하 여 `Main.storyboard` 창 디자인 Xcode의 인터페이스 작성기에서 편집을 위해 열 파일입니다.
3. 새 창 사용자 인터페이스 디자인에 끌어서 창을 사용 하 여 주 창을 후크하고 _Segues_ (자세한 내용은 참조는 [Segues](~/mac/platform/storyboards/indepth.md#Segues) 의 섹션 우리의 [스토리보드와작업](~/mac/platform/storyboards/indepth.md) 설명서).
3. 모든 필요한 창 속성을 설정는 **특성 검사기** 및 **크기 검사기**합니다.
4. 사용자 인터페이스를 만들고 구성에서 해당 하는 데 필요한 컨트롤을 끌어는 **특성 검사기**합니다.
5. 사용 하 여는 **크기 검사기** UI 요소에 대 한 크기 조정을 처리 하도록 합니다.
6. 창의 UI 요소를 통해 C# 코드를 노출 **콘센트** 및 **동작**합니다.
7. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 다시 전환 합니다.

이제 만든 기본 창 했으므로에서 살펴보게 일반적인 프로세스 응용 프로그램에서는 windows와 함께 작업 하는 경우 Xamarin.Mac 합니다. 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>기본 창 표시

기본적으로 새 Xamarin.Mac 응용 프로그램은 자동으로 표시에 정의 된 창 고 `MainWindow.xib` 시작 될 때 파일:

[![](window-images/display01.png "실행 하는 예제 창")](window-images/display01.png#lightbox)

이제 기본 도구 모음 포함 되어 해당 창 위의의 디자인을 수정한 이후 및 **텍스트 보기** 제어 합니다. 다음 섹션에 `Info.plist` 파일은이 창을 표시 합니다.

[![](window-images/display00.png "Info.plist 편집")](window-images/display00.png#lightbox)

**주 인터페이스** 드롭다운은 기본 응용 프로그램 UI로 사용 될 스토리 보드를 선택 하는 데 사용 됩니다 (이 경우 `Main.storyboard`).

뷰 컨트롤러의 기본 뷰) (함께 표시 되는 기본 Windows를 제어 하는 프로젝트에 자동으로 추가 됩니다. 에 정의 되어는 `ViewController.cs` 파일을 연결할는 **파일의 소유자만** 아래 인터페이스 작성기에는 **Identity 관리자**:

[![](window-images/display02.png "파일의 소유자를 설정합니다.")](window-images/display02.png#lightbox)

이 창에 대 한 같은의 제목을 갖도록 `untitled` 처음 열 때 이제 재정의 `ViewWillAppear` 에서 메서드는 `ViewController.cs` 에 다음과 같습니다:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> 창의의 값을 설정 하는 것 `Title` 속성에는 `ViewWillAppear` 메서드 대신는 `ViewDidLoad` 메서드 뷰는 메모리에 로드 될 수, 하는 동안이 아직 완전히 인스턴스화되지 때문에 있습니다. 에 액세스 하려고 할 경우는 `Title` 속성에는 `ViewDidLoad` 는 얻을 수 있는 메서드는 `null` 예외 창 않은 생성 하 고 유선 접속 속성에 아직 때문입니다.

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>프로그래밍 방식으로 창 닫기

프로그래밍 방식으로 Xamarin.Mac 응용 프로그램에서 창의 닫기 하고자 하는 경우가 있을 수 있습니다, 해야 사용자가 창의 클릭 **닫습니다** 단추 또는 메뉴 항목을 사용 하 여 합니다. macOS 제공를 닫으려면는 두 가지 방법으로 `NSWindow` 프로그래밍 방식으로: `PerformClose` 및 `Close`합니다.

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

호출의 `PerformClose` 의 메서드는 `NSWindow` 는 창을 클릭 하 고 사용자를 시뮬레이션 **닫기** 일시적으로 단추를 강조 표시 하 고 다음 창을 닫을 수 있는 여는 단추입니다.

응용 프로그램을 구현 하는 경우는 `NSWindow`의 `WillClose` 이벤트 창을 닫기 전에 발생 합니다. 이벤트를 반환 하는 경우 `false`, 다음 창 닫히지 것입니다. 창 없는 경우는 **닫기** 단추 하거나 닫을 수 없습니다. 어떤 이유로 든 OS 경고 소리 내보냅니다.

예를 들어:

```csharp
MyWindow.PerformClose(this);
```

시도를 닫으려면는 `MyWindow` `NSWindow` 인스턴스. 성공 했을 경우 창이 닫힙니다, 그리고 다른 경고 소리를 내보냅니다 및 열린 채로 합니다.

<a name="Close" />

### <a name="close"></a>닫기

호출 된 `Close` 의 메서드는 `NSWindow` 는 창을 클릭 하 고 사용자를 시뮬레이션 하지 않습니다 **닫기** 단추 일시적으로 단추를 강조 표시를 하 여 그 단순히 창을 닫습니다.

닫을 수 있게 표시 되도록 한 창 없는 및 `NSWindowWillCloseNotification` 알림 닫히는 창에 대 한 기본 알림 센터에 게시 됩니다.

`Close` 방법은 두 가지 중요 한 면에서 차이가 `PerformClose` 메서드:

1. 발생 시키는 시도 하지 않습니다는 `WillClose` 이벤트입니다.
2. 클릭 하는 사용자를 시뮬레이션 하지 않습니다는 **닫기** 일시적으로 단추를 강조 표시 하 여 단추입니다.

예를 들어:

```csharp
MyWindow.Close();
```

닫을 때는 `MyWindow` `NSWindow` 인스턴스.

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>수정 된 Windows 콘텐츠

Apple가 알려 주는 하는 방법을 macOS 등에서 제공 하는 창의 내용을 (`NSWindow`) 저장 되어야 하 고 사용자가 수정 되었습니다. 에 작은 검정색 점이 표시 됩니다는 창 수정 된 내용이 있으면, **닫기** 위젯:

[![](window-images/close01.png "수정 된 표식으로 창")](window-images/close01.png#lightbox)

Mac 앱이 저장 하는 동안 창의 콘텐츠를 변경, 표시 하도록 사용자가 창을 닫거나 종료 하려고 하는 경우는 [대화 상자](~/mac/user-interface/dialog.md) 또는 [모달 시트](~/mac/user-interface/dialog.md) 자신의 변경 내용을 저장 하 고 사용자 첫 번째:

[![](window-images/close02.png "창이 닫힐 때 표시 되 고 시트 저장")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>수정 된으로 창을 표시합니다.

콘텐츠를 수정 것으로 창을 표시 하려면 다음 코드를 사용 합니다.

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

하 고 변경을 저장 한 후 사용 하 여 수정된 플래그 선택을 취소 합니다.

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>변경 내용을 저장 하는 창을 닫기 전에

서브 클래스를 만든 할 미리 수정 된 콘텐츠를 저장 하려면 창을 닫고 수 있도록 사용자를 시청 하려면 `NSWindowDelegate` 재정의 하 고 해당 `WindowShouldClose` 메서드. 예를 들어:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class EditorWidowDelegate : NSWindowDelegate
    {
        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public EditorWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            // is the window dirty?
            if (Window.DocumentEdited) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Critical,
                    InformativeText = "Save changes to document before closing window?",
                    MessageText = "Save Document",
                };
                alert.AddButton ("Save");
                alert.AddButton ("Lose Changes");
                alert.AddButton ("Cancel");
                var result = alert.RunSheetModal (Window);

                // Take action based on resu;t
                switch (result) {
                case 1000:
                    // Grab controller
                    var viewController = Window.ContentViewController as ViewController;

                    // Already saved?
                    if (Window.RepresentedUrl != null) {
                        var path = Window.RepresentedUrl.Path;

                        // Save changes to file
                        File.WriteAllText (path, viewController.Text);
                        return true;
                    } else {
                        var dlg = new NSSavePanel ();
                        dlg.Title = "Save Document";
                        dlg.BeginSheet (Window, (rslt) => {
                            // File selected?
                            if (rslt == 1) {
                                var path = dlg.Url.Path;
                                File.WriteAllText (path, viewController.Text);
                                Window.DocumentEdited = false;
                                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                                viewController.View.Window.RepresentedUrl = dlg.Url;
                                Window.Close();
                            }
                        });
                        return true;
                    }
                    return false;
                case 1001:
                    // Lose Changes
                    return true;
                case 1002:
                    // Cancel
                    return false;
                }
            }

            return true;
        }
        #endregion
    }
}
```

다음 코드를 사용 하 여이 대리자의 인스턴스를 사용자가 창을 연결할:

```csharp
// Set delegate
Window.Delegate = new EditorWidowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>응용 프로그램을 닫기 전에 변경 내용을 저장

마지막으로 수정 된 콘텐츠를 포함 한 종료 하기 전에 변경 내용을 저장 하는 데 사용할 수 있는 창을 모두 Xamarin.Mac 앱 확인 해야 합니다. 이 작업을 수행 하려면 편집 프로그램 `AppDelegate.cs` 파일, 재정의 `ApplicationShouldTerminate` 메서드 다음과 비슷하게 표시 및:

```csharp
public override NSApplicationTerminateReply ApplicationShouldTerminate (NSApplication sender)
{
    // See if any window needs to be saved first
    foreach (NSWindow window in NSApplication.SharedApplication.Windows) {
        if (window.Delegate != null && !window.Delegate.WindowShouldClose (this)) {
            // Did the window terminate the close?
            return NSApplicationTerminateReply.Cancel;
        }
    }

    // Allow normal termination
    return NSApplicationTerminateReply.Now;
}
```

<a name="Working_with_Multiple_Windows" />

## <a name="working-with-multiple-windows"></a>여러 창 작업

대부분의 문서를 기반으로 Mac 응용 프로그램 동시에 여러 문서를 편집할 수 있습니다. 예를 들어, 텍스트 편집기는 여러 텍스트 파일 같은 시간에 편집을 위해 열려 있을 수 있습니다. 기본적으로 새 Xamarin.Mac 응용 프로그램에는 **파일** 포함 된 메뉴는 **새로** 항목 자동으로 유선 업은 `newDocument:` **동작**합니다.

이 새 항목을 활성화 하 고 사용자를 한 번에 여러 문서를 편집 하 여 주 창의 여러 복사본을 열도록 허용을 하겠습니다.

편집이 `AppDelegate.cs` 파일을 다음 컴퓨팅 된 속성 추가:

```csharp
public int UntitledWindowCount { get; set;} =1;
```

이 피드백 (위에 설명 된 대로 Apple의 지침) 당 사용자에 게 제공할 수 있도록 저장 되지 않은 파일의 수를 추적을 사용 합니다.

다음으로, 다음 메서드를 추가 합니다.

```csharp
[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

이 코드 창 Controller의 새로운 버전을 만듭니다, 그리고 새 창이 로드, Main 및 키 창이 있고 제목을 설정 합니다. 이 응용 프로그램을 실행 하 고 선택 하는 경우 이제 **새로** 에서 **파일** 새 편집기 창이 열리고 표시 메뉴:

[![](window-images/display04.png "제목이 없는 새 창 추가")](window-images/display04.png#lightbox)

에서는 열 경우는 **Windows** 메뉴에서 응용 프로그램은 자동으로 추적 하 고이 열려 있는 창을 처리 볼 수 있습니다.

[![](window-images/display05.png "Widows 메뉴")](window-images/display05.png#lightbox)

Xamarin.Mac 응용 프로그램에서 메뉴 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [메뉴 작업](~/mac/user-interface/menu.md) 설명서입니다.

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>현재 활성 창의 가져오기

Xamarin.Mac 응용 프로그램의 여러 창을 (문서)를 열 수 있는 경우 하는 경우 현재, 맨 위 창 (키 창)를 가져올 해야 합니다. 다음 코드에는 키 창을 반환 합니다.

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

모든 클래스 또는 키, 현재 창에 액세스 하는 메서드를 호출할 수 있습니다. 창이 현재 열려 반환 `null`합니다.

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>모든 응용 프로그램 창에 액세스

경우도 Xamarin.Mac 앱 현재 열기에이 windows의 모든 액세스 해야 합니다. 예를 들어 있는지 파일을 열려는 사용자가 기존 창에서 열려 있는입니다.

`NSApplication.SharedApplication` 유지 관리는 `Windows` 응용 프로그램에서 열려 있는 모든 창의 배열을 포함 하는 속성입니다. 응용 프로그램의 현재 windows의 모든 액세스 하려면이 배열을 반복할 수 있습니다. 예를 들어:

```csharp
// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

예제 코드에 반환 된 각 창에 사용자 지정을 캐스팅 하는 것 `ViewController` 앱 및 테스트할 사용자 지정 값 클래스 `Path` 사용자가을 열고 파일의 경로 대 한 속성입니다. 파일이 이미 열려 있으면 앞에 해당 창으로 전환는 했습니다.

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>코드에서 창 크기 조정

코드의 창 크기를 조정 하려면 응용 프로그램이 필요로 하는 경우 경우가 있습니다. 조정 크기와 위치를 창의 `Frame` 속성입니다. 창의 크기를 조정 하는 경우에 일반적으로 macOS의 좌표 시스템으로 인해 동일한 위치에는 창을 항상의 출처를 조정할 수도 필요 합니다.

와 달리 iOS 여기서 왼쪽 위 모퉁이 나타냅니다 (0, 0), macOS에서는 사용 수치 연산 좌표계 화면의 하단 왼쪽 모서리 쪽 (0, 0)을 나타냅니다. IOS에서 좌표는 오른쪽 방향으로 아래쪽으로 이동 하면 늘립니다. 좌표, macOS에서 위쪽으로 오른쪽 값을 증가합니다. 

다음 예제 코드는 창의 크기를 조정 합니다.

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> 창 크기 및 코드의 위치를 조정 하는 경우 인터페이스 작성기에서 설정 된 최소 및 최대 크기를 준수 하는지 확인 해야 합니다. 이 자동으로 정책이 적용 되지 않습니다 및 크거나 이러한 한도 보다 작은 창을 만들 수 있습니다.

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>창 크기가 변경 모니터링

경우도 Xamarin.Mac 앱 내에서 창 크기의 변화를 모니터링 해야 합니다. 예를 들어, 새 크기에 맞게 콘텐츠 다시 그리게 합니다.

크기 변화를 모니터링 하려면 먼저 창 컨트롤러 Xcode의 인터페이스 작성기에 대 한 사용자 지정 클래스를 할당 했는지 확인 합니다. 예를 들어 `MasterWindowController` 다음에서:

[![](window-images/resize01.png "Identity 관리자")](window-images/resize01.png#lightbox)

다음으로, 사용자 지정 창 컨트롤러 클래스 및 모니터를 편집할는 `DidResize` 라이브 크기 변경의 알림을 받을 수 있는 컨트롤러의 창에는 이벤트입니다. 예를 들어:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

사용할 수 있습니다는 `DidEndLiveResize` 만 알림을 받으려면 사용자가 창의 크기를 변경한 후 이벤트입니다. 예를 들면 다음과 같습니다.

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

        Window.DidEndLiveResize += (sender, e) => {
        // Do something after the user's finished resizing
        // the window
    };
}
```

<a name="Setting_a_Window’s_Title_and_Represented_File" />

## <a name="setting-a-windows-title-and-represented-file"></a>직함 창 설정 및 파일 표시

문서를 나타내는 창을 작업할 때 `NSWindow` 에 `DocumentEdited` 속성이로 설정 되는 경우 `true` 닫기 단추를 표시 파일 수정 되었는지 및 닫기 전에 저장 해야 사용자에 게 부여에서 작은 점이 표시 됩니다.

편집이 `ViewController.cs` 파일을 다음과 같이 변경 합니다.

```csharp
public bool DocumentEdited {
    get { return View.Window.DocumentEdited; }
    set { View.Window.DocumentEdited = value; }
}
...

public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";

    View.Window.WillClose += (sender, e) => {
        // is the window dirty?
        if (DocumentEdited) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to give the user the ability to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    };
}

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Show when the document is edited
    DocumentEditor.TextDidChange += (sender, e) => {
        // Mark the document as dirty
        DocumentEdited = true;
    };

    // Overriding this delegate is required to monitor the TextDidChange event
    DocumentEditor.ShouldChangeTextInRanges += (NSTextView view, NSValue[] values, string[] replacements) => {
        return true;
    };

}
```

에서는 또한 모니터링 하는 `WillClose` 창과의 상태를 확인 하는 중에 이벤트는 `DocumentEdited` 속성입니다. 이 경우 `true` 사용자에 게 파일에 변경 내용을 저장 하는 기능을 제공 해야 합니다. 앱을 실행 하 고 텍스트를 입력는 점이 표시 됩니다.

[![](window-images/file01.png "변경 된 창")](window-images/file01.png#lightbox)

창을 닫으려면 하려고 하면 경고를 가져올 있습니다.

[![](window-images/file02.png "저장 표시 대화 상자")](window-images/file02.png#lightbox)

파일의 창의 제목을 설정할 수 있습니다 파일에서 문서를 로드 하는 것 경우 사용 하 여 이름을 `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` 메서드 (있다고 가정 `path` 열리는 파일을 나타내는 문자열)입니다. 또한 사용 하 여 파일의 URL을 설정할 수 있습니다는 `window.RepresentedUrl = url;` 메서드.

URL가 운영 체제에서 알려진 파일 형식을 가리키는의 아이콘 제목 표시줄에 나타납니다. 마우스 오른쪽 단추로 아이콘을 클릭 하는 경우에 파일의 경로를 표시 됩니다.

편집 된 `AppDelegate.cs` 파일을 다음 메서드를 추가 합니다.

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = true;
    dlg.CanChooseDirectories = false;

    if (dlg.RunModal () == 1) {
        // Nab the first file
        var url = dlg.Urls [0];

        if (url != null) {
            var path = url.Path;

            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow(this);

            // Load the text into the window
            var viewController = controller.Window.ContentViewController as ViewController;
            viewController.Text = File.ReadAllText(path);
                    viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
            viewController.View.Window.RepresentedUrl = url;

        }
    }
}
```

이제 앱을 실행 하는 경우 선택 **열기...**  에서 **파일** 메뉴를 선택 하는 텍스트 파일에서 **열고** 대화 상자를 엽니다.

[![](window-images/file03.png "열기 대화 상자")](window-images/file03.png#lightbox)

파일이 표시 되 고 제목 파일 아이콘을 사용 하 여 설정 됩니다.

[![](window-images/file04.png "파일의 내용을 로드")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>새 창에서 프로젝트에 추가

기본 문서 창 외 Xamarin.Mac 응용 프로그램 기본 설정 또는 검사기 패널 등 사용자에 게 다른 유형의 windows 표시를 해야 합니다.

새 창에 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**, 두 번 클릭은 `Main.storyboard` Xcode의 인터페이스 작성기에서 편집을 위해 열 파일입니다.
2. 새 끌어 **창 컨트롤러** 에서 **라이브러리** 놓습니다는 **디자인 화면**:

    [![](window-images/new01.png "라이브러리에 새 창 컨트롤러를 선택 하면")](window-images/new01.png#lightbox)
3. 에 **Identity 관리자**, 입력 `PreferencesWindow` 에 대 한는 **스토리 보드 ID**: 

    [![](window-images/new02.png "스토리 보드 ID 설정")](window-images/new02.png#lightbox)
5. 사용자 인터페이스를 디자인 합니다. 

    [![](window-images/new03.png "UI 디자인")](window-images/new03.png#lightbox)
6. 응용 프로그램 메뉴를 열고 (`MacWindows`)을 선택 **기본 설정 중...** , 컨트롤을 클릭 하 고 새 창으로 끌어 옵니다. 

    [![](window-images/new05.png "segue 만들기")](window-images/new05.png#lightbox)
7. 선택 **표시** 팝업 메뉴에서 합니다.
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

코드를 실행 하 고 선택 하는 경우는 **기본 설정 중...**  에서 **응용 프로그램 메뉴**, 창이 표시 됩니다.

[![](window-images/new04.png "샘플 기본 설정 메뉴")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>패널 사용

이 문서의 시작 부분에 나와 있는 것 처럼 패널 다른 창 위에 배치 하 고 도구 또는 문서가 열려 있는 동안 사용자가 작업할 수 있는 컨트롤을 제공 합니다. 

만들고 Xamarin.Mac 응용 프로그램에서 사용 하는 창의 다른 모든 형식과 마찬가지로 프로세스는 기본적으로 동일 합니다.

1. 새 창 정의 프로젝트에 추가 합니다.
2. 두 번 클릭 하 여 `.xib` 창 디자인 Xcode의 인터페이스 작성기에서 편집을 위해 열 파일입니다.
2. 모든 필요한 창 속성을 설정는 **특성 검사기** 및 **크기 검사기**합니다.
4. 사용자 인터페이스를 만들고 구성에서 해당 하는 데 필요한 컨트롤을 끌어는 **특성 검사기**합니다.
5. 사용 하 여는 **크기 검사기** UI 요소에 대 한 크기 조정을 처리 하도록 합니다.
6. 창의 UI 요소를 통해 C# 코드를 노출 **콘센트** 및 **동작**합니다.
7. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 다시 전환 합니다.

에 **특성 검사기**, 특정 패널에 다음 옵션을 사용할 수:

[![](window-images/panel03.png "특성 검사기")](window-images/panel03.png#lightbox)

- **스타일** -에서 패널의 스타일을 조정할 수 있도록: 일반 패널 (표준 창 아래), 유틸리티 패널 (더 작은 제목 표시줄에) HUD 패널 (반투명 제목 표시줄의 배경의 일부인).
- **비 활성 중** -에 결정 패널 키 창이 됩니다.
- **Modal 문서** -경우 문서 모달 패널의 응용 프로그램의 창 위에 배치만 됩니다, 다른 모든 부동 합니다.


새 패널을 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** .
2. 새 파일 대화 상자에서 선택 **Xamarin.Mac** > **컨트롤러를 사용 하 여 Cocoa 창을**:

    [![](window-images/panels00.png "새 창 컨트롤러 추가")](window-images/panels00.png#lightbox)
3. **이름**에 대해 `DocumentPanel`를 입력하고 **새로 만들기** 단추를 클릭합니다.
4. 두 번 클릭 하 여 `DocumentPanel.xib` 인터페이스 작성기에서 편집을 위해 열 파일입니다. 

    [![](window-images/new02.png "pannel 편집")](window-images/new02.png#lightbox)
5. 패널을 끌어서 기존 창을 삭제는 **라이브러리 관리자** 에서 여는 **인터페이스 편집기**: 

    [![](window-images/panels01.png "기존 창 삭제")](window-images/panels01.png#lightbox)
6. 패널을 최대 후크는 **파일의 소유자만*-**창*- **콘센트**: 

    [![](window-images/panels02.png "패널을 실시간으로 끌어")](window-images/panels02.png#lightbox)
7. 전환 하는 **Identity 관리자** 패널의 클래스 설정 하 고 `DocumentPanel`: 

    [![](window-images/panels03.png "패널의 클래스를 설정합니다.")](window-images/panels03.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.
7. 편집 된 `DocumentPanel.cs` 파일을 다음에 클래스 정의 변경 합니다. 

    `public partial class DocumentPanel : NSPanel`
8. 파일의 변경 내용을 저장합니다.

편집 된 `AppDelegate.cs` 파일을 확인는 `DidFinishLaunching` 다음과 같은 메서드 모양을:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

응용 프로그램을 실행 하는 경우 패널이 표시 됩니다.

[![](window-images/panels04.png "실행 중인 응용 프로그램의 패널")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> 패널 Windows Apple에 의해 사용이 중단 된 및로 대체 해야 **검사자 인터페이스**합니다. 만드는 전체 예를 보려면는 **검사기** Xamarin.Mac 앱에서 참조 하십시오 우리의 [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) 샘플 응용 프로그램입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서는 Windows 및 패널 Xamarin.Mac 응용 프로그램에서 작업에 대해 자세히를 수행 했습니다. 다양 한 유형을 보여 준다는 사실을 알았습니다 하 고 창 및 패널, 만들고 창과 Xcode의 인터페이스 작성기의 패널을 유지 관리 하는 방법 및 C# 코드에서 Windows 및 패널 사용 방법을 사용 합니다.

## <a name="related-links"></a>관련 링크

- [MacWindows (샘플)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (샘플)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [메뉴 작업](~/mac/user-interface/menu.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
