---
title: Objective C 지원
description: 이 문서에서는 Objective-c.NET 포함에 대 한 지원에 대 한 설명을 제공 합니다. 자동 참조 계산, NSString, 프로토콜, NSObject 프로토콜, 예외 및 자세히 설명합니다.
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 48caa70cf2bd408f8afc673b400f7d5a4369e108
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61230850"
---
# <a name="objective-c-support"></a>Objective C 지원

## <a name="specific-features"></a>특정 기능

Objective-c로 생성에 주목할 만한 몇 가지 특별 한 기능이 있습니다.

### <a name="automatic-reference-counting"></a>자동 참조 계산

자동 참조 계산 (호)의 사용은 **필요한** 생성 된 바인딩 호출 합니다. 포함 하는.NET 기반 라이브러리를 사용 하 여 프로젝트를 사용 하 여 컴파일해야 합니다. `-fobjc-arc`합니다.

### <a name="nsstring-support"></a>NSString 지원

Api를 노출 하는 `System.String` 형식으로 변환 됩니다 `NSString`합니다. 이렇게 하면 메모리 관리 처리할 때 보다 쉽게 `char*`합니다.

### <a name="protocols-support"></a>프로토콜 지원

관리 되는 인터페이스는 모든 멤버는 Objective-c 프로토콜 변환 된 `@required`합니다.

### <a name="nsobject-protocol-support"></a>NSObject 프로토콜 지원

기본적으로 기본 해시 및.NET과 Objective-c 런타임에서의 같음 의미 체계가 유사한 공유 서로 호환 되도록 간주 됩니다.

관리 되는 형식을 재정의 하는 경우 `Equals(Object)` 또는 `GetHashCode`, 일반적으로 기본 (.NET) 동작이 충분 하지 않은 의미, 즉, 기본 Objective-c 동작 가능성이 있는지 부족 중 하나입니다.

생성자 재정의 이러한 경우에는 [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) 메서드 및 [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) 에 정의 된 속성을 [ `NSObject` 프로토콜](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)합니다. 이 투명 하 게 Objective-c 코드에서 사용할 사용자 지정 관리 되는 구현을 허용 합니다.

### <a name="exceptions-support"></a>예외 지원

전달 `--nativeexception` 인수로 `objcgen` Objective-c 예외 포착 하 고 처리할 수 있는로 관리 되는 예외를 변환 합니다. 

### <a name="comparison"></a>비교

관리 되는 구현 하는 형식 `IComparable` (또는 해당 제네릭 버전 `IComparable<T>`)를 반환 하는 친숙 한 메서드를 Objective-c로 생성 됩니다는 `NSComparisonResult` 받고는 `nil` 인수. 이렇게 하면 생성된 된 API는 Objective-c 개발자에 게 더 친숙 한 합니다. 예를 들어:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>범주

관리 확장 메서드를 범주별으로 변환 됩니다. 예를 들어, 확장의 다음 메서드는 `Collection`:

```csharp
public static class SomeExtensions {
    public static int CountNonNull (this Collection collection) { ... }
    public static int CountNull (this Collection collection) { ... }
}
```

이와 같은 Objective-c로 범주를 만듭니다.

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

관리 되는 단일 형식에는 여러 형식을 확장, Objective-c로 범주를 여러 개 생성 됩니다.

### <a name="subscripting"></a>첨자

관리 되는 인덱싱된 속성은 개체 첨자가으로 변환 됩니다. 예를 들어:

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

만듭니다 Objective-c와 비슷합니다.

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

Objective-c 첨자 구문을 통해 사용할 수 있습니다.

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

인덱서의 유형에 따라 적절 한 인덱스 또는 키 첨자가 생성 됩니다.

이렇게 [문서](http://nshipster.com/object-subscripting/) 첨자에 대 한 훌륭한 소개 됩니다.

## <a name="main-differences-with-net"></a>.NET을 사용 하 여 주요 차이점

### <a name="constructors-vs-initializers"></a>생성자 및 이니셜라이저

Objective-c에서 호출할 수 있습니다 이니셜라이저의 프로토타입 상속 체인의 부모 클래스의 모든 사용할 수 없는 상태로 표시 되어 있지 않으면 (`NS_UNAVAILABLE`).

C# 생성자는 상속 되지 즉 클래스, 생성자 멤버를 명시적으로 선언 해야 합니다.

오른쪽 표현의 노출 하는 C# Objective C API `NS_UNAVAILABLE` 부모 클래스에서 자식 클래스에 존재 하지 않는 모든 이니셜라이저에 추가 됩니다.

C# API:

```csharp
public class Unique {
    public Unique () : this (1)
    {
    }

    public Unique (int id)
    {
    }
}

public class SuperUnique : Unique {
    public SuperUnique () : base (911)
    {
    }
}
```

Objective C API를 표시 합니다.

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

이때 `initWithId:` 불가능으로 표시 되었습니다.

### <a name="operator"></a>연산자

Objective C 연산자를 오버 로드를 지원 하지 않습니다 C# 는 연산자 클래스 선택기에 변환 됩니다.

```csharp
public static AllOperators operator + (AllOperators c1, AllOperators c2)
{
    return new AllOperators (c1.Value + c2.Value);
}
```

다음으로 변경:

```objc
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

그러나 일부.NET 언어 연산자 지원 하지 않는 오버 로드도 포함에 공통적으로 적용 되므로 ["친숙 한"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) 명명 된 메서드는 연산자 오버 로드 하는 것 외에도 합니다.

경우 연산자 버전과 "친숙 한" 버전, 친숙 한 버전에만 생성 될 때 발견 동일한 Objective-c 이름으로 생성 됩니다.

```csharp
public static AllOperatorsWithFriendly operator + (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
{
    return new AllOperatorsWithFriendly (c1.Value + c2.Value);
}

public static AllOperatorsWithFriendly Add (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
{
    return new AllOperatorsWithFriendly (c1.Value + c2.Value);
}
```

다음과 같이 사용하십시오.

```objc
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>같음 연산자

일반 연산자에서 `==` 에서 C# 일반 연산자를 설명한 것 처럼 위의로 처리 됩니다.

그러나 "친숙 한" Equals 연산자 발견 되 면 두 연산자 `==` and 연산자 `!=` 세대에서 건너뜁니다.

### <a name="datetime-vs-nsdate"></a>DateTime vs NSDate

[ `NSDate` ](https://developer.apple.com/reference/foundation/nsdate?language=objc) 설명서:

> `NSDate` 개체는 시간이 나 특정 calendrical 시스템 표준 시간대와 무관 단일 시점을 캡슐화합니다. 날짜 개체를 절대 참조 날짜를 기준으로 고정 시간 간격을 나타내는 변경할 수 없는 (00: 00:00 UTC 2001 년 1 월 1).

로 인해 `NSDate` 해당 간의 모든 변환 날짜를 참조 하 고 `DateTime` UTC에서 수행 해야 합니다.

#### <a name="datetime-to-nsdate"></a>날짜/시간 NSDate

변환할 때는 `DateTime` 를 `NSDate`, `Kind` 속성을 `DateTime` 고려 됩니다.

|종류|결과|
|---|---|
|`Utc`|제공 된를 사용 하 여 변환할 `DateTime` 는 개체입니다.|
|`Local`|호출의 결과 `ToUniversalTime()` 제공 된 `DateTime` 개체 변환에 사용 됩니다.|
|`Unspecified`|제공 된 `DateTime` 개체는 동일한 동작 UTC로 간주 됩니다 때 `Kind` 는 `Utc`합니다.|

변환에는 다음 수식을 사용합니다.

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

이 수식: 

- `NSDateReferenceDateTicks` 기준으로 계산 됩니다는 `NSDate` 2001 년 1 월 1 00시: 00 UTC의 날짜를 참조 합니다. 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) 에 정의 된 [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

만들려는 합니다 `NSDate` 개체를 `TimeInterval` 사용 됩니다는 `NSDate` [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) 선택기.

#### <a name="nsdate-to-datetime"></a>날짜/시간으로 NSDate

변환 `NSDate` 에 `DateTime` 다음 수식을 사용 합니다.

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

이 수식: 

- `NSDateReferenceDateTicks` 기준으로 계산 됩니다는 `NSDate` 2001 년 1 월 1 00시: 00 UTC의 날짜를 참조 합니다. 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) 에 정의 된 [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

계산한 후 `DateTimeTicks`는 `DateTime` [생성자](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_) 가 호출 설정 해당 `kind` 에 `DateTimeKind.Utc`입니다.

> [!NOTE]
> `NSDate` 일 수 있습니다 `nil`, 하지만 `DateTime` 는.net에서 정의 될 수 없는 구조체 `null`합니다. 제공 하는 경우는 `nil` `NSDate`, 기본 변환 됩니다 `DateTime` 값에 매핑되는 `DateTime.MinValue`합니다.

`NSDate` 더 높은 최대값 및 최소값 보다 낮은 지원 `DateTime`합니다. 변환할 때 `NSDate` 에 `DateTime`, 이러한 높고 낮은 값으로 변경 됩니다 합니다 `DateTime` [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) 또는 [MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue)각각.
