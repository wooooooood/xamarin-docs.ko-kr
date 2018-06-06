---
title: 앱에서 구매 Xamarin.iOS에
description: 이 문서에서는 디지털 제품 및 StoreKit Api를 사용 하 여 서비스를 판매 하는 방법에 설명 합니다. 구성, 사용 될 제품에서 사용할 제품, 트랜잭션, 구독 및 자세히 설명 하는 지침에 연결 합니다.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 8a41ed44a331c91a333b95c1d62136244a6945dd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787343"
---
# <a name="in-app-purchasing-in-xamarinios"></a>앱에서 구매 Xamarin.iOS에

디지털 제품 또는 서비스 StoreKit-id입니다. Apple를 통해 사용자와 금융 거래를 수행 하 고 Apple 서버와 통신 하는 iOS에서 제공 하는 Api 집합을 사용 하 여 iOS 응용 프로그램 판매할 수 있습니다. StoreKit Api 주로 고려 제품 정보를 검색 하 고 트랜잭션을 수행 하는 – 사용자 인터페이스 구성 요소가 없습니다. 앱에서 바로 구매를 구현 하는 응용 프로그램 사용자 인터페이스를 작성 하 고 사용자에 게 필요한 제품 또는 서비스를 제공 하기 위해 사용자 지정 코드로 구매한 항목을 추적 해야 합니다.

앱에서 바로 구매 기능을 제공 하는 몇을 가지 단계 필요 합니다.

-  **앱 구성** – 프로 비전 프로필을 응용 프로그램의 설치 프로그램을 올바르게 이어야 합니다.
-  **제품을 만드는** – 제품 설명 및 가격을 iTunes Connect 포털에서 만들어야 합니다.
-  **StoreKit 구현** – 판매 되는 제품의 형식에 따라 StoreKit API를 구현 해야 합니다.
-  **사용자 인터페이스 및 제품 자체 구축** -제품을 구현 해야, 백업/복원에 해당 하는 경우 각 구매를 추적 하는 메커니즘을 포함 합니다.
-  **Sales를 모니터링 하 고 받는 자금** – 판매 추세를 모니터링 하 고 수입이 추적를 iTunes Connect에서 제공한 정보를 사용 하 여 합니다.

이 문서에서는 앱에서 바로 구매 Xamarin.iOS를 사용 하 여 수 있도록 이러한 모든 단계를 완료 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

앱에서 바로 구매를 지원 하기 위해 Xcode 7 이상 Xamarin.iOS 5.0 이상 버전을 사용 해야 합니다.

## <a name="contents"></a>목차

 * [앱에서 바로 구매 기본 사항 및 구성](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [StoreKit 개요 및 제품 정보를 검색 하는 중](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [소모성 제품 구매](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [영구 제품 구매](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [트랜잭션 및 확인](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [구독 및 보고](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## <a name="summary"></a>요약

이 문서는 앱에서 바로 구매의 개념을 도입 하 고 활용 하도록 응용 프로그램을 구성 하는 방법을 설명 Xamarin.iOS를 사용 하는 예제를 제공 했습니다. 검사가:

-  **iOS 프로 비전 포털** – 앱에서 사용 하도록 설정 하는 것에 대 한 지침 기능을 구입 합니다.
-  **iTunes Connect** – 응용 프로그램에서 판매 하는 제품을 구성 합니다.
-  **키트 저장** – 앱에서 바로 구매 기능을 작성 하는 데 사용 되는 클래스에 대 한 설명입니다.
-  **구매에 대 한 앱을 코딩** – Xamarin.iOS 앱에 앱에서 바로 구매를 작성 하는 방법의 예입니다.
-  **보고** – iTunes Connect 통해 제공 되는 통계의 개요.


## <a name="related-links"></a>관련 링크

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [응용 프로그램 구매 프로그래밍 가이드](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect 개발자 가이드](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [키트 프레임 워크 참조를 저장 합니다.](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [앱에서 바로 구매 제품 식별자 질문 및 답변](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [앱에서 바로 구매 기술 노트](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [첫 번째 앱 스토어 제출](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [앱 스토어 리소스 센터](https://developer.apple.com/appstore/index.html)
- [앱 스토어 제출 팁](https://developer.apple.com/appstore/resources/submission/tips.html)
- [앱 스토어 검토 지침](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [앱 관리](https://developer.apple.com/appstore/resources/managing/index.html)
