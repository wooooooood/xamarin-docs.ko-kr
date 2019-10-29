---
title: Xamarin.ios의 목적-C 선택기
description: 이 문서에서는에서 C#목표-C 선택기와 상호 작용 하는 방법을 설명 합니다. 이를 수행할 때 고려해 야 하는 선택기 및 기술적 고려 사항을 호출 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/12/2017
ms.openlocfilehash: 79f226c137c3ab6b1dd2de9f92cb868056aa9d59
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022286"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Xamarin.ios의 목적-C 선택기

목표-C 언어는 *선택기*를 기반으로 합니다. 선택기는 개체 또는 *클래스로*보낼 수 있는 메시지입니다. [Xamarin.ios](~/ios/internals/api-design/index.md) 는 인스턴스 선택기와 정적 메서드에 대 한 클래스 선택기를 인스턴스 메서드에 매핑합니다.

일반적인 C 함수와 달리 멤버 함수와 마찬가지로 C++ 선택기는 [P/invoke](https://www.mono-project.com/docs/advanced/pinvoke/) 를 사용 하 여 직접 호출할 수 없으며 선택기는를 사용 하 여 객관적인 c 클래스 또는 인스턴스로 전송 됩니다 [`objc_msgSend`](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)
칩셋용으로.

목적-C의 메시지에 대 한 자세한 내용은 Apple의 [개체 작업](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW2) 가이드를 참조 하세요.

## <a name="example"></a>예제

[`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont) 를 호출 한다고 가정 합니다.
[`NSString`](https://developer.apple.com/documentation/foundation/nsstring)선택기입니다.
Apple의 설명서에서 선언은 다음과 같습니다.

```objc
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

이 API에는 다음과 같은 특징이 있습니다.

- 반환 형식은 Unified API `CGSize` 됩니다.
- 매개 변수 `font`는 [uifont](xref:UIKit.UIFont) [nsobject](xref:Foundation.NSObject)에서 파생 된 형식 (간접적) 이며, 이는 [System.IntPtr](xref:System.IntPtr)에 매핑됩니다.
- `CGFloat``width` 매개 변수는 `nfloat`에 매핑됩니다.
- [`UILineBreakMode`](https://developer.apple.com/documentation/uikit/uilinebreakmode?language=objc)`lineBreakMode` 매개 변수는 이미 xamarin.ios에서 [`UILineBreakMode`](xref:UIKit.UILineBreakMode) 으로 바인딩 되었습니다.
열거할.

모두 함께 배치 하면 `objc_msgSend` 선언이 일치 해야 합니다.

```csharp
CGSize objc_msgSend(
    IntPtr target, 
    IntPtr selector, 
    IntPtr font, 
    nfloat width, 
    UILineBreakMode mode
);
```

다음과 같이 선언 합니다.

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, 
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

이 메서드를 호출 하려면 다음과 같은 코드를 사용 합니다.

```csharp
NSString target = ...
Selector selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont font = ...
nfloat width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, 
    selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode
);
```

반환 된 값의 크기가 8 바이트 보다 작은 구조 (통합 Api로 전환 하기 전에 사용 되는 이전 `SizeF`) 였습니다. 위의 코드는 시뮬레이터에서 실행 되었지만 장치에서 충돌 합니다. 크기가 8 비트 보다 작은 값을 반환 하는 선택기를 호출 하려면 `objc_msgSend_stret` 함수를 선언 합니다.

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, 
    IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode
);
```

이 메서드를 호출 하려면 다음과 같은 코드를 사용 합니다.

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, 
        selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode
    );
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode
    );
```

## <a name="invoking-a-selector"></a>선택기 호출

선택기를 호출 하면 세 단계가 있습니다.

1. 선택기 대상을 가져옵니다.
2. 선택기 이름을 가져옵니다.
3. 적절 한 인수를 사용 하 여 `objc_msgSend`를 호출 합니다.

### <a name="selector-targets"></a>선택기 대상

선택기 대상은 개체 인스턴스이거나 목표-C 클래스입니다. 대상이 인스턴스이고 바인딩된 Xamarin.ios 형식에서 제공 되는 경우 [`ObjCRuntime.INativeObject.Handle`](xref:ObjCRuntime.INativeObject.Handle) 속성을 사용 합니다.

대상이 클래스인 경우 [`ObjCRuntime.Class`](xref:ObjCRuntime.Class) 를 사용 하 여 클래스 인스턴스에 대 한 참조를 가져온 다음 [`Class.Handle`](xref:ObjCRuntime.Class.Handle) 속성을 사용 합니다.

### <a name="selector-names"></a>선택기 이름

선택기 이름은 Apple 설명서에 나와 있습니다. 예를 들어 [`NSString`](https://developer.apple.com/documentation/foundation/nsstring?language=objc) [`sizeWithFont:`](https://developer.apple.com/documentation/foundation/nsstring/1619917-sizewithfont?language=objc) 및 [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont?language=objc) 선택기를 포함 합니다. 포함 된 콜론 및 후행 콜론은 선택기 이름의 일부 이며 생략할 수 없습니다.

선택기 이름이 있으면이에 대 한 [`ObjCRuntime.Selector`](xref:ObjCRuntime.Selector) 인스턴스를 만들 수 있습니다.

### <a name="calling-objc_msgsend"></a>Objc_msgSend 호출

`objc_msgSend`는 개체에 메시지 (선택기)를 보냅니다. 이 함수 패밀리에는 선택기 대상 (인스턴스 또는 클래스 핸들), 선택기 자체 및 선택기에 필요한 모든 인수의 두 개 이상의 필수 인수가 사용 됩니다. 인스턴스 및 선택기 인수는 `System.IntPtr`해야 하며 나머지 인수는 모두 선택기에 필요한 형식 (예: `int`에 대 한 `nint` 또는 모든 `NSObject`파생 형식에 대 한 `System.IntPtr`)과 일치 해야 합니다. [`NSObject.Handle`](xref:Foundation.NSObject.Handle) 사용
목적-C 형식 인스턴스의 `IntPtr`을 가져오기 위한 속성입니다.

`objc_msgSend` 함수는 두 개 이상 있습니다.

- 구조체를 반환 하는 선택기에 [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc) 를 사용 합니다. ARM에는 열거형 또는 C 기본 제공 형식 (`char`, `short`, `int`, `long`, `float``double`)이 아닌 모든 반환 형식이 포함 됩니다. X86 (시뮬레이터)에서이 메서드는 크기가 8 바이트 보다 큰 모든 구조체에 사용 해야 합니다 (`CGSize` 8 바이트 이며 시뮬레이터에서 `objc_msgSend_stret`를 사용 하지 않음). 
- X 86의 부동 소수점 값을 반환 하는 선택기에 [`objc_msgSend_fpret`](https://developer.apple.com/documentation/objectivec/1456697-objc_msgsend_fpret?language=objc) 를 사용 합니다. 이 함수는 ARM에서 사용할 필요가 없습니다. 대신 `objc_msgSend`를 사용 합니다. 
- 주 [objc_msgSend](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend) 함수는 다른 모든 선택기에 사용 됩니다.

호출 해야 하는 `objc_msgSend` 함수를 결정 한 후에는 시뮬레이터와 장치에서 각각 다른 메서드를 필요로 할 수 있습니다. 일반적 [`[DllImport]`](xref:System.Runtime.InteropServices.DllImportAttribute) 메서드를 사용 하 여 나중에 호출할 수 있도록 함수를 선언할 수 있습니다.

미리 만들어진 `objc_msgSend` 선언 집합은 `ObjCRuntime.Messaging`에서 찾을 수 있습니다.

## <a name="different-invocations-on-simulator-and-device"></a>시뮬레이터 및 장치에 대 한 여러 호출

위에서 설명한 것 처럼, 목표 C에는 일반 호출에 대해 하나, 부동 소수점 값을 반환 하는 호출 (x 86에만 해당), 구조체 값을 반환 하는 호출에 대 한 `objc_msgSend` 메서드의 세 가지 종류가 있습니다. 후자는 `ObjCRuntime.Messaging`에 `_stret` 접미사를 포함 합니다.

특정 구조체를 반환 하는 메서드를 호출 하는 경우 (아래 설명 된 규칙) 반환 값을 `out` 값으로 첫 번째 매개 변수로 사용 하 여 메서드를 호출 해야 합니다.

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

`_stret_` 메서드를 사용 하는 시기에 대 한 규칙은 x86 및 ARM에 따라 다릅니다.
시뮬레이터와 장치 모두에서 바인딩이 작동 하도록 하려면 다음과 같은 코드를 추가 합니다.

```csharp
if (Runtime.Arch == Arch.DEVICE)
{
    PointF ret;
    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);
    return ret;
} 
else
{
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
}
```

### <a name="using-the-objc_msgsend_stret-method"></a>Objc_msgSend_stret 메서드 사용

ARM 용으로 빌드하는 경우 [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc) 를 사용 합니다.
열거형이 아닌 값 형식 또는 열거형에 대 한 기본 형식 (`int`, `byte`, `short`, `long`, `double`, `float`)

X 86 용으로 빌드하는 경우 [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc) 사용
열거형이 아닌 값 형식 또는 열거형 (`int`, `byte`, `short`, `long`, `double`, `float`)의 기본 형식 또는 기본 크기가 8 바이트 보다 큰 경우입니다.

### <a name="creating-your-own-signatures"></a>사용자 고유의 서명 만들기

필요한 경우 다음 [요점](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) 를 사용 하 여 고유한 서명을 만들 수 있습니다.

## <a name="related-links"></a>관련 링크

- [목표-C 선택기](https://developer.xamarin.com/samples/mac-ios/Objective-C/) 샘플
