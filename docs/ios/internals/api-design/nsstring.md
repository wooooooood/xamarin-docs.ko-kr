---
title: Xamarin.iOS 및 Xamarin.Mac의 NSString
description: 이 문서에서는 발생하지 않은 경우 Xamarin.iOS가 NSString 개체를 C# 문자열 개체로 투명하게 변환하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: f744f4ed5619e4e7f4a9d85897c4451bf7e5b9bc
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73022356"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>Xamarin.iOS 및 Xamarin.Mac의 NSString

Xamarin.iOS 및 Xamarin.Mac의 디자인은 C# 및 기타 .NET 프로그래밍 언어에서 문자열 조작을 위해 네이티브 .NET 문자열 유형 `string`을 노출하고  `NSString`  데이터 형식 대신 API에서 노출하는 데이터 형식으로 문자열을 노출합니다.

즉, 개발자는 특정 형식(`Foundation.NSString`)에서 Xamarin.iOS 및 Xamarin.Mac API(통합)를 호출하는 데 사용되는 문자열을 유지할 필요가 없으며, 모든 작업에 Mono의 `System.String`을 계속 사용할 수 있으며, Xamarin.iOS 또는 Xamarin.Mac의 API에 문자열이 필요할 때마다 API 바인딩이 정보 마샬링을 처리합니다.

예를 들어 `NSString` 유형의 `UILabel`에서 Objective-C "text" 속성은 다음과 같이 선언됩니다.

```objc
@property(nonatomic, copy) NSString *text
```

이는 Xamarin.iOS에서 다음과 같이 노출됩니다.

```csharp
class UILabel {
    public string Text { get; set; }
}
```

내부적으로 이 속성의 구현은 C# 문자열을 `NSString`으로 마샬링하고 Objective-C와 동일한 방식으로 `objc_msgSend` 메서드를 호출합니다.

`NSString`을 사용하지 않고 대신 C 문자열("*char*")을 사용하는 소수의 타사 Objective-C API가 있습니다. 이러한 경우 C# 문자열 데이터 형식을 계속 사용할 수 있지만 [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) 특성을 사용하여 이 문자열을 `NSString`으로 마샬링하지 말고 C 문자열로 마샬링해야 함을 바인딩 생성기에 알려야 합니다.

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>규칙에 대한 예외

Xamarin.iOS 및 Xamarin.Mac 둘 다에서 이 규칙에 대한 예외가 발생했습니다.  `string`을 노출할 때와  `NSString`을 제외하고 노출할 때 간의 결정은  `NSString`  메서드가 콘텐츠 비교 대신 포인터 비교를 수행할 수 있는 경우에 이루어집니다.

이는 Objective-C API가 문자열의 실제 내용을 비교하는 대신 일부 동작을 나타내는 토큰으로 public `NSString` 상수를 사용하는 경우에 발생할 수 있습니다.

이러한 경우 `NSString` API가 노출되며 이를 포함하는 소수의 API가 있습니다. 또한 일부 클래스에서 NSString 속성이 노출되는 것을 알 수 있습니다. 이러한 `NSString` 속성은 알림과 같은 항목에 대해 노출됩니다. 이러한 속성은 일반적으로 다음과 같습니다.

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

알림은 런타임에 의해 브로드캐스팅하는 특정 이벤트에 등록하려는 경우 `NSNotification` 클래스에 사용되는 키입니다.

키는 일반적으로 다음과 같이 표시됩니다.

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

`NSString`이 API에 노출되는 또 다른 위치는 `NSDictionary` 개체를 매개 변수로 사용하는 iOS 또는 OS X의 특정 API에 매개 변수로 사용되는 토큰입니다. 사전에는 일반적으로 `NSString` 키가 포함되어 있습니다. Xamarin.iOS는 규칙에 따라 "키" 이름을 추가하여 이러한 정적 `NSString` 속성의 이름을 지정합니다.
