---
title: Xamarin.iOS에 대 한 형식 등록
description: 이 문서에서는 C# 클래스를 Objective-c 런타임에 사용할 수 있도록 Xamarin.iOS 유형 등록자를 설명 합니다.
ms.prod: xamarin
ms.assetid: 610A0834-1141-4D09-A05E-B7ADF99462C5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e818d6a2092f408823e4a635a70c4f6666e3a7a9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786176"
---
# <a name="type-registrar-for-xamarinios"></a>Xamarin.iOS에 대 한 형식 등록

이 문서에서는 Xamarin.iOS에서 사용 하는 형식 등록 시스템에 설명 합니다.

## <a name="registration-of-managed-classes-and-methods"></a>등록 관리 되는 클래스 및 메서드

시작 하는 동안 Xamarin.iOS 등록 됩니다.

  - 클래스와 [[등록]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) Objective-c 클래스로 특성입니다.
  - 클래스와 [[Category]](https://developer.xamarin.com/api/type/CRuntime.CategoryAttribute) Objective-c 범주로 특성입니다.
  - 와 상호 작용 한 [[Protocol]](https://developer.xamarin.com/api/type/Foundation.ProtocolAttribute/) Objective-c 프로토콜로 특성입니다.

각 멤버의 경우에는 [[내보내기]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) 특성 Objective c 내보내집니다. 이렇게 하면 관리 되는 클래스가 Objective C에서 호출 될 메서드를 관리 하 고 만들고 메서드 및 속성 하나 Objective C 및 C# 세계 간의 연결 된 하는 방법입니다.

매우 간단한 예는 모든 응용 프로그램에 있는 AppDelegate 클래스입니다. 관리 되는 Main 메서드에 이와 같은 줄에 회수 됩니다.

    UIApplication.Main (args, null, "AppDelegate");

이 응용 프로그램의 대리자 클래스 "AppDelegate" 이라는 형식을 만들 Objective C 런타임을 지시 합니다.  C#으로 작성 된 "AppDelegate" 클래스의 인스턴스를 만드는 방법을 알고 Objective-c 런타임용이 클래스에를 등록 해야 합니다.

Xamarin.iOS 런타임 하므로 등록, 내부적으로이 기능을이 등록 (동적 등록) 런타임 시 전적으로 수행할 수 있습니다 또는 컴파일 시 (정적 등록)를 수행할 수 있습니다.  동적 방법은 시작할 때 리플렉션을 사용 하 여 모든 클래스 및 메서드를 등록 하 고 전달 Objective C 런타임 확인란을 찾을 수 있습니다.  정적 접근 방식은 컴파일 타임에 응용 프로그램에서 사용 되는 어셈블리를 검사 합니다.  클래스와 Objective-c를 등록 하는 메서드를 결정 하 고 이진 파일에 포함 되는 지도 생성 합니다.  그런 다음 시작 시 맵을 사용 하 여 등록 Objective C 런타임.

### <a name="categories"></a>범주

Xamarin.iOS 8.10 것부터 C# 구문을 사용 하 여 Objective-c 범주를 만들 수 됩니다.

이 작업은 수행 특성에 대 한 인수로 확장할 형식을 지정 하 고 [Category] 특성을 사용 합니다.
다음 예제에서는 NSString 확장할 예를 들어 됩니다.

    [Category (typeof (NSString))]

각 범주 메서드 Objective-c [내보내기] 특성을 사용 하 여 메서드 내보내기 위한 기본 메커니즘을 사용 하는:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

모든 관리 되는 확장 메서드는 정적 이어야 합니다. 그러나 C#의 확장 메서드에 대 한 표준 구문을 사용 하 여 Objective-c 인스턴스 메서드를 만들 수 있습니다.

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

및 확장 메서드의 첫 번째 인수에는 메서드가 호출 된 인스턴스가 됩니다.

전체 예제:

    [Category (typeof (NSString))]
    public static class MyStringCategory
    {
        [Export ("toUpper")]
        static string ToUpper (this NSString self)
        {
            return self.ToString ().ToUpper ();
        }
    }

이 예제에서는 NSString 클래스에 없습니다. 목표에서에서 호출할 수 있는 네이티브 toUpper 인스턴스 메서드를 추가 합니다.

    [Category (typeof (UIViewController))]
    public static class MyViewControllerCategory
    {
        [Export ("shouldAutoRotate")]
        static bool GlobalRotate ()
        {
            return true;
        }
    }

### <a name="protocols"></a>프로토콜

Xamarin.iOS 8.10 부터는 [Protocol] 특성을 가진 인터페이스를 내보낼 Objective-c 프로토콜로 합니다.

예제:

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

이를 내보낼 Objective-c (MyProtocol) 프로토콜 및 프로토콜을 구현 하는 클래스 (MyClass).

 **동적 등록**

빌드/디버그 주기를 가속화 하는 대로 시뮬레이터 빌드에 사용 됩니다.  이 응용 프로그램을 시작 하 고 일반 용도의 시작 관리자가 될 때마다를 대신 사용 될 때마다 클래스 매핑와 프로그램 응용 프로그램 실행 프로그램에이 맵 테이블의 컴파일을 생성 하는 단계를 제거 하 여 결과입니다.  데스크톱 컴퓨터에 런타임 수행 하는 전원 충분 하므로 성능이 중요 하지 않은 클래스의 신속 하 게 검색 합니다.

 **정적 등록**

모바일 장치를 데스크톱 컴퓨터 보다 느립니다 하 여 런타임 수행 검색 속도가 느린 장치 빌드에 사용 됩니다.  장치 빌드 새 이진을 만들 필요가 항상, 빌드/디버그 주기 등록 매핑 생성에 의해 영향을 받지 않습니다.

## <a name="new-registration-system"></a>새 등록 시스템

안정적인 6.2.6 부터는 버전과 6.3.4 베타 버전 새 정적 등록자 추가 했습니다. 7.2.1에서 버전 내렸습니다 새 등록자 기본값입니다.

이 새 등록 시스템에서는 다음과 같은 새로운 기능을 제공합니다.

- 프로그래머가 오류 시간 검색을 컴파일하십시오.
    - 동일한 이름으로 등록 되 고 두 클래스입니다.
    - 내보낸 동일한 선택기에 응답 하는 둘 이상의 메서드



- 사용 되지 않는 네이티브 코드를 제거할 수 있습니다.
    - 새 등록 시스템 결과 이진에서 사용 되지 않는 네이티브 코드를 제거 네이티브 링커 수 있도록 정적 라이브러리에 사용 된 코드에 대 한 강력한 참조를 추가 합니다.
      Xamarin의 샘플 바인딩에서 대부분의 응용 프로그램 300 k 이상 더 작은 수 있습니다.

- NSObject의 제네릭 하위 클래스에 대 한 지원입니다. 참조 [NSObject 제네릭](~/ios/internals/api-design/nsobject-generics.md) 자세한 정보에 대 한 합니다. 또한 새 등록 시스템은 런타임 시 임의의 동작이 발생 이전에 지원 되지 않는 일반 구문 catch 합니다.

다음은 새 registar에서 발생 하는 오류의 몇 가지 예입니다.

동일한 클래스에서 동일한 선택기를 두 번 이상 내보냅니다.

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo:")]
    void Foo (NSString str);
    [Export ("foo:")]
    void Foo (string str)
}
```

Objective C 이름이 같은 둘 이상의 관리 되는 클래스를 내보내는 중입니다.

```csharp
[Register ("Class")]
class MyClass : NSObject {}

[Register ("Class")]
class YourClass : NSObject {}
```

제네릭 메서드를 내보내는 중입니다.

```csharp
[Register]
class MyDemo : NSObject {
    [Export ("foo")]
    void Foo<T> () {}
}
```



새 등록 기관에 대 한 염두에 몇 가지 사항은 다음과 같습니다.
- 일부 타사 라이브러리를 새 등록 시스템에서 사용 하도록 업데이트 해야 합니다. 섹션을 참조 하세요. [수정 아래 필요한](#required_modifications) 자세한 세부 정보에 대 한 합니다.
- 단기 단점은 이기도 계정 프레임 워크에서 사용 되는 경우 Clang에 사용 해야 합니다 (이 Apple의 accounts.h 헤더는 Clang 서만 컴파일할 수 있으며 때문에). 추가 <code>--compiler:clang</code> 추가 mtouch 인수로 경우 Clang을 사용 하 여 Xcode 4.6 또는 이전 버전을 사용 하는 (Xamarin.iOS가 자동으로 선택 Xcode 5.0 이상 Clang.)

    <li>Xcode 4.6 (또는 이전 버전)이 사용 되는 GCC / G + + 선택 해야 내보낸 형식 이름 (이 Xcode 4.6과 함께 제공 되는 Clang 버전 Objective C 코드에서 식별자 내 비 ascii 문자를 지원 하지 않으므로) 비 ascii 문자가 포함 합니다. 추가 <code>--compiler:gcc</code> GCC를 사용 하 고 추가 mtouch 인수에 있습니다.


## <a name="selecting-a-registrar"></a>등록자를 선택합니다.

프로젝트의 iOS 빌드 옵션에서에서 추가 mtouch 인수에는 다음 옵션 중 하나를 추가 하 여 다른 등록 기관을 선택할 수 있습니다.

-  `--registrar:static` : 장치 빌드에 대 한 기본
-  `--registrar:dynamic` : 시뮬레이터 빌드에 대 한 기본
-  `--registrar:legacystatic` : Xamarin.iOS 7.2.1 될 때까지 장치 빌드에 대 한 기본
-  `--registrar:legacydynamic` : 시뮬레이터 Xamarin.iOS 7.2.1까지 빌드에 대 한 기본


## <a name="shortcomings-in-the-old-registration-system"></a>기존 등록 시스템의 단점

기존 등록 시스템에 다음과 같은 단점이 있습니다.

-  (모든 항목이 제거 됩니다) 때문에 실제로 사용 되지 않은 공급 업체 네이티브 코드를 제거 하려면 기본 링커 하 게 수 없습니다. 즉 제 3 자 네이티브 라이브러리의 Objective-c 클래스 및 메서드를 정적 (네이티브) 참조가 했습니다. 에 대 한 이유는 "-force_load libNative.a" 모든 공급 업체 바인딩에 수행 해야 (또는 해당 ForceLoad = [LinkWith] 특성에서 true).
-  경고 없이 동일한 Objective-c 이름의 관리 되는 두 형식을 내보낼 수 있습니다. 거의 사용 되지 않습니다 (다른 네임 스페이스)에 두 개의 AppDelegate 클래스 끝나야 하는 것 이었습니다. 런타임 시 것은 완전히 임의의 어떤 것 (하느라 들고 하면 당황 스 러 디버깅 환경을 다시 작성도 되지 않은-앱의 실행 간에 다양 한 팩트)에서 찾았습니다.
-  Objective C 시그니처가 같은 두 메서드를 내보낼 수 있습니다. 아직 다시 Objective C에서 호출할 수는 어떤 것이 임의의 (하지만 실제로이 버그를 발생 하는 유일한 방법은 운 관리 되는 메서드를 재정의 하는 것 이었습니다 때문에 주로이 문제는 이전 쿼리에서와 같이 일반적인 되지).
-  내보낸 메서드의 집합 동적 및 정적 빌드 간 약간 다른 했습니다.
-  제네릭 클래스를 내보낼 때 제대로 작동 하지 않습니다 (어떤 정확한 제네릭 구현을 런타임에 실행 일 임의 효과적으로 지정 하지 않은 동작이 생성).


 <a name="required_modifications" />


## <a name="new-registrar-required-changes-to-bindings"></a>바인딩 새 등록자: 필요한 변경 사항


기존 Objective-c 바인딩을 새 registar 사용 하도록 업데이트 되어야 할 수 있습니다.

다음은 수행 해야 하는 변경 사항 목록입니다.

### <a name="protocols-must-have-the-protocol-attribute"></a>프로토콜 [Protocol] 특성이 있어야 합니다.

프로토콜 구현에는 이제 [Protocol] 특성을 적용할 수 있어야 합니다.  이렇게 하지 않으면 이와 같은 네이티브 링커 오류가 발생을 합니다.

```csharp
Undefined symbols for architecture i386: "_OBJC_CLASS_$_ProtocolName", referenced from: ...
```

### <a name="selectors-must-have-a-valid-number-of-parameters"></a>선택기에는 매개 변수 수가 잘못 있어야 합니다.

모든 선택기 개수의 매개 변수를 올바르게 지정 해야 합니다.  이전에 이러한 오류가 무시 되었습니다 및 런타임 문제를 일으킬 수입니다.

즉, 콜론 수가 매개 변수 개수가 일치 해야 합니다.

예를 들어:

-  매개 변수가 없습니다: 'foo'
-  하나의 매개 변수: ' foo:'
-  두 개의 매개 변수: ' foo:parameterName2:'


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

Variadic 함수 해야 하므로 해당 내보내기 특성에 올린 (선택기는 수의 인수에 대 한 잘못 된 때문에 이것이, 즉, 2를 가리키고 있습니다. 위에서 위반):

```csharp
[Export ("variadicMethod:", IsVariadic = true)]
void VariadicMethod (NSObject first, IntPtr subsequent);
```

### <a name="must-link-to-existing-symbols"></a>기존 기호에 연결 해야 합니다.

네이티브 라이브러리에 존재 하지 않는 클래스에 바인딩하는 것이 불가능 합니다.

존재 하지 않는 클래스 바인딩하려고 하는 경우 네이티브 링커 오류를 받습니다.

이 문제는 일반적으로 바인딩을 일정 시간 동안 존재 하 고 네이티브 코드는 특정 기본 클래스 중 하나 제거 되거나 바인딩을 업데이트 되지 않은 동안 이름이 변경 되도록이 시간 동안 수정한 후 때 발생 합니다.

