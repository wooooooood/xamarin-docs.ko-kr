---
title: 느슨하게 결합된 구성 요소 간 통신
description: '이 장에서 eShopOnContainers 모바일 앱 게시를 구현 하는 방법-구독 패턴을 편리 하 게 개체 및 형식을 참조 하 여 연결 되지 않는 구성 요소 간 메시지 기반 통신을 허용 '
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 9848d2b832990032bc7eb7f2e3a93c896457134c
ms.sourcegitcommit: e95296f9e516975f5f32d822c323a71fd84007b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538691"
---
# <a name="communicating-between-loosely-coupled-components"></a>느슨하게 결합된 구성 요소 간 통신

게시-구독 패턴은 게시자의 구독자 라고 하는 모든 수신기에 대 한 지식을 갖추지 않고도 메시지를 보냅니다는 메시징 패턴입니다. 마찬가지로, 구독자가 게시자에 대 한 지식이 필요 없이 특정 메시지를 수신.

.NET의 이벤트 구현 게시-구독 패턴 및 가장 단순 하 고 간단한 방법은 통신 계층에 대 한 느슨한 컨트롤을 포함 하는 페이지 등 필요 하지 않을 경우 구성 요소 간에 합니다. 그러나 게시자와 구독자 수명을 서로에 대 한 개체 참조 하 여 결합 됩니다 하 고 구독자 유형 게시자 형식에 대 한 참조가 있어야 합니다. 됩니다. 이 만들면 메모리 관리 문제를 정적 또는 수명이 긴 개체의 이벤트를 구독 하는 수명이 짧은 개체가 필요한 경우에 특히 됩니다. 이벤트 처리기를 제거 하지 않으면 구독자는 유지 됩니다 게시자의를 참조 하 여 및이 방지 하 하거나 구독자의 가비지 수집이 지연 됩니다.

## <a name="introduction-to-messagingcenter"></a>MessagingCenter 소개

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) 클래스 구현 게시-구독 패턴을 편리 하 게 개체 및 형식을 참조 하 여 연결 되지 않는 구성 요소 간 메시지 기반 통신을 허용 합니다. 이 메커니즘에는 게시자 및 구독자도 구성 요소를 독립적으로 개발 하 고 테스트를 허용 하는 동안 구성 요소 간의 종속성을 줄이기 위해 서로에 대 한 참조가 필요 없이 전달할 수 있습니다.

합니다 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) 클래스는 멀티 캐스트 게시-구독 기능을 제공 합니다. 즉, 단일 메시지를 게시 하는 여러 게시자 수 동일한 메시지를 수신 대기 하는 등록 자가 여러 명 있을 수 있습니다. 그림 4-1에서는이 관계를 보여 줍니다.

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "멀티 캐스트 게시-구독 기능")

**그림 4-1:** 멀티 캐스트 게시-구독 기능

게시자를 사용 하 여 메시지를 전송 합니다 [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) 메서드를 사용 하 여 메시지를 수신 하는 구독자는 동안는 [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 메서드. 또한 구독자도 구독을 취소할 수 메시지 구독에서 필요한 경우 사용 합니다 [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 메서드.

내부적으로 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) 클래스는 약한 참조를 사용 합니다. 즉, 것을 막지는 개체 활성 상태로 유지 하 고 가비지 수집 하도록 허용 합니다. 따라서만 필요는 클래스는 더 이상 메시지를 수신 하려고 할 때 메시지를 구독 취소 합니다.

EShopOnContainers 모바일 앱에서는 합니다 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) 느슨하게 간에 통신 하는 클래스는 구성 요소를 결합 합니다. 앱 세 개의 메시지를 정의합니다.

-   `AddProduct` 에서 메시지가 게시 되는 `CatalogViewModel` 클래스 항목 시장 바구니에 추가 됩니다. 반환 시는 `BasketViewModel` 클래스는 메시지에 등록 하 고 응답에서 시장 바구니의 항목 수가 증가 합니다. 또한는 `BasketViewModel` 클래스도이 메시지에서 구독 취소 합니다.
-   `Filter` 하 여 메시지가 게시 되는 `CatalogViewModel` 사용자는 카탈로그에서 표시 된 항목에 브랜드 또는 형식 필터를 적용 하는 경우 클래스입니다. 반환 시는 `CatalogView` 클래스는 메시지에 등록 하 고 필터 조건과 일치 하는 항목만 표시 되도록 UI를 업데이트 합니다.
-   `ChangeTab` 에서 메시지가 게시 되는 `MainViewModel` 때 클래스를 `CheckoutViewModel` 이동할는 `MainViewModel` 만들기 성공 및 새 주문 제출 합니다. 를 `MainView` 클래스 등록 메시지 및 UI 업데이트 하므로를 **내 프로필** 탭이 활성화는 사용자가 주문한 제품을 표시 합니다.

> [!NOTE]
> 하지만 합니다 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) 느슨하게 결합 된 클래스 간의 통신을 허용 하는 클래스만 아키텍처이 문제를 해결 하려면이 제공 하지 않습니다. 예를 들어 바인딩 엔진에서 속성 변경 알림을 통해 뷰와 뷰 모델 간의 통신 구현할 수도 있습니다. 또한 두 가지 보기 모델 간의 통신 탐색 하는 동안 데이터를 전달 하 여 가능 합니다.

EShopOnContainers의 모바일 앱에서 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) 다른 클래스에서 발생 하는 작업에 대 한 응답에서 UI를 업데이트 하는 데 사용 됩니다. 따라서 메시지는 UI 스레드에서 동일한 스레드에서 메시지를 받는 구독자를 사용 하 여 게시 됩니다.

> [!TIP]
> 업데이트 UI를 수행 하는 경우 UI 스레드로 마샬링하십시오. UI를 업데이트 하는 데 필요한 백그라운드 스레드에서 전송 되는 메시지의 경우 호출 하 여 구독자에서 UI 스레드에서 메시지를 처리 합니다 [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) 메서드.

에 대 한 자세한 내용은 [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter)를 참조 하십시오 [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)합니다.

## <a name="defining-a-message"></a>메시지 정의

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 메시지는 메시지를 식별 하는 데 사용 되는 문자열입니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱 내에 정의 된 메시지를 보여 줍니다.

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

이 예제에서는 상수를 사용 하 여 메시지가 정의 됩니다. 이 방식의 장점은 컴파일 타임 형식 안전성 및 리팩터링 지원을 제공 한다는 점입니다.

## <a name="publishing-a-message"></a>메시지 게시

게시자 알림 구독자 중 하나를 사용 하 여 메시지를 [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) 오버 로드 합니다. 다음 코드 예제에서는 게시 된 `AddProduct` 메시지:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

이 예제에서는 합니다 [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) 메서드 3 개 인수를 지정 합니다.

-   첫 번째 인수는 보낸 사람 클래스를 지정 합니다. 보낸 사람 클래스는 메시지를 수신 하려는 모든 구독자가 지정 되어야 합니다.
-   두 번째 인수는 메시지를 지정 합니다.
-   세 번째 인수는 페이로드 데이터를 구독자에 게 보낼 수를 지정 합니다. 이 예제의 경우 페이로드 데이터는 `CatalogItem` 인스턴스.

합니다 [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) 메서드는 메시지 및 실행 후 제거 방식을 사용 하 여 페이로드 데이터를 게시 합니다. 따라서 메시지는 메시지를 수신 하도록 등록 된 구독자가 없는 경우에 전송 됩니다. 이 경우 전송된 된 메시지는 무시 됩니다.

> [!NOTE]
> 합니다 [ `MessagingCenter.Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) 메서드가 메시지 전달 되는 방식을 제어 하는 데 제네릭 매개 변수를 사용할 수 있습니다. 따라서 다른 구독자가 메시지 id를 공유 하지만 서로 다른 페이로드 데이터 유형을 전송 하는 여러 메시지를 받을 수 있습니다.

## <a name="subscribing-to-a-message"></a>메시지에 등록

구독자 중 하나를 사용 하 여 메시지를 받도록 등록할 수 있습니다 합니다 [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 오버 로드 합니다. 다음 코드 예제에서는 eShopOnContainers 모바일 앱, 구독 및 처리 하는 방법을 보여 줍니다는 `AddProduct` 메시지:

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

이 예제에서는 합니다 [ `Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 메서드를 구독할는 `AddProduct` 메시지 및 응답 메시지를 수신 하는 콜백 대리자를 실행 합니다. UI를 업데이트 하는 코드를 실행 하는이 콜백 대리자, 람다 식으로 지정 합니다.

> [!TIP]
> 변경할 수 없는 페이로드 데이터를 사용 하는 것이 좋습니다. 여러 스레드에 액세스할 수 받은 데이터를 동시에 때문에 콜백 대리자 내에서 페이로드 데이터를 수정 하지 마세요. 이 시나리오에서는 페이로드 데이터는 변경할 수 없어야 동시성 오류가 발생 하지 않도록 합니다.

구독자는 게시 된 메시지의 모든 인스턴스를 처리 하지 않아도 될 수 고에 지정 된 제네릭 형식 인수를 제어할 수 있습니다 합니다 [ `Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 메서드. 이 예제에서는 구독자만 받게 됩니다 `AddProduct` 에서 전송 되는 메시지를 `CatalogViewModel` 페이로드 데이터를 가진 클래스를 `CatalogItem` 인스턴스.

## <a name="unsubscribing-from-a-message"></a>메시지에서 구독 취소

구독자는 더 이상 수신 하려는 메시지에서 구독 취소할 수 없습니다. 중 하나를 사용 하 여 이렇게 합니다 [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 다음 코드 예제에서와 같이 오버 로드 합니다.

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

이 예제에서는 합니다 [ `Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 메서드 구문을 반영 수신 하도록 구독할 때 지정 된 형식 인수는 `AddProduct` 메시지.

## <a name="summary"></a>요약

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) 클래스 구현 게시-구독 패턴을 편리 하 게 개체 및 형식을 참조 하 여 연결 되지 않는 구성 요소 간 메시지 기반 통신을 허용 합니다. 이 메커니즘에는 게시자 및 구독자도 구성 요소를 독립적으로 개발 하 고 테스트를 허용 하는 동안 구성 요소 간의 종속성을 줄이기 위해 서로에 대 한 참조가 필요 없이 전달할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [2mb PDF 전자책 다운로드](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (샘플)](https://github.com/dotnet-architecture/eShopOnContainers)
