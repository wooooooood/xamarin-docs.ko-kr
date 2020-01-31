---
title: Xamarin.Mac 앱에서 네이티브 크래시 디버깅
description: 이 문서에서는 Objective-C 런타임에서 발생하는 예외를 디버그하는 방법을 설명합니다. 어설션 오류, 콜백의 문제, 예외 버블링 등을 설명합니다.
ms.prod: xamarin
ms.assetid: B0C0CE31-2737-4969-8EA5-D39D3333E9C2
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 10/19/2016
ms.openlocfilehash: 40d849ad403f2f47c00be9d3da7b59fc27ce8002
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725495"
---
# <a name="debugging-a-native-crash-in-a-xamarinmac-app"></a>Xamarin.Mac 앱에서 네이티브 크래시 디버깅

## <a name="overview"></a>개요

프로그래밍 오류 때문에 네이티브 Objective-C 런타임에서 크래시가 가끔 발생합니다. C# 예외와는 달리, 이 예외는 직접 살펴보면서 수정할 수 있도록 코드의 특정 줄을 가리키지 않습니다. 찾아서 고치기에는 아주 사소한 경우도 있고, 추적하기가 매우 어려운 경우도 있습니다.

지금부터 몇 가지 실제 네이티브 크래시 예제를 살펴보겠습니다.

## <a name="example-1-assertion-failure"></a>예제 1: 어설션 실패

다음은 간단한 테스트 애플리케이션에서 크래시의 처음 몇 줄입니다(이 정보는 **애플리케이션 출력** 패드에 있음).

```csharp
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.381 NSOutlineViewHottness[79111:1304993] (
  0   CoreFoundation                      0x91888343 __raiseError + 195
  1   libobjc.A.dylib                     0x9a5e6a2a objc_exception_throw + 276
  2   CoreFoundation                      0x918881ca +[NSException raise:format:arguments:] + 138
  3   Foundation                          0x950742b1 -[NSAssertionHandler handleFailureInMethod:object:file:lineNumber:description:] + 118
  4   AppKit                              0x975db476 -[NSTableView _uncachedRectHeightOfRow:] + 373
  5   AppKit                              0x975db2f8 -[_NSTableRowHeightStorage _uncachedRectHeightOfRow:] + 143
  6   AppKit                              0x975db206 -[_NSTableRowHeightStorage _cacheRowHeights] + 167
  7   AppKit                              0x975db130 -[_NSTableRowHeightStorage _createRowHeightsArray] + 226
  8   AppKit                              0x975b5851 -[_NSTableRowHeightStorage _ensureRowHeights] + 73
  9   AppKit                              0x975b5790 -[_NSTableRowHeightStorage computeTableHeightForNumberOfRows:] + 89
  10  AppKit                              0x975b4c38 -[NSTableView _totalHeightOfTableView] + 220
```

숫자 접두사가 붙은 줄은 기본 스택 추적입니다. 여기서는 행 높이를 처리하는 `NSTableView` 내부 어딘가에서 발생한 크래시를 볼 수 있습니다. 그리고 `NSAssertionHandler`가 `NSException (objc_exception_throw)`를 실행하면 어설션 실패가 보입니다.

```csharp
rowHeight error: The value must be > 0 for row 0, but the delegate
<NSOutlineView_ViewDelegate: 0xaa01860> gave -1.000
```

어설션 실패가 보이면 일부 `NSOutlineViewDelegate` 메서드가 음수를 반환하는 것이 확실합니다. 이것이 문제였습니다.

```csharp
public override nfloat GetRowHeight (NSTableView tableView, nint row)
{
    return -1;
}
```

## <a name="example-2-callback-jumped-into-middle-of-nowhere"></a>예제 2: 콜백이 어딘가로 점프

```csharp
Stacktrace:

 at <unknown> <0xffffffff>
 at (wrapper managed-to-native) MonoMac.AppKit.NSApplication.NSApplicationMain (int,string[]) <IL 0x000a4, 0xffffffff>
 at MonoMac.AppKit.NSApplication.Main (string[]) [0x00041] in /Users/donblas/Programming/xamcore-master/src/AppKit/NSApplication.cs:107
 at NSOutlineViewHottness.MainClass.Main (string[]) [0x00007] in /Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/Main.cs:14
 at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr) <IL 0x00050, 0xffffffff>

Native stacktrace:

Debug info from gdb:

(lldb) command source -s 0 '/tmp/mono-gdb-commands.qrHllW'
Executing commands in '/private/tmp/mono-gdb-commands.qrHllW'.
(lldb) process attach --pid 79229
Process 79229 stopped
Executable module set to "/Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/bin/Debug/NSOutlineViewHottness.app/Contents/MacOS/NSOutlineViewHottness".
Architecture set to: i386-apple-macosx.
(lldb) thread list
Process 79229 stopped
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 thread #2: tid = 0x142790, 0x9af768d2 libsystem_kernel.dylib`kevent64 + 10, queue = 'com.apple.libdispatch-manager'
 thread #3: tid = 0x142792, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #4: tid = 0x142794, 0x9af6fa6a libsystem_kernel.dylib`semaphore_wait_trap + 10
 thread #5: tid = 0x142795, 0x9af75772 libsystem_kernel.dylib`__recvfrom + 10
 thread #6: tid = 0x142799, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #7: tid = 0x14279a, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #8: tid = 0x14279b, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #9: tid = 0x1427f8, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #10: tid = 0x1427fe, 0x9af6fa2e libsystem_kernel.dylib`mach_msg_trap + 10
(lldb) thread backtrace all
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 * frame #0: 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10
   frame #1: 0x986bfb25 libsystem_c.dylib`waitpid$UNIX2003 + 48
   frame #2: 0x028ba36d libmono-2.0.dylib`mono_handle_native_sigsegv(signal=11, ctx=0x03115fe0) + 541 at mini-exceptions.c:2323
   frame #3: 0x0290a8bb libmono-2.0.dylib`mono_arch_handle_altstack_exception(sigctx=<unavailable>, fault_addr=<unavailable>, stack_ovf=0) + 155 at exceptions-x86.c:1159
   frame #4: 0x0280b4fd libmono-2.0.dylib`mono_sigsegv_signal_handler(_dummy=<unavailable>, info=<unavailable>, context=<unavailable>) + 445 at mini.c:6861
   frame #5: 0x91ef403b libsystem_platform.dylib`_sigtramp + 43
   frame #6: 0x9a5dd0bd libobjc.A.dylib`objc_msgSend + 45
   frame #7: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #8: 0x9773ba91 AppKit`-[NSApplication sendAction:to:from:] + 548
   frame #9: 0x9773b82d AppKit`-[NSControl sendAction:to:] + 102
   frame #10: 0x97934d36 AppKit`__26-[NSCell _sendActionFrom:]_block_invoke + 176
   frame #11: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #12: 0x97787975 AppKit`-[NSCell _sendActionFrom:] + 161
   frame #13: 0x979188ea AppKit`-[NSButtonCell _sendActionFrom:] + 55
   frame #14: 0x979366e6 AppKit`__48-[NSCell trackMouse:inRect:ofView:untilMouseUp:]_block_invoke965 + 43
   frame #15: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #16: 0x977a3d00 AppKit`-[NSCell trackMouse:inRect:ofView:untilMouseUp:] + 2815
   frame #17: 0x977a2df4 AppKit`-[NSButtonCell trackMouse:inRect:ofView:untilMouseUp:] + 524
   frame #18: 0x977a233b AppKit`-[NSControl mouseDown:] + 762
   frame #19: 0x97cbc112 AppKit`-[_NSThemeWidget mouseDown:] + 378
   frame #20: 0x97d36d74 AppKit`-[NSWindow _reallySendEvent:] + 12353
   frame #21: 0x977201f9 AppKit`-[NSWindow sendEvent:] + 409
   frame #22: 0x976cdc67 AppKit`-[NSApplication sendEvent:] + 4679
   frame #23: 0x9754807c AppKit`-[NSApplication run] + 1003
```

이 문제는 추적하기가 훨씬 더 어렵습니다. 관리 스택 추적의 맨 위에 `at <unknown> <0xffffffff>` 또는 `MonoMac.ObjCRuntime.Runtime.GetNSObject (IntPtr ptr)`가 표시되면 가비지 수집된 개체로 일부 관리 코드를 실행할 것을 제안합니다. 기본 스택 추적이 `trackMouse:inRect:ofView:untilMouseUp`을 `NSCell _sendActionFrom`에 표시하므로 어딘가에서 클릭 이벤트를 처리하고, C#으로 콜백하려고 시도하다가 중지됩니다.

일반적으로 이와 같은 오류는 추적하기 어렵습니다. 이 문제를 추적하기 위해(가비지 수집 강제 실행) 문제를 재현할 수 있도록 단추 처리기에 `GC.Collect(2)`를 추가했습니다. 예제를 잠시 이등분한 후 문제가 사라질 때까지 코드 섹션을 제거하니, [다음과 같은 버그](https://bugzilla.xamarin.com/show_bug.cgi?id=23378)인 것으로 판명되었습니다.

```csharp
mainWindowController.Window.StandardWindowButton (NSWindowButton.CloseButton).Activated += HandleActivated;
```

`StandardWindowButton()`에서 반환한 `NSButton`은 이벤트가 등록되는 경우에도 수집되었습니다(이것이 버그). 이 이벤트를 클릭하여 호출하려고 시도하면 단추가 가비지 수집된 경우 크래시가 발생합니다.

이 특정 문제의 근본 원인은 아니지만, Objective-C로 `[Export]`된 함수의 메서드 서명이 잘못되어 이와 같은 스택 추적이 발생할 수 있습니다. 예를 들어 메서드에서는 매개 변수로 `out string`을 예상하고 있는데 `string`이 입력되면 같은 방식으로 크래시가 발생할 수 있습니다.

## <a name="example-3-callbacks-and-managed-objects"></a>예제 3: 콜백 및 관리형 개체

많은 Cocoa API는 일부 이벤트가 발생할 때(이때 콜백되면 사용자에게 응답할 기회 제공) 또는 작업을 수행하기 위해 일부 데이터가 필요할 때 라이브러리에 의해 "콜백"됩니다. 많은 분들이 **대리자** 및 **DataSource** 패턴을 생각하시겠지만, 이런 방식으로 작동하는 여러 API가 있습니다. 예를 들어 `NSView` 메서드를 재정의하여 시각적 트리에 삽입하면 특정 이벤트가 발생할 때 AppKit가 사용자를 호출할 것으로 예상할 수 있습니다.

대부분의 경우, Xamarin.Mac이 이러한 콜백의 관리 개체 대상이 가비지 수집되는 것을 올바르게 방지하는 동시에 콜백이 가능합니다. 그러나 매우 드물게 바인딩 버그 때문에 이것이 안 될 수 있습니다. 이 경우 다음과 비슷한 불쾌한 크래시가 발생할 수 있습니다.

```csharp
Thread 0 Crashed:: Dispatch queue: com.apple.main-thread

0   libsystem_kernel.dylib              0x98c2f69a __pthread_kill + 10
1   libsystem_pthread.dylib             0x90341f19 pthread_kill + 101
2   libsystem_c.dylib                   0x9453feee abort + 156
3   libmonosgen-2.0.dylib               0x020bfba5 mono_handle_native_sigsegv + 757
4   libmonosgen-2.0.dylib               0x0210b812 mono_arch_handle_altstack_exception + 162
5   libmonosgen-2.0.dylib               0x0200c55e mono_sigsegv_signal_handler + 446
6   libsystem_platform.dylib            0x9513003b _sigtramp + 43
7   ???                                 0xffffffff 0 + 4294967295
8   libmonosgen-2.0.dylib               0x0200c3a0 mono_sigill_signal_handler + 48
9   com.apple.AppKit                    0x99f76041 -[NSView setFrame:] + 448
10  com.apple.AppKit                    0x9a1fd4ea -[NSToolbarView adjustToWindow:attachedToEdge:] + 198
11  com.apple.AppKit                    0x9a1fd414 -[NSToolbar _adjustViewToWindow] + 68
12  com.apple.AppKit                    0x9a01eb0d -[NSToolbar _windowWillShowToolbar] + 79

```

이 가이드는 크래시가 발생할 경우 이러한 특성의 버그를 추적하고, 해결할 수 있도록 올바르게 보고하고, 그때까지 코드에서 버그를 해결할 수 있도록 도와줍니다.

### <a name="locating"></a>찾기

이러한 특성의 버그가 있는 경우 가장 흔한 증상은 네이티브 크래시로, 일반적으로 `mono_sigsegv_signal_handler`와 비슷하거나, 스택의 맨 위 프레임에 있는 `_sigtrap`과 비슷합니다. Cocoa는 C# 코드로 콜백을 시도하고, 가비지 수집된 개체가 적중하고, 크래시가 발생합니다. 그러나 이러한 기호가 포함된 모든 크래시가 이와 같은 바인딩 문제 때문에 발생하는 것은 아니므로 몇 가지 추가 조사를 실시하여 이 문제가 맞는지 확인해야 합니다.

이러한 버그를 추적하기가 어려운 이유는 가비지 수집이 의심스러운 개체를 삭제한 **후**에만 발생하기 때문입니다. 이러한 버그 중 하나를 찾았다고 생각되면 다음 코드를 시작 시퀀스 어딘가에 추가합니다.

```csharp
new System.Threading.Thread (() =>
{
    while (true) {
         System.Threading.Thread.Sleep (1000);
         GC.Collect ();
    }
}).Start ();
```

이렇게 하면 애플리케이션이 가비지 수집기를 1초마다 실행합니다. 애플리케이션을 다시 실행하고 버그를 재현해 봅니다. 크래시가 즉시 발생하거나, 임의로 발생하는 것이 아니라 지속적으로 발생하는 경우 제대로 추적하고 있는 것입니다.

### <a name="reporting"></a>보고

그 다음 단계는 향후 릴리스에서 바인딩을 수정할 수 있도록 Xamarin에 문제를 보고하는 것입니다. 비즈니스 또는 엔터프라이즈 라이선스 소유자인 경우

[visualstudio.microsoft.com/vs/support/](https://visualstudio.microsoft.com/vs/support/)

그렇지 않으면 다음 단계에 따라 기존 문제를 검색합니다.

- [Xamarin.Mac 포럼](https://forums.xamarin.com/categories/xamarin-mac) 확인
- [문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues) 검색
- GitHub 문제로 전환하기 전에 Xamarin 문제가 [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi)에서 추적되었습니다. 여기서 일치하는 문제를 검색해 보세요.
- 일치하는 문제를 찾을 수 없는 경우 [GitHub 문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 제출하세요.

GitHub 문제는 모두 공용입니다. 설명 또는 첨부 파일을 숨길 수 없습니다.

다음 정보를 가능한 많이 포함하세요.

- 문제를 재현하는 간단한 예제. 문제를 재현할 수 있다면 **매우 유용합니다**.
- 크래시의 전체 스택 추적.
- 크래시 주변의 C# 코드.   

### <a name="working-around"></a>해결 방법

문제를 추적한 후에는 바인딩을 수정할 수 있을 때까지 해결 방법을 통해 문제를 패치합니다. 목표는 열린 참조를 유지하여 올바르지 않게 제거되는 개체(**보기**, **대리자**, **DataSource**)가 메모리에서 없어지지 않도록 방지하는 것입니다.

다음과 같이 개체의 단일 인스턴스만 있는 간단한 예제에서 코드를 변경해 보겠습니다.

```csharp
void AddObject ()
{
    item.View = new MyView ();
    ...
}
```

다음과 같은 정적 변수를 사용하려면:

```csharp
static NSObject view;
...

void AddObject ()
{
    view = new MyView ();
    item.View = view;
    ...
}
```

여러 인스턴스를 만들 수 있는 경우 정적 `HashSet`를 이용할 수 있습니다.

```csharp
static HashSet<NSObject> collection = new HashSet<NSObject> ();
...

void AddObject ()
{
    item.View = new MyView ();
    collection.Add (item.View );
    ...
}
```

바인딩이 수정되고 픽스가 포함된 Xamarin.Mac 버전으로 업그레이드한 후에는 문제 해결 코드를 제거해도 됩니다.

## <a name="exception-bubbling-and-objective-c"></a>예외 버블링 및 Objective-C

C# 예외가 관리 코드를 호출하는 Objective-C 메서드로 "이스케이프"하는 것을 절대 허용하면 안 됩니다. 허용할 경우 결과를 확신할 수는 없지만 대부분 크래시가 발생합니다. 사용자가 문제를 신속하게 해결할 수 있도록, 저희는 네이티브 및 관리 크래시에 대한 유용한 정보를 제공하기 위해 할 수 있는 모든 조치를 취하고 있습니다.

기술적 이유로 인한 어려움이 없다면, 모든 관리/네이티브 경계에서 관리 예외를 catch하도록 인프라를 설정하면 비용이 적지 않게 소모되며 여러 일반 작업에서 발생하는 _많은_ 전환이 있습니다. 많은 작업, 특히 UI 스레드와 관련된 작업은 신속하게 완료되어야 합니다. 그렇지 않으면 앱이 느려지고 성능이 저하됩니다. 이러한 콜백의 상당수는 throw 가능성이 별로 없는 간단한 작업만 하기 때문에 이와 같은 상황에서 이 오버헤드는 비용이 많이 들고 불필요합니다.

따라서 try/catch를 설정하지 않습니다. 코드가 사소하지 않은 작업(부울 반환 또는 간단한 수학 이상)을 수행하는 경우 사용자가 직접 catch를 시도할 수 있습니다.
