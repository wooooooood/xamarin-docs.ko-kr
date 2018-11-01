---
title: Xamarin.Forms MessagingCenter
description: 이 문서에서는 Xamarin.Forms MessagingCenter 모델 보기와 같은 클래스 간의 결합을 줄이기 위해 메시지를 받고 보내는 데 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 7fef4443cacba0fa8bdb8d5df070c4244730b4f5
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675177"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

_Xamarin.Forms에는 메시지를 주고받을 수 있는 간단한 메시징 서비스가 포함 됩니다._

<a name="Overview" />

## <a name="overview"></a>개요

Xamarin.Forms `MessagingCenter` 모델 및 기타 구성 요소는 간단한 메시지 계약 외에도 서로 대 한 지식이 필요 없이 통신할 수 있도록 확인 합니다.

<a name="How_the_MessagingCenter_Works" />

## <a name="how-the-messagingcenter-works"></a>MessagingCenter의 작동 원리

두 부분이 있습니다 `MessagingCenter`:

-  **구독** -특정 시그니처를 사용 하 여 메시지를 수신 하 고 수신 되 면 일부 작업을 수행 합니다. 여러 구독자가 동일한 메시지 수신 하 고 있을 수 있습니다.
-  **보낼** -를 사용 하는 수신기에 대 한 메시지를 게시 합니다. 수신기를 찾지 가입한 메시지가 무시 됩니다.


합니다 `MessagingService` 포함 된 정적 클래스는 `Subscribe` 및 `Send` 솔루션 전체에서 사용 되는 메서드.

메시지 문자열이 있을 `message` 방법으로 사용 되는 매개 변수 *주소* 메시지입니다. `Subscribe` 하 고 `Send` 메시지 배달 되는 방법을 제어 하기 제네릭 매개 변수를 사용 하는 방법-두 개의 동일한 메시지를 `message` 텍스트 하지만 다른 제네릭 형식 인수 배달 되지 것입니다 동일한 구독자에 합니다.

에 대 한 API `MessagingCenter` 간단 합니다.

- `Subscribe<TSender> (object subscriber, string message, Action<TSender> callback, TSender source = null)`
- `Subscribe<TSender, TArgs> (object subscriber, string message, Action<TSender, TArgs> callback, TSender source = null)`
- `Send<TSender> (TSender sender, string message)`
- `Send<TSender, TArgs> (TSender sender, string message, TArgs args)`
- `Unsubscribe<TSender, TArgs> (object subscriber, string message)`
- `Unsubscribe<TSender> (object subscriber, string message)`

이러한 메서드는 아래 설명 되어 있습니다.

<a name="Using_the_MessagingCenter" />

## <a name="using-the-messagingcenter"></a>MessagingCenter를 사용 하 여

사용자 상호 작용 (예: 단추 클릭), 시스템 이벤트 (예: 상태를 변경 하는 컨트롤) 또는 일부 다른 인시던트 (예: 비동기 다운로드 완료)으로 인해 메시지를 보낼 수 있습니다. 사용자 인터페이스의 모양을 변경, 데이터 저장 또는 다른 작업 트리거를 구독자 수신할 수도 있습니다.

### <a name="simple-string-message"></a>간단한 문자열 메시지

가장 간단한 메시지에 문자열로 포함 된 `message` 매개 변수입니다. A `Subscribe` 메서드는 *수신* 아래에 간단한 문자열 메시지가 표시 됩니다에 대 한 보낸 사람에 게 형식 이어야 하는데 지정 된 제네릭 형식 확인 `MainPage`합니다. 솔루션의 모든 클래스는이 구문을 사용 하 여 메시지를 구독할 수 있습니다.

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

에 `MainPage` 클래스를 다음 코드로 *보냅니다* 메시지입니다. 합니다 `this` 매개 변수는 인스턴스의 `MainPage`합니다.

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

문자열을 변경 하지 않습니다 나타냅니다는 *메시지 유형을* 알릴 구독자를 결정 하는 데 사용 됩니다. 발생 했음을 나타내기 위해 몇 가지 이벤트를 "완료 된 업로드" 등이 유형의 메시지는 추가 정보 없음이 필요한 경우.

### <a name="passing-an-argument"></a>인수 전달

메시지를 사용 하 여 인수를 전달 하려면 형식 인수를 지정에 `Subscribe` 제네릭 인수 및 작업 서명 합니다.

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

인수를 사용 하 여 메시지를 보내려고 형식 제네릭 매개 변수 및 인수 값을 포함 합니다 `Send` 메서드를 호출 합니다.

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

이 간단한 예제를 사용 하는 `string` 인수가 있지만 모든 C# 개체를 전달할 수 있습니다.

### <a name="unsubscribe"></a>구독 취소

개체는 전달 된 이후 메시지가 없습니다 메시지 서명에서 구독 취소할 수 있습니다. `Unsubscribe` 메서드 구문을 메시지 서명의 반영 해야 (따라서 해야 할 메시지 인수에 대 한 제네릭 형식 매개 변수를 포함).

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## <a name="summary"></a>요약

MessagingCenter 특히 보기 모델 간의 결합을 줄일 수 간단한 방법이 있습니다. 보내기 및 간단한 메시지를 수신, 클래스 간의 인수를 전달에 사용할 수 있습니다. 클래스는 더 이상 수신 하고자 하는 메시지에서 구독 취소 해야 합니다.


## <a name="related-links"></a>관련 링크

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
