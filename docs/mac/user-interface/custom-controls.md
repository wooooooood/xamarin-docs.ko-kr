---
title: Xamarin.Mac의 사용자 지정 컨트롤 만들기
description: 이 문서에서는 Xamarin.Mac의 사용자 지정 컨트롤을 빌드하는 방법을 설명 합니다. 사용자 지정 컨트롤을 빌드, 해당 상태를 추적, 해당 인터페이스를 그리는, 사용자 입력에 응답 및 응용 프로그램에서 컨트롤을 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 015c1e315b6070777542a8f8c5871c00cf336b5c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61236197"
---
# <a name="creating-custom-controls-in-xamarinmac"></a>Xamarin.Mac의 사용자 지정 컨트롤 만들기

작업할 때 C# 에 액세스할 수 있는 Xamarin.Mac 응용 프로그램에서.NET 및 동일한 사용자 컨트롤에서 작업을 수행할 *Objective-c*를 *Swift* 및 *Xcode*않습니다. Xcode의 Xamarin.Mac이 Xcode와 직접 통합 되므로 사용할 수 있습니다 _Interface Builder_ 만들기 및 사용자 컨트롤을 유지 관리 (또는 필요에 따라 C# 코드에서 바로 작성).

MacOS에서 다양 한 기본 제공 사용자 정의 컨트롤을 제공 하지만 기본 제공 지원 되지 않는 기능을 제공 하거나 (예: 게임 인터페이스) 사용자 지정 UI 테마에 맞게 사용자 지정 컨트롤을 만드는 해야 할 수 있습니다.

[![](custom-controls-images/intro01.png "사용자 지정 UI 컨트롤의 예")](custom-controls-images/intro01.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 다시 사용할 수 있는 사용자 지정 사용자 인터페이스 컨트롤을 만드는 기본 사항을 설명 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서를 사용 하는 기술을 설명 합니다.

확인 하려는 경우는 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md) 섹션을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 설명도 문서를 `Register` 및 `Export` 명령 하 여 통신-C# 클래스 Objective-c 개체 및 UI 요소에 사용 합니다.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>사용자 지정 컨트롤 소개

위에서 설명한 대로 재사용, Xamarin.Mac 앱의 UI에 대 한 고유 기능을 제공 하거나 (예: 게임 인터페이스) 사용자 지정 UI 테마를 만들려면 사용자 지정 사용자 인터페이스 컨트롤을 만들려는 경우가 있을 수 있습니다.

이러한 상황에서 쉽게에서 상속할 수 있습니다 `NSControl` 만들고 사용자 지정 도구는 C# 코드를 통해 또는 Xcode의 Interface Builder를 통해 앱의 UI를 추가할 수 있습니다. 상속 하 여 `NSControl` 사용자 지정 컨트롤이 자동으로 모든 적용 됩니다. 기본 제공 사용자 인터페이스 컨트롤에는 표준 기능 (같은 `NSButton`).

상속 하려는 사용자 지정 사용자 인터페이스 컨트롤 정보 (예: 사용자 지정 차트를 그래픽 도구)만 표시 하는 경우 `NSView` 대신 `NSControl`합니다.

기본 클래스를 사용 하 든 사용자 지정 컨트롤을 만드는 기본 단계는 동일 합니다.

이 문서는 고유한 사용자 인터페이스 테마 및 모든 기능을 갖춘 사용자 지정 사용자 인터페이스 컨트롤을 작성 하는 예제를 제공 하는 사용자 지정 대칭 스위치 구성 요소를 만듭니다.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>사용자 지정 컨트롤 작성

상속 하려고 만든 사용자 지정 컨트롤이 사용자 입력 (마우스 왼쪽된 단추 클릭)에 응답 합니다, 이후 `NSControl`합니다. 이러한 방식으로 사용자 지정 컨트롤 자동으로 기본 제공 사용자 인터페이스 컨트롤에는 표준 기능의 모든를 표준 macOS 컨트롤 같이 응답 합니다.

Mac 용 Visual Studio의 Xamarin.Mac 프로젝트에 대 한 사용자 지정 사용자 인터페이스 컨트롤을 만드는 (또는 새로 만들기)를 엽니다. 새 클래스를 추가 하 고 호출할 `NSFlipSwitch`:

[![](custom-controls-images/custom01.png "새 클래스 추가")](custom-controls-images/custom01.png#lightbox)

그런 다음 편집을 `NSFlipSwitch.cs` 클래스 및 다음과 비슷하게 표시:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using AppKit;
using CoreGraphics;

namespace MacCustomControl
{
    [Register("NSFlipSwitch")]
    public class NSFlipSwitch : NSControl
    {
        #region Private Variables
        private bool _value = false;
        #endregion

        #region Computed Properties
        public bool Value {
            get { return _value; }
            set {
                // Save value and force a redraw
                _value = value;
                NeedsDisplay = true;
            }
        }
        #endregion

        #region Constructors
        public NSFlipSwitch ()
        {
            // Init
            Initialize();
        }

        public NSFlipSwitch (IntPtr handle) : base (handle)
        {
            // Init
            Initialize();
        }

        [Export ("initWithFrame:")]
        public NSFlipSwitch (CGRect frameRect) : base(frameRect) {
            // Init
            Initialize();
        }

        private void Initialize() {
            this.WantsLayer = true;
            this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
        }
        #endregion

        #region Draw Methods
        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Use Core Graphic routines to draw our UI
            ...

        }
        #endregion

        #region Private Methods
        private void FlipSwitchState() {
            // Update state
            Value = !Value;
        }
        #endregion

    }
}
```

먼저 사용자 지정 클래스에서 상속 하는 것에 주목 `NSControl` 하 고 사용 하 여 **등록** Objective-c와 Xcode의 Interface Builder를이 클래스를 노출 하는 명령:

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

다음 섹션에서는 나머지 위의 코드를 자세하게 살펴보겠습니다를 알아보겠습니다.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>컨트롤의 상태를 추적합니다.

사용자 지정 컨트롤에는 스위치 이므로 스위치의 On/Off 상태를 추적 하는 방법이 필요 합니다. 다음 코드를 사용 하 여 처리 하는 `NSFlipSwitch`:

```csharp
private bool _value = false;
...

public bool Value {
    get { return _value; }
    set {
        // Save value and force a redraw
        _value = value;
        NeedsDisplay = true;
    }
}
```

스위치의 상태 변경 되 면 UI 업데이트 방법이 필요 합니다. 사용 하 여 UI를 다시 그릴 컨트롤을 강제 적용 하 여 할까요 `NeedsDisplay = true`합니다.

제어권 필요한 경우는 단일 설정/해제 상태가 (예: 3 개 위치를 사용 하 여 다중 상태 전환) 사용 하는 **Enum** 상태를 추적 하도록 합니다. 예를 들어 간단한 **bool** 수행 됩니다.

설정 및 해제를 전환 상태의 교환 위해 도우미 메서드를 추가 했습니다.

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

나중에 스위치 상태가 변경 되 면 호출자에 게 알리기 위해이 도우미 클래스를 확장할 것입니다.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>컨트롤의 인터페이스를 그리기

런타임 시 사용자 지정 컨트롤의 사용자 인터페이스를 그리는 핵심 그래픽 그리기 루틴을 사용 하려고 합니다. 이 실행 하기 전에 컨트롤에 대 한 계층에서 설정 해야 합니다. 다음 전용 메서드를 사용 하 여이 작업을 수행 합니다.

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

이 메서드는 각 컨트롤을 올바르게 구성 되어 있는지 확인 하기 위해 컨트롤의 생성자에서 호출 됩니다. 예를 들어:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

다음으로 재정의 해야 합니다 `DrawRect` 메서드 추가 컨트롤을 그리는 핵심 그래픽 루틴 및:

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

에서는 됩니다 수 조정 컨트롤의 시각적 상태로 변경 되 면 (에서 이동 하는 등 **에** 하 **해제**). 언제 든 지 상태 변경, 사용할 수 있습니다는 `NeedsDisplay = true` 새 상태에 대 한 다시 그리도록 컨트롤에 적용할 명령.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>사용자 입력에 응답

사용자 입력 사용자 지정 컨트롤을 추가할 수 있는 두 가지 기본 방법이 있습니다. **마우스 처리 루틴을 재정의** 나 **제스처 인식기**합니다. 를 사용 하는 방법은 제어에 필요한 기능에 따라 달라 집니다.

> [!IMPORTANT]
> 만든 모든 사용자 지정 컨트롤을 사용 해야 하거나 **메서드 재정의** _또는_ **제스처 인식기**, 하지만 둘 다 동시 시간을 서로 충돌할 수 있습니다.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>재정의 메서드를 사용 하 여 사용자 입력 처리

상속 된 개체 `NSControl` (또는 `NSView`) 여러 입력 키보드 또는 마우스 처리에 대 한 메서드를 재정의 합니다. 이 예제에서는 컨트롤에 대 한 상태 간 전환을 대칭 이동 하고자 **에** 및 **해제** 사용자가 왼쪽된 마우스 단추를 사용 하 여 컨트롤을 클릭할 때입니다. 다음 메서드를 재정의 추가할 수 있습니다는 `NSFlipSwitch` 이 처리 하는 클래스:

```csharp
#region Mouse Handling Methods
// --------------------------------------------------------------------------------
// Handle mouse with Override Methods.
// NOTE: Use either this method or Gesture Recognizers, NOT both!
// --------------------------------------------------------------------------------
public override void MouseDown (NSEvent theEvent)
{
    base.MouseDown (theEvent);

    FlipSwitchState ();
}

public override void MouseDragged (NSEvent theEvent)
{
    base.MouseDragged (theEvent);
}

public override void MouseUp (NSEvent theEvent)
{
    base.MouseUp (theEvent);
}

public override void MouseMoved (NSEvent theEvent)
{
    base.MouseMoved (theEvent);
}
## endregion
```

위의 코드에서 호출 합니다 `FlipSwitchState` (위에 정의 된) On 대칭 이동 /에 스위치의 상태를 해제 하는 메서드를 `MouseDown` 메서드. 이렇게 하면 현재 상태를 반영 하도록 다시 그릴 컨트롤을 강제로 수도 있습니다.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>제스처 인식기를 사용 하 여 사용자 입력 처리

필요에 따라 사용자 컨트롤과 상호 작용을 처리 하도록 제스처 인식기를 사용할 수 있습니다. 위에서 추가한 재정의 제거, 편집을 `Initialize` 메서드 수 있으며 다음과 같은 모양:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;

    // --------------------------------------------------------------------------------
    // Handle mouse with Gesture Recognizers.
    // NOTE: Use either this method or the Override Methods, NOT both!
    // --------------------------------------------------------------------------------
    var click = new NSClickGestureRecognizer (() => {
        FlipSwitchState();
    });
    AddGestureRecognizer (click);
}
```

여기에서 만든 새 `NSClickGestureRecognizer` 를 호출 하 고 우리의 `FlipSwitchState` 에서 마우스 왼쪽된 단추를 클릭할 때 스위치의 상태를 변경 하는 방법입니다. `AddGestureRecognizer (click)` 메서드 제스처 인식기를 컨트롤에 추가 합니다.

다시 사용 하는 방법을 사용자 지정 컨트롤을 사용 하 여 수행 하려는 항목에 따라 달라 집니다. 낮은 수준 액세스 해야 하는 경우는 사용자 상호 작용 하려면 재정의 메서드를 사용 합니다. 미리 정의 된 기능을 해야 하는 경우의 마우스 클릭 같은 제스처 인식기를 사용 합니다.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>상태 변경 이벤트에 응답

코드에서 상태 변경에 응답 하는 방법을 해야 사용자가 사용자 지정 컨트롤의 상태를 변경 하는 경우 (무언가 수행 하는 등 때 사용자 지정 단추를 클릭). 

이 기능을 제공 하려면 편집을 `NSFlipSwitch` 클래스 및 다음 코드를 추가 합니다.

```csharp
#region Events
public event EventHandler ValueChanged;

internal void RaiseValueChanged() {
    if (this.ValueChanged != null)
        this.ValueChanged (this, EventArgs.Empty);

    // Perform any action bound to the control from Interface Builder
    // via an Action.
    if (this.Action !=null) 
        NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
}
## endregion
```

그런 다음 편집을 `FlipSwitchState` 메서드 수 있으며 다음과 같은 모양:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

에서는 먼저를 `ValueChanged` 이벤트를 추가할 수 있습니다 하는 처리기 C# 코드에서 사용자 스위치의 상태를 변경할 때 작업을 수행할 수 있도록 합니다.

둘째, 사용자 지정 컨트롤에서 상속 되므로 `NSControl`를 자동으로 갖습니다는 **동작** Xcode의 Interface Builder에서 할당할 수 있습니다. 이 메서드를 호출 하려면 **동작** 다음 코드를 사용 하 여 상태가 변경 되 면:

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

먼저 확인 하는 경우는 **동작** 컨트롤에 할당 되었습니다. 호출 다음에 **동작** 정의 된 경우.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>사용자 지정 컨트롤을 사용 하 여

완벽 하 게 정의 하는 사용자 지정 컨트롤을 사용 하 여 추가할 수 있습니다 하거나 해당 C# 코드를 사용 하 여 Xamarin.Mac 앱의 UI 또는 Xcode의 Interface Builder에서.

Interface Builder를 사용 하 여 컨트롤을 추가 하려면 먼저 Xamarin.Mac 프로젝트의 클린 빌드를 수행한 다음 두 번 클릭 하 여 `Main.storyboard` Interface Builder에서 편집 하기 위해 열려는 파일:

[![](custom-controls-images/custom02.png "Xcode에서 스토리 보드를 편집합니다.")](custom-controls-images/custom02.png#lightbox)

다음으로 끌어는 `Custom View` 사용자 인터페이스 디자인에:

[![](custom-controls-images/custom03.png "라이브러리에서 사용자 지정 보기를 선택합니다.")](custom-controls-images/custom03.png#lightbox)

선택 된 상태, 사용자 지정 보기를 전환 하는 **검사기** 뷰의 변경 **클래스** 에 `NSFlipSwitch`:

[![](custom-controls-images/custom04.png "보기의 클래스를 설정합니다.")](custom-controls-images/custom04.png#lightbox)

전환할를 **도우미 편집기** 만들고는 **출 선** 사용자 지정 컨트롤에 대 한 (에 바인딩할 수 있도록 합니다 `ViewController.h` 파일 아니라는 `.m` 파일):

[![](custom-controls-images/custom05.png "새 콘센트를 구성합니다.")](custom-controls-images/custom05.png#lightbox)

Mac 용 Visual Studio로 돌아가서 변경 내용을 저장 하 고 변경 내용을 동기화를 허용 합니다. 편집 합니다 `ViewController.cs` 파일을 확인 합니다 `ViewDidLoad` 다음과 같은 메서드 확인:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Do any additional setup after loading the view.
    OptionTwo.ValueChanged += (sender, e) => {
        // Display the state of the option switch
        Console.WriteLine("Option Two: {0}", OptionTwo.Value);
    };
}
``` 

여기에 대응 합니다 `ValueChanged` 에서 위에서 정의한 이벤트를 `NSFlipSwitch` 클래스 및 현재 작성 **값** 사용자 컨트롤을 클릭할 때.

필요에 따라 수 Interface Builder를 반환 하 고 정의 **동작** 컨트롤에서:

[![](custom-controls-images/custom06.png "새 작업을 구성합니다.")](custom-controls-images/custom06.png#lightbox)

편집 다시는 `ViewController.cs` 파일과 다음 메서드를 추가 합니다.

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> 하나를 사용 해야 합니다 **이벤트** 정의 또는 **동작** Interface Builder에서 하지만 동시에 두 메서드를 사용 하지 않아야 하거나 서로 충돌할 수 있습니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 응용 프로그램에서 다시 사용할 수 있는 사용자 지정 사용자 인터페이스 컨트롤을 만들기를 자세히 보기를 수행 했습니다. 마우스 및 사용자 입력에 응답 하는 두 가지 사용자 지정 컨트롤 UI를 그리는 방법 및 Xcode의 Interface Builder에서 작업에 새 컨트롤을 노출 하는 방법에 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacCustomControl (샘플)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [마우스 이벤트를 처리합니다.](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
