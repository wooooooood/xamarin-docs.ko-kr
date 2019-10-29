---
title: 목표에 대 한 .NET 포함 모범 사례-C
description: 이 문서에서는 목표로 .NET 포함을 사용 하기 위한 다양 한 모범 사례를 설명 합니다. 관리 코드의 하위 집합을 노출 하는 방법에 대해 설명 하 고 chunkier API, 이름 지정 등을 제공 합니다.
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 2f632e3218d817aa0162a63ea81c61ca18c52b93
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006791"
---
# <a name="net-embedding-best-practices-for-objective-c"></a>목표에 대 한 .NET 포함 모범 사례-C

이는 초안 이며 현재 도구에서 지원 되는 기능과 동기화 되지 않을 수 있습니다. 이 문서는 별도로 진화 하 고 최종적으로 최종 도구와 일치 하는 것이 좋습니다. 즉, 즉각적인 해결 방법이 아니라 장기적인 모범 사례를 제안 합니다.

이 문서의 많은 부분은 지원 되는 다른 언어에도 적용 됩니다. 그러나 제공 된 모든 예제는 C# 및 목표-C에 있습니다.

## <a name="exposing-a-subset-of-the-managed-code"></a>관리 코드의 하위 집합 노출

생성 된 네이티브 라이브러리/프레임 워크에는 노출 된 각 관리 되는 Api를 호출 하는 목표 C 코드가 포함 됩니다. 표시 되는 API (공용)를 늘리면 기본 _붙이기_ 라이브러리가 커집니다.

네이티브 개발자에 게 필요한 Api만 노출 하는 다른 작은 어셈블리를 만드는 것이 좋을 수 있습니다. 이 외관을 사용 하면 표시 유형, 명명, 오류 검사 등을 더 자세히 제어할 수 있습니다. 생성 된 코드의입니다.

## <a name="exposing-a-chunkier-api"></a>Chunkier API 노출

네이티브에서 관리 (및 뒤로)로 전환 하기 위해 비용을 지불 해야 합니다. 따라서 네이티브 개발자에 게 _번잡 api 대신 청크_ 를 노출 하는 것이 좋습니다 (예:).

**번잡**

```csharp
public class Person {
  public string FirstName { get; set; }
  public string LastName { get; set; }
}
```

```objc
// this requires 3 calls / transitions to initialize the instance
Person *p = [[Person alloc] init];
p.firstName = @"Sebastien";
p.lastName = @"Pouliot";
```

**청크**

```csharp
public class Person {
  public Person (string firstName, string lastName) {}
}
```

```objc
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

전환 횟수가 작을수록 성능이 향상 됩니다. 또한 더 적은 수의 코드를 생성 해야 하므로 더 작은 네이티브 라이브러리도 생성 됩니다.

## <a name="naming"></a>명명

이름을 지정 하는 작업은 컴퓨터 과학에서 가장 어려운 두 가지 문제, 다른 사용자가 캐시를 무효화 하는 문제 및 1 ~ 2 개 오류 중 하나입니다. .NET 포함을 통해 이름을 지정할 수 있지만 이름을 지정할 수 있습니다.

### <a name="types"></a>유형

목적-C는 네임 스페이스를 지원 하지 않습니다. 일반적으로 해당 형식에는 프레임 워크를 나타내는 UIKit의 보기에 대 한 `UIView` 같이 2 (Apple의 경우) 또는 3 (타사) 문자 접두사가 접두사로 붙습니다.

.NET 형식의 경우 중복 되거나 혼동 되는 이름을 도입할 수 있으므로 네임 스페이스를 건너뛸 수 없습니다. 이렇게 하면 기존 .NET 형식이 매우 깁니다. 예를 들면 다음과 같은 경우입니다.

```csharp
namespace Xamarin.Xml.Configuration {
  public class Reader {}
}
```

다음과 같이 사용 됩니다.

```objc
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

그러나 형식을 다음과 같이 다시 표시할 수 있습니다.

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

다음을 사용 하 여 더 쉽게 이해할 수 있습니다.

```objc
id reader = [[XAMXmlConfigReader alloc] init];
```

### <a name="methods"></a>메서드

좋은 .NET 이름도 목표-C API에 적합 하지 않을 수 있습니다.

목표에 대 한 명명 규칙은 .NET과 다릅니다 (파스칼식 대/소문자가 아닌 카멜식 대/소문자, 자세한 정보).
[Cocoa의 코딩 지침](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF)을 읽어 보세요.

목표-C 개발자 관점에서 `Get` 접두사가 있는 메서드는 인스턴스를 소유 하 고 있지 않음을 의미 합니다 (예: [get 규칙](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1)).

이 명명 규칙은 .NET GC 세계와 일치 하지 않습니다. `Create` 접두사가 있는 .NET 메서드는 .NET에서 동일 하 게 동작 합니다. 그러나 목표-C 개발자의 경우에는 일반적으로 반환 되는 인스턴스 (예: [create rule](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029))를 소유 하 고 있음을 의미 합니다.

## <a name="exceptions"></a>예외

.NET에서 예외를 광범위 하 게 사용 하 여 오류를 보고 하는 것이 일반적입니다. 그러나 속도가 느리고 목표에서 동일 하지는 않습니다. 가능 하면 항상 목표-C 개발자 로부터 숨겨야 합니다.

예를 들어, .NET `Try` 패턴은 목표-C 코드에서 훨씬 더 쉽게 사용할 수 있습니다.

```csharp
public int Parse (string number)
{
  return Int32.Parse (number);
}
```

위험과

```csharp
public bool TryParse (string number, out int value)
{
  return Int32.TryParse (number, out value);
}
```

### <a name="exceptions-inside-init"></a>`init*` 내의 예외

.NET에서 생성자는 성공 하 고 올바른_인스턴스를 반환_하거나 예외를 throw 해야 합니다.

이와 대조적으로, 목표는 인스턴스를 만들 수 없을 때 `init*` `nil`를 반환할 수 있도록 합니다. 이 패턴은 대부분의 Apple 프레임 워크에서 사용 되는 일반적인 패턴입니다. 다른 경우에는 `assert` 발생 하 고 현재 프로세스를 중단할 수 있습니다.

생성자는 생성 된 `init*` 메서드에 대해 동일한 `return nil` 패턴을 따릅니다. 관리 되는 예외가 throw 되 면 `NSLog`를 사용 하 여 출력 되 고 `nil` 호출자에 게 반환 됩니다.

## <a name="operators"></a>연산자

목적-C에서는 연산자가 처럼 C# 오버 로드 될 수 없으므로 클래스 선택기로 변환 됩니다.

["친숙 한"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) 명명 된 메서드는 발견 될 때 연산자 오버 로드에 대 한 우선 순위로 생성 되며 API를 더 쉽게 사용할 수 있습니다.

및/또는 `!=` `==` 연산자를 재정의 하는 클래스는 표준 Equals (Object) 메서드도 재정의 해야 합니다.
