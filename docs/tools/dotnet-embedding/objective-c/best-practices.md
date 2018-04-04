---
title: Embeddinator 4000 ObjC에 대 한 모범 사례
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 93dd98dcff772adceb3650ec327cc1a14e4e056b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="embeddinator-4000-best-practices-for-objc"></a>Embeddinator 4000 ObjC에 대 한 모범 사례

초안 이며 지원 되지 않는 기능을 통해 동기화 현재 도구에서. 이 문서는 개별적으로 발전 하 고 결국 최종 도구 일치, 장기 가장 좋은 방법을 하지 즉시 해결 방법을 제안 합니다 즉, 하시기 바랍니다.

이 문서는 대부분 다른 지원 되는 언어에도 적용 됩니다. 그러나 제공 된 모든 예는 C# 및 목표 없습니다.


# <a name="exposing-a-subset-of-the-managed-code"></a>관리 코드의 하위 집합을 노출합니다.

생성 된 네이티브 라이브러리/프레임 워크 Objective-c 각 노출 되는 관리 되는 Api를 호출 하는 코드를 포함 합니다. 노출 하는 더 많은 API (공개로 설정) 다음 큰 네이티브 _붙이기_ 라이브러리 됩니다.

네이티브 개발자에 게 필요한 api만을 서로 다른 수의 작은 어셈블리를 만드는 것이 좋습니다 수도 있습니다. 해당 외관 또한 하면 표시 유형, 이름 지정, 오류 생성된 된 코드의... 검사를 더 많이 제어할 수 있습니다.


# <a name="exposing-a-chunkier-api"></a>Chunkier API 노출

네이티브 코드에서 전환 비용을 지불 하려면 가격에 관리 되는 (뒤로). 노출 하는 것이 좋습니다 따라서 _수 다 스러운 대신 대규모_ 네이티브 개발자에 게 Api 예:

**수 다 스러운**
```
public class Person {
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

```csharp
// this requires 3 calls / transitions to initialize the instance
Person *p = [[Person alloc] init];
p.firstName = @"Sebastien";
p.lastName = @"Pouliot";
```

**Chunky**
```
public class Person {
    public Person (string firstName, string lastName) {}
}
```

```csharp
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

전환의 수를 더 작은 이므로 성능이 향상 됩니다. 더 적은 코드를 생성 하도록도 필요 하므로 이러한 네이티브 작은 라이브러리에도 생성 됩니다 합니다.


# <a name="naming"></a>명명

이름 지정에서 다른 캐시 무효화 및 오프-여-1 오류 컴퓨터 분야에서 가장 어려운 두 가지 문제 중 하나입니다. 다행 스럽게도 포함 하는.NET 수을 보호 하면에서 제외한 모든 명명 합니다.

## <a name="types"></a>유형

Objective C 네임 스페이스를 지원 하지 않습니다. 일반적으로 해당 형식의 접두사가 추가 되며 2 (Apple)에 대 한 3 (타사)에 대 한 접두사를 원하는 문자 또는 `UIView` 프레임 워크 임을 나타내는 UIKit의 보기에 대 한 합니다.

.NET 형식에 대 한 네임 스페이스를 건너뛰는 중 불가능으로 중복 또는 혼란 스러운, 이름 발생할 수 있습니다. 예를 들어 이렇게 하면 매우 긴 기존.NET 형식

```csharp
namespace Xamarin.Xml.Configuration {
    public class Reader {}
}
```

것 처럼 사용할 수 있습니다.

```csharp
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

그러나 형식을로 다시 노출할 수 있습니다.

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

더 많은 Objective-c 활용이 편한을 만드는 예를 들어을 사용 하려면:

```csharp
id reader = [[XAMXmlConfigReader alloc] init];
```

## <a name="methods"></a>메서드

양호한.NET 이름을 Objective-c API에 대 한 이상적인 아닐 수 있습니다.

Objective C의 명명 규칙은.NET (더 자세한 파스칼식 대 / 소문자로 대신 카멜식 대/소문자) 다릅니다.
읽으십시오는 [코딩 Cocoa에 대 한 지침](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF)합니다.

Objective C 개발자의 관점에서를 사용 하 여 메서드는 `Get` 접두사 인스턴스, 즉, 소유 하지 않은 의미는 [규칙 가져오기](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1)합니다.

.NET GC 세계;에 일치 항목이 없는이 명명 규칙 .NET 메서드를 한 `Create` 접두사.NET에서 동일 하 게 동작 합니다. 그러나 Objective-c 개발자를 위해 일반적으로 의미 즉, 반환 된 인스턴스를 소유 하는 [규칙을 만들](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029)합니다.

# <a name="exceptions"></a>예외

광범위 하 게 오류를 보고 하는 예외를 사용 하도록.net에서 quite commont 이며 그러나 이들 프로토콜은 느리고 ObjC에서 매우 동일 하지는 않습니다. 가능 하면 항상 Objective-c 개발자 로부터 숨겨야 합니다.

예를 들어.NET `Try` 패턴은 훨씬 쉽게 Objective C 코드에서 사용할 수 없습니다.

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

## <a name="exceptions-inside-init"></a>내부 예외 `init*`

.NET의 생성자 중 하나는 성공 해야 하며 반환는 (_다행 스럽게도_) 올바른 인스턴스 또는 예외를 throw 합니다.

Objective C 허용 하는 반면, `init*` 반환할 `nil` 인스턴스를 만들 수 없습니다. 대부분의 Apple의 프레임 워크에서 사용 되는 일반적인 하지만 하지 일반적인 패턴입니다. 다른 경우에는 `assert` 수 발생할 (및 해당 현재 프로세스를 중지) 합니다.

생성자에 따라 동일한 `return nil` 생성에 대 한 패턴 `init*` 메서드. 관리 되는 예외가 throw 되 면 표시 됩니다 (사용 하 여 `NSLog`) 및 `nil` 호출자에 게 반환 됩니다.

## <a name="operators"></a>연산자

ObjC 연산자 오버 로드 될 마찬가지로 C#, 되므로 변환 되 면 클래스 선택기를 허용 하지 않습니다.

["친숙 한"](https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx) 명명된 메서드 연산자 오버 로드 대신 생성 된 때 발견 하 고 API를 사용 하는 보다 쉽게 생성할 수 있습니다.

연산자를 재정의 하는 클래스 및/또는 = =! = 표준 Equals (Object) 메서드를 재정의 해야 합니다.
