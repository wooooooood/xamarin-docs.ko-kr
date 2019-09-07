---
title: Xamarin.ios의 구독 및 보고
description: 이 문서에서는 갱신 되지 않은 구독, 무료 구독, 자동 갱신 가능 구독, iTunes Connect를 사용 하 여 이러한 항목에 대해 보고 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 81e8f5c1beafeaafcf0d5dcbcc3bf4d66ee05a66
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70752671"
---
# <a name="subscriptions-and-reporting-in-xamarinios"></a>Xamarin.ios의 구독 및 보고

## <a name="about-non-renewing-subscriptions"></a>갱신 되지 않는 구독 정보

갱신 되지 않은 구독은 시간 제한이 있는 서비스의 판매를 나타내는 제품을 위한 것입니다 (예: 탐색 응용 프로그램에 대 한 1 주 또는 데이터 보관에 대 한 시간 제한 액세스).   
   
갱신 되지 않는 구독과 기타 제품 유형 간의 주요 차이점은 다음과 같습니다.

- ITunes Connect의 제품 정의는 용어를 포함 하지 않습니다. 응용 프로그램 코드는 제품 ID에서 유효 기간을 유추할 수 있어야 합니다. 
- 여러 번 구입할 수 있습니다 (예: 소비재 제품). 응용 프로그램은 구독 용어/만료 및 갱신을 관리 하 고 사용자가 겹치는 구독을 구입 하는 것을 방지 하는 데 필요 합니다. 
- 지원 되지 않는 제품은 복원 기능을 지원 합니다. 모든 사용자의 장치에서 구독을 사용할 수 있는 경우 응용 프로그램은 원격 서버와 함께이 기능을 설계 하 고 구현 해야 합니다. 또한 응용 프로그램은 백업이 백업 된 후 백업에서 복원 되는 경우에 대 한 구독 상태를 백업 하는 작업을 담당 합니다. 
- 구현 개요
- 갱신 되지 않은 구독은 일반적으로 서버에서 제공 하는 워크플로를 사용 하 여 구현 되 고 사용 가능한 제품과 같이 관리 됩니다. 

## <a name="about-free-subscriptions"></a>무료 구독 정보

개발자는 무료 구독을 통해 Newsstand apps에 무료 콘텐츠를 배치할 수 있습니다 (Newsstand 않는 앱에서 사용할 수 없음). 무료 구독을 시작한 후에는 모든 사용자의 장치에서 사용할 수 있습니다. 무료 구독은 만료 되지 않습니다. 응용 프로그램이 제거 될 때만 종료 됩니다.

### <a name="implementation-overview"></a>구현 개요

무료 구독은 자동 갱신 가능 구독과 매우 유사 하 게 작동 합니다. 응용 프로그램에는 iTunes Connect에서 ' 구매 '에 사용할 수 있는 무료 구독 제품이 있어야 합니다. 사용자가 구매할 때 무료 구독 구입은 자동 갱신 가능 구독 제품과 같이 유효성을 검사 해야 합니다. 무료 구독 트랜잭션을 복원할 수 있습니다.

## <a name="about-auto-renewable-subscriptions"></a>자동 갱신 가능한 구독 정보

자동 갱신 가능한 구독은 Newsstand 응용 프로그램에서 주로 사용 됩니다. 이는 iTunes Connect에서 구성 된 지정 된 기간 동안 동적 콘텐츠에 대 한 액세스 권한을 사용자에 게 부여 하는 제품을 나타냅니다 (7 일에서 1 년으로 범위가 지정 된 기간 설정). 구독은 자동으로 갱신 되어 사용자가 opts 하지 않는 한 각 구독 기간이 끝날 때 사용자 Apple ID를 청구 합니다. 이 제품 유형은 사용자가 구독이 유효한 동안 게시 된 각 문제에 대 한 액세스 권한을 가진 잡지 또는 news 구독에 적합 합니다.

### <a name="implementation-overview"></a>구현 개요

자동 갱신 가능한 구독은 서버에서 제공 하는 제품 워크플로를 사용 하 여 구현 해야 합니다 ( *수신 확인 및 서버에서 제공* 하는 제품 섹션 참조).

#### <a name="shared-secret"></a>공유 암호

서버에서 자동 갱신 가능한 구독을 확인할 때 JSON 요청에서 앱 내 구매 공유 암호를 사용 해야 합니다. ITunes Connect를 통해 공유 암호가 생성/액세스 됩니다.

ITunes Connect 홈 페이지에서 **내 앱**을 선택 합니다.   
   
 [![](subscriptions-and-reporting-images/image2.png "내 앱 선택")](subscriptions-and-reporting-images/image2.png#lightbox)  

응용 프로그램을 선택 하 고 **앱에서 바로 구매** 탭을 클릭 합니다.

[![](subscriptions-and-reporting-images/image6.png "앱에서 바로 구매 탭을 클릭 합니다.")](subscriptions-and-reporting-images/image6.png#lightbox)

페이지 맨 아래에서 **공유 암호 보기 또는 생성**을 선택 합니다.
   
 [![](subscriptions-and-reporting-images/image40.png "공유 암호 보기 또는 생성을 선택 합니다.")](subscriptions-and-reporting-images/image40.png#lightbox)

 [![](subscriptions-and-reporting-images/image41.png "공유 암호 생성")](subscriptions-and-reporting-images/image41.png#lightbox)   

공유 암호를 사용 하려면 다음과 같이 자동 갱신 가능한 구독에 대 한 앱 내 구매 수령의 유효성을 검사할 때 Apple 서버에 전송 되는 JSON 페이로드에 해당 암호를 포함 합니다.

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

다른 제품 유형과 마찬가지로 구매가 유효한 경우 응답의 상태 필드는 0이 됩니다.

#### <a name="downloading-items-after-the-initial-subscription-term"></a>초기 구독 기간 후 항목 다운로드

구독 제품을 제공 하는 과정에서 코드는 Apple 서버에 대해 알려진 최신 확인을 자주 확인 해야 합니다. 마지막 확인 이후 구독에 자동 갱신 된 경우 JSON 응답에는 발생 한 트랜잭션을 응용 프로그램에 알리는 추가 필드 (구독 유효성을 연장 해야 함)가 포함 됩니다. JSON 응답에는 다음이 포함 됩니다.

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

상태가 0 이면 구독은 여전히 유효 하 고 다른 필드는 유효한 데이터를 보유 합니다. 상태가 21006 이면 구독이 만료 된 것입니다. 다른 오류 코드는 [자동 갱신 가능한 구독](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) 확인 설명서를 참조 하세요.

#### <a name="restoring-auto-renewable-subscriptions"></a>자동 갱신 가능한 구독 복원

원본 구매 트랜잭션과 구독이 갱신 된 각 기간에 대 한 별도의 트랜잭션으로 여러 트랜잭션을 다시 받게 됩니다. 유효 기간을 이해 하려면 시작 날짜와 조건을 추적 해야 합니다.   

SKPaymentTransaction 개체는 구독 기간을 포함 하지 않습니다. 각 용어에 대해 다른 제품 ID를 사용 하 고 트랜잭션 구매 날짜 로부터 구독 기간을 예측할 수 있는 코드를 작성 해야 합니다.

#### <a name="testing-auto-renewal"></a>자동 갱신 테스트

구독을 쉽게 테스트 하려면 샌드박스에서 테스트할 때 해당 기간이 압축 됩니다. 1 주 구독은 3 분 마다 갱신 되 고 1 년 구독은 1 시간 마다 갱신 됩니다. 구독은 샌드박스에서 테스트 하는 동안 최대 6 번 자동 갱신 됩니다.

## <a name="reporting"></a>보고

iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com))는 다음을 제공 합니다.   
   
 **판매 및 추세** – 앱 다운로드, 업데이트 및 앱에서의 구매에 대 한 세부 정보를 표시 합니다.   
   
 **지불 및 재무 보고서** – 앱의 수입을 자세히 설명 하 고 사용자에 게 발생 한 지불액과 빚 정도를 나열 합니다.

판매 및 추세 보고서의 예는 다음과 같습니다.   

 [![](subscriptions-and-reporting-images/image42.png "예: 판매 및 추세 보고서")](subscriptions-and-reporting-images/image42.png#lightbox)   
   
 또한 [ **ITC 연결 모바일**IOS 앱 (iTunes 링크)](http://itunes.apple.com/us/app/itunes-connect-mobile/id376771144?mt=8)이 있습니다.
사용할 수 있는 통계 중 일부에 대 한 iPhone 스크린샷는 다음과 같습니다.   
   
 [![](subscriptions-and-reporting-images/image43.png "사용할 수 있는 통계 중 일부에 대 한 iPhone 스크린샷")](subscriptions-and-reporting-images/image43.png#lightbox)
