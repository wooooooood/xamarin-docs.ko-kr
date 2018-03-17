---
title: "Objective C 지원"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 047f7d7497a114bf4b7c94e50bdf09862b882794
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/17/2018
---
# <a name="objective-c-support"></a>Objective C 지원

## <a name="specific-features"></a>특정 기능

ObjC 세대에 주목할 만한 있는 몇 가지, 특별 한 기능이 있습니다.

### <a name="automatic-reference-counting"></a>자동 참조 계산

자동 참조 계산 (호)의 사용은 **필요한** 를 생성 된 바인딩을 호출 합니다. 로 embeddinator 기반 라이브러리를 사용 하 여 프로젝트를 컴파일해야 `-fobjc-arc`합니다.

### <a name="nsstring-support"></a>NSString 지원

Api를 노출 하는 `System.String` 형식으로 변환 됩니다 `NSString`합니다. 이렇게 하면 메모리 관리 작업 보다 더 쉽게 `char*`합니다.

### <a name="protocols-support"></a>프로토콜 지원

관리 되는 인터페이스는 모든 멤버는 여기서 ObjC 프로토콜으로 변환 하는 `@required`합니다.

### <a name="nsobject-protocol-support"></a>NSObject 프로토콜 지원

기본적으로 기본 해시 가정 및.net 및 ObjC 런타임의 같음 요소가 세밀 하 게 하 고 사용이 가능을 공유 하는 의미 체계와 매우 유사 합니다.

관리 되는 형식이 재정의 하는 경우 `Equals(Object)` 또는 `GetHashCode` 일반적으로 (.NET) 정렬과 제대로 작동 하지 않을 가장 적합 한 없었음을 의미 합니다. 유추할 수 있는 기본 Objective-c 동작 하지 않는 것 중 하나입니다.

이런 경우에서 생성자 재정의 [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) 메서드 및 [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) 에 정의 된 속성의 [ `NSObject` 프로토콜](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)합니다. 따라서 사용자 지정 관리 되는 구현을 ObjC 코드에서 투명 하 게 사용할 수 있습니다.

### <a name="comparison"></a>비교

관리 되는 형식이 구현 하는 `IComparable` 제네릭 버전이 `IComparable<T>` ObjC 친숙 한 메서드를 반환 하는 생성 한 `NSComparisonResult` क र च는 `nil` 인수입니다. 이렇게 하면 생성된 된 API ObjC 개발자에 게 더 친숙 한 예:

```csharp
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

```csharp
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;
@end
```

단일 관리 하는 경우 Objective-c 범주가 여러 개 생성 됩니다 형식이 여러 종류를 확장 합니다.

### <a name="subscripting"></a>첨자

관리 되는 인덱싱된 속성은 개체 첨자도 변환 됩니다. 예를 들어:

```csharp
    public bool this[int index] {
        get { return c[index]; }
        set { c[index] = value; }
    }
```

만들 Objective-c 비슷합니다.

```csharp
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

Objective C 첨자 구문을 사용할 수 있는:

```csharp
    if ([intCollection [0] isEqual:@42])
        intCollection[0] = @13;
```

적절 한 인덱서 형식에 따라 인덱싱된 또는 키가 지정 된 첨자가 생성 됩니다.

이 [문서](http://nshipster.com/object-subscripting/) 첨자를 충분히 소개 합니다.

## <a name="main-differences-with-net"></a>.NET과 함께 주요 차이점

### <a name="constructors-vs-initializers"></a>생성자 및 이니셜라이저

Objective C에서 호출할 수 있습니다 이니셜라이저의 상속 체인에 부모 클래스의 프로토타입 사용할 수 없는 (NS_UNAVAILABLE)으로 표시 되어 있지 않으면입니다.

C#에서 명시적으로 선언 해야 클래스 내부 생성자 멤버, 즉, 생성자는 상속 되지 않습니다.

Objective C는 C# API의 오른쪽 표현에 노출 하기 위해 추가 `NS_UNAVAILABLE` 부모 클래스에서 자식 클래스에 존재 하지 않는 모든 이니셜라이저에 있습니다.

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

```objectivec
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

여기 수 있다는 것을 알 `initWithId:` 사용할 수 없는 것으로 표시 되어 있습니다.

### <a name="operator"></a>연산자

ObjC 연산자를 지원 하지 않는 연산자 클래스 선택기로 변환할지 하므로 마찬가지로 C#, 오버 로드 합니다.

```csharp
    public static AllOperators operator + (AllOperators c1, AllOperators c2)
    {
        return new AllOperators (c1.Value + c2.Value);
    }
```

다음으로 변경:

```csharp
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

그러나 일부.NET 언어 지원 하지 않습니다 연산자 오버 로드도 포함에 공통적으로 적용 되기 때문에 ["친숙 한"](https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx) 연산자 오버 로드 외에도 메서드 라는 합니다.

경우 연산자 버전과 "친숙 한" 버전을 모두 발견 되는 친숙 한 버전에만 생성 될 같은 objective c 이름으로 생성 됩니다.

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

```csharp
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>같음 연산자

= = 일반 연산자에서 C# 일반 연산자를 언급 하지 않는 위의으로 처리 됩니다.

그러나 "친숙 한" Equals 연산자 발견 되 면 두 연산자 = = 및 연산자! =는 세대에 건너뜁니다.

### <a name="datetime-vs-nsdate"></a>날짜/시간 vs NSDate

[NSDate의](https://developer.apple.com/reference/foundation/nsdate?language=objc) 설명서:

> NSDate 개체 시간, 모든 특정 calendrical 시스템 및 표준 시간대에 관계 없이 단일 시점을 캡슐화합니다. 날짜 개체를 절대 참조 날짜를 기준으로 고정 시간 간격을 나타내는 변경할 수 없는 (00: 00:00 UTC 2001 년 1 월 1).

로 인해 `NSDate` 날짜 간의 모든 변환을 참조 하 고 `DateTime` UTC에서 수행 되어야 합니다.

#### <a name="datetime-to-nsdate"></a>날짜/시간 NSDate

변환할 때 `DateTime` 를 `NSDate` DateTime의 `Kind` 속성 고려 됩니다.

|종류|결과                                                                                            |
|---|---|
|Utc|제공 된를 사용 하 여 변환이 수행 되어 `DateTime` 그대로 개체입니다.|
|로컬|호출 결과 `ToUniversalTime()` 을 제공 된 `DateTime` 개체 변환에 사용 됩니다.|
|지정되지 않음|제공 된 `DateTime` 개체 종류와 동일한 동작 UTC로 간주 됩니다 Utc = = 합니다.|

다음 수식을 사용 하 여 변환이 수행 됩니다.

> [!NOTE]
> **TimeInterval** = DateTimeObjectTicks - NSDateReferenceDateTicks[dt] / [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx)

NSDate의 사용은 TimeInterval 있으면 [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) 선택기를 만듭니다.

#### <a name="nsdate-to-datetime"></a>NSDate을 datetime으로 변환

NSDate에서 진행 되는 NSDate 가정 하는 날짜/시간 하려는 참조 날짜 인스턴스는 **2001 년 1 월 1 00시: 00 UTC** 다음 수식을 사용 하 고 있습니다.

> [!NOTE]
> **DateTimeTicks** = NSDateTimeIntervalSinceReferenceDate * [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx) + NSDateReferenceDateTicks[dt]

계산 되 면는 **DateTimeTicks** 사용 하 여 다음 DateTime [생성자](https://msdn.microsoft.com/en-us/library/w0d47c9c(v=vs.110).aspx) 설정을 해당 `kind` 를 `DateTimeKind.Utc`합니다.

일부 고려 사항을 알고 있어야 하는, NSDate 수 `nil` 하지만 DateTime는.net에서 구조체 및 정의 될 수 없습니다 `null`합니다. 제공 하는 경우는 `nil` NSDate 우리는 변환 것에 매핑되는 기본 날짜/시간 값으로 `DateTime.MinValue`합니다.

MinValue 및 MaxValue 서로, 하므로 크거나 더 낮은 값을 지정 하는 때마다 DateTime의으로 설정 됩니다 म NSDate DateTime의 보다 큰 및 하 한 경계를 지원할 수 [MaxValue](https://msdn.microsoft.com/en-us/library/system.datetime.maxvalue(v=vs.110).aspx) 또는 [MinValue](https://msdn.microsoft.com/en-us/library/system.datetime.minvalue(v=vs.110).aspx) 각각.

**dt**: NSDate 참조 날짜 **2001 년 1 월 1 00시: 00 UTC** => `new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;`
