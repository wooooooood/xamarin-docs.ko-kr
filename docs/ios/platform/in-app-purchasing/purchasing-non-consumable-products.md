---
title: Xamarin.ios에서 사용할 때 제품이 아닌 제품 구매
description: 이 문서에서는 Xamarin.ios에서 사용할 수 없는 제품에 대해 설명 합니다 .이는 장치에 관계 없이 무기한으로 사용할 수 있는 사용자가 구매한 기능입니다.
ms.prod: xamarin
ms.assetid: 635D9CA2-6BCA-53E1-7B10-968029AA3493
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: aa478636b4ab94ab000fd98860646bfa300e9fab
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291322"
---
# <a name="purchasing-non-consumable-products-in-xamarinios"></a>Xamarin.ios에서 사용할 때 제품이 아닌 제품 구매

사용할 수 없는 제품은 고객에 의해 ' 소유 ' 됩니다. 장치를 분실/도난당 하거나 새로운 장치를 구입 하는 경우에도 항상 액세스할 수 있습니다. 책, 잡지 문제, 게임 수준, 사진 필터, ' pro 기능 ' 등에 유용 합니다. 사용자가 사용할 수 없는 제품을 구매한 후에는 다시 요금을 지불할 필요가 없습니다. 코드에서 실수로 시도 하는 경우에는 사용자가 이미 구매한 메시지를 표시 합니다.

## <a name="non-consumable-products-sample"></a>사용할 없는 제품 샘플

[InAppPurchaseSample 코드](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit) 에는 *비 소비*라는 프로젝트가 포함 되어 있습니다. 코드 샘플에서는 사진 필터를 사용 하 여 사용할 수 없는 제품을 구현 하는 방법을 보여 줍니다. 필터를 구매한 후에는 사진에 다시 적용할 수 있습니다. 다시 구매할 필요가 없습니다.   
   
   
   
 구매 프로세스는이 일련의 스크린샷에 표시 됩니다. **구입** 단추는 기능 활성화 단추가 됩니다.   
   
   
   
 [![](purchasing-non-consumable-products-images/image34.png "구매 프로세스는이 일련의 스크린샷에 표시 됩니다.")](purchasing-non-consumable-products-images/image34.png#lightbox)   
   
   
   
 구매 프로세스는 소비재 제품과 동일 합니다. 주요 차이점은 응용 프로그램 코드에서 구매가 추적 되는 방법에 있습니다. 이 예에서는 제품을 아직 구매 하지 않은 경우에만 구입 단추를 사용할 수 있습니다. 그렇지 않으면 단추는 기능 자체를 활성화 합니다.   
   
   
   

다음 다이어그램에서는 사용할 수 없는 제품 구매를 수행 하기 위해 클래스와 App Store 서버 간의 상호 작용을 보여 줍니다.   
   
   
   
 [![](purchasing-non-consumable-products-images/image35.png "사용할 수 없는 제품 구매를 수행 하기 위한 클래스와 앱 스토어 서버 간의 상호 작용")](purchasing-non-consumable-products-images/image35.png#lightbox)   
   
   
   
 사용 가능한 예제와의 주요 차이점은 구매가 완료 되 면 사용자 인터페이스를 업데이트 하 여 다시 구매 하지 않도록 하는 것입니다. 이 예에서 성공한 트랜잭션 알림은 사용자 인터페이스를 업데이트 하 여 **구매** 단추가 기능 자체를 활성화 하는 단추로 변환 되도록 합니다.

## <a name="re-purchasing-non-consumable-products"></a>사용할 때 사용할 없는 제품 다시 구입

제품이 성공적으로 구매 된 후 사용자가 제품을 다시 구매 하지 못하도록 하기 위해 코드는 일반적으로 구매 단추를 숨기 거 나 용도 지정 해야 합니다. 샘플 응용 프로그램은이 작업을 수행 합니다 .이 작업은 예제 사진 필터를 사용 하는 단추로 **구매** 단추를 변경 하는 것입니다.   
   
   
   
 응용 프로그램에서 사용할 수 없는 제품이 이미 구매 되었는지 여부를 확인할 수 없는 경우가 있습니다.

- 장치에 응용 프로그램을 삭제 하 고 다시 설치 하는 경우 모든 구매 레코드는 삭제 됩니다 (사용자가 백업 복원을 수행 하는 경우 제외). 
- 사용자가 두 개 이상의 장치에 응용 프로그램을 설치 하 고 장치 중 하나에서 구매를 수행 하는 경우 다른 장치는 구매할 수 있는 제품을 계속 표시 합니다. 
- 고객이 이러한 상황에서 사용할 수 없는 제품을 다시 구입 하려고 하면 앱 스토어에서 제품을 무료로 다시 제공 합니다. 사용자 인터페이스는 처음에 구매를 수행 하는 것으로 표시 됩니다. 예를 들어 확인 경고가 표시 되 고 Apple ID가 필요 합니다. 그러나 사용자는 제품이 이미 구매 되었음을 알리는 메시지를 표시 합니다.  
   
   
   
 이 시나리오에서 코드 경로는 정상적인 구매와 동일 합니다. 유일한 차이점은 다음과 같습니다.

- 사용자가 제품에 대 한 요금이 다시 청구 되지 않습니다.
- 응용 `SKPaymentTransaction` 프로그램에 전달 되는 개체에는 `OriginalTransaction` 제품을 처음 구입할 때 생성 된 트랜잭션을 참조 하는 속성이 있습니다. 
- 지원 되지 않는 제품을 판매 하는 응용 프로그램은 사용자가 기존 구매를 검색할 수 있도록 지 수 키트의 **복원** 기능을 구현 해야 합니다. 
