---
title: . xib-Xamarin.ios의 less 사용자 인터페이스 디자인
description: 이 문서에서는 storyboard 파일,. xib 파일 또는 Interface Builder 없이 코드에서 C# 직접 xamarin.ios 응용 프로그램의 사용자 인터페이스를 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: bcc176f8d3eb97751e6957039c2a14ed02aad653
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "70770153"
---
# <a name="storyboardxib-less-user-interface-design-in-xamarinmac"></a>. xib-Xamarin.ios의 less 사용자 인터페이스 디자인

_이 문서에서는 storyboard 파일,. xib 파일 또는 Interface Builder 없이 코드에서 C# 직접 xamarin.ios 응용 프로그램의 사용자 인터페이스를 만드는 방법을 설명 합니다._

## <a name="overview"></a>개요

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우 *목표-C* 및 *Xcode* 에서 작업 하는 개발자가 사용 하는 것과 동일한 사용자 인터페이스 요소 및 도구에 액세스할 수 있습니다. 일반적으로 Xamarin.ios 응용 프로그램을 만들 때 storyboard 또는. xib 파일과 함께 Xcode의 Interface Builder 사용 하 여 응용 프로그램의 사용자 인터페이스를 만들고 유지 관리 합니다.

또한 코드에서 C# 직접 xamarin.ios 응용 프로그램 UI의 일부 또는 전부를 만들 수 있습니다. 이 문서에서는 코드에서 C# 사용자 인터페이스 및 UI 요소를 만드는 기본 사항을 설명 합니다.

[![Mac용 Visual Studio 코드 편집기](xibless-ui-images/intro01.png "Mac용 Visual Studio 코드 편집기")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>코드를 사용 하도록 창 전환

새 Xamarin.ios Cocoa 응용 프로그램을 만들면 기본적으로 표준 빈 창이 표시 됩니다. 이 창은 프로젝트에 자동으로 포함 되는 **기본 storyboard** (또는 일반적으로 xib) 파일에 정의 되어 있습니다 **mainwindow.xaml.** 여기에는 앱의 기본 보기 (또는 일반적으로 **MainWindow.cs** 및 **MainWindowController.cs** 파일)를 관리 하는 **ViewController.cs** 파일도 포함 됩니다.

응용 프로그램에 대 한 Xibless 창으로 전환 하려면 다음을 수행 합니다.

1. @No__t_0 또는. xib 파일을 사용 하 여 중지 하려는 응용 프로그램을 열고 Mac용 Visual Studio 사용자 인터페이스를 정의 합니다.
2. **Solution Pad**에서 **mainwindow.xaml 또는 xib** **파일을 마우스** 오른쪽 단추로 클릭 하 고 **제거**를 선택 합니다.

    ![주 storyboard 또는 창 제거](xibless-ui-images/switch01.png "주 storyboard 또는 창 제거")
3. **제거 대화 상자**에서 **삭제** 단추를 클릭 하 여 프로젝트에서 storyboard 또는. xib을 완전히 제거 합니다.

    ![삭제 확인](xibless-ui-images/switch02.png "삭제 확인")

이제 **MainWindow.cs** 파일을 수정 하 여 창 레이아웃을 정의 하 고 **ViewController.cs** 또는 **MainWindowController.cs** 파일을 수정 하 여 더 이상. storyboard를 사용 하지 않으므로 `MainWindow` 클래스의 인스턴스를 만들도록 해야 합니다. . xib 파일입니다.

사용자 인터페이스에 대해 Storyboard를 사용 하는 최신 Xamarin.ios 앱은 **MainWindow.cs**, **ViewController.cs** 또는 **MainWindowController.cs** 파일을 자동으로 포함 하지 않을 수 있습니다. 필요 C# 에 따라 프로젝트에 비어 있는 새 클래스를 추가 하 고 ( > **일반** **파일**  > **빈 클래스**)  >  하 고, 누락 된 파일과 동일 하 게 이름을**추가** 하면 됩니다.

### <a name="defining-the-window-in-code"></a>코드에서 창 정의

그런 다음 **MainWindow.cs** 파일을 편집 하 여 다음과 같이 만듭니다.

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

몇 가지 주요 요소에 대해 설명 하겠습니다.

먼저, storyboard 또는 xib 파일에서 창이 생성 된 것 처럼, 다음과 같이 콘센트 처럼 동작 하는 몇 가지 _계산 된 속성_ 을 추가 했습니다.

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

이를 통해 창에 표시 되는 UI 요소에 액세스할 수 있습니다. 이 창은 storyboard 또는. xib 파일에서 팽창 되지 않으므로이를 인스턴스화하는 방법이 필요 합니다. (나중에 `MainWindowController` 클래스에서 볼 수 있습니다.) 이 새로운 생성자 메서드는 다음과 같습니다.

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

여기서는 창의 레이아웃을 디자인 하 고 필요한 사용자 인터페이스를 만드는 데 필요한 모든 UI 요소를 배치 합니다. 모든 UI 요소를 창에 추가 하려면 다음 요소를 포함 하는 _콘텐츠 뷰가_ 필요 합니다.

```csharp
ContentView = new NSView (Frame);
```

그러면 창을 채울 콘텐츠 뷰가 생성 됩니다. 이제 첫 번째 UI 요소인 `NSButton`를 창에 추가 합니다.

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

여기에서 가장 먼저 기억해 야 할 사항은 iOS와 달리 macOS는 수학 표기법을 사용 하 여 해당 창 좌표계를 정의 한다는 것입니다. 따라서 원점은 창의 왼쪽 아래 모서리에 있으며 값은 창의 오른쪽 위 모퉁이의 오른쪽 위 모퉁이에 있습니다. 새 `NSButton`을 만들 때 화면에서의 위치와 크기를 정의할 때이를 고려 합니다.

@No__t_0 속성은 창의 세로 크기를 조정할 때 창 맨 위에서와 동일한 위치에 유지 하려는 단추를 알려 줍니다. 이는 (0, 0)이 창의 왼쪽 아래에 있기 때문에 필요 합니다.

마지막으로, `ContentView.AddSubview (ClickMeButton)` 메서드는 응용 프로그램이 실행 되 고 창이 표시 될 때 화면에 표시 되도록 콘텐츠 뷰에 `NSButton`를 추가 합니다.

다음으로 `NSButton` 클릭 한 횟수를 표시 하는 레이블이 창에 추가 됩니다.

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

MacOS는 특정 _레이블_ UI 요소를 포함 하지 않으므로 레이블 역할을 할 편집할 수 있는 스타일 없는 `NSTextField`을 추가 했습니다. 이전 단추와 마찬가지로 크기 및 위치는 (0, 0)가 창의 왼쪽 아래에 있는 것을 고려 합니다. @No__t_0 속성은 **or** 연산자를 사용 하 여 두 개의 `NSViewResizingMask` 기능을 결합 합니다. 그러면 창의 가로 크기를 조정 하 고 창의 크기를 가로로 조정 하는 경우 창의 위쪽에서 같은 위치에 레이블을 유지 하 게 됩니다.

또한 `ContentView.AddSubview (ClickMeLabel)` 메서드는 응용 프로그램이 실행 되 고 창이 열릴 때 화면에 표시 될 수 있도록 콘텐츠 뷰에 `NSTextField`를 추가 합니다.

### <a name="adjusting-the-window-controller"></a>창 컨트롤러 조정

@No__t_0 디자인이 storyboard 또는. xib 파일에서 더 이상 로드 되지 않으므로 창 컨트롤러를 몇 가지 조정 해야 합니다. **MainWindowController.cs** 파일을 편집 하 여 다음과 같이 만듭니다.

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

이 수정의 주요 요소에 대해 설명 하겠습니다.

먼저 `MainWindow` 클래스의 새 인스턴스를 정의 하 고 기본 창 컨트롤러의 `Window` 속성에 할당 합니다.

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

@No__t_0를 사용 하 여 화면 창의 위치를 정의 합니다. 창의 좌표계와 마찬가지로 화면에서 왼쪽 아래 모서리로 (0, 0)을 정의 합니다. 다음으로 **또는** 연산자를 사용 하 여 두 개 이상의 `NSWindowStyle` 기능을 결합 하는 창의 스타일을 정의 합니다.

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
```

다음 `NSWindowStyle` 기능을 사용할 수 있습니다.

- **테두리 없음-창의** 테두리를 포함 하지 않습니다.
- **제목이-창** 에 제목 표시줄이 표시 됩니다.
- **Closable** -창에 닫기 단추가 있으며 닫을 수 있습니다.
- **Miniaturizable** -창에는 Miniaturize 단추가 있으며 최소화 될 수 있습니다.
- **크기 조정 가능-창의** 크기 조정 단추가 있으며 크기를 조정할 수 있습니다.
- **유틸리티** -유틸리티 스타일 창 (패널)입니다.
- **Docmodal** -창이 패널 인 경우 시스템 모달 대신 문서 모달이 됩니다.
- **NonactivatingPanel** -창이 패널 인 경우 주 창이 됩니다.
- **TexturedBackground** -창에 질감이 배경으로 포함 됩니다.
- **크기** 를 조정 하지 않습니다. 창의 크기가 조정 되지 않습니다.
- **UnifiedTitleAndToolbar** -창의 제목 및 도구 모음 영역이 조인 됩니다.
- **Hud** -창이 헤드 디스플레이 패널로 표시 됩니다.
- **Fullscreenwindow** -창이 전체 화면 모드로 전환 될 수 있습니다.
- **FullSizeContentView** -창의 콘텐츠 뷰가 제목 및 도구 모음 영역 뒤에 있습니다.

마지막 두 속성은 창의 _버퍼링 형식을_ 정의 하 고 창의 그리기가 지연 되는 경우를 정의 합니다. @No__t_0에 대 한 자세한 내용은 Windows의 Apple [소개 문서를](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) 참조 하세요.

마지막으로 storyboard 또는. xib 파일에서 창이 팽창 않으므로 windows `AwakeFromNib` 메서드를 호출 하 여 **MainWindowController.cs** 에서 시뮬레이션 해야 합니다.

```csharp
Window.AwakeFromNib ();
```

이렇게 하면 storyboard 또는 xib 파일에서 로드 된 표준 창과 마찬가지로 창에 대해 코딩할 수 있습니다.

### <a name="displaying-the-window"></a>창 표시

Xib 파일을 제거 하 고 **MainWindow.cs** 및 **MainWindowController.cs** 파일을 수정한 경우 Xcode의 Interface Builder. xib 파일로 만든 표준 창과 마찬가지로 창을 사용 하 게 됩니다.

다음은 창 및 해당 컨트롤러의 새 인스턴스를 만들고 창을 화면에 표시 합니다.

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

이 시점에서 응용 프로그램을 실행 하 고 단추를 몇 번 클릭 하면 다음이 표시 됩니다.

![예제 앱 실행](xibless-ui-images/run01.png "예제 앱 실행")

## <a name="adding-a-code-only-window"></a>코드 전용 창 추가

Xibless 코드를 추가 하려면 기존 Xamarin.ios 응용 프로그램에 해당 **Solution Pad** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고  > **새 파일** **추가** ...를 선택 합니다. **새 파일** 대화 상자에서 아래 그림과 같이 컨트롤러를**사용 하 여 xamarin.ios  >  cocoa 창을**선택 합니다.

![새 창 컨트롤러 추가](xibless-ui-images/add01.png "새 창 컨트롤러 추가")

이전과 마찬가지로 프로젝트에서 xib 파일 (이 경우 **SecondWindow. xib**)을 삭제 하 고 위의 [코드를 사용 하](#Switching_a_Window_to_use_Code) 여 창 전환 섹션의 단계에 따라 창의 정의를 코드에 포함 합니다.

## <a name="adding-a-ui-element-to-a-window-in-code"></a>코드에서 창에 UI 요소 추가

코드에서 창이 만들어지거나 storyboard 또는. xib 파일에서 로드 되었는지 여부에 관계 없이 코드에서 창에 UI 요소를 추가 하려는 경우가 있을 수 있습니다. 예를 들면,

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

위의 코드는 새 `NSButton`를 만들어 표시를 위해 `MyWindow` 창 인스턴스에 추가 합니다. 기본적으로 storyboard 또는 xib 파일에서 Xcode의 Interface Builder에 정의할 수 있는 모든 UI 요소는 코드에서 만들고 창에 표시할 수 있습니다.

## <a name="defining-the-menu-bar-in-code"></a>코드에서 메뉴 모음 정의

Xamarin.ios의 현재 제한 사항 때문에 Xamarin.ios 응용 프로그램의 메뉴 모음을 만드는 것은 권장 되지 않습니다. 코드에서 `NSMenuBar` – **기본 storyboard** 또는 **xib** 파일을 사용 하 여 정의 하는 것이 좋습니다. 즉, 코드에서 C# 메뉴 및 메뉴 항목을 추가 하 고 제거할 수 있습니다.

예를 들어 **AppDelegate.cs** 파일을 편집 하 고 `DidFinishLaunching` 메서드를 다음과 같이 만듭니다.

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

위에서는 코드에서 상태 표시줄 메뉴를 만들고 응용 프로그램이 시작 될 때이를 표시 합니다. 메뉴 사용에 대 한 자세한 내용은 [메뉴](~/mac/user-interface/menu.md) 설명서를 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 Xcode 또는 xib 파일과 함께의 Interface Builder 사용 하는 대신 코드에서 C# xamarin.ios 응용 프로그램의 사용자 인터페이스를 만드는 방법에 대해 자세히 살펴봅니다.

## <a name="related-links"></a>관련 링크

- [MacXibless (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macxibless)
- [Windows](~/mac/user-interface/window.md)
- [메뉴](~/mac/user-interface/menu.md)
- [macOS 휴먼 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Windows 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
