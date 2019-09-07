---
title: Xamarin에서 watchOS의 Apple Pay
description: 이 문서에서는 watchOS 3의 Apple Pay에 대 한 Apple의 향상 된 기능 및 Apple Watch 용 Xamarin.ios에서 이러한 기능을 구현 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 579f2afd8e52251973900f35ef91ac086adf7603
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768645"
---
# <a name="apple-pay-on-watchos-in-xamarin"></a>Xamarin에서 watchOS의 Apple Pay

Apple은 watchOS 3의 Apple Pay에 대 한 몇 가지 향상 된 기능을 제공 하 여 앱 내 지불액에 대 한 지원을 추가 합니다. 이를 통해 사용자는 지불 및 연락처 정보를 안전 하 게 제공 하 여 Apple Watch에서 직접 물리적 상품 및 서비스에 대 한 비용을 지불할 수 있습니다.

## <a name="about-apple-pay-enhancements"></a>Apple Pay 향상 정보

위에서 설명한 것 처럼 Apple은 watchOS 3의 Apple Pay에 대 한 몇 가지 향상 된 기능을 사용 하 여 Apple Watch에서 직접 물리적 상품 및 서비스에 대 한 요금을 지불 하는 보안 지불 및 연락처 정보가 있습니다. 이러한 향상 된 기능은 PassKit 프레임 워크를 수정 하 여 제공 됩니다.

IOS 10 및 watchOS 3을 사용 하는 경우 iOS와 watchOS 모두에서 작동 하 여 동적 지불 네트워크와 새 샌드박스 테스트 환경을 지 원하는 몇 가지 새로운 Api가 추가 되었습니다.

## <a name="passkit-framework-enhancements"></a>PassKit Framework의 향상 된 기능

IOS 10에서 PassKit 프레임 워크는 외부 `UIKit` Apple Pay를 지원 하도록 확장 되었으며 카드 발급자가 자신의 앱 내에서 카드를 제공할 수 있도록 합니다. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>UIKit 외부에서 Apple Pay 지원

앱은 [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) 및 [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)를 사용 하 여 uikit를 사용 하지 않고 [PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) 에서 제공 하는 것과 동일한 기능을 지원할 수 있습니다. 이 새로운 API는 Apple Watch에서 Apple Pay을 지원 하기 위해 필요 하지만 (특정 의도에도 해당) 다른 상황 (예: 기존 앱)에서는 선택 사항입니다. 그러나 Apple은 가능한 한 빨리 새 API로 전환 하 여 단일 코드 베이스를 사용 하 여 모든 개발자 앱 전체에 걸쳐 광범위 한 Apple Pay 지원을 제공할 것을 제안 합니다. 의도 및 Siri 통합에 대 한 자세한 내용은 [SiriKit 소개 설명서를](~/ios/platform/sirikit/index.md) 참조 하세요.

### <a name="presenting-issuer-cards-from-within-apps"></a>앱 내에서 발급자 카드 제공

IOS 10 및 watchOS 3을 사용 하는 경우 카드 발급자가 자신의 앱 내에서 지불 카드를 제공할 수 있도록 하는 새로운 기능이 PassKit 프레임 워크에 추가 되었습니다. 개발자는 카드에 대 `PKPaymentButtonTypeInStore` 한 Apple Pay 단추를 표시 하는 응용 프로그램의 사용자 인터페이스에 uibutton를 추가할 수 있습니다.

[PKPassLibrary 클래스](https://developer.apple.com/reference/passkit/pkpasslibrary) 의 메서드를사용하여프로그래밍방식으로카드를표시할수도`PresentPaymentPass` 있습니다.

## <a name="new-payment-network-support"></a>새 결제 네트워크 지원

IOS 10 및 watchOS 3의 새로운 기능으로, 앱은 개발자가 수정 하지 않고도 사용할 수 있게 되 면 새 결제 네트워크를 자동으로 지원할 수 있습니다. 앱을 다시 컴파일하고 앱 스토어에 다시 전송 해야 합니다.

`PKPaymentNetwork` 클래스의 새 [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) 메서드를 사용 하면 앱이 런타임에 사용자 장치에서 사용할 수 있는 네트워크를 검색할 수 있습니다. 또한 [Supportednetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) 속성은 지불 공급자의 이름을 인수로 사용 하도록 확장 되었습니다. 이러한 방법을 사용 하 여 앱은 지불 공급자가 지 원하는 모든 네트워크를 자동으로 지원할 수 있습니다.

자세한 내용은 [Apple Pay 구성](~/ios/platform/apple-pay.md) 및 Apple [Apple Pay 가이드](https://developer.apple.com/apple-pay/)를 참조 하세요.

## <a name="new-testing-environment"></a>새 테스트 환경

IOS 10 및 watchOS 3을 사용 하 여 Apple은 개발자가 iOS 장치에서 직접 테스트 지불 카드를 프로 비전 할 수 있는 새로운 테스트 환경을 도입 했습니다. 그러면이 새 테스트 환경이 암호화 된 테스트 지불 데이터를 앱에 반환 합니다.

새 테스트 환경을 사용 하도록 설정 하려면 다음을 수행 합니다.

1. ITunes Connect에서 새 테스트 iCloud 계정을 만듭니다.
2. 새 테스트 계정을 사용 하 여 iOS 장치에 로그인 합니다.
3. 에서 앱을 테스트 하는 데 필요한 영역을 설정 합니다.
4. [Apple Pay 가이드](https://developer.apple.com/apple-pay/) 에서 테스트 지불 카드 중 하나를 사용 하 여 지불액을 만듭니다.

> [!NOTE]
> ICloud 계정을 전환 하면 장치가 새 테스트 환경으로 자동 전환 됩니다. 그러나 Apple에서는 iTunes 앱 스토어에 제출 하기 전에 프로덕션 환경에서 실제 카드로 앱을 **테스트 해야 합니다** .

## <a name="summary"></a>요약

이 문서에서는 watchOS 3의 Apple Pay에 대 한 Apple의 향상 된 기능에 대해 설명 하 고 Xamarin.ios에서 이러한 기능을 구현 하는 방법을 설명 했습니다.
