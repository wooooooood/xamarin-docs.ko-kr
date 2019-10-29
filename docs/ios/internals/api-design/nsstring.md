---
title: Xamarin.ios 및 Xamarin.ios의 NSString
description: 이 문서에서는 Xamarin.ios가 발생 하지 않는 경우 Xamarin.ios가 NSString C# 개체를 문자열 개체로 투명 하 게 변환 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: f744f4ed5619e4e7f4a9d85897c4451bf7e5b9bc
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022356"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>Xamarin.ios 및 Xamarin.ios의 NSString

Xamarin.ios 및 Xamarin.ios의 디자인은 사용 API에 대 한를 호출 하 여 네이티브 .NET 문자열 형식, `string`, C# 및 기타 .net 프로그래밍 언어의 문자열 조작 및를 사용 하는 대신 api에서 노출 하는 데이터 형식으로 문자열을 노출 합니다. `NSString` 데이터 형식입니다.

즉, 개발자는 특정 형식 (`Foundation.NSString`)에서 xamarin.ios & Xamarin.ios API (통합)를 호출 하는 데 사용 되는 문자열을 유지할 필요가 없으며, 모든 작업에 대해 Mono의 `System.String`을 계속 사용할 수 있으며, 언제 든 지 Xamarin.ios 또는 Xamarin.ios에는 문자열이 필요 하며, API 바인딩에서 정보 마샬링을 처리 합니다.

예를 들어 `NSString`형식 `UILabel`의 목표-C "텍스트" 속성은 다음과 같이 선언 됩니다.

```objc
@property(nonatomic, copy) NSString *text
```

이는 Xamarin.ios에서 다음과 같이 노출 됩니다.

```csharp
class UILabel {
    public string Text { get; set; }
}
```

내부적으로이 속성의 구현은 문자열을 C#`NSString`로 마샬링하고 목표와 같은 방식으로`objc_msgSend`메서드를 호출 합니다.

`NSString`사용 하지 않고 C 문자열 ("*char*")을 사용 하는 제 3 자 목표-C api가 있습니다. 이러한 경우에도 C# 문자열 데이터 형식을 사용할 수 있지만, [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) 특성을 사용 하 여 바인딩 생성기에이 문자열을`NSString`로 마샬링할 수 없고 그 대신 C 문자열로 매핑해야 함을 알려야 합니다.

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>규칙에 대 한 예외

Xamarin.ios 및 Xamarin.ios에서이 규칙에 대 한 예외가 발생 했습니다.  `string`s를 노출할 때를 결정 하는 것과 `NSString`를 제외 하 고 노출 하는 경우 `NSString` 메서드가 콘텐츠 비교 대신 포인터 비교를 수행할 수 있는 경우가 만들어집니다.

이는 목표 C Api가 문자열의 실제 내용을 비교 하는 대신 일부 동작을 나타내는 토큰으로 public `NSString` 상수를 사용 하는 경우에 발생할 수 있습니다.

이러한 경우 `NSString` Api가 노출 되며이를 포함 하는 소수의 Api가 있습니다. 또한 일부 클래스에서 NSString 속성이 노출 되는 것을 알 수 있습니다. 이러한 `NSString` 속성은 알림과 같은 항목에 대해 노출 됩니다. 이러한 속성은 일반적으로 다음과 같습니다.

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

알림은 런타임에 의해 브로드캐스팅하는 특정 이벤트에 등록 하려는 경우 `NSNotification` 클래스에 사용 되는 키입니다.

키는 일반적으로 다음과 같이 표시 됩니다.

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

`NSString`s가 API에 노출 되는 또 다른 위치는 `NSDictionary` 개체를 매개 변수로 사용 하는 iOS 또는 OS X의 특정 Api에 대 한 매개 변수로 사용 되는 토큰입니다. 사전에는 일반적으로 `NSString` 키가 포함 되어 있습니다. Xamarin.ios는 규칙에 따라 "키" 이름을 추가 하 여 이러한 정적 `NSString` 속성의 이름을로 입력 합니다.
