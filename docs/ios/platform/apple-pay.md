---
title: Xamarin.iOS에서 Apple Pay
description: 이 가이드에서는 음식, 엔터테인먼트 등 멤버 자격에 앱을 통해 실제 상품에 대 한 비용을 지불 Apple Pay 사용에 대 한 Xamarin.iOS 환경 설정 합니다. 필요한 식별자, 인증서 및 자격에 대 한 정보를 포함합니다.
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: b971029ff3b2b1e8f5e63233d1d754c44b0e3309
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61346976"
---
# <a name="apple-pay-in-xamarinios"></a>Xamarin.iOS에서 Apple Pay

_이 가이드에서는 음식, 엔터테인먼트 등 멤버 자격에 앱을 통해 실제 상품에 대 한 비용을 지불 Apple Pay 사용에 대 한 Xamarin.iOS 환경 설정 합니다. 필요한 식별자, 인증서 및 자격에 대 한 정보를 포함합니다._

Apple Pay 도입 iOS 8 함께 식품, 엔터테인먼트 및 iOS 장치를 통해 멤버 자격 등 실제 상품에 대 한 요금을 지불 하는 할 수 있도록 합니다. IPhone 6 및 iPhone 6에서 사용할 수 또한 및 store에서 구매에 대 한 Apple Watch 사용 하 여 연결할 수 있습니다. IPhone에서 사용 하는 경우 확인 하 고 트랜잭션을 사용자의 신용 또는 직불 카드에 권한을 부여 하는 방법으로 Touch ID를 사용 합니다.

## <a name="requirements"></a>요구 사항

Apple Pay 내 iOS 8 이상에 사용할 수 고만 하므로 Xcode 6 개 있습니다.

다음 항목은 앱에 Apple Pay를 통합 하는 데 필요한도:

 - 결제 프로세서 플랫폼
 - 판매자 식별자
 - Apple Pay 인증서를
 - Apple Pay 자격

이 문서에서는 이러한 항목을 자세히 살펴봅니다.

## <a name="differences-between-apple-pay-and-iap"></a>Apple Pay 및 IAP 간의 차이점

Apple Pay에 대 한 주요 차이점 및 *인 앱 구매* (IAP) 제품을 판매와 관련이 있습니다. *실제* 상품 Apple Pay를 통해 판매 되는 식품, 숙박 및 실제 엔터테인먼트 (예: 영화 티켓)이 모두 예입니다. IAP 판매 하는 반면 *가상* 상품, premium 또는 추가 콘텐츠를 스트리밍 서비스, 추가 거주 게임의 추가 개월 구독 – 인지 등입니다.

주요 차이점;도 사용 되는 프레임 워크 [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) 가 Apple Pay에 대 한 사용 [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) IAP에 대 한 API는 프레임 워크를 제공 합니다.

사용 하 여 Apple Pay Apple [상태](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) 는 해당 "[]을 청구 하지 않습니다, merchants 사용자나 개발자 Apple Pay를 사용 하 여 지불"입니다. 반면 IAP에는 각 트랜잭션에 대해 30% 요금이 있습니다. 또한 Apple Pay를 사용 하 여 트랜잭션을 통과 하지 않으므로 Apple 전혀, 대신 지불 플랫폼을 통해 전달 됩니다.

## <a name="using-a-payment-processor-platform"></a>결제 프로세서 플랫폼을 사용 하 여

Apple Pay에 대 한 기본 부분 지불을 처리 하는 것입니다. 암호화의 중요 한 전문 지식이 있어야이 작업을 직접 할 수 있지만,
- Apple에 설명 된 대로 [지불 처리 가이드](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html)합니다.
반면, 결제 처리 플랫폼 앱 빌드에 집중할 수 있도록 사용자에 대 한 이러한 작업을 처리 합니다.

두 가지 옵션은 다음과 같습니다.

- **Stripe** -로그인 [Stripe.com](https://stripe.com/) 해당 Api에 액세스 합니다.

- **JudoPay** -체크 아웃 해당 [github의 Xamarin 샘플 코드](https://github.com/Judopay/Xamarin-Sample-App), 및에서 등록 [JudoPay.com](https://www.judopay.com/)합니다.

## <a name="provisioning-for-apple-pay"></a>Apple Pay에 대 한 프로 비전

Apple Pay를 사용 하도록 앱을 구성 및 앱 내 Apple Developer 포털에서 설치를 해야 합니다. Apple pay에 대 한 앱을 성공적으로 프로 비전 하기 위해 수행 해야 하는 단계 수 있습니다.

1. 가맹점 ID를 만듭니다.
    - 단계에 따라 [여기](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. 적용 Pay 기능으로 앱 ID를 만들고 가맹점 것을 추가 합니다.
    - 단계에 따라 [여기](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. 가맹점 ID에 대 한 인증서를 생성 합니다.
    - 단계에 따라 [여기](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. 새로 만든된 앱 ID를 사용 하 여 프로 비전 프로필을 생성 합니다.
    - 단계에 따라 [여기](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Apple Pay 자격을 추가 합니다.
    - 설명 된 대로 Apple pay 자격 선택 [여기](~/ios/deploy-test/provisioning/entitlements.md)에서 파일에 키/값 쌍을 수동으로 추가 또는 [여기](~/ios/deploy-test/provisioning/entitlements.md)

## <a name="working-with-apple-pay"></a>Apple Pay 사용

Apple 웹 사이트에서 Siri 및 맵 상호 작용을 통해 보안 지불 하는 사용자를 허용 하는 iOS 10에서에서 Apple Pay에 몇 가지 향상 된 기능을 했습니다.

IOS 10 사용 하 여 몇 가지 새로운 Api가 사용 하 여 iOS 및 watchOS 동적 결제 네트워크 및 새 샌드박스 테스트 환경을 지원 하기 위해 추가 되었습니다.

### <a name="apple-pay-website-integration"></a>Apple Pay 웹 사이트 통합

새 ios 10, 개발자에 통합할 수 Apple Pay 직접 사용 하 여 해당 웹 사이트 **ApplePay JS**합니다. IOS 또는 macOS에서 Safari 사용 하 여 웹 사이트를 검색 하는 사용자는 자신의 iPhone 또는 Apple Watch 트랜잭션 유효성 검사 하 여 Apple Pay 사용 하 여 지불을 만들 수 있습니다. 자세한 내용은 Apple의를 참조 하세요 [ApplePay JP 프레임 워크 참조](https://developer.apple.com/reference/applepayjs)합니다.

### <a name="passkit-framework-enhancements"></a>PassKit Framework의 향상 된

IOS 10, PassKit framework 외부에서 Apple Pay를 지원 하도록 확장 되었습니다에 `UIKit` 하 고 자신의 앱 내에서 고유한 카드를 제공 하는 카드 발급자를 허용 합니다.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>UIKit 외부에서 Apple Pay를 지원합니다.

사용 하 여 [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) 하 고 [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)를 앱에서 제공 하는 동일한 기능을 지원할 수 있습니다 [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) UIKit 사용 하지 않고 있습니다. 이 새로운 API가 Apple Watch (및 특정 의도 뿐만) Apple Pay를 지원 하기 위해 필요한 동안에 다른 상황 (예: 기존 앱)는 선택 사항입니다. 그러나 Apple에서 제안 하는 단일 코드 베이스를 사용 하 여 모든 개발자의 앱 전체에서 광범위 하 게 Apple Pay 지원 수 있도록 가능한 한 빨리 새 API로 이동 합니다. 의도 대 한 자세한 내용은 및 Siri 통합을 참조 하십시오 우리의 [SiriKit 소개](~/ios/platform/sirikit/index.md) 설명서.

#### <a name="presenting-issuer-cards-from-within-apps"></a>앱 내에서 발급자 카드를 제시합니다.

IOS 10 사용 하 여 PassKit framework가 자신의 앱 내에서 해당 카드를 제공 하는 카드 발급자를 허용 하는 새 기능이 추가 되었습니다. 개발자가 추가할 수는 `PKPaymentButtonTypeInStore` UIButton 카드에 대 한 Apple Pay 단추를 표시 하는 앱의 사용자 인터페이스입니다.

합니다 `PresentPaymentPass` 메서드를 [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) 카드를 프로그래밍 방식으로 표시할 클래스도 사용할 수 있습니다.

### <a name="new-payment-network-support"></a>새 결제 네트워크 지원

새 ios 10, 앱을 지원할 수 있습니다 자동으로 새 결제 네트워크를 사용할 수 있을 때 개발자가 수정, 앱을 다시 컴파일하고 다시 앱 스토어에 제출 하지 않아도 됩니다.

새 [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) 메서드를 `PKPaymentNetwork` 클래스를 사용 하면 앱이 런타임 시 사용자의 장치에서 사용할 수 있는 네트워크를 검색 합니다. 또한 합니다 [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) 속성 결제 공급자의 이름을 인수로 사용 하도록 확장 되었습니다. 이러한 메서드를 사용 하는 앱 지불 공급자가 지 원하는 네트워크를 지원할 자동으로 수 있습니다.

자세한 내용은 참조 하십시오 우리의 [Apple 지불 구성](~/ios/platform/apple-pay.md) 과 Apple [Apple 지불 가이드](https://developer.apple.com/apple-pay/)합니다.

### <a name="new-testing-environment"></a>새 테스트 환경

IOS 10 사용 하 여 Apple에서는 개발자가 iOS 장치에서 직접 테스트 지불 카드를 프로 비전 할 수 있도록 새 테스트 환경을 도입 되었습니다. 이 새 테스트 환경을 앱에 암호화 된 테스트 지불 데이터를 반환합니다.

새 테스트 환경의 사용 하도록 설정 하려면 다음을 수행 합니다.

1. ITunes Connect에서에서 새 테스트 iCloud 계정을 만듭니다.
2. 새 테스트 계정 사용 하 여 iOS 장치에 로그인 합니다.
3. 앱을 테스트 하려면 원하는 지역을 설정 합니다.
4. 테스트 지불 카드 중 하나를 사용 합니다 [Apple 지불 가이드](https://developer.apple.com/apple-pay/) 지불을 합니다.

> [!IMPORTANT]
> ICloud 계정 전환 하 여 새 테스트 환경에 장치가 자동으로 전환 됩니다. 그러나 Apple 여전히 **필요** iTunes App Store에 제출 하기 전에 프로덕션 환경에서 카드 응용 프로그램에서 실시간으로 테스트 합니다.

## <a name="summary"></a>요약

이 문서에서는 Apple Pay 앱 내에서 사용 하는 데 필요한 다른 항목을 살펴보았습니다. 가맹점 ID를 만드는 방법 및에서 사용 되는 방법을 살펴보았습니다 합니다 **Entitlements.plist**는 수동으로 수정 해야 합니다.

## <a name="related-links"></a>관련 링크

- [앱에서 바로 구매](~/ios/platform/in-app-purchasing/index.md)
- [PassKit 소개](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [상점 (샘플)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
