---
title: Xamarin.ios의 메뉴
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 메뉴를 사용 하는 방법을 설명 합니다. Xcode 및 Interface Builder에서 메뉴 및 메뉴 항목을 만들고 유지 관리 하 고 프로그래밍 방식으로 작업 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 0879fcc529e72e03df4eaba7790a534ace38856f
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657339"
---
# <a name="menus-in-xamarinmac"></a>Xamarin.ios의 메뉴

_이 문서에서는 Xamarin.ios 응용 프로그램에서 메뉴를 사용 하는 방법을 설명 합니다. Xcode 및 Interface Builder에서 메뉴 및 메뉴 항목을 만들고 유지 관리 하 고 프로그래밍 방식으로 작업 하는 방법을 설명 합니다._

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우 목표-C 및 Xcode에서 작업 하는 개발자와 동일한 cocoa 메뉴에 액세스할 수 있습니다. Xamarin.ios는 Xcode와 직접 통합 되므로 Xcode의 Interface Builder를 사용 하 여 메뉴 모음, 메뉴 및 메뉴 항목을 만들고 유지 관리할 수 있습니다 (또는 필요에 따라 코드에서 C# 직접 만들 수 있음).

메뉴는 Mac 응용 프로그램의 사용자 경험의 핵심 부분이 며 일반적으로 사용자 인터페이스의 다양 한 부분에 표시 됩니다.

- **응용 프로그램의 메뉴 모음** -모든 Mac 응용 프로그램의 화면 맨 위에 표시 되는 주 메뉴입니다.
- **상황에 맞는 메뉴** -사용자가 창의 항목을 마우스 오른쪽 단추로 클릭 하거나 컨트롤을 클릭할 때 나타납니다.
- **상태 표시줄** -화면 맨 위에 표시 되 고 (메뉴 모음 클록의 왼쪽), 항목이 추가 되 면 왼쪽으로 증가 하는 응용 프로그램 메뉴 모음의 오른쪽 끝에 있는 영역입니다.
- **도킹 메뉴** -사용자가 응용 프로그램의 아이콘을 마우스 오른쪽 단추로 클릭 하거나 컨트롤을 클릭할 때 표시 되는 도킹의 각 응용 프로그램에 대 한 메뉴 또는 사용자가 아이콘을 마우스 왼쪽 단추로 클릭 하 고 마우스 단추를 누른 경우에 표시 되는 메뉴입니다.
- **팝업 단추 및 드롭다운 목록** -팝업 단추는 선택한 항목을 표시 하 고 사용자가 클릭할 때 선택할 수 있는 옵션 목록을 표시 합니다. 풀 다운 목록은 일반적으로 현재 태스크의 컨텍스트와 관련 된 명령을 선택 하는 데 사용 되는 팝업 단추의 유형입니다. 둘 다 창의 어디에 나 나타날 수 있습니다.

[![예제 메뉴](menu-images/intro01.png "예제 메뉴")](menu-images/intro01-large.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램에서 Cocoa 메뉴 모음, 메뉴 및 메뉴 항목을 사용 하는 기본 사항을 설명 합니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 가장 좋습니다. 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하 고,에서 사용할 주요 개념 및 기술을 설명 하 고 있습니다. 이 문서를 참조 하세요.

[Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서의 [클래스/메서드를 대상으로 노출 C# -C](~/mac/internals/how-it-works.md) 섹션을 살펴보고 C# 클래스를 연결 하는 데 사용 되는 `Register` 및 `Export` 특성에 대해 설명 합니다. 목표-C 개체 및 UI 요소입니다.

## <a name="the-applications-menu-bar"></a>응용 프로그램의 메뉴 모음 

Windows OS에서 실행 되는 응용 프로그램과 달리 모든 창에 자체 메뉴 모음이 연결 되어 있는 경우 macOS에서 실행 되는 모든 응용 프로그램에는 해당 응용 프로그램의 모든 창에 사용 되는 화면 위쪽에 실행 되는 단일 메뉴 모음이 있습니다.

[![메뉴 모음](menu-images/appmenu01.png "메뉴 모음")](menu-images/appmenu01-large.png#lightbox)

이 메뉴 모음의 항목은 지정 된 순간에 현재 컨텍스트 또는 응용 프로그램의 상태와 해당 사용자 인터페이스를 기반으로 활성화 되거나 비활성화 됩니다. 예를 들어 사용자가 텍스트 필드를 선택 하면 **복사** 및 **잘라내기**와 같이 **편집** 메뉴의 항목이 활성화 됩니다.

Apple 및 기본적으로 모든 macOS 응용 프로그램은 응용 프로그램의 메뉴 모음에 표시 되는 표준 메뉴 및 메뉴 항목 집합을 포함 합니다.

- **Apple 메뉴** -이 메뉴는 실행 중인 응용 프로그램에 관계 없이 항상 사용자가 사용할 수 있는 시스템 차원 항목에 대 한 액세스를 제공 합니다. 개발자는 이러한 항목을 수정할 수 없습니다.
- **앱 메뉴** -이 메뉴는 응용 프로그램의 이름을 굵게 표시 하 고 사용자가 현재 실행 중인 응용 프로그램을 식별 하는 데 도움이 됩니다. 응용 프로그램을 종료 하는 것과 같이 지정 된 문서 또는 프로세스가 아니라 전체적으로 응용 프로그램에 적용 되는 항목을 포함 합니다.
- **파일 메뉴** -응용 프로그램이 작동 하는 문서를 생성, 열기 또는 저장 하는 데 사용 되는 항목입니다. 응용 프로그램이 문서 기반이 아니면이 메뉴의 이름을 바꾸거나 제거할 수 있습니다.
- **편집 메뉴** -응용 프로그램의 사용자 인터페이스에서 요소를 편집 하거나 수정 하는 데 사용 되는 **잘라내기**, **복사**및 **붙여넣기** 와 같은 명령을 보관 합니다.
- **서식 메뉴** -응용 프로그램이 텍스트와 함께 작동 하는 경우이 메뉴에는 해당 텍스트의 서식을 조정 하는 명령이 포함 됩니다.
- **보기 메뉴** -응용 프로그램의 사용자 인터페이스에서 콘텐츠가 표시 되는 방법 (표시)에 영향을 주는 명령을 보유 합니다.
- **응용 프로그램 관련 메뉴** -응용 프로그램과 관련 된 모든 메뉴 (예: 웹 브라우저의 책갈피 메뉴)입니다. 막대의 **보기** 창과 **창** 메뉴 사이에 표시 되어야 합니다.
- **창 메뉴** -응용 프로그램에서 windows를 사용 하기 위한 명령과 현재 열려 있는 창 목록을 포함 합니다.
- **도움말 메뉴** -응용 프로그램이 화면 도움말을 제공 하는 경우 도움말 메뉴가 표시줄의 가장 오른쪽 메뉴 여야 합니다. 

응용 프로그램 메뉴 모음과 표준 메뉴 및 메뉴 항목에 대 한 자세한 내용은 Apple의 [휴먼 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)을 참조 하십시오.

### <a name="the-default-application-menu-bar"></a>기본 응용 프로그램 메뉴 모음

새 Xamarin.ios 프로젝트를 만들 때마다 macOS 응용 프로그램에 일반적으로 적용 되는 일반적인 항목을 포함 하는 표준 기본 응용 프로그램 메뉴 모음이 자동으로 표시 됩니다 (위 섹션에서 설명). 응용 프로그램의 기본 메뉴 모음은 **Solution Pad**의 프로젝트 아래에 있는 **주 storyboard** 파일 (앱 UI의 나머지 부분과 함께)에서 정의 됩니다.  

![주 스토리 보드를 선택 합니다] . (menu-images/appmenu02.png "주 스토리 보드를 선택 합니다") .

**주 storyboard** 파일을 두 번 클릭 하 여 Xcode의 Interface Builder 편집을 위해 엽니다. 그러면 메뉴 편집기 인터페이스가 나타납니다.

[![Xcode에서 UI 편집](menu-images/defaultbar01.png "Xcode에서 UI 편집")](menu-images/defaultbar01-large.png#lightbox)

여기에서 **파일** 메뉴의 **열기** 메뉴 항목 등의 항목을 클릭 하 고 **특성 검사자**에서 해당 속성을 편집 하거나 조정할 수 있습니다.

[![메뉴의 특성 편집](menu-images/defaultbar02.png "메뉴의 특성 편집")](menu-images/defaultbar02-large.png#lightbox)

이 문서의 뒷부분에서 메뉴와 항목을 추가, 편집 및 삭제할 수 있습니다. 이제 기본적으로 사용할 수 있는 메뉴 및 메뉴 항목을 확인 하 고 미리 정의 된 콘센트 및 작업 집합을 통해 코드에 자동으로 노출 되는 방법에 대해 알아봅니다. 자세한 내용은 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 설명서를 참조 하세요.

예를 들어 **열기** 메뉴 항목에 대 한 `openDocument:` **연결 검사기** 를 클릭 하면 작업에 자동으로 연결 되는 것을 볼 수 있습니다. 

[![연결 된 작업 보기](menu-images/defaultbar03.png "연결 된 작업 보기")](menu-images/defaultbar03-large.png#lightbox)

**인터페이스 계층** 에서 **첫 번째 응답자** 를 선택 하 고 **연결 검사기**에서 아래로 스크롤하면 `openDocument:` **열기** 메뉴 항목이 연결 된 작업의 정의 (몇 가지 및 인 응용 프로그램에 대 한 기타 기본 동작은 자동으로 컨트롤에 연결 되지 않습니다.

[![모든 연결 된 작업 보기](menu-images/defaultbar04.png "모든 연결 된 작업 보기")](menu-images/defaultbar04-large.png#lightbox) 

이것이 중요 한 이유는 무엇 인가요? 다음 섹션에서는 이러한 자동 정의 작업이 다른 Cocoa 사용자 인터페이스 요소와 함께 작동 하 여 메뉴 항목을 자동으로 활성화 및 비활성화 하 고 항목에 대 한 기본 제공 기능을 제공 하는 방법을 확인 합니다.

나중에 이러한 기본 제공 작업을 사용 하 여 코드에서 항목을 활성화 및 비활성화 하 고, 선택 하는 경우 고유한 기능을 제공 합니다.

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>기본 제공 메뉴 기능

UI 항목 또는 코드를 추가 하기 전에 새로 만든 Xamarin.ios 응용 프로그램을 실행 한 경우 일부 항목이 자동으로 자동으로 연결 되 고 사용 하도록 설정 된 것을 알 수 있습니다 (전체 기능이 자동으로 기본 제공 됨).  **앱** 메뉴:

![활성화 된 메뉴 항목](menu-images/appmenu03.png "활성화 된 메뉴 항목")

**잘라내기**, **복사**및 **붙여넣기** 와 같은 다른 메뉴 항목은 그렇지 않습니다.

![메뉴 항목 사용 안 함](menu-images/appmenu04.png "메뉴 항목 사용 안 함")

응용 프로그램을 중지 하 고 **Solution Pad** 의 **주 storyboard** 파일을 두 번 클릭 하 여 Xcode의 Interface Builder에서 편집할 수 있도록 엽니다. 다음으로, **라이브러리** 의 **텍스트 뷰** 를 **인터페이스 편집기**의 창 뷰 컨트롤러로 끌어 옵니다.

[![라이브러리에서 텍스트 뷰 선택](menu-images/appmenu05.png "라이브러리에서 텍스트 뷰 선택")](menu-images/appmenu05-large.png#lightbox)

**제약 조건 편집기** 에서 창의 가장자리에 텍스트 뷰를 고정 하 고 편집기 맨 위에 있는 네 개의 빨간색 빔을 모두 클릭 하 고 **4 개의 제약 조건 추가** 단추를 클릭 하 여 창에서 확대 및 축소 되는 위치를 설정 합니다.

[![제약 조건 편집](menu-images/appmenu06.png "제약 조건 편집")](menu-images/appmenu06-large.png#lightbox)

사용자 인터페이스 디자인에 대 한 변경 내용을 저장 하 고 Mac용 Visual Studio 다시 전환 하 여 변경 내용을 Xamarin.ios 프로젝트와 동기화 합니다. 이제 응용 프로그램을 시작 하 고 텍스트 뷰에 텍스트를 입력 하 고 선택한 다음 **편집** 메뉴를 엽니다.

![메뉴 항목이 자동으로 설정/해제 됩니다] . (menu-images/appmenu07.png "메뉴 항목이 자동으로 설정/해제 됩니다") .

코드를 한 줄도 작성 하지 않고 **잘라내기**, **복사**및 **붙여넣기** 항목을 자동으로 사용 하도록 설정 하 고 완벽 하 게 작동 하는 방법을 확인 합니다. 

여기에 무슨 일이 일어나고 있나요? 기본 제공 되는 기본 제공 작업 (위에서 설명한 대로)은 기본 메뉴 항목에 연결 되어 있습니다 (위에서 설명한 대로). macOS의 일부인 Cocoa 사용자 인터페이스 요소 대부분은 특정 작업 (예: `copy:`)에 대 한 후크를 제공 합니다. 따라서 창에 추가 되 고 활성 및 선택 된 경우 해당 메뉴 항목이 나 해당 작업에 연결 된 항목이 자동으로 활성화 됩니다. 사용자가 해당 메뉴 항목을 선택 하면 UI 요소에 기본 제공 되는 기능을 개발자 개입 없이 모두 호출 하 고 실행 합니다.

### <a name="enabling-and-disabling-menus-and-items"></a>메뉴 및 항목 사용 및 사용 안 함

기본적으로 사용자 이벤트가 발생할 때마다는 `NSMenu` 응용 프로그램의 컨텍스트를 기반으로 표시 되는 각 메뉴 및 메뉴 항목을 자동으로 사용 하거나 사용 하지 않도록 설정 합니다. 항목을 설정/해제 하는 방법에는 다음 세 가지가 있습니다.

- **자동 메뉴 사용** -항목이 연결 된 작업에 응답 `NSMenu` 하는 적절 한 개체를 찾을 수 있는 경우 메뉴 항목이 활성화 됩니다. 예를 들어 위의 텍스트 뷰에는 `copy:` 동작에 대 한 기본 제공 후크가 있습니다.
- **사용자 지정 작업 및 validateMenuItem:** [창 또는 뷰 컨트롤러 사용자 지정 작업](#Working-with-Custom-Window-Actions)에 바인딩된 메뉴 항목의 경우 `validateMenuItem:` 작업을 추가 하 고 메뉴 항목을 수동으로 사용 하거나 사용 하지 않도록 설정할 수 있습니다.
- **수동 메뉴 사용** -각 `Enabled` `NSMenuItem` 항목의 속성을 수동으로 설정 하 여 메뉴의 각 항목을 개별적으로 사용 하거나 사용 하지 않도록 설정할 수 있습니다.

시스템을 선택 하려면 `AutoEnablesItems` `NSMenu`의 속성을 설정 합니다. `true`는 자동 (기본 동작) `false` 이며 수동입니다. 

> [!IMPORTANT]
> 수동 메뉴 사용을 사용 하도록 선택 하는 경우와 같은 `NSTextView`appkit 클래스에 의해 제어 되는 메뉴 항목도 자동으로 업데이트 되지 않습니다. 코드에서 모든 항목을 직접 사용 하거나 사용 하지 않도록 설정 하는 일을 담당 합니다.

#### <a name="using-validatemenuitem"></a>ValidateMenuItem 사용

위에서 설명한 것 처럼 [창 또는 보기 컨트롤러 사용자 지정 작업](#Working-with-Custom-Window-Actions)에 바인딩된 모든 메뉴 항목에 대해 `validateMenuItem:` 작업을 추가 하 고 메뉴 항목을 수동으로 사용 하거나 사용 하지 않도록 설정할 수 있습니다.

다음 예제 `Tag` 에서는 속성을 사용 하 여에서 선택한 텍스트 `NSTextView`의 상태에 따라 동작에 `validateMenuItem:` 의해 활성화/비활성화 될 메뉴 항목의 형식을 결정 합니다. 속성 `Tag` 은 각 메뉴 항목에 대 한 Interface Builder에 설정 되어 있습니다.

![Tag 속성 설정](menu-images/validate01.png "Tag 속성 설정")

그리고 다음 코드를 뷰 컨트롤러에 추가 했습니다.

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

이 코드를 실행 하 고에서 `NSTextView`텍스트를 선택 하지 않은 경우 두 줄 바꿈 메뉴 항목이 사용 하지 않도록 설정 됩니다 (뷰 컨트롤러에서 작업에 연결 된 경우에도).

![비활성화 된 항목 표시](menu-images/validate02.png "비활성화 된 항목 표시")

텍스트의 섹션을 선택 하 고 메뉴가 다시 열리면 두 줄 바꿈 메뉴 항목을 사용할 수 있습니다.

![활성화 된 항목 표시](menu-images/validate03.png "활성화 된 항목 표시")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>코드에서 메뉴 항목 활성화 및 응답

위에서 설명한 것 처럼 UI 디자인에 특정 Cocoa 사용자 인터페이스 요소 (예: 텍스트 필드)를 추가 하기만 하면 코드를 작성할 필요 없이 몇 가지 기본 메뉴 항목이 활성화 되 고 자동으로 작동 합니다. 다음으로, 사용자가 메뉴 항목을 C# 사용 하도록 설정 하 고 기능을 선택할 때 사용자 고유의 코드를 xamarin.ios 프로젝트에 추가 하 여 기능을 제공 하는 방법을 살펴보겠습니다.

예를 들어 사용자가 **파일** 메뉴에서 **열기** 항목을 사용 하 여 폴더를 선택할 수 있도록 하려는 경우를 가정해 보겠습니다. 이는 응용 프로그램 전체 함수 이며 제공 창이 나 UI 요소로 제한 되지 않으므로 응용 프로그램 대리자에 게이를 처리 하는 코드를 추가 합니다.

**Solution Pad**에서 `AppDelegate.CS` 파일을 두 번 클릭 하 여 편집용으로 엽니다.

![앱 대리자 선택](menu-images/appmenu08.png "앱 대리자 선택")

`DidFinishLaunching` 메서드 아래에 다음 코드를 추가 합니다.

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

이제 응용 프로그램을 실행 하 고 **파일** 메뉴를 엽니다. 

![파일 메뉴](menu-images/appmenu09.png "파일 메뉴")

이제 **열기** 메뉴 항목이 활성화 된 것을 확인할 수 있습니다. 이를 선택 하면 열기 대화 상자가 표시 됩니다.

![열기 대화 상자](menu-images/appmenu10.png "열기 대화 상자")

**열기** 단추를 클릭 하면 다음과 같은 경고 메시지가 표시 됩니다.

![예제 대화 상자 메시지](menu-images/appmenu11.png "예제 대화 상자 메시지")

여기 `[Export ("openDocument:")]`에서 키 줄은 **AppDelegate** `void OpenDialog (NSObject sender)` `NSMenu` 에게작업에응답하는`openDocument:` 메서드가 있음을 나타냅니다. 위에서 기억할 경우 기본적으로 Interface Builder에서 열기 메뉴 항목이 자동으로이 작업에 **연결** 됩니다.

[![연결 된 작업 보기](menu-images/defaultbar03.png "연결 된 작업 보기")](menu-images/defaultbar03-large.png#lightbox)

다음으로, 자체 메뉴, 메뉴 항목 및 작업을 만들고 코드에서이에 대해 응답 하는 방법을 살펴보겠습니다.

### <a name="working-with-the-open-recent-menu"></a>최근 항목 열기 메뉴 작업

기본적으로 **파일** 메뉴에는 사용자가 앱을 사용 하 여 연 마지막 몇 개의 파일을 추적 하는 **최근 열기** 항목이 포함 되어 있습니다. `NSDocument` 기반 xamarin.ios 앱을 만드는 경우이 메뉴는 자동으로 처리 됩니다. 다른 형식의 Xamarin.ios 앱의 경우에는이 메뉴 항목을 수동으로 관리 하 고 응답할 책임이 있습니다.

**최근 항목 열기** 메뉴를 수동으로 처리 하려면 다음을 사용 하 여 새 파일이 열리거나 저장 되었음을 먼저 알려 주어 야 합니다.

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

앱에서을 사용 `NSDocuments`하지 않는 경우에도를 `SharedDocumentController` `NSDocumentController` 사용 하 여의 `NoteNewRecentDocumentURL` 메서드에 파일의 위치를 포함 하 `NSUrl` 는를 전송 하 여 **최근 열기** 메뉴를 유지 관리 합니다.

그런 다음 사용자가 `OpenFile` 최근 항목 **열기** 메뉴에서 선택한 파일을 열도록 앱 대리자의 메서드를 재정의 해야 합니다. 예를 들어:

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

파일 `true` 을 열 수 있으면를 반환 하 고, `false` 그렇지 않으면를 반환 하 고, 파일을 열 수 없는 사용자에 게 기본 제공 경고를 표시 합니다.

**최근 항목 열기** 메뉴에서 반환 된 파일 이름과 경로에 공백이 포함 될 수 있으므로을 `NSUrl` 만들기 전에이 문자를 제대로 이스케이프 해야 합니다. 그렇지 않으면 오류가 발생 합니다. 다음 코드를 사용 하 여이 작업을 수행 합니다.

```csharp
filename = filename.Replace (" ", "%20");
```

마지막으로, 파일을 `NSUrl` 가리키는를 만들고 앱 대리자의 도우미 메서드를 사용 하 여 새 창을 열고 파일을 로드 합니다.

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

모든 항목을 함께 끌어 **AppDelegate.cs** 파일의 구현 예를 살펴보겠습니다.

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

앱의 요구 사항에 따라 사용자가 동시에 두 개 이상의 창에서 동일한 파일을 열지 않으려고 할 수 있습니다. 예제 앱에서 사용자가 이미 열려 있는 파일을 선택 하는 경우 (열기 **최근** 항목에서 열기 메뉴 항목)에서 파일을 포함 하는 창을 맨 앞으로 가져옵니다.

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

`ViewController` 해당`Path` 속성에 파일의 경로를 포함 하도록 클래스를 디자인 했습니다. 이제 앱에서 현재 열려 있는 모든 창을 반복 합니다. 파일이 windows 중 하나에서 이미 열려 있는 경우 다음을 사용 하 여 다른 모든 창의 맨 앞으로 가져옵니다.

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

일치 항목을 찾을 수 없는 경우 파일이 로드 된 새 창이 열리고 **최근 열기** 메뉴에 파일이 표시 됩니다.

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

### <a name="working-with-custom-window-actions"></a>사용자 지정 창 작업 작업

표준 메뉴 항목으로 연결 되는 기본 제공 **첫 번째 응답자** 작업과 마찬가지로 새 사용자 지정 작업을 만들고 Interface Builder의 메뉴 항목에 연결할 수 있습니다.

먼저 앱의 창 컨트롤러 중 하나에 대 한 사용자 지정 작업을 정의 합니다. 예를 들어:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

다음으로 **Solution Pad** 에서 앱의 스토리 보드 파일을 두 번 클릭 하 여 Xcode의 Interface Builder에서 편집할 수 있도록 엽니다. **응용 프로그램 장면**아래에서 **첫 번째 응답자** 를 선택 하 고 **특성 검사자**로 전환 합니다.

![특성 검사자](menu-images/action01.png "특성 검사자")

**특성 검사자** 의 맨 아래에 있는 **단추를클릭하여새사용자지정작업을추가합니다.+**

![새 작업 추가](menu-images/action02.png "새 작업 추가")

창 컨트롤러에서 만든 사용자 지정 작업과 동일한 이름을 지정 합니다.

![작업 이름 편집](menu-images/action03.png "작업 이름 편집")

컨트롤을 클릭 하 고 메뉴 항목에서 **응용 프로그램 장면의** **첫 번째 응답자** 로 끕니다. 팝업 목록에서 방금 만든 새 작업 (`defineKeyword:` 이 예제에서는)을 선택 합니다.

![작업 연결](menu-images/action04.png "작업 연결")

스토리 보드에 대 한 변경 내용을 저장 하 고 Mac용 Visual Studio로 돌아가서 변경 내용을 동기화 합니다. 앱을 실행 하는 경우 사용자 지정 작업을 연결한 메뉴 항목이 자동으로 사용/사용 안 함으로 설정 되 고 (작업이 열리는 창에 따라) 메뉴 항목을 선택 하면 작업이 실행 됩니다.

[![새 작업 테스트](menu-images/action05.png "새 작업 테스트")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>메뉴 추가, 편집 및 삭제

이전 섹션에서 볼 수 있듯이 Xamarin.ios 응용 프로그램에는 특정 UI 컨트롤에서 자동으로 활성화 하 고 응답 하는 기본 메뉴 및 메뉴 항목의 미리 설정 된 수와 함께 제공 됩니다. 또한 이러한 기본 항목을 사용 하 고 응답할 수 있도록 응용 프로그램에 코드를 추가 하는 방법도 살펴보았습니다.

이 섹션에서는 필요 하지 않은 메뉴 항목을 제거 하 고, 메뉴를 다시 구성 하 고, 새 메뉴, 메뉴 항목 및 동작을 추가 하는 방법을 살펴봅니다.

**Solution Pad** 에서 **주 storyboard** 파일을 두 번 클릭 하 여 편집용으로 엽니다.

[![Xcode에서 UI 편집](menu-images/maint01.png "Xcode에서 UI 편집")](menu-images/maint01-large.png#lightbox)

특정 Xamarin.ios 응용 프로그램의 경우 기본 **보기** 메뉴를 사용 하지 않으므로 제거 하겠습니다. **인터페이스 계층 구조** 에서 주 메뉴 모음의 일부인 **보기** 메뉴 항목을 선택 합니다.

![보기 메뉴 항목 선택](menu-images/maint02.png "보기 메뉴 항목 선택")

메뉴를 삭제 하려면 delete 또는 백스페이스 키를 누릅니다. 다음으로는 **서식** 메뉴의 모든 항목을 사용 하지 않고 하위 메뉴 아래에서 사용할 항목을 이동 하려고 합니다. **인터페이스 계층 구조** 에서 다음 메뉴 항목을 선택 합니다.

![여러 항목 강조 표시](menu-images/maint03.png "여러 항목 강조 표시")

부모 **메뉴** 의 항목을 현재 있는 하위 메뉴에서 끌어 옵니다.

[![메뉴 항목을 부모 메뉴에 끌기](menu-images/maint04.png "메뉴 항목을 부모 메뉴에 끌기")](menu-images/maint04-large.png#lightbox)

이제 메뉴가 다음과 같이 표시 됩니다.

[![새 위치의 항목](menu-images/maint05.png "새 위치의 항목")](menu-images/maint05-large.png#lightbox)

그런 다음 **서식** 메뉴에서 **텍스트** 하위 메뉴를 바깥쪽으로 끌어와 **창** 메뉴의 주 메뉴 모음에 놓습니다.

[![텍스트 메뉴](menu-images/maint06.png "텍스트 메뉴")](menu-images/maint06-large.png#lightbox)

**서식** 메뉴로 돌아가서 **글꼴** 하위 메뉴 항목을 삭제 하겠습니다. 그런 다음 **서식** 메뉴를 선택 하 고 이름을 "Font"로 바꿉니다.

[![글꼴 메뉴](menu-images/maint07.png "글꼴 메뉴")](menu-images/maint07-large.png#lightbox)

다음에는 텍스트 보기를 선택할 때 텍스트 보기의 텍스트에 자동으로 추가 되는 미리 정의 된 구의 사용자 지정 메뉴를 만들어 보겠습니다. **라이브러리 검사자** 의 아래쪽에 있는 검색 상자에 "메뉴"를 입력 합니다. 이를 통해 모든 메뉴 UI 요소를 쉽게 찾고 작업할 수 있습니다.

![라이브러리 검사기](menu-images/maint08.png "라이브러리 검사기")

이제 다음을 수행 하 여 메뉴를 만듭니다.

1. **메뉴 항목** 을 **라이브러리 검사기** 에서 **텍스트** 창과 **창** 메뉴 사이에 있는 메뉴 모음으로 끌어 옵니다. 

    ![라이브러리에서 새 메뉴 항목 선택](menu-images/maint10.png "라이브러리에서 새 메뉴 항목 선택")
2. "문구" 항목의 이름을 바꿉니다. 

    [![메뉴 이름 설정](menu-images/maint09.png "메뉴 이름 설정")](menu-images/maint09-large.png#lightbox)
3. 다음으로 **라이브러리 검사기**에서 **메뉴** 를 끌어 옵니다. 

    ![라이브러리에서 메뉴 선택](menu-images/maint11.png "라이브러리에서 메뉴 선택")
4. 방금 만든 새 **메뉴 항목** 의 드롭다운 **메뉴** 를 클릭 하 고 이름을 "문구"로 변경 합니다. 

    [![메뉴 이름 편집](menu-images/maint12.png "메뉴 이름 편집")](menu-images/maint12-large.png#lightbox)
5. 이제 세 가지 기본 **메뉴 항목인** "Address", "Date" 및 "인사말이"로 이름을 바꿉니다. 

    [![구 메뉴](menu-images/maint13.png "구 메뉴")](menu-images/maint13-large.png#lightbox)
6. **라이브러리 검사기** 에서 **메뉴 항목** 을 끌어서 "Signature"를 호출 하 여 네 번째 **메뉴 항목** 을 추가 해 보겠습니다. 

    [![메뉴 항목 이름 편집](menu-images/maint14.png "메뉴 항목 이름 편집")](menu-images/maint14-large.png#lightbox)
7. 메뉴 모음에 변경 내용을 저장 합니다.

이제 새 메뉴 항목이 코드에 C# 노출 되도록 일련의 사용자 지정 작업을 만들어 보겠습니다. Xcode let에서 **보조자** 보기로 전환 합니다.

[![필요한 작업 만들기](menu-images/maint15.png "필요한 작업 만들기")](menu-images/maint15-large.png#lightbox)

다음 작업을 수행해 보겠습니다.

1. **주소** 메뉴 항목에서 **AppDelegate** 파일로 컨트롤을 끕니다.
2. **연결** 유형을 **작업**으로 전환 합니다. 

    [![작업 유형 선택](menu-images/maint17.png "작업 유형 선택")](menu-images/maint17-large.png#lightbox)
3. "PhraseAddress"의 **이름을** 입력 하 고 **연결** 단추를 클릭 하 여 새 작업을 만듭니다. 

    [![작업 구성](menu-images/maint18.png "작업 구성")](menu-images/maint18-large.png#lightbox)
4. **날짜**, **인사말**및 **서명** 메뉴 항목에 대해 위의 단계를 반복 합니다. 

    [![완료 된 작업](menu-images/maint19.png "완료 된 작업")](menu-images/maint19-large.png#lightbox)
5. 메뉴 모음에 변경 내용을 저장 합니다.

다음으로 코드에서 콘텐츠를 조정할 수 있도록 텍스트 보기에 대 한 유출를 만들어야 합니다. **길잡이 편집기** 에서 `documentText` **viewcontroller .h** 파일을 선택 하 고 라는 새 콘센트를 만듭니다.

[![콘센트 만들기](menu-images/maint20.png "콘센트 만들기")](menu-images/maint20-large.png#lightbox)

Xcode에서 변경 내용을 동기화 하는 Mac용 Visual Studio로 돌아갑니다. 그런 다음 **ViewController.cs** 파일을 편집 하 여 다음과 같이 만듭니다.

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

그러면 `ViewController` 클래스 외부에서 텍스트 뷰의 텍스트가 노출 되 고 창이 포커스를 얻거나 잃을 때 앱 대리자에 게 알립니다. 이제 **AppDelegate.cs** 파일을 편집 하 여 다음과 같이 만듭니다.

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

여기서는 Interface Builder에서 정의한 `AppDelegate` 작업과 콘센트를 사용할 수 있도록 partial 클래스를 만들었습니다. 또한 현재 포커스가 있는 `textEditor` 창을 추적 하기 위해를 노출 합니다.

다음 메서드는 사용자 지정 메뉴 및 메뉴 항목을 처리 하는 데 사용 됩니다.

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

이제 응용 프로그램을 실행 하는 경우 **구** 메뉴의 모든 항목이 활성화 되 고 선택 시 텍스트 보기에 문구 제공이 추가 됩니다.

![실행 중인 앱의 예](menu-images/maint21.png "실행 중인 앱의 예")

이제 응용 프로그램 메뉴 모음을 사용 하 여 작업 하는 기본 사항을 만들었으므로 사용자 지정 상황에 맞는 메뉴를 만드는 방법을 살펴보겠습니다.

### <a name="creating-menus-from-code"></a>코드에서 메뉴 만들기

Xcode의 Interface Builder를 사용 하 여 메뉴 및 메뉴 항목을 만드는 것 외에도 Xamarin.ios 앱에서 메뉴, 하위 메뉴 또는 메뉴 항목을 코드에서 생성, 수정 또는 제거 해야 하는 경우가 있을 수 있습니다.

다음 예제에서는 동적으로 생성 되는 메뉴 항목 및 하위 메뉴에 대 한 정보를 포함 하는 클래스를 만듭니다.

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

이 클래스를 정의 하면 다음 루틴은 개체의 `LanguageFormatCommand`컬렉션을 구문 분석 하 고, 전달 된 기존 메뉴 (Interface Builder에서 만들어짐)의 맨 아래에 추가 하 여 새 메뉴 및 메뉴 항목을 재귀적으로 빌드합니다.

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

`LanguageFormatCommand` 빈`Title` 속성이 있는 개체의 경우이 루틴은 메뉴 섹션 사이에 **구분 기호 메뉴 항목** (얇은 회색 선)을 만듭니다.

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

제목이 제공 되 면 해당 제목이 있는 새 메뉴 항목이 생성 됩니다.

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

개체에 `LanguageFormatCommand` 자식 `LanguageFormatCommand` 개체가 포함 되어 있으면 하위 `AssembleMenu` 메뉴가 만들어지고 메서드를 재귀적으로 호출 하 여 해당 메뉴를 빌드합니다.

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

하위 메뉴가 없는 새 메뉴 항목의 경우 사용자가 선택 하는 메뉴 항목을 처리 하는 코드가 추가 됩니다.

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>메뉴 만들기 테스트

위의 모든 코드를 만든 다음 개체의 `LanguageFormatCommand` 컬렉션을 만든 경우 다음을 수행 합니다.

```csharp
// Define formatting commands
FormattingCommands.Add(new LanguageFormatCommand("Strong","**","**"));
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
FormattingCommands.Add(new LanguageFormatCommand("Image","![](",")"));
FormattingCommands.Add(new LanguageFormatCommand("Image Link","[![](",")](LinkImageHere)"));
```

이 컬렉션을 `AssembleMenu` 함수에 전달 하면 ( **서식** 메뉴가 기본으로 설정 됨) 다음과 같은 동적 메뉴 및 메뉴 항목이 생성 됩니다.

![실행 중인 응용 프로그램의 새 메뉴 항목](menu-images/dynamic01.png "실행 중인 응용 프로그램의 새 메뉴 항목")

#### <a name="removing-menus-and-items"></a>메뉴 및 항목 제거

앱의 사용자 인터페이스에서 메뉴 또는 메뉴 항목을 제거 해야 하는 경우 제거할 항목의 0부터 시작 `RemoveItemAt` 하는 인덱스 `NSMenu` 를 지정 하 여 클래스의 메서드를 간단히 사용할 수 있습니다.

예를 들어 위의 루틴에서 만든 메뉴 및 메뉴 항목을 제거 하기 위해 다음 코드를 사용할 수 있습니다.

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

위의 코드의 경우 처음 네 개의 메뉴 항목이 앱에서 사용할 수 있는 Xcode의 Interface Builder 및 aways에 생성 되므로 동적으로 제거 되지 않습니다.

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>상황에 맞는 메뉴

사용자가 창의 항목을 마우스 오른쪽 단추로 클릭 하거나 컨트롤을 클릭 하면 상황에 맞는 메뉴가 나타납니다. 기본적으로 macOS에 내장 된 여러 UI 요소에는 텍스트 뷰와 같은 상황별 메뉴가 이미 연결 되어 있습니다. 그러나 창에 추가한 UI 요소에 대 한 사용자 지정 상황에 맞는 메뉴를 만들려는 경우가 있을 수 있습니다.

Xcode에서 **기본 storyboard** 파일을 편집 하 고 **창** 창을 디자인에 추가 하 고, **Identity Inspector**에서 해당 **클래스** 를 "NSPanel"로 설정 하 고, **창** 메뉴에 새 **보조자** 항목을 추가 하 고, 새 항목에 연결 해 보겠습니다. **Show Segue**를 사용 하는 창:

[![Segue 형식 설정](menu-images/context01.png "Segue 형식 설정")](menu-images/context01-large.png#lightbox)

다음 작업을 수행해 보겠습니다.

1. **라이브러리 검사기** 에서 **패널** 창으로 **레이블을** 끌어 오고 텍스트를 "Property"로 설정 합니다. 

    [![레이블 값 편집](menu-images/context03.png "레이블 값 편집")](menu-images/context03-large.png#lightbox)
2. 그런 다음 **라이브러리 검사기** 에서 뷰 계층의 뷰 컨트롤러로 **메뉴** 를 끌고 세 개의 기본 메뉴 항목 **문서**, **텍스트** 및 **글꼴**의 이름을 바꿉니다.

    [![필수 메뉴 항목] 입니다. (menu-images/context02.png "필수 메뉴 항목") 입니다.](menu-images/context02-large.png#lightbox)
3. 이제 **속성 레이블에서** **메뉴**로 컨트롤을 끕니다.

    [![드래그 하 여 segue 만들기](menu-images/context04.png "드래그 하 여 segue 만들기")](menu-images/context04-large.png#lightbox)
4. 팝업 대화 상자에서 **메뉴**를 선택 합니다. 

    ![Segue 형식 설정](menu-images/context05.png "Segue 형식 설정")
5. **Id 검사자**에서 뷰 컨트롤러의 클래스를 "PanelViewController"로 설정 합니다. 

    [![Segue 클래스 설정](menu-images/context10.png "Segue 클래스 설정")](menu-images/context10-large.png#lightbox)
6. Mac용 Visual Studio으로 다시 전환 하 여 동기화 한 다음 Interface Builder로 돌아갑니다.
7. **길잡이 편집기** 로 전환 하 고 **PanelViewController** 파일을 선택 합니다.
8. 이라는`propertyDocument` **문서** 메뉴 항목에 대 한 작업을 만듭니다. 

    [![작업 구성](menu-images/context06.png "작업 구성")](menu-images/context06-large.png#lightbox)
9. 나머지 메뉴 항목에 대 한 작업 만들기를 반복 합니다. 

    [![필요한 작업](menu-images/context07.png "필요한 작업")](menu-images/context07-large.png#lightbox)
10. 마지막으로 라는 `propertyLabel` **속성 레이블에** 대 한 콘센트를 만듭니다. 

    [![콘센트 구성](menu-images/context08.png "콘센트 구성")](menu-images/context08-large.png#lightbox)
11. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

**PanelViewController.cs** 파일을 편집 하 고 다음 코드를 추가 합니다.

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

이제 응용 프로그램을 실행 하 고 패널에서 속성 레이블을 마우스 오른쪽 단추로 클릭 하면 사용자 지정 상황에 맞는 메뉴가 표시 됩니다. 메뉴에서 및 항목을 선택 하면 레이블의 값이 다음과 같이 변경 됩니다.

![실행 상황에 맞는 메뉴](menu-images/context09.png "실행 상황에 맞는 메뉴")

다음으로 상태 표시줄 메뉴 만들기를 살펴보겠습니다.

## <a name="status-bar-menus"></a>상태 표시줄 메뉴

상태 표시줄 메뉴는 응용 프로그램의 상태를 반영 하는 이미지 또는 메뉴와 같은 사용자 의견을 제공 하는 상태 메뉴 항목의 컬렉션을 표시 합니다. 응용 프로그램의 상태 표시줄 메뉴는 응용 프로그램이 백그라운드에서 실행 되는 경우에도 활성화 되 고 활성화 됩니다. 시스템 수준 상태 표시줄은 응용 프로그램 메뉴 모음의 오른쪽에 있으며, 현재 macOS에서 사용할 수 있는 유일한 상태 표시줄입니다.

**AppDelegate.cs** 파일을 편집 하 고 메서드를 `DidFinishLaunching` 다음과 같이 만듭니다.

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

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;`시스템 차원의 상태 표시줄에 대 한 액세스를 제공 합니다. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);`새 상태 표시줄 항목을 만듭니다. 여기에서 메뉴 및 메뉴 항목을 몇 개 만들고 방금 만든 상태 표시줄 항목에 메뉴를 연결 합니다. 

응용 프로그램을 실행 하면 새 상태 표시줄 항목이 표시 됩니다. 메뉴에서 항목을 선택 하면 텍스트 보기의 텍스트가 변경 됩니다. 

![상태 표시줄 메뉴 실행 중](menu-images/statusbar01.png "상태 표시줄 메뉴 실행 중")

다음으로 사용자 지정 도크 메뉴 항목을 만드는 방법을 살펴보겠습니다.

## <a name="custom-dock-menus"></a>사용자 지정 고정 메뉴

사용자가 도크의 응용 프로그램 아이콘을 마우스 오른쪽 단추로 클릭 하거나 컨트롤을 클릭 하면 Mac 응용 프로그램에 대 한 도킹 메뉴가 나타납니다.

![사용자 지정 도크 메뉴](menu-images/dock01.png "사용자 지정 도크 메뉴")

다음을 수행 하 여 응용 프로그램에 대 한 사용자 지정 도크 메뉴를 만들어 보겠습니다.

1. Mac용 Visual Studio에서 응용 프로그램의 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. 새 파일 대화 상자에서 **xamarin.ios** > **빈 인터페이스 정의**를 선택 하 고 **이름** 에 "DockMenu"를 사용 하 고 **새로** 만들기 단추를 클릭 하 여 새 **DockMenu** 파일을 만듭니다.

    ![빈 인터페이스 정의 추가](menu-images/dock02.png "빈 인터페이스 정의 추가")
2. **Solution Pad**에서 **xib** 파일을 두 번 클릭 하 여 Xcode에서 편집할 수 있도록 엽니다. 다음 항목을 사용 하 여 새 **메뉴** 를 만듭니다. **주소**, **날짜**, **인사말**및 **서명** 

    [![UI 레이아웃](menu-images/dock03.png "UI 레이아웃")](menu-images/dock03-large.png#lightbox)
3. 다음으로, 위의 메뉴 [추가, 편집 및 삭제](#Adding,_Editing_and_Deleting_Menus) 섹션에서 사용자 지정 메뉴에 대해 만든 기존 작업에 새 메뉴 항목을 연결 하겠습니다. **연결 검사자** 로 전환 하 고 **인터페이스 계층**에서 **첫 번째 응답자** 를 선택 합니다. 아래로 스크롤하여 작업을 `phraseAddress:` 찾습니다. 해당 작업의 원에서 **주소** 메뉴 항목으로 선을 끌어 옵니다.

    [![끌어서 작업] 연결 (menu-images/dock04.png "끌어서 작업") 연결](menu-images/dock04-large.png#lightbox)
4. 해당 작업에 연결 하는 다른 모든 메뉴 항목에 대해이 작업을 반복 합니다. 

    [![필요한 작업](menu-images/dock05.png "필요한 작업")](menu-images/dock05-large.png#lightbox)
5. 다음으로 **인터페이스 계층 구조**에서 **응용 프로그램** 을 선택 합니다. **연결 검사기**에서 `dockMenu` 콘센트의 원에서 방금 만든 메뉴로 선을 끌어 옵니다.

    [연결 ![을 콘센트 위로 끌기] 연결 (menu-images/dock06.png "을 콘센트 위로 끌기")](menu-images/dock06-large.png#lightbox)
6. 변경 내용을 저장 하 고 다시 Mac용 Visual Studio로 전환 하 여 Xcode와 동기화 합니다.
7. **Info.plist** 파일을 두 번 클릭 하 여 편집용으로 엽니다. 

    [![Info.plist 파일 편집](menu-images/dock07.png "Info.plist 파일 편집")](menu-images/dock07-large.png#lightbox)
8. 화면 맨 아래에 있는 **원본** 탭을 클릭 합니다. 

    [![원본 뷰 선택](menu-images/dock08.png "원본 뷰 선택")](menu-images/dock08-large.png#lightbox)
9. **새 항목 추가**를 클릭 하 고, 녹색 더하기 단추를 클릭 하 고, 속성 이름을 "AppleDockMenu"로 설정 하 고, 값을 "DockMenu" (확장명이 없는 xib 파일의 이름)로 설정 합니다. 

    [![DockMenu 항목 추가](menu-images/dock09.png "DockMenu 항목 추가")](menu-images/dock09-large.png#lightbox)

이제 응용 프로그램을 실행 하 고 도크에서 해당 아이콘을 마우스 오른쪽 단추로 클릭 하면 새 메뉴 항목이 표시 됩니다.

![실행 되는 도킹 메뉴의 예](menu-images/dock10.png "실행 되는 도킹 메뉴의 예")

메뉴에서 사용자 지정 항목 중 하나를 선택 하면 텍스트 뷰의 텍스트가 수정 됩니다.

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>팝업 단추 및 풀 다운 목록

팝업 단추는 선택한 항목을 표시 하 고 사용자가 클릭할 때 선택할 수 있는 옵션 목록을 표시 합니다. 풀 다운 목록은 일반적으로 현재 태스크의 컨텍스트와 관련 된 명령을 선택 하는 데 사용 되는 팝업 단추의 유형입니다. 둘 다 창의 어디에 나 나타날 수 있습니다.

다음을 수행 하 여 응용 프로그램에 대 한 사용자 지정 팝업 단추를 만들어 보겠습니다.

1. Xcode에서 **기본 storyboard** 파일을 편집 하 고 **라이브러리 검사자** 의 **팝업 단추** 를 [상황에 맞는 메뉴](#Contextual_Menus) 섹션에서 만든 **패널** 창으로 끌어 놓습니다. 

    [![팝업 단추 추가](menu-images/popup01.png "팝업 단추 추가")](menu-images/popup01-large.png#lightbox)
2. 새 메뉴 항목을 추가 하 고 팝업에서 항목의 제목을로 설정 합니다. **주소**, **날짜**, **인사말**및 **서명** 

    [![메뉴 항목 구성](menu-images/popup02.png "메뉴 항목 구성")](menu-images/popup02-large.png#lightbox)
3. 다음으로, 위의 메뉴 [추가, 편집 및 삭제](#Adding,_Editing_and_Deleting_Menus) 섹션에서 사용자 지정 메뉴에 대해 만든 기존 작업에 새 메뉴 항목을 연결 하겠습니다. **연결 검사자** 로 전환 하 고 **인터페이스 계층**에서 **첫 번째 응답자** 를 선택 합니다. 아래로 스크롤하여 작업을 `phraseAddress:` 찾습니다. 해당 작업의 원에서 **주소** 메뉴 항목으로 선을 끌어 옵니다. 

    [![끌어서 작업] 연결 (menu-images/popup03.png "끌어서 작업") 연결](menu-images/popup03-large.png#lightbox)
4. 해당 작업에 연결 하는 다른 모든 메뉴 항목에 대해이 작업을 반복 합니다. 

    [![필요한 모든 작업](menu-images/popup04.png "필요한 모든 작업")](menu-images/popup04-large.png#lightbox)
5. 변경 내용을 저장 하 고 다시 Mac용 Visual Studio로 전환 하 여 Xcode와 동기화 합니다.

이제 응용 프로그램을 실행 하 고 팝업에서 항목을 선택 하면 텍스트 뷰의 텍스트가 변경 됩니다.

![실행 중인 팝업의 예](menu-images/popup05.png "실행 중인 팝업의 예")

팝업 단추와 정확히 같은 방식으로 풀 다운 목록을 만들고 작업할 수 있습니다. 기존 작업에 연결 하는 대신 [상황별](#Contextual_Menus) 메뉴 섹션의 상황에 맞는 메뉴에 대 한 것 처럼 사용자 고유의 사용자 지정 작업을 만들 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 메뉴 및 메뉴 항목을 사용 하는 방법에 대해 자세히 살펴봅니다. 먼저 응용 프로그램의 메뉴 모음을 검사 한 다음 상황에 맞는 메뉴 및 사용자 지정 도킹 메뉴를 검사 한 다음 상황에 맞는 메뉴를 살펴보았습니다. 마지막으로 팝업 메뉴와 풀 다운 목록에 대해 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [MacMenus (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macmenus)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [인간 인터페이스 지침-메뉴](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [응용 프로그램 메뉴 및 팝업 목록 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
