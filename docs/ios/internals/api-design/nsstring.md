---
title: Xamarin.iOS 및 Xamarin.Mac NSString
description: 이 문서에서는 Xamarin.iOS NSString 개체를 투명 하 게 변환 하는 방법을 설명 합니다. C# 이 발생 하지 않습니다 하는 경우 개체를 문자열입니다.
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: cc9e3f992642f3cdc3d16fe6f829b6a6c06b50fc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61277816"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>Xamarin.iOS 및 Xamarin.Mac NSString

Xamarin.iOS 및 Xamarin.Mac 디자인 네이티브.NET 문자열 형식에서 노출 하는 사용 하 여 API에 대 한 호출이 `string`에서 문자열 조작에 대 한 C# 및 다른.NET 프로그래밍 언어를 문자열 대신 API에서 노출 된 데이터 형식으로 노출 하 고 합니다 `NSString` 데이터 형식입니다.

즉, 개발자가 Xamarin.iOS 및 Xamarin.Mac API (통합)를 호출 하는 데 사용할 수 있는 문자열을 유지할 필요가 없습니다에 특별 한 형식 (`Foundation.NSString`)를 사용 하 여 Mono의를 유지할 수 있습니다 이러한 `System.String` 모든 작업에 대 한 언제 Xamarin.iOS 또는 Xamarin.Mac API는 문자열이 필요, API 바인딩의 마샬링 정보를 담당 합니다.

"예를 들어, Objective-c로 text" 속성을 `UILabel` 형식의 `NSString`, 다음과 같이 선언 됩니다.

```objc
@property(nonatomic, copy) NSString *text
```

이 기능은으로 Xamarin.iOS에서 표시 됩니다.

```csharp
class UILabel {
    public string Text { get; set; }
}
```

내부적으로이 속성의 구현에서는 마샬링합니다를 C# 문자열는 `NSString` 호출 및는 `objc_msgSend` Objective-c는 동일한 방식에서 메서드.

사용 하지 않는 타사 Objective C Api의 몇 가지는 `NSString`를 대신 C 문자열을 사용 하지만 (을 "*char*"). 이러한 경우 계속 사용할 수 있습니다는 C# 문자열 데이터 형식 사용 해야 합니다는 [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) 특성으로이 문자열을 마샬링할 수 해야 바인딩 생성기에 알림을 보내야는 `NSString`, 하지만 대신 C 문자열로 합니다.

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>규칙의 예외

Xamarin.iOS 및 Xamarin.Mac에서이 규칙에 예외를 만들었습니다. 위해 노출 하는 경우 간의 의사 결정 `string`s, 확인 했습니다를 제외 하 고 노출 `NSString`s, 경우에 수행 됩니다는 `NSString` 메서드 콘텐츠 비교 대신 포인터 비교를 수행할 수 있습니다.

Objective C Api는 공용을 사용 하는 경우 발생할 수 있습니다 `NSString` 상수를 문자열의 실제 콘텐츠를 비교 하는 대신 몇 가지 작업을 나타내는 토큰입니다.

이러한 경우 `NSString`  Api은 표시 되며이 Api의 소수입니다. NSString 속성 일부 클래스에 노출 되는 확인할 수 있습니다. 이러한 `NSString` 알림과 같은 항목에 대 한 속성 노출 됩니다. 되 속성 일반적으로 다음과 같습니다.

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```
알림은에 사용 되는 키를 `NSNotification` 런타임에 의해 브로드캐스팅 되는 특정 이벤트에 등록 하려고 할 때 클래스입니다.

일반적으로 키를 모양은 다음과 같습니다.

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

다른 위치를 `NSString`s에 노출 된 API를 사용 하는 iOS에서 특정 Api 또는 OS X에 대 한 매개 변수로 사용 되는 토큰은 `NSDictionary` 개체를 매개 변수로 합니다. 일반적으로 사전에 포함 되어 `NSString` 키입니다. Xamarin.iOS를 규칙에 따라 이름을 해당 정적 `NSString` "키" 이름을 추가 하 여 속성입니다.
