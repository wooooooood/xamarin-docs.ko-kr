---
title: Objective-c 플랫폼
description: 이 문서에서는 Objective-c 코드를 사용 하 여 작업 하는 경우에 대상 수.NET 포함 하는 다양 한 플랫폼을 설명 합니다. MacOS, iOS, tvOS 및 watchOS에 설명 합니다.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 8091fb4e8328f61f1471d061b51b4735de3c089c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108885"
---
# <a name="objective-c-platforms"></a>Objective-c 플랫폼

.NET 포함 Objective-c 코드를 생성할 때 다양 한 플랫폼 대상:

* macOS
* iOS
* tvOS
* watchOS [아직 구현 되지]

플랫폼을 전달 하 여 선택 된 `--platform=<platform>` 명령줄 인수를 포함 하는.NET.

IOS 용 빌드, tvOS 및 watchOS 플랫폼 및.NET 포함 항상 Xamarin.iOS에는 이러한 플랫폼에서 필요한 런타임 지원 코드를 많이 포함 되어 있으므로 Xamarin.iOS를 포함 하는 프레임 워크를 만듭니다.

그러나 macOS 플랫폼을 빌드할 때 여부 생성된 프레임 워크에서 Xamarin.Mac를 포함 해야 하는지 여부를 선택할 수는 있습니다. Xamarin.Mac을 포함 하지 않습니다 (직접 또는 간접적으로)에 바인딩된 어셈블리 Xamarin.Mac.dll 참조 하지 않습니다 하 고이 전달 하 여 선택한 경우 있기 `--platform=macOS` .NET 포함 도구입니다.

바인딩된 어셈블리 Xamarin.Mac.dll에 대 한 참조가 들어 있으면 Xamarin.Mac을 포함 하는 데 필요한 이며 또한 embeddinator를 사용 하는 대상 프레임 워크를 알고 있어야 합니다.

세 가지 가능한 Xamarin.Mac 대상 프레임 워크는: `modern` (이전의 `mobile`), `full` 하 고 `system` (xamarin.mac의 각 간의 차이점 설명 [대상 프레임 워크] [ 1] 설명서)을 전달 하 여 선택한는 각 `--platform=macOS-modern`, `--platform=macOS-full` 또는 `--platform=macOS-system` .NET 포함 도구입니다.

[1]: ~/mac/platform/target-framework.md
