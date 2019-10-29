---
title: Xamarin Studio에서 iOS 프로젝트에 대한 Mono 런타임 환경 변수를 설정하려면 어떻게 해야 하나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/31/2017
ms.openlocfilehash: fdd208122934b6fa8194a644592d1e23c4000d57
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030899"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Xamarin Studio에서 iOS 프로젝트에 대한 Mono 런타임 환경 변수를 설정하려면 어떻게 해야 하나요?

Mono에 대 한 런타임 환경 변수를 설정 해야 하는 경우 **프로젝트 옵션에서 > 일반 페이지 실행 >** 설정할 수 있습니다.

참고: SGen (MONO\_GC\_PARAMS)에 대 한 가비지 수집 환경 변수는 Xamarin Studio에서 시작 하는 경우에만 사용 됩니다. 장치에서 앱을 시작 하면 Sgen에 대 한 설정이 무시 됩니다. 

앱에 대 한 환경 변수를 영구적으로 설정 하려면 추가 mtouch 인수 (모든 관련 구성의 경우)로 추가 해야 합니다.

```csharp
   --setenv=NAME=VALUE
```

설정할 수 있는 환경 변수를 보려면 Mono man 페이지를 참조 하세요. [http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1)) 섹션을 참조 하세요. `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Setting environment variables for a project")
