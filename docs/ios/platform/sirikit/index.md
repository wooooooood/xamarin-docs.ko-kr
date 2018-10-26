---
title: Xamarin.iOS에서 SiriKit
description: 이 문서에서는 Xamarin.iOS 앱에 SiriKit를 사용 하 여 iOS 장치에서 Siri를 사용 하 여 사용자에 게 액세스할 수 있는 서비스를 제공 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 84E5681A-F557-4967-AA99-F831169157AA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 9f7cbb3f7d9e448947ec8163a8660616910e750f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117527"
---
# <a name="sirikit-in-xamarinios"></a>Xamarin.iOS에서 SiriKit

_이 문서에서는 Xamarin.iOS 앱에 SiriKit를 사용 하 여 iOS 장치에서 Siri를 사용 하 여 사용자에 게 액세스할 수 있는 서비스를 제공 하는 방법을 보여 줍니다._

새 iOS 10, SiriKit 허용 앱 확장 및 새로운를 사용 하 여 iOS 장치에서 Siri 및 맵 앱을 사용 하 여 사용자에 게 액세스할 수 있는 서비스를 제공 하는 iOS 앱 **의도** 하 고 **인 텐트 UI** 프레임 워크입니다.

Siri의 개념을 사용 하 여 작동 **도메인**, 관련된 작업에 대 한 작업 그룹을 알고 있어야 합니다. Siri를 사용 하 여 앱에 있는 각 상호 작용 다음과 같이 해당 알려진된 서비스 도메인 중 하나에 속합니다 해야 합니다.:

- 오디오 또는 비디오 호출 합니다.
- 승객을 예약 합니다.
- 달리기를 관리 합니다.
- 메시징입니다.
- 사진을 검색 합니다.
- 지불을 주고받지 설정 합니다.

SiriKit 보냅니다 확장 사용자 요청 하면 Siri의 관련 앱 확장의 서비스 중 하나에 **의도** 지 원하는 모든 데이터와 함께 사용자의 요청을 설명 하는 개체입니다. 앱 확장 한 다음 적절 한 생성 **응답** 개체를 지정 **의도**, 확장 요청을 처리할 수 있는 방법을 자세히 설명 합니다.

## <a name="understanding-sirikit-conceptsiosplatformsirikitunderstanding-sirikitmd"></a>[SiriKit 개념 이해하기](~/ios/platform/sirikit/understanding-sirikit.md)

이 문서에서는 Xamarin.iOS 앱에 SiriKit을 사용 해야 하는 주요 개념을 다룹니다. 새 설명 인 텐트와 인 텐트 UI 확장 지점 및 Siri에 앱을 열려면 앱 및 사용자 어휘를 사용 하 여 작동 방식입니다.

## <a name="implementing-sirikitiosplatformsirikitimplementing-sirikitmd"></a>[SiriKit 구현](~/ios/platform/sirikit/implementing-sirikit.md)

이 문서에서는 Xamarin.iOS 앱에 SiriKit 지원을 구현 하는 데 필요한 단계를 다룹니다. 개발자 개념 나와 성공적으로 구현 해야 하는 키로 SiriKit 지원을 앱에 추가 하기 전에 위의 SiriKit 개념 이해 가이드를 읽어야 합니다.





## <a name="related-links"></a>관련 링크

- [ElizaChat 샘플](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [SiriKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [인 텐트 프레임 워크 참조](https://developer.apple.com/reference/intents)
- [인 텐트 UI 프레임 워크 참조](https://developer.apple.com/reference/intentsui)
