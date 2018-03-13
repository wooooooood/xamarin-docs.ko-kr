---
title: "구독 및 보고"
ms.topic: article
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 237a986d6db2fb6984e99c6265fbbc212b35a351
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="subscriptions-and-reporting"></a>구독 및 보고

## <a name="about-non-renewing-subscriptions"></a>비-갱신 구독에 대 한

서비스에 대 한 제한 시간 (1 주일에 대 한 액세스 탐색 응용 프로그램) 또는 데이터 보관에 대 한 시간이 제한 된 액세스 등의 판매를 나타내는 제품에 대 한 비 갱신 구독 사용 됩니다.   
   
   
   
 비 갱신 구독 및 기타 제품 형식 간의 주요 차이점:

-  ITunes Connect에서에서 제품 정의 용어를 포함 하지 않습니다. 응용 프로그램 코드는 id입니다. 제품의 유효 기간을 유추할 수 있어야 합니다. 
-  여러 번 (예: 사용 될 제품) 구입할 수 있습니다. 응용 프로그램은 구독 용어/만료 및 갱신을 관리 하 고 사용자 겹치는 구독 구입 하는 것을 금지 해야 합니다. 
-  구매 내용이 StoreKit 복원 함수에서 지원 되지 않습니다. 모든 사용자의 장치에서 구독을 사용할 수 있어야 하는 경우 응용 프로그램 디자인 하 고 원격 서버와 함께에서이 기능을 구현 갖습니다. 응용 프로그램은 또한 백업 사례에 대 한 구독 상태는 장치를 백업 하는 경우 다음 복원--백업에서 합니다. 
-  구현 개요
-  서버-배달 된 워크플로 사용 하 고 소모품 처럼 관리할 구독 비 갱신 일반적으로 구현 되어야 합니다. 


## <a name="about-free-subscriptions"></a>무료 구독에 대 한

무료 구독을 뉴스 스탠드 앱 (사용할 수 없습니다 뉴스 스탠드 아닌 앱에서)에 사용 가능한 콘텐츠를 저장할 수 있습니다. 무료 구독을 시작 하는 모든 사용자의 장치에서 사용할 수 있습니다. 무료 구독은 만료 되지 않도록 합니다. 이러한 응용 프로그램을 제거할 때만 종료 합니다.

### <a name="implementation-overview"></a>구현 개요

무료 구독 자동 갱신 가능한 구독 처럼 훨씬 작동합니다. 응용 프로그램에는 iTunes Connect에서에서 무료 구독 제품 '구입'에 사용할 수 있어야 합니다. 사용자가을 구입 하는 경우 자동 갱신 가능한 구독 제품 같은 무료 구독 구매 유효성을 검사 해야 합니다. 무료 구독 트랜잭션은 복원할 수 있습니다.


## <a name="about-auto-renewable-subscriptions"></a>자동 갱신 가능한 구독에 대 한

자동 갱신 가능한 구독은 뉴스 스탠드 응용 프로그램에서 주로 사용 됩니다. 지정된 된 기간 동안 iTunes Connect (7 일에서 1 년까지 범위의 지속 시간 설정)에 구성 된 시간에 대 한 동적 내용에 대 한 사용자 액세스 권한을 부여 하는 제품을 나타냅니다. 구독은 사용자 opts 아웃 하지 않는 한 각 구독 기간이 끝날 때 사용자의 Apple ID를 충전 자동으로 갱신 합니다. 이 유형의 사용자가 자신의 구독 유효한 동안 게시 된 각 문제에 대 한 액세스를 가져오는 위치 잡지 또는 뉴스 구독에 적합 합니다.

### <a name="implementation-overview"></a>구현 개요

Server-Delivered 제품 워크플로 사용 하 여 자동 갱신 가능한 구독을 구현 (참조는 *수신 확인 및 Server-Delivered 제품* 섹션).

#### <a name="shared-secret"></a>공유 암호

에 응용 프로그램 구매 공유 암호는 서버에서 자동 갱신 가능한 구독을 확인 했을 때 JSON 요청에 사용 합니다. 공유 암호는 iTunes Connect 통해 만든/액세스 합니다.

ITunes Connect 홈 페이지 선택에서에서 **My Apps**:   
   
 [![](subscriptions-and-reporting-images/image2.png "내 앱 선택")](subscriptions-and-reporting-images/image2.png#lightbox)  
 
응용 프로그램을 선택 하 고 클릭는 **앱에서 바로 구매** 탭:

[![](subscriptions-and-reporting-images/image6.png "내 앱에서 바로 구매 탭을 클릭")](subscriptions-and-reporting-images/image6.png#lightbox)

페이지 아래쪽에서 선택 **보기는 공유 암호 생성 또는**:
   
 [![](subscriptions-and-reporting-images/image40.png "보기를 선택 하거나 공유 암호를 생성 합니다.")](subscriptions-and-reporting-images/image40.png#lightbox)

 [![](subscriptions-and-reporting-images/image41.png "공유 암호를 생성 합니다.")](subscriptions-and-reporting-images/image41.png#lightbox)   
   
   
   
 공유 암호를 사용 하려면 다음과 같이 자동 갱신 가능한 구독에는 앱에서 바로 구매 확인 메일 유효성을 검사할 때 Apple 서버에 전송 된 JSON 페이로드에서 포함:

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

응답의 상태 필드는 구매와 다른 제품 형식으로 유효한 경우 0이 됩니다.

#### <a name="downloading-items-after-the-initial-subscription-term"></a>초기 구독 기간 후 항목을 다운로드합니다.

제품을 구독 제공의 일환으로, 코드 Apple의 서버에 대해 알려진된 최신 수신을 자주 확인 해야 합니다. 구독이 자동-갱신 이후 마지막 확인 하는 경우 JSON 응답 응용 프로그램에 발생 한 트랜잭션의 추가 필드에 포함 됩니다 (확장 하는 구독 유효성). JSON 응답 포함 됩니다.

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

상태 0 이면 구독이 여전히 유효 하 고 다른 필드는 유효한 데이터를 보관 합니다. 21006 상태인 경우에 구독이 만료 되었습니다. 참조는 [는 자동 갱신 가능한 구독 수신 확인](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) 기타 오류 코드에 대 한 설명서입니다.

#### <a name="restoring-auto-renewable-subscriptions"></a>자동 갱신 가능한 구독 복원

여러 개의 트랜잭션이 원래 구매 트랜잭션 및 각 구독을 갱신 하는 기간에 대 한 별도 트랜잭션을 다시 있게 됩니다. 유효 기간이 내용을 이해 하려면 시작 날짜 및 용어를 추적 해야 합니다.   
   
   
   
 SKPaymentTransaction 개체에는 구독 기간-각 용어에 대 한 다른 제품 ID를 사용 하 고 트랜잭션 구매 날짜에서 구독 기간을 추정 하는 코드를 작성 해야 합니다.

#### <a name="testing-auto-renewal"></a>자동 갱신 테스트

구독 테스트 좀 더 쉽게 기간 샌드박스에 테스트할 때 압축 됩니다. 1 주 구독 1 년 구독 갱신 1 시간 마다 3 분 마다 갱신 합니다. 구독은 최대 6 회 샌드박스에 테스트 하는 동안 자동 갱신 됩니다.

## <a name="reporting"></a>보고

iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com))를 제공 합니다.   
   
 **판매 및 추세** -다운로드, 업데이트 및 앱에서 바로 구매 응용 프로그램의 세부 정보를 표시 합니다.   
   
 **지급액 및 재무 보고서** – 작업을 하는 사용자 응용 프로그램과 하 고 사용 되는 정도에 적용 된 목록 지불 소득에 자세히 설명 합니다.

판매 및 추세 보고서 예제는 다음과 같습니다.   

 [![](subscriptions-and-reporting-images/image42.png "판매 및 추세 보고서 예제")](subscriptions-and-reporting-images/image42.png#lightbox)   
   
 또한는 [ **체 연결 모바일**iOS 앱 (iTunes 링크)](http://itunes.apple.com/us/app/itunes-connect-mobile/id376771144?mt=8)합니다.
일부 제공 되는 통계에 대 한 iPhone 스크린샷은 다음과 같습니다.   
   
 [![](subscriptions-and-reporting-images/image43.png "일부 제공 되는 통계에 대 한 iPhone 스크린 샷")](subscriptions-and-reporting-images/image43.png#lightbox)
