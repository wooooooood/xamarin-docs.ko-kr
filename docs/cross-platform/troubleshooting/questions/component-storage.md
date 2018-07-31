---
title: 내 컴퓨터에서 구성 요소를 저장 하는 위치
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 4152c8ef7eeba3748d9244e27e48f3f9a2c0019b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350721"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>내 컴퓨터에서 구성 요소를 저장 하는 위치

앱 프로젝트로 Xamarin 구성 요소를 설치할 때마다 두 위치에 배치 되 고:

1. 솔루션 폴더의 루트 수준에서 구성 요소 폴더입니다. 솔루션의 모든 프로젝트의 구성 요소를 제거 하는 경우 것도이 폴더에서 제거 됩니다.

2. 복사본은 다음 위치에도 저장 됩니다.
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

따라서 시스템에서 구성 요소를 완전히 제거 하려면 삭제 프로젝트/솔루션에서 위의 캐시 폴더입니다.
