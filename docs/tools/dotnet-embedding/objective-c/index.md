---
title: Objective C 지원
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 66ba31b1054559516fdbfbeb0421a82f4a0e9fa5
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
---
# <a name="objective-c-support"></a>Objective C 지원

## <a name="specific-features"></a>특정 기능

Objective C의 생성에 주목할 만한 있는 몇 가지 특수 한 기능이 있습니다.

### <a name="automatic-reference-counting"></a>자동 참조 계산

자동 참조 계산 (호)의 사용은 **필요한** 를 생성 된 바인딩을 호출 합니다. 로 embeddinator 기반 라이브러리를 사용 하 여 프로젝트를 컴파일해야 `-fobjc-arc`합니다.

### <a name="nsstring-support"></a>NSString 지원

Api를 노출 하는 `System.String` 형식으로 변환 됩니다 `NSString`합니다. 이 통해 메모리 관리 쉽게 처리할 때 보다 `char*`합니다.

### <a name="protocols-support"></a>프로토콜 지원

관리 되는 인터페이스는 모든 멤버는 여기서 Objective-c 프로토콜으로 변환 하는 `@required`합니다.

### <a name="nsobject-protocol-support"></a>NSObject 프로토콜 지원

기본적으로 기본 해시 및.NET 및 Objective-c 런타임의 같음 비교와 유사한 의미 체계를 공유 교환 가능한 것으로 간주 됩니다.

관리 되는 형식이 재정의 하는 경우 `Equals(Object)` 또는 `GetHashCode`, 기본적 (.NET) 충분 하지 않은 일반적으로 의미 이므로,이 기본 Objective-c 동작 가능성이 임을 의미 부족 함 중 하나입니다.

이러한 경우 생성자 재정의 [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) 메서드 및 [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) 에 정의 된 속성의 [ `NSObject` 프로토콜](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)합니다. 따라서 사용자 지정 관리 되는 구현을 Objective C 코드에서 투명 하 게 사용할 수 있습니다.

### <a name="exceptions-support"></a>예외 지원

전달 `--nativeexception` 인수로 `objcgen` Objective C 예외를 발견 하 고 처리할 수로 관리 되는 예외를 변환 합니다. 

### <a name="comparison"></a>비교

관리 되는 형식이 구현 하는 `IComparable` (또는 해당 제네릭 버전 `IComparable<T>`)를 반환 하는 Objective-c 친숙 한 메서드를 생성 합니다는 `NSComparisonResult` क र च는 `nil` 인수입니다. 따라서 생성된 된 API Objective-c 개발자에 게 더 친숙 한 합니다. 예를 들어:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>범주

관리 확장 메서드를 범주별으로 변환 됩니다. 예를 들어 확장의 다음 메서드를 `Collection`:

```csharp
public static class SomeExtensions {
    public static int CountNonNull (this Collection collection) { ... }
    public static int CountNull (this Collection collection) { ... }
}
```

이와 같은 경우 Objective-c 범주를 만들 있습니다.

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

여러 가지 형식을 확장 하는 관리 되는 단일 형식, 여러 Objective-c 범주 생성 됩니다.

### <a name="subscripting"></a>첨자

관리 되는 인덱싱된 속성은 개체 첨자도 변환 됩니다. 예를 들어:

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

만들 Objective-c 비슷합니다.

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

Objective C 첨자 구문을 사용할 수 있는:

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

적절 한 인덱서 형식에 따라 인덱싱된 또는 키가 지정 된 첨자가 생성 됩니다.

이 [문서](http://nshipster.com/object-subscripting/) 첨자를 충분히 소개 합니다.

## <a name="main-differences-with-net"></a>.NET과 함께 주요 차이점

### <a name="constructors-vs-initializers"></a>생성자 및 이니셜라이저

Objective C에서 호출할 수 있습니다 이니셜라이저의 상속 체인에 부모 클래스의 프로토타입 사용할 수 없는 상태로 표시 되어 있지 않으면 (`NS_UNAVAILABLE`).

C#의 생성자는 상속 되지 즉 클래스, 생성자 멤버를 명시적으로 선언 해야 합니다.

오른쪽 표현의 Objective C는 C# API를 노출 하려면 `NS_UNAVAILABLE` 부모 클래스에서 자식 클래스에 존재 하지 않는 모든 이니셜라이저에 추가 됩니다.

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

여기서 `initWithId:` 사용할 수 없는 것으로 표시 되어 있습니다.

### <a name="operator"></a>연산자

Objective C 연산자를 지원 하지 않는 연산자 클래스 선택기로 변환할지 하므로 마찬가지로 C#, 오버 로드 합니다.

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

그러나 일부.NET 언어 지원 하지 않습니다 연산자 오버 로드도 포함에 공통적으로 적용 되기 때문에 ["친숙 한"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) 연산자 오버 로드 외에도 메서드 라는 합니다.

경우 연산자 버전과 "친숙 한" 버전을 모두 발견 되는 친숙 한 버전에만 생성 됩니다 같은 Objective-c 이름으로 생성 됩니다.

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

다음과 같이 됩니다.

```objc
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>같음 연산자

일반 연산자에서 `==` C#로 처리 되는 일반 연산자 위에 설명 된 대로 합니다.

그러나 "친숙 한" Equals 연산자 발견 되 면 두 연산자 `==` and 연산자 `!=` 세대에 건너뜁니다.

### <a name="datetime-vs-nsdate"></a>날짜/시간 vs NSDate

[ `NSDate` ](https://developer.apple.com/reference/foundation/nsdate?language=objc) 설명서:

> `NSDate` 개체는 특정 calendrical 시스템 및 표준 시간대에 관계 없이 시간 내에 단일 시점을 캡슐화합니다. 날짜 개체를 절대 참조 날짜를 기준으로 고정 시간 간격을 나타내는 변경할 수 없는 (00: 00:00 UTC 2001 년 1 월 1).

로 인해 `NSDate` 날짜 간의 모든 변환을 참조 하 고 `DateTime` UTC에서 수행 되어야 합니다.

#### <a name="datetime-to-nsdate"></a>날짜/시간 NSDate

변환할 때 `DateTime` 를 `NSDate`, `Kind` 속성을 `DateTime` 고려 됩니다.

|종류|결과|
|---|---|
|`Utc`|제공 된를 사용 하 여 변환이 수행 되어 `DateTime` 그대로 개체입니다.|
|`Local`|호출 결과 `ToUniversalTime()` 을 제공 된 `DateTime` 개체 변환에 사용 됩니다.|
|`Unspecified`|제공 된 `DateTime` 개체 UTC, 동일한 동작으로 간주 됩니다 때 `Kind` 은 `Utc`합니다.|

변환에는 다음 수식을 사용 합니다.

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

이 수식: 

- `NSDateReferenceDateTicks` 기준으로 계산 되는 `NSDate` 2001 년 1 월 1 00시: 00 UTC의 날짜를 참조 합니다. 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) 에 정의 되어 [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

만들려는 `NSDate` 개체는 `TimeInterval` 와 함께 사용 되는 `NSDate` [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) 선택기입니다.

#### <a name="nsdate-to-datetime"></a>NSDate을 datetime으로 변환

변환 `NSDate` 를 `DateTime` 는 다음 수식을 사용 합니다.

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

이 수식: 

- `NSDateReferenceDateTicks` 기준으로 계산 되는 `NSDate` 2001 년 1 월 1 00시: 00 UTC의 날짜를 참조 합니다. 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) 에 정의 되어 [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

계산한 후 `DateTimeTicks`, `DateTime` [생성자](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_) 설정 호출 되는 `kind` 를 `DateTimeKind.Utc`합니다.

> [!NOTE]
> `NSDate` 수 `nil`, 하지만 `DateTime` 는 정의 될 수 없는.net 구조체 `null`합니다. 제공 하는 경우는 `nil` `NSDate`, 기본 변환 됩니다 `DateTime` 값에 매핑되는 `DateTime.MinValue`합니다.

`NSDate` 높은 최대와 보다 작은 최소 값 지원 `DateTime`합니다. 변환할 때 `NSDate` 를 `DateTime`, 이러한 상위 수준과 하위 값으로 변경 되는 `DateTime` [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) 또는 [MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue)각각 합니다.
