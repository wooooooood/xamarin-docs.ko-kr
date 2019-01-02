---
title: Xamarin.Forms MessagingCenter
description: 이 문서에서는 Xamarin.Forms MessagingCenter를 사용하여 메시지를 보내고 받으면서 보기 모델과 같은 클래스 간의 결합을 줄이는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: ecd3fe7256eeaa51baf1bc2c367ff7560db51b0c
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055817"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/UsingMessagingCenter)

_Xamarin.Forms에는 메시지를 보내거나 받을 수 있는 간단한 메시징 서비스가 포함되어 있습니다._

<a name="Overview" />

## <a name="overview"></a>개요

Xamarin.Forms `MessagingCenter`를 사용하면 보기 모델 및 다른 구성 요소에서 간단한 메시지 계약 외에도 서로에 대해 인식할 필요 없이 통신할 수 있습니다.

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>MessagingCenter의 작동 방식

`MessagingCenter`에는 두 가지 부분이 있습니다.

-  **Subscribe** - 특정 서명이 있는 메시지를 수신 대기하고, 수신되면 일부 작업을 수행합니다. 여러 구독자가 동일한 메시지를 수신 대기할 수 있습니다.
-  **Send** - 수신기에서 수행할 메시지를 게시합니다. 수신기에서 구독하지 않으면 메시지가 무시됩니다.


`MessagingService`는 솔루션 전체에서 사용되는 `Subscribe` 및 `Send` 메서드가 있는 정적 클래스입니다.

메시지에는 메시지를 *처리*하는 방법으로 사용되는 `message` 문자열 매개 변수가 있습니다. `Subscribe` 및 `Send` 메서드는 제네릭 매개 변수를 사용하여 메시지를 전달하는 방식을 더 자세히 제어합니다. 동일한 `message` 텍스트를 사용하지만 제네릭 형식 인수가 다른 두 개의 메시지는 동일한 구독자에게 전달되지 않습니다.

`MessagingCenter`에 대한 API는 다음과 같이 간단합니다.

- `Subscribe<TSender> (object subscriber, string message, Action<TSender> callback, TSender source = null)`
- `Subscribe<TSender, TArgs> (object subscriber, string message, Action<TSender, TArgs> callback, TSender source = null)`
- `Send<TSender> (TSender sender, string message)`
- `Send<TSender, TArgs> (TSender sender, string message, TArgs args)`
- `Unsubscribe<TSender, TArgs> (object subscriber, string message)`
- `Unsubscribe<TSender> (object subscriber, string message)`

이러한 메서드는 아래에 설명되어 있습니다.

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>MessagingCenter 사용

메시지는 사용자 상호 작용(예: 단추 클릭), 시스템 이벤트(예: 컨트롤 변경 상태) 또는 일부 다른 인시던트(예: 비동기 다운로드 완료 등)의 결과로 전송될 수 있습니다. 구독자는 사용자 인터페이스의 모양을 변경하거나, 데이터를 저장하거나, 일부 다른 작업을 트리거하기 위해 수신 대기 중일 수 있습니다.

### <a name="simple-string-message"></a>간단한 문자열 메시지

가장 간단한 메시지에는 `message` 매개 변수의 문자열만 포함됩니다. 간단한 문자열 메시지를 *수신 대기*하는 `Subscribe` 메서드는 아래와 같습니다. 보낸 사람을 지정하는 제네릭 형식이 `MainPage` 형식이어야 합니다. 솔루션의 모든 클래스에서 다음 구문을 사용하여 메시지를 구독할 수 있습니다.

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

`MainPage` 클래스의 다음 코드에서 메시지를 *보냅니다*. `this` 매개 변수는 `MainPage`의 인스턴스입니다.

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

문자열은 변경되지 않습니다. *메시지 유형*을 나타내며 알림을 보낼 구독자를 결정하는 데 사용됩니다. 이러한 종류의 메시지는 추가 정보가 필요하지 않은 "업로드 완료"와 같은 일부 이벤트가 발생했음을 나타내는 데 사용됩니다.

### <a name="passing-an-argument"></a>인수 전달

인수를 메시지와 함께 전달하려면 `Subscribe` 제네릭 인수와 Action 서명에 Type 인수를 지정합니다.

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

인수를 사용하여 메시지를 보내려면 Type 제네릭 매개 변수와 인수의 값을 `Send` 메서드 호출에 포함시킵니다.

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

이 간단한 예제에서는 `string` 인수를 사용하지만 C# 개체를 전달할 수 있습니다.

### <a name="unsubscribe"></a>구독 취소

개체는 메시지 서명에서 구독을 취소하여 이후 메시지가 전달되지 않도록 할 수 있습니다. `Unsubscribe` 메서드 구문에서 메시지의 서명을 반영해야 하므로 메시지 인수에서 Type 제네릭 매개 변수를 포함해야 할 수도 있습니다.

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>요약

MessagingCenter는 특히 보기 모델 간의 결합을 줄일 수 있는 간단한 방법입니다. 간단한 메시지를 보내고 받고, 클래스 rks에 인수를 전달하는 데 사용할 수 있습니다. 클래스는 더 이상 받지 않으려는 메시지의 구독을 취소해야 합니다.


## <a name="related-links"></a>관련 링크

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
