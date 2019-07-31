---
title: Xamarin.ios에서 사용자 지정 컨트롤 만들기
description: 이 문서에서는 Xamarin.ios에서 사용자 지정 컨트롤을 빌드하는 방법을 설명 합니다. 사용자 지정 컨트롤을 작성 하 고, 상태를 추적 하 고, 인터페이스를 그리고, 사용자 입력에 응답 하 고, 응용 프로그램에서 컨트롤을 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: aee3d81375ab619fa2016a87951cce3e72cdbe47
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68650193"
---
# <a name="creating-custom-controls-in-xamarinmac"></a>Xamarin.ios에서 사용자 지정 컨트롤 만들기

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우, *Swift* 및 *Xcode* *에서 개발자*가 작업 하는 것과 동일한 사용자 정의 컨트롤에 액세스할 수 있습니다. Xamarin.ios는 Xcode와 직접 통합 되므로 Xcode의 _Interface Builder_ 를 사용 하 여 사용자 정의 컨트롤을 만들고 유지 관리 하거나 선택적으로 코드에서 C# 직접 만들 수 있습니다.

MacOS는 다양 한 기본 제공 사용자 정의 컨트롤을 제공 하지만 사용자 지정 컨트롤을 만들어 사용자 지정 컨트롤을 만들어 사용자 지정 UI 테마 (예: 게임 인터페이스)와 사용자 지정 UI 테마를 일치 시 키 지 않도록 해야 할 수 있습니다.

[![](custom-controls-images/intro01.png "사용자 지정 UI 컨트롤의 예")](custom-controls-images/intro01.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램에서 재사용 가능한 사용자 지정 사용자 인터페이스 컨트롤을 만드는 기본 사항을 다룹니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 가장 좋습니다. 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하 고,에서 사용할 주요 개념 및 기술을 설명 하 고 있습니다. 이 문서를 참조 하세요.

[Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서의 [클래스/메서드를 대상으로 노출 C# -C](~/mac/internals/how-it-works.md) 섹션을 살펴볼 수 있습니다. C# 클래스를 연결 하는 데 사용 되는 `Register` 및 `Export` 명령을 설명 합니다. 목표-C 개체 및 UI 요소입니다.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>사용자 지정 컨트롤 소개

위에서 설명한 것 처럼, 다시 사용할 수 있는 사용자 지정 사용자 인터페이스 컨트롤을 만들어 Xamarin.ios 앱 UI에 고유한 기능을 제공 하거나 사용자 지정 UI 테마 (예: 게임 인터페이스)를 만들어야 하는 경우가 있을 수 있습니다.

이러한 상황에서는 코드를 통해 `NSControl` C# 또는 Xcode의 Interface Builder을 통해 앱의 UI에 추가할 수 있는 사용자 지정 도구를 쉽게 상속 하 고 만들 수 있습니다. 사용자 지정 컨트롤 `NSControl` 에서 상속 하는 경우에는 기본 제공 사용자 인터페이스 컨트롤의 모든 표준 기능 ( `NSButton`예:)이 자동으로 포함 됩니다.

사용자 지정 사용자 인터페이스 컨트롤이 사용자 지정 차트 및 그래픽 도구와 같은 정보를 표시 하는 경우 `NSView` `NSControl`대신에서 상속 하는 것이 좋습니다.

사용 되는 기본 클래스에 관계 없이 사용자 지정 컨트롤을 만드는 기본 단계는 동일 합니다.

이 문서에서는 고유한 사용자 인터페이스 테마를 제공 하는 사용자 지정 플립 스위치 구성 요소와 완전히 작동 하는 사용자 지정 사용자 인터페이스 컨트롤을 빌드하는 예제를 만듭니다.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>사용자 지정 컨트롤 빌드

만드는 사용자 지정 컨트롤은 사용자 입력에 응답 하 게 되므로 (마우스 왼쪽 단추 클릭)에서 `NSControl`상속 됩니다. 이러한 방식으로 사용자 지정 컨트롤에는 기본 제공 사용자 인터페이스 컨트롤의 모든 표준 기능이 자동으로 포함 되 고 표준 macOS 컨트롤과 같이 응답 합니다.

Mac용 Visual Studio에서 사용자 지정 사용자 인터페이스 컨트롤을 만들 Xamarin.ios 프로젝트를 엽니다 (또는 새 항목을 만듭니다). 새 클래스를 추가 하 고 호출 `NSFlipSwitch`합니다.

[![](custom-controls-images/custom01.png "새 클래스 추가")](custom-controls-images/custom01.png#lightbox)

그런 다음 `NSFlipSwitch.cs` 클래스를 편집 하 고 다음과 같이 만듭니다.

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

에서 사용자 지정 클래스에 대해 알아두어야 할 첫 번째 사항은이 클래스를 Xcode `NSControl` 및의 Interface Builder에 노출 하기 위해 **Register** 명령을 사용 하는 것입니다.

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

다음 섹션에서는 위의 코드의 나머지 부분에 대해 자세히 살펴보겠습니다.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>컨트롤 상태 추적

사용자 지정 컨트롤이 스위치 이므로 스위치의 설정/해제 상태를 추적 하는 방법이 필요 합니다. 에서 `NSFlipSwitch`다음 코드를 사용 하 여이를 처리 합니다.

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

스위치의 상태가 변경 되 면 UI를 업데이트 하는 방법이 필요 합니다. 이렇게 하려면 컨트롤이로 `NeedsDisplay = true`UI를 다시 그리도록 합니다.

컨트롤이 단일 On/Off 상태 (예: 3 위치를 포함 하는 다중 상태 스위치)를 더 많이 필요로 하는 경우에는 **열거형** 을 사용 하 여 상태를 추적할 수 있습니다. 이 예에서는 간단한 **bool** 을 사용할 수 있습니다.

또한 스위치 상태를 설정 및 해제 하는 도우미 메서드를 추가 했습니다.

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

나중에이 도우미 클래스를 확장 하 여 스위치 상태가 변경 되 면 호출자에 게 알립니다.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>컨트롤의 인터페이스 그리기

핵심 그래픽 그리기 루틴을 사용 하 여 런타임에 사용자 지정 컨트롤의 사용자 인터페이스를 그릴 예정입니다. 이렇게 하려면 먼저 컨트롤에 대 한 계층을 설정 해야 합니다. 다음 개인 방법으로이 작업을 수행 합니다.

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

이 메서드는 컨트롤이 적절 하 게 구성 되었는지 확인 하기 위해 각 컨트롤의 생성자에서 호출 됩니다. 예를 들어:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

다음에는 `DrawRect` 메서드를 재정의 하 고 핵심 그래픽 루틴을 추가 하 여 컨트롤을 그려야 합니다.

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

컨트롤의 상태가 변경 될 때 (예: **On** 에서 **Off**로 전환) 컨트롤에 대 한 시각적 표시를 조정 합니다. 상태가 변경 될 때마다이 `NeedsDisplay = true` 명령을 사용 하 여 컨트롤이 새 상태를 다시 그리도록 강제할 수 있습니다.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>사용자 입력에 응답

사용자 지정 컨트롤에 사용자 입력을 추가할 수 있는 두 가지 기본 방법은 다음과 같습니다. **마우스 처리 루틴** 또는 **제스처 인식기**를 재정의 합니다. 사용 되는 메서드는 컨트롤에 필요한 기능을 기반으로 합니다.

> [!IMPORTANT]
> 사용자가 만드는 모든 사용자 지정 컨트롤에 대해 **재정의 메서드나** **제스처 인식자**를 사용 해야 하지만 서로 충돌할 수 있습니다.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>재정의 메서드를 사용 하 여 사용자 입력 처리

(또는 `NSControl` `NSView`)에서 상속 되는 개체에는 마우스나 키보드 입력을 처리 하는 여러 재정의 메서드가 있습니다. 예제 컨트롤의 경우 사용자가 왼쪽 마우스 단추를 사용 하 여 컨트롤을 클릭할 때 스위치 상태를 **설정** 및 **해제** 합니다. 다음 재정의 메서드를 `NSFlipSwitch` 클래스에 추가 하 여이를 처리할 수 있습니다.

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

위의 코드에서 메서드 (위에 정의 됨 `FlipSwitchState` )를 호출 하 여 `MouseDown` 메서드에서 스위치의 On/Off 상태를 대칭 이동 합니다. 이렇게 하면 현재 상태를 반영 하도록 컨트롤을 강제로 다시 그릴 수도 있습니다.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>제스처 인식기를 사용 하 여 사용자 입력 처리

필요에 따라 제스처 인식기를 사용 하 여 컨트롤과 상호 작용 하는 사용자를 처리할 수 있습니다. 위에 추가 된 재정의를 제거 하 고 `Initialize` , 메서드를 편집 하 고 다음과 같이 표시 합니다.

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

여기서는 새 `NSClickGestureRecognizer` 를 만들고 `FlipSwitchState` 메서드를 호출 하 여 사용자가 마우스 왼쪽 단추로 클릭 했을 때 스위치의 상태를 변경 합니다. 메서드 `AddGestureRecognizer (click)` 는 제스처 인식기를 컨트롤에 추가 합니다.

다시, 사용 하는 방법은 사용자 지정 컨트롤을 사용 하 여 수행 하려는 작업에 따라 달라 집니다. 사용자 상호 작용에 대 한 낮은 수준의 액세스 권한이 필요한 경우 재정의 메서드를 사용 합니다. 마우스 클릭과 같이 미리 정의 된 기능을 사용 해야 하는 경우 제스처 인식기를 사용 합니다.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>상태 변경 이벤트에 대 한 응답

사용자가 사용자 지정 컨트롤의 상태를 변경 하는 경우 코드의 상태 변경에 응답 하는 방법이 필요 합니다 (예: 사용자 지정 단추를 클릭할 때 수행 하는 작업). 

이 기능을 제공 하려면 `NSFlipSwitch` 클래스를 편집 하 고 다음 코드를 추가 합니다.

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

그런 다음 `FlipSwitchState` 메서드를 편집 하 여 다음과 같이 만듭니다.

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

먼저 사용자가 스위치의 `ValueChanged` 상태를 변경할 때 작업을 수행할 수 있도록 C# 코드에서 처리기를 추가할 수 있는 이벤트를 제공 합니다.

둘째, 사용자 지정 컨트롤은에서 `NSControl`상속 되므로 Xcode의 Interface Builder에서 할당할 수 있는 **동작이** 자동으로 포함 됩니다. 상태가 변경 될 때이 **작업** 을 호출 하려면 다음 코드를 사용 합니다.

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

먼저 컨트롤이 컨트롤에 할당 되었는지 확인 합니다 . 그런 다음 작업을 정의 하는 경우이 **작업** 을 호출 합니다.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>사용자 지정 컨트롤 사용

사용자 지정 컨트롤을 완전히 정의한 상태에서 코드 또는 Xcode의 Interface Builder을 사용 하 여 C# xamarin.ios 앱의 UI에 추가할 수 있습니다.

Interface Builder를 사용 하 여 컨트롤을 추가 하려면 먼저 xamarin.ios 프로젝트의 클린 빌드를 수행 하 고 `Main.storyboard` 파일을 두 번 클릭 하 여 편집을 위해 Interface Builder에서 엽니다.

[![](custom-controls-images/custom02.png "Xcode에서 storyboard 편집")](custom-controls-images/custom02.png#lightbox)

그런 다음를 사용자 `Custom View` 인터페이스 디자인으로 끌어 옵니다.

[![](custom-controls-images/custom03.png "라이브러리에서 사용자 지정 보기 선택")](custom-controls-images/custom03.png#lightbox)

사용자 지정 보기를 선택한 상태에서 **Id 검사기** 로 전환 하 고 뷰의 **클래스** 를로 `NSFlipSwitch`변경 합니다.

[![](custom-controls-images/custom04.png "뷰의 클래스 설정")](custom-controls-images/custom04.png#lightbox)

**길잡이 편집기** 로 전환 하 고 사용자 지정 컨트롤에 대 한 **콘센트** 를 만듭니다 (파일이 아닌 `ViewController.h` 파일에 바인딩되어야 함 `.m` ).

[![](custom-controls-images/custom05.png "새 콘센트 구성")](custom-controls-images/custom05.png#lightbox)

변경 내용을 저장 하 고 Mac용 Visual Studio로 돌아간 다음 변경 내용을 동기화 할 수 있습니다. 파일을 편집 하 고 메서드 `ViewDidLoad` 를 다음과 같이 만듭니다. `ViewController.cs`

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

여기서는 위에서 `ValueChanged` `NSFlipSwitch` 정의한 이벤트에 응답 하 고 사용자가 컨트롤을 클릭할 때 현재 **값** 을 작성 합니다.

필요에 따라 Interface Builder로 돌아가서 컨트롤에 대 한 **작업** 을 정의할 수 있습니다.

[![](custom-controls-images/custom06.png "새 작업 구성")](custom-controls-images/custom06.png#lightbox)

다시 `ViewController.cs` 파일을 편집 하 고 다음 메서드를 추가 합니다.

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Interface Builder에서 **이벤트** 를 사용 하거나 **작업** 을 정의 해야 하지만 두 메서드를 동시에 사용 하거나 서로 충돌할 수 있습니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 재사용 가능한 사용자 지정 사용자 인터페이스 컨트롤을 만드는 방법에 대해 자세히 살펴봅니다. 사용자 지정 컨트롤 UI를 그리는 방법, 마우스 및 사용자 입력에 응답 하는 두 가지 주요 방법 및 Xcode의 Interface Builder 작업에 새 컨트롤을 노출 하는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacCustomControl (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccustomcontrol)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [마우스 이벤트 처리](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
