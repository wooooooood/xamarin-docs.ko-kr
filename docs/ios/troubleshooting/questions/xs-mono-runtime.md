---
title: Xamarin Studio에서 iOS 프로젝트에 대한 Mono 런타임 환경 변수를 설정하려면 어떻게 해야 하나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/31/2017
ms.openlocfilehash: ba74e316f706e5bf22f973de4dad38d94ccd0db9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61417667"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Xamarin Studio에서 iOS 프로젝트에 대한 Mono 런타임 환경 변수를 설정하려면 어떻게 해야 하나요?

Mono에 대 한 모든 런타임 환경 변수를 설정 해야 할 경우 설정할 수 있습니다 합니다 **프로젝트 옵션 > 실행 > 일반** 페이지입니다.

참고: SGen 가비지 컬렉션 환경 변수 (MONO\_GC\_PARAMS) 집합을 이러한 방식으로 Xamarin Studio에서 시작 하는 경우에 사용 됩니다. 장치에서 앱을 시작 하는 경우 Sgen에 대 한 설정은 무시 됩니다. 

앱에 대 한 환경 변수를 영구적으로 설정 하려면 (모든 관련 구성)에 대 한 추가 mtouch 인수로 추가 해야 합니다.

```csharp
   --setenv=NAME=VALUE
```

설정할 수 있는 환경 변수를 Mono 매뉴얼 페이지를 참조 하십시오.  [http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1)) 섹션을 참조 하세요. `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "프로젝트에 대 한 환경 변수 설정")
