---
title: Xamarin.Mac의 Windows
description: 이 문서에서는 windows 및 Xamarin.Mac 응용 프로그램에서 패널을 사용 하 여 작업을 다룹니다. Windows 만들고 패널 Xcode 및 Interface Builder 스토리 보드 및.xib 파일에서 로드 하 고 프로그래밍 방식으로 작업에서 설명 합니다.
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 2d129c72366224cedca26df6fa1499f65d04e92d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106977"
---
# <a name="windows-in-xamarinmac"></a>Xamarin.Mac의 Windows

_이 문서에서는 windows 및 Xamarin.Mac 응용 프로그램에서 패널을 사용 하 여 작업을 다룹니다. Windows 만들고 패널 Xcode 및 Interface Builder 스토리 보드 및.xib 파일에서 로드 하 고 프로그래밍 방식으로 작업에서 설명 합니다._

Xamarin.Mac 응용 프로그램에서 C# 및.NET을 사용 하 여 작업을 하는 경우 동일한 Windows에 액세스할 수 하는 개발자의 작업 패널 *Objective-c* 하 고 *Xcode* 않습니다. Xcode의 Xamarin.Mac이 Xcode와 직접 통합 되므로 사용할 수 있습니다 _Interface Builder_ 만들기 및 Windows 및 패널 유지 (또는 필요에 따라 C# 코드에서 바로 작성).

Xamarin.Mac 응용 프로그램의 용도 따라 관리 하 고 정보를 표시 하 고 함께 조정 화면에서 하나 이상의 Windows를 표시할 수 있습니다. 창의 사용자 기능은 다음과 같습니다.

1. 보기와 컨트롤 배치 하 고 관리할 수 있는 영역을 제공 합니다.
2. 적용 하는 키보드와 마우스를 사용 하 여 사용자 상호 작용에 대 한 응답으로 이벤트에 응답 합니다.

Windows (예: 응용 프로그램을 계속 하기 전에 해제 해야 하는 내보내기 대화 상자) 모달 또는 모덜리스 상태 (예: 여러 문서를 한 번에 열 수 있는 텍스트 편집기)에서 사용 될 수 있습니다.

패널은 특수 한 유형의 창 (자료의 서브 클래스 `NSWindow` 클래스)에 일반적으로 사용할 수 있는 보조 함수를 텍스트 형식 검사기 및 시스템 색 선택과 같은 windows 유틸리티 같은 응용 프로그램에서.

[![](window-images/intro01.png "Xcode에서 창의 편집")](window-images/intro01.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 Windows 및 패널을 사용 하 여 작업의 기본 사항을 설명 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서를 사용 하는 기술을 설명 합니다.

확인 하려는 경우는 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md) 섹션을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 설명도 문서를 `Register` 및 `Export` 명령 하 여 통신-C# 클래스 Objective-c 개체 및 UI 요소에 사용 합니다.

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Windows 소개

위에서 설명한 대로 창 있는 보기와 컨트롤 배치 하 고 관리할 수 있는 영역을 제공 및 사용자 상호 작용 (중 키보드 또는 마우스를 통해)을 기준으로 이벤트에 응답 합니다.

Apple에 따라 가지 macOS 앱의에서 다섯 가지 유형으로 Windows

- **문서 창** -문서 창 텍스트 문서나 스프레드시트와 같은 사용자 파일 기반 데이터를 포함 합니다.
- **앱 창** -는 앱 기간은 (예: Mac에서 일정 앱) 문서 기반 하지 않는 응용 프로그램의 주 창입니다.
- **패널** -패널 다른 창 위에 배치 하 고 도구를 제공 하거나 문서는 사용자가 사용할 수 있는 컨트롤을 엽니다. 경우에 따라 패널 반투명 될 수 있습니다 (경우와 같이 큰 그래픽 작업).
- **대화** -대화 상자를 사용자 작업에 대 한 응답에 표시 되 고 같은 방법으로 사용자가 작업을 완료할 수는 일반적으로 제공 합니다. 대화 상자를 사용자의 응답을 거쳐야 닫을 수 있습니다. (참조 [대화 상자를 사용 하 여 작업](~/mac/user-interface/dialog.md))
- **경고** -경고 대화 상자가 나타나면 (예: 오류) 심각한 문제가 발생 한 경우의 특수 형식이 또는 경고 (예: 파일을 삭제 하려면 준비). 경고 대화 상자 이기 때문에 문제가 닫힐 수 있음 전에 사용자 응답도 필요 합니다. (참조 [경고를 사용 하 여 작업](~/mac/user-interface/alert.md))

자세한 내용은 참조는 [에 대 한 Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>Main, 키 및 비활성 Windows

Xamarin.Mac 응용 프로그램에서 Windows를 확인 하 고 어떻게 사용자가 현재 상호 작용에 따라 다르게 동작할 수 있습니다. 주목할 만한 문서 또는 현재 사용자의 관심 있는 앱 창 이라고 합니다 _주 창_합니다. 대부분의 경우가이 창을 수도 합니다 _키 창_ (현재 사용자 입력을 받고 창). 그러나 이것이 경우 항상, 예를 들어, 색 선택 수 열어야 (이 여전히 주 창) 문서 창에 있는 항목의 상태를 변경 하는 사용자가 상호 작용 하는 키 창 수입니다.

주 보고서 및 키 Windows (별도 경우)는 항상 활성 상태를 _비활성 Windows_ 포그라운드에 있지 않은 열린 창이 있습니다. 예를 들어, 텍스트 편집기 응용 프로그램을 둘 이상의 문서를 가지기 열고 한 번에 주 창 활성화 됩니다만, 다른 모든 하지 활성화 됩니다. 

자세한 내용은 참조는 [에 대 한 Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Windows를 명명합니다.

창 제목 표시줄을 표시할 수 있습니다 이며 제목 표시 되 면 일반적으로 응용 프로그램의 이름, (예: 관리자) 창의 함수나 작업 중인 문서의 이름입니다. 일부 응용 프로그램 sight에서 인식할 수 있는 한 문서를 사용 하 여 작동 하지 않습니다 하므로 제목 표시줄을 표시 하지 않습니다.

Apple 다음 지침을 제안 합니다.

- 주, 비 문서 창의 제목에 대 한 응용 프로그램 이름을 사용 합니다. 
- 새 문서 창 이름을 `untitled`입니다. 제목에 숫자가 추가 없는 첫 번째 새 문서에 대해 (같은 `untitled 1`). 사용자 저장 하 고 첫 번째 제목 전에 다른 새 문서를 만드는 경우 해당 창을 호출 `untitled 2`, `untitled 3`등입니다.

자세한 내용은 참조는 [Windows 명명](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>전체 화면 Windows

MacOS에서 응용 프로그램 창의 넘어가면 전체 화면 숨기기 (화면 맨 위에 커서를 이동 하 여 표시할 수 있습니다)는 응용 프로그램 메뉴를 포함 한 모든 콘텐츠는 무료 상호 혼란을 제공 합니다.

Apple 다음 지침을 제안합니다.

- 전체 화면을 이동 하려면 창의 적합 한지 확인 합니다. 간단한 상호 작용 (예: 계산기)를 제공 하는 응용 프로그램에는 전체 화면 모드를 제공 해서는 안 됩니다.
- 전체 화면 작업이 필요한 경우 도구 모음을 표시 합니다. 일반적으로 도구 모음에서 전체 화면 모드에서 숨겨집니다.
- 전체 화면 창 작업을 완료 하는 데 필요한 모든 기능 사용자가 있어야 합니다.
- 가능한 경우 사용자가 전체 화면 창에 있는 동안 Finder 상호 작용을 피하십시오.
- 향상 된 화면 공간 없이 활용 주 작업에서 포커스를 이동 합니다.

자세한 내용은 참조는 [전체 화면 Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>패널

패널은 컨트롤 및 선택 항목 (예: 시스템 색 선택)을 활성 문서에 영향을 주는 옵션을 포함 하는 보조 창의 같습니다.

[![](window-images/panel01.png "색 패널")](window-images/panel01.png#lightbox)

패널 일 수 있습니다 _앱 별_ 하거나 _시스템 전반에 걸쳐_합니다. 앱 별 패널 응용 프로그램의 문서 창 맨 위에 위치 및 응용 프로그램은 백그라운드에서 사라집니다. 시스템 전반에 걸쳐 패널 (같은 합니다 **글꼴** 패널), 응용 프로그램에 관계 없이 모든 열린 창 위에 float입니다. 

Apple 다음 지침을 제안합니다.

- 일반적으로 표준 패널을 사용 하 여, 필요한 경우에 및 그래픽 집약적인 작업에 대 한 투명 한 패널만 사용 해야 합니다.
- 사용자에 게 쉽게 액세스할 수 있도록 중요 한 컨트롤 또는 직접 자신의 작업에 영향을 주는 정보를 제공 하는 패널을 사용 하는 것이 좋습니다.
- 필요에 따라 패널 표시 / 숨기기.
- 패널 제목 표시줄을 항상 포함 해야 합니다.
- 패널에는 활성 최소화 단추를 포함 되지 않습니다.

#### <a name="inspectors"></a>검사기

보조 컨트롤 및 활성 문서 또는 선택에 영향을 주는 옵션을 제공 하는 가장 최신 macOS 응용 프로그램 _검사기_ 주 창에 속하는 (같은 합니다 **페이지** 아래에 표시 된 앱), 대신 Windows 패널을 사용 합니다.

[![](window-images/panel02.png "예제 검사기")](window-images/panel02.png#lightbox)

자세한 내용은 참조는 [패널](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) 및 [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) 를의전체구현에대한샘플앱**검사기 인터페이스** Xamarin.Mac 앱에서.

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>만들기 및 Xcode에서 Windows 유지 관리

새 Xamarin.Mac Cocoa 응용 프로그램을 만들면 기본적으로 표준 비어 창을 가져옵니다. 이 windows에 정의 된를 `.storyboard` 파일 프로젝트에 자동으로 포함 합니다. windows 디자인에 맞게 편집 하는 **솔루션 탐색기**를 두 번 클릭 합니다 `Main.storyboard` 파일:

[![](window-images/edit01.png "주 스토리 보드를 선택합니다.")](window-images/edit01.png#lightbox)

Xcode의 Interface Builder에서 창 디자인을 엽니다.

[![](window-images/edit02.png "Xcode에서 UI를 편집합니다.")](window-images/edit02.png#lightbox)

에 **특성 검사기**를 정의 하 고 제어 창을 사용할 수 있는 속성이 몇 가지:

- **제목** -창의 제목 표시줄에 표시 되는 텍스트입니다.
- **자동 저장** -이 _키_ 의 위치 및 설정을 자동으로 저장 되 면 창 ID 하는 데 수.
- **제목 표시줄** -창 제목 표시줄을 표시 합니다.
- **제목 및 도구 모음을 통합** -창 도구 모음에 제목 표시줄의 일부를이 여야 합니다.
- **콘텐츠 보기 크기의 전체** -콘텐츠 영역 창의 제목 표시줄 아래에 있을 수 있습니다.
- **섀도** -창 그림자가 있습니다.
- **질감** -질감이 windows 효과 (예: 표현)을 사용할 수 및 해당 본문에서 아무 곳 이나 끌어서 이동할 수 있습니다.
- **닫기** -창 닫기 단추가 없는 합니다.
- **최소화** -않습니다 창을 최소화 단추가 있습니다.
- **크기 조정** -창에는 크기 조정 컨트롤입니다.
- **도구 모음 단추** -않습니다 창 표시/숨기기 도구 모음 단추가 있습니다.
- **가능한** -창 위치 및 설정을 자동으로 저장 및 복원 됩니다.
- **시작 시 표시** -창이 자동으로 표시 되는 경우는 `.xib` 파일을 로드 합니다.
- **비활성화에 숨기기** -응용 프로그램이 백그라운드에 들어가면 창이 숨겨진 됩니다.
- **닫힙니다 릴리스** -창의 닫을 때 메모리에서 제거 됩니다.
- **항상 표시 도구 설명** -도구 설명을 지속적으로 표시 됩니다.
- **보기 루프를 다시 계산** -보기 순서가 창을 그리기 전에 다시 계산 합니다.
- **공간**, **Exposé** 하 고 **Cycling** -모든 창을 이러한 macOS 환경에서 작동 하는 방식을 정의 합니다. 
- **전체 화면** -이 창을 전체 화면 모드를 시작할 수 하는 경우를 결정 합니다. 
- **애니메이션** -창에 사용할 수 있는 애니메이션의 종류를 제어 합니다.
- **모양을** -창의 모양을 제어 합니다. 지금은 바다색 모양을 하나만 있습니다.

Apple의를 참조 하세요 [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) 하 고 [NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) 자세한 설명서.

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>기본 크기 및 위치를 설정합니다.

전환으로 창의 초기 위치를 설정 하 고 해당 크기를 제어 합니다 **크기 검사기**:

[![](window-images/edit07.png "기본 크기 및 위치")](window-images/edit07.png#lightbox)

여기에서 창의 초기 크기를 설정, 최소 및 최대 크기를 제공, 화면에서 초기 위치를 설정 하 창 주변의 테두리를 제어 합니다.

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>사용자 지정 주 창의 컨트롤러 설정

C# 코드에 UI 요소를 노출 하는 출 선 및 작업을 만들 수, Xamarin.Mac 앱 사용자 지정 창 컨트롤러를 사용 해야 합니다.

다음을 수행합니다.

1. Xcode의 Interface Builder에서 앱의 스토리 보드를 엽니다.
2. 선택 된 `NSWindowController` 디자인 화면에서 합니다.
3. 전환할 합니다 **검사기** 살펴보고 입력 `WindowController` 으로 **클래스 이름**: 

    [![](window-images/windowcontroller01.png "클래스 이름 설정")](window-images/windowcontroller01.png#lightbox)
4. 변경 내용을 저장 하 고 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.
5. A `WindowController.cs` 에서 프로젝트에 추가할 파일을 **솔루션 탐색기** Mac 용 Visual Studio에서: 

    [![](window-images/windowcontroller02.png "Windows 컨트롤러를 선택 하면")](window-images/windowcontroller02.png#lightbox)
6. Xcode의 Interface Builder 스토리 보드를 다시 엽니다.
7. `WindowController.h` 파일 용도로 제공 됩니다. 

    [![](window-images/windowcontroller03.png "WindowController.h 파일 편집")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>UI 요소를 추가합니다.

창의 콘텐츠를 정의 하려면 컨트롤을 끌어 옵니다 합니다 **라이브러리 검사기** 에 **인터페이스 편집기**합니다. 하십시오 우리의 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) Interface Builder를 사용 하 여 만들고 컨트롤을 사용 하는 방법에 대 한 자세한 내용은 설명서.

예를 들어 보겠습니다에서 도구 모음을 끌어 합니다 **라이브러리 검사기** 창에는 **인터페이스 편집기**:

[![](window-images/edit03.png "라이브러리에서 도구 모음 선택")](window-images/edit03.png#lightbox)

다음으로 끌어를 **텍스트 뷰** 도구 모음에서 영역을 채우도록 크기를 조정 하 고:

[![](window-images/edit04.png "텍스트 뷰를 추가합니다.")](window-images/edit04.png#lightbox)

것 이므로 합니다 **텍스트 뷰** 축소 창의 크기를 변경 함에 따라 확장 하 고으로 전환 해 보겠습니다 합니다 **제약 조건 편집기** 다음 제약 조건이 추가:

[![](window-images/edit05.png "제약 조건 편집")](window-images/edit05.png#lightbox)

클릭 하 여는 대 한 **빨간색 I-빔** 편집기의 맨 위 및 클릭 **4 개 제약 조건 추가**, 지정 된 X, Y 좌표 맞도록 및 늘리거나 가로 및 세로로 텍스트 뷰를 지시 하는 것으로 창 크기가 조정 됩니다.

마지막으로 노출 하겠습니다를 **텍스트 뷰** 사용 하는 코드를 **출 선** (선택 해야는 `ViewController.h` 파일):

[![](window-images/edit06.png "출 선 구성")](window-images/edit06.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 다시 전환 합니다.

작업에 대 한 자세한 내용은 **출 선** 및 **작업**를 참조 하십시오 우리의 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 설명서.

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>표준 창 워크플로

만들고 Xamarin.Mac 응용 프로그램에서 사용 하는 모든 창에 대 한 프로세스는 기본적으로 새로운 방금 수행한 위에서 동일:

1. 새 windows의 기본 프로젝트에 자동으로 추가 되지 않은 경우 새 창 정의 프로젝트에 추가 합니다. 이 아래에 자세히 설명 합니다.
2. 두 번 클릭 합니다 `Main.storyboard` Xcode의 Interface Builder에서 편집용으로 창 디자인을 열려는 파일입니다.
3. 사용자 인터페이스 디자인에 새 창을 끌어서 창의 주 창에 사용 하 여 연결 _고 Segues_ (자세한 내용은 참조는 [고 Segues](~/mac/platform/storyboards/indepth.md#Segues) 부분 우리의 [storyboards작업](~/mac/platform/storyboards/indepth.md) 설명서).
3. 필수 창의 속성을 설정할 합니다 **특성 검사기** 하며 **크기 검사기**합니다.
4. 인터페이스를 만들고 구성 하는 데 필요한 컨트롤에 끌어 합니다 **특성 검사기**합니다.
5. 사용 하 여 합니다 **크기 검사기** 처리할 UI 요소에 대 한 크기를 조정 합니다.
6. 창의 UI 요소를 통해 C# 코드에 노출 **출 선** 하 고 **작업**합니다.
7. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 다시 전환 합니다.

이제 만든 기본 창 했으므로 살펴보겠습니다 일반적인 프로세스는 Xamarin.Mac 응용 프로그램에서 windows를 사용 하 여 작업 하는 경우. 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>기본 창 표시

기본적으로 새 Xamarin.Mac 응용 프로그램을 자동으로 표시 됩니다에 정의 된 기간이 `MainWindow.xib` 시작 될 때 파일:

[![](window-images/display01.png "실행 하는 예제 윈도우")](window-images/display01.png#lightbox)

해당 창 위에의 디자인을 수정 했습니다 이제 여기에 기본 도구 모음 및 **텍스트 뷰** 제어 합니다. 다음 섹션은 `Info.plist` 파일이이 창을 표시 하는 일을 담당 합니다.

[![](window-images/display00.png "Info.plist를 편집합니다.")](window-images/display00.png#lightbox)

합니다 **주 인터페이스** 드롭다운을 사용 하면 주 응용 프로그램 UI로 사용할 스토리 보드 선택 (이 경우 `Main.storyboard`).

뷰 컨트롤러 (기본 뷰)와 함께 표시 되는 해당 주 Windows 컨트롤을 프로젝트에 자동으로 추가 됩니다. 에 정의 되어는 `ViewController.cs` 파일과 연결 합니다 **파일의 소유자** 에서 Interface Builder에서 합니다 **Id 검사기**:

[![](window-images/display02.png "파일의 소유자를 설정합니다.")](window-images/display02.png#lightbox)

이 창에 대 한 좀의 제목이 되도록 `untitled` 처음 열 때 보겠습니다 재정의 된 `ViewWillAppear` 에서 메서드를 `ViewController.cs` 다음과 같이:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> 창의 값을 설정 하는 것 `Title` 속성에는 `ViewWillAppear` 메서드 대신는 `ViewDidLoad` 메서드 뷰를 메모리로 로드 될 수 있습니다, 있지만 하지 아직 완전히 인스턴스화된 때문에 합니다. 에 액세스 하려고 할 경우는 `Title` 속성에는 `ViewDidLoad` 얻게 되는 메서드를 `null` 창 않은 생성 하 고 했으니 속성에 아직 있으므로 예외.

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>프로그래밍 방식으로 창 닫기

프로그래밍 방식으로 Xamarin.Mac 응용 프로그램의 창 닫기 하려는 경우가 있을 것 이외의 사용자는 창의 클릭 합니다 **닫습니다** 단추 또는 메뉴 항목을 사용 합니다. macOS 제공 닫으려면 두 가지 방법으로 `NSWindow` 프로그래밍 방식으로: `PerformClose` 및 `Close`합니다.

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

호출을 `PerformClose` 메서드는 `NSWindow` 창의 클릭 하는 사용자를 시뮬레이션 **닫기** 일시적으로 단추를 강조 표시 하 고 창을 종료 한 후 단추.

응용 프로그램을 구현 하는 경우는 `NSWindow`의 `WillClose` 이벤트 창이 닫히기 전에 발생 합니다. 이벤트를 반환 하는 경우 `false`, 다음 창이 닫히지 것입니다. 창 없는 경우는 **닫기** 단추 또는 닫을 수 없습니다. 어떤 이유로 OS 경고 소리를 내보냅니다.

예를 들어:

```csharp
MyWindow.PerformClose(this);
```

닫기 하려고 합니다 `MyWindow` `NSWindow` 인스턴스. 성공적으로 수행 되었으면 창, 다른 내보낼 경고 소리 닫히고는 열린 채로 합니다.

<a name="Close" />

### <a name="close"></a>닫기

호출을 `Close` 메서드는 `NSWindow` 창의 클릭 하는 사용자를 시뮬레이션 하지 않습니다 **닫기** 단추 일시적으로 단추를 강조 표시를 하 여 단순히 창을 닫기입니다.

창이 닫혀 있는 것으로 표시 될 필요가 없습니다 및 `NSWindowWillCloseNotification` 알림 닫히기 창에 대 한 기본 알림 센터에 게시 됩니다.

합니다 `Close` 에서 두 가지 방식에서 다른 메서드는 `PerformClose` 메서드:

1. 발생 하려고 하지는 `WillClose` 이벤트입니다.
2. 클릭 하는 사용자를 시뮬레이션 하지 않습니다 합니다 **닫기** 일시적으로 단추를 강조 표시 단추입니다.

예를 들어:

```csharp
MyWindow.Close();
```

닫을 때 합니다 `MyWindow` `NSWindow` 인스턴스.

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>수정 된 Windows 콘텐츠

Apple에 사용자에 게 알릴 방법 macOS에서 제공 하는 창의 내용을 (`NSWindow`) 저장 하며 사용자가 수정 되었습니다. 작은 검정색 점의에 표시할 창 수정 된 콘텐츠가 들어 있으면 **닫기** 위젯:

[![](window-images/close01.png "수정 된 마커를 사용 하 여 창")](window-images/close01.png#lightbox)

사용자는 창을 닫거나 종료 하려고 시도 하는 경우 윈도우의 콘텐츠가 변경 내용을 저장 하지 않습니다 하는 동안 Mac 앱을 제공 해야는 [대화 상자](~/mac/user-interface/dialog.md) 하거나 [모달 시트](~/mac/user-interface/dialog.md) 해당 변경 내용을 저장 하 고 사용자 첫 번째:

[![](window-images/close02.png "창이 닫힐 때 표시 되는 시트 저장")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>수정 된 창 표시

콘텐츠를 수정 것으로 창에 표시 하려면 다음 코드를 사용 합니다.

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

및 변경 내용을 저장 된 후 수정 된 플래그를 통해 선택을 취소 합니다.

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>창을 닫기 전에 변경 내용을 저장

서브 클래스를 만든 해야 미리 수정 된 콘텐츠를 저장 하려면 창을 닫고 수 있도록 사용자를 보려면 `NSWindowDelegate` 시키고 해당 `WindowShouldClose` 메서드. 예를 들어:

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

창에이 대리자의 인스턴스를 연결 하려면 다음 코드를 사용 합니다.

```csharp
// Set delegate
Window.Delegate = new EditorWindowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>앱을 닫기 전에 변경 내용을 저장

마지막으로 Xamarin.Mac 앱은 해당 Windows의 모든 수정 된 콘텐츠를 포함 하며 종료 하기 전에 변경 내용을 저장 하는 데 사용할 수 확인 해야 합니다. 이 작업을 수행 하려면 편집 프로그램 `AppDelegate.cs` 파일에서 재정의 된 `ApplicationShouldTerminate` 메서드 수 있으며 다음과 같은 모양:

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

## <a name="working-with-multiple-windows"></a>여러 Windows 작업

대부분의 문서를 기반으로 Mac 응용 프로그램 동시에 여러 문서를 편집할 수 있습니다. 예를 들어, 텍스트 편집기에는 여러 텍스트 파일을 동시에 편집을 위해 열려 있을 수 있습니다. 기본적으로 새 Xamarin.Mac 응용 프로그램에는 **파일** 사용 하 여 메뉴를 **새로 만들기** 자동으로 유선-최대 항목을 `newDocument:` **작업**.

이 새 항목을 활성화 하 고 한 번에 여러 문서를 편집 하는 주 창의 여러 복사본을 열도록 허용 하 예정입니다.

편집할 우리의 `AppDelegate.cs` 파일과 다음 계산 된 속성을 추가 합니다.

```csharp
public int UntitledWindowCount { get; set;} =1;
```

(위에 설명 된 대로 Apple의 지침)에 따라 사용자에 게 피드백을 제공할 수 있습니다 있도록 저장 되지 않은 파일의 수를 추적에이 사용 하겠습니다.

다음에 다음 메서드를 추가 합니다.

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

이 코드 창 컨트롤러의 새 버전을 작성, 새 창이 로드, 있도록 주 보고서 및 키 창 및 제목을 설정 합니다. 이제가 응용 프로그램을 실행 하 고 선택 **새로 만들기** 에서 합니다 **파일** 새 편집기 창이 열리고 표시 메뉴:

[![](window-images/display04.png "새 제목이 없는 창을 추가한")](window-images/display04.png#lightbox)

열겠습니다 경우 합니다 **Windows** 메뉴에서 응용 프로그램은 자동으로 추적 및 처리는 열려 있는 창을 볼 수 있습니다.

[![](window-images/display05.png "Windows 메뉴")](window-images/display05.png#lightbox)

Xamarin.Mac 응용 프로그램에서 작업 메뉴에 대 한 자세한 내용은 참조 하십시오 우리의 [메뉴를 사용 하 여 작업](~/mac/user-interface/menu.md) 설명서.

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>현재 활성 창 시작

Xamarin.Mac 응용 프로그램의 여러 windows (문서)를 열 수 있는 경우 현재, 최상위 창 (키 창)를 가져올 해야 하는 경우 경우가 있습니다. 다음 코드는 키 창을 반환 합니다.

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

모든 클래스 또는 현재, 키 창에 액세스 해야 하는 메서드 호출 될 수 있습니다. 경우 창이 현재 열려 있으면 반환 `null`합니다.

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>모든 앱 Windows 액세스

경우도 Xamarin.Mac 앱에 현재 열려가 windows의 모든 액세스 해야 합니다. 예를 들어 경우 여는 사용자가 파일을 확인 하려면 열려 있는 기존 창에서.

합니다 `NSApplication.SharedApplication` 유지 관리는 `Windows` 열려 있는 모든 windows 앱의 배열을 포함 하는 속성입니다. 모든 앱의 현재 windows에 액세스 하려면이 배열을 반복할 수 있습니다. 예를 들어:

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

예제 코드에서에서는 캐스팅 하는 사용자 지정 반환 된 각 창은 `ViewController` 앱 및 사용자 지정 값을 테스트 클래스 `Path` 열려는 사용자가 파일의 경로 대 한 속성입니다. 파일이 이미 열려 있으면 앞으로 해당 창의 대상인 것입니다.

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>코드에서 창 크기 조정

응용 프로그램 코드에서 창의 크기를 조정 해야 할 경우 경우가 있습니다. 조정 크기와 위치를 창의 `Frame` 속성입니다. 창의 크기를 조정 하는 경우에 일반적으로 macOS의 좌표 시스템으로 인해 창 같은 위치에 유지 하려면 해당 원본을 조정할 수도 필요 합니다.

왼쪽 위 모서리는 (0, 0)를 나타냅니다 iOS와 달리 macOS 사용 하 여 수학 좌표 시스템을 화면의 하단 왼쪽 모서리는 (0, 0)를 나타냅니다. IOS에서 오른쪽 방향으로 아래로 이동 좌표 증가 합니다. MacOS에서 좌표 오른쪽 위쪽에 값이 증가 합니다. 

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
> 코드의 위치를 창 크기를 조정 하는 경우에 Interface Builder에서 설정한 최소 및 최대 크기를 준수 하는지 확인 해야 합니다. 이 적용 되지 않습니다 자동으로 및 크거나 이러한 한도 보다 작은 창을 확인 하는 일을 할 수 있습니다.

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>창 크기가 변경 모니터링

경우도 Xamarin.Mac 앱 내에서 창 크기의 변경 내용을 모니터링 해야 합니다. 예를 들어, 새 크기에 맞도록 콘텐츠를 다시 그려야 합니다.

크기 변화를 모니터링 하려면 먼저 Xcode의 Interface Builder에서 창 컨트롤러에 대 한 사용자 지정 클래스를 할당 했는지 확인 합니다. 예를 들어 `MasterWindowController` 다음에서:

[![](window-images/resize01.png "검사기")](window-images/resize01.png#lightbox)

다음으로, 사용자 지정 창 컨트롤러 클래스 및 모니터를 편집 합니다 `DidResize` 라이브 크기 변경의 알림을 받을 컨트롤러의 창에는 이벤트입니다. 예를 들어:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

필요한 경우 사용할 수는 `DidEndLiveResize` 만 사용자가 창의 크기를 변경 후 알림을 받으려면 이벤트입니다. 예를 들면 다음과 같습니다.

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

## <a name="setting-a-windows-title-and-represented-file"></a>창 제목의 설정과 파일 표시

문서를 나타내는 windows를 사용 하 여 작업 하는 경우 `NSWindow` 에 `DocumentEdited` 속성이로 설정 된 경우 `true` 표시는 파일 수정 되었는지와를 닫기 전에 저장 해야 하는 사용자에 게 닫기 단추에 작은 점을 표시 합니다.

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

또한 모니터링 하는 합니다 `WillClose` 창과의 상태를 확인 하는 중에 이벤트를 `DocumentEdited` 속성입니다. 있으면 `true` 사용자 파일에 변경 내용을 저장 하는 기능을 제공 해야 합니다. 앱을 실행 하 고 일부 텍스트를 입력, 점 표시 됩니다.

[![](window-images/file01.png "변경 된 창")](window-images/file01.png#lightbox)

창을 닫으려면 하려고 하면 경고를 수신 했습니다.

[![](window-images/file02.png "저장 표시 대화 상자")](window-images/file02.png#lightbox)

파일의 창의 제목을 설정할 수 있습니다를 로드 하 고 문서 파일의 경우 사용 하 여 이름을 합니다 `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` 메서드 (가정 `path` 열려 있는 파일을 나타내는 문자열입니다). 또한 사용 하 여 파일의 URL을 설정할 수 있습니다는 `window.RepresentedUrl = url;` 메서드.

URL은 OS에서 알려진 파일 형식을 가리키는 경우 해당 아이콘 제목 표시줄에 표시 됩니다. 마우스 오른쪽 단추로 클릭할 아이콘에는 파일의 경로를 표시 됩니다.

편집할는 `AppDelegate.cs` 파일과 다음 메서드를 추가 합니다.

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

이제 앱을 실행 하는 경우 선택 **열기...**  에서 **파일** 메뉴에서 선택 된 텍스트 파일에서 **엽니다** 대화 상자를 엽니다.

[![](window-images/file03.png "열린 대화 상자")](window-images/file03.png#lightbox)

파일 표시 되 고 파일의 아이콘으로 제목 설정 됩니다.

[![](window-images/file04.png "파일의 내용을 로드")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>프로젝트에 새 창을 추가

기본 문서 창 외에도 Xamarin.Mac 응용 프로그램을 다른 유형의 windows 기본 설정이 나 검사기 패널 등 사용자에 게 표시 해야 합니다.

새 창에 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**를 두 번 클릭를 `Main.storyboard` Xcode의 Interface Builder에서 편집용으로 열 파일입니다.
2. 새 끌어 **창 컨트롤러** 에서 **라이브러리** 놓습니다 합니다 **디자인 화면**:

    [![](window-images/new01.png "라이브러리에서 새 창 컨트롤러를 선택 하면")](window-images/new01.png#lightbox)
3. 에 **검사기**를 입력 `PreferencesWindow` 에 대 한 합니다 **스토리 보드 ID**: 

    [![](window-images/new02.png "Storyboard ID를 설정합니다.")](window-images/new02.png#lightbox)
5. 사용자 인터페이스를 디자인 합니다. 

    [![](window-images/new03.png "UI 디자인")](window-images/new03.png#lightbox)
6. 응용 프로그램 메뉴를 열고 (`MacWindows`)을 선택 **기본 설정...** , 컨트롤을 클릭 하 고 새 창으로 끌어 옵니다. 

    [![](window-images/new05.png "Segue를 만들기")](window-images/new05.png#lightbox)
7. 선택 **표시** 팝업 메뉴에서.
6. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

코드를 실행 하 고 선택 된 **기본 설정...**  에서 합니다 **응용 프로그램 메뉴**는 창이 표시 됩니다.

[![](window-images/new04.png "샘플 기본 설정 메뉴")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>패널 사용

이 문서 맨 앞에서 설명한 대로 패널 다른 창 위에 배치 하 고 도구 또는 문서가 열려 있는 동안 사용자가 사용할 수 있는 컨트롤을 제공 합니다. 

만들고 Xamarin.Mac 응용 프로그램에서 사용 하는 창의 다른 모든 형식과 마찬가지로 프로세스는 기본적으로 동일 합니다.

1. 새 창 정의 프로젝트에 추가 합니다.
2. 두 번 클릭 합니다 `.xib` Xcode의 Interface Builder에서 편집용으로 창 디자인을 열려는 파일입니다.
2. 필수 창의 속성을 설정할 합니다 **특성 검사기** 하며 **크기 검사기**합니다.
4. 인터페이스를 만들고 구성 하는 데 필요한 컨트롤에 끌어 합니다 **특성 검사기**합니다.
5. 사용 하 여 합니다 **크기 검사기** 처리할 UI 요소에 대 한 크기를 조정 합니다.
6. 창의 UI 요소를 통해 C# 코드에 노출 **출 선** 하 고 **작업**합니다.
7. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 다시 전환 합니다.

에 **특성 검사기**, 특정 패널에 다음 옵션을 사용할 수:

[![](window-images/panel03.png "특성 검사기")](window-images/panel03.png#lightbox)

- **스타일** -에서 패널의 스타일을 조정할 수 있습니다: 일반 패널 (같습니다 표준 창), 유틸리티 패널 (작은 제목 표시줄이 있는) HUD 패널 (반투명 백그라운드의 일부인 제목 표시줄).
- **비 활성** -에 따라 결정 패널 키 창이 됩니다.
- **모달 문서** -경우 문서 모달 패널의 응용 프로그램의 창 위에 배치만 됩니다, 다른 모든 부동 합니다.


새 패널을 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** .
2. 새 파일 대화 상자에서 선택 **Xamarin.Mac** > **컨트롤러를 사용 하 여 Cocoa 창을**:

    [![](window-images/panels00.png "새 창 컨트롤러 추가")](window-images/panels00.png#lightbox)
3. **이름**에 대해 `DocumentPanel`를 입력하고 **새로 만들기** 단추를 클릭합니다.
4. 두 번 클릭 하 여 `DocumentPanel.xib` Interface Builder에서 편집 하기 위해 열려는 파일: 

    [![](window-images/new02.png "pannel 편집")](window-images/new02.png#lightbox)
5. 패널을 끌어서 기존 창을 삭제 합니다 **라이브러리 검사기** 에 **인터페이스 편집기**: 

    [![](window-images/panels01.png "기존 창을 삭제")](window-images/panels01.png#lightbox)
6. 최대 패널을 연결 합니다 **파일의 소유자** - **창** - **콘센트**: 

    [![](window-images/panels02.png "패널을 실시간으로 끌어")](window-images/panels02.png#lightbox)
7. 으로 전환 합니다 **검사기** 패널의 클래스 설정 하 고 `DocumentPanel`: 

    [![](window-images/panels03.png "패널의 클래스를 설정합니다.")](window-images/panels03.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.
7. 편집 된 `DocumentPanel.cs` 파일과 클래스 정의 변경 합니다. 

    `public partial class DocumentPanel : NSPanel`
8. 파일의 변경 내용을 저장합니다.

편집 합니다 `AppDelegate.cs` 파일을 확인 합니다 `DidFinishLaunching` 다음과 같은 메서드 확인:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

응용 프로그램을 실행 하는 경우 패널에 표시 됩니다.

[![](window-images/panels04.png "실행 중인 앱에서 패널")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> 패널 Windows Apple에서 사용이 중단 된 및로 바꿔야 **검사기 인터페이스**합니다. 만드는 전체 예제는 **검사기** Xamarin.Mac 앱을 참조 하세요 우리의 [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) 샘플 앱입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 응용 프로그램에서 Windows 및 패널을 사용 하 여 작업을 자세히 살펴보려면을 걸렸습니다. 다양 한 종류를 확인 하 고 Windows 및 패널, 만들고 Windows 및 Xcode의 Interface Builder에서 패널을 유지 관리 하는 방법 및 C# 코드에서 Windows 및 패널을 사용 하 여 작업 하는 방법을 사용 합니다.

## <a name="related-links"></a>관련 링크

- [MacWindows (샘플)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (샘플)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [메뉴 작업](~/mac/user-interface/menu.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
