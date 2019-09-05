---
title: Visual Studio에서 내 참조된 라이브러리 프로젝트를 빌드에 포함시키지 않는 이유는 무엇인가요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: conceptdev
ms.author: crdun
ms.date: 12/02/2016
ms.openlocfilehash: 37fa93ef7377456d61d1a5f5de56d5de6b0f3c7f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282905"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Visual Studio에서 내 참조된 라이브러리 프로젝트를 빌드에 포함시키지 않는 이유는 무엇인가요?

Visual Studio는 **Configuration Manager** 를 사용 하 여 지정 된 빌드 또는 배포 구성에 자동으로 포함 되는 솔루션의 프로젝트를 결정 합니다.

참조 된 라이브러리 프로젝트를 사용 하 여 생성 된 일부 템플릿에는 이미 참조 된 라이브러리가 구성에 포함 되어 있습니다. 그렇지 않으면 수동으로 설정 해야 합니다.

## <a name="how-to-use-the-configuration-manager"></a>Configuration Manager 사용 방법

1. **빌드 >** 를 엽니다 Configuration Manager
2. 사용자 지정할 구성 선택 (예: **디버그 | iPhone** )
3. 포함할 프로젝트의 확인란을 선택 합니다.

> [!NOTE]
> 회색 상자는 자동으로 처리 되며 변경 내용이 필요 하지 않습니다.

다음 단계를 동영상 가이드.[http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
