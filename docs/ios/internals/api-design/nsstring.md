---
title: Xamarin.iOS 및 Xamarin.Mac NSString
description: 이 문서에서는 어떻게 Xamarin.iOS 투명 하 게 NSString 때 개체를 변환 C# string 개체의 경우이 문제가 발생 하지 설명 합니다.
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: baf36700ab4d608296a9a67e234ce613da9ca077
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786092"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>Xamarin.iOS 및 Xamarin.Mac NSString

네이티브.NET string 형식의 노출 하는 사용 하 여 API에 대 한 Xamarin.iOS 및 Xamarin.Mac의 설계 시 `string`는 대신API에의해노출되는데이터형식으로문자열을노출하고C#에서문자열조작및다른.NET프로그래밍언어에대한`NSString` 데이터 형식입니다.

즉, 개발자가 없어야 한다는 Xamarin.iOS 및 Xamarin.Mac API (통합)를 호출 하는 데 사용 하는 문자열을 유지 하는 특수 한 종류의 (`Foundation.NSString`)를 사용 하 여 모노의를 유지할 수 있습니다 `System.String` 모든 작업을 언제 Xamarin.iOS 또는 Xamarin.Mac API에는 문자열이 필요, API 바인딩에 마샬링 정보를 처리할 수 있습니다.

예를 들어의 Objective-c "text" 속성은 `UILabel` 형식의 `NSString`, 다음과 같이 선언 됩니다.

```objc
@property(nonatomic, copy) NSString *text
```

이것으로 Xamarin.iOS에 노출 됩니다.

```csharp
class UILabel {
    public string Text { get; set; }
}
```

내부적으로이 속성의 구현 마샬링합니다 C# 문자열에는 `NSString` 호출는 `objc_msgSend` Objective C는 동일한 방식으로 메서드.

제 3 자 Objective-c api를 사용 하지 않는 여러는 `NSString`, 하지만 C 문자열을 대신 사용 (한 "*char*"). 이러한 경우에도 사용할 수 C# string 데이터 형식에 있지만 사용 해야 합니다는 [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) 특성으로이 문자열을 마샬링할 수 해야 하는 바인딩 생성기를 알리기 위해는 `NSString`, 않고 대신 C 문자열입니다.

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>규칙의 예외

Xamarin.iOS 및 Xamarin.Mac 모두에서이 규칙에 예외를 기능이 되었습니다. 공개 했습니다 효과적일 `string`s, 확인 우리는 제외 하 고 노출 `NSString`s, 경우에 수행 됩니다는 `NSString` 메서드 콘텐츠 비교 대신 포인터 비교를 수행 될 수 없습니다.

Objective C Api 공용을 사용 하는 경우 `NSString` 토큰 문자열의 실제 내용을 비교 하는 대신 일부 작업을 나타내는 상수입니다.

이 경우 `NSString` Api 노출 되 고이 있는 Api의 소수입니다. 알게 될 NSString 속성 일부 클래스에 노출 됩니다. 이러한 `NSString` 알림 같은 항목에 대 한 속성은 표시 됩니다. 이러한 속성은 일반적으로 다음과 같이 표시 됩니다.

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```
키에 사용 되는 알림은 `NSNotification` 런타임에 의해 브로드캐스팅 되는 특정 이벤트에 대 한 등록 하려는 경우 클래스입니다.

일반적으로 키를 모양은 다음과 같습니다.

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

다른 장소 여기서 `NSString`s에 노출 되는 API를 사용 하는 iOS에서 특정 Api 또는 OS X에 대 한 매개 변수로 사용 되는 토큰으로는 `NSDictionary` 개체를 매개 변수로 합니다. 사전에 포함 되어 일반적으로 `NSString` 키입니다. Xamarin.iOS, 규칙에 따라 이름을 지정 하는 것 정적 `NSString` "키" 이름을 추가 하 여 속성입니다.
