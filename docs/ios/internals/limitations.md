---
title: "제한 사항"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 43b099e8ddd6acc3e8cc4ce94580313a39a0c686
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="limitations"></a>제한 사항

Xamarin.iOS를 사용 하 여 iPhone에 있는 응용 프로그램은 정적 코드에 컴파일됩니다 되므로 런타임에 코드 생성을 필요로 하는 모든 기능을 사용할 수 없습니다.

다음은 데스크톱 모노에 비해 Xamarin.iOS 제한 사항입니다.

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>제한 된 제네릭 지원

일반적인 모노/.NET 달리 iPhone의 코드는 정적으로 JIT 컴파일러에서 필요에 따라 컴파일 중인 하지 않고 미리 컴파일됩니다.

모노의 [전체 AOT](http://www.mono-project.com/AOT#Full_AOT) 기술에는 제네릭와 관련 하 여 몇 가지 제한 사항,이 컴파일 타임에 모든 가능한 제네릭 인스턴스화를 들겠지만 결정 때문에 발생 합니다. 코드가 항상 시간 컴파일러에는 마법사를 사용 하 여 런타임에 컴파일됩니다 대로 일반.NET 또는 모노 런타임에 대 한 문제가 되지 않습니다. 하지만이 사용 하는 경우 Xamarin.iOS 같은 정적 컴파일러에 대 한 합니다.

개발자가를 실행 하는 일반적인 문제 중 일부는 다음과 같습니다.

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>제네릭 NSObjects 하위 클래스는 제한 됩니다.

Xamarin.iOS 현재 제한 된 제네릭 메서드에 대 한 지원 되지 않습니다 같은 NSObject 클래스의 제네릭 하위 클래스를 만들 수 있는 기능 7.2.1, 현재 NSObjects의 제네릭 하위 클래스를 사용 하는 다음과 같은 가능한:

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> NSObjects의 제네릭 하위 클래스는 가능한, 몇 가지 제한이 있습니다. 읽기는 [NSObject의 제네릭 하위 클래스](~/ios/internals/api-design/nsobject-generics.md) 문서에 대 한 자세한 내용은



### <a name="pinvokes-in-generic-types"></a>제네릭 형식에 P/Invokes

P/Invoke 제네릭 클래스에서 지원 되지 않습니다.

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### <a name="propertysetinfo-on-a-nullable-type-is-not-supported"></a>Property.SetInfo Nullable 형식에서 지원 되지 않습니다.

리플렉션의 Property.SetInfo 인 null 허용에 값을 설정 하려면 사용 하 여&lt;T&gt; 현재 지원 되지 않습니다.

 <a name="Value_types_as_Dictionary_Keys" />


### <a name="value-types-as-dictionary-keys"></a>사전의 키와 값 형식

값 형식을 사용 하 여 사전으로 서&lt;TKey, TValue&gt; 키가 문제가 있는, 기본적으로 사전 생성자 EqualityComparer 사용 하려고 시도&lt;TKey&gt;합니다. 기본값입니다. EqualityComparer&lt;TKey&gt;합니다. 기본적으로 리플렉션을 사용 하 여 IEqualityComparer를 구현 하는 새 형식을 인스턴스화하지 차례로 시도&lt;TKey&gt; 인터페이스입니다.

이 참조 형식에 적용 (반사로 새 + 형식 단계를 건너뜁니다), 이지만 값에 대 한 충돌 형식 및 장치에 사용 하려고 하면 일단 보다 신속 하 게 번입니다.

 **해결 방법**: 수동으로 구현는 [IEqualityComparer&lt;TKey&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) 인터페이스는 새 형식이 고 해당 형식의 인스턴스를 제공 된 [사전&lt;TKey, TValue&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%3CTKey,TValue%3E/) [(IEqualityComparer&lt;TKey&gt;)](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) 생성자입니다.


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>동적 코드를 생성 하지

IPhone의 커널 응용 프로그램에서 코드를 동적으로 생성 하는 것을 금지 하므로 iPhone의 모노 동적 코드 생성의 모든 형태를 지원 하지 않습니다. 여기에는 다음이 포함됩니다.

-  System.Reflection.Emit ´ ù.
-  System.Runtime.Remoting 지원 되지 않습니다.
-  동적으로 형식 만들기에 대 한 지원 되지 않습니다 (없음 Type.GetType ("MyType ' 1")), 하지만 기존 형식 (Type.GetType ("System.String") 예를 들어 사용할)을 조회 합니다. 
-  컴파일 타임에 역방향 콜백은 런타임에 등록 되어야 합니다.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

System.Reflection 부족 합니다. **내보낼** 런타임 코드 생성에 종속 된 코드가 작동 함을 의미 합니다. 등이 포함 됩니다.

-  동적 언어 런타임입니다.
-  동적 언어 런타임 기반으로 구축 된 모든 언어입니다.
-  Remoting의 TransparentProxy 또는 코드를 동적으로 생성 하도록 런타임에 발생 시키는 기타 항목입니다. 


 **중요:** 혼동 하지 마십시오. **Reflection.Emit** 와 **리플렉션**합니다. Reflection.Emit 코드를 동적으로 생성 하는 방법에 대 한 이며 해당 jit 컴파일된 코드와 네이티브 코드로 컴파일됩니다. IPhone (JIT 컴파일 제외)의 제한으로 인해이 지원 되지 않습니다.

하지만 Type.GetType ("someClass") 메서드를 나열 하는 특성 및 값을 가져오는 속성을 나열을 포함 하 여 전체 리플렉션 API를 문제 없이 작동 합니다.

 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>역방향 콜백

표준 모노 함수 포인터 대신이 대화 상자 비관리 코드에 C# 대리자 인스턴스를 전달 하는 것이 같습니다. 런타임에서 관리 되지 않는 코드가 관리 코드를 다시 호출할 수 있도록 허용 하는 작은 썽크 함수 포인터를 변형 일반적으로 합니다.

모노 이러한 브리지 시간에서 마법사에 의해 구현 됩니다 컴파일러입니다. 시간 미리 컴파일러를 사용 하 여 필요한 경우 iPhone에서 없는 다음이 시점에서 두 가지 중요 한 제한:

-  모든 콜백 메서드를 플래그 지정 해야 합니다는 [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/MonoPInvokeCallbackAttribute/) 
-  지원 되지 않습니다 예를 들어 메서드를 메서드를 정적 메서드 일 해야 있습니다. 


 
 <a name="No_Remoting" />


## <a name="no-remoting"></a>원격 작업 없음

Remoting 스택 Xamarin.iOS에 사용할 수 없는 경우


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>런타임 기능을 사용 하지 않도록 설정

모노의 iOS 런타임에에서 다음 기능이 비활성화 되었습니다.

-  프로파일러
-  Reflection.Emit
-  Reflection.Emit.Save 기능
-  COM 바인딩
-  JIT 엔진
-  메타 데이터 확인 프로그램 (없습니다 JIT 때문)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>.NET API 제한 사항

.NET API를 공개 전체 프레임 워크의 하위 집합은 iOS에서 사용할 수 있는 것은 아닙니다 표시 됩니다. 에 대 한 FAQ 참조는 [현재 지원 되는 어셈블리의 목록](~/cross-platform/internals/available-assemblies.md)합니다.



특히, Xamarin.iOS 사용 하는 API 프로필의 런타임 동작을 구성을 외부 XML 파일을 사용 하 여 이므로 System.Configuration 포함 되지 않습니다.
