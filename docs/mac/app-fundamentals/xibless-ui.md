---
title: xamarin.mac에서.storyboard/.xib-less 사용자 인터페이스 디자인
description: 이 문서에서는 Xamarin.Mac 응용 프로그램의 사용자 인터페이스를 직접 만들고 C# Interface Builder.storyboard 파일,.xib 파일 없이 코드입니다.
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: e41f19c1a2d02537f300ae82b7f3d45bc6571e1b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112470"
---
# <a name="storyboardxib-less-user-interface-design-in-xamarinmac"></a>xamarin.mac에서.storyboard/.xib-less 사용자 인터페이스 디자인

_이 문서에서는 Xamarin.Mac 응용 프로그램의 사용자 인터페이스를 직접 만들고 C# Interface Builder.storyboard 파일,.xib 파일 없이 코드입니다._

## <a name="overview"></a>개요

동일한 사용자 인터페이스 요소에 대 한 액세스 권한이 Xamarin.Mac 응용 프로그램에서 C# 및.NET으로 작업을 하는 경우 및 작업 하는 개발자 도구를 *Objective-C* 및 *Xcode* 않습니다. 일반적으로 Xamarin.Mac 응용 프로그램을 만들 때.storyboard 또는.xib 파일을 사용 하 여 Xcode의 Interface Builder 사용 하 여 만들고 응용 프로그램의 사용자 인터페이스 유지 관리 됩니다.

Xamarin.Mac 응용 프로그램의 UI의 일부 또는 전부를 직접 만들 수 있습니다 C# 코드입니다. 이 문서에서는 사용자 인터페이스를 만들고 UI 요소에는 기본적인 다룰 것 C# 코드입니다.

[![코드 편집기 Mac 용 Visual Studio](xibless-ui-images/intro01.png "Mac 코드 편집기에 대 한 Visual Studio")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>코드를 사용 하는 창을 전환

새 Xamarin.Mac Cocoa 응용 프로그램을 만들면 기본적으로 표준 비어 창을 가져옵니다. 이 windows에 정의 되어는 **Main.storyboard** (또는 일반적으로 **MainWindow.xib**) 프로젝트에 자동으로 포함 된 파일입니다. 또한이 **ViewController.cs** 앱의 기본 보기를 관리 하는 파일 (또는 일반적으로 다시를 **MainWindow.cs** 및 **MainWindowController.cs** 파일).

응용 프로그램에 대 한 Xibless 창으로 전환 하려면 다음을 수행 합니다.

1. 사용 중지 하려는 응용 프로그램을 엽니다 `.stroyboard` 또는 mac 용 Visual Studio에서 사용자 인터페이스를 정의 하려면.xib 파일
2. 에 **Solution Pad**를 마우스 오른쪽 단추로 클릭 합니다 **Main.storyboard** 또는 **MainWindow.xib** 파일을 선택 **제거**: 

    ![주 스토리 보드 또는 창 제거](xibless-ui-images/switch01.png "주 스토리 보드 또는 창 제거")
3. **제거 대화**를 클릭 합니다 **삭제** 프로젝트에서.storyboard 또는.xib을 완전히 제거 하려면 단추: 

    ![삭제 확인](xibless-ui-images/switch02.png "삭제 확인")

수정 해야 하는 이제 합니다 **MainWindow.cs** 창의 레이아웃을 정의 하 고 수정 하는 파일을 **ViewController.cs** 또는 **MainWindowController.cs** 파일 만들기를 인스턴스는 `MainWindow` .storyboard 또는.xib 파일을 더 이상 사용 하 고 있으므로 클래스.

사용자 인터페이스는 자동으로 포함 되지 않을 수에 대 한 스토리 보드를 사용 하는 최신 Xamarin.Mac 앱의 **MainWindow.cs**하십시오 **ViewController.cs** 또는 **MainWindowController.cs** 파일입니다. 필요에 따라 추가 비어 있는 새 C# 클래스를 프로젝트 (**추가** > **새 파일...**   >  **일반** > **빈 클래스**) 고 누락 된 파일과 같은 이름을 지정 합니다. 


### <a name="defining-the-window-in-code"></a>코드에서 창 정의

다음에 편집 합니다 **MainWindow.cs** 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using System;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindow : NSWindow
    {
        #region Private Variables
        private int NumberOfTimesClicked = 0;
        #endregion

        #region Computed Properties
        public NSButton ClickMeButton { get; set;}
        public NSTextField ClickMeLabel { get ; set;}
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }

        public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
            // Define the user interface of the window here
            Title = "Window From Code";

            // Create the content view for the window and make it fill the window
            ContentView = new NSView (Frame);

            // Add UI elements to window
            ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
                AutoresizingMask = NSViewResizingMask.MinYMargin
            };
            ContentView.AddSubview (ClickMeButton);

            ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
                BackgroundColor = NSColor.Clear,
                TextColor = NSColor.Black,
                Editable = false,
                Bezeled = false,
                AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
                StringValue = "Button has not been clicked yet."
            };
            ContentView.AddSubview (ClickMeLabel);
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Wireup events
            ClickMeButton.Activated += (sender, e) => {
                // Update count
                ClickMeLabel.StringValue = (++NumberOfTimesClicked == 1) ? "Button clicked one time." : string.Format("Button clicked {0} times.",NumberOfTimesClicked);
            };
        }
        #endregion

    }
}
```

핵심 요소 중 일부를 살펴보겠습니다.

먼저 몇 가지를 추가 했습니다 _계산 속성_ (처럼 창.storyboard 또는.xib 파일에서 만든) 출 선을 처럼 작동 하는:

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

이러한 미국에 액세스할 창에 표시 하려고 하는 UI 요소입니다. 인스턴스화해야 하는 방법이 필요 창.storyboard 또는.xib 파일에서 확장 됩니다 없습니다, 이후 (나중에 볼 수 있겠지만 `MainWindowController` 클래스). 이 새 생성자 메서드가 수행 하는 것이입니다.

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

이 여기서 창의 레이아웃을 디자인 하 고 필요한 사용자 인터페이스를 만드는 데 필요한 모든 UI 요소를 배치 합니다. 필요한 UI 요소 창에 추가할 수 있습니다, 전에 _콘텐츠 뷰_ 요소를 포함 하려면:

```csharp
ContentView = new NSView (Frame);
```

이 창을 채울 콘텐츠 뷰를 만듭니다. 이 첫 번째 UI 요소를 추가 하는 이제는 `NSButton`, 창:

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

여기에 가장 먼저는 iOS와 달리 macOS를 사용 하 여 수학 표기법 해당 창의 좌표 시스템 정의 됩니다. 따라서 원점을 값 오른쪽 창의 오른쪽 위 모서리 쪽으로 증가 하는 창의 왼쪽 위 모서리에 있습니다. 새를 만들 때 `NSButton`,에서는 고려해 야 화면의 해당 위치와 크기를 정의 했습니다.

`AutoresizingMask = NSViewResizingMask.MinYMargin` 속성 단추를 지시 한다고 창의 세로 크기 조정 시 창의 위쪽에서 같은 위치에 유지 되도록 합니다. 마찬가지로 때문에 이것이 필요 (0, 0) 창의 왼쪽 아래에 있습니다.

마지막으로 `ContentView.AddSubview (ClickMeButton)` 메서드를 추가 합니다 `NSButton` 콘텐츠 뷰에 응용 프로그램은 실행 및 표시 창 화면에 표시 됩니다 있도록.

마지막 레이블을 횟수를 표시 하는 창에 추가 하는 `NSButton` 했: 

```csharp
ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
    BackgroundColor = NSColor.Clear,
    TextColor = NSColor.Black,
    Editable = false,
    Bezeled = false,
    AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
    StringValue = "Button has not been clicked yet."
};
ContentView.AddSubview (ClickMeLabel);
``` 

MacOS 특정 없기 때문 _레이블을_ UI 요소를 특수 하 게 스타일을 추가 했습니다 편집할 수 없는 `NSTextField` 레이블 역할을 합니다. (0, 0)는 창의 왼쪽 아래에서 이전에 계정에 크기와 위치는 단추 처럼 합니다. `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin` 속성을 사용 하는 합니다 **또는** 연산자를 결합 하는 두 개의 `NSViewResizingMask` 기능. 창 크기를 조정할 때 세로 방향으로 창의 위쪽에서 같은 위치에 유지 하 고 축소는 창의 가로 크기를 조정할 너비에서 증가 레이블을 확인 합니다.

다시는 `ContentView.AddSubview (ClickMeLabel)` 메서드를 추가 합니다 `NSTextField` 콘텐츠 뷰에 응용 프로그램이 실행 되 고 창을 열 때 화면에 표시 됩니다 있도록 합니다.


### <a name="adjusting-the-window-controller"></a>창 컨트롤러를 조정합니다.

디자인 이후에 `MainWindow` 로드 되 고 더 이상 창 컨트롤러 약간 조정해 해야.storyboard 또는.xib 파일에서 합니다. 편집 된 **MainWindowController.cs** 파일을 다음과 같이 표시 되도록 합니다.

```csharp
using System;

using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindowController : NSWindowController
    {
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindowController (NSCoder coder) : base (coder)
        {
        }

        public MainWindowController () : base ("MainWindow")
        {
            // Construct the window from code here
            CGRect contentRect = new CGRect (0, 0, 1000, 500);
            base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);

            // Simulate Awaking from Nib
            Window.AwakeFromNib ();
        }

        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }

        public new MainWindow Window {
            get { return (MainWindow)base.Window; }
        }
    }
}

```

이 수정 작업의 주요 요소에 설명 하는 사용 수 있습니다.

첫째, 정의의 새 인스턴스를 `MainWindow` 클래스 및 기본 창 컨트롤러의 할당할 `Window` 속성:

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

사용 하 여 화면의 창의 위치를 정의 하는 것을 `CGRect`입니다. 창의 좌표 시스템 처럼 화면 왼쪽 아래 모서리를 (0, 0)을 정의합니다. 사용 하 여 창의 스타일 정의 다음에 **또는** 연산자를 결합 하는 두 개 이상의 `NSWindowStyle` 기능:

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
``` 

다음 `NSWindowStyle` 기능은 사용할 수 있습니다.

- **여백 없는** -창의 테두리가 해야 합니다.
- **이라는** -창의 제목 표시줄을 갖습니다.
- **Closable** -창 닫기 단추가 및 닫을 수 있습니다.
- **Miniaturizable** -Miniaturize 단추가 창과 최소화할 수 있습니다.
- **크기 조정 가능한** -창 크기 조정 단추 되며 크기를 조정할 수 있습니다.
- **유틸리티** -는 유틸리티 스타일 창 (제어판)입니다.
- **DocModal** -시스템 모달 대신 문서 모달 됩니다 창 패널 인 경우. 
- **NonactivatingPanel** -창 패널 인 경우이 걸 수 없습니다 주 창입니다.
- **TexturedBackground** -창 질감이 배경이 됩니다.
- **실제 크기** -창이 확장 되지 것입니다.
- **UnifiedTitleAndToolbar** -창의 제목 및 도구 모음 영역에 참여할 수 있습니다.
- **Hud** -창으로 헤드업 디스플레이 패널 표시 됩니다.
- **FullScreenWindow** -창을 전체 화면 모드를 입력할 수 있습니다.
- **FullSizeContentView** -창의 콘텐츠 뷰는 뒤에 제목 및 도구 모음 영역입니다.

마지막 두 속성은 정의 _버퍼링 형식_ 창의 경우 창의 그리기 지연 됩니다. 에 대 한 자세한 `NSWindows`, Apple의를 참조 하세요 [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) 설명서.

마지막으로, 있으므로 창.storyboard 또는.xib 파일에서 확장 됩니다 하지, 해야 시뮬레이션에서 우리의 **MainWindowController.cs** windows를 호출 하 여 `AwakeFromNib` 메서드:

```csharp
Window.AwakeFromNib ();
```

그러면.storyboard 또는.xib 파일에서 로드 된 표준 창 같은 창에 대 한 코드에 있습니다.


### <a name="displaying-the-window"></a>창 표시

.Storyboard 또는.xib 파일을 제거 하며 **MainWindow.cs** 하 고 **MainWindowController.cs** 파일 수정 사용할 창에서 만들어진 모든 표준 창 처럼 Xcode의 Interface Builder.xib 파일을 사용 하 여 구성 합니다.

다음 창 및 해당 컨트롤러의 새 인스턴스를 만들고 화면에 창을 표시 합니다.

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

이 시점에서 응용 프로그램을 실행 하 고 단추를 몇 번 클릭 하는 경우 다음 표시 됩니다.

![예제 앱 실행](xibless-ui-images/run01.png "실행 하는 예제 앱")


## <a name="adding-a-code-only-window"></a>코드 전용 창 추가

기존 Xamarin.Mac 응용 프로그램 코드에만 해당, xibless 창 추가 하고자 하는 경우에 프로젝트를 클릭 합니다 **Solution Pad** 선택한 **추가** > **새 파일...** . 에 **새 파일** 대화 상자 선택 **Xamarin.Mac** > **컨트롤러를 사용 하 여 Cocoa 창을**아래 그림과 같이:

![새 창 컨트롤러 추가](xibless-ui-images/add01.png "새 창 컨트롤러 추가") 

마찬가지로 다음 이전에 기본.storyboard 또는.xib 파일을 프로젝트에서 삭제 됩니다 (이 예제의 **SecondWindow.xib**)의 단계를 수행 하 고는 [코드를 사용 하는 창을 전환](#Switching_a_Window_to_use_Code) 를 처리 하기 위해 위의 섹션을 코드 정의 창입니다.


## <a name="adding-a-ui-element-to-a-window-in-code"></a>코드에서 창에는 UI 요소 추가

창의 코드에서 생성 되었거나.storyboard 또는.xib 파일에서 로드를 여부를 경우가 있을 창에 코드에서 UI 요소를 추가 하려고 합니다. 예를 들어:

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

위의 코드에서는 새 `NSButton` 에 추가 된 `MyWindow` 표시할 창 인스턴스. 기본적으로.storyboard 또는.xib 파일에서 Xcode의 Interface Builder에서 정의 될 수 있는 UI 요소의 코드에서 만든 고 창에 표시 될 수 있습니다.


## <a name="defining-the-menu-bar-in-code"></a>코드에서 메뉴를 정의합니다.

Xamarin.Mac의 현재 제한으로 인해이 제안 되지 않습니다 사용자가 만든 Xamarin.Mac 응용 프로그램의 메뉴 모음 –`NSMenuBar`–에서 코드 없지만 계속 사용 합니다 **Main.storyboard** 또는 **MainMenu.xib** 파일을 정의 합니다. 즉, 추가 및 메뉴 및 메뉴 항목을 제거할 수 있습니다 C# 코드입니다.

예를 들어 편집 합니다 **AppDelegate.cs** 파일을 확인 합니다 `DidFinishLaunching` 다음과 같은 메서드 확인:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    // Create a Status Bar Menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Phrases";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Phrases");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        Console.WriteLine("Address Selected");
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        Console.WriteLine("Date Selected");
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        Console.WriteLine("Greetings Selected");
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        Console.WriteLine("Signature Selected");
    };
    item.Menu.AddItem (signature);
}
```

위의 상태 표시줄 메뉴에서 코드를 만들고 응용 프로그램을 시작할 때 표시 합니다. 작업 메뉴에 대 한 자세한 내용은 참조 하십시오 우리의 [메뉴](~/mac/user-interface/menu.md) 설명서.


## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 응용 프로그램의 사용자 인터페이스에서 만들기를 자세히 살펴보려면 되었고 C# Xcode의 Interface Builder를 사용 하 여.storyboard 또는.xib 파일 대신 코드입니다.



## <a name="related-links"></a>관련 링크

- [MacXibless (샘플)](https://developer.xamarin.com/samples/mac/MacXibless/)
- [Windows](~/mac/user-interface/window.md)
- [메뉴](~/mac/user-interface/menu.md)
- [macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Windows 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
