---
title: MessagingCenter
description: "Xamarin.Forms는 메시지를 받거나 보내기 위해 간단한 메시징 서비스를 포함 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: ede78d4a041f8619ff97b3da802efb18943ef8ae
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="messagingcenter"></a>MessagingCenter

_Xamarin.Forms는 메시지를 받거나 보내기 위해 간단한 메시징 서비스를 포함 합니다._

<a name="Overview" />

## <a name="overview"></a>개요

Xamarin.Forms `MessagingCenter` 뷰 모델 및 기타 구성 요소는 간단한 메시지 계약 외에도 서로 대해 알 필요 없이 통신할 수 있습니다.

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>MessagingCenter 작동 방식

위한 두 가지 `MessagingCenter`:

-  **구독** -특정 서명 사용 하 여 메시지를 수신 대기 하 고 전송 되 면 일부 작업을 수행 합니다. 여러 구독자가 동일한 메시지를 수신 대기 수 수 있습니다.
-  **보내기** -를 사용 하는 수신기에 대 한 메시지를 게시 합니다. 수신기가 구독 하는 경우 메시지는 무시 됩니다.


`MessagingService` 사용 하는 정적 클래스는 `Subscribe` 및 `Send` 솔루션 전체에서 사용 되는 메서드입니다.

메시지 문자열이 있을 `message` 방법으로 사용 되는 매개 변수 *주소* 메시지입니다. `Subscribe` 및 `Send` 메시지 배달 되는 방법을 제어 하기 제네릭 매개 변수를 사용 하는 방법-2가 동일한 메시지를 `message` 텍스트 하지만 서로 다른 제네릭 형식 인수로 전달 되지 것입니다 동일한 구독자에 있습니다.

에 대 한 API `MessagingCenter` 은 간단 합니다.

-  구독&lt;한 > (구독자 개체, 문자열 메시지를 작업&lt;한 > 콜백, 한 원본 = null)
-  구독&lt;, TArgs > (구독자 개체, 문자열 메시지를 작업&lt;, TArgs > 콜백, 한 원본 = null)
-  보내기&lt;한 > (한 보낸 사람, 문자열 메시지)
-  보내기&lt;, TArgs > (한 보낸 사람, 문자열 메시지 TArgs args)
-  구독 취소&lt;, TArgs > (구독자 개체, 문자열 메시지)
-  구독 취소&lt;한 > (구독자 개체, 문자열 메시지)


이러한 메서드는 아래 설명 되어 있습니다.

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>MessagingCenter를 사용 하 여

사용자 상호 작용 (예: 단추 클릭), 시스템 이벤트 (예: 상태를 변경 하는 제어) 또는 일부 다른 인시던트 (예: 비동기 다운로드 완료)으로 인해 메시지를 보낼 수 있습니다. 구독자는 사용자 인터페이스의 모양을 변경, 데이터를 저장 하거나 다른 작업을 트리거할 수신 대기 중일 수 있습니다.

### <a name="simple-string-message"></a>단순 문자열 메시지

가장 간단한 메시지에 문자열로 포함는 `message` 매개 변수입니다. A `Subscribe` 메서드는 *수신* 간단한 문자열 메시지는 다음과 같습니다-에 대 한 표시 형식 이어야 하는데 보낸 사람을 지정 하는 제네릭 형식 `MainPage`합니다. 솔루션의 모든 클래스는이 구문을 사용 하 여 메시지를 구독할 수 있습니다.

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

에 `MainPage` 다음 코드를 클래스 *보냅니다* 메시지입니다. `this` 매개 변수는 인스턴스의 `MainPage`합니다.

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

문자열-바뀌지 않습니다 나타냅니다는 *메시지 유형을* 알릴 구독자를 결정 하는 데 사용 됩니다. 이러한 종류의 메시지 일부 이벤트가 발생 했음을 "업로드가 완료 됨" 등을 나타내는 데 사용 됩니다 추가 정보 없음가 필요 합니다.

### <a name="passing-an-argument"></a>인수 전달

메시지와 함께 인수를 전달 하려면 형식 인수를 지정에 `Subscribe` 제네릭 인수 및 작업 서명이 있습니다.

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

인수를 사용 하 여 메시지를 보내려면 등이 제네릭 형식 매개 변수에서 인수 값은 `Send` 메서드를 호출 합니다.

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

사용 하 여이 간단한 예제는 `string` 인수가 있지만 모든 C# 개체를 전달할 수 있습니다.

### <a name="unsubscribe"></a>구독 취소

개체 이후 메시지는 배달 되지 않도록 메시지 서명에서 구독 취소할 수 있습니다. `Unsubscribe` 메서드 구문을 메시지의 서명을 반영 해야 (따라서 해야 할 수도 메시지 인수에 대 한 제네릭 형식 매개 변수 포함).

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>요약

MessagingCenter는 특히 보기 모델 간의 결합을 줄이기 위해 간단한 방법입니다. 및 간단한 메시지를 받을 보내거나 클래스 간의 인수를 전달 합니다. 사용할 수 있습니다. 클래스는 더 이상 수신 하고자 하는 메시지에서 구독 취소 해야 합니다.


## <a name="related-links"></a>관련 링크

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
