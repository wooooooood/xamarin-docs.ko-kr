---
title: Xamarin.iOS 용 형식 등록
description: 이 문서는 Xamarin.iOS 형식 등록 기관에 설명 합니다 C# Objective-c 런타임에 사용할 수 있는 클래스입니다.
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/29/2018
ms.openlocfilehash: 83340ce2d5db145c29166d90d3a5180b1767d7ca
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672497"
---
# <a name="type-registrar-for-xamarinios"></a>Xamarin.iOS 용 형식 등록

이 문서에서는 Xamarin.iOS에서 사용 하는 형식 등록 시스템을 설명 합니다.

## <a name="registration-of-managed-classes-and-methods"></a>관리 되는 클래스 및 메서드는 등록

Xamarin.iOS를 시작 하는 동안 등록 합니다.

- 클래스는 [[등록]](xref:Foundation.RegisterAttribute) Objective-c 클래스 특성입니다.
- 클래스는 [[Category]](xref:ObjCRuntime.CategoryAttribute) Objective-c 범주로 특성입니다.
- 상호 작용을 [[Protocol]](xref:Foundation.ProtocolAttribute) Objective-c 프로토콜 특성입니다.
- 사용 하 여 멤버를 [[내보내기]](xref:Foundation.ExportAttribute), Objective-c에 액세스할 수 있게 합니다.

예를 들어 관리 되는 것이 좋습니다 `Main` Xamarin.iOS 응용 프로그램에서 공용 메서드.

```csharp
UIApplication.Main (args, null, "AppDelegate");
```

이 코드는 Objective-c 하도록 런타임에 지시 하 이라는 형식을 사용해 `AppDelegate` 응용 프로그램의 대리자 클래스입니다. 인스턴스를 만들 수 있게 되기를 Objective-c 런타임에서 C# `AppDelegate` 클래스에서 클래스를 등록 되어야 합니다.

Xamarin.iOS (정적 등록) 컴파일 타임 또는 런타임 (동적 등록)에 자동으로 등록을 수행합니다.

동적 등록 리플렉션을 사용 하 여 시작 시 모든 클래스 및 메서드를 등록 하려면 찾을 Objective-c 런타임에 전달 합니다. 동적 등록 시뮬레이터 빌드에서 기본적으로 사용 됩니다.

정적 등록 응용 프로그램에서 사용 된 어셈블리 컴파일 타임 검사 합니다. 클래스 및 Objective-c로 등록 하는 방법 결정 하 고 이진에 포함 된 맵을 생성 합니다.
그런 다음 시작 시 등록 지도 Objective C 런타임을 사용 합니다. 정적 등록 장치 빌드에 사용 됩니다.

### <a name="categories"></a>범주

Xamarin.iOS 8.10부터 사용 하 여 Objective-c로 범주를 만들 수는 C# 구문입니다.

범주를 만들려면 사용 합니다 `[Category]` 특성 및 확장 유형을 지정 합니다. 다음 코드를 확장 하는 예를 들어 `NSString`:

```csharp
[Category (typeof (NSString))]
```

각각의 범주의 메서드는 `[Export]` Objective-c 런타임에서 사용할 수 있도록 특성:

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

표준을 사용 하 여 Objective-c 인스턴스 메서드를 만드는 것 이지만 모든 관리 되는 확장 메서드는 정적 이어야 합니다. C# 확장 메서드 구문:

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

확장 메서드의 첫 번째 인수는 메서드가 호출 된 인스턴스:

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
 }
 ```

이 예제에서는 네이티브를 추가 합니다 `toUpper` 메서드를 인스턴스는 `NSString` 클래스입니다. 목표를 c:이 메서드를 호출할 수 있습니다.

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAutoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

### <a name="protocols"></a>프로토콜

상호 작용 Xamarin.iOS 8.10부터는 `[Protocol]` 프로토콜로 특성 Objective-c로 내보내집니다.

```csharp
[Protocol ("MyProtocol")]
interface IMyProtocol
{
    [Export ("method")]
    void Method ();
}

class MyClass : IMyProtocol
{
    void Method ()
    {
    }
}
```

이 코드를 내보냅니다 `IMyProtocol` 프로토콜로 objective-c 호출 `MyProtocol` 클래스 및 `MyClass` 프로토콜을 구현 하는 합니다.

## <a name="new-registration-system"></a>새 등록 시스템

안정적인 6.2.6부터 버전과 6.3.4 베타 버전의 경우는 새 정적 등록자 추가 했습니다. 7.2.1에서 버전인 했습니다 새 등록자 기본값입니다.

이 새 등록 시스템에서는 다음과 같은 새로운 기능을 제공합니다.

- 컴파일 타임 프로그래머 오류 검색:
    - 동일한 이름으로 등록 되는 두 클래스입니다.
    - 동일한 선택기에 응답할 내보낸 둘 이상의 메서드
- 사용 되지 않는 네이티브 코드를 제거 합니다.
    - 새 등록 시스템 결과 이진에서 사용 되지 않는 네이티브 코드를 제거 네이티브 링커가 수 있도록 정적 라이브러리, 사용 된 코드에 대 한 강력한 참조를 추가 합니다. Xamarin 샘플 바인딩에서 대부분의 응용 프로그램에는 300 k 이상 작은 됩니다.

- 제네릭 서브 클래스에 대 한 지원을 `NSObject`; 참조 [NSObject 제네릭](~/ios/internals/api-design/nsobject-generics.md) 자세한 내용은 합니다. 또한 새 등록 시스템은 런타임 시 임의의 동작이 발생 이전에 지원 되지 않는 제네릭 구문 catch 합니다.

### <a name="errors-caught-by-the-new-registrar"></a>새 등록 기관에서 전달 된 오류

다음은 새 등록자에서 포착 오류의 몇 가지 예입니다.

- 동일한 선택기를 두 번 이상 동일한 클래스에서 내보내기:

    ```csharp
    [Register]
    class MyDemo : NSObject
    {
        [Export ("foo:")]
        void Foo (NSString str);
        [Export ("foo:")]
        void Foo (string str)
    }
    ```

- Objective-c 이름이 같은 둘 이상의 관리 되는 클래스를 내보내기:

    ```csharp
    [Register ("Class")]
    class MyClass : NSObject {}

    [Register ("Class")]
    class YourClass : NSObject {}
    ```

- 제네릭 메서드 내보내기:

    ```csharp
    [Register]
    class MyDemo : NSObject
    {
        [Export ("foo")]
        void Foo<T> () {}
    }
    ```

### <a name="limitations-of-the-new-registrar"></a>새 등록자의 제한 사항

새 등록 기관에 대 한 염두에 몇 가지 사항은 다음과 같습니다.

- 일부 타사 라이브러리는 새 등록 시스템과 함께 작동 하도록 업데이트 되어야 합니다. 참조 [수정 필요한](#required-modifications) 아래 대 한 자세한 내용은 합니다.

- 단기 단점이 이기도 Clang 계정 프레임 워크를 사용 하는 경우 사용 해야 함을 (때문에 이것이 Apple **accounts.h** 헤더 Clang 서만 컴파일할 수 있으며). 추가 `--compiler:clang` Clang Xcode 4.6 또는 이전 버전을 사용 하는 경우 사용할 추가 mtouch 인수에 (Xamarin.iOS가 자동으로 선택 Clang Xcode 5.0 이상.)

- Xcode 4.6 (또는 이전) 이면를 사용 하 고 GCC / G + +를 선택 해야 합니다를 내보낸 형식의 경우 이름 (이 Xcode 4.6 에서도 제공 하는 Clang 버전 Objective-c 코드에서 내 식별자에서 비 ASCII 문자를 지원 하지 않으므로) 비 ASCII 문자를 포함 합니다. 추가 `--compiler:gcc` GCC를 사용 하려면 추가 mtouch 인수에 있습니다.

## <a name="selecting-a-registrar"></a>등록 기관 선택

프로젝트의 추가 mtouch 인수에는 다음 옵션 중 하나를 추가 하 여 다른 등록 기관을 선택할 수 있습니다 **iOS 빌드** 설정:

- `--registrar:static` – 장치 빌드에 대 한 기본
- `--registrar:dynamic` – 시뮬레이터 빌드에 대 한 기본

> [!NOTE]
> Xamarin의 클래식 API와 같은 다른 옵션을 지원 `--registrar:legacystatic` 고 `--registrar:legacydynamic`입니다. 그러나 이러한 옵션은 Unified API에서 지원 되지 않습니다.

## <a name="shortcomings-in-the-old-registration-system"></a>이전 등록 시스템의 단점

이전 등록 시스템에 다음과 같은 단점이 있습니다.

- 없음 (네이티브) 정적으로 참조 하는 제 3 자 (모든 항목이 제거 됩니다) 때문에 실제로 사용 되지 않은 네이티브 코드를 제거 하려면 네이티브 링커가 요청 수 없습니다 즉 타사 네이티브 라이브러리의 Objective-c 클래스 및 메서드 있었습니다. 이유는이 `-force_load libNative.a` 수행 하는 모든 타사 바인딩 (이거나 이와 동등한 `ForceLoad=true` 에 `[LinkWith]` 특성).
- 경고 없이 Objective-c 이름이 같은 두 개의 관리 되는 형식을 내보낼 수 있습니다. 드문 시나리오 2를 사용 하 여 종료 하는 것 이었습니다 `AppDelegate` 다른 네임 스페이스의 클래스입니다. 런타임 시 것 완전 한 임의 어느 (사실 매우 어려운 고 불편 함을 느끼게 디버깅 환경을 위해을 작성 되지 않은-하는 앱의 실행 간에 다양 한 것) 선택 되었습니다.
- Objective-c 시그니처가 동일한 두 메서드를 내보낼 수 있습니다. 아직 다시 Objective-c에서 호출 되는 임의 (했지만 실제로이 버그를 경험 하는 유일한 방법은 운 관리 되는 메서드를 재정의 하는 것 이었습니다 있으므로이 문제는 이전 쿼리에서와 같이 일반적인 않았습니다).
- 내보낸 메서드 집합에서 동적 및 정적 빌드 간에 약간 다릅니다.
- 제네릭 클래스를 내보낼 때 제대로 작동 하지 않습니다 (런타임에 실행 하는 정확한 제네릭 구현을 무작위 일 효과적으로 결정 되지 않은 동작이 발생).

<a name="required-modifications" />

## <a name="new-registrar-required-changes-to-bindings"></a>새 등록자: 필요한 바인딩 변경

이 섹션에는 새 등록자를 사용 하기 위해 수행 해야 하는 바인딩 변경 내용을 설명 합니다.

### <a name="protocols-must-have-the-protocol-attribute"></a>프로토콜 [Protocol] 특성이 있어야 합니다.

프로토콜 있어야 이제는 `[Protocol]` 특성입니다. 이렇게 하지 않으면 기본 링커 오류와를 같은 됩니다.

```console
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>선택 기가 유효한 매개 변수 수가 있어야 합니다.

모든 선택기 개수의 매개 변수를 올바르게 표시 해야 합니다. 이전에 이러한 오류가 무시 되었습니다 및 런타임 문제가 발생할 수 있습니다.

즉, 콜론 수가 매개 변수 개수가 일치 해야 합니다.

- 매개 변수가 없습니다. `foo`
- 하나의 매개 변수: `foo:`
- 두 매개 변수: `foo:parameterName2:`

다음은 잘못 된 사용입니다.

```csharp
// Invalid: export takes no arguments, but function expects one
[Export ("apply")]
void Apply (NSObject target);

// Invalid: exported as taking an argument, but the managed version does not have one:
[Export ("display:")]
void Display ();
```

### <a name="use-isvariadic-parameter-in-export"></a>내보내기에서 IsVariadic 매개 변수를 사용 합니다.

Variadic 함수를 사용 해야 합니다 `IsVariadic` 인수는 `[Export]` 특성:

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>기존 기호에 연결 해야 합니다.

네이티브 라이브러리에 존재 하지 않는 클래스에 바인딩하는 것이 불가능 합니다.
클래스에서 제거 되었거나 네이티브 라이브러리에서 이름을 바꿀 경우에 맞게 바인딩을 업데이트 해야 합니다.
