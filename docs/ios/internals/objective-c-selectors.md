---
title: Xamarin.iOS에서 Objective-c 선택기
description: 이 문서에서는 C#에서 Objective-c 선택기와 상호 작용 하는 방법을 설명 합니다. 이렇게 하면 계정에 수행 해야 하는 기술적인 고려 사항과 선택기를 호출 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/12/2017
ms.openlocfilehash: 3083770fd2874eca317585b6bf949f3efe56f879
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351732"
---
# <a name="objective-c-selectors-in-xamarinios"></a>Xamarin.iOS에서 Objective-c 선택기

Objective C 언어를 기반으로 *선택기*합니다. 선택기는 개체에 보낼 수 있는 메시지 또는 *클래스*합니다. [Xamarin.iOS](~/ios/internals/api-design/index.md) 맵 인스턴스 메서드에 선택기 인스턴스 및 정적 메서드를 선택기 클래스입니다.

일반 C 함수와 달리 (및 c + + 멤버 함수와 마찬가지로)을 호출할 수 없습니다 직접 사용 하 여 선택기 [P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/)합니다.
(*따로*: 이론상에서 가상이 아닌 c + + 멤버 함수에 대 한 P/Invoke를 사용할 수 있지만 해야 컴파일러 당 이름 관리를 걱정할는 수많은 문제점을 더 잘 무시 합니다.) 대신 선택기 Objective-c 클래스에 보내거나를 사용 하 여 인스턴스를 [ `objc_msgSend` 함수](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend)합니다.

사용할 수 있습니다 [Objective-c 메시징이 유용한 가이드](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html) 유용 합니다.

<a name="Example" />

## <a name="example"></a>예

호출 하려는 경우 다음과 같이 합니다 [-[NSString sizeWithFont:forWidth:lineBreakMode:]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:) 선택기입니다.
Apple 설명서) (에서 선언은 다음과 같습니다.

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  반환 형식은 *CGSize* Unified API에 대 한 합니다.
-  합니다 *글꼴* 매개 변수는를 [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (및 형식 (직접)에서 파생 된 [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ), 따라서 매핑됩니다 [System.IntPtr](xref:System.IntPtr) 합니다.
-  합니다 *너비* 매개 변수를 *CGFloat* , 매핑되 *nfloat*합니다.
-  *lineBreakMode* 매개 변수를 *UILineBreakMode* ,으로 Xamarin.iOS에서 이미 바인딩된 합니다 [UILineBreakMode 열거형](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/) .


이제 모두 정리해, 그리고 일치 하는 objc_msgSend 선언:

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

해당 함수를 선언 해야 합니다.

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

선언 된 후 해당 매개 변수에 있으면이 호출할 수 있습니다.

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode);
```

반환된 된 값 구조체의 크기가 8 바이트 미만 했던 것 (이전 처럼 `SizeF` Unified Api로 전환 하기 전에 사용) 위의 코드는 장치에서 충돌 하지만 시뮬레이터에서 실행 됩니다. 결과적으로 우리는 크기가 8 비트 미만의 값을 반환 하는 선택기를 호출 하도록 *수도* 선언 해야 합니다 `objc_msgSend_stret()` 함수:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

호출이 됩니다.

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode);
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode);
```


<a name="Invoking_a_Selector" />

## <a name="invoking-a-selector"></a>선택기를 호출합니다.

선택기를 호출 하는 세 단계가 있습니다.

1.  대상을 선택기를 가져옵니다.
1.  선택기 이름을 가져옵니다.
1.  적절 한 인수를 사용 하 여 objc_msgSend()를 호출 합니다.


<a name="Selector_Targets" />

### <a name="selector-targets"></a>선택기 대상

선택기 대상이 개체 인스턴스 또는 Objective-c 클래스 중 하나입니다. 대상 인스턴스는 바인딩된 Xamarin.iOS 유형에 서 제공를 사용 합니다 [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) 속성입니다.

대상 클래스를 사용 하 여 [ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) 클래스 인스턴스에 대 한 참조를 가져오려면 다음을 사용 합니다 [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) 속성입니다.


<a name="Selector_Names" />

### <a name="selector-names"></a>선택기 이름

Apple 설명서에 선택기 이름이 나열 됩니다. 예를 들어 합니다 [UIKit NSString 확장 메서드](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html) 포함 [sizeWithFont:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:) 하 고 [sizeWithFont:forWidth:lineBreakMode:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:)합니다. Embedded 및 후행 콜론은 중요 하 고 선택기 이름의 일부입니다.

선택기 이름이 만들 수 있습니다는 [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) 인스턴스에 있습니다.


<a name="Calling_objc_msgSend()" />

### <a name="calling-objcmsgsend"></a>Objc_msgSend() 호출

 `objc_msgSend()` 메시지를 보내려면 (선택기) 개체에 사용 됩니다. 이 함수 패밀리는 필수 인수 두 개 이상의: 선택기 대상 (인스턴스 또는 처리 하는 클래스), 자체 선택기 및 다음 특정 선택기에 대 한 필수 인수입니다. 인스턴스 및 선택기 인수 여야 합니다 `System.IntPtr`, 모든 나머지 인수는 예를 들어 선택기 예상 형식과 일치 해야 합니다는 `nint` 에 대 한는 `int`, 또는 `System.IntPtr` 모든 `NSObject`-파생 형식입니다. 사용 된 [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) 속성을는 `IntPtr` Objective-c 형식 인스턴스에 대 한 합니다.

그러나는 둘 이상의 `objc_msgSend()` 함수입니다.

사용 하 여 [ `objc_msgSend_stret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) 구조체를 반환 하는 선택기에 대 한 합니다.
"흥미로운" ARM에서이 유지 하기 위해 포함 된 모든 반환 형식이 *되지* 열거형 또는 C 기본 제공 형식 (char, short, int, long, float, double). (시뮬레이터) x86의 크기가 8 바이트 보다 큰 모든 구조에 사용할 수 해야 합니다. (CGSize--위의 예제에 사용 되는 정확 하 게는 8 바이트가 고 따라서 사용 하지 않는 `objc_msgSend_stret()` 시뮬레이터에서.)

사용 하 여 [ `objc_msgSend_fpret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret) 부동 반환 하는 선택기 소수점 x86에 값에 대 한 합니다. 이 함수를 ARM;에서 사용할 필요가 없습니다. 대신 `objc_msgSend()`합니다.

주 [objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend) 다른 모든 선택기에 대 한 함수를 사용 합니다.

하기로 했다면 `objc_msgSend()` 함수 호출 해야 합니다 (및에 둘 이상의 예: 시뮬레이터 및 장치 수 있습니다), 일반적인을 사용할 수 있습니다 [ `[DllImport]` ](xref:System.Runtime.InteropServices.DllImportAttribute) 이후 호출에 대 한 함수를 선언 하는 방법입니다.

미리 만든된 집합이 `objc_msgSend()` 선언에서 찾을 수 있습니다 [ `ObjCRuntime.Messaging` ](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/)합니다.


<a name="ugly" />

## <a name="the-ugly"></a>좋지 않은

Objective C에는 세 가지 종류의 `objc_msgSend` 메서드: 부동 소수점 값 (x86 전용)를 반환 하는 호출 및 구조체 값을 반환 하는 호출에 대 한 일반 호출 합니다. 후자는 접미사가 포함 `_stret` 에서 `ObjCRuntime.Messaging`합니다.

특정 구조 (규칙은 아래에서 설명)를 반환 하는 메서드를 호출 하는 경우 이와 같은 out 값으로 첫 번째 매개 변수로 반환 값을 사용 하 여 메서드를 호출 해야 합니다.

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

여기에 까다로운 가져오기 작업 이므로 사용 해야 하는 것에 대 한 규칙 _stret_ 다릅니다 X86 및 ARM에서 시뮬레이터와 장치 모두에서 작동 하도록 바인딩을 원하는 경우 다음과 같은 코드를 추가 하려면:

```csharp
if (Runtime.Arch == Arch.DEVICE){
    PointF ret;

    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);

    return ret;
} else
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
```

### <a name="using-the-objcmsgsendstret-method"></a>objc를 사용 하 여\_msgSend\_stret 메서드

사용 하는 경우에 대 한 규칙을 [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) 에 대 한이 같습니다 **ARM**:

-  임의의 값 형식을 열거형 또는 열거형에 대 한 기본 유형이 아닌 (int, byte, short, long, double, float).


사용 하는 경우에 대 한 규칙을 [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) 에 대 한이 같습니다 **X86**:

-  열거형 또는 열거형의 기본 형식이 아닌 형식 값 (int, byte, short, long, double, float) 및 네이티브 크기가 8 바이트 보다 큽니다.


### <a name="creating-your-own-signatures"></a>자신의 서명을 만드는 중입니다.

다음 [요점](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) 필요한 경우 고유한 서명을 만드는 사용할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [선택기 예제](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
