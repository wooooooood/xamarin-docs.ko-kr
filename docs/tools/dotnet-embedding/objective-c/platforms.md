---
title: 목표-C 플랫폼
description: 이 문서에서는 목표-C 코드로 작업할 때 .NET 포함이 대상으로 지정할 수 있는 다양 한 플랫폼에 대해 설명 합니다. MacOS, iOS, tvOS 및 watchOS에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: f97b595f129cb1ad1ea56e3ae43b0f0a477fef5a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282723"
---
# <a name="objective-c-platforms"></a>목표-C 플랫폼

.NET 포함은 목표-C 코드를 생성할 때 다양 한 플랫폼을 대상으로 할 수 있습니다.

* macOS
* iOS
* tvOS
* watchOS [아직 구현 되지 않음]

.Net 포함에 `--platform=<platform>` 명령줄 인수를 전달 하 여 플랫폼을 선택 합니다.

IOS, tvOS 및 watchOS 플랫폼용으로 빌드할 때 .NET 포함은 항상 Xamarin.ios를 포함 하는 프레임 워크를 만듭니다. 예를 들어 Xamarin.ios는 이러한 플랫폼에 필요한 많은 런타임 지원 코드를 포함 하 고 있기 때문입니다.

그러나 macOS 플랫폼용으로 빌드할 때 생성 된 프레임 워크에서 Xamarin.ios를 포함할지 여부를 선택할 수 있습니다. 바인딩된 어셈블리가 xamarin.ios (직접 또는 간접적)를 참조 하지 않는 경우 xamarin.ios를 포함 하지 않을 수 있으며 .net 포함 도구에 전달 `--platform=macOS` 하 여이를 선택 합니다.

바인딩된 어셈블리에 Xamarin.ios에 대 한 참조가 포함 되어 있는 경우 Xamarin.ios를 포함 해야 하며 추가적으로 embeddinator는 사용할 대상 프레임 워크를 알아야 합니다.

세 가지 가능한 xamarin.ios `modern` 대상 프레임 워크 (이전에는 호출 `mobile`됨) `full` `system` 가 있으며, 각각의 차이점은 xamarin.ios의 [대상 프레임 워크][1] 설명서에 설명 되어 있습니다. 는를 `--platform=macOS-modern` `--platform=macOS-full` 전달 하거나`--platform=macOS-system` .net 포함 도구에 전달 하 여 선택 합니다.

[1]: ~/mac/platform/target-framework.md
