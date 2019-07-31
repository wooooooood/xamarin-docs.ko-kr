---
title: Xamarin.ios의 SiriKit
description: 이 문서에서는 Xamarin.ios 앱에서 SiriKit를 사용 하 여 iOS 장치에서 Siri를 사용 하는 사용자에 게 액세스할 수 있는 서비스를 제공 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 0438ea08bbdcbf0ce3c64e15192cbb90cd835b00
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68654010"
---
# <a name="sirikit-in-xamarinios"></a>Xamarin.ios의 SiriKit

_이 문서에서는 Xamarin.ios 앱에서 SiriKit를 사용 하 여 iOS 장치에서 Siri를 사용 하는 사용자에 게 액세스할 수 있는 서비스를 제공 하는 방법을 보여 줍니다._

IOS 10의 새로운 기능으로 SiriKit를 사용 하면 iOS 앱이 앱 확장 및 새 **의도** 및 **의도 UI** 프레임 워크를 사용 하 여 Ios 장치에서 siri 및 Maps 앱을 사용 하 여 사용자가 액세스할 수 있는 서비스를 제공할 수 있습니다.

Siri는 **도메인**개념과 관련 태스크에 대 한 알고 있는 작업 그룹을 사용 하 여 작동 합니다. 앱에서 Siri를 사용 하는 각 상호 작용은 다음과 같이 알려진 서비스 도메인 중 하나에 속해야 합니다.

- 오디오나 비디오를 호출 합니다.
- 을 예약 합니다.
- 워크를 관리 합니다.
- 전송.
- 사진을 검색 하 고 있습니다.
- 지불 보내기 또는 받기.

사용자가 앱 확장의 서비스 중 하나를 포함 하는 Siri를 요청 하면 SiriKit에서 해당 확장을 지원 데이터와 함께 사용자 요청을 설명 하는 **의도** 개체에 보냅니다. 그러면 앱 확장이 지정 된 **의도**에 대 한 적절 한 **응답** 개체를 생성 하 여 확장에서 요청을 처리할 수 있는 방법을 자세히 설명 합니다.

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[SiriKit 개념 이해하기](~/ios/platform/sirikit/understanding-sirikit.md)

이 문서에서는 Xamarin.ios 앱에서 SiriKit를 사용 하는 데 필요한 핵심 개념을 설명 합니다. 새 의도 및 의도 UI 확장 지점과 앱 및 사용자 어휘를 사용 하 여 앱을 Siri로 여는 방법에 대해 설명 합니다.

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[SiriKit 구현](~/ios/platform/sirikit/implementing-sirikit.md)

이 문서에서는 Xamarin.ios 앱에서 SiriKit 지원을 구현 하는 데 필요한 단계를 설명 합니다. 개발자는 성공적인 구현에 필요한 주요 개념에 대해 설명 하는 대로 앱에 SiriKit 지원을 추가 하기 전에 위의 SiriKit 개념 이해 가이드를 읽어야 합니다.





## <a name="related-links"></a>관련 링크

- [ElizaChat 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-elizachat)
- [SiriKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [의도 프레임 워크 참조](https://developer.apple.com/reference/intents)
- [의도 UI 프레임 워크 참조](https://developer.apple.com/reference/intentsui)
