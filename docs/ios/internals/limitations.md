---
title: Xamarin.iOS의 제한 사항
description: 이 문서에는 Xamarin.iOS, 제네릭, 제네릭 개체의 P/Invoke NSObjects 등의 제네릭 서브 클래스에 대해 설명 제한 사항을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/09/2018
ms.openlocfilehash: b79d3683c8e4979cbbd13550f3df86c39622ad2b
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58870184"
---
# <a name="limitations-of-xamarinios"></a>Xamarin.iOS의 제한 사항

정적 코드를 Xamarin.iOS를 사용 하 여 응용 프로그램을 컴파일한 후 런타임에 코드를 생성 해야 하는 기능을 사용 하는 것이 불가능 합니다.

다음은 Mono 데스크톱에 비해 Xamarin.iOS 제한 사항입니다.

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>제한 된 제네릭 지원

일반적인 Mono/.NET, 달리 iPhone의 코드는 정적으로 요청 시 JIT 컴파일러로 컴파일할 대신 미리 컴파일됩니다.

Mono [전체 AOT](https://www.mono-project.com/docs/advanced/aot/#full-aot) 기술에는 제네릭 관련 하 여 몇 가지 제한 사항, 이러한는 때문에 컴파일 시간에 모든 가능한 제네릭 인스턴스화를 미리 확인할 수 있습니다. 코드는을 사용 하 여 시간 컴파일러에서 런타임에 컴파일된 항상 일반.NET 또는 Mono 런타임 문제가 아닙니다. 하지만이 Xamarin.iOS와 같은 정적 컴파일러에 대 한 문제를 제기 합니다.

개발자를 실행 하는 일반적인 문제 중 일부는 다음과 같습니다.

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>NSObjects의 제네릭 서브 클래스는 제한 됩니다.

현재 Xamarin.iOS 제네릭 메서드에 대 한 지원 되지 않습니다 같은 NSObject 클래스의 제네릭 서브 클래스를 만드는 기능 지원이 제한적입니다. 7.2.1를 기준으로 NSObjects의 제네릭 서브 클래스를 사용 하 여이 이와 같은 가능 합니다.

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> NSObjects의 제네릭 서브 클래스는 가능한, 몇 가지 제한이 있습니다. 읽기를 [NSObject의 제네릭 서브](~/ios/internals/api-design/nsobject-generics.md) 자세한 문서


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>동적 코드를 생성 하지 않음

IOS 커널에서 동적 코드 생성에서 응용 프로그램 방지, 때문에 Xamarin.iOS 동적 코드 생성의 모든 형태를 지원 하지 않습니다. 여기에는 다음이 포함됩니다.

-  System.Reflection.Emit 제공 되지 않습니다.
-  System.Runtime.Remoting 지원 되지 않습니다.
-  형식을 동적으로 만드는 지원 되지 않습니다 (없습니다 Type.GetType ("MyType ' 1")), 하지만 기존 형식 (Type.GetType ("System.String") 아무 문제 없이 작동 예를 들어,)를 조회 합니다. 
-  역방향 콜백은 컴파일 시 런타임에 등록 되어야 합니다.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

System.Reflection 부족 합니다. **내보낼** 런타임 코드 생성에 종속 된 코드가 작동 함을 의미 합니다. 이 등이 포함 됩니다.

-  동적 언어 런타임입니다.
-  동적 언어 런타임을 기반으로 구축 된 모든 언어입니다.
-  Remoting의 TransparentProxy 또는 런타임 코드를 동적으로 생성 하는 다른 것입니다. 


 **중요:** 혼동 하지 마십시오 **Reflection.Emit** 사용 하 여 **리플렉션**합니다. Reflection.Emit 코드를 동적으로 생성 하는 방법에 대 한 이며 해당 jit 컴파일된 코드 및 네이티브 코드로 컴파일됩니다. IOS (JIT 컴파일 안 함)의 제한으로 인해이 지원 되지 않습니다.

하지만 Type.GetType ("someClass") 메서드를 나열 하는 특성 및 값을 가져오는 속성을 나열을 비롯 한 전체 리플렉션 API는 잘 작동 합니다.

### <a name="using-delegates-to-call-native-functions"></a>대리자를 사용 하 여 네이티브 함수를 호출 하려면

C# 대리자를 통해 네이티브 함수를 호출 하려면 대리자의 선언 다음 특성 중 하나를 사용 하 여 데코레이팅 해야 합니다.

- [UnmanagedFunctionPointerAttribute](xref:System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute) (기본 플랫폼 간 및.NET 표준 1.1 +와 호환 되므로)
- [MonoNativeFunctionWrapperAttribute](xref:ObjCRuntime.MonoNativeFunctionWrapperAttribute)

이러한 특성 중 하나를 제공 하는 데 실패 하면 다음과 같은 런타임 오류가 발생 합니다.

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```
 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>역방향 콜백

표준 Mono에서 함수 포인터를 대신 관리 되지 않는 코드를 C# 대리자 인스턴스를 전달 하는 것이 같습니다. 런타임에서 비관리 코드에서 관리 코드로 콜백할 수 있도록 작은 썽크를 해당 함수 포인터 변환 일반적으로 합니다.

Mono에서 이러한 브리지는 구현한 시간에서 Just 컴파일러. 시간 미리 컴파일러를 사용 하 여 필요한 경우 iPhone에서 두 가지 중요 한 제약이 따릅니다가 시점에서:

-  사용 하 여 콜백 메서드의 모든 플래그 해야는 [MonoPInvokeCallbackAttribute](xref:ObjCRuntime.MonoPInvokeCallbackAttribute)
-  메서드가 정적 메서드로 지원은 없습니다. 예를 들어 메서드.
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>없는 원격

원격 스택을 Xamarin.iOS에서 제공 되지 않습니다.


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>런타임 기능을 사용 하지 않도록 설정

Mono의 iOS 런타임에서에서 다음 기능이 비활성화 되었습니다.

-  프로파일러
-  Reflection.Emit
-  Reflection.Emit.Save 기능
-  COM 바인딩
-  JIT 엔진
-  메타 데이터 검증 도구 (없습니다 JIT 때문)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>.NET API 제한 사항

.NET API를 공개 전체 프레임 워크의 하위 집합은는 iOS에서 사용할 수 있는 것은 아닙니다. 에 대 한 FAQ를 참조를 [현재 지원 되는 어셈블리 목록을](~/cross-platform/internals/available-assemblies.md)합니다.



특히, Xamarin.iOS를 사용한 API 프로필 외부 XML 파일의 런타임 동작을 구성 하는 데 불가능 하므로 System.Configuration을 포함 하지 않습니다.
