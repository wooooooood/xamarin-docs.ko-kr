---
title: 사용자 지정 컨트롤 만들기
description: 이 문서에서는 사용자 지정 컨트롤을 만들고 인터페이스 작성기에서 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: e1ab3977df96e241fa2a5a80f6cabd74d7d775f8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="creating-custom-controls"></a>사용자 지정 컨트롤 만들기

_이 문서에서는 사용자 지정 컨트롤을 만들고 인터페이스 작성기에서 사용 하는 방법을 설명 합니다._

C# 및.NET Xamarin.Mac 응용 프로그램에서에서 작업할 때는 동일 하 게 액세스할 수 있습니다 하는 사용자의 작업 개발자를 제공 하는 컨트롤 *Objective-c*, *Swift* 및 *Xcode* 않습니다 . Xamarin.Mac Xcode에 직접 통합을 사용 하면 Xcode의 _인터페이스 작성기_ 만들기 및 사용자 정의 컨트롤을 유지 관리 (또는 C# 코드에서 직접 만들 필요에 따라).

MacOS 다양 한 기본 제공 사용자 정의 컨트롤을 제공 하며, 기본적으로 지원 되지 않는 기능을 제공 하거나 (예: 게임 인터페이스) 사용자 지정 UI 테마에 맞게 사용자 지정 컨트롤을 만들어야 하는 횟수가 있을 수 있습니다.

[![](custom-controls-images/intro01.png "사용자 지정 UI 컨트롤의 예")](custom-controls-images/intro01.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 다시 사용할 수 있는 사용자 지정 사용자 인터페이스 컨트롤을 작성 하는 기본적인 하겠습니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 섹션으로이 문서에서 사용할 수 있는 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 명령 와이어 접속 C# 클래스 Objective-c 개체 및 UI 요소에 하는 데 사용 합니다.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>사용자 지정 컨트롤 소개

위에서 설명 했 듯이 재사용 Xamarin.Mac 응용 프로그램의 UI에 고유한 기능을 제공 하거나 (예: 게임 인터페이스) 사용자 지정 UI 테마를 만들 사용자 지정 사용자 인터페이스 컨트롤을 만들 하는 경우가 있을 수 있습니다.

이러한 상황에서는 쉽게에서 상속할 수 있습니다 `NSControl` 하거나 Xcode의 인터페이스 작성기 또는 C# 코드를 통해 응용 프로그램의 UI에 추가할 수 있는 사용자 지정 도구를 만듭니다. 상속 하 여 `NSControl` 사용자 지정 컨트롤이 자동으로 모든 적용 됩니다. 기본 제공 사용자 인터페이스 컨트롤에 표준 기능 (예: `NSButton`).

상속 하려는 사용자 지정 사용자 인터페이스 컨트롤 정보 (예: 사용자 지정 차트를 그래픽 도구)를 표시 하는 경우 `NSView` 대신 `NSControl`합니다.

기본 클래스를 사용 하는 관계 없이 사용자 지정 컨트롤을 만드는 기본 단계는 같습니다.

이 문서에는 고유한 사용자 인터페이스 테마 및의 모든 기능을 갖춘 사용자 지정 사용자 인터페이스 컨트롤 작성 예제를 제공 하는 사용자 지정 스위치 대칭 구성 요소를 만듭니다.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>사용자 지정 컨트롤을 만들기

상속 하도록 하겠습니다 생성 사용자 지정 컨트롤 (마우스 왼쪽된 단추 클릭) 하는 사용자 입력에 응답 합니다, 이후 `NSControl`합니다. 이러한 방식으로 사용자 지정 컨트롤 자동으로 모든 표준 기능을 기본 제공 사용자 인터페이스 컨트롤에 적용 하 고 표준 macOS 컨트롤 같이 응답 합니다.

Mac 용 Visual Studio에서 사용자 지정 사용자 인터페이스 컨트롤을 만드는 (또는 새로 만들) 하려는 Xamarin.Mac 프로젝트를 엽니다. 새 클래스를 추가 하 고 호출할 `NSFlipSwitch`:

[![](custom-controls-images/custom01.png "새 클래스 추가")](custom-controls-images/custom01.png#lightbox)

다음으로 편집 된 `NSFlipSwitch.cs` 클래스와 다음과 비슷하게 표시:

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

이 사용자 지정 클래스에서 상속 하는 것에 주의 해야 할 가장 먼저 `NSControl` 를 사용 하 고는 **등록** Objective C와 Xcode의 인터페이스 작성기에이 클래스를 노출 하는 명령:

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

다음 섹션에서 자세히 위 코드의 나머지를 살펴보면을 이동 합니다.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>컨트롤의 상태를 추적합니다.

이 사용자 지정 컨트롤은 스위치, 스위치의 On/Off 상태를 추적 하는 방법을 원하므로 해야 합니다. 다음 코드로 처리 하는 `NSFlipSwitch`:

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

스위치의 상태 변경 될 때 UI를 업데이트 하는 방법이 필요 합니다. 와 함께 해당 UI를 다시 그리게 컨트롤을 강제로 수행 하 여 할까요 `NeedsDisplay = true`합니다.

사용할 수도 제어권 필요한 경우 하는 단일 설정/해제 상태가 (예: 3 위치와 다중 상태 스위치)는 **Enum** 상태를 추적할 수 있습니다. 이 예에서는 간단한 **bool** 작업을 수행 합니다.

또한 켜고 사이의 스위치의 상태를 교체 하는 도우미 메서드를 추가 했습니다.

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

나중 스위치 상태가 변경 될 때 호출자에 게 알리기 위해이 도우미 클래스를 확장 합니다.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>컨트롤의 인터페이스를 그리기

런타임 시 사용자 지정 컨트롤의 사용자 인터페이스를 그리는 코어 그래픽 그리기 루틴을 사용 하려면 하겠습니다. 이렇게 하려면,이 컨트롤에 대 한 계층 설정 여부를 해야 합니다. 다음 전용 메서드를 수행 합니다.

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

이 메서드는 각 컨트롤 올바르게 구성 되어 있는지 확인 하기 위해 컨트롤의 생성자에서 호출 됩니다. 예를 들어:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

다음으로 재정의 해야는 `DrawRect` 메서드 컨트롤을 그리는 코어 그래픽 루틴을 추가 합니다.

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

म 합니다 수 조정 컨트롤에 대 한 시각적 표시의 상태가 변경 될 때 (에서 이동 하는 등 **에** 를 **오프**). 언제 든 지 상태 변경, 사용할 수는 `NeedsDisplay = true` 명령을 강제로 새로운 상태에 대 한 다시 그리기를 제어 합니다.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>사용자 입력에 응답

사용자 입력 사용자 지정 컨트롤을 추가할 수 있는 기본적인 방법은 두 가지가: **마우스 처리 루틴 재정의** 또는 **제스처 인식기**합니다. 를 사용 하는 방법은 제어권에 필요한 기능에 따라 달라 집니다.

> [!IMPORTANT]
> 만드는 모든 사용자 지정 컨트롤을 사용 해야 하거나 **메서드 재정의** _또는_ **제스처 인식기**, 둘 중 하나에서 동일한 시간을 서로 충돌할 수 있습니다.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>재정의 메서드를 사용 하 여 사용자 입력 처리

상속 하는 개체 `NSControl` (또는 `NSView`) 여러 키보드 입력 또는 마우스 처리에 대 한 메서드를 재정의 합니다. 이 예제에서는 컨트롤에 대 한 사이의 스위치의 상태를 대칭 이동 하려고 **에** 및 **오프** 사용자가 왼쪽된 마우스 단추를 사용 하 여 컨트롤을 클릭할 때. 메서드를 재정의 하는 다음을 추가할 수 있습니다는 `NSFliwSwitch` 이 처리 하기 위해 클래스:

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

위의 코드에서 호출는 `FlipSwitchState` 메서드 (앞에서 정의한) On 대칭 이동/에 스위치의 상태를 해제 하는 `MouseDown` 메서드. 또한 이렇게 하면 현재 상태를 반영 하도록 다시 그릴 컨트롤을 시작 합니다.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>제스처 인식기를 사용 하 여 사용자 입력 처리

필요에 따라 사용자 컨트롤과 상호 작용을 처리 하기 제스처 인식기를 사용할 수 있습니다. 위에 추가 재정의 제거, 편집는 `Initialize` 메서드 다음과 같이 만듭니다.

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

여기서는 만들기는 새 `NSClickGestureRecognizer` 호출 우리의 `FlipSwitchState` 사용자가 마우스 왼쪽된 단추 클릭에 스위치의 상태를 변경 하는 메서드. `AddGestureRecognizer (click)` 메서드 제스처 인식기 컨트롤을 추가 합니다.

다시 사용 하는 방법을 우리 서는 사용자 지정 컨트롤을 수행 하려는 작업에 따라 다릅니다. 낮은 수준 액세스 해야 하는 경우는 사용자 상호 작용을 재정의 메서드를 사용 합니다. 미리 정의 된 기능을 필요 예: 마우스 클릭 제스처 인식기를 사용 합니다.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>상태 변경 이벤트에 응답

코드에서 상태 변경에 응답 하는 방법이 필요 사용자 사용자 지정 컨트롤의 상태를 변경 하는 경우 (같은 작업을 수행할 때 사용자 지정 단추를 클릭). 

이 기능을 제공 하려면 편집는 `NSFlipSwitch` 클래스를 다음 코드를 추가 합니다.

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

다음에 편집는 `FlipSwitchState` 메서드 다음과 같이 만듭니다.

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

먼저 제공는 `ValueChanged` 이벤트를 추가할 수 있습니다 하는 처리기 C# 코드에서을 스위치의 상태를 변경할 때 작업을 수행할 수 있습니다.

사용자 지정 컨트롤에서 상속 하기 때문에 두 번째 `NSControl`, 자동으로 가지는 **동작** Xcode의 인터페이스 작성기에서 할당할 수 있는 합니다. 이 메서드를 호출 하려면 **동작** 다음 코드는 상태가 변경 될 때 사용 합니다.

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

첫째, 있는지 확인 한 **동작** 컨트롤에 할당 되었습니다. 호출 다음에 **동작** 정의 된 경우.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>사용자 지정 컨트롤 사용

완전 하 게 정의 하는 사용자 지정 컨트롤을 추가할 수 있습니다 하거나 Xcode의 인터페이스 작성기 또는 C# 코드를 사용 하 여 Xamarin.Mac 앱의 UI에 있습니다.

인터페이스 작성기를 사용 하 여 컨트롤을 추가 하려면 먼저 Xamarin.Mac 프로젝트의 클린 빌드를 수행 합니다. 다음 두 번 클릭 하 고 `Main.storyboard` 인터페이스 작성기에서 편집 하기 위해 열려는 파일:

[![](custom-controls-images/custom02.png "Xcode에서 스토리 보드를 편집합니다.")](custom-controls-images/custom02.png#lightbox)

를 끌어 한 `Custom View` 사용자 인터페이스 디자인에:

[![](custom-controls-images/custom03.png "라이브러리에서 사용자 지정 보기를 선택합니다.")](custom-controls-images/custom03.png#lightbox)

선택 된 상태, 사용자 지정 보기를 전환 하는 **Identity 관리자** 뷰의 변경 **클래스** 를 `NSFlipSwitch`:

[![](custom-controls-images/custom04.png "보기의 클래스를 설정합니다.")](custom-controls-images/custom04.png#lightbox)

전환 하는 **도우미 편집기** 만듭니다는 **콘센트** 사용자 지정 컨트롤에 대 한 (에 바인딩할 수 있도록는 `ViewControler.h` 파일 및 not는 `.m` 파일):

[![](custom-controls-images/custom05.png "새 콘센트 구성")](custom-controls-images/custom05.png#lightbox)

Mac 용 Visual Studio로 돌아가서 여 변경 내용을 저장 하 고 변경 내용을 동기화를 허용 합니다. 편집 된 `ViewController.cs` 파일을 확인는 `ViewDidLoad` 다음과 같은 메서드 모양을:

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

응답 여기서는 `ValueChanged` 에 위에서 정의한 이벤트는 `NSFlipSwitch` 클래스를 현재 기록 **값** 사용자 컨트롤을 클릭할 때입니다.

필요에 따라 우리 수 인터페이스 작성기 돌아가서 정의 **동작** 컨트롤에:

[![](custom-controls-images/custom06.png "새 동작 구성")](custom-controls-images/custom06.png#lightbox)

다시 편집는 `ViewController.cs` 파일을 다음 메서드를 추가 합니다.

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> 하나를 사용 해야는 **이벤트** 하거나 정의 **동작** 인터페이스 작성기에서 하지만 동시에 두 메서드를 사용 하지 않아야 하거나 서로 충돌할 수 있습니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에 자세히 살펴보고 Xamarin.Mac 응용 프로그램에서 다시 사용할 수 있는 사용자 지정 사용자 인터페이스 컨트롤을 만드는 수행 했습니다. 마우스 및 사용자 입력에 응답 하는 두 가지 주요 방법으로 사용자 지정 컨트롤 UI를 그리는 방법 및 Xcode의 인터페이스 작성기에서 작업에 새 컨트롤을 노출 하는 방법에 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacCustomControl (샘플)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [마우스 이벤트를 처리합니다.](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
