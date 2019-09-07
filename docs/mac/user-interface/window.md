---
title: Xamarin.ios의 Windows
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 창과 패널을 사용 하는 방법을 설명 합니다. Xcode 및 Interface Builder에서 창과 패널을 만들고 storyboard와 xib 파일에서 로드 하 여 프로그래밍 방식으로 작업 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: df623efcc1da617ac6b700b42d3ac058dea817ca
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772647"
---
# <a name="windows-in-xamarinmac"></a>Xamarin.ios의 Windows

_이 문서에서는 Xamarin.ios 응용 프로그램에서 창과 패널을 사용 하는 방법을 설명 합니다. Xcode 및 Interface Builder에서 창과 패널을 만들고 storyboard와 xib 파일에서 로드 하 여 프로그래밍 방식으로 작업 하는 방법을 설명 합니다._

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우 *목표-C* 및 *Xcode* 에서 작업 하는 개발자가 동일한 창과 패널에 액세스할 수 있습니다. Xamarin.ios는 Xcode와 직접 통합 되므로 Xcode의 _Interface Builder_ 를 사용 하 여 Windows 및 패널을 만들고 유지 관리 하거나 선택적으로 코드에서 C# 직접 만들 수 있습니다.

용도에 따라 Xamarin.ios 응용 프로그램은 표시 되 고 작동 하는 정보를 관리 하 고 조정할 수 있는 하나 이상의 창을 화면에 표시할 수 있습니다. 창의 주요 기능은 다음과 같습니다.

1. 보기 및 컨트롤을 배치 하 고 관리할 수 있는 영역을 제공 합니다.
2. 키보드와 마우스 모두의 사용자 상호 작용에 대 한 응답으로 이벤트를 수락 하 고 응답 합니다.

Windows는 모덜리스 상태 (예: 한 번에 여러 문서를 열 수 있는 텍스트 편집기) 또는 모달 (예: 응용 프로그램을 계속 하기 전에 해제 해야 하는 내보내기 대화 상자)에서 사용할 수 있습니다.

패널은 일반적으로 응용 프로그램에서 보조 기능을 제공 하는 `NSWindow` 특수 한 종류의 창 (기본 클래스의 하위 클래스)으로, 텍스트 형식 검사기 및 시스템 색 선택기와 같은 유틸리티 창과 같은 응용 프로그램의 보조 기능을 제공 합니다.

[![](window-images/intro01.png "Xcode에서 창 편집")](window-images/intro01.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램에서 Windows 및 패널로 작업 하는 기본 사항을 설명 합니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 가장 좋습니다. 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하 고,에서 사용할 주요 개념 및 기술을 설명 하 고 있습니다. 이 문서를 참조 하세요.

[Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서의 [클래스/메서드를 대상으로 노출 C# -C](~/mac/internals/how-it-works.md) 섹션을 살펴볼 수 있습니다. C# 클래스를 연결 하는 데 사용 되는 `Register` 및 `Export` 명령을 설명 합니다. 목표-C 개체 및 UI 요소입니다.

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Windows 소개

위에서 설명한 것 처럼 창에는 보기와 컨트롤을 배치 하 고 관리 하 고 키보드 또는 마우스를 통해 사용자 상호 작용에 따라 이벤트에 응답할 수 있는 영역이 제공 됩니다.

Apple에 따르면 macOS 앱에는 다음과 같은 5 가지 주요 유형의 창이 있습니다.

- **문서 창** -문서 창에는 스프레드시트 또는 텍스트 문서와 같은 파일 기반 사용자 데이터가 포함 됩니다.
- **앱 창** -앱 창은 문서 기반이 아닌 응용 프로그램의 주 창입니다 (예: Mac의 일정 앱).
- **Panel** -패널은 다른 창 위에 배치 되며, 문서가 열려 있는 동안 사용자가 작업할 수 있는 도구나 컨트롤을 제공 합니다. 일부 경우에는 패널이 클 수 있는 경우 (예: 커다란 그래픽으로 작업 하는 경우)
- **대화 상자** -사용자 동작에 대 한 응답으로 대화 상자가 나타나고 일반적으로 사용자가 작업을 완료 하는 방법을 제공 합니다. 대화 상자를 닫기 전에 사용자의 응답이 필요 합니다. ( [대화 작업](~/mac/user-interface/dialog.md)참조)
- **경고** -경고는 심각한 문제 (예: 오류) 또는 경고 (예: 파일 삭제 준비)가 발생 한 경우 표시 되는 특별 한 유형의 대화 상자입니다. 경고는 대화 상자 이므로 닫아야 하기 전에 사용자 응답이 필요 합니다. ( [경고 작업](~/mac/user-interface/alert.md)참조)

자세한 내용은 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) 의 [Windows 정보](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) 섹션을 참조 하세요.

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>주, 키 및 비활성 창

Xamarin.ios 응용 프로그램의 Windows는 현재 사용자가 상호 작용 하는 방법에 따라 다르게 표시 및 동작할 수 있습니다. 현재 사용자의 주의에 초점을 맞춘 가장 많은 문서 또는 앱 창을 _주 창_이라고 합니다. 대부분의 경우이 창은 _키 창_ (현재 사용자 입력을 수락 하는 창)도 됩니다. 그러나이는 항상 그렇지는 않습니다. 예를 들어 색 선택은 열려 있을 수 있으며, 사용자가 문서 창에서 항목의 상태를 변경 하기 위해 상호 작용 하는 키 창이 될 수 있습니다 (여전히 주 창이 됨).

기본 및 키 창 (분리 된 경우)은 항상 활성 상태이 고 _비활성 windows_ 는 전경에 있지 않은 열려 있는 창입니다. 예를 들어 텍스트 편집기 응용 프로그램은 한 번에 둘 이상의 문서를 열어 둘 수 있으며, 주 창만 활성화 되 고 다른 모든 항목은 비활성 상태가 됩니다. 

자세한 내용은 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) 의 [Windows 정보](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) 섹션을 참조 하세요.

<a name="Naming_Windows" />

### <a name="naming-windows"></a>창 이름 지정

창에서 제목 표시줄을 표시할 수 있으며 제목이 표시 되 면 일반적으로 응용 프로그램 이름, 작업 중인 문서 이름 또는 창 기능 (예: 검사기)입니다. 일부 응용 프로그램은 시야가 인식 되 고 문서에서 작동 하지 않기 때문에 제목 표시줄을 표시 하지 않습니다.

Apple에서 다음 지침을 제안 합니다.

- 응용 프로그램 이름을 사용 하 여 문서를 사용 하지 않는 주 창의 제목을 사용 합니다. 
- 새 문서 창 `untitled`에 이름을로 합니다. 첫 번째 새 문서의 경우 제목에 숫자를 추가 하지 않습니다 (예: `untitled 1`). 사용자가 첫 번째 문서를 저장 하 고 제목 하기 전에 다른 새 문서를 만드는 `untitled 2`경우 `untitled 3`해당 창, 등을 호출 합니다.

자세한 내용은 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) 의 [Windows 이름 지정](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) 섹션을 참조 하세요.

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>전체 화면 창

MacOS에서 응용 프로그램의 창은 전체 화면으로 이동 하 여 응용 프로그램 메뉴 모음 (커서를 화면 맨 위로 이동 하 여 표시 될 수 있음)을 표시 하 여 해당 콘텐츠와의 혼란 스 러 향상 된 기능을 제공 합니다.

Apple에서는 다음 지침을 제안 합니다.

- 창을 전체 화면으로 이동 하는 것이 적절 한지 여부를 결정 합니다. 계산기와 같이 간략 한 상호 작용을 제공 하는 응용 프로그램은 전체 화면 모드를 제공 하지 않아야 합니다.
- 전체 화면 작업에 필요한 경우 도구 모음을 표시 합니다. 일반적으로 도구 모음은 전체 화면 모드에서 숨겨집니다.
- 전체 화면 창에는 사용자가 작업을 완료 하는 데 필요한 모든 기능이 있어야 합니다.
- 가능 하면 사용자가 전체 화면 창에 있는 동안 찾기 상호 작용을 방지 합니다.
- 주 작업에서 포커스를 벗어나 이동 하지 않고 늘어난 화면 공간을 활용 합니다.

자세한 내용은 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) 의 [전체 화면 창](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) 섹션을 참조 하세요.

<a name="Panels" />

### <a name="panels"></a>패널

패널은 활성 문서 또는 선택 (예: 시스템 색 선택)에 영향을 주는 컨트롤과 옵션을 포함 하는 보조 창입니다.

[![](window-images/panel01.png "색 패널")](window-images/panel01.png#lightbox)

패널은 _앱 특정_ _또는 전체를 사용할_수 있습니다. 앱 특정 패널은 응용 프로그램의 문서 창 맨 위에 고정 되어 있고 응용 프로그램이 백그라운드에 있을 때 사라집니다. 응용 프로그램에 상관 없이 모든 시스템 패널 (예: **글꼴** 패널)은 열려 있는 모든 창 위에 배치 됩니다. 

Apple에서는 다음 지침을 제안 합니다.

- 일반적으로 표준 패널을 사용 하 고 투명 패널은 그래픽을 많이 사용 하는 작업에만 사용 해야 합니다.
- 패널을 사용 하 여 사용자에 게 직접 작업에 영향을 주는 중요 한 컨트롤이 나 정보에 쉽게 액세스할 수 있도록 하는 것이 좋습니다.
- 필요에 따라 패널을 숨기 거 나 표시 합니다.
- 패널은 항상 제목 표시줄을 포함 해야 합니다.
- 패널은 활성 최소화 단추를 포함 하지 않아야 합니다.

#### <a name="inspectors"></a>검사기

최신 macOS 응용 프로그램은 패널 창을 사용 하는 대신 주 창에 포함 된 _검사기_ 로 활성 문서 또는 선택 항목에 영향을 주는 보조 컨트롤 및 옵션을 제공 합니다 (예: 아래에 표시 된 **페이지** 앱).

[![](window-images/panel02.png "예제 검사자")](window-images/panel02.png#lightbox)

자세한 내용은 Xamarin.ios 앱에서 **검사기 인터페이스** 의 전체 구현을 위한 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) 및 [Macinspector](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector) 샘플 앱의 [패널](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) 섹션을 참조 하세요.

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>Xcode에서 Windows 만들기 및 유지 관리

새 Xamarin.ios Cocoa 응용 프로그램을 만들면 기본적으로 표준 빈 창이 표시 됩니다. 이 창은 프로젝트에 자동으로 `.storyboard` 포함 되는 파일에 정의 됩니다. Windows 디자인을 편집 하려면 **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭 합니다.

[![](window-images/edit01.png "주 스토리 보드 선택")](window-images/edit01.png#lightbox)

이렇게 하면 Xcode의 Interface Builder에서 창 디자인이 열립니다.

[![](window-images/edit02.png "Xcode에서 UI 편집")](window-images/edit02.png#lightbox)

**특성 검사자**에는 창을 정의 하 고 제어 하는 데 사용할 수 있는 몇 가지 속성이 있습니다.

- **Title** -창의 titlebar에 표시 되는 텍스트입니다.
- **자동 저장** -위치 및 설정이 자동으로 저장 될 때 창의 ID를 지정할 때 사용 되는 _키_ 입니다.
- **제목 표시줄** -창에 제목 표시줄이 표시 됩니다.
- **통합 제목 및 도구 모음** -창에 도구 모음이 포함 되어 있으면 제목 표시줄의 일부가 됩니다.
- **전체 크기의 콘텐츠 보기** -창의 콘텐츠 영역을 제목 표시줄 아래에 지정할 수 있습니다.
- **그림자** -창에 그림자가 있습니다.
- **질감이** 있는 질감 창은 효과 (예: vibrancy)를 사용할 수 있으며 본문에서 아무 곳 이나 끌어 이동할 수 있습니다.
- **닫기** -창에 닫기 단추가 있습니다.
- **최소화** -창에 최소화 단추가 있습니다.
- **크기 조정** -창에 크기 조정 컨트롤이 있습니다.
- **도구 모음 단추** -창에는 숨기기/표시 도구 모음 단추가 있습니다.
- **복원 가능한** -창의 위치 및 설정이 자동으로 저장 되 고 복원 됩니다.
- **시작 시 표시** - `.xib` 파일이 로드 될 때 자동으로 표시 되는 창입니다.
- **비활성화 시 숨기기** -응용 프로그램이 백그라운드에 들어가면 창이 숨겨집니다.
- **닫힘 시 릴리스** -메모리를 닫을 때 메모리에서 제거 되는 창입니다.
- **항상 표시 도구** 설명-도구 설명이 계속 표시 됩니다.
- **뷰 루프 다시 계산** -창을 그리기 전에 다시 계산 된 뷰 순서입니다.
- **공백**, **처럼** 및 **순환** -모두 해당 macos 환경에서 창이 작동 하는 방식을 정의 합니다. 
- **전체 화면** -이 창이 전체 화면 모드로 전환 될 수 있는지 여부를 결정 합니다. 
- **애니메이션** -창에 사용할 수 있는 애니메이션의 유형을 제어 합니다.
- **모양** -창의 모양을 제어 합니다. 지금은 바다색이 하나 뿐입니다.

자세한 내용은 Apple의 Windows 및 [Nswindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) [소개 설명서를](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) 참조 하세요.

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>기본 크기 및 위치 설정

창의 초기 위치를 설정 하 고 크기를 제어 하려면 **크기 검사자**로 전환 합니다.

[![](window-images/edit07.png "기본 크기 및 위치")](window-images/edit07.png#lightbox)

여기에서 창의 초기 크기를 설정 하 고, 최소 및 최대 크기를 지정 하 고, 화면에서 초기 위치를 설정 하 고, 창 둘레의 테두리를 제어할 수 있습니다.

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>사용자 지정 주 창 컨트롤러 설정

UI 요소를 코드에 C# 노출 하는 작업 및 작업을 만들 수 있으려면 xamarin.ios 앱은 사용자 지정 창 컨트롤러를 사용 해야 합니다.

다음을 수행합니다.

1. Xcode의 Interface Builder에서 앱의 스토리 보드를 엽니다.
2. Design Surface에서 `NSWindowController` 을 선택 합니다.
3. **Identity Inspector** 뷰로 전환 하 고 `WindowController` **클래스 이름**으로을 입력 합니다. 

    [![](window-images/windowcontroller01.png "클래스 이름 설정")](window-images/windowcontroller01.png#lightbox)
4. 변경 내용을 저장 하 고 동기화 할 Mac용 Visual Studio로 돌아갑니다.
5. Mac용 Visual Studio의 **솔루션 탐색기** 에서 프로젝트에 파일이추가됩니다.`WindowController.cs` 

    [![](window-images/windowcontroller02.png "Windows 컨트롤러 선택")](window-images/windowcontroller02.png#lightbox)
6. Xcode의 Interface Builder에서 Storyboard를 다시 엽니다.
7. 이 `WindowController.h` 파일은 다음과 같이 사용할 수 있습니다. 

    [![](window-images/windowcontroller03.png "WindowController .h 파일 편집")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>UI 요소 추가

창의 내용을 정의 하려면 **라이브러리 검사기** 에서 **인터페이스 편집기**로 컨트롤을 끌어 옵니다. Interface Builder를 사용 하 여 컨트롤을 만들고 사용 하는 방법에 대 한 자세한 내용은 [Xcode 및 Interface Builder 소개 문서를](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 참조 하세요.

예를 들어 **라이브러리 검사기** 의 도구 모음을 **인터페이스 편집기**의 창으로 끌어 보겠습니다.

[![](window-images/edit03.png "라이브러리에서 도구 모음 선택")](window-images/edit03.png#lightbox)

다음으로 **텍스트 뷰** 를 끌고 크기를 조정 하 여 도구 모음 아래의 영역을 채웁니다.

[![](window-images/edit04.png "텍스트 뷰 추가")](window-images/edit04.png#lightbox)

창 크기가 변경 되 면 **텍스트 보기가** 축소 되 고 증가 하도록 하기 때문에 **제약 조건 편집기** 로 전환 하 여 다음 제약 조건을 추가 하겠습니다.

[![](window-images/edit05.png "제약 조건 편집")](window-images/edit05.png#lightbox)

편집기 위쪽에 있는 **Red I-빔 For Red I--** 를 클릭 하 고 **4 개의 제약 조건 추가**를 클릭 하 여 텍스트 뷰에 지정 된 X, Y 좌표를 그대로 유지 하 고 창의 크기를 조정할 때 가로 및 세로로 가로 및 세로로 크기를 조정 합니다.

마지막으로, `ViewController.h` 파일을 선택 하 여 **콘센트** 를 사용 하는 코드에 **텍스트 뷰를 표시** 해 보겠습니다.

[![](window-images/edit06.png "콘센트 구성")](window-images/edit06.png#lightbox)

변경 내용을 저장 하 고 다시 Mac용 Visual Studio로 전환 하 여 Xcode와 동기화 합니다.

**콘센트** 및 작업을 사용 하는 방법에 대 한 자세한 내용은 [유출 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) **설명서를 참조**하세요.

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>표준 창 워크플로

Xamarin.ios 응용 프로그램에서 만들고 사용 하는 모든 창에 대 한 프로세스는 기본적으로 위에서 수행한 것과 동일 합니다.

1. 프로젝트에 자동으로 추가 되는 기본값이 아닌 새 창에는 새 창 정의를 프로젝트에 추가 합니다. 이에 대해서는 아래에서 자세히 설명 합니다.
1. 파일을 `Main.storyboard` 두 번 클릭 하 여 Xcode의 Interface Builder에서 편집할 창 디자인을 엽니다.
1. 새 창을 사용자 인터페이스의 디자인으로 끌고 _segue_ 를 사용 하 여 창을 주 창에 연결 합니다 (자세한 내용은 [storyboard](~/mac/platform/storyboards/indepth.md) 사용 설명서의 [segue](~/mac/platform/storyboards/indepth.md#Segues) 섹션 참조).
1. **특성 검사자** 및 **크기 검사자**에서 필요한 창 속성을 설정 합니다.
1. 인터페이스를 빌드하고 **특성 검사자**에서 구성 하는 데 필요한 컨트롤을 끌어 옵니다.
1. **크기 검사기** 를 사용 하 여 UI 요소의 크기 조정을 처리 합니다.
1. **콘센트** 및 **작업**을 통해 코드에 C# 창의 UI 요소를 노출 합니다.
1. 변경 내용을 저장 하 고 다시 Mac용 Visual Studio로 전환 하 여 Xcode와 동기화 합니다.

이제 기본 창이 만들어졌으므로 windows에서 작업할 때 Xamarin.ios 응용 프로그램에서 수행 하는 일반적인 프로세스를 살펴보겠습니다. 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>기본 창 표시

기본적으로 새 xamarin.ios 응용 프로그램이 시작 될 때 `MainWindow.xib` 파일에 정의 된 창이 자동으로 표시 됩니다.

[![](window-images/display01.png "실행 중인 예제 창")](window-images/display01.png#lightbox)

위에서 해당 창의 디자인을 수정 했으므로 이제 기본 도구 모음과 **텍스트 뷰** 컨트롤을 포함 합니다. `Info.plist` 파일의 다음 섹션은이 창을 표시 하는 작업을 담당 합니다.

[![](window-images/display00.png "Info.plist 편집")](window-images/display00.png#lightbox)

주 **인터페이스** 드롭다운은 기본 앱 UI (이 경우 `Main.storyboard`)로 사용 될 Storyboard를 선택 하는 데 사용 됩니다.

표시 되는 주 창 (기본 보기와 함께)을 제어 하기 위해 뷰 컨트롤러가 프로젝트에 자동으로 추가 됩니다. 이 `ViewController.cs` 파일은 파일에 정의 되 고 **id 검사자**에서 Interface Builder의 **파일 소유자** 에 연결 됩니다.

[![](window-images/display02.png "파일의 소유자 설정")](window-images/display02.png#lightbox)

이 창의 경우에는를 처음 열 때의 `untitled` 제목을 사용 하 여 다음과 같이의 메서드 `ViewController.cs` 를 `ViewWillAppear` 재정의 합니다.

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
```    

> [!NOTE]
> 뷰가 메모리로 로드 될 수 있지만 아직 완전히 인스턴스화되지 `Title` 않았기 때문에 `ViewWillAppear` `ViewDidLoad` 메서드 대신 메서드에서 창의 속성 값을 설정 합니다. 메서드에서 속성에 액세스 하려고 하면 창이 생성 되지 않았고 속성에 아직 연결 되지 않았기 때문에 예외가발생합니다.`null` `Title` `ViewDidLoad`

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>프로그래밍 방식으로 창 닫기

사용자가 창의 **닫기** 단추를 클릭 하거나 메뉴 항목을 사용 하는 것 외에도 사용자가 xamarin.ios 응용 프로그램에서 창을 프로그래밍 방식으로 닫아야 하는 경우가 있을 수 있습니다. macos는 `NSWindow` `PerformClose` 프로그래밍 방식으로를 닫는 두 가지 방법 및 `Close`를 제공 합니다.

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

의 메서드를 호출 하면 단추를 잠시 강조 표시 한 다음 창을 닫아 사용자가 창의 **닫기** 단추를 시뮬레이션합니다.`NSWindow` `PerformClose`

응용 프로그램에서 `NSWindow`의 `WillClose` 이벤트를 구현 하는 경우 창이 닫힐 때까지 발생 합니다. 이벤트가 반환 `false`되 면 창이 닫히지 않습니다. 창에 **닫기** 단추가 없거나 어떤 이유로 든 닫을 수 없는 경우 OS가 경고 소리를 내보냅니다.

예를 들어:

```csharp
MyWindow.PerformClose(this);
```

`MyWindow` 에서`NSWindow` 인스턴스를 닫으려고 시도 합니다. 성공적으로 완료 되 면 창이 닫히고 경고 소리가 내보내지고가 열린 상태로 유지 됩니다.

<a name="Close" />

### <a name="close"></a>닫기

의 메서드를 호출 하면 단추를 일시적으로 강조 표시 하 여 창의 닫기 단추를 클릭할 때 시뮬레이션 하지 않습니다. 창이 닫힙니다. `Close` `NSWindow`

창을 닫을 `NSWindowWillCloseNotification` 수 없으며 알림이 닫히는 창에 대 한 기본 알림 센터에 게시 됩니다.

메서드 `Close` 는 `PerformClose` 메서드에서 두 가지 중요 한 방법으로 다릅니다.

1. `WillClose` 이벤트를 발생 시 키 지 않습니다.
2. 사용자가 단추를 일시적으로 강조 표시 하 여 **닫기** 단추를 클릭 하는 것을 시뮬레이션 하지 않습니다.

예를 들어:

```csharp
MyWindow.Close();
```

`NSWindow` 인스턴스를 닫습니다. `MyWindow`

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>수정 된 Windows 콘텐츠

Macos에서 Apple은 사용자가 창 내용 (`NSWindow`)을 수정 하 고 저장 해야 함을 사용자에 게 알리는 방법을 제공 합니다. 창에 수정 된 내용이 포함 되어 있는 경우 **닫는** 위젯에 작은 검정색 점이 표시 됩니다.

[![](window-images/close01.png "수정 된 표식을 포함 하는 창입니다.")](window-images/close01.png#lightbox)

창의 내용에 저장 하지 않은 변경 내용이 있는 상태에서 사용자가 창을 닫거나 Mac 앱을 종료 하려는 경우에는 [대화 상자](~/mac/user-interface/dialog.md) 또는 [모달 시트](~/mac/user-interface/dialog.md) 를 표시 하 고 사용자가 변경 내용을 먼저 저장할 수 있도록 해야 합니다.

[![](window-images/close02.png "창이 닫힐 때 표시 되는 저장 시트")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>창을 수정 된 것으로 표시

창을 수정 된 콘텐츠로 표시 하려면 다음 코드를 사용 합니다.

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

변경 내용을 저장 한 후에는 다음을 사용 하 여 수정 된 플래그를 지웁니다.

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>창을 닫기 전에 변경 내용 저장

사용자가 창을 닫고 수정 된 콘텐츠를 미리 저장할 수 있도록 허용 하려면의 `NSWindowDelegate` 서브 클래스를 만들고 해당 `WindowShouldClose` 메서드를 재정의 해야 합니다. 예를 들어:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class EditorWindowDelegate : NSWindowDelegate
    {
        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public EditorWindowDelegate (NSWindow window)
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

                // Take action based on result
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

이 대리자의 인스턴스를 창에 연결 하려면 다음 코드를 사용 합니다.

```csharp
// Set delegate
Window.Delegate = new EditorWindowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>앱을 닫기 전에 변경 내용 저장

마지막으로, Xamarin.ios 앱은 해당 창에 수정 된 내용이 포함 되어 있는지 확인 하 고 사용자가 변경 내용을 저장 하도록 허용 하 여 종료 합니다. 이렇게 하려면 `AppDelegate.cs` 파일을 편집 하 고 `ApplicationShouldTerminate` 메서드를 재정의 하 여 다음과 같이 만듭니다.

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

## <a name="working-with-multiple-windows"></a>여러 창에서 작업

대부분의 문서 기반 Mac 응용 프로그램은 여러 문서를 동시에 편집할 수 있습니다. 예를 들어 텍스트 편집기에서 동시에 편집을 위해 여러 텍스트 파일을 열어 둘 수 있습니다. 기본적으로 새 xamarin.ios 응용 프로그램에는 **새** 항목이 자동으로 `newDocument:` **작업**에 연결 된 **파일** 메뉴가 있습니다.

이 새 항목을 활성화 하 고 사용자가 주 창의 여러 복사본을 열어 여러 문서를 한 번에 편집할 수 있도록 합니다.

`AppDelegate.cs` 파일을 편집 하 고 다음 계산 된 속성을 추가 해 보겠습니다.

```csharp
public int UntitledWindowCount { get; set;} =1;
```

이는 사용자에 게 피드백을 제공할 수 있도록 저장 되지 않은 파일 수를 추적 하는 데 사용 됩니다 (위에서 설명한 대로 Apple의 지침에 따라).

다음에는 다음 메서드를 추가 합니다.

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

이 코드는 창 컨트롤러의 새 버전을 만들고, 새 창을 로드 하 고, 기본 및 키 창으로 설정 하 고, 제목으로 설정 합니다. 이제 응용 프로그램을 실행 하 고 **파일** 메뉴에서 **새로 만들기** 를 선택 하면 새 편집기 창이 열리고 표시 됩니다.

[![](window-images/display04.png "제목 없음 창이 새로 추가 되었습니다.")](window-images/display04.png#lightbox)

**Windows** 메뉴를 열면 응용 프로그램에서 열려 있는 창을 자동으로 추적 하 고 처리 하는 것을 볼 수 있습니다.

[![](window-images/display05.png "Windows 메뉴")](window-images/display05.png#lightbox)

Xamarin.ios 응용 프로그램에서 메뉴를 사용 하는 방법에 대 한 자세한 내용은 [메뉴 작업](~/mac/user-interface/menu.md) 설명서를 참조 하세요.

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>현재 활성 창 가져오기

여러 창 (문서)을 열 수 있는 Xamarin.ios 응용 프로그램에서 현재 최상위 창 (키 창)을 가져와야 하는 경우가 있습니다. 다음 코드에서는 키 창을 반환 합니다.

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

현재 키 창에 액세스 해야 하는 모든 클래스 또는 메서드에서 호출 될 수 있습니다. 현재 열려 있는 창이 없으면을 반환 `null`합니다.

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>모든 앱 창에 액세스

Xamarin.ios 앱이 현재 열려 있는 모든 창에 액세스 해야 하는 경우가 있을 수 있습니다. 예를 들어 사용자가 열려는 파일이 종료 창에 이미 열려 있는지 여부를 확인 합니다.

는 `NSApplication.SharedApplication` 앱에서 `Windows` 열려 있는 모든 창의 배열을 포함 하는 속성을 유지 관리 합니다. 이 배열을 반복 하 여 모든 응용 프로그램의 현재 창에 액세스할 수 있습니다. 예:

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

예제 코드에서는 반환 된 각 창을 앱의 사용자 지정 `ViewController` 클래스로 캐스팅 하 고 사용자가 열려는 파일의 경로에 대해 사용자 지정 `Path` 속성의 값을 테스트 합니다. 파일이 이미 열려 있는 경우 해당 창을 맨 앞으로 가져오는 것입니다.

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>코드에서 창 크기 조정

응용 프로그램에서 코드의 창 크기를 조정 해야 하는 경우가 있습니다. 창의 크기를 조정 하 고 위치를 변경 하려면 속성 `Frame` 의 크기를 조정 합니다. 창 크기를 조정할 때는 일반적으로 macOS 좌표계로 인해 창을 동일한 위치에 유지 하기 위해 원본도 조정 해야 합니다.

왼쪽 위 모퉁이가 (0, 0)를 나타내는 iOS와 달리 macOS는 화면의 왼쪽 아래 모퉁이가 (0, 0)를 나타내는 수학 좌표계를 사용 합니다. IOS에서 좌표는 오른쪽 아래로 이동할 때 증가 합니다. MacOS에서 좌표는 오른쪽의 값을 늘립니다. 

다음 예제 코드에서는 창의 크기를 조정 합니다.

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> 코드에서 windows 크기와 위치를 조정 하는 경우 Interface Builder에 설정 된 최소 및 최대 크기를 준수 하는지 확인 해야 합니다. 이는 자동으로 적용 되지 않으며 창을 이러한 제한 보다 크거나 작게 만들 수 있습니다.

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>모니터링 창 크기 변경

Xamarin.ios 앱 내에서 창의 크기 변경을 모니터링 해야 하는 경우가 있을 수 있습니다. 예를 들어 새 크기에 맞게 콘텐츠를 다시 그리도록 합니다.

크기 변경을 모니터링 하려면 먼저 Xcode의 Interface Builder 창 컨트롤러에 대 한 사용자 지정 클래스를 할당 했는지 확인 합니다. 예를 들면 `MasterWindowController` 다음과 같습니다.

[![](window-images/resize01.png "Identity Inspector")](window-images/resize01.png#lightbox)

다음으로, 사용자 지정 창 컨트롤러 클래스를 편집 하 `DidResize` 고 컨트롤러 창의 이벤트를 모니터링 하 여 라이브 크기 변경에 대 한 알림이 표시 되도록 합니다. 예를 들어:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

필요에 따라 이벤트를 `DidEndLiveResize` 사용 하 여 사용자가 창의 크기 변경을 완료 한 후에만 알릴 수 있습니다. 예를 들면 다음과 같습니다.

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

## <a name="setting-a-windows-title-and-represented-file"></a>창의 제목 및 표시 된 파일 설정

문서 `NSWindow` 를 나타내는 창에서 작업 하는 경우에 `DocumentEdited` 는 `true` 속성을 사용 합니다 .이 속성에는 닫기 단추에 작은 점이 표시 됩니다.

`ViewController.cs` 파일을 편집 하 고 다음과 같이 변경 하겠습니다.

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

또한 창에서 이벤트를 `WillClose` 모니터링 하 고 `DocumentEdited` 속성의 상태를 확인 합니다. `true` 사용자에 게 파일에 대 한 변경 내용을 저장 하는 기능을 제공 해야 하는 경우 앱을 실행 하 고 일부 텍스트를 입력 하면 점이 표시 됩니다.

[![](window-images/file01.png "변경 된 창")](window-images/file01.png#lightbox)

창을 닫으려고 하면 다음과 같은 경고 메시지가 표시 됩니다.

[![](window-images/file02.png "저장 대화 상자 표시")](window-images/file02.png#lightbox)

파일에서 문서를 로드 하는 경우 메서드 `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` `path` 를 사용 하 여 창의 제목을 파일 이름으로 설정할 수 있습니다. 즉, 열려는 파일을 나타내는 문자열입니다. 또한 `window.RepresentedUrl = url;` 메서드를 사용 하 여 파일의 URL을 설정할 수 있습니다.

URL이 OS에서 알려진 파일 형식을 가리키는 경우 해당 아이콘이 제목 표시줄에 표시 됩니다. 사용자가 아이콘을 마우스 오른쪽 단추로 클릭 하면 파일 경로가 표시 됩니다.

파일을 `AppDelegate.cs` 편집 하 고 다음 메서드를 추가 해 보겠습니다.

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

이제 앱을 실행 하는 경우 **파일** 메뉴에서 **열기 ...** 를 선택 하 고 **열기** 대화 상자에서 텍스트 파일을 선택 하 여 엽니다.

[![](window-images/file03.png "열기 대화 상자")](window-images/file03.png#lightbox)

파일이 표시 되 고 제목은 파일의 아이콘을 사용 하 여 설정 됩니다.

[![](window-images/file04.png "로드 된 파일의 내용")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>프로젝트에 새 창 추가

기본 문서 창 외에도 Xamarin.ios 응용 프로그램은 기본 설정 또는 검사기 패널과 같은 다른 형식의 창을 사용자에 게 표시 해야 할 수 있습니다.

새 창을 추가 하려면 다음을 수행 합니다.

1. **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 Xcode의 Interface Builder에서 편집할 수 있도록 엽니다.
2. **라이브러리** 에서 새 **창 컨트롤러** 를 끌어 **Design Surface**에 놓습니다.

    [![](window-images/new01.png "라이브러리에서 새 창 컨트롤러 선택")](window-images/new01.png#lightbox)
3. **Id 검사자**에서 `PreferencesWindow` **스토리 보드 ID**로을 입력 합니다. 

    [![](window-images/new02.png "스토리 보드 ID 설정")](window-images/new02.png#lightbox)
4. 인터페이스 디자인: 

    [![](window-images/new03.png "UI 디자인")](window-images/new03.png#lightbox)
5. 앱 메뉴 (`MacWindows`)를 열고, **기본 설정 ...** 을 선택 하 고, 컨트롤을 클릭 한 다음 새 창으로 끕니다. 

    [![](window-images/new05.png "Segue 만들기")](window-images/new05.png#lightbox)
6. 팝업 메뉴에서 **표시** 를 선택 합니다.
7. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

**응용 프로그램 메뉴**에서 코드를 실행 하 고 **기본 설정 ...** 을 선택 하는 경우 창이 표시 됩니다.

[![](window-images/new04.png "샘플 기본 설정 메뉴")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>패널 작업

이 문서의 시작 부분에서 설명한 것 처럼 패널은 다른 창 위에 배치 되며, 문서가 열려 있는 동안 사용자가 작업할 수 있는 도구나 컨트롤을 제공 합니다. 

Xamarin.ios 응용 프로그램에서 만들고 사용 하는 다른 유형의 창과 마찬가지로 프로세스는 기본적으로 동일 합니다.

1. 새 창 정의를 프로젝트에 추가 합니다.
2. 파일을 `.xib` 두 번 클릭 하 여 Xcode의 Interface Builder에서 편집할 창 디자인을 엽니다.
3. **특성 검사자** 및 **크기 검사자**에서 필요한 창 속성을 설정 합니다.
4. 인터페이스를 빌드하고 **특성 검사자**에서 구성 하는 데 필요한 컨트롤을 끌어 옵니다.
5. **크기 검사기** 를 사용 하 여 UI 요소의 크기 조정을 처리 합니다.
6. **콘센트** 및 **작업**을 통해 코드에 C# 창의 UI 요소를 노출 합니다.
7. 변경 내용을 저장 하 고 다시 Mac용 Visual Studio로 전환 하 여 Xcode와 동기화 합니다.

**특성 검사자**의 패널에는 다음과 같은 옵션이 있습니다.

[![](window-images/panel03.png "특성 검사자")](window-images/panel03.png#lightbox)

- **스타일** -패널의 스타일을 다음과 같이 조정할 수 있습니다. 일반 패널 (표준 창 처럼 보임), 유틸리티 패널 (더 작은 제목 표시줄 있음), HUD 패널 (반투명 이며 제목 표시줄은 배경의 일부)입니다.
- **활성화 되지 않음** -패널의 키 창이 됩니다.
- **문서 모달** -문서 모달 인 경우 패널은 응용 프로그램 창 위에만 고정 되 고, 다른 창에는 부동으로 배치 됩니다.

새 패널을 추가 하려면 다음을 수행 합니다.

1. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.
2. 새 파일 대화 상자에서 컨트롤러를 사용 하는 **xamarin.ios** > **cocoa 창**을 선택 합니다.

    [![](window-images/panels00.png "새 창 컨트롤러 추가")](window-images/panels00.png#lightbox)
3. **이름**에 대해 `DocumentPanel`를 입력하고 **새로 만들기** 단추를 클릭합니다.
4. 파일을 `DocumentPanel.xib` 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. 

    [![](window-images/new02.png "패널 편집")](window-images/new02.png#lightbox)
5. 기존 창을 삭제 하 고 **인터페이스 편집기**에서 **라이브러리 검사기** 의 패널을 끕니다. 

    [![](window-images/panels01.png "기존 창 삭제")](window-images/panels01.png#lightbox)
6. 패널을 **파일의 소유자** - **창** - **콘센트**에 연결 합니다. 

    [![](window-images/panels02.png "끌어서 패널 연결")](window-images/panels02.png#lightbox)
7. **Id 검사자** 로 전환 하 고 패널의 클래스를로 `DocumentPanel`설정 합니다. 

    [![](window-images/panels03.png "패널의 클래스 설정")](window-images/panels03.png#lightbox)
8. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.
9. `DocumentPanel.cs` 파일을 편집 하 고 클래스 정의를 다음과 같이 변경 합니다. 

    `public partial class DocumentPanel : NSPanel`
10. 파일의 변경 내용을 저장합니다.

파일을 편집 하 고 메서드 `DidFinishLaunching` 를 다음과 같이 만듭니다. `AppDelegate.cs`

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

응용 프로그램을 실행 하는 경우 패널이 표시 됩니다.

[![](window-images/panels04.png "실행 중인 앱의 패널")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> 패널 창은 Apple에서 더 이상 사용 되지 않으므로 **검사기 인터페이스로**바꾸어야 합니다. Xamarin.ios 앱에서 **검사기** 를 만드는 전체 예제는 [macinspector](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector) 샘플 앱을 참조 하세요.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 창과 패널을 사용 하는 방법에 대해 자세히 살펴봅니다. Xcode의 Interface Builder에서 창과 패널을 만들고 유지 관리 하는 방법 및 코드에서 C# 창과 패널을 사용 하는 방법에 대 한 다양 한 형식 및 사용 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacWindows (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [MacInspector (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [메뉴 작업](~/mac/user-interface/menu.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
