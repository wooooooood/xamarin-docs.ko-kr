---
title: 목표-C 플랫폼
description: 이 문서에서는 목표-C 코드로 작업할 때 .NET 포함이 대상으로 지정할 수 있는 다양 한 플랫폼에 대해 설명 합니다. MacOS, iOS, tvOS 및 watchOS에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: dbc2c40e06f14ec636b4aa4cc11fc4f2d230ee6d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006420"
---
# <a name="objective-c-platforms"></a>목표-C 플랫폼

.NET 포함은 목표-C 코드를 생성할 때 다양 한 플랫폼을 대상으로 할 수 있습니다.

* macOS
* iOS
* tvOS
* watchOS [아직 구현 되지 않음]

`--platform=<platform>` 명령줄 인수를 .NET 포함에 전달 하 여 플랫폼을 선택 합니다.

IOS, tvOS 및 watchOS 플랫폼용으로 빌드할 때 .NET 포함은 항상 Xamarin.ios를 포함 하는 프레임 워크를 만듭니다. 예를 들어 Xamarin.ios는 이러한 플랫폼에 필요한 많은 런타임 지원 코드를 포함 하 고 있기 때문입니다.

그러나 macOS 플랫폼용으로 빌드할 때 생성 된 프레임 워크에서 Xamarin.ios를 포함할지 여부를 선택할 수 있습니다. 바인딩된 어셈블리가 Xamarin.ios를 참조 하지 않는 경우 (직접 또는 간접적으로) Xamarin.ios를 포함 하지 않을 수 있으며, .NET 포함 도구에 `--platform=macOS`를 전달 하 여이를 선택 합니다.

바인딩된 어셈블리에 Xamarin.ios에 대 한 참조가 포함 되어 있는 경우 Xamarin.ios를 포함 해야 하며 추가적으로 embeddinator는 사용할 대상 프레임 워크를 알아야 합니다.

다음 세 가지 가능한 Xamarin.ios 대상 프레임 워크가 있습니다. `modern` (이전에는 `mobile`이라고 함), `full` 및 `system` (각각의 차이점은 Xamarin.ios의 [대상 프레임 워크][1] 설명서에 설명 되어 있음)를 전달 하 여 선택 합니다. `--platform=macOS-modern``--platform=macOS-full` 하거나 .NET 포함 도구에 `--platform=macOS-system` 합니다.

[1]: ~/mac/platform/target-framework.md
