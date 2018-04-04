---
title: Xamarin Studio에서 iOS 프로젝트에 대 한 모노 런타임 환경 변수를 설정 하려면 어떻게 합니까?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/31/2017
ms.openlocfilehash: 5f4f3a2de012d35ddca9c1fa830d599d9d5acb17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Xamarin Studio에서 iOS 프로젝트에 대 한 모노 런타임 환경 변수를 설정 하려면 어떻게 합니까?

모노에 대 한 모든 런타임 환경 변수를 설정 해야 할 경우 설정할 수 있기는 **프로젝트 옵션 > 실행 > 일반** 페이지.

참고: SGen에 대 한 환경 변수를 가비지 수집 (모노\_GC\_PARAMS) 집합을 이러한 방식으로 Xamarin Studio에서 시작할 때에 사용 됩니다. 장치에서 앱을 시작 하는 경우 Sgen에 대 한 설정이 무시 됩니다. 

응용 프로그램에 대 한 환경 변수를 영구적으로 설정 하려면 (모든 관련 구성)에 대 한 추가 mtouch 인수로 서이 추가 해야 합니다.

```csharp
   --setenv=NAME=VALUE
```

설정할 수 있는 환경 변수를 보려면 모노 매뉴얼 페이지를 참조: [ http://docs.go-mono.com/?link=man%3amono(1) ](http://docs.go-mono.com/?link=man%3amono(1)) 섹션을 참조 하세요. `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "프로젝트에 대 한 환경 변수 설정")
