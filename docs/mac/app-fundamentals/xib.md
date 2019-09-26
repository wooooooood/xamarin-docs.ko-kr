---
title: Xamarin.ios의 xib 파일
description: 이 문서에서는 Xcode의 Interface Builder에서 만든 xib 파일을 사용 하 여 Xamarin.ios 응용 프로그램에 대 한 사용자 인터페이스를 만들고 유지 관리 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: be737dfb92cf2ce90dc64dd527f908d52cf2c580
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "70770349"
---
# <a name="xib-files-in-xamarinmac"></a>Xamarin.ios의 xib 파일

_이 문서에서는 Xcode의 Interface Builder에서 만든 xib 파일을 사용 하 여 Xamarin.ios 응용 프로그램에 대 한 사용자 인터페이스를 만들고 유지 관리 하는 방법을 설명 합니다._

> [!NOTE]
> Xamarin.ios 앱에 대 한 사용자 인터페이스를 만드는 기본 방법은 storyboard를 사용 하는 것입니다. 이 설명서는 이전 버전의 Xamarin.ios 프로젝트를 사용 하 고 기록 하기 위해 아직 준비 되지 않았습니다. 자세한 내용은 [Storyboard 소개](~/mac/platform/storyboards/index.md) 설명서를 참조 하세요.

## <a name="overview"></a>개요

동일한 사용자 인터페이스 요소에 대 한 액세스 권한이 Xamarin.Mac 응용 프로그램에서 C# 및.NET으로 작업을 하는 경우 및 작업 하는 개발자 도구를 *Objective-C* 및 *Xcode* 않습니다. Xamarin.ios는 Xcode와 직접 통합 되므로 Xcode의 _Interface Builder_ 를 사용 하 여 사용자 인터페이스를 만들고 유지 관리할 수 있습니다 (또는 필요에 따라 코드에서 C# 직접 만들 수 있음).

Xib 파일은 macOS에서 Xcode의 Interface Builder에 그래픽으로 만들고 유지 관리 하는 응용 프로그램의 사용자 인터페이스 요소 (예: 메뉴, 창, 보기, 레이블, 텍스트 필드)를 정의 하는 데 사용 됩니다.

[![실행 중인 앱의 예](xib-images/intro01.png "실행 중인 앱의 예")](xib-images/intro01-large.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램의 xib 파일 작업에 대 한 기본 사항을 다룹니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 좋습니다 .이 문서에서는이 문서에서 사용할 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 특성 요소 Objective-C 개체 및 UI에 C# 클래스를 연결 하는 데 사용 합니다.

## <a name="introduction-to-xcode-and-interface-builder"></a>Xcode 및 Interface Builder 소개

Xcode의 일부로 Apple은 디자이너에서 시각적으로 사용자 인터페이스를 만들 수 있도록 하는 Interface Builder 이라는 도구를 만들었습니다. Xamarin.Mac 통합 fluently 작성기 인터페이스를 Objective-C 사용자가 수행 하는 같은 도구로 UI를 만들 수 있습니다.

### <a name="components-of-xcode"></a>Xcode 구성 요소

Mac용 Visual Studio에서 Xcode의 xib 파일을 열면 왼쪽에는 **프로젝트 탐색기** 가 열리고, 가운데에는 **인터페이스 계층 구조** 와 **인터페이스 편집기** 가 표시 되 고, 오른쪽에는 **속성 & 유틸리티** 섹션이 표시 됩니다.

[![XCODE UI의 구성 요소](xib-images/xcode03.png "XCODE UI의 구성 요소")](xib-images/xcode03-large.png#lightbox)

이러한 각 Xcode 섹션의 기능과이를 사용 하 여 Xamarin.ios 응용 프로그램에 대 한 인터페이스를 만드는 방법을 살펴보겠습니다.

#### <a name="project-navigation"></a>프로젝트 탐색

Xcode에서 편집을 위해 xib 파일을 열면 Mac용 Visual Studio은 백그라운드에서 Xcode 프로젝트 파일을 만들어 자체와 Xcode 간의 변경 내용을 전달 합니다. 나중에 Xcode에서 Mac용 Visual Studio로 다시 전환 하면이 프로젝트에 대 한 변경 내용이 Mac용 Visual Studio 하 여 Xamarin.ios 프로젝트와 동기화 됩니다.

**프로젝트 탐색** 섹션에서이 _shim_ Xcode 프로젝트를 구성 하는 모든 파일을 탐색할 수 있습니다. 일반적으로이 목록에 있는 xib 파일 (예: **xib** 및 **mainwindow.xaml**)에만 관심이 있습니다.

#### <a name="interface-hierarchy"></a>인터페이스 계층 구조

**인터페이스 계층 구조** 섹션에서 **자리 표시자** 및 주 **창과**같은 사용자 인터페이스의 여러 가지 주요 속성에 쉽게 액세스할 수 있습니다. 이 섹션을 사용 하 여 사용자 인터페이스를 구성 하는 개별 요소 (보기)에 액세스 하 고 계층 구조 내에서 해당 요소를 끌어서 중첩 하는 방법을 조정할 수도 있습니다.

#### <a name="interface-editor"></a>인터페이스 편집기

**인터페이스 편집기** 섹션에서는 사용자 인터페이스를 그래픽으로 레이아웃 하는 화면을 제공 합니다. **속성 & 유틸리티** 섹션의 **라이브러리** 섹션에서 요소를 끌어 디자인을 만듭니다. 디자인 화면에 사용자 인터페이스 요소 (뷰)를 추가 하면 인터페이스 **편집기**에 표시 되는 순서 대로 **인터페이스 계층 구조** 섹션에 추가 됩니다.

#### <a name="properties--utilities"></a>속성 & 유틸리티

**속성 & 유틸리티** 섹션은 다음과 같은 **속성** (검사기 라고도 함) 및 **라이브러리**를 사용 하는 두 가지 주요 섹션으로 나뉩니다.

![속성 검사자](xib-images/xcode04.png "속성 검사자")

처음에는이 섹션이 거의 비어 있지만 **인터페이스 편집기나** **인터페이스 계층 구조**에서 요소를 선택 하면 **속성** 섹션이 지정 된 요소 및 속성에 대 한 정보로 채워집니다. 조정해.

**속성** 섹션 내에는 다음 그림처럼 8개의 *검사기 탭*이 있습니다.

[![모든 검사기의 개요](xib-images/xcode05.png "모든 검사기의 개요")](xib-images/xcode05-large.png#lightbox)

왼쪽부터 시작해서 오른쪽으로 가면서 다음과 같은 탭이 있습니다.

- **파일 검사기** – 파일 검사기는 파일 이름, 편집 중인 Xib 파일의 위치와 같은 파일 정보를 보여줍니다.
- **빠른 도움말** – 빠른 도움말 탭에서는 현재 Xcode에서 선택한 항목에 따라 상황에 맞는 도움말을 제공합니다.
- **ID 검사기** – ID 검사기는 선택한 컨트롤/보기에 대한 정보를 제공합니다.
- **특성 검사자** – 특성 검사자를 사용 하 여 선택한 컨트롤/뷰의 다양 한 특성을 사용자 지정할 수 있습니다.
- **크기 검사자** – 크기 검사자를 사용 하 여 선택한 컨트롤/뷰의 크기 및 크기 조정 동작을 제어할 수 있습니다.
- **연결 검사기** – 연결 검사기는 선택한 컨트롤의 콘센트 및 동작 연결을 표시 합니다. 잠시 후에는 출 선 및 작업을 살펴봅니다.
- **바인딩 검사자** – 바인딩 검사기를 사용 하면 해당 값이 데이터 모델에 자동으로 바인딩되도록 컨트롤을 구성할 수 있습니다.
- **보기 효과 검사자** – 애니메이션 보기와 같은 컨트롤에 대 한 효과를 지정할 수 있습니다.

**라이브러리** 섹션에서 디자이너에 추가 하 여 사용자 인터페이스를 그래픽으로 작성 하는 컨트롤과 개체를 찾을 수 있습니다.

![라이브러리 검사기의 예](xib-images/xcode06.png "라이브러리 검사기의 예")

이제 Xcode IDE 및 Interface Builder에 대해 잘 알고 있으므로이를 사용 하 여 사용자 인터페이스를 만드는 방법을 살펴보겠습니다.

## <a name="creating-and-maintaining-windows-in-xcode"></a>Xcode에서 windows 만들기 및 유지 관리

Xamarin.ios 앱의 사용자 인터페이스를 만드는 기본 방법은 Storyboard를 사용 하는 것입니다. 자세한 내용은 [스토리 보드 설명서 소개](~/mac/platform/storyboards/index.md) 를 참조 하세요. 그러면 xamarin에서 시작 하는 모든 새 프로젝트가 생성 됩니다. 기본.

Xib 기반 UI를 사용 하도록 전환 하려면 다음을 수행 합니다.

1. Mac용 Visual Studio를 열고 새 Xamarin.ios 프로젝트를 시작 합니다.
2. **Solution Pad**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.
3. **Mac** > **Windows 컨트롤러**를 선택 합니다.

    ![새 창 컨트롤러 추가](xib-images/setup00.png "새 창 컨트롤러 추가")
4. 이름 `MainWindow` 으로를 입력 하 고 **새로 만들기** 단추를 클릭 합니다.

    ![새 주 창 추가](xib-images/setup01.png "새 주 창 추가")
5. 프로젝트를 다시 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.
6. **Mac** > **주 메뉴**를 선택 합니다.

    ![새 주 메뉴 추가](xib-images/setup02.png "새 주 메뉴 추가")
7. 이름을 `MainMenu` 그대로 두고 **새로 만들기** 단추를 클릭 합니다.
8. **Solution Pad** 에서 **주 storyboard** 파일을 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **제거**를 선택 합니다.

    ![주 스토리 보드 선택](xib-images/setup03.png "주 스토리 보드 선택")
9. 제거 대화 상자에서 **삭제** 단추를 클릭 합니다.

    ![삭제 확인](xib-images/setup04.png "삭제 확인")
10. **Solution Pad**에서 info.plist 파일을 두 번 클릭 하 여 편집을 위해 엽니다 **.**
11. `MainMenu` **기본 인터페이스** 드롭다운 목록에서 다음을 선택 합니다.

    [![주 메뉴 설정](xib-images/setup05.png "주 메뉴 설정")](xib-images/setup05-large.png#lightbox)
12. **Solution Pad**에서 **xib** 파일을 두 번 클릭 하 여 Xcode의 Interface Builder에서 편집할 수 있도록 엽니다.
13. **라이브러리 검사기**의 검색 필드에 `object` 를 입력 한 다음 새 **개체** 를 디자인 화면으로 끌어 옵니다.

    [![주 메뉴 편집](xib-images/setup06.png "주 메뉴 편집")](xib-images/setup06-large.png#lightbox)
14. **Identity Inspector**에서 클래스에 대해 `AppDelegate` 을 입력합니다.

    [![앱 대리자 선택](xib-images/setup07.png "앱 대리자 선택")](xib-images/setup07-large.png#lightbox)
15. **인터페이스 계층 구조**에서 **파일의 소유자** 를 선택 하 고, **연결 검사자** 로 전환 하 고, 대리자의 줄을 `AppDelegate` 프로젝트에 추가 된 **개체** 에 끌어 놓습니다.

    [![앱 대리자 연결](xib-images/setup08.png "앱 대리자 연결")](xib-images/setup08-large.png#lightbox)
16. 변경 내용을 저장 하 고 Mac용 Visual Studio로 돌아갑니다.

이러한 모든 변경 내용을 적용 하 여 **AppDelegate.cs** 파일을 편집 하 고 다음과 같이 표시 합니다.

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

이제 응용 프로그램의 주 창이 창 컨트롤러를 추가할 때 프로젝트에 자동으로 포함 되는 **xib** 파일에 정의 됩니다. Windows 디자인을 편집 하려면 **Solution Pad**에서 **mainwindow.xaml xib** 파일을 두 번 클릭 합니다.

![Xib 파일 선택 mainwindow.xaml](xib-images/edit01.png "Xib 파일 선택 mainwindow.xaml")

이렇게 하면 Xcode의 Interface Builder에서 창 디자인이 열립니다.

[![Mainwindow.xaml xib를 편집 합니다.](xib-images/edit02.png "Mainwindow.xaml xib를 편집 합니다.")](xib-images/edit02-large.png#lightbox)

### <a name="standard-window-workflow"></a>표준 창 워크플로

Xamarin.ios 응용 프로그램에서 만들고 사용 하는 모든 창의 경우 프로세스는 기본적으로 동일 합니다.

1. 프로젝트에 자동으로 추가 되는 기본값이 아닌 새 창에는 새 창 정의를 프로젝트에 추가 합니다.
2. Xib 파일을 두 번 클릭 하 여 Xcode의 Interface Builder에서 편집할 창 디자인을 엽니다.
3. **특성 검사자** 및 **크기 검사자**에서 필요한 창 속성을 설정 합니다.
4. 인터페이스를 빌드하고 **특성 검사자**에서 구성 하는 데 필요한 컨트롤을 끌어 옵니다.
5. **크기 검사기** 를 사용 하 여 UI 요소의 크기 조정을 처리 합니다.
6. 콘센트 및 작업을 통해 코드에 C# 창의 UI 요소를 노출 합니다.
7. 변경 내용을 저장 하 고 다시 Mac용 Visual Studio로 전환 하 여 Xcode와 동기화 합니다.

### <a name="designing-a-window-layout"></a>창 레이아웃 디자인

인터페이스 작성기에서 사용자 인터페이스를 레이아웃 하는 프로세스는 기본적으로 추가 하는 모든 요소에 대해 동일 합니다.

1. **라이브러리 검사기** 에서 원하는 컨트롤을 찾아서 **인터페이스 편집기** 로 끌어 놓습니다.
2. **특성 검사자**에서 필요한 창 속성을 설정 합니다.
3. **크기 검사기** 를 사용 하 여 UI 요소의 크기 조정을 처리 합니다.
4. 사용자 지정 클래스를 사용 하는 경우 **Identity Inspector**에서 설정 합니다.
5. 콘센트 및 작업을 통해 C# 코드에 UI 요소를 노출 합니다.
6. 변경 내용을 저장 하 고 다시 Mac용 Visual Studio로 전환 하 여 Xcode와 동기화 합니다.

예를 들면 다음과 같습니다.

1. Xcode의 **라이브러리 섹션**에서 **누름 단추**를 끕니다.

    [![라이브러리에서 단추 선택](xib-images/xcode07.png "라이브러리에서 단추 선택")](xib-images/xcode07-large.png#lightbox)
2. 단추를 **인터페이스 편집기**의 **창** 에 놓습니다.

    [![창에 단추 추가](xib-images/xcode08.png "창에 단추 추가")](xib-images/xcode08-large.png#lightbox)
3. **특성 검사기**에서 **제목** 속성을 클릭하고 단추 제목을 `Click Me`로 변경합니다.

    ![단추 특성 설정](xib-images/xcode09.png "단추 특성 설정")
4. **라이브러리 섹션**에서 **레이블**을 끕니다.

    [![라이브러리에서 레이블 선택](xib-images/xcode10.png "라이브러리에서 레이블 선택")](xib-images/xcode10-large.png#lightbox)
5. **인터페이스 편집기**에서 단추 옆에 있는 **창**에 레이블을 놓습니다.

    [![창에 레이블 추가](xib-images/xcode11.png "창에 레이블 추가")](xib-images/xcode11-large.png#lightbox)
6. 레이블의 오른쪽 핸들을 잡고 창의 가장자리 근처까지 끕니다.

    [![레이블 크기 조정](xib-images/xcode12.png "레이블 크기 조정")](xib-images/xcode12-large.png#lightbox)
7. **인터페이스 편집기**에서 레이블을 선택한 상태에서 **크기 검사자**로 전환 합니다.

    ![크기 검사자 선택](xib-images/xcode13.png "크기 검사자 선택")
8. **자동 크기 조정 상자** 에서 오른쪽에 있는 **dim red 대괄호** 를 클릭 하 고 가운데에 있는 **흐리게 빨강 가로 화살표** 를 클릭 합니다.

    ![자동 크기 조정 속성 편집](xib-images/xcode14.png "자동 크기 조정 속성 편집")
9. 이렇게 하면 실행 중인 응용 프로그램에서 창의 크기를 조정할 때 레이블이 늘어나거나 축소 됩니다. **자동 크기 조정 상자** 상자의 **빨간색 대괄호** 와 왼쪽은 레이블이 지정 된 X 및 Y 위치에 멈춰 있음을 알려 줍니다.
10. 사용자 인터페이스에 대 한 변경 내용 저장

컨트롤의 크기를 조정 하 고 이동 하는 중에는 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)을 기반으로 하는 유용한 맞춤 힌트를 제공 하는 Interface Builder에 대해 알고 있어야 합니다. 이러한 지침은 Mac 사용자에 게 친숙 한 모양과 느낌을 주는 고품질 응용 프로그램을 만드는 데 도움이 됩니다.

**인터페이스 계층 구조** 섹션에서 사용자 인터페이스를 구성 하는 요소의 레이아웃과 계층 구조가 어떻게 표시 되는지 확인 합니다.

![인터페이스 계층 구조에서 요소 선택](xib-images/xcode15.png "인터페이스 계층 구조에서 요소 선택")

여기에서 필요에 따라 편집 하거나 끌 항목을 선택 하 여 UI 요소를 다시 정렬할 수 있습니다. 예를 들어 다른 요소에서 UI 요소를 검사 하는 경우이 요소를 목록의 맨 아래로 끌어 창에서 맨 위 항목으로 만들 수 있습니다.

Xamarin.ios 응용 프로그램에서 Windows를 사용 하는 방법에 대 한 자세한 내용은 [windows](~/mac/user-interface/window.md) 설명서를 참조 하세요.

## <a name="exposing-ui-elements-to-c-code"></a>코드에 C# UI 요소 노출

Interface Builder에서 사용자 인터페이스의 모양과 느낌을 파악 했으면 코드에서 C# 액세스할 수 있도록 UI의 요소를 노출 해야 합니다. 이렇게 하려면 작업과 콘센트가 사용 됩니다.

### <a name="setting-a-custom-main-window-controller"></a>사용자 지정 주 창 컨트롤러 설정

UI 요소를 코드에 C# 노출 하는 작업 및 작업을 만들 수 있으려면 xamarin.ios 앱은 사용자 지정 창 컨트롤러를 사용 해야 합니다.

다음을 수행합니다.

1. Xcode의 Interface Builder에서 앱의 스토리 보드를 엽니다.
2. Design Surface에서 `NSWindowController` 을 선택 합니다.
3. **Identity Inspector** 뷰로 전환 하 고 `WindowController` **클래스 이름**으로을 입력 합니다.

    [![클래스 이름 편집](xib-images/windowcontroller01.png "클래스 이름 편집")](xib-images/windowcontroller01-large.png#lightbox)
4. 변경 내용을 저장 하 고 동기화 할 Mac용 Visual Studio로 돌아갑니다.
5. **WindowController.cs** 파일은 Mac용 Visual Studio의 **Solution Pad** 에서 프로젝트에 추가 됩니다.

    ![Mac용 Visual Studio 새 클래스 이름](xib-images/windowcontroller02.png "Mac용 Visual Studio 새 클래스 이름")
6. Xcode의 Interface Builder에서 Storyboard를 다시 엽니다.
7. **Windowcontroller .h** 파일을 사용할 수 있습니다.

    [![Xcode의 일치 하는 .h 파일](xib-images/windowcontroller03.png "Xcode의 일치 하는 .h 파일")](xib-images/windowcontroller03-large.png#lightbox)

### <a name="outlets-and-actions"></a>출 선 및 작업

따라서 콘센트 및 작업은 무엇 인가요? 전통적인 .NET 사용자 인터페이스 프로그래밍에서, 사용자 인터페이스의 컨트롤은 추가될 때 자동으로 속성으로 노출됩니다. Mac에서는 동작이 약간 다릅니다. 보기에 컨트롤을 추가하기만 해서는 코드에 액세스할 수 없습니다. 개발자가 UI 요소를 코드에 명시적으로 노출해야 합니다. 이 작업을 위해 Apple에서는 다음 두 가지 옵션을 제공 합니다.

- **출선** – 출선은 속성과 비슷합니다. 컨트롤을 콘센트에 연결 하는 경우에는 속성을 통해 코드에 노출 되므로 이벤트 처리기를 연결 하거나 메서드를 호출 하는 등의 작업을 수행할 수 있습니다.
- **작업** – 작업은 WPF의 명령 패턴과 비슷합니다. 예를 들어 컨트롤에 대해 작업을 수행 하는 경우 단추 클릭 이라고 하는 경우 컨트롤은 코드에서 메서드를 자동으로 호출 합니다. 작업은 여러 컨트롤을 동일한 작업에 연결할 수 있으므로 강력 하 고 편리 합니다.

Xcode에서 콘센트 및 작업은 *컨트롤 끌기를*통해 코드에 직접 추가 됩니다. 좀 더 구체적으로 말해서, 콘센트가 나 작업을 만들려면 콘센트가 나 작업을 추가할 컨트롤 요소를 선택 하 고 키보드에서 **컨트롤** 단추를 누른 채 해당 컨트롤을 직접 코드로 끌어 옵니다.

Xamarin.Mac 개발자를 위한 콘센트 또는 동작을 만들려면 원하는 C# 파일에 해당 하는 Objective-C 스텁 파일에 끌어을 의미 합니다. Mac용 Visual Studio Interface Builder을 사용 하기 위해 생성 한 shim Xcode 프로젝트의 일부로 **mainwindow.xaml** 이라는 파일을 만들었습니다.

[![Xcode에 있는 .h 파일의 예](xib-images/xcode16.png "Xcode에 있는 .h 파일의 예")](xib-images/xcode16-large.png#lightbox)

이 **MainWindow.designer.cs** 파일은 새 `NSWindow` 가 만들어질 때 xamarin.ios 프로젝트에 자동으로 추가 되는를 미러링합니다. 이 파일은 Interface Builder에서 변경한 내용을 동기화 하는 데 사용 되며, UI 요소가 코드에 C# 노출 되도록 콘센트 및 작업을 만듭니다.

#### <a name="adding-an-outlet"></a>콘센트 추가

출 선 및 작업에 대 한 기본적인 이해를 통해 UI 요소를 C# 코드에 노출 하는 코드를 만드는 방법을 살펴보겠습니다.

다음을 수행합니다.

1. 화면의 오른쪽 맨 위 모서리에 있는 Xcode에서 **이중 원** 단추를 클릭하여 **도우미 편집기**를 엽니다.

    [![길잡이 편집기 선택](xib-images/outlet01.png "길잡이 편집기 선택")](xib-images/outlet01-large.png#lightbox)
2. Xcode가 분할 보기 모드로 전환되어 한 쪽에는 **인터페이스 편집기**가, 다른 쪽에는 **코드 편집기** 표시됩니다.
3. Xcode는 **코드 편집기**에서 잘못 된 **mainwindowcontroller. m** 파일을 자동으로 선택 했습니다. 위의 콘센트 및 작업에 대 한 논의를 잊어버린 경우 **mainwindow.xaml** 을 선택 해야 합니다.
4. **코드 편집기** 위쪽에서 **자동 링크** 를 클릭 하 고 **mainwindow.xaml** 파일을 선택 합니다.

    [![올바른 .h 파일 선택](xib-images/outlet02.png "올바른 .h 파일 선택")](xib-images/outlet02-large.png#lightbox)
5. Xcode가 이제 올바른 파일을 선택했습니다.

    [![올바른 파일이 선택 됨](xib-images/outlet03.png "올바른 파일이 선택 됨")](xib-images/outlet03-large.png#lightbox)
6. **마지막 단계는 매우 중요합니다!** 올바른 파일을 선택 하지 않은 경우에는 콘센트 및 작업을 만들 수 없으며에서 C#잘못 된 클래스에 노출 됩니다.
7. **인터페이스 편집기**에서 키보드의 **Control** 키를 누른 채 위에서 만든 레이블을 클릭 하 여 `@interface MainWindow : NSWindow { }` 코드 바로 아래에 있는 코드 편집기 위로 끕니다.

    [![끌어서 새 콘센트 만들기](xib-images/outlet04.png "끌어서 새 콘센트 만들기")](xib-images/outlet04-large.png#lightbox)
8. 대화 상자가 표시됩니다. **연결** 을 콘센트로 설정 된 채로 두고 `ClickedLabel` **이름**으로를 입력 합니다.

    [![유출 속성 설정](xib-images/outlet05.png "유출 속성 설정")](xib-images/outlet05-large.png#lightbox)
9. **연결** 단추를 클릭 하 여 콘센트를 만듭니다.

    ![완료 된 콘센트](xib-images/outlet06.png "완료 된 콘센트")
10. 파일의 변경 내용을 저장합니다.

#### <a name="adding-an-action"></a>작업 추가

다음으로, UI 요소와의 사용자 상호 작용을 C# 코드에 노출 하는 작업을 만드는 방법을 살펴보겠습니다.

다음을 수행합니다.

1. 여전히 **길잡이 편집기** 에 있고 **Mainwindow.xaml** 파일이 **코드 편집기**에 표시 되는지 확인 합니다.
2. **인터페이스 편집기**에서 키보드의 **Control** 키를 누른 채 위에서 만든 단추를 클릭 하 여 `@property (assign) IBOutlet NSTextField *ClickedLabel;` 코드 바로 아래에 있는 코드 편집기 위로 끕니다.

    [![끌어서 작업 만들기](xib-images/action01.png "끌어서 작업 만들기")](xib-images/action01-large.png#lightbox)
3. **연결** 유형을 작업으로 변경 합니다.

    [![작업 유형 선택](xib-images/action02.png "작업 유형 선택")](xib-images/action02-large.png#lightbox)
4. **이름**으로 `ClickedButton`을 입력합니다.

    [![작업 구성](xib-images/action03.png "작업 구성")](xib-images/action03-large.png#lightbox)
5. **연결** 단추를 클릭 하 여 작업을 만듭니다.

    ![완료 된 작업](xib-images/action04.png "완료 된 작업")
6. 파일의 변경 내용을 저장합니다.

사용자 인터페이스를 연결 하 여 코드에 C# 노출 하 고 Mac용 Visual Studio 다시 전환 하 여 Xcode 및 Interface Builder의 변경 내용을 동기화 할 수 있습니다.

### <a name="writing-the-code"></a>코드 작성

사용자 인터페이스를 만들고, 해당 UI 요소가 콘센트 및 작업을 통해 코드에 노출 되 면 프로그램을 개발 하는 코드를 작성할 준비가 된 것입니다. 예를 들어 **Solution Pad**에서 편집을 위해 **MainWindow.cs** 파일을 두 번 클릭 하 여 엽니다.

[![MainWindow.cs 파일](xib-images/code01.png "MainWindow.cs 파일")](xib-images/code01-large.png#lightbox)

`MainWindow` 클래스에 다음 코드를 추가 하 여 위에서 만든 샘플 콘센트를 사용 합니다.

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

는 `NSLabel` Xcode에서 해당 콘센트를 C# 만들 때 Xcode에서 할당 한 직접 이름으로에 액세스 합니다 .이 경우에는가 호출 `ClickedLabel`됩니다. 일반 C# 클래스와 동일한 방식으로 노출 된 개체의 메서드나 속성에 액세스할 수 있습니다.

> [!IMPORTANT]
> 는 OS가 xib `AwakeFromNib`파일에서 사용자 인터페이스를 로드 하 `Initialize`고 인스턴스화한 `AwakeFromNib` _후_ 에 호출 되기 때문에와 같은 다른 메서드 대신를 사용 해야 합니다. Xib 파일이 완전히 로드 되어 인스턴스화되기 `NullReferenceException` 전에 레이블 컨트롤에 액세스 하려고 하면 레이블 컨트롤이 아직 만들어지지 않았기 때문에 오류가 발생 합니다.

그런 다음 `MainWindow` 클래스에 다음 partial 클래스를 추가 합니다.

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

이 코드는 Xcode 및 Interface Builder에서 만든 작업에 연결 하 고 사용자가 단추를 클릭할 때마다 호출 됩니다.

일부 UI 요소에는 기본 제공 작업 (예: **열기 ...** 메뉴 항목 (`openDocument:`))과 같은 기본 메뉴 모음의 항목이 자동으로 포함 됩니다. **Solution Pad**에서 **AppDelegate.cs** 파일을 두 번 클릭 하 여 편집을 위해 열고 다음 `DidFinishLaunching` 코드를 메서드 아래에 추가 합니다.

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

여기서 `[Export ("openDocument:")]`키 줄은 **AppDelegate** 에 게 `void OpenDialog (NSObject sender)` `openDocument:` 작업 `NSMenu` 에 응답 하는 메서드가 있음을 나타냅니다.

메뉴 사용에 대 한 자세한 내용은 [메뉴](~/mac/user-interface/menu.md) 설명서를 참조 하세요.

## <a name="synchronizing-changes-with-xcode"></a>Xcode와 변경 내용 동기화

Xcode에서 Mac용 Visual Studio로 다시 전환 하면 Xcode에서 변경한 내용이 자동으로 Xamarin.ios 프로젝트와 동기화 됩니다.

Solution Pad에서 **MainWindow.designer.cs** 을 선택 하는 경우 C# 코드에서 유출 및 동작이 어떻게 연결 되었는지 확인할 수 있습니다.

[![Xcode와 변경 내용 동기화](xib-images/sync01.png "Xcode와 변경 내용 동기화")](xib-images/sync01-large.png#lightbox)

**MainWindow.designer.cs** 파일의 두 정의는 다음과 같습니다.

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Xcode의 **mainwindow.xaml** 파일에 있는 정의를 사용 하 여 다음을 수행 합니다.

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

여기에서 볼 수 있듯이 Mac용 Visual Studio는 .h 파일의 변경 내용을 수신 대기 하 고 해당 변경 내용을 **designer.cs** 파일에 자동으로 동기화 하 여 응용 프로그램에 노출 합니다. 또한 **MainWindow.designer.cs** 는 partial 클래스 이므로 Mac용 Visual Studio는 클래스에 대 한 모든 변경 내용을 덮어쓰는 **MainWindow.cs** 를 수정할 필요가 없습니다.

일반적으로 **MainWindow.designer.cs** 을 직접 열 필요가 없습니다. 여기에는 교육용 으로만 제공 됩니다.

> [!IMPORTANT]
> 대부분의 경우 Mac용 Visual Studio는 Xcode에서 변경 된 내용을 자동으로 확인 하 고 Xamarin.ios 프로젝트와 동기화 합니다. 동기화가 자동으로 수행되지 않는 경우 Xcode로 돌아가서 다시 Mac용 Visual Studio로 돌아갑니다. 대부분 이렇게 하면 동기화 주기가 시작됩니다.

## <a name="adding-a-new-window-to-a-project"></a>프로젝트에 새 창 추가

기본 문서 창 외에도 Xamarin.ios 응용 프로그램은 기본 설정 또는 검사기 패널과 같은 다른 형식의 창을 사용자에 게 표시 해야 할 수 있습니다. 프로젝트에 새 창을 추가 하는 경우 xib 파일에서 창을 더 쉽게 로드 하는 프로세스를 수행 하므로 항상 **Controller 옵션과 함께 Cocoa 창을** 사용 해야 합니다.

새 창을 추가 하려면 다음을 수행 합니다.

1. **Solution Pad**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.
2. 새 파일 대화 상자에서 컨트롤러를 사용 하는 **xamarin.ios** > **cocoa 창**을 선택 합니다.

    ![새 창 컨트롤러 추가](xib-images/new01.png "새 창 컨트롤러 추가")
3. **이름**에 대해 `PreferencesWindow`를 입력하고 **새로 만들기** 단추를 클릭합니다.
4. **PreferencesWindow xib** 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다.

    [![Xcode에서 창 편집](xib-images/new02.png "Xcode에서 창 편집")](xib-images/new02-large.png#lightbox)
5. 인터페이스 디자인:

    [![Windows 레이아웃 디자인](xib-images/new03.png "Windows 레이아웃 디자인")](xib-images/new03-large.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

**AppDelegate.cs** 에 다음 코드를 추가 하 여 새 창을 표시 합니다.

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

줄 `var preferences = new PreferencesWindowController ();` 은 xib 파일에서 창을 로드 하 고 늘어납니다 하는 window 컨트롤러의 새 인스턴스를 만듭니다. 줄 `preferences.Window.MakeKeyAndOrderFront (this);` 에는 사용자에 게 새 창이 표시 됩니다.

**응용 프로그램 메뉴**에서 코드를 실행 하 고 **기본 설정 ...** 을 선택 하는 경우 창이 표시 됩니다.

![샘플 앱 실행](xib-images/new04.png "샘플 앱 실행")

Xamarin.ios 응용 프로그램에서 Windows를 사용 하는 방법에 대 한 자세한 내용은 [windows](~/mac/user-interface/window.md) 설명서를 참조 하세요.

## <a name="adding-a-new-view-to-a-project"></a>프로젝트에 새 뷰 추가

창 디자인을 보다 xib 파일의 여러 가지로 더 쉽게 분할 하는 경우가 있습니다. 예를 들어 **기본 설정 창** 에서 도구 모음 항목을 선택 하거나 **소스 목록** 선택에 대 한 응답으로 콘텐츠를 교환 하는 경우 주 창의 콘텐츠를 전환 하는 것과 같습니다.

프로젝트에 새 보기를 추가할 때는 항상 **Controller 옵션과 함께 Cocoa 뷰** 를 사용 해야 합니다. 이렇게 하면 xib 파일에서 뷰를 보다 쉽게 로드할 수 있습니다.

새 보기를 추가 하려면 다음을 수행 합니다.

1. **Solution Pad**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.
2. 새 파일 대화 상자에서 > 컨트롤러를**사용 하 여 xamarin.ios cocoa 보기**를 선택 합니다.

    ![새 뷰 추가](xib-images/view01.png "새 뷰 추가")
3. **이름**에 대해 `SubviewTable`를 입력하고 **새로 만들기** 단추를 클릭합니다.
4. **Xib** 파일을 두 번 클릭 하 여 Interface Builder 편집을 위해 열고 사용자 인터페이스를 디자인 합니다.

    [![Xcode에서 새 뷰 디자인](xib-images/view02.png "Xcode에서 새 뷰 디자인")](xib-images/view02-large.png#lightbox)
5. 필요한 작업 및 콘센트를 연결 합니다.
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

그런 다음 **SubviewTable.cs** 를 편집 하 고 **AwakeFromNib** 파일에 다음 코드를 추가 하 여 새 뷰가 로드 될 때이를 채웁니다.

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

현재 표시 되 고 있는 뷰를 추적 하기 위해 프로젝트에 열거형을 추가 합니다. 예: **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

뷰를 사용 하 고 표시 하는 창의 xib 파일을 편집 합니다. 코드에 의해 C# 메모리에 로드 된 후 보기의 컨테이너 역할을 하는 `ViewContainer` **사용자 지정 보기** 를 추가 하 고 라는 콘센트에 노출 합니다.

[![필요한 콘센트 만들기](xib-images/view03.png "필요한 콘센트 만들기")](xib-images/view03-large.png#lightbox)

변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

다음으로 새 보기를 표시 하는 창의 .cs 파일 (예: **MainWindow.cs**)을 편집 하 고 다음 코드를 추가 합니다.

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

창 컨테이너 (위에 추가 된 **사용자 지정 뷰** )의 xib 파일에서 로드 된 새 뷰를 표시 해야 하는 경우이 코드는 기존 뷰를 제거 하 고 새 뷰를 새 뷰로 교체 하는 기능을 처리 합니다. 표시 되는 뷰가 이미 있는 경우 화면에서 제거 하는 것을 볼 수 있습니다. 그런 다음 뷰 컨트롤러에서 로드 된 것으로 전달 된 보기를 사용 하 여 콘텐츠 영역에 맞게 크기를 조정 하 고 표시를 위해 콘텐츠에 추가 합니다.

새 뷰를 표시 하려면 다음 코드를 사용 합니다.

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

그러면 표시할 새 뷰에 대 한 뷰 컨트롤러의 새 인스턴스가 생성 되 고, 해당 형식이 프로젝트에 추가 된 열거형에 의해 지정 된 대로 설정 되며, 창 클래스에 추가 `DisplaySubview` 된 메서드를 사용 하 여 실제로 뷰가 표시 됩니다. 예:

[![샘플 앱 실행](xib-images/view04.png "샘플 앱 실행")](xib-images/view04-large.png#lightbox)

Xamarin.ios 응용 프로그램에서 Windows를 사용 하는 방법에 대 한 자세한 내용은 [windows](~/mac/user-interface/window.md) 및 [대화 상자](~/mac/user-interface/dialog.md) 설명서를 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 xib 파일을 사용 하는 방법에 대해 자세히 살펴봅니다. Xib 파일을 사용 하 여 응용 프로그램의 사용자 인터페이스를 만드는 방법, Xcode의 Interface Builder xib 파일을 만들고 유지 관리 하는 방법 및 코드에서 C# xib 파일을 사용 하는 방법에 대해 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacImages(샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [메뉴](~/mac/user-interface/menu.md)
- [대화 상자](~/mac/user-interface/dialog.md)
- [이미지 작업](~/mac/app-fundamentals/image.md)
- [macOS 휴먼 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
