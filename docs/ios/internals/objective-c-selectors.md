---
title: Xamarin.iOS에서 Objective-c 선택기
description: 이 문서에서는 C#에서 Objective-c 선택기와 상호 작용 하는 방법을 설명 합니다. 이렇게 하면 계정에 수행 해야 하는 기술적인 고려 사항과 선택기를 호출 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/12/2017
ms.openlocfilehash: cf39d548dc83fae67e8703d42e9387b8f19504e6
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669755"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Xamarin.iOS에서 Objective-c 선택기

Objective C 언어를 기반으로 *선택기*합니다. 선택기는 개체에 보낼 수 있는 메시지 또는 *클래스*합니다. [Xamarin.iOS](~/ios/internals/api-design/index.md) 맵 인스턴스 메서드에 선택기 인스턴스 및 정적 메서드를 선택기 클래스입니다.

일반 C 함수와 달리 (및 c + + 멤버 함수와 마찬가지로)을 호출할 수 없습니다 직접 사용 하 여 선택기 [P/Invoke](https://www.mono-project.com/docs/advanced/pinvoke/) 대신 선택기 Objective-c 클래스에 보내거나를 사용 하 여 인스턴스를 [`objc_msgSend`](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend)
함수입니다.

Objective C에서 메시지에 대 한 자세한 내용은 살펴보겠습니다 Apple [개체를 사용 하 여 작업](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html#//apple_ref/doc/uid/TP40011210-CH4-SW2) 가이드입니다.

## <a name="example"></a>예제

호출 하기를 원한다고 가정 합니다 [`sizeWithFont:forWidth:lineBreakMode:`](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont)
선택기 [ `NSString` ](https://developer.apple.com/documentation/foundation/nsstring)합니다.
Apple 설명서) (에서 선언은 다음과 같습니다.

```objc
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

이 API에는 다음과 같은 특징이 있습니다.

- 반환 형식은 `CGSize` Unified API에 대 한 합니다.
- 합니다 `font` 매개 변수는를 [UIFont](xref:UIKit.UIFont) (및 형식 (직접)에서 파생 된 [NSObject](xref:Foundation.NSObject)에 매핑된 [System.IntPtr](xref:System.IntPtr)합니다.
- 합니다 `width` 매개 변수를 `CGFloat`, 매핑되 `nfloat`합니다.
- 합니다 `lineBreakMode` 매개 변수를 [ `UILineBreakMode` ](https://developer.apple.com/documentation/uikit/uilinebreakmode?language=objc),으로 Xamarin.iOS에서 이미 바인딩된는 [`UILineBreakMode`](xref:UIKit.UILineBreakMode)
열거형입니다.

전체적인 통합,는 `objc_msgSend` 선언이 일치 해야 합니다.

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

반환된 된 값 구조체의 크기가 8 바이트 미만 했던 것 (이전 처럼 `SizeF` Unified Api로 전환 하기 전에 사용), 위 코드는 장치에서 충돌 하지만 시뮬레이터에서 실행 합니다. 크기가 8 비트 미만의 값을 반환 하는 선택기를 호출 하려면 선언 된 `objc_msgSend_stret` 함수:

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

## <a name="invoking-a-selector"></a>선택기를 호출합니다.

선택기를 호출 하는 세 단계가 있습니다.

1. 대상을 선택기를 가져옵니다.
2. 선택기 이름을 가져옵니다.
3. 호출 `objc_msgSend` 적절 한 인수를 사용 합니다.

### <a name="selector-targets"></a>선택기 대상

선택기 대상이 개체 인스턴스 또는 Objective-c 클래스 중 하나입니다. 대상 인스턴스는 바인딩된 Xamarin.iOS 유형에 서 제공를 사용 합니다 [ `ObjCRuntime.INativeObject.Handle` ](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) 속성입니다.

대상 클래스를 사용 하 여 [ `ObjCRuntime.Class` ](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) 클래스 인스턴스에 대 한 참조를 가져오려면 다음을 사용 합니다 [ `Class.Handle` ](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) 속성입니다.

### <a name="selector-names"></a>선택기 이름

Apple 설명서에 선택기 이름이 나열 됩니다. 예를 들어 [ `NSString` ](https://developer.apple.com/documentation/foundation/nsstring?language=objc) 포함 [ `sizeWithFont:` ](https://developer.apple.com/documentation/foundation/nsstring/1619917-sizewithfont?language=objc) 고 [ `sizeWithFont:forWidth:lineBreakMode:` ](https://developer.apple.com/documentation/foundation/nsstring/1619914-sizewithfont?language=objc) 선택기입니다. Embedded 및 후행 콜론 선택기 이름의 일부인 되며 마찬가지로 생략 될 수 없습니다.

선택기 이름이 만들 수 있습니다는 [ `ObjCRuntime.Selector` ](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) 인스턴스에 있습니다.

### <a name="calling-objcmsgsend"></a>Objc_msgSend 호출

`objc_msgSend` 개체 (선택기) 메시지를 보냅니다. 이 함수 패밀리는 필수 인수 두 개 이상의: 선택기 대상 (인스턴스 또는 처리 하는 클래스), 자체 선택기 및 선택기에 필요한 인수입니다. 인스턴스 및 선택기 인수 여야 합니다 `System.IntPtr`, 모든 나머지 인수는 예를 들어 선택기 예상 형식과 일치 해야 합니다는 `nint` 에 대 한는 `int`, 또는 `System.IntPtr` 모든 `NSObject`-파생 형식입니다. 사용 된 [`NSObject.Handle`](xref:Foundation.NSObject.Handle)
가져올 속성을 `IntPtr` Objective-c 형식 인스턴스에 대 한 합니다.

둘 이상의 방법이 `objc_msgSend` 함수:

- 사용 하 여 [ `objc_msgSend_stret` ](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc) 구조체를 반환 하는 선택기에 대 한 합니다. ARM에서 여기에 열거형이 아닌 모든 반환 형식 또는 C 기본 제공 형식 중 하나 (`char`, `short`, `int`, `long`를 `float`, `double`). X86 (시뮬레이터)에서이 메서드를 사용 해야 모든 구조체의 크기가 8 바이트 보다 큰 (`CGSize` 는 8 바이트가 고 사용 하지 않는 `objc_msgSend_stret` 시뮬레이터에서). 
- 사용 하 여 [ `objc_msgSend_fpret` ](https://developer.apple.com/documentation/objectivec/1456697-objc_msgsend_fpret?language=objc) 부동 반환 하는 선택기 소수점 x86에 값에 대 한 합니다. 이 함수를 ARM;에서 사용할 필요가 없습니다. 대신 `objc_msgSend`합니다. 
- 주 [objc_msgSend](https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend) 다른 모든 선택기에 대 한 함수를 사용 합니다.

하기로 했다면 `objc_msgSend` 함수를 호출 해야 (시뮬레이터 및 장치 필요할 수 있습니다 각 다른 메서드), 일반적인을 사용할 수 있습니다 [ `[DllImport]` ](xref:System.Runtime.InteropServices.DllImportAttribute) 이후 호출에 대 한 함수를 선언 하는 방법입니다.

미리 만든된 집합이 `objc_msgSend` 선언에서 찾을 수 있습니다 `ObjCRuntime.Messaging`합니다.

## <a name="different-invocations-on-simulator-and-device"></a>시뮬레이터 및 장치에 다른 호출

Objective C에는 세 가지 종류 위에서 설명한 대로의 `objc_msgSend` 메서드: 부동 소수점 값 (x86 전용)를 반환 하는 호출 및 구조체 값을 반환 하는 호출에 대 한 일반 호출 합니다. 후자는 접미사가 포함 `_stret` 에서 `ObjCRuntime.Messaging`합니다.

로 첫 번째 매개 변수로 반환 값을 사용 하 여 메서드를 호출 해야 합니다 (아래에 설명 된 규칙) 특정 구조체를 반환 하는 메서드를 호출 하는 경우는 `out` 값:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

사용 하는 경우에 대 한 규칙을 `_stret_` 메서드 x86 및 ARM에서 다릅니다.
시뮬레이터와 장치 모두에서 작동 하도록 바인딩을 원하는 경우 다음과 같은 코드를 추가 합니다.

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

### <a name="using-the-objcmsgsendstret-method"></a>Objc_msgSend_stret 메서드를 사용 하 여

ARM 용으로 빌드하는 경우 사용 합니다 [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
열거자 또는 열거자에 대 한 기본 형식의 아닌 모든 값 형식에 대 한 (`int`, `byte`를 `short`를 `long`를 `double`, `float`).

X86 용으로 빌드하는 경우 사용 [`objc_msgSend_stret`](https://developer.apple.com/documentation/objectivec/1456730-objc_msgsend_stret?language=objc)
열거형 또는 열거형의 기본 형식이 아닌 모든 값 형식에 대 한 (`int`, `byte`, `short`, `long`, `double`, `float`) 및 네이티브 크기가 8 바이트 보다 큽니다.

### <a name="creating-your-own-signatures"></a>자체 서명 만들기

다음 [요점](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) 필요한 경우 고유한 서명을 만드는 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Objective-c 선택기](https://developer.xamarin.com/samples/mac-ios/Objective-C/) 샘플
