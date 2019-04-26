---
title: Xamarin.iOS에서 마샬링 예외
description: 이 문서에서는 Xamarin.iOS 앱에 네이티브 및 관리 되는 예외를 사용 하는 방법을 설명 합니다. 발생할 수 있는 문제 및 이러한 문제에 대 한 솔루션에 설명 합니다.
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/05/2017
ms.openlocfilehash: 167d6ac421bdd2652e7f8474e1ea21bd9040723f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075097"
---
# <a name="exception-marshaling-in-xamarinios"></a>Xamarin.iOS에서 마샬링 예외

_Xamarin.iOS에는 특히 네이티브 코드에서에서 예외에 응답 하는 데 새 이벤트가 포함 됩니다._

관리 되는 코드와 Objective-c는 런타임 예외 (try/catch/finally 절)에 대 한 지원이 있습니다.

그러나 구현 이므로 다른는 런타임 라이브러리 (Mono 런타임 및 Objective C 런타임 라이브러리) 문제가 예외를 처리 한 다음 다른 언어로 작성 된 코드를 실행 해야 하는 경우.

이 문서에서는 발생할 수 있는 문제와 가능한 해결 방법을 설명 합니다.

또한 샘플 프로젝트 [예외 마샬링](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling), 다양 한 시나리오 및 솔루션 테스트에 사용할 수 있는 합니다.

## <a name="problem"></a>문제점

예외가 throw 되 고 발생 한 스택 프레임을 해제 하는 동안 throw 된 예외 형식의 일치 하지 않는 때 문제가 발생 합니다.

Xamarin.iOS 또는 Xamarin.Mac이 일반적인 예는 네이티브 API는 Objective-c 예외를 throw 하 고 스택 해제 프로세스는 관리 되는 프레임에 도달할 때 Objective-c 예외를 처리 어떤 이유로 든 해야 다음 경우 됩니다.

기본 동작은 아무 작업도 하지 않도록 합니다. 위의 샘플에서는 Objective-c 런타임에서 관리 되는 해제 프레임 수 있도록 의미 합니다. Objective-c 런타임에서 관리 되는 프레임을 해제 하는 방법을 알지 못하므로이 문제가 예를 들어이 하나라도 실행 하지 않습니다 `catch` 또는 `finally` 프레임 절.

### <a name="broken-code"></a>중단된 코드

다음 코드 예제를 살펴보세요.

``` csharp
var dict = new NSMutableDictionary ();
dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

네이티브 코드에는 Objective-c NSInvalidArgumentException throw 합니다.

    NSInvalidArgumentException *** setObjectForKey: key cannot be nil

및 스택 추적을 다음과 같이 표시 됩니다.

    0   CoreFoundation          __exceptionPreprocess + 194
    1   libobjc.A.dylib         objc_exception_throw + 52
    2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
    3   libobjc.A.dylib         objc_msgSend + 102
    4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
    5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
    6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()

0-3 프레임은 네이티브 프레임 및 스택 해제기 Objective-c 런타임에서 _수_ 추출한 프레임을 해제 합니다. 특히 모든 Objective-c 실행될지 `@catch` 또는 `@finally` 절.

그러나 Objective-c 스택 해제기는 _하지_ 프레임 스택이, 됩니다 하지만 관리 되는 예외 논리를 실행할 수 없습니다 (4 ~ 6 프레임) 관리 되는 프레임을 제대로 해제 지원 합니다.

즉,는 일반적으로 수 없는 다음과 같은 방식으로 이러한 예외를 catch 합니다.

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

Objective-c 스택 해제기는 관리 되는 방법에 대 한 알 수 없기 때문입니다 `catch` 모두 되며 절을 `finally` 절이 실행 됩니다.

때 위의 코드 샘플 _은_ 효과적으로 있기 때문에 처리 되지 않은 Objective C 예외를 알리는의 메서드를 Objective C [`NSSetUncaughtExceptionHandler`][2]이며 Xamarin.iOS 및 Xamarin.Mac 사용 및 해당 지점에서 모든 Objective C 예외는 관리 되는 예외를 변환 하려고 합니다.

## <a name="scenarios"></a>시나리오

### <a name="scenario-1---catching-objective-c-exceptions-with-a-managed-catch-handler"></a>시나리오 1-관리 되는 catch 처리기를 사용 하 여 Objective-c 예외 catch

다음 시나리오에서 Objective C 예외를 catch 하는 관리를 사용 하 여 `catch` 처리기:

1. Objective C 예외가 throw 됩니다.
2. Objective C 런타임 스택을 (하지만 해제 하지 않습니다) 네이티브 찾고 `@catch` 예외를 처리할 수 있는 처리기.
3. Objective-c 런타임에서 하나를 찾지 못하면 `@catch` 처리기를 호출 `NSGetUncaughtExceptionHandler`, Xamarin.iOS/Xamarin.Mac 설치자 처리기를 호출 합니다.
4. Xamarin.iOS/Xamarin.Mac's 처리기는 관리 되는 예외를 Objective C 예외 변환를 throw 합니다. Objective-c 이후 (해당 진행할)만 런타임 스택 해제 하지 않은, 현재 프레임은 동일한 Objective C 예외가 throw 되었습니다.

또 다른 문제는 Mono 런타임이 제대로 Objective-c 프레임을 해제 하는 방법을 모르기 때문에 여기에서 발생 합니다.

Xamarin.iOS' 확인할 수 없는 Objective-c 예외 콜백을 호출 스택은 다음과 같이 이동:

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

여기에서 유일한 관리 되는 프레임 프레임 8 ~ 10의 경우는 0 프레임에서 관리 되는 예외가 throw 됩니다. 이 Mono 런타임에 해제 되어야 네이티브 프레임이 0-7, 위에서 설명한 문제에 해당 하는 문제가 발생 하는 것을 의미 합니다: 모든 Objective-c를 실행 하지 않습니다 하지만 Mono 런타임이에서는 네이티브 프레임 해제를 `@catch` 또는 `@finally` 절 .

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

및 `@finally` 절에 대 한이 프레임을 해제 하는 Mono 런타임에 모르기 때문에 실행 되지 것입니다.

이 작업의 변형은 가져올 네이티브 프레임을 통해 해제 한 다음 관리 되는 코드에서 관리 되는 예외를 throw 하는 첫 번째 관리 되는 `catch` 절:

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

관리 되는 `UIApplication:Main` 메서드는 네이티브 호출 `UIApplicationMain` 메서드 및 iOS는 많은 결국 관리 되는 호출 하기 전에 네이티브 코드 실행을 수행 하는 `AppDelegate:FinishedLaunching` 네이티브 스택 프레임의 관리 되는 예외 때 많은 여전히 메서드 throw 됩니다.

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

0-1 프레임과 27 ~ 30 사이 이러한 모든 작업은 기본 관리 됩니다. 이러한 프레임 없음 Objective-c를 통해 Mono를 해제 하는 경우 `@catch` 또는 `@finally` 절이 실행 됩니다.

### <a name="scenario-2---not-able-to-catch-objective-c-exceptions"></a>시나리오 2-Objective C 예외를 catch 할 수 없습니다.

에서는 다음 시나리오 _되지_ Objective C 예외를 catch 할 수를 사용 하 여 관리 `catch` 처리기 Objective C 예외가 다른 방식으로 처리 되었습니다:

1. Objective C 예외가 throw 됩니다.
2. Objective C 런타임 스택을 (하지만 해제 하지 않습니다) 네이티브 찾고 `@catch` 예외를 처리할 수 있는 처리기.
3. Objective C 런타임은 `@catch` 처리기는 스택 및 실행이 시작 되는 `@catch` 처리기입니다.

이 시나리오는 있기 때문에 주 스레드에서 일반적으로 다음과 같은 코드 Xamarin.iOS 앱에서 흔히 발견 됩니다.

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

즉, 주 스레드에서 되지 실제로 Objective C 예외가 있고 따라서 Objective-c 예외 관리 되는 예외를 변환 하는 콜백이 호출 되지 않습니다.

또한 매우 일반적인 경우 Xamarin.Mac에서는 실행 중인 플랫폼 (에서 존재 하지 않는 선택기에 해당 하는 속성을 인출 하려고 디버거에서 대부분 UI 개체를 검사 하기 때문에 보다 이전 macOS 버전에서 Xamarin.Mac 앱을 디버깅 Xamarin.Mac 포함 되어 있으므로 더 높은 macOS 버전에 대 한 지원). 이러한 선택기 호출 시킵니다는 `NSInvalidArgumentException` ("인식할 수 없는 선택기 전송..."), 프로세스 충돌 하면 결국 합니다.

요약 하자면, Objective-c 런타임에서 또는 되도록 프로그래밍 하지 된 Mono 런타임 해제 프레임 있는 핸들 충돌, 메모리 누수 등 다른 형식의 예상 하지 못한 (mis) 동작은 정의 되지 않은 동작이 발생할 수 있습니다.

## <a name="solution"></a>솔루션

10 Xamarin.iOS 및 Xamarin.Mac 2.10에서 다른 형식으로 해당 예외를 변환 하는 것에 대 한 모든 관리 되는 네이티브 경계에서 관리 되는 및 Objective-c 예외를 catch 하는 것에 대 한 지원을 추가 했습니다.

의사 (pseudo) 코드에서 것 같습니다.

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

P/Invoke objc_msgSend 가로채어이 대신 호출 됩니다.

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

및 비슷한 (Objective-c 예외에 대 한 관리 되는 예외 마샬링) 역방향 사례에 대해 수행 됩니다.

관리 되는 네이티브 경계에서 예외를 catch 되지 비용 무료 이므로 항상 그렇지는 않습니다 기본적으로 사용 됩니다.

- Xamarin.iOS/tvOS: Objective-c 예외 인터 셉 션 시뮬레이터에서 사용 됩니다.
- Xamarin.watchOS: 인터 셉 션 모든 경우에 적용 됩니다 프레임 가비지 수집기에 혼동는 관리 되는 Objective-c 런타임에서 해제 하도록 허용 하기 때문에, 및 중지 또는 충돌 하거나 확인 합니다.
- Xamarin.Mac: 인터 셉 션 Objective C 예외는 디버그 빌드에 대해 활성화 됩니다.

합니다 [빌드 시간 플래그](#build_time_flags) 섹션에는 기본적으로 활성화 되지 않은 경우 인터 셉 션을 사용 하도록 설정 하는 방법을 설명 합니다.

## <a name="events"></a>이벤트

예외가 가로채 면 발생 하는 두 개의 새 이벤트가: `Runtime.MarshalManagedException` 고 `Runtime.MarshalObjectiveCException`입니다.

두 이벤트가 전달 될를 `EventArgs` throw 된 원래 예외를 포함 하는 개체 (합니다 `Exception` 속성), 및 `ExceptionMode` 예외 마샬링되어야 하는 방법을 정의 하는 속성입니다.

`ExceptionMode` 속성을 변경할 수는 이벤트 처리기에서 수행 된 모든 사용자 지정 처리에 따라 동작을 변경 하는 처리기. 예로 특정 예외를 발생 하는 경우 프로세스를 중단 하는 것입니다.

변경 된 `ExceptionMode` 단일 이벤트에 적용 되는 속성, 모든 예외를 가로챌 나중에 영향을 주지 않습니다.

다음 모드를 사용할 수 있습니다.

- `Default`: 기본은 플랫폼에 따라 다릅니다. 것 `ThrowObjectiveCException` GC (watchOS) 협조적 모드인 경우 및 `UnwindNativeCode` 그렇지 않은 경우 (iOS / watchOS / macOS). 기본값은 나중에 변경할 수 있습니다.
- `UnwindNativeCode`: 이 동작은 이전 (정의 되지 않음)입니다. 사용할 수 없는 협조적 모드에서 GC를 사용 하는 경우 (watchOS에서 유일한 옵션에 따라서 아닙니다 watchOS에서 유효한 옵션), 다른 모든 플랫폼에 대 한 기본 옵션 이지만 합니다.
- `ThrowObjectiveCException`: Objective C 예외가 관리 되는 예외를 변환 및 Objective-c 예외를 throw 합니다. WatchOS에서 기본값입니다.
- `Abort`: 프로세스를 중단 합니다.
- `Disable`: 이벤트 처리기에서이 값을 설정 하려면 적합 하지 않습니다 하지만 사용 하지 않도록 늦는 이벤트가 발생 되 면 예외 가로채기를 사용 하지 않도록 설정 합니다. 어떤 경우 든 경우 설정 처럼 동작 `UnwindNativeCode`합니다.

관리 코드에 대 한 Objective-c 예외 마샬링에 대 한 다음 모드는 다음과 같은 작업 사용할 수 있습니다.

- `Default`: 기본은 플랫폼에 따라 다릅니다. 것 `ThrowManagedException` GC (watchOS) 협조적 모드인 경우 및 `UnwindManagedCode` 그렇지 않은 경우 (iOS / tvOS / macOS). 기본값은 나중에 변경할 수 있습니다.
- `UnwindManagedCode`: 이 동작은 이전 (정의 되지 않음)입니다. 사용할 수 없는 협조적 모드에서 GC를 사용 하는 경우 (watchOS에만 유효한 GC 모드는; 따라서이 유효한 옵션이 아닙니다 watchOS에서), 다른 모든 플랫폼에 대 한 기본값 이지만 합니다.
- `ThrowManagedException`: Objective-c 예외 관리 되는 예외를 변환 하 고 관리 되는 예외를 throw 합니다. WatchOS에서 기본값입니다.
- `Abort`: 프로세스를 중단 합니다.
- `Disable`: 예외를 가로채 사용 하지 않도록 설정, 설정에 적합 하지 않습니다 있도록 이벤트 처리기를 수 있지만 한 번 이벤트에서 발생 하는 값이 너무 늦게 사용 하지 않도록 하는 것입니다. 어떤 경우 든 경우 설정 하는 프로세스를 중단 합니다.

따라서 예외가 마샬링됩니다 때마다를 보려면이 수행할 수 있습니다.

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

다음 옵션을 전달할 수 있기 **mtouch** (용 Xamarin.iOS 앱) 및 **mmp** (Xamarin.Mac 앱의 경우)에 대 한는 확인 예외 인터 셉 션을 사용 하는 경우 및 기본 작업 설정 발생 합니다.

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

제외 하 고 `disable`, 이러한 값은 동일 합니다 `ExceptionMode` 에 전달 되는 값을 `MarshalManagedException` 및 `MarshalObjectiveCException` 이벤트입니다.

합니다 `disable` 옵션이 됩니다 _대부분_ 모든 실행 오버 헤드를 추가 하지 않습니다 하는 경우에 여전히 예외를 가로채 됩니다 것 점을 제외 하 고 인터 셉 션을 사용 하지 않도록 설정 합니다. 마샬링 이벤트는 여전히 실행 중인 플랫폼에 대 한 기본 모드 되 고 기본 모드를 사용 하 여 이러한 예외에 대해 발생 됩니다.

## <a name="limitations"></a>제한 사항

에서는 P/Invoke를 가로챌만 `objc_msgSend` Objective C 예외를 catch 하려고 할 때 함수의 제품군입니다. 즉, 다음 Objective C 예외를 throw 하는 다른 C 함수에 P/Invoke (이 향상 될 수 있습니다 나중에) 이전 및 정의 되지 않은 동작을 계속 실행 됩니다.

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc


## <a name="related-links"></a>관련 링크

- [예외 마샬링 (샘플)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
