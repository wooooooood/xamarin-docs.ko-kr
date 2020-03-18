---
title: Xamarin.iOS에서 NSObject의 제네릭 서브클래스
description: 이 문서에서는 NSObject의 제네릭 서브클래스를 만드는 방법을 설명합니다. 이를 통해 수행할 수 있는 작업과 수행할 수 없는 작업을 알아보고, 정적 등록자에 대해 설명하고, 성능을 살펴봅니다.
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 279fcac1611038613bf442e1b766fda45dd5a429
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73022360"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Xamarin.iOS에서 NSObject의 제네릭 서브클래스

## <a name="using-generics-with-nsobjects"></a>NSObjects에서 제네릭 사용

`NSObject`의 서브클래스(예: [UIView](xref:UIKit.UIView))에서 제네릭을 사용할 수 있습니다.

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

`NSObject`를 서브클래스하는 개체는 Objective-C 런타임에 등록되기 때문에 `NSObject` 형식의 제네릭 서브클래스에서 가능한 몇 가지 제한 사항이 있습니다.

## <a name="considerations-for-generic-subclasses-of-nsobject"></a>NSObject의 제네릭 서브클래스에 대한 고려 사항

이 문서에서는 `NSObjects`의 제네릭 서브클래스에 대한 제한된 지원의 제한 사항을 자세히 설명합니다.

### <a name="generic-type-arguments-in-member-signatures"></a>멤버 시그니처의 제네릭 형식 인수

Objective-C에 노출된 멤버 시그니처의 모든 제네릭 형식 인수에는 `NSObject` 제약 조건이 있어야 합니다.

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

**이유**: 제네릭 형식 매개 변수는 `NSObject`이므로 `myMethod:`에 대한 선택기 시그너처가 Objective-C에 안전하게 노출될 수 있습니다(항상 `NSObject` 또는 해당 서브클래스).

**불량**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**이유**: 시그니처는 제네릭 형식 `T`의 정확한 형식에 따라 달라지므로, Objective-C 코드가 호출할 수 있는 내보낸 멤버에 대한 Objective-C 시그니처를 만들 수는 없습니다.

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

**이유**: 내보낸 멤버 시그니처의 일부를 사용하지 않는 한 제한되지 않은 제네릭 형식 인수를 사용할 수 있습니다.

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

**이유**: Objective-C에서 내보낸 `MyMethod`의 `T` 매개 변수는 `NSObject`로 제한되며, 제한되지 않은 형식 `U`는 시그니처의 일부가 아닙니다.

### <a name="instantiations-of-generic-types-from-objective-c"></a>Objective-C에서 제네릭 형식 인스턴스화

Objective-C에서 제네릭 형식 인스턴스화는 허용되지 않습니다. 이는 일반적으로 xib 또는 스토리보드에서 관리되는 형식을 사용할 때 발생합니다.

`IntPtr`를 사용하는 생성자를 노출하는 다음 클래스 정의를 살펴보세요(Xamarin.iOS가 네이티브 Objective-C 인스턴스에서 C# 개체를 구성하는 방법).

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

위 구문은 문제가 없지만 런타임에는 Objective-C가 인스턴스를 만들려고 시도하는 경우에서 예외를 throw합니다.

이는 Objective-C에 제네릭 형식 개념이 없고 만들 정확한 제네릭 형식을 지정할 수 없기 때문입니다.

이 문제는 제네릭 형식의 특수화된 서브클래스를 만들어 해결할 수 있습니다. 예를 들어:

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

이제 더 이상 모호성이 없으므로 클래스 `GenericUIView`를 xib 또는 스토리보드에서 사용할 수 있습니다.

## <a name="no-support-for-generic-methods"></a>제네릭 메서드 지원 안 함

### <a name="generic-methods-are-not-allowed"></a>제네릭 메서드는 지원되지 않습니다.

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

**이유**: 이는 메서드가 Objective-C에서 호출될 때 Xamarin.iOS가 형식 인수 `T`에 사용할 형식을 알 수 없기 때문에 허용되지 않습니다.

대신 특수화된 메서드를 만들어 대신 내보내는 것이 좋습니다.

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

### <a name="no-exported-static-members-allowed"></a>내보낸 정적 멤버가 허용되지 않음

`NSObject`의 제네릭 서브클래스 내에서 호스팅되는 정적 멤버는 Objective-C에 노출할 수 없습니다.

지원되지 않는 시나리오 예:

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

**이유:** 제네릭 메서드와 마찬가지로 Xamarin.iOS 런타임은 제네릭 형식 인수 `T`에 사용할 형식을 알 수 있어야 합니다.

인스턴스 멤버의 경우 인스턴스 자체가 사용되지만(인스턴스 `Generic<T>`가 없고 항상 `Generic<SomeSpecificClass>`이기 때문) 정적 멤버의 경우에는 이 정보가 제공되지 않습니다.

이는 문제의 멤버가 형식 인수 `T`를 사용하지 않는 경우에도 적용됩니다.

이 경우에는 특수화된 하위 클래스를 만드는 방법이 있습니다.

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

정적 등록자는 빌드 시 제네릭 형식의 내보낸 멤버를 확인할 수 없습니다. 이 멤버는 런타임에 조회되어야 합니다. 즉, 이 메서드를 Objective-C에서 호출하는 것은 비 제네릭 클래스에서 멤버를 호출하는 것보다 약간 느립니다.
