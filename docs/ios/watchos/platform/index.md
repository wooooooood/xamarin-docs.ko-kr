---
title: watchOS 플랫폼 기능
description: 이 문서는 Apple Pay, 알림, 복잡, 사전 제안, 체력 앱 등의 watchOS 플랫폼 기능을 설명 하는 다양 한 가이드에 연결 됩니다.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: e571132b5f1e30bececb8302f2dacfcd908ad42e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028289"
---
# <a name="watchos-platform-features"></a>watchOS 플랫폼 기능

## <a name="introduction-to-watchos-5introduction-to-watchos5indexmd"></a>[watchOS 5 소개](introduction-to-watchos5/index.md)

이 문서에서는 Xamarin을 사용 하 여 watchOS apps를 빌드할 때 사용할 수 있는 watchOS 5의 새롭고 업데이트 된 기능에 대 한 개략적인 개요를 제공 합니다.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[watchOS 4 소개](introduction-to-watchos4.md)

이 문서에서는 watchOS 4에서 추가 및 업데이트 된 기능의 개략적인 개요를 제공 합니다.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[watchOS 3 소개](introduction-to-watchos3/index.md)

이 문서에서는 watchOS 3의 새로운 Api 및 업데이트 된 Api에 대해 설명 합니다.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[향상 된 Apple Pay](~/ios/watchos/platform/apple-pay.md)

WatchOS 3에서는 PassKit 프레임 워크를 확장 하 여 Apple Watch에서 실행 되는 앱에 대 한 안전한 앱 내 지불 (물리적 상품 및 서비스 모두)을 지원할 수 있습니다.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[백그라운드 작업](~/ios/watchos/platform/background-tasks.md)

watchOS 3에는 앱이 정보를 업데이트 하는 데 사용할 수 있는 몇 가지 백그라운드 작업이 도입 되어 사용자가 파일을 열기 전에 필요한 콘텐츠가 있는지 확인 합니다.

## <a name="notificationsnotificationsmd"></a>[알림](notifications.md)

Watch 앱에서 사용자 지정 알림 처리를 제공 하는 방법을 알아봅니다.

## <a name="complicationscomplicationsmd"></a>[컴플리케이션](complications.md)

복잡 한 지원을 추가 하 여 조사식에 최신 데이터를 표시 합니다.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[사전 권장 사항](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3을 사용 하면 앱에서 지정 된 컨텍스트 내에서 사용자에 게 정보를 사전에 제공할 수 있습니다. 이 기능을 지원 하기 위해 [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) 에는 나중에 다른 앱에서 사용할 수 있도록 앱에서 위치 정보를 제공할 수 있도록 하는 `MapItem` 속성이 포함 되어 있습니다.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[빠른 상호 작용 기술](~/ios/watchos/platform/quick-interaction-techniques.md)

빠른 사용자 상호 작용을 제공 하는 것은 뛰어난 Apple Watch 앱 및 복잡 한 문제를 만드는 데 필수적입니다. WatchOS 3에 새로 추가 된 Apple에는 제스처 인식기, Digital Crown에 대 한 액세스 및 새로운 사용자 알림과 탐색 기술에 대 한 지원이 추가 되었습니다. SceneKit 및 SpriteKit 모두에 대 한 추가 지원과 함께 개발자는 빠르고 응답성이 뛰어난 풍부한 glanceable 인터페이스를 쉽게 만들 수 있습니다.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[향상 된 응용 프로그램 향상](~/ios/watchos/platform/workout-apps.md)

WatchOS 3의 새로운 기능으로, 진행 중인 관련 앱은 Apple Watch의 배경에서 실행할 수 있습니다. 이 기능을 사용 하도록 설정 하 고 HealthKit 데이터에 대 한 액세스 권한을 얻으려면 앱은 `workout-processing`값을 사용 하 여 `Info.plist` 파일에 `WKBackgroundModes` 키를 포함 해야 합니다.
