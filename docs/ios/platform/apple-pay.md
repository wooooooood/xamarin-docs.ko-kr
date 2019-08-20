---
title: Xamarin.ios의 Apple Pay
description: 이 가이드에서는 Apple Pay와 함께 사용 하기 위해 Xamarin.ios 환경을 설정 하는 방법에 대해 설명 하 고 앱을 통해 음식, 엔터테인먼트 및 멤버 자격과 같은 물리적 상품을 지불 합니다. 필수 식별자, 인증서 및 자격에 대 한 정보를 포함 합니다.
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: cd5293e90caef81c875c0b06b9e5db06cd562655
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69620520"
---
# <a name="apple-pay-in-xamarinios"></a>Xamarin.ios의 Apple Pay

_이 가이드에서는 Apple Pay와 함께 사용 하기 위해 Xamarin.ios 환경을 설정 하는 방법에 대해 설명 하 고 앱을 통해 음식, 엔터테인먼트 및 멤버 자격과 같은 물리적 상품을 지불 합니다. 필수 식별자, 인증서 및 자격에 대 한 정보를 포함 합니다._

Apple Pay은 iOS 8과 함께 도입 되어 사용자가 iOS 장치를 통해 음식, 엔터테인먼트 및 멤버 자격과 같은 물리적 상품에 대해 지불할 수 있습니다. IPhone 6 및 iPhone 6 Plus에서 사용할 수 있으며, 매장 내 구매를 위해 Apple Watch와 쌍으로 연결 될 수도 있습니다. IPhone에서 사용 되는 경우 사용자의 신용 카드 또는 직불 카드에 대 한 트랜잭션을 확인 하 고 권한을 부여 하는 방법으로 Touch ID를 사용 합니다.

## <a name="requirements"></a>요구 사항

Apple Pay은 iOS 8 이상 에서만 사용할 수 있으므로 Xcode 6 이상이 필요 합니다.

다음 항목은 Apple Pay를 앱에 통합 하는 데에도 필요 합니다.

- 지불 프로세서 플랫폼
- 판매자 식별자
- Apple Pay 인증서
- Apple Pay 자격

이 문서에서는 이러한 항목을 자세히 살펴봅니다.

## <a name="differences-between-apple-pay-and-iap"></a>Apple Pay와 IAP의 차이점

Apple Pay와 iap ( *앱 간 구매* )의 주요 차이점은 판매 하는 제품과 관련이 있습니다. *물리적* 상품은 Apple Pay를 통해 판매 됩니다. 이에 대 한 예는 음식, 숙박 및 물리적 엔터테인먼트 (예: 영화 티켓)입니다. 반면, IAP는 프리미엄 또는 추가 콘텐츠, 구독 등의 *가상* 상품을 판매 합니다. 즉, 스트리밍 서비스의 추가 월을 고려 하거나 추가 게임을 진행 합니다.

사용 되는 프레임 워크는 키의 차이점 이기도 합니다. [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) 는 Apple Pay에 사용 되 고, 지 수 [키트](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) 는 iap 용 프레임 워크 API를 제공 합니다.

Apple Pay를 사용 하는 경우 Apple은 사용자, 상인 또는 개발자가 지불을 위해 Apple Pay를 사용할 수 있도록 요금을 청구 하지 않습니다. "라는 [메시지가](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) 제공 됩니다. 반면, IAP는 각 트랜잭션에 대해 30% 요금이 부과 됩니다. 또한 Apple Pay를 사용 하는 경우에도 트랜잭션은 Apple을 통해 이동 하지 않고 지불 플랫폼을 거칩니다.

## <a name="using-a-payment-processor-platform"></a>지불 프로세서 플랫폼 사용

Apple Pay의 기본적인 부분 중 하나는 지불액의 처리입니다. 이 작업을 직접 수행할 수는 있지만 암호화에 대 한 중요 한 지식이 필요 합니다.
- Apple의 [결제 처리 가이드](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html)에 자세히 설명 되어 있습니다.
반면 지불 처리 플랫폼은 이러한 작업을 처리 하 여 앱 빌드에 집중할 수 있도록 합니다.

두 가지 옵션은 다음과 같습니다.

- **Stripe** - [Stripe.com](https://stripe.com/) 에서 등록 하 여 api에 액세스 합니다.

- **JudoPay** - [github에서 Xamarin 샘플 코드](https://github.com/Judopay/Xamarin-Sample-App)를 확인 하 고 [JudoPay.com](https://www.judopay.com/)에 등록 합니다.

## <a name="provisioning-for-apple-pay"></a>Apple Pay 프로 비전

Apple Pay를 사용 하도록 앱을 구성 하려면 Apple 개발자 포털 및 앱 내에서 설정 해야 합니다. Apple 지불을 위해 앱을 성공적으로 프로 비전 하려면 다음 단계를 수행 해야 합니다.

1. 판매자 ID를 만듭니다.
    - [다음](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid) 단계를 수행 합니다.
2. 지불 적용 기능을 사용 하 여 앱 ID를 만들고 판매자를 추가 합니다.
    - [다음](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid) 단계를 수행 합니다.
3. 판매자 ID에 대 한 인증서를 생성 합니다.
    - [다음](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate) 단계를 수행 합니다.
4. 새로 만든 앱 ID를 사용 하 여 프로 비전 프로필을 생성 합니다.
    - [다음](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning) 단계를 수행 합니다.
5. Apple Pay 자격 추가:
    - [여기](~/ios/deploy-test/provisioning/entitlements.md)에 설명 된 대로 Apple 요금 자격을 선택 하거나 [여기](~/ios/deploy-test/provisioning/entitlements.md) 에서 파일에 키/값 쌍을 수동으로 추가 합니다.

## <a name="working-with-apple-pay"></a>Apple Pay 사용

Apple은 사용자가 웹 사이트와 Siri 및 Maps의 상호 작용을 통해 보안 지불을 수행할 수 있도록 하는 iOS 10의 Apple Pay에 대 한 몇 가지 향상 된 기능을 만들었습니다.

IOS 10을 사용 하 여 동적 지불 네트워크 및 새 샌드박스 테스트 환경을 지원 하기 위해 iOS와 watchOS 모두에서 작동 하는 몇 가지 새로운 Api가 추가 되었습니다.

### <a name="apple-pay-website-integration"></a>Apple Pay 웹 사이트 통합

IOS 10을 처음 사용 하는 개발자는 전 세계의 웹 사이트에Apple Pay를 직접 통합할 수 있습니다. IOS 또는 macOS에서 Safari를 사용 하 여 웹 사이트를 검색 하는 사용자는 iPhone 또는 Apple Watch에서 트랜잭션의 유효성을 검사 하 여 Apple Pay 지불을 수행할 수 있습니다. 자세한 내용은 Apple의 [사과 Epay 프레임 워크 참조](https://developer.apple.com/reference/applepayjs)를 참조 하세요.

### <a name="passkit-framework-enhancements"></a>PassKit Framework의 향상 된 기능

IOS 10에서 PassKit 프레임 워크는 외부 `UIKit` Apple Pay를 지원 하도록 확장 되었으며 카드 발급자가 자신의 앱 내에서 자신의 카드를 제공할 수 있도록 합니다.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>UIKit 외부에서 Apple Pay 지원

앱은 [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) 및 [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)를 사용 하 여 uikit를 사용 하지 않고 [PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) 에서 제공 하는 것과 동일한 기능을 지원할 수 있습니다. 이 새로운 API는 Apple Watch에서 Apple Pay을 지원 하기 위해 필요 하지만 (특정 의도에도 해당) 다른 상황 (예: 기존 앱)에서는 선택 사항입니다. 그러나 Apple은 가능한 한 빨리 새 API로 전환 하 여 단일 코드 베이스를 사용 하 여 모든 개발자 앱 전체에 걸쳐 광범위 한 Apple Pay 지원을 제공할 것을 제안 합니다. 의도 및 Siri 통합에 대 한 자세한 내용은 [SiriKit 소개 설명서를](~/ios/platform/sirikit/index.md) 참조 하세요.

#### <a name="presenting-issuer-cards-from-within-apps"></a>앱 내에서 발급자 카드 제공

IOS 10을 사용 하는 경우 카드 발급자가 자신의 앱 내에서 카드를 제공할 수 있도록 하는 새로운 기능이 PassKit 프레임 워크에 추가 되었습니다. 개발자는 카드에 대 `PKPaymentButtonTypeInStore` 한 Apple Pay 단추를 표시 하는 응용 프로그램의 사용자 인터페이스에 uibutton를 추가할 수 있습니다.

[PKPassLibrary 클래스](https://developer.apple.com/reference/passkit/pkpasslibrary) 의 메서드를사용하여프로그래밍방식으로카드를표시할수도`PresentPaymentPass` 있습니다.

### <a name="new-payment-network-support"></a>새 결제 네트워크 지원

IOS 10의 새로운 기능으로, 앱은 개발자가 수정 하지 않고도 사용할 수 있게 되 면 새 결제 네트워크를 자동으로 지원할 수 있습니다. 앱을 다시 컴파일하고 앱 스토어에 다시 전송 해야 합니다.

`PKPaymentNetwork` 클래스의 새 [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) 메서드를 사용 하면 앱이 런타임에 사용자 장치에서 사용할 수 있는 네트워크를 검색할 수 있습니다. 또한 [Supportednetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) 속성은 지불 공급자의 이름을 인수로 사용 하도록 확장 되었습니다. 이러한 방법을 사용 하 여 앱은 지불 공급자가 지 원하는 모든 네트워크를 자동으로 지원할 수 있습니다.

자세한 내용은 [Apple Pay 구성](~/ios/platform/apple-pay.md) 및 Apple [Apple Pay 가이드](https://developer.apple.com/apple-pay/)를 참조 하세요.

### <a name="new-testing-environment"></a>새 테스트 환경

IOS 10을 사용 하 여 Apple은 개발자가 iOS 장치에서 직접 테스트 지불 카드를 프로 비전 할 수 있는 새로운 테스트 환경을 도입 했습니다. 그러면이 새 테스트 환경이 암호화 된 테스트 지불 데이터를 앱에 반환 합니다.

새 테스트 환경을 사용 하도록 설정 하려면 다음을 수행 합니다.

1. ITunes Connect에서 새 테스트 iCloud 계정을 만듭니다.
2. 새 테스트 계정을 사용 하 여 iOS 장치에 로그인 합니다.
3. 에서 앱을 테스트 하는 데 필요한 영역을 설정 합니다.
4. [Apple Pay 가이드](https://developer.apple.com/apple-pay/) 에서 테스트 지불 카드 중 하나를 사용 하 여 지불액을 만듭니다.

> [!IMPORTANT]
> ICloud 계정을 전환 하면 장치가 새 테스트 환경으로 자동 전환 됩니다. 그러나 Apple에서는 iTunes 앱 스토어에 제출 하기 전에 프로덕션 환경에서 실제 카드로 앱을 테스트 해야 합니다.

## <a name="summary"></a>요약

이 문서에서는 앱 내에서 Apple Pay를 사용 하는 데 필요한 다양 한 항목을 살펴보았습니다. 판매자 ID를 만드는 방법 및 수동으로 수정 해야 하는 info.plist 내에서이를 사용 하는 방법을 살펴보았습니다 **.**

## <a name="related-links"></a>관련 링크

- [앱에서 바로 구매](~/ios/platform/in-app-purchasing/index.md)
- [PassKit 소개](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-emporium)
