---
title: Xamarin에서 watchOS에서 Apple Pay
description: 이 문서에서는 향상 된 기능을 설명 watchOS 3 및 Xamarin.iOS에서 Apple Watch 대 한 구현 하는 Apple에 Apple Pay에 대 한 합니다.
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 354e03ee1e07ba99fcdeb05617bc65ed89f0e8c2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61364180"
---
# <a name="apple-pay-on-watchos-in-xamarin"></a>Xamarin에서 watchOS에서 Apple Pay

Apple 앱 지불에 대 한 지원을 추가 하는 watchOS 3에서에서 Apple Pay에 몇 가지 향상 된 기능을 했습니다. 이 수 있도록 안전 하 게 지불을 제공 하 고 연락처 정보를 Apple Watch 직접 실제 상품 및 서비스에 대 한 비용을 지불 합니다.


## <a name="about-apple-pay-enhancements"></a>Apple Pay 향상 된 기능에 대 한

Stated 위의으로 Apple 몇 가지 향상 되었습니다 Apple Pay까지 보안 결제에 대 한 허용 및 담당자 정보를 Apple Watch 직접 실제 상품 및 서비스에 대 한 요금을 지불 하는 watchOS 3에에서 있습니다. 이러한 향상 된이 기능 수정 PassKit framework에 의해 제공 됩니다.

WatchOS 3 iOS 10과 몇 가지 새로운 Api가 iOS 및 watchOS 동적 결제 네트워크 및 새 샌드박스 테스트 환경을 지원 하기 위해 사용 되는 추가 되었습니다.

## <a name="passkit-framework-enhancements"></a>PassKit Framework의 향상 된

IOS 10, PassKit framework 외부에서 Apple Pay를 지원 하도록 확장 되었습니다에 `UIKit` 하 고 자신의 앱 내에서 해당 카드를 제공 하는 카드 발급자를 허용 합니다. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>UIKit 외부에서 Apple Pay를 지원합니다.

사용 하 여 [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) 하 고 [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)를 앱에서 제공 하는 동일한 기능을 지원할 수 있습니다 [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) UIKit 사용 하지 않고 있습니다. 이 새로운 API가 Apple Watch (및 특정 의도 뿐만) Apple Pay를 지원 하기 위해 필요한 동안에 다른 상황 (예: 기존 앱)는 선택 사항입니다. 그러나 Apple에서 제안 하는 단일 코드 베이스를 사용 하 여 모든 개발자의 앱 전체에서 광범위 하 게 Apple Pay 지원 수 있도록 가능한 한 빨리 새 API로 이동 합니다. 의도 대 한 자세한 내용은 및 Siri 통합을 참조 하십시오 우리의 [SiriKit 소개](~/ios/platform/sirikit/index.md) 설명서.

### <a name="presenting-issuer-cards-from-within-apps"></a>앱 내에서 발급자 카드를 제시합니다.

WatchOS 3 iOS 10과 새로운 기능이 자신의 앱 내에서 해당 지불 카드를 제공 하는 카드 발급자를 허용 하는 PassKit framework에 추가 되었습니다. 개발자가 추가할 수는 `PKPaymentButtonTypeInStore` UIButton 카드에 대 한 Apple Pay 단추를 표시 하는 앱의 사용자 인터페이스입니다.

합니다 `PresentPaymentPass` 메서드를 [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) 카드를 프로그래밍 방식으로 표시할 클래스도 사용할 수 있습니다.

## <a name="new-payment-network-support"></a>새 결제 네트워크 지원

새 iOS 10 및 watchOS 3 앱 지원할 수 있습니다 자동으로 새 결제 네트워크를 사용할 수 있을 때 개발자가 수정, 앱을 컴파일하고, 앱 스토어에 다시 제출 합니다.

새 [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) 메서드를 `PKPaymentNetwork` 클래스를 사용 하면 앱이 런타임 시 사용자의 장치에서 사용할 수 있는 네트워크를 검색 합니다. 또한 합니다 [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) 속성 결제 공급자의 이름을 인수로 사용 하도록 확장 되었습니다. 이러한 메서드를 사용 하는 앱 지불 공급자가 지 원하는 네트워크를 지원할 자동으로 수 있습니다.

자세한 내용은 참조 하십시오 우리의 [Apple 지불 구성](~/ios/platform/apple-pay.md) 과 Apple [Apple 지불 가이드](https://developer.apple.com/apple-pay/)합니다.

## <a name="new-testing-environment"></a>새 테스트 환경

IOS 10 및 watchOS 3 사용 하 여 Apple에서는 개발자가 iOS 장치에서 직접 테스트 지불 카드를 프로 비전 할 수 있도록 새 테스트 환경을 도입 되었습니다. 이 새 테스트 환경을 앱에 암호화 된 테스트 지불 데이터를 반환합니다.

새 테스트 환경의 사용 하도록 설정 하려면 다음을 수행 합니다.

1. ITunes Connect에서에서 새 테스트 iCloud 계정을 만듭니다.
2. 새 테스트 계정 사용 하 여 iOS 장치에 로그인 합니다.
3. 앱을 테스트 하려면 원하는 지역을 설정 합니다.
4. 테스트 지불 카드 중 하나를 사용 합니다 [Apple 지불 가이드](https://developer.apple.com/apple-pay/) 지불을 합니다.

> [!NOTE]
> ICloud 계정 전환 하 여 새 테스트 환경에 장치가 자동으로 전환 됩니다. 그러나 Apple 여전히 **필요** iTunes App Store에 제출 하기 전에 프로덕션 환경에서 카드 응용 프로그램에서 실시간으로 테스트 합니다.

## <a name="summary"></a>요약

이 문서에서는 이러한 향상 된 부분이 watchOS 3 Xamarin.iOS에서 구현 하는 방법에 Apple에 Apple Pay에 대 한 합니다.
