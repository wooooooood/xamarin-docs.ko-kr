---
title: 구독 및 Xamarin.iOS에서 보고
description: 이 문서에는 갱신 되지 않은 구독, 무료 구독, 자동 갱신 구독 및 이러한 항목에 보고서를 iTunes Connect를 사용 하 여 설명 합니다.
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4e63894cb862db3b5b5a1e7a2bebd79160c311a9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61367495"
---
# <a name="subscriptions-and-reporting-in-xamarinios"></a>구독 및 Xamarin.iOS에서 보고

## <a name="about-non-renewing-subscriptions"></a>비-구독 갱신에 대 한

비 갱신 구독 제품에 대 한 제한 시간 (1 주일 탐색 응용 프로그램에 액세스) 등 데이터 보관에 대 한 시간 제한 액세스를 사용 하 여 서비스의 판매를 나타내는 위해 사용 됩니다.   
   
비 갱신 구독 및 다른 제품 유형 간의 주요 차이점:

-  ITunes Connect에서에서 제품 정의 용어를 포함 되지 않습니다. 응용 프로그램 코드는 id입니다. 제품의 유효 기간을 유추할 수 있어야 합니다. 
-  이러한 여러 번 (예: 사용할 수 있는 제품)을 구입할 수 있습니다. 응용 프로그램 구독 용어/만료 및 갱신을 관리 하 고 겹치는 구독 구입에서 사용자를 방지 해야 합니다. 
-  구매는 StoreKit 복원 함수에서 지원 되지 않습니다. 모든 사용자의 장치에서 구독을 사용할 수 있어야 하는 경우 응용 프로그램을 디자인 하 고 원격 서버와 함께에서이 기능을 구현 해야 합니다. 응용 프로그램은 또한 백업 사례에 대 한 구독 상태는 장치를 백업 하는 경우 다음 복원에서-백업 합니다. 
-  구현 개요
-  서버 배달 워크플로 사용 하 고 소모 성 제품 같은 관리 되는 비 갱신 구독 일반적으로 구현 되어야 합니다. 


## <a name="about-free-subscriptions"></a>무료 구독에 대 한

무료 구독을 Newsstand 앱 (사용할 수 없습니다 Newsstand이 아닌 앱에서)에 사용 가능한 콘텐츠를 저장할 수 있습니다. 무료 구독 시작 되 면 모든 사용자 장치에서 제공 됩니다. 사용 가능한 구독이 만료 되지 않도록 합니다. 이러한 응용 프로그램을 제거할 때만 종료 합니다.

### <a name="implementation-overview"></a>구현 개요

무료 구독을 자동 갱신 구독 처럼 훨씬 작동합니다. ITunes Connect에서에서 응용 프로그램에 '구매'에 사용할 수 있는 무료 구독 제품이 있어야 합니다. 사용자가 구매를 하는 경우 자동 갱신 구독 제품을 같은 무료 구독 구매 유효성을 검사 해야 합니다. 무료 구독 트랜잭션은 복원할 수 있습니다.


## <a name="about-auto-renewable-subscriptions"></a>자동 갱신 구독에 대 한

자동 갱신 구독 Newsstand 응용 프로그램에서 주로 사용 됩니다. ITunes Connect (7 일에서 1 년 까지의 기간을 설정 합니다.)에 구성 된 경우 지정 된 기간에 대 한 동적 콘텐츠에 대 한 사용자 액세스를 부여 하는 제품을 나타냅니다. 구독은 사용자 opts 옵트아웃 하지 않는 한 각 구독 기간의 끝에는 사용자의 Apple ID를 충전 자동으로 갱신 합니다. 이 제품 유형에 적합 잡지 또는 뉴스 구독 사용자 자신의 구독이 유효 하는 동안 게시 된 각 문제에 대 한 액세스를 가져오는 위치 합니다.

### <a name="implementation-overview"></a>구현 개요

자동 갱신 구독 Server-Delivered 제품 워크플로 사용 하 여 구현 되어야 합니다 (참조를 *수신 확인 및 Server-Delivered 제품* 섹션).

#### <a name="shared-secret"></a>공유 암호

앱 내 구매 공유 암호는 서버에서 자동 갱신 구독을 확인 하는 경우 JSON 요청에 사용 되어야 합니다. 공유 암호는 iTunes Connect 통해 만든/액세스 합니다.

ITunes Connect 홈 페이지 선택에서에서 **My Apps**:   
   
 [![](subscriptions-and-reporting-images/image2.png "내 앱 선택")](subscriptions-and-reporting-images/image2.png#lightbox)  
 
응용 프로그램을 선택 하 고 클릭 합니다 **앱에서 바로 구매** 탭:

[![](subscriptions-and-reporting-images/image6.png "앱 내 구매 탭 클릭")](subscriptions-and-reporting-images/image6.png#lightbox)

페이지 맨 아래에서 선택 **보기는 공유 암호 생성 또는**:
   
 [![](subscriptions-and-reporting-images/image40.png "뷰를 선택 하거나 공유 암호를 생성 합니다.")](subscriptions-and-reporting-images/image40.png#lightbox)

 [![](subscriptions-and-reporting-images/image41.png "공유 암호를 생성 합니다.")](subscriptions-and-reporting-images/image41.png#lightbox)   
   
   
   
 공유 암호를 사용 하려면 다음과 같은 자동 갱신 구독에 대해는 인 앱 구매 확인 메일의 유효성을 검사 하는 경우 Apple의 서버로 전송 되는 JSON 페이로드에 포함:

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

응답의 상태 필드는 구매 다른 제품 형식으로 유효한 경우 0이 됩니다.

#### <a name="downloading-items-after-the-initial-subscription-term"></a>초기 구독 기간 후 항목을 다운로드합니다.

구독 제품을 제공의 일부로, 코드를 Apple의 서버에 대해 알려진된 최신 수신을 자주 확인 해야 합니다. 구독에 자동으로 갱신 마지막 확인 이후, 하는 경우 JSON 응답을 발생 한 트랜잭션 응용 프로그램을 알리는 추가 필드에 포함 됩니다 (구독 유효성을 검사 하는 확장 해야) 합니다. JSON 응답에 포함 됩니다.

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

상태가 0 이면 구독이 여전히 유효 하 고 다른 필드에 유효한 데이터가 있습니다. 21006 상태인 경우에 구독이 만료 되었습니다. 참조 된 [는 자동 갱신 구독 수신 확인](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) 다른 오류 코드에 대 한 설명서입니다.

#### <a name="restoring-auto-renewable-subscriptions"></a>자동 갱신 구독 복원

여러 트랜잭션-원래 구입 거래가 plus 구독을 갱신할 각 기간에 대 한 별도 트랜잭션을 다시 표시 됩니다. 시작 날짜 및 용어 유효 기간이 무엇 인지 파악할 수를 추적 해야 합니다.   
   
   
   
 SKPaymentTransaction 개체 구독 기간을 다루지 않습니다 – 각 용어에 대 한 다른 제품 ID를 사용 하 고 트랜잭션 구매 날짜 로부터 구독 기간을 추정 하는 코드를 작성 해야 합니다.

#### <a name="testing-auto-renewal"></a>자동 갱신 테스트

구독 테스트 좀 더 쉽게 기간 샌드박스에서 테스트할 때 압축 됩니다. 1 주 1 년 구독 갱신 매시간 3 분 마다 구독을 갱신 합니다. 구독이 최대 6 번 샌드박스에서 테스트 하는 동안 자동 갱신 됩니다.

## <a name="reporting"></a>보고

iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com))를 제공 합니다.   
   
 **판매 및 추세** – 다운로드, 업데이트 및 앱 내 구매 앱의 세부 정보를 표시 합니다.   
   
 **지급액 및 재무 보고서** – 목록 지불 하 고 징수할 얼마나 변경한 뿐만 아니라 사용자 앱에서 획득 한 수입을 자세히 설명 합니다.

판매 및 추세 보고서 예제는 다음과 같습니다.   

 [![](subscriptions-and-reporting-images/image42.png "판매 및 추세 보고서 예제")](subscriptions-and-reporting-images/image42.png#lightbox)   
   
 또한는 [ **체 연결 Mobile**iOS 앱 (iTunes 링크)](http://itunes.apple.com/us/app/itunes-connect-mobile/id376771144?mt=8)합니다.
iPhone 스크린샷 제공 되는 통계 중 일부는 다음과 같습니다.   
   
 [![](subscriptions-and-reporting-images/image43.png "제공 되는 통계의 일부에 대 한 iPhone 스크린 샷")](subscriptions-and-reporting-images/image43.png#lightbox)
