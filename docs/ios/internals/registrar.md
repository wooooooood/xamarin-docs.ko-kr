---
title: Xamarin.ios에 대 한 형식 등록자
description: 이 문서에서는 클래스를 목표-C 런타임에 사용할 수 있도록 C# 하는 xamarin.ios 형식 등록자에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: f38c49ce9334a5659f0a8b5dd03e3bae8863cf5a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022273"
---
# <a name="type-registrar-for-xamarinios"></a>Xamarin.ios에 대 한 형식 등록자

이 문서에서는 Xamarin.ios에서 사용 하는 형식 등록 시스템에 대해 설명 합니다.

## <a name="registration-of-managed-classes-and-methods"></a>관리 되는 클래스 및 메서드 등록

시작 하는 동안 Xamarin.ios는 다음을 등록 합니다.

- [[Register]](xref:Foundation.RegisterAttribute) 특성을 목표로 하는 클래스를 목표로 C 클래스로 바꿉니다.
- [[Category]](xref:ObjCRuntime.CategoryAttribute) 특성이 있는 클래스를 목표-C 범주로 바꿉니다.
- [[Protocol]](xref:Foundation.ProtocolAttribute) 특성이 목표-C 프로토콜인 인터페이스
- [[Export]](xref:Foundation.ExportAttribute)를 사용 하는 멤버를 사용 하 여 목표에 액세스할 수 있습니다.

예를 들어 Xamarin.ios 응용 프로그램에서 일반적으로 관리 되는 `Main` 메서드를 살펴보겠습니다.

```csharp
UIApplication.Main (args, null, "AppDelegate");
```

이 코드는 목표-C 런타임에 응용 프로그램의 대리자 클래스로 `AppDelegate` 라는 형식을 사용 하도록 지시 합니다. 목적-C 런타임이 C#`AppDelegate`클래스의 인스턴스를 만들 수 있도록 하려면 해당 클래스를 등록 해야 합니다.

Xamarin.ios는 런타임에 (동적 등록) 또는 컴파일 시간 (정적 등록)에서 등록을 자동으로 수행 합니다.

동적 등록은 시작 시 리플렉션을 사용 하 여 등록할 모든 클래스와 메서드를 찾고이를 목표-C 런타임에 전달 합니다. 동적 등록은 시뮬레이터 빌드에 대해 기본적으로 사용 됩니다.

정적 등록은 컴파일 시간에 응용 프로그램에서 사용 하는 어셈블리를 검사 합니다. 이 클래스는 목표 C에 등록할 클래스와 메서드를 결정 하 고 이진에 포함 된 맵을 생성 합니다.
그런 다음 시작 시 지도를 목표-C 런타임으로 등록 합니다. 정적 등록은 장치 빌드에 사용 됩니다.

### <a name="categories"></a>범주

Xamarin.ios 8.10부터 구문을 사용 하 여 C# 목표-C 범주를 만들 수 있습니다.

범주를 만들려면 `[Category]` 특성을 사용 하 고 확장할 형식을 지정 합니다. 예를 들어 다음 코드는 `NSString`를 확장 합니다.

```csharp
[Category (typeof (NSString))]
```

각 범주의 메서드에는 `[Export]` 특성이 있으며,이 특성은 목표-C 런타임에 사용할 수 있도록 합니다.

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

모든 관리 되는 확장 메서드는 정적 이어야 하지만 확장 메서드에 대 한 표준 C# 구문을 사용 하 여 객관적인 C 인스턴스 메서드를 만들 수 있습니다.

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

확장 메서드의 첫 번째 인수는 메서드가 호출 된 인스턴스입니다.

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

이 예제에서는 `NSString` 클래스에 네이티브 `toUpper` 인스턴스 메서드를 추가 합니다. 이 메서드는 목표-C에서 호출 될 수 있습니다.

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

Xamarin.ios 8.10부터 `[Protocol]` 특성을 사용 하는 인터페이스는 다음과 같은 목적-C를 프로토콜로 내보냅니다.

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

이 코드는 `IMyProtocol`를 `MyProtocol` 라는 프로토콜로, 프로토콜을 구현 하는 `MyClass` 클래스를 목표로 내보냅니다.

## <a name="new-registration-system"></a>새 등록 시스템

안정적인 6.2.6 버전 및 beta 6.3.4 버전부터 새 정적 등록자를 추가 했습니다. 7\.2.1 버전에서는 새 등록자를 기본값으로 만들었습니다.

이 새 등록 시스템은 다음과 같은 새로운 기능을 제공 합니다.

- 프로그래머 오류에 대 한 컴파일 시간 검색:
  - 동일한 이름을 사용 하 여 두 클래스를 등록 합니다.
  - 동일한 선택기에 응답 하기 위해 내보낸 메서드가 둘 이상 있습니다.
- 사용 하지 않는 네이티브 코드 제거:
  - 새 등록 시스템은 정적 라이브러리에서 사용 되는 코드에 대 한 강력한 참조를 추가 하 여 네이티브 링커가 결과 이진에서 사용 하지 않는 네이티브 코드를 제거할 수 있도록 합니다. Xamarin의 샘플 바인딩에서 대부분의 응용 프로그램은 최소 제한은 30만 개의 됩니다.

- `NSObject`의 제네릭 서브 클래스에 대 한 지원 자세한 내용은 [Nsobject 제네릭을](~/ios/internals/api-design/nsobject-generics.md) 참조 하세요. 또한 새 등록 시스템은 런타임에 임의 동작을 발생 시킨 지원 되지 않는 제네릭 구문을 catch 합니다.

### <a name="errors-caught-by-the-new-registrar"></a>새 등록자에 의해 발생 한 오류

다음은 새 등록자에 의해 발생 한 오류의 몇 가지 예입니다.

- 동일한 클래스에서 동일한 선택기를 두 번 이상 내보냅니다.

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

- 동일한 목표를 사용 하 여 둘 이상의 관리 되는 클래스 내보내기-C 이름:

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

새 등록자를 염두에 두어야 할 몇 가지 사항은 다음과 같습니다.

- 새 등록 시스템을 사용 하려면 일부 타사 라이브러리를 업데이트 해야 합니다. 자세한 내용은 아래의 [필수 수정 사항을](#required-modifications) 참조 하세요.

- 단기 단점은 계정 프레임 워크를 사용 하는 경우 Clang를 사용 해야 한다는 것입니다 .이는 **Apple의 Clang** 헤더를로만 컴파일할 수 있기 때문입니다. Xcode 4.6 이전 버전을 사용 하는 경우 Clang를 사용 하기 위해 추가 mtouch 인수에 `--compiler:clang`를 추가 합니다 (Xamarin.ios는 Xcode 5.0 이상에서 Clang를 자동으로 선택).

- Xcode 4.6 (또는 이전 버전)를 사용 하는 경우 내보낸 형식 이름에 ASCII 문자가 아닌 문자가 포함 된 경우 GCC/G + +를 선택 해야 합니다 .이는 Xcode 4.6와 함께 제공 되는 Clang 버전이 목표-C 코드의 식별자 내에서 비 ASCII 문자를 지원 하지 않기 때문입니다. GCC를 사용 하기 위해 추가 mtouch 인수에 `--compiler:gcc`를 추가 합니다.

## <a name="selecting-a-registrar"></a>등록자 선택

프로젝트의 **IOS 빌드** 설정에서 추가 mtouch 인수에 다음 옵션 중 하나를 추가 하 여 다른 등록자를 선택할 수 있습니다.

- `--registrar:static` – 장치 빌드에 대 한 기본값
- `--registrar:dynamic` – 시뮬레이터 빌드에 대 한 기본값

> [!NOTE]
> Xamarin의 Classic API `--registrar:legacystatic` 및 `--registrar:legacydynamic`와 같은 다른 옵션을 지원 합니다. 그러나 이러한 옵션은 Unified API에서 지원 되지 않습니다.

## <a name="shortcomings-in-the-old-registration-system"></a>이전 등록 시스템의 단점

이전 등록 시스템의 단점은 다음과 같습니다.

- 타사 네이티브 라이브러리의 목적-C 클래스 및 메서드에 대 한 (네이티브) 정적 참조가 없습니다. 즉, 모든 항목이 제거 되기 때문에 실제로는 사용 되지 않는 타사 네이티브 코드를 제거 하도록 네이티브 링커에 요청할 수 없습니다. 이는 모든 타사 바인딩에서 수행 해야 하는 `-force_load libNative.a` 또는 `[LinkWith]` 특성에 해당 하는 `ForceLoad=true`입니다.
- 동일한 객관적인 C 이름으로 두 개의 관리 되는 형식을 내보낼 수 있습니다 (경고 없음). 드문 시나리오는 다른 네임 스페이스에 있는 두 개의 `AppDelegate` 클래스를 사용 하는 것 이었습니다. 런타임에는 완전히 무작위로 선택 된 것입니다. 즉, 매우 난해해 하 고 뛰어난 디버깅 환경을 위해 다시 작성 되지 않은 앱의 실행 사이에서 차이가 있습니다.
- 동일한 목표-C 서명을 사용 하 여 두 개의 메서드를 내보낼 수 있습니다. 이는 목표에서 임의로 호출 되는 것 이지만,이 문제는 일반적으로이 문제를 해결 하는 것이 unlucky 관리 되는 메서드를 재정의 하는 유일한 방법 이기 때문입니다.
- 내보낸 메서드 집합은 동적 빌드와 정적 빌드에서 약간 다릅니다.
- 제네릭 클래스를 내보낼 때 제대로 작동 하지 않습니다. 런타임에 실행 되는 정확한 제네릭 구현은 임의로 수행 되며 실제로는 결정 되지 않은 동작이 발생 합니다.

<a name="required-modifications" />

## <a name="new-registrar-required-changes-to-bindings"></a>새 등록자: 바인딩에 필요한 변경 내용

이 섹션에서는 새 등록자를 사용 하기 위해 수행 해야 하는 바인딩 변경 내용에 대해 설명 합니다.

### <a name="protocols-must-have-the-protocol-attribute"></a>프로토콜에는 [Protocol] 특성이 있어야 합니다.

이제 프로토콜에 `[Protocol]` 특성이 있어야 합니다. 이렇게 하지 않으면 다음과 같은 네이티브 링커 오류가 발생 합니다.

```console
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>선택기에는 올바른 수의 매개 변수가 있어야 합니다.

모든 선택기는 매개 변수 개수를 정확 하 게 나타내야 합니다. 이전에는 이러한 오류가 무시 되어 런타임 문제가 발생할 수 있었습니다.

즉, 콜론 수가 매개 변수 수와 일치 해야 합니다.

- 매개 변수 없음: `foo`
- 매개 변수 한 개: `foo:`
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

### <a name="use-isvariadic-parameter-in-export"></a>내보내기에서 IsVariadic 매개 변수 사용

Variadic 함수는 `[Export]` 특성에 `IsVariadic` 인수를 사용 해야 합니다.

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>기존 기호에 연결 해야 합니다.

네이티브 라이브러리에 없는 클래스는 바인딩할 수 없습니다.
네이티브 라이브러리에서 클래스가 제거 되거나 이름이 변경 된 경우 일치 하도록 바인딩을 업데이트 해야 합니다.
