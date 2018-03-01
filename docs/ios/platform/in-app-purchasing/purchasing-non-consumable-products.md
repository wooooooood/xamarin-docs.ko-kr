---
title: "에서 사용할 제품 구매"
ms.topic: article
ms.prod: xamarin
ms.assetid: 635D9CA2-6BCA-53E1-7B10-968029AA3493
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f5fbd2282dc2f1d5e9a3fa6d29e7b74fde89fc42
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="purchasing-non-consumable-products"></a>에서 사용할 제품 구매

비-사용 가능한 제품 ' 소유 '는 고객이 합니다. 항상 액세스할 수 있는, 자신의 장치를 분실 또는 도난당 하거나 새를 구입 하는 경우에으로 예상이 됩니다. 설명서, 잡지 문제, 게임 수준, 사진 필터, ' pro 기능 ' 등 유용합니다. 사용자가 아닌 소모품 제품 구입 후 다시 지불할 필요가 있습니다. 실수로 코드를 사용 하면 사용 하도록 해보세요 있습니다, StoreKit 이미 구매 하는 메시지를 표시 됩니다.

## <a name="non-consumable-products-sample"></a>에서 사용할 제품 샘플

[InAppPurchaseSample 코드](https://developer.xamarin.com/samples/monotouch/StoreKit/) 이라는 프로젝트가 들어 *NonConsumables*합니다. 코드 샘플에서 사용할 제품 사진 필터를 사용 하 여 예를 들어를 구현 하는 방법을 보여 줍니다. 필터를 구입한 후 적용할 수 있습니다는 사진에 반복 해 서 다시 합니다. 다시 구매할 필요가 없습니다.   
   
   
   
 구매 프로세스는 이러한 일련의 스크린 샷 –에 나와 **구입** 단추는 기능을 활성화 단추가 됩니다.   
   
   
   
 [ ![](purchasing-non-consumable-products-images/image34.png "구매 프로세스는 이러한 일련의 스크린 샷에 나와")](purchasing-non-consumable-products-images/image34.png)   
   
   
   
 구매 과정 사용 될 제품;와 같습니다. 주요 차이점은 구매 응용 프로그램 코드에서 추적 되는 방식입니다. 이 예제 구입 단추는만 제품 이미 구입 하지 않은 경우에 사용할 수 그렇지 않으면 단추 활성화 기능 자체입니다.   
   
   
   

다음 다이어그램에서는 클래스 및에서 사용할 제품 구매를 수행 하려면 App Store 서버 간의 상호 작용을 보여 줍니다.   
   
   
   
 [ ![](purchasing-non-consumable-products-images/image35.png "클래스 및에서 사용할 제품을 수행 하려면 App Store 서버 간의 상호 작용 구입")](purchasing-non-consumable-products-images/image35.png)   
   
   
   
 사용 될 수 있습니다이 예제에서 가지 주요 차이가 구매 완료 되 면 다시 구매를 방지 하기 위해 사용자 인터페이스 업데이트 됩니다. 성공적인 트랜잭션 알림이 예제에서는 사용자 인터페이스를 업데이트 하는 **구입** 단추 기능 자체를 활성화 하는 단추로 변환 됩니다.

## <a name="re-purchasing-non-consumable-products"></a>다시 사용할 수 없는 제품을 구입

코드 해야 정상적으로 숨기 거 나 제품에 되었습니다 성공적으로 구입 후 사용자가 다시 제품을 구입 하려고 하지 않도록 하려면 구매 단추 용도 다시 설정 됩니다. 샘플 응용 프로그램 변경 하 여 수행 된 **구입** 예제 사진 필터 작동 하는 단추로 단추 합니다.   
   
   
   
 경우에 응용 프로그램에서 사용할 제품 구입 했는지 여부를 알 수 없습니다.

-  응용 프로그램 삭제 한 장치에 다시 설치 된 모든 구매 레코드 없어집니다 (하지 않는 한 / 사용자가 백업 복원 될 때까지). 
-  사용자에 게 응용 프로그램 두 개 이상의 장치에 설치 하 고 구매 하는 장치 중 하나입니다. 다른 장치는 구매에 사용할 수 있는 제품을 표시 하도록 계속 됩니다. 
-  고객을 이러한 상황에서 사용할 제품을 구매 다시 하려고 하는 경우 앱 스토어를 무료로 다시 제품을 충족 합니다. 그러나 사용자 인터페이스 구매를 수행 하려면 처음에 표시 됩니다 (예를 들어 하면 확인 경고가 표시 되 고 Apple ID를 필요한 수) 사용자는 제품을 구입 했습니다 이미 있는지를 알려 주는 메시지가 표시 다음 됩니다.  
   
   
   
 이 시나리오에서 코드 경로 정확 하 게 일반 구매와 동일 하 게, 유일한 차이점은 있습니다.

-  제품에 대 한 다시 사용자를 청구 가져올지 않습니다.
-  `SKPaymentTransaction` 응용 프로그램에 전달 된 개체는 `OriginalTransaction` 처음으로 제품을 구입한 경우 생성 된 트랜잭션을 나타나는 속성입니다. 
-  비-사용 가능한 제품을 판매 하는 응용 프로그램 StoreKit의 구현 해야 **복원** 가 기존 구매를 검색할 수 있도록 하는 기능입니다. 
