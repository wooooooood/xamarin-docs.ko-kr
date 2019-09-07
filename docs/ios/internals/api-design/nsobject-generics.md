---
title: Xamarin.ios에서 NSObject의 일반 서브 클래스
description: 이 문서에서는 NSObject의 일반 서브 클래스를 만드는 방법을 설명 합니다. 이를 통해 수행할 수 있는 작업과 수행할 수 없는 작업을 검사 하 고, 정적 등록자에 대해 설명 하 고, 성능을 확인 합니다.
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 136efbd936bc39563c419a87ed48f6fc5436efa9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768540"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Xamarin.ios에서 NSObject의 일반 서브 클래스

## <a name="using-generics-with-nsobjects"></a>NSObjects에서 제네릭을 사용 하는 경우

의 `NSObject`서브 클래스에서 제네릭을 사용할 수 있습니다 (예: [uiview](xref:UIKit.UIView):

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

하위 클래스가 `NSObject` 목표-C 런타임에 등록 되기 때문에 `NSObject` 형식의 제네릭 서브 클래스에서 가능한 작업에 대 한 몇 가지 제한 사항이 있습니다.

## <a name="considerations-for-generic-subclasses-of-nsobject"></a>NSObject의 제네릭 서브 클래스에 대 한 고려 사항

이 문서에서는의 `NSObjects`제네릭 서브 클래스에 대해 제한적으로 지원 되는 제한 사항에 대해 자세히 설명 합니다.

### <a name="generic-type-arguments-in-member-signatures"></a>멤버 시그니처의 제네릭 형식 인수

목적-C에 노출 된 멤버 시그니처의 모든 제네릭 형식 인수에는 `NSObject` 제약 조건이 있어야 합니다.

**양호**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**이유**: 제네릭 형식 매개 변수 `NSObject`는 이므로에 대 한 `myMethod:` 선택기 시그니처는 목표-C에 안전 하 게 노출 될 수 있습니다 `NSObject` (항상 또는이의 서브 클래스).

**잘못**됨:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**이유**: 시그니처는 제네릭 형식의 `T`정확한 형식에 따라 달라 지므로 목표-c 코드에서 호출할 수 있는 내보낸 멤버에 대 한 목표-c 시그니처를 만들 수는 없습니다.

**양호**:

```csharp
class Generic<T> : NSObject
{
    T storage;

    [Export ("myMethod:")]
    public void MyMethod (NSObject value)
    {
    }
}
```

**이유**: 내보낸 멤버 시그니처의 일부를 사용 하지 않는 한 제한 되지 않은 제네릭 형식 인수를 사용할 수 있습니다.

**양호**:

```csharp
class Generic<T, U> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
        Console.WriteLine (typeof (U));
    }
}
```

**이유**: `T` 내보낸 `MyMethod` 목표 C의 매개 변수는로 `NSObject`제한 되 고 제한 되지 않는 형식은 `U` 시그니처의 일부가 아닙니다.

### <a name="instantiations-of-generic-types-from-objective-c"></a>목표에서 제네릭 형식의 인스턴스화-C

목표-C에서 제네릭 형식의 인스턴스화는 허용 되지 않습니다. 이는 일반적으로 xib 또는 storyboard에서 관리 되는 형식을 사용할 때 발생 합니다.

을 `IntPtr` (를) 사용 하는 생성자를 노출 하는이 클래스 정의를 살펴보겠습니다 (네이티브 목표- C# C 인스턴스에서 개체를 구성 하는 xamarin.ios 방법).

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

위의 구문은 문제가 없지만 런타임에는 목표 C가 인스턴스를 만들려고 시도 하는 경우에서 예외를 throw 합니다.

이는 목표 C에 제네릭 형식의 개념이 없고 만들 정확한 제네릭 형식을 지정할 수 없기 때문에 발생 합니다.

이 문제는 제네릭 형식의 특수 한 하위 클래스를 만들어 해결할 수 있습니다. 예:

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

이제 더 이상 모호성이 없으므로 클래스 `GenericUIView` 를 xib 또는 storyboard에서 사용할 수 있습니다.

## <a name="no-support-for-generic-methods"></a>제네릭 메서드 지원 안 함

### <a name="generic-methods-are-not-allowed"></a>제네릭 메서드는 허용 되지 않습니다.

다음 코드는 컴파일되지 않습니다.

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**이유**: 이는 메서드가 목표-C에서 호출 될 때 형식 인수 `T` 에 사용할 형식을 알 수 없기 때문에 허용 되지 않습니다.

대신 특수화 된 메서드를 만들고 대신 내보내는 것이 좋습니다.

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyUIViewMethod (UIView argument)
    {
        MyMethod<UIView> (argument);
    }
    public void MyMethod<T> (T argument)
    {
    }
}
```

### <a name="no-exported-static-members-allowed"></a>내보낸 정적 멤버를 사용할 수 없습니다.

의 `NSObject`제네릭 서브 클래스 내에서 호스팅되는 경우 목표-C에 정적 멤버를 노출할 수 없습니다.

지원 되지 않는 시나리오의 예:

```csharp
class Generic<T> : NSObject where T : NSObject
{
    [Export ("myMethod:")]
    public static void MyMethod ()
    {
    }

    [Export ("myProperty")]
    public static T MyProperty { get; set; }
}
```

**문서화** 제네릭 메서드와 마찬가지로 Xamarin.ios 런타임은 제네릭 형식 인수 `T`에 사용할 형식을 알 수 있어야 합니다.

인스턴스 자체를 사용 하는 경우 인스턴스 자체가 사용 `Generic<T>`됩니다. 인스턴스는 아니지만 `Generic<SomeSpecificClass>`항상 이지만 정적 멤버의 경우에는이 정보가 제공 되지 않습니다.

이는 해당 멤버가 어떤 방식으로든 형식 인수 `T` 를 사용 하지 않는 경우에도 적용 됩니다.

이 경우에는 특수 한 하위 클래스를 만드는 방법이 있습니다.

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIViewProperty {
        get { return MyProperty; }
        set { MyProperty = value; }
    }
}

class Generic<T> : NSObject where T : NSObject
{
    public static void MyMethod () {}
    public static T MyProperty { get; set; }
}
```

## <a name="performance"></a>성능

정적 등록 기관은 일반적으로 빌드 시 제네릭 형식의 내보낸 멤버를 확인할 수 없습니다 .이 멤버는 런타임에 조회 되어야 합니다. 즉,이 메서드를 목표 C에서 호출 하는 것은 제네릭이 아닌 클래스의 멤버를 호출 하는 것 보다 약간 느립니다.
