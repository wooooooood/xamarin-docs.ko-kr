---
title: watchOS 플랫폼 기능
description: 이 문서는 Apple Pay, 알림, 복잡성, 자동 제안, 운동 앱 등과 같은 watchOS 플랫폼 기능을 설명 하는 다양 한 가이드에 연결 합니다.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: 09200ba5968838edf829b30a50a8ad0f4a3ab3aa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61224562"
---
# <a name="watchos-platform-features"></a>watchOS 플랫폼 기능

## <a name="introduction-to-watchos-5introduction-to-watchos5indexmd"></a>[watchOS 5 소개](introduction-to-watchos5/index.md)

이 문서는 Xamarin 사용 하 여 watchOS 앱을 빌드할 때 사용 하 여 사용할 수 있는 watchOS 5의에서 새로운 기능과 업데이트 된 기능에 대 한 개략적인 개요를 제공 합니다.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[watchOS 4 소개](introduction-to-watchos4.md)

이 문서에 기능 추가 및 watchOS 4에서에서 업데이트의 대략적인 개요를 제공 합니다.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[watchOS 3 소개](introduction-to-watchos3/index.md)

이 문서는 watchOS 3의에서 새로운 기능과 업데이트 된 Api를 설명합니다.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[향상 된 Apple Pay 기능](~/ios/watchos/platform/apple-pay.md)

WatchOS 3, PassKit framework Apple Watch 실행 되는 앱에 대 한 실제 상품 및 서비스) (의 안전 하 고 앱에서 지불에 대 한 지원을 수 있도록 확장 되었습니다.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[백그라운드 작업](~/ios/watchos/platform/background-tasks.md)

watchOS 3에서는 앱을 열기 전에 사용자가 해야 하는 콘텐츠가 있는지 확인 합니다. 해당 정보를 업데이트 하는 데 사용할 수 있는 몇 가지 백그라운드 작업을 소개 합니다.

## <a name="notificationsnotificationsmd"></a>[알림](notifications.md)

Watch 앱에서 처리 하는 사용자 지정 알림을 제공 하는 방법에 알아봅니다.

## <a name="complicationscomplicationsmd"></a>[컴플리케이션](complications.md)

Watch 화면에서 최신 데이터를 표시할 complication 지원을 추가 합니다.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[자동 제안](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 사전 내에서 사용자에 게 정보를 제공 하는 앱을 사용 하면 컨텍스트를 지정 합니다. 이 기능을 지원 하기를 [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) 이제는 `MapItem` 앱에 다른 앱에서 나중에 사용할 위치 정보를 제공 하는 속성입니다.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[빠른 상호 작용 기술](~/ios/watchos/platform/quick-interaction-techniques.md)

빠른 사용자 상호 작용을 제공 하는 뛰어난 Apple Watch 앱 및 복잡성을 만드는 데 매우 중요 합니다. 새 watchOS 3 Apple에는 제스처 인식기를 액세스 하 고 디지털 Crown 사용자 알림 및 탐색에 대 한 새로운 기술에 대 한 지원이 추가 되었습니다. 따라서 SceneKit 및 SpriteKit를 둘 다에 대 한 지원이 추가 되었습니다 함께 사용 하면 개발자는 빠르고 응답성 있는 풍부 하 고 glanceable 인터페이스를 쉽게 만들 합니다.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[운동 앱 향상 된 기능](~/ios/watchos/platform/workout-apps.md)

새 watchOS 3, 운동 관련 앱 Apple Watch 백그라운드에서 실행할 수 있습니다. 앱이이 기능을 사용 하도록 설정 (및 HealthKit 데이터에 액세스)를 포함 해야 합니다 `WKBackgroundModes` 키를 `Info.plist` 값을 사용 하 여 파일 `workout-processing`합니다.
