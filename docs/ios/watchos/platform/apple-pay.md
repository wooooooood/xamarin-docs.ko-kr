---
title: Apple Pay watchOS에
description: 이 문서에서는 이러한 향상 된 기능에 설명 Apple에 수행한 Apple Pay watchOS 3 및 Apple Watch 대 한 Xamarin.iOS에 구현 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: b46a0e57ea9abc5c4ec4fc2aba1e6940249b64fb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="apple-pay-on-watchos"></a>Apple Pay watchOS에

Apple가 응용 프로그램에서 결제에 대 한 지원을 추가 하는 watchOS 3에서에서 Apple 지불을 위해 여러 개선 된 기능이 만들었습니다. 이렇게 하면 안전 하 게 지불을 제공 하 고 연락처에 Apple Watch 직접 실제 상품 및 서비스에 대 한 비용을 지불 정보를 사용자입니다.


## <a name="about-apple-pay-enhancements"></a>Apple Pay 향상 된 기능에 대 한

Stated 위의으로 Apple 몇 가지 향상 되었습니다 Apple 지불을 위해 보안 지불 하 고 연락처 정보에 Apple Watch 직접 실제 상품 및 서비스에 대 한 비용을 지불 하는 watchOS 3에에서 있습니다. 이러한 개선 사항은 수정 PassKit 프레임 워크에 의해 제공 됩니다.

IOS 10와 3 watchOS 몇 가지 새로운 Api가 iOS 및 watchOS 동적 지불 네트워크와 새 샌드박스 테스트 환경을 지원 하기 위해 둘 다에서 작동 하는 추가 되었습니다.

## <a name="passkit-framework-enhancements"></a>PassKit 프레임 워크의 향상 된 기능

10 iOS PassKit 프레임 워크 외부에 Apple Pay 지원 하도록 확장 되었습니다 되었습니다 `UIKit` 앱 내에서 카드를 표시 하도록 카드 발급자를 허용 하 고 있습니다. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>Apple Pay UIKit 외부에서 지원

사용 하 여 [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) 및 [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), 응용 프로그램에서 제공 하는 동일한 기능을 지원할 수 [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) UIKit 사용 하지 않고 있습니다. 이 새로운 API Apple Pay Apple Watch (및 특정 의도)을 지원 하기 위해 필요한 상태인 동안에 다른 상황 (예: 기존 응용 프로그램)에서는 선택 사항입니다. 그러나 Apple 제안 지원을 제공 하기 위해 광범위 한 Apple Pay 개발자의 응용 프로그램의 모든 단일 코드 베이스를 최대한 빨리 새로운 API를 이동 합니다. 의도 대 한 자세한 내용은 및 Siri 통합을 참조 하십시오 우리의 [SiriKit 소개](~/ios/platform/sirikit/index.md) 설명서입니다.

### <a name="presenting-issuer-cards-from-within-apps"></a>앱 내에서 발급자 카드를 제시합니다.

IOS 10와 3 watchOS 자신의 앱 내에서 지불 카드 표시 하도록 카드 발급자를 허용 하는 PassKit framework에 대 한 새 기능이 추가 되었습니다. 개발자를 추가할 수는 `PKPaymentButtonTypeInStore` UIButton는 카드에 대 한 Apple Pay 단추를 사용 하는 표시 하는 응용 프로그램의 사용자 인터페이스에 있습니다.

`PresentPaymentPass` 의 메서드는 [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) 클래스 프로그래밍 방식으로 카드 표시를 사용할 수도 있습니다.

## <a name="new-payment-network-support"></a>새 지불 네트워크 지원

새로운 iOS 10 및 watchOS 3에는 응용 프로그램이 자동으로 지원할 수 새 지불 네트워크를 사용할 수 있을 때 개발자가 수정, 응용 프로그램을 다시 컴파일하고 앱 스토어에 다시 전송 하 합니다.

새 [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) 의 메서드는 `PKPaymentNetwork` 클래스를 사용 하면 앱이 런타임 시 사용자의 장치에서 사용 가능한 네트워크 검색 합니다. 또한는 [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) 속성 결제 공급자의 이름을 인수로 사용할 수 있도록 확장 되었습니다. 이러한 메서드를 사용 하는 응용 프로그램 결제 공급자가 지 원하는 모든 네트워크를 지원할 자동으로 수 있습니다.

자세한 내용은 참조 하십시오 우리의 [Apple 지불 구성](~/ios/platform/apple-pay.md) 과 Apple [Apple 지불 가이드](https://developer.apple.com/apple-pay/)합니다.

## <a name="new-testing-environment"></a>새 테스트 환경

Apple는 iOS 10와 3 watchOS 개발자가 iOS 장치에서 직접 테스트 결제 카드를 프로 비전 하는 새 테스트 환경을 도입 합니다. 이 새 테스트 환경 응용 프로그램에 암호화 된 테스트 결제 데이터를 반환합니다.

새 테스트 환경의 사용 하도록 설정 하려면 다음을 수행 합니다.

1. ITunes Connect에서에서 새 테스트 iCloud 계정을 만듭니다.
2. 새 테스트 계정 사용 하 여 iOS 장치에 로그인 합니다.
3. 앱을 테스트 하려면 원하는 영역을 설정 합니다.
4. 테스트 결제 카드 중 하나를 사용 하 여는 [Apple 지불 가이드](https://developer.apple.com/apple-pay/) 지불 되도록 합니다.

> [!NOTE]
> ICloud 계정을 전환 하 여 장치는 자동으로 새 테스트 환경으로 전환 됩니다. 그러나, 사과 여전히 **필요** iTunes App Store에 제출 하기 전에 프로덕션 환경에서 카드 응용 프로그램에서 실수와 함께 테스트할 수 있습니다.

## <a name="summary"></a>요약

이 문서는 향상 된 기능 검사가 수행 Apple에 수행한 Apple Pay watchOS 3 및 Xamarin.iOS에 구현 하는 방법입니다.
