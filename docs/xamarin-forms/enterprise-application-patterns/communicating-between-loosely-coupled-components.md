---
title: "구성 요소를 결합 된 느슨하게 간의 통신"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: e05cd0ec7d03a033e24dcbfb8124cfc2ccfa438e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="communicating-between-loosely-coupled-components"></a>구성 요소를 결합 된 느슨하게 간의 통신

게시-구독 패턴은 게시자 구독자 라고 하는 모든 수신자에 대 한 지식이 필요 없이 메시지를 보낼 메시징 패턴입니다. 마찬가지로, 구독자가 게시자에 대 한 지식이 필요 없이 특정 메시지에 대 한 수신 대기 합니다.

.NET의 이벤트 구현 게시-구독 패턴을, 가장 단순 하 고 간단한 방법은 통신 계층에 대 한 느슨한 결합이 포함 된 페이지는 컨트롤 등 필요 하지 않은 경우 구성 요소 간의 되며 합니다. 그러나 게시자와 구독자 수명을 서로에 개체 참조에 의해 연결 되어 있으며 구독자 유형을 게시자 형식에 대 한 참조가 있어야 합니다. 이 생깁니다 메모리 관리 문제 특히 정적 또는 수명이 긴 개체의 이벤트를 구독 하는 수명이 짧은 개체가 때. 이벤트 처리기 삭제 되지 않고 구독자는에서 활성화 되어 게시자에서 해당 참조를 및 방지 되거나 구독자의 가비지 수집이 지연 됩니다.

## <a name="introduction-to-messagingcenter"></a>MessagingCenter 소개

Xamarin.Forms는 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) 클래스 구현 게시-구독 패턴, 개체 및 형식 참조 하 여 연결 편리한 되지 않는 구성 요소 간에 메시지 기반 통신을 허용 합니다. 이 메커니즘에는 게시자와 구독자에 대 한 참조, 구성 요소를 독립적으로 개발 하 고 테스트 가능 하도록 구성 요소 간의 종속성을 줄이는 데 도움이 필요 없이 통신할 수 있습니다.

[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) 클래스는 멀티 캐스트 게시-구독 기능을 제공 합니다. 즉, 단일 메시지를 게시 하는 여러 게시자 될 수 있으며 동일한 메시지를 수신 대기 하는 여러 구독자가 있을 수 있습니다. 그림 4-1이이 관계를 보여 줍니다.

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "멀티 캐스트 게시-구독 기능")

**그림 4-1:** 멀티 캐스트 게시-구독 기능

게시자를 사용 하 여 메시지를 보낼는 [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender%7D/p/TSender/System.String/) 메서드를 사용 하 여 메시지를 수신 하는 구독자 동안는 [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) 메서드. 또한 구독자도 구독을 취소할 수 메시지 등록에서 필요한 경우와 [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender%7D/p/System.Object/System.String/) 메서드.

내부적으로 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) 약한 참조를 사용 하는 클래스입니다. 즉, 유지 하지 것입니다 개체 활성화 하 고 가비지 수집 될 수 있도록 합니다. 따라서만 것 클래스는 더 이상 메시지를 수신 하려고 할 때 메시지에서 구독을 취소 해야 합니다.

EShopOnContainers 모바일 앱 사용 하 여는 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) 느슨하게 간에 통신 하는 클래스는 구성 요소를 결합 합니다. 응용 프로그램 세 개의 메시지를 정의합니다.

-   `AddProduct` 의해 게시 된 메시지는 `CatalogViewModel` 시장 바구니에 항목 추가 될 때 클래스입니다. 는 `BasketViewModel` 클래스는 메시지에 등록 하 고 응답에서 시장 바구니의 항목 수를 증가 시킵니다. 또한는 `BasketViewModel` 클래스도이 메시지에서 구독 해제 합니다.
-   `Filter` 의해 게시 된 메시지는 `CatalogViewModel` 클래스 사용자는 카탈로그에서 표시 되는 항목에 브랜드 또는 형식 필터를 적용 합니다. 는 `CatalogView` 클래스는 메시지에 등록 하 고 필터 조건과 일치 하는 항목에만 표시 되도록 UI를 업데이트 합니다.
-   `ChangeTab` 의해 게시 된 메시지는 `MainViewModel` 클래스는 `CheckoutViewModel` 이동는 `MainViewModel` 성공적으로 만들기 및 새 주문 제출 합니다. 는 `MainView` 클래스 구독 하는 메시지 및 UI 업데이트 하므로 하는 **내 프로필** 탭이 활성화는 사용자가 주문한 제품을 표시 합니다.

> [!NOTE]
> 반면는 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) 느슨하게 결합 된 클래스 간의 통신을 허용 하는 클래스만 아키텍처이 문제를 해결 하려면이 제공 하지 않습니다. 예를 들어 뷰와 뷰 모델 간의 통신도 가능 바인딩 엔진에 의해 및 속성 변경 알림을 통해 합니다. 또한 두 개의 뷰 모델 간의 통신 탐색 하는 동안 데이터를 전달 하 여 가능 합니다.

EShopOnContainers 모바일 앱에서[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) 다른 클래스에서 발생 하는 작업에 대 한 응답으로 UI를 업데이트 하는 데 사용 됩니다. 따라서 메시지는 UI 스레드에서 동일한 스레드에서 메시지를 받는 구독자에 게시 됩니다.

> [!TIP]
> 업데이트 UI를 수행 하는 경우을 UI 스레드에 마샬링해야 합니다. UI를 업데이트 하는 데 필요한 백그라운드 스레드에서 전송 된 메시지의 경우 호출 하 여 구독자에는 UI 스레드에서 메시지를 처리는 [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) 메서드.

에 대 한 자세한 내용은 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/), 참조 [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)합니다.

## <a name="defining-a-message"></a>메시지 정의

[`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) 메시지는 메시지를 식별 하는 데 사용 되는 문자열입니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱 내에서 정의 된 메시지를 보여 줍니다.

```csharp
public class MessengerKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

이 예제에서는 상수를 사용 하 여 메시지가 정의 됩니다. 이 방식의 장점은 컴파일 타임 형식 안전성 및 리팩터링 지원을 제공 한다는 것입니다.

## <a name="publishing-a-message"></a>메시지를 게시

게시자 알림 중 하나가 지정 된 메시지의 구독자는 [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) 오버 로드 합니다. 다음 코드 예제에서는 게시 된 `AddProduct` 메시지:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

이 예제는 [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) 메서드 3 개 인수를 지정 합니다.

-   첫 번째 인수는 보낸 사람 클래스를 지정합니다. 보낸 사람 클래스는 메시지를 수신 하려는 모든 구독자가 지정 되어야 합니다.
-   두 번째 인수는 메시지를 지정합니다.
-   세 번째 인수에는 페이로드 데이터를 구독자에 게 보내도록 지정 합니다. 이 경우에 페이로드 데이터는 `CatalogItem` 인스턴스.

[ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) 메서드는 메시지 및 실행 하 고 잊어 방식을 사용 하 여 페이로드 데이터를 게시 합니다. 따라서 메시지를 수신 하도록 등록 된 구독자가 없는 경우에 메시지가 보내집니다. 이 상황에서 보낸된 메시지는 무시 됩니다.

> [!NOTE]
> [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) 메서드 메시지 배달 되는 방법을 제어 하려면 제네릭 매개 변수를 사용할 수 있습니다. 따라서 메시지 id를 공유 하지만 페이로드 형식이 서로 다른 데이터를 전송 하는 여러 메시지를 다른 구독자가 받을 수 있습니다.

## <a name="subscribing-to-a-message"></a>구독 하는 메시지

구독자 중 하나를 사용 하 여 메시지를 수신 하도록 등록할 수 있습니다는 [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) 오버 로드 합니다. 다음 코드 예제는 방법을 보여 주는 eShopOnContainers 모바일 앱, 구독 처리는 `AddProduct` 메시지:

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

이 예제는 [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) 메서드 구독 하는 `AddProduct` 한 대 한 응답으로 메시지를 받는 콜백 대리자를 실행 합니다. UI를 업데이트 하는 코드를 실행 하는 람다 식으로 지정 된이 콜백 대리자입니다.

> [!TIP]
> 변경할 수 없는 페이로드 데이터를 사용 하는 것이 좋습니다. 여러 스레드에 액세스할 수 받은 데이터 동시에 때문에 콜백 대리자를 내에서 페이로드 데이터를 수정 하지 않음. 이 시나리오에서는 페이로드 데이터는 변경할 수 없어야 동시성 오류를 방지 합니다.

구독자 필요가 없을 수도 게시 된 메시지의 모든 인스턴스를 처리 하 고 제네릭 형식 인수에 지정 된를 제어할 수이 고 [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) 메서드. 이 예제에서는 구독자 받습니다 `AddProduct` 에서 보낸 메시지는 `CatalogViewModel` 페이로드 데이터를 가진 클래스는 `CatalogItem` 인스턴스.

## <a name="unsubscribing-from-a-message"></a>메시지에서 가입 취소

구독자에서 더 이상 수신 하려는 메시지 구독 취소할 수 없습니다. 이 통해 중 하나는 [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) 다음 코드 예제에서와 같이 오버 로드:

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

이 예제는 [ `Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) 수신를 구독할 때는 지정 된 형식 인수를 반영 하는 메서드 구문을 `AddProduct` 메시지입니다.

## <a name="summary"></a>요약

Xamarin.Forms는 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) 클래스 구현 게시-구독 패턴, 개체 및 형식 참조 하 여 연결 편리한 되지 않는 구성 요소 간에 메시지 기반 통신을 허용 합니다. 이 메커니즘에는 게시자와 구독자에 대 한 참조, 구성 요소를 독립적으로 개발 하 고 테스트 가능 하도록 구성 요소 간의 종속성을 줄이는 데 도움이 필요 없이 통신할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [EBook (2mb PDF)를 다운로드 합니다.](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
