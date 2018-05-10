---
title: 내 컴퓨터에 구성 요소를 저장 하는 위치
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.openlocfilehash: a53045a6179a26b30d824976d11fd2769a84811e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>내 컴퓨터에 구성 요소를 저장 하는 위치

응용 프로그램 프로젝트에 Xamarin 구성 요소를 설치할 때마다 두 위치에 배치를 가져옵니다.

1. 솔루션 폴더의 루트 수준에서 구성 요소 폴더입니다. 솔루션에는 모든 프로젝트의 구성 요소를 제거 하는 경우에이 폴더에서 제거 가져오기지 것입니다.

2. 복사본은 다음 위치에도 저장 됩니다.
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

따라서 시스템에서 구성 요소를 완전히 제거 하려면 삭제 위의 캐시 폴더와 프로젝트/솔루션에서.
