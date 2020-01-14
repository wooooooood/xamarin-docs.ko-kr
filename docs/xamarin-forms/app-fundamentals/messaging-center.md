---
title: Xamarin.Forms MessagingCenter
description: Xamarin.ios MessagingCenter 클래스는 게시-구독 패턴을 구현하여 개체 및 형식 참조로 연결하기 불편한 구성 요소 사이의 메시지 기반 통신을 허용합니다.
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
ms.openlocfilehash: 0e5fd88678becd7becfcb1c43e14b1e33aad72de
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489884"
---
# <a name="xamarinforms-messagingcenter"></a>Xamarin.Forms MessagingCenter

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingmessagingcenter)

게시-구독 패턴은 게시자가 구독자로 알려진 수신자를 몰라도 메시지를 보내는 메시징 패턴입니다. 마찬가지로 구독자는 게시자를 전혀 알지 못해도 특정 메시지를 수신 대기합니다.

.NET의 이벤트는 게시-구독 패턴을 구현하는데, 이는 컨트롤과 컨트롤을 포함하는 페이지처럼 느슨한 결합이 필요하지 않은 경우 구성 요소 간의 통신 레이어를 위한 가장 간단한 접근 방식입니다. 그러나 게시자 수명과 구독자 수명은 상호 개체 참조에 의해 결합되어 있으며, 구독자 유형에는 게시자 유형에 대한 참조가 있어야 합니다. 이로 인해 특히 정적 이벤트를 구독하는 수명이 짧은 개체 또는 수명이 긴 개체가 있는 경우 메모리 관리 문제가 발생할 수 있습니다. 이벤트 처리기가 제거되지 않은 경우, 구독자는 게시자에 있는 해당 구독자에 대한 참조에 의해 활성 상태로 유지되고, 이로 인해 구독자의 가비지 수집이 방지 또는 지연됩니다.

Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 클래스는 게시-구독 패턴을 구현하여 개체 및 형식 참조로 연결하기 불편한 구성 요소 사이의 메시지 기반 통신을 허용합니다. 이 메커니즘을 통해 게시자와 구독자가 서로를 참조하지 않고 통신할 수 있으므로 둘 사이의 종속성을 줄일 수 있습니다.

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 클래스에서는 멀티캐스트 게시-구독 기능을 제공합니다. 즉, 단일 메시지를 게시하는 여러 게시자가 있을 수 있으며 동일한 메시지를 수신 대기하는 여러 구독자가 있을 수 있습니다.

![](messaging-center-images/messaging-center.png "Multicast publish-subscribe functionality")

게시자는 [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 메서드를 사용하여 메시지를 보내는 한편, 구독자는 [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 메서드를 사용하여 메시지를 수신 대기합니다. 또한 구독자는 필요한 경우 [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 메서드를 사용하여 메시지 구독을 구독 취소할 수도 있습니다.

> [!IMPORTANT]
> 내부적으로 [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 클래스에서는 약한 참조를 사용합니다. 즉, 개체가 활성 상태로 유지되지 않으며 가비지 수집될 수 있습니다. 따라서 클래스가 더 이상 메시지를 받지 않으려는 경우에만 메시지의 구독을 취소해야 합니다.

## <a name="publish-a-message"></a>메시지 게시

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 메시지는 문자열입니다. 게시자는 [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 오버로드 중 하나로 구독자에게 메시지를 알립니다. 다음 코드 예제에서는 `Hi` 메시지를 게시합니다.

```csharp
MessagingCenter.Send<MainPage>(this, "Hi");
```

이 예제에서는 [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 메서드를 통해 보낸 사람을 나타내는 제네릭 인수를 지정합니다. 메시지를 수신하려면 구독자가 동일한 제네릭 인수를 지정하여 보낸 사람의 메시지를 수신 대기하고 있음을 나타내야 합니다. 또한 이 예제에서는 두 개의 메서드 인수를 지정합니다.

- 첫 번째 인수는 보낸 사람 인스턴스를 지정합니다.
- 두 번째 인수는 메시지를 지정합니다.

페이로드 데이터는 메시지와 함께 보낼 수도 있습니다.

```csharp
MessagingCenter.Send<MainPage, string>(this, "Hi", "John");
```

이 예제에서는 [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 메서드를 통해 두 개의 제네릭 인수를 지정합니다. 첫 번째는 메시지를 보내는 형식이고, 두 번째는 전송되는 페이로드 데이터의 유형입니다. 메시지를 받으려면 구독자가 동일한 제네릭 인수도 지정해야 합니다. 그러면 여러 메시지에서 메시지 ID를 공유하지만 여러 다른 구독자가 받도록 여러 다른 페이로드 데이터 형식을 보낼 수 있습니다. 이 예제에서는 구독자에게 보낼 페이로드 데이터를 나타내는 세 번째 메서드 인수도 지정합니다. 이 경우 페이로드 데이터는 `string`입니다.

[`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 메서드에서는 실행 후 제거(fire-and-forget) 방법을 사용하여 메시지와 페이로드를 게시합니다. 따라서 메시지를 수신하도록 등록된 구독자가 없는 경우에도 메시지가 전송됩니다. 이 경우 보낸 메시지는 무시됩니다.

## <a name="subscribe-to-a-message"></a>메시지 구독

구독자는 [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 오버로드 중 하나를 사용하여 메시지를 받도록 등록할 수 있습니다. 다음 코드 예제는 이러한 예를 보여줍니다.

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) =>
{
    // Do something whenever the "Hi" message is received
});
```

이 예제에서 [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 메서드는 `MainPage` 형식으로 전송된 `Hi` 메시지에 `this` 개체를 등록하고 메시지를 수신하는 데 대한 응답으로 콜백 대리자를 실행합니다. 람다 식으로 지정된 콜백 대리자는 UI를 업데이트하거나 일부 데이터를 저장하거나 다른 작업을 트리거하는 코드일 수 있습니다.

> [!NOTE]
> 구독자는 게시된 메시지의 모든 인스턴스를 처리할 필요가 없으며 이는 [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 메서드에 지정된 제네릭 형식 인수를 통해 제어할 수 있습니다.

다음 예에서는 페이로드 데이터를 포함하는 메시지를 구독하는 방법을 보여줍니다.

```csharp
MessagingCenter.Subscribe<MainPage, string>(this, "Hi", async (sender, arg) =>
{
    await DisplayAlert("Message received", "arg=" + arg, "OK");
});
```

이 예제에서는 [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 메서드를 통해 페이로드 데이터가 `string`인 `MainPage` 형식으로 보낸 `Hi` 메시지를 구독합니다. 콜백 대리자는 경고에 페이로드 데이터를 표시하는 해당 메시지를 수신하는 데 대한 응답으로 실행됩니다.

> [!IMPORTANT]
> `Subscribe` 메소드로 실행된 대리자는 `Send` 메소드를 사용하여 메시지를 게시하는 동일한 스레드에서 실행됩니다.

## <a name="unsubscribe-from-a-message"></a>메시지 구독 취소

구독자는 더 이상 수신하지 않을 메시지를 구독 취소할 수 있습니다. 이 조치는 [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 오버로드 중 하나를 통해 수행합니다.

```csharp
MessagingCenter.Unsubscribe<MainPage>(this, "Hi");
```

이 예제에서는 [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 메소드가 `MainPage` 형식을 통해 보낸 `Hi` 메시지에서 `this` 개체를 구독 취소합니다.

페이로드 데이터를 포함하는 메시지는 두 개의 제네릭 인수를 지정하는 [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 오버로드를 사용하여 구독을 취소해야 합니다.

```csharp
MessagingCenter.Unsubscribe<MainPage, string>(this, "Hi");
```

이 예제에서는 [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 메소드가 `MainPage` 형식을 통해 보낸 `Hi` 메시지에서 `this` 개체를 구독 취소합니다. 해당 페이로드 데이터는 `string`입니다.

## <a name="related-links"></a>관련 링크

- [MessagingCenterSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingmessagingcenter)
