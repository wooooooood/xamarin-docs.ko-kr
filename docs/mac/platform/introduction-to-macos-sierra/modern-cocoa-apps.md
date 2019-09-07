---
title: 최신 macOS 앱 빌드
description: 이 문서에서는 개발자가 Xamarin.ios에서 최신 macOS 앱을 빌드하는 데 사용할 수 있는 몇 가지 팁, 기능 및 기술을 설명 합니다.
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 4be5670829b2b8c1a5a73f564b4c031b6a26bd54
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769853"
---
# <a name="building-modern-macos-apps"></a>최신 macOS 앱 빌드

_이 문서에서는 개발자가 Xamarin.ios에서 최신 macOS 앱을 빌드하는 데 사용할 수 있는 몇 가지 팁, 기능 및 기술을 설명 합니다._

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>최신 보기를 사용 하 여 최신 빌드 빌드

최신 창 및 도구 모음 모양 (예: 아래에 표시 된 예제 앱)이 포함 됩니다.

[![](modern-cocoa-apps-images/content08.png "최신 Mac 앱 UI의 예")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>전체 크기의 콘텐츠 보기 사용

이를 위해 Xamarin.ios 앱에서 개발자는 _전체 크기의 콘텐츠 보기_를 사용 하려고 합니다. 즉, 콘텐츠가 도구 및 제목 표시줄 영역 아래에 확장 되 고 macos에 의해 자동으로 흐리게 표시 됩니다.

코드에서이 기능을 사용 하려면에 대 한 사용자 지정 클래스 `NSWindowController` 를 만들고 다음과 같이 표시 되도록 합니다.

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

창을 선택 하 고 **전체 크기의 콘텐츠 보기**를 확인 하 여 Xcode의 Interface Builder 에서도이 기능을 사용 하도록 설정할 수 있습니다.

[![](modern-cocoa-apps-images/content01.png "Xcode의 Interface Builder에서 주 스토리 보드 편집")](modern-cocoa-apps-images/content01.png#lightbox)

전체 크기 콘텐츠 보기를 사용 하는 경우 개발자는 특정 콘텐츠 (예: 레이블)에서 슬라이드를 이동 하지 않도록 제목 및 도구 모음 영역 아래에 콘텐츠를 오프셋 해야 할 수 있습니다.

이 문제를 복잡 하 게 하려면 제목 및 도구 모음 영역에 사용자가 현재 수행 하 고 있는 작업, 사용자가 설치한 macOS 버전 및/또는 앱이 실행 되 고 있는 Mac 하드웨어에 따라 동적 높이가 있을 수 있습니다.

따라서 사용자 인터페이스를 레이아웃할 때 오프셋을 하드 코딩 하는 것은 쉽지 않습니다. 개발자가 동적 접근 방법을 사용 해야 합니다.

Apple은 `NSWindow` 클래스의 [키-값 관찰](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes) `ContentLayoutRect` 가능 속성을 포함 하 여 코드에서 현재 콘텐츠 영역을 가져옵니다. 개발자는이 값을 사용 하 여 콘텐츠 영역이 변경 될 때 필요한 요소를 수동으로 배치할 수 있습니다.

더 나은 솔루션은 자동 레이아웃 및 크기 클래스를 사용 하 여 UI 요소를 두 코드 또는 Interface Builder 배치 하는 것입니다.

다음 예제와 같은 코드를 사용 하 여 앱의 뷰 컨트롤러에서 자동 레이아웃 및 크기 클래스를 사용 하 여 UI 요소를 배치할 수 있습니다.

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

이 코드는 제목 및 도구 모음 영역에서 지연 되지 않도록 레이블 (`ItemTitle`)에 적용 되는 상위 제약 조건에 대 한 저장소를 만듭니다.

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

개발자는 뷰 컨트롤러의 `UpdateViewConstraints` 메서드를 재정의 하 여 필요한 제약 조건이 이미 빌드 되었는지 확인 하 고 필요한 경우이를 만들지를 테스트할 수 있습니다.

새 제약 조건을 빌드해야 `ContentLayoutGuide` 하는 경우 창의 속성은 제한 되어야 하는 컨트롤에 액세스 하 여로 캐스팅 `NSLayoutGuide`합니다.

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

의 `NSLayoutGuide` topanchor 속성에 액세스 하 고 사용할 수 있는 경우 원하는 오프셋 양만큼 새 제약 조건을 작성 하는 데 사용 되며 새 제약 조건이 활성으로 설정 되어 적용 됩니다.

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>간소화 된 도구 모음 사용

표준 macOS 창에는 창의 위쪽 가장자리를 따라 실행에 표준 제목 표시줄이 있습니다. 창에도 도구 모음이 포함 되어 있으면이 제목 표시줄 영역 아래에 표시 됩니다.

[![](modern-cocoa-apps-images/content02.png "표준 Mac 도구 모음")](modern-cocoa-apps-images/content02.png#lightbox)

간소화 된 도구 모음을 사용 하면 제목 영역이 사라지고 도구 모음이 창 닫기, 최소화 및 최대화 단추를 사용 하 여 제목 표시줄의 위치로 이동 합니다.

[![](modern-cocoa-apps-images/content03.png "간소화 된 Mac 도구 모음")](modern-cocoa-apps-images/content03.png#lightbox)

간소화 된 도구 모음은 `ViewWillAppear` `NSViewController` 의 메서드를 재정의 하 여 활성화 되며 다음과 같이 표시 됩니다.

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

이 효과는 일반적으로 지도, 일정, 메모 및 시스템 기본 설정과 같은 _Shoebox 응용 프로그램_ (하나의 창 앱)에 사용 됩니다. 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>액세서리 보기 컨트롤러 사용

응용 프로그램의 디자인에 따라 개발자는 제목/도구 모음 영역 바로 아래에 표시 되는 액세서리 뷰 컨트롤러를 사용 하 여 제목 표시줄 영역을 보완 하 여 해당 활동에 따라 사용자에 게 상황에 맞는 컨트롤을 제공할 수도 있습니다. 현재 참여 하 고 있습니다.

[![](modern-cocoa-apps-images/content04.png "예제 액세서리 뷰 컨트롤러")](modern-cocoa-apps-images/content04.png#lightbox)

액세서리 뷰 컨트롤러는 개발자 개입 없이 시스템에 의해 자동으로 흐리게 표시 되 고 크기가 조정 됩니다.

액세서리 뷰 컨트롤러를 추가 하려면 다음을 수행 합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭하여 엽니다.
2. **사용자 지정 뷰 컨트롤러** 를 창의 계층으로 끌어 옵니다. 

    [![](modern-cocoa-apps-images/content05.png "새 사용자 지정 뷰 컨트롤러 추가")](modern-cocoa-apps-images/content05.png#lightbox)
3. 액세서리 보기의 UI 레이아웃: 

    [![](modern-cocoa-apps-images/content06.png "새 뷰 디자인")](modern-cocoa-apps-images/content06.png#lightbox)
4. 해당 UI에 대 한 **콘센트** 및 기타 **작업** 또는 **콘센트** 로 액세서리 보기를 노출 합니다. 

    [![](modern-cocoa-apps-images/content07.png "필요한 콘센트 추가")](modern-cocoa-apps-images/content07.png#lightbox)
5. 변경 내용을 저장합니다.
6. Mac용 Visual Studio로 돌아와서 변경 내용을 동기화 합니다.

를 편집 `NSWindowController` 하 고 다음과 같이 표시 되도록 합니다.

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

이 코드의 핵심 요소는 뷰가 Interface Builder 정의 되 고 **유출**로 노출 되는 사용자 지정 뷰로 설정 되는 위치입니다.

```csharp
accessoryView.View = AccessoryViewGoBar;
```

그리고 액세서리를 표시할 _위치_ 를 정의 하는입니다.`LayoutAttribute`

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

이제 macos는 완전히 지역화 되었으므로 및 `Left` `Right` `NSLayoutAttribute` 속성은 더 이상 사용 되지 않으며 및로 `Leading` `Trailing`바꾸어야 합니다.

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>탭 창 사용

또한 macOS 시스템은 앱의 창에 액세서리 보기 컨트롤러를 추가할 수 있습니다. 예를 들어 여러 앱 창이 하나의 가상 창으로 병합 되는 탭 창을 만들려면 다음을 수행 합니다.

[![](modern-cocoa-apps-images/content08.png "탭 Mac 창의 예")](modern-cocoa-apps-images/content08.png#lightbox)

일반적으로 개발자는 사용자의 Xamarin.ios 앱에서 탭 창을 사용 하 여 제한 된 작업을 수행 해야 합니다. 시스템은 다음과 같이 자동으로 처리 합니다.

- `OrderFront` 메서드가 호출 되 면 창이 자동으로 탭 됩니다.
- `OrderOut` 메서드가 호출 되 면 창이 자동으로 탭 되지 않습니다.
- 코드에서 모든 탭 창은 여전히 "표시" 된 것으로 간주 되지만 CoreGraphics를 사용 하 여 시스템에서 맨 앞에 있지 않은 탭은 숨겨집니다.
- `TabbingIdentifier` 의`NSWindow` 속성을 사용 하 여 창을 탭으로 그룹화 합니다.
- `NSDocument` 기반 앱 인 경우 개발자 작업 없이 이러한 기능 중 일부는 자동으로 사용 하도록 설정 됩니다 (예: 탭 모음에 추가 되는 더하기 단추).
- 기반이 아닌 앱에서는의 `NSWindowsController`메서드를 `GetNewWindowForTab` 재정의 하 여 새 문서를 추가 하는 탭 그룹의 "더하기" 단추를 사용할 수 있습니다.`NSDocument`

모든 부분을 결합 하 여 시스템 기반 `AppDelegate` 탭 창을 사용 하려는 앱의는 다음과 같습니다.

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

여기서 속성 `NewDocumentNumber` 은 생성 된 새 문서 수를 추적 하 고 메서드는 `NewDocument` 새 문서를 만들어 표시 합니다.

는 `NSWindowController` 다음과 같습니다.

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

여기서 static `App` 속성은에 `AppDelegate`대 한 바로 가기를 제공 합니다. 메서드 `SetDefaultDocumentTitle` 는 생성 된 새 문서 수를 기준으로 새 문서 제목을 설정 합니다.

다음 코드는 응용 프로그램에서 탭 사용을 선호 하 고 앱의 창을 탭으로 그룹화 할 수 있도록 하는 문자열을 제공 하도록 macOS에 지시 합니다.

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

다음 재정의 메서드는 사용자가 클릭 했을 때 새 문서를 만들 탭 표시줄에 plus 단추를 추가 합니다.

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>핵심 애니메이션 사용

핵심 애니메이션은 macOS에 내장 된 고성능 그래픽 렌더링 엔진입니다. 핵심 애니메이션은 CPU에서 그래픽 작업을 실행 하는 것이 아니라 최신 macOS 하드웨어에서 사용할 수 있는 GPU (그래픽 처리 장치)를 활용 하도록 최적화 되었습니다 .이로 인해 컴퓨터의 속도가 느려질 수 있습니다.

핵심 `CALayer`애니메이션에서 제공 되는는 고속, 유체 스크롤 및 애니메이션과 같은 작업에 사용할 수 있습니다. 코어 애니메이션을 완벽 하 게 활용 하려면 앱의 사용자 인터페이스를 여러 하위 뷰 및 계층으로 구성 해야 합니다.

개체 `CALayer` 는 개발자가 사용자에 게 화면에 표시 되는 내용을 제어할 수 있는 여러 가지 속성을 제공 합니다.

- `Content`-계층의 콘텐츠 `NSImage` 를 `CGImage` 제공 하는 또는 일 수 있습니다.
- `BackgroundColor`-계층의 배경색을로 설정 합니다.`CGColor`
- `BorderWidth`-테두리 두께를 설정 합니다.
- `BorderColor`-테두리 색을 설정 합니다.

앱의 UI에서 핵심 그래픽을 활용 하려면 _계층 지원_ 뷰를 사용 해야 합니다 .이 뷰는 Apple에서 개발자가 항상 창의 콘텐츠 보기에서 사용 하도록 설정 해야 한다는 것을 제안 합니다. 이러한 방식으로 모든 자식 보기는 계층 백업도 자동으로 상속 합니다.

또한 Apple은 새 `CALayer` 를 하위 계층으로 추가 하는 것과는 반대로 계층 백업 뷰를 사용 하는 것을 제안 합니다 .이는 시스템에서 필요한 설정 (예: 레 티 나 표시에 필요한 설정)을 자동으로 처리 하기 때문입니다.

계층을 지 원하는 작업은 **주요 애니메이션 계층**을 `NSView` 확인 `true` 하 여 **보기 효과 검사자** 아래에서 Xcode의 Interface Builder에 대 한 또는 내부의를 설정 `WantsLayer` 하 여 사용할 수 있습니다.

[![](modern-cocoa-apps-images/content09.png "보기 효과 검사자")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>레이어를 사용 하 여 뷰 다시 그리기

Xamarin.ios 앱에서 계층 기반 뷰를 사용 하는 경우 또 다른 중요 한 단계는 `LayerContentsRedrawPolicy` 에서 `NSView` `NSViewController`의를 `OnSetNeedsDisplay` 로 설정 하는 것입니다. 예를 들어:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

개발자가이 속성을 설정 하지 않으면 해당 프레임 원본이 변경 될 때마다 뷰가 다시 그려져 성능상의 이유로 적절 하지 않습니다. 그러나이 속성을로 `OnSetNeedsDisplay` 설정 하면 개발자가 수동으로를 `true` 로 `NeedsDisplay` 설정 하 여 콘텐츠를 강제로 다시 그려야 합니다.

뷰가 커밋되지 않은 것으로 표시 되 면 시스템은 뷰의 `WantsUpdateLayer` 속성을 확인 합니다. 가 반환 `true`되면메서드가호출 되 고, 그렇지 않으면 뷰의 메서드가호출되어뷰의내용이업데이트됩니다.`DrawRect` `UpdateLayer`

Apple에는 필요할 때 보기 콘텐츠를 업데이트 하기 위한 다음과 같은 제안이 있습니다.

- Apple은 가능한 `UpdateLater` 경우 `DrawRect` 상당한 성능 향상을 제공 하므로를 사용 하는 것이 선호 됩니다.
- 비슷한 모양의 UI `layer.Contents` 요소에 대해 동일한를 사용 합니다.
- 또한 Apple은 가능한 한 언제 든 지와 `NSTextField`같은 표준 뷰를 사용 하 여 UI를 작성 하도록 개발자에 게 선호 합니다.

를 사용 `UpdateLayer`하려면 `NSView` 에 대 한 사용자 지정 클래스를 만들고 코드를 다음과 같이 만듭니다.

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

## <a name="using-modern-drag-and-drop"></a>최신 끌어서 놓기를 사용 하 여

사용자에 게 최신 끌어서 놓기 환경을 제공 하기 위해 개발자는 앱의 끌어서 놓기 작업에서 _끌어서_ 놓기를 채택 해야 합니다. 끌기 flocking는 사용자가 끌기 작업을 계속할 때 처음에 끌어 오는 개별 파일 또는 항목이 각각 flocking의 개별 요소로 표시 됩니다 (항목 수를 포함 하 여 커서 아래에 그룹화).

사용자가 끌기 작업을 종료 하면 개별 요소가 Unflock 원래 위치로 돌아갑니다.

다음 예제 코드에서는 사용자 지정 뷰에서 끌기를 사용할 수 있습니다.

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

의 `BeginDraggingSession` 메서드로`NSView` 끌어 온 각 항목을 배열의 개별 요소로 보내 flocking 효과를 달성 했습니다.

`NSTableView` 또는 `PastboardWriterForRow` `NSTableViewDataSource` 로 작업 하는 경우 클래스의 메서드를 사용 하 여 끌기 작업을 시작 합니다. `NSOutlineView`

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

이를 통해 개발자는 모든 행을 `NSDraggingItem` 단일 그룹으로 모든 행을 기록 하는 이전 메서드와 `WriteRowsWith` 반대로 끌고 있는 테이블의 모든 항목에 대 한 개별를에 게 제공할 수 있습니다.

로 `NSCollectionViews`작업할 때 끌기를 시작할 때 `WriteItemsAt` 메서드와 `PasteboardWriterForItemAt` 달리 메서드를 다시 사용 합니다.

개발자는 항상 많은 파일을 대지의 위에 배치 하지 않아야 합니다. MacOS Sierra를 처음 사용 하는 경우 _파일 약속_ 을 사용 하면 개발자가 새 `NSFilePromiseProvider` 및 `NSFilePromiseReceiver` 클래스를 사용 하 여 삭제 작업을 완료할 때 나중에 충족 될 수 있는 지정 된 파일에 대 한 참조를 나중에 수행할 수 있습니다.

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>최신 이벤트 추적 사용

제목 또는 도구 모음 영역에 추가 된 사용자 `NSButton`인터페이스 요소 (예:)의 경우 사용자는 요소를 클릭 하 여 이벤트를 정상적으로 발생 시킬 수 있어야 합니다 (예: 팝업 창 표시). 그러나 항목은 제목 또는 도구 모음 영역에도 있으므로 사용자는 요소를 클릭 하 고 끌어서 창도 이동할 수 있어야 합니다.

코드에서이를 수행 하려면 요소에 대 한 사용자 지정 클래스 (예 `NSButton`:)를 만들고 다음과 같이 이벤트를 재정의 합니다. `MouseDown`

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

이 코드는 UI `TrackEventsMatching` 요소가 연결 된 `NSWindow` 의 메서드를 사용 하 여 `LeftMouseUp` 및 `LeftMouseDragged` 이벤트를 가로챕니다. `LeftMouseUp` 이벤트의 경우 UI 요소는 정상적으로 응답 합니다. 이벤트의 경우 이벤트를 `NSWindow`의 `PerformWindowDrag` 메서드에 전달 하 여 화면에서 창을 이동 합니다. `LeftMouseDragged`

클래스의 메서드를 `PerformWindowDrag` 호출 하면 다음과 같은 이점이 있습니다. `NSWindow`

- 앱이 중지 된 경우 (예: 심층 루프를 처리 하는 경우)에도 창이 이동 될 수 있습니다.
- 공간 전환은 예상 대로 작동 합니다.
- 공백 막대가 정상적으로 표시 됩니다.
- 창 맞춤 및 맞춤은 정상적으로 작동 합니다.

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>최신 컨테이너 뷰 컨트롤 사용

macOS Sierra는 이전 버전의 OS에서 사용할 수 있는 기존 컨테이너 뷰 컨트롤의 많은 최신 향상 기능을 제공 합니다.

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>테이블 뷰 향상

개발자는 항상와 `NSView` `NSTableView`같은 컨테이너 뷰 컨트롤의 새 기반 버전을 사용 해야 합니다. 예를 들어:

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

이렇게 하면 테이블의 지정 된 행에 사용자 지정 테이블 행 작업을 연결할 수 있습니다 (예: 행을 삭제 하려면 오른쪽으로 살짝 밀기). 이 동작을 사용 하려면 `RowActions` `NSTableViewDelegate`의 메서드를 재정의 합니다.

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

Static `NSTableViewRowAction.FromStyle` 은 다음 스타일의 새 테이블 행 작업을 만드는 데 사용 됩니다.

- `Regular`-행의 내용 편집과 같은 표준 비 소거식 작업을 수행 합니다.
- `Destructive`-테이블에서 행을 삭제 하는 등의 파괴적인 작업을 수행 합니다. 이러한 작업은 빨간색 배경으로 렌더링 됩니다.

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>스크롤 뷰 향상 

스크롤 뷰 (`NSScrollView`)를 직접 사용 하거나 다른 컨트롤의 일부로 사용 하는 경우 ( `NSTableView`예:) 스크롤 뷰의 내용이 최신 모양과 보기를 사용 하 여 xamarin.ios 앱의 제목 및 도구 모음 영역에서 슬라이드를 사용할 수 있습니다.

결과적으로, 스크롤 뷰 콘텐츠 영역의 첫 번째 항목은 제목 및 도구 모음 영역에 의해 부분적으로 가려져 있을 수 있습니다.

이 문제를 해결 하기 위해 Apple은 `NSScrollView` 클래스에 새 속성 두 개를 추가 했습니다.

- `ContentInsets`-개발자가 스크롤 뷰의 위쪽에 `NSEdgeInsets` 적용 될 오프셋을 정의 하는 개체를 제공할 수 있습니다.
- `AutomaticallyAdjustsContentInsets`-스크롤 `true` 뷰가 개발자에 대 한를 `ContentInsets` 자동으로 처리 하는 경우입니다.

를 `ContentInsets` 사용 하 여 개발자는 스크롤 뷰의 시작을 조정 하 여 다음과 같은 액세서리를 포함할 수 있습니다.

- 메일 응용 프로그램에 표시 되는 것과 같은 정렬 표시기입니다.
- 검색 필드입니다.
- 새로 고침 또는 업데이트 단추입니다.

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>최신 앱에서 자동 레이아웃 및 지역화

Apple은 개발자가 국제화 된 macOS 앱을 쉽게 만들 수 있도록 하는 Xcode의 여러 기술을 포함 하 고 있습니다. 이제 개발자는 Xcode를 사용 하 여 사용자가 제공 하는 텍스트를 해당 스토리 보드 파일에 있는 앱의 사용자 인터페이스 디자인과 분리 하 고 UI가 변경 되는 경우이 분리를 유지 하는 도구를 제공 합니다.

자세한 내용은 Apple의 [국제화 및 지역화 가이드](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)를 참조 하세요.

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>기본 국제화 구현 

개발자는 기본 국제화를 구현 하 여 응용 프로그램의 UI를 나타내는 단일 스토리 보드 파일을 제공 하 고 모든 사용자 연결 문자열을 구분할 수 있습니다. 

개발자가 앱의 사용자 인터페이스를 정의 하는 초기 Storyboard 파일 (또는 파일)을 만드는 경우 기본 국제화 (개발자가 말하는 언어)에서 빌드됩니다.

그런 다음 개발자는 여러 언어로 변환할 수 있는 지역화 및 기본 국제화 문자열 (스토리 보드 UI 디자인)을 내보낼 수 있습니다.

나중에 이러한 지역화를 가져올 수 있으며 Xcode는 스토리 보드에 대 한 언어별 문자열 파일을 생성 합니다.

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>지역화를 지원 하기 위한 자동 레이아웃 구현

지역화 된 버전의 문자열 값은 크기 및/또는 읽기 방향이 크게 다를 수 있으므로 개발자는 자동 레이아웃을 사용 하 여 응용 프로그램의 사용자 인터페이스를 스토리 보드 파일에 배치 하 고 크기를 조정 해야 합니다.

Apple에서는 다음을 수행 하는 것이 좋습니다.

- **고정 폭 제약 조건 제거** -모든 텍스트 기반 보기는 내용에 따라 크기를 조정할 수 있어야 합니다. 고정 폭 보기가 특정 언어로 콘텐츠를 자를 수 있습니다.
- **내장 콘텐츠 크기 사용** -기본적으로 텍스트 기반 보기는 내용에 맞게 자동으로 크기가 조정 됩니다. 크기를 올바르게 조정 하지 않는 텍스트 기반 보기의 경우 Xcode의 Interface Builder에서 선택한 다음**내용에 맞게 크기** **편집** > 을 선택 합니다.
- **선행 및 후행 특성 적용** -텍스트의 방향이 사용자의 언어에 따라 변경 될 수 `Leading` 있으므로 기존 `Right` 및 `Left` 와 달리 새 및 `Trailing` 제약 조건 특성을 사용 합니다. 특성. `Leading`및 `Trailing` 는 언어 방향에 따라 자동으로 조정 됩니다.
- **인접 뷰에 뷰 고정** -선택한 언어에 대 한 응답으로 뷰를 변경 하 고 크기를 조정할 수 있습니다.
- **Windows 최소 및/또는 최대 크기를 설정 하지 마세요** . 선택한 언어가 콘텐츠 영역의 크기를 조정 하므로 windows에서 크기를 변경할 수 있습니다.
- **레이아웃 변경 내용을 지속적으로 테스트** -앱에서 개발 하는 동안 다른 언어로 지속적으로 테스트 해야 합니다. 자세한 내용은 Apple의 [국제화 앱 테스트](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1) 설명서를 참조 하세요.
- **Nsstackviews를 사용 하 여 뷰**  -  `NSStackViews` 를 고정 하면 예측 가능한 방식으로 콘텐츠를 이동 하 고 확장할 수 있으며 선택한 언어에 따라 콘텐츠를 변경할 수 있습니다.

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Xcode의 Interface Builder에서 지역화

Apple은 지역화를 지원 하기 위해 응용 프로그램의 UI를 디자인 하거나 편집할 때 개발자가 사용할 수 있는 Xcode의 Interface Builder에 몇 가지 기능을 제공 합니다. 개발자는 **특성 검사자** 의 `NSTextField` **텍스트 방향** 섹션을 사용 하 여 텍스트 기반 보기 선택 (예:)에서 방향을 사용 하 고 업데이트 하는 방법에 대 한 힌트를 제공할 수 있습니다.

[![](modern-cocoa-apps-images/content10.png "텍스트 방향 옵션입니다.")](modern-cocoa-apps-images/content10.png#lightbox)

**텍스트 방향**에는 세 가지 값을 사용할 수 있습니다.

- **자연** -레이아웃은 컨트롤에 할당 된 문자열을 기반으로 합니다.
- **왼쪽에서 오른쪽** 으로 레이아웃은 항상 왼쪽에서 오른쪽으로 강제 적용 됩니다.
- **오른쪽에서 왼쪽** -레이아웃은 항상 오른쪽에서 왼쪽으로 강제 적용 됩니다.

**레이아웃**에 사용할 수 있는 값에는 두 가지가 있습니다.

- **왼쪽에서 오른쪽** 으로 레이아웃은 항상 왼쪽에서 오른쪽입니다.
- **오른쪽에서 왼쪽** -레이아웃은 항상 오른쪽에서 왼쪽입니다.

일반적으로 특정 맞춤이 필요 하지 않으면 이러한 항목을 변경 하면 안 됩니다.

**미러** 속성은 특정 컨트롤 속성 (예: 셀 이미지 위치)을 대칭 이동 하도록 시스템에 지시 합니다. 다음과 같은 세 가지 값을 사용할 수 있습니다.

- **자동** -선택한 언어의 방향에 따라 위치가 자동으로 변경 됩니다.
- **오른쪽에서 왼쪽으로의 인터페이스** -위치는 오른쪽에서 왼쪽으로만 변경 됩니다.
- **Never** -위치가 변경 되지 않습니다.

개발자가 텍스트 기반 뷰의 내용에 대해 **가운데**맞춤, **양쪽** 맞춤 또는 **전체** 맞춤을 지정한 경우 선택한 언어에 따라 대칭 이동 되지 않습니다.

MacOS Sierra 이전에는 코드에서 만든 컨트롤이 자동으로 미러링 되지 않습니다. 개발자는 다음과 같은 코드를 사용 하 여 미러링을 처리 해야 했습니다.

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

컨트롤의을 `ImagePosition` `Alignment` 기반으로및를설정`UserInterfaceLayoutDirection` 하는입니다.

macOS Sierra는 여러 가지 매개 변수 (예: 제목 `CreateButton` , 이미지 및 작업)를 사용 하는 몇 가지 새로운 편의 생성자 (정적 메서드를 통해)를 추가 하 고 올바르게 자동으로 미러링됩니다. 예를 들어:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>시스템 모양 사용

최신 macOS 앱은 이미지 생성, 편집 또는 프레젠테이션 앱에서 잘 작동 하는 새로운 짙은 인터페이스 모양을 채택할 수 있습니다.

[![](modern-cocoa-apps-images/content11.png "어두운 Mac 창 UI의 예")](modern-cocoa-apps-images/content11.png#lightbox)

이 작업은 창이 표시 되기 전에 코드 줄 하나를 추가 하 여 수행할 수 있습니다. 예:

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

클래스의 정적 `GetAppearance` 메서드는 시스템에서 명명 된 모양 (이 경우 `NSAppearance.NameVibrantDark`)을 가져오는 데 사용 됩니다. `NSAppearance`

Apple에는 시스템 모양새 사용에 대 한 다음과 같은 제안이 있습니다.

- 하드 코드 된 값 (예: `LabelColor` 및 `SelectedControlColor`)에 대해 명명 된 색을 선호 합니다.
- 가능 하면 시스템 표준 컨트롤 스타일을 사용 합니다.

시스템 모양새를 사용 하는 macOS 앱은 시스템 기본 설정 앱에서 내게 필요한 옵션 기능을 사용 하도록 설정한 사용자에 대해 자동으로 제대로 작동 합니다. 따라서 Apple은 개발자가 macOS 앱에서 항상 시스템 모양새를 사용 하도록 제안 합니다.

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>스토리 보드를 사용 하 여 Ui 디자인

개발자는 스토리 보드를 사용 하 여 응용 프로그램의 사용자 인터페이스를 구성 하는 개별 요소를 디자인할 뿐만 아니라 UI 흐름과 지정 된 요소의 계층 구조를 시각화 하 고 디자인할 수 있습니다.

개발자는 컨트롤러를 사용 하 여 요소를 컴퍼지션 및 Segue abstract 단위로 수집 하 고, 뷰 계층 구조 전체에서 이동 하는 데 필요한 일반적인 "글 루 코드"를 제거할 수 있습니다.

[![](modern-cocoa-apps-images/content12.png "Xcode의 Interface Builder에서 UI 편집")](modern-cocoa-apps-images/content12.png#lightbox)

자세한 내용은 [Storyboard 소개](~/mac/platform/storyboards/index.md) 설명서를 참조 하세요.

스토리 보드에 정의 된 지정 된 장면이 뷰 계층 구조에 있는 이전 장면의 데이터를 요구 하는 경우가 많습니다. Apple에는 장면 간에 정보를 전달 하기 위한 다음과 같은 제안이 있습니다.

- 데이터 dependancies은 항상 계층 구조를 통해 하위 중첩 되어야 합니다.
- Ui 구조적 dependancies을 사용 하지 마십시오 .이는 UI 유연성이 제한 되기 때문입니다.
- 인터페이스 C# 를 사용 하 여 제네릭 데이터 dependancies을 제공 합니다.

Segue의 소스로 작동 하는 뷰 컨트롤러는 Segue를 실행 하 여 대상 뷰 `PrepareForSegue` 컨트롤러를 표시 하기 전에 메서드를 재정의 하 고 데이터 전달과 같은 모든 초기화 작업을 수행할 수 있습니다. 예를 들어:

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

자세한 내용은 [segue](~/mac/platform/storyboards/indepth.md#Segues) 설명서를 참조 하세요.

<a name="Propagating-Actions" />

## <a name="propagating-actions"></a>작업 전파

MacOS 앱의 디자인에 따라 ui 컨트롤에 대 한 작업에 대 한 최상의 처리기가 UI 계층 구조의 다른 위치에 있을 수 있습니다. 이는 일반적으로 자신의 장면에 상주 하는 메뉴 및 메뉴 항목에 적용 되며, 나머지 앱 UI와는 별개입니다.

이러한 상황을 처리 하기 위해 개발자는 사용자 지정 작업을 만들고 해당 작업을 응답자 체인에 전달할 수 있습니다. 자세한 내용은 [사용자 지정 창 작업](~/mac/user-interface/menu.md) 작업 설명서를 참조 하세요.

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>최신 Mac 기능

Apple은 개발자가 다음과 같은 대부분의 Mac 플랫폼을 만들 수 있도록 하는 macOS Sierra에 몇 가지 사용자 측 기능을 포함 하 고 있습니다.

- **NSUserActivity** -앱에서 사용자가 현재 관련 된 작업을 설명할 수 있습니다. `NSUserActivity`처음에는 사용자의 장치 중 하나에서 시작 된 활동을 선택 하 고 다른 장치에서 계속 진행할 수 있는 전달을 지원 하기 위해 처음으로 만들었습니다. `NSUserActivity`는 iOS에서와 마찬가지로 macOS에서 동일 하 게 작동 하므로 자세한 내용은 ios [소개 문서를](~/ios/platform/handoff.md) 참조 하세요.
- **Mac의 siri** -siri는 현재 활동 (`NSUserActivity`)을 사용 하 여 사용자가 발급할 수 있는 명령에 컨텍스트를 제공 합니다.
- **상태 복원** -사용자가 macos에서 앱을 종료 한 다음 나중에 relaunches 앱이 이전 상태로 자동으로 돌아갑니다. 개발자는 사용자 인터페이스가 사용자에 게 표시 되기 전에 상태 복원 API를 사용 하 여 임시 UI 상태를 인코딩 및 복원할 수 있습니다. 응용 프로그램의 기반이 `NSDocument` 되는 경우 상태 복원이 자동으로 처리 됩니다. `NSDocument` 기반이 아닌 앱에 대해 상태 복원을 사용 하도록 설정 하려면 `NSWindow` 클래스 `Restorable` 의를로 `true`설정 합니다.
- **클라우드의 문서** -macOS Sierra 하기 전에 앱은 사용자의 iCloud 드라이브에서 문서를 사용 하도록 명시적으로 옵트인 (opt in) 해야 했습니다. MacOS Sierra 사용자의 **데스크톱** 및 **문서** 폴더가 시스템에 의해 자동으로 iCloud 드라이브와 동기화 될 수 있습니다. 따라서 문서의 로컬 복사본을 삭제 하 여 사용자 컴퓨터의 공간을 확보할 수 있습니다. `NSDocument`기반 앱은이 변경 내용을 자동으로 처리 합니다. 다른 모든 앱 형식은를 `NSFileCoordinator` 사용 하 여 문서 읽기 및 쓰기를 동기화 해야 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 개발자가 Xamarin.ios에서 최신 macOS 앱을 빌드하는 데 사용할 수 있는 몇 가지 팁, 기능 및 기술에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [macOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
