---
title: Xamarin.iOS에서 NSObject의 제네릭 서브 클래스
description: 이 문서를 만드는 방법을 설명 NSObject의 제네릭 서브 클래스를 만듭니다. 대상 수 및 수행할 수 없습니다, 정적 등록 기관에 설명 및 성능을 살펴보고을 검사 합니다.
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 512280e9c298cfbcea6f693b0691236fd1cf5a5f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036484"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Xamarin.iOS에서 NSObject의 제네릭 서브 클래스

## <a name="using-generics-with-nsobjects"></a>NSObjects 제네릭 사용

제네릭의 서브 클래스에서 사용할 수 7.2.1 Xamarin.iOS를 사용 하 여 시작 `NSObject` (예를 들어 [UIView](xref:UIKit.UIView)합니다.

이제 다음과 같은 제네릭 클래스를 만들 수 있습니다.

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

이후에 해당 서브 클래스를 개체 `NSObject` 등록 된 Objective-c 런타임에서 사용 하 여 몇 가지 제한 사항이의 제네릭 서브 클래스를 사용 하 여 가능한 것에 대 한 `NSObject` 형식입니다.
    
## <a name="considerations-for-generic-subclasses-of-nsobject"></a>NSObject의 제네릭 서브 클래스에 대 한 고려 사항

이 문서에서는 자세히 설명의 제네릭 서브 클래스에 대 한 제한 된 지원의 제한 사항이 `NSObjects` 7.2.1 Xamarin.iOS를 사용 하 여 도입 합니다.
    
### <a name="generic-type-arguments-in-member-signatures"></a>멤버 시그니처에 제네릭 형식 인수

Objective-c에 노출 하는 멤버 시그니처에 제네릭 형식 인수를 모든 있어야는 `NSObject` 제약 조건입니다.

**좋은**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**이유**: 제네릭 형식 매개 변수가 `NSObject`이므로 선택기 시그니처 `myMethod:` Objective-c로 안전 하 게 노출 될 수 있습니다 (해당 값은 항상 `NSObject` 또는 해당 서브 클래스).

**잘못 된**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**이유**: Objective-c 코드를 호출할 수 있으므로 시그니처는 정확한 형식의 제네릭 형식에 따라 달라 있는 내보낸된 멤버에 대 한는 Objective-c로 서명을 만들 수 없는 `T`합니다.

**좋은**:

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

**이유**: 내보낸된 멤버 시그니처의 일부로 사용할 수 없습니다으로 제네릭 형식 인수 제약 없이 수 있습니다.

**좋은**:

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

**이유**: 합니다 `T` 매개 변수는 Objective-c로 내보낸 `MyMethod` 으로 제한 되는 `NSObject`, 비제한 형식 `U` 서명의 일부가 아닙니다.
    
### <a name="instantiations-of-generic-types-from-objective-c"></a>Objective-c에서 제네릭 형식의 인스턴스화

Objective-c에서 제네릭 형식의 인스턴스화 허용 되지 않습니다. 이 문제는 일반적으로 관리 되는 형식 xib에 사용 된 경우 발생 합니다.

이 클래스 정의 사용 하는 생성자를 노출 하는 것이 좋습니다는 `IntPtr` (Xamarin.iOS 방식의 생성 된 C# 네이티브 Objective-c 인스턴스에서 개체):
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

위의 구문은 런타임에 제대로 하는 동안에 Objective-c의 인스턴스를 만들려고 하는 경우에 예외가 throw 됩니다이 합니다.

이 제네릭 형식의 개념이 Objective-c 및 만들 정확한 제네릭 형식을 지정할 수 없습니다 때문에 발생 합니다.

제네릭 형식의 특수화 된 서브 클래스를 만들어이 문제를 해결할 수 있습니다.   예를 들어:
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

모호성이 없습니다 더 이상 클래스는 이제 `GenericUIView` xib에 사용할 수 있습니다.

## <a name="no-support-for-generic-methods"></a>제네릭 메서드에 대 한 지원 되지 않습니다

### <a name="generic-methods-are-not-allowed"></a>제네릭 메서드가 허용 되지 않습니다.

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

**이유**: Xamarin.iOS 형식 인수에 사용할 형식을 알지 못하므로이 허용 되지 `T` Objective-c에서 메서드가 호출 되는 경우.

대 안으로 특수 메서드를 만들고 대신 내보낼:

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

### <a name="no-exported-static-members-allowed"></a>내보낸된 정적 멤버가 허용

제네릭 서브 클래스 내에서 호스팅되는 경우 objective-c 정적 멤버를 노출 하지 `NSObject`합니다.

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

**원인:** 제네릭 메서드의 경우와 마찬가지로 Xamarin.iOS 런타임은 T.는 제네릭 형식 인수에 사용할 유형을 알 수 해야

자체 인스턴스를 사용 하는 멤버 예를 들어 (될 수 없으므로 제네릭 인스턴스 때문<T>, 해당 값은 항상 일반<SomeSpecificClass>), 했지만 정적 멤버에 대 한이 정보를 제공 합니다.

문제의 멤버 어떤 방식으로든에서 T 형식 인수를 사용 하지 않는 경우에 적용이 note 합니다.

이 경우 대신 특수화 된 서브 클래스를 만들 때:

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIVIewProperty {
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

### <a name="requires-new-static-registrar"></a>새 정적 등록 필요

제네릭 지원 하려면 새로운 [등록 시스템](~/ios/internals/registrar.md)입니다.

이전에 사용 하려는 경우 레거시 등록 시스템 제네릭 형식 (정의 되지 않은 동작이 발생 하는 올바른 코드를 생성 하지 않도록 추가)를 발견할 때 경고를 표시 합니다.
    
## <a name="performance"></a>성능

와 마찬가지로 일반적으로 정적 등록 기관은 빌드 타임에 제네릭 형식에 내보낸된 멤버를 확인할 수 없습니다, 그리고이를 런타임에 조회할 수 있습니다. 이 제네릭이 아닌 클래스에서 멤버를 호출 하는 보다 조금 느리기는 Objective C에서 이러한 메서드를 호출을 의미 합니다.

