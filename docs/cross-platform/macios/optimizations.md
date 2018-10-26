---
title: 빌드 최적화
description: 이 문서에서는 Xamarin.iOS 및 Xamarin.Mac 앱에 대 한 빌드 시간에 적용 되는 다양 한 최적화를 설명 합니다.
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
author: conceptdev
ms.author: crdun
ms.date: 04/16/2018
ms.openlocfilehash: 7d67b924253dfea66781f16b5f83007811de5909
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119035"
---
# <a name="build-optimizations"></a>빌드 최적화

이 문서에서는 Xamarin.iOS 및 Xamarin.Mac 앱에 대 한 빌드 시간에 적용 되는 다양 한 최적화를 설명 합니다.

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>UIApplication.EnsureUIThread 제거 / NSApplication.EnsureUIThread

에 대 한 호출을 제거 [UIApplication.EnsureUIThread] [ 1] (Xamarin.iOS)에 대 한 또는 `NSApplication.EnsureUIThread` (Xamarin.Mac)에 대 한 합니다.

이 최적화는 다음과 같은 유형의 코드를 변경 합니다.

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    global::UIKit.UIApplication.EnsureUIThread ();
    // ...
}
```

에 다음과 같이 변경 합니다

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    // ...
}
```

이 최적화를 사용 하도록 링커에 필요와 방법만 적용 됩니다는 `[BindingImpl (BindingImplOptions.Optimizable)]` 특성입니다.

릴리스에 대 한 사용 하도록 설정 하는 기본적으로 빌드합니다.

전달 하 여 기본 동작을 재정의할 수 있습니다 `--optimize=[+|-]remove-uithread-checks` mtouch/mmp 하 합니다.

[1]: https://developer.xamarin.com/api/member/UIKit.UIApplication.EnsureUIThread/

## <a name="inline-intptrsize"></a>인라인 IntPtr.Size

인라인 상수 값의 `IntPtr.Size` 대상 플랫폼에 따라 합니다.

이 최적화는 다음과 같은 유형의 코드를 변경 합니다.

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

에 다음 (64 비트 플랫폼에 대 한 빌드) 하는 경우:

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

이 최적화를 사용 하도록 링커에 필요와 방법만 적용 됩니다는 `[BindingImpl (BindingImplOptions.Optimizable)]` 특성입니다.

플랫폼 어셈블리 또는 기본적으로 한 아키텍처를 대상으로 하는 경우 활성화 됩니다 (**Xamarin.iOS.dll**를 **Xamarin.TVOS.dll**를 **Xamarin.WatchOS.dll** 또는 **Xamarin.Mac.dll**).

여러 아키텍처를 대상으로 하는 경우이 최적화는 32 비트 버전과 64 비트 버전의 앱을 위한 다른 어셈블리를 만들고 두 버전 모두 효과적으로 감소 하지 않고 최종 앱 크기를 늘리면 앱에 포함 해야 합니다. 있습니다.

전달 하 여 기본 동작을 재정의할 수 있습니다 `--optimize=[+|-]inline-intptr-size` mtouch/mmp 하 합니다.

## <a name="inline-nsobjectisdirectbinding"></a>인라인 NSObject.IsDirectBinding

`NSObject.IsDirectBinding` 특정 인스턴스 래퍼 형식 인지 여부를 확인 하는 인스턴스 속성 (래퍼 형식을 네이티브 형식에 매핑되는 관리 되는 형식인;에 대 한 관리 되는 인스턴스 `UIKit.UIView` 네이티브에 매핑됩니다 `UIView` 형식이-반대 사용자 형식인 여기서 `class MyUIView : UIKit.UIView` 사용자 형식 이어야 합니다).

값은 알지만 필요가 `IsDirectBinding` 값의 버전을 결정 하므로 Objective-c를 호출할 때 `objc_msgSend` 사용 하도록 합니다.

다음 코드를 지정 합니다.

```csharp
class UIView : NSObject {
    public virtual string SomeProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class NSUrl : NSObject {
    public virtual string SomeOtherProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class MyUIView : UIView {
}
```

확인할 수 있습니다 `UIView.SomeProperty` 변수의 `IsDirectBinding` 상수가 아닙니다. 및 인라인 될 수 없습니다.

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

그러나 것 수의 앱 형식 모든 보기에서 상속 된 형식이 있는지 확인 `NSUrl`, 인라인을 안전 하 게 되므로 합니다 `IsDirectBinding` 상수 값 `true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

이 최적화는 다음과 같은 유형의 코드를 변경 하는 특히 (이 대 한 바인딩 코드는 `NSUrl.AbsoluteUrl`):

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

다음으로 (때 확인할 수 없는 하위 클래스는 `NSUrl` 앱에서):

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

이 최적화를 사용 하도록 링커에 필요와 방법만 적용 됩니다는 `[BindingImpl (BindingImplOptions.Optimizable)]` 특성입니다.

Xamarin.iOS에 대 한 항상 기본적으로 사용 되 고 Xamarin.Mac 용 기본적으로 항상 비활성화 (동적으로 Xamarin.Mac의 어셈블리를 로드할 수 이기 때문에 수 없는 특정 클래스 서브클래싱된 되지 있는지 확인 하려면).

전달 하 여 기본 동작을 재정의할 수 있습니다 `--optimize=[+|-]inline-isdirectbinding` mtouch/mmp 하 합니다.

## <a name="inline-runtimearch"></a>인라인 Runtime.Arch

이 최적화는 다음과 같은 유형의 코드를 변경 합니다.

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

에 다음 (장치에 대 한 빌드) 하는 경우:

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

이 최적화를 사용 하도록 링커에 필요와 방법만 적용 됩니다는 `[BindingImpl (BindingImplOptions.Optimizable)]` 특성입니다.

Xamarin.iOS (사용할 수 없는 Xamarin.Mac 용)에 대 한 항상 기본적으로 활성화 됩니다.

전달 하 여 기본 동작을 재정의할 수 있습니다 `--optimize=[+|-]inline-runtime-arch` mtouch를 합니다.

## <a name="dead-code-elimination"></a>데드 코드 제거

이 최적화는 다음과 같은 유형의 코드를 변경 합니다.

```csharp
if (true) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

붙여넣을 위치:

```csharp
Console.WriteLine ("Doing this");
```

상수 비교의 경우 다음과 같은 것도 평가:

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

확인 및 식 `8 == 8` 되는 항상 true 및 축소 하:

```csharp
Console.WriteLine ("Doing this");
```

이 강력한 최적화 인라인 최적화를 함께 사용 하는 경우 다음과 같은 유형의 코드를 변환할 수 있으므로 (이 대 한 바인딩 코드는 `NFCIso15693ReadMultipleBlocksConfiguration.Range`):

```csharp
NSRange ret;
if (IsDirectBinding) {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret (out ret, this.Handle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    }
} else {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret (out ret, this.SuperHandle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    }
}
return ret;
```

이 (또한 수 없는지 확인 하는 경우 및 64 비트 장치에 대해 빌드할 때 없습니다 `NFCIso15693ReadMultipleBlocksConfiguration` 앱의 서브 클래스).

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

AOT 컴파일러는 이러한 데드 코드가 제거 수행 할 이미 있지만이 최적화는 링커는 더 이상 사용 되지 않습니다 하 고 있으므로 제거할 수 있습니다 (사용 하지 않는 한 다른 곳에서) 하는 여러 메서드가 있는지 볼 수 있음을 의미 하는 링커 내에서 수행 됩니다. :

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

이 최적화를 사용 하도록 링커에 필요와 방법만 적용 됩니다는 `[BindingImpl (BindingImplOptions.Optimizable)]` 특성입니다.

(링커 사용 가능) 하는 경우 항상 기본적으로 활성화 됩니다.

전달 하 여 기본 동작을 재정의할 수 있습니다 `--optimize=[+|-]dead-code-elimination` mtouch/mmp 하 합니다.

## <a name="optimize-calls-to-blockliteralsetupblock"></a>BlockLiteral.SetupBlock에 대 한 호출을 최적화

Xamarin.iOS/Mac 런타임 대리자 블록 서명이 관리 되는 Objective-c로 블록을 만들 때 알고 있어야 합니다. 이 인해 매우 많은 작업을 수 있습니다. 이 최적화는 빌드 시 블록 서명을 계산 및 호출 하는 IL을 수정 된 `SetupBlock` 대신 인수로 서명을 사용 하는 메서드. 이 런타임 시 서명 계산에 대 한 필요가 없습니다.

벤치 마크가 10 ~ 15 배 블록 호출 속도 보여 줍니다.

다음 변환 것 [코드](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211):

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlock (callback, completionHandler);
    // ...
}
```

붙여넣을 위치:

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlockImpl (callback, completionHandler, true, "v@?B");
    // ...
}
```

이 최적화를 사용 하도록 링커에 필요와 방법만 적용 됩니다는 `[BindingImpl (BindingImplOptions.Optimizable)]` 특성입니다.

정적 등록 기관 (Xamarin.iOS 정적 등록 기관 장치 빌드에 대 한 기본으로 활성화 되 고 xamarin.mac 정적 등록 기관은 릴리스에 대 한 기본적으로 활성화 되어 빌드)에서 사용 하는 경우 기본적으로 활성화 됩니다.

전달 하 여 기본 동작을 재정의할 수 있습니다 `--optimize=[+|-]blockliteral-setupblock` mtouch/mmp 하 합니다.

## <a name="optimize-support-for-protocols"></a>프로토콜에 대 한 지원을 최적화합니다

Xamarin.iOS/Mac 런타임에서 관리 되는 형식 구현 Objective-c 프로토콜 정보가 필요합니다. 인터페이스 (및 이러한 인터페이스의 특성)에서이 정보는 저장 하는 매우 효율적인 형식 없는 되거나 링커 친화적입니다.

한 가지 예는 이러한 인터페이스의 모든 프로토콜 멤버에 대 한 정보를 저장 하는 `[ProtocolMember]` 무엇 보다도 해당 멤버의 매개 변수 형식에 대 한 참조를 포함 하는 특성입니다. 즉, 해당 인터페이스에 사용 되는 모든 형식을 유지 링커 하 게 이러한 인터페이스를 구현 하기만 선택적 멤버에 대해서도 앱 되지 호출 또는 구현 합니다.

이 최적화는 쉽고 빠른 런타임 시 찾을 수 있는 작은 메모리를 사용 하는 효율적인 형식으로 필요한 정보를 저장 합니다. 정적 등록자를 수행 합니다.

대해서도 배울 링커는이 반드시 않아도 이러한 인터페이스와 관련된 특성을 유지 합니다.

이 최적화는 링커와 정적 등록 기관은 사용 하도록 설정 해야 합니다.

Xamarin.iOS 링커 및 정적 등록 기관에 설정 된 경우이 최적화 기본적으로 사용 됩니다.

Xamarin.Mac로이 최적화가 있으며 때문에 어셈블리를 동적으로 로드 Xamarin.Mac 지원 하 고 해당 어셈블리 수 하지 되었습니다 빌드 시 알려지지 (따라서 최적화 되지 않은) 기본적으로 사용 되지 않습니다.

전달 하 여 기본 동작을 재정의할 수 있습니다 `--optimize=-register-protocols` mtouch/mmp 하 합니다.

## <a name="remove-the-dynamic-registrar"></a>동적 등록자를 제거 합니다.

Xamarin.iOS 및 Xamarin.Mac 런타임 모두에 대 한 지원을 포함 [관리 되는 형식 등록](https://developer.xamarin.com/guides/ios/advanced_topics/registrar/) Objective C 런타임을 사용 합니다. 빌드 시 또는 런타임에 (또는 빌드 시간 및 런타임 시 rest에서 부분적으로)에 하거나 수행할 수 있지만 완전히 빌드 시간에 완료 되 면 런타임 시 작업을 위한 지원 코드를 제거할 수 있습니다. 이 인해 앱 크기가 크게 감소 되는 특히 확장과 같은 더 작은 앱 또는 watchOS 앱에 대 한 합니다.

이 최적화는 정적 등록 기관은 링커에서 사용 하도록 설정 해야 합니다.

링커를 제거 하려고 하는 경우 확인 하 고 동적 등록자를 제거 해도 인지 확인 하려고 합니다.

Xamarin.Mac을 지 원하는 동적으로 (빌드 시 알려지지 않은)는 런타임에 어셈블리를 로드할 되므로 안전 하 게 최적화 인지 여부를 빌드할 때 확인할 수 없습니다. 즉, Xamarin.Mac 앱에 대해 기본적으로이 최적화가 사용 되지 않습니다는 합니다.

전달 하 여 기본 동작을 재정의할 수 있습니다 `--optimize=[+|-]remove-dynamic-registrar` mtouch/mmp 하 합니다.

동적 등록자를 제거 하려면 기본 재정의 되는 링커가 내보낼 경고는 감지 되 면은 안전 하지 않습니다 (하지만 동적 등록자 제거 됩니다).

## <a name="inline-runtimedynamicregistrationsupported"></a>인라인 Runtime.DynamicRegistrationSupported

인라인 값의 `Runtime.DynamicRegistrationSupported` 빌드 시 결정 합니다.

동적 등록자 제거 된 경우 (참조를 [동적 등록자 제거](#remove-the-dynamic-registrar) 최적화),이 상수 `false` 값에 상수가 고, 그렇지 `true` 값입니다.

이 최적화는 다음과 같은 유형의 코드를 변경 합니다.

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

동적 등록 기관에서 제거 되 면 다음과 같이에

```csharp
throw new Exception ("dynamic registration is not supported");
```

으로 동적 등록자 제거 되지 않은 경우:

```csharp
Console.WriteLine ("do something");
```

이 최적화를 사용 하도록 링커에 필요와 방법만 적용 됩니다는 `[BindingImpl (BindingImplOptions.Optimizable)]` 특성입니다.

(링커 사용 가능) 하는 경우 항상 기본적으로 활성화 됩니다.

전달 하 여 기본 동작을 재정의할 수 있습니다 `--optimize=[+|-]inline-dynamic-registration-supported` mtouch/mmp 하 합니다.

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Precompute Objective-c 블록에 대 한 관리 되는 대리자를 만드는 방법

매개 변수로 블록을 사용 하면 Objective-c 선택기를 호출 하 고 다음 관리 코드는 재정의 된 메서드는 Xamarin.iOS / Xamarin.Mac 런타임은 해당 블록에 대 한 대리자를 만드는 해야 합니다.

바인딩 생성기에서 생성 된 바인딩 코드 포함 됩니다는 `[BlockProxy]` 특성을 사용 하 여 형식 지정을 `Create` 이 수행할 수 있는 메서드.

다음 Objective-c 코드를 지정 합니다.

```objc
@interface ObjCBlockTester : NSObject {
}
-(void) classCallback: (void (^)())completionHandler;
-(void) callClassCallback;
@end

@implementation ObjCBlockTester
-(void) classCallback: (void (^)())completionHandler
{
}

-(void) callClassCallback
{
    [self classCallback: ^()
    {
        NSLog (@"called!");
    }];
}
@end
```

및 다음과 같은 바인딩 코드:

```csharp
[BaseType (typeof (NSObject))]
interface ObjCBlockTester
{
    [Export ("classCallback:")]
    void ClassCallback (Action completionHandler);
}
```

생성기를 생성 합니다.

```csharp
[Register("ObjCBlockTester", true)]
public unsafe partial class ObjCBlockTester : NSObject {
    // unrelated code...

    [Export ("callClassCallback")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public virtual void CallClassCallback ()
    {
        if (IsDirectBinding) {
            ApiDefinition.Messaging.void_objc_msgSend (this.Handle, Selector.GetHandle ("callClassCallback"));
        } else {
            ApiDefinition.Messaging.void_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("callClassCallback"));
        }
    }
    
    [Export ("classCallback:")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public unsafe virtual void ClassCallback ([BlockProxy (typeof (Trampolines.NIDActionArity1V0))] System.Action completionHandler)
    {
        // ...
        
    }
}

static class Trampolines
{
    [UnmanagedFunctionPointerAttribute (CallingConvention.Cdecl)]
    [UserDelegateType (typeof (System.Action))]
    internal delegate void DActionArity1V0 (IntPtr block);

    static internal class SDActionArity1V0 {
        static internal readonly DActionArity1V0 Handler = Invoke;
        
        [MonoPInvokeCallback (typeof (DActionArity1V0))]
        static unsafe void Invoke (IntPtr block) {
            var descriptor = (BlockLiteral *) block;
            var del = (System.Action) (descriptor->Target);
            if (del != null)
                del (obj);
        }
    }
    
    internal class NIDActionArity1V0 {
        IntPtr blockPtr;
        DActionArity1V0 invoker;
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe NIDActionArity1V0 (BlockLiteral *block)
        {
            blockPtr = _Block_copy ((IntPtr) block);
            invoker = block->GetDelegateForBlock<DActionArity1V0> ();
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        ~NIDActionArity1V0 ()
        {
            _Block_release (blockPtr);
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe static System.Action Create (IntPtr block)
        {
            if (block == IntPtr.Zero)
                return null;
            if (BlockLiteral.IsManagedBlock (block)) {
                var existing_delegate = ((BlockLiteral *) block)->Target as System.Action;
                if (existing_delegate != null)
                    return existing_delegate;
            }
            return new NIDActionArity1V0 ((BlockLiteral *) block).Invoke;
        }
        
        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        unsafe void Invoke ()
        {
            invoker (blockPtr);
        }
    }
}
```

Objective-c로 호출 하는 경우 `[ObjCBlockTester callClassCallback]`에 Xamarin.iOS Xamarin.Mac 런타임 살펴봅니다 /는 `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` 매개 변수에 특성입니다. 한 후 표시 됩니다는 `Create` 해당 형식에 메서드 대리자를 만드는 메서드를 호출 하 고 있습니다.

이 최적화를 찾을 수는 `Create` 빌드 시간 및 정적 등록 기관에서 메서드를 대신 특성 및 리플렉션 (속도가 훨씬 빨라지고 이며 있습니다 다음 링커를 사용 하 여 메타 데이터 토큰을 사용 하 여 런타임에 메서드를 찾는 코드를 생성 합니다 앱을 더 작게 해당 런타임 코드를 제거) 합니다.

Mmp/mtouch를 찾을 수 없는 경우는 `Create` MT4174/MM4174 경고가 표시 될 다음 메서드와 런타임에 대신 조회를 수행 합니다.
주요 원인 없이 필요한 바인딩 코드를 수동으로 기록 됩니다 `[BlockProxy]` 특성입니다.

이 최적화 하도록 정적 등록 기관에 필요 합니다.

(으로 정적 등록 기관에 사용 됨) 항상 기본적으로 활성화 됩니다.

전달 하 여 기본 동작을 재정의할 수 있습니다 `--optimize=[+|-]static-delegate-to-block-lookup` mtouch/mmp 하 합니다.
