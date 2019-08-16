---
title: Xamarin.ios의 예외 마샬링
description: 이 문서에서는 Xamarin.ios 앱에서 네이티브 및 관리 되는 예외를 사용 하는 방법을 설명 합니다. 발생할 수 있는 문제와 이러한 문제에 대 한 해결 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/05/2017
ms.openlocfilehash: 13b88619ccd7d01f32afd5b9332c7dcb426380b0
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69527985"
---
# <a name="exception-marshaling-in-xamarinios"></a>Xamarin.ios의 예외 마샬링

_Xamarin.ios에는 예외에 응답 하는 데 도움이 되는 새로운 이벤트 (특히 네이티브 코드)가 포함 되어 있습니다._

관리 코드와 목적-C는 모두 런타임 예외 (try/catch/finally 절)를 지원 합니다.

그러나 해당 구현은 서로 다릅니다. 즉, 런타임 라이브러리 (Mono 런타임 및 목표-C 런타임 라이브러리)가 예외를 처리 하 고 다른 언어로 작성 된 코드를 실행 해야 하는 경우 문제가 발생 합니다.

이 문서에서는 발생할 수 있는 문제 및 가능한 해결 방법을 설명 합니다.

또한 다양 한 시나리오 및 해당 솔루션을 테스트 하는 데 사용할 수 있는 샘플 프로젝트인 [예외 마샬링을](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)포함 합니다.

## <a name="problem"></a>문제점

예외가 throw 되 고 스택 해제 중에 throw 된 예외 유형과 일치 하지 않는 프레임이 발견 되 면이 문제가 발생 합니다.

Xamarin.ios 또는 Xamarin.ios에 대 한 일반적인 예는 네이티브 API에서 목표-C 예외를 throw 한 후 스택 해제 프로세스가 관리 되는 프레임에 도달할 때 해당 목표-C 예외를 처리 해야 하는 경우입니다.

기본 작업은 아무 작업도 수행 하지 않는 것입니다. 위의 샘플에서이는 목표-C 런타임이 관리 되는 프레임을 해제 하도록 허용 하는 것을 의미 합니다. 목표-C 런타임이 관리 되는 프레임을 해제 하는 방법을 알지 못하기 때문에 문제가 발생 합니다. 예를 들어 해당 프레임에서 `catch` 또는 `finally` 절은 실행 되지 않습니다.

### <a name="broken-code"></a>손상 된 코드

다음 코드 예제를 참조 하세요.

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

이렇게 하면 네이티브 코드에서 목표 C NSInvalidArgumentException이 throw 됩니다.

```
NSInvalidArgumentException *** setObjectForKey: key cannot be nil
```

그리고 스택 추적은 다음과 같습니다.

```
0   CoreFoundation          __exceptionPreprocess + 194
1   libobjc.A.dylib         objc_exception_throw + 52
2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
3   libobjc.A.dylib         objc_msgSend + 102
4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()
```

프레임 0-3은 네이티브 프레임 이며, 목표-C 런타임의 stack 해제기 이러한 프레임을 해제할 _수 있습니다_ . 특히 목표-C `@catch` 또는 `@finally` 절이 실행 됩니다.

그러나 목표-C stack 해제기는 프레임을 해제 하지만 관리 되는 예외 논리는 실행 되지 않도록 관리 되는 프레임 (프레임 4-6)을 제대로 해제할 수 _없습니다_ .

즉, 일반적으로 다음과 같은 방법으로 이러한 예외를 catch 할 수 없습니다.

```csharp
try {
    var dict = new NSMutableDictionary ();
    dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero);
} catch (Exception ex) {
    Console.WriteLine (ex);
} finally {
    Console.WriteLine ("finally");
}
```

이는 목표-C stack 해제기가 관리 되 `catch` 는 절에 대해 알지 못하기 때문 이며, `finally` 절이 실행 되지 않습니다.

위의 코드 예제를 효과적 으로 사용 하는 경우 목표-c에는 처리 되지 않은 목표-c 예외 ( [`NSSetUncaughtExceptionHandler`][2]xamarin.ios 및 xamarin.ios에서 사용 됨)에 대 한 알림이 있고 해당 지점에서 목표-c 예외를 변환 하려고 시도 하기 때문입니다. 관리 되는 예외입니다.

## <a name="scenarios"></a>시나리오

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>시나리오 1-관리 되는 catch 처리기를 사용 하 여 목표-C 예외 catch

다음 시나리오에서는 관리 되 `catch` 는 처리기를 사용 하 여 목표 C 예외를 catch 할 수 있습니다.

1. 목표-C 예외가 throw 됩니다.
2. 목표 C 런타임은 예외를 처리할 수 있는 네이티브 `@catch` 처리기를 찾기 위해 스택을 탐색 하 고 해제 하지는 않습니다.
3. 목표 C 런타임은 `@catch` 처리기를 찾지 않고를 호출 `NSGetUncaughtExceptionHandler`하 고 xamarin.ios/xamarin.ios에 의해 설치 된 처리기를 호출 합니다.
4. Xamarin.ios/Xamarin.ios의 처리기는 목표 C 예외를 관리 되는 예외로 변환 하 고 throw 합니다. 목표 C 런타임에서는 스택을 해제 하지 않았으므로 (한 번만) 현재 프레임은 목표-C 예외가 throw 된 것과 같습니다.

Mono 런타임에서 목표-C 프레임을 제대로 해제 하는 방법을 알지 못하기 때문에 다른 문제가 발생 합니다.

Xamarin.ios ' catch 되지 않은 목표-C 예외 콜백이 호출 되 면 스택은 다음과 같습니다.

```
 0 libxamarin-debug.dylib   exception_handler(exc=name: "NSInvalidArgumentException" - reason: "*** setObjectForKey: key cannot be nil")
 1 CoreFoundation           __handleUncaughtException + 809
 2 libobjc.A.dylib          _objc_terminate() + 100
 3 libc++abi.dylib          std::__terminate(void (*)()) + 14
 4 libc++abi.dylib          __cxa_throw + 122
 5 libobjc.A.dylib          objc_exception_throw + 337
 6 CoreFoundation           -[__NSDictionaryM setObject:forKey:] + 1015
 7 libxamarin-debug.dylib   xamarin_dyn_objc_msgSend + 102
 8 TestApp                  ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
 9 TestApp                  Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr) [0x00000]
10 TestApp                  ExceptionMarshaling.Exceptions.ThrowObjectiveCException () [0x00013]
```

여기에서 관리 되는 프레임은 프레임 8-10 뿐 이지만, 관리 되는 예외는 프레임 0에서 throw 됩니다. 즉, mono 런타임은 네이티브 프레임 0-7을 해제 해야 합니다 .이로 인해 위에서 설명한 문제와 동일한 문제가 발생 합니다. mono 런타임은 네이티브 프레임을 해제 하지만 목표-C `@catch` 또는 `@finally` 절은 실행 되지 않습니다. .

코드 예제:

```objc
-(id) setObject: (id) object forKey: (id) key
{
    @try {
        if (key == nil)
            [NSException raise: @"NSInvalidArgumentException"];
    } @finally {
        NSLog (@"This will not be executed");
    }
}
```

이 프레임을 해제 하는 Mono 런타임이 해당 절을알지못하기때문에절이실행되지않습니다.`@finally`

이는 관리 되는 코드에서 관리 되는 예외를 throw 한 다음 네이티브 프레임을 통해 해제 하 여 첫 번째 관리 되 `catch` 는 절에 가져오는 것을 변형 한 것입니다.

```csharp
class AppDelegate : UIApplicationDelegate {
    public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
    {
        throw new Exception ("An exception");
    }
    static void Main (string [] args)
    {
        try {
            UIApplication.Main (args, null, typeof (AppDelegate));
        } catch (Exception ex) {
            Console.WriteLine ("Managed exception caught.");
        }
    }
}
```

관리 되 `UIApplication:Main` 는 메서드는 네이티브 `UIApplicationMain` 메서드를 호출 하 고, iOS는 궁극적으로 관리 되는 `AppDelegate:FinishedLaunching` 메서드를 호출 하기 전에 많은 네이티브 코드를 실행 하 고 관리 되는 예외가 없으면

```
 0: TestApp                 ExceptionMarshaling.IOS.AppDelegate:FinishedLaunching (UIKit.UIApplication,Foundation.NSDictionary)
 1: TestApp                 (wrapper runtime-invoke) <Module>:runtime_invoke_bool__this___object_object (object,intptr,intptr,intptr) 
 2: libmonosgen-2.0.dylib   mono_jit_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
 3: libmonosgen-2.0.dylib   do_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
 4: libmonosgen-2.0.dylib   mono_runtime_invoke [inlined] mono_runtime_invoke_checked(method=<unavailable>, obj=<unavailable>, params=<unavailable>, error=0xbff45758)
 5: libmonosgen-2.0.dylib   mono_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>)
 6: libxamarin-debug.dylib  xamarin_invoke_trampoline(type=<unavailable>, self=<unavailable>, sel="application:didFinishLaunchingWithOptions:", iterator=<unavailable>), context=<unavailable>)
 7: libxamarin-debug.dylib  xamarin_arch_trampoline(state=0xbff45ad4)
 8: libxamarin-debug.dylib  xamarin_i386_common_trampoline
 9: UIKit                   -[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:]
10: UIKit                   -[UIApplication _callInitializationDelegatesForMainScene:transitionContext:]
11: UIKit                   -[UIApplication _runWithMainScene:transitionContext:completion:]
12: UIKit                   __84-[UIApplication _handleApplicationActivationWithScene:transitionContext:completion:]_block_invoke.3124
13: UIKit                   -[UIApplication workspaceDidEndTransaction:]
14: FrontBoardServices      __37-[FBSWorkspace clientEndTransaction:]_block_invoke_2
15: FrontBoardServices      __40-[FBSWorkspace _performDelegateCallOut:]_block_invoke
16: FrontBoardServices      __FBSSERIALQUEUE_IS_CALLING_OUT_TO_A_BLOCK__
17: FrontBoardServices      -[FBSSerialQueue _performNext]
18: FrontBoardServices      -[FBSSerialQueue _performNextFromRunLoopSource]
19: FrontBoardServices      FBSSerialQueueRunLoopSourceHandler
20: CoreFoundation          __CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__
21: CoreFoundation          __CFRunLoopDoSources0
22: CoreFoundation          __CFRunLoopRun
23: CoreFoundation          CFRunLoopRunSpecific
24: CoreFoundation          CFRunLoopRunInMode
25: UIKit                   -[UIApplication _run]
26: UIKit                   UIApplicationMain
27: TestApp                 (wrapper managed-to-native) UIKit.UIApplication:UIApplicationMain (int,string[],intptr,intptr)
28: TestApp                 UIKit.UIApplication:Main (string[],intptr,intptr)
29: TestApp                 UIKit.UIApplication:Main (string[],string,string)
30: TestApp                 ExceptionMarshaling.IOS.Application:Main (string[])
```

0-1 및 27-30 프레임은 관리 되지만 사이에 있는 모든의는 네이티브입니다. 이러한 프레임을 통해 Mono가 해제 되 면 목표- `@catch` C `@finally` 또는 절이 실행 되지 않습니다.

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>시나리오 2-목표-C 예외를 catch 할 수 없음

다음 시나리오에서는 목표 c 예외가 다른 방식으로 처리 되기 때문에 관리 되 `catch` 는 처리기를 사용 하 여 객관적인 c 예외를 catch 할 수 없습니다.

1. 목표-C 예외가 throw 됩니다.
2. 목표 C 런타임은 예외를 처리할 수 있는 네이티브 `@catch` 처리기를 찾기 위해 스택을 탐색 하 고 해제 하지는 않습니다.
3. 목표 C 런타임은 `@catch` 처리기를 찾고, 스택을 해제 하 고, `@catch` 처리기 실행을 시작 합니다.

이 시나리오는 일반적으로 Xamarin.ios 앱에서 찾을 수 있습니다. 주 스레드에는 일반적으로 다음과 같은 코드가 있습니다.

``` objective-c
void UIApplicationMain ()
{
    @try {
        while (true) {
            ExecuteRunLoop ();
        }
    } @catch (NSException *ex) {
        NSLog (@"An unhandled exception occured: %@", exc);
        abort ();
    }
}

```

즉, 주 스레드에서 처리 되지 않은 목표-C 예외가 발생 하지 않으므로 목표-C 예외를 관리 되는 예외로 변환 하는 콜백이 호출 되지 않습니다.

이는 대부분의 UI 개체를 검사 하면 실행 중인 플랫폼에 없는 선택기에 해당 하는 속성을 페치 하기 때문에 Xamarin.ios에서 지 원하는 이전 macOS 버전에서 Xamarin.ios 앱을 디버그 하는 경우에도 매우 일반적입니다. Xamarin.ios에는 더 높은 macOS 버전에 대 한 지원이 포함 되어 있기 때문입니다. 이러한 선택기를 호출 하면 `NSInvalidArgumentException` ("인식할 수 없는 선택기 보냄 ...")이 throw 되어 프로세스가 충돌 합니다.

요약 하자면, 목표-C 런타임 또는이를 처리 하기 위해 프로그래밍할 수 없는 Mono 런타임 해제 프레임은 크래시, 메모리 누수 및 다른 유형의 예측할 수 없는 (mis) 동작 등의 정의 되지 않은 동작이 발생할 수 있습니다.

## <a name="solution"></a>솔루션

Xamarin.ios 10 및 Xamarin.ios 2.10에서는 관리 되는 네이티브 경계에서 관리 되는 및 목표 C 예외를 모두 catch 하 고 해당 예외를 다른 형식으로 변환 하기 위한 지원이 추가 되었습니다.

의사 코드에서 다음과 같이 표시 됩니다.

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

Objc_msgSend에 대 한 P/Invoke가 가로채 며 대신 호출 됩니다.

``` objective-c
void
xamarin_dyn_objc_msgSend (id obj, SEL sel)
{
    @try {
        objc_msgSend (obj, sel);
    } @catch (NSException *ex) {
        convert_to_and_throw_managed_exception (ex);
    }
}
```

반대의 경우도 마찬가지입니다 (관리 되는 예외를 목표-C 예외로 마샬링).

관리 되는 네이티브 경계에서 예외를 catch 하는 것은 비용이 들지 않으므로 항상 기본적으로 사용 하도록 설정 되지 않습니다.

- Xamarin.ios/tvOS: 시뮬레이터에서 목표-C 예외 가로채기를 사용할 수 있습니다.
- WatchOS: 가로채기는 모든 경우에 적용 됩니다 .이는 목표-C 런타임 해제 관리 되는 프레임을 사용 하 여 가비지 수집기를 혼동 하 고이를 중지 하거나 중단 시킬 수 있기 때문입니다.
- Xamarin.ios: 디버그 빌드에는 목표-C 예외 가로채기를 사용할 수 있습니다.

[빌드 타임 플래그](#build_time_flags) 섹션에서는 기본적으로 사용 하도록 설정 되지 않은 경우 가로채기를 사용 하도록 설정 하는 방법을 설명 합니다.

## <a name="events"></a>이벤트

예외가 가로채 `Runtime.MarshalManagedException` 면 발생 하는 두 개의 새 이벤트 인 및 `Runtime.MarshalObjectiveCException`가 있습니다.

두 이벤트 모두 throw 된 `EventArgs` 원래 예외 `Exception` ( `ExceptionMode` 속성)를 포함 하는 개체와 예외가 마샬링되는 방법을 정의 하는 속성을 전달 합니다.

처리기에서 수행 되는 사용자 지정 처리에 따라 동작을 변경 하려면 이벤트 처리기에서 속성을변경할수있습니다.`ExceptionMode` 한 가지 예는 특정 예외가 발생 하는 경우 프로세스를 중단 하는 것입니다.

`ExceptionMode` 속성을 변경 하는 것은 단일 이벤트에 적용 되 고 나중에 가로채는 예외에는 영향을 주지 않습니다.

사용할 수 있는 모드는 다음과 같습니다.

- `Default`: 기본값은 플랫폼에 따라 다릅니다. GC가 `ThrowObjectiveCException` 협조적 모드 (watchOS) 이면이 고 `UnwindNativeCode` , 그렇지 않으면 (iOS/watchOS/macos)입니다. 기본값은 나중에 변경 될 수 있습니다.
- `UnwindNativeCode`: 이전 (정의 되지 않은) 동작입니다. 협조적 모드로 GC를 사용 하는 경우에는이 옵션을 사용할 수 없습니다 .이 옵션은 watchOS의 유일한 옵션 이지만 watchOS에 대 한 올바른 옵션이 아니지만 다른 모든 플랫폼의 기본 옵션입니다.
- `ThrowObjectiveCException`: 관리 되는 예외를 목표-C 예외로 변환 하 고 목표-C 예외를 throw 합니다. WatchOS에 대 한 기본값입니다.
- `Abort`: 프로세스를 중단 합니다.
- `Disable`: 예외 가로채기를 사용 하지 않도록 설정 하므로 이벤트 처리기에서이 값을 설정 하는 것은 적합 하지 않습니다. 단, 이벤트가 발생 하면 너무 늦게 발생 하 여 사용 하지 않도록 설정할 수 있습니다. 어떤 경우 든 설정 하는 경우로 `UnwindNativeCode`동작 합니다.

관리 코드에 대 한 마샬링 목표-C 예외에는 다음 모드를 사용할 수 있습니다.

- `Default`: 기본값은 플랫폼에 따라 다릅니다. GC가 `ThrowManagedException` 협조적 모드 (watchOS) 이면이 고 `UnwindManagedCode` , 그렇지 않으면 (iOS/tvOS/macos)입니다. 기본값은 나중에 변경 될 수 있습니다.
- `UnwindManagedCode`: 이전 (정의 되지 않은) 동작입니다. 협조적 모드에서 GC를 사용 하는 경우에는이 옵션을 사용할 수 없습니다 .이는 watchOS에서 유일 하 게 유효한 GC 모드입니다 .이는 watchOS의 올바른 옵션이 아니지만 다른 모든 플랫폼의 기본값입니다.
- `ThrowManagedException`: 목표-C 예외를 관리 되는 예외로 변환 하 고 관리 되는 예외를 throw 합니다. WatchOS에 대 한 기본값입니다.
- `Abort`: 프로세스를 중단 합니다.
- `Disable`:D는 예외 가로채기를 가능 하 게 하므로 이벤트 처리기에서이 값을 설정 하는 것은 적합 하지 않지만 이벤트가 발생 한 후에는이 값을 사용 하지 않도록 설정 하는 것이 너무 늦습니다. 설정 된 경우 어떤 경우 든 프로세스가 중단 됩니다.

따라서 예외가 마샬링될 때마다 다음과 같은 작업을 수행할 수 있습니다.

``` csharp
Runtime.MarshalManagedException += (object sender, MarshalManagedExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling managed exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
    
};
Runtime.MarshalObjectiveCException += (object sender, MarshalObjectiveCExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling Objective-C exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
};
```

<a name="build_time_flags" />

## <a name="build-time-flags"></a>빌드 시간 플래그

다음 옵션을 **mtouch** (xamarin.ios 앱의 경우) 및 **mmp** (xamarin.ios 앱의 경우)에 전달 하 여 예외 가로채기를 사용 하도록 설정 했는지 여부를 확인 하 고 수행 해야 하는 기본 작업을 설정할 수 있습니다.

- `--marshal-managed-exceptions=`
  - `default`
  - `unwindnativecode`
  - `throwobjectivecexception`
  - `abort`
  - `disable`

- `--marshal-objectivec-exceptions=`
  - `default`
  - `unwindmanagedcode`
  - `throwmanagedexception`
  - `abort`
  - `disable`

을 제외 `MarshalManagedException` 하 고 이러한 값은 및 `MarshalObjectiveCException` 이벤트 `ExceptionMode` 에 전달 된 값과 동일 합니다. `disable`

옵션 `disable` 은 대부분의 경우에는 실행 오버 헤드를 추가 하지 않을 때 예외를 가로채는 점을 제외 하 고는 _대부분_ 가로채기를 사용 하지 않습니다. 마샬링 이벤트는 이러한 예외에 대해 계속 발생 하며 기본 모드는 실행 중인 플랫폼에 대 한 기본 모드입니다.

## <a name="limitations"></a>제한 사항

목표-C 예외를 catch 하려고 할 `objc_msgSend` 때 함수 제품군에 대 한 P/invoke만 가로챕니다. 즉, 모든 목표-C 예외를 throw 하는 다른 C 함수에 대 한 P/Invoke는 여전히 이전 및 정의 되지 않은 동작으로 실행 됩니다 .이는 나중에 향상 될 수 있습니다.

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc


## <a name="related-links"></a>관련 링크

- [예외 마샬링 (샘플)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
