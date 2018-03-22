---
title: Apple Pay
description: "이 가이드에는 멤버 자격 응용 프로그램을 통해 식품, 엔터테인먼트 등 물리적 상품 비용을 지불 하려면 Apple Pay와 함께 사용할 Xamarin.iOS 환경 설정을 탐색 합니다. 필요한 식별자, 인증서 및 권한에 대 한 정보를 포함합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: af899bb1c5708e3fc0be88db6224d9127f5a5c6d
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="apple-pay"></a>Apple Pay

_이 가이드에는 멤버 자격 응용 프로그램을 통해 식품, 엔터테인먼트 등 물리적 상품 비용을 지불 하려면 Apple Pay와 함께 사용할 Xamarin.iOS 환경 설정을 탐색 합니다. 필요한 식별자, 인증서 및 권한에 대 한 정보를 포함합니다._


Apple Pay 도입 되었습니다 8, iOS 함께 식품, 엔터테인먼트 및 iOS 장치를 통해 멤버 자격 같은 물리적 상품 비용을 지불 하려면 사용자가 사용 하도록 설정 합니다. IPhone 6 및 iPhone 6에 있는 더하기 및 스토어의 구입에 대 한 Apple Watch도 연결할 수 있습니다. IPhone에서 사용 되는, Touch ID를 확인 하 고 트랜잭션을 사용자의 신용 또는 직불 카드에 권한을 부여 하는 방법으로 사용 합니다.


## <a name="requirements"></a>요구 사항

Apple Pay은 내 iOS 8 이상 사용할 수 있으며 따라서 Xcode 6 최소 필요.

다음 항목은 응용 프로그램에 Apple Pay를 통합 하는 데 필요 합니다.

 - 결제 프로세서 플랫폼입니다.
 - 판매자 식별자
 - Apple Pay 인증서
 - Apple Pay 권한

이 문서에서는 이러한 항목을 더 자세히 살펴봅니다.

## <a name="differences-between-apple-pay-and-iap"></a>Apple Pay 및 IAP 간의 차이점

Apple Pay 사이의 주요 차이점은 및 *앱에서 바로 구매* (IAP) 제품을 판매와 관련이 있습니다. *물리적* Apple Pay 통해 상품은 판매; 음식과 숙박 물리적 엔터테인먼트 (예: 시네마 티켓)은이의 모든 예입니다. IAP 판매 하는 반면, *가상* 프리미엄 또는 기타 항목 및 구독 – 인지 추가 개월 스트리밍 서비스 또는 추가 생활 게임에서 같은 물건 합니다.

사용 되는 프레임 워크는 중요 한 차이점이; 수도 있습니다. [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) 은 Apple Pay에 대 한 사용 [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) IAP에 대 한 프레임 워크 API를 제공 합니다.

Apple Pay, 사과와 [상태](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) 있는지 것 "[] 충전 되지 않습니다. 결제에 대 한 Apple Pay를 사용 하 여 사용자, merchants 또는 개발자가"입니다. 이 비해 IAP 각 트랜잭션에 대해 30% 충전을 있습니다. 또한 Apple Pay와 트랜잭션을 통과 하지 않으므로 Apple 전혀, 지불 플랫폼 거치 대신 합니다.


## <a name="using-a-payment-processor-platform"></a>결제 프로세서 플랫폼을 사용 하 여

Apple Pay의 기본 구성 요소 중 하나는 지불을 처리 합니다. 암호화의 중요 한 지식이 있어야이 직접 수행 하는 가능 하지만
- Apple에 설명 된 대로 [지불을 처리 하는 가이드](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html)합니다.
반면에 지불 처리 플랫폼에서 앱 작성에 집중할 수 있도록 이러한 작업을 처리 합니다.

두 가지 옵션은 다음과 같습니다.

- **Stripe** -에서 등록 [Stripe.com](https://stripe.com/) 해당 Api에 액세스할 수 있습니다.

- **JudoPay** -체크 아웃의 [github에서 Xamarin 샘플 코드](https://github.com/Judopay/Xamarin-Sample-App), 및에서 등록 한 다음 [JudoPay.com](https://www.judopay.com/)합니다.


## <a name="provisioning-for-apple-pay"></a>Apple Pay에 대 한 프로 비전

Apple Pay 사용 하도록 앱을 구성 하려면 및 앱 내 Apple 개발자 포털에서 설치 합니다. Apple pay에 대 한 응용 프로그램 프로 비전 하기 위해 수행 해야 하는 단계는 여러 가지가:

1. 판매자 ID를 만듭니다.
    - 단계에 따라 [여기](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. 비용을 지불 적용 기능이 있는 앱 ID를 만들고 판매 업체를 추가 합니다.
    - 단계에 따라 [여기](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. 판매 업체 ID에 대 한 인증서를 생성 합니다.
    - 단계에 따라 [여기](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. 새로 만든된 응용 프로그램 ID로 프로 비전 프로필을 생성 합니다.
    - 단계에 따라 [여기](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Apple Pay 자격을 추가 합니다.
    - 에 설명 된 대로 Apple pay 자격 선택 [여기](~/ios/deploy-test/provisioning/entitlements.md), 수동으로에서 파일에 키/값 쌍을 추가 또는 [여기](~/ios/deploy-test/provisioning/entitlements.md)


## <a name="working-with-apple-pay"></a>Apple Pay 작업

Apple 웹 사이트에서 및 Siri 및 지도와 상호 작용을 통해 보안 상환를 사용할 수 있는 iOS 10에서에서 Apple 지불을 위해 여러 개선 된 기능을 했습니다.

10 ios 몇 가지 새로운 Api가 iOS 및 watchOS 동적 지불 네트워크와 새 샌드박스 테스트 환경을 지원 하기 위해 둘 다에서 작동 하는 추가 되었습니다.


### <a name="apple-pay-website-integration"></a>Apple Pay 웹 사이트 통합

새로운 iOS 10에는 개발자에 통합할 수 Apple Pay 직접 사용 하 여 웹 사이트 **ApplePay JS**합니다. IOS 또는 macOS에서 safari 웹 사이트를 검색 하는 사용자가 iPhone 또는 Apple Watch 트랜잭션을 유효성을 검사 하 여 Apple Pay와 상환 수 있습니다. 자세한 내용은 Apple의를 참조 하십시오 [ApplePay JP 프레임 워크 참조](https://developer.apple.com/reference/applepayjs)합니다.

### <a name="passkit-framework-enhancements"></a>PassKit 프레임 워크의 향상 된 기능

10 iOS PassKit 프레임 워크 외부에 Apple Pay 지원 하도록 확장 되었습니다 되었습니다 `UIKit` 앱 내에서 자신의 카드 표시 하도록 카드 발급자를 허용 하 고 있습니다.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>Apple Pay UIKit 외부에서 지원

사용 하 여 [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) 및 [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), 응용 프로그램에서 제공 하는 동일한 기능을 지원할 수 [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) UIKit 사용 하지 않고 있습니다. 이 새로운 API Apple Pay Apple Watch (및 특정 의도)을 지원 하기 위해 필요한 상태인 동안에 다른 상황 (예: 기존 응용 프로그램)에서는 선택 사항입니다. 그러나 Apple 제안 지원을 제공 하기 위해 광범위 한 Apple Pay 개발자의 응용 프로그램의 모든 단일 코드 베이스를 최대한 빨리 새로운 API를 이동 합니다. 의도 대 한 자세한 내용은 및 Siri 통합을 참조 하십시오 우리의 [SiriKit 소개](~/ios/platform/sirikit/index.md) 설명서입니다.

#### <a name="presenting-issuer-cards-from-within-apps"></a>앱 내에서 발급자 카드를 제시합니다.

10 ios 자신의 앱 내에서 카드를 표시 하도록 카드 발급자를 허용 하는 PassKit framework에 대 한 새 기능이 추가 되었습니다. 개발자를 추가할 수는 `PKPaymentButtonTypeInStore` UIButton는 카드에 대 한 Apple Pay 단추를 사용 하는 표시 하는 응용 프로그램의 사용자 인터페이스에 있습니다.

`PresentPaymentPass` 의 메서드는 [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) 클래스 프로그래밍 방식으로 카드 표시를 사용할 수도 있습니다.

### <a name="new-payment-network-support"></a>새 지불 네트워크 지원

새로운 10 iOS에는 응용 프로그램이 자동으로 지원할 수 새 지불 네트워크를 사용할 수 있을 때 개발자가 수정, 응용 프로그램을 다시 컴파일하고 앱 스토어에 다시 전송 하 합니다.

새 [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) 의 메서드는 `PKPaymentNetwork` 클래스를 사용 하면 앱이 런타임 시 사용자의 장치에서 사용 가능한 네트워크 검색 합니다. 또한는 [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) 속성 결제 공급자의 이름을 인수로 사용할 수 있도록 확장 되었습니다. 이러한 메서드를 사용 하는 응용 프로그램 결제 공급자가 지 원하는 모든 네트워크를 지원할 자동으로 수 있습니다.

자세한 내용은 참조 하십시오 우리의 [Apple 지불 구성](~/ios/platform/apple-pay.md) 과 Apple [Apple 지불 가이드](https://developer.apple.com/apple-pay/)합니다.

### <a name="new-testing-environment"></a>새 테스트 환경

IOS 10, 사과 도입 했는데 개발자가 iOS 장치에서 직접 테스트 결제 카드를 프로 비전 하는 새 테스트 환경입니다. 이 새 테스트 환경 응용 프로그램에 암호화 된 테스트 결제 데이터를 반환합니다.

새 테스트 환경의 사용 하도록 설정 하려면 다음을 수행 합니다.

1. ITunes Connect에서에서 새 테스트 iCloud 계정을 만듭니다.
2. 새 테스트 계정 사용 하 여 iOS 장치에 로그인 합니다.
3. 앱을 테스트 하려면 원하는 영역을 설정 합니다.
4. 테스트 결제 카드 중 하나를 사용 하 여는 [Apple 지불 가이드](https://developer.apple.com/apple-pay/) 지불 되도록 합니다.

> [!IMPORTANT]
> ICloud 계정을 전환 하 여 장치는 자동으로 새 테스트 환경으로 전환 됩니다. 그러나, 사과 여전히 **필요** iTunes App Store에 제출 하기 전에 프로덕션 환경에서 카드 응용 프로그램에서 실수와 함께 테스트할 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 Apple Pay 앱 내에서 사용 하는 데 필요한 다양 한 항목에서 살펴본 합니다. 판매 업체 ID를 만드는 방법 및에서 사용 되는 방법을 살펴보았습니다는 **Entitlements.plist**는 수동으로 수정 해야 합니다.


## <a name="related-links"></a>관련 링크

- [앱에서 바로 구매](~/ios/platform/in-app-purchasing/index.md)
- [PassKit 소개](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [상점 (샘플)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
