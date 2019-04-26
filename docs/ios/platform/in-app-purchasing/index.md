---
title: 앱에서 Xamarin.iOS에서 구입
description: 이 문서에 디지털 제품 및 StoreKit Api를 사용 하 여 서비스를 판매 하는 방법을 설명 합니다. 구성, 소모 성 제품, 영구 제품, 트랜잭션, 구독 및 자세히 설명 하는 지침에 연결 합니다.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4b301c18ea0e69c818cf65b3b7df1cc8351350f5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61402457"
---
# <a name="in-app-purchasing-in-xamarinios"></a>앱에서 Xamarin.iOS에서 구입

디지털 제품 또는 서비스 StoreKit – 해당 Apple id.를 통해 사용자를 사용 하 여 금융 거래를 Apple 서버와 통신 하는 iOS에서 제공 하는 Api 집합을 사용 하 여 iOS 응용 프로그램을 판매할 수 있습니다. StoreKit Api 트랜잭션 및 제품 정보 검색을 사용 하 여 주로 관심이 – 사용자 인터페이스 구성 요소가 없습니다. 인 앱 구매를 구현 하는 응용 프로그램 자체 사용자 인터페이스를 작성 하 고 사용자에 게 필요한 제품 또는 서비스를 제공 하는 사용자 지정 코드를 사용 하 여 구매한 항목을 추적 해야 합니다.

앱에서 바로 구매 기능을 제공 하는 몇을 가지 단계에 필요 합니다.

-  **앱 구성** – 응용 프로그램의 프로 비전 프로필 설치를 올바르게 수 있어야 합니다.
-  **제품을 만드는** – iTunes Connect 포털에서 제품 설명 및 가격을 만들 수 있어야 합니다.
-  **StoreKit 구현** – StoreKit API 판매 되는 제품의 형식에 따라 구현 해야 합니다.
-  **사용자 인터페이스 및 자체 제품 빌드** – 제품 구현 해야, 백업/복원에 해당 하는 경우 각 구매를 추적 하는 메커니즘을 포함 합니다.
-  **Sales를 모니터링 및 자금을 받는** – iTunes Connect에서 제공 하는 정보를 사용 하 여 판매 추세를 모니터링 하 고 프로그램 수입을 추적 합니다.

이 문서에서는 Xamarin.iOS를 사용 하 여 앱 내 구매를 제공 하는 이러한 모든 단계를 완료 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

인 앱 구매를 지원 하기 위해 Xcode 7 이상 Xamarin.iOS 5.0 이상을 사용 해야 합니다.

## <a name="contents"></a>목차

 * [앱에서 바로 구매 기본 사항 및 구성](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [StoreKit 개요 및 제품 정보 검색](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [소모성 제품 구매](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [영구 제품 구매](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [트랜잭션 및 확인](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [구독 및 보고](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## <a name="summary"></a>요약

이 문서는 인 앱 구매의 개념이 도입 되었습니다., 활용 하도록 응용 프로그램을 구성 하는 방법을 설명 및 Xamarin.iOS를 사용 하는 예제를 제공 합니다. 에서는 이러한 부분이:

-  **iOS Provisioning Portal** – 앱을 사용 하도록 설정 하는 것에 대 한 지침 기능을 구매 합니다.
-  **iTunes Connect** -앱에서 판매 하려면 제품을 구성 합니다.
-  **키트 저장** -앱에서 바로 구매 기능을 빌드하는 데 사용 하는 클래스를 설명 합니다.
-  **구매를 위한 앱을 코딩** – Xamarin.iOS 앱에 앱 내 구매를 빌드하는 방법의 예입니다.
-  **보고** – iTunes Connect 통해 제공 되는 통계의 개요입니다.


## <a name="related-links"></a>관련 링크

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [앱에서 바로 구매 프로그래밍 가이드](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect 개발자 가이드](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [키트 프레임 워크 참조를 저장 합니다.](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [질문 및 답변 인 앱 구매 제품 식별자](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [인 앱 구매 Technical Note](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [첫 번째 앱 스토어 제출](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [앱 스토어 리소스 센터](https://developer.apple.com/appstore/index.html)
- [앱 스토어 제출 팁](https://developer.apple.com/appstore/resources/submission/tips.html)
- [앱 스토어 검토 지침](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [앱 관리](https://developer.apple.com/appstore/resources/managing/index.html)
