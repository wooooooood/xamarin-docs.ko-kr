---
title: 머신에 구성 요소가 저장된 위치는 어디인가요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 9447cf903c8789078e66082e720eeecfa7bb3e0d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014232"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>머신에 구성 요소가 저장된 위치는 어디인가요?

Xamarin 구성 요소를 앱 프로젝트에 설치할 때마다 다음 두 위치에 배치 됩니다.

1. 솔루션 폴더의 루트 수준에 있는 구성 요소 폴더 솔루션의 모든 프로젝트에서 구성 요소를 제거 하면이 폴더도 제거 됩니다.

2. 복사본은 다음 위치에도 저장 됩니다.
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

따라서 시스템에서 구성 요소를 완전히 제거 하려면 프로젝트/솔루션 및 위의 캐시 폴더에서 삭제 합니다.
