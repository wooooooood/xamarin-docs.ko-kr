---
title: Xamarin.ios의 앱 내 구매
description: 이 문서에서는 서비스 키트 Api를 사용 하 여 디지털 제품 및 서비스를 판매 하는 방법을 설명 합니다. 구성, 사용할 수 있는 제품, 사용할 수 없는 제품, 트랜잭션, 구독 등에 대해 설명 하는 가이드로 연결 됩니다.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 63530595d9892ed99b7eace3d248e1bc0d0b37d4
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70288398"
---
# <a name="in-app-purchasing-in-xamarinios"></a>Xamarin.ios의 앱 내 구매

iOS 응용 프로그램은 휴대폰 키트를 사용 하 여 디지털 제품 또는 서비스를 판매할 수 있습니다. 즉, apple의 서버와 통신 하 여 Apple ID를 통해 재무 트랜잭션을 수행 하는 iOS에서 제공 하는 Api 집합입니다. 보관 키트 Api는 주로 제품 정보 검색 및 트랜잭션 수행과 관련 된 것입니다. 사용자 인터페이스 구성 요소는 없습니다. 앱에서 바로 구매를 구현 하는 응용 프로그램은 사용자에 게 필요한 제품이 나 서비스를 제공 하는 사용자 지정 코드를 사용 하 여 사용자 인터페이스를 빌드하고 구매한 항목을 추적 해야 합니다.

앱 내 구매 기능을 제공 하려면 다음과 같은 여러 단계가 필요 합니다.

- **앱 구성** – 응용 프로그램의 프로 비전 프로필을 올바르게 설정 해야 합니다.
- 제품 **만들기** – iTunes Connect 포털에서 제품 설명 및 가격을 만들어야 합니다.
- 복사본 **키트 구현** – 판매 되는 제품의 형식에 따라 나이 키트 API를 구현 해야 합니다.
- **사용자 인터페이스 및 제품 자체 빌드** – 각 구매를 추적 하는 메커니즘과 해당 하는 경우 백업/복원 하는 메커니즘을 포함 하 여 제품을 구현 해야 합니다.
- **판매 모니터링 및 자금 수령** – iTunes Connect에서 제공 하는 정보를 사용 하 여 판매 추세를 모니터링 하 고 수입을 추적 합니다.

이 문서에서는 Xamarin.ios를 사용 하 여 앱에서 바로 구매를 제공 하기 위해 이러한 모든 단계를 완료 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

앱 내 구매를 지원 하려면 Xcode 7 이상에서 Xamarin.ios 5.0 이상 버전을 사용 해야 합니다.

## <a name="contents"></a>목차

- [앱에서 바로 구매 기본 사항 및 구성](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

- [보관 키트 개요 및 제품 정보 검색](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

- [소모성 제품 구매](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

- [영구 제품 구매](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

- [트랜잭션 및 확인](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

- [구독 및 보고](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## <a name="summary"></a>요약

이 문서에서는 앱을 사용 하도록 응용 프로그램을 구성 하는 방법과 Xamarin.ios를 사용 하는 예제를 활용 하도록 응용 프로그램을 구성 하는 방법에 대해 설명 하는 개념을 소개 했습니다. 설명 합니다.

- **IOS 프로 비전 포털** – 앱에서의 구매 기능을 사용 하도록 설정 하기 위한 지침입니다.
- **ITunes Connect** – 앱에서 판매 하도록 제품을 구성 합니다.
- **스토어 키트** – 앱에서의 구매 기능을 빌드하는 데 사용 되는 클래스에 대 한 설명입니다.
- **구매를 위해 앱 코딩** – 앱 내 구매를 xamarin.ios 앱으로 빌드하는 방법에 대 한 예제입니다.
- **보고** -iTunes Connect를 통해 사용할 수 있는 통계에 대 한 개요입니다.


## <a name="related-links"></a>관련 링크

- [InAppPurchaseSample](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit/)
- [앱 구매 프로그래밍 가이드](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect 개발자 가이드](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [스토어 키트 프레임 워크 참조](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [앱 내 구매 제품 식별자 Q & A](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [앱에서 구매 기술 참고 사항](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [첫 번째 앱 스토어 제출](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [앱 스토어 리소스 센터](https://developer.apple.com/appstore/index.html)
- [앱 스토어 제출 팁](https://developer.apple.com/appstore/resources/submission/tips.html)
- [앱 스토어 검토 지침](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [앱 관리](https://developer.apple.com/appstore/resources/managing/index.html)
