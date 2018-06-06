---
title: Objective C 플랫폼
description: 이 문서에서는 Objective C 코드로 작업할 때 대상.NET 포함 하는 다양 한 플랫폼에 설명 합니다. MacOS, iOS, tvOS, 및 watchOS에 설명 합니다.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 6eeb776959d1a2a37d67bfae6603971d0e5a22b7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793893"
---
# <a name="objective-c-platforms"></a>Objective C 플랫폼

포함 하는.NET Objective C 코드를 생성할 때 다양 한 플랫폼을 대상 수 있습니다.

* macOS
* iOS
* tvOS
* watchOS [아직 구현 되지]

플랫폼은 전달 하 여 선택 하는 `--platform=<platform>` .NET 포함 하는 명령줄 인수입니다.

IOS를 빌드할 때.NET 포함 tvOS 및 watchOS 플랫폼은 항상 Xamarin.iOS 이러한 플랫폼에서 필요한 런타임 지원 코드를 많이 포함 되어 있으므로 Xamarin.iOS를 포함 하는 프레임 워크를 만들어집니다.

그러나 macOS 플랫폼을 만들 때 생성 된 프레임 워크 여부 Xamarin.Mac을 포함 해야 하는지 여부를 선택할 수는 있습니다. 바인딩된 어셈블리 (직접 또는 간접적으로), Xamarin.Mac.dll 참조 하지 않는 하 고 전달 하 여이 확인란을 선택 하는 경우 Xamarin.Mac 포함 하지 않는 것이 불가능 `--platform=macOS` 를 포함 하는.NET 도구입니다.

바인딩된 어셈블리 Xamarin.Mac.dll에 대 한 참조가 들어 있으면 Xamarin.Mac를 포함 하는 데 필요한 되며 또한는 embeddinator 사용 하는 대상 프레임 워크를 알고 있어야 합니다.

세 가지 가능한 Xamarin.Mac 대상 프레임 워크는: `modern` (이전에 호출 `mobile`), `full` 및 `system` (Xamarin.Mac의에서 각 간의 차이점 설명 [대상 프레임 워크] [ 1] 설명서), 각 전달 하 여 선택 하 고 `--platform=macOS-modern`, `--platform=macOS-full` 또는 `--platform=macOS-system` 를 포함 하는.NET 도구입니다.

[1]: ~/mac/platform/target-framework.md
