---
title: 최신 macOS 앱 빌드
description: 이 문서에서는 몇 가지 팁, 기능 및 기술을 개발자 Xamarin.Mac에서 최신 macOS 앱을 빌드하는 데 사용할에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 53bfc9f147b6cf369b8f5ce8d1257dbaf6b0f807
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113523"
---
# <a name="building-modern-macos-apps"></a>최신 macOS 앱 빌드

_이 문서에서는 몇 가지 팁, 기능 및 기술을 개발자 Xamarin.Mac에서 최신 macOS 앱을 빌드하는 데 사용할에 대해 설명 합니다._

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>최신 보기를 사용 하 여 최신 같습니다 구축

최신 모양에는 아래 예제에서는 앱과 같은 최신 창 및 도구 모음 모양을 포함 됩니다.

[![](modern-cocoa-apps-images/content08.png "최신 Mac 앱 UI의 예")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>콘텐츠 뷰 크기 전체를 사용 하도록 설정

Xamarin.Mac 앱에서이 조회를 위해 개발자가 사용할를 _콘텐츠 뷰의 전체 크기_, 도구 및 제목 표시줄 영역 아래에 있는 콘텐츠 확장 의미 및 macOS에서 자동으로 흐리게 표시 될 됩니다.

코드에서이 기능을 사용 하려면 사용자 지정 클래스를 만듭니다는 `NSWindowController` 다음과 같을 수 있도록 합니다.

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

이 기능은 또한 활성화할 수 있습니다 Xcode의 Interface Builder에서 창을 선택 하 고 확인 하 여 **크기의 전체 콘텐츠 보기**:

[![](modern-cocoa-apps-images/content01.png "Xcode의 Interface Builder에서 주 스토리 보드를 편집합니다.")](modern-cocoa-apps-images/content01.png#lightbox)

전체 크기 콘텐츠 보기를 사용 하는 경우 해당 항목 아래에서 특정 콘텐츠 (예: 레이블) 이동 하지 않습니다 있도록 제목 및 도구 모음 영역 아래의 콘텐츠를 오프셋 개발자 해야 합니다.

이 문제가 복잡해 집니다, 제목 및 도구 모음 영역 사용자를 수행 중인 동작을 기준으로 동적 높이 가질 수 있습니다 macOS 사용자가 설치 및/또는 앱에서 실행 되는 Mac 하드웨어의 버전입니다.

결과적으로, 사용자 인터페이스를 배치 하는 경우 오프셋을 단순히 하드 코딩도 작동 하지 않습니다. 개발자는 동적 접근 해야 합니다.

Apple 포함 된를 [Observable 키-값](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes) `ContentLayoutRect` 의 속성을 `NSWindow` 클래스 코드에서 현재 콘텐츠 영역을 가져오려면. 개발자 콘텐츠 영역이 변경 될 때 수동으로 필수 요소를 배치 하려면이 값을 사용할 수 있습니다.

자동 레이아웃 및 Size 클래스는 UI 요소의 위치에 코드 또는 Interface Builder를 사용 하는 것이 좋습니다.

자동 레이아웃 및 Size 클래스를 사용 하 여 앱의 뷰 컨트롤러에서 UI 요소의 위치를 다음과 같이 코드를 사용할 수 있습니다.

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

이 코드는 레이블에 적용 될 상위 제약 조건에 대 한 저장소를 만듭니다 (`ItemTitle`) 제목 및 도구 모음 영역에서 지연 될 하지 확인 합니다.

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

뷰 컨트롤러의 재정의 하 여 `UpdateViewConstraints` 개발자 수 필요한 제약 조건이 이미 빌드된 경우를 확인 하는 테스트 메서드와 필요한 경우 만듭니다.

새 제약 조건을 작성 해야 할 경우는 `ContentLayoutGuide` 제한 해야 하는 컨트롤에 액세스 한 캐스팅 창의 속성을 `NSLayoutGuide`:

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

TopAnchor 속성은 `NSLayoutGuide` 에 액세스 하 고 사용 가능한 경우 원하는 오프셋된 만큼을 사용 하 여 새 제약 조건을 빌드를 사용 하 고 새 제약 조건을 적용할 활성 상태가 됩니다.

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>간소화 된 도구 모음을 사용 하도록 설정

일반 macOS 창 제목 표시줄에서 창의 위쪽 가장자리를를 실행 하는 표준 포함 됩니다. 창의 도구 모음에 포함 된 경우 제목 표시줄 영역이 표시 됩니다.

[![](modern-cocoa-apps-images/content02.png "표준 Mac 도구 모음")](modern-cocoa-apps-images/content02.png#lightbox)

제목 영역 사라집니다 간소화 된 도구 모음을 사용 하 여 제목 표시줄의 위치에 도구 모음을 위로 이동 하는 경우 인라인 창 닫기, 최소화 및 최대화 단추를 사용 하 여:

[![](modern-cocoa-apps-images/content03.png "간소화 된 Mac 도구 모음")](modern-cocoa-apps-images/content03.png#lightbox)

재정의 하 여 간소화 된 도구 모음이 사용 합니다 `ViewWillAppear` 메서드는 `NSViewController` 다음 처럼 보이게 하 고:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

이 효과 일반적으로 _Shoebox 응용 프로그램_ (하나의 창을 앱) 같은 맵, 일정, 노트 및 시스템 기본 설정 합니다. 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>액세서리 뷰 컨트롤러를 사용 하 여

앱의 디자인에 따라 개발자는 상황에 맞는 제어 작업에 따라 사용자에 게는 제공 제목/도구 모음 영역 아래의 오른쪽에 나타나는 액세서리 뷰 컨트롤러를 사용 하 여 영역에는 제목 표시줄을 보완 하기 위해 수도 있습니다. 현재에 참여 합니다.

[![](modern-cocoa-apps-images/content04.png "예로 액세서리 뷰 컨트롤러")](modern-cocoa-apps-images/content04.png#lightbox)

액세서리 뷰 컨트롤러를 자동으로 흐리게 표시 되며 개발자의 개입 없이 시스템에서 크기를 조정 합니다.

액세서리 보기 컨트롤러에 추가 하려면 다음을 수행 합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭하여 엽니다.
2. 끌어서를 **사용자 지정 뷰 컨트롤러** 창의 계층 구조로: 

    [![](modern-cocoa-apps-images/content05.png "새 사용자 지정 뷰 컨트롤러를 추가합니다.")](modern-cocoa-apps-images/content05.png#lightbox)
3. 레이아웃 액세서리 보기의 UI: 

    [![](modern-cocoa-apps-images/content06.png "새 뷰를 디자인합니다.")](modern-cocoa-apps-images/content06.png#lightbox)
4. 액세서리 뷰로 표시는 **출 선** 및 기타 **작업** 또는 **출 선** UI에 대 한 합니다. 

    [![](modern-cocoa-apps-images/content07.png "필요한 출 선 추가")](modern-cocoa-apps-images/content07.png#lightbox)
5. 변경 내용을 저장합니다.
6. 변경 내용을 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

편집을 `NSWindowController` 다음과 같을 수 있도록 합니다.

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

이 코드의 핵심 포인트는 보기에서 Interface Builder에서 정의 되 고으로 노출 된 사용자 지정 보기에 설정 된 위치는 **출 선**:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

및 `LayoutAttribute` 정의 하는 _여기서_ 접근자 표시 됩니다.

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

MacOS 이제 지역화 완벽 하 게 되므로 합니다 `Left` 하 고 `Right` `NSLayoutAttribute` 속성 사용이 중단 된 및로 바꿔야 `Leading` 및 `Trailing`합니다.

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>Windows 탭된을 사용 하 여

또한 macOS 시스템 앱의 창에 액세서리 뷰 컨트롤러를 추가할 수 있습니다. 예를 들어, 하나의 가상 창에 병합은 다양 한 앱의 Windows 탭 Windows를 만들려면:

[![](modern-cocoa-apps-images/content08.png "탭 Mac 창의 예")](modern-cocoa-apps-images/content08.png#lightbox)

일반적으로 개발자는 제한 동작 사용 하 여 Xamarin.Mac 앱의 Windows 탭 되려면 해야, 시스템 다음과 같이 자동으로 처리 됩니다.

- Windows는 자동으로 됩니다 때 탭은 `OrderFront` 메서드가 실행 됩니다.
- Windows는 자동으로 됩니다 때 Untabbed는 `OrderOut` 메서드가 실행 됩니다.
- 그러나 모든 탭 창을 여전히 "표시" 것으로 간주 되는 코드에서 모든 비 앞쪽 탭 CoreGraphics를 사용 하 여 시스템에 의해 숨겨집니다.
- 사용 된 `TabbingIdentifier` 속성의 `NSWindow` 탭으로 함께 Windows 그룹에.
- 경우는 `NSDocument` 이러한 기능의 몇 가지 기반된 앱을 자동으로 (예: 탭 모음에 추가 되 고 더하기 단추) 개발자 작업 없이 사용 됩니다.
- 이외`NSDocument` 기반된 앱에서 새 문서를 재정의 하 여 추가 탭 그룹의 "더하기" 단추를 설정할 수 있습니다. 합니다 `GetNewWindowForTab` 메서드는 `NSWindowsController`합니다.

모든 부분이 함께 가져오는 `AppDelegate` 사용 하고자 하는 앱의 기반 시스템 탭 Windows 다음과 같이 표시 될 수 있습니다.

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

여기서는 `NewDocumentNumber` 속성 만들어지는 새로운 문서의 수가 하는 추적 및 `NewDocument` 메서드는 새 문서를 만들고 표시 합니다.

`NSWindowController` 다음 같을 수 있습니다.

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

정적 `App` 속성에 대 한 바로 가기를 제공 합니다 `AppDelegate`합니다. `SetDefaultDocumentTitle` 메서드를 생성 하는 새 문서 수에 따라 새 문서 제목을 설정 합니다.

다음 코드에서는 탭을 사용 하 여 앱에서 지 원하는 기본 macOS를 지시 하 고 앱의 Windows 탭으로 그룹화 할 수 있도록 하는 문자열을 제공 합니다.

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

및 다음 재정의 메서드는 사용자가 클릭할 때 새 문서를 만든 탭 표시줄에 더하기 단추를 추가 합니다.

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>코어 애니메이션을 사용 하 여

코어 애니메이션은 macOS에 기본 제공 되는 고성능된 그래픽 렌더링 엔진입니다. 코어 애니메이션 GPU (그래픽 처리 장치) 컴퓨터 저하 될 수 있으므로 CPU에서 그래픽 작업을 실행 하는 대신 최신 macOS 하드웨어에서 사용할 수 있는 기능을 활용 하도록 최적화 되었습니다.

`CALayer`, Core Animation 제공한, 빠르고 부드러운 스크롤 애니메이션 등의 작업에 사용할 수 있습니다. 앱의 사용자 인터페이스의 여러 하위 뷰 및 완전히 활용 하기 위해 핵심 애니메이션 구성 되어야 합니다.

`CALayer` 개체 개발자 수 있도록 표시 되는 내용을 제어 하려면 사용자에 게 화면와 같은 여러 속성을 제공 합니다.

- `Content` -수는 `NSImage` 또는 `CGImage` 계층의 내용을 제공 합니다.
- `BackgroundColor` --으로 계층의 배경색을 설정 하는 중에 `CGColor`
- `BorderWidth` -테두리 두께 설정합니다.
- `BorderColor` -테두리 색을 설정합니다.

앱 UI의 핵심 그래픽을 사용 하려면이 사용 해야 합니다 _계층 지원_ Apple 개발자는 항상 사용할 수 있도록 창의 콘텐츠 보기에서 제안 하는 보기입니다. 이 이렇게 하면 모든 자식 뷰는 자동으로 상속 계층 백업도 합니다.

또한 Apple 제안 계층 지원 뷰를 사용 하 여 새 추가 달리 `CALayer` 는 하위 계층으로 시스템은 자동으로 처리 (예: 레 티 나 디스플레이에 필요한 것) 필수 설정을 여러 때문에 합니다.

지 원하는 계층을 설정 하 여 사용할 수 있습니다는 `WantsLayer` 의 `NSView` 에 `true` 또는 Xcode의 Interface Builder에서 내부 합니다 **보기 효과 검사기** 확인 하 여 **Core 애니메이션 계층**:

[![](modern-cocoa-apps-images/content09.png "보기 효과 검사기")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>계층을 사용 하 여 뷰를 다시 그리기

계층 지원 뷰를 사용 하 여 Xamarin.Mac 앱에서 설정 하는 경우 또 다른 중요 한 단계는 `LayerContentsRedrawPolicy` 의 `NSView` 에 `OnSetNeedsDisplay` 에 `NSViewController`합니다. 예를 들어:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

프레임 원점이 변경 될 때마다 뷰를 다시 그릴 개발자가이 속성을 설정 하지 않으면 성능상의 이유로 원하지 않는 것입니다. 그러나이 속성 설정 하 여 `OnSetNeedsDisplay` 개발자 수동으로 설정 해야 합니다 `NeedsDisplay` 에 `true` 강제로 다시 그리게 합니다 콘텐츠.

뷰를 더티로 표시 되 면 시스템 검사를 `WantsUpdateLayer` 보기의 속성입니다. 반환 하는 경우 `true` 해당 `UpdateLayer` 그렇지 않으면 메서드가 호출 되는 `DrawRect` 뷰의 메서드는 보기의 내용을 업데이트 하려면.

Apple에 필요한 경우에 뷰 콘텐츠를 업데이트 하기 위한 다음 제안에 있습니다.

- 사용 하 여 Apple 선호 `UpdateLater` 를 통해 `DrawRect` 가능한 것으로 상당한 성능 향상을 제공 하는 때마다 합니다.
- 사용한 것과 동일한 `layer.Contents` 비슷한 UI 요소에 대 한 합니다.
- Apple에도 같은 표준 보기를 사용 하 여 해당 UI를 작성 하려면 개발자 선호 `NSTextField`, 다시 가능 합니다.

사용 하도록 `UpdateLayer`에 대 한 사용자 지정 클래스를 만듭니다는 `NSView` 다음과 같이 코드를 확인:

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

## <a name="using-modern-drag-and-drop"></a>최신 끌어서 놓기 사용

개발자 채택 해야 하는 사용자에 대 한 최신 끌어서 놓기 환경을 제공 합니다 _끌어서 떼지_ 해당 앱의 끌어서 놓기 작업 합니다. 끌어서 Flocking는 각 개별 파일 또는 처음 끌고 있는 항목 (항목의 개수를 사용 하 여 커서 아래에 함께 그룹)를 사용자 끌기 작업을 계속 flocks는 개별 요소로 나타납니다.

끌기 작업으로 종료 되 면 사용자의 개별 요소를 Unflock 원래 위치로 반환 합니다.

다음 예제 코드를 사용 하면 사용자 지정 보기에서 끌어서 떼지:

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

떼지 효과를 끌고 있는 각 항목을 전송 하 여 달성 된 합니다 `BeginDraggingSession` 메서드를 `NSView` 배열에 별도 요소로 합니다.

작업할 때를 `NSTableView` 또는 `NSOutlineView`를 사용 합니다 `PastboardWriterForRow` 메서드의 `NSTableViewDataSource` 끌기 작업을 시작 하는 클래스:

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

이렇게 하면 개발자는 개별 `NSDraggingItem` 이전 메서드 대신 끌고 있는 테이블의 모든 항목에 대 한 `WriteRowsWith` 는 쓸 행의 모든 단일 그룹으로 지 합니다.

작업할 때 `NSCollectionViews`를 다시 사용 하 여는 `PasteboardWriterForItemAt` 메서드가 아닌는 `WriteItemsAt` 끌어올 때 메서드를 시작 합니다.

개발자는 임시 보드에 큰 파일을 배치 하지 항상 않아야 합니다. MacOS Sierra 접하는 _파일 프라미스_ 개발자 참조를 배치할 수 있도록 지정 된 사용자는 새 놓기 작업을 완료 하는 경우 나중에 수행 됩니다는 지에 있는 파일을 `NSFilePromiseProvider` 및 `NSFilePromiseReceiver` 클래스입니다.

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>최신 이벤트 추적을 사용 하 여

사용자 인터페이스 요소에 대 한 (같은 `NSButton`) 추가 된 제목 또는 도구 모음 영역을 사용자가 요소를 클릭 하 고 (예: 팝업 창을 표시) 일반적인 방법으로 이벤트를 발생 시 키도 있을 수 있어야 합니다. 그러나 항목 Title 또는 도구 모음 영역 이기도 하므로 사용자는 클릭 하 고도 창을 이동 하는 요소를 끌어 수 있어야 합니다.

이를 위해 코드에서 요소에 대 한 사용자 지정 클래스를 만듭니다 (같은 `NSButton`) 하 고 재정의 `MouseDown` 다음과 같은 이벤트:

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

이 코드를 사용 합니다 `TrackEventsMatching` 메서드를 `NSWindow` UI 요소를 연결 된를 `LeftMouseUp` 및 `LeftMouseDragged` 이벤트. 에 대 한는 `LeftMouseUp` UI 요소에 이벤트를 정상적으로 응답 합니다. 에 대 한 합니다 `LeftMouseDragged` 이벤트를 이벤트에 전달 되는 `NSWindow`의 `PerformWindowDrag` 화면의 창으로 이동 하는 방법.

호출 된 `PerformWindowDrag` 메서드는 `NSWindow` 클래스는 다음과 같은 이점을 제공:

- 앱이 중단 된 경우에 이동 창을 수 있습니다 (경우와 같이 전체 루프 처리).
- 공간 전환 예상 대로 작동 합니다.
- 공간 표시줄 정상적으로 표시 됩니다.
- 창 맞추기 및 맞춤 정상적으로 작동 합니다.

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>최신 컨테이너 보기 컨트롤을 사용 하 여

macOS Sierra OS의 이전 버전에서 사용할 수 있는 기존 컨테이너 보기 컨트롤에 많은 향상 된 최신 기능을 제공합니다.

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>테이블 보기 향상 된 기능

개발자는 항상 사용 하 여 새 `NSView` 컨테이너 보기 컨트롤의 버전을 같은 기반 `NSTableView`입니다. 예를 들어:

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

사용자 지정 테이블 행 동작 지정 (예: 행을 삭제 하는 오른쪽에서 살짝) 테이블의 행에 연결할 수 있습니다. 이 동작을 사용 하도록 설정 하려면 재정의 `RowActions` 메서드는 `NSTableViewDelegate`:

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
- `Destructive` -작업을 수행 파괴적 삭제 같은 행 테이블에서. 이러한 작업은 배경이 빨간색으로 렌더링 됩니다.

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>향상 된 스크롤 뷰 기능 

스크롤 뷰를 사용 하는 경우 (`NSScrollView`)를 직접 또는 다른 컨트롤의 일부로 (같은 `NSTableView`), 스크롤 보기의 콘텐츠는 최신 모양과 뷰를 사용 하 여 Xamarin.Mac 앱에서 제목 및 도구 모음 영역 아래에 있는 슬라이드 할 수 있습니다.

결과적으로, 스크롤 뷰 콘텐츠 영역의 첫 번째 항목 제목 및 도구 모음 영역에서 부분적으로 가려질 수 있습니다.

Apple이이 문제를 해결 하려면 두 개의 새 속성을이 추가 된 `NSScrollView` 클래스:

- `ContentInsets` -개발자가 제공할 수 있습니다.는 `NSEdgeInsets` 스크롤 보기의 맨 위에 적용 될 오프셋을 정의 하는 개체입니다.
- `AutomaticallyAdjustsContentInsets` - `true` 스크롤 뷰는 자동으로 처리 된 `ContentInsets` 개발자에 대 한 합니다.

사용 하 여는 `ContentInsets` 개발자와 같은 보조 프로그램의 포함을 허용 하는 데 스크롤 보기 시작을 조정할 수 있습니다.

- 정렬 표시기를 메일 앱에서 표시 된 것을 선호 합니다.
- 검색 필드입니다.
- 새로 고침 또는 업데이트 단추입니다.

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>자동 레이아웃 및 최신 앱에서 지역화

Apple는 개발자가 쉽게 국제화 된 macOS 앱을 만들 수 있도록 Xcode에서 여러 가지 기술을 포함 된 합니다. 이제 Xcode 개발자가 앱의 사용자 인터페이스 디자인 해당 스토리 보드 파일에서 사용자 쪽 텍스트를 별도 수 있습니다 하 고 UI가 변경 될 경우 이러한 구분을 유지 하기 위해 도구를 제공 합니다.

자세한 내용은 Apple의를 참조 하세요 [국제화 및 지역화 가이드](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)합니다.

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>기본 국제화를 구현합니다. 

개발자는 기본 국제화를 구현 하 여 앱의 UI를 나타내는 모든 사용자 연결 문자열을 구분 하는 단일 스토리 보드 파일을 제공할 수 있습니다. 

초기 스토리 보드 파일 (또는 파일) 개발자 만드는 경우 앱의 사용자 인터페이스를 정의 하는, 기본 국제화 (개발자 강연 하는 언어)에서 빌드됩니다.

다음으로, 지역화 및 국제화 기본 문자열 (스토리 보드 UI 디자인)을 여러 언어로 번역할 수 있는 개발자 내보낼 수 있습니다.

나중에 이러한 지역화를 가져올 수 있습니다 하 고 Xcode에서 스토리 보드에 대 한 언어별 문자열 파일을 생성 합니다.

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>지역화를 지원 하기 위해 자동 레이아웃 구현

지역화 된 버전 때문에 값 문자열의 훨씬 다양 한 크기를 가질 수 있습니다 및/또는 읽기 방향, 개발자는 데 자동 레이아웃을 배치 하 고 앱의 사용자 인터페이스에서 스토리 보드 파일 크기입니다.

Apple 다음을 수행 하는 것이 좋습니다.

- **고정 너비 제약 조건을 제거** -모든 텍스트 기반 뷰에서 해당 콘텐츠를 기반으로 하는 크기 조정 되도록 허용 되어야 합니다. 고정 폭 보기 특정 언어로 콘텐츠를 자를 수 있습니다.
- **기본 콘텐츠 크기를 사용 하 여** -기본 텍스트를 기반으로 뷰는 자동으로 크기가 해당 내용에 맞게 합니다. 올바르게 크기 조정 되는 텍스트 기반 보기에 대 한 Xcode의 Interface Builder에서 선택할 선택한 **편집할** > **크기에 맞게 콘텐츠**합니다.
- **선행 및 후행 특성 적용** 텍스트의 방향을 변경 될 수 있으므로-사용자의 언어에 따라 사용 하 여 새 `Leading` 하 고 `Trailing` 달리 기존 제약 조건 특성 `Right` 및 `Left` 특성입니다. `Leading` 및 `Trailing` 언어 방향에 따라 자동으로 조정 됩니다.
- **Pin 뷰를 인접 한 뷰에** -보기의 위치 및 선택한 언어에 대 한 응답에서 이들을 뷰 변경 크기를 사용 하면이 있습니다.
- **Windows 최소 및/또는 최대 크기를 설정 하지 않은** -선택한 언어에 해당 콘텐츠 영역 크기 조정으로 크기를 변경 하려면 Windows를 허용 합니다.
- **레이아웃 변경 내용을 계속 테스트** 동안 개발 앱에서 다른 언어에서 지속적으로 테스트 해야 합니다. Apple의를 참조 하세요 [Your 다국어 테스트 앱](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1) 자세한 세부 정보에 대 한 설명서입니다.
- **NSStackViews 뷰 함께 Pin을 사용 하 여**  -  `NSStackViews` 해당 콘텐츠를 이동 하 고 예측 가능한 방식으로 증가 하 고 콘텐츠 선택한 언어에 따라 크기를 변경할 수 있습니다.

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Xcode의 Interface Builder에서 지역화합니다.

Apple는 개발자 디자인 하거나 앱의 UI를 편집할 때 지역화를 지원 하는 데 사용할 수 있는 Xcode의 Interface Builder의 몇 가지 기능 제공 했습니다. **텍스트 방향** 섹션을 **특성 검사기** 개발자가 방향 해야 사용 및 텍스트 기반 뷰에서 select에서 업데이트 하는 방법에 힌트를 제공할 수 있습니다 (같은 `NSTextField`):

[![](modern-cocoa-apps-images/content10.png "텍스트 방향 옵션")](modern-cocoa-apps-images/content10.png#lightbox)

세 가지 가능한 값에 대 한 합니다 **텍스트 방향**:

- **자연 스러운** -레이아웃은 컨트롤에 할당 된 문자열에 기반 합니다.
- **왼쪽에서 오른쪽** -레이아웃 항상 왼쪽에서 오른쪽으로 강제 적용 합니다.
- **오른쪽에서 왼쪽으로** -오른쪽에서 왼쪽으로 레이아웃 항상 강제 적용 합니다.

에 대 한 가능한 두 값을 가지는 **레이아웃**:

- **왼쪽에서 오른쪽** -레이아웃 항상 왼쪽에서 오른쪽입니다.
- **오른쪽에서 왼쪽으로** -오른쪽에서 왼쪽으로 레이아웃은 항상 있습니다.

일반적으로 이러한 변경할 수 없습니다 특정 맞춤을 필요한 경우를 제외 합니다.

합니다 **미러** 속성이 특정 컨트롤 속성 (예: 셀 이미지의 위치) 대칭 이동 하려면 시스템에 알려줍니다. 세 가지 가능한 값을 포함 합니다.

- **자동으로** -위치가 자동으로 선택 된 언어의 방향에 따라 변경 됩니다.
- **오른쪽부터 왼쪽 인터페이스에서** -위치 왼쪽 기반 언어는 오른쪽에만 변경 됩니다.
- **되지** -위치는 변경 되지 것입니다.

개발자가 지정 하는 경우 **Center**를 **양쪽 맞춤** 하거나 **전체** 텍스트 기반 보기의 내용에 맞춤, 이러한 되지 대칭 이동 된 기준된으로 언어 선택 합니다.

MacOS Sierra 이전 코드에서 만든 컨트롤은 미러링할 수 없습니다 자동으로. 개발자는 다음과 같은 코드를 사용 하 여 미러링 처리 해야 했습니다.

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

여기서는 `Alignment` 및 `ImagePosition` 에 따라 설정 되는 `UserInterfaceLayoutDirection` 컨트롤의 합니다.

몇 가지 새 편의 생성자를 추가 하는 macOS Sierra (정적 통해 `CreateButton` 메서드) (예: 제목, 이미지 및 작업) 여러 매개 변수를 사용 하 고 자동으로 올바르게 반영 합니다. 예를 들어:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>시스템 모양을 사용 하 여

최신 macOS 앱 이미지 만들기, 편집 또는 프레젠테이션 응용 프로그램에 대해 제대로 작동 하는 새로운 어두운 인터페이스 모양을 채택할 수 있습니다.

[![](modern-cocoa-apps-images/content11.png "어두운 Mac 창 UI의 예")](modern-cocoa-apps-images/content11.png#lightbox)

이 창이 표시 되기 전에 한 줄의 코드를 추가 하 여 수행할 수 있습니다. 예를 들어:

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

정적 `GetAppearance` 메서드를 `NSAppearance` 클래스는 시스템에서 명명 된 모양을 가져오는 데 사용 됩니다 (이 경우 `NSAppearance.NameVibrantDark`).

Apple는 시스템 모양을 사용 하기 위한 다음 제안에 있습니다.

- 하드 코드 된 값을 사용 하 여 명명 된 색 선호할 (같은 `LabelColor` 고 `SelectedControlColor`).
- 가능한 경우 시스템 표준 컨트롤 스타일을 사용 합니다.

시스템 기본 설정 앱에서 내게 필요한 옵션 기능 사용 하도록 설정 하는 사용자에 대 한 자동으로 시스템 모양을 사용 하는 macOS 앱을 올바르게 작동 합니다. 결과적으로, Apple 개발자가 macOS 앱의 시스템 모양을 사용 해야 항상 제안 합니다.

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>스토리 보드를 사용 하 여 Ui를 디자인합니다.

스토리 보드를 사용 하면 개발자 뿐 아니라 앱의 사용자 인터페이스를 구성 하지만 시각화 하 고 UI를 디자인 하는 개별 요소를 배치 하는 디자인 및 지정 된 요소의 계층 구조입니다.

컨트롤러 개발자 컴퍼지션 및 고 Segues 추상 단위로 요소를 수집 하는 일반적인 "글 루 코드" 뷰 계층 구조 전체에서 이동 하는 데 필요한 제거를 허용 합니다.

[![](modern-cocoa-apps-images/content12.png "Xcode의 Interface Builder에서 UI를 편집합니다.")](modern-cocoa-apps-images/content12.png#lightbox)

자세한 내용은 참조 하십시오 우리의 [스토리 보드 소개](~/mac/platform/storyboards/index.md) 설명서.

스토리 보드에 정의 된 지정 된 장면 뷰 계층 구조에서 이전 장면에서 데이터를 해야 하는 여러 인스턴스가 있습니다. Apple 장면 사이의 정보를 전달 하기 위한 다음 제안에 있습니다.

- 계층 구조를 통해 데이터 dependancies 아래로 연계 항상 해야 합니다.
- 이 인해 UI 유연성 제한으로 하드 코딩 UI 구조 dependancies 하지 마세요.
- 사용 하 여 C# 인터페이스를 제네릭 데이터 dependancies를 제공 합니다.

Segue 원본으로 역할을 하는 뷰 컨트롤러를 재정의할 수는 `PrepareForSegue` 메서드와 초기화 해야 (예: 데이터를 전달) Segue는 수행 대상 뷰 컨트롤러를 표시 하려면 실행 됩니다. 예를 들어:

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

자세한 내용은 참조 하십시오 우리의 [고 Segues](~/mac/platform/storyboards/indepth.md#Segues) 설명서.

<a name="Propagating-Actions" />

## <a name="propagating-actions"></a>작업 전파

MacOS 앱의 디자인에 따라 경우가 있을 때 UI 컨트롤에 대 한 작업에 대 한 최상의 처리기에서 다른 곳에 있을 UI 계층 구조입니다. 메뉴 및 메뉴 항목 앱 UI의 나머지 부분에서 별도 자체 장면에서 작동 하는 경우 일반적으로 그렇습니다.

이 상황을 처리 하려면 개발자는 사용자 지정 작업을 만들 하 고 응답자 체인 동작을 전달할 수 있습니다. 자세한 내용은 참조 하십시오 우리의 [사용자 지정 창 작업을 사용 하 여 작업](~/mac/user-interface/menu.md) 설명서.

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>최신 Mac 기능

Apple는 개발자와 같은 Mac 플랫폼의 가장을 허용 하는 macOS Sierra에에서 여러 사용자 용 기능을 포함 했습니다.

- **NSUserActivity** -이렇게 하면 응용 프로그램에서 현재 사용자에 관련 된 활동에 설명 합니다. `NSUserActivity` 핸드 오프를 사용자의 장치 중 하나에서 시작 된 활동 위치 선택 및 다른 장치에서 계속 수를 지원 하기 위해 처음에 만들어졌습니다. `NSUserActivity` iOS에서 수행 되는 macOS에서 동일 works 참조 하세요 우리의 [핸드 오프 소개](~/ios/platform/handoff.md) 대 한 자세한 내용은 iOS 설명서.
- **Mac에서 Siri** -Siri 현재 활동을 사용 하 여 (`NSUserActivity`) 사용자가 실행할 수 있습니다 명령에는 컨텍스트를 제공 합니다.
- **복원 상태** -나중 및 macos 앱이 시작 되 고, 앱은 자동으로 종료 되는 사용자를 이전 상태로 반환 됩니다. 개발자 상태 복원 API를 사용 하 여 인코딩하고 사용자 인터페이스를 사용자에 게 표시 되기 전에 임시 UI 상태를 복원할 수 있습니다. 앱이 없으면 `NSDocument` 기반, 상태 복원이 자동으로 처리 됩니다. 에 대 한 복원 상태를 사용 하도록 설정 하려면 이외`NSDocument` 기반된 앱을 설정 합니다 `Restorable` 의 `NSWindow` 클래스를 `true`입니다.
- **클라우드의 문서** -macOS Sierra 하기 전에 앱에 명시적으로 사용자의 iCloud 드라이브에서에서 문서를 사용 하 여 작업을 했습니다. MacOS Sierra에서 사용자의 **바탕 화면** 하 고 **문서** 폴더도 시스템에서 자동으로 해당 iCloud 드라이브를 사용 하 여 동기화 할 수 있습니다. 결과적으로, 사용자의 컴퓨터에서 공간을 확보 문서의 로컬 복사본을 삭제할 수 있습니다. `NSDocument` 기반된 앱이이 변경을 자동으로 처리 합니다. 다른 모든 앱 유형을 사용 해야 합니다는 `NSFileCoordinator` 읽기 및 문서 쓰기를 동기화 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 몇 가지 팁, 기능 및 기술을 개발자 Xamarin.Mac에서 최신 macOS 앱을 빌드하는 데 사용할 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [macOS 샘플](https://developer.xamarin.com/samples/mac/)
