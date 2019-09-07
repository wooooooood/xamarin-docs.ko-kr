---
title: 느슨하게 결합된 구성 요소 간 통신
description: '이 장에서는 eShopOnContainers 모바일 앱이 게시-구독 패턴을 구현 하는 방법에 대해 설명 합니다 .이 패턴을 사용 하면 개체 및 형식 참조로 쉽게 연결할 수 있는 구성 요소 간에 메시지 기반 통신이 가능 합니다. '
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d4ed362fdd5587eabc028949b82682922adead0a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760299"
---
# <a name="communicating-between-loosely-coupled-components"></a>느슨하게 결합된 구성 요소 간 통신

게시-구독 패턴은 게시자가 구독자로 알려진 수신자를 몰라도 메시지를 보내는 메시징 패턴입니다. 마찬가지로 구독자는 게시자를 전혀 알지 못해도 특정 메시지를 수신 대기합니다.

.NET의 이벤트는 게시-구독 패턴을 구현 하며,이를 포함 하는 컨트롤 및 페이지와 같이 느슨한 결합이 필요 하지 않은 경우에는 구성 요소 간의 통신 계층을 가장 간단 하 고 간단한 방법으로 사용할 수 있습니다. 그러나 게시자 및 구독자 수명은 서로 개체 참조로 결합 되며 구독자 유형에는 게시자 유형에 대 한 참조가 있어야 합니다. 이렇게 하면 특히 정적 또는 수명이 긴 개체의 이벤트를 구독 하는 수명이 짧은 개체가 있는 경우 메모리 관리 문제가 발생할 수 있습니다. 이벤트 처리기가 제거 되지 않은 경우 구독자는 게시자에서이에 대 한 참조에 의해 활성 상태로 유지 되 고 구독자의 가비지 수집을 방지 하거나 지연 합니다.

## <a name="introduction-to-messagingcenter"></a>MessagingCenter 소개

Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 클래스는 게시-구독 패턴을 구현하여 개체 및 형식 참조로 연결하기 불편한 구성 요소 사이의 메시지 기반 통신을 허용합니다. 이 메커니즘을 통해 게시자와 구독자는 서로에 대 한 참조 없이 통신할 수 있으며, 구성 요소 간의 종속성을 줄이고 구성 요소를 독립적으로 개발 하 고 테스트할 수 있습니다.

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 클래스에서는 멀티캐스트 게시-구독 기능을 제공합니다. 즉, 단일 메시지를 게시 하는 여러 게시자가 있을 수 있으며 동일한 메시지를 수신 대기 하는 여러 구독자가 있을 수 있습니다. 그림 4-1에서는이 관계를 보여 줍니다.

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "멀티캐스트 게시-구독 기능")

**그림 4-1:** 멀티 캐스트 게시-구독 기능

게시자는 [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 메서드를 사용하여 메시지를 보내는 한편, 구독자는 [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 메서드를 사용하여 메시지를 수신 대기합니다. 또한 구독자는 필요한 경우 [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 메서드를 사용하여 메시지 구독을 구독 취소할 수도 있습니다.

내부적으로 [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 클래스에서는 약한 참조를 사용합니다. 즉, 개체가 활성 상태로 유지되지 않으며 가비지 수집될 수 있습니다. 따라서 클래스가 더 이상 메시지를 받지 않으려는 경우에만 메시지의 구독을 취소해야 합니다.

EShopOnContainers 모바일 앱은 [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 클래스를 사용 하 여 느슨하게 결합 된 구성 요소 간에 통신 합니다. 앱은 세 개의 메시지를 정의 합니다.

- 이 `AddProduct` 메시지는 바구니에 항목이 `CatalogViewModel` 추가 될 때 클래스에 의해 게시 됩니다. 반환 시 클래스는 `BasketViewModel` 메시지를 구독 하 고 장바구니의 항목 수를 응답으로 증가 시킵니다. 또한 클래스는 `BasketViewModel` 이 메시지에서 구독을 취소 합니다.
- 이 `Filter` 메시지는 사용자가 카탈로그 `CatalogViewModel` 에서 표시 된 항목에 브랜드 또는 형식 필터를 적용할 때 클래스에 의해 게시 됩니다. 반환 시 클래스는 `CatalogView` 메시지를 구독 하 고 필터 조건과 일치 하는 항목만 표시 되도록 UI를 업데이트 합니다.
- 이 `ChangeTab` 메시지는가 성공적으로 생성 되 고 `CheckoutViewModel` 새 주문의 전송 `MainViewModel` 으로 이동 하면 `MainViewModel` 클래스에 의해 게시 됩니다. 반환 시 클래스는 `MainView` 메시지를 구독 하 고 사용자의 주문을 표시 하기 위해 **내 프로필** 탭이 활성화 되도록 UI를 업데이트 합니다.

> [!NOTE]
> 클래스는 [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 느슨하게 결합 된 클래스 간의 통신을 허용 하지만이 문제에 대 한 유일한 아키텍처 솔루션은 제공 하지 않습니다. 예를 들어 뷰 모델과 뷰 간의 통신은 바인딩 엔진과 속성 변경 알림을 통해 구현할 수도 있습니다. 또한 탐색 하는 동안 데이터를 전달 하 여 두 뷰 모델 간의 통신을 수행할 수도 있습니다.

EShopOnContainers 모바일 앱 [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 에서는 다른 클래스에서 발생 하는 동작에 대 한 응답으로 UI에서를 업데이트 하는 데 사용 됩니다. 따라서 메시지는 동일한 스레드에서 메시지를 받는 구독자와 함께 UI 스레드에 게시 됩니다.

> [!TIP]
> UI 업데이트를 수행할 때 UI 스레드로 마샬링합니다. Ui를 업데이트 하기 위해 백그라운드 스레드에서 보낸 메시지가 필요한 경우 메서드를 [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) 호출 하 여 구독자의 ui 스레드에서 메시지를 처리 합니다.

에 대 한 [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)자세한 내용은 [messagingcenter](~/xamarin-forms/app-fundamentals/messaging-center.md)를 참조 하세요.

## <a name="defining-a-message"></a>메시지 정의

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter)메시지는 메시지를 식별 하는 데 사용 되는 문자열입니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱 내에서 정의 된 메시지를 보여 줍니다.

```csharp
public class MessengerKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

이 예제에서 메시지는 상수를 사용 하 여 정의 됩니다. 이 방법의 장점은 컴파일 시간 형식 안전성 및 리팩터링 지원을 제공 한다는 것입니다.

## <a name="publishing-a-message"></a>메시지 게시

게시자는 [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 오버로드 중 하나로 구독자에게 메시지를 알립니다. 다음 코드 예제에서는 메시지를 `AddProduct` 게시 하는 방법을 보여 줍니다.

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

이 예제에서 메서드는 [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 세 개의 인수를 지정 합니다.

- 첫 번째 인수는 sender 클래스를 지정 합니다. 보낸 사람 클래스는 메시지를 받으려는 모든 구독자가 지정 해야 합니다.
- 두 번째 인수는 메시지를 지정합니다.
- 세 번째 인수는 구독자로 보낼 페이로드 데이터를 지정 합니다. 이 경우 페이로드 데이터 `CatalogItem` 는 인스턴스입니다.

메서드 [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 는 화재 및 잊은 방법을 사용 하 여 메시지와 해당 페이로드 데이터를 게시 합니다. 따라서 메시지를 수신하도록 등록된 구독자가 없는 경우에도 메시지가 전송됩니다. 이 경우 보낸 메시지는 무시됩니다.

> [!NOTE]
> 메서드 [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 는 제네릭 매개 변수를 사용 하 여 메시지를 배달 하는 방법을 제어할 수 있습니다. 따라서 여러 다른 구독자가 메시지 id를 공유 하지만 다른 페이로드 데이터 형식을 보내는 여러 메시지를 수신할 수 있습니다.

## <a name="subscribing-to-a-message"></a>메시지 구독

구독자는 [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 오버로드 중 하나를 사용하여 메시지를 받도록 등록할 수 있습니다. 다음 코드 예제에서는 eShopOnContainers mobile 앱이 `AddProduct` 메시지를 구독 하 고 처리 하는 방법을 보여 줍니다.

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

이 예제에서 메서드는 [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) `AddProduct` 메시지를 구독 하 고 메시지 수신에 대 한 응답으로 콜백 대리자를 실행 합니다. 람다 식으로 지정 된이 콜백 대리자는 UI를 업데이트 하는 코드를 실행 합니다.

> [!TIP]
> 변경할 수 없는 페이로드 데이터를 사용 하는 것이 좋습니다. 여러 스레드가 받은 데이터에 동시에 액세스할 수 있으므로 콜백 대리자 내에서 페이로드 데이터를 수정 하지 마세요. 이 시나리오에서는 동시성 오류가 발생 하지 않도록 페이로드 데이터를 변경할 수 없습니다.

구독자는 게시된 메시지의 모든 인스턴스를 처리할 필요가 없으며 이는 [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 메서드에 지정된 제네릭 형식 인수를 통해 제어할 수 있습니다. 이 예에서 구독자는 페이로드 데이터가 `AddProduct` `CatalogItem` 인스턴스인 `CatalogViewModel` 클래스에서 전송 된 메시지만 받습니다.

## <a name="unsubscribing-from-a-message"></a>메시지에서 구독 취소

구독자는 더 이상 수신하지 않을 메시지를 구독 취소할 수 있습니다. 이는 다음 코드 예제에서 보여 [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 주는 것 처럼 오버 로드 중 하나를 사용 하 여 수행 됩니다.

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

이 예제 [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 에서 메서드 구문은 `AddProduct` 메시지 수신을 구독할 때 지정 된 형식 인수를 반영 합니다.

## <a name="summary"></a>요약

Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 클래스는 게시-구독 패턴을 구현하여 개체 및 형식 참조로 연결하기 불편한 구성 요소 사이의 메시지 기반 통신을 허용합니다. 이 메커니즘을 통해 게시자와 구독자는 서로에 대 한 참조 없이 통신할 수 있으며, 구성 요소 간의 종속성을 줄이고 구성 요소를 독립적으로 개발 하 고 테스트할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [다운로드 전자책 (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
