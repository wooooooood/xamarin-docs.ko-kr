---
title: Objective C 플랫폼
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: a783dc554f54922f4fa4f99a43d83c4c864ef681
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="objective-c-platforms"></a>Objective C 플랫폼


포함 하는.NET Objective C 코드를 생성할 때 다양 한 플랫폼을 대상 수 있습니다.

* macOS
* iOS
* tvOS
* watchOS [아직 구현 되지]

플랫폼은 전달 하 여 선택 하는 `--platform=<platform>` 명령줄 인수는 embeddinator입니다.

IOS, tvOS 및 watchOS 플랫폼을 빌드하는 경우는 embeddinator 항상 Xamarin.iOS 이러한 플랫폼에서 필요한 런타임 지원 코드를 많이 포함 되어 있으므로 Xamarin.iOS를 포함 하는 프레임 워크를 만들어집니다.

그러나 macOS 플랫폼을 만들 때 생성 된 프레임 워크 여부 Xamarin.Mac을 포함 해야 하는지 여부를 선택할 수는 있습니다. 바인딩된 어셈블리 (직접 또는 간접적으로), Xamarin.Mac.dll 참조 하지 않는 하 고 전달 하 여이 확인란을 선택 하는 경우 Xamarin.Mac 포함 하지 않는 것이 불가능 `--platform=macOS` 는 embeddinator에 있습니다.

바인딩된 어셈블리 Xamarin.Mac.dll에 대 한 참조가 들어 있으면 Xamarin.Mac를 포함 하는 데 필요한 되며 또한는 embeddinator 사용 하는 대상 프레임 워크를 알고 있어야 합니다.

세 가지 가능한 Xamarin.Mac 대상 프레임 워크는: `modern` (이전에 호출 `mobile`), `full` 및 `system` (Xamarin.Mac의에서 각 간의 차이점 설명 [대상 프레임 워크] [ 1] 설명서), 각 전달 하 여 선택 하 고 `--platform=macOS-modern`, `--platform=macOS-full` 또는 `--platform=macOS-system` 는 embeddinator를 합니다.

[1]: ~/mac/platform/target-framework.md
