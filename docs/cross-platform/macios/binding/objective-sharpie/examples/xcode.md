---
title: Xcode 프로젝트를 사용 하는 실제 예제
description: 이 문서에서는 Xcode 프로젝트를 목표 Sharpie에 대 한 직접 입력으로 사용 하 여 목표-C 코드에 C# 대 한 바인딩을 만드는 프로세스를 간소화 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 083bebd093a8db92b0e64ba11d13bd32da88f604
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521937"
---
# <a name="real-world-example-using-an-xcode-project"></a>Xcode 프로젝트를 사용 하는 실제 예제

**이 예제에서는 [Facebook의 POP 라이브러리](https://github.com/facebook/pop)를 사용 합니다.**

버전 3.0에서 새로 만들기, 목표 Sharpie는 Xcode 프로젝트를 입력으로 지원 합니다. 이러한 프로젝트는 네이티브 라이브러리를 컴파일하는 데 필요한 올바른 헤더 파일 및 컴파일러 플래그를 지정 하므로이를 바인딩해야 합니다. 목표 Sharpie는 다른 방법으로 지시 하지 않은 경우 프로젝트의 첫 번째 _대상과_ 기본 구성을 선택 합니다.

목표 Sharpie는 프로젝트 및 헤더 파일의 구문 분석을 시도 하기 전에 빌드해야 합니다. 프로젝트에는 외부 사용 및 통합을 위한 헤더 파일 구조를 올바르게 구성 하는 빌드 단계가 포함 되어 있기 때문에 바인딩을 시도 하기 전에 항상 전체 프로젝트를 빌드하는 것이 좋습니다.

```
$ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   (more git clone output)

$ cd pop
$ sharpie bind pop.xcodeproj -sdk iphoneos9.0
```
