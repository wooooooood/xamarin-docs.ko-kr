---
title: "NSObject의 제네릭 하위 클래스"
ms.topic: article
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 76c35cfb993bade324b25f86e75db7eb028a7399
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="generic-subclasses-of-nsobject"></a>NSObject의 제네릭 하위 클래스

## <a name="using-generics-with-nsobjects"></a>NSObjects 제네릭을 사용

제네릭의 서브 클래스에서 사용할 수 Xamarin.iOS 7.2.1로 시작 `NSObject` (예를 들어 [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/)).

이 이와 같은 제네릭 클래스를 만들 수 있습니다.

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

이후에 해당 하위 클래스 개체 `NSObject` 등록 Objective C 런타임는 제네릭 하위 클래스의 사용 가능한 것에 대 한 몇 가지 제한이 `NSObject` 형식입니다.
    
## <a name="considerations-for-generic-subclasses-of-nsobject"></a>NSObject의 제네릭 하위 클래스에 대 한 고려 사항

이 문서의 제네릭 하위 클래스에 대 한 제한 된 지원이 제한 사항에 자세히 설명 `NSObjects` Xamarin.iOS 7.2.1 도입 합니다.
    
### <a name="generic-type-arguments-in-member-signatures"></a>멤버 시그니처에 제네릭 형식 인수

Objective C에 노출 하는 멤버 시그니처에 제네릭 형식 인수를 모든 있어야는 `NSObject` 제약 조건입니다.

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

**이유**: 제네릭 형식 매개 변수는는 `NSObject`하므로 선택기 서명을 `myMethod:` Objective-c에 안전 하 게 노출할 수 (항상 `NSObject` 또는 그 하위 클래스).

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

**이유**: 내보낸된 멤버 Objective C 코드를 호출할 수는 정확한 형식의 제네릭 형식에 따라 서명을 다르기 때문에 대 한 Objective-c 서명을 만들 수 없으면 `T`합니다.

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

**이유**: 내보낸된 멤버 서명의 일부가 사용 하지 않는 상태로 제네릭 형식 인수 제약 없이 수 있습니다.

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

**이유**:는 `T` Objective C의 매개 변수 내보낸 `MyMethod` 으로 제한 되는 `NSObject`, 제한 되지 않은 형식 `U` 서명의 일부가 아닙니다.
    
### <a name="instantiations-of-generic-types-from-objective-c"></a>Objective C에서 제네릭 형식의 인스턴스화

Objective C에서 제네릭 형식을 인스턴스화할 수 없습니다. 이 문제는 일반적으로 xib에서 관리 되는 형식을 사용 하는 경우 발생 합니다.

이 클래스 정의 사용 하는 생성자를 노출 하는 것이 좋습니다는 `IntPtr` (C# 개체는 네이티브 Objective-c 인스턴스에서 생성의 Xamarin.iOS 방법).
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

위의 구문 런타임에 마찬가지 이지만, Objective C가 해당 형식의 인스턴스를 생성 하려고 하는 경우에 예외를 throw 합니다이 합니다.

두 일이 생기 Objective-c에 제네릭 형식의 개념이 없기 때문에 만들 정확 하 게 제네릭 형식을 지정할 수 없습니다.

제네릭 형식의 특수화 된 서브 클래스를 만들어이 문제를 해결할 수 수 있습니다.   예:
    
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

모호성이 없는 더 이상 클래스 이제 `GenericUIView` xibs에서 사용할 수 있습니다.

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

**이유**: Xamarin.iOS는 형식 인수에 사용할 형식을 알지 못하므로이 허용 되지 `T` Objective C에서 메서드가 호출 되는 시기입니다.

이를 대체할 수는 특수 메서드를 만들고 하는 대신 내보내기:

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

### <a name="no-exported-static-members-allowed"></a>허용 되는 내보낸된 정적 멤버 없음

제네릭 하위 클래스 내에서 호스팅되는 경우 Objective C에 정적 멤버를 노출 하지 `NSObject`합니다.

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

**원인:** 동일 하 게 제네릭 메서드이므로 화 제네릭 형식 인수에 사용할 형식을 알고 있어야 Xamarin.iOS 런타임 요구 사항

자체 인스턴스를 사용 하는 멤버 예를 들어 (될 수 없으므로 제네릭 인스턴스 이후<T>, 제네릭 항상 됩니다<SomeSpecificClass>), 정적 멤버에 대 한이 정보는 존재 하지만 합니다.

문제의 멤버 어떤 방식으로든에서 T 형식 인수를 사용 하지 않는 경우에 적용이 note 합니다.

이 경우 다른 방법은 특수화 된 서브 클래스를 만들려면:

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

### <a name="requires-new-static-registrar"></a>새 정적 등록 자가 필요합니다.

제네릭 지원 하려면 새 [등록 시스템](~/ios/internals/registrar.md)합니다.

이전에 사용 하려는 경우 레거시 등록 시스템 정의 되지 않은 동작이 발생 하는 올바른 코드를 생성 하지 않도록 더하기) (의 제네릭 형식에 도달할 때 경고 메시지를 표시 합니다.
    
## <a name="performance"></a>성능

와 마찬가지로 일반적으로 정적 등록 기관 빌드 타임에 제네릭 형식에는 내보낸된 멤버 확인할 수 없습니다, 그리고 런타임에 조회에 있습니다. 이 Objective C에서 이러한 메서드를 호출 제네릭이 아닌 클래스에서 멤버를 호출 하는 보다 조금 느리기 임을 의미 합니다.

