---
title: SiriKit
description: 이 문서에는 iOS 장치에서 Siri를 사용 하는 사용자에 액세스할 수 있는 서비스를 제공 하도록 SiriKit Xamarin.iOS 앱에서 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: ac68f249361fd5bee8af102a1418c9d5f1d2c0ca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="sirikit"></a>SiriKit

_이 문서에는 iOS 장치에서 Siri를 사용 하는 사용자에 액세스할 수 있는 서비스를 제공 하도록 SiriKit Xamarin.iOS 앱에서 사용 하는 방법을 보여 줍니다._

새 응용 프로그램 확장 및 새를 사용 하 여 iOS 장치에서 Siri와 지도 응용 프로그램을 사용 하는 사용자에 액세스할 수 있는 서비스를 제공 하는 iOS 앱을 사용 하면 SiriKit 10 iOS에 **의도** 및 **의도 UI** 프레임 워크입니다.

Siri의 개념과 작동 **도메인**, 그룹의 관련된 작업에 대 한 작업을 알고 있습니다. 앱 Siri 개가 있는 각 상호 작용 다음과 같이 알려진된 서비스 도메인 중 하나에 해당 해야 합니다.

- 오디오 또는 비디오 호출 합니다.
- 안에서 예약 합니다.
- 체력 단련을 관리 합니다.
- 메시징입니다.
- 사진을 검색 합니다.
- 송신 하거나 수신 지불 합니다.

사용자가 응용 프로그램 확장의 서비스 중 하나는 관련 된 Siri의 요청, SiriKit 확장 보냅니다는 **의도** 지원 데이터와 함께 사용자의 요청을 설명 하는 개체입니다. 앱 확장 한 다음 적절 한 생성 **응답** 개체에 대 한는 주어진 **의도**, 확장에서 요청을 처리할 수는 방법에 대해 자세히 설명 합니다.

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[SiriKit 개념 이해하기](~/ios/platform/sirikit/understanding-sirikit.md)

이 문서에서는 필요한 SiriKit Xamarin.iOS 앱에서 사용 하기 위한 주요 개념을 설명 합니다. 적용 되는 새 의도 및 의도 UI 확장 지점 및 응용 프로그램 및 사용자 어휘 Siri에 앱을 열을 사용 하 여 작동 방식입니다.

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[SiriKit 구현](~/ios/platform/sirikit/implementing-sirikit.md)

이 문서에 Xamarin.iOS 앱 SiriKit 지원을 구현 하는 데 필요한 단계에 설명 합니다. 개발자는 성공적으로 구현에 대 한 필요한 개념 검사 된 키로 응용 프로그램에 SiriKit 지원을 추가 하기 전에 위에 SiriKit 개념 이해 가이드를 읽어야 합니다.





## <a name="related-links"></a>관련 링크

- [ElizaChat 샘플](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [의도 프레임 워크 참조](https://developer.apple.com/reference/intents)
- [의도 UI 프레임 워크 참조](https://developer.apple.com/reference/intentsui)
