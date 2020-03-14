---
title: Xamarin.ios의 제한 사항
description: 이 문서에서는 Xamarin.ios의 제한 사항, 제네릭 토론, NSObjects의 일반 서브 클래스, 제네릭 개체에서 P/Invoke 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/09/2018
ms.openlocfilehash: 91513936a0223af0e4220154d0fe65ee0a599a4f
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305866"
---
# <a name="limitations-of-xamarinios"></a>Xamarin.ios의 제한 사항

Xamarin.ios를 사용 하는 응용 프로그램은 정적 코드로 컴파일되기 때문에 런타임에 코드를 생성 해야 하는 기능을 사용할 수 없습니다.

다음은 데스크톱 Mono와 비교할 때 Xamarin.ios의 제한 사항입니다.

 <a name="Limited_Generics_Support" />

## <a name="limited-generics-support"></a>제한 된 제네릭 지원

기존 Mono/.NET과 달리 iPhone의 코드는 JIT 컴파일러에서 요청 시 컴파일되지 않고 미리 정적으로 컴파일됩니다.

Mono의 [전체 AOT](https://www.mono-project.com/docs/advanced/aot/#full-aot) 기술에는 제네릭과 관련 된 몇 가지 제한이 있습니다 .이는 가능한 제네릭 인스턴스화를 컴파일 시간에 앞으로 확인할 수 있기 때문에 발생 합니다. 코드는 항상 Just-in-time 컴파일러를 사용 하 여 런타임에 컴파일되기 때문에 일반적인 .NET 또는 Mono 런타임에는 문제가 되지 않습니다. 그러나 Xamarin.ios와 같은 정적 컴파일러의 경우에는 문제가 발생 합니다.

개발자가 실행 하는 일반적인 문제 중 몇 가지는 다음과 같습니다.

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />

### <a name="generic-subclasses-of-nsobjects-are-limited"></a>NSObjects의 일반 서브 클래스가 제한 됩니다.

현재 xamarin.ios는 제네릭 메서드를 지원 하지 않는 등 NSObject 클래스의 제네릭 서브 클래스를 만들 수 있도록 제한적으로 지원 합니다. 7\.2.1를 사용 하 여 다음과 같이 NSObjects의 제네릭 서브 클래스를 사용할 수 있습니다.

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> NSObjects의 일반 서브 클래스가 가능 하지만 몇 가지 제한 사항이 있습니다. 자세한 내용은 [NSObject 문서의 일반 서브 클래스](~/ios/internals/api-design/nsobject-generics.md) 를 참조 하세요.

 <a name="No_Dynamic_Code_Generation" />

## <a name="no-dynamic-code-generation"></a>동적 코드 생성 안 함

IOS 커널은 응용 프로그램이 동적으로 코드를 생성 하는 것을 방지 하므로 Xamarin.ios는 동적 코드 생성의 형태를 지원 하지 않습니다. 이러한 개체는 다음과 같습니다.

- System.object를 사용할 수 없습니다.
- System.object를 지원 하지 않습니다.
- 형식을 동적으로 만들 수 없습니다 (예: gettype ("MyType ' 1")). 예를 들어, 기존 형식 (예: GetType ("System.string"))을 조회 하는 것은 제대로 작동 합니다.
- 역방향 콜백은 컴파일 시간에 런타임에 등록 해야 합니다.

 <a name="System.Reflection.Emit" />

### <a name="systemreflectionemit"></a>System.Reflection.Emit

시스템 리플렉션이 부족 합니다. **내보내기** 는 런타임 코드 생성에 의존 하는 코드가 작동 하지 않음을 의미 합니다. 여기에는 다음과 같은 항목이 포함됩니다.

- 동적 언어 런타임입니다.
- 동적 언어 런타임 위에 빌드된 모든 언어
- Remoting의 TransparentProxy 또는 런타임에서 코드를 동적으로 생성 하 게 하는 기타 항목입니다.

  > [!IMPORTANT]
  > 리플렉션과 **리플렉션을 혼동**해서는 안 **됩니다.** JITed는 코드를 동적으로 생성 하 고 해당 코드를 네이티브 코드로 컴파일하고 컴파일하는 방법에 대 한 것입니다. IOS에 대 한 제한 사항 (JIT 컴파일 없음)으로 인해이는 지원 되지 않습니다.

그러나 GetType ("조각이 someclass")을 비롯 한 전체 리플렉션 API (""), 메서드 나열, 속성 나열, 특성 및 값 페치는 정상적으로 작동 합니다.

### <a name="using-delegates-to-call-native-functions"></a>대리자를 사용 하 여 네이티브 함수 호출

C# 대리자를 통해 네이티브 함수를 호출 하려면 대리자의 선언이 다음 특성 중 하나를 사용 하 여 데코 레이트 되어야 합니다.

- [UnmanagedFunctionPointerAttribute](xref:System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute) (플랫폼 간 및 .NET Standard 1.1 +와 호환 되므로 선호 됨)
- [MonoNativeFunctionWrapperAttribute](xref:ObjCRuntime.MonoNativeFunctionWrapperAttribute)

이러한 특성 중 하나를 제공 하지 못하면 런타임 오류가 발생 합니다.

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```

 <a name="Reverse_Callbacks" />

### <a name="reverse-callbacks"></a>역방향 콜백

표준 Mono에서는 함수 포인터 대신 대리자 인스턴스 C# 를 비관리 코드에 전달할 수 있습니다. 런타임은 일반적으로 이러한 함수 포인터를 관리 되지 않는 코드가 관리 코드로 다시 호출할 수 있도록 하는 작은 썽크로 변환 합니다.

Mono에서 이러한 브리지는 Just-in-time 컴파일러에 의해 구현 됩니다. IPhone에 필요한 사전 컴파일러를 사용 하는 경우이 시점에는 두 가지 중요 한 제한이 있습니다.

- 모든 콜백 메서드에 [MonoPInvokeCallbackAttribute](xref:ObjCRuntime.MonoPInvokeCallbackAttribute) 를 사용 하 여 플래그를 지정 해야 합니다.
- 메서드는 정적 메서드 여야 하며 인스턴스 메서드를 지원 하지 않습니다.

<a name="No_Remoting" />

## <a name="no-remoting"></a>원격이 없음

Xamarin.ios에서 원격 스택을 사용할 수 없습니다.

 <a name="Runtime_Disabled_Features" />

## <a name="runtime-disabled-features"></a>런타임 사용 안 함 기능

Mono의 iOS 런타임에서 다음 기능이 사용 하지 않도록 설정 되었습니다.

- 프로파일러
- 리플렉션. 내보내기
- Reflection. 저장 기능
- COM 바인딩
- JIT 엔진
- 메타 데이터 검증 도구 (JIT가 없기 때문)

 <a name=".NET_API_Limitations" />

## <a name="net-api-limitations"></a>.NET API 제한 사항

제공 되는 .NET API는 모든 것이 iOS에서 제공 되는 것이 아니라 전체 프레임 워크의 하위 집합입니다. [현재 지원 되는 어셈블리 목록은](~/cross-platform/internals/available-assemblies.md)FAQ를 참조 하십시오.

특히 Xamarin.ios에서 사용 하는 API 프로필에는 System.object가 포함 되어 있지 않으므로 외부 XML 파일을 사용 하 여 런타임의 동작을 구성할 수 없습니다.
