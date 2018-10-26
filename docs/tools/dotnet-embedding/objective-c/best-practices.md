---
title: Objective-c 용 모범 사례.NET 포함
description: 이 문서의 목표 3.를 사용 하 여.NET 포함을 사용 하기 위한 다양 한 모범 사례를 설명 합니다. 관리 코드의 하위 집합을 노출, chunkier API 노출, 명명 및 자세히 설명 합니다.
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 33138b7858b8bc04a5be30f9fad1709e916f5575
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105404"
---
# <a name="net-embedding-best-practices-for-objective-c"></a>Objective-c 용 모범 사례.NET 포함

초안 이며 지원 되지 않는 기능을 사용 하 여 동기화에서 현재 도구에서. 이 문서를 발전 시키기 결국 최종 도구에 맞게 일치, 즉 제안 장기 가장 좋은 방법을-하지 즉시 해결 되기를 바랍니다.

이 문서의 많은 부분 지원 되는 다른 언어에도 적용 됩니다. 그러나 모든 제공 된 예제는 C# 및 보여주기 위한 것입니다.

## <a name="exposing-a-subset-of-the-managed-code"></a>관리 코드의 하위 집합을 노출합니다.

생성 된 네이티브 라이브러리/프레임 워크는 각 노출 되는 관리 되는 Api를 호출 하는 Objective-c 코드를 포함 합니다. 노출 하는 자세한 API (공용 확인) 다음 더 큰 네이티브 _붙이기_ 라이브러리 됩니다.

좋습니다 네이티브 개발자에 게 필요한 Api만을 노출 하는 다른 수의 작은 어셈블리를 만들 수 있습니다. 해당 외관도 하면 표시 유형, 이름 지정, 생성된 된 코드의... 검사 오류를 더 많이 제어할 수 있습니다.

## <a name="exposing-a-chunkier-api"></a>Chunkier API를 노출합니다.

에 관리 되는 (및 백)에 네이티브 코드에서 전환 비용을 지불을 가격입니다. 따라서 것이 좋습니다 노출할 _번잡 한 대신 대규모_ 네이티브 개발자에 게 Api 예:

**수 다 스러운**

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

**Chunky**

```csharp
public class Person {
  public Person (string firstName, string lastName) {}
}
```

```objc
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

전환 횟수 작은 이므로 성능이 향상 됩니다. 또한 네이티브 작은 라이브러리에도이 생성 되므로 더 적은 코드를 생성 하도록 필요 합니다.

## <a name="naming"></a>명명

이름 지정에서 다른 캐시 무효화 및 오프-에서-1 오류 전산학 석사 학위를 두 가지 어려운 문제 중 하나입니다. 다행히.NET 포함를 보호할 수 있습니다를 제외한 모든 명명 합니다.

### <a name="types"></a>유형

Objective-c는 네임 스페이스를 지원 하지 않습니다. 일반적으로 해당 형식 붙습니다 2 (Apple)에 대 한 3 (타사)에 대 한 접두사를 같은 문자 또는 `UIView` UIKit의 뷰에 대 한 프레임 워크를 나타내는입니다.

.NET 형식에 대 한 네임 스페이스를 건너뛰는 중 아닙니다 가능한 중복 되었거나 혼동 이름 발생할 수 있습니다. 예를 들어 이렇게 하면 매우 긴 기존.NET 형식

```csharp
namespace Xamarin.Xml.Configuration {
  public class Reader {}
}
```

것 처럼 사용할 수 있습니다.

```objc
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

그러나 형식으로 다시 노출할 수 있습니다.

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

Objective-c의 좀 더 친숙 한 하므로 예를 사용 하려면:

```objc
id reader = [[XAMXmlConfigReader alloc] init];
```

### <a name="methods"></a>메서드

양호한.NET 이름은 Objective C API에 적합 하지 않을 수도 있습니다.

Objective C에서 명명 규칙 (더 자세한 파스칼식 대 / 소문자 대신 카멜식 대/소문자).NET과 다릅니다.
읽어보세요 합니다 [Cocoa에 대 한 지침을 코딩](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF)합니다.

Objective-c 개발자의 관점에서를 사용 하 여 메서드를 `Get` 접두사 인스턴스, 즉, 소유 하지 않은 것을 의미 합니다 [규칙 가져오기](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1).

.NET GC 전 세계에 일치 하는 항목에이 명명 규칙 사용 하 여.NET 메서드는 `Create` 접두사.NET에서 동일 하 게 동작 합니다. 그러나 Objective-c 개발자를 위해 일반적으로 의미 즉, 반환 된 인스턴스를 소유 하는 [규칙을 만들](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029)합니다.

## <a name="exceptions"></a>예외

오류 보고를 광범위 하 게 예외를 사용 하는.NET에서 매우 일반적 이며 그러나 느리고 목표 C에서 매우 동일 하지 않습니다. 가능 하면 Objective-c 개발자 로부터 숨길 해야 있습니다.

예를 들어.NET `Try` 패턴은 Objective-c 코드에서 사용 하기 쉽게 확인할 수 있습니다.

```csharp
public int Parse (string number)
{
  return Int32.Parse (number);
}
```

비교

```csharp
public bool TryParse (string number, out int value)
{
  return Int32.TryParse (number, out value);
}
```

### <a name="exceptions-inside-init"></a>내부 예외 `init*`

.NET의 생성자 중 하나는 성공 해야 하며 반환를 (_바랍니다_) 유효한 인스턴스 또는 예외를 throw 합니다.

Objective-c로 허용 되는 반면 `init*` 반환할 `nil` 인스턴스를 만들 수 없습니다. 다양 한 Apple 프레임 워크에 사용 되는 일반적 이지만 하지 일반적인 패턴입니다. 일부 다른 경우에는 `assert` 수 발생과 현재 프로세스를 종료 합니다.

동일한 생성기 수행 `return nil` 생성에 대 한 패턴 `init*` 메서드. 관리 되는 예외가 throw 된 경우 인쇄 됩니다 (사용 하 여 `NSLog`) 및 `nil` 호출자에 게 반환 됩니다.

## <a name="operators"></a>연산자

Objective C 연산자를 오버 로드를 허용 하지 않습니다 C# 는 이러한 클래스 선택기에 변환 됩니다.

["친숙 한"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) 명명 된 메서드는 연산자 오버 로드 보다 우선적으로 생성 된 때 발견 하 고 API를 사용 하는 보다 쉽게 생성할 수 있습니다.

연산자를 재정의 하는 클래스 `==` 및/또는 `!=` 표준 Equals (개체) 메서드를 재정의 해야 합니다.
