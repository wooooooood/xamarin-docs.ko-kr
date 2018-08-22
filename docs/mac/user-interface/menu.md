---
title: Xamarin.Mac의 메뉴
description: 이 문서에서는 Xamarin.Mac 응용 프로그램에서 메뉴를 사용 하 여 작업을 설명 합니다. 만들고 유지 관리 메뉴 및 메뉴 항목 Xcode 및 Interface Builder에서 프로그래밍 방식으로 작업을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d84910cd5c2bc72a563fb04457532d544aedf571
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251159"
---
# <a name="menus-in-xamarinmac"></a>Xamarin.Mac의 메뉴

_이 문서에서는 Xamarin.Mac 응용 프로그램에서 메뉴를 사용 하 여 작업을 설명 합니다. 만들고 유지 관리 메뉴 및 메뉴 항목 Xcode 및 Interface Builder에서 프로그래밍 방식으로 작업을 설명 합니다._

Xamarin.Mac 응용 프로그램에서 C# 및.NET을 사용 하 여 작업을 하는 경우 Objective C Xcode에서 작업 하는 개발자는 같은 Cocoa 메뉴에 액세스를 해야 합니다. Xamarin.Mac이 Xcode와 직접 통합 되므로 만들기 및 프로그램 메뉴 모음, 메뉴 및 메뉴 항목을 유지 관리 (또는 필요에 따라 C# 코드에서 바로 작성) Xcode의 Interface Builder를 사용할 수 있습니다.

메뉴는 Mac 응용 프로그램의 사용자 경험의 필수적인 부분 되며 일반적으로 사용자 인터페이스의 여러 부분에 나타납니다.

- **응용 프로그램의 메뉴 모음** -모든 Mac 응용 프로그램에 대 한 화면 위쪽에 표시 되는 주 메뉴입니다.
- **상황에 맞는 메뉴** -이러한 사용자 마우스 오른쪽 단추로 클릭 하거나 컨트롤-클릭 창에서 항목을 표시 합니다.
- **상태 표시줄** -(에서 메뉴 표시줄 시계의 왼쪽) 화면 위쪽에 표시 되는 항목에 추가 됨에 따라 왼쪽으로 증가 함에 따라 응용 프로그램 메뉴 모음의 오른쪽 끝에 있는 영역입니다.
- **도킹 메뉴** -도킹 스테이션에서 각 응용 프로그램에 대 한 메뉴 표시 되는 사용자가 아이콘을 마우스 단추를 누르고 경우 또는 사용자 마우스 오른쪽 단추로 클릭 하거나 응용 프로그램의 아이콘 컨트롤-클릭 합니다.
- **팝업 단추 및 목록을 풀 다운** -팝업 단추 선택된 된 항목을 표시 하 고 사용자가 클릭할 때 선택할 옵션 목록을 표시 합니다. 드롭다운 목록에는 일반적으로 현재 작업의 컨텍스트에 관련 특정 명령을 선택할 때 사용 되는 팝업 단추 형식입니다. 둘 다 창의 아무 곳 이나 나타날 수 있습니다.

[![예제 메뉴](menu-images/intro01.png "는 예제 메뉴")](menu-images/intro01-large.png#lightbox)

이 문서에서는 Cocoa 메뉴 모음, 메뉴 및 Xamarin.Mac 응용 프로그램에서 메뉴 항목을 사용 하는 기본 사항을 설명 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서를 사용 하는 기술을 설명 합니다.

확인 하려는 경우를 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md) 부분을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 설명도 문서를 `Register` 및 `Export` 특성 하 여 통신-C# 클래스 Objective-c 개체 및 UI 요소에 사용 합니다.

## <a name="the-applications-menu-bar"></a>응용 프로그램의 메뉴 모음 

모든 창의 연결 된 메뉴 모음 자체를 가질 수 있는 Windows OS에서 실행 되는 응용 프로그램과 달리 macOS에서 실행 중인 모든 응용 프로그램에는 해당 응용 프로그램의 모든 창에 사용 되는 화면 상단의 실행 되는 단일 메뉴 모음에 있습니다.

[![메뉴 모음의](menu-images/appmenu01.png "메뉴 모음")](menu-images/appmenu01-large.png#lightbox)

이 메뉴 모음에서 항목 활성화 되거나 비활성화 된 특정된 시점에는 현재 컨텍스트 또는 응용 프로그램 및 해당 사용자 인터페이스의 상태에 따라 합니다. 예를 들어: 사용자가 텍스트 필드를 선택 하는 경우 항목에 **편집** 메뉴와 같은 사용 가능한 상태가 됩니다 **복사** 하 고 **잘라내기**합니다.

Apple에 따라 및 기본적으로 모든 macOS 응용 프로그램에는 메뉴 및 응용 프로그램의 메뉴 모음에 나타나는 메뉴 항목의 표준 집합을 있습니다.

- **Apple 메뉴** -이 메뉴 어떤 응용 프로그램이 실행 중인지에 관계 없이 항상 사용자에 게 제공 하는 와이드 항목 시스템에 대 한 액세스를 제공 합니다. 개발자가 이러한 항목을 수정할 수 없습니다.
- **앱 메뉴** -이 메뉴 굵게 표시 된 응용 프로그램의 이름을 표시 하 고는 사용자가 현재 실행 중인 응용 프로그램을 식별 하는 데 도움이 됩니다. 전체 및 없습니다 특정된 문서 또는 응용 프로그램을 종료 하는 등의 프로세스와 응용 프로그램에 적용 되는 항목을 포함 합니다.
- **파일 메뉴** -항목 만들기, 열기, 하는 데 사용 하거나 저장 문서를 사용 하 여 응용 프로그램이 작동 합니다. 응용 프로그램이 없는 경우 문서 기반,이 메뉴 이름을 바꾸거나 제거 합니다.
- **편집 메뉴** -같은 명령을 보유 **잘라내기**, **복사**, 및 **붙여넣기** 편집 하거나 응용 프로그램의 사용자 인터페이스에서 요소를 수정 하는 데 사용 되는 합니다.
- **서식 메뉴** -텍스트의 형식을 조정 하려면 저장 명령 텍스트를이 메뉴를 사용 하 여 응용 프로그램이 작동 하는 경우.
- **보기 메뉴** -명령에 영향을 주는 콘텐츠 표시 되는 방법을 응용 프로그램의 사용자 인터페이스에 (표시)를 보유 합니다.
- **응용 프로그램별 메뉴** -(예: 웹 브라우저에 대 한 책갈피 메뉴) 응용 프로그램에 관련 된 모든 메뉴는 합니다. 간에 표시 되어야 합니다 **보기** 하 고 **창** 메뉴 모음에서.
- **창 메뉴** -windows 응용 프로그램 뿐만 아니라 현재 열린 창의 목록에서 사용 하기 위한 명령이 포함 되어 있습니다.
- **도움말 메뉴** -도움말 메뉴 맨 오른쪽 메뉴 모음에서 해야 응용 프로그램에서 온라인 도움말을 제공 합니다. 

응용 프로그램 메뉴 및 표준 메뉴 및 메뉴 항목에 대 한 자세한 내용은 Apple의를 참조 하세요 [Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)합니다.

### <a name="the-default-application-menu-bar"></a>기본 응용 프로그램 메뉴 모음

새 Xamarin.Mac 프로젝트를 만들 때마다 자동으로 macOS 응용 프로그램을 정상적으로 (위의 섹션에 설명)으로 포함 하는 일반적인 항목에 있는 표준 기본 응용 프로그램 메뉴 모음을 가져옵니다. 에 정의 되어 있는 응용 프로그램의 기본 메뉴 표시줄의 **Main.storyboard** 파일 (앱 UI의 나머지 부분)의 프로젝트 아래에 **Solution Pad**:  

![주 스토리 보드 선택](menu-images/appmenu02.png "주 스토리 보드를 선택 합니다.")

두 번 클릭 합니다 **Main.storyboard** 메뉴 편집기 인터페이스를 사용 하 여 표시 됩니다 Xcode의 Interface Builder에서 편집에 대 한 열려는 파일:

[![UI Xcode에서 편집](menu-images/defaultbar01.png "UI Xcode에서 편집")](menu-images/defaultbar01-large.png#lightbox)

여기에서 수 클릭 항목 같은 **열기** 의 메뉴 항목은 **파일** 메뉴를 편집 하거나에서 속성을 조정 합니다 **특성 검사기**:

[![메뉴의 특성을 편집](menu-images/defaultbar02.png "메뉴의 속성 편집")](menu-images/defaultbar02-large.png#lightbox)

추가, 편집 및 메뉴 및이 문서의 뒷부분에 나오는 항목 삭제 하겠습니다. 이제 방금 확인 하려는 기본적으로 사용할 수 있는 메뉴 및 메뉴 항목 및 방법을 자동으로 표시 되지 미리 정의 된 출 선 및 작업 집합을 통해 코드에 대 한 (자세한 내용은 참조 하세요. 당사의 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 설명서)입니다.

예를 들어 여기를 클릭 합니다 **연결 검사기** 에 대 한를 **열기** 가 자동으로 연결 되는 최대 보면 메뉴 항목을 `openDocument:` 작업: 

[![연결 된 작업 보기](menu-images/defaultbar03.png "연결 된 작업 보기")](menu-images/defaultbar03-large.png#lightbox)

선택 하는 경우는 **첫 번째 응답자** 에 **인터페이스 계층 구조** 에서 아래로 스크롤하여는 **연결 검사기**의 정의 볼 수 있습니다는 `openDocument:` 작업 하는 **열려** (함께 여러 가지 다른 기본 작업 응용 프로그램에 대 한 하는 자동으로 연결 되어 있지 않음 컨트롤까지) 메뉴 항목에 연결 된:

[![연결 된 모든 작업 보기](menu-images/defaultbar04.png "모든 연결 된 작업 보기")](menu-images/defaultbar04-large.png#lightbox) 

이 중요 한 이유는? 다음에서 섹션에서는 이러한 자동으로 정의 된 작업 방식으로 자동으로 사용 하도록 설정 하 고 메뉴 항목을 사용 하지 않도록 설정으로, 항목에 대 한 기본 제공 기능을 제공 다른 Cocoa 사용자 인터페이스 요소를 사용 하 여 표시 됩니다.

나중에 사용할 예정 이러한 기본 제공 작업을 사용 하도록 설정 하 고 코드에서 항목을 사용 하지 않도록 선택 하면 고유한 기능을 제공 합니다.

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>기본 제공 메뉴 기능

일부 항목이 자동으로 했으니를 활성화 (완벽 하 게 기능으로 자동으로 기본 제공)와 같은 보면 UI 항목 또는 코드를 추가 하기 전에 새로 만든된 Xamarin.Mac 응용 프로그램을 실행 하는 경우에 **Quit** 항목에 **앱** 메뉴:

![활성화 메뉴 항목](menu-images/appmenu03.png "활성화 메뉴 항목")

다른 메뉴 항목과 같은 while **잘라내기**, **복사본**, 및 **붙여넣기** 없습니다:

![메뉴 항목을 사용 하지 않도록 설정](menu-images/appmenu04.png "메뉴 항목을 사용 하지 않도록 설정")

보겠습니다 응용 프로그램을 중지 하 고 두 번 클릭 합니다 **Main.storyboard** 파일는 **Solution Pad** 의 Interface Builder를 Xcode에서 편집을 위해 엽니다. 를 끌어를 **텍스트 뷰** 에서 합니다 **라이브러리** 에서 창의 뷰 컨트롤러에는 **인터페이스 편집기**:

[![라이브러리에서 텍스트 뷰를 선택 하](menu-images/appmenu05.png "라이브러리에서 텍스트 보기를 선택 합니다.")](menu-images/appmenu05-large.png#lightbox)

에 **제약 조건 편집기** 보겠습니다 텍스트 뷰 창의 가장자리를 고정 하 고 증가 하 고 모든 4 개의 빨간색 I-빔 편집기의 맨 위에 있는 클릭 하 여 창을 사용 하 여 축소 설정 된 **4 개 제약 조건추가** 단추:

[![편집 제약](menu-images/appmenu06.png "제약 편집")](menu-images/appmenu06-large.png#lightbox)

사용자 인터페이스 디자인에 변경 내용을 저장 하 고 Xamarin.Mac 프로젝트를 사용 하 여 변경 내용을 동기화 하는 Mac 용 Visual Studio를 다시 전환 합니다. 이제 응용 프로그램을 시작, 텍스트 보기에 일부 텍스트를 입력, 선택 및 엽니다는 **편집** 메뉴:

![메뉴 항목은 자동으로 사용/사용 안 함](menu-images/appmenu07.png "메뉴 항목은 자동으로 사용/사용 안 함")

알림 방법을 **잘라내기**를 **복사**, 및 **붙여넣기** 항목이 자동으로 사용 하도록 설정 되어 코드를 전혀 작성 하지 않고도 완전 한 기능을 합니다. 

여기에 무슨 일이 일어나고 있나요? 기본 제공 (위에서 제공) 처럼 유선 최대 기본 메뉴 항목 제공 되는 작업을 미리 정의할를 후크 특정 작업을 기본적으로 대부분의 macos에 포함 된 Cocoa 사용자 인터페이스 요소 (같은 `copy:`). 따라서 활성 창에 추가 하 고 선택 하면 해당 메뉴 항목 또는 항목에 연결할 때 해당 작업은 자동으로 사용 합니다. 사용자가 해당 메뉴 항목을 선택 하는 경우에 UI 요소에 기본 제공 기능 호출 및 개발자의 개입 없이 실행 합니다.

### <a name="enabling-and-disabling-menus-and-items"></a>설정 및 해제 메뉴 및 항목

기본적으로 사용자 이벤트가 발생할 때마다 `NSMenu` 자동으로 사용 하도록 설정 하 고 각 표시 메뉴 및 메뉴 항목 컨텍스트를 기반으로 응용 프로그램의 사용 하지 않도록 설정 합니다. 항목에 대 한 활성화/비활성화 하는 방법은 세 가지가 있습니다.

- **자동 메뉴를 사용 하도록 설정** -경우에 메뉴 항목을 사용할 수는 `NSMenu` 항목은 유선에 접속 하는 작업에 반응 하는 적절 한 개체를 찾을 수 있습니다. 예를 들어 텍스트 보기 위의 기본 제공 후크를 갖고 있던는 `copy:` 동작 합니다.
- **사용자 지정 작업 및 validateMenuItem:** -바인딩되는 메뉴 항목에 대 한는 [창 또는 보기 컨트롤러 사용자 지정 작업](#Working-with-Custom-Window-Actions)를 추가할 수 있습니다는 `validateMenuItem:` 작업을 수동으로 활성화 또는 비활성화 메뉴 항목입니다.
- **수동 메뉴를 사용 하도록 설정** -수동으로 설정 합니다 `Enabled` 의 각 속성 `NSMenuItem` 사용 하도록 설정 하거나 메뉴의 각 항목이 개별적으로 사용 하지 않도록 설정 합니다.

시스템을 선택 하려면 설정 합니다 `AutoEnablesItems` 의 속성을 `NSMenu`입니다. `true` 자동 (기본 동작) 및 `false` 수동입니다. 

> [!IMPORTANT]
> 사용 하 여 수동 메뉴를 사용 하도록 설정 하려는 경우 없음 메뉴의 항목에 같은 AppKit 클래스에 의해 제어 되는 `NSTextView`를 자동으로 업데이트 됩니다. 사용 하도록 설정 하 고 코드에서 직접 모든 항목을 사용 하지 않도록 설정 됩니다.

#### <a name="using-validatemenuitem"></a>ValidateMenuItem를 사용 하 여

가 바인딩되는 모든 메뉴 항목에 대해 설명한 것 처럼를 [창 또는 보기 컨트롤러 사용자 지정 작업](#Working-with-Custom-Window-Actions)를 추가할 수 있습니다를 `validateMenuItem:` 작업을 수동으로 활성화 또는 비활성화 메뉴 항목입니다.

다음 예제에서는 `Tag` 속성은 수 활성화/비활성화 하 여는 메뉴 항목의 형식을 결정 하는 `validateMenuItem:` 에서 선택한 텍스트의 상태를 기반으로 작업을 `NSTextView`. `Tag` Interface Builder에서 각 메뉴 항목 속성 설정 되었습니다.

![태그 속성을 설정](menu-images/validate01.png "Tag 속성 설정")

및 다음 코드는 보기 컨트롤러에 추가:

```csharp
[Action("validateMenuItem:")]
public bool ValidateMenuItem (NSMenuItem item) {

    // Take action based on the menu item type
    // (As specified in its Tag)
    switch (item.Tag) {
    case 1:
        // Wrap menu items should only be available if
        // a range of text is selected
        return (TextEditor.SelectedRange.Length > 0);
    case 2:
        // Quote menu items should only be available if
        // a range is NOT selected.
        return (TextEditor.SelectedRange.Length == 0);
    }

    return true;
}
```

이 코드를 실행 및에서 선택한 텍스트가 없는 경우는 `NSTextView`, 두 줄 바꿈 메뉴 항목은도 사용할 수 없습니다 (해당은 뷰 컨트롤러의 작업에 연결):

![사용 하지 않도록 설정 하는 표시 항목](menu-images/validate02.png "사용할 수 없는 항목 표시")

텍스트 섹션을 선택 하 고 메뉴를 다시 열 경우에 두 개의 줄 바꿈 메뉴 항목을 사용할 수 있습니다.

![사용 하도록 설정 하는 표시 항목](menu-images/validate03.png "보여 주는 항목을 사용 하도록 설정")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>사용 하도록 설정 하 고 코드에서 메뉴 항목에 응답

(예: 텍스트 필드의 경우)는 UI 디자인에 특정 Cocoa 사용자 인터페이스 요소를 추가 하 여 위에서 살펴본 대로 사용할 기본 메뉴 항목의 여러 및 코드를 작성할 필요 없이 자동으로 작동 합니다. 다음 메뉴 항목을 선택할 때 기능을 제공할 Xamarin.Mac 프로젝트에 고유한 C# 코드를 추가에 대해 살펴보겠습니다.

사용자가 사용할 수 있게 하고자 하는 예를 들어, let 말입니다.는 **엽니다** 항목에 **파일** 메뉴 폴더를 선택 합니다. 응용 프로그램 수준의 함수가 고 제공 창 또는 UI 요소에 제한 되지 때문이 응용 프로그램 대리자에 게이 처리 하는 코드를 추가 하겠습니다.

에 **Solution Pad**, 두 번 클릭 합니다 `AppDelegate.CS` 파일을 편집 하는 것에 대 한 열:

![앱 대리자를 선택 하면](menu-images/appmenu08.png "앱 대리자를 선택 합니다.")

아래 다음 코드를 추가 합니다 `DidFinishLaunching` 메서드:

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

보겠습니다 이제 응용 프로그램을 실행 하 고 엽니다는 **파일** 메뉴: 

![파일 메뉴](menu-images/appmenu09.png "파일 메뉴")

**열려** 메뉴 항목은 이제 사용 하도록 설정 합니다. 를 선택 하는 경우 열기 대화 상자가 표시 됩니다.

![대화 상자를 엽니다](menu-images/appmenu10.png "열기 대화 상자를")

클릭 하면를 **열려** 단추를이 경고 메시지가 표시 됩니다.

![예제 대화 메시지](menu-images/appmenu11.png "대화 메시지의 예")

키 줄 여기 되었습니다 `[Export ("openDocument:")]`, 알려줍니다 `NSMenu` 는 우리의 **AppDelegate** 메서드가 `void OpenDialog (NSObject sender)` 응답 하는 `openDocument:` 작업. 위의 기억할 수 하는 경우는 **열고** 메뉴 항목은 자동으로 했으니가이 동작에 Interface Builder에서 기본적으로:

[![연결 된 작업을 보는](menu-images/defaultbar03.png "연결 된 작업 보기")](menu-images/defaultbar03-large.png#lightbox)

다음에 대해 살펴보겠습니다 고유한 메뉴, 메뉴 항목 및 작업 만들기 및 응답에 코드에서 해당 합니다.

### <a name="working-with-the-open-recent-menu"></a>최근 열기 메뉴 사용

기본적으로 **파일** 메뉴에는 **열린 최근** 추적 하는 사용자가 앱을 사용 하 여 열려 있는 마지막 몇 가지 파일 항목입니다. 만들려는 경우를 `NSDocument` 기반 Xamarin.Mac 앱이이 메뉴를에 자동으로 처리 합니다. Xamarin.Mac 앱의 다른 모든 형식에 대 한 관리 및이 메뉴 항목을 수동으로 대응 하는 일을 담당 해야 합니다.

직접 처리 합니다 **열린 최근** 메뉴, 먼저 새 파일을 열거나 다음을 사용 하 여 저장 된에 알리기 위해:

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

앱을 사용 하지 않는 경우에 `NSDocuments`를 계속 사용할를 `NSDocumentController` 유지 하기 위해를 **열린 최근** 전송 하 여 메뉴를 `NSUrl` 파일의 위치를 사용 하 여를 `NoteNewRecentDocumentURL` 메서드의 `SharedDocumentController`합니다.

재정의 해야 하는 다음에 `OpenFile` 에서 사용자가 선택한 모든 파일을 열고 앱 대리자의 메서드를 **최근** 메뉴. 예를 들어:

```csharp
public override bool OpenFile (NSApplication sender, string filename)
{
    // Trap all errors
    try {
        filename = filename.Replace (" ", "%20");
        var url = new NSUrl ("file://"+filename);
        return OpenFile(url);
    } catch {
        return false;
    }
}
```

반환 `true` 파일을 열 수 있는 경우 다른 반환 `false` 기본 제공 경고는 파일을 열 수 없습니다 사용자에 게 표시 됩니다.

파일 이름 및 경로 반환 하기 때문에 **열린 최근** 메뉴에는 공백이 포함 될 수 있습니다, 올바르게를 만들기 전에이 문자를 이스케이프 해야는 `NSUrl` 오류가 발생 하면 또는 합니다. 에서는 다음 코드를 사용 하 여 그렇게 합니다.

```csharp
filename = filename.Replace (" ", "%20");
```

마지막으로 만들 것을 `NSUrl` 파일 및 앱에서 도우미 메서드를 새 창을 열고 파일 로드 대리자 사용을 가리키는:

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

모든 항목이 함께 끌어올 살펴보겠습니다는의 구현 예제는 **AppDelegate.cs** 파일:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace MacHyperlink
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }

        public override bool OpenFile (NSApplication sender, string filename)
        {
            // Trap all errors
            try {
                filename = filename.Replace (" ", "%20");
                var url = new NSUrl ("file://"+filename);
                return OpenFile(url);
            } catch {
                return false;
            }
        }
        #endregion

        #region Private Methods
        private bool OpenFile(NSUrl url) {
            var good = false;

            // Trap all errors
            try {
                var path = url.Path;

                // Is the file already open?
                for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
                    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
                    if (content != null && path == content.FilePath) {
                        // Bring window to front
                        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
                        return true;
                    }
                }

                // Get new window
                var storyboard = NSStoryboard.FromName ("Main", null);
                var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

                // Display
                controller.ShowWindow(this);

                // Load the text into the window
                var viewController = controller.Window.ContentViewController as ViewController;
                viewController.Text = File.ReadAllText(path);
                viewController.SetLanguageFromPath(path);
                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                viewController.View.Window.RepresentedUrl = url;

                // Add document to the Open Recent menu
                NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);

                // Make as successful
                good = true;
            } catch {
                // Mark as bad file on error
                good = false;
            }

            // Return results
            return good;
        }
        #endregion

        #region actions
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
                    // Open the document in a new window
                    OpenFile (url);
                }
            }
        }
        #endregion
    }
}
```

앱의 요구 사항에 따라 하지 않을 사용자가 동시에 둘 이상의 창에서 동일한 파일을 엽니다. 예제 앱에서 사용자가 이미 열려 있는 파일을 선택 하는 경우 (에서 합니다 **최근** 또는 **열기...** 메뉴 항목의 경우) 파일이 포함 된 창 앞으로 상태로 전환 됩니다.

이를 위해이 도우미 메서드에 다음 코드를 사용 했습니다.

```csharp
var path = url.Path;

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

설계 했습니다 우리의 `ViewController` 경로에서 파일에 포함 된 클래스를 해당 `Path` 속성입니다. 그런 다음 앱에서 현재 열려 있는 모든 창을 통해 루프입니다. 이면 파일이 이미 열려 창 중 하나에서 사용 하 여 다른 모든 windows의 맨 앞으로 가져오기:

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

일치 항목이 없으면, 새 창을 로드 된 파일을 사용 하 여 연 파일에 설명 되어는 **열린 최근** 메뉴:

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);

// Load the text into the window
var viewController = controller.Window.ContentViewController as ViewController;
viewController.Text = File.ReadAllText(path);
viewController.SetLanguageFromPath(path);
viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
viewController.View.Window.RepresentedUrl = url;

// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

<a name="Working-with-Custom-Window-Actions" />

### <a name="working-with-custom-window-actions"></a>사용자 지정 창 작업 사용

마찬가지로 다음 기본 제공 **첫 번째 응답자** 미리 유선 표준 메뉴 항목에 제공 되는 작업을 만들어 새 사용자 지정 작업 하는 Interface Builder에서 메뉴 항목에 연결 하 합니다.

먼저, 응용 프로그램의 창 컨트롤러 중 하나에서 사용자 지정 작업을 정의 합니다. 예를 들어:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

다음으로 앱의 스토리 보드 파일을 두 번 클릭 합니다 **Solution Pad** 의 Interface Builder를 Xcode에서 편집을 위해 엽니다. 선택 합니다 **첫 번째 응답자** 아래를 **응용 프로그램 장면**, 전환를 **특성 검사기**:

![특성 검사기](menu-images/action01.png "특성 검사기")

클릭 합니다 **+** 아래쪽의 단추를 **특성 검사기** 새 사용자 지정 동작을 추가 하려면:

![새 작업 추가](menu-images/action02.png "새 작업 추가")

창 컨트롤러에서 만든 사용자 지정 작업으로 동일한 이름을 지정 합니다.

![작업 이름을 편집](menu-images/action03.png "작업 이름을 편집")

메뉴 항목을 끌어서 컨트롤-클릭 합니다 **첫 번째 응답자** 아래에서 **응용 프로그램 장면**합니다. 팝업 목록에서 방금 만든 새 작업을 선택 합니다 (`defineKeyword:` 이 예제의):

![액션을 연결](menu-images/action04.png "작업 연결")

스토리 보드에 변경 내용을 저장 하 고 변경 내용을 동기화 하는 Mac 용 Visual Studio로 돌아갑니다. 앱을 실행 하는 경우 사용자 지정 동작을 연결 하는 메뉴 항목은 자동으로 수 사용/사용 안 함 (열린 상태에서 작업을 사용 하 여 창에 기반한) 로깅하고 메뉴 항목을 선택 하는 작업:

[![새 동작을 테스트](menu-images/action05.png "새 동작 테스트")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>추가, 편집 및 삭제 메뉴

이전 섹션에서 살펴본 대로 Xamarin.Mac 응용 프로그램을 미리 설정 된 수의 기본 메뉴와 특정 UI 컨트롤은 자동으로 활성화 하 고 응답할 수 있는 메뉴 항목 제공 됩니다. 또한 사용 하도록 설정 하 고 이러한 기본 항목에 응답 하는 응용 프로그램 코드를 추가 하는 방법 또한 살펴보았습니다.

이 섹션에서 살펴보겠습니다 필요 하지 않은 메뉴 항목을 제거 하 고 메뉴를 다시 구성 하면 새 메뉴, 메뉴 항목 및 작업을 추가 합니다.

두 번 클릭 합니다 **Main.storyboard** 파일는 **Solution Pad** 열어 편집 하려면:

[![UI Xcode에서 편집](menu-images/maint01.png "UI Xcode에서 편집")](menu-images/maint01-large.png#lightbox)

특정 Xamarin.Mac 응용 프로그램에 대 한 것 하지 않는 기본값 사용 **보기** 메뉴를 제거 하겠습니다. 에 **인터페이스 계층 구조** 선택 합니다 **보기** 주 메뉴 표시줄의 포함 된 메뉴 항목:

![보기 메뉴 항목 선택](menu-images/maint02.png "보기 메뉴 항목 선택")

삭제를 누르거나 메뉴를 삭제 하려면 백스페이스 합니다. 다음으로, 않겠습니다에서 항목을 모두 사용 합니다 **형식** 메뉴 아웃 하위 메뉴에서 사용 하려는 항목을 이동 하려고 합니다. 에 **인터페이스 계층 구조** 다음 메뉴 항목을 선택 합니다.

![여러 항목을 강조 표시](menu-images/maint03.png "여러 항목을 강조 표시")

부모 아래에 있는 항목을 끌어 **메뉴** 현재 되 고 하위 메뉴에서:

[![부모 메뉴에 메뉴 항목을 끌어](menu-images/maint04.png "부모 메뉴에 메뉴 항목을 끌어")](menu-images/maint04-large.png#lightbox)

이제 메뉴가 같습니다.

[![새 위치에 있는 항목](menu-images/maint05.png "새 위치에 있는 항목")](menu-images/maint05-large.png#lightbox)

다음 끌어 보겠습니다 합니다 **텍스트** 아웃 하위 메뉴 아래에서 **형식** 메뉴 사이의 주 메뉴 모음에 배치 하는 **형식** 및 **창** 메뉴:

[![텍스트 메뉴](menu-images/maint06.png "The 텍스트 메뉴")](menu-images/maint06-large.png#lightbox)

다시 돌아가 보겠습니다 합니다 **형식** 메뉴와 삭제는 **글꼴** 하위 메뉴 항목입니다. 다음으로, 선택는 **형식** 메뉴 "글꼴"로 이름을 바꿉니다.

[![글꼴 메뉴](menu-images/maint07.png "The 글꼴 메뉴")](menu-images/maint07-large.png#lightbox)

다음으로 자동으로 가져올 텍스트에 추가 텍스트 뷰에서 선택 하면 미리 정의 된 구의 사용자 지정 메뉴를 만들어 보겠습니다. 맨 위에 있는 검색 상자에는 **라이브러리 검사기** 유형 "메뉴"로 지정 합니다. 이 쉽게 찾아서 모든 메뉴 UI 요소를 사용 하 여 작업:

![라이브러리 검사기](menu-images/maint08.png "라이브러리 검사기")

이제이 메뉴를 만들려면 다음을 수행 하겠습니다.

1. 끌어서를 **메뉴 항목** 에서 **라이브러리 검사기** 간에 메뉴 모음에는 **텍스트** 하 고 **창** 메뉴: 

    ![라이브러리에 새 메뉴 항목을 선택 하면](menu-images/maint10.png "라이브러리에 새 메뉴 항목을 선택 합니다.")
2. "구" 항목의 이름을 바꾸려면 

    [![메뉴 이름 설정](menu-images/maint09.png "메뉴 이름 설정")](menu-images/maint09-large.png#lightbox)
3. 다음 끌어를 **메뉴** 에서 합니다 **라이브러리 검사기**: 

    ![라이브러리에서 메뉴를 선택 하면](menu-images/maint11.png "라이브러리에서 메뉴를 선택 합니다.")
4. 다음 drop **메뉴** 새 **메뉴 항목** 방금 생성 하 고 해당 이름을 "구"로 변경 합니다. 

    [![편집 메뉴 이름을](menu-images/maint12.png "메뉴 이름 편집")](menu-images/maint12-large.png#lightbox)
5. 이제 세 가지 기본 이름을 **메뉴 항목** "Address", "날짜" 및 "Greeting": 

    [![구 메뉴](menu-images/maint13.png "The 구 메뉴")](menu-images/maint13-large.png#lightbox)
6. 네 번째를 추가 해 보겠습니다 **메뉴 항목** 끌어를 **메뉴 항목** 에서 합니다 **라이브러리 검사기** "서명"을 호출 하 고: 

    [![메뉴 항목 이름을 편집](menu-images/maint14.png "메뉴 항목 이름 편집")](menu-images/maint14-large.png#lightbox)
7. 메뉴 모음에 저장 합니다.

이제 C# 코드에 노출 되는 새 메뉴 항목을 사용자 지정 동작의 집합을 만들어 보겠습니다. Xcode에서으로 전환 해 보겠습니다 합니다 **Assistant** 보기:

[![필요한 작업을 만드는](menu-images/maint15.png "필요한 작업 만들기")](menu-images/maint15-large.png#lightbox)

다음을 수행 하겠습니다.

1. 컨트롤 끌기 합니다 **주소** 메뉴 항목에는 **AppDelegate.h** 파일입니다.
2. 스위치를 **연결** 형식을 **동작**: 

    [![작업 유형 선택](menu-images/maint17.png "작업 유형 선택")](menu-images/maint17-large.png#lightbox)
3. 입력을 **이름** 누릅니다 "phraseAddress"는 **Connect** 새 작업을 만드는 단추: 

    [![동작 구성](menu-images/maint18.png "작업 구성")](menu-images/maint18-large.png#lightbox)
4. 에 대해 위의 단계를 반복 합니다 **날짜**를 **인사말**, 및 **서명을** 메뉴 항목: 

    [![완료 된 작업](menu-images/maint19.png "완료 된 작업")](menu-images/maint19-large.png#lightbox)
5. 메뉴 모음에 저장 합니다.

다음에서는 코드에서 해당 콘텐츠를 조정할 수 있도록 텍스트 보기에 대 한 콘센트를 만들어야 합니다. 선택 합니다 **ViewController.h** 파일을 **도우미 편집기** 를 호출 하는 새 콘센트를 만들어 `documentText`:

[![만드는 출 선](menu-images/maint20.png "아웃렛 만들기")](menu-images/maint20-large.png#lightbox)

Xcode에서 변경 내용을 동기화 하는 Mac 용 Visual Studio로 돌아갑니다. 다음 편집 합니다 **ViewController.cs** 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using System;

using AppKit;
using Foundation;

namespace MacMenus
{
    public partial class ViewController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }

        public string Text {
            get { return documentText.Value; }
            set { documentText.Value = value; }
        } 
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            App.textEditor = this;
        }

        public override void ViewWillDisappear ()
        {
            base.ViewDidDisappear ();

            App.textEditor = null;
        }
        #endregion
    }
}
```

외부의 텍스트 보기의 텍스트를 노출 하는이 `ViewController` 클래스 및 때 창이 포커스를 얻거나 잃을 앱 대리자에 게 알립니다. 이제 편집 합니다 **AppDelegate.cs** 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using AppKit;
using Foundation;
using System;

namespace MacMenus
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public ViewController textEditor { get; set;} = null;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
        #endregion

        #region Custom actions
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

        partial void phrasesAddress (Foundation.NSObject sender) {

            textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
        }

        partial void phrasesDate (Foundation.NSObject sender) {

            textEditor.Text += DateTime.Now.ToString("D");
        }

        partial void phrasesGreeting (Foundation.NSObject sender) {

            textEditor.Text += "Dear Sirs,\n\n";
        }

        partial void phrasesSignature (Foundation.NSObject sender) {

            textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
        }
        #endregion
    }
}
```

여기 만들었습니다는 `AppDelegate` 정의한 Interface Builder에서 출 선 및 작업을 사용할 수 있도록 partial 클래스입니다. 도 노출 한 `textEditor` 는 기간은 현재 포커스를 추적할 수 있습니다.

다음 메서드는 사용자 지정 메뉴와 메뉴 항목을 처리 하는 데 사용 됩니다.

```csharp
partial void phrasesAddress (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
}

partial void phrasesDate (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += DateTime.Now.ToString("D");
}

partial void phrasesGreeting (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Dear Sirs,\n\n";
}

partial void phrasesSignature (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
}
```

에 있는 항목의 모든 응용 프로그램을 실행 하는 경우 이제는 **구를** 메뉴 활성화 되 고 선택 하면 텍스트 뷰로 제공 라는 문구를 추가 합니다.

![실행 중인 앱의 예가](menu-images/maint21.png "실행 되는 앱의 예")

이제 아래로 응용 프로그램 메뉴를 사용 하 여 작업의 기초부터 했으므로 사용자 지정 상황에 맞는 메뉴를 만드는 살펴 보겠습니다.

### <a name="creating-menus-from-code"></a>코드에서 메뉴 만들기

Xcode의 Interface Builder를 사용 하 여 메뉴 및 메뉴 항목을 만들기, 하는 것 외에도 Xamarin.Mac 앱을 만들기, 수정 또는 코드에서 메뉴, 하위 메뉴 또는 메뉴 항목을 제거 해야 하는 경우 시간이 있을 수 있습니다.

다음 예제에서는 메뉴 항목 및 즉석에서 동적으로 생성 되는 하위 메뉴에 대 한 정보를 저장 하는 클래스가 만들어집니다.

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace AppKit.TextKit.Formatter
{
    public class LanguageFormatCommand : NSObject
    {
        #region Computed Properties
        public string Title { get; set; } = "";
        public string Prefix { get; set; } = "";
        public string Postfix { get; set; } = "";
        public List<LanguageFormatCommand> SubCommands { get; set; } = new List<LanguageFormatCommand>();
        #endregion

        #region Constructors
        public LanguageFormatCommand () {

        }

        public LanguageFormatCommand (string title)
        {
            // Initialize
            this.Title = title;
        }

        public LanguageFormatCommand (string title, string prefix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
        }

        public LanguageFormatCommand (string title, string prefix, string postfix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
            this.Postfix = postfix;
        }
        #endregion
    }
}
```

#### <a name="adding-menus-and-items"></a>메뉴 및 항목 추가

이 클래스 정의 사용 하 여 다음 루틴의 컬렉션인 분석 `LanguageFormatCommand`개체 및 재귀적으로 빌드는에 전달 된 (Interface Builder에서 만든) 기존 메뉴의 아래쪽에 추가 하 여 새 메뉴 및 메뉴 항목:

```csharp
private void AssembleMenu(NSMenu menu, List<LanguageFormatCommand> commands) {
    NSMenuItem menuItem;

    // Add any formatting commands to the Formatting menu
    foreach (LanguageFormatCommand command in commands) {
        // Add separator or item?
        if (command.Title == "") {
            menuItem = NSMenuItem.SeparatorItem;
        } else {
            menuItem = new NSMenuItem (command.Title);

            // Submenu?
            if (command.SubCommands.Count > 0) {
                // Yes, populate submenu
                menuItem.Submenu = new NSMenu (command.Title);
                AssembleMenu (menuItem.Submenu, command.SubCommands);
            } else {
                // No, add normal menu item
                menuItem.Activated += (sender, e) => {
                    // Apply the command on the selected text
                    TextEditor.PerformFormattingCommand (command);
                };
            }
        }
        menu.AddItem (menuItem);
    }
}
``` 

에 대 한 `LanguageFormatCommand` 비어 있는 개체 `Title` 속성을이 루틴 만듭니다를 **메뉴 항목 구분 기호** (씬 회색 선) 메뉴 섹션 간:

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

제목을 제공 하는 경우 해당 제목의 새 메뉴 항목을 만들어집니다.

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

경우는 `LanguageFormatCommand` 개체에 하위 `LanguageFormatCommand` 개체에 하위 메뉴가 만들어집니다 및 `AssembleMenu` 메서드는 재귀적으로 해당 메뉴를 구축 하기 위해 호출 됩니다.

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

하위 메뉴가 없는 모든 새 메뉴 항목에 대 한 코드는 사용자가 선택한 메뉴 항목 처리에 추가 됩니다.

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>테스트 메뉴 만들기

모든 위치에 위의 코드를 사용 하 여 하는 경우 다음 컬렉션 `LanguageFormatCommand` 개체를 만들었습니다.

```csharp
// Define formatting commands
FormattingCommands.Add(new LanguageFormatCommand("Stong","**","**"));
FormattingCommands.Add(new LanguageFormatCommand("Emphasize","_","_"));
FormattingCommands.Add(new LanguageFormatCommand("Inline Code","`","`"));
FormattingCommands.Add(new LanguageFormatCommand("Code Block","```\n","\n```"));
FormattingCommands.Add(new LanguageFormatCommand("Comment","<!--","-->"));
FormattingCommands.Add (new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Unordered List","* "));
FormattingCommands.Add(new LanguageFormatCommand("Ordered List","1. "));
FormattingCommands.Add(new LanguageFormatCommand("Block Quote","> "));
FormattingCommands.Add (new LanguageFormatCommand ());

var Headings = new LanguageFormatCommand ("Headings");
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 1","# "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 2","## "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 3","### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 4","#### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 5","##### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 6","###### "));
FormattingCommands.Add (Headings);

FormattingCommands.Add(new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Link","[","]()"));
FormattingCommands.Add(new LanguageFormatCommand("Image","![]("-")"));
FormattingCommands.Add(new LanguageFormatCommand("Image Link","[ ![]("-")](linkimagehere/index.md)"));
```

있으며이 컬렉션을 전달 합니다 `AssembleMenu` 함수 (사용 하 여는 **형식** 메뉴 기본 설정), 다음 동적 메뉴 및 메뉴 항목 생성:

![실행 중인 앱에서 새 메뉴 항목](menu-images/dynamic01.png "실행 중인 앱에서 새 메뉴 항목")

#### <a name="removing-menus-and-items"></a>메뉴 및 항목 제거

앱의 사용자 인터페이스에서 모든 메뉴 또는 메뉴 항목을 제거 해야 하는 경우 사용할 수 있습니다 합니다 `RemoveItemAt` 메서드는 `NSMenu` 클래스 0을 지정 하 여 기반 제거할 항목의 인덱스입니다.

예를 들어, 메뉴 및 위의 루틴에 의해 생성 되는 메뉴 항목을 제거 하려면 다음 코드를 사용할 수 있습니다.

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

위의 코드의 경우 처음 네 가지 메뉴 항목을 만들었으므로 Xcode의 Interface Builder 및 앱에서 사용할 수 있는 주요 사항에서 동적으로 제거 되지 않습니다.

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>상황에 맞는 메뉴

상황에 맞는 메뉴 사용자 마우스 오른쪽 단추로 클릭 하거나 창의 항목 컨트롤-클릭 하면 나타납니다. 기본적으로 (예: 텍스트 뷰)에 연결 하는 상황에 맞는 메뉴는 다양 한 이미 macOS에 기본 제공 되는 UI 요소입니다. 그러나 때 창에 추가 하는 UI 요소에 대 한 고유한 사용자 지정 상황에 맞는 메뉴를 생성 하려는 경우가 있을 수 있습니다.

편집할 우리의 **Main.storyboard** Xcode에서 파일을 추가 **창** 이 디자인 창을 설정 해당 **클래스** 에서 "NSPanel"에 **Id 검사기**, 새 **Assistant** 항목을 **창** 메뉴를 사용 하 여 새 창에 연결을 **Segue 표시**:

[![Segue 형식 설정](menu-images/context01.png "segue 형식 설정")](menu-images/context01-large.png#lightbox)

다음을 수행 하겠습니다.

1. 끌어서를 **레이블을** 에서 합니다 **라이브러리 검사기** 에 **패널** 창 "Property"를 해당 텍스트를 설정 하 고: 

    [![레이블 값을 편집](menu-images/context03.png "레이블의 값 편집")](menu-images/context03-large.png#lightbox)
2. 다음 끌어를 **메뉴** 에서 합니다 **라이브러리 검사기** 뷰 계층 구조 및 이름 바꾸기 세 가지 기본 메뉴 항목에서 뷰 컨트롤러를 **문서**, **텍스트**  하 고 **글꼴**:

    [![필요한 메뉴 항목](menu-images/context02.png "필요한 메뉴 항목")](menu-images/context02-large.png#lightbox)
3. 컨트롤 끌기 이제 합니다 **속성 레이블** 에 **메뉴**:

    [![Segue를 만들려면 끌기](menu-images/context04.png "끌기 segue를 만들려면")](menu-images/context04-large.png#lightbox)
4. 팝업 대화 상자에서 선택 **메뉴**: 

    ![Segue 형식 설정](menu-images/context05.png "segue 형식 설정")
5. **검사기**, "PanelViewController" 뷰 컨트롤러의 클래스를 설정 합니다. 

    [![Segue 클래스 설정](menu-images/context10.png "segue 클래스를 설정 합니다.")](menu-images/context10-large.png#lightbox)
6. 동기화 하는 Mac 용 Visual Studio로 다시 전환 하 면 Interface Builder를 반환 합니다.
7. 전환할 합니다 **도우미 편집기** 선택한 합니다 **PanelViewController.h** 파일입니다.
8. 에 대 한 작업 만들기를 **문서** 메뉴 항목 이라는 `propertyDocument`: 

    [![동작 구성](menu-images/context06.png "작업 구성")](menu-images/context06-large.png#lightbox)
9. 나머지 메뉴 항목에 대 한 만들기 작업을 반복 합니다. 

    [![필요한 작업을](menu-images/context07.png "필요한 작업")](menu-images/context07-large.png#lightbox)
10. 마지막에 대 한 콘센트를 작성 합니다 **속성 레이블** 호출 `propertyLabel`: 

    [![구성 콘센트](menu-images/context08.png "콘센트를 구성 합니다.")](menu-images/context08-large.png#lightbox)
11. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

편집 된 **PanelViewController.cs** 파일과 다음 코드를 추가 합니다.

```csharp
partial void propertyDocument (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Document";
}

partial void propertyFont (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Font";
}

partial void propertyText (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Text";
}
```

이제 응용 프로그램을 실행 하 고 속성 레이블 패널에서 마우스 오른쪽 단추로 클릭, 우리의 사용자 지정 상황에 맞는 메뉴를 알아봅니다. 선택 하 고 메뉴에서 항목을 레이블의 값이 변경 됩니다.

![실행 상황에 맞는 메뉴](menu-images/context09.png "실행 상황에 맞는 메뉴")

다음 상태 표시줄 메뉴 만들기에 대해 살펴보겠습니다.

## <a name="status-bar-menus"></a>상태 표시줄 메뉴

상태 표시줄 메뉴는 메뉴 또는 응용 프로그램의 상태를 반영 하며 이미지와 같은 사용자에 게 피드백와의 상호 작용을 제공 하는 상태 메뉴 항목의 컬렉션을 표시 합니다. 응용 프로그램의 상태 표시줄 메뉴 응용 프로그램이 백그라운드에서 실행 되는 경우에 되 고 활성화 됩니다. 시스템 차원의 상태 표시줄 응용 프로그램 메뉴 표시줄의 오른쪽에 상주 하며 현재 macOS에서 사용할 수 있는 유일한 상태 표시줄입니다.

편집할 우리의 **AppDelegate.cs** 파일을 확인 합니다 `DidFinishLaunching` 다음과 같은 메서드 확인:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Create a status bar menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Text";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Text");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        PhraseAddress(address);
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        PhraseDate(date);
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        PhraseGreeting(greeting);
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        PhraseSignature(signature);
    };
    item.Menu.AddItem (signature);
}
```

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` 액세스를 제공 시스템 차원의 상태 표시줄에 있습니다. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` 새 상태 모음 항목을 만듭니다. 여기에서 메뉴 및 메뉴 항목의 수를 만들고 메뉴를 방금 만든 상태 표시줄 항목에 연결 합니다. 

응용 프로그램을 실행 하는 경우 새 상태 모음 항목 표시 됩니다. 메뉴에서 항목을 선택 하면 텍스트 보기에 있는 텍스트를 변경 됩니다. 

![실행 상태 표시줄 메뉴](menu-images/statusbar01.png "실행 상태 표시줄 메뉴")

다음으로, 사용자 지정 도킹 메뉴 항목을 만들기에 대해 살펴보겠습니다.

## <a name="custom-dock-menus"></a>사용자 지정 도킹 메뉴

도킹 메뉴를 마우스 오른쪽 단추로 클릭 또는 도킹 스테이션에서 응용 프로그램의 아이콘 컨트롤-클릭 하는 경우 Mac 응용 프로그램에 대 한 표시 됩니다.

![사용자 지정 메뉴 도킹](menu-images/dock01.png "사용자 지정 도킹 메뉴")

다음을 수행 하 여 응용 프로그램에 대 한 사용자 지정 도킹 메뉴를 만들어 보겠습니다.

1. Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 응용 프로그램의 프로젝트 및 선택 **추가** > **새 파일...** 새 파일 대화 상자에서 선택 **Xamarin.Mac** > **빈 인터페이스 정의**, "DockMenu" 하는 데는 **이름** 을 클릭 합니다 **새로 만들기**  만들 새 단추 **DockMenu.xib** 파일:

    ![빈 인터페이스 정의 추가](menu-images/dock02.png "빈 인터페이스 정의 추가 합니다.")
2. 에 **Solution Pad**, 두 번 클릭 합니다 **DockMenu.xib** Xcode에서 편집 하기 위해 열려는 파일입니다. 새 **메뉴** 다음 항목과: **주소**에 **날짜**를 **인사말**, 및 **서명** 

    [![UI를 레이아웃할](menu-images/dock03.png "UI 레이아웃")](menu-images/dock03-large.png#lightbox)
3. 그런 다음에 사용자 지정 메뉴에 대 한 만든이 기존 작업에는 새 메뉴 항목을 연결 하겠습니다 합니다 [추가, 편집 및 삭제 메뉴](#Adding,_Editing_and_Deleting_Menus) 위의 섹션. 전환할 합니다 **연결 검사기** 선택한를 **첫 번째 응답자** 에 **인터페이스 계층 구조**. 아래로 스크롤하여 찾을 `phraseAddress:` 동작 합니다. 원 줄 해당 작업을 끌기에 합니다 **주소** 메뉴 항목:

    [![연결 작업을 끌어](menu-images/dock04.png "끌기 작업 연결")](menu-images/dock04-large.png#lightbox)
4. 모든 해당 액션에 연결할 다른 메뉴 항목에 대해 반복 합니다. 

    [![필요한 작업을](menu-images/dock05.png "필요한 작업")](menu-images/dock05-large.png#lightbox)
5. 다음으로, 선택는 **응용 프로그램** 에 **인터페이스 계층 구조**합니다. 에 **연결 검사기**, 원을에서 줄을 끌어 놓습니다는 `dockMenu` 작동 시키기 위해 방금 만든 메뉴:

    [![콘센트를 연결할 끌어](menu-images/dock06.png "콘센트를 연결할 끌어")](menu-images/dock06-large.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 다시 전환 합니다.
7. 두 번 클릭 합니다 **Info.plist** 파일을 편집용으로 엽니다. 

    [![Info.plist 파일 편집](menu-images/dock07.png "Info.plist 파일 편집")](menu-images/dock07-large.png#lightbox)
8. 클릭 합니다 **원본** 화면 아래쪽에 있는 탭: 

    [![원본 뷰 선택](menu-images/dock08.png "원본 뷰 선택")](menu-images/dock08-large.png#lightbox)
9. 클릭 **새 항목 추가**녹색 더하기 단추 클릭, "AppleDockMenu"에 속성 이름 및 "DockMenu" (확장명 없이 새.xib 파일의 이름) 값을 설정 합니다. 

    [![DockMenu 항목 추가](menu-images/dock09.png "DockMenu 항목 추가")](menu-images/dock09-large.png#lightbox)

이제 응용 프로그램을 실행 하 고 도킹 스테이션에서 해당 아이콘을 마우스 오른쪽 단추로 클릭, 우리의 새 메뉴 항목이 표시 됩니다.

![도킹 메뉴 실행 예가](menu-images/dock10.png "실행 도킹 메뉴의 예")

메뉴에서 사용자 지정 항목 중 하나를 선택, 텍스트 뷰에 텍스트 수정 됩니다.

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>팝업 단추 및 드롭다운 목록

팝업 단추는 선택한 항목을 표시 하 고 사용자가 클릭할 때 선택할 옵션 목록을 제공 합니다. 드롭다운 목록에는 일반적으로 현재 작업의 컨텍스트에 관련 특정 명령을 선택할 때 사용 되는 팝업 단추 형식입니다. 둘 다 창의 아무 곳 이나 나타날 수 있습니다.

다음을 수행 하 여 응용 프로그램에 대 한 사용자 지정 팝업 단추를 만들어 보겠습니다.

1. 편집 합니다 **Main.storyboard** 놓습니다 Xcode에서 파일을 **팝업 단추** 에서 **라이브러리 검사기** 에 **패널** 창에서 만든 합니다 [상황에 맞는 메뉴](#Contextual_Menus) 섹션: 

    [![단추 추가 하는 팝업](menu-images/popup01.png "팝업 단추 추가")](menu-images/popup01-large.png#lightbox)
2. 새 메뉴 항목을 추가 하 고를 팝업에서 항목의 제목을 설정: **주소**, **날짜**하십시오 **인사말**, 및 **서명** 

    [![메뉴 항목을 구성](menu-images/popup02.png "메뉴 항목 구성")](menu-images/popup02-large.png#lightbox)
3. 다음으로 사용자 지정 메뉴에서 만든 기존 작업에는 새 메뉴 항목을 연결 하겠습니다 합니다 [추가, 편집 및 삭제 메뉴](#Adding,_Editing_and_Deleting_Menus) 위의 섹션입니다. 전환할 합니다 **연결 검사기** 선택한를 **첫 번째 응답자** 에 **인터페이스 계층 구조**. 아래로 스크롤하여 찾을 `phraseAddress:` 동작 합니다. 원 줄 해당 작업을 끌기에 합니다 **주소** 메뉴 항목: 

    [![연결 작업을 끌어](menu-images/popup03.png "끌기 작업 연결")](menu-images/popup03-large.png#lightbox)
4. 모든 해당 액션에 연결할 다른 메뉴 항목에 대해 반복 합니다. 

    [![작업에 필요한 모든](menu-images/popup04.png "모든 필수 작업")](menu-images/popup04-large.png#lightbox)
5. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 다시 전환 합니다.

이제 응용 프로그램을 실행 하 고 팝업에서 항목을 선택, 텍스트 보기에 있는 텍스트 변경 됩니다.

![실행 팝업의 예로](menu-images/popup05.png "실행 팝업의 예")

수 만들고 팝업 단추와 정확히 동일한 방식으로 풀 다운 목록을 사용 하 여 작동 합니다. 기존 작업에 연결 하는 대신 만들 수는 상황에 맞는 메뉴에서 수행한 것 처럼 사용자 고유의 사용자 지정 작업을 [상황에 맞는 메뉴](#Contextual_Menus) 섹션입니다.

## <a name="summary"></a>요약

이 문서에서는 자세히 살펴보고 메뉴 및 Xamarin.Mac 응용 프로그램에서 메뉴 항목을 사용 하 여 작업을 수행 했습니다. 먼저 하 고 상황에 맞는 메뉴를 만들기에 대해 살펴보았습니다, 다음 상태 표시줄 메뉴 검사할 사용자 지정 도킹 메뉴 응용 프로그램의 메뉴 표시줄을 검사 했습니다. 마지막으로 앞서 설명한 팝업 메뉴 및 풀 다운 목록.


## <a name="related-links"></a>관련 링크

- [MacMenus (샘플)](https://developer.xamarin.com/samples/mac/MacMenus/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [휴먼 인터페이스 지침-메뉴](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [응용 프로그램 메뉴 및 팝업 목록 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
