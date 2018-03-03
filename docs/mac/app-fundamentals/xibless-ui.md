---
title: ".storyboard/.xib-less 사용자 인터페이스 디자인"
description: "이 문서에서는.storyboard 파일,.xib 파일 또는 인터페이스 작성기 없이 C# 코드에서 직접 Xamarin.Mac 응용 프로그램의 사용자 인터페이스를 생성 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 544aad278b9bc66120e188eec54fa68be71dc625
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="storyboardxib-less-user-interface-design"></a>.storyboard/.xib-less 사용자 인터페이스 디자인

_이 문서에서는.storyboard 파일,.xib 파일 또는 인터페이스 작성기 없이 C# 코드에서 직접 Xamarin.Mac 응용 프로그램의 사용자 인터페이스를 생성 합니다._


## <a name="overview"></a>개요

동일한 사용자 인터페이스 요소에 대 한 액세스 권한이 Xamarin.Mac 응용 프로그램에서 C# 및.NET으로 작업을 하는 경우 및 작업 하는 개발자 도구를 *Objective-c* 및 *Xcode* 않습니다. 일반적으로 Xamarin.Mac 응용 프로그램을 만들 때 사용 합니다 Xcode의 인터페이스 작성기.storyboard 또는.xib 파일을 만들고 응용 프로그램의 사용자 인터페이스를 유지.

C# 코드에서 직접 Xamarin.Mac 응용 프로그램의 UI의 일부 또는 전부를 만들을 수도 있습니다. 이 문서에서는 C# 코드에서 사용자 인터페이스와 UI 요소를 만드는 기본적인 하겠습니다.

[![Mac 코드 편집기에 대 한 Visual Studio](xibless-ui-images/intro01.png "Mac 코드 편집기에 대 한 Visual Studio")](xibless-ui-images/intro01-large.png)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>코드를 사용 하는 창을 전환

새 Xamarin.Mac Cocoa 응용 프로그램을 만들 때 기본적으로 표준, 빈 창이 얻을 수 있습니다. 에 정의 되어 있는이 windows는 **Main.storyboard** (또는 일반적으로 **MainWindow.xib**) 파일을 프로젝트에 자동으로 포함 합니다. 이 포함 됩니다는 **ViewController.cs** 응용 프로그램의 기본 보기를 관리 하는 파일 (또는 다시 일반적으로 **MainWindow.cs** 및 **MainWindowController.cs** 파일).

응용 프로그램에 대 한 Xibless 창으로 전환 하려면 다음을 수행 합니다.

1. 응용 프로그램 사용을 중지 하려는 열고 `.stroyboard` 또는.xib Mac.에 대 한 Visual Studio에서 사용자 인터페이스를 정의 하는 파일
2. 에 **솔루션 패드**를 마우스 오른쪽 단추로 클릭는 **Main.storyboard** 또는 **MainWindow.xib** 파일을 선택 **제거**: 

    ![주 스토리 보드 또는 창 제거](xibless-ui-images/switch01.png "주 스토리 보드 또는 창 제거")
3. **제거 대화**, 클릭는 **삭제** 단추.storyboard 또는.xib 프로젝트에서 완전히 제거 하려면: 

    ![삭제 확인](xibless-ui-images/switch02.png "삭제 확인")

수정 해야 하는 이제는 **MainWindow.cs** 창의 레이아웃을 정의 하 고 수정 하는 파일의 **ViewController.cs** 또는 **MainWindowController.cs** 파일을 만듭니다는 인스턴스는 `MainWindow` .storyboard 또는.xib 파일이 더 이상 사용 하 고 있으므로 클래스.

사용자 인터페이스 자동으로 포함 되지 않을 수에 대 한 스토리 보드를 사용 하는 최신 Xamarin.Mac 앱은 **MainWindow.cs**, **ViewController.cs** 또는 **MainWindowController.cs** 파일입니다. 필요에 따라 새 빈 C# 클래스에 추가 프로젝트 (**추가** > **새 파일...**   >  **일반** > **빈 클래스**) 하 고 누락 된 파일과 동일 이름을 합니다. 


### <a name="defining-the-window-in-code"></a>코드에서 시간을 정의

다음에 편집는 **MainWindow.cs** 파일을 다음과 같이 되도록 합니다.

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

몇 가지 주요 요소에 설명 하겠습니다.

첫째, 몇 가지 추가 되었습니다 _계산 속성_ 입니다 (처럼 창에서에서 만들어진 경우.storyboard 또는.xib 파일) 콘센트 처럼 작동 합니다.

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

이에 액세스할 수 우리 UI 요소는 창에 표시할 하겠습니다. 인스턴스화해야 하는 방법이 필요 창.storyboard 또는.xib 파일에서 확장 됩니다 하지, 이후 (나중에 볼 수 있겠지만 `MainWindowController` 클래스). 즉,이 새로운 생성자 방법을 수행 하는 작업:

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

이것이, 창 레이아웃을 디자인 하 고 필요한 사용자 인터페이스를 만드는 데 필요한 모든 UI 요소를 배치 합니다 위치입니다. UI 요소는 창에 추가할 수 있습니다, 전에 필요한는 _콘텐츠 뷰_ 는 요소를 포함 합니다.

```csharp
ContentView = new NSView (Frame);
```

이 창을 채울 콘텐츠 뷰를 만듭니다. 이 첫 번째 UI 요소를 추가 하는 이제는 `NSButton`, 창에:

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

여기에 첫 번째 사항은, iOS, 달리 macOS 수학 표기법 사용 하 여 정의 해당 창 좌표계입니다. 따라서 원점은 오른쪽 창의 오른쪽 위 모서리 쪽으로 값의 증가 따라 창의 왼쪽 아래에는 합니다. 새 만들 때 `NSButton`, 우리 고려해이 화면에서의 위치와 크기를 정의 했습니다.

`AutoresizingMask = NSViewResizingMask.MinYMargin` 속성이 있으면 단추 창의 세로 크기를 조정 하면 창 위쪽에서 같은 위치에 유지 하도록 하겠습니다. 다시, 때문에 이것이 필요 (0, 0)는 창의 왼쪽 아래에 있습니다.

마지막으로 `ContentView.AddSubview (ClickMeButton)` 메서드 추가 `NSButton` 콘텐츠 보기 응용 프로그램을 실행 및 표시 창을 한다는 화면에 표시 됩니다.

그런 다음 레이블을 횟수를 표시 하는 창에 추가 됩니다 하는 `NSButton` 클릭 했습니다. 

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

MacOS 특정 한 _레이블_ UI 요소를 특수 스타일을 추가 했습니다 편집할 수 없는 `NSTextField` 는 레이블 역할을 합니다. 해당 (0, 0)은 창의 왼쪽 아래에는 크기와 위치를 고려 하기 전에 단추 처럼 합니다. `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin` 속성을 사용 하 여는 **또는** 결합 두 연산자를 `NSViewResizingMask` 기능입니다. 이렇게 하면 레이블 창을 세로로 크기를 조정 하면 창 위쪽에서 같은 위치에 유지 해 서 축소 하 고는 창의 가로 크기 조정에 따라 너비로 확장 합니다.

다시,는 `ContentView.AddSubview (ClickMeLabel)` 메서드 추가 `NSTextField` 콘텐츠 보기 응용 프로그램을 실행 하 고 창을 열 때 해당 it에 화면에 표시 됩니다.


### <a name="adjusting-the-window-controller"></a>창 컨트롤러를 조정합니다.

디자인 이후는 `MainWindow` 로드 되 고 더 이상.storyboard 또는.xib 파일에서 창 컨트롤러에 대 한 몇 가지 조정이 수행 해야 합니다. 편집 된 **MainWindowController.cs** 파일을 다음과 같이 표시 하 게 합니다.

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

사용이 수정 작업의 주요 요소에 설명 합니다.

새 인스턴스를 먼저 정의 `MainWindow` 클래스를 기본 창 컨트롤러에 할당 `Window` 속성:

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

화면에 창 위치 정의 `CGRect`합니다. 창의 좌표계를 마찬가지로 화면 하단 왼쪽 모서리 쪽으로 (0, 0)을 정의합니다. 다음으로 창 스타일을 사용 하 여 정의 된 **또는** 을 둘 이상 결합 `NSWindowStyle` 기능:

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
``` 

다음 `NSWindowStyle` 기능을 사용할 수 있습니다.

- **여백 없이** -창이 없는 테두리가 없습니다.
- **라는** -창 제목 표시줄을 갖습니다.
- **Closable** -창에 닫기 단추를 닫을 수 있습니다.
- **Miniaturizable** -창에 Miniaturize 단추 고를 최소화할 수 있습니다.
- **크기 조정 가능한** -창 크기를 조정 하는 단추를 크기 조정할 수 있습니다.
- **유틸리티** -유틸리티 스타일 창 (패널)입니다.
- **DocModal** -시스템 모달 대신 문서 모달 됩니다 패널 창의 사용 하는 경우. 
- **NonactivatingPanel** -창을 패널 경우 것 걸 수 없습니다 주 창.
- **TexturedBackground** -창이 질감 배경이 됩니다.
- **소수 자릿수가 없는** -창 크기가 조정 되지 됩니다.
- **UnifiedTitleAndToolbar** -창의 제목 및 도구 모음 영역에 참여할 수 있습니다.
- **Hud** -창이 패널에는 화면 표시로 나타납니다.
- **FullScreenWindow** -창을 전체 화면 모드를 입력할 수 있습니다.
- **FullSizeContentView** -창의 콘텐츠 보기 제목 및 도구 모음 영역 보다 낮습니다.

마지막 두 개의 속성 정의 _버퍼링 형식_ 창에 대 한 경우 창의 그리기 지연 됩니다. 대 한 자세한 내용은 `NSWindows`, Apple의를 참조 하십시오 [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) 설명서입니다.

마지막으로, 창.storyboard 또는.xib 파일에서 확장 됩니다 하지, 원하므로 해야에서 시뮬레이션 우리의 **MainWindowController.cs** windows를 호출 하 여 `AwakeFromNib` 메서드:

```csharp
Window.AwakeFromNib ();
```

따라서 정당한 창에 대 한 코드를.storyboard 또는.xib 파일에서 로드 하는 표준 창 원할 수 있습니다.


### <a name="displaying-the-window"></a>창 표시

.Storyboard 또는.xib 파일 제거 및 **MainWindow.cs** 및 **MainWindowController.cs** 수정, 파일 사용 하고자 창에서 만들어진 모든 표준 창 것 처럼 Xcode의.xib 파일과 함께 인터페이스 작성기입니다.

됩니다 창과 해당 컨트롤러의 새 인스턴스를 만들고 आ स ा 화면에 창을 표시 합니다.

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

이 시점에서 응용 프로그램을 실행 하는 경우 단추가 클릭 몇 번 다음 표시 됩니다.

![실행 하는 예제 앱](xibless-ui-images/run01.png "실행 하는 예제 응용 프로그램")


## <a name="adding-a-code-only-window"></a>코드 유일한 창 추가

프로젝트 오른쪽 단추로 클릭 하면 기존 Xamarin.Mac 응용 프로그램 코드에만, xibless 창 추가 하고자 하는 경우는 **솔루션 패드** 선택 **추가** > **새 파일...** . 에 **새 파일** 대화 선택 **Xamarin.Mac** > **컨트롤러를 사용 하 여 Cocoa 창을**아래 그림과 같이,:

![새 창 컨트롤러 추가](xibless-ui-images/add01.png "새 창 컨트롤러 추가") 

나중에 삭제 되는 기본.storyboard 또는.xib 파일 프로젝트에서 이전과 마찬가지로 (이 경우 **SecondWindow.xib**)의 단계를 수행 하 고는 [코드를 사용 하는 창을 전환](#Switching_a_Window_to_use_Code) 를 포함 하는 위의 섹션에서 창 정의 코드입니다.


## <a name="adding-a-ui-element-to-a-window-in-code"></a>코드의 창으로 UI 요소를 추가합니다.

코드 창을 만들어지거나.storyboard 또는.xib 파일에서 로드, 여부를 경우가 있습니다 코드에서 UI 요소는 창에 추가 하고자 합니다. 예:

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

위의 코드에서는 새 `NSButton` 에 추가 된 `MyWindow` 표시 하기 위한 창 인스턴스. 기본적으로 Xcode의 인터페이스 작성기.storyboard 또는.xib 파일에 정의 되어 있을 수 있는 UI 요소 만들고 사용할 수 있는 코드에서 창에 표시 합니다.


## <a name="defining-the-menu-bar-in-code"></a>코드에서 메뉴 모음을 정의합니다.

Xamarin.Mac의 현재 제한으로 인해 하지 좋습니다 만드는 Xamarin.Mac 응용 프로그램의 메뉴 모음 –`NSMenuBar`–에 계속 사용 하지만 코드는 **Main.storyboard** 또는 **MainMenu.xib** 파일을 정의 합니다. 즉, 추가 하 고 C# 코드에서 메뉴 및 메뉴 항목을 제거할 수 있습니다.

예를 들어 편집는 **AppDelegate.cs** 파일을 확인는 `DidFinishLaunching` 다음과 같은 메서드 보기:

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

위의 상태 표시줄 메뉴에서 코드를 만들고 응용 프로그램을 시작할 때 표시 됩니다. 작업 메뉴에 대 한 자세한 내용은 참조 하십시오 우리의 [메뉴](~/mac/user-interface/menu.md) 설명서입니다.


## <a name="summary"></a>요약

이 문서에 자세히 살펴보고 Xcode의 인터페이스 작성기를 사용 하 여.storyboard 또는.xib 파일와 달리 C# 코드에서 Xamarin.Mac 응용 프로그램의 사용자 인터페이스를 만드는 수행 했습니다.



## <a name="related-links"></a>관련 링크

- [MacXibless (샘플)](https://developer.xamarin.com/samples/mac/MacXibless/)
- [Windows](~/mac/user-interface/window.md)
- [메뉴](~/mac/user-interface/menu.md)
- [Human Interface Guidelines macOS](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Windows 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
