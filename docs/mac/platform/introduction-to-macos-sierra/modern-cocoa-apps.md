---
title: "최신 macOS 앱 빌드"
description: "이 문서에서는 몇 가지 팁, 기능 및 기술 Xamarin.Mac에 최신 macOS 앱을 빌드하기 위해 개발자가 사용할 수에 대해 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9073d64c43c6817b45dca02b870fcfe093ebf46d
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="building-modern-macos-apps"></a>최신 macOS 앱 빌드

_이 문서에서는 몇 가지 팁, 기능 및 기술 Xamarin.Mac에 최신 macOS 앱을 빌드하기 위해 개발자가 사용할 수에 대해 설명 합니다._

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>최신 보기와 문서 최신 검색

최신 보기 아래에 표시 된 예제에서는 앱과 같은 최신 창 및 도구 모음 모양을 포함 됩니다.

[ ![](modern-cocoa-apps-images/content08.png "최신 Mac 앱 UI의 예")](modern-cocoa-apps-images/content08.png)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>콘텐츠 뷰 크기의 전체를 사용 하도록 설정

Xamarin.Mac 응용 프로그램에서이은이 위해 개발자가 사용할는 _전체 크기 콘텐츠 보기_, 도구 및 제목 표시줄 영역에서 확장 콘텐츠 의미 및 macOS에서 자동으로 흐리게 표시 되어야 합니다.

코드에서이 기능을 사용 하려면에 대 한 사용자 지정 클래스를 만듭니다는 `NSWindowController` 다음과 같이 만듭니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Set window to use Full Size Content View
            Window.StyleMask = NSWindowStyle.FullSizeContentView;
        }
        #endregion
    }
}
```

이 기능은 사용할 수 있는 또한 Xcode의 인터페이스 작성기 창을 선택 하 고 확인 하 여 **전체 크기의 콘텐츠 뷰**:

[ ![](modern-cocoa-apps-images/content01.png "Xcode의 인터페이스 작성기에서 주 스토리 보드를 편집합니다.")](modern-cocoa-apps-images/content01.png)

전체 크기 콘텐츠 보기를 사용할 때의 제목 및 도구 모음 영역 아래에 있는 콘텐츠를 오프셋 하 여 특정 콘텐츠 (예: 레이블) 아래의 슬라이드 하지 개발자 해야 합니다.

이 문제를 복잡 하 게 만들의 제목 및 도구 모음 영역 사용자를 수행 하는 현재 작업에 따라 동적 높이 가질 수 있습니다 macOS 사용자가을 설치 및/또는 응용 프로그램에서 실행 되는 Mac 하드웨어의 버전입니다.

결과적으로, 사용자 인터페이스를 배치 하는 경우 오프셋을 단순히 하드 코딩도 작동 하지 않습니다. 개발자는 동적 방법으로 접근 해야 합니다.

Apple 내에 포함 된 [Observable 키-값](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes) `ContentLayoutRect` 속성의는 `NSWindow` 코드에서 현재 콘텐츠 영역을 가져올 클래스를 합니다. 개발자 콘텐츠 영역 변경 될 때 필요한 요소를 수동으로 배치 하려면이 값을 사용할 수 있습니다.

자동 레이아웃 및 크기 클래스는 UI 요소의 위치에 코드 또는 인터페이스 작성기를 사용 하는 것이 좋습니다.

다음 예제에서는 다음과 같은 코드를 응용 프로그램의 뷰-컨트롤러에서 자동 레이아웃 및 크기 클래스를 사용 하 여 UI 요소의 위치를 사용할 수 있습니다.

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        #region Computed Properties
        public NSLayoutConstraint topConstraint { get; set; }
        #endregion

        ...

        #region Override Methods
        public override void UpdateViewConstraints ()
        {
            // Has the constraint already been set?
            if (topConstraint == null) {
                // Get the top anchor point
                var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
                var topAnchor = contentLayoutGuide.TopAnchor;

                // Found?
                if (topAnchor != null) {
                    // Assemble constraint and activate it
                    topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
                    topConstraint.Active = true;
                }
            }

            base.UpdateViewConstraints ();
        }
        #endregion
    }
}
```

이 코드는 레이블에 적용 될 최상위 제약 조건에 대 한 저장소를 만듭니다 (`ItemTitle`) 제목 및 도구 모음 영역 아래에 지연 하지 않는 것을 확인 하려면:

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

뷰 컨트롤러를 재정의 하 여 `UpdateViewConstraints` 개발자 수 필요한 제약 조건을 이미 빌드된 경우 테스트 메서드와 필요한 경우 만듭니다.

새 제약 조건을를 구축할 수 있어야 하는 경우는 `ContentLayoutGuide` 제한 하는 컨트롤에 액세스 하 고으로 캐스팅 창의 속성을 `NSLayoutGuide`:

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

TopAnchor 속성은 `NSLayoutGuide` 액세스 및 사용 가능한 경우, 적정 오프셋된 하는 새 제약 조건을 작성 하는 데 사용 됩니다 하 고 새 제약 조건을 적용할 수도 활성화 될:

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>간소화 된 도구 모음을 사용 하도록 설정

일반 macOS 창 제목 표시줄에는 창의 위쪽 가장자리에 실행 하는 표준에 포함 됩니다. 창 도구 모음을 포함 하는 경우이 제목 표시줄 영역에서 표시 됩니다.

[ ![](modern-cocoa-apps-images/content02.png "표준 Mac 도구 모음")](modern-cocoa-apps-images/content02.png)

제목 영역 사라집니다 간소화 된 도구 모음을 사용 하 고 제목 표시줄의 위치에 도구 모음을 위로 이동 하는 경우와 동일한 줄에 창 닫기, 최소화 및 최대화 단추:

[ ![](modern-cocoa-apps-images/content03.png "간소화 된 Mac 도구 모음")](modern-cocoa-apps-images/content03.png)

재정의 하 여 효율적인 도구 모음이 사용는 `ViewWillAppear` 의 메서드는 `NSViewController` 다음와 같은 보이게 하 고:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

이 효과 일반적으로 사용 _신발 상자 응용 프로그램_ (창 하나 앱) 같은 맵, 일정, 정보 및 시스템 환경 설정 합니다. 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>컨트롤러 액세서리 보기를 사용 하 여

응용 프로그램의 디자인에 따라 개발자는 오른쪽 아래 상황에 맞는 컨트롤 제공 활동에 따라 사용자에 게 이러한 하기 위해 제목/도구 모음으로 표시 되는 액세서리 보기 컨트롤러와 영역은 제목 표시줄을 보완 하기 위해 수도 있음 현재에 포함 되어 있습니다.

[ ![](modern-cocoa-apps-images/content04.png "예로 액세서리 뷰-컨트롤러")](modern-cocoa-apps-images/content04.png)

액세서리 뷰 컨트롤러 자동으로 흐리게 표시 되며 개발자의 개입 없이 시스템 크기를 조정 합니다.

액세서리 뷰 컨트롤러를 추가 하려면 다음을 수행 합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭하여 엽니다.
2. 끌어서는 **사용자 지정 보기 컨트롤러** 창의 계층 구조에: 

    [ ![](modern-cocoa-apps-images/content05.png "새 사용자 지정 보기 컨트롤러 추가")](modern-cocoa-apps-images/content05.png)
3. 레이아웃 액세서리 보기의 UI: 

    [ ![](modern-cocoa-apps-images/content06.png "새 뷰 디자인")](modern-cocoa-apps-images/content06.png)
4. 노출 액세서리 뷰는 **콘센트** 및 기타 **동작** 또는 **콘센트** UI에 대 한: 

    [ ![](modern-cocoa-apps-images/content07.png "필요한 콘센트 추가")](modern-cocoa-apps-images/content07.png)
5. 변경 내용을 저장합니다.
6. 변경 내용을 동기화 하는 Mac에 대 한 Visual Studio로 반환 합니다.

편집 된 `NSWindowController` 다음과 같이 만듭니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }
        #endregion
    }
}
```

이 코드의 핵심 요소는 보기 인터페이스 작성기에 정의 되어으로 노출 된 사용자 지정 보기에 설정 되어 있는 한 **콘센트**:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

및 `LayoutAttribute` 정의 하는 _여기서_ 접근자 표시 됩니다.

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

MacOS 이제 지역화 완벽 하 게 되므로 `Left` 및 `Right` `NSLayoutAttribute` 속성 사용이 중단 된 및로 대체 해야 `Leading` 및 `Trailing`합니다.

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>탭된 창 사용

또한 macOS 시스템 컨트롤러 액세서리 보기 응용 프로그램의 창을 추가할 수 있습니다. 예를 들어, 하나의 가상 창에 병합 됩니다는 다양 한 응용 프로그램의 Windows 탭 창을 만들 수 있습니다.

[ ![](modern-cocoa-apps-images/content08.png "탭된 창 Mac의 예")](modern-cocoa-apps-images/content08.png)

일반적으로 개발자 탭 Windows가 Xamarin.Mac 앱에 제한 된 작업 사용을 수행 해야 합니다, 그리고 시스템이 다음과 같이 자동으로 처리 됩니다.

- Windows는 자동으로 됩니다 때 탭의 `OrderFront` 메서드가 호출 됩니다.
- Windows는 자동으로 됩니다 때 Untabbed는 `OrderOut` 메서드가 호출 됩니다.
- 그러나 모든 탭 창을 것으로 간주 "표시" 하는 코드에서이 아닌 앞쪽 탭 CoreGraphics를 사용 하 여 시스템에 의해 숨겨집니다.
- 사용 하 여는 `TabbingIdentifier` 속성 `NSWindow` 창을 함께 탭으로 그룹에 있습니다.
- 이 경우는 `NSDocument` 개발자의 개입 없이 자동으로 (예: 탭 모음에 추가 되 고 더하기 단추) 이러한 기능 중 몇 가지 기반된 응용 프로그램을 사용할 수 있습니다.
- 비-`NSDocument` 기반된 앱 "+" 단추를 재정의 하 여 새 문서를 추가 하려면 탭 그룹에 사용 하도록 설정할 수는 `GetNewWindowForTab` 의 메서드는 `NSWindowsController`합니다.

모든 부분이 함께, 가져오기는 `AppDelegate` 탭 Windows 기반 시스템을 사용 하려고 하는 응용 프로그램의 다음과 같은 표시 될 수 있습니다.

```csharp
using AppKit;
using Foundation;

namespace MacModern
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewDocumentNumber { get; set; } = 0;
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

        #region Custom Actions
        [Export ("newDocument:")]
        public void NewDocument (NSObject sender)
        {
            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow (this);
        } 
        #endregion
    }
}
```

여기서는 `NewDocumentNumber` 속성은 생성 하는 새 문서 수와 `NewDocument` 메서드는 새 문서를 만들고 표시 합니다.

`NSWindowController` 다음 다음과 같을 수 있습니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Application Access
        /// <summary>
        /// A helper shortcut to the app delegate.
        /// </summary>
        /// <value>The app.</value>
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void SetDefaultDocumentTitle ()
        {
            // Is this the first document?
            if (App.NewDocumentNumber == 0) {
                // Yes, set title and increment
                Window.Title = "Untitled";
                ++App.NewDocumentNumber;
            } else {
                // No, show title and count
                Window.Title = $"Untitled {App.NewDocumentNumber++}";
            }
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Prefer Tabbed Windows
            Window.TabbingMode = NSWindowTabbingMode.Preferred;
            Window.TabbingIdentifier = "Main";

            // Set default window title
            SetDefaultDocumentTitle ();

            // Set window to use Full Size Content View
            // Window.StyleMask = NSWindowStyle.FullSizeContentView;

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }

        public override void GetNewWindowForTab (NSObject sender)
        {
            // Ask app to open a new document window
            App.NewDocument (this);
        }
        #endregion
    }
}
```

여기서는 정적 `App` 속성을 시작 하는 바로 가기를 제공는 `AppDelegate`합니다. `SetDefaultDocumentTitle` 메서드를 생성 하는 새 문서 수에 따라 새 문서 제목을 설정 합니다.

다음 코드는 탭을 사용 하는 앱에서 지 원하는 기본 macOS 지시 하 고 응용 프로그램의 Windows 탭으로 그룹화 할 수 있도록 하는 문자열을 제공 합니다.

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

및 다음 재정의 메서드를 사용자가 클릭 하면 새 문서 만들기 탭 모음에 더하기 단추를 추가 합니다.

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>코어 애니메이션을 사용 하 여

코어 애니메이션은 macOS에 포함 된 높은 전원이 그래픽 렌더링 엔진입니다. 코어 애니메이션 GPU (그래픽 처리 장치) 그래픽 작업 컴퓨터 속도가 느려질 수 있는 CPU에서 실행 되는 달리 최신 macOS 하드웨어에서 사용할 수 있는 기능을 활용 하도록 최적화 되었습니다.

`CALayer`, 코어 애니메이션에 제공 된, 신속 하 고 유동적인 스크롤 및 애니메이션 같은 작업에 사용할 수 있습니다. 여러 개의 하위와 완벽 하 게 활용 하기 위해 핵심 애니메이션 레이어는 응용 프로그램의 사용자 인터페이스 구성 되어야 합니다.

A `CALayer` 개체와 같은 표시 되는 내용을 제어 하는 개발자 화면에 나타나는 사용자에 게 허용 하는 몇 가지 속성을 제공 합니다.

- `Content` -수는 `NSImage` 또는 `CGImage` 계층의 내용을 제공 하는 합니다.
- `BackgroundColor` -으로 계층의 배경색을 설정 합니다.는 `CGColor`
- `BorderWidth` -테두리 두께 설정합니다.
- `BorderColor` -테두리 색을 설정합니다.

응용 프로그램의 UI의 핵심 그래픽을 사용 하려면이 사용 해야 _계층 지원_ Apple 제안 개발자 창의 콘텐츠 뷰에 항상 사용 해야 하는 보기입니다. 이러한 방식으로 모든 자식 뷰에 자동으로 상속 계층 백업도 합니다.

또한 Apple 제안 레이어 백업 뷰를 사용 하 여 새 추가 달리 `CALayer` 는 하위 계층으로 시스템은 자동으로 처리 몇 가지 필수 설정 (예: 레 티 나 디스플레이에서 필요한 것) 때문에 합니다.

백업 레이어를 설정 하 여 사용할 수는 `WantsLayer` 의 `NSView` 를 `true` 이나 아래 Xcode의 인터페이스 작성기 안에 **보기 효과 검사기** 확인 하 여 **코어 애니메이션 레이어**:

[ ![](modern-cocoa-apps-images/content09.png "보기 효과 검사기")](modern-cocoa-apps-images/content09.png)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>레이어를 사용 하 여 뷰를 다시 그리기

레이어 백업 뷰를 사용 하 여 Xamarin.Mac 응용 프로그램에서을 설정할 때 또 다른 중요 한 단계는 `LayerContentsRedrawPolicy` 의 `NSView` 를 `OnSetNeedsDisplay` 에 `NSViewController`합니다. 예:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

개발자는이 속성을 설정 하지 않는 경우 보기 다시 그릴 프레임 원점이 변경 될 때마다 성능상의 이유로 원하지 않는 합니다. 그러나이 속성 설정 하 여 `OnSetNeedsDisplay` 개발자 수동으로 설정 해야 합니다 `NeedsDisplay` 를 `true` 콘텐츠 다시 그리기를 강제로 합니다.

시스템 확인 하는 뷰는 커밋된 것으로 표시 되 면는 `WantsUpdateLayer` 보기의 속성입니다. 반환 하는 경우 `true` 하면 `UpdateLayer` 다른 메서드가 호출 되는 `DrawRect` 보기의 내용을 업데이트 하는 보기의 메서드가 호출 됩니다.

Apple에 필요할 때 뷰 콘텐츠를 업데이트 하기 위한 다음 제안 사항이 있습니다.

- 사용 하 여 Apple 선호 `UpdateLater` 통해 `DrawRect` 때마다 가능한 것으로 성능이 크게 향상을 제공 합니다.
- 동일한를 사용 하 여 `layer.Contents` UI 요소와 유사 합니다.
- 또한 Apple 등의 표준 보기를 사용 하 여 해당 UI를 작성 하는 개발자가 선호 `NSTextField`, 다시 가능 합니다.

사용 하려면 `UpdateLayer`에 대 한 사용자 지정 클래스를 만듭니다는 `NSView` 다음과 같은 코드를 확인 합니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView
    {
        #region Computed Properties
        public override bool WantsLayer {
            get { return true; }
        }

        public override bool WantsUpdateLayer {
            get { return true; }
        }
        #endregion

        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void DrawRect (CoreGraphics.CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

        }

        public override void UpdateLayer ()
        {
            base.UpdateLayer ();

            // Draw view 
            Layer.BackgroundColor = NSColor.Red.CGColor;
        }
        #endregion
    }
}
```

<a name="Using-Modern-Drag-and-Drop" />

## <a name="using-modern-drag-and-drop"></a>최신 끌어 놓기를 사용 하 여

개발자는 사용자에 대 한 최신 끌어서 놓기 환경을 제공, 채택 해야 _끌어서 떼지_ 해당 응용 프로그램에서 끌어서 놓기 작업 합니다. 끌기 Flocking는 각 개별 파일 또는 처음 끌고 항목 (항목 수의 개수와 커서 아래에 함께 그룹)를 사용자가 계속 되는 끌기 작업 flocks 하는 개별 요소로 나타납니다.

끌기 작업을 종료 하는 경우 개별 요소 Unflock이 고 원래 위치로 반환 합니다.

다음 코드 예제에서는 사용자 지정 보기에서 끌어서 떼지 수 있습니다.

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView, INSDraggingSource, INSDraggingDestination
    {
        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void MouseDragged (NSEvent theEvent)
        {
            // Create group of string to be dragged
            var string1 = new NSDraggingItem ((NSString)"Item 1");
            var string2 = new NSDraggingItem ((NSString)"Item 2");
            var string3 = new NSDraggingItem ((NSString)"Item 3");

            // Drag a cluster of items
            BeginDraggingSession (new [] { string1, string2, string3 }, theEvent, this);
        }
        #endregion
    }
}
```

떼지 효과를 끌고 각 항목을 전송 하 여 이루어졌습니다는 `BeginDraggingSession` 의 메서드는 `NSView` 배열에서 별도 요소로 합니다.

작업할 때는 `NSTableView` 또는 `NSOutlineView`를 사용 하 여는 `PastboardWriterForRow` 의 메서드는 `NSTableViewDataSource` 끌기 작업을 시작 하는 클래스:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDataSource: NSTableViewDataSource
    {
        #region Constructors
        public ContentsTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override INSPasteboardWriting GetPasteboardWriterForRow (NSTableView tableView, nint row)
        {
            // Return required pasteboard writer
            ...
            
            // Pasteboard writer failed
            return null;
        }
        #endregion
    }
}
```

이 통해 개발자는 개별 `NSDraggingItem` 이전 방법과 반대로 끌고 있는 테이블에 있는 모든 항목에 대 한 `WriteRowsWith` 쓰기에 모든 행을 단일 그룹으로 지를 합니다.

작업할 때 `NSCollectionViews`, 다시 사용 하 여는 `PasteboardWriterForItemAt` 메서드가 아닌는 `WriteItemsAt` 끌어 올 때 메서드를 시작 합니다.

개발자는 큰 파일을 지에 두면 안 항상 됩니다. MacOS 시에라, 새 _파일 Promises_ 개발자 참조를 배치할 수 있도록 지정 된 파일을 사용 하 여 새 놓기 작업을 마쳤을 때 나중에 달성 될 지를 `NSFilePromiseProvider` 및 `NSFilePromiseReceiver` 클래스입니다.

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>최신 이벤트 추적을 사용 하 여

사용자 인터페이스 요소에 대 한 (예:는 `NSButton`) 추가 된 도구 모음 또는 제목 영역에 사용자 요소를 클릭 하 고 (예: 팝업 창이 표시) 일반적인 방법으로 이벤트를 발생 시킬 수 있어야 합니다. 그러나 항목 제목 또는 도구 모음 영역 이기도 하므로 사용자 수 있어야 클릭 하 고도 창을 이동 하는 요소를 끌어 옵니다.

이를 위해 코드에서 요소에 대 한 사용자 지정 클래스를 만듭니다 (같은 `NSButton`)를 재정의 하 고는 `MouseDown` 다음과 같이 이벤트로:

```csharp
public override void MouseDown (NSEvent theEvent)
{
    var shouldCallSuper = false;

    Window.TrackEventsMatching (NSEventMask.LeftMouseUp, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Handle event as normal
        stop = true;
        shouldCallSuper = true;
    });

    Window.TrackEventsMatching(NSEventMask.LeftMouseDragged, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Pass drag event to window
        stop = true;
        Window.PerformWindowDrag (evt);
    });

    // Call super to handle mousedown
    if (shouldCallSuper) {
        base.MouseDown (theEvent);
    }
}
```

사용 하 여이 코드는 `TrackEventsMatching` 의 메서드는 `NSWindow` 가로채기 위해 UI 요소에 연결 된는 `LeftMouseUp` 및 `LeftMouseDragged` 이벤트입니다. 에 대 한는 `LeftMouseUp` 이벤트가, UI 요소에 일반적인 방법으로 응답 합니다. 에 대 한는 `LeftMouseDragged` 이벤트, 이벤트를 전달 되는 `NSWindow`의 `PerformWindowDrag` 화면에서 창을 이동 하는 메서드.

호출의 `PerformWindowDrag` 의 메서드는 `NSWindow` 클래스는 다음과 같은 이점을 제공 합니다.

- 앱이 중단 된 경우에 이동할 창에 대 한 허용 (예를 들어 전체 루프 처리).
- 공간 전환 예상 대로 작동 합니다.
- 공간 막대는 정상적으로 표시 됩니다.
- 창 맞추기 및 맞춤 정상적으로 작동 합니다.

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>최신 컨테이너 보기 컨트롤을 사용 하 여

macOS 시에라 최신 향상 된 여러 운영 체제의 이전 버전에서 사용할 수 있는 기존 컨테이너 보기 컨트롤에 제공합니다.

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>테이블 보기의 향상 된 기능

개발자는 새 항상 사용 해야 `NSView` 와 같은 컨테이너 뷰 컨트롤의 버전을 기반으로 `NSTableView`합니다. 예:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }
        #endregion
    }
}
```

따라서 사용자 지정 테이블 행 작업 (예: 행을 삭제 하는 오른쪽에서 넘기기가) 테이블의 행 지정한에 연결 될 수 있습니다. 이 동작을 사용 하도록 설정 하려면 재정의 `RowActions` 의 메서드는 `NSTableViewDelegate`:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }

        public override NSTableViewRowAction [] RowActions (NSTableView tableView, nint row, NSTableRowActionEdge edge)
        {
            // Take action based on the edge
            if (edge == NSTableRowActionEdge.Trailing) {
                // Create row actions
                var editAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Regular, "Edit", (action, rowNum) => {
                    // Handle row being edited
                    ...
                });

                var deleteAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Destructive, "Delete", (action, rowNum) => {
                    // Handle row being deleted
                    ...
                });

                // Return actions
                return new [] { editAction, deleteAction };
            } else {
                // No matching actions
                return null;
            }
        }
        #endregion
    }
}
```

정적 `NSTableViewRowAction.FromStyle` 스타일의 새 테이블 행 작업을 만드는 데 사용 됩니다.

- `Regular` -표준 비파괴 동작 편집 같은 행의 내용을 수행합니다.
- `Destructive` -파괴적 같은 작업을 수행 삭제 행 테이블에서 합니다. 이러한 작업은 배경이 빨간색으로 렌더링 됩니다.

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>향상 된 스크롤 뷰 기능 

스크롤 보기를 사용 하는 경우 (`NSScrollView`) 직접 또는 다른 컨트롤의 일부로 (같은 `NSTableView`), 스크롤 보기의 콘텐츠는 최신 모양과 뷰를 사용 하 여 Xamarin.Mac 응용 프로그램에서 제목 및 도구 모음 영역 아래 동일 수입니다.

결과적으로, 스크롤 보기 콘텐츠 영역에 첫 번째 항목 제목 및 도구 모음 영역별로 부분적으로 가려질 수 있습니다.

이 문제를 해결 하기 위해 Apple 두 개의 새 속성을 추가 했습니다는 `NSScrollView` 클래스:

- `ContentInsets` -개발자는 허용 된 `NSEdgeInsets` 스크롤 보기의 맨 위에 적용 될 오프셋을 정의 하는 개체입니다.
- `AutomaticallyAdjustsContentInsets` -If `true` 스크롤 보기를 자동으로 처리할는 `ContentInsets` 개발자에 대 한 합니다.

사용 하 여는 `ContentInsets` 개발자와 같은 액세서리의 포함 여부에 대 한 있도록 스크롤 보기의 시작을 조정할 수 있습니다.

- 정렬 표시기 같은 메일 응용 프로그램에 표시 된 것입니다.
- 검색 필드입니다.
- 새로 고침 또는 업데이트 단추입니다.

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>자동 레이아웃 및 최신 앱에 대 한 지역화

Apple는 개발자는 다국어 macOS 응용 프로그램을 쉽게 만들 수 있도록 하는 Xcode에서 여러 가지 기술을 포함 않았습니다. 이제 Xcode 개발자가 사용자 쪽 텍스트는 해당 스토리 보드 파일에 응용 프로그램의 사용자 인터페이스 디자인에서 구분 하 고 UI가 변경 될 경우 이러한 구분을 유지 하는 도구를 제공 합니다.

자세한 내용은 Apple의를 참조 하십시오 [국제화 및 지역화 가이드](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)합니다.

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>국제화 기본 구현 

개발자는 자료 국제화를 구현 하 여 모든 사용자 용 문자열의 분리 및 응용 프로그램의 UI를 나타내는 단일 스토리 보드 파일을 제공할 수 있습니다. 

초기 스토리 보드 파일 (또는 파일) 개발자가 작성 하는 경우 응용 프로그램의 사용자 인터페이스를 정의 하는, 자료 국제화 (개발자 말하는 언어)에서 생성 됩니다.

다음으로, 개발자 지역화 및 여러 언어로 번역할 수 있습니다 (스토리 보드 UI 디자인)에서 기본 국제화 문자열을 내보낼 수 있습니다.

이상에서는 이러한 지역화 가져올 수 있으며 Xcode에서 스토리 보드에 대 한 언어별 문자열 파일을 생성 합니다.

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>지역화를 지 원하는 자동 레이아웃 구현

지역화 된 버전 때문에 문자열의 값 매우 다양 한 크기를 가질 수 있습니다 및/또는 읽기 방향, 개발자는 데 자동 레이아웃 위치를 지정 하 고 응용 프로그램의 사용자 인터페이스를 스토리 보드 파일의 크기입니다.

Apple 다음을 수행 하 제안 합니다.

- **고정 너비 제약 조건을 제거** -모든 텍스트 기반 뷰 허용 크기를 조정 내용을 기준으로 합니다. 고정 폭 보기 특정 언어에서 해당 콘텐츠를 자를 수 있습니다.
- **기본 콘텐츠 크기를 사용 하 여** 기본 텍스트 기반 뷰에서 자동 크기 내용에 맞게 그룹화 합니다. 올바르게 크기 조정 하지 텍스트 기반 보기에 대 한 Xcode의 인터페이스 작성기에서 해당를 선택한 다음 선택 **편집** > **크기에 맞게 콘텐츠**합니다.
- **선행 및 후행 특성 적용** 텍스트 방향을 변경할 수 있으므로-사용자의 언어에 따라, 사용 하 여 새 `Leading` 및 `Trailing` 달리 기존 제약 조건 속성 `Right` 및 `Left` 특성입니다. `Leading` 및 `Trailing` 언어 방향에 따라 자동으로 조정 됩니다.
- **Pin 뷰를 인접 한 뷰에** -이렇게 하면는 위치를 변경 하 고 뷰를 선택 된 언어에 따르게 뷰 변경 크기를 조정 합니다.
- **Windows 최소 및/또는 최대 크기를 설정 하지 않으면** -크기를 변경 하려면 선택한 언어의 콘텐츠 영역 크기를 조정할 때 창 허용 합니다.
- **계속 해 서 레이아웃 변경 내용을 테스트** 도중-서로 다른 언어로 개발 응용 프로그램에 지속적으로 테스트 해야 합니다. Apple의 참조 [Your Internationalized 테스트 앱](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1) 자세한 내용은 설명서입니다.
- **NSStackViews 뷰 함께 Pin을 사용 하 여**  -  `NSStackViews` 내용을 이동 하 고 예측 가능한 방식으로 성장 하 고 콘텐츠 선택 된 언어에 따라 크기를 변경할 수 있습니다.

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Xcode의 인터페이스 작성기에서 지역화

Apple는 개발자 때 사용할 수 있는 디자인 하거나 응용 프로그램의 UI 편집 지역화를 지 원하는 Xcode의 인터페이스 작성기의 몇 가지 기능 제공 했습니다. **텍스트 방향을** 섹션은 **특성 검사기** 개발자 방향 해야 사용 되 고 select에서는 텍스트 기반 뷰에서 업데이트 방법에 힌트를 제공할 수 있습니다 (같은 `NSTextField`):

[ ![](modern-cocoa-apps-images/content10.png "텍스트 방향 옵션")](modern-cocoa-apps-images/content10.png)

에 대 한 세 가지 가능한 값은 **텍스트 방향**:

- **자연** -레이아웃 컨트롤에 할당 된 문자열은 기반으로 합니다.
- **왼쪽에서 오른쪽으로** -레이아웃 항상 왼쪽에서 오른쪽으로 강제 적용 합니다.
- **오른쪽에서 왼쪽** -레이아웃 항상 오른쪽에서 왼쪽으로 강제 적용 합니다.

에 대 한 두 개의 가능한 값은 **레이아웃**:

- **왼쪽에서 오른쪽으로** -레이아웃 항상 왼쪽에서 오른쪽입니다.
- **오른쪽에서 왼쪽** -오른쪽에서 왼쪽 레이아웃은 항상 있습니다.

일반적으로 이러한 변경할 수 없습니다 특정 맞춤 필요한 경우를 제외 합니다.

**미러** 속성 (예: 셀 이미지의 위치) 특정 컨트롤 속성을 대칭 이동 하려면 시스템에 알려줍니다. 여기에 세 가지 가능한 값에 있습니다.

- **자동으로** -위치가 자동으로 선택 된 언어의 방향에 따라 변경 됩니다.
- **오른쪽부터 왼쪽 인터페이스에서** -위치를 기준으로 왼쪽된 언어 오른쪽에 변경할 수만 있습니다.
- **하지** -위치 변경 되지 것입니다.

개발자가 지정한 경우 **센터**, **Justify** 또는 **전체** 텍스트 기반 보기의 내용에 맞춤, 이러한를 절대로 수에 따라 대칭 이동 된 언어를 선택 합니다.

시에라 macOS 하기 전에 코드에서 만든 컨트롤은 수 미러되지 자동으로. 개발자는 다음과 같은 코드를 사용 하 여 미러링 처리 해야 했습니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Setting a button's mirroring based on the layout direction
    var button = new NSButton ();
    if (button.UserInterfaceLayoutDirection == NSUserInterfaceLayoutDirection.LeftToRight) {
        button.Alignment = NSTextAlignment.Right;
        button.ImagePosition = NSCellImagePosition.ImageLeft;
    } else {
        button.Alignment = NSTextAlignment.Left;
        button.ImagePosition = NSCellImagePosition.ImageRight;
    }
}
```

여기서는 `Alignment` 및 `ImagePosition` 에 따라 설정 되 고 `UserInterfaceLayoutDirection` 컨트롤의 합니다.

여러 새 편의 생성자를 추가 하는 macOS 시에라 (정적 통해 `CreateButton` 메서드) (예: 제목, 이미지 및 작업)의 여러 매개 변수를 사용 하 고 자동으로 올바르게 미러링하면 합니다. 예:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>시스템 모양을 사용 하 여

최신 macOS 앱 이미지 만들기, 편집 또는 프레젠테이션 응용 프로그램에 사용할 새 어두운 인터페이스 모양을 채택할 수 있습니다.

[ ![](modern-cocoa-apps-images/content11.png "어두운 Mac 창 UI의 예")](modern-cocoa-apps-images/content11.png)

이 창을 표시 하기 전에 한 줄의 코드를 추가 하 여 수행할 수 있습니다. 예:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        ...
    
        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Apply the Dark Interface Appearance
            View.Window.Appearance = NSAppearance.GetAppearance (NSAppearance.NameVibrantDark);

            ...
        }
        #endregion
    }
}
```

정적 `GetAppearance` 의 메서드는 `NSAppearance` 클래스는 시스템에서 명명 된 모양을 가져오는 데 사용 됩니다 (이 경우 `NSAppearance.NameVibrantDark`).

Apple에 시스템 모양을 사용 하기 위한 다음 제안 사항을:

- 명명 된 색 하드 코드 된 값 보다 것이 좋은 (예: `LabelColor` 및 `SelectedControlColor`).
- 가능한 경우 시스템 표준 컨트롤 스타일을 사용 합니다.

내게 필요한 옵션 기능 시스템 환경 설정 앱에서 사용 하도록 설정한 사용자에 대 한 시스템 모양을 사용 하는 macOS 앱 올바르게 자동으로 작동 합니다. 결과적으로, 사과 개발자가 macOS 앱에서 시스템 모양을 사용 해야 항상 제안 합니다.

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>스토리 보드와 Ui를 디자인합니다.

스토리 보드를 사용 하면 개발자는 응용 프로그램의 사용자 인터페이스를 구성 하지만 시각화 하 고 UI를 디자인 하는 개별 요소를 배치 하는 디자인 및 지정 된 요소 들의 계층이 뿐만 아니라 합니다.

컨트롤러를 사용 하면 개발자 단위 컴퍼지션 및 Segues 추상으로 요소를 수집 하는 일반적인 "글 루 코드" 보기 계층 구조 전체에서 이동 하는 데 필요한를 제거할:

[ ![](modern-cocoa-apps-images/content12.png "UI Xcode의 인터페이스 작성기에서 편집")](modern-cocoa-apps-images/content12.png)

자세한 내용은 참조 하십시오 우리의 [스토리 보드에는 소개](~/mac/platform/storyboards/index.md) 설명서입니다.

스토리 보드에 정의 된 해당된 장면 뷰 계층 구조에서 이전 장면에서 데이터를 필요 합니다 많은 인스턴스가 있습니다. Apple 장면 간에 정보를 전달 하는 것에 대 한 다음 제안 사항을 있습니다.

- 계층 구조를 통해 데이터 dependancies 아래쪽으로 캐스케이드될 항상 해야 합니다.
- 이 인해 UI 유연성 제한으로 하드 코딩 UI 구조적 dependancies를 하지 마십시오.
- C#의 인터페이스를 사용 하 여 일반 데이터 dependancies 제공.

Segue의 원본으로 사용 되는 보기 컨트롤러 재정의할 수는 `PrepareForSegue` 메서드와 초기화 (예: 데이터 전달)는 Segue 하기 전에 필요한 수행 대상 뷰-컨트롤러를 표시 하려면 실행 됩니다. 예:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

자세한 내용은 참조 하십시오 우리의 [Segues](~/mac/platform/storyboards/indepth.md#Segues) 설명서입니다.

<a name="Propagating-Actions" />

## <a name="propagating-actions"></a>동작 전파

MacOS 응용 프로그램의 디자인에 따라, 경우도 경우 UI 컨트롤에 대 한 작업에 대 한 최상의 처리기에 있을 수 있습니다 다른 곳에서 UI 계층 구조에서. 메뉴 및 응용 프로그램의 UI의 나머지 부분과에서 별도 자체 장면에 거주 하는 메뉴 항목의 경우 일반적으로 그렇습니다.

이 상황을 처리 하려면 개발자 사용자 지정 작업을 만들고 전달할 수 응답자 체인을 작업 합니다. 자세한 내용은 참조 하십시오 우리의 [작업 사용자 지정 창 작업을](~/mac/user-interface/menu.md) 설명서입니다.

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>최신 Mac 기능

Apple는 개발자와 같은 Mac 플랫폼의 대부분을 사용할 수 있는 macOS 시에라에에서 여러 사용자 용 기능이 포함:

- **NSUserActivity** -이렇게 하면 응용 프로그램에서 사용자가 현재에 관련 활동에 설명 합니다. `NSUserActivity` 가 사용자의 장치 중 하나에서 시작 된 활동 수을 선택 및 다른 장치에서 계속 여기서 핸드 오프를 지원 하기 위해 처음 생성 됩니다. `NSUserActivity` iOS에서 수행 하므로 macOS에서 동일 하 게 작동을 참조 하십시오 우리의 [핸드 오프 소개](~/ios/platform/handoff.md) 자세한 내용은 iOS 설명서입니다.
- **Mac에서 Siri** -Siri 현재 활동을 사용 하 여 (`NSUserActivity`)는 사용자가 생성할 수 명령에 대 한 컨텍스트를 제공 합니다.
- **복원 상태** -macOS에서 한 다음 나중에 응용 프로그램 자동으로는 응용 프로그램을 relaunches 종료 되는 사용자를 이전 상태로 반환 하는 경우. 개발자를 인코딩 및 사용자 인터페이스가 사용자에 게 표시 하기 전에 임시 UI 상태를 복원 상태 복원 API를 사용할 수 있습니다. 하는 경우 `NSDocument` 따르면 상태 복원이 자동으로 처리 됩니다. 에 대 한 복원 상태를 사용 하도록 설정 하려면 비-`NSDocument` 기반된 앱 설정의 `Restorable` 의 `NSWindow` 클래스를 `true`합니다.
- **클라우드에서 문서** -시에라 macOS 이전 앱 명시적으로 옵트인 사용자의 iCloud 드라이브의에서 문서로 작업할 해야 했습니다. 시에라 사용자의 macOS에서 **데스크톱** 및 **문서** 폴더도 시스템에서 자동으로 해당 iCloud 드라이브와 동기화 수 수 있습니다. 결과적으로, 사용자의 컴퓨터에 대 한 공간을 확보 하도록 문서의 로컬 복사본을 삭제할 수 있습니다. `NSDocument` 기반된 앱이 변경이 내용을 자동으로 처리 됩니다. 다른 모든 응용 프로그램 종류를 사용 해야 합니다는 `NSFileCoordinator` 읽기 및 쓰기의 문서 동기화 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서는 몇 가지 팁, 기능 및 기술 Xamarin.Mac에 최신 macOS 앱을 빌드하기 위해 개발자가 사용할 수 검사가 합니다.



## <a name="related-links"></a>관련 링크

- [macOS 샘플](https://developer.xamarin.com/samples/mac/)
