---
title: 메뉴
description: 이 문서에서는 Xamarin.Mac 응용 프로그램에서 메뉴 작업에 설명 합니다. 만들기 및 메뉴 및 메뉴 항목 Xcode 및 인터페이스 작성기에서 유지 관리 및 이러한 작업을 프로그래밍 방식으로 설명 합니다.
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 50c9cf333ff7965bbdfbb964a2301e677eb6aa59
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="menus"></a>메뉴

_이 문서에서는 Xamarin.Mac 응용 프로그램에서 메뉴 작업에 설명 합니다. 만들기 및 메뉴 및 메뉴 항목 Xcode 및 인터페이스 작성기에서 유지 관리 및 이러한 작업을 프로그래밍 방식으로 설명 합니다._

를 사용할 때 C# 및.NET Xamarin.Mac 응용 프로그램에서 개발자 Objective C 및 Xcode에서 작업을 수행 하는 동일한 Cocoa 메뉴에는 액세스할을 수 있습니다. Xamarin.Mac Xcode에 직접 통합, 때문에 만들기 및 메뉴 모음, 메뉴 및 메뉴 항목을 유지 관리 (또는 필요에 따라 C# 코드에서 직접 만들) Xcode의 인터페이스 작성기를 사용할 수 있습니다.

메뉴는 Mac 응용 프로그램 사용자 경험의 필수적인 부분 중 이며 일반적으로 사용자 인터페이스의 여러 부분에 나타납니다.

- **응용 프로그램의 메뉴 모음** -모든 Mac 응용 프로그램에 대 한 화면 위쪽에 표시 되는 주 메뉴입니다.
- **상황에 맞는 메뉴** -이러한 사용자가 해야 하는지 또는 컨트롤 창에서 항목 때 나타납니다.
- **상태 표시줄** -(의 왼쪽에 있는 메뉴 모음 클록) 화면 위쪽에 표시 및에 항목이 추가 되는 왼쪽으로 증가 하는 응용 프로그램 메뉴 표시줄의 오른쪽 끝에 있는 영역입니다.
- **도킹 메뉴** -도킹 스테이션에 각 응용 프로그램에 대 한 메뉴 나타나는 경우 사용자가 해야 하는지 또는 제어 응용 프로그램의 아이콘 또는 사용자가 아이콘 한 마우스 단추를 누르고 경우.
- **팝업 단추 및 풀 다운 목록** -팝업 단추 선택된 된 항목을 표시 하 고 사용자가 클릭할 때에서 선택 하는 옵션의 목록을 제공 합니다. 풀 다운 목록은 일반적으로 현재 작업 컨텍스트에 특정 명령을 선택 하는 데 사용 되는 팝업 단추의 한 종류입니다. 둘 다 창에서 아무 곳 이나 나타날 수 있습니다.

[![예제에서는 메뉴](menu-images/intro01.png "예제 메뉴")](menu-images/intro01-large.png#lightbox)

이 문서는 기본적인 Cocoa 메뉴 모음, 메뉴 및 Xamarin.Mac 응용 프로그램에서 메뉴 항목을 사용 하겠습니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 섹션으로이 문서에서 사용할 수 있는 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 특성 와이어 접속 C# 클래스 Objective-c 개체 및 UI 요소에 하는 데 사용 합니다.

## <a name="the-applications-menu-bar"></a>응용 프로그램의 메뉴 모음 

모든 창의 연결 된 메뉴 모음 자체를 가질 수 있는 Windows 운영 체제에서 실행 되는 응용 프로그램과 달리 macOS에서 실행 중인 모든 응용 프로그램에는 해당 응용 프로그램의 모든 창에 사용 되는 화면 맨 실행 되는 단일 메뉴 모음에 있습니다.

[![메뉴 모음](menu-images/appmenu01.png "메뉴 모음")](menu-images/appmenu01-large.png#lightbox)

이 메뉴 모음에 있는 항목은 활성화 또는 비활성화 하는 지정된 된 시점에는 현재 컨텍스트 또는 응용 프로그램 및 해당 사용자 인터페이스의 상태를 기반 합니다. 예를 들어: 사용자가 텍스트 필드에 선택 항목에 **편집** 메뉴와 같은 사용 가능한 상태가 됩니다 **복사** 및 **잘라내기**합니다.

Apple에 따라 및 기본적으로 모든 macOS 응용 프로그램 메뉴 및 응용 프로그램의 메뉴 모음에 나타나는 메뉴 항목의 표준 집합을 가집니다.

- **Apple 메뉴** -이 메뉴 어떤 응용 프로그램을 실행 하는 것에 관계 없이 항상 사용자에 게 사용할 수 있는 넓은 항목 시스템에 대 한 액세스를 제공 합니다. 개발자가 이러한 항목을 수정할 수 없습니다.
- **응용 프로그램 메뉴** -이 메뉴 굵게 표시 된 응용 프로그램의 이름을 표시 하 고를 통해 사용자는 현재 실행 중인 응용 프로그램을 식별 합니다. 전체 및 하지 특정된 문서 또는 응용 프로그램을 종료 하는 같은 프로세스와 응용 프로그램에 적용 되는 항목을 포함 합니다.
- **파일 메뉴** -항목 만들기, 열기, 하는 데 사용 하거나 저장 된 문서와 응용 프로그램이 작동 합니다. 응용 프로그램은 문서 기반,이 메뉴를 이름이 변경 또는 제거할 수 있습니다.
- **편집 메뉴** -과 같은 명령을 유지 **잘라내기**, **복사**, 및 **붙여넣기** 편집 하거나 응용 프로그램의 사용자 인터페이스에서 요소를 수정 하는 데 사용 되는 합니다.
- **서식 메뉴** -보류 명령을 해당 텍스트의 형식을 조정 하려면이 메뉴의 텍스트가 포함 된 응용 프로그램이 작동 하는 경우.
- **보기 메뉴** -명령에 영향을 주는 콘텐츠가 표시 되는 방식에 응용 프로그램의 사용자 인터페이스 (표시)을 보유 합니다.
- **응용 프로그램별 메뉴** -(예: 웹 브라우저에 대 한 책갈피 메뉴) 응용 프로그램에 있는 모든 메뉴는 합니다. 사이의 표시 되어야 하는 **보기** 및 **창** 메뉴 모음에서 합니다.
- **창 메뉴** -현재 열려 있는 창 목록이 뿐만 아니라 응용 프로그램에서 windows를 사용 하기 위한 명령이 포함 되어 있습니다.
- **도움말 메뉴** -도움말 메뉴 모음에서 맨 오른쪽 메뉴 여야 합니다 응용 프로그램에서 온라인 도움말을 제공 합니다. 

응용 프로그램 메뉴 모음 및 표준 메뉴 및 메뉴 항목에 대 한 자세한 내용은 Apple의를 참조 하십시오 [Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)합니다.

### <a name="the-default-application-menu-bar"></a>기본 응용 프로그램 메뉴 모음

새 Xamarin.Mac 프로젝트를 만들 때마다 자동으로 macOS 응용 프로그램 (위의 섹션에서 설명)으로 게 일반적으로 있는 일반적인 항목이 포함 된 표준 기본 응용 프로그램 메뉴 모음을 가져옵니다. 에 정의 되어 있는 응용 프로그램의 기본 메뉴 모음에서 **Main.storyboard** 파일 (응용 프로그램의 UI의 나머지 부분)에서 프로젝트는 **솔루션 패드**:  

![주 스토리 보드 선택](menu-images/appmenu02.png "주 스토리 보드를 선택 합니다.")

두 번 클릭은 **Main.storyboard** 메뉴 편집기 인터페이스와 함께 제공 됩니다 Xcode의 인터페이스 작성기에서 편집에 대 한 열 파일입니다.

[![Xcode에서 UI 편집](menu-images/defaultbar01.png "Xcode에서 UI를 편집 합니다.")](menu-images/defaultbar01-large.png#lightbox)

여기에서 우리 항목을 클릭 수와 같은 **열려** 메뉴 항목에는 **파일** 메뉴 편집 하거나 해당 속성을 조정 하 고는 **특성 검사기**:

[![메뉴의 특성을 편집](menu-images/defaultbar02.png "메뉴의 속성 편집")](menu-images/defaultbar02-large.png#lightbox)

추가, 편집 및 삭제 메뉴와이 문서의 뒷부분에 나오는 항목을 살펴보겠습니다. 이제 방금 확인 하려는 기본으로 사용할 수 있는 메뉴 및 메뉴 항목 및 방법을 자동으로 표시 되지 미리 정의 된 콘센트 / 작업 집합을 통해 코드에 대 한 (자세한 내용은 참조는 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 설명서)입니다.

예를 들어에서 클릭 하면는 **연결 검사기** 에 대 한는 **열려** 메뉴 항목이 자동으로 연결 되는 최대 볼 수 있습니다는 `openDocument:` 동작: 

[![연결 된 작업을 보기](menu-images/defaultbar03.png "연결 된 작업 보기")](menu-images/defaultbar03-large.png#lightbox)

선택 하는 경우는 **첫 번째 응답자** 에 **인터페이스 계층 구조** 에서 아래로 스크롤하여 및는 **연결 검사기**의 정의 볼 수 있습니다는 `openDocument:` 작업 하는 **열려** 메뉴 항목 (함께 여러 가지 다른 기본 작업 응용 프로그램에 대 한 이며 반드시 컨트롤까지 자리 자동으로 있는)에 연결 된:

[![연결 된 모든 작업을 보기](menu-images/defaultbar04.png "모든 연결 된 동작 보기")](menu-images/defaultbar04-large.png#lightbox) 

이유는이 중요 한가요? 다음에서 섹션으로 자동으로 사용 하도록 설정 및 메뉴 항목을 사용 하지 않도록 설정, 항목에 대 한 기본 제공 기능을 제공 합니다. 다른 Cocoa 사용자 인터페이스 요소와 이러한 자동으로 정의 된 작업 어떻게 표시 되는지 확인 됩니다.

나중에 사용할 예정 이러한 기본 제공 동작을 사용 하도록 설정 하 고 코드에서 항목을 사용 하지 않도록 설정 하 고 선택할 때 고유한 기능을 제공 합니다.

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>기본 제공 메뉴 기능

일부 항목이 자동으로 유선 접속을 사용할 수 있습니다 (완벽 하 게 기능으로 자동으로 기본 제공)와 같은 모든 UI 항목 또는 코드를 추가 하기 전에 새로 만든된 Xamarin.Mac 응용 프로그램 실행 인 경우 알 수 있습니다는 **Quit** 항목에 **앱** 메뉴:

![활성화 되지 않은 메뉴 항목](menu-images/appmenu03.png "활성화 되지 않은 메뉴 항목")

와 같은 다른 메뉴 항목과 동안 **잘라내기**, **복사**, 및 **붙여넣기** 되지 않습니다.

![메뉴 항목을 사용 하지 않도록 설정](menu-images/appmenu04.png "메뉴 항목을 사용 하지 않도록 설정")

응용 프로그램을 중지 하 고 두 번 클릭 해 보겠습니다는 **Main.storyboard** 파일에 **솔루션 패드** Xcode에서 편집 하기 위해 열려는 인터페이스 작성기의 합니다. 를 끌어 한 **텍스트 보기** 에서 **라이브러리** 창의 보기 컨트롤러에는 **인터페이스 편집기**:

[![라이브러리에서 텍스트 뷰를 선택 하면](menu-images/appmenu05.png "라이브러리에서 텍스트 뷰를 선택 합니다.")](menu-images/appmenu05-large.png#lightbox)

에 **제약 조건 편집기** 텍스트 뷰 창의 가장자리를 고정 하 고 증가 하 고 모든 4 개의 빨간색 I-빔 편집기의 맨 위쪽에 클릭 하 고 클릭 하 여 창 축소 설정 하겠습니다는 **4 제약 조건을추가** 단추:

[![제약 조건 편집](menu-images/appmenu06.png "제약 조건 편집")](menu-images/appmenu06-large.png#lightbox)

사용자 인터페이스 디자인에 변경 내용을 저장 하 고 변경 내용을 Xamarin.Mac 프로젝트와 동기화 하는 Mac에 대 한 Visual Studio를 다시 전환 합니다. 이제 응용 프로그램을 시작, 텍스트 보기에 텍스트를 입력, 선택한 열은 **편집** 메뉴:

![메뉴 항목은 자동으로 사용/사용 안 함](menu-images/appmenu07.png "메뉴 항목은 자동으로 사용/사용 안 함")

알림 방법을 **잘라내기**, **복사**, 및 **붙여넣기** 항목 자동으로 사용 하도록 설정 되어 코드 한 줄도 작성 하지 않고도 완전 한 기능을 합니다. 

여기에 무슨 일이 일어나고 있나요? 기억 (위에서 설명한) 처럼 기본 메뉴 항목 하도록 유선 오는 함수를 미리 정의 하는 기본 제공, 특정 작업을 수행할 후크 기본적으로 대부분의 macOS의 일부인 Cocoa 사용자 인터페이스 요소 (예: `copy:`). 따라서 활성 창에 추가 하 고 해당 메뉴 항목 또는 항목에 연결 된 선택한 때 해당 작업이 자동으로 활성화 됩니다. 사용자가 해당 메뉴 항목을 선택 하는 경우 UI 요소에 기본 제공 기능이 호출 되 고 모든 개발자의 개입 없이 실행 합니다.

### <a name="enabling-and-disabling-menus-and-items"></a>설정 및 해제 메뉴 및 항목

기본적으로 사용자 이벤트가 발생할 때마다 `NSMenu` 자동으로 사용 하도록 설정 하 고 각 표시 된 메뉴 및 메뉴 항목에 기반 하는 응용 프로그램 환경에서 사용 하지 않도록 설정 합니다. 세 가지 방법으로를 설정/해제 항목.

- **자동 메뉴를 사용 하도록 설정** -메뉴 항목은 경우 사용할 `NSMenu` 항목은 유선에 접속 하는 동작에 응답 하는 적절 한 개체를 찾을 수 있습니다. 예를 들어 텍스트 위의 보기를 기본 제공 후크 수 있었던는 `copy:` 동작 합니다.
- **사용자 지정 작업 및 validateMenuItem:** -에 바인딩되는 메뉴 항목에 대 한는 [컨트롤러 사용자 지정 작업 창 또는 보기](#Working-with-Custom-Window-Actions), 추가할 수 있습니다는 `validateMenuItem:` 동작 하 고 수동으로 사용 하도록 설정 하거나 메뉴 항목을 사용 하지 않도록 설정 합니다.
- **수동 메뉴를 사용 하도록 설정** -수동으로 설정는 `Enabled` 각 속성 `NSMenuItem` 을 개별적으로 메뉴의 각 항목을 사용 합니다.

시스템을 선택 하려면 설정는 `AutoEnablesItems` 속성은 `NSMenu`합니다. `true` 자동 (기본 동작) 및 `false` 수동입니다. 

> [!IMPORTANT]
> 수동 메뉴를 사용 하도록 설정 사용 하기로 선택한 경우 없음 메뉴의 항목을 같은 AppKit 클래스에 의해 제어 이더라도 `NSTextView`를 자동으로 업데이트 됩니다. 사용자 설정 및 코드에서 직접 모든 항목을 해제 해야 합니다.

#### <a name="using-validatemenuitem"></a>ValidateMenuItem를 사용 하 여

에 바인딩된 모든 메뉴 항목에 대해 설명한 것 처럼는 [창 또는 보기 컨트롤러 사용자 지정 작업](#Working-with-Custom-Window-Actions), 추가할 수 있습니다는 `validateMenuItem:` 동작 하 고 수동으로 사용 하도록 설정 하거나 메뉴 항목을 사용 하지 않도록 설정 합니다.

다음 예제에서는 `Tag` 속성을 사용 하는 수 사용/사용 안 함에서 메뉴 항목의 유형을 결정 하는 `validateMenuItem:` 에서 선택한 텍스트의 상태를 기준으로 작업 한 `NSTextView`합니다. `Tag` 각 메뉴 항목에 대 한 인터페이스 작성기에서 속성이 설정 되어 있습니다.

![Tag 속성 설정](menu-images/validate01.png "Tag 속성 설정")

다음 코드 보기 컨트롤러에 추가 하 고:

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

이 코드가 실행 되 고에서 선택한 텍스트가 없는 경우는 `NSTextView`, 줄 바꿈 두 메뉴 항목은도 사용할 수 없습니다 (뷰 컨트롤러에서 작업을 수행할 것은 유선):

![사용할 수 없게 표시 항목](menu-images/validate02.png "사용할 수 없는 항목 표시")

텍스트 섹션을 선택 하는 경우 메뉴에 다시 열에 두 줄 바꿈 메뉴 항목을 사용할 수 있습니다.

![표시 된 사용 항목](menu-images/validate03.png "보여 주는 항목을 사용 하도록 설정")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>사용 하도록 설정 하 고 코드에서 메뉴 항목에 응답

특정 Cocoa 사용자 인터페이스 요소 (예: 텍스트 필드의 경우) UI 디자인에 추가 하 여 위에서 살펴본 대로 기본 메뉴 항목의 몇 가지를 사용할 수 있으며 코드를 작성할 필요 없이 자동으로 작동 합니다. 다음 메뉴 항목을 사용 하도록 설정 하 고 사용자가 기능을 제공 하려면 우리의 Xamarin.Mac 프로젝트에 C# 코드를 추가에 대해 살펴보겠습니다.

예를 들어 할 수 있도록된 의견을 언급 해서도 원하는 사용자를 사용할 수는 **열려** 항목에 **파일** 메뉴 폴더를 선택 합니다. 응용 프로그램 수준 함수 하 고 제공 창이 나 UI 요소에 제한 되지,이 응용 프로그램 대리자에 게이 처리 하는 코드를 추가 하겠습니다.

**솔루션 패드**, 두 번 클릭는 `AppDelegate.CS` 편집을 위해 열 파일입니다.

![응용 프로그램 대리자를 선택 하면](menu-images/appmenu08.png "앱 대리자를 선택 합니다.")

아래에 다음 코드를 추가 `DidFinishLaunching` 메서드:

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

이제 응용 프로그램을 실행 하 고 열 하겠습니다는 **파일** 메뉴: 

![파일 메뉴](menu-images/appmenu09.png "파일 메뉴")

에 **열려** 메뉴 항목은 이제 사용 하도록 설정 합니다. 를 선택 하는 경우 열기 대화 상자가 표시 됩니다.

![열기 대화](menu-images/appmenu10.png "가 열기 대화 상자")

클릭 하면는 **열려** 단추, 우리의 경고 메시지가 표시 됩니다.

![예제 대화 메시지](menu-images/appmenu11.png "예제 대화 메시지")

키 통화 여기 `[Export ("openDocument:")]`, 인지 `NSMenu` 하는 우리의 **AppDelegate** 메서드가 `void OpenDialog (NSObject sender)` 에 대해 응답 하는 `openDocument:` 동작 합니다. 위쪽에서 기억 하는 경우는 **열려** 메뉴 항목은 자동으로 유선 접속이이 동작에 인터페이스 작성기에서 기본적으로:

[![연결 된 작업을 보기](menu-images/defaultbar03.png "연결 된 작업 보기")](menu-images/defaultbar03-large.png#lightbox)

다음에 대해 살펴보겠습니다 고유한 메뉴, 메뉴 항목 및 작업을 만들고 응답 코드에서 항목에 있습니다.

### <a name="working-with-the-open-recent-menu"></a>최근 열기 메뉴 사용

기본적으로는 **파일** 메뉴에는 **열기** 마지막 여러 파일을 앱과 사용자가 열었습니다을 추적 하는 항목입니다. 만드는 경우는 `NSDocument` 기반 Xamarin.Mac 앱이이 메뉴를에 자동으로 처리 합니다. Xamarin.Mac 응용 프로그램의 다른 모든 형식에 대 한를 관리 하 고 수동으로이 메뉴 항목에 응답 해야 합니다.

직접 처리 된 **열기** 메뉴는 먼저 새 파일을 열거나 다음을 사용 하 여 저장 된에 알리기 위해.

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

앱을 사용 하지 않는 경우에 `NSDocuments`, 여전히 사용는 `NSDocumentController` 유지 관리 하는 **열기** 전송 하 여 메뉴는 `NSUrl` 파일의 위치와는 `NoteNewRecentDocumentURL` 의 메서드는 `SharedDocumentController`합니다.

재정의 해야 하는 다음으로 `OpenFile` 앱에서 사용자가 선택한 파일을 열 대리자의 메서드는 **최근** 메뉴. 예를 들어:

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

반환할 `true` 파일을 열 수 있는, 그렇지 않으면 반환 `false` 기본 제공 경고 파일 열 수는 사용자에 게 표시 됩니다.

가져오기 때문에 파일 이름 및 경로 **열기** 메뉴에는 공백이 포함 될 수 있습니다, 올바르게를 만들기 전에이 문자를 이스케이프 해야는 `NSUrl` 또는 오류가 발생 합니다. 다음 코드를 사용 하는 수행:

```csharp
filename = filename.Replace (" ", "%20");
```

마지막으로 만듭니다는 `NSUrl` 파일 및 응용 프로그램의 도우미 메서드에 위임 새 창을 열고 파일 로드를 사용 하 여 가리키는:

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

모든 항목이 함께 가져오기 살펴보겠습니다는 구현에는 **AppDelegate.cs** 파일:

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

응용 프로그램의 요구에 따라, 하지 않을 사용자를 한 번에 둘 이상의 창에서 동일한 파일을 엽니다. 예제에서는 앱에서 사용자가 이미 열려 있는 파일을 선택 하는 경우 (에서 **최근** 또는 **열기...** 메뉴 항목의 경우)를 앞으로 파일을 포함 하는 창 상태가 됩니다.

이를 위해 도우미 메서드에서 다음 코드를 사용 했습니다.

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

으로 설계 했으므로 우리의 `ViewController` 경로에 있는 파일에 포함 된 클래스를 해당 `Path` 속성입니다. 다음으로 응용 프로그램에서 현재 열려 있는 모든 창을 통해 루프입니다. 파일이 이미 열려 있으면 창 중 하나를 사용 하 여 다른 모든 창의 앞으로 상태가:

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

일치 항목이 없으면 파일이 로드 된 새 창이 열립니다 하 고 파일에 기록 됩니다는 **열기** 메뉴:

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

마찬가지로 다음 기본 제공 **첫 번째 응답자** 표준 메뉴 항목에 미리 유선 도착 하는 작업을 새로 만들고, 사용자 지정 작업 있고 인터페이스 작성기의 메뉴 항목에이 연결 합니다.

먼저, 응용 프로그램의 창 컨트롤러 중 하나에서 사용자 지정 작업을 정의 합니다. 예를 들어:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

다음으로 응용 프로그램의 스토리 보드 파일에 두 번 클릭 하 고 **솔루션 패드** Xcode에서 편집 하기 위해 열려는 인터페이스 작성기의 합니다. 선택의 **첫 번째 응답자** 아래는 **응용 프로그램 장면**, 다음 전환 하는 **특성 검사기**:

![속성 검사자](menu-images/action01.png "특성 검사기")

클릭는 **+** 의 맨 아래에 있는 단추는 **특성 검사기** 새 사용자 지정 동작을 추가 하려면:

![새 동작 추가](menu-images/action02.png "새 동작 추가")

창 컨트롤러에서 만든 사용자 지정 작업으로 동일한 이름을 지정 합니다.

![작업 이름 편집](menu-images/action03.png "작업 이름 편집")

컨트롤을 클릭 하 고 끌어에 메뉴 항목의 **첫 번째 응답자** 아래는 **응용 프로그램 장면**합니다. 팝업 목록에서 방금 만든 새 작업을 선택 합니다 (`defineKeyword:` 이 예에서):

![동작의 첨부](menu-images/action04.png "연결 동작")

스토리 보드의 변경 내용을 저장 하 고 변경 내용을 동기화 하는 Mac에 대 한 Visual Studio로 돌아갑니다. 응용 프로그램을 실행 하는 경우 사용자 지정 작업을 연결 하는 메뉴 항목은 자동으로 수 사용/사용 안 함 (열려 있기 액션과 창에 기반) 및 작업 실행 메뉴 항목이 선택:

[![새 동작을 테스트](menu-images/action05.png "새 동작을 테스트")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>추가, 편집 및 삭제 메뉴

이전 섹션에서 살펴본 대로 Xamarin.Mac 응용 프로그램 기본 메뉴와 특정 UI 컨트롤 자동으로 활성화 하 고에 응답 하는 메뉴 항목의 미리 지정 된 번호로 함께 제공 됩니다. 또한 설정 하 고 이러한 기본 항목에 응답 하는 응용 프로그램에 코드를 추가 하는 방법 또한 보았습니다.

이 섹션에서는 살펴보겠습니다 필요 하지 않습니다는 메뉴 항목을 메뉴를 다시 구성 하 고 새 메뉴, 메뉴 항목 및 작업을 추가 합니다.

두 번 클릭은 **Main.storyboard** 파일에 **솔루션 패드** 편집 하기 위해 열려는:

[![Xcode에서 UI 편집](menu-images/maint01.png "Xcode에서 UI를 편집 합니다.")](menu-images/maint01-large.png#lightbox)

특정 Xamarin.Mac 응용 프로그램에 대 한 하지 것 이며 기본 사용할 **보기** 메뉴를 제거 하도록 합니다. 에 **인터페이스 계층 구조** 선택 된 **보기** 주 메뉴 모음의 일부인 메뉴 항목:

![보기 메뉴 항목을 선택 하면](menu-images/maint02.png "보기 메뉴 항목을 선택")

Delete 키를 누릅니다 또는 백스페이스 삭제 메뉴에 있습니다. 에 항목을 모두 사용 하지 않겠습니다 다음으로 **형식** 메뉴 되 고, 하위 메뉴 아래쪽에서 아웃 사용 하도록 하겠습니다 항목을 이동 하려고 합니다. 에 **인터페이스 계층 구조** 다음 메뉴 항목을 선택 합니다.

![여러 항목을 강조 표시](menu-images/maint03.png "여러 항목을 강조 표시")

부모 아래 항목을 끌어 **메뉴** 현재이 위치에서:

[![부모 메뉴에 메뉴 항목을 끌어](menu-images/maint04.png "부모 메뉴에 메뉴 항목을 끌어")](menu-images/maint04-large.png#lightbox)

이제 메뉴가 같습니다.

[![새 위치에 있는 항목](menu-images/maint05.png "새 위치에 있는 항목")](menu-images/maint05-large.png#lightbox)

다음 보겠습니다 끌어는 **텍스트** 아래에서 아웃 하위 메뉴는 **형식** 메뉴 사이의 주 메뉴 모음에 배치 하는 **형식** 및 **창** 메뉴:

[![텍스트 메뉴](menu-images/maint06.png "Ext 메뉴")](menu-images/maint06-large.png#lightbox)

아래에서 돌아가겠습니다는 **형식** 메뉴와 삭제는 **글꼴** 하위 메뉴 항목입니다. 다음으로, 선택는 **형식** 메뉴 "글꼴"로 이름을 바꿉니다.

[![글꼴 메뉴](menu-images/maint07.png "The 글꼴 메뉴")](menu-images/maint07-large.png#lightbox)

다음으로, 사용자 지정 메뉴는 자동으로 가져오기 텍스트에 추가 텍스트 보기에서 선택 하면 미리 정의 된 구의 만들어 보겠습니다. 맨 아래에 검색 상자에는 **라이브러리 관리자** "메뉴" 형식 이렇게 하면 더욱 쉽게 찾아 메뉴 UI 요소를 모두 사용할 수 있습니다.

![라이브러리 관리자](menu-images/maint08.png "라이브러리 관리자")

이제 보겠습니다 우리의 메뉴를 만들려면 다음을 수행 합니다.

1. 끌어서는 **메뉴 항목** 에서 **라이브러리 검사기** 사이의 메뉴 모음에는 **텍스트** 및 **창** 메뉴: 

    ![라이브러리에 새 메뉴 항목을 선택 하면](menu-images/maint10.png "라이브러리에 새 메뉴 항목을 선택 합니다.")
2. "구" 항목을 이름을 바꿉니다. 

    [![설정 메뉴 이름](menu-images/maint09.png "설정 메뉴 이름")](menu-images/maint09-large.png#lightbox)
3. 이제는 **메뉴** 에서 **라이브러리 관리자**: 

    ![라이브러리에서 메뉴를 선택 하면](menu-images/maint11.png "라이브러리에서 메뉴를 선택 하면")
4. 다음 drop **메뉴** 새 **메뉴 항목** 것 바로 전에 만들고 해당 이름을 "구"로 변경 합니다. 

    [![편집 메뉴 이름](menu-images/maint12.png "메뉴 이름 편집")](menu-images/maint12-large.png#lightbox)
5. 이제 세 가지 기본의 이름을 **메뉴 항목** "Address", "날짜" 및 "Greeting": 

    [![구 메뉴](menu-images/maint13.png "The 구 메뉴")](menu-images/maint13-large.png#lightbox)
6. 네 번째 추가해보겠습니다 **메뉴 항목** 끌어는 **메뉴 항목** 에서 **라이브러리 관리자** "서명" 호출: 

    [![메뉴 항목 이름을 편집](menu-images/maint14.png "메뉴 항목 이름을 편집 합니다.")](menu-images/maint14-large.png#lightbox)
7. 메뉴 모음에 저장 합니다.

이제 C# 코드에 표시 되는 새 메뉴 항목 수 있도록 사용자 지정 동작의 집합을 만들어 보겠습니다. Xcode에서으로 전환 해 보겠습니다는 **도우미** 보기:

[![필요한 작업을 만드는](menu-images/maint15.png "필요한 동작 만들기")](menu-images/maint15-large.png#lightbox)

다음을 수행 하겠습니다.

1. 컨트롤 끌기는 **주소** 메뉴 항목에는 **AppDelegate.h** 파일입니다.
2. 스위치는 **연결** 형식을 **동작**: 

    [![동작 종류를 선택 하면](menu-images/maint17.png "동작 종류를 선택 합니다.")](menu-images/maint17-large.png#lightbox)
3. 입력 한 **이름** "phraseAddress" 고 키를 누릅니다는 **연결** 단추를 새 작업 만들기: 

    [![액션 구성](menu-images/maint18.png "액션 구성")](menu-images/maint18-large.png#lightbox)
4. 에 대해 위의 단계를 반복 하는 **날짜**, **인사말**, 및 **서명** 메뉴 항목: 

    [![완료 된 동작은](menu-images/maint19.png "완료 된 작업")](menu-images/maint19-large.png#lightbox)
5. 메뉴 모음에 저장 합니다.

다음 코드에서 해당 내용을 사용자가 조정할 수 있도록 우리의 텍스트 보기에 대 한 콘센트 만들 해야 합니다. 선택의 **ViewController.h** 파일에 **도우미 편집기** 라는 새 콘센트 만들고 `documentText`:

[![콘센트 만들기](menu-images/maint20.png "콘센트 만들기")](menu-images/maint20-large.png#lightbox)

Xcode에서 변경 내용을 동기화 하는 Mac에 대 한 Visual Studio로 반환 합니다. 다음 편집는 **ViewController.cs** 파일을 다음과 같이 되도록 합니다.

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

노출 되어 우리의 텍스트 보기의 외부의 텍스트는 `ViewController` 클래스 및 창 얻거나 포커스를 잃을 때 응용 프로그램 대리자에 게 알립니다. 이제 편집할는 **AppDelegate.cs** 파일을 다음과 같이 표시 하 게 합니다.

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

만들었고 여기는 `AppDelegate` partial 클래스 작업 및 콘센트 인터페이스 작성기에 정의한를 사용할 수 있도록 합니다. 또한 노출 한 `textEditor` 창을 현재 포커스를 추적 하 합니다.

다음 방법의 사용자 지정 메뉴 및 메뉴 항목을 처리 하는 데 사용 됩니다.

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

에 있는 항목의 모든 응용 프로그램을 실행 하는 경우 이제는 **구** 메뉴는 활성 상태 이며 제공 구를 선택 하면 텍스트 보기에 추가 합니다.

![실행 중인 응용 프로그램의 예로](menu-images/maint21.png "실행 중인 앱의 예")

이제의 기본적인 아래로 응용 프로그램 메뉴 모음으로 사용 했으므로 사용자 지정 컨텍스트 메뉴를 만드는 살펴 보겠습니다.

### <a name="creating-menus-from-code"></a>코드에서 메뉴 만들기

메뉴 및 메뉴 항목 Xcode의 인터페이스 작성기를 만들 Xamarin.Mac 응용 프로그램 만들기, 수정 또는 코드에서 메뉴, 하위 메뉴 또는 메뉴 항목을 제거 하려면 필요한 시간이 있을 수 있습니다.

다음 예제에서는 메뉴 항목 및 즉석에서 동적으로 생성 됩니다 하위 메뉴에 대 한 정보를 보관 하는 클래스를 만들:

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

다음 루틴은 컬렉션을 정의 하는이 클래스와 16 `LanguageFormatCommand`개체 및 재귀적으로 빌드에 전달 된 (인터페이스 작성기에서 만든) 기존 메뉴 아래에 추가 하 여 새 메뉴 및 메뉴 항목:

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

에 대 한 `LanguageFormatCommand` 는 빈 값을 가진 개체를 `Title` 속성을이 루틴 생성 되는 **메뉴 항목이** (씬 회색 선) 메뉴 섹션 사이:

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

제목을 제공 하는 경우 해당 이름의 새 메뉴 항목을 만들어집니다.

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

경우는 `LanguageFormatCommand` 개체에 하위 `LanguageFormatCommand` 개체 하위 메뉴가 만들어집니다 및 `AssembleMenu` 메서드는 해당 메뉴를 빌드하기 위해 호출 하는 재귀적으로:

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

하위 메뉴를 하지 않은 모든 새 메뉴 항목에 대해 사용자가 선택한 메뉴 항목을 처리 하는 코드가 추가 됩니다.

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>테스트 메뉴 만들기

원위치에서 위 코드의 모든 경우의 다음 컬렉션 `LanguageFormatCommand` 개체를 만들지 못했습니다.

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

컬렉션에 전달 하는 `AssembleMenu` 함수 (으로 **형식** 메뉴의 기본으로 설정), 다음과 같은 동적 메뉴 및 메뉴 항목을 만들 수는:

![실행 중인 응용 프로그램에 새 메뉴 항목](menu-images/dynamic01.png "실행 중인 응용 프로그램에 새 메뉴 항목")

#### <a name="removing-menus-and-items"></a>메뉴 및 항목 제거

응용 프로그램의 사용자 인터페이스에서 모든 메뉴 또는 메뉴 항목을 제거 해야 하는 경우 사용할 수 있습니다는 `RemoveItemAt` 의 메서드는 `NSMenu` 클래스 단순히 0을 지정 하 여 기반 제거할 항목의 인덱스입니다.

예를 들어, 메뉴 및 위의 루틴에 의해 생성 되는 메뉴 항목을 제거 하려면 다음 코드를 사용할 수 있습니다.

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

위 코드의 경우 동적으로 제거 되지 Xcode의 작성기 인터페이스 및 앱에서 다음을 사용할 수 있는 주요 사항에 첫 번째 4 개의 메뉴 항목이 생성 됩니다.

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>상황에 맞는 메뉴

상황에 맞는 메뉴에는 사용자가 해야 하는지 또는 컨트롤 창에서 항목 때 나타납니다. 기본적으로 이미 macOS에 기본 제공 UI 요소에 여러 가지 상황에 맞는 메뉴 (예: 텍스트 뷰) 여기에 연결 된는. 그러나 시간을 창에 추가 했습니다 하는 UI 요소에 대 한 고유한 사용자 지정 상황에 맞는 메뉴를 만들려고 할 때 있을 수 있습니다.

편집 우리의 **Main.storyboard** Xcode에서 파일을 추가 **창** 우리의 디자인 창 설정 해당 **클래스** 에서 "NSPanel"에 **Identity 관리자**, 새 추가 **도우미** 항목의 **창** 메뉴를 사용 하 여 새 창에 연결을 **Segue 표시**:

[![설정 segue 유형](menu-images/context01.png "segue 유형을 설정")](menu-images/context01-large.png#lightbox)

다음을 수행 하겠습니다.

1. 끌어서는 **레이블** 에서 **라이브러리 관리자** 에 **패널** 창 고 해당 텍스트를 "Property" 설정: 

    [![레이블 값을 편집](menu-images/context03.png "레이블의 값 편집")](menu-images/context03-large.png#lightbox)
2. 이제는 **메뉴** 에서 **라이브러리 검사기** 기본 메뉴 항목의 이름 바꾸기를 세 고 계층 구조 보기에서 보기 컨트롤러에 **문서**, **텍스트**  및 **글꼴**:

    [![필요한 메뉴 항목](menu-images/context02.png "필요한 메뉴 항목")](menu-images/context02-large.png#lightbox)
3. 컨트롤 끌기 이제는 **속성 레이블** 에 **메뉴**:

    [![segue 만들려는 끌기](menu-images/context04.png "는 segue 만들려는 끌기")](menu-images/context04-large.png#lightbox)
4. 팝업 대화 상자에서 선택 **메뉴**: 

    ![설정 segue 유형](menu-images/context05.png "segue 유형을 설정")
5. **Identity 관리자**, 보기 컨트롤러의 클래스 "PanelViewController"를 설정 합니다. 

    [![Segue 클래스 설정](menu-images/context10.png "segue 클래스를 설정 합니다.")](menu-images/context10-large.png#lightbox)
6. 동기화 되는 데 Mac 용 Visual Studio로 다시 전환 하 면 작성기 인터페이스를 반환 합니다.
7. 전환 하는 **도우미 편집기** 선택 하 고는 **PanelViewController.h** 파일입니다.
8. 에 대 한 작업을 만듭니다는 **문서** 메뉴 항목 라는 `propertyDocument`: 

    [![액션 구성](menu-images/context06.png "액션 구성")](menu-images/context06-large.png#lightbox)
9. 나머지 메뉴 항목에 대 한 만들기 작업을 반복 합니다. 

    [![필요한 동작은](menu-images/context07.png "필요한 동작")](menu-images/context07-large.png#lightbox)
10. 마지막으로 콘센트에 대 한 연결을 만듭니다는 **속성 레이블** 호출 `propertyLabel`: 

    [![콘센트 구성](menu-images/context08.png "콘센트 구성")](menu-images/context08-large.png#lightbox)
11. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

편집 된 **PanelViewController.cs** 파일을 다음 코드를 추가 합니다.

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

이제 응용 프로그램을 실행 하 고 속성 레이블 패널에서를 마우스 오른쪽 단추로 클릭,에서는 사용자 지정이 상황에 맞는 메뉴를 표시 됩니다. 선택 하 고 메뉴에서 항목을 레이블의 값이 변경 됩니다.

![실행 하는 상황에 맞는 메뉴](menu-images/context09.png "실행 상황에 맞는 메뉴")

다음 상태 표시줄 메뉴 만들기에 대해 살펴보겠습니다.

## <a name="status-bar-menus"></a>상태 표시줄 메뉴

상태 표시줄 메뉴는 메뉴 또는 응용 프로그램의 상태를 반영 하는 이미지와 같은 사용자에 게와 상호 작용을 제공 하는 상태 메뉴 항목이 나 피드백의 컬렉션을 표시 합니다. 응용 프로그램의 상태 표시줄 메뉴를 사용할 수는 응용 프로그램이 백그라운드에서 실행 되는 경우에 합니다. 시스템 수준 상태 표시줄 응용 프로그램 메뉴 표시줄의 오른쪽에 있는 되며 유일한 상태 표시줄 macOS에서 현재 사용할 수 있습니다.

편집 우리의 **AppDelegate.cs** 파일을 확인는 `DidFinishLaunching` 다음과 같은 메서드 보기:

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

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` 시스템 수준 상태 표시줄에 액세스할 수 있습니다. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` 상태 표시줄 항목을 새로 만듭니다. 여기에서 우리는 메뉴 및 메뉴 항목의 수 만들고 메뉴 상태 표시줄 항목 방금 만든를 연결 합니다. 

응용 프로그램을 실행 하는 경우 새 상태 표시줄 항목 표시 됩니다. 메뉴에서 항목을 선택 하면 텍스트 보기에 있는 텍스트를 변경 됩니다. 

![실행 상태 표시줄 메뉴](menu-images/statusbar01.png "실행 상태 표시줄 메뉴")

다음으로, 사용자 지정 도킹 메뉴 항목을 만드는 중에 대해 살펴보겠습니다.

## <a name="custom-dock-menus"></a>사용자 지정 도킹 메뉴

도킹 메뉴 사용자 마우스 오른쪽 단추로 클릭 하거나 컨트롤 도킹 스테이션에서 응용 프로그램의 아이콘 클릭 하면 Mac 응용 프로그램에 대 한 나타납니다.

![사용자 지정 메뉴 도킹](menu-images/dock01.png "사용자 지정 메뉴 도킹")

다음을 수행 하 여 응용 프로그램에 대 한 사용자 지정 도킹 메뉴를 만들어 봅니다.

1. Mac 용 Visual Studio에서 단추로 클릭 하 여 응용 프로그램의 프로젝트 고 **추가** > **새 파일...** 새 파일 대화 상자에서 선택 **Xamarin.Mac** > **빈 인터페이스 정의**에 대 한 "DockMenu"를 사용 하 여는 **이름** 클릭는 **새로 만들기**  를 만드는 새 단추 **DockMenu.xib** 파일:

    ![빈 인터페이스 정의 추가](menu-images/dock02.png "빈 인터페이스 정의 추가 합니다.")
2. 에 **솔루션 패드**, 두 번 클릭은 **DockMenu.xib** Xcode에서 편집을 위해 열 파일입니다. 새 **메뉴** 다음 항목이 포함: **주소**, **날짜**, **인사말**, 및 **서명** 

    [![UI 레이아웃](menu-images/dock03.png "UI 배치")](menu-images/dock03-large.png#lightbox)
3. 다음으로, 보겠습니다 새 메뉴 항목의 동작에 연결 우리의 기존에 사용자 지정 메뉴에 작성 된는 [추가, 편집 및 삭제 메뉴](#Adding,_Editing_and_Deleting_Menus) 위의 섹션. 로 전환는 **연결 검사기** 선택 하 고는 **첫 번째 응답자** 에 **인터페이스 계층 구조**합니다. 아래로 스크롤하여 찾아는 `phraseAddress:` 동작 합니다. 해당 작업을 원에서 선을 끌어서는 **주소** 메뉴 항목:

    [![작업을 연결으로 끌어](menu-images/dock04.png "액션을 연결으로 끌어")](menu-images/dock04-large.png#lightbox)
4. 모든 해당 액션에 연결할 다른 메뉴 항목에 대해 반복 합니다. 

    [![필요한 동작은](menu-images/dock05.png "필요한 동작")](menu-images/dock05-large.png#lightbox)
5. 다음으로, 선택는 **응용 프로그램** 에 **인터페이스 계층 구조**합니다. 에 **연결 검사기**, 원에서 선을 끌어서는 `dockMenu` 콘센트에 방금 만든 메뉴:

    [![콘센트를 연결 하는 끌어](menu-images/dock06.png "콘센트를 연결 하는 끌어")](menu-images/dock06-large.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 다시 전환 합니다.
7. 두 번 클릭은 **Info.plist** 편집을 위해 열 파일입니다. 

    [![Info.plist 파일 편집](menu-images/dock07.png "Info.plist 파일 편집")](menu-images/dock07-large.png#lightbox)
8. 클릭는 **소스** 화면 아래쪽에서 탭: 

    [![소스 뷰를 선택 하면](menu-images/dock08.png "원본 뷰 선택")](menu-images/dock08-large.png#lightbox)
9. 클릭 **새 항목 추가**, 녹색 더하기 단추 클릭, "AppleDockMenu"에 속성 이름 및 값 "DockMenu" (쿼리하여 새로운.xib 파일 확장명 없이 이름)을 설정 합니다. 

    [![DockMenu 항목 추가](menu-images/dock09.png "DockMenu 항목 추가")](menu-images/dock09-large.png#lightbox)

이제 응용 프로그램을 실행 하 고 도킹 스테이션에서 해당 아이콘을 마우스 오른쪽 단추로 클릭, 우리의 새 메뉴 항목이 표시 됩니다.

![도킹 메뉴 실행의 예로](menu-images/dock10.png "실행 도킹 메뉴의 예")

메뉴에서 사용자 지정 항목 중 하나를 선택, 우리의 텍스트 보기에 있는 텍스트 수정 됩니다.

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>팝업 단추 및 드롭다운 목록

선택한 항목을 표시 하 고 사용자가 클릭할 때에서 선택 하는 옵션의 목록을 제공 하는 팝업 단추 합니다. 풀 다운 목록은 일반적으로 현재 작업 컨텍스트에 특정 명령을 선택 하는 데 사용 되는 팝업 단추의 한 종류입니다. 둘 다 창에서 아무 곳 이나 나타날 수 있습니다.

다음을 수행 하 여 응용 프로그램에 대 한 사용자 지정 팝업 단추를 만들어 봅니다.

1. 편집 된 **Main.storyboard** 끌어서 Xcode에서 파일은 **팝업 단추** 에서 **라이브러리 관리자** 에 **패널** 에서 만든 창 [상황에 맞는 메뉴](#Contextual_Menus) 섹션: 

    [![단추 추가 하는 팝업](menu-images/popup01.png "팝업 단추 추가")](menu-images/popup01-large.png#lightbox)
2. 새 메뉴 항목을 추가 하 고를 팝업에서 항목의 제목이 설정: **주소**, **날짜**, **인사말**, 및 **서명** 

    [![메뉴 항목을 구성](menu-images/popup02.png "는 메뉴 항목을 구성")](menu-images/popup02-large.png#lightbox)
3. 다음으로, 보겠습니다 우리의 새 메뉴 항목에는 사용자 지정 메뉴에 대 한 만든 기존 작업에 연결 된 [추가, 편집 및 삭제 메뉴](#Adding,_Editing_and_Deleting_Menus) 위의 섹션. 로 전환는 **연결 검사기** 선택 하 고는 **첫 번째 응답자** 에 **인터페이스 계층 구조**합니다. 아래로 스크롤하여 찾아는 `phraseAddress:` 동작 합니다. 해당 작업을 원에서 선을 끌어서는 **주소** 메뉴 항목: 

    [![작업을 연결으로 끌어](menu-images/popup03.png "액션을 연결으로 끌어")](menu-images/popup03-large.png#lightbox)
4. 모든 해당 액션에 연결할 다른 메뉴 항목에 대해 반복 합니다. 

    [![작업에 필요한 모든](menu-images/popup04.png "모든 필요한 작업")](menu-images/popup04-large.png#lightbox)
5. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 다시 전환 합니다.

이제 응용 프로그램을 실행 하 고 팝업에서 항목을 선택, 우리의 텍스트 보기에 있는 텍스트 변경 됩니다.

![예를 실행 하는 팝업의](menu-images/popup05.png "실행 팝업의 예")

만들기 및 팝업 단추와 정확히 같은 방식에서 풀 다운 목록을 사용 하 여 작업 수 있습니다. 대신 기존 작업에 연결을 만들 수 우리의 상황에 맞는 메뉴에 대해 했던 것 처럼 사용자 고유의 사용자 지정 동작의 [상황에 맞는 메뉴](#Contextual_Menus) 섹션.

## <a name="summary"></a>요약

이 문서에는 메뉴 및 메뉴 항목 Xamarin.Mac 응용 프로그램에서 작업에 대해 자세히를 수행 했습니다. 먼저 상황에 맞는 메뉴를 만들 때, 다음 상태 표시줄 메뉴 검사 했습니다 및 사용자 지정 메뉴 도킹 한 다음 응용 프로그램의 메뉴 모음을 검사 했습니다. 마지막으로, 팝업 메뉴, 풀 다운 목록에서 다룬 합니다.


## <a name="related-links"></a>관련 링크

- [MacMenus (샘플)](https://developer.xamarin.com/samples/mac/MacMenus/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [휴먼 인터페이스 지침-메뉴](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [응용 프로그램 메뉴 및 팝업 목록 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
