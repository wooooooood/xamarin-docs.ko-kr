---
title: watchOS 플랫폼 기능
description: Apple Pay, 알림, 복잡성, 자동 관리 제안, 워크 아웃 응용 프로그램 등과 같은 watchOS 플랫폼 기능을 설명 하는 다양 한 지침은이 문서 연결 되어 있습니다.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 16d10dd69223f404aac7c933302992a1544461e9
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066616"
---
# <a name="watchos-platform-features"></a>watchOS 플랫폼 기능

Apple Pay, 알림, 복잡성, 자동 관리 제안, 워크 아웃 응용 프로그램 등과 같은 watchOS 플랫폼 기능을 설명 하는 다양 한 지침은이 문서 연결 되어 있습니다.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[watchOS 4 소개](introduction-to-watchos4.md)

이 문서는 추가 되 고 watchOS 4에서에서 업데이트 기능의 상위 수준 개요를 제공 합니다.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[watchOS 3 소개](introduction-to-watchos3/index.md)

이 문서에서는 watchOS 3의에서 새로운 기능과 업데이트 된 Api를 설명 합니다.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay 향상 된 기능](~/ios/watchos/platform/apple-pay.md)

3 watchOS PassKit 프레임 워크 Apple Watch 실행 되는 앱에 대 한 실제 상품 및 서비스) (의 안전 하 고, 응용 프로그램에서 결제에 대 한 지원을 수 있도록 확장 되었습니다.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[백그라운드 작업](~/ios/watchos/platform/background-tasks.md)

watchOS 3 응용 프로그램 콘텐츠가가지고 있는지 확인 해야 하는 열기 전에 해당 정보를 업데이트 하는 데 사용할 수 있는 몇 가지 백그라운드 작업을 소개 합니다.

## <a name="notificationsnotificationsmd"></a>[알림](notifications.md)

조사식 응용 프로그램에서 처리 하는 사용자 지정 알림을 제공 하는 방법에 알아봅니다.

## <a name="complicationscomplicationsmd"></a>[컴플리케이션](complications.md)

Watch 화면에 최신 데이터를 표시할 complication 지원을 추가 합니다.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[자동 관리 제안](~/ios/watchos/platform/proactive-suggestions.md)

3 watchOS 사전 내 사용자에 게 정보를 표시 하려면 응용 프로그램 허용 컨텍스트를 제공 합니다. 이 기능을 지원 하기 위해는 [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) 이제는 `MapItem` 앱에 다른 앱에서 나중에 사용할 위치 정보를 제공 하는 속성입니다.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[빠른 상호 작용 방법](~/ios/watchos/platform/quick-interaction-techniques.md)

빠른 사용자 상호 작용을 제공 하는 강력한 Apple Watch 응용 프로그램 및 복잡성을 만드는 데 매우 중요 합니다. 새로운 3 watchOS 하기 위해 Apple 제스처 인식기, 디지털 왕관 및 새 사용자 알림 및 탐색 기술에 대 한 액세스에 대 한 지원을 추가 했습니다. 이렇게, SceneKit와 SpriteKit에 대 한 지원 추가 함께 수 개발자가 다양 하 고 동적인 인터페이스를 신속 하 고 응답을 쉽게 만들 수 있습니다.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[워크 아웃 응용 프로그램의 향상 된 기능](~/ios/watchos/platform/workout-apps.md)

새로운 3 watchOS 워크 아웃 관련 응용 프로그램에서 Apple Watch 백그라운드에서 실행 하는 기능이 있습니다. 응용 프로그램을 사용 하려면이 기능 (HealthKit 데이터에 액세스할 수) 포함 해야 합니다는 `WKBackgroundModes` 키에 `Info.plist` 값을 가진 파일 `workout-processing`합니다.
