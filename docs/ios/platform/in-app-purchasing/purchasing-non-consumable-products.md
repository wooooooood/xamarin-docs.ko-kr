---
title: Xamarin.iOS에서 영구 제품 구매
description: 이 문서에서는 기능은 사용자가 구매한 장치에 관계 없이 무제한으로 사용 가능한 상태로 유지 하는 Xamarin.iOS에서 영구 제품을 설명 합니다.
ms.prod: xamarin
ms.assetid: 635D9CA2-6BCA-53E1-7B10-968029AA3493
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 060403baf8ac28b9b160632a01471b9828735069
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61403220"
---
# <a name="purchasing-non-consumable-products-in-xamarinios"></a>Xamarin.iOS에서 영구 제품 구매

비-사용할 수 있는 제품 ''가 소유한 고객입니다. 예상 되는 항상 해야 함을에 액세스할 수는 해당 장치를 분실/도난당 하거나 새로 구입 하는 경우에 합니다. 책, 잡지, 게임 수준, 사진 필터, ' pro 기능 ' 등 유용합니다. 사용자 영구 제품을 구매한 후 다시에 대 한 지불 필요가 있습니다. 코드에서는 시도 실수로 수, StoreKit 이미 구입 하는 메시지를 표시 됩니다.

## <a name="non-consumable-products-sample"></a>영구 제품 샘플

합니다 [InAppPurchaseSample 코드](https://developer.xamarin.com/samples/monotouch/StoreKit/) 라는 프로젝트가 포함 되어 *NonConsumables*합니다. 코드 샘플에는 예를 들어 사진 필터를 사용 하 여 영구 제품을 구현 하는 방법을 보여 줍니다. 필터를 구입한 후 적용할 수 있습니다이 사진에 반복 해 서. 다시 구매할 필요가 없습니다.   
   
   
   
 구매 프로세스 스크린샷-이 시리즈에 표시 됩니다는 **구입** 버튼을 기능 활성화 단추:   
   
   
   
 [![](purchasing-non-consumable-products-images/image34.png "구매 프로세스에서이 일련의 스크린샷은 같습니다.")](purchasing-non-consumable-products-images/image34.png#lightbox)   
   
   
   
 구매 과정을 사용할 수 있는 제품 동일 주요 차이점은 응용 프로그램 코드에서 구매를 추적 하는 방법에 있습니다. 이 예제만 구입 단추를 제품 이미 구입 하지 않은 경우에 사용할 수이 고, 그렇지 단추를 활성화 기능 자체입니다.   
   
   
   

다음 다이어그램은 클래스 및 영구 제품 구매 하는 데 앱 스토어 서버 간의 상호 작용을 보여줍니다.   
   
   
   
 [![](purchasing-non-consumable-products-images/image35.png "클래스 및 영구 제품을 수행 하려면 앱 스토어 서버 간의 상호 작용 구매")](purchasing-non-consumable-products-images/image35.png#lightbox)   
   
   
   
 사용할 수 있는 예제의 주요 차이점은 구입을 완료 했으면 다시 구입을 방지 하기 위해 사용자 인터페이스 업데이트 됩니다. 성공적인 트랜잭션 알림을이 예제에서는 사용자 인터페이스를 업데이트 하는 **구입** 단추 자체 기능을 활성화 하는 단추 변환 됩니다.

## <a name="re-purchasing-non-consumable-products"></a>다시 영구 제품 구매

코드는 일반적으로 숨기 거 나 제품에 성공적으로 구입한는 사용자가 제품을 다시 구입할 하려고 하지 못하게 하려면 후 구매 단추 용도 변경 합니다. 샘플 응용 프로그램을 변경 하 여 수행 합니다 **구입** 작동 예제 사진 필터 하는 단추에는 단추입니다.   
   
   
   
 여기서 응용 프로그램 영구 제품 구입 했는지 여부를 알 수 없습니다 하는 경우가 있습니다.

-  응용 프로그램은를 삭제 하 고 장치에 다시 설치 하는 경우 모든 구매 레코드 내용이 사라집니다 (하지 않는 한 / 사용자가 수행 하는 백업-복원 될 때까지). 
-  사용자가 응용 프로그램 두 개 (이상) 장치에 설치 하 고 구매 하는 장치 중 하나. 다른 장치를 계속 구입할 수 있는 제품 표시 됩니다. 
-  고객을 다시 이러한 상황에서 영구 제품을 구매 하려고 하는 경우 앱 스토어를 무료로 다시 제품을 충족 합니다. 그러나 사용자 인터페이스를 구매 하는 데 처음에 표시 됩니다 (예를 들어 확인 경고가 표시 됩니다 및 Apple ID 요구 됩니다.) 사용자는 제품에 이미 구입한를 알려 주는 메시지가 표시 다음 됩니다.  
   
   
   
 이 시나리오에서 코드 경로 정확 하 게 일반 구매 동일, 유일한 차이점은:

-  사용자는 부과 되지는 않습니다 다시 제품에 대 한 합니다.
-  합니다 `SKPaymentTransaction` 응용 프로그램에 전달 된 개체에 포함 됩니다는 `OriginalTransaction` 처음으로 제품을 구입한 경우 생성 된 트랜잭션 나타나는 속성입니다. 
-  비-사용할 수 있는 제품을 판매 하는 응용 프로그램 StoreKit의 구현 해야 합니다 **복원** 기존 구매를 검색할 수 있도록 하는 기능입니다. 
